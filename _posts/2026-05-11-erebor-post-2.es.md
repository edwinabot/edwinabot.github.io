---
layout: post
title: "Ingestión del libro de órdenes: iterando sobre una spec que escribí pero un código que no"
date: 2026-05-11
tags: [agentic-ai, software-engineering, go, side-projects]
lang: es
permalink: /2026/05/11/erebor-post-2.html
---

En el [primer post]({% post_url 2026-04-27-erebor-post-1 %}) describí el flujo de trabajo al que me estoy comprometiendo para Erebor: arquitectar, delegar, revisar, aceptar. Dije que el siguiente post iba a ser sobre el servicio de ingestión del libro de órdenes — cómo era la spec, qué produjo Claude Code, y qué tuve que cuestionarle.

Este es ese post. Fueron entre 8 y 10 horas de trabajo real distribuidas en varios días. [El PR](https://github.com/edwinabot/erebor/pull/1) terminó con 4.893 líneas agregadas en 36 archivos — un servicio Go completo, esquema de base de datos, configuración Docker, pipeline de CI y suite de tests. Acá va lo que pasó en realidad.

---

## Empezar desde una spec, no desde una tarea

Antes de abrir Claude Code, tuve dos conversaciones con Claude para preparar el trabajo de implementación. La primera produjo el [ADR-001](https://github.com/edwinabot/erebor/blob/main/adrs/ADR-001-order-book-ingestion/ADR-001-order-book-ingestion.md). Las decisiones de arquitectura son mías — yo hice la investigación, evalué las opciones y tomé las decisiones. Lo que aportó Claude fue organizar y componer el manuscrito: convertir mis decisiones y notas en un documento estructurado y legible. El ADR cubre cinco decisiones: persistencia híbrida (diffs crudos más checkpoints periódicos), TimescaleDB como capa de almacenamiento, un stream WebSocket combinado con dispatch interno, `shopspring/decimal` para toda la aritmética financiera, y un límite de profundidad configurable por símbolo.

La segunda conversación produjo el [checklist de code review](https://github.com/edwinabot/erebor/blob/main/adrs/ADR-001-order-book-ingestion/CODE-REVIEW-CHECKLIST-001.md). Ocho secciones: corrección, integridad de interfaces, seguridad, comportamiento operacional, esquema de persistencia, cobertura de tests y calidad de código. El checklist existe porque revisar código generado por IA sin un contrato previo es un ejercicio de adivinación. El contrato tiene que venir primero.

Después escribí el prompt de delegación — un documento que desde entonces guardé como [DELEGATION-PROMPTS-001.md](https://github.com/edwinabot/erebor/blob/main/adrs/ADR-001-order-book-ingestion/DELEGATION-PROMPTS-001.md). Tiene varios cientos de palabras, incluye firmas exactas de interfaces, tipos de dominio exactos, condiciones exactas de alineación del bootstrap, y una lista de cosas que explícitamente no hay que construir. Esta es la spec que recibió Claude Code.

---

## El código funcionó. El sistema, no.

Después del prompt inicial, Claude Code produjo una implementación sustancial. Todos los tests pasaban. El código Go compilaba limpiamente. Y entonces intenté ejecutarlo de verdad.

No se podía ejecutar. La configuración de los contenedores estaba mal, las migraciones no se estaban aplicando, la integración entre el CLI y la base de datos en ejecución no estaba cableada correctamente. Los tests estaban en verde porque probaban las unidades en aislamiento — lo que estaba roto era todo lo que las rodeaba. Esto requirió varias rondas adicionales de prompts para resolver, incluyendo uno focalizado para agregar Docker Compose para una instancia local de TimescaleDB y un comando CLI para pruebas de integración contra ella.

Esta fue una lección útil sobre la brecha entre la corrección a nivel de unidad y la ejecutabilidad a nivel de sistema. Que los tests pasen no es lo mismo que que el software funcione. La spec había cubierto el protocolo de bootstrap con un detalle exhaustivo y no decía casi nada sobre la ejecución local. Esa omisión fue mía, y el agente entregó exactamente lo que estaba descripto.

---

## La paradoja del recién llegado

Una vez que tuve algo funcionando, me topé con algo que no esperaba.

Me sentí como un recién llegado al proyecto.

No de manera confusa — había escrito la spec del producto, y había tomado cada decisión de arquitectura en el ADR. Son cosas distintas: la spec define qué construir; el ADR define los guardrails y las restricciones de implementación que determinan cómo la spec puede ejecutarse. Ambas eran mías, y ambas me tenían todo el sentido. Pero en los primeros diez o quince minutos después de que Claude terminó de generar el código, no podía conectar inmediatamente lo que estaba leyendo con ninguna de las dos. El código era correcto, los nombres eran razonables, la estructura seguía el diseño. Y aun así sentí la fricción específica de llegar a una codebase en curso sin haber estado presente durante la implementación.

Esta es la paradoja: tenía dos capas de documentación — la spec del producto y las restricciones de arquitectura — y aun así me sentí como un recién llegado al código que las implementaba. El código no es la spec, aunque el código implemente fielmente la spec.

Los Engineering Managers que no son tan hands-on van a reconocer esta sensación. Son dueños del roadmap, aprobaron la arquitectura, saben exactamente por qué se tomó cada decisión — y después se sientan con la codebase y se pierden. No es ignorancia del dominio. Es la brecha entre el mapa y el territorio. Yo fui ese EM. Esto fue un recordatorio de lo que esa brecha se siente de verdad desde adentro.

Pensé en el capítulo de Robert Martin sobre comentarios en *Clean Code*. Todavía tiene razón. Le pedí a Claude que agregara docstrings a todos los símbolos exportados — métodos, structs, interfaces. Eso hizo una diferencia inmediata. No porque los nombres estuvieran mal, sino porque la intención detrás de cada componente se volvió explícita en el lugar donde la necesitaba, en vez de obligarme a rastrear de vuelta hacia el ADR.

---

## Aprendiendo goroutines siendo un desarrollador de Python

Un efecto secundario inesperado de trabajar con cuidado este código fue aprender más sobre goroutines de lo que había aprendido antes.

Mi background es Python. Entiendo los procesos y los threads de la vieja escuela, y en los últimos años trabajé bastante con asyncio. Las goroutines son un modelo diferente — conceptualmente entre los threads y las corrutinas, pero con un scheduler en el runtime que las hace sentir más livianas y componibles que cualquiera de los dos. Trabajar a través del procedimiento de bootstrap del `SymbolHandler`, donde el fetch del snapshot corre concurrentemente con el buffer de diffs y los dos tienen que unirse de manera segura, ayudó a que las goroutines se sintieran concretas en lugar de abstractas. Son genuinamente elegantes para este tipo de problema.

---

## Qlty y el costo de la complejidad cognitiva

Le pedí a Claude que agregara configuración de Qlty CLI al proyecto. Qlty ejecutó `golangci-lint` y su propio análisis y devolvió una lista de hallazgos. Después le pedí a Claude que los abordara.

Las reducciones de complejidad cognitiva fueron significativas. Varias funciones en el `SymbolHandler` tenían puntajes de complejidad que eran Go técnicamente válido pero difícil de leer — condicionales profundamente anidadas dentro de las transiciones de la máquina de estados. El refactoring que Qlty empujó produjo código más limpio sin cambiar el comportamiento. No los hubiera detectado todos en la revisión.

---

## Los tests como documentación, y sus límites

Tenía poca cobertura de tests después de la implementación inicial. Ejecuté Codecov y lo confirmó. Reprompeé en etapas: primero para tests del happy path en todos los paquetes, después para los caminos de ejecución faltantes, y después específicamente para comentar cada test sin comentar explicando la funcionalidad siendo testeada.

El último prompt fue el que más valor aportó. Llevo años diciendo que los tests son gran documentación. Y lo son — pero solo cuando tengo tiempo de leerlos con cuidado. Lo que en realidad necesitaba era que los tests se explicaran a sí mismos sin requerir ese tiempo de estudio. El prompt "comentá todos los tests sin comentar explicando la funcionalidad siendo testeada" entregó exactamente eso.

Me tomé el tiempo de verificar que los comentarios fueran precisos — que la explicación coincidiera con las aserciones del test. Coincidían. Después de hacer eso, pude explicar cómo funciona el servicio de ingestión, de punta a punta, con confianza. Esa confianza vino de los tests comentados, no del código en sí.

---

## Dónde están las cosas

El [PR](https://github.com/edwinabot/erebor/pull/1) está mergeado. El servicio ingesta datos del libro de órdenes desde Binance, persiste diffs crudos y checkpoints periódicos en TimescaleDB, implementa el protocolo de sincronización de bootstrap especificado en el ADR-001, y hace un shutdown gracioso ante SIGTERM y SIGINT.

El próximo paso es ejecutar esto en hardware dedicado. Compré una máquina que voy a usar como servidor — es más barato que la nube en esta etapa y me da más runway para experimentar con infraestructura de contenedores antes de compreterme con una topología. El próximo post va a cubrir eso: hardware local, deployment basado en Docker, y la primera corrida de recolección de datos continua.

Entre 8 y 10 horas de trabajo. Diecinueve commits. Una cosa más que sé cómo construir.

---

*Edwin Alexis Abot — Mayo de 2026*

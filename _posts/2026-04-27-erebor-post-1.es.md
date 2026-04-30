---
layout: post
title: "Por qué estoy construyendo un sistema de trading sin escribir la mayor parte del código"
date: 2026-04-27
tags: [agentic-ai, software-engineering, career, side-projects]
lang: es
permalink: /2026/04/27/erebor-post-1.html
---

Tengo más de diez años en backend e ingeniería de sistemas. Escribí mucho código. Y hace un tiempo que vengo dándole vueltas a una idea incómoda: creo que los días del código artesanal, como medida principal del valor de un ingeniero, están terminando.

Este post es sobre un side project que recién empecé — Erebor, una solución de trading de microestructura de alta frecuencia para Binance, enfocada exclusivamente en datos de Level 2 del libro de órdenes. Pero en realidad va de otra cosa: un intento deliberado de pasar de ser alguien que escribe software a alguien que dirige agentes que escriben software. Un arquitecto. Un líder técnico. Un guardián de la calidad.

Quiero ser transparente sobre todo el proceso, incluyendo cómo arrancó este proyecto.

---

## Una preocupación que viene gestándose hace rato

Esto no apareció de la nada. La primera vez que vi a un modelo producir código realmente de calidad de producción —no un fragmento, no un ejemplo de juguete, sino algo que efectivamente publicarías— algo cambió. Empecé a tener la misma conversación una y otra vez: con colegas de la empresa donde trabajo hoy, con gente de afuera, tomando un café, por Slack, a veces simplemente conmigo mismo en el viaje al trabajo.

La conversación siempre va por el mismo camino. Alguien menciona un post que leyó —ya hay muchos— argumentando que el código artesanal como modo por defecto de la ingeniería está terminado. Algunos van más lejos y predicen que dentro de diez años el ingeniero senior típico no escribirá demasiado código. Va a supervisar agentes que lo escriben. Las respuestas en estas conversaciones van desde el rechazo hasta la ansiedad y el acuerdo cauteloso. Yo aterricé en el acuerdo cauteloso, con un agregado: si para allá vamos, prefiero practicar el rol nuevo deliberadamente y no llegar a él de casualidad.

Erebor es esa práctica.

---

## Cómo empezó Erebor — en una conversación de planificación con Claude

No abrí un editor de código para arrancar este proyecto. Abrí una conversación con Claude.

Le describí lo que quería construir y lo que quería sacar de eso. Lo que volvió no fue solo una lista de tareas. Fue una hoja de ruta estructurada de 6 meses, con fases, hitos, un ritmo semanal sugerido y —cuando seguí presionando— un replanteo que yo no había articulado del todo.

En algún momento de esa conversación dije algo como: *este proyecto también es mi entrenamiento para hacer la transición de programar a supervisar agentes*. Y la respuesta me ayudó a ver que esto no era un objetivo secundario. Era el primario. El sistema de trading es el vehículo. El verdadero destino es aprender a operar como arquitecto en un mundo donde los agentes hacen la implementación.

Estoy documentando esa conversación como parte del registro del proyecto. No porque usar una herramienta de planificación con IA sea remarcable —no lo es, en 2026— sino porque creo que la *honestidad* de esa conversación importa. No estaba actuando una visión. La estaba elaborando.

---

## Qué es Erebor en realidad

Erebor es una solución de trading de alta frecuencia con un foco angosto: datos de Level 2 del libro de órdenes de Binance. Sin análisis de sentimiento, sin predicción de precios con ML, sin ruido. Solo microestructura — desbalance del libro de órdenes, dinámica de spreads, detección de órdenes grandes, las señales que viven en la forma del mercado antes de que ocurra una operación.

El stack es Go para el motor principal, TypeScript y Next.js para el dashboard y la UI de estrategias, con prácticas de DevSecOps integradas desde el día uno —no atornilladas al final—. Tengo la certificación CSSLP y parte de lo que quiero demostrar es que la ingeniería segura es una postura, no una fase. El proyecto dura seis meses y termina con un harness de backtesting funcionando y un loop de paper trading contra el Testnet de Binance.

---

## Lo que en realidad estoy entrenando

Acá viene la parte incómoda de decirla en limpio: no estoy construyendo Erebor para mostrar que puedo escribir Go o TypeScript. Lo estoy construyendo para mostrar que no necesito hacerlo.

Lo que quiero decir es esto. El flujo de trabajo al que me estoy comprometiendo para cada feature se ve así:

1. **Arquitectar** — definir la interfaz, el contrato, los criterios de aceptación, los casos límite. Todavía no hay código. Esto es la spec.
2. **Delegar** — entregarle la spec a Claude Code. Dejar que implemente el módulo con tests.
3. **Revisar** — leer el output como un revisor senior. Cuestionar las decisiones de diseño. Rechazar y reprompar si la abstracción está mal, el patrón es inseguro, o la suposición de performance es ingenua.
4. **Aceptar y documentar** — mergear. Escribir un Architecture Decision Record para las decisiones no triviales.

La spec es mi artefacto. El ADR es mi artefacto. La revisión es mi artefacto. El agente escribió el archivo Go.

Esto es lo que un líder técnico hace de verdad — solo que tradicionalmente hay un equipo de ingenieros donde está el agente. La transición que estoy haciendo es aprender a hacer ese trabajo con un colaborador muy rápido, muy capaz y muy literal. Y "muy literal" es la palabra clave. Los agentes implementan exactamente lo que les describiste — incluyendo las partes que describiste mal. La habilidad más difícil en este flujo no es promptear. Es escribir una spec lo suficientemente precisa como para que no haya espacio para una interpretación incorrecta.

Estas son habilidades de arquitecto. Las quiero practicar con cada commit.

---

## Por qué escribo sobre esto

Mi sitio en GitHub Pages es, como yo mismo lo describí, un lugar para anotaciones — cosas que tuve que resolver y que podrían serle útiles a alguien más. Este proyecto encaja exactamente en esa descripción.

Escribo sobre Erebor porque creo que esta transición —de implementador a arquitecto, de escribir código a dirigir agentes— es una que muchos ingenieros con experiencia están enfrentando y de la que no se está hablando con claridad. Hay mucho contenido sobre cómo las herramientas de IA hacen a los desarrolladores más productivos. Hay bastante menos sobre lo que realmente significa *cambiar tu rol* en respuesta a lo que estas herramientas pueden hacer.

Voy a estar posteando a lo largo del proyecto. No solo las victorias. Los rechazos, los re-prompts, los momentos en los que el agente produjo algo que parecía bien y no lo estaba. Las specs que estaban demasiado vagas. Las revisiones que detectaron algo sutil.

Si sos un ingeniero senior preguntándote si el camino de arquitecto / TL tiene sentido para vos en este contexto, espero que esto te resulte útil. Si me estás considerando para un proyecto — así es como pienso y como trabajo.

El proyecto es [Erebor en GitHub](https://github.com/users/edwinabot/projects/3) y acá está el [repo de erebor](https://github.com/edwinabot/erebor). El próximo post va a ser sobre la construcción del servicio de ingestión del libro de órdenes en Go — específicamente cómo era la spec, qué produjo Claude Code, y qué tuve que cuestionarle.

Todavía no sé cuánto de la transición de rol va a sentirse natural y cuánto va a sentirse como pérdida. Probablemente las dos cosas. Eso también vale la pena escribirlo.

---

*Edwin Alexis Abot — abril de 2026*

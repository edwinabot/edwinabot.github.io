---
layout: post
title: "La spec es el código"
date: 2026-05-22
tags: [agentic-ai, software-engineering, spec-driven-development, side-projects]
lang: es
permalink: /2026/05/22/erebor-post-3.html
---

## Nombrando la metodología

Estuve dando vueltas alrededor de algo en estos posts sin nombrarlo directamente. El flujo de trabajo — arquitectar, delegar, revisar, aceptar — es una instancia de **spec-driven development**, o SDD. La afirmación central del SDD es simple: la spec es el artefacto principal. No el código. El código es una consecuencia de la spec, producido por un agente que implementa exactamente lo que la spec describe — incluyendo lo que describe mal.

Esa afirmación tiene consecuencias. Cambia qué es realmente la habilidad de ingeniería en este flujo de trabajo, y cambia qué prácticas existentes terminan importando.

---

## La spec del producto es lo que importa

En SDD la spec del producto ocupa el lugar que antes ocupaba el código. Es aquello de lo que sos dueño. Es de lo que sos responsable. Es donde viven las decisiones de diseño. Cuando algo está mal en la implementación, la primera pregunta no es "¿por qué el agente hizo eso?" — es "¿qué dejó ambiguo la spec?"

Esto es un trabajo más difícil de lo que suena. Escribir una spec lo suficientemente precisa como para que un agente la implemente sin adivinar es una habilidad que tardé varias semanas en empezar a desarrollar, y todavía la estoy desarrollando. La dificultad es que la ambigüedad en una spec es a menudo invisible hasta que la implementación la revela. Creías que habías sido claro. El agente entendió algo diferente. El código es la evidencia.

Todo lo que viene después de la spec — la calidad del código, la cobertura de tests, la limpieza arquitectónica — es función de qué tan bien fue escrita la spec.

---

## Clean Code como mecanismo de verificación

*Clean Code* solía ser sobre cómo escribir buen código. Bajo el SDD es sobre cómo verificar que la implementación realmente expresa la spec.

Cuando Claude Code produce una implementación, mi primera pasada de revisión es una pasada de Clean Code. ¿Los nombres son honestos? ¿La función hace una sola cosa? ¿Hay comentarios compensando lógica oscura? Estas no son preferencias estéticas — son señales diagnósticas. Una función larga con un comentario explicativo me dice que o el modelo no encontró una abstracción limpia, o la spec no restringió la interfaz con suficiente precisión como para producir una. El smell no está en el código. Es evidencia de algo que salió mal en la spec.

El código limpio es el mecanismo por el cual puedo decir, con confianza, que la implementación expresa fielmente lo que la spec pretendía. Cuando el código es oscuro, no puedo hacer esa afirmación. Puedo verificar que los tests pasen. No puedo verificar que lo que se está testeando es lo que fue especificado. Esa es la brecha.

Robert Martin escribía para equipos donde las personas que escribían el código y las que lo revisaban eran personas distintas. Resulta que esa es exactamente la situación en el SDD — excepto que el autor es un agente al que no se le pueden hacer preguntas de seguimiento. La legibilidad no es una preferencia; es el único canal a través del cual la spec y la implementación pueden compararse.

---

## Clean Architecture y el loop spec-arquitectura

La arquitectura y la spec no tienen una secuencia limpia. No podés arquitectar completamente un producto que no fue especificado — no sabés qué necesita hacer el sistema, así que no sabés cuáles deberían ser sus límites. Pero tampoco podés especificar completamente una feature sin conocer las restricciones arquitectónicas — no sabés qué es posible, qué está prohibido, o contra qué capa estás especificando.

Algunas decisiones pueden venir primero. Las elecciones tecnológicas — Go para el motor principal, TypeScript para la UI, TimescaleDB para la persistencia de series temporales — son más como axiomas que como arquitectura. No requieren una spec completa del producto; requieren una comprensión del dominio del problema y un conjunto de preferencias sobre herramientas. Podés fijarlas temprano y construir desde ahí.

Pero las decisiones más profundas — cómo se dividen las capas, dónde se ubican los límites, cuáles son las reglas de dependencia — esas emergen del producto. Necesitás saber que estás ingiriendo diffs del libro de órdenes a alta frecuencia antes de poder decidir que la capa WebSocket no debe llegar a la capa de persistencia. Necesitás saber que múltiples símbolos corren concurrentemente antes de poder diseñar la arquitectura de dispatch interno. La spec revela lo que la arquitectura debe acomodar. La arquitectura revela lo que la spec puede demandar con precisión.

Lo que Clean Architecture aporta al SDD es disciplina sobre este feedback loop. A medida que la spec y la arquitectura co-evolucionan, las reglas de Clean Architecture — abstracciones estables, dependencias acíclicas, definiciones claras de límites — mantienen ambos lados coherentes. Sin esa disciplina, el loop produce capas enredadas imposibles de especificar limpiamente. Con ella, el loop eventualmente se estabiliza: el producto está lo suficientemente comprendido, los límites están lo suficientemente claros, y llegás al momento de delegación con algo preciso contra lo que escribir una spec.

El ADR es donde ese loop se cierra. Es el registro de las decisiones que estabilizaron el feedback — no las primeras decisiones tomadas, sino las que se sostuvieron.

---

## Lean y la paradoja del recién llegado

El Lean Software Development tiene un principio al que sigo volviendo: decidir lo más tarde posible. No comprometerse con una decisión irreversible hasta que sea necesario. Diferir hasta el último momento responsable, cuando la información necesaria para decidir bien ha tenido tiempo de acumularse.

Todavía estoy trabajando cómo aplicar esto al SDD. Hay una situación específica que vengo llamando la Paradoja del Recién Llegado desde el post anterior — la tentación de seguir definiendo specs e inmediatamente dárselas a Claude para implementar. La velocidad es real. El volumen de lo que podés construir en una sesión está más allá de lo que experimenté antes. Y a veces tengo que resistir activamente la tentación de simplemente generar lo siguiente.

El principio Lean es el contrapeso. No toda decisión necesita una spec hoy. No toda spec necesita una implementación en esta sesión. Algo de lo que estoy tentado de generar ahora se beneficiaría de dejarlo reposar un día — de preguntarme qué todavía no sé antes de escribir lo que creo que sí sé.

El principio de eliminar el desperdicio corta en la otra dirección. El exceso de especificación también es desperdicio. Un prompt de delegación que especifica lo que el agente habría hecho bien de todas formas es ruido que aumenta la probabilidad de malinterpretación. La spec debe ser exactamente tan larga como para ser inequívoca, y no más.

---

## Qué pasa cuando corrés agentes en paralelo

La pregunta de la velocidad tiene una segunda dimensión que todavía no describí: el paralelismo.

Durante las primeras semanas de Erebor, estaba corriendo Claude Code en forma secuencial — una spec, una sesión de implementación, una revisión. Eso ya es rápido. Pero Claude Code soporta worktrees aislados, lo que significa que podés correr múltiples agentes simultáneamente, cada uno en una rama separada, cada uno implementando una spec diferente, sin que interfieran entre sí. Las ramas se mantienen limpias. Las revisiones se mantienen independientes.

Estuve coordinando estas sesiones en Warp. Múltiples paneles, cada uno corriendo una sesión de Claude Code en su propio worktree. Escribís dos o tres specs a la mañana, levantás los worktrees, lanzás agentes en los paneles, y los dejás correr. Después revisás cada uno por turno. La fase de implementación — que antes serializaba todo — se vuelve paralela. El cuello de botella se desplaza completamente hacia la revisión y la calidad de la spec, que es donde debería estar.

Lo que esto produce es una experiencia diferente del tiempo en un proyecto. La pregunta deja de ser "¿cuándo va a estar lista esta feature?" y pasa a ser "¿cuántas specs puedo sostener con suficiente claridad en mi cabeza para correr en paralelo y revisar con precisión?" Esa es una pregunta cognitiva, no de planificación. Todavía no encontré el techo. Tres sesiones en paralelo se siente sostenible. Cuatro es posible si las specs son relativamente independientes. Más que eso y la calidad de la revisión empieza a degradarse, lo cual derrota el propósito.

Vale la pena nombrar Warp específicamente acá porque su gestión de sesiones y su layout de paneles hacen que esta coordinación sea práctica en lugar de solo teóricamente posible. Cada panel tiene contexto. Podés scrollear hacia atrás, correr comandos junto a la salida del agente, y cambiar entre sesiones sin perder estado. No es una feature revolucionaria, pero el flujo de trabajo se rompe sin ella.

---

## Lo que no puedo creer

Describir lo que se siente combinar herramientas a este nivel es difícil sin sonar hiperbólico, pero lo intento.

Kiro agrega otra dimensión — orquestación agéntica a nivel de IDE que va más allá de lo que Claude Code maneja solo. Las capacidades de IA de Figma están por delante de mí en la capa del dashboard que todavía no construí. Lo que sé por los experimentos tempranos es que hay un umbral donde las herramientas dejan de sentirse como aceleración y empiezan a sentirse como algo cualitativamente diferente. La comparación no es "trabajar más rápido." Se parece más a "operar a una escala diferente." Más cosas en vuelo, más decisiones ejecutándose en paralelo, más superficie bajo desarrollo en cualquier momento dado.

La restricción que queda es la que siempre permanece: la spec. Todo lo demás está aguas abajo de si la spec fue escrita bien.

---

## Los fundamentos y la nueva pregunta

Los fundamentos no importan menos bajo el SDD. Los principios de Clean Code son los criterios para revisar el output generado por IA. Clean Architecture es el prerequisito para escribir software especificable. Los principios Lean son la guía para cuándo especificar y cuándo esperar.

El cambio está en para qué son estas prácticas. Fueron diseñadas para un mundo donde los ingenieros escribían el código. Resultan ser exactamente lo que necesitás en un mundo donde lo revisás, lo especificás, y decidís cuándo está listo — y un agente lo produce.

La nueva pregunta que están respondiendo no es "¿cómo escribo mejor código?" Es "¿cómo escribo mejores specs?"

---

*Edwin Alexis Abot — Mayo de 2026*

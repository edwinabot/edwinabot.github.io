---
layout: post
title: Revisiones de código efectivas y eficientes
date: 2023-05-15 08:03:00-0300
description: o cómo ser amable y profesional al programar y revisar con otros
tags: pr codereviews teamwork
categories: good-practices
related_posts: false
lang: es
permalink: /good-practices/2023/05/15/pull-requests-code-reviews.html
---
En todos los proyectos en los que participé, observé variaciones de los mismos desperdiciadores de tiempo y de sus efectos sobre el equipo durante el proceso de revisión de código. En este post voy a recopilarlos, explicarlos brevemente junto con sus efectos secundarios, y describir prácticas que fomenté e implementé cuando fue posible.

Empiezo resumiendo un proceso de code review:

>Después de trabajar en una nueva feature o de arreglar un bug, le pedimos a nuestros pares que revisen nuestro trabajo abriendo un pull request.
>La mayor parte del tiempo, nuestra colaboración cumple los criterios de calidad del equipo, pero a veces no. Entramos en debates, recibimos feedback y hacemos ajustes. **Eventualmente**, nuestro código pasa la revisión y se libera a producción.

Vi que las revisiones de código son valiosas para mejorar la calidad del código e identificar problemas potenciales. Sin embargo, hay **desperdiciadores de tiempo** específicos que pueden afectar la efectividad y eficiencia de las revisiones, impactando directamente en los tiempos de entrega y en la moral del equipo. Tuve la oportunidad de levantar estos desperdiciadores como issues en muchas reuniones de retrospectiva y de colaborar en su mitigación.


## Desperdiciadores de tiempo

Sin un orden particular, estos son los principales desperdiciadores de tiempo durante las revisiones de código:

**Falta de claridad en el envío del código:** Cuando el envío no es claro o le falta contexto suficiente, los revisores pueden gastar una cantidad de tiempo poco razonable tratando de entender el propósito del código o cómo encaja en el sistema del que forma parte. Explicaciones claras y concisas que acompañen al código ayudan a reducir esa pérdida de tiempo.

**Changesets grandes o demasiado complejos:** Revisar changesets grandes o código complejo puede consumir mucho tiempo. Dividir los cambios en piezas más chicas y manejables ayuda a los revisores a enfocarse en áreas específicas y a dar feedback más certero.

**Documentación incompleta o ausente:** Documentación insuficiente dentro de la base de código puede llevar a los revisores a dedicar tiempo extra descifrando la funcionalidad o intención del código. Código bien documentado y comentarios breves ayudan a los revisores a entender el código más rápido y a dar feedback más relevante.

**Falta de adherencia a los estándares de código:** Código que se aparta de los estándares establecidos o de las buenas prácticas puede llevar a discusiones y debates largos durante las revisiones. Adherir consistentemente a las pautas acordadas minimiza este desperdicio.

**Discusiones excesivas de ida y vuelta:** Si las revisiones se vuelven debates largos sin resoluciones claras, consumen tiempo valioso. Promover discusiones concisas y enfocadas, fijar un límite de tiempo, o involucrar a un mediador ayuda a evitar idas y vueltas excesivos.

**Comentarios no accionables o subjetivos:** Comentarios vagos o subjetivos pueden generar confusión y pedidos adicionales de aclaración, ralentizando el proceso de revisión. Los revisores deben dar feedback específico y accionable, sugiriendo mejoras concretas o señalando problemas potenciales.

**Sobrecarga de revisores:** Si un grupo chico de revisores ya saturado con otras tareas también es asignado a code review, se generan demoras y backlogs. Distribuir la carga entre varios revisores o adoptar un sistema de rotación previene el burnout y acelera el proceso.

**Falta de herramientas y chequeos automatizados:** La inspección manual de cada línea de código consume tiempo y es propensa a errores. Herramientas automáticas, como análisis estático o linters, ayudan a detectar problemas comunes y reducen el esfuerzo manual durante la revisión.

Más adelante voy a describir un conjunto de prácticas que evitaron, si no por completo, en gran medida los efectos de estos desperdiciadores. La mayoría fueron listadas como action items en retrospectivas y adoptadas después.


## Impacto de los desperdiciadores en la moral del equipo

Como decía antes, discutí estos desperdiciadores en retrospectivas, lo que me permitió hablar de los sentimientos a su alrededor y de cómo nos sentíamos mis compañeros y yo. Nos permitió analizar el impacto en el equipo y en nuestra capacidad de entrega.

**Frustración y desmotivación:** Algunos miembros del equipo se frustran con el proceso cuando las revisiones se vuelven costosas en tiempo por distintas ineficiencias. La sensación de estar perdiendo tiempo en una actividad improductiva los desmotivó y les bajó el entusiasmo por participar en code reviews.

**Productividad disminuida:** Tiempo excesivo en revisiones le quita tiempo y energía necesarios para otras tareas esenciales como programar o monitorear. Lleva a demoras en los timelines del proyecto y a una caída en la productividad, impactando la capacidad del equipo de entregar resultados eficientemente.

**Falta de compromiso y participación:** Los miembros del equipo se desconectan gradualmente si las revisiones son consistentemente largas e improductivas. Las perciben como una carga más que como una oportunidad valiosa de colaboración. Esa desconexión resulta en menos personas participando activamente, lo que reduce la diversidad de perspectivas y la efectividad general del proceso.

**Tensión y conflicto:** Debates prolongados o discusiones sin resolver durante las revisiones generaron tensión en el equipo. Desacuerdos sobre cuestiones subjetivas o nitpicking excesivo llevaron a conflictos interpersonales, dañando la dinámica del equipo y su moral. Crearon un clima hostil durante las revisiones, desalentando la comunicación abierta y la colaboración.

**Calidad y orgullo en el trabajo:** las revisiones no identificaron y abordaron problemas con efectividad; pasar por alto esos problemas permitió mergear código de menor calidad. Eso erosionó el sentido de orgullo del equipo en su trabajo, y lo percibieron como que los estándares de calidad de la entrega estaban comprometidos. Una caída en la calidad del código también lleva a más esfuerzo de mantenimiento y a problemas potenciales.

**Retención y rotación:** Frustraciones prolongadas y desconexión derivadas de los desperdiciadores en las revisiones contribuyeron a una baja moral y, en última instancia, impactaron la retención. Algunos compañeros empezaron a buscar oportunidades en otro lado tras sentirse consistentemente improductivos y perdiendo el tiempo.


## Optimizando la eficiencia de las revisiones

> _TL;DR: la eficiencia es una cuestión de voluntad._

Lo que quiero decir con esa frase es que, no importa qué nivel de automatización tenga tu proceso de code review, el eslabón más débil de la cadena es la voluntad de las personas para hacer las cosas con consciencia y a fondo. Esa debilidad vive en quienes programan y en quienes revisan.

Podemos reducir el desperdicio al mínimo siendo conscientes de nuestro uso del tiempo y del tiempo de los demás. Pensar: "voy a hacer todo lo que pueda para que Jane me dé feedback accionable en el menor tiempo posible", o "primero, me voy a asegurar de entender en qué está trabajando John", son buenos puntos de partida que pueden guiar nuestras acciones como colaboradores.

La idea de fondo es que queremos ahorrarnos tiempo, y ahorrárselo a otros, para disfrutar de otras actividades. Ser eficiente y efectivo en el trabajo nos ahorra tiempo, y ser eficiente se vuelve esencial cuando pasamos el tiempo de nuestras vidas trabajando, literalmente. Así que aprovechemos mejor nuestro tiempo y no desperdiciemos el de los demás.

Te comparto un conjunto de buenas prácticas que recopilé con el tiempo y que te van a ayudar a ser más eficiente como revisor y como autor.

### Buenas prácticas para el autor del PR

> _TL;DR: dá contexto y acotá el alcance._

La estrategia es maximizar la salida de los revisores, minimizando el tiempo que necesitan para revisar el código. Se trata de acotar el alcance y de dar contexto. La táctica es usar estas prácticas recomendadas:

**Dale al PR un título corto y significativo:** el título suele ser la primera impresión, y las primeras impresiones importan. Acá es donde le entregás al revisor la primera pieza de contexto.

**Escribí una descripción corta y bien organizada del PR:** si tu equipo todavía no tiene una plantilla de PR, podés empezar a llenar la descripción respondiendo tres preguntas simples: ¿Qué? ¿Por qué? ¿Cómo? Estás explicando qué estás haciendo, por qué lo estás haciendo y cómo lo hiciste. Proponele al equipo una plantilla de PR para futuras colaboraciones.

**Seguí las convenciones:** asegurate de seguirlas. Tu comunidad fomenta convenciones porque demostraron su valor en miles de proyectos. Las podés ver como estándares; los estándares hacen que producir y comunicar sea más fácil. Habrá un momento en que vas a tener que ser creativo porque no te están ayudando, pero la mayor parte del tiempo son suficientes.

**Mantené los PR por debajo de 400 líneas de código (LOC):** esto es bueno por dos razones. Primero, porque la atención del revisor es limitada: cuanto más código tenga que leer, más se le debilita la atención con el tiempo, así que mantenelo corto o partilo. Segundo, densidad de errores: encontrar un error en demasiado código es como encontrar una aguja en un pajar, así que si hay mucho código y hay errores, va a ser más difícil detectarlos por el solo volumen de cosas a analizar.

**PRs atómicos:** resolvé un problema a la vez; tu PR debería atender uno y solo un asunto. Podés dividir una feature en varias piezas y combinarlas más adelante en la rama de la feature. Un bug suele requerir un solo PR, salvo que se distribuya en varios repositorios.

**Anotá tu código:** el código fuente es para humanos, no para máquinas, así que hacé que leerlo sea una buena experiencia. Va a haber piezas difíciles de seguir; lo vi sobre todo cuando se expresan reglas de negocio complejas, así que agregar comentarios para dar contexto es bueno. Pensá también en el vos del futuro o en un colega que llegue a este pedazo de código a refactorizarlo. ¿No es lindo recibir el contexto que llevó a esta implementación?

**Escribí tests:** escribir tests es la mejor práctica; no puedo enfatizarlo lo suficiente; es la práctica de codificación que todas las buenas prácticas quieren ser. En el contexto de un code review, los tests le explican al revisor el comportamiento de tu código; también muestran qué tan profundamente pensaste el código que escribiste. Son, además, otra forma de documentación.

### Buenas prácticas para el revisor

> _TL;DR: conseguí los requisitos, sé minucioso y dá feedback accionable._

Como revisor, es tu responsabilidad asegurar que la calidad de la entrega sea buena. Es enriquecedor para todos si te esforzás por analizar el código con una actitud reflexiva, paciencia y un espíritu de escepticismo sano. Siempre podés buscar aprender y criticar constructivamente la colaboración de otros. Para hacerlo, te recomiendo:

**Entendé a fondo la feature o el bug:** aunque parezca obvio, este entendimiento se pasa por alto más seguido de lo que pensás. Es más fácil validar la implementación con un buen conocimiento del bug o de la feature. De lo contrario, no sos un guardián del valor de entrega esperado. Si no entendés la entrega esperada, te enfocás simplemente en la corrección técnica, que es importante, pero no tanto como el valor agregado.

**Time box:** tenés muchas tareas que cumplir, y el code review es una de ellas, así que asignale tiempo a propósito. Tomá un PR a la vez y asignale tiempo para hacerlo. Tu atención cede con el tiempo, así que dedicale a lo sumo una hora por sesión de revisión. Tratá de mantenerte enfocado durante ese tiempo. Hacer time boxing te ayuda a organizar tus tareas.

**Validá si la colaboración arregla el bug o entrega la feature:** esto es fundamental. De nuevo, sos un guardián del valor de entrega. Si es un bug, tenés que asegurarte de que lo arregle. Si es una feature, tenés que asegurarte de que cumpla todos los requisitos.

**Dá feedback significativo y accionable:** vas a encontrar algo que merezca un comentario, sea un code smell, una mala práctica, o un bloque poco claro que te parezca incorrecto; eso te va a obligar a producir un comentario, y ese comentario tiene que ser una pieza de feedback accionable. Es una oportunidad para ayudar a un compañero a aprender algo nuevo sobre la tecnología en juego, el negocio, etc. La mejor manera de aprender es enseñar, así que si tenés ganas de aprender, aprovechá la oportunidad, juntá recursos y referencias, y armá una microclase.

**Revisá a tiempo:** es esencial hacerlo a tiempo, o estamos demorando o bloqueando el progreso de algún trabajo. Al revisar a tiempo también evitás un cambio de contexto al compañero que construyó la solución. Algunos colegas van a tomar otra tarea apenas esté disponible; querés que se contengan y que mantengan caliente su conocimiento de la solución que estás revisando, haciéndolo rápido y eficazmente. Revisar a tiempo también evita generar ansiedad en los colaboradores.

**No te dejes condicionar por la seniority:** te vas a encontrar revisando código de un compañero más senior. No te frenes en levantar un defecto por su seniority; tarde o temprano va a cometer un error, y vos tenés que estar bien encontrando ese defecto. Y cuando pase, tenés que reconocerlo y comunicarlo con un buen comentario, una pieza de feedback accionable y significativa. A veces los devs senior tienen que prestarle más atención a cosas básicas, como escribir un test.

**Sé un promotor de las buenas prácticas y convenciones:** si hay una forma más idiomática o una solución mejor establecida para un problema común, comunicale al colaborador y pedile que siga las convenciones. De nuevo, dá feedback accionable y significativo, y aprovechá la oportunidad para reforzar buen conocimiento enseñando a otros.


### Ejemplos de code reviews

Me gusta explorar proyectos Open Source para aprender cómo hacen las cosas. Estas herramientas increíbles las construyen miles de contribuidores, y unos pocos son miembros del core team que cuidan las buenas prácticas; estos proyectos son elementos centrales de productos que llegan a millones de personas en todo el mundo. ¿Y adiviná qué? Estos equipos también hacen code reviews. Te dejo una lista para que los explores:

* [Un bug en Django](https://github.com/django/django/pull/16835)
* [Un refactor en GitLab](https://gitlab.com/gitlab-org/gitlab/-/merge_requests/13703#2c3621e9eaa266f4e1db62954ec460a38c45d8b2)
* [Un bug en Rails](https://github.com/rails/rails/pull/48220)
* [Un refactor en Python](https://github.com/python/cpython/pull/104547)
* [Una feature en Spring Boot](https://github.com/spring-projects/spring-boot/pull/10944)

En algunos casos escriben una descripción completa en el PR, en otros casos enlazan al issue que contiene toda la discusión. Tratá de seguir los enlaces de estos PRs y MRs y aprendé cómo colaboran estos equipos.

## Cierre

Siempre hay lugar para mejorar nuestros procesos. Experimentamos distintas formas de implementar el Software Development Lifecycle (SDLC) en cada proyecto del que formamos parte.

En este evento particular del SDLC, el code review, hay lugar para la controversia, las batallas de ego y los síndromes del impostor. Tenemos que ser amables y humildes; eso construye un espacio de colaboración seguro y fructífero.

Quiero cerrar con una lista de pensamientos que guiaron la escritura de este post:


* Quiero evitar la frustración del ida y vuelta sin sentido y sin fin en las revisiones de código.
* Quiero dejar de perder el tiempo, recuperarlo para mí, y usarlo en otras actividades gratificantes como estudiar música o pasar tiempo con mi familia.
* Quiero permitirles a otros hacer lo mismo.

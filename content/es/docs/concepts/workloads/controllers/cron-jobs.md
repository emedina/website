---
title: CronJob
content_template: templates/concept
weight: 80
---

{{% capture overview %}}

Un _Cron Job_ ejecuta [Jobs](/docs/concepts/workloads/controllers/jobs-run-to-completion/) según un horario programado en el tiempo.

Un objeto CronJob es como una línea de un archivo _crontab_ (tabla cron). Ejecuta un trabajo de forma periódica
según un horario programado, escrito en formato [Cron](https://en.wikipedia.org/wiki/Cron).

{{< note >}}
Todos los `horarios` **CronJob** se basan en la zona horaria del máster donde se inicia el trabajo.
{{< /note >}}

Para instrucciones acerca de cómo crear y trabajar con trabajos programados, y por ejemplo un ejemplo de un archivo de especificación
de un trabajo programado, ver [Ejecutar tareas automatizadas con trabajos programados](/docs/tasks/job/automated-tasks-with-cron-jobs).

{{% /capture %}}


{{% capture body %}}

## Limitaciones de los trabajos programados

Un trabajo programado crea un objeto job _alrededor de_ una vez por cada ejecución de su programación. Decimos "alrededor de" porque
hay determinadas circunstancias bajo las cuales dos trabajos pueden crearse, o ninguno de ellos se crea. Se intenta que estos casos sean residuales,
pero no se puede evitarlos de completamente. Por lo tanto, los trabajos deberían ser _idempotentes_.

Si el valor de `startingDeadlineSeconds` se establece a un valor grande o se deja sin especificar (por defecto)
y si el valor de `concurrencyPolicy` se establece a `Allow`, los trabajos siempre se ejecutarán por lo menos una vez.

Para cada CronJob, el controlador de CronJob chequea cuántas programaciones se han perdido desde la última programación hasta el momento actual. 
Si hay más de 100 programaciones perdidas, entonces ya no vuelve a ejecutar el trabajo y registra el error:

````
Cannot determine if job needs to be started. Too many missed start time (> 100). Set or decrease .spec.startingDeadlineSeconds or check clock skew.
````

Es importante destacar que si el campo `startingDeadlineSeconds` se configura como (not `nil`), el controlador cuenta cuántos trabajos perdidos se produjeron desde el valor de `startingDeadlineSeconds` 
hasta ahora en vez de la última programación hasta el momento actual. Por ejemplo, si `startingDeadlineSeconds` es `200`, el controlador cuenta cuántos trabajos perdidos se produjeron en los últimos 200 segundos.

Se cuenta un CronJob como perdido si no se ha podido crear a la hora programada. Por ejemplo, si establecemos el valor de `concurrencyPolicy` a `Forbid` y se intentó programar 
un CronJob cuando otro previamente programado estaba todavía ejecutándose, entonces contará como perdido.

Por ejemplo, imagina que un CronJob se configura para programar un nuevo Job cada minuto a partir de las `08:30:00`, y su campo 
`startingDeadlineSeconds` no se configura. Si el controlador del CronJob no estuviera disponible de `08:29:00` a `10:21:00`, 
el trabajo no comenzaría porque el número de trabajos perdidos que se habría perdido en su programación sería superior a 100.

Para ilustrar este concepto mejor , vamos a suponer que programamos un CronJob para que ejecute un nuevo Job cada minuto comenzando a las `08:30:00`, y establecemos el valor del campo
`startingDeadlineSeconds` a 200 segundos. Si el controlador del CronJob no se encuentra disponibles
durante el mismo período que en el ejemplo anterior (`08:29:00` a `10:21:00`,) el Job aún así comenzará a las 10:22:00. 
Esto ocurre porque el controlador ahora comprueba cuántas programaciones perdidas ha habido en los últimos 200 segundos (esto es, 3 programaciones perdidas), en vez de comprobarlo a partir de la última programación hasta el momento actual.

El Cronjob es únicamente responsable de crear los Jobs que coinciden con su programación, y
el Job por otro lado es el responsable de gestionar los Pods que representa.

{{% /capture %}}

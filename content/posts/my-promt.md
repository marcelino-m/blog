---
title: "Mi prompt"
date: 2020-05-20T00:37:02-04:00
draft: true
toc: false
images:
tags:
  - bash
  - prompt
  - productividad
---

Cómo todo  desarrollador que  se respeta  uso la consola  y la  uso un
montón!.  En este  post quiero compartir justamente eso,  mi promt. La
idea la  tome prestada del proyecto  [Oh My BASH!][1] cuando  le di un
chance hace  un tiempo pero desistí  de él por tener  demasiadas cosas
que no usaba, yo solo quería un prompt!.

## El prompt por defecto

EL típico prompt por defecto es el siguiente:

![default prompt](/my-prompt/default-prompt.png)

El problema con este prompt es que  si estoy muy adentro en el sistema
de  archivos o  bien el  directorio en  el que  me encuentro  tiene un
nombre muy largo se pierde mucho  espacio, y así es imposible trabajar
cómodamente cuando por  ejemplo quiero hacer un  split horizontal. Una
posible solución es solo mostrar el nombre del directorio actual, pero
con esto  se pierde  visibilidad de  la ruta  completa.  Junto  con lo
anterior,  encuentro  que este  prompt  entrega  muy poca  información
contextual.

![prompt folders too long](/my-prompt/prompt-folder-too-long.png)


## El prompt que uso
En una búsqueda por balancear el minimalismo, por una parte, y maximizar
la información  que entrega el prompt,  por otra. Es que  desarrolle el
siguiente prompt:

![new prompt](/my-prompt/new-prompt.png)

Con  este prompt  siempre tengo  el  mismo espacio  para ejecutar  mis
comandos, no importa que tan largo sea el path en el que me encuentre

![prompt folders too long](/my-prompt/new-folder-too-long.png)


Si  estoy dentro  de un  repo  me informa  en que  rama estoy  cuantos
archivos modificados nuevos o no trackeados hay

![new prompt](/my-prompt/new-prompt-repo.png)

Si tengo activado un virualenv

![new prompt](/my-prompt/new-prompt-venv.png)

Si estoy  conectado por ssh  a otra  maquina, el comportamiento  es el
mismo pero el prompt en vez de ser purpura se presenta como rojo, para
mi esto es muy útil, ya que sin pensar y solo con mirar se si estoy en
una terminal local o remota.

![new prompt](/my-prompt/new-prompt-remote.png)

[1]: https://ohmybash.github.io/

---
title: "El padrón electoral del servel y apellidos en peligro de extinción "
date: 2018-01-05T23:11:06-03:00
toc: false
images:
tags:
  - unix
  - servel
---

## El padrón electoral del Servel

[Servel][1] expone de forma irresponsable (obligado por la legislación
actual) los datos del padrón  electoral, cuyo atributo mas sensible es
la dirección electoral  al presentarse junto al nombre y  R.U.T de los
habilitados para votar, que son casi todos los mayores de 18 años.

En concreto es la ley [18\.556][2] la que obliga al Servel a presentar
junto con  el padrón electoral  los datos  de tu nombre,  R.U.T, sexo,
dirección electoral y mesa de votación.

Este post no  es para hablar sobre el peligro  que representa para una
sociedad el que no se trate con el debido cuidado información sensible
de cada uno de nosotros ni tampoco lo fácil que es construir un perfil
masivo  con información  que se  puede  encontrar de  forma pública  y
complementarse con otras fuentes que se pueden adquirir con un poco de
dinero y tiempo por cualquier persona u organización.

Hoy comentaré  sobre algunos datos  "Freak" e inocentes que  se pueden
extraer desde esta  base de datos.

## Datos "Freaks"!

1. Nombres menos/más  frecuentes? cuales?.
2. Apellidos menos/más  frecuentes? cuales?.
3. Hay personas con nombres únicos?, cuantas? cuales?.
4. Hay personas con apellidos únicos?, cuantas? cuales?.
5. **Hay apellidos en peligro de extinción?**.
6. Cuales son los R.U.T mas antiguos?, lo cual puede inferir que son las personas con mayor edad.

Con **apellidos en peligro de extinción** me refiero a un apellido que
solo lo  tenga una o  muy pocas personas, y  que estas tengan  una muy
baja probabilidad de  reproducirse, como personas de  la tercera edad,
esta condición se  puede inferir a partir del  R.U.T. Otra posibilidad
es que  sean personas realmente  desagradables y nadie  quiera copular
con ellos pero  esto ya es mas  difícil de estimar (con  los datos del
padrón).

Hace tiempo, quizás no tanto, me encontraba procrastinado y el mono de
la satisfacción instantánea que está en mi cabeza me sugirió jugar con
ésta BD, cómo resultado me lleve  la grata sorpresa que mi amada novia
tiene el  privilegio de  ser una  de ésas  personas que  tienen nombre
único.

## Show me the source Luke...

El primer paso es transformar casi los 2 GB de PDF's (el formato de la
BD  es un  conjunto  de  PDF's) que  puedes  encontrar  en internet  o
pedirlos directamente en las oficinas del Servel, a un formato que sea
de fácil  manipulación como lo  es formato  CSV. Con esto  logrado, el
paso siguiente  es hacer algo  de magia  con tu lenguaje  de scripting
favorito.

Bueno bueno, este post ya se ha alargado lo suficiente (en realidad me
esta dando paja  seguir escribiendo), así que dejare  el código fuente
para el siguiente post.


- Si este post  te ha gustado no dudes en compartirlo
- Si has llegado hasta aquí gracias totales!


[1]: https://www.servel.cl/
[2]: https://www.servel.cl/inscripciones-electorales-y-servicio-electoral/



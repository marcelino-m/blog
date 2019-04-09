---
title: "El padrón electoral del Servel en tu base de datos"
date: 2018-02-06T23:11:34-03:00
toc: false
images:
tags:
  - unix
  - servel
---

En este  post mostrare una técnica  para parsear los datos  del padrón
electoral del  Servel que vienen  en formato PDF's y  transformarlos a
formato  CSV, así  puedes trabajar  con  ellos y  hacer lo  que se  te
ocurra, como encontrar [apellidos en peligro de extinción](/posts/servel-uno),
construir tu
propio [rutificador](https://rutchile.cl/), ingresar
eso datos a SQLite, Postgres, MongoDB, o Exel si prefieres.

Al final del post agregare un link a un script que junta todo lo visto
aquí para automatizar el proceso y  lo puedas hacer tu mismo (llegar y
usar).

## La fuente de datos


Los datos del padrón electoral vienen  como un conjunto de archivos en
formato PDF agrupados por comuna lo que significa que en total son 346
documentos con  cientos de páginas  cada uno,  en total esta  "base de
datos" pesa 1.5 GB (que no es tanto).

A continuación una  página de un documento cualquiera de  esta base de
datos,  cabe destacar  que todas  las páginas  de cada  archivo siguen
exactamente el mismo formato:

![ejemplo pdf servel](/servel-post/pdfservelejemplo.png)



## Como se hace?

El  primer paso  es usar  una herramienta  llamada pdftotext  que hace
justamente lo  que su nombre  indica, transforma PDF's en  archivos de
texto plano (y a las personas  como yo nos encantan los textos planos,
ya que son simples y muy versátiles)

El programa se ejecuta de esta forma:

```bash
pdftotext -layout /path/to/file.pdf > file.txt
```

El resultado es un archivo de  texto plano, con el flag **-layout** el
programa pdftotext intenta mantener el formato del documento original.

El resultado luce algo así:

![pdftotext](/servel-post/pdf-servel-ejemplo-txt.png)

El  siguiente  paso es  eliminar  secciones  del  archivo que  no  nos
interesan:  la parte  del recuadro  en Verde  que he  remarcado.  Este
recuadro  corresponde al  cabecera  de  cada una  de  las páginas  del
documento que acabamos  de procesar.  Nuestro archivo  de texto tendrá
tantos de estos recuadros como páginas tenga el documento original.



Este patrón  se repite tantas veces  por cada archivo que  lo mejor es
usar  alguna  herramienta que  haga  el  trabajo sucio  por  nosotros.
**sed** es tu turno!:

```bash
cat file.txt | sed -e "/REPUBLICA/,/NOMBRE.*SEXO.*MESA$/d" > file-woh.txt
```

Ahora nos queda la  ultima parte, que es la de  detectar y separar los
respectivos campos  del documento para  generar el CSV.   El resultado
intermedio que  tenemos presenta algunas complicaciones  que hacen que
esta tarea no sea directa, estas complicaciones aparecen alrededor del
30% de los registros, a continuación presentamos algunas:

1.   Aveces  un registro  aparece  desplegado  en  mas de  una  línea,
observemos el recuadro en Rojo

2. No todos  los registros tienes todos  sus campos si te  fijas en el
recuadro Azul hay dos registros que no tienen domicilio electoral.

3. La separación entre campos  no es constante, en diferentes registros
la **dirección  electoral** y  la **circunscripción**  están separados
por 3 espacios , aveces  por más y como  se menciono en  el punto
anterior aveces no  existe esta información, lo mismo  ocurre en otras
partes   entre  la   circunscripción  y   la  mesa   electoral.

Para corregir el prunto **1**  ocuparemos la observación de que cuando
un registro aparece  desplegado en múltiples líneas  ocupa como máximo
cuatro, Este script busca este patrón y corrige el error:

```bash
cat file-woh.txt | sed -e "N;N;N;N; s/\n  */ /; P; D" > file-woh-fix1.txt
```

En este punto  ya tenemos un archivo con todos  sus registros ocupando
solo una  línea, lo  que queda  es generar  el CSV,  esto nos  lleva a
resolver el punto **2** y **3**

Para esto  ocuparemos como "pivote" el  campo sexo (con valores  VAR o
MUJ) observando  que la separación  de este  campo con respecto  a los
otros se  mantiene constante  y es  de exactamente  3 espacios  (en un
registro con dirección electoral y de 4 o más, a su derecha, cuando el
registro no posee  esta información, este patrón  nos permite detectar
el punto  **2** ). El  punto **3** se  simplifica al haber  resulto el
**2** ya  que se observa que  se puede esperar  a que el resto  de los
campos están separados por al menos 3 espacios.

El siguiente script aplica esta heurística a cada registro del archivo

```bash
read -r -d '' SEDSCRIPT << EOS
  s/\s\{3,\}MUJ\s\{4,\}/;MUJ;;/
  s/\s\{3,\}VAR\s\{4,\}/;VAR;;/
  s/\s\s\sMUJ\s\s\s/;MUJ;/
  s/\s\s\sVAR\s\s\s/;VAR;/
  s/\s\{3,\}/;/g
EOS

cat  file-woh-fix1.txt | sed -e "$SEDSCRIPT" > file.csv

```

El resultado es un  es un CSV con separador de campo "  **;** ". Yo he
aplicado estos pasos  a todos los PDF's del padrón  electoral del 2016
que  contiene 1,384,620  registros, el  error  que se  obtiene es  del
0.58%.

## Hazlo tu!

Puedes descargar un script de que realice [aquí][1] para transformar uno o todos
los PDF, con la opción de generar un CSV con todos los resultados o un CSV
por cada PDF:

[![asciicast](https://asciinema.org/a/141371.png)](https://asciinema.org/a/141371)

[1]: https://github.com/marcelino-m/servel/blob/master/script/pdfservel2csv.sh

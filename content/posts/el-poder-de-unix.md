---
title: "El poder de Unix al rescate "
date: 2017-09-09T22:29:26-03:00
toc: false
images:
tags:
  - unix
---

### El problema

Hace poco tiempo atrás me  encontraba con el siguiente misión:

Generar 34 millones de registros alfanuméricos (solo letras y números)
únicos, aleatorios, de  largo 7. Todos tenían  que tener estrictamente
al menos  una minúscula, una  mayúscula y  un número.  Por  último los
registros debían ser  guardados en un CSV de 37  columnas.

Un ejemplo de  un registro valido seria: **dUH08XG** ,  y de registros
no validos: **duHaGxg** ni tampoco: **duh08xg**

### La solución

Solo  usando   herramientas  estándar   que  se  encuentran   en  toda
distribución GNU/Linux se puede resolver este problema en unas cuantas
lineas de código y de forma muy clara

```bash
#!/bin/bash

read -r -d '' SEDSCRIPT << EOS
/[[:lower:]]/{
        /[[:upper:]]/{
                /[[:digit:]]/p
        }
}
EOS

cat                             \
    /dev/urandom              | \  # (1)
    tr -dc 'a-zA-Z0-9'        | \  # (2)
    fold -w 7                 | \  # (3)
    sed -n -E -e "$SEDSCRIPT" | \  # (4)
    awk '!x[$0]++'                 # (5)
```

Funcionamiento del script:

1. Generamos un flujo aleatorio de bytes
2. Dejamos pasar sólo letras y números
3. Partimos el flujo en lineas de largo 7
4. Dejamos pasar sólo lineas compuestas estrictamente por números, mayúsculas y minúsculas
5. Sólo dejamos pasar la linea si no ha salido antes

Finalmente para generar el  CSV con los 34 millones de  registros, se puede hacer
los siguiente:


```bash
./script | head -n 34000000 | xargs -n 34 | tr ' ' ,  > result.csv
```
### Nota

Un  principio de  diseño  de  software es  descomponer  un sistema  en
unidades simples encargadas  de resolver sólo una tarea y  que ésta se
resuelva  bien, permitiendo  que la  complejidad y  potencia aparezcan
cuando se  combinan. Éste es un  principio  muy importante en  la
[filosofía Unix][fu]

[fu]: https://en.wikipedia.org/wiki/Unix_philosophy



EOF



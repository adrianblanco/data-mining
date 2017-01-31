### Extracción, transformación y análisis de datos

En primer lugar instalamos el paquete rvest que nos permite, entre otras cosas, scrapear tablas publicadas en páginas web:

`install.packages("rvest")`

Lo cargamos en R:

`library(rvest)`

A continuación, elegimos la tabla que queramos scrapear. En este caso es un conjunto de datos publicado en una página de Wikipedia.

Asociamos el link o url a la variable `url`:

`url <- "https://en.wikipedia.org/wiki/List_of_religious_populations"`

Una vez hecho eso, escribimos el código que nos va a permitir seleccionar y extraer los datos de la tabla que queremos procesar:

```
websites <- url %>%
read_html() %>%
html_nodes(xpath='//*[@id="mw-content-text"]/table[2]') %>%
html_table()
```

R ya tiene los datos, ahora podemos exportarlos. Para ello, utilizamos la función `write.csv`:

`write.csv(websites, file ="data.csv", row.names=FALSE)`

Es momento de comprobar cómo han sido extraídos los datos. Abrimos el archivo en Excel, spreadsheet, OpenOffice, Refine... Limpiamos los datos, si no se han extraído de forma correcta y guardamos el archivo.

Una vez limpios, volvemos a R. Para seleccionar los datos, escribimos:

`seleccionar <- read.csv(file="data.csv", header=T, sep=",")`

Los siguientes pasos consisten en transformar ligeramente la base de datos. Para ello necesitamos instalar el paquete `plyr`.

`install.packages("plyr")`

Cuando esté instalado, cargamos la librería:

`library(plyr)`

Si queremos renombrar alguna de las columnas o variables de la base de datos, utilizamos la función `rename`.

```
renombrar <- rename(data, c("Religion"="religion", "Percentage"="porcentaje"))
```

Para comprobar que lo hemos hecho bien escribimos `renombrar`

En este caso, convertimos las mayúsculas en minúsculas. No es un gran problema pero nos facilitará el trabajo. Ahora vamos a "filtrar" la tabla y eliminamos una de las columnas que no vamos a utilizar. Utilizamos `keeps` para ello:

```
keeps <- c("religion", "porcentaje")
filtro <- renombrar[keeps]
```

Tras estas sencillas transformaciones vamos a dibujar de manera rápida un scatterplot, como primer análisis visual de los datos. Para ello utilizamos

```
ggplot(data = filtro,
	aes(x = sum(porcentaje),
            y = porcentaje,
            fill = religion)) +
   geom_bar(stat = 'identity',
            position = 'stack') +
   theme(panel.background = element_blank())
```

Obtendremos un resultado similar a éste:

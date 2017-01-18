## Análisis de datos con ggplot2

ggplot2 es un paquete de visualización de datos para R desarrollado por Hadley Wickham. Inspirado por Leland Wilkinson de “La Gramática de Gráficos”. Ofrece una filosofía de organización para la construcción de gráficos - un enfoque estructurado para hacer gráficos.

En primer lugar, y para comenzar a trabajar, instalamos el paquete ggplot2. Para ello, escribimos en la consola:

`install.packages('ggplot2')`

R descargará las dependencias y las instalará. Una vez instaladas y para poder trabajar, recordad que siempre debemos cargar la librería ggplot2 previamente.

`library('ggplot2')`

Ya podemos comenzar a escribir las primeras funciones y argumentos en la consola.

Recuerda, en caso de duda:

`?ggplot2`

==============

#### Para saber más...
ggplot2 es un paquete desarrollado por Hadley Wickham y Winston Chang.

Una primera referencia de este paquete es [la documentación de ggplot2](http://docs.ggplot2.org/current/).

Un libro muy recomendable para iniciarse en ggplot2, apropiado para el análisis y la visualización de los datos, es [Elegant Graphics for Data Analysis](http://moderngraphics11.pbworks.com/f/ggplot2-Book09hWickham.pdf).

==============

Recordemos cómo cargabamos los datos

`state.x77`

o lo que es lo mismo:

`print(state.x77)`

Una primera aproximación a los datos puede ser analizar cómo se relacionan dos variables presentes en el conjunto como son XXXX y XXXX. Para ello, realizamos la siguiente operación:

`data(state)
states.data <- as.data.frame(state.x77)
states.data$states <- rownames(states.data)
colnames(states.data)[4] <- "Life.Exp"`

Aunque no lo vemos, hemos estructurado los datos. Es importante tener en cuenta la modificación en el nombre de una de las columnas "Life Exp" columnas que hemos realizado para poder visualizar los datos `colnames(states.data)[4] <- "Life.Exp"`.

Para evitar este tipo de operaciones es *importante* que cuando carguemos un conjunto de datos, el nombre de las columnas no tenga espacios entre palabras. También es conveniente evitar tildes, comas...

 Ahora, es posible visualizarlos mediante un sencillo diagrama de dispersión.

`qplot(Income, Life.Exp, data = states.data)`

Si queremos añadir un título al gráfico:

`qplot(Income, Life.Exp, data = states.data, main = "Ingresos vs Esperanza de vida")`

![scatterplot](https://github.com/adrianblanco/data-mining/blob/master/img/scatterggplot2.png | width=200)

Otro modo de expresarlo es:

`ggplot(data = states.data,
       aes(x = Income,
           y = Life.Exp)) +
  geom_point() +
  ggtitle("Income v. Life Expectancy")`

Aunque ya podemos realizar un rápido análisis de los datos, desconocemos a qué estados se refieren los puntos. Para solucionarlo, podemos visualizar como etiqueta cada uno de los estados al lado del círculo que les representa. Escribimos:

`ggplot(data = states.data,
       aes(x = Income,
           y = Life.Exp)) +
           geom_point() +
        geom_text(aes(label = states), size=5)`

![scatterplot con labels](https://github.com/adrianblanco/data-mining/blob/master/img/scatterlabels.png | width=200)

Con los nombres en cada uno de los puntos, podemos analizar los datos de forma sencilla. Aunque todavía nos es un poco complejo por el tamaño del texto. Para verlo más claro podemos añadir un argumento a geom_text y reducir el tamaño del texto. Se haría así `geom_text(aes(label = states), size=3)`.

Si quisiésemos colorearlo:

`ggplot(data = states.data,
       aes(x = Income,
           y = Life.Exp,
           color = states)) +
  geom_point()`

¿Y si queremos ajustar los ejes? Para ello añadimos `scale_x_continuous` y los límites que queremos mostrar en el eje x.

`ggplot(data = states.data,
       aes(x = Income,
           y = Life.Exp)) +
  geom_point() +
  scale_x_continuous(limits = c(1000, 7000))`

Además también podemos colorear los valores:

`ggplot(data = states.data,
       aes(x = Income,
           y = Life.Exp,
           color = "estados")) +
  geom_point()`

Ya hemos invertido suficiente tiempo en los gráficos de dispersión o scatterplots. Estos son útiles pero no siempre nos dan la clave que estamos buscando en los datos. Ahora vamos a generar un gráfico simple, pero muchas veces útil, como es el de barras o bar graph.

Para empezar, vamos a realizar una selección de los datos:

`four.states <- states.data[states.data$states == "California" | states.data$states == "New York" | states.data$states == "Texas" | states.data$states == "Alabama", ]`

![bargraph](https://github.com/adrianblanco/data-mining/blob/master/img/bargraph.png | width=200)

Una vez transformada la tabla, escribimos los argumentos para visualizarla:

  `ggplot(data = four.states,
       aes(x = states,
           y = Population)) +
  geom_bar(stat = 'identity', fill = 'red')`

### Cómo trabajar en ggplot2 con mis propios datos

Dejemos atrás los datos precargados en R. Ahora vamos a cargar un conjunto de datos desde nuestro ordenador.

Para ello, utilizamos la función read.csv() para conjuntos de datos que estén en este formato. Esa función la asignamos a un nombre que, a partir de ahora, hará las veces de nuestra base de datos.

  `mis_datos <- read.csv("ruta")`

Para comprobar que está todo en orden, inspeccionamos las primeras filas de nuestro conjunto de datos:

  `head(mis_datos[1:5])`

Veámoslo con un ejemplo real como es el análisiis de los datos de las últimas elecciones gallegas y vascas. Dicho análisis y posterior visualización de datos está publicado en El Confidencial en el artículo ["Gallego y euskera, el voto nacionalista no siempre habla su idioma"](
http://www.elconfidencial.com/espana/2016-10-02/elecciones-galicia-pais-vasco-resultados-analisis-voto-idioma-edad_1268665/).

Primer descargamos los datos en formato .csv de [este gist o enlace] (https://gist.github.com/adrianblanco/c4fe8925e6ae9cae962d24f4742f83eb).

Para realizar el análisis de datos, utilizaremos de nuevo la librería ggplot2. El código, que iremos desmenuzando en clase, es el siguiente:

```

lee <- read.csv(file="c:/Users/ablanco/Documents/rgraph/elecciones_galicia_euskadi.csv", header=T, sep=",")

visualiza <- ggplot(lee, aes(x=mediana, y=porcentaje_voto, color=partido)) + geom_point() + scale_x_continuous(limits=c(35,62)) + scale_y_continuous(limits=c(0,80)) + facet_grid (~ partido) 

tendencia <- visualiza + stat_smooth(method = 'lm', se=FALSE)

library(ggthemes)

economist <- + theme_economist() + scale_colour_economist()


```



Por último, y ahora que hemos visto cómo trabajar con nuestros propios datos, es importante tener en cuenta que no siempre debemos guiarnos por nuestra primera intuición o por los datos que tengamos más a mano. Para muestra [este vídeo](https://www.youtube.com/watch?v=N8Votwxx8a0) que ofrece alternativas a unos datos utilizados en todos los informes, redacciones, estudios económicos, etc. del mundo y que, en muchos casos, no nos hemos parado a valorar si son los adecuados o correctos.



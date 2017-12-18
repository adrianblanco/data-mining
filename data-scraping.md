Instalamos el paquete:

```
install.packages("rvest")
```

```
library(rvest)
```

```
url <- "https://en.wikipedia.org/wiki/Corporate_tax"
```

```
is  <- url %>%
    read_html() %>%
    html_nodes(xpath='//*[@id="mw-content-text"]/div/table[5]') %>%
    html_table()
```


```
install.packages("tidyverse")
```

```
library(tidyverse)
```



     # your xpath defines the single table, so you can use html_node() instead of html_nodes()

```
corporatetax <- url %>% 
     read_html() %>% 
     html_node(xpath='//*[@id="mw-content-text"]/div/table[5]') %>% 
     html_table() %>% as_tibble() %>% 
     setNames(c("country", "corporate_tax", "combined_tax"))
```

Probamos

```
corporatetax
```

```
bar <- corporatetax %>% 
     mutate(corporate_tax=as.numeric(str_replace(corporate_tax, "%", "")),
            combined_tax=as.numeric(str_replace(combined_tax, "%", ""))
     )
```

Probamos

```
bar
```



```
visualizar <- ggplot(data=bar, aes(x=country, y=corporate_tax)) +
     geom_bar(stat="identity")
```


```
visualizar
```

Si queremos rotar el gráfico:

```
p + coord_flip()
```


IMG

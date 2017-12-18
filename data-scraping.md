install.packages("rvest")

library(rvest)

url <- "https://en.wikipedia.org/wiki/Corporate_tax"

is  <- url %>%
    read_html() %>%
    html_nodes(xpath='//*[@id="mw-content-text"]/div/table[5]') %>%
    html_table()
    
    

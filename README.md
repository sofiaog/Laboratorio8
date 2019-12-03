# Laboratorio8
## Codigo para la realización de gráfico animado

+ Librerías necesarias  
library(tidyverse)  
library(ggplot2)  
library(gganimate)

+ Descargar mallas en *GitHub*  
coast_vs_waste <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-21/coastal-population-vs-mismanaged-plastic.csv")  
mismanaged_vs_gdp <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-21/per-capita-mismanaged-plastic-waste-vs-gdp-per-capita.csv")  
waste_vs_gdp <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-21/per-capita-plastic-waste-vs-gdp-per-capita.csv")

+ Uno las mallas *mismanaged_vs_gdp* y *waste_vs_gdp*, la guardo como *malla_join*  
malla_join <-
  left_join(mismanaged_vs_gdp, waste_vs_gdp, by = c("Entity","Code","Year","Total population (Gapminder)")) %>%
  print()
  
+ Uno la tercera malla *coast_vs_waste* a la anterior malla generada *malla_join*  
malla_join<-
  left_join(malla_join, coast_vs_waste, by = c("Entity","Code","Year","Total population (Gapminder)")) %>%
  print()
  
+ Filtro por paises de America latina  
malla_join <-
  malla_join %>%
  filter(Entity %in% c("Argentina", "Brazil", "Chile", 
                                       "Colombia", "Costa Rica", "Cuba", 
                                       "Ecuador", "El Salvador", "Guatemala", 
                                       "Honduras", "Mexico", "Nicaragua", 
                                       "Panama", "Puerto Rico", "Peru", 
                                       "Dominican Republic", "Uruguay", "Venezuela")) %>%
  print()
  
+ Me quedo solo con las columnas que voy a utilizar y renombro las necesarias  
malla_join <-
  malla_join %>%
  select(Year, "pais"= `Entity`,
         "poblacion" = `Total population (Gapminder)`,
         "PIB_pc"= `GDP per capita, PPP (constant 2011 international $) (Rate)`) %>% 
  print()

+ Genero el gráfico animado  
ggplot(na.omit(malla_join), aes(PIB_pc, poblacion, size = poblacion, color = pais)) +
  geom_point() +
  scale_x_log10() +
  theme_bw() +
  labs(title="PIB per capita y Poblacion Total de Paises de AL", 
       subtitle="De 1990 a 2013", 
       x = 'PIB per capita', y = 'Poblacion Total',
       caption="fuente: Gapminder") +
  transition_time(Year) +
  ease_aes('linear')
  
+ Para guardar el grafico animado como gif  
anim_save("nombre.gif")
  
  
  

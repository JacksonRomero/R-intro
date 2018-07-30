# Comunicación

El trabajo estadístico no termina cuando entendemos la estructura de los datos. Nuestro trabajo estará incompleto a menos que logremos llevar aquello que destilamos de la evidencia a la combinación correcta de palabras e imágenes que logren recrear el mensaje en otra mente.  

La mayoría de los usos a continuación son explotados a su máxima expresión si se posee cierta familiaridad con los sistemas de control de versiones (por ejemplo, GIT) y plataformas online basadas en dichos sistemas (por ejemplo, GitHub). Dicho material excede a la complejidad de este libro[^git]. Sin embargo, los principios generales que desarrollo a continuación son independientes de los sistemas de control de versiones.

## Ciencia reproducible

Uno de los grandes motores detrás de la ciencia reproducible es el acceso a abierto a los datos y el código que se utilizó para analizarlos. La ventaja de realizar ciencia abierta de este modo es la disminución de errores de procedimiento que quedan escondidos en una computadora a la que nadie tiene acceso. Por ejemplo, un error de tipeo en una fórmula en Excel puede pasar desapercibido y llevar a conclusiones erróneas. Errores en planillas de Excel son muy complicados de hallar.  

Aunque parezca extremo, existen casos documentados de este tipo.

* [Nota en Washington Post](https://www.washingtonpost.com/news/wonk/wp/2016/08/26/an-alarming-number-of-scientific-papers-contain-excel-errors/?noredirect=on&utm_term=.d265244d3d9b)
* [Nota en The Economist](https://www.economist.com/graphic-detail/2016/09/07/excel-errors-and-science-papers)

## Gráficos

En una sección anterior realizamos análisis exploratorios. Nuestro objetivo era entender los datos de manera rápida (ver Sección \@ref(caso-de-estudio)). Dichos análisis no se focalizaron en comunicar visualmente hacia un *externo* o *consumidor*. Retomemos el dataset del Titanic y hagamos un gráfico tipo *waffle* (también conocidos como gráficos tipo *tile* o azulejo).


```r
# Si es la primera vez, vamos a necesitar unos cuantos paquetes
# install.packages("rlang")
# install.packages("stringi")
# install.packages("curl")
# install.packages("devtools")
# devtools::install_github("hrbrmstr/waffle")
# Paquete para hacer gráficos de tipo waffle
library(waffle)

library(tidyverse)
library(titanic)
```


```r
# Calculamos supervivencia
sobrev <- titanic_train %>%
          group_by(Survived) %>%
          count() %>%
          ungroup()%>%
          mutate(Survived = c("No Sobrevivió", "Sobrevivió"))

# Waffle plot
waffle(sobrev, rows = 25, size = 1,
       colors = c("#1696d2", "#fdbf11"),
       legend_pos = "bottom") +
  labs(title = "Sobrevivientes del Titanic",
      subtitle = "1 cuadrado == 1 persona")
```

<img src="06-Comunicacion_files/figure-html/unnamed-chunk-2-1.png" width="672" />


## RMarkdown

Markdown es un lenguage de marcado ligero que permite agregar marcajes sencillos al texto plano (`.md`), para convertirlo en un poderoso conjunto de operaciones que pueden exportarse a formatos de publicación profesional como `.html`, `.pdf` y `.tex`[^markdown-wiki]. *RMarkdown* es la implementación de Markdown dentro de Rstudio. RMarkdown posee toda la funcionalidad de Markdown y además permite introducir trozos de código.  

Los archivos de Rmarkdown son archivos de texto con extensión `.Rmd`. Podemos crearlos dentro de Rstudio desde el mismo menú con el que abrimos scripts.  

![Abrir una ventana de Rmd](img/rmarkdown-01.png)

Al abrirlo, un navegador nos permitirá seleccionar opciones (como el título, autor, etc). No me centraré en esa ventana debido a que todas estas opciones pueden modificarse en el encabezado del nuevo archivo `.Rmd` (ver Sección \@ref(encabezado)).   

## Texto

Markdown es un lenguaje que permite escribir de manera veloz, con muy pocas distracciones. La funcionalidad para editar la presentación de texto es limitada, pero es más que suficiente para el 99% de las cosas que necesitamos al escribir. Los siguientes ejemplos están ordenados segun cómo se escriben en Rmarkdown y cómo es el render.


`Lo esencial es *invisible* a los ojos.` $\rightarrow$ Lo esencial es *invisible* a los ojos.  
`Lo esencial es **invisible** a los ojos.` $\rightarrow$ Lo esencial es **invisible** a los ojos.  
`Lo esencial es ~~in~~visible a los ojos.` $\rightarrow$ Lo esencial es ~~in~~visible a los ojos.  

Si tienes familiaridad con $\LaTeX$ es posible utilizarlo para fórmulas matemáticas en línea con la frase que se está escribiendo. Por ejemplo, un supuesto de los modelos lineales generales es $\mathcal{E}_i \sim \  \mathcal{N}(\mu,\,\sigma^{2})$. Utilzar $\LaTeX$ en RMarkdown es tan sencillo como englobar el código utilizando el símbolo `$`. Si queremos utilizar una ecuación, por ejemplo, utilizamos los entornos `\begin{equation}` y `\end{equation}` y escribimos sin necesidad de utilizar el símbolo `$`.  

Para utilizar resaltado tipo `código` en línea utilizamos palabras envueltas en *backticks* (`` ` ``)[^backticks]. Podemos utilizar código de R en la línea en la que estamos escribiendo de la forma `r`+`espacio`+`función deseada`. Por ejemplo, si combinamos con $\LaTeX$ y le preguntamos a R cuánto es $\pi$, nos dará el resultado en la misma línea así: $\pi$ = 3.1415927[^en-mi-pc].

## Separando líneas y párrafos

La belleza de Rmarkdown es que acomoda el texto y las figuras automáticamente, por lo que no debemos preocuparnos por el número de líneas y demás problemas que aparecen con un editor de texto tradicional. Sin embargo, en ciertas ocasiones realmente deseamos forzar un quiebre, un espacio extra. Para separar párrafos, basta con terminar la línea que se está escribiendo con dos espacios. Por ejemplo,  

*No te des por vencido, ni aún vencido, no te sientas esclavo, ni aún esclavo;*  

Puede transformarse en:  

*No te des por vencido, ni aún vencido,*  
*no te sientas esclavo, ni aún esclavo;*  

Si agregamos dos espacios entre las palabras deseadas[^normalmente].  

Las listas de objetos son un caso puntual muy útil:

```
# Listas no numeradas
* item 1
* item 2
+ item 3
* item 4
    * sub-item
    * sub-item

```
Se ve así:  

* item 1
* item 2
+ item 3
* item 4
    * sub-item
    * sub-item

```
# Listas numeradas
1. item 1
1. item 2
1. item 3
    a) item 3a
    a) item 3b
```

Se ve así: 

1. item 1
1. item 2
1. item 3
    a) item 3a
    a) item 3b


En las listas numeradas no debemos mantener la cuenta. Simplemente `1. ` y escribimos lo que necesitamos, Markdown hace el resto!  

Otra posibilidad es utilizar texto resaltado. Para resaltar texto utilizamos el símbolo mayor (>) seguido de un espacio. Esta opción no es recomendada para uso extensivo, pero puede resaltar una frase corta de un modo interesante. Por ejemplo,  

> R es genial.

Cuando terminamos una sección o deseamos una marca física entre párrafos, podemos usar 3 asteriscos (*).  

```
# Línea horizontal
***
```

Se ve:  

***

## Trozos de código

En Rmarkdown podemos mezclar texto y código de R. La siguiente imagen muestra cómo se ve en mi computadora:  

![El párrafo previo en mi editor](img/rmarkdown-code-chunk.png)

Cuando estos trozos se agregan al documento final, corren en la consola de R y el output se incluye en los resultados. Por ejemplo, el siguiente trozo:


```r
summary(cars)
```

```
##      speed           dist       
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
```

Para insertar trozos de código podemos hacer click en insert o mediante el comando rápido Ctrl+Alt+I.


## Links e imágenes

Los links e imágenes pueden incluirse utilizando corchetes ([]) y paréntesis. Por ejemplo,

```
# Imágenes
![descripcion de mi imagen](link)
# URLs
[descripción de mi link](link)
```
El link puede ser un directorio dentro de nuestra computadora (`imagenes/vacaciones/.png`) o via `www` con un link externo, como el logo de Rstudio:  
![](https://www.rstudio.com/wp-content/uploads/2016/09/RStudio-Logo-Blue-Gray-250.png)

## Encabezado

Los encabezados son útiles para diversos archivos, ciertamente necesarios para la organización de documentos más complejos. Rmarkdown utiliza un encabezado conocido como YAML y tiene la siguiente forma:

```
--- 
title: "Un mundo feliz"
author: "Aldous Huxley"
date: "1932"
output: html_document
---
```

Por ejemplo, el encabezado de este libro es el siguiente[^encabezado]:

```
--- 
title: "Introducción a estadística con R"
author: "Matias Andina"
date: "2018-07-29"
site: bookdown::bookdown_site
documentclass: book
---

```

En este caso, la fecha la he creado con un llamado a la función `Sys.Date()` (`r` + `espacio` + `Sys.Date()`) que permite que cada vez que este libro se compila, se use la fecha de sistema para el compilado., De este modo, no tengo que actualizarlo manualmente cada vez que edito el libro.  

## Knit

Para armar el documento, debemos hacer click en *knit*. El producto final será un documento tipo `.html` que contenga todo el texto y las imágenes del archivo `.Rmd`. 

## Otros proyectos con RStudio

* Presentaciones de diapositivas con Markdown y `R presentation`.
* Usando el paquete `blogdown`, Websites como [este](http://matiasandina.netlify.com)
* Usando el paquete `bookdown`, libros como éste ([y otros!](bookdown.org))

## Notas

Mucha tecnología cotidiana detrás de los servicios de publicación está principalmente desarrollada alrededor del idioma inglés. Problemas inesperados pueden aparecer incluso cuando hacemos todo bien utilizando R, Rstudio y Rmarkdown. Por ejemplo, mientras escribía este libro tuve un problema con el nombre de mis archivos que contenían tilde (por ejemplo, el archivo de este capítulo era "Comunicación.Rmd"). La inconveniente `ó` provocó que el link a ciertas imágenes del capítulo (`www. .../Comunicación/imagen-01.png`) no pudiera ser encontrado y dichas imágenes no aparecieran. La solición de compromiso es (como en muchas otras direcciones de internet) no utilizar caracteres que no existen en inglés (por ejemplo, la ñ, o palabras con tildes).

## Recursos

* Cheatsheet oficial de Rmarkdown en español por Rstudio [aquí](https://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-spanish.pdf)
* Buen material sobre visualización con ggplot2 [aquí](http://urbaninstitute.github.io/urban_R_theme/)

[^encabezado]: El encabezado que muestro es una porción que es comparable con un documento clásico. El encabezado real contiene ciertas especificaciones para que este libro pueda ser publicado online y entiendo que no aportan al ejemplo.
[^markdown-wiki]: Puedes encontrar más información sobre Markdown[aquí](es.wikipedia.org/markdown)
[^git]: Puedes aprender sobre GIT [aquí](https://git-scm.com/book/es/v1/Empezando)
[^normalmente]: Normalmente suelo agregar un enter y escribir en la siguiente línea. Aunque no es necesrario, me ayuda a visualizar el render antes de tiempo.
[^backticks]: Los backtics son difícilies de encontrar en el teclado en español, puedes probar ALT+96.
[^en-mi-pc]: Para lograr esto el formato debe ser `` $\pi$ = 3.1415927 ``
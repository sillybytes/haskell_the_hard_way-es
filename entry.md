# Aprende Haskell rápido y difícil
# Asómbrate con Haskell

![](http://yannesposito.com/Scratch/img/blog/Haskell-the-Hard-Way/magritte_pleasure_principle.jpg)

Esta es la traducción al español del artículo [Haskell the hard
way](http://yannesposito.com/Scratch/en/blog/Haskell-the-Hard-Way/) por Yann
Esposito.


TL;DR\*: Un corto y denso tutorías para aprender Haskell.

De verdad pienso que todos los desarrolladores deberían aprender Haskell. No
creo que todos necesitan convertirse en ninjas de Haskell, pero deberían al
menos descubrir que es lo que Haskell tiene para ofrecer. Aprender Haskell abre
tu mente.

Los lenguajes comunes comparten los mismos fundamentos:

* variables
* loops
* punteros[^1]
* estructuras de datos, objetos y clases

Haskell es muy diferente. El lenguaje usa muchos conceptos que nunca he
escuchado antes. Muchos de esos conceptos te ayudarán a convertirte en un mejor
programador.

Pero aprender Haskell puede ser difícil. Lo fue para mi. En este artículo
intentaré proveer lo que me faltó durante mi aprendizaje.

Este artículo será ciertamente difícil de seguir. Esto es intencional. No hay
atajo alguno para aprender Haskell. Es difícil y retador. Pero creo que es algo
bueno. Debido a que es difícil es que Haskell es interesante.

El método convencional de aprender Haskell es leer dos libros. Primero ["Learn
You a Haskell"](http://learnyouahaskell.com/) y justo después ["Real World
Haskell"](http://www.realworldhaskell.org/). También pienso que esta es la forma
correcta. Pero aprender de que se trata Haskell, deberás leerlos en detalle.

En contraste, este artículo es un resumen muy breve y denso de los principales
aspectos de Haskell. También he agregado información que a mi me faltó mientras
aprendía Haskell.

El artículo contiene cinco partes:

* Introducción: un corto ejemplo para mostrar que Haskell puede ser amigable.
* Haskell básico: sintaxis de Haskell, y algunas nociones esenciales.
* Parte muy difícil:
    * Estilo funcional; un ejemplo progresivo, desde estilo imperativo al
    * funcional
    * Tipos; tipos y el ejemplo estándar del árbol binario
    * Estructuras infinitas; manipulando un árbol binario infinito!

* Parte infernalmente difícil:
    * Lidiar con IO; un ejemplo reducido
    * El truco de IO explicado; el detalle ocultó que yo no tuve para entender
      IO
    * Monads; increíble como podemos generalizar

* Apéndice:
    * Más sobre arboles infinitos; una discusión más matemática sobre arboles
    * infinitos


# Introducción

## Instalación

![](http://yannesposito.com/Scratch/img/blog/Haskell-the-Hard-Way/Haskell-logo.png)

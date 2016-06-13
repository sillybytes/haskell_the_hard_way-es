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


    Nota: El código de ejemplo se almacena en ficheros con un nombre
    específico que termina en la extensión `.hs` (Haskell), y por eso en los
    ejemplos se escribre la ejecución de los mismos como `$ runhaskell
    algo.hs` pero el nombre puede ser cualquiera.


# Introducción

## Instalación

![](http://yannesposito.com/Scratch/img/blog/Haskell-the-Hard-Way/Haskell-logo.png)

* [La plataforma de Haskell](https://www.haskell.org/platform/) es la forma
  estándar de instalar Haskell.

Herramientas:

`ghc`: Compilador similar a *gcc* para `C`.
`ghci`: Haskell interactivo (REPL)
`runhaskell`: Ejecutar un programa sin compilarlo. Conveniente pero muy lento
    comparado a programas compilados


## No tengas miedo

![](http://yannesposito.com/Scratch/img/blog/Haskell-the-Hard-Way/munch_TheScream.jpg)

Muchos libros/artículos sobre Haskell empiezan por introducir alguna formula
esotérica (quick sort, Fibonacci, etc...). Yo lo haré justamente al revés. Al
principio no mostraré ningún super poder de Haskell. Empezaré por las
similaridades entre Haskell y otros lenguajes de programación. Saltemos al "Hola
Mundo" obligatorio.

```Haskell
main = putStrLn "Hola Mundo!"
```

Para ejecutarlo, puedes guardar el código en un fichero `hola.hs` y:

    $ runhaskell ./hola.hs
    Hola Mundo!


Ahora, un programa que pregunte tu nombre y responda "Hola" usando el nombre
ingresado:

```Haskell
main = do
    print "Cuál es tu nombre?"
    name <- getLine
    print ("Hola " ++ name ++ "!")
```

Primero, comparemos esto con programas similares en algunos lenguajes
imperativos:


```Python
# Python
print "What is your name?"
name = raw_input()
print "Hello %s!" % name
```

```Ruby
# Ruby
puts "What is your name?"
name = gets.chomp
puts "Hello #{name}!"
```

```C
// In C
#include <stdio.h>
int main (int argc, char **argv) {
    char name[666]; // <- An Evil Number!
    // What if my name is more than 665 character long?
    printf("What is your name?\n");
    scanf("%s", name);
    printf("Hello %s!\n", name);
    return 0;
}
```

La estructura es la misma, pero hay diferencias en la sintaxis. La parte
principal de este tutorial será dedicada a explicar por qué.

En Haskell hay una función `main` y todo elemento tiene un tipo. El tipo de
`main` es `IO ()`. Esto significa que `main` causará efectos
secundarios.

Solamente recuerda que Haskell puede lucir mucho como los lenguajes
imperativos populares.


## Haskell básico

![](http://yannesposito.com/Scratch/img/blog/Haskell-the-Hard-Way/picasso_owl.jpg)

Antes de continuar debes ser advertido sobre algunas propiedades
esenciales de Haskell.


**Funcional**

Haskell es un lenguaje funcional. Si tienes experiencia con lenguajes
imperativos, deberás aprender muchas cosas nuevas. Con suerte muchos de estos
nuevos conceptos te ayudarán a programas incluso en lenguajes
imperativos.


**Tipado estático inteligente**

En lugar de meterse en tu camino como en `C`, `C++` o `Java`, el sistema de
tipos está aquí para ayudarte.


**Pureza**

Generalmente tus funciones no modificarán nada en el mundo exterior. Esto
significa que no pueden modificar el valor de una variable, no pueden obtener
entrada del usuario, no pueden escribir en la pantalla, no pueden lanzar un
misil. Por otro lado, el paralelismo será muy fácil de lograr. Haskell
hace deja claro donde los efectos secundarios pueden ocurrir y donde el código
es puro. También, será mucho más fácil razonar sobre el programa. La mayoría de
los errores serán prevenidos en las partes puras del programa.

Además, las funciones puras siguen una ley fundamental en Haskell:

    Aplicar una funcion con los mismos parámetros siempre producirá los
    mismos valores.


**Perezoso (laziness)**

Laziness por defecto es un diseño de lenguaje muy poco común. Por defecto,
Haskell evalúa algo solamente cuando lo necesita. En consecuencia, provee una
forma muy elegante de manipular estructuras infinitas, por ejemplo.

Una ultima advertencia sobre como deberías leer código Haskell. Para mi, es
como leer artículos científicos. Algunas partes son muy claras, pero cuando vez
una formula, enfócate y lee más despacio. También, mientras se lee código
Haskell, en realidad no importa mucho si no se comprenden los detalles
de la sintaxis. Si encuentras algo como `>>=`, `<$>`, `<-` o cualquier
símbolo extraño, solamente ignóralos y continua el flujo del código.


### Declaración de funciones

Seguramente estarás acostumbrado a funciones como:

En `C`:

```C
int f(int x, int y) {
    return x*x + y*y;
}
```

En `JavaScript`:

```JS
function f(x,y) {
    return x*x + y*y;
}
```

En Python:

```Python
def f(x,y):
    return x*x + y*y
```

En Ruby:

```Ruby
def f(x,y)
    x*x + y*y
end
```

En Scheme:

```Scheme
(define (f x y)
    (+ (* x x) (* y y)))
```

Finalmente, en Haskell es:

```Haskell
f x y = x*x + y*y
```

Muy limpio. No paréntesis, no `def.`

No olvides, Haskell usa funciones y tipos un montón. Por lo que es muy fácil
definirlos. La sintaxis fuer particularmente pensada para estos elementos.


### Un ejemplo de tipo

Aunque no es obligatorio, la información sobre los tipos para las funciones
usualmente se hace explicita. No es obligatorio por que el compilador es lo
bastante inteligente para descubrirlo por ti. Es una buena idea hacerlo de
todas formas por que indica la intensión y facilita la comprensión.

Juguemos un poco. Declaramos el tipo usando `::`


```Haskell
f :: Int -> Int -> Int
f x y = x*x + y*y

main = print (f 2 3)
```

    $ runhaskell 20_very_basic.lhs
    13


Ahora intenta

```Haskell
f :: Int -> Int -> Int
f x y = x*x + y*y

main = print (f 2.3 4.2)
```

Deberías obtener este error:

    21_very_basic.lhs:6:23:
        No instance for (Fractional Int)
        arising from the literal `4.2'
        Possible fix: add an instance declaration for (Fractional Int)
        In the second argument of `f', namely `4.2'
        In the first argument of `print', namely `(f 2.3 4.2)'
        In the expression: print (f 2.3 4.2)

El problema: 4.2 no es un `Int`.

La solución: No declarar un tipo para `f` por el momento y dejar a Haskell
inferir el tipo más general por nosotros:

```Haskell
f x y = x*x + y*y

main = print (f 2.3 4.2)
```

Funciona! afortunadamente, no tenemos que declarar una nueva funcion para cada
tipo. Por ejemplo, in `C`, deberíamos declarar una función para `int`,
para `float` para `long`, para `double`, etc...

Pero, que tipo deberíamos declarar? Para descubrir el tipo que Haskell a
usado por nosotros ejecutaremos **ghci**:


    % ghci

    GHCi, version 7.0.4: http://www.haskell.org/ghc/  :? for help
    Loading package ghc-prim ... linking ... done.
    Loading package integer-gmp ... linking ... done.
    Loading package base ... linking ... done.
    Loading package ffi-1.0 ... linking ... done.
    Prelude>


Y escribimos:

    let f x y = x*x + y*y
    Prelude>
    :type f
    f :: Num a => a -> a -> a


Uh? Que es ese tipo extraño?

```Haskell
Num a => a -> a -> a
```

Primero, enfoquémonos en la parte de la derecha `a -> a -> a`. Para
comprenderlo, solo mira una lista de ejemplos progresivos:

| El tipo | Su significado |
| ------- | -------------- |
| Int     | El tipo Int    |
| Int -> Int | El tipo de función de Int a Int |
| Float -> Int | El tipo de función de Float a Int |
| a -> Int | El tipo de función de cualquier tipo a Int |
| a -> a | El tipo de función de cualquier tipo al mismo tipo a |
| a -> a -> a | El tipo de función de dos argumentos de cualquier tipo a al
mismo tipo a |

En el tipo `a -> a -> a`, la letra `a` es una *variable de tipo*. Significa
que `f` es una función con dos argumentos y esos dos argumentos y el resultado
tienen que ser del mismo tipo, La variable de tipo `a` puede ser cualquier
tipo. Por ejemplo `Int`, `Integer`, `Float`...

Así que en lugar de forzar un tipo en particular como en `C` y tener que
declarar una función para `int`, `long`, `float`, `double`, etc., podemos
declarar una sola función como en un lenguaje de tipado dinámico.

Esto es algunas veces llamado polimorfismo paramétrico.

Generalmente `a` puede ser cualquier tipo, por ejemplo un `String` a un `Int`,
pero también puede ser tipos más complejos, como `Trees`, otras funciones,
etc. Pero en este caso nuestro tipo tiene como prefijo `Num a =>`.

`Num` es una *clase de tipo* (type class). Una clase de tipo puede ser vista
como un conjunto de tipos. `Num` contiene solamente los tipos que pueden
comportarse como números. Más concretamente, `Num` es una clase que contiene
tipos que implementan una lista especifica de funciones, en particular
`(+)` y `(*)`.

Las clases de tipos son un aspecto muy potente del lenguaje. Podemos hacer
cosas increíbles con esto. Más sobre el tema luego.

Finalmente, `Num a => a -> a -> a` significa:

Sea `a` un tipo que pertenece a la clase de tipo `Num`. Esto es una función de
tipo `a` a (`a -> a`).

Si, extraño. De echo, en Haskell ninguna función tiene dos argumentos.
En lugar de eso todas las funciones pueden tener un solo argumento. Pero
notaremos que tomar dos argumentos es equivalente a tomar un argumento
y retornar una función que toma el segundo argumento como parámetro.

Más concretamente `f 3 4` es equivalente a `(f 3) 4`. Nótese que `f 3` es una
función:

```Haskell
f :: Num a => a -> a -> a

g :: Num a => a -> a
g = f 3

g y ⇔ 3*3 + y*y
```

Existe otra notación para funciones. La notación *lambda* nos permite crear
funciones sin asignarles un nombre. Llamamos a estas funciones anónimas.
Podemos escribirlas como:

```Haskell
g = \y -> 3*3 + y*y
```

El `\` es usado por que se parece a `λ` (símbolo lambda) y es ASCII.

Si no estás acostumbrado a la programación funcional tu cerebro debería
estar empezando a calentarse. Es tiempo de hacer una aplicación real.

Pero antes de eso, deberíamos verificar que el sistema de tipos funciona
según lo esperado.

```Haskell
f :: Num a => a -> a -> a
f x y = x*x + y*y

main = print (f 3 2.4)
```

Funciona, porque, `3` es una representación valida para números
fraccionarios como `Float` así como para `Integer`. Como `2.4` es una numero
fraccionario, `3` es interpretado también como un numero fraccionario.

Si forzamos nuestra función a trabajar con tipos diferentes, fallará.

```Haskell
f :: Num a => a -> a -> a
f x y = x*x + y*y

x :: Int
x = 3
y :: Float
y = 2.4
-- No funcionará por que el tipo x ≠ tipo y
main = print (f x y)
```

El compilador se queja. Los dos parámetros deben ser del mismo tipo.

Si piensas que esto es una mala idea, y que el compilador debería hacer la
transformación de un tipo al otro por ti, deberías ver este fantástico
(y divertido) vídeo: [WAT](https://www.destroyallsoftware.com/talks/wat)


# Haskell esencial

![](http://yannesposito.com/Scratch/img/blog/Haskell-the-Hard-Way/kandinsky_gugg.jpg)

Sugiero que leas con ligereza esta parte. Mírala como una referencia. Haskell
tiene un montón de características. Regresa aquí cada vez que la notación
te parezca extraña.

Uso el símbolo `⇔` para indicar que dos expresiones son equivalentes. Es una
meta notación, `⇔` no existe en Haskell. También usaré `⇒` para indicar
cual es el valor de retorno de una expresión.


## Notaciones

**Aritmética**

    3 + 2 * 6 / 3 ⇔ 3 + ((2*6)/3)


**Lógica**

    True || False ⇒ True
    True && False ⇒ False
    True == False ⇒ False
    True /= False ⇒ True  (/=) es el operador diferencia


**Potencias**

    x^n     para un n entero (Int o Integer)
    x**y    para cualquier tipo de numero y (como un Float)

`Integer` no tiene ningún limite además de la capacidad de tu máquina.

    4^103
    102844034832575377634685573909834406561420991602098741459288064


Si! También hay números racionales! Pero hay que importar el modulo
`Data.Ratio`:

    $ ghci
    ....
    Prelude> :m Data.Ratio
    Data.Ratio> (11 % 15) * (5 % 3)
    11 % 9


**Listas**

    []                      ⇔ Lista vacia
    [1,2,3]                 ⇔ Lista de enteros
    ["foo","bar","baz"]     ⇔ Lista de cadenas
    1:[2,3]                 ⇔ [1,2,3], (:) anteponer un elemento
    1:2:[]                  ⇔ [1,2]
    [1,2] ++ [3,4]          ⇔ [1,2,3,4], (++) concatenar
    [1,2,3] ++ ["foo"]      ⇔ ERROR String ≠ Integral
    [1..4]                  ⇔ [1,2,3,4]
    [1,3..10]               ⇔ [1,3,5,7,9]
    [2,3,5,7,11..100]       ⇔ ERROR! No soy tan inteligente!
    [10,9..1]               ⇔ [10,9,8,7,6,5,4,3,2,1]


**Cadenas**

En Haskell las cadenas son listas de `Char`.

    'a' :: Char
    "a" :: [Char]
    ""  ⇔ []
    "ab" ⇔ ['a','b'] ⇔  'a':"b" ⇔ 'a':['b'] ⇔ 'a':'b':[]
    "abc" ⇔ "ab"++"c"


    En código real no se debería usar una lista de `Char` para
    representar texto. Se debería usar `Data.Text`. Si quieres
    representar un flujo de caracteres ASCII, deberías usar
    `Data.ByteString`.


**Tuplas**

El tipo de una tupla es `(a,b)`. Los elementos dentro de una tupla pueden
tener diferentes tipos.

    -- Todas estas tuplas son validas
    (2,"foo")
    (3,'a',[2,3])
    ((2,"a"),"c",3)

    fst (x,y)       ⇒  x
    snd (x,y)       ⇒  y

    fst (x,y,z)     ⇒  ERROR: fst :: (a,b) -> a
    snd (x,y,z)     ⇒  ERROR: snd :: (a,b) -> b


**Controlar los paréntesis**

Para remover algunos paréntesis se pueden usar dos funciones: `($)` y `(.)`.

    -- Por defecto:
    f g h x         ⇔  (((f g) h) x)

    -- el $ reemplaza los paréntessis desde el $
    -- hasta el final de la expresión
    f g $ h x       ⇔  f g (h x) ⇔ (f g) (h x)
    f $ g h x       ⇔  f (g h x) ⇔ f ((g h) x)
    f $ g $ h x     ⇔  f (g (h x))

    -- (.) composición de funciones
    (f . g) x       ⇔  f (g x)
    (f . g . h) x   ⇔  f (g (h x))


## Notaciones útiles para funciones

Solo un recordatorio:

    x :: Int            ⇔ x es de tipo Int
    x :: a              ⇔ x puede ser de cualquier tipo
    x :: Num a => a     ⇔ x puede ser cualquier tipo a
                        que pertenezca a la class de typo Num
    f :: a -> b         ⇔ f es una función de a hacia b
    f :: a -> b -> c    ⇔ f es una función de a hacia (b→c)
    f :: (a -> b) -> c  ⇔ f es una función de (a→b) hacia c

Recuerda que definir el tipo de una función antes de su declaración no es
obligatorio. Haskell infiere el tipo más general por ti. Pero es
considerado una buena practica hacerlo de todos modos.


**Notación infijo**


```Haskell
square :: Num a => a -> a
square x = x^2
```

Nótese que `^` usa notación infijo. Para cada operador infijo hay una notación
prefijo asociada. Solo debe ponerse entre paréntesis.

```Haskell
square' x = (^) x 2
square'' x = (^2) x
```

Podemos remover `x` en el lado izquierdo y derecho!
Eso se llama reducción  η.

```Haskell
square''' = (^2)
```

Nótese que podemos declarar funciones con un `'` en su nombre:

    square ⇔ square' ⇔ square'' ⇔ square'''


**Tests**

```Haskell
absolute :: (Ord a, Num a) => a -> a
absolute x = if x >= 0 then x else -x
```

Nota: el `if .. then .. else` en Haskell es como el `algo ? algo : algo` en C.
No puedes olvidar el `else`

Otra versión equivalente:

```Haskell
absolute' x
    | x >= 0 = x
    | otherwise = -x
```

    Advertencia: la indentación es importante en Haskell. Como en
    Python, una mala indentación pueden dañar el código!

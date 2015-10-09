---
layout: post
title: Historia y Fundamentos
categories: serie js
permalink: serie/js/historia-y-fundamentos
---

# Una breve Historia...

##### [Fuente de Wikipedia](https://es.wikipedia.org/wiki/JavaScript)
JavaScript (abreviado comúnmente "JS") es un lenguaje de programación interpretado, dialecto del estándar ECMAScript. Se define como orientado a objetos,3 basado en prototipos, imperativo, débilmente tipado y dinámico.

Se utiliza principalmente en su forma del lado del cliente (client-side), implementado como parte de un navegador web permitiendo mejoras en la interfaz de usuario y páginas web dinámicas4 aunque existe una forma de JavaScript del lado del servidor (Server-side JavaScript o SSJS). Su uso en aplicaciones externas a la web, por ejemplo en documentos PDF, aplicaciones de escritorio (mayoritariamente widgets) es también significativo.

JavaScript se diseñó con una sintaxis similar al C, aunque adopta nombres y convenciones del lenguaje de programación Java. Sin embargo Java y JavaScript no están relacionados y tienen semánticas y propósitos diferentes.

Todos los navegadores modernos interpretan el código JavaScript integrado en las páginas web. Para interactuar con una página web se provee al lenguaje JavaScript de una implementación del Document Object Model (DOM).

Tradicionalmente se venía utilizando en páginas web HTML para realizar operaciones y únicamente en el marco de la aplicación cliente, sin acceso a funciones del servidor. Actualmente es ampliamente utilizado para enviar y recibir información del servidor junto con ayuda de otras tecnologías como AJAX. JavaScript se interpreta en el agente de usuario al mismo tiempo que las sentencias van descargándose junto con el código HTML.

Desde el lanzamiento en junio de 1997 del estándar ECMAScript 1, han existido las versiones 2, 3 y 5, que es la más usada actualmente (la 4 se abandonó5 ). En junio de 2015 se cerró y publicó la versión ECMAScript 66 

JavaScript fue desarrollado originalmente por Brendan Eich de Netscape con el nombre de Mocha, el cual fue renombrado posteriormente a LiveScript, para finalmente quedar como JavaScript. El cambio de nombre coincidió aproximadamente con el momento en que Netscape agregó compatibilidad con la tecnología Java en su navegador web Netscape Navigator en la versión 2.002 en diciembre de 1995. La denominación produjo confusión, dando la impresión de que el lenguaje es una prolongación de Java, y se ha caracterizado por muchos como una estrategia de mercadotecnia de Netscape para obtener prestigio e innovar en lo que eran los nuevos lenguajes de programación web.7 8

«JAVASCRIPT» es una marca registrada de Oracle Corporation.9 Es usada con licencia por los productos creados por Netscape Communications y entidades actuales como la Fundación Mozilla.10 11

Microsoft dio como nombre a su dialecto de JavaScript «JScript», para evitar problemas relacionadas con la marca. JScript fue adoptado en la versión 3.0 de Internet Explorer, liberado en agosto de 1996, e incluyó compatibilidad con el Efecto 2000 con las funciones de fecha, una diferencia de los que se basaban en ese momento. Los dialectos pueden parecer tan similares que los términos «JavaScript» y «JScript» a menudo se utilizan indistintamente, pero la especificación de JScript es incompatible con la de ECMA en muchos aspectos.

Para evitar estas incompatibilidades, el World Wide Web Consortium diseñó el estándar Document Object Model (DOM, o Modelo de Objetos del Documento en español), que incorporan Konqueror, las versiones 6 de Internet Explorer y Netscape Navigator, Opera la versión 7, Mozilla Application Suite y Mozilla Firefox desde su primera versión.[cita requerida]

En 1997 los autores propusieron12 JavaScript para que fuera adoptado como estándar de la European Computer Manufacturers 'Association ECMA, que a pesar de su nombre no es europeo sino internacional, con sede en Ginebra. En junio de 1997 fue adoptado como un estándar ECMA, con el nombre de ECMAScript. Poco después también como un estándar ISO.

## JavaScript en el lado servidor
Netscape introdujo una implementación de script del lado del servidor con Netscape Enterprise Server, lanzada en diciembre de 1994 (poco después del lanzamiento de JavaScript para navegadores web).13 14 A partir de mediados de la década de los 2000, ha habido una proliferación de implementaciones de JavaScript para el lado servidor. Node.js es uno de los notables ejemplos de JavaScript en el lado del servidor, siendo usado en proyectos importantes.15 16

## Desarrollos Posteriores
JavaScript se ha convertido en uno de los lenguajes de programación más populares en internet. Al principio, sin embargo, muchos desarrolladores renegaban del lenguaje porque el público al que va dirigido lo formaban publicadores de artículos y demás aficionados, entre otras razones.17 La llegada de Ajax devolvió JavaScript a la fama y atrajo la atención de muchos otros programadores. Como resultado de esto hubo una proliferación de un conjunto de frameworks y librerías de ámbito general, mejorando las prácticas de programación con JavaScript, y aumentado el uso de JavaScript fuera de los navegadores web, como se ha visto con la proliferación de entornos JavaScript del lado del servidor. En enero de 2009, el proyecto CommonJS fue inaugurado con el objetivo de especificar una librería para uso de tareas comunes principalmente para el desarrollo fuera del navegador web.18

En Junio de 2015 se cerró y publicó el estándar ECMAScript 619 20 (última versión hasta la fecha) con un soporte irregular entre navegadores21 y que dota a JavaScript de características avanzadas que se echaban de menos y que son de uso habitual en otros lenguajes como, por ejemplo, módulos para organización del código, verdaderas clases para POO, expresiones de flecha, iteradores, generadores o promesas para programación asíncrona.


# Fundamentos de JavaScript

## Tipos de Datos y Variables

en JavaScript exiten dos tipos de datos, Primitivos: (String, Number, Boolean, Null y Undefined) y de Objetos (Date, RegExp, Error, Function, Array). En este caso me voy a enfocar a definir variables con __Datos Primitivos__, en futuros articulos voy en profundidad los __Tipos de Objetos__.

Bien, ya que sabemos que cuales son los __Datos Primitivos__ vamos a definir variables. Para definir variables en JavaScript es muy facil a diferencia de otros lenguajes como Java por ejemplo que tenemos que definir el tipo de variable, en JavaScript solo con escribir la palabra reserva __var__ luego el nombre de la variable y seguido asignar el valor con el operador de asignación __=__ y terminado con __;__ aqui un ejemplo

{% highlight javascript %}
	var variable;
{% endhighlight %}

> __undefined__ es un valor que una variable declarable puede mantener, si no declaras la variable va a lanzar un error "is no defined". 

ahora ya que sabemos declarar variables, vamos a darle un valor primitivo

{% highlight javascript %}

// cadena de texto
var nombre = 'Jose';

// numero entero
var numero = 10;

// numero decimal
var decimal = 500.250;

// booleano
var verdad = true;
var falso = false;

// valor nulo
var valor = null;

{% endhighlight %}

> el nombre de la variable debe ser único, si declaras una variable con el nombre en CamelCase no es la misma que la que declaraste con minúscula siendo el mismo nombre JavaScript lo interpreta como otra variable. 

para reasignar el valor de una variable solo debes volver a declarar la variable pero esta vez sin la palabra reservada __var__

{% highlight javascript %}

var nombre = 'Jose';

nombre = 'Pedro';

nombre // ahora el valor de la variable es "Pedro"

{% endhighlight %}

## Operadores básicos

Los operadores se usan tal cual como en las matemáticas, suma, resta, multiplicación, división, compararación. aqui algunos ejemplos

{% highlight javascript %}

// suma o concatenación
var numero = 9;

numero + 2 // 11 como resultado en la consola

// un ejemplo concatenando cadenas de texto
var saludo = 'Hola';
var palabra = 'Mundo';

saludo + ' ' + palabra	

{% endhighlight %}

las demas operaciones son como tal en las matemáticas pero usando son respectivos simbolos: __-__ para resta, __/__ para division y __*__ para multiplicacion.

Los Operadores de Comparación comprueba si dos valores son iguales y devuelve un valor boleano (true o false), ahora algunos ejemplos con operaciones de comparación

{% highlight javascript %}

var miNumero = 12;
miNumero === 20; // false  

// en js el simbolo ! o !== equivalente al NOT es de negación, 
// con este podemos obligar a cambiar el resultado contrario

miNumero === 12; // aqui si es igual y por lo tanto es true
!miNumero === 12; // volvemos a obtener false porque usamos el simbolo de negación para obligar a cambiar el resultado contrario
miNumero !== 20 // "miNumero NO ES IGUAL a 20" asi que la salida es true

{% endhighlight %}

> porque __===__ y no __==__? esto puede ser raro, pero js trabaja asi, el __===__ compara el tipo de datos y el valor mientras que __==__ solo compara el valor, lo correcto es que uses __===__. Otros operadores son __&&__ equivalente a AND y __||__ equivalente a OR.


## Condicionales

Los condicionales son estructuras de código, donde dependiendo la expresión (true o false) se ejecuta el código dentro de la condición.

{% highlight javascript %}

var nombre = 'Jose';

// declaramos la palabra reservada if, y dentro la expresión.
if(nombre === 'Jose') {
	alert('Mi Nombre es ' + nombre) // si es true se ejecuta este alert
} else {
	alert('Ese no es mi nombre :(') // si es false se ejecuta este alert
}

{% endhighlight %}

## Bucles
un bucle tambien es una condicional, a diferencia de las demas es que se ejecuta repetidas veces hasta que sea __false__. Vamos a ver unos ejemplos con el bucle __for__.

{% highlight javascript %}
for ([expresion inicial]; [condicion]; [incremento])
  bloque de codigo
{% endhighlight %}

__expresión inicial__ es simplemente una variable con un valor inicial, la __condición__ es la expresión a evaluar, si es true se ejecuta el __bloque de código__ y el __incremento__ se actualiza.

{% highlight javascript %}

var ninjaArms = ["ninjato", "shuriken", "shikoro"];

for(var i = 0; i < ninjaArms.length; i++) { 
	console.log(ninjaArms[i]) 
}
// la salida en la consola: 
// ninjato
// shuriken
// shikoro

{% endhighlight %}

> los arrays en js se definen con [], en el proximo post vamos a profundizar los arrays (o vectores).

otro bucle en js es el do...while, este bucle trabaja muy similar al for.

{% highlight javascript %}

do
  bloque de codigo
while (condicion);

{% endhighlight %}

aqui el __do__ ejecuta el __bloque de código__ mientras la __condición__ sea true.

{% highlight javascript %}
 
var i = 0;

do {
  i += 1;
  console.log(i);
} while (i < 10);
// la salida en la consola es del 1 al 10.

{% endhighlight %}

otra forma de usar el __while__ es asi

{% highlight javascript %}
 
while (condicion)
  bloque de codigo

{% endhighlight %}

literalmente lo mismo pero sin el __do__, cuando la __condición__ llega a false se termina el bucle o de lo contrario sigue.

{% highlight javascript %}
 
n = 0;
x = 0;
while (n < 3) {
  n++;
  x += n;
}
// la salida en la consola es 6

{% endhighlight %}

## Funciones
Las Funciones son un bloque de código para definir una funcionalidad o tarea específica, las funciones tienen parámetros(se les llama argumentos cuando la función es invocada) y returnan un valor. Para definir una función declaramos la palabra reservada __function__ seguido el nombre de la función, seguido entre __()__ los parámetros y luego el bloque de código entre __{}__. Para invocar un función hay que llamarla, las funciones se pueden llamar cuando un evento ocurre o desde el mismo código JavaScript. Un ejemplo mas claro:

{% highlight javascript %}
 
function nombreDeFuncion(parametro1, parametro2, parametro3) {
    Bloque de codigo a ejecutar
}

{% endhighlight %}

las funciones retornan un valor, usando el __return__ retornamos un valor y puede parar la ejecución de la función, aqui un ejemplo de una función real

{% highlight javascript %}
 
function suma(numero1, numero2) {
    return numero1 + numero2
}

suma(20, 10)

// y nos retorna el valor que es 30

{% endhighlight %}

> en JavaScript se pueden almacenar funciones en variables mejor conocidas como __function expression__ o Funciones expresadas en español, en posts futuros cubriré mas en profundidad las funciones.


# Conclusión
Este es el primer post del Blog, por lo tanto, eso es lo fundamental y muy básico pero por ahora ya sabes que es JavaScript, un poco de su historia, sabes como declarar variables, bucles y funciones, por aqui va el camino...

























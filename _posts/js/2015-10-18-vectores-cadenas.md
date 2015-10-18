---
layout: post
title: Vectores y Cadenas de Caracteres
categories: serie js
permalink: serie/js/vectores-cadenas
---

# Vectores (Arrays)
Los Vectores o arreglos (arrays en ingles), como en las matemáticas son matrices unidimensionales o multidimensionales (no en todos los casos) en la programación usamos los vectores para manipular estructuras de datos. __Array__ un objeto global y constructor de JavaScript nos permite crear vectores.

## Creando vectores
Podemos crear vectores de dos formas, con el constructor del objeto __Array__ o de forma literal __[]__. sintaxis:

{% highlight javascript %}

// constructor
new Array(elemento, elemento, elemento) 

// o de forma literal
[elemento, elemento, elemento]

{% endhighlight %}

> los elementos dentro del vector son los valores (pueden ser numeros, cadenas de caracteres y hasta un objeto)

{% highlight javascript %}

// constructor
var ninjaSet = new Array('ninjato', 'shuriken', 10.33); 
ninjaSet // ["ninjato", "shuriken", 10.33]

// o de forma literal
var ninjaSet = ['ninjato', 'shuriken', 10.33]; 
ninjaSet // ["ninjato", "shuriken", 10.33]

{% endhighlight %}

> la forma literal es la recomendada, no llamamos el constructor y es mucho mas fácil de entender y de leer.

## Accediendo a los elementos de un vector
El orden de los valores de la matriz comienzan desde el 0.

{% highlight javascript %}

var ninjaSet = ['ninjato', 'shuriken', 10.33]; 

ninjaSet[0] // ninjato
ninjaSet[1] // shuriken
ninjaSet[2] // 10.33

{% endhighlight %}

támbien podemos modificar el slot de la matriz

{% highlight javascript %}

var ninjaSet = ['ninjato', 'shuriken', 10.33]; 

ninjaSet[0] // ninjato
ninjaSet[0] = 'sword';
ninjaSet[0] // sword

// y si llamamos el vector
ninjaSet // ['sword', 'shuriken', 10.33]

{% endhighlight %}

## Métodos Mutadores
Los Métodos mutadores modifican el vector.

### push()
Añade un elemento al final del vector y retorna la nueva longitud.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33]; 

ninjaSet.push('kunai')
// 4

ninjaWeapons // ["ninjato", "shuriken", 10.33, "kunai"]

{% endhighlight %}

### pop()
Elimina el último elemento del vector y lo retorna.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33, 'kunai']; 

ninjaSet.pop()
// "kunai"

ninjaWeapons // ["ninjato", "shuriken", 10.33]

{% endhighlight %}

### Array.prototype.reverse()
Invierte el orden de los elementos del vector.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33, 'kunai']; 

ninjaSet.reverse()
// [10.33, "shuriken", "ninjato"]

{% endhighlight %}

### sort()
Ordena los elementos del vector y lo retorna, el orden es por el valor de la posición del string unicode.

{% highlight javascript %}

var numeros = [1, 5, 8, 10];
numeros.sort() // [1, 10, 5, 8]

var ninjaWeapons = ['ninjato', 'shuriken','kunai']; 
ninjaWeapons.sort() // ["kunai", "ninjato", "shuriken"]

{% endhighlight %}

### shift()
Elimina el primer elemento del vector y lo retorna.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33, 'kunai']; 
ninjaWeapons.shift() // "ninjato"
ninjaWeapons // ["shuriken", 10.33, "kunai"]

{% endhighlight %}

### unshift()
Añade un elemento al comienzo de la matriz unidimensional y retorna la nueva longitud.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33, 'kunai']; 

ninjaWeapons.unshift(50.6) // 5

ninjaWeapons // [50.6, "ninjato", "shuriken", 10.33, "kunai"]

{% endhighlight %}

### splice()
Añade y elimina elementos de una matriz unidimensional.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33, 'kunai']; 

// añade 'shinwa' en la posicion 2 de la matriz
ninjaWeapons.splice(2, 0, 'shinwa') // []
ninjaWeapons // ["ninjato", "shuriken", "shinwa", 10.33, "kunai"]

// elimina 'shinwa' en la posicion 2 de la matriz
ninjaWeapons.splice(2, 1) // ["shinwa"]
ninjaWeapons // ["ninjato", "shuriken", 10.33, "kunai"]

// elimina 10.33 y añade 'musha'
ninjaWeapons.splice(2, 1, 'musha') // [10.33]
ninjaWeapons // ["ninjato", "shuriken", "musha", "kunai"]

// elimina desde la posicion del elemnto 0 al 2
ninjaWeapons.splice(0,2) // ["ninjato", "shuriken"]
ninjaWeapons // ["musha", "kunai"]

{% endhighlight %}

## Métodos accesores
Estos métodos no modifican la matriz unidimensional.

### concat()
Une dos vectores y retorna ambos unidos.

{% highlight javascript %}

var ninjaWeapons1 = ['ninjato', 'shuriken'];

var ninjaWeapons2 = ['shinwa', 'musha', 'kunai']

ninjaWeapons1.concat(ninjaWeapons2) // ["ninjato", "shuriken", "shinwa", "musha", "kunai"]

{% endhighlight %}

### join()
Une los elementos del vector y los retorna en una cadena de carácteres.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33, 'kunai']; 

// se le puede pasar un separador a la cadena mediante un parametro del metodo join()

ninjaWeapons.join() // "ninjato,shuriken,10.33,kunai"

ninjaWeapons.join(' ') // "ninjato shuriken 10.33 kunai"

ninjaWeapons.join(',') // "ninjato,shuriken,10.33,kunai"

ninjaWeapons.join('-') // "ninjato-shuriken-10.33-kunai"

ninjaWeapons.join('+') // "ninjato+shuriken+10.33+kunai"

{% endhighlight %}

### slice()
Extrae una parte de los elementos del vector y retorna una nuevo vector. 

> A diferencia de __splice()__ es que __slice()__ no modifica el vector.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33, 'kunai']; 

ninjaWeapons.slice(0,2) // ["ninjato", "shuriken"]
ninjaWeapons // ["ninjato", "shuriken", 10.33, "kunai"]

{% endhighlight %}

### toString()
Convierte el vector en cadena de carácteres.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33, 'kunai']; 

ninjaWeapons.toString() // "ninjato,shuriken,10.33,kunai"

{% endhighlight %}

### indexOf()
Retorna el índice donde se encuentra el elemento del vector.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 10.33, 'kunai']; 

ninjaWeapons.indexOf('kunai') // 3

{% endhighlight %}

## Métodos de Iteración
Estos métodos toman el vector mediante un __callback__ (función de orden superior) para ser procesados.

### forEach()
Llama un función para cada elemento en el vector.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 'musha', 'kunai'];

ninjaWeapons.forEach(function(value, index) { 
	console.log(index +': ' + value) 
});

/*
 0: ninjato
 1: shuriken
 2: musha
 3: kunai
*/

{% endhighlight %}

### map()
Crea un nuevo vector y lo retorna llamando cada elemento desde la función. El callback de este método tiene tres argumentos: el valor del elemento el índice y el vector que estamos llamando.

{% highlight javascript %}

var numbers = [10, 20, 30, 40, 50];

// en este caso solo tomamos el valor de cada elemento y lo multiplicamos por 2.
numbers.map(function(val, index, array) { console.log(val * 2)})

/*
   20
   40
   60
   80
   100
*/

{% endhighlight %}

### reduce()
Aplica un acumulador a cada elemento del vector a reducirlo a un solo valor. El callback de __Reduce__ tiene 5 argumentos: el valor previo del vector, el valor actual, el índice, el vector que estamos llamando y un valor inicial (opcional).

{% highlight javascript %}

var numbers = [10, 20, 30, 40, 50];

// el vector numbers lo reducimos a un simple valor
numbers.reduce(function(previousValue, currentValue, index, array) { 
	console.log(previousValue * currentValue)
})

// 200

{% endhighlight %}

### filter()
Busca un elemento en el vector, si lo encuentra retorna true de lo contrario es false.

{% highlight javascript %}

var ninjaWeapons = ['ninjato', 'shuriken', 'musha', 'kunai'];

// verificamos si en un elemento del vector se encuentra "ninjato"
ninjaWeapons.filter(function(element, index, array) { console.log(element === 'ninjato')})

// true

{% endhighlight %}

> todos los métodos vienen por defecto de la propiedad global __Array.prototype__.

## Creando Vectores de dos dimensiones
Támbien podemos crear Vectores multidimensional (dos dimensiones), los ejemplos anteriores los hemos visto vectores unidimensionales. Acá un ejemplo:

{% highlight javascript %}

var things = [ 
	[10, 20.33, 'ninjato', 438.235, 5333.32 ], 
	['ninja', 75, 802, 296, 'sword'], 
	['musha', 1262.142, 1387.22, 'samurai', 150], 
	[1540, 1702, 'kunai', 551, 333], 
	[1010, 'shuriken', 20.552, 'bomb'] 
];

// asi accedemos a los elementos
things[0][2] // "ninjato"
things[1][0] // "ninja"
things[1][4] // "sword"
things[2][0] // "musha"
things[2][3] // "samurai"
things[3][2] // "kunai"
things[4][1] // "shuriken"
things[4][3] // "bomb"

{% endhighlight %}

# Cadena de Carácteres (Strings)
Las cadenas de carácteres se encuentran en el constructor y objeto global __String__.

## Creando Cadenas de carácteres
Igual que los vectores las cadenas de carácteres se pueden crear desde el constructor o desde la forma literal.

{% highlight javascript %}

// constructor
new String('hola')
// aunque tambien podemos omitir la palabra reservada new
String('hey')

// literal
'Hola Mundo'

// con la propiedad length podemos contar los caracteres
var hello = 'Hello';
hello.length // 5

{% endhighlight %}

## Métodos
Las cadenas de carácteres al igual que los vectores tienen sus métodos que vienen de la propiedad global __String.prototype__, las cadenas son inmutables, es decir, no existen métodos mutadores en las cadenas de carácteres.

### toLowerCase() y toUpperCase()
Retorna la cadena de carácteres en minúsculas (toLowerCase) o mayúsculas (toUpperCase).

{% highlight javascript %}

var hello = 'hello';

var helloMayus = hello.toUpperCase();
helloMayus // "HELLO"

helloMayus.toLowerCase() // "hello"

{% endhighlight %}

### charAt()
Retorna el específico carácter entre la longitud de la cadena de carácteres.

{% highlight javascript %}

var hello = 'hello';

hello.charAt(0) // "h"
hello.charAt(1) // "e"
hello.charAt(2) // "l"
hello.charAt(3) // "l"
hello.charAt(4) // "o"

{% endhighlight %}

### charCodeAt()
Retorna el valor numerico unicode del específico carácter entre la longitud de la cadena de carácteres.

{% highlight javascript %}

var hello = 'hello';

hello.charCodeAt(0) // 104
hello.charCodeAt(1) // 101
hello.charCodeAt(2) // 108
hello.charCodeAt(3) // 108
hello.charCodeAt(4) // 111

{% endhighlight %}

### fromCharCode()
Es un método estático que retorna una cadena de carácteres desde un valor numerico unicode.

{% highlight javascript %}

// el ejemplo anterior conseguimos el valor unicode de cada caracter con charCodeAt()
String.fromCharCode(104, 101, 108, 108, 111) // "hello"

{% endhighlight %}

### concat()
Une dos cadenas de carácteres y retorna una nueva.

{% highlight javascript %}

var hello = 'hello';

hello.concat('world!') // "helloworld!"

{% endhighlight %}

### slice() y substring()
Extrae una parte de los elementos de la cadena de carácteres y retorna una nueva cadena. El primer argumento es donde comienza la extracción y el segundo es el final de la extracción, el segundo argumento es opcional y si es omitido se extrae hasta el final de la cadena de carácteres.

{% highlight javascript %}

var hello = 'hello';

// con slice
hello.slice(3,5) // "lo"
hello.slice(3,4) // "l"
hello.slice(2,4) // "ll"
hello.slice(2) // "llo"

// con substring
hello.substring(1, 3) // "el"
hello.substring(1, 4) // "ell"
hello.substring(2, 4) // "ll"
hello.substring(0, 2) // "he"

{% endhighlight %}

> Ambos métodos __substring()__ y __slice()__ hacen el mismo trabajo.

### substr()
Extrae una parte de los elementos de la cadena de carácteres y retorna una nueva cadena específicando la longitud. El primer argumento es donde comienza la extracción y el segundo argumento el número de carácteres a extraer, este segundo argumento es opcional si se omite se extrae la cadena completa.

{% highlight javascript %}

var hello = 'hello';

hello.substr(0, 1) // "h"
hello.substr(0, 2) // "he"
hello.substr(0, 3) // "hel"
hello.substr(0, 4) // "hell"
hello.substr(0, 5) // "hello"

// si omitimos el segundo argumento
hello.substr(1) // "ello"
hello.substr(2) // "llo"
hello.substr(3) // "lo"
hello.substr(4) // "o"

// mas ejemplos
hello.substr(3,4) // "lo"
hello.substr(1,4) // "ello"
hello.substr(4,4) // "o"

{% endhighlight %}

### split()
Divide la cadena de carácteres a un vector unidimensional, a este método se le pasan dos argumentos: el primero es el separador, el segundo es el limite de la longitud de la cadena de carácteres, ambos argumentos son opcionales.

{% highlight javascript %}

var hello = 'hello';

hello.split() // ["hello"]
hello.split('') // ["h", "e", "l", "l", "o"]
hello.split('', 2) // ["h", "e"]
hello.split('', 3) // ["h", "e", "l"]
hello.split('', 1) // ["h"]

{% endhighlight %}

### trim()
Remueve el espacio de la cadena de carácteres.

{% highlight javascript %}

var hello = '  hello  ';

hello.trim() // "hello"

{% endhighlight %}

### indexOf()
Retorna el índice donde se encuentra el específico carácter en la cadena de carácteres. El primer argumento es el carácter a buscar, el segundo argumento es el final desde el índice (este es opcional).

{% highlight javascript %}

var hello = 'hello';

hello.indexOf('h') // 0
hello.indexOf('e') // 1
hello.indexOf('l') // 2
hello.indexOf('o') // 4
hello.indexOf('he') //0
hello.indexOf('e', 1) // 1

{% endhighlight %}

## Expresiones Regulares
Las Expresiones Regulares son patrones de búsqueda, concidencia y reemplazo de cadenas de carácteres

### Creando Expresiones Regulares
Los pátrones pueden ser creados con el constructor del objeto __RegExp__ o de forma literal.

{% highlight javascript %}

// constructor
var pattern = new RegExp("ab+c");

// literal
var pattern = /ab+c/;

{% endhighlight %}

### Modificadores
Los modificadores son usados para buscar coincidencias entre mayúsculas, minúsculas y palabras globales.

|  Modificador  |  Descripción  |
| ------------- | ------------- |
|       i 	    | coincide entre mayúsculas y minúsculas. |
|       g 	    | Encuentra todas las coincidencias. |
|       m       | Coincidencia multilinea. |

### Soporte

|  Expresión  	|  Descripción  |
| ------------- | ------------- |
|     [abc]		| Encuentra cualquier carácter entre los corchetes. |
|     [^abc] 	| Encuentra cualquier carácter que no este entre los corchetes. |
|     [0-9]     | Encuentra cualquier dígito entre los corchetes. |
|     [^0-9]    | Encuentra cualquier dígito que no este entre los corchetes. |
|     (x|y)     | Encuentra cualquiera de las alternativa (ya sea x o y por ejemplo). |

### Metacarácteres

|  	 Carácter  	|  Descripción  |
| ------------- | ------------- |
|     	.		|  Encuentra cualquier carácter pero no un carácter de nueva linea. |
|      \w		|  Encuentra cualquier carácter sea alfanumerico, equivalente a [A-Za-z0-9_]. |
|      \W  		|  Encuentra cualquier carácter que no sea una palabra, equivalente a [^A-Za-z0-9_]. |
|      \d  		|  Encuentra cualquier dígito, equivalente a [0-9]. |
|      \D    	|  Encuentra cualquier carácter que no sea un dígito, equivalente [^0-9]. |
|      \s  		|  Encuentra un simple espacio en blanco, equivalente a [\f\n\r\t\v​\u00a0\u1680​\u180e\u2000​\u2001\u2002​\u2003\u2004​\u2005\u2006​\u2007\u2008​\u2009\u200a​\u2028\u2029​​\u202f\u205f​\u3000].|
|      \s  		|  Ecuentra un carácter que no tenga espacio en blanco, equivalente a [^ \f\n\r\t\v​\u00a0\u1680​\u180e\u2000​\u2001\u2002​\u2003\u2004​\u2005\u2006​\u2007\u2008​\u2009\u200a​\u2028\u2029​\u202f\u205f​\u3000].|
|      \b  		|  Encuentra una coincidencia desde el inicio hasta el final de una palabra. |
|      \B  		|  Encuentra una coincidencia desde el inicio hasta el final que no sea una palabra. |
|      \0  		|  Encuentra un carácter nulo. |
|      \n  		|  Encuentra una nueva linea en el carácter. |
|      \f  		|  Encuentra una forma de alimentación. |
|      \r    	|  Encuentra un retorno de carro. |
|      \t    	|  Encuentra un tabulador. |
|      \v    	|  Encuentra un tabulador vertical. |

### Cuantificadores

|  	 Carácter  	|  Descripción  |
| ------------- | ------------- |
|       +		|   Encuentra uno o más carácteres, equivalente a {1,}. |
|      	*		|   Encuentra desde 0 o más carácteres, equivalente a {0,}. |
|       ? 		|   Encuentra 0 o 1 carácter, es equivalente a {0,1}. |
|       $ 		|   Encuentra cualquier carácter al final de la cadena. |
|       ^  		|   Encuentra cualquier carácter al comienzo de la cadena. |

### Métodos

|  	 Carácter  	|  Descripción  |
| ------------- | ------------- |
|    exec		| Ejecuta una coincidencia en una cadena de carácteres y lo devuelve en un vector.  |
|    test		| Ejecuta una coincidencia en una cadena de carácteres y devuelve un boleano (true si coincide, de lo contrario retorna false) |
|    match		| Ejecuta una coincidencia en una cadena de carácteres y lo devuelve en un vector de lo contrario devuelve null. |
|    search		| Ejecuta una coincidencia en una cadena de carácteres y lo devuelve el índice de la coincidencia de la contrario -1. |
|    replace	| Ejecuta una coincidencia en una cadena de carácteres y reemplaza la subcadena encontrada. |

#### Ejemplos
{% highlight javascript %} 

var texto = 'Hola, estoy aprendiendo javascript'; 
var pattern = /javascript/; 

// exec()
pattern.exec(texto) // ["javascript"] 

// test()
pattern.test(texto) // true

// match()
texto.match(pattern) // ["javascript"]

// search()
texto.search(pattern) // 24

// replace 
texto.replace(pattern, 'js') // "Hola, estoy aprendiendo js"

{% endhighlight %}

# Conclusión
Los vectores y cadena de carácteres son muy importantes en cualquier lenguaje de programación, en este articulo aprendimos a manipular estructuras de datos con vectores, modificar, ordenar y transformar los elementos de un vector, las expresiones regulares es otro punto muy importante en la programación de toda una decada, esto nos ayuda a buscar, emparejar o reemplazar cadenas de carácteres en un algoritmo.

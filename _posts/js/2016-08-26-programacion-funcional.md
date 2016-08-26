---
layout: post
title: Programación Funcional
categories: serie js
permalink: serie/js/programacion-funcional
---

Bien sabemos que JavaScript no es un lenguaje Funcional del todo, comparandolo con Haskell, Scala, Clojure, Erlang y algunos otros. Sin embargo, __JavaScript__ tiene sus características que podemos adaptar el Paradigma de __Programación Funcional__.

Ahora, que es la __Programación Funcional__?

> La __Programación Funcional__ es un paradigma de programación, un estilo de construir la estructura y elementos de programas de computadores, que trata los cálculos como la evaluación de funciones matemáticas y evita el cambio de estado y mutar la estructura de datos, se puede decir que también es un Paradigma de __Programación Declarativa__ ya que depende de las expresiones o declaraciones en vez de estados. En la __Programación Funcional__ el valor de la salida de una función depende solo en los argumentos que son la entrada a la función, así eliminando efectos secundarios, cambios en el estado que no dependen en las entradas de la función, mas legible para entender y ver el comportamiento del programa.

En este Post tocare los temas mas importantes de la __FP__, si ya has trabajado con lenguajes funcionales ya estas muy consciente, de lo contrario terminaras entendiendo los conceptos claves de la __FP__. 


# Funciones de Primera Clase
Cuando decimos __Funciones de Primera Clase__ no hablamos de una Clase como tal, nos referimos al tipo de dato, por ejemplo: un número o una cadena de carácteres puede ser una __Función de Primera Clase__. Esto significa que podemos tratar Funciones como cualquier tipo de dato (en vez de tomar un número o cadena), no existe ningún método, palabra clave o algo especial para definirlas, en este caso decimos que las funciones se pueden asignar en una variable, objecto o incluso un vector.
Veamos unos simples ejemplos en código.

un número se puede asignar a una variable y así asignamos una función retornando un número:

{% highlight javascript %}
	var number = function() { return 24 };
{% endhighlight %}


un número se puede asignar a un espacio en un vector y así una función:

{% highlight javascript %}
	var arrayNumbers = [20, function() { return 24 }];
{% endhighlight %}


un número puede ser asignado a un campo de un objeto y así una función:

{% highlight javascript %}
	var objectNumbers = {
		number: 22, 
		fun: function() { return 24 }
	};
{% endhighlight %}


un número puede ser creado como sea necesario y también una función:

{% highlight javascript %}
	20 + (function() { return 24 })();
	// => 44
{% endhighlight %}



un número puede ser pasado a una función y así puede hacerlo una función:

{% highlight javascript %}
	function sumFunc(n, f) { return n + f() }
	sumFunc(20, function() { return 24 });
	// => 44
{% endhighlight %}

un número puede ser retornado desde una función y así puede hacerlo una función:

{% highlight javascript %}
	return 42;
	return function() { return 42 };
{% endhighlight %}

Como se ve en los ejemplos las __Funciones de Primera Clase__ no tienen nada de especial, solo las tomamos como cualquier otro tipo de dato pero en este caso es una función retornando un valor. Los últimos dos ejemplos le llamamos __Funciones de Orden Superior__ que es el siguiente tema a tratar.


# Funciones de Orden Superior
En esta capítulo del Post vamos a extender nuestra idea de __Funciones de Primera Clase__, aqui voy a describir que las funciones no solo puede ser asignadas a una variable, objeto, vector o pasadas como cualquier tipo de dato, ellas también pueden ser retornadas desde otras funciones.
Las __Funciones de Orden Superior__ toman dos acciones claves: toman una función como un argumento y retorna una función como un resultado, fundamental para el paradigma de __Programación Funcional__, ahora ya estas listo para ver la explicación en código!

Vamos a desglosar este tópico en un programa muy sencillo para su buen entendimiento del mismo, vamos allá!

{% highlight javascript %}

	var add = function(x) {
		return function(y) {
			return x + y;
		};
	};

	var sum = add(2);
	sum(2);
	// => 4

{% endhighlight %}

Bien, ahora vamos a dividir esta explicación en partes: 

Primero he asignado una función a una variable llamada __add__ como si fuese un tipo de dato (__Funciones de Primera Clase__ que vimos anteriormente), este tipo de funciones son también conocidas como __Funciones Anónimas__ o algunos lenguajes de programación se les conoce como __Lambda__, esta función anónima toma un argumento.

Luego dentro del __ámbito__ (Es decir, es un closure) de __add__ retornamos otra función anónima con su argumento y así tomando el argumento del closure y la de esta para retornar una suma de los dos argumentos.

Para llamar la función debemos asignarla a una variable con su respectivo parámetro a la primera función (la asignamos a una variable porque no queremos que se ejecute, si solo ejecutamos la primera función no va a devolver el resultado), luego ejecutamos la segunda función asignandole su respectivo parámetro y listo nos devuelve el resultado.

Es algo tedioso llamar las __Funciones de Orden Superior__ (te puedes imaginar si tienes muchas más funciones que este simple ejemplo? xD), para esto usamos una técnica llamada __Currying__ muy usada en el paradigma de __Programación Funcional__. 
Pero que diablos es __Currying__?

## Currying

> __Currying__ es una forma de construir funciones por partes mientras que permita la aplicación parcial de los argumentos de una función.

Es decir podriamos pasar todo los argumentos de una función en partes y ejecutarla de una vez, muy simple y así reduce la complejidad en el código, vamos a ejecutar el ejemplo de arriba usando __Currying__, veamos!

{% highlight javascript %}

	var add = function(x) {
		return function(y) {
			return x + y;
		};
	};

	add(2)(2);
	// => 4

{% endhighlight %}

Muy simple! así solo tenemos que llamar la función una sola vez pasandole los argumentos o resto de elementos a las funciones en parcial. En lenguajes como Haskell, Scala, ya vienen por default con esta feature ya que son lenguajes funcionales totalmente, en cambio en __JavaScript__ no viene por default, la técnica de __Currying__ se aplica como un tipo de truco o hack para hacerlo trabajar como __Currying__.

Sin embargo podemos usar alguna librería Open Source que nos facilite mucho la vida para estos procesos funcionales, voy a tomar el mismo ejemplo pero usando [__Lodash__](https://lodash.com/), vamos alla!

{% highlight javascript %}

	var add = _.curry(function (x, y) {
  		return x + y;
	})

	add(2)(2);
	// => 4

{% endhighlight %}

El método __curry__ de [__Lodash__](https://lodash.com/) toma una función como argumento (un callback) transformando los argumentos de este callback en una función __Currying__.

## Map, Reduce y Filter 
Pues si, __JavaScript__ tiene funciones pre-construidas (a partir de ES5) que toman una función como argumento y retornan un valor y esas funciones son __map()__, __reduce()__ y __filter()__ para vectores, veamos un ejemplo de cada una tomando esta simple estructuras de datos: 

{% highlight javascript %}

	var students = [
	    { name: 'Anna', grade: 6 },
	    { name: 'John', grade: 4 },
	    { name: 'Maria', grade: 9 }
	];	

{% endhighlight %}



con __filter()__

{% highlight javascript %}	

	students.filter(function(student) {
	  return student.grade >= 6;
	});

	// [ { name: 'Anna', grade: 6 }, { name: 'Maria', grade: 9 } ]

{% endhighlight %}



con __map()__

{% highlight javascript %}	

	students.map(function(obj) {
	  return obj.name;
	});	

	// [ 'Anna', 'John', 'Maria' ]

{% endhighlight %}



con __reduce()__

{% highlight javascript %}	

	students.reduce(function(sum, student) { 
		return sum + student.grade;
	}, 0)

	// 19

{% endhighlight %}

Encadenando métodos

{% highlight javascript %}

	students
			.filter(function (student) { 
				return student.grade >= 6 
			})
			.map(function (obj) {
				return obj.name;
			})

	// ['Anna', 'Maria']

{% endhighlight %}

Como vemos __map()__, __reduce()__ y __filter()__ son __Funciones de Orden Superior__ que vienen en el core de __JavaScript__, estas funciones toman datos de un vector pasandolo al callback y retornan un valor transformado sin mutar el vector original, luego veremos en profundida la parte de las __Funciones Puras__ e __Immutabilidad__, la próxima parada es para la __Composición de Funciones__.


# Composición
Yo pienso que __Componer Funciones es un arte__, pero realmente en un contexto en general, que es Composición?

> Formación de un todo o un conjunto unificado uniendo con cierto orden una serie de elementos.
por Google.

Así es, Los elementos serian las funciones que van transformando las entradas, ya sabemos que una función puede ser declarada como cualquier otro tipo de dato y que las funciones pueden retornar otras funciones, pero no como pueden ser compuestas para transformar datos, ahora veamos un simple ejemplo:

{% highlight javascript %}	

	var compose = function(f,g) {
		return function(x) {
			return f(g(x));
		};
	};

{% endhighlight %}

__f__ y __g__ son funciones y __x__ el valor para ser transformado a través de ellas.

La composición de dos funciones retorna una nueva función, componer dos unidades de un tipo de dato en este caso una función, deberia retornar una nueva unidad de ese tipo, similar a __map()__, __reduce()__ y __filter()__ solo que estas funciones heredan del __Array.prototype__ y retornan es un nuevo vector.

> Componer funciones es como construir un juego de Lego, combinas dos pieza (dos funciones) y tienes una pieza completa (retorna una nueva función).

Ahora vamos a llamar __compose__ y veamos los resultados: 

{% highlight javascript %}	

	var toReverse = function (string) {
	  return string.split('').reverse().join('');
	};

	var toString = function (string) {
	  return 'Return reverse String ' + string;
	};

	var reverseString = compose(toString, toReverse);

	reverseString('Hello JavaScript');

	// -> Return reverse String tpircSavaJ olleH

{% endhighlight %}

En la función __compose__, el argumento __g__ se ejecuta antes que __f__, creando un flujo de datos de derecha a izquierda, así es mucho más legible que anidar varias llamadas de funciones, sin __compose__, debería ser así:

{% highlight javascript %}	
	
	var reverseString = function (string) {
  		return toString(toReverse(string));
	} 	

	// -> Return reverse String tpircSavaJ olleH

{% endhighlight %}

Trabaja igual, pero como ves el flujo cambia, en vez de ser de adentro hacia fuera __compose__ lo hace de derecha a izquierda,
la secuencia también importa, __composición__ viene muy fuerte de las Matemáticas, __Teoría de las Categorías__ para ser especifico (tranquilo, no tocare este tema en profundidad, solo ejemplos con código), vamos que a ver algunas propiedades de la __Composición__:

{% highlight javascript %}	
		
	var associative = compose(f, compose(g, h)) == compose(compose(f, g), h);
	// true

{% endhighlight %}

La Composición es asociativa, es decir que no importa como se agrupen las propiedades, ahora vamos a probar con un ejemplo:

{% highlight javascript %}	
	
	compose(toUpperCase, compose(toString, toReverse));

	// o

	compose(compose(toUpperCase, toString), toReverse);

{% endhighlight %}

Como visualizamos en el ejemplo de arriba, no importa como nosotros agrupamos nuestra llamada a __compose__, el resultado puede ser el mismo, así podemos componer varias funciones.	


# Pureza 
Una función pura es una función que dada la misma entrada puede siempre retornar la misma salida, no produce efectos sedundarios y no confia en estados externos.
Muy buenas razones para usar __funciones puras__ y parte de la __Programación Funcional__, suelen ser muy simples, reusables y muy fácil para testear, manteniendo el principio de diseño __KISS (Keep It Simple, Stupid)__. Las __funciones puras__ son totalmente independientes, no dependen de estados externos del programa, así evitando bugs y manteniendo el código mas flexible y ordenado.

Ahora vamos a ver un ejemplo de una simple función pura que dada la misma entrada retorna la misma salida:

{% highlight javascript %}	
	
	function double (x) {
		return x * 2;
	}		

{% endhighlight %}

Así de simple es una __función pura__, toma una entrada (argumento) y retorna una salida (retornando el mismo resultado), sin modificar estados externos, sin efectos secundarios, ahora vamos a tomar este mismo ejemplo para una función impura.

{% highlight javascript %}

	var number;

	function double (x) {
		number = x;
		return number * 2;
	}		

{% endhighlight %}

En este caso la función __double()__ toma una variable global, asignandole dentro de su contexto el argumento y así modificando su valor, es decir __double()__ depende de una variable mutable (modificando estados externos como he comentado), a las __funciones puras__ no les agrada modificar estados externos, y debemos tenerlo en cuenta para evitar bugs.

Vamos hacer lo mismo para mostrar un ejemplo de una función con efectos secundarios:

{% highlight javascript %}

	function double (x) {
		var doubleResult = x * 2;
		getId(doubleResult);
	}	

{% endhighlight %}

> Un efecto secundario es cambiar el estado o interación global que ocurre durante el cálculo de un resultado.

En este caso la función __double()__ tiene en su contexto la función __getId()__ que toma como argumento el resultado, como vemos __double()__ depende de __getId()__, esto produce efectos secundarios ya que el resultado lo toma otra función y mutar datos, __getId()__ problamente sea una petición http (Ajax) obteniendo un __id__, otros efectos secundarios serian como: insertar datos en una base de datos, manipular el DOM, cambiar el sistema de archivos o simplemente mutar estados globales del programa.

Las __funciones puras__ solo confían en estados __immutables__, para el próximo paso vamos a tocar este tema de __Immutabilidad__.


# Immutabilidad
Como hemos visto anteriormente, las __funciones puras__ no confían en mutar datos (modificar el estado del programa), cuando tenemos una función que modifica una propiedad en un objeto o vector estamos modificando el estado desde el contexto global, las funciones puras no deben mutar el estado externo del programa.
Veamos los ejemplos:

{% highlight javascript %}

	function addPleople (pleopleData, person) {  
	  pleopleData.push(person);  
	  return pleopleData;
	}

	var data = [];
	var uuid = parseInt(Math.floor(Math.random() * 1000000), 10);
	var me = {id: uuid, name: 'Jose', last_name: 'Lopez', role: 'Javascript Dev'};

	addPleople(data, me);

{% endhighlight %}

__addPleople()__ es una función que toma dos argumentos, un vector __pleopleData__ y un objeto __person__, la función trabaja sin problemas, agrega una persona a los datos de personas, pero el problema es que estamos mutando el estado que compartimos en la función, __addPleople()__ depende de __peopleData__ (recuerda que las __funciones puras__ son independientes) y modificando su estado original, para resolver este problema podriamos clonar los datos antes de enviarlos.

Voy a usar __lodash__ para clonar los datos, veamos esta versión del ejemplo:

{% highlight javascript %}

	function addPleople (pleopleData, pleople) { 
	  var pleopleClone = _.cloneDeep(pleopleData);  
	  pleopleClone.push(pleople);  

	  return pleopleClone;  
	}

	var data = [];
	var uuid = parseInt(Math.floor(Math.random() * 1000000), 10);
	var me = {id: uuid, name: 'Jose', last_name: 'Lopez', role: 'Javascript Dev'};

	addPleople(data, me);

{% endhighlight %}

Como hemos visto en este ejemplo, ahora tomamos los datos y lo clonamos, luego enviamos el objeto y retornamos el resultado, así evitamos mutar el estado original de la aplicación y ademas en el contexto de la función siendo __addPleople()__ una función totalmente independiente.

# Memoización
Como hemos mencionado las __funciones puras__ toman entradas que se pueden repetir muchas veces, esas entradas pueden ser cacheadas, para esto usamos una técnica llamada __Memoización__ que es una técnica de optimización para almacenar resultados de llamadas a funciones y retorna el resultado cacheado cuando la misma entrada ocurre de nuevo.

Vamos a implementar un ejemplo usando __Memoización__:

{% highlight javascript %}

	var memoize = function(f) {
		var cache = {};
		return function() {
			var arg_str = JSON.stringify(arguments);
			cache[arg_str] = cache[arg_str] || f.apply(f, arguments);
			return cache[arg_str];
		};
	};	
	
	var squareNumber = memoize(function(x){ return x*x; });

	squareNumber(4)
	// -> 16

	squareNumber(4)
	// el mismo resultado pero ya cacheado

	squareNumber(5);
	// -> 25

	squareNumber(5)
	// el mismo resultado pero ya cacheado

{% endhighlight %}

Si repetimos la misma entrada nos retorna el mismo resultado pero ya desde el "cache".

# Recursión
La __Programación Funcional__ esta muy relacionada con la __Recursividad__. La __Recursión__ básicamente es que una función pueda llamarse así misma para resolver una instancia de un problema.
__Recursión__ suele aplicarse cuando necesitamos llamar la misma función repetidamente con diferentes parámetros desde un bucle. Podemos resolver diferentes problemas, el más común es iterar sobre una estructura de datos hasta encontrar, calcular o ordenar y obtener el resultado.
Las funciones __recursivas__ son puras, ya que no comparte el estado con variables locales, así siendo muy faciles de testear. Retornan un valor para cualquier entrada sin efectos secundarios en variables globales.

Veamos un típico ejemplo de una función factorial:

{% highlight javascript %}

	function factorial (number) {
	  if (number <= 0) { // terminal case
	    return 1;
	  } else { // block to execute
	    return (number * factorial(number - 1));
	  }
	};

	factorial(4); // -> 24

{% endhighlight %}

__factorial()__ es una función recursiva donde toma un argumento, en este caso es un número y si este número es menor o igual a 0 retorna 1 de lo contrario retorna el número multiplicado por el factorial, como vemos llamamos la función así misma para retornar el resultado final.
Y así trabajan las funciones recursivas, como un bucle o condicional, de hecho en algunos lenguajes que no tienen condicionales o bucles por default suelen usar __recursividad__.


# Conclusión
El paradigma de __Programación Funcional__ es mucho más complejo y extenso, pero no es la idea hacerlo mas complejo y extenso, este post te puede dar un conocimiento en general de la __Programación Funcional__, se consciente de que __JavaScript__ no es un lenguaje 100% funcional, sin embargo tiene sus características funcionales que las debemos aprovechar al máximo, si usas lenguajes funcionales ya esto sera muy fácil para ti.
Lo más cool es que con __JavaScript__ podemos combinar __Programación Orientada a Objetos__ y __Funcional__ dependiendo de nuestras necesidades o del problema que nos toque resolver.
Y así concluyó este post sobre __Programación Funcional en JavaScript__, espero que te haya gustado y recibido toda esta info para aplicarla en tus programas.


---
layout: post
title: Prototipos y Herencias de Prototipos
categories: serie js
permalink: serie/js/herencias-de-prototipos
---

Otro gran tópico en __JavaScript__ es la __Programación Orientada a Objetos(OOP)__, suele ser muy confuso porque no existen Clases y Herencias como en otros lenguajes.
En __JavaScript__ podemos hacer __Programación Orientada a Objetos(OOP)__ o como dicen algunos "emular" Clases y Herencias con __Prototipos__, para empezar y termines con un buen entendimiento de los __Prototipos__ en __JavaScript__ es requerido que tengas conocimientos de como trabajan los [Objetos](http://elblogdejavascript.com/serie/js/objetos/) y los [Contextos](http://elblogdejavascript.com/serie/js/this-y-contextos/).

# Concepto de Clases y Herencias
Antes de entrar a la __Programación Orientada a Objetos(OOP)__ basada en __Prototipos__ debemos saber que son las Clases y Herencias en general.
una __Clase__ es la forma en que organizamos el código a un asociado comportamiento, una __Clase__ también implica a una forma de clasificar cierta estructura de datos, vamos a tomar un ejemplo en teoría: una implementación específica de Vehiculo, existen diferentes tipo de vehiculos, un Carro por ejemplo, tiene ciertas características comunes como motor, propulsion, marca, caja, etc.

Definimos Carro como un tipo de Vehiculo, en un programa de Software aplicando la __Programación Orientada a Objetos(OOP)__ podemos indicar que Carro hereda o se extiende de Vehiculo.
Vehiculo y Carro trabajan en conjunto para definir un comportamiento mediante métodos y datos, por ejemplo teniendo en cuenta que cada Carro debe tener su propio identificador en este caso podria ser el número de placa.

En todas las clases existe un método especial llamado __Constructor__, este método toma los argumentos de que se le pasa a la clase cuando es instanciada e inicializa la el estado de la clase.

Otro concepto clave de __Clases y Herencias__ es el __Polimorfismo__, se describe como un comportamiento general de todo el programa desde una clase padre que puede sobre escribir métodos y datos en una clase hija para conseguir comportamientos más específicos, aunque la teoría de clases propone que la clase padre y la clase hija compartan los mismos métodos y datos para cierto comportamiento así la clase hija pueda sobreescribir la clase padre.

> Una simple descripción de __Clases y Herencias__ para tener una idea básica de lo que vamos a tratar en los siguientes temas.

# Prototipos
Todos los objetos en __JavaScript__ tienen una propiedad interna específicada como __[[Prototype]]__ por defecto, un __Prototipo__ desde el cual puede heredar propiedades de otro objeto, los __Prototipos__ tienen sus __Prototipos__ (esto es llamado __Cadena de Prototipos__ que mas tarde voy a describir en este post). En __JavaScript__ si no es un primitivo es un objeto, por eso decimos que en __JavaScript todo es un objeto__.

Un método accesor estándar __Object.getPrototypeOf(object)__ cual puede ser implementado en Firefox, Safari, Chrome and IE9. Otro método accesor aunque no es un estándar es ____proto____ y es soportado en todos los navegadores excepto IE.

{% highlight javascript %}

var obj = {};

Object.getPrototypeOf(obj); //[object Object]

obj.__proto__; //[object Object]

{% endhighlight %}

Podemos considerar un ejemplo de un objeto que tiene un vinculo vacio a __[[Prototype]]__

{% highlight javascript %}

var obj = {
  a: 10
};

obj.a // 10

{% endhighlight %}

en este ejemplo nosotros ejecutamos el operador __[[Get]]__ del __[[Prototype]]__, el operador __[[Get]]__ es invocado cuando accedemos a una propiedad de un objeto como lo hicimos en __obj.a__, por defecto el operador revisa si la propiedad en el objeto esta y si esta la ejecuta.
Pero que pasa si queremos llamar nuestra propiedad desde otro objeto o que no esta en ese objeto? sencillo, podemos vincular los objetos mediante el __[[Prototype]]__ con el método __Object.create()__ (de EcmaScript5), vamos a considerar un ejemplo:

{% highlight javascript %}

var obj = {
  a: 10
};

var otroObj = Object.create(obj);

otroObj.a // 10

{% endhighlight %}

que paso aca? el __Object.create()__ crea un objeto __otroObj__ vinculado con la propiedad interna __[[Prototype]]__ de el objeto __obj__ y asi podemos acceder a la propiedad y __[[Get]]__ es ejecutado.

Un método que nos ayuda a comprobar si una propiedad es definida en su cadena de prototipos es __hasOwnProperty()__, de esta manera:

{% highlight javascript %}

var obj = {
  a: 10
};

var otroObj = Object.create(obj);

otroObj.a // 10

otroObj.hasOwnProperty('a') // false
obj.hasOwnProperty('a') // true

{% endhighlight %}

# Cadena de Prototipos
La __Cadena de Prototipos__ termina hasta llegar a un valor __undefined__ o __null__ de lo contrario sigue, todos los objetos en __JavaScript__ heredan propiedades y métodos de __Object.prototype__. Estas propiedades heredadas son __constructor__, __hasOwnProperty()__, __isPrototypeOf()__, __propertyIsEnumerable()__, __toLocaleString()__, __toString()__, y __valueOf()__ y cuatro métodos accesores que son agregados en __EcmaScript 5__ al __Object.prototype__.

Ahora, de que nos sirven los __Prototipos__? como hemos visto __JavaScript__ es un lenguaje __Basado en Prototipos__ ya que todo es un objeto, distinto a otros lenguajes con la clásica __Programación Orientada a Objetos__ de Clases y Herencias que todos conocemos, y si, aprovechando esta gran característica de la __Herencia basada en Prototipos__ para agregar funcionalidad a nuestras funciones/objetos.

## Constructores
Lo primero que debemos saber antes de comenzar con las __Herencias basadas en Prototipos__ es sobre los __Constructores__ (en el primer tema se encuentra un breve concepto de __Constructores__) pero si en __JavaScript__ no existe la palabra reservada __class__ ni mucho menos __constructor__ como en la clásica __Programación Orientada a Objetos__, entonces como implementar un __Constructor__ en __JavaScript__? simple solo declaramos una función que a la vez toma el rol de "Clase" y __Constructor__ al mismo tiempo, sus propiedades y métodos las definimos dentro de su propio contexto es decir con __this__, vamos a tomar un ejemplo de esta manera:

{% highlight javascript %}

function Pokemon(nombre, tipo) {
  this.nombre = nombre;
  this.tipo = tipo;
}

var charmander = new Pokemon('charmander', 'fuego');

charmander.nombre // "charmander"
charmander.tipo // "fuego"

{% endhighlight %}

La palabra reservada __new__ si es reconocida en la clásica __Programación Orientada a Objetos__ y significa crear un instancia de la "Clase" en este caso __Pokemon__, por eso el nombre de la función comenzando por mayúscula solo por convención pero es una buena práctica, __new__ crea el objeto que a la final es como definir un __Objeto Literal__ de esta manera:

{% highlight javascript %}

var Pokemon = {
  nombre: 'charmander',
  tipo: 'fuego'
};

Pokemon.nombre // "charmander"
Pokemon.tipo // "fuego"

{% endhighlight %}

como hemos visto es el mismo objeto, las funciones son también objetos la diferencia es que las funciones tiene una propiedad interna especificada como __[[Call]]__ que hace que se ejecute.

La propiedad interna __[[Prototype]]__ en las funciones la encontramos como __prototype__ de esta manera entramos a la __Cadena de Prototipos__ y así compartir funcionalidad. Comprobamos si el __Constructor__ es __Pokemon__:

{% highlight javascript %}

// accedemos al prototipo de Pokemon
Pokemon.prototype // Pokemon {}

// accedemos al constructor del prototipo para verificar que se llama Pokemon
Pokemon.prototype.constructor === Pokemon // true

{% endhighlight %}

## Herencia Basada en Prototipos
Genial, ahora vamos a un ejemplo compartiendo propiedades y métodos mediante la __Cadena de Prototipos__.

{% highlight javascript %}

function Pokemon(nombre, tipo) {
  this.nombre = nombre;
  this.tipo = tipo;
}

Pokemon.prototype.damePokemon = function() {
  console.log(this.nombre + ' es de tipo ' + this.tipo);
}

var charmander = new Pokemon('charmander', 'fuego');

charmander.damePokemon() // charmander es de tipo fuego

{% endhighlight %}

Bien, primero creamos el __Constructor__ con el nombre __Pokemon__ con dos parametros que seran el valor de cada propiedad, dentro del contexto del __Constructor__ definimos las propiedades con __this__, luego accedemos a la __Cadena de Prototipos__ de __Pokemon__ y definimos un método llamado __damePokemon__ dentro de este método ejecutamos un __console.log()__ para imprimir en la consola las dos propiedades con sus respectivos valores, entonces pasamos a crear la instancia de __Pokemon__ pasadonle dos argumentos en este proceso se crea el objeto y se comparte las propiedades y métodos mediante la propiedad interna __[[Prototype]]__ de la que hablamos en un tema anterior, despues ejecutamos el método __damePokemon()__ y muestra la salida en la consola.


## Herencia con el Object.create()
Otra forma de hacerlo es con el método __Object.create()__ desde __EcmaScript 5__ en adelante, vamos a ver como trabaja con este método:

{% highlight javascript %}

var pokemon = {};

pokemon.nombre = 'charmander';

pokemon.tipo = 'fuego';

var charmander = Object.create(pokemon);

charmander.damePokemon = function() {
  console.log(this.nombre + ' es de tipo ' + this.tipo);
}

charmander.damePokemon() // charmander es de tipo fuego

{% endhighlight %}

__Object.create()__ crea un nuevo objeto en el prototipo del objeto __pokemon__ asi agregamos el método __damePokemon()__ a la __Cadena de Prototipos__.

## SuperClases
Las __SuperClases__ en la __Programación Orientada a Objetos__ son clases padres que son invocadas desde clases hijas, es decir las __Clases Hijas__ o __SubClases__ tienen una clase padre, vamos a desmitificar esto con un simple ejemplo:

{% highlight javascript %}

function Pokemon(nombre, tipo) {
  this.nombre = nombre;
  this.tipo = tipo;
}

Pokemon.prototype.damePokemon = function() {
  console.log(this.nombre + ' es de tipo ' + this.tipo);
}

function Pokedex(nombre, tipo) {
  Pokemon.call(this, nombre, tipo);
}

Pokedex.prototype = Object.create(Pokemon.prototype)

var charmander = new Pokedex('charmander', 'fuego')

charmander.damePokemon() // charmander es de tipo fuego

{% endhighlight %}

De acuerdo al ejemplo, la __Clase Padre__ o __SuperClase__ es __Pokemon__, esta clase padre tiene sus propiedades y un método en la __Cadena de Prototipos__, así nosotros queremos invocarla desde una __SubClase__ o __Clase Hija__ llamada __Pokedex__ donde le pasamos dos parametros, para llamar la __Clase Padre__ usamos el método __call()__ pasando el contexto actual y los dos parametros, luego vinculamos el __Prototipo__ de la __Clase Hija__ al de la __Clase Padre__ con el método __Object.create()__ especificamente esta declaración: __Pokedex.prototype = Object.create(Pokemon.prototype)__. Para terminar creamos la instancia y le pasamos los argumentos a la __Clase Hija__ llamada __Pokedex__ luego llamamos el método __damePokemon()__ y se ejecuta la salida, vemos como podemos vincular __Prototipos__ de __Clases Hijas__ a una __Clase Padre__.

> Una Clase Padre puede tener muchas Clases Hijas, y una Clase Hija solo puede tener una Clase Padre.

# Conclusión
Los __Prototipos__ son un parte fundamental en __JavaScript__ como hemos visto en este post que todo es un Objeto y todos los Objetos tienen su __Prototipo__ por defecto, aprovechamos esta gran caracteristica del lenguaje para usar la __Herencia Basada en Prototipos__. Es muy recomendado usar __Prototipos__ para mejor rendimiento, arquitectura, lectura y buenas practicas en el código.

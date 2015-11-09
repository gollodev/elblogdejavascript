---
layout: post
title: this y extendiendo contextos con call, apply y bind
categories: serie js
permalink: serie/js/this-y-contextos
---

En este post voy a desmitificar __this__, en __JavaScript__ esto es algo confuso ya que no trabaja igual a otros lenguajes de programación, también voy a describir como extender contextos de __this__ con los métodos __call()__, __apply()__ y __bind()__.

# this
__this__ simplemente es un variable global tal cual como __window__ (en el navegador), dentro de un ámbito se refiere al objeto actual.

{% highlight javascript %}

this === window // true

{% endhighlight %}

como vemos __this__ es igual a un objeto global como __window__, esto cambia en modo estricto.

{% highlight javascript %}

function func() {
 	return this;
}

func() // regresa el objeto global

function func2() {
	'use strict'
 	return this;	
}

func2() // undefined

{% endhighlight %}

en el modo estricto (__use strict__) retorna __undefined__ porque this no tiene ninguna valor definido, sin el modo estricto no pasa nada y se ejecuta, __this__ nos ayuda a referenciar objetos globales a una función/objeto o en su misma función/objeto como sitio de llamada, de esta manera:

{% highlight javascript %}

var pokemon = {
	nombre: 'charmander',
	tipo: 'fuego',
	color: 'rojo',
	amistad: 70,
	pasos: 5120,
    getPokemon: function() {
      console.log(this.nombre + ' es de tipo:' + ' ' + this.tipo);
    }
};

pokemon.getPokemon() // charmander es de tipo: fuego

{% endhighlight %}

puede tener el mismo comportamiento si definimos la función fuera del objeto

{% highlight javascript %}

function getPokemonFn() {
    console.log(this.nombre + ' es de tipo:' + ' ' + this.tipo);
}

var pokemon = {
	nombre: 'charmander',
	tipo: 'fuego',
	color: 'rojo',
	amistad: 70,
	pasos: 5120,
    getPokemon: getPokemonFn
};

pokemon.getPokemon() // charmander es de tipo: fuego

{% endhighlight %}

como vemos el valor de __this__ se mantiene mientras consiga su enlace con un objeto, otro buen uso de __this__ es con los constructores.

{% highlight javascript %}

function func() {
  this.nombre = 'Jose'
}

var getName = new func();
getName.nombre // "Jose"

{% endhighlight %}

cuando creamos el objeto el __this__ se le asigna una propiedad con su valor respectivamente.

# call y apply
Los métodos __call()__ y __apply()__ de __Function.prototype__ usan __this__ para enlazar objetos a funciones de una manera muy explícita, __call()__ toma dos parámetros: el primero es el objeto actual que se la va a pasar a la función y el segundo parámetro son argumentos para el objeto, __apply()__ es igual la diferencia es que el primer parámetro se le pasa un vector en vez de un objeto, veamos como trabajan estos métodos en este ejemplo:

{% highlight javascript %}

function getPokemon() {
    console.log(this.nombre + ' es de tipo:' + ' ' + this.tipo);
}

var pokemon = {
	nombre: 'charmander',
	tipo: 'fuego',
	color: 'rojo',
	amistad: 70,
	pasos: 5120
};

getPokemon.call(pokemon); // charmander es de tipo: fuego

{% endhighlight %}

## call para Super-clase
Podemos aprovechar __call()__ para encadenar constructores para un objeto, de esta manera por ejemplo:

{% highlight javascript %}

function GetPokemon(nombre, tipo) {
    console.log(nombre + ' es de tipo:' + ' ' + tipo);
}

function Pokemon(nombre, tipo) {
 GetPokemon.call(this, nombre, tipo);
}

var pokemon = new Pokemon('charmander', 'fuego'); // charmander es de tipo: fuego

{% endhighlight %}

## Vectores como Objetos
Un buen truco es usar el método __call()__ pasandole el objeto __arguments__ como objeto actual y junto con __Array.prototype.slice__, accedemos a estos argumentos como un vector. Veamos este simple ejemplo:

{% highlight javascript %}

function getPokemon() {
 var args = Array.prototype.slice.call(arguments);
 console.log(args[0] +' es de tipo ' + args[1]);
}

getPokemon('charmander', 'fuego') // charmander es de tipo fuego

{% endhighlight %}

# bind
__bind()__ se crea a partir de __EcmaScript5__, este método crea una nueva función explícitamente cuando es invocada, con el primer parámetro le pasamos el contexto y el segundo los argumentos que se le van a pasar a la función enlazada (el segundo parámetro de __bind()__ es opcional), veamos un ejemplo de un tipico problema cuando perdemos el contexto.

{% highlight javascript %}

var pokemon = {
	nombre: 'charmander',
	tipo: 'fuego',
	color: 'rojo',
	amistad: 70,
	pasos: 5120,
    getPokemon: function() {      
      setInterval(function() {
        console.log(this.nombre + ' es de tipo:' + ' ' + this.tipo);
      })      
    }
};

pokemon.getPokemon() // undefined es de tipo: undefined

{% endhighlight %}

en este caso perdemos el contexto del objeto actual por la función anidada (__setInterval()__)en __getPokemon()__ entonces no puede encontrar el contexto que en este caso es el objeto __pokemon__, muchos desarrolladores de __JavaScript__ solucionan este problema de esta manera:

{% highlight javascript %}

var pokemon = {
	nombre: 'charmander',
	tipo: 'fuego',
	color: 'rojo',
	amistad: 70,
	pasos: 5120,
    getPokemon: function() {
      var self = this;      
      setInterval(function() {
        console.log(self.nombre + ' es de tipo:' + ' ' + self.tipo);
      })      
    }
};

pokemon.getPokemon() // charmander es de tipo: fuego

{% endhighlight %}

mantenemos el objeto actual en el método __getPokemon()__ con la variable __self__, de esta manera resolvemos el problema del contexto aunque con __bind()__ podemos enlazar el contexto de la función de una manera mucha más explícita.  

{% highlight javascript %}

var pokemon = {
	nombre: 'charmander',
	tipo: 'fuego',
	color: 'rojo',
	amistad: 70,
	pasos: 5120,
    getPokemon: function() {            
      setInterval(function() {
        console.log(this.nombre + ' es de tipo:' + ' ' + this.tipo);
      }.bind(this));      
    }
};

pokemon.getPokemon() // charmander es de tipo: fuego

{% endhighlight %}

mucho mejor que enlazar el contexto con una variable, pero que hace __bind()__? __bind()__ simplemente crea una nueva función cuando es invocada, le pasamos __this__ como primer parámetro para enlazar el contexto del objeto actual en este caso el objeto __pokemon__ y entonces cuando es ejecutada la función (el callback __setInterval__) se enlaza con el objeto __pokemon__.


# Conclusión
Como describo en este post __this__ es simplemente una variable global y definiendola desde una función u objeto toma su contexto actual, __call()__ y __apply()__ trabajan para definir contextos a funciones explícitamente similar __bind()__ la diferencia es que __bind()__ crea una nueva función para pasarle el contexto actual, espero que con este post puedas evitar estos tópicos problemas de __JavaScript__.   
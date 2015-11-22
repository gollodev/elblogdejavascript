---
layout: post
title: Ámbitos de Variables y Clausuras
categories: serie js
permalink: serie/js/ambitos-y-clausuras
---

> Si no entiendes como trabajan los contextos en JavaScript puedes ir a [post](http://elblogdejavascript.com/serie/js/this-y-contextos) antes de comenzar este post ya que es requerido para un buen entendimiento de los __ámbitos__.

Ya hemos visto en posts pasados como se declaran variables, como accedemos y reasignamos un nuevo valor, pero las variables van más alla de solo almacenar valores de estado en el programa. Bien, ahora que son los __Ámbitos__ (scopes en ingles)? un __ámbito__ es el acceso a una variable desde la parte del programa donde fue declarada, se puede decir que es un conjunto de reglas entre el compilador y el motor de __JavaScript__ para que los __ámbitos__ de las variables puedan ser encontradas y ejecutadas, existen diferentes reglas para manipular los __ámbitos__ que estare describiendo en este post.

# Ámbito Léxico
El __ámbito léxico__ es cuando se declara una variable en funciones anidadas, a esto también se les llaman __Clausuras__ __(Closures en ingles)__. Esto significa que una función interna consigue el __ámbito__ de una función padre, veamos este ejemplo:

{% highlight javascript %}

var nombre = 'Jose';

function padre() {
   var frase = 'Mi nombre es: ';

   function interna() {
     alert(frase + nombre)
   }
   
   interna()
}

padre() // se ejecuta el alert "Mi nombre es: Jose"

{% endhighlight %}

Bien, el ejemplo es muy sencillo, primero se declara una variable de __ámbito global__, estas variables globales se declaran por encima de cualquier función u objeto para acceder a desde cualquier ámbito, luego declaramos un función __padre()__ y dentro de esta función declaramos una función __interna()__, esta función __interna__ toma dos variables para el __alert()__ que son __frase__ y __nombre__, la función __interna()__ la llamamos en el __ámbito__ de la función __padre()__ luego de que esta es llamada se muestra el __alert()__ con "Mi nombre es: Jose", realmente que paso aca? la función __interna()__ tiene acceso al __ámbito global__ y toma la variable global __nombre__ y también tiene acceso al __ámbito__ de la función __padre()__ y toma la variable __frase__ y asi el __alert()__ consigue estas dos variables para mostrarlas en la ventana.

> Las variables globales también se pueden declarar con el objeto window, __window.nombre = 'Jose';__ es igual que __var nombre = 'Jose';__.

# Ámbitos Desde funciones
Como vimos en el ejemplo anterior, el __ámbito__ de variables son ciertas reglas aunque muy dinamicas y flexibles en __JavaScript__, vamos a ver este simple ejemplo:

{% highlight javascript %}

function edadFn() {
  var x = 23;
  if(x > 18) {
    var edad = x;
  }
  alert(edad);
}

edadFn()

{% endhighlight %}

todas la variables declaradas dentro de una función existen hasta el final de esta, esto significa que las variables toman el __ámbito desde funciones__, podemos imaginar que un __ámbito__ es como una burbuja en cada función, estas variables declaradas dentro de una función se les llama __variables locales__ entonces en el ejemplo de arriba las varibles __x__ y __edad__ son __variables locales__ de la función __edadFn()__.

# Variables Elevadas
Una __variable elevada__ es cuando se declara en un __ámbito desde funciónes__ como en el ejemplo anterior pero la declaramos sin valor y en el inicio de la función, estas variables le vamos asignando o reasignando valor en todo el __ámbito__ de la función donde es __elevada__, veamos un ejemplo:

{% highlight javascript %}

function edadFn() {
  var edad;  // variable elevada
  if(true) {
    edad = 23;
  }
  alert(edad);
}

edadFn()

{% endhighlight %}

simple, declaramos la variable __edad__ al principio de la función luego le asignamos un valor y la mostramos con un __alert()__ y asi podriamos reasignarle nuevos valores dependiendo de nuestras necesidades en el programa.

# Clausuras
una __Clausura__ (Closure en ingles) es cuando una función es capaz de acceder a su __ámbito léxico__ incluso hasta cuando su ejecución pasa fuera de su __ámbito léxico__. El primer ejemplo __Ámbito Léxico__ se puede decir que es una __Clausura__ pero que tal si queremos un nuevo __ámbito__ de variable para la función interna, una función no puede bloquear el __ámbito__ de sus variables y para esto usamos la función __IIFE (Immediately Invoked Function Expression)__ pronunciado "iffy" y declarada de este forma:

{% highlight javascript %}

(function(){
	// ambito cerrado
})();

{% endhighlight %}

la función IIFE bloquea el ámbito y crea uno propio e inicializa inmediatamente, ya que vimos un ejemplo de una __Clausura__ en el primer tema, voy a tratar otro buen uso para una __Clausura__ y es emular métodos privados ya que __JavaScript__ no cuenta con una declaración explícita para métodos o variables privadas, aqui un ejemplo para entender como trabaja esto:

{% highlight javascript %}

var Module = (function() {
  var pokemon = 'charmander',
  	  tipo    = 'fuego';
  function pokedex() {
  	alert(pokemon +':'+' '+ tipo)
  }

  return {
    getPokemon: function() {
    	return pokedex();
    }
  }   
})();

Module.getPokemon() // se ejecuta el alert "charmander: fuego"
Module.pokedex() // Uncaught TypeError: Module.pokedex is not a function(…)
{% endhighlight %}

en este básico ejemplo usamos el famoso __Patrón de Módulo (Module Pattern en ingles)__ este Patrón consta de una variable que su valor es una función IIFE para darle su propio entorno en este módulo se encuentran dos variables y un método privado, dentro del __return__ esta un método público que toma el método privado __pokedex()__ y lo ejecuta y este método accede al __ámbito léxico__ de las variables, en otros lenguajes esto se conoce como una __Interface__, esto significa que no podemos acceder a la variable __pokemon__, __tipo__ ni al método __pokedex()__ como vemos en el ejemplo dispara un __Uncaught TypeError__ si intentamos acceder a un método privado e igual para las variables.

# Conclusión
Genial, ya con esto estaras consciente de lo que son los __Ámbitos de Variables y Clausuras__, en otros lenguajes de programación los __ámbitos__ no son muy dinámicos y flexibles como en __JavaScript__ y esto lo hace un poco confuso de entender, en este post vimos desde los tipos de __ámbitos__ hasta como usar el __Patrón de Módulo__ con la función __IIFE__ y proteger nuestro __ámbito__ para acceder a métodos y variables. 








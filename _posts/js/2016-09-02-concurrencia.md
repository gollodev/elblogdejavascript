---
layout: post
title: Concurrencia
categories: serie js
permalink: serie/js/concurrencia
---

Antes de comenzar, __Que es Concurrencia?__

En un contexto general de programación, concurrencia es la habilidad para ejecutar dos o más procesos computacionales simultáneamente mientras se comparten recursos. Estos procesos son llamados "hilos", los hilos pueden compartir un simple procedimiento o pueden distribuirlos a través de una red y sincronizarlo más tarde. La comunicación entre procesos paralelos son usualmente explícitos, pasandolos a través de mensajes o compartiendo variables.

Aunque JavaScript no tiene una verdadera Concurrencia, es posible emular sus efectos a través del uso de funciones como: setInterval(), setTimeout(), o la versión asincrona de XMLHttpRequest().
JavaScript viene por default con un modelo de Concurrencia basado en "loop de eventos", algo distinto a otros modelos de Concurrencia, este __loop de eventos__ puede evaluar los programas continuamente para ver los mensajes de eventos entrantes al proceso, un simple hilo significa que hay un solo __loop de eventos__ por tiempo de ejecución. el __Loop de Eventos__ de __JavaScript__ esta profundamente relacionado con dos conceptos: __Ejecutar hasta completar (run-to-completion)__ y __Sin Bloqueo (I/O)__.

# Ejecutar hasta completar (run-to-completation)
El __Loop de Eventos__ es diseñado como entorno para __Ejecutar hasta completar__. Es decir que una vez que JavaScript inicia a ejecutar una tarea, esta no puede ser interrumpida hasta que se complete. Sin __Ejecutar hasta completar__, no podriamos saber sobre el estado del objeto porque no podemos en ese caso acceder desde afuera de el __Loop de Eventos__.

Vamos a ver un simple ejemplo para entender este concepto:

{% highlight javascript %}

	(function () {
	  'use strict';
	  
	  console.log('Estado fuera del Loop de Eventos!');
	  
	  setTimeout(function () {
	    console.log('Estado dentro del Loop de Eventos!');
	  }, 500)
	  
	  console.log('Estado terminado');
	})();
	
{% endhighlight %}

En el ejemplo vemos como se imprime el primer log, luego el último log fuera del __Loop de Eventos__, y por último el log dentro del __Loop de Eventos__ con la función __setTimeout()__, esta función toma como parámetro otra función (callback) y los milisegundos (el tiempo para que se dispare el callback), observamos que el log __"Estado terminado"__ se imprime primero que el log que esta dentro de __setTimeout()__, y esto es porque JavaScript va agregando a una cola de mensajes para procesar cada tarea, entonces al llamar __setTimeout()__ va agregando un mensaje a la cola dependiendo el tiempo (segundo parámetro). En este caso tenemos dos mensajes en la cola __("Estado fuera del Loop de Eventos!")__ y __("Estado terminado")__, __setTimeout()__ espera a que los otros mensajes sean procesados, si no existe ningún mensaje en la cola el mensaje se procesa al instante.

# Sin Bloqueo (I/O)
Hablamos del __Sin Bloqueo (I/O)__ como una arquitectura asincrónica, lo contrario a __Bloqueo (Ejecutar hasta completar)__, en vez de esperar hasta que se complete la tarea, esta se ejecuta inmediatamente, es decir que cualquier tarea de corto o largo plazo para finalizar, por ejemplo como puede ser procesar un archivo, comunicarse a una API via HTTP o hacer consultas a una Base de Datos, son peticiones que se retornan cuando los resultados estan completados, y estos resultados se retornan via un __callback__.

# Callbacks
La función __callback__ puede ser invocada cuando la operación es completada y así manipular peticiones adicionales, esto es fundamental en el __Loop de Eventos__ de __Nodejs__ ya que su arquitectura es totalmente asíncrona. Node opera de forma asíncrona via el __Loop de Eventos__ y __Callbacks__, es decir el __Loop de Eventos__ en Node no es nada más que llamar e invocar eventos en el tiempo adecuado. En Node, una función __Callback__ es el controlador de eventos.

Vamos a ver un ejemplo con __Nodejs__ para aclarar el concepto:

{% highlight javascript %}

'use strict';

const http = require('http');
const fs = require('fs');

http.createServer(function (req, res) {
	fs.readFile('hello.txt', 'utf-8', function (err, file) {
		res.writeHead(200, { 'Content-Type': 'text/plain' });
		if (err) {	
			// si hay error		
			res.write('File not found');
		} else {
			// si no hay error, escribe el texto del archivo al navegador
			res.write(file);
			res.end();
		}
	})
}).listen('3000', function () {
	// el servidor escucha en el puerto local 3000
	console.log('Server Rocks in port 3000! imi');
});

{% endhighlight %}

Voy a explicarlo por partes: primero importamos dos módulos de __Nodejs__ (__http__ y __fs__), con el módulo __http__ creamos un servidor con el método __createServer()__, este método toma como argumento un __callback__, este __callback__ tiene dos argumentos que son __req__ (petición) y __res__ (respuesta), dentro de la declaración de este __callback__ usamos el módulo __fs__ y su método __readFile__, este toma tres argumentos: el archivo que vamos a procesar (en este caso __hello.txt__), el segundo argumento es la __codificación___, y por último el __callback__ con dos argumentos: el error y la lectura del archivo. Dentro de la declaración del __callback__ de __readFile()__, configuramos el head del http y le decimos que el tipo de contenido que vamos a servir como respuesta es de texto plano (text/plain), luego verificamos si hay un error en el proceso, si lo hay respondemos __File not found__, de lo contrario el archivo se proceso exitosamente y servimos el archivo al cliente. para iniciar el servidor el módulo __http__ necesita el método __listen()__ tomando como argumentos el puerto (en este caso el puerto 3000) y un __callback__ que dentro de su declaración solo mostramos que el servidor esta corriendo.
Como hemos visto en este ejemplo todos los métodos toman su respectivo __callback__ para ejecutar la tarea inmediatamente, pero que pasa si tenemos un programa mucho más complejo, que despues de una tarea queremos ejecutar otra y otra seguidamente, seria algo como esto:

{% highlight javascript %}

fs.readdir(source, function (err, files) {
  if (err) {
    console.log('Error finding files: ' + err)
  } else {
    files.forEach(function (filename, fileIndex) {
      console.log(filename)
      gm(source + filename).size(function (err, values) {
        if (err) {
          console.log('Error identifying file size: ' + err)
        } else {
          console.log(filename + ' : ' + values)
          aspect = (values.width / values.height)
          widths.forEach(function (width, widthIndex) {
            height = Math.round(width / aspect)
            console.log('resizing ' + filename + 'to ' + height + 'x' + height)
            this.resize(width, height).write(dest + 'w' + width + '_' + filename, function(err) {
              if (err) console.log('Error writing file: ' + err)
            })
          }.bind(this))
        }
      })
    })
  }
})

{% endhighlight %}   

Para nada agradable, termina siendo mucho código engorroso, dificil de tratar y leer cuando se trata de trabajar con un equipo completo, esto conocido como el __Callback Hell__. Para evitar el __Callback Hell__ hacemos el uso de __Promesas__, del cual sera el siguiente tema a tratar.

# Promesas
El objetivo de las __Promesas__ es simplificar la asincronia en __JavaScript__, manejando ordenes de ejecuciones a traves de una serie de pasos y manejar cualquier error asincrono que se dispare. Con los Promises
evitamos el uso de los __callbacks__ y así organizar el codigo asíncrono, mucho más facil de leer y mantener.

Una __Promesa__ simplemente es un objeto que retorna un valor asíncrono, es decir: el valor de una operación asíncrona, tal como el resultado de una peticion HTTP o la lectura de un archivo en disco. Cuando una función asíncrona es llamada
puede inmediatamente retornar una objeto de __Promesas__, usando ese objeto, puedes invocar callbacks que puedan ejecutar cuando la operación es exitosa o cuando ocurre un error.

vamos a tomar el ejemplo anterior de los __Callbacks__:

{% highlight javascript %}

'use strict';

const http = require('http');
const fs = require('fs');

http.createServer(function (req, res) {
	fs.readFile('hello.txt', 'utf-8', function (err, file) {
		res.writeHead(200, { 'Content-Type': 'text/plain' });
		if (err) {	
			// si hay error		
			res.write('File not found');
		} else {
			// si no hay error, escribe el texto del archivo al navegador
			res.write(file);
			res.end();
		}
	})
}).listen('3000', function () {
	// el servidor escucha en el puerto local 3000
	console.log('Server Rocks in port 3000! imi');
});

{% endhighlight %}

Ya sabemos lo que hace este programa, vamos a usarlo de nuevo pero en vez de __Callbacks__ vamos a usar __Promesas__ para manejar los procesos asíncronos.

{% highlight javascript %}

'use strict';

const http = require('http');
const fs = require('fs');

function readFile (filename) {
    return new Promise(function (resolve, reject) {
        fs.readFile(filename, 'utf-8', function (err, file) {            
            if (err) {	
                // si hay error		
                reject(err);                
            } else {
                // si no hay error, escribe el texto del archivo al navegador
                resolve(file);                
            }
        })
    })
}

http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    readFile('hello.txt').then(function (file) {
        res.write(file);        
        res.end();
    }).catch(function (err) {
        res.write('File not found');
    })
	
}).listen('3000', function () {
	// el servidor escucha en el puerto local 3000
	console.log('Server Rocks in port 3000! imi');
});

{% endhighlight %}

Vamos por pasos, en __Javascript__ para retornar una __Promesa__ necesitamos un contructor/función global llamado __Promise__, este nos facilita toda la funcionalidad para las __Promesas__. Ahora fijandonos en el ejemplo, en la
declaración de la función __readFile__ creamos y retornamos una nueva __Promesa__, cuando el __Promise__ lo usamos como constructor le pasamos un callback, este callback toma dos argumentos que son: el resolvedor (resolve) 
y el rechazador (reject) de las cuales ambos son funciones que se usan manipular la __Promesa__ cuando se ejecute, así como suena claro: el resolvedor (resolve) es llamado cuando la __Promesa__ se cumple exitosamente,
en este caso de nuestro ejemplo, el resolvedor se llama cuando se carga el archivo y el rechazador (reject) es llamado si el archivo no puede ser cargado. Entonces para tomar el valor que retorna la __Promesa__
usamos el método __then__ para recibir el valor, en el caso de nuestro ejemplo recibe el archivo (file) y lo mandamos al cliente como respuesta.

## Estados
Los estados de una __Promesa__ nos ayuda a manipular la operación representada y almacenada por una __Promesa__, los estados nos indica cuando inicio, si esta en progreso, si ya se completo o se ha parado y no puede completarse,
estas condiciones son representadas por tres comunes estados:

- __Pendiente__: La operación no ha podido iniciar o esta en progreso.
- __Completada__: La operación esta completada.
- __Rechazada__: La operación no puede ser completada.

Nos referimos a los estados __Completada__ y __Rechazada__ como exitosamente y error, el estado __Pendiente__ puede ser diferente, porque una __Promesa__ puede ser __Completada__ con un error, y cuando una operación no puede
ser __Completada__ no regresa ningun error, en resumen: __Completada__ es cuando la __Promesa__ es completada y retorna el valor exitosamente y __Rechazada__ cuando la __Promesa__ retorna un error.

## Encadenamiento
Como hemos visto, los métodos __then()__ y __catch()__ retornan el valor de la __Promesa__ y así podemos encadenar los métodos facilmente, en caso de encadenar otro método no es la misma __Promesa__ que regresa, es creada
y retornada una nueva __Promesa__. Vamos a retornar una nueva Promesa!

{% highlight javascript %}

'use strict';

const http = require('http');
const fs = require('fs');

function readFile (filename) {
    return new Promise(function (resolve, reject) {
        fs.readFile(filename, 'utf-8', function (err, file) {            
            if (err) {	
                // si hay error		
                reject(err);                
            } else {
                // si no hay error, escribe el texto del archivo al navegador
                resolve(file);                
            }
        })
    })
}

http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    readFile('hello.txt')
    .then(function (file) {
        return 'This text come from the hello.txt: ' + file;
    })
    .then(function (text) {
        return text.toUpperCase();
    })
    .then(function (upperText) {
        res.write(upperText);        
        res.end();
    }).catch(function (err) {
        res.write('File not found');
    })
	
}).listen('3000', function () {
	// el servidor escucha en el puerto local 3000
	console.log('Server Rocks in port 3000! imi');
});

{% endhighlight %}

Bien, usando el mismo ejemplo pero en este hemos encadenado dos métodos más, cada __then()__ regresa una nueva __Promesa__ y toman el valor de la __Promesa__ retornada en el anterior, es decir en cada paso enviamos y retornamos el
valor al próximo paso, es una secuencia de pasos donde podemos sacarle provecho e ir transformando ese valor y enviarlo, funciona así como un "pipeline". Entonces enfocándonos en el ejemplo: el primer paso tomamos el archivo
(la Promesa Completada) concatenamos una cadena y la retornamos al siguiente paso, el segundo paso toma la nueva __Promesa__ retornada (la cadena concatenada) transformamos la cadena a mayúsculas y la retornamos al siguiente paso,
el tercer y último paso tomamos la nueva __Promesa__ retornada del paso anterior, aqui enviamos el valor (la cadena concatenada y en mayúsculas) al navegador y vemos el resultado final __THIS TEXT COME FROM THE HELLO.TXT: HELLO WORLD__.
Así podemos ir encadenando más métodos si es necesario, no existe ningún límite para encadenar métodos, en el caso de este ejemplo fue para mantenerlo simple.

## Capturando Errores
En operaciones síncronas manejamos los errores con el típico __try catch__, igual pasa en las __Promesas__, pero en este caso son para manejar operaciones asincrónicas. El error retorna si el estado se cumple pero es __Rechazado__,
es decir el método __reject__ dentro del callback de un constructor __Promise__ (o método estático), y el método __catch__ nos permite capturar el error retornado por el __reject__.
Vamos a visualizar este concepto en un ejemplo muy simple:

{% highlight javascript %}

'use strict';

return Promise.reject(Error('My Promise Error')).then(function () {
    console.log('not run');
})
.then(function () {
    console.log('not run');
})
.catch(function (err) {
    // cuando la promesa se cumple con algun Error
    console.log(err);
})

{% endhighlight %}

> Se puede acceder a los métodos __resolve__ y __reject__ mediante el callback del constructor __Promise__ o de forma estática.

En este ejemplo retornamos el __Promise__ con el método __reject__, con un error dentro para propuesta de este ejemplo. Encadenamos dos métodos __then__ y estos nunca seran ejecutados, de lo contrario son ejecutados cuando
la __Promesa__ es Completada exitosamente, ahora en este caso la __Promesa__ es __Rechazada__ y pasa al método donde manipulamos el error retornado.

## Ejecución Paralela
Puede que necesitemos ejecutar multiples tareas asincrónas, las __Promesas__ nos permite correr multiples tareas en paralelo, es decir las tareas se ejecutan simultáneamente hasta que se cumpla la __Promesa__ y envia a las
secuencias (__then__ y __catch__) tal y como lo venimos haciendo. Para ejecutar tareas en paralelo usamos el método __all__ desde la API de __Promise__, este método toma como argumento un iterable (un array de valores)
y retorna una __Promesa__ cuando todas las promesas del iterable hayan sido cumplidas exitosamente o de lo contrario si alguna __Promesa__ del iterable falla sera rechazada. Vamos a visualizar el concepto en dos ejemplos de código, 
uno sera cuando es cumplida todas las promesas del iterable y otro cuando sea rechazada.

El primer ejemplo sera cuando todas las __Promesas__ son cumplidas.

{% highlight javascript %}

'use strict';

const http = require('axios');
const apiUrl = 'https://pokeapi.co/api/v2/pokemon/';

function getBulbasaur () {
    return new Promise(function(resolve, reject) {
        http.get(apiUrl + 'bulbasaur')
            .then(function (data) {
                resolve(data);
            })           
            .catch(function (err) {
                reject(err);
            })
    });
}

function getSquirtle () {
    return new Promise(function(resolve, reject) {
        http.get(apiUrl + 'squirtle')
            .then(function (data) {
                resolve(data);
            })           
            .catch(function (err) {
                reject(err);
            })
    });
}

function getCharmander () {
    return new Promise(function(resolve, reject) {
        http.get(apiUrl + 'charmander')
            .then(function (data) {
                resolve(data);
            })           
            .catch(function (err) {
                reject(err);
            })
    });
}

function getButterfree () {
    return new Promise(function(resolve, reject) {
        http.get(apiUrl + 'butterfree')
            .then(function (data) {
                resolve(data);
            })           
            .catch(function (err) {
                reject(err);
            })
    });
}

const iterable = [getBulbasaur(), getSquirtle(), getCharmander(), getButterfree()];

Promise.all(iterable)
       .then(function (results) {
           console.log(results);
       })
       .catch(function (err) {
           console.log(err);
       })

{% endhighlight %}

En este ejemplo usamos __axios__ para las peticiones __http__, tenemos cuatro funciones que regresan una __Promesa__. Cada función la agregamos a un array que le llamamos en este caso __iterable__, entonces el método __all__
toma el __iterable__ (array de promesas) y si todas las __Promesas__ son cumplidas exitosamente retorna los resultados a __then__, de lo contrario la __Promesa__ sera rechazada y enviada a __catch__.

Vamos a ver el otro ejemplo pero con una promesa rechazada pero mucho más simple. 

{% highlight javascript %}

'use strict';

const promise1 = Promise.resolve('Promise Resolve 1');
const promise2 = Promise.resolve('Promise Resolve 2');
const promise3 = Promise.resolve('Promise Resolve 3');
const promiseReject = Promise.reject('Promise Reject');

const iterable = [promise1, promise2, promise3, promiseReject];

Promise.all(iterable)
       .then(function (results) {
           console.log(results);
       })
       .catch(function (err) {
           console.log(err);
       })

{% endhighlight %}

Esta muy claro! si alguna de las promesas en el __iterable__ es rechazada y pasa al __catch__.  

# Generadores
Normalmente cuando una función es invocada y retorna un valor, la declaración del __return__ activa un nuevo contexto de ejecución y descarta el contexto anterior porque lo hemos retornado, es decir se envia al 
__call stack__ y se asigna a la memoria. Los Generadores nos permiten evitar la asignación en memoria innecesariamente y así evitamos invocar el "recolector de basura" frecuentemente, a parte que también mantienen su propio estado.
Los Generadores tienen su propia sintaxis, se declaran mediante una función (como siempre lo hemos hecho) y un asterisco seguido de la palabra clave __function__ y antes del nombre de la función y se ve así __function*__,
el cuerpo de la función se declara igual que una función normal pero cuando es invocada no se ejecuta inmediatamente y esto es porque devuelve un objeto __iterador__, no es necesario instanciar ya que los generadores mantienen su 
propio estado y ya estan instanciados, vamos a ver como se ve un __Generador__.

{% highlight javascript %}

'use strict';

function* generator() {
    return 'Hello Generator!';
}

const gen = generator().next();

console.log(gen);

// { value: 'Hello Generator!', done: true }

{% endhighlight %}

Declaramos el __Generador__ como he comentado al comienzo, a diferencia de una función normal cambia dos cosas que son: el __*__ después de la palabra reservada __function__ e invocamos la función y encadenamos el método __next__,
el método __next__ simplemente recorre el objeto secuencialmente y devuelve un objeto con dos propiedades __value__ y __done__.

## Retornando valores con yield
Hemos visto en el ejemplo anterior que retornamos el valor del __Generador__ con __return__, y trabaja sin ningún problema, pero el caso común para retornar valores de un __Generador__ es usando __yield__. La palabra 
reservada __yield__ es el __return__ pero basado en __Generadores__, la diferencia es que __yield__ puede controlar el invocador del __Generador__, cuando retornamos con __yield__ la expresión puede ser pausada hasta que invoquemos
el método __next__ para pasar a la siguiente ejecución. El __yield__ igual que __return__ retorna un objeto iterable con dos propiedades __value__ y __done__, la diferencia acá es que __yield__ retorna __done__ como falso porque
el __yield__ vuelve atrás y evalúa el __Generador__ y hasta que no se retornen todos los valores __done__ sera falso, de lo contrario __done__ pasa a verdadero. 
Vamos a tomar un ejemplo muy simple retornando valores con __yield__.

{% highlight javascript %}

'use strict';

function* generator () {
    yield 'Hello';
    yield 'World';
    yield 'Generator';
}

const gen = generator();

console.log(gen.next()); // { value: 'Hello', done: false }
console.log(gen.next()); // { value: 'World', done: false }
console.log(gen.next()); // { value: 'Generator', done: false }
console.log(gen.next()); // { value: undefined, done: true }

{% endhighlight %}

Bien! primero declaramos el __Generador__ como ya sabemos, usamos la palabra reservada __yield__ para retorna los valores, luego invocamos el __Generador__ usando el método __next__ para pasar de secuencia, fijandonos en los valores
que retornamos con __yield__ (Hello, World y Generator) cada __next__ nos retorna el objeto iterable con las dos propiedades __value__ y __done__, el primer __next__ nos retorna el primer valor y el proximo nos retorna el segundo valor
y así sucesivamente en orden secuencial, el último __next__ es cuando termina el __Generador__ y nos retorna el __value__ como undefined y el __done__ como true.

# Conclusión
JavaScript no es un lenguaje totalmente Concurrente, pero estos últimos años ha cambiado para bien con nuevas características desde __ES 2015__. Realmente llevamos muchos años tratando Concurrencia con JavaScript, desde aquellos
comienzos de las peticiones http con __Ajax__. Las Promesas ya llevan un tiempo también, antes era posible con librerías externas ahora ya es nativo del lenguaje, los __Generadores__ son otra nueva funcionalidad al lenguaje que cambia
la forma en la que pensamos sobre Concurrencia en JavaScript.

<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a función es una variable que almacena la dirección de memoria de una función, permitiendo invocarla indirectamente.

#include <stdio.h>
#include <ctype.h>

void aMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    char texto[] = "hola mundo";

    void (*ptr)(char *) = aMayusculas; // puntero a función
    ptr(texto); // invocación

    printf("%s\n", texto);
    return 0;
}



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una lambda es una función anónima que puede asignarse a variables o pasarse como parámetro.

JavaScript:

let aMayusculas = (cadena) => cadena.toUpperCase();

console.log(aMayusculas("hola mundo"));

Java:

import java.util.function.Function;

Function<String, String> aMayusculas = s -> s.toUpperCase();

System.out.println(aMayusculas.apply("hola mundo"));



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?
Es un estilo de programación basado en funciones puras, inmutabilidad y composición.

Java es multi-paradigma porque soporta:

Orientación a objetos
Programación funcional (desde Java 8)

Funciones como ciudadanos de primera clase significa que:

Se pueden asignar a variables
Pasar como argumentos
Devolver como resultado


## 4. Explica la sintaxis básica de una función lambda en Java.

(parámetros) -> expresión
(parámetros) -> { bloque }

Ejemplo:

Function<String, String> f = s -> s.toUpperCase();

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.
JavaScript:

function transformar(cadena, fn) {
    return fn(cadena);
}

let aMayusculas = s => s.toUpperCase();
console.log(transformar("hola", aMayusculas));

Java:

import java.util.function.Function;

static String transformar(String s, Function<String, String> fn) {
    return fn.apply(s);
}


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

JavaScript:

console.log(transformar("hola", s => s.split("").reverse().join("")));

Java:

System.out.println(transformar("hola",
    s -> new StringBuilder(s).reverse().toString()));



## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Una lambda puede capturar variables del entorno.

Java:

String sufijo = "!!!";

Function<String, String> f = s -> s + sufijo;

System.out.println(f.apply("hola"));

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

C: solo dirección de función
Lambda: incluye contexto (closure)
Lambda es más expresiva y segura


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.
import java.util.function.Function;

static Function<Double, Double> crearDescuento(double porcentaje) {
    return precio -> precio - (precio * porcentaje);
}

public static void main(String[] args) {
    Function<Double, Double> d10 = crearDescuento(0.10);
    Function<Double, Double> d20 = crearDescuento(0.20);

    System.out.println(d10.apply(100.0));
    System.out.println(d20.apply(100.0));
}



## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?
Es una interfaz con un único método abstracto.

Requisitos:

Solo un método abstracto
Puede tener métodos default o static
Se puede usar con lambdas


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

interface Transformador {
    String transformar(String s);
}

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

@FunctionalInterface
interface Transformador<T, R> {
    R transformar(T t);
}

// Ejemplo
Transformador<Double, Integer> redondear = d -> (int)Math.round(d);

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.


Principales:

Function<T,R>
Consumer<T>
Supplier<T>
Predicate<T>
UnaryOperator<T>
BinaryOperator<T>

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.
import java.util.*;

List<Integer> lista = Arrays.asList(1, -2, 3);

lista.forEach(n -> {
    if (n > 0) {
        System.out.println("Positivo: " + n);
    }
});

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.
PECS = Producer Extends, Consumer Super

Producer → ? extends T
Consumer → ? super T

forEach usa Consumer<? super T> porque consume elementos.

Aplicado a transformar:

Function<? super T, ? extends R>
## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.


class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log("Hola " + this.nombre);
    }
}

let p = new Persona("Ana");
let ref = p.saludar.bind(p);
ref();

Java:

class Persona {
    String nombre;
    Persona(String n) { nombre = n; }
    void saludar() { System.out.println("Hola " + nombre); }
}

Persona p = new Persona("Ana");
Runnable ref = p::saludar;
ref.run();


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
// Estático
Function<String, Integer> f1 = Integer::parseInt;

// Constructor
Supplier<ArrayList<String>> f2 = ArrayList::new;

// Instancia concreta
Persona p = new Persona("Ana");
Runnable f3 = p::saludar;

// Instancia arbitraria
Function<String, String> f4 = String::toUpperCase;



## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

import java.util.*;

class Persona {
    String nombre;
    int edad;
    Persona(String n, int e) { nombre = n; edad = e; }
}

Manual:

Collections.sort(lista, (p1, p2) -> {
    if (p1.edad != p2.edad)
        return p1.edad - p2.edad;
    return p1.nombre.compareTo(p2.nombre);
});

Con Comparator:

Collections.sort(lista,
    Comparator.comparingInt((Persona p) -> p.edad)
              .thenComparing(p -> p.nombre));
<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Abstracción:  Seleccionar los aspectos esenciales de un objeto y ocultar los irrelevantes. Permite modelar conceptos del mundo real.

Encapsulamiento:  Agrupar datos y métodos que operan sobre esos datos, controlando el acceso mediante visibilidad.

Herencia:  Crear nuevas clases a partir de otras, reutilizando y extendiendo comportamiento.

Polimorfismo:  Permitir que una misma operación tenga comportamientos distintos según el objeto que la ejecute.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Java , C, C++ , Python


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

Programación estructurada  
Paradigma basado en dividir el programa en bloques lógicos usando secuencia, selección (if, switch) e iteración (for, while). Evita el uso de goto.

Programación modular  
Evolución de la estructurada: divide el programa en módulos independientes, cada uno con responsabilidad clara. Facilita mantenimiento, reutilización y pruebas.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Java , C, C++ , Python

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Estado (atributos)

Comportamiento (métodos)

Identidad (cada objeto es único en memoria)


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

Clase: plantilla o molde que define atributos y métodos.

Objeto: entidad concreta creada a partir de una clase.

Instancia: sinónimo de objeto creado.

¿Todos los lenguajes OO usan clases?  
No. Ejemplo: JavaScript es orientado a objetos basado en prototipos, no en clases tradicionales (aunque hoy tenga sintaxis class).

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Método: función asociada a una clase/objeto.

Sobrecarga: definir varios métodos con el mismo nombre pero distinta lista de parámetros.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

public class Punto {
    int x;
    int y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

class Ejemplo {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen()); // 5.0
    }
}



## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada es el método:

java
public static void main(String[] args)
static: pertenece a la clase, no a una instancia.
Se usa en main porque el programa arranca sin haber creado objetos.

También se usa para métodos utilitarios, atributos compartidos, constantes, etc.

static + final: define constantes (ej.: public static final double PI = 3.14159;).

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Supongamos un archivo Punto.java.

Compilar:

Código
javac Punto.java
Esto genera Punto.class.

Ejecutar:

Código
java Punto
Java es compilado a bytecode, no a código máquina.

La JVM (Java Virtual Machine) ejecuta ese bytecode.

Los ficheros .class contienen ese bytecode portable.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

new: reserva memoria en el heap y devuelve una referencia al objeto.

Constructor: método especial que inicializa el objeto.

Ejemplo:

java
public class Empleado {
    String dni;
    String nombre;
    String apellidos;

    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

double calculaDistanciaAOrigen() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
}


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

double calculaDistanciaAOrigen() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
}



## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, los objetos se pasan por copia de referencia.
Si modificas atributos del Punto dentro del método, se modifican fuera.

Los tipos primitivos (int, double, etc.) se pasan por copia del valor, así que no se modifican fuera.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java
Método que devuelve una representación textual del objeto.

Existe en muchos lenguajes: Python (__str__), C# (ToString()), JavaScript (toString()).

@Override
public String toString() {
    return "(" + x + ", " + y + ")";
}



## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


Un struct es parecido porque agrupa datos, pero le falta:

Métodos asociados

Encapsulamiento

Herencia

Polimorfismo

Constructores automáticos

Control de visibilidad

Un struct solo contiene datos, no comportamiento.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

#include <math.h>

typedef struct {
    int x;
    int y;
} Punto;

double calculaDistanciaAOrigen(Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}

Aquí no existe this.
El programador debe pasar explícitamente el objeto como parámetro.

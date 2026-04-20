<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.
En C se puede simular una estructura genérica utilizando punteros void*, que permiten referenciar cualquier tipo de dato. Por ejemplo, se puede definir una lista basada en un array de punteros genéricos, donde cada posición apunta a un dato cuyo tipo concreto se desconoce en tiempo de compilación. Esto proporciona flexibilidad, aunque delega la responsabilidad del manejo de tipos en el programador.
#include <stdio.h>

typedef struct {
    void* datos[10];
    int size;
} Lista;

void insertar(Lista* l, void* elemento) {
    l->datos[l->size++] = elemento;
}

int main() {
    Lista l = {.size = 0};
    int a = 5;
    double b = 3.14;

    insertar(&l, &a);
    insertar(&l, &b);

    printf("%d\n", *(int*)l.datos[0]);
    printf("%f\n", *(double*)l.datos[1]);
}
En Java, el mismo concepto se implementa utilizando Object, ya que todas las clases heredan de él. Se puede construir una estructura que almacene elementos de cualquier tipo dentro de un array de Object. Sin embargo, al recuperar los valores será necesario realizar conversiones explícitas.  
class Lista {
    private Object[] datos = new Object[10];
    private int size = 0;

    public void insertar(Object elemento) {
        datos[size++] = elemento;
    }

    public Object get(int i) {
        return datos[i];
    }
}

public class Main {
    public static void main(String[] args) {
        Lista l = new Lista();
        l.insertar(5);
        l.insertar("hola");

        int a = (int) l.get(0);
        String s = (String) l.get(1);

        System.out.println(a);
        System.out.println(s);
    }
}

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 
La programación genérica consiste en definir algoritmos y estructuras de datos de forma independiente del tipo concreto de los datos con los que operan. En lugar de escribir múltiples versiones de una misma estructura para distintos tipos (por ejemplo, listas de enteros, listas de dobles, etc.), se define una única versión parametrizable que puede reutilizarse con distintos tipos, aumentando así la reutilización y la seguridad del código.

El ejemplo anterior puede considerarse un ejemplo muy básico o rudimentario de programación genérica, ya que permite almacenar valores de distintos tipos en una misma estructura. Sin embargo, no es una solución completa ni segura, porque no existe un control estático de tipos: el compilador no puede verificar que los datos recuperados coincidan con el tipo esperado.

Por tanto, aunque conceptualmente apunta hacia la genericidad, en realidad se trata de una simulación manual de la misma. Los mecanismos modernos de programación genérica, como los generics en Java o los templates en C++, añaden comprobación de tipos en tiempo de compilación, lo que mejora significativamente la robustez del código.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 
El principal problema de emplear void* en C o Object en Java es la pérdida de información de tipo en tiempo de compilación. El compilador no puede verificar si el tipo de dato insertado en la estructura coincide con el tipo esperado al recuperarlo, lo que puede provocar errores que solo se detectan en tiempo de ejecución.

Esto obliga a realizar conversiones explícitas (casting o downcasting) al recuperar los datos. Estas conversiones son potencialmente peligrosas, ya que si se realiza un cast incorrecto, el comportamiento del programa puede ser indefinido en C o lanzar excepciones en Java (como ClassCastException). En ambos casos, se reduce la fiabilidad del sistema.

Además, se pierde expresividad en la interfaz de la estructura de datos, ya que no se puede indicar de forma clara qué tipo de elementos contiene. Esto dificulta el mantenimiento y comprensión del código, y aumenta la probabilidad de errores al utilizar la estructura en programas más complejos.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 
Los parámetros de tipo son una característica de los lenguajes modernos que permiten definir clases, interfaces o métodos de forma genérica, parametrizando el tipo de los datos con los que operan. En lugar de trabajar con tipos concretos, se introducen variables de tipo (por ejemplo, <T>) que se sustituyen por tipos reales cuando se instancia la clase o se invoca el método.

Esto permite al compilador realizar comprobaciones de tipo en tiempo de compilación, garantizando que las operaciones realizadas sobre los datos son coherentes con su tipo real. De esta forma, se evita el uso de conversiones explícitas y se mejora la seguridad del código.

Además, los parámetros de tipo hacen que el código sea más reutilizable y expresivo. Se puede definir una única estructura de datos genérica, como una lista o un mapa, que funcione correctamente para cualquier tipo, manteniendo siempre la coherencia de tipos y evitando errores comunes asociados al uso de tipos genéricos no seguros.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.
En Java, los generics permiten definir colecciones parametrizadas por tipo. Al declarar una lista como List<String>, el compilador garantiza que solo se podrán insertar objetos de tipo String, evitando errores de tipo en tiempo de compilación y eliminando la necesidad de conversiones explícitas al recuperar elementos.

import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();
        lista.add("uno");
        lista.add("dos");

        for (String s : lista) {
            System.out.println(s.toUpperCase()); // seguro, es String
        }
    }
}

En C++, los templates permiten definir estructuras parametrizadas de forma similar. En este caso, std::vector<std::string> garantiza en compilación que solo se almacenan cadenas, y cada elemento recuperado es directamente de ese tipo concreto.

#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> v;
    v.push_back("uno");
    v.push_back("dos");

    for (const std::string& s : v) {
        std::cout << s << std::endl;
    }
}


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?
Cuando se instancia una clase genérica, el compilador debe transformar esa definición abstracta en una forma concreta que pueda ejecutarse. Sin embargo, Java y C++ implementan este proceso de forma diferente, lo que tiene implicaciones importantes en tiempo de compilación y ejecución.

En Java, se utiliza el mecanismo conocido como type erasure. Esto significa que, durante la compilación, los parámetros de tipo se eliminan y se sustituyen por su límite superior (por defecto Object). El código generado no contiene información específica del tipo genérico, y las comprobaciones de tipo se realizan en compilación. Por tanto, en tiempo de ejecución no se distingue entre, por ejemplo, List<String> y List<Integer>.

En C++, en cambio, se utiliza la instanciación de plantillas. El compilador genera una versión específica del código para cada tipo utilizado. Por ejemplo, vector<string> y vector<int> generan implementaciones distintas en el código compilado. Esto permite optimizaciones más agresivas y mantiene la información de tipo en tiempo de compilación, pero puede aumentar el tamaño del binario.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 
Se puede definir una clase genérica con dos parámetros de tipo, lo que permite almacenar dos valores de tipos distintos manteniendo la seguridad de tipos. Cada tipo se representa mediante un parámetro, por ejemplo <T, U>, y se emplea en los atributos y métodos de la clase.

class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}

Se puede utilizar esta clase para devolver múltiples valores desde una función, como la media y la desviación típica de un array de double, manteniendo el tipo concreto de cada resultado.

public class Main {
    public static Par<Double, Double> calcular(double[] datos) {
        double suma = 0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;

        double var = 0;
        for (double d : datos) var += Math.pow(d - media, 2);
        double desv = Math.sqrt(var / datos.length);

        return new Par<>(media, desv);
    }
}


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Se puede definir un método que trabaje con Object, pero esto implica pérdida de información de tipo. El método no puede garantizar que ambos parámetros sean del mismo tipo, ni que el valor devuelto sea seguro sin conversiones explícitas.

import java.util.Random;

public static Object seleccionaUno(Object a, Object b) {
    return new Random().nextBoolean() ? a : b;
}

En este caso, al recuperar el valor devuelto será necesario realizar un downcasting, lo que puede provocar errores en tiempo de ejecución si el tipo no coincide con el esperado.

Por otro lado, un método genérico permite declarar un parámetro de tipo <T>, garantizando que ambos argumentos son del mismo tipo y que el valor devuelto también lo es. Esto elimina la necesidad de conversiones y mejora la seguridad.

public static <T> T seleccionaUno(T a, T b) {
    return new Random().nextBoolean() ? a : b;
}

El compilador fuerza que ambos argumentos sean del mismo tipo, y el valor devuelto es directamente de ese tipo, evitando errores y mejorando la claridad del código.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?
Sí, en Java se pueden establecer restricciones mediante límites superiores en los parámetros de tipo, usando la palabra clave extends. Esto permite indicar que un tipo genérico debe ser subtipo de una clase concreta, como Number, lo que permite tratar los valores como números.

Una primera solución consiste en usar directamente Number como tipo de las coordenadas, lo que permite almacenar distintos tipos numéricos, pero pierde precisión en el tipo concreto.

class Punto {
    private Number x, y;

    public Punto(Number x, Number y) {
        this.x = x; this.y = y;
    }

    public double distanciaA(Punto p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}

Una solución con generics permite mantener el tipo concreto:

class Punto<T extends Number> {
    private T x, y;

    public Punto(T x, T y) {
        this.x = x; this.y = y;
    }

    public double distanciaA(Punto<T> p) {
        double dx = x.doubleValue() - p.x.doubleValue();
        double dy = y.doubleValue() - p.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}

Tras la compilación, debido al type erasure, el tipo T se sustituye por Number, por lo que el tipo real en bytecode es equivalente a la primera solución.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?
Ambas soluciones permiten trabajar con distintos tipos numéricos sin duplicar la clase, pero difieren en el nivel de control de tipos. En la versión sin generics, es posible crear un punto con coordenadas de distintos tipos, como un Integer y un Double, ya que ambos son subtipos de Number.

En cambio, en la versión con generics, se fuerza que ambas coordenadas sean del mismo tipo T. Esto impide mezclar tipos distintos en una misma instancia, lo que refuerza la coherencia y evita posibles errores o conversiones implícitas no deseadas.

Respecto al tipo de retorno, en la versión sin generics los métodos getX y getY devuelven Number, lo que obliga a realizar conversiones si se quiere trabajar con un tipo concreto. En la versión genérica, devuelven T, lo que permite trabajar directamente con el tipo específico sin necesidad de casting

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

Se puede redefinir la interfaz Punto utilizando un parámetro de tipo que represente el propio tipo concreto. Esto se conoce como self-bounded generics o patrón CRTP en Java, y permite asegurar que las operaciones se realizan entre objetos del mismo tipo.

public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

Las implementaciones concretas especifican su propio tipo como parámetro:

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x; this.y = y;
    }

    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

De este modo, el compilador garantiza que solo se pueden comparar puntos del mismo tipo, eliminando la necesidad de instanceof y downcasting, y mejorando la seguridad y claridad del diseño.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

En Java, String es subtipo de Object, pero List<String> no es subtipo de List<Object>. Esto se debe a que los tipos genéricos en Java son invariantes, es decir, no mantienen la relación de subtipado entre sus parámetros de tipo. Permitirlo podría romper la seguridad de tipos.

En cambio, los arrays sí son covariantes: String[] es subtipo de Object[]. Esto puede provocar errores en tiempo de ejecución, ya que se puede asignar un array de String a una referencia de Object[] y luego intentar insertar un objeto que no sea String, lo que generará una excepción (ArrayStoreException).

Un tipo genérico es covariante si respeta la relación de subtipado en la misma dirección (T → S implica F<T> → F<S>), contravariante si la invierte, e invariante si no permite ninguna relación entre distintos parámetros de tipo. En Java, los genéricos son invariantes por defecto.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un wildcard (?) representa un tipo desconocido en una expresión genérica. Permite flexibilizar el uso de genéricos sin perder completamente la seguridad de tipos. Se utiliza junto con límites superiores (extends) o inferiores (super) para recuperar cierta variancia.

List<? extends T> es covariante y se usa cuando se quiere leer elementos de una lista sin modificarlos. Permite trabajar con listas de subtipos de T, pero no insertar elementos (salvo null).

public static double suma(List<? extends Number> lista) {
    double s = 0;
    for (Number n : lista) {
        s += n.doubleValue();
    }
    return s;
}

List<? super T> es contravariante y se usa cuando se quieren insertar elementos de tipo T. Permite añadir objetos de tipo T, pero al leer solo se garantiza que son Object.

public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
}

De este modo, los wildcards permiten recuperar flexibilidad en el uso de genéricos, manteniendo un equilibrio entre seguridad de tipos y expresividad.
<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.
En C, donde no existen mecanismos formales de excepciones, se pueden usar distintas estrategias para comunicar errores desde una función. Una opción común es devolver un código especial que indique si hubo error o no, mientras que el resultado real se devuelve por referencia mediante un puntero. Por ejemplo, la función raiz podría devolver 0 para éxito y 1 para error si recibe un número negativo, y el resultado de la raíz se almacena en una variable apuntada por un parámetro.

Otra opción es usar variables globales o estáticas para almacenar el estado del error, como la variable errno en C estándar. La función raiz devolvería el valor de la raíz si es válido, o un valor especial como -1 para indicar error, y el usuario consulta errno para saber qué ocurrió. Sin embargo, este método puede ser menos seguro en programas concurrentes y menos claro que la primera opción.

c
// Opción 1: código de error con resultado por referencia
#include <stdio.h>
#include <math.h>

int raiz(double x, double* resultado) {
    if (x < 0) return 1; // error
    *resultado = sqrt(x);
    return 0; // éxito
}

int main() {
    double res;
    if (raiz(-5, &res)) {
        printf("Error: número negativo\n");
    } else {
        printf("Raíz: %f\n", res);
    }
    return 0;
}


c
// Opción 2: valor especial y variable errno
#include <stdio.h>
#include <math.h>
#include <errno.h>

double raiz(double x) {
    if (x < 0) {
        errno = EDOM; // dominio erróneo
        return -1.0;
    }
    errno = 0;
    return sqrt(x);
}

int main() {
    double res = raiz(-5);
    if (errno) {
        printf("Error: número negativo\n");
    } else {
        printf("Raíz: %f\n", res);
    }
    return 0;
}



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?
Una excepción es un mecanismo para manejar errores o situaciones anómalas que pueden ocurrir durante la ejecución de un programa, interrumpiendo el flujo normal de instrucciones. Su objetivo principal es separar la lógica del programa del control de errores, facilitando el diseño y la lectura del código.

Los programadores utilizan excepciones para señalar que ocurrió una condición inesperada dentro de una función, sin tener que manejar el error inmediatamente allí. Las excepciones permiten que el código que llama pueda decidir cómo reaccionar, ya sea capturando la excepción para manejarla o dejándola propagarse para un manejo posterior. Esto mejora la robustez y mantenibilidad del software.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.
En Java, las excepciones se usan para controlar errores en tiempo de ejecución. En el ejemplo de la raíz, se puede definir un método dentro de una clase Calculadora que lance una excepción si el argumento es negativo. El método main llamará a este método y controlará la excepción mediante un bloque try-catch.

Esto demuestra cómo el error se propaga fuera de la función y es manejado externamente, dejando que la función raiz se centre en su lógica principal sin preocuparse por cómo se reporta el error.

java
public class Calculadora {
    public double raiz(double x) throws IllegalArgumentException {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            double resultado = calc.raiz(-5);
            System.out.println("Raíz: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}



## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

"Lanzar" una excepción significa que una función detecta una condición anómala y crea un objeto excepción que interrumpe el flujo normal para señalar el error. En Java, se usa la instrucción throw para lanzar una excepción.

"Controlar" o "capturar" una excepción es interceptarla mediante un bloque try-catch, donde el programa puede reaccionar adecuadamente, evitando que el error termine abruptamente la ejecución.

La "propagación" ocurre cuando una excepción no es capturada en la función donde se lanzó, y sube por la pila de llamadas buscando un manejador adecuado. Las funciones por donde pasa la excepción sin capturarla no se reanudan después; la ejecución salta directamente al primer bloque catch encontrado o termina el programa si no hay ninguno.

En el ejemplo de la raíz cuadrada, si raiz lanza la excepción, el método main la captura y maneja. Si main no la capturara, la excepción seguiría subiendo y podría terminar el progra


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?
La propagación natural de excepciones permite que los errores se comuniquen automáticamente desde el lugar donde ocurren hasta el código que puede manejarlos adecuadamente, sin necesidad de que cada función intermedia tenga que chequear y reenviar manualmente códigos de error. Esto reduce la complejidad y la cantidad de código repetitivo.

Además, mejora la claridad y mantenibilidad, ya que la lógica principal no se mezcla con el control de errores, y facilita la centralización del manejo de errores en puntos específicos, evitando que errores queden sin ser detectados o mal gestionados.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?
Sí, en lenguajes orientados a objetos como Java, las excepciones son objetos que derivan de una clase base de excepción. Esto permite encapsular toda la información relevante sobre el error (mensaje, causa, estado) dentro del objeto.

La ventaja es que el encapsulamiento facilita la organización, extensión y reutilización del código. También permite transportar detalles del error de forma estructurada entre métodos.

Por tanto, es posible y común crear excepciones personalizadas heredando de clases de excepción, adaptando la información y comportamiento a necesidades específicas de la aplicación.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

El objeto excepción suele contener un mensaje descriptivo del error, la causa original (otra excepción que provocó esta), y la pila de llamadas (stack trace) donde ocurrió el error.

Esta información es esencial porque permite al manejador diagnosticar el problema con mayor precisión, entender el contexto y rastrear el origen del fallo, facilitando la depuración y la toma de decisiones en el manejo.

En C, esta información se pierde o debe gestionarse manualmente, mientras que en Java está integrada y disponible automáticamente.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?
Sí, en Java se pueden encadenar varios bloques catch para capturar diferentes tipos de excepciones que puedan lanzarse dentro del bloque try. Solo se ejecuta el primer bloque catch cuyo tipo coincida o sea supertipo de la excepción lanzada.

Los demás bloques catch se ignoran tras capturar la excepción en el primero adecuado.

---

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con finally, tanto con catch como sin él.



Para garantizar que cierto código se ejecute siempre, independientemente de si ocurre una excepción o no, se usa el bloque finally después de try (y opcionalmente catch). El código dentro de finally se ejecuta siempre antes de salir del bloque try-catch-finally, incluso si se lanzó una excepción o hay un return.

Ejemplo con catch:

java
try {
    // abrir fichero
    // operar
} catch (IOException e) {
    // manejar error
} finally {
    // cerrar fichero, liberar recursos
}


Ejemplo sin catch (propaga excepción):

java
try {
    // abrir fichero
    // operar
} finally {
    // cerrar fichero, liberar recursos
}


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.
Sí, en Java es válido usar try-finally sin catch. El bloque finally se ejecuta siempre, tanto si ocurre una excepción como si no.

Incluso si dentro del bloque try hay un return, el código del finally se ejecutará antes de que el método termine y retorne el valor.

Esto asegura la liberación de recursos o la ejecución de limpieza sin importar cómo termine el try.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?
Las excepciones "controladas" (checked exceptions) son aquellas que el compilador obliga a manejar explícitamente con try-catch o declarar con throws. Se usan para situaciones previsibles y recuperables, como errores de E/S.

Las "no controladas" (unchecked exceptions) derivan de RuntimeException y representan errores de programación o condiciones inesperadas, como errores de lógica o uso incorrecto de API. No es obligatorio manejarlas o declararlas.

Ejemplos de excepciones controladas:

* IOException (fallo en acceso a fichero)
* SQLException (errores en base de datos)
* ClassNotFoundException (clase no encontrada)
* InterruptedException (hilo interrumpido)

Ejemplos de excepciones no controladas:

* NullPointerException (objeto nulo)
* IllegalArgumentException (argumento inválido)
* IndexOutOfBoundsException (índice fuera de rango)
* ArithmeticException (división por cero)

Se suele preferir excepción controlada para errores externos que pueden recuperarse o necesitar manejo específico. La no controlada es mejor para errores de lógica que indican bugs y no suelen manejarse en producción.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.
La palabra clave throws en la firma de un método indica que este método puede lanzar ciertas excepciones controladas, pero no las maneja internamente. Esto obliga a quien llama al método a manejar o declarar la excepción.

Usar throws es una alternativa a capturar la excepción dentro del método, permitiendo que el manejo se delegue al nivel superior que tenga más contexto para decidir cómo reaccionar.

---

## 13. Pon un ejemplo en Java de firma de método que incluya throws, de una función que abre un fichero pero que declara que no le interesa manejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del finally.
import java.io.FileInputStream;
import java.io.IOException;

public class Ejemplo {

    public static void leerFichero(String ruta) throws IOException {
        FileInputStream fichero = null;
        try {
            fichero = new FileInputStream(ruta);
            int dato;
            while ((dato = fichero.read()) != -1) {
                System.out.print((char) dato);
            }
        } finally {
            // Se ejecuta siempre, haya o no excepción
            if (fichero != null) {
                try {
                    fichero.close();
                } catch (IOException e) {
                    System.out.println("Error al cerrar el fichero");
                }
            }
        }
    }
}

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?
Sí, es posible declarar en throws excepciones no controladas, aunque no es obligatorio. En general, no se suele hacer porque el manejo de RuntimeException es opcional y representan errores de programación.

El método llamador no está obligado a capturarlas. Capturarlas podría tener sentido en casos donde se quiera hacer una recuperación específica o registrar información antes de terminar la ejecución, pero no es común.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.
java
import java.io.*;

public class Archivo {
    public void abrirArchivo(String nombre) throws FileNotFoundException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader(nombre));
            // Operaciones con el archivo
        } finally {
            if (br != null) {
                try {
                    br.close();
                } catch (IOException e) {
                    // Manejo de error al cerrar
                }
            }
        }
    }
}


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?
Sí, es posible declarar en throws excepciones no controladas, aunque no es obligatorio. En general, no se suele hacer porque el manejo de RuntimeException es opcional y representan errores de programación.

El método llamador no está obligado a capturarlas. Capturarlas podría tener sentido en casos donde se quiera hacer una recuperación específica o registrar información antes de terminar la ejecución, pero no es común.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?
Se recomienda usar excepciones controladas para errores previsibles y externos, como problemas con archivos o red, que pueden tener soluciones o recuperación.

Las excepciones no controladas se usan para errores de lógica interna, como pasar argumentos inválidos, que indican bugs y no deberían ser capturados rutinariamente.

No todos los lenguajes distinguen ambas categorías. Por ejemplo, Java sí, pero otros como Python solo tienen un sistema unificado de excepciones no verificadas. En esos casos, la opción más habitual es el modelo de excepciones no controladas, donde el manejo es opcional

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.
Sí, tiene sentido lanzar una nueva excepción dentro de un catch para transformar o enriquecer la información del error o para encapsularlo en una excepción más apropiada al contexto.

También es posible relanzar la misma excepción capturada con throw para permitir que otro nivel superior la maneje, por ejemplo, después de realizar tareas de limpieza o registro.

Ejemplo de lanzar nueva excepción:

java
catch(IOException e) {
    throw new MiExcepcionPersonalizada("Error al procesar archivo", e);
}


Ejemplo de relanzar la misma excepción:

java
catch(IOException e) {
    log(e);
    throw e;
}


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?
Que una excepción sea la "causa" de otra significa que la segunda envuelve a la primera, manteniendo la referencia para conservar el origen original del error. Esto es útil para abstraer detalles técnicos y dar contexto adicional.

En Java, muchas excepciones permiten pasar la excepción original como causa en el constructor.

Ejemplo:

java
try {
    abrirArchivo("archivo.txt");
} catch (FileNotFoundException e) {
    throw new MiExcepcionPersonalizada("No se pudo abrir el archivo", e);
}


Al imprimir la excepción, la causa se muestra en la pila de llamadas, facilitando la depuración y mostrando toda la cadena de errores.



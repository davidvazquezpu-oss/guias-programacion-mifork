<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.
En el lenguaje C, la composición se logra anidando tipos de datos definidos por el usuario. Una estructura puede contener como miembros otras estructuras previamente definidas, estableciendo una relación de jerarquía física en la memoria. Esta técnica permite modelar entidades complejas basándose en componentes más simples, donde el "contenedor" posee una instancia directa de los "componentes".

Para calcular distancias y longitudes, se emplean funciones que reciben estas estructuras. Dado que en C no existe el concepto de métodos asociados a datos, la lógica se mantiene separada de la declaración del struct, operando sobre los miembros de forma externa.

C
#include <stdio.h>
#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double calcularDistancia(struct Punto a, struct Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

double calcularLongitud(struct Linea l) {
    return calcularDistancia(l.p1, l.p2);
}


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  
En Java, la composición se realiza declarando variables de instancia (atributos) cuyo tipo es otra clase. A diferencia de C, la orientación a objetos permite unir los datos con el comportamiento mediante métodos. Para garantizar la inmutabilidad, se marcan los atributos como final y se omiten los métodos "setter", asegurando que el estado del objeto no cambie tras su construcción.

La ocultación de información (encapsulación) protege la integridad de los datos internos. Al declarar los puntos como private final, se evita que agentes externos modifiquen las coordenadas o sustituyan los puntos de la línea, superando la fragilidad de las estructuras simples de C donde cualquier parte del programa podría alterar los miembros del struct.

Java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

public class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double obtenerLongitud() {
        return inicio.distanciaA(fin);
    }
}


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.
La multiplicidad define el número de instancias de una clase que pueden estar vinculadas a una instancia de otra clase en una relación específica. Es un concepto fundamental en el diseño de sistemas para determinar si la relación es de uno a uno (1:1), uno a muchos (1:N) o muchos a muchos (N:M). Ayuda a definir cómo se deben implementar las estructuras de almacenamiento (variables simples o colecciones).

En el ejemplo propuesto, la multiplicidad se analiza en ambos sentidos:

De Linea a Punto: La multiplicidad es 2 (o exactamente 2), ya que una línea requiere estrictamente un punto de inicio y un punto de fin para existir.

De Punto a Linea: La multiplicidad puede ser 0..* (de cero a muchas), puesto que un punto puede existir de forma independiente sin pertenecer a ninguna línea, o puede ser el extremo de múltiples líneas distintas.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.
La diferencia radica en el grado de dependencia y la gestión del ciclo de vida. En la composición fuerte (referida simplemente como Composición), el objeto hijo no tiene sentido sin el padre; si el contenedor se destruye, sus partes también deben desaparecer. Existe una propiedad estricta donde el componente pertenece exclusivamente a un solo compuesto en un momento dado.

En la composición débil (conocida como Agregación o Asociación), los objetos tienen ciclos de vida independientes. El objeto "parte" puede existir antes de que se cree el "todo" y puede seguir existiendo después de que el contenedor sea destruido. Es una relación de "pertenencia" más flexible donde los objetos suelen ser compartidos o pasados al contenedor desde el exterior.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?
En este caso se habla de dependencia. A diferencia de la composición, donde una clase "tiene" a otra como un atributo persistente que define su estado, la dependencia es una relación transitoria y mucho más débil. Se produce cuando una clase simplemente utiliza a otra de forma puntual para realizar una tarea específica.

Si la clase A no almacena a la clase B en una variable de instancia, sino que solo la usa como una herramienta dentro de un método (como un parámetro o una variable local), no hay un vínculo estructural permanente. Se dice que "A usa a B", pero no que "A se compone de B". Es el nivel más bajo de acoplamiento entre clases en orientación a objetos.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.
Para implementar la composición fuerte, el objeto contenedor debe ser responsable de la creación de sus partes. En el constructor de Linea, se reciben las coordenadas y se instancian nuevos objetos Punto internamente. De este modo, esos puntos nacen y mueren exclusivamente con esa línea, y no son accesibles ni compartidos por nadie más fuera de ella.

En la composición débil (Agregación), la Linea recibe objetos Punto ya creados a través de sus parámetros. La línea simplemente guarda una referencia a objetos que existen de forma independiente en el programa. Si la línea se destruye, los puntos originales siguen existiendo en la memoria de la aplicación porque no fueron creados por la clase Linea.

Java
// Composición Fuerte (La línea crea sus propios puntos)
public class LineaFuerte {
    private Punto p1, p2;
    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

// Composición Débil (La línea recibe puntos externos)
public class LineaDebil {
    private Punto p1, p2;
    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
}


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?
En Java, a diferencia de C++, no existe una instrucción de destrucción explícita (como delete). El encargado de liberar la memoria es el Garbage Collector (Recolector de Basura). En una composición fuerte, cuando el objeto contenedor (la Linea) deja de ser referenciado y es marcado para su eliminación, las referencias internas a los objetos hijos (Punto) también se pierden.

Si esos objetos Punto no tienen ninguna otra referencia activa en el programa (lo cual es propio de la composición fuerte), el Garbage Collector detectará que son inalcanzables y liberará su memoria automáticamente. Por lo tanto, la destrucción es implícita y ocurre cuando el grafo de objetos queda desconectado de la ejecución principal del programa.

8. Pon un ejemplo de composición débil entre un departamento que tiene varios profesores...
Respuesta
Se presenta la implementación de la clase Departamento. Se utiliza un array de tamaño fijo para almacenar los profesores y una referencia específica para el director. Para mantener la integridad, el constructor y los métodos de modificación validan las reglas de negocio mediante excepciones.

La lógica de eliminación incluye una comprobación crítica: no se puede eliminar a un profesor si este es actualmente el director, ya que el director debe ser siempre un miembro activo de la lista del departamento.

Java
public class Departamento {
    private String nombre;
    private Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(String nombre, Profesor director) throws Exception {
        if (director == null) throw new Exception("El director es obligatorio");
        this.nombre = nombre;
        this.director = director;
        aniadirProfesor(director);
    }

    public void aniadirProfesor(Profesor p) throws Exception {
        if (numProfesores >= 50) throw new Exception("Lista llena");
        if (p == null) throw new Exception("Profesor nulo");
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int pos) throws Exception {
        if (pos < 0 || pos >= numProfesores) throw new Exception("Posición inválida");
        if (profesores[pos] == director) throw new Exception("No se puede eliminar al director");
        
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--numProfesores] = null;
    }

    public void setDirector(Profesor nuevoDirector) throws Exception {
        boolean existe = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) { existe = true; break; }
        }
        if (!existe) throw new Exception("El director debe pertenecer al departamento");
        this.director = nuevoDirector;
    }

    public int getTotalProfesores() { return numProfesores; }
    public Profesor getProfesor(int pos) { return profesores[pos]; }
}


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.
Se presenta la implementación de la clase Departamento. Se utiliza un array de tamaño fijo para almacenar los profesores y una referencia específica para el director. Para mantener la integridad, el constructor y los métodos de modificación validan las reglas de negocio mediante excepciones.

La lógica de eliminación incluye una comprobación crítica: no se puede eliminar a un profesor si este es actualmente el director, ya que el director debe ser siempre un miembro activo de la lista del departamento.

Java
public class Departamento {
    private String nombre;
    private Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(String nombre, Profesor director) throws Exception {
        if (director == null) throw new Exception("El director es obligatorio");
        this.nombre = nombre;
        this.director = director;
        aniadirProfesor(director);
    }

    public void aniadirProfesor(Profesor p) throws Exception {
        if (numProfesores >= 50) throw new Exception("Lista llena");
        if (p == null) throw new Exception("Profesor nulo");
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int pos) throws Exception {
        if (pos < 0 || pos >= numProfesores) throw new Exception("Posición inválida");
        if (profesores[pos] == director) throw new Exception("No se puede eliminar al director");
        
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--numProfesores] = null;
    }

    public void setDirector(Profesor nuevoDirector) throws Exception {
        boolean existe = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) { existe = true; break; }
        }
        if (!existe) throw new Exception("El director debe pertenecer al departamento");
        this.director = nuevoDirector;
    }

    public int getTotalProfesores() { return numProfesores; }
    public Profesor getProfesor(int pos) { return profesores[pos]; }
}


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?
Al utilizar ArrayList, el código se simplifica drásticamente al eliminar la gestión manual del tamaño del array y los desplazamientos necesarios al borrar elementos. Se ahorra toda la lógica de control de índices para la inserción y el borrado (for de desplazamiento), así como la declaración del tamaño máximo (50), ya que las listas son dinámicas.

El problema de devolver la lista interna directamente es la rotura de la encapsulación. Si un método devuelve la referencia a ArrayList, cualquier agente externo podría añadir o borrar profesores sin pasar por las validaciones de la clase Departamento (como la regla del director). Para resolverlo, se debe devolver una copia de la lista o, preferiblemente, una vista no modificable mediante Collections.unmodifiableList(profesores).

Java
import java.util.*;

public class DepartamentoList {
    private List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public DepartamentoList(String nombre, Profesor director) throws Exception {
        this.director = director;
        aniadirProfesor(director);
    }

    public void aniadirProfesor(Profesor p) {
        profesores.add(p);
    }

    public void eliminarProfesor(int pos) throws Exception {
        if (profesores.get(pos) == director) throw new Exception("Es el director");
        profesores.remove(pos);
    }

    public List<Profesor> getProfesores() {
        // Se devuelve una copia para proteger la integridad
        return new ArrayList<>(profesores);
    }
}

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.
La composición recursiva ocurre cuando una clase tiene un atributo de su mismo tipo. Es la base para estructuras de datos jerárquicas o en cadena. En el caso de la clase Persona, cada objeto contiene una referencia a otra instancia de Persona que representa a su progenitor, permitiendo navegar por un árbol genealógico de longitud arbitraria.

Otros ejemplos clásicos incluyen las Estructuras de Directorios (una carpeta contiene archivos y otras carpetas), los Nodos de una Lista Enlazada (cada nodo apunta al siguiente nodo) o los Sistemas de Menús (una opción de menú puede desplegar otro submenú compuesto por más opciones).

Java
public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Berta", abuela);
        Persona nieto = new Persona("Carlos", madre);
        
        System.out.println("Nieto: " + nieto.nombre);
        System.out.println("Madre: " + nieto.madre.nombre);
    }
}

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?
Una relación bidireccional permite que ambos objetos involucrados en la composición tengan una referencia mutua. Mientras que en una relación unidireccional el Departamento conoce a sus Profesores pero el profesor no sabe a qué departamento pertenece, en la bidireccional ambos "se conocen". Esto facilita la navegación entre objetos desde cualquier extremo de la relación.

Para implementarlo en el ejemplo anterior, la clase Profesor debería incluir un atributo de tipo Departamento. Al añadir un profesor a la lista del departamento, sería necesario actualizar también el atributo interno del profesor para que apunte a ese departamento. Esto requiere un cuidado especial para mantener la consistencia, asegurando que si A apunta a B, B apunte necesariamente a A.
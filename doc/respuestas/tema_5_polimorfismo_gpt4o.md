<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo es la capacidad que poseen los objetos de responder de manera distinta ante un mismo mensaje o llamada a un método, dependiendo de su tipo real en tiempo de ejecución. En programación orientada a objetos, sirve para diseñar sistemas desacoplados y extensibles, permitiendo que un programa trabaje con una jerarquía de clases de forma genérica a través de sus clases base, sin necesidad de conocer los detalles específicos de cada subclase.

La sobreescritura (overriding) es el mecanismo que hace posible el polimorfismo. Consiste en volver a definir en una subclase un método que ya existe en la clase superior (superclase), manteniendo la misma firma (nombre y parámetros). Al sobreescribir, se proporciona una implementación específica que reemplaza o extiende el comportamiento original para los objetos de esa subclase.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica o enlace tardío (late binding) es el proceso por el cual se decide qué implementación de un método se va a ejecutar en tiempo de ejecución y no en tiempo de compilación. Cuando se llama a un método sobre una referencia de una clase base que apunta a un objeto de una subclase, el sistema busca la versión más específica del método en la jerarquía de objetos. Esta es la base técnica del polimorfismo, ya que permite que la identidad real del objeto determine el comportamiento.

La necesidad de indicarlo explícitamente varía según el lenguaje. En C++, el polimorfismo no es el comportamiento por defecto; es necesario declarar los métodos como virtual en la clase base para habilitar la ligadura dinámica. Si no se usa virtual, C++ aplica ligadura estática, ejecutando el método según el tipo de la referencia o puntero, no según el objeto apuntado.

En cambio, en Java todos los métodos no estáticos son, por defecto, polimórficos (equivalentes a virtual en C++), a menos que se marquen como final. No se requiere ninguna palabra clave para habilitar la ligadura dinámica. En Python, al ser un lenguaje de tipado dinámico, el polimorfismo es intrínseco y se aplica siempre mediante el "duck typing", donde el enlace se resuelve en el momento exacto de la llamada.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

En este diseño, se define una clase base con un comportamiento general que luego es especializado por las subclases. El polimorfismo permite tratar a todos como el tipo genérico pero manteniendo sus particularidades.
class Soldado {
    void saludar() {
        System.out.println("Soldado presentándose.");
    }
}

class Zapador extends Soldado {
    @Override
    void saludar() {
        System.out.println("Zapador despejando el camino. ¡Firmes!");
    }
}

class Artillero extends Soldado {
    // Hereda el saludo por defecto o podría sobreescribirlo
}

public class Principal {
    public static void main(String[] args) {
        Soldado[] peloton = { new Zapador(), new Artillero() };
        
        for (Soldado s : peloton) {
            s.saludar(); // Aquí actúa el polimorfismo
        }
    }
}
Al recorrer el array, aunque la variable s es de tipo Soldado, el programa ejecuta el método saludar de Zapador para el primer elemento y el de Soldado para el segundo. Se observa cómo el código del bucle es agnóstico a las especialidades del soldado.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, es posible y frecuente invocar el método de la clase base desde la subclase para reutilizar lógica existente y añadir funcionalidad adicional. Esto evita la duplicación de código y permite que la subclase actúe como una extensión incremental de la superclase.

En Java, se utiliza la palabra clave super para referenciar a la clase inmediata superior. Para el caso propuesto del Zapador, el código se estructuraría llamando primero a la lógica general y añadiendo después el mensaje específico.
class Zapador extends Soldado {
    @Override
    void saludar() {
        super.saludar(); // Invoca "Soldado presentándose."
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Para sobreescribir un método en Java, el nombre y los parámetros deben ser idénticos. El tipo de retorno debe ser el mismo o un subtipo del original (retorno covariante). Además, el método sobreescrito no puede reducir la visibilidad del original (por ejemplo, de public a private) ni lanzar nuevas excepciones comprobadas que no estuvieran declaradas en la base.

La diferencia fundamental entre sobreescritura (overriding) y sobrecarga (overloading) radica en la firma y el momento de resolución. La sobrecarga ocurre en una misma clase cuando existen métodos con el mismo nombre pero distintos parámetros; se resuelve en tiempo de compilación. La sobreescritura ocurre entre clases con herencia, tiene la misma firma y se resuelve en tiempo de ejecución.

La anotación @Override sirve para informar al compilador de la intención de sobreescribir un método. Su uso es altamente recomendable porque actúa como medida de seguridad: si se comete un error tipográfico en el nombre del método o en los parámetros, el compilador generará un error al no encontrar un método equivalente en la clase base, evitando que se cree un método nuevo por error en lugar de sobreescribir el deseado.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Efectivamente, el polimorfismo se utiliza en Java desde el aprendizaje de sus conceptos más básicos, incluso de forma inconsciente. Dado que en Java todas las clases heredan de la clase raíz Object, cualquier redefinición de métodos como toString() o equals() es un ejercicio de polimorfismo.

Cuando se sobreescribe toString(), se está proporcionando una implementación específica que será invocada por métodos genéricos del sistema (como System.out.println) que solo conocen la existencia de la clase Object. Este es un ejemplo puro de cómo la plataforma Java se apoya en el polimorfismo para manejar cualquier tipo de dato de forma estandarizada.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta es una clase que no está completa y, por tanto, no puede ser instanciada (no se pueden crear objetos con new de ella). Su propósito principal es servir como molde o contrato para otras subclases, definiendo un estado y comportamiento común, pero dejando partes sin implementar.

Un método abstracto es una declaración de método que no posee cuerpo (implementación). Obliga a las subclases no abstractas a proporcionar una implementación concreta de dicho método. Se utiliza cuando se sabe qué debe hacer la jerarquía de clases, pero no cómo debe hacerlo cada una de ellas de forma específica.

abstract class Soldado {
    void saludar() { System.out.println("Hola"); }
    
    abstract void atacar(); // Sin cuerpo
}

class Zapador extends Soldado {
    @Override
    void atacar() {
        System.out.println("Colocando explosivos.");
    }
}
La palabra clave abstract debe colocarse tanto en la firma del método que no tiene implementación como en la declaración de la clase que contiene dicho método.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave final tiene un efecto restrictivo que detiene las capacidades de extensión de la orientación a objetos. Cuando se aplica a una clase, impide que esta sea heredada por cualquier otra subclase. Cuando se aplica a un método, impide que este sea sobreescrito en las subclases, fijando su comportamiento de forma definitiva.

En relación con el polimorfismo, final actúa como el opuesto directo. Al marcar un método como final, se permite al compilador realizar optimizaciones de ligadura estática, ya que se garantiza que ninguna subclase cambiará esa implementación. Se utiliza por razones de seguridad o para asegurar que el diseño de una clase no sea alterado.

Un ejemplo clásico de clase final en la API estándar de Java es la clase String. Debido a su importancia crítica en la seguridad y el manejo de memoria, Java prohíbe que el usuario cree subclases de String que puedan alterar su comportamiento interno de inmutabilidad.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las interfaces son estructuras que definen un contrato de comportamiento que las clases deben cumplir. A diferencia de las clases abstractas, las interfaces tradicionalmente no podían almacenar estado (variables de instancia) y solo contenían constantes y firmas de métodos. Representan lo que una clase "puede hacer" más que lo que "es".

Una de las diferencias clave es que Java no permite la herencia múltiple de clases (una clase solo puede tener una superclase), pero sí permite que una clase implemente múltiples interfaces. Esto ofrece una flexibilidad enorme, permitiendo que un objeto sea tratado desde diferentes puntos de vista según la interfaz que se utilice para referenciarlo. 


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

El siguiente diseño muestra cómo una clase puede operar con abstracciones sin conocer los detalles de implementación, delegando la responsabilidad del cálculo a las subclases.
abstract class Punto {
    abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;
    @Override
    double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro; // Downcasting
            return Math.sqrt(Math.pow(p.x - this.x, 2) + Math.pow(p.y - this.y, 2));
        }
        return 0;
    }
}

class Linea {
    Punto p1, p2;
    double obtenerLongitud() {
        return p1.calcularDistanciaA(p2); // Polimorfismo puro
    }
}
La clase Linea no sabe si trabaja con puntos 2D o 3D; simplemente invoca el método polimórfico. El uso de instanceof asegura que la operación de downcasting (convertir la referencia de la base a la subclase) sea segura antes de acceder a los atributos específicos de las coordenadas.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces permite que una interfaz herede de otra u otras, expandiendo el contrato original. A diferencia de las clases, Java sí permite la herencia múltiple de interfaces, lo que significa que una interfaz puede extender varias interfaces padres simultáneamente mediante el uso de comas.

Este mecanismo permite construir capacidades modulares. Una clase que implemente la interfaz más específica estará obligada por contrato a implementar todos los métodos definidos en toda la cadena de herencia de interfaces.

interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}

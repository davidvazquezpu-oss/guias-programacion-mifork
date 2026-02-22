<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.
La encapsulación busca agrupar datos y comportamientos relacionados dentro de una misma unidad lógica, normalmente una clase. La idea es que el objeto controle cómo se accede y modifica su estado interno, evitando que otras partes del programa manipulen directamente sus atributos. Esto permite que el objeto mantenga su coherencia interna y que los cambios en su implementación no afecten al resto del sistema.

La ocultación de información complementa a la encapsulación restringiendo el acceso a los detalles internos de un objeto. Al ocultar los atributos y exponer solo una interfaz pública, se evita que el código externo dependa de aspectos internos que podrían cambiar. Entre sus ventajas destacan: reducir errores al impedir modificaciones indebidas, facilitar el mantenimiento al permitir cambiar la implementación sin afectar al exterior, y mejorar la claridad del diseño al obligar a interactuar con los objetos mediante métodos bien definidos.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.
La interfaz pública de una clase es el conjunto de métodos y, en algunos casos, constantes accesibles desde fuera de la clase. Representa la forma oficial en la que otros objetos pueden interactuar con ella. Es, por así decirlo, el “contrato” que la clase ofrece al exterior, indicando qué operaciones están permitidas y cómo deben utilizarse.

Esta interfaz pública está directamente relacionada con la ocultación de información porque actúa como una barrera entre el interior del objeto y el resto del programa. Todo lo que no forme parte de la interfaz pública puede mantenerse oculto mediante modificadores de acceso como private. De este modo, la clase controla qué se expone y qué se protege, garantizando que su estado interno solo pueda modificarse de formas seguras y previstas.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?
Diseñar con cuidado la interfaz pública es fundamental porque cualquier cambio posterior puede afectar a todas las partes del programa que dependan de ella. Una interfaz mal diseñada puede obligar a modificar múltiples módulos, dificultar la evolución del software o incluso romper compatibilidad con versiones anteriores. Por ello, se recomienda que la interfaz sea clara, estable y lo más pequeña posible.

Cambiar la interfaz pública no suele ser fácil, especialmente en proyectos grandes o en bibliotecas utilizadas por terceros. Una vez que otros componentes dependen de ciertos métodos, eliminarlos o modificarlos puede generar errores o comportamientos inesperados. Por eso, la ocultación de información y el diseño cuidadoso ayudan a minimizar la necesidad de cambios futuros.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?
Las invariantes de clase son condiciones que deben cumplirse siempre para que un objeto sea válido. Normalmente se refieren a restricciones sobre los valores de los atributos o sobre relaciones entre ellos. Por ejemplo, en una clase que representa un intervalo numérico, la invariante podría ser que el límite inferior nunca sea mayor que el superior.

La ocultación de información ayuda a mantener estas invariantes porque impide que el código externo modifique directamente los atributos. Si los cambios solo pueden realizarse mediante métodos controlados, la clase puede verificar que las invariantes se cumplen antes de aceptar cualquier modificación. Esto reduce errores y garantiza que los objetos permanezcan en un estado coherente.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

public class Punto { 
    private double x; private double y; 
    public Punto(double x, double y) { this.x = x; this.y = y; 
    } 
    public double calcularDistanciaAOrigen() { 
        return Math.sqrt(x * x + y * y); 
    } 
}
La interfaz pública de esta clase está formada por el constructor Punto(double, double) y el método calcularDistanciaAOrigen(). Estos son los elementos accesibles desde fuera de la clase. Los atributos x e y están ocultos porque son privados, y solo la propia clase puede acceder a ellos.

En Java, public indica que un miembro es accesible desde cualquier parte del programa, mientras que private limita el acceso exclusivamente a la propia clase. Esto permite controlar qué se expone y qué se mantiene protegido.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores public y private pueden aplicarse tanto a clases como a miembros de clase, es decir, atributos, métodos y constructores. Sin embargo, no todas las clases pueden ser privadas: solo las clases internas pueden declararse como private. Las clases de nivel superior solo pueden ser public o tener visibilidad por defecto.

En el caso de los miembros, estos modificadores permiten controlar el acceso desde otras clases. Un miembro private solo es accesible desde la propia clase, mientras que un miembro public puede ser utilizado desde cualquier otra clase del programa. Esto es esencial para implementar la encapsulación.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Además de la visibilidad pública y privada, muchos lenguajes orientados a objetos incluyen niveles intermedios. En Java existen cuatro niveles: public, private, protected y el nivel por defecto (package-private). Cada uno define un grado distinto de acceso, permitiendo un control más fino sobre qué partes del código pueden interactuar con un miembro.

El modificador protected permite el acceso desde clases del mismo paquete y desde clases hijas, incluso si están en paquetes distintos. El nivel por defecto permite el acceso solo dentro del mismo paquete. Otros lenguajes, como C++, incluyen también visibilidad protected, pero su sistema de módulos y paquetes difiere, por lo que las reglas exactas cambian.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los miembros privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Esto significa que un objeto puede acceder a los atributos privados de otro objeto si ambos pertenecen a la misma clase. Esta característica permite implementar métodos que comparan o combinan información entre objetos sin necesidad de exponer sus atributos.
public double calcularDistanciaAPunto(Punto otro) { 
    double dx = this.x - otro.x; 
    double dy = this.y - otro.y; 
    return Math.sqrt(dx * dx + dy * dy); 
}
Aunque x e y son privados, el método puede acceder a otro.x y otro.y porque ambos objetos son instancias de la misma clase Punto. Esto no viola la encapsulación, ya que sigue siendo la propia clase la que controla el acceso a sus datos.
## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos "getter" son métodos públicos que permiten obtener el valor de un atributo privado. Suelen seguir la convención getNombreAtributo(). Por su parte, los métodos "setter" permiten modificar el valor de un atributo privado y suelen llamarse setNombreAtributo(valor). Ambos forman parte de la interfaz pública cuando se desea permitir el acceso controlado a los atributos.

Estos métodos son útiles porque permiten validar o transformar los datos antes de devolverlos o asignarlos. Además, permiten mantener la encapsulación: los atributos siguen siendo privados, pero se ofrece un acceso regulado a ellos. Esto ayuda a preservar las invariantes de clase y a evitar modificaciones indebidas


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

Cuando se dice que la ocultación de información mejora la seguridad, no se está hablando de seguridad informática en el sentido de evitar ataques externos o hackeos. La seguridad a la que se refiere la POO es la seguridad del diseño y del mantenimiento del software. Es decir, evitar que el programa entre en estados inconsistentes o que se produzcan errores por accesos indebidos.

La ocultación de información ayuda a que los objetos mantengan su coherencia interna y a que el código externo no pueda modificar atributos de forma arbitraria. Esto reduce errores lógicos y facilita el mantenimiento, pero no protege frente a ataques maliciosos. Para eso se necesitan otras medidas como cifrado, autenticación o control de accesos a nivel de sistema.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?
Un miembro de instancia pertenece a cada objeto individual, por lo que cada instancia tiene su propia copia del atributo o método. En cambio, un miembro de clase pertenece a la clase en sí misma y es compartido por todas las instancias. En Java, los miembros de clase se declaran con la palabra clave static.

Los miembros de clase también pueden ocultarse utilizando los mismos modificadores de acceso (private, public, etc.). Esto permite controlar si el miembro está disponible para otras clases o si solo debe ser utilizado internamente. La encapsulación se aplica tanto a miembros de instancia como a miembros de clase.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, en algunos casos tiene sentido que los constructores sean privados. Esto ocurre cuando se quiere controlar estrictamente cómo se crean las instancias de una clase. Por ejemplo, en el patrón Singleton, el constructor es privado para evitar que se creen múltiples instancias y se proporciona un método estático que devuelve la única instancia existente.

También es útil cuando se desea obligar al uso de métodos factoría, que pueden realizar validaciones, transformaciones o aplicar lógica adicional antes de crear un objeto. En estos casos, el constructor privado garantiza que las instancias solo se creen de formas controladas.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

En Java, los miembros de clase se indican mediante la palabra clave static. Esto hace que el atributo o método pertenezca a la clase en sí misma y no a cada instancia. Todos los objetos comparten el mismo valor de un atributo estático, lo que permite almacenar información común.

Un ejemplo sería:

java
public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}
Aquí, maxX y maxY son miembros de clase que registran los valores máximos observados. Los métodos estáticos permiten consultarlos sin necesidad de una instancia concreta.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

Un método factoría suele ser estático porque crea instancias sin necesidad de tener un objeto previo. Un ejemplo sería:

public static Punto crearRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
El uso de static permite llamar al método como Punto.crearRedondeado(...) sin necesidad de una instancia. Además, encapsula la lógica de redondeo dentro de la clase, manteniendo la coherencia del diseño.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.
La interfaz pública puede mantenerse igual mientras se cambia la representación interna. Esto demuestra la utilidad de la ocultación de información: el exterior no necesita saber cómo se almacenan los datos. Un ejemplo sería:

java
public class Punto {
    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }
}
El cambio es completamente interno y no afecta a los usuarios de la clase. Esto permite modificar la implementación sin romper código existente.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Aunque un atributo tenga getter y setter públicos, no es recomendable declararlo público. Los métodos permiten añadir validaciones, restricciones o lógica adicional en el futuro sin cambiar la interfaz pública. Si el atributo fuera público, cualquier cambio en su gestión obligaría a modificar el código externo, rompiendo la encapsulación.

La convención más habitual es declarar los atributos como privados y proporcionar getters y setters solo cuando sea necesario. Esto está relacionado con las invariantes de clase porque los setters pueden comprobar que los nuevos valores no violan dichas invariantes. Si los atributos fueran públicos, no habría forma de controlar estas condiciones.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable cuando sus objetos no pueden cambiar su estado después de ser creados. Esto implica que no existen métodos que modifiquen los atributos internos y que estos se establecen únicamente en el constructor. Las clases inmutables suelen ser más seguras y fáciles de razonar, ya que su estado no cambia inesperadamente.

Un método modificador es cualquier método que altere el estado interno de un objeto. Aunque los setters son un tipo de método modificador, no todos los modificadores son setters; por ejemplo, un método que incremente un contador también modifica el estado. Las clases inmutables evitan estos métodos y, en su lugar, devuelven nuevas instancias cuando se necesita un cambio.

Las clases inmutables tienen ventajas como facilitar la concurrencia, evitar errores por modificaciones inesperadas y simplificar el mantenimiento. Por eso, muchos tipos básicos en Java, como String, son inmutables.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No es recomendable incluir setters por defecto. Solo deben añadirse cuando realmente se necesite permitir la modificación de un atributo. Incluir setters indiscriminadamente puede debilitar la encapsulación y permitir cambios que violen las invariantes de clase o que dificulten el mantenimiento del código.

En muchos diseños, especialmente cuando se busca inmutabilidad o control estricto del estado, se evita proporcionar setters. En su lugar, se prefiere que los objetos se construyan completamente desde el principio o que se utilicen métodos que devuelvan nuevas instancias en lugar de modificar las existentes.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable. Esto significa que cada vez que se realiza una concatenación, no se modifica la cadena original, sino que se crea un nuevo objeto String con el resultado. Este comportamiento puede ser costoso si se realizan muchas concatenaciones en un bucle.

Para operaciones que implican concatenar repetidamente, se recomienda utilizar clases mutables como StringBuilder o StringBuffer. 
Estas permiten modificar la cadena sin crear nuevos objetos en cada operación, lo que mejora significativamente el rendimiento en 
construcciones de texto extensas.
## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 
En POO, la comparación de objetos puede hacerse por identidad (si son el mismo objeto en memoria) o por contenido (si representan la misma información). En Java, el operador == compara identidad, mientras que el método equals se utiliza para comparar contenido, siempre que la clase lo haya sobrescrito adecuadamente.

El método equals en Java, por defecto, hereda la implementación de Object, que compara identidad. Para comparar contenido, una
clase debe sobrescribir equals y definir qué significa que dos objetos sean “iguales”. En el caso de las cadenas, String ya
sobrescribe equals, por lo que deben compararse usando cadena1.equals(cadena2).

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper son clases que encapsulan tipos primitivos para tratarlos como objetos.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?
En POO, un tipo de dato enumerado representa un conjunto finito y cerrado de valores posibles. Se utiliza cuando una variable solo puede tomar uno de varios valores predefinidos, como los días de la semana, los meses del año o los estados de un proceso. Esto permite expresar de forma clara y segura que solo existen ciertas opciones válidas, evitando errores como usar valores arbitrarios.

En Java, un enumerado (enum) es realmente una clase especial. Cada valor del enumerado es una instancia única y predefinida de
esa clase. Esto significa que un enum puede tener atributos, métodos y constructores privados, igual que cualquier otra clase,
aunque su sintaxis sea más compacta y su uso más restringido.

En términos de encapsulación, los enumerados ofrecen ventajas importantes. Al ser un conjunto cerrado de instancias, se evita que
el código externo cree nuevos valores no previstos. Además, los atributos internos de cada valor pueden mantenerse privados, y
los métodos públicos pueden controlar cómo se accede a la información. Esto permite representar conceptos complejos de forma
segura y coherente.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

Un enumerado puede definirse con atributos privados y un constructor que inicialice cada instancia. Esto permite asociar información adicional a cada mes, como su número de días y su posición en el año. El constructor debe ser privado, lo cual es obligatorio en los enumerados, ya que solo pueden crearse las instancias definidas en el propio enum.

Aquí tienes un ejemplo completo:

java
public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }
}
Este diseño encapsula los datos de cada mes y expone únicamente los métodos necesarios para consultarlos. El exterior no puede modificar los valores ni crear nuevos meses, lo que garantiza consistencia.


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

Para determinar la estación, basta con comprobar el ordinal del mes y ajustar la lógica según el hemisferio. En el hemisferio norte, las estaciones comienzan aproximadamente en marzo (primavera), junio (verano), septiembre (otoño) y diciembre (invierno). En el hemisferio sur ocurre lo contrario: las estaciones se invierten.

Una forma sencilla de implementarlo es definir las estaciones según rangos de ordinales y, si el hemisferio es sur, intercambiar las estaciones opuestas. Esto mantiene la lógica encapsulada dentro del propio enumerado, sin necesidad de exponer detalles adicionales.

Aquí tienes la ampliación del enum:

java
public boolean esDePrimavera(boolean norte) {
    if (norte) {
        return ordinal >= 3 && ordinal <= 5;
    } else {
        return ordinal >= 9 && ordinal <= 11;
    }
}

public boolean esDeVerano(boolean norte) {
    if (norte) {
        return ordinal >= 6 && ordinal <= 8;
    } else {
        return ordinal >= 12 || ordinal <= 2;
    }
}

public boolean esDeOtoño(boolean norte) {
    if (norte) {
        return ordinal >= 9 && ordinal <= 11;
    } else {
        return ordinal >= 3 && ordinal <= 5;
    }
}

public boolean esDeInvierno(boolean norte) {
    if (norte) {
        return ordinal == 12 || ordinal <= 2;
    } else {
        return ordinal >= 6 && ordinal <= 8;
    }
}
Esta implementación mantiene la encapsulación, ya que las reglas de las estaciones están dentro del propio tipo Mes. El código externo solo necesita llamar a los métodos públicos sin preocuparse por los detalles internos.

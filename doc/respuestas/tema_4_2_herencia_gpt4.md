<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La relación “A es-un B” (is-a) indica que una subclase es un tipo más específico de la superclase.
Ejemplo: Un Artillero es un Soldado.

Compatibilidad de tipos
Un objeto de una subclase puede tratarse como un objeto de la superclase.
Permite el polimorfismo, como almacenar distintos subtipos en una misma colección.
Herencia de estado y comportamiento
La subclase hereda atributos (estado) y métodos (comportamiento) de la superclase.

 Ejemplo 
// Superclase
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }

    public String getNombre() {
        return nombre;
    }
}

// Subclase Artillero
public class Artillero extends Soldado {
    private int numeroCohetes;

    public Artillero(String nombre, int numeroCohetes) {
        super(nombre);
        this.numeroCohetes = numeroCohetes;
    }

    public int getNumeroCohetes() {
        return numeroCohetes;
    }

    public void dispararCohete() {
        System.out.println("¡Disparando cohete!");
    }
}

// Subclase Zapador
public class Zapador extends Soldado {
    private int numeroMinas;

    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
        this.numeroMinas = numeroMinas;
    }

    public int getNumeroMinas() {
        return numeroMinas;
    }

    public void ponerMina() {
        System.out.println("¡Mina colocada!");
    }
}

// Uso de compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        Soldado[] soldados = new Soldado[] {
            new Artillero("Juan", 5),
            new Zapador("Luis", 3),
            new Artillero("Ana", 8)
        };

        for (Soldado s : soldados) {
            s.saludar(); // Polimorfismo
        }
    }
}

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Número de constructores ejecutados:
Se ejecuta uno por cada nivel de la jerarquía, comenzando por la superclase y terminando en la subclase.
Orden de ejecución:
Soldado → Artillero o Zapador.
Significado de super:
super(...) llama al constructor de la superclase.
Debe ser la primera instrucción del constructor de la subclase.
Si la superclase no tiene constructor sin parámetros:
Sí, es obligatorio llamar explícitamente a super, ya que el compilador intenta invocar el constructor por defecto y fallará si no existe.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

public class Zapador extends Soldado {
    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
    }

    public void mostrarNombre() {
        
        System.out.println(getNombre()); // ✅ Acceso mediante getter
    }
}

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

Sí, una referencia del supertipo puede apuntar a un objeto de un subtipo (upcasting).


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Solo se pueden invocar directamente los métodos definidos en el supertipo. Para acceder a métodos específicos se requiere downcasting.

 Definiciones
Upcasting: Conversión implícita de subtipo a supertipo.
Downcasting: Conversión explícita de supertipo a subtipo.
instanceof: Permite verificar el tipo real del objeto.
 Ejemplo
Soldado[] soldados = {
    new Artillero("Juan", 5),
    new Zapador("Luis", 3),
    new Artillero("Ana", 8)
};

for (Soldado s : soldados) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting
        System.out.println("Cohetes: " + a.getNumeroCohetes());
    }
}


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El modificador protected permite el acceso:

Dentro de la misma clase.
En el mismo paquete.
En las subclases, incluso si están en paquetes distintos.
 Ejemplo
public class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Zapador extends Soldado {
    private int numeroMinas;

    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
        this.numeroMinas = numeroMinas;
    }

    public void ponerMina() {
        System.out.println(nombre + " ha colocado una mina.");
    }
}

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

Java: Todas las clases heredan implícitamente de Object.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

¿Existe en Java?
No para clases.
Sí mediante interfaces.
interface Volador {
    void volar();
}

interface Nadador {
    void nadar();
}

class SoldadoEspecial extends Soldado implements Volador, Nadador {
    public SoldadoEspecial(String nombre) {
        super(nombre);
    }

    public void volar() { }
    public void nadar() { }
}


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 
public class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}



## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

Porque la herencia implica una relación “es-un” y crea un fuerte acoplamiento entre clases. Si solo se desea reutilizar funcionalidad sin esa relación conceptual, la herencia puede generar diseños rígidos y difíciles de mantener.



## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

Ventajas:

Mayor flexibilidad.
Menor acoplamiento.
Permite cambiar comportamientos en tiempo de ejecución.
Mejora la reutilización sin comprometer la jerarquía de tipos.



## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

Las subclases dependen de los detalles internos de la superclase. Si la implementación de la superclase cambia, puede afectar a las subclases, incluso sin modificar su interfaz pública, lo que debilita la encapsulación.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.
### Opción 1: Herencia
public class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

public class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

public class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
 Opción 2: Composición
public class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

public class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}

public class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}

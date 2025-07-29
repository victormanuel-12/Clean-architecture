# Clean-architecture
## ¿Por qué preocuparse por la arquitectura?
Porque la arquitectura define la estructura de tu software.

Es como el plano de una casa. Si está mal hecho, la casa se cae. Si tu software no tiene una buena arquitectura:

- Es difícil de entender.

- Es complicado de modificar o escalar.

- Aparecen muchos errores cuando cambias algo.

- Se vuelve lento y costoso de mantener.

Una buena arquitectura te permite tener un sistema organizado, flexible y fácil de mantener.

## ¿Qué es la Arquitectura Limpia?
Es una forma de organizar el código que separa claramente las reglas del negocio (la lógica principal) de los detalles técnicos (como la base de datos, la web, o un framework).

Pone al núcleo del sistema (la lógica del negocio) en el centro.

Todo lo demás (como controladores, base de datos, interfaces gráficas) depende de ese núcleo, no al revés.

Esto hace que el sistema sea más fácil de probar, modificar y entender.

## ¿Qué son los principios SÓLID (SOLID)?
Es un conjunto de 5 principios para escribir buen código mantenible y escalable orientado a objetos. Son:

- S: Principio de responsabilidad única → Una clase debe tener una sola razón para cambiar una sola responsabilidad.
  ¿Cómo saber que una clase debe dividirse?
  - ¿Tiene más de una razón para cambiar? (SRP)
    Si al modificar una cosa (por ejemplo, la lógica de negocio), terminas afectando otra (por ejemplo, el envío de             emails), es hora de dividir.
  🔎 Ejemplo:
    Tienes una clase que guarda usuarios y también envía correos. Si mañana cambia el proveedor de correo, ¿por qué             tendrías que tocar la lógica de guardar usuarios?
    👉 ¡Divídela!
  -  2. ¿Tiene métodos que no están relacionados entre sí?
      Si los métodos no comparten lógica ni propósito claro, probablemente estás mezclando responsabilidades.
En este caso si comparten
```
public void guardarUsuario(Usuario u) { ... }
public void eliminarUsuario(String id) { ... }
public void actualizarUsuario(Usuario u) { ... }
```
Sí comparten una responsabilidad clara: gestionar entidades de tipo Usuario.
Entonces sí deben estar en la misma clase, como por ejemplo un UsuarioService o un UsuarioRepository (según la capa de tu arquitectura).
Esto no comparten una misma responsabilidad
```
public class Utilidades {
    
    public void calcularImpuesto(double monto) {
        System.out.println("Impuesto calculado: " + monto * 0.18);
    }

    public void enviarCorreo(String mensaje) {
        System.out.println("Enviando correo: " + mensaje);
    }

    public void generarReporteExcel() {
        System.out.println("Reporte en Excel generado.");
    }
}
```
- O: Principio Abierto/Cerrado → El código debe estar abierto a extensión, pero cerrado a modificación. ES DECIR
   puedes agregar nuevas funcionalidades sin cambiar el código existente.
  EJEMPLO
  puedeser que se cree una interfaz para generar reporte y se creen implementaciones de esta interfaz para pdf y excel cosa que si se quiere agregar otra clase de generacion de reporte no se va a necesitar modificar lo que ya gay sino solo se agregara la nueva implementacion
  Otro ejemplo es  Imagina una clase CarritoCompras que calcula el total aplicando descuentos según el tipo de cliente: si se agregga otro tipo de cliente se tendria que modificar lo que ya hay
  se haria mejor se crearia una interfaz EstrategiaDescuento
  interface EstrategiaDescuento {
    double aplicarDescuento(double subtotal);

}
  y se haria implementaciones  como por ejemplo
  ```
  class DescuentoRegular implements EstrategiaDescuento {
    public double aplicarDescuento(double subtotal) {
        return subtotal * 0.95;
    }
}
```
  COSA QUE SI SE QUIERE AGREGAR UN NUEVO TIEPO DE DESCUENTO SOLO SE AGREGARIA UNA NUEVA IMPLEMENTACION


- L: Principio de sustitución de Liskov → Las subclases deben poder reemplazar a sus clases padre sin problemas.
  Es decir La subclase debe cumplir con todas las expectativas de comportamiento de la superclase
  No puede relajar precondiciones ni fortalecer postcondiciones
    EJEMPLO
 ```
 class Hotel {
    /**
     * Reserva una habitación
     * @param dias Debe ser >= 1 (precondición)
     * @return Número de confirmación (postcondición: nunca es null)
     */
    String reservar(int dias) {
        if (dias < 1) throw new IllegalArgumentException();
        return "H-" + UUID.randomUUID();
    }

}class HotelEconómico extends Hotel {
    @Override
    String reservar(int dias) {
        // ¡ERROR! Permite dias = 0 (relaja la precondición)
        if (dias < 0) throw new IllegalArgumentException();
        return "E-" + dias; // Además cambia el formato
    }
}
```
  

- I:Principio de segregación de interfaz → No obligues a una clase a implementar métodos que no usa.
  Mejor Divide interfaces grandes en otras más pequeñas y específicas.

 ```
interface IMultifuncional {
    void imprimir();
    void escanear();
    void enviarFax();}
```

```
interface IImpresora {
    void imprimir();
}

interface IEscanner {
    void escanear();
}

interface IFax {
    void enviarFax();
}

// Ahora las clases implementan SOLO lo que necesitan:
class ImpresoraAvanzada implements IImpresora, IEscanner {
    public void imprimir() { /* Lógica */ }
    public void escanear() { /* Lógica */ }
}

class ImpresoraBasica implements IImpresora {
    public void imprimir() { /* Solo imprime */ }
}

class MaquinaOficina implements IImpresora, IEscanner, IFax {
    public void imprimir() { /* Lógica */ }
    public void escanear() { /* Lógica */ }
    public void enviarFax() { /* Lógica */ }
}

class ImpresoraAvanzada implements IMultifuncional {
    public void imprimir() { /* Lógica de impresión */ }
    public void escanear() { /* Lógica de escaneo */ }
    public void enviarFax() { /* ¡Pero esta impresora NO envía fax! */ }
}
```

- D: Principio de inversión de dependencia → Las clases deben depender de abstracciones(interfaz o clases abstractas), no de cosas concretas. como clases
   DONDE APLICARLO
  Cuando una clase depende de servicios externos (APIs, bases de datos, librerías).

  Cuando necesitas cambiar implementaciones fácilmente (ej: desarrollo vs producción).

  Al trabajar con frameworks de inyección de dependencias (Spring, Angular, etc.).
  

Estos principios ayudan a escribir código limpio, flexible y sin errores comunes.

## ¿Qué son los patrones de diseño?
Son soluciones reutilizables a problemas comunes en el desarrollo de software.

Ejemplos conocidos:

- Singleton: Garantiza que una clase solo tenga una instancia.

- Factory: Crea objetos sin especificar exactamente su tipo.

- Observer: Permite que varios objetos "observen" a otro y respondan a sus cambios.

Son como recetas o estrategias que otros programadores ya han probado y funcionan bien.

## ¿Qué es la Programación Orientada a Objetos (POO)?
Es un paradigma de programación que organiza el código en "objetos". Cada objeto representa una entidad del mundo real con sus características (atributos) y acciones (métodos).
Este enfoque ayuda a crear software más modular, reutilizable y fácil de mantener.

### Clase
Una clase es como un molde o una plantilla para crear objetos.
Define qué datos tendrá un objeto (atributos) y qué puede hacer (métodos).
No es un objeto real, sino una descripción de cómo serán los objetos creados a partir de ella.

### Objeto
Un objeto es una instancia de una clase. Es el elemento real que se crea en la memoria y se puede usar.
Cada objeto puede tener valores propios en sus atributos.

### Herencia
La herencia permite que una clase (llamada subclase o clase hija) herede atributos y métodos de otra clase (superclase o clase padre).
Permite reutilizar código y especializar comportamientos.

### Encapsulamiento
El encapsulamiento significa ocultar la información interna de un objeto y controlar el acceso a ella.
Se logra usando modificadores de acceso como private, public, protected.
Promueve la seguridad y la modularidad del código.

### Polimorfismo
El polimorfismo permite que una misma operación se comporte de diferentes formas, dependiendo del objeto que la use.
Puede ser polimorfismo por herencia (sobrescritura) o polimorfismo por sobrecarga (métodos con el mismo nombre pero diferente firma).

## ¿Por qué son importantes los principios SÓLID para una arquitectura limpia?
Porque los principios SÓLID ayudan a que el código siga siendo limpio, modular y fácil de mantener, lo cual es esencial en una Arquitectura Limpia.
En resumen:
SOLID se enfoca en el código de las clases.
Arquitectura Limpia se enfoca en la estructura de todo el sistema.
Pero ambos trabajan juntos para hacer que tu software sea fuerte, escalable y mantenible.

## ¿Cuál es la diferencia entre arquitectura SÓLID y limpia?
SÓLID: Son principios de diseño de clases y objetos dentro del código (más a nivel de detalle).
Arquitectura Limpia: Es una forma de organizar el sistema completo (más a nivel estructural).
👉 SÓLID es una parte importante dentro de una Arquitectura Limpia, pero no son lo mismo.


ESTRUCTURA EN CLEAN ARCHITECTURE
🟢 1. Capa de Dominio (Domain Layer)
No conoce nada de frameworks ni tecnología externa (como Cloudinary o Spring Boot).

Entidad: Curso

Repositorio (Interface): CursoRepository

Curso guardar(Curso curso);

Servicio (Use Case / Interactor): CrearCursoUseCase

Método: Curso ejecutar(CrearCursoRequest request);

🟡 2. Capa de Aplicación (Application Layer)
Coordina lógica y casos de uso, orquesta la interacción entre capas.

DTOs / Request Models:

CrearCursoRequest (nombre, descripción, archivo imagen, etc.)

CursoResponse

Interfaces para servicios externos:

CloudinaryService

String subirImagen(MultipartFile imagen);

🔵 3. Capa de Infraestructura (Infrastructure Layer)
Implementaciones técnicas.

Repositorio de base de datos (implementación):

CursoRepositoryImpl implements CursoRepository

Usa JPA, MongoDB u otro ORM para guardar.

Servicio de Cloudinary:

CloudinaryServiceImpl implements CloudinaryService

Usa el SDK de Cloudinary para subir imágenes.

🔴 4. Capa de Entrada (Interface/Controller Layer)
Puntos de entrada como REST API.

Controlador REST: CursoController

POST /cursos

Recibe formulario con datos e imagen.

Llama a CrearCursoUseCase con un CrearCursoRequest.

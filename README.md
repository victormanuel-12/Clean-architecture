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
Es un conjunto de 5 principios para escribir buen código orientado a objetos. Son:

- S: Single Responsibility Principle → Una clase debe tener una sola razón para cambiar.

- O: Open/Closed Principle → El código debe estar abierto a extensión, pero cerrado a modificación.

- L: Liskov Substitution Principle → Las subclases deben poder reemplazar a sus clases padre sin problemas.

- Interface Segregation Principle → No obligues a una clase a implementar métodos que no usa.

- D: Dependency Inversion Principle → Las clases deben depender de abstracciones, no de cosas concretas.

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

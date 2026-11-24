# TP Final – Sistema de gestión de personal y armamento policial
**Curso de Java Inicial · Tecno3F · Municipalidad de Tres de Febrero**

## Contexto y objetivo
La policía de Tres de Febrero necesita un sistema para administrar su personal, organizado en escuadrones, y las armas que se les asignan. 

Tu objetivo es **diseñar e implementar el modelo de clases en Java** aplicando programación orientada a objetos: encapsulamiento, herencia, clases abstractas, enumeraciones (`enum`), interfaces (`Comparable`), colecciones y manejo de excepciones.

---

## Requerimientos del sistema

### 1. El Escuadrón
Cada escuadrón tiene un nombre, pertenece a una comisaría y administra tanto a su grupo de policías como a su arsenal de armas. El sistema debe permitir:
* Incorporar policías y armas al escuadrón.
* Consultar cuántos integrantes tiene el escuadrón.
* Listar por consola todas las armas del escuadrón indicando quién la tiene asignada (nombre, apellido y legajo) o `"Sin asignar"` si nadie la porta.
* Consultar cuántas armas del escuadrón están en condiciones de ser utilizadas.

### 2. Los Policías
De cada policía se registra su nombre, apellido, número de legajo, el escuadrón al que pertenece y una **única arma asignada**. 
* Al dar de alta un policía en el sistema, todavía no tiene escuadrón ni arma asignada.
* Debe existir una forma de asignarle un arma, asegurando que **ambos objetos queden vinculados entre sí**.
* **Regla de negocio:** Un policía no puede tener más de un arma, ni un arma puede asignarse a dos policías a la vez. Si se intenta realizar una asignación inválida, el sistema debe impedirlo (ya sea rechazando la operación con un mensaje o lanzando una excepción; documentá tu decisión).

### 3. El Armamento
Todas las armas registradas comparten: marca, calibre (expresado en milímetros, ej. `9.0`), capacidad de municiones, alcance efectivo en metros, un estado actual y el policía que la tiene asignada (que puede ser nulo si está en depósito).

* **Estados posibles:** Un arma únicamente puede encontrarse *nueva*, *en mantenimiento* o *en uso*.
* **Condición de uso:** Cualquier arma se considera **en condiciones** únicamente si su estado es *en uso* y su calibre es de al menos 9 mm.
* En el sistema no deben poder instanciarse armas genéricas; solo existen dos tipos específicos:
  * **Armas Cortas:** Además de los datos generales, interesa saber si son automáticas o no. Deben poder informar si son capaces de disparar a más de 200 metros.
  * **Armas Largas:** Poseen un indicador de si tienen sello del RENAR, una descripción que justifica su uso y un **nivel de potencia/uso que va del 1 al 5** (si se intenta crear un arma con un nivel fuera de ese rango, el sistema debe lanzar un error). Además, las armas largas deben poder **compararse entre sí** según su nivel (a mayor nivel, mayor es el arma).

---

## Pruebas requeridas (`Main`)
Creá una clase ejecutable que demuestre el funcionamiento completo del sistema. Como mínimo debe:
1. Crear un escuadrón con al menos 4 policías.
2. Instanciar 2 armas cortas y 2 armas largas con distintos estados y calibres.
3. Vincular policías y armas al escuadrón, dejando **al menos un arma sin asignar**.
4. Mostrar por consola:
   * Cantidad de integrantes del escuadrón.
   * Listado completo de armas y sus portadores.
   * Cantidad de armas en condiciones de ser usadas.
   * Evaluación de alcance (> 200 m) para las armas cortas.
   * Comparación entre dos armas largas (indicando cuál tiene mayor nivel o si son iguales).
5. Probar al menos **dos casos de error o restricción** (por ejemplo: intentar crear un arma larga con nivel 7, o intentar asignar un arma que ya tiene dueño) sin que el programa se interrumpa abruptamente (`try-catch`).

---

## 💡 Pistas y ayudas para el diseño
Si te trabás en algún punto, revisá estas recomendaciones antes de consultar:

* **Tipos de datos:** Usá un `enum` para los estados del arma en lugar de `String` para evitar errores de tipeo. Para el calibre, recordá que existen calibres decimales (como `7.62` o `11.25`).
* **Relaciones bidireccionales:** Cuando un policía recibe un arma (`policia.asignarArma(arma)`), no alcanza con guardar el arma dentro del policía; ese mismo método debería avisarle al arma quién es su nuevo policía. Lo mismo ocurre al agregar un policía a un escuadrón.
* **Cuidado con el `toString()`:** Si el `toString()` de `Policia` imprime su `Arma`, y el `toString()` de `Arma` imprime su `Policia`, al imprimir cualquiera de los dos se producirá un bucle infinito (`StackOverflowError`). Hacé que el arma muestre solo los datos básicos del policía (nombre, apellido y legajo) en lugar de llamar al `toString()` completo del objeto.
* **Validación en el constructor:** Para validar el rango del nivel en `ArmaLarga` (1 a 5), podés lanzar `new IllegalArgumentException("Mensaje de error")`.
* **Comparación de objetos:** Para comparar armas largas de forma estándar en Java, implementá la interfaz `Comparable<ArmaLarga>` y sobreescribí el método `compareTo`.

---

## Checklist antes de entregar
* [ ] Todos los atributos son `private` y se accede a ellos mediante getters/setters o métodos de negocio.
* [ ] No se repite código entre `ArmaCorta` y `ArmaLarga` (se aprovecha la herencia y `super(...)`).
* [ ] El método que verifica si el arma está en condiciones está escrito una sola vez en la clase padre.
* [ ] El proyecto compila y ejecuta todas las pruebas pedidas en el `Main`.

**Entrega:** Proyecto comprimido (.zip) con el código fuente. *(Opcional: diagrama de clases UML en imagen o PDF).*

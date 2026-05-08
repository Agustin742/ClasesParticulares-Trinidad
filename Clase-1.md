# Que es un Pardigma
es un modelo, teorías y métodos compartidos por una comunidad que define cómo se entiende, interpreta y actúa sobre la realidad. Actúa como una guía aceptada que dicta qué se debe investigar y cómo, marcando la visión del mundo en un tiempo determinado

## 1. Concepto: Clase vs Objeto

### ¿Qué es una Clase?

Una **clase** es una estructura que define un tipo de objeto.  
En ella se especifica:

- Atributos (datos)
- Métodos (comportamientos)

Es un modelo o plantilla que describe cómo serán los objetos.

---

### ¿Qué es un Objeto?

Un **objeto** es una instancia de una clase.  
Es una representación concreta basada en esa plantilla.

Cada objeto tiene:

- Sus propios valores en los atributos
- Acceso a los métodos definidos en la clase|

---

### Relación entre Clase y Objeto

- La clase define
- El objeto se crea a partir de la clase

```text
Clase → Objeto
Auto  → auto1, auto2
```

## 2. Atributos y Métodos

### ¿Qué es un Atributo?

Un **atributo** es una variable que pertenece a una clase.  
Representa las características o datos de un objeto.

Ejemplos de atributos:

- nombre
- edad
- precio
- marca

---

### ¿Qué es un Método?

Un **método** es una función definida dentro de una clase.  
Representa las acciones o comportamientos que puede realizar un objeto.

Ejemplos de métodos:

- saludar()
- calcularTotal()
- arrancar()

---

### Diferencia entre Atributos y Métodos

| Atributos           | Métodos                    |
| ------------------- | -------------------------- |
| Guardan información | Definen acciones           |
| Son variables       | Son funciones              |
| Representan estado  | Representan comportamiento |

---

### Ejemplo en TypeScript

```ts
class Persona {
  nombre: string;
  edad: number;

  saludar() {
    console.log("Hola, soy " + this.nombre);
  }
}
```
### Uso de this

**this** hace referencia al objeto actual.
Permite acceder a sus propios atributos dentro de un método.

```ts
saludar() {
  console.log("Hola, soy " + this.nombre);
}
```

#### Ejemplo Aplicado de this

```ts
class Producto {
  nombre: string;
  precio: number;

  mostrarInfo() {
    console.log(this.nombre + " cuesta $" + this.precio);
  }
}

const producto1 = new Producto();
producto1.nombre = "Mouse";
producto1.precio = 1500;

producto1.mostrarInfo();"
```

La idea general es:

```ts
Atributos → qué es el objeto
Métodos → qué hace el objeto
```

## Visibilidad de los miembros

La **visibilidad** define desde dónde se pueden acceder a los atributos y métodos de una clase.

En TypeScript existen tres niveles:

---

### public

Los miembros **public** pueden ser accedidos desde cualquier parte del programa.

```ts
class Persona {
  public nombre: string;

  public saludar() {
    console.log("Hola " + this.nombre);
  }
}

const p = new Persona();
p.nombre = "Ana";
p.saludar();
```

### private

Los miembros private solo pueden ser accedidos dentro de la misma clase.

```ts
class Cuenta {
  private saldo: number;

  public mostrarSaldo() {
    console.log(this.saldo);
  }
}

const c = new Cuenta();
c.saldo = 1000; //Error
```

### protected

Los miembros protected pueden ser accedidos dentro de la clase y por clases que heredan de ella.

```ts
class Animal {
  protected nombre: string;
}

class Perro extends Animal {
  mostrarNombre() {
    console.log(this.nombre);
  }
}
```

### Ejemplo combinado

```ts
class Usuario {
  public nombre: string;
  private contraseña: string;

  public login(pass: string) {
    if (pass === this.contraseña) {
      console.log("Acceso correcto");
    }
  }
}
```

### Idea clave

```text
public → acceso desde cualquier lugar
private → solo dentro de la clase
protected → clase y clases hijas
```

## Getters y Setters

Los **getters** y **setters** son métodos especiales que permiten **leer** y **modificar** atributos de una clase de forma controlada.

Se usan principalmente cuando los atributos son `private`.

---

### ¿Por qué usarlos?

- Controlar cómo se accede a los datos
- Validar valores antes de asignarlos
- Proteger la información interna del objeto

---

### Ejemplo básico

```ts
class Persona {
  private nombre: string;

  setNombre(nuevoNombre: string) {
    this.nombre = nuevoNombre;
  }

  getNombre() {
    return this.nombre;
  }
}

const p = new Persona();
p.setNombre("Ana");

console.log(p.getNombre()); 
```



## 3. Uso de Objetos

### Creación de objetos (instanciación)

Para usar una clase, es necesario crear objetos a partir de ella.  
Esto se hace utilizando la palabra clave `new`.

```ts
class Auto {
  marca: string;
  modelo: string;

  arrancar() {
    console.log("El auto arranca");
  }
}

const auto1 = new Auto();
const auto2 = new Auto();
```

### Asignación de valores

Cada objeto puede tener valores propios en sus atributos:

```ts
auto1.marca = "Toyota";
auto1.modelo = "Corolla";

auto2.marca = "Ford";
auto2.modelo = "Fiesta";
```

### Uso de métodos

Los objetos pueden ejecutar los métodos definidos en la clase:

```ts
auto1.arrancar(); 
auto2.arrancar(); 
```

### Independencia entre objetos

Aunque provienen de la misma clase, cada objeto mantiene su propio estado:

```ts
console.log(auto1.marca); // "Toyota"
console.log(auto2.marca); // "Ford"
```

### Idea clave

```ts
Cada objeto es independiente, pero comparte la misma estructura definida por la clase
```

## 4. Ejercicio guiado

### Consigna

Crear una clase `Producto` que tenga:

- Atributos:
  - nombre
  - precio
- Método:
  - mostrarInfo()

---

### Paso 1: Definir la clase

### Paso 2: Crear un objeto

### Paso 3: Asignar valores

### Paso 4: Usar el método

### Paso 5: Crear otro producto convalores diferentes

## 5. Cierre

### Repaso de conceptos

- Clase → estructura que define objetos
- Objeto → instancia de una clase
- Atributos → datos del objeto
- Métodos → acciones del objeto

---

### Ejemplo de repaso

```ts
class Producto {
  nombre: string;
  precio: number;

  mostrarInfo() {
    console.log(this.nombre + " cuesta $" + this.precio);
  }
}

const producto1 = new Producto();
producto1.nombre = "Mouse";
producto1.precio = 3000;

producto1.mostrarInfo();
````

## 🧪 Tarea

### Ejercicio 1

Crear una clase `CuentaBancaria` que represente una cuenta.

Debe tener:

- Atributos:
  - titular
  - saldo

- Métodos:
  - depositar(monto)
  - retirar(monto)

---

### Ejercicio 2

Resolver la siguiente situación de dos formas:

“Se necesitan representar 2 productos con nombre y precio”

#### Forma 1
Usar variables sueltas para guardar la información.

#### Forma 2
Crear una clase `Producto` y representar los productos usando objetos.

### Desarrollar un pequeño escrito con las diferencias entre una y otra, y el por que usarias una u otra en la vida real.

---

### Ejercicio 3

Crear una clase `Usuario` que tenga:

- Atributos:
  - nombre (public)
  - email (public)
  - contraseña (private)

- Métodos:
  - mostrarInfo() → debe mostrar nombre y email
  - login(pass) → debe validar la contraseña

- Requisito:
  - No se debe poder acceder directamente a la contraseña desde fuera de la clase

---

### Ejercicio 4

Crear una clase `Frutas` utilizando:

- Atributos:
  - nombre (public)
  - precio (private)

- Métodos:
  - setter para modificar el precio
  - getter para obtener el precio
  - mostrarInfo()

- Requisito:
  - El precio no puede ser menor o igual a 0 (validar en el setter)

---

### Extra

Explicar en un breve texto:

- ¿Por qué usar `private` en algunos atributos?
- ¿Para qué sirven los getters y setters?
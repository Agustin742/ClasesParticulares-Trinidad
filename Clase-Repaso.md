## Clase de repaso — Objetos, encapsulamiento y herencia

Pasó bastante desde las clases 1 y 2, así que hoy no arrancamos de cero: arrancamos **sacando de tu cabeza** lo que ya está ahí.

La clase es a preguntas. Yo pregunto, vos contestás, y **recién después** confirmamos. Si algo no sale, no importa: eso es exactamente lo que vinimos a encontrar.

---

## Checkpoint — la pregunta de siempre

**Pregunta:** ¿Qué es una **clase** y qué es un **objeto**? Decilo con tus palabras, no con la definición del apunte.

Y después: en este código, **¿cuántas clases hay y cuántos objetos hay?**

```typescript
class Auto {
  marca: string;
}

const auto1 = new Auto();
const auto2 = new Auto();
const auto3 = new Auto();
```

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Una clase (`Auto`) y tres objetos** (`auto1`, `auto2`, `auto3`).
>
> - La **clase** es el **molde**. Define qué van a tener los autos: una marca. No es un auto. No podés manejarla.
> - El **objeto** es lo que sale del molde. Es una cosa concreta, que ocupa lugar en la memoria y tiene **sus propios valores**.
>
> Cuando escribís `new Auto()` estás **instanciando**: fabricando un auto a partir del plano.
>
> Un plano de edificio, mil edificios construidos. El plano es uno solo.

---

## Clase y objeto

> [IMAGEN: a la izquierda, un plano de arquitecto (plano técnico, líneas, medidas) rotulado "CLASE — Casa". Una flecha grande hacia la derecha, etiquetada "new". A la derecha, tres casas construidas, distintas entre sí (una roja de dos pisos, una azul de un piso, una blanca con techo a dos aguas), rotuladas "OBJETOS — casa1, casa2, casa3". Debajo del plano: "define". Debajo de las casas: "existen".]

```
Clase → define        Auto  →  la estructura
Objeto → existe       auto1, auto2, auto3  →  cada uno con sus valores
```

Cada objeto es **independiente**, aunque compartan la misma estructura.

---

## Atributos y métodos

| Atributos | Métodos |
|---|---|
| Guardan información | Definen acciones |
| Son variables | Son funciones |
| Representan el **estado** | Representan el **comportamiento** |
| `nombre`, `precio`, `saldo` | `saludar()`, `depositar()`, `arrancar()` |

La regla mental:

```
Atributos → qué ES el objeto
Métodos   → qué HACE el objeto
```

---

## Predecí — ¿qué imprime?

```typescript
class Auto {
  marca: string;

  arrancar(): void {
    console.log(`El ${this.marca} arranca`);
  }
}

const auto1 = new Auto();
const auto2 = new Auto();

auto1.marca = "Toyota";
auto2.marca = "Ford";

auto1.marca = "Fiat";

auto1.arrancar();
auto2.arrancar();
```

Sin ejecutarlo: **¿qué sale por consola?**

---

## Predecí — la respuesta

```
El Fiat arranca
El Ford arranca
```

**Por qué:** `auto1` y `auto2` son objetos **independientes**. Cambiarle la marca a uno **no toca** al otro, aunque salgan de la misma clase.

Cada objeto tiene su propio espacio en memoria con sus propios valores. Comparten la **estructura**, no los **datos**.

Si esto te resultó obvio, perfecto. Es la base de todo lo que viene: cuando más adelante tengas un array de 50 animales, cada uno va a tener su propio nombre y su propia energía, y vas a poder confiar en eso.

---

## El constructor

Asignar los valores a mano, uno por uno, es tedioso y peligroso: te podés **olvidar** de alguno.

```typescript
const auto1 = new Auto();
auto1.marca = "Toyota";
// ...y si me olvido del modelo, el auto queda a medio construir
```

El **constructor** es el método que se ejecuta **automáticamente** con `new`. Es el lugar donde el objeto nace **completo**.

```typescript
class Persona {
  nombre: string;
  edad: number;

  constructor(nombre: string, edad: number) {
    this.nombre = nombre;   // mi atributo = el parámetro que llegó
    this.edad = edad;
  }

  saludar(): void {
    console.log(`Hola, soy ${this.nombre}`);
  }
}

const p = new Persona("Trinidad", 22);   // imposible olvidarse: los pide
p.saludar();
```

> **Atajo de TypeScript:** `constructor(private nombre: string) {}` crea el atributo y lo asigna solo.

---

## Pará y pensá — `this.nombre` vs `nombre`

En esta línea del constructor hay **dos cosas distintas** que se llaman igual:

```typescript
constructor(nombre: string) {
  this.nombre = nombre;
}
```

**Pregunta:** ¿qué es cada una? ¿Y qué pasaría si escribís `nombre = nombre;` (sin el `this`)?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> - **`this.nombre`** (izquierda) → el **atributo del objeto**. El que va a vivir adentro de la instancia.
> - **`nombre`** (derecha) → el **parámetro** del constructor. Un dato de paso, que existe solo mientras corre el constructor y después desaparece.
>
> `this` significa **"yo mismo"**: es la forma que tiene el objeto de hablar de sí mismo. `this.nombre` es *mi* nombre; `nombre` a secas es el que me acaban de pasar.
>
> **Si escribís `nombre = nombre;`** no pasa nada útil: le estás asignando el parámetro a sí mismo. El atributo del objeto **nunca se toca** y queda `undefined`.
>
> Es un bug clásico y silencioso: no rompe, no avisa. Simplemente después imprimís `Hola, soy undefined`.

---

## Ejercicio en vivo 1 — la clase Producto

**Consigna:** escribí una clase `Producto` con:

- Atributos: `nombre` y `precio`
- Un **constructor** que reciba los dos
- Un método `mostrarInfo()` que imprima `"Mouse cuesta $3000"`

Después creá **dos** productos distintos y mostrá la info de cada uno.

---

## Ejercicio en vivo 1 — Resolución

```typescript
class Producto {
  nombre: string;
  precio: number;

  constructor(nombre: string, precio: number) {
    this.nombre = nombre;
    this.precio = precio;
  }

  mostrarInfo(): void {
    console.log(`${this.nombre} cuesta $${this.precio}`);
  }
}

const producto1 = new Producto("Mouse", 3000);
const producto2 = new Producto("Teclado", 12000);

producto1.mostrarInfo();   // Mouse cuesta $3000
producto2.mostrarInfo();   // Teclado cuesta $12000
```

**Fijate lo que ganaste con el constructor:** es **imposible** crear un producto sin precio. El compilador te obliga a pasárselo.

Sin constructor, `new Producto()` te daba un producto con `nombre` y `precio` en `undefined`, y recién te enterabas cuando imprimías `undefined cuesta $undefined`.

**Cada vez que puedas mover un error de "runtime" a "no compila", hacelo.** Es el hilo conductor de toda la materia.

---

## Encapsulamiento — el problema

Ahora mirá esto:

```typescript
class CuentaBancaria {
  titular: string;
  saldo: number;

  constructor(titular: string, saldoInicial: number) {
    this.titular = titular;
    this.saldo = saldoInicial;
  }

  depositar(monto: number): void {
    this.saldo += monto;
  }
}

const cuenta = new CuentaBancaria("Trinidad", 1000);
```

La clase está bien escrita. Tiene constructor, tiene método. Y sin embargo…

---

## Razoná — el agujero

Con la clase de la slide anterior, alguien puede escribir esto:

```typescript
cuenta.saldo = -99999;
cuenta.saldo = 999999999;
cuenta.titular = "Otro";
```

**Preguntas:**
1. ¿Por qué el lenguaje se lo permite?
2. ¿De qué sirvió el método `depositar()` si podés tocar el saldo directamente?
3. ¿Cómo lo impedirías?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **1.** Porque los atributos son **públicos** por defecto. Si no decís nada, cualquiera desde afuera puede leerlos **y escribirlos**.
>
> **2.** De nada. `depositar()` era la **puerta**, pero dejaste la **ventana abierta**. Toda la lógica y las validaciones que pongas adentro del método son inútiles si el atributo se puede modificar por el costado.
>
> **3.** Haciendo el atributo **`private`**. Ahí el saldo deja de ser tocable desde afuera, y la **única** forma de modificarlo pasa a ser los métodos que vos escribiste — que sí validan.
>
> Eso es el **encapsulamiento**: no es "esconder por esconder". Es **elegir qué puertas dejás abiertas**, para poder controlar lo que entra por ellas.
>
> La pregunta que te tenés que hacer al diseñar cualquier clase:
> **¿qué debería poder hacer alguien que use esta clase desde afuera?** Todo lo demás, privado.

---

## Los tres niveles de visibilidad

| Modificador | ¿Quién puede acceder? |
|---|---|
| `public` | Cualquiera. Es el default si no ponés nada. |
| `private` | **Solo** la propia clase. |
| `protected` | La clase **y sus subclases**. |

> [IMAGEN: tres círculos concéntricos. El círculo interior, más chico y sólido, rotulado "private — solo yo". El del medio, rotulado "protected — yo y mis hijas". El exterior, más grande, rotulado "public — todo el mundo". Fuera de los tres, una figurita de persona con una flecha que rebota contra el borde de "private" y entra sin problema al de "public".]

**Convención:** los atributos van `private` (o `protected`) salvo que tengas una razón para lo contrario. **Empezás cerrando, y abrís solo lo que hace falta.**

---

## Getters y setters

Si el atributo es privado, ¿cómo lo leo y lo modifico desde afuera? Con **métodos públicos** que vos controlás.

```typescript
class Producto {
  private nombre: string;
  private precio: number;

  constructor(nombre: string, precio: number) {
    this.nombre = nombre;
    this.setPrecio(precio);      // uso el setter para validar desde el arranque
  }

  // GETTER: leer
  getPrecio(): number {
    return this.precio;
  }

  // SETTER: escribir, pero con reglas
  setPrecio(nuevoPrecio: number): void {
    if (nuevoPrecio <= 0) {
      console.log("El precio debe ser mayor a 0");
      return;                     // no asigna nada: el objeto queda intacto
    }
    this.precio = nuevoPrecio;
  }
}
```

**El setter no es un trámite.** Un setter que solo hace `this.precio = precio` no sirve para nada: es un atributo público con pasos extra. **El setter existe para validar.**

---

## Ejercicio en vivo 2 — CuentaBancaria blindada

**Consigna:** reescribí la `CuentaBancaria` para que `cuenta.saldo = -99999` sea **imposible**.

- `titular` y `saldo`: privados
- Un getter para el saldo
- **Ningún setter de saldo.** El saldo solo se mueve con:
  - `depositar(monto)` → el monto debe ser positivo
  - `extraer(monto)` → el monto debe ser positivo **y** no puede superar el saldo

---

## Ejercicio en vivo 2 — Resolución

```typescript
class CuentaBancaria {
  private titular: string;
  private saldo: number;

  constructor(titular: string, saldoInicial: number) {
    this.titular = titular;
    this.saldo = 0;
    this.depositar(saldoInicial);   // ni siquiera el constructor se saltea la validación
  }

  getSaldo(): number {
    return this.saldo;
  }

  // No hay setSaldo(). A propósito.

  depositar(monto: number): void {
    if (monto <= 0) {
      console.log("El monto debe ser positivo.");
      return;
    }
    this.saldo += monto;
  }

  extraer(monto: number): void {
    if (monto <= 0) {
      console.log("El monto debe ser positivo.");
      return;
    }
    if (monto > this.saldo) {
      console.log("Saldo insuficiente.");
      return;
    }
    this.saldo -= monto;
  }

  mostrarEstado(): void {
    console.log(`Titular: ${this.titular} | Saldo: $${this.saldo}`);
  }
}

const cuenta = new CuentaBancaria("Trinidad", 1000);
cuenta.depositar(500);
cuenta.extraer(200);
cuenta.mostrarEstado();          // Titular: Trinidad | Saldo: $1300
// cuenta.saldo = -99999;        ← ❌ Error: 'saldo' is private
```

**Dos decisiones para mirar con atención:**

1. **No hay `setSaldo()`.** No es un olvido. El saldo **no es algo que se setea**: es algo que **resulta** de depositar y extraer. Un setter lo convertiría otra vez en un atributo público con más pasos.

2. **El constructor llama a `depositar()`** en vez de asignar directo. Así, si alguien intenta abrir una cuenta con saldo inicial `-500`, la validación **también** lo frena. Ni siquiera el constructor tiene permiso de saltearse las reglas de la clase.

---

## Herencia — el problema que resuelve

Mirá estas dos clases:

```typescript
class Perro {
  private nombre: string;
  private energia: number = 100;

  comer(): void   { this.energia += 20; }
  dormir(): void  { this.energia += 30; }
  ladrar(): void  { console.log("Guau"); }
}

class Gato {
  private nombre: string;
  private energia: number = 100;

  comer(): void   { this.energia += 20; }
  dormir(): void  { this.energia += 30; }
  maullar(): void { console.log("Miau"); }
}
```

`comer()` y `dormir()` están escritos **dos veces, idénticos**. Si mañana comer suma 25 en vez de 20, tenés que acordarte de cambiarlo en los dos lados. Y no te vas a acordar.

---

## `extends` y `super()`

La **herencia** te deja escribir lo común **una sola vez**, en una clase padre.

```typescript
class Animal {
  protected nombre: string;      // protected: las hijas lo necesitan
  protected energia: number;

  constructor(nombre: string) {
    this.nombre = nombre;
    this.energia = 100;
  }

  comer(): void  { this.energia += 20; }
  dormir(): void { this.energia += 30; }
}

class Perro extends Animal {
  private raza: string;

  constructor(nombre: string, raza: string) {
    super(nombre);        // ← OBLIGATORIO y PRIMERO: inicializá al padre
    this.raza = raza;     // ← después, lo mío
  }

  ladrar(): void { console.log(`${this.nombre}: Guau`); }
}

const perro = new Perro("Fido", "Labrador");
perro.comer();     // heredado de Animal
perro.ladrar();    // propio de Perro
```

`super(nombre)` es el llamado al constructor del padre. **Va primero, siempre.** El padre se inicializa antes que la hija.

---

## Checkpoint — ¿"es un" o "tiene un"?

La herencia se usa cuando podés decir **"B es un A"**. Si no podés, la herencia es la herramienta equivocada.

**Para cada par, decidí si corresponde herencia:**

1. `Perro` y `Animal`
2. `Auto` y `Motor`
3. `Gerente` y `Empleado`
4. `Persona` y `Dirección`
5. `CajaDeAhorro` y `Cuenta`

> [RESPUESTA: ocultar, permitir ver con un click]
>
> | Par | ¿Herencia? | El test de la frase |
> |---|---|---|
> | Perro / Animal | ✅ Sí | "Un perro **es un** animal" ✔ |
> | Auto / Motor | ❌ No | "Un auto **es un** motor" ✘ — un auto **TIENE** un motor |
> | Gerente / Empleado | ✅ Sí | "Un gerente **es un** empleado" ✔ |
> | Persona / Dirección | ❌ No | "Una persona **es una** dirección" ✘ — la **TIENE** |
> | CajaDeAhorro / Cuenta | ✅ Sí | "Una caja de ahorro **es una** cuenta" ✔ |
>
> **El error clásico:** heredar solo para **reutilizar código**, sin que haya relación lógica. Si `Auto extends Motor` te "ahorra" escribir `encender()`, igual está mal: el día que necesites un auto con dos motores, o un motor sin auto, todo el diseño se cae.
>
> Los casos de "tiene un" se resuelven con otra herramienta, que vemos más adelante.

---

## Ejercicio en vivo 3 — jerarquía de vehículos

**Consigna:** armá esta jerarquía.

- `Vehiculo`: `marca` y `velocidadMaxima`, más un método `describirse()`
- `Auto extends Vehiculo`: agrega `cantidadPuertas`
- `Moto extends Vehiculo`: agrega `tieneSidecar: boolean`

Cada subclase con su propio constructor, que llame a `super()`.

**Antes de escribir**, contestá: ¿`marca` va `private` o `protected`? ¿Por qué?

---

## Ejercicio en vivo 3 — Resolución

```typescript
class Vehiculo {
  protected marca: string;              // protected, NO private
  protected velocidadMaxima: number;

  constructor(marca: string, velocidadMaxima: number) {
    this.marca = marca;
    this.velocidadMaxima = velocidadMaxima;
  }

  describirse(): void {
    console.log(`${this.marca} — máx ${this.velocidadMaxima} km/h`);
  }
}

class Auto extends Vehiculo {
  private cantidadPuertas: number;

  constructor(marca: string, velocidadMaxima: number, cantidadPuertas: number) {
    super(marca, velocidadMaxima);
    this.cantidadPuertas = cantidadPuertas;
  }
}

class Moto extends Vehiculo {
  private tieneSidecar: boolean;

  constructor(marca: string, velocidadMaxima: number, tieneSidecar: boolean) {
    super(marca, velocidadMaxima);
    this.tieneSidecar = tieneSidecar;
  }
}

new Auto("Toyota", 180, 4).describirse();   // Toyota — máx 180 km/h
new Moto("Honda", 160, false).describirse();
```

**La pregunta de `private` vs `protected`:**

Va **`protected`**. Si `marca` fuera `private`, **solo `Vehiculo`** podría leerla — ni `Auto` ni `Moto` podrían tocarla.

En este ejemplo puntual zafás igual, porque `describirse()` está en el padre y es el padre el que la lee. Pero en cuanto `Auto` quiera hacer algo con `this.marca`, `private` te frena.

**La regla:** `private` mientras alcance. `protected` cuando las hijas realmente lo necesiten. **Nunca `public` por defecto.**

---

## Ejercicio en vivo 4 — el bug de visibilidad

**Consigna:** este código **no compila**. Encontrá el error, explicá **por qué** y arreglalo.

```typescript
class Empleado {
  private nombre: string;
  private sueldo: number;

  constructor(nombre: string, sueldo: number) {
    this.nombre = nombre;
    this.sueldo = sueldo;
  }
}

class Gerente extends Empleado {
  private bono: number;

  constructor(nombre: string, sueldo: number, bono: number) {
    super(nombre, sueldo);
    this.bono = bono;
  }

  mostrarSueldoTotal(): void {
    console.log(`${this.nombre} cobra ${this.sueldo + this.bono}`);
  }
}
```

---

## Ejercicio en vivo 4 — Resolución

**El error:** `Gerente` intenta leer `this.nombre` y `this.sueldo`, pero en `Empleado` están declarados **`private`**.

```
Property 'nombre' is private and only accessible within class 'Empleado'.
```

`private` significa **"solo yo"** — y "yo" es **la clase**, no la familia. Una subclase **no** es "yo". Aunque `Gerente` herede de `Empleado`, no tiene permiso para meter la mano en sus atributos privados.

**El arreglo:** `protected`, que es exactamente "yo **y mis hijas**".

```typescript
class Empleado {
  protected nombre: string;    // ✅
  protected sueldo: number;    // ✅

  constructor(nombre: string, sueldo: number) {
    this.nombre = nombre;
    this.sueldo = sueldo;
  }
}
```

**Y ojo con la conclusión fácil.** La lección **no** es "poné todo `protected` por las dudas". `protected` sigue siendo una puerta: se la estás abriendo a todas las subclases presentes y futuras.

La lección es: **`private` por defecto, y lo abrís a `protected` cuando el compilador te demuestra que hace falta** — como acaba de pasar acá. Que el código te pida permiso, no que vos se lo regales por adelantado.

---

## Recap — lo de hoy

| Concepto | En una línea |
|---|---|
| **Clase / Objeto** | El molde y la cosa. Una clase, muchos objetos independientes. |
| **Atributos / Métodos** | Qué **es** el objeto / qué **hace** el objeto. |
| **`this`** | "Yo mismo". Distingue el atributo del parámetro. |
| **Constructor** | El objeto nace **completo**. Imposible olvidarse un dato. |
| **Encapsulamiento** | Cerrás las ventanas para que todos entren por la puerta. |
| **Getter / Setter** | Un setter sin validación no sirve para nada. |
| **Herencia** | Lo común, escrito **una sola vez**. Solo si vale el "es un". |
| **`super()`** | El padre se inicializa primero. Siempre. |

**El hilo que las conecta a todas:** cada una de estas herramientas existe para que **el compilador te frene antes** de que el error llegue a ejecutarse.

---

## Después de clase

Seis ejercicios. **Intentá cada uno antes de abrir la respuesta**: si mirás primero, no sirvió de nada.

---

## Después de clase — Ejercicio 1: Usuario con contraseña

**Consigna:** creá una clase `Usuario` con:

- `nombre` y `email` → públicos
- `contrasenia` → **privada**
- `mostrarInfo()` → muestra nombre y email (nunca la contraseña)
- `login(pass)` → devuelve `true` si la contraseña es correcta

**Requisito:** debe ser **imposible** leer la contraseña desde afuera de la clase.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> ```typescript
> class Usuario {
>   public nombre: string;
>   public email: string;
>   private contrasenia: string;
>
>   constructor(nombre: string, email: string, contrasenia: string) {
>     this.nombre = nombre;
>     this.email = email;
>     this.contrasenia = contrasenia;
>   }
>
>   mostrarInfo(): void {
>     console.log(`${this.nombre} — ${this.email}`);
>   }
>
>   login(pass: string): boolean {
>     if (pass === this.contrasenia) {
>       console.log("Acceso correcto");
>       return true;
>     }
>     console.log("Contraseña incorrecta");
>     return false;
>   }
> }
>
> const u = new Usuario("Trini", "trini@mail.com", "1234");
> u.login("mal");     // Contraseña incorrecta
> u.login("1234");    // Acceso correcto
> // console.log(u.contrasenia);  ← ❌ Error: 'contrasenia' is private
> ```
>
> **Fijate que no hay `getContrasenia()`.** Y no es un olvido.
>
> Un getter de contraseña **anularía todo el punto** del `private`: volvés a poder leerla desde afuera, solo que con dos pasos más. Si el dato no debe salir, **no le pongas puerta de salida**.
>
> El objeto no te **devuelve** la contraseña: te **contesta una pregunta sobre ella** (`¿esta que te paso es la correcta?`). La contraseña nunca sale de la clase.
>
> Esa distinción —*pedirle un dato* vs *pedirle una respuesta*— es una de las ideas más útiles del encapsulamiento.

---

## Después de clase — Ejercicio 2: Termostato

**Consigna:** creá una clase `Termostato` con:

- `temperatura` → **privada**, arranca en 22
- `getTemperatura()`
- `subir()` y `bajar()` → mueven la temperatura de a 1 grado
- **La temperatura nunca puede salir del rango 15–30.** Si se intenta pasar, se avisa y no se cambia nada.

**No debe existir un setter directo de temperatura.**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> ```typescript
> class Termostato {
>   private temperatura: number = 22;
>
>   private readonly MINIMA: number = 15;
>   private readonly MAXIMA: number = 30;
>
>   getTemperatura(): number {
>     return this.temperatura;
>   }
>
>   subir(): void {
>     if (this.temperatura >= this.MAXIMA) {
>       console.log(`No se puede pasar de ${this.MAXIMA}°`);
>       return;
>     }
>     this.temperatura++;
>   }
>
>   bajar(): void {
>     if (this.temperatura <= this.MINIMA) {
>       console.log(`No se puede bajar de ${this.MINIMA}°`);
>       return;
>     }
>     this.temperatura--;
>   }
> }
> ```
>
> **Por qué no hay setter:** con un `setTemperatura(t)` alguien podría poner 500 grados de un saque. Con `subir()` y `bajar()`, el objeto **controla su propio rango** y es imposible sacarlo de ahí.
>
> Es la misma idea que el saldo de la cuenta bancaria: **la temperatura no se setea, se mueve.** Y quien decide cuánto se puede mover es la clase, no el de afuera.
>
> Los `MINIMA` y `MAXIMA` como constantes privadas son un extra: sacan los "números mágicos" del medio del código y le ponen nombre a la regla.

---

## Después de clase — Ejercicio 3: Detectá los errores

**Consigna:** este código tiene **al menos 4 problemas**. Encontralos, explicá **por qué** son problemas, y reescribilo.

```typescript
class Producto {
  nombre: string;
  precio: number;
  stock: number;

  setPrecio(precio: number): void {
    this.precio = precio;
  }

  vender(cantidad: number): void {
    this.stock = this.stock - cantidad;
  }
}

const p = new Producto();
p.nombre = "Mouse";
p.precio = -500;
p.stock = 10;
p.vender(50);
```

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Problema 1 — No hay constructor.**
> `new Producto()` crea un producto **vacío**: nombre, precio y stock quedan en `undefined`. El objeto nace roto y hay que "armarlo" a mano desde afuera, con el riesgo de olvidarse un campo.
>
> **Problema 2 — Todos los atributos son públicos.**
> Por eso `p.precio = -500` funciona sin chistar. El `setPrecio()` que escribieron es **decorativo**: la ventana está abierta al lado de la puerta.
>
> **Problema 3 — El setter no valida nada.**
> `setPrecio()` hace `this.precio = precio` y listo. Acepta `-500`, acepta `0`. **Un setter sin validación es un atributo público con pasos extra.**
>
> **Problema 4 — `vender()` no controla el stock.**
> `p.vender(50)` con stock 10 deja el stock en **-40**. Vendiste 50 unidades de algo de lo que tenías 10.
>
> **Reescrito:**
>
> ```typescript
> class Producto {
>   private nombre: string;
>   private precio: number;
>   private stock: number;
>
>   constructor(nombre: string, precio: number, stock: number) {
>     this.nombre = nombre;
>     this.precio = 0;
>     this.stock = 0;
>     this.setPrecio(precio);      // que el constructor tampoco se saltee las reglas
>     this.reponer(stock);
>   }
>
>   getPrecio(): number { return this.precio; }
>   getStock(): number  { return this.stock; }
>
>   setPrecio(precio: number): void {
>     if (precio <= 0) {
>       console.log("El precio debe ser mayor a 0");
>       return;
>     }
>     this.precio = precio;
>   }
>
>   reponer(cantidad: number): void {
>     if (cantidad <= 0) {
>       console.log("La cantidad a reponer debe ser positiva");
>       return;
>     }
>     this.stock += cantidad;
>   }
>
>   vender(cantidad: number): void {
>     if (cantidad <= 0) {
>       console.log("La cantidad debe ser positiva");
>       return;
>     }
>     if (cantidad > this.stock) {
>       console.log(`Stock insuficiente. Hay ${this.stock}.`);
>       return;
>     }
>     this.stock -= cantidad;
>   }
> }
> ```
>
> **El patrón que se repite en los cuatro arreglos:** el objeto **se defiende solo**. No confía en que el de afuera lo use bien.

---

## Después de clase — Ejercicio 4: Jerarquía de empleados

**Consigna:** modelá esto.

- `Empleado`: `nombre`, `legajo`, `sueldoBase`. Método `mostrarDatos()`.
- `Vendedor extends Empleado`: agrega `zona`.
- `Repartidor extends Empleado`: agrega `patenteMoto`.

Cada subclase con su constructor llamando a `super()`, y un método propio (`Vendedor.visitarCliente()`, `Repartidor.entregar()`).

**Además, contestá:** ¿`sueldoBase` va `private` o `protected`? ¿Cambiaría tu respuesta si `Vendedor` tuviera que calcular una comisión sobre el sueldo?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> ```typescript
> class Empleado {
>   protected nombre: string;
>   protected legajo: number;
>   protected sueldoBase: number;
>
>   constructor(nombre: string, legajo: number, sueldoBase: number) {
>     this.nombre = nombre;
>     this.legajo = legajo;
>     this.sueldoBase = sueldoBase;
>   }
>
>   mostrarDatos(): void {
>     console.log(`${this.nombre} (legajo ${this.legajo}) — $${this.sueldoBase}`);
>   }
> }
>
> class Vendedor extends Empleado {
>   private zona: string;
>
>   constructor(nombre: string, legajo: number, sueldoBase: number, zona: string) {
>     super(nombre, legajo, sueldoBase);
>     this.zona = zona;
>   }
>
>   visitarCliente(): void {
>     console.log(`${this.nombre} sale a visitar clientes en ${this.zona}`);
>   }
> }
>
> class Repartidor extends Empleado {
>   private patenteMoto: string;
>
>   constructor(nombre: string, legajo: number, sueldoBase: number, patenteMoto: string) {
>     super(nombre, legajo, sueldoBase);
>     this.patenteMoto = patenteMoto;
>   }
>
>   entregar(): void {
>     console.log(`${this.nombre} entrega con la moto ${this.patenteMoto}`);
>   }
> }
> ```
>
> **La pregunta de la visibilidad — y por qué es la parte importante del ejercicio:**
>
> Tal como está el enunciado original, `sueldoBase` **podría** ser `private`: nadie más que `Empleado` lo usa (el `mostrarDatos()` vive en el padre).
>
> Pero apenas agregás *"`Vendedor` calcula una comisión sobre el sueldo"*, `Vendedor` necesita **leer** `this.sueldoBase` — y con `private` **no compila**. Ahí sí tiene que ser `protected`.
>
> **La moraleja:** la visibilidad **no se decide de una vez y para siempre**. Se decide según **quién necesita el dato**. Empezás cerrando, y abrís cuando el compilador te demuestra que hace falta.
>
> Y fijate el matiz: `Vendedor` necesita **leerlo**, no **escribirlo**. Un `protected sueldoBase` le da las dos cosas. Si quisieras darle solo la lectura, la alternativa sería dejarlo `private` y agregar un `protected getSueldoBase()`. Más prolijo, más código. Es un trade-off real y no hay una única respuesta correcta.

---

## Después de clase — Ejercicio 5: Variables sueltas vs objetos

**Consigna:** resolvé esta situación **de dos formas**:

> *"Se necesitan representar 3 productos con nombre, precio y stock, y poder venderlos."*

- **Forma 1:** con variables sueltas (`nombreProducto1`, `precioProducto1`, …).
- **Forma 2:** con una clase `Producto` y tres objetos.

Después escribí un texto corto: **¿cuáles son las diferencias?** ¿Qué pasa cuando en vez de 3 productos son 500?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Forma 1 — variables sueltas:**
>
> ```typescript
> let nombreProducto1 = "Mouse";
> let precioProducto1 = 3000;
> let stockProducto1 = 10;
>
> let nombreProducto2 = "Teclado";
> let precioProducto2 = 12000;
> let stockProducto2 = 5;
>
> // ...y así
>
> function venderProducto1(cantidad: number): void {
>   stockProducto1 -= cantidad;   // una función por producto 🙃
> }
> ```
>
> **Forma 2 — con objetos:** la clase `Producto` del ejercicio 3, y `const productos = [p1, p2, p3];`
>
> **Las diferencias que importan:**
>
> | | Variables sueltas | Objetos |
> |---|---|---|
> | Agregar un producto | Escribir 3 variables + 1 función más | `new Producto(...)` |
> | 500 productos | 1500 variables | Un array |
> | Validar el precio | En cada lugar donde lo asignes | Una vez, en el setter |
> | Los datos relacionados | Sueltos, unidos solo por el nombre de la variable | **Juntos adentro del objeto** |
> | Recorrerlos todos | Imposible sin escribirlos uno por uno | `productos.forEach(...)` |
>
> **El punto de fondo, que es el que quiero que te lleves:**
>
> Con variables sueltas, `precioProducto1` y `stockProducto1` están relacionadas **solo en tu cabeza**. Para el programa son dos variables cualesquiera. Nada impide restarle stock a un producto y precio a otro.
>
> El objeto **hace explícita esa relación**: el precio y el stock **son del mismo producto** porque viven adentro del mismo objeto. Y encima le podés colgar el comportamiento (`vender()`) al lado de los datos que ese comportamiento toca.
>
> **Con 500 productos, la forma 1 no es "peor": es imposible.** Y ese salto —de "se puede pero es feo" a "no se puede"— es el que justifica todo el paradigma.

---

## Después de clase — Ejercicio 6: Integrador — Biblioteca

**Consigna:** modelá el sistema de una biblioteca.

- `Libro`: `titulo`, `autor`, `isbn`, y `estaPrestado` (privado, arranca en `false`).
  - `prestar()` → solo si no está prestado ya
  - `devolver()` → solo si está prestado
  - `estaDisponible()` → getter
- `Persona`: `nombre` y `dni`.
- `Socio extends Persona`: agrega `numeroDeSocio` y un array privado `librosPrestados: Libro[]`.
  - `pedirPrestado(libro)` → solo si el libro está disponible **y** el socio tiene menos de 3 libros
  - `devolverLibro(libro)`
  - `mostrarPrestamos()`

Probalo: que un socio pida 4 libros y que intente pedir uno ya prestado.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> ```typescript
> class Libro {
>   private titulo: string;
>   private autor: string;
>   private isbn: string;
>   private prestado: boolean = false;
>
>   constructor(titulo: string, autor: string, isbn: string) {
>     this.titulo = titulo;
>     this.autor = autor;
>     this.isbn = isbn;
>   }
>
>   getTitulo(): string { return this.titulo; }
>
>   estaDisponible(): boolean {
>     return !this.prestado;
>   }
>
>   prestar(): boolean {
>     if (this.prestado) {
>       console.log(`"${this.titulo}" ya está prestado`);
>       return false;
>     }
>     this.prestado = true;
>     return true;
>   }
>
>   devolver(): void {
>     if (!this.prestado) {
>       console.log(`"${this.titulo}" no estaba prestado`);
>       return;
>     }
>     this.prestado = false;
>   }
> }
>
> class Persona {
>   protected nombre: string;
>   protected dni: string;
>
>   constructor(nombre: string, dni: string) {
>     this.nombre = nombre;
>     this.dni = dni;
>   }
> }
>
> class Socio extends Persona {
>   private numeroDeSocio: number;
>   private librosPrestados: Libro[] = [];
>
>   private readonly MAXIMO: number = 3;
>
>   constructor(nombre: string, dni: string, numeroDeSocio: number) {
>     super(nombre, dni);
>     this.numeroDeSocio = numeroDeSocio;
>   }
>
>   pedirPrestado(libro: Libro): void {
>     if (this.librosPrestados.length >= this.MAXIMO) {
>       console.log(`${this.nombre} ya tiene ${this.MAXIMO} libros`);
>       return;
>     }
>     if (libro.prestar()) {              // el libro decide si se deja prestar
>       this.librosPrestados.push(libro);
>       console.log(`${this.nombre} se llevó "${libro.getTitulo()}"`);
>     }
>   }
>
>   devolverLibro(libro: Libro): void {
>     const i = this.librosPrestados.indexOf(libro);
>     if (i === -1) {
>       console.log(`${this.nombre} no tiene ese libro`);
>       return;
>     }
>     libro.devolver();
>     this.librosPrestados.splice(i, 1);
>   }
>
>   mostrarPrestamos(): void {
>     console.log(`${this.nombre} tiene ${this.librosPrestados.length} libro(s):`);
>     this.librosPrestados.forEach(l => console.log(`  - ${l.getTitulo()}`));
>   }
> }
> ```
>
> **Lo que este ejercicio junta, y por qué es el integrador:**
>
> **1. Cada objeto se defiende a sí mismo.** El `Libro` no deja que lo presten dos veces. El `Socio` no deja pasar de 3. Ninguno confía en el otro.
>
> **2. La verificación vive donde vive el dato.** Fijate esta línea:
>
> ```typescript
> if (libro.prestar()) { ... }
> ```
>
> El `Socio` **no pregunta** `if (libro.prestado === false)` — no puede, es privado. Le **pide al libro que se preste**, y el libro contesta si pudo o no. Quien sabe si un libro está disponible es **el libro**, no el socio.
>
> Si el socio pudiera leer `libro.prestado` directamente, esa regla se podría saltear desde cualquier parte del sistema. Encapsulada adentro de `prestar()`, **no hay forma de esquivarla**.
>
> **3. `librosPrestados` es privado.** Si fuera público, alguien podría hacer `socio.librosPrestados.push(libro)` y saltearse **las dos** validaciones de un saque: el máximo de 3 **y** el estado del libro.
>
> **4. Herencia con criterio.** "Un socio **es una** persona" ✔. Y `Socio` **tiene** libros — esa relación **no** es herencia. Es otra cosa, que tiene nombre, y que vamos a ver pronto.

---

## Para pensar hasta la clase que viene

1. `Perro` hereda `comer()` de `Animal`. ¿Y si el perro tuviera que comer **distinto** que un animal genérico? ¿Podés cambiarle el comportamiento **sin tocar** `Animal`?
2. Tenés un array con perros, gatos y vacas. Querés que **cada uno haga su sonido**. ¿Se te ocurre cómo, sin escribir un `if` por cada tipo de animal?
3. En el ejercicio de la biblioteca, `Socio` **tiene** libros y `Auto` **tiene** un motor. Esa relación —"tiene un"— no es herencia. **¿Qué es?**

**Retomamos con la clase 3:** sobreescritura y polimorfismo. Las preguntas 1 y 2 son exactamente el tema. La 3 la contestamos un poco más adelante.

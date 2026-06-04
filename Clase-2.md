# Clase 2 - Encapsulamiento y Herencia

## Repaso rápido

### Clase, Objeto e Instancia

Una **clase** es como una plantilla o molde. Define cómo van a ser los objetos que se creen a partir de ella.

Un **objeto** es una copia concreta de esa plantilla, con sus propios valores, guardada en memoria. Cuando creás un objeto se dice que lo *instanciás*.

```
Clase   → el plano de un edificio
Objeto  → el edificio construido a partir de ese plano
```

Podés tener un solo plano y construir mil edificios distintos. Cada uno tiene sus propias características (color, cantidad de pisos), pero todos comparten la misma estructura base.

### Atributos, Métodos y Constructor

- **Atributos**: las variables que describen al objeto (su nombre, edad, saldo, etc.)
- **Métodos**: las funciones que el objeto puede hacer (saludar, depositar dinero, atacar)
- **Constructor**: el método especial que se ejecuta automáticamente cuando creás un objeto con `new`. Es el lugar ideal para asignar los valores iniciales.

### `this` — "yo mismo"

Dentro de una clase, `this` hace referencia al objeto que se está usando en ese momento. Es la forma que tiene el objeto de hablar de sí mismo.

```typescript
class Persona {
  private nombre: string;

  // El constructor recibe 'nombre' como parámetro
  // 'this.nombre' es el atributo de la clase
  // 'nombre' (sin this) es el parámetro que llega
  constructor(nombre: string) {
    this.nombre = nombre; // "mi atributo nombre = el parámetro nombre"
  }

  saludar(): void {
    // this.nombre accede al atributo del objeto
    console.log(`Hola, soy ${this.nombre}`);
  }
}

const p = new Persona("Trinidad");
p.saludar(); // "Hola, soy Trinidad"
```

> **Truco de TypeScript**: podés abreviar el constructor así y TypeScript crea los atributos automáticamente:
> ```typescript
> constructor(private nombre: string) {}
> ```

---

## Encapsulamiento

### ¿Qué problema resuelve?

Imaginá que tenés una clase `CuentaBancaria` con un atributo `saldo`. Si ese atributo es público, cualquiera podría hacer esto:

```typescript
cuenta.saldo = -99999; // ← nadie debería poder hacer esto directamente
```

El **encapsulamiento** protege los datos del objeto. Define qué puede ver el exterior y qué no.

### Niveles de visibilidad en TypeScript

| Modificador | ¿Quién puede acceder? |
|-------------|----------------------|
| `private`   | Solo la propia clase |
| `protected` | La clase y sus subclases |
| `public`    | Cualquiera |

### Getters y Setters

Por convención, los atributos se declaran `private` y se acceden a través de métodos públicos llamados *getters* (para leer) y *setters* (para escribir).

Esto permite **controlar** qué se puede leer, qué se puede modificar, y con qué condiciones.

```typescript
class CuentaBancaria {
  private saldo: number;
  private titular: string;

  constructor(titular: string, saldoInicial: number) {
    this.titular = titular;
    // Usamos el setter para validar desde el inicio
    this.depositar(saldoInicial);
  }

  // Getter: solo lectura del saldo
  getSaldo(): number {
    return this.saldo;
  }

  getTitular(): string {
    return this.titular;
  }

  // No hay setter de saldo directo — solo se puede modificar con métodos controlados

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
cuenta.mostrarEstado(); // Titular: Trinidad | Saldo: $1300
// cuenta.saldo = -99999; ← ERROR: 'saldo' is private
```

### Ejercicio guiado — Mascota

Completá esta clase siguiendo el modelo de `CuentaBancaria`:

```typescript
class Mascota {
  private nombre: string;
  private energia: number; // va de 0 a 100

  constructor(nombre: string) {
    this.nombre = nombre;
    this.energia = 50; // energía inicial
  }

  getNombre(): string {
    return this.nombre;
  }

  getEnergia(): number {
    return this.energia;
  }

  // TODO: implementar comer(cantidad: number)
  // — aumenta la energia
  // — no puede superar 100
  // — 'cantidad' debe ser positiva

  // TODO: implementar jugar()
  // — reduce energia
  // — si energía es baja, mostrar "está cansada, necesita comer"
}
```

### Errores comunes en encapsulamiento

```typescript
//  MAL: atributo público, cualquiera puede modificarlo
class Persona {
  public edad: number; // ← sin control
}

//  MAL: setter sin validación
setEdad(edad: number): void {
  this.edad = edad; // ← acepta -5, 999, cualquier cosa
}

//  BIEN: setter con validación
setEdad(edad: number): void {
  if (edad >= 0 && edad <= 120) {
    this.edad = edad;
  }
}
```

### En proyectos reales

El encapsulamiento aparece en todos lados: en APIs que no exponen su lógica interna, en formularios que validan antes de guardar, en sistemas bancarios que impiden saldos negativos. Siempre que diseñes una clase, preguntate: **¿qué debería poder hacer alguien que use esta clase desde afuera?** Todo lo demás, privado.

---

## Herencia

### ¿Qué problema resuelve?

Imaginá que tenés que modelar `Perro` y `Gato`. Ambos tienen nombre, edad, comen, duermen. Si duplicás el código, cuando quieras cambiar algo tenés que cambiarlo en dos lugares. La herencia permite definir las partes comunes **una sola vez** en una clase padre.

### La relación "es un"

La herencia se usa cuando podés decir que **"B es un A"**:
- `Perro` **es un** `Animal` 
- `Auto` **es un** `Vehiculo` 
- `Motor` **es un** `Auto`  (un motor NO es un auto)

Si no podés decir "es un", la herencia no es la herramienta correcta.

### `extends` — cómo se escribe

```typescript
class Animal {
  protected nombre: string; // 'protected' para que las subclases puedan usarlo
  protected energia: number;

  constructor(nombre: string) {
    this.nombre = nombre;
    this.energia = 100;
  }

  comer(): void {
    this.energia += 20;
    console.log(`${this.nombre} comió. Energía: ${this.energia}`);
  }

  dormir(): void {
    this.energia += 30;
    console.log(`${this.nombre} duerme. Energía: ${this.energia}`);
  }

  getNombre(): string {
    return this.nombre;
  }
}

// Perro HEREDA de Animal con 'extends'
class Perro extends Animal {
  private raza: string;

  // El constructor de Perro recibe también la raza
  constructor(nombre: string, raza: string) {
    super(nombre); // ← OBLIGATORIO: llama al constructor del padre
    this.raza = raza;
  }

  // Método propio de Perro, no existe en Animal
  ladrar(): void {
    console.log(`${this.nombre} dice: ¡Guau!`);
  }

  getRaza(): string {
    return this.raza;
  }
}

class Gato extends Animal {
  private estaDormido: boolean;

  constructor(nombre: string, estaDormido: boolean) {
    super(nombre);
    this.estaDormido = estaDormido;
  }

  maullar(): void {
    console.log(`${this.nombre} dice: ¡Miau!`);
  }
}

const perro = new Perro("Fido", "Labrador");
perro.comer();    // heredado de Animal
perro.ladrar();   // propio de Perro

const gato = new Gato("Luna", true);
gato.dormir();    // heredado de Animal
gato.maullar();   // propio de Gato
```

### `super()` — llamar al padre

Cuando una subclase tiene constructor, **siempre** debe llamar a `super()` primero. Es la forma de decirle al padre "inicializate vos primero, después me inicializo yo".

```typescript
constructor(nombre: string, raza: string) {
  super(nombre); // primero el padre
  this.raza = raza; // después yo
}
```

### ¿Cuándo NO usar herencia?

Si la relación no es "es un", usá **composición** (la vemos más adelante). Algunos casos donde NO corresponde herencia:

- `Persona` hereda de `Direccion`  (una persona no ES una dirección, la TIENE)
- `Auto` hereda de `Motor`  (un auto no ES un motor, lo TIENE)
- Cuando heredás solo para reutilizar código pero no hay relación lógica 

### Ejercicio guiado — Personajes RPG

```typescript
class Personaje {
  protected nombre: string;
  protected vida: number;
  protected ataque: number;

  constructor(nombre: string, vida: number, ataque: number) {
    this.nombre = nombre;
    this.vida = vida;
    this.ataque = ataque;
  }

  atacar(objetivo: Personaje): void {
    console.log(`${this.nombre} ataca a ${objetivo.nombre} por ${this.ataque} puntos`);
    objetivo.vida -= this.ataque;
  }

  estaVivo(): boolean {
    return this.vida > 0;
  }

  mostrarEstado(): void {
    console.log(`${this.nombre} - Vida: ${this.vida}`);
  }
}

// TODO: Creá una clase 'Guerrero' que extienda Personaje
// — tiene un atributo 'escudo: number'
// — constructor recibe nombre, vida, ataque, escudo
// — método 'defender()': reduce el próximo daño en 'escudo' puntos (podés guardar un booleano)

// TODO: Creá una clase 'Mago' que extienda Personaje
// — tiene un atributo 'mana: number'
// — método 'lanzarHechizo(objetivo)': hace el doble de daño normal pero cuesta 30 de mana
// — si no hay mana suficiente, ataca normal
```

---

### Ejercicio Herencia

Creá una jerarquía de vehículos:
- `Vehiculo` con atributos `marca`, `velocidadMaxima` y método `describirse()`
- `Auto` extiende `Vehiculo`, agrega `cantidadPuertas`
- `Moto` extiende `Vehiculo`, agrega `tieneCarritoAcompañante: boolean`

Cada subclase debe tener su propio constructor que llame a `super()`.

**Preguntas de razonamiento:**
1. ¿`Vehiculo` debería ser abstracta? ¿Por qué?
2. ¿Podría existir un objeto `Vehiculo` que no sea ni auto ni moto? ¿Tiene sentido?
3. ¿Qué pasaría si cambiás el atributo `marca` de `protected` a `private` en `Vehiculo`?
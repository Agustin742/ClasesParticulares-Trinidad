## Clase 5 — Diagramas de clase UML y relaciones entre objetos

---

## Antes de arrancar — variables

Empezamos con un repaso de la base, porque hoy vas a usar arrays todo el tiempo y conviene tenerlos afilados.

**Una variable es un nombre para un valor guardado en memoria.** Nada más que eso: en vez de andar acarreando el número `250000`, le ponés una etiqueta y usás la etiqueta.

```typescript
let precio: number = 250000;
```

Tres partes, y cada una hace algo distinto:

| Parte | Qué es | Para qué sirve |
|---|---|---|
| `let` | La **declaración** | Avisa que estás creando una variable nueva |
| `precio` | El **nombre** | Cómo la vas a llamar de acá en adelante |
| `: number` | El **tipo** | La promesa de qué se puede guardar ahí |
| `= 250000` | El **valor inicial** | Lo que tiene adentro al nacer |

Los tres tipos básicos que ya venís usando:

```typescript
let nombre: string = "Trinidad";     // texto, siempre entre comillas
let edad: number = 21;               // números: enteros y decimales, el mismo tipo
let esSocio: boolean = true;         // sólo true o false
```

> **El tipo no es un trámite.** Es lo que hace que TypeScript te avise, **antes de correr el programa**, que estás por hacer algo que no cierra. Si declarás `precio: number` y después escribís `precio = "caro"`, el editor te lo subraya al instante.
>
> Es la misma idea que venimos usando con `private`: **poner una regla para que la máquina la haga cumplir por vos.**

---

## `let` y `const` — cuál va

Son las dos formas de declarar. La diferencia es una sola:

```typescript
let contador: number = 0;
contador = 1;              // ✅ perfecto, let se puede reasignar

const MAXIMO: number = 3;
MAXIMO = 5;                // ❌ error: no se puede reasignar una const
```

| | Se puede reasignar | Cuándo la usás |
|---|---|---|
| `let` | Sí | El valor va a cambiar: un contador, un acumulador, un estado |
| `const` | No | El valor no cambia nunca: una constante, un objeto que creás una vez |

**La regla práctica: empezá siempre con `const`.** Sólo cambiás a `let` cuando el compilador te obligue, o sea, cuando realmente necesites reasignar.

¿Por qué al revés y no "empezá con `let` por las dudas"? Porque una `const` te está dando una garantía: **leyendo esa línea ya sabés que ese nombre va a significar lo mismo en todo el bloque.** Con `let` tenés que revisar todo el archivo para estar segura.

> **Ojo con una que ya te crucé:** `const` sirve para variables **adentro de una función o un bloque**. Para una propiedad **de clase** que no cambia, la palabra es `readonly`:
>
> ```typescript
> class Socio {
>   private readonly MAXIMO: number = 3;   // ← adentro de una clase va readonly
> }
> ```
>
> Son la misma idea —"esto no se reasigna"— en dos lugares distintos.

---

## Predecí — el `const` que sí cambia

Mirá este código:

```typescript
const numeros: number[] = [1, 2, 3];

numeros.push(4);
console.log(numeros);

numeros = [9, 9, 9];
```

**Pregunta:** ¿cuál de las dos últimas operaciones falla? ¿Y **por qué** una sí y la otra no, si las dos parecen "cambiar la const"?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **El `push(4)` funciona perfecto.** Imprime `[1, 2, 3, 4]`.
>
> **El `numeros = [9, 9, 9]` es un error de compilación.**
>
> Y la explicación es la parte importante:
>
> > **`const` no congela el contenido. Congela el nombre.**
>
> Pensalo como una casa con un cartel en la puerta que dice "Casa de Trinidad". El cartel está atornillado: no podés despegarlo y ponerlo en otra casa. **Pero adentro podés mover todos los muebles que quieras.**
>
> - `numeros.push(4)` → mover un mueble adentro. **Permitido.**
> - `numeros = [9,9,9]` → despegar el cartel y ponerlo en otra casa. **Prohibido.**
>
> | Operación | Qué toca | ¿`const` lo permite? |
> |---|---|---|
> | `numeros.push(4)` | El contenido del array | Sí |
> | `numeros[0] = 99` | El contenido del array | Sí |
> | `numeros = [9,9,9]` | **A qué array apunta el nombre** | No |
>
> Por eso casi todos los arrays y objetos se declaran `const`: **casi nunca querés reemplazar el array entero, querés modificarlo.**
>
> Y esto conecta con algo de la clase pasada: cuando dos autos comparten el mismo `Conductor`, comparten **el objeto**, no una copia. El nombre y el objeto son dos cosas distintas — hoy lo vamos a ver dibujado.

---

## Arrays — qué son

Una variable guarda **un** valor. ¿Y si necesitás guardar quinientos?

Ésta es exactamente la pregunta del ejercicio de variables sueltas vs objetos:

```typescript
// Con variables sueltas
let nombreProducto1 = "Heladera";
let nombreProducto2 = "Cocina";
let nombreProducto3 = "Smart TV";
// ...y así hasta 500
```

**Un array es una variable que guarda muchos valores, en orden, bajo un solo nombre.**

```typescript
const productos: string[] = ["Heladera", "Cocina", "Smart TV"];
```

Tres propiedades que lo definen, y las tres importan:

| Propiedad | Qué significa |
|---|---|
| **Ordenado** | Hay un primero, un segundo, un tercero. El orden se mantiene. |
| **Indexado** | A cada elemento se llega por su **posición**, un número. |
| **Homogéneo** (en TS) | Todos los elementos son **del mismo tipo**. |

> **Y esa tercera fila es la que conecta con toda la materia.** Cuando escribís `Animal[]`, estás diciendo "acá adentro van animales". Y como un perro **es un** animal, entra. Como un gato también, entra.
>
> Por eso el polimorfismo funciona: **el array del tipo padre acepta a todas las hijas.** El array no es un detalle de sintaxis, es la estructura donde el polimorfismo se vuelve útil.

---

## Arrays — cómo se crean

Tres formas, y en la práctica usás las tres.

```typescript
// 1. Con contenido, tipo explícito
const numeros: number[] = [10, 20, 30];

// 2. Vacío, para llenarlo después
const libros: Libro[] = [];

// 3. La otra notación del tipo — es exactamente lo mismo
const nombres: Array<string> = ["Ana", "Juan"];
```

**Las dos notaciones del tipo:**

| Se escribe | Se lee |
|---|---|
| `number[]` | "array de números" |
| `Array<number>` | "array de números" |

Son **idénticas**. `number[]` es la corta y es la que vas a ver casi siempre; `Array<number>` la vas a necesitar cuando lleguemos a estructuras de datos propias.

**El caso más común de todos** — el atributo array que arranca vacío:

```typescript
class Socio {
  private librosPrestados: Libro[] = [];
  //                       ↑ tipo      ↑ arranca vacío
}
```

> **¿Por qué `= []` y no dejarlo sin inicializar?** Porque si no lo inicializás, el atributo arranca en `undefined` — y `undefined.push(libro)` **revienta el programa**.
>
> Un array vacío es un array. `undefined` no es nada. **Empezá siempre en `[]`.**

---

## Índices y `length`

**Los índices arrancan en cero.** No en uno. Nunca en uno.

```typescript
const productos: string[] = ["Heladera", "Cocina", "Smart TV"];
//                              ↑ 0         ↑ 1        ↑ 2

productos[0];        // "Heladera"
productos[2];        // "Smart TV"
productos[3];        // undefined  ← no existe, y NO tira error
productos.length;    // 3
```

> [IMAGEN: una fila horizontal de tres cajas cuadradas pegadas entre sí, como un tren. Dentro de cada caja, de izquierda a derecha, los textos "Heladera", "Cocina" y "Smart TV". Debajo de cada caja, un número chico: "0", "1" y "2" respectivamente. A la derecha de la última caja, una llave que abarca las tres de abajo hacia arriba con la etiqueta "length = 3". Una flecha señalando el hueco justo después de la última caja, con la etiqueta "índice 3 → undefined".]

**El detalle que da más dolores de cabeza:** `length` cuenta **desde uno**, los índices arrancan **desde cero**. Entonces el último elemento **nunca** está en `length`, sino en `length - 1`.

```typescript
productos[productos.length];       // undefined ❌
productos[productos.length - 1];   // "Smart TV" ✅
```

Y ojo con esto, que es distinto de otros lenguajes: pedir un índice que no existe **no rompe nada**. Te devuelve `undefined` y el programa sigue. Es un error silencioso — el peor tipo.

---

## Pará y pensá — el array que se recorre mal

```typescript
const notas: number[] = [8, 6, 10, 4];

for (let i = 0; i <= notas.length; i++) {
  console.log(notas[i]);
}
```

**Pregunta:** ¿cuántas líneas imprime, y qué imprime cada una? ¿Dónde está el error, y **por qué el programa no se rompe** si el error existe?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Imprime cinco líneas:** `8`, `6`, `10`, `4` y **`undefined`**.
>
> **El error está en el `<=`.** Tiene que ser `<`:
>
> ```typescript
> for (let i = 0; i < notas.length; i++) { }
> ```
>
> La cuenta: `length` es 4, y los índices válidos son 0, 1, 2 y 3. Con `<=`, la última vuelta usa `i = 4`, que **no existe**.
>
> **Y por qué no se rompe:** porque en JavaScript pedir un índice que no existe devuelve `undefined` en vez de tirar error. El programa sigue como si nada.
>
> Eso lo hace un bug **peligroso**, no inofensivo. Si en vez de imprimir estuvieras sumando, `8 + 6 + 10 + 4 + undefined` da `NaN` — y ese `NaN` se propaga por todo el cálculo hasta que alguien, tres pantallas después, se pregunta por qué el total dice "NaN".
>
> **Este error tiene nombre propio: se llama *error de uno* (off-by-one), y es uno de los más frecuentes que hay.** Se evita casi siempre de la misma forma: **no escribiendo el `for` a mano.**
>
> ```typescript
> notas.forEach(nota => console.log(nota));
> ```
>
> Ahí no hay índice, no hay `length`, no hay `<=`. **No podés equivocarte en algo que no escribís.**

---

## Métodos — agregar y sacar

Los cuatro que vas a usar todos los días. Los cuatro **modifican el array original**.

```typescript
const cola: string[] = ["Ana", "Juan"];

cola.push("Pedro");      // agrega al FINAL     → ["Ana", "Juan", "Pedro"]
cola.pop();              // saca el ÚLTIMO      → ["Ana", "Juan"]     y devuelve "Pedro"
cola.unshift("Lucía");   // agrega al PRINCIPIO → ["Lucía", "Ana", "Juan"]
cola.shift();            // saca el PRIMERO     → ["Ana", "Juan"]     y devuelve "Lucía"
```

| Método | Dónde opera | Qué devuelve |
|---|---|---|
| `push(x)` | Final | La nueva longitud |
| `pop()` | Final | **El elemento que sacó** |
| `unshift(x)` | Principio | La nueva longitud |
| `shift()` | Principio | **El elemento que sacó** |

Y el que saca del medio, que es el que te va a hacer falta para `devolverLibro()`:

```typescript
const i = libros.indexOf(libro);   // ¿en qué posición está? -1 si no está
if (i !== -1) {
  libros.splice(i, 1);             // desde la posición i, borrá 1 elemento
}
```

> **Fijate el `if`.** `indexOf` devuelve **`-1`** cuando no encuentra el elemento — no `undefined`, no `null`. Y `splice(-1, 1)` **borra el último elemento**, que no es para nada lo que querías.
>
> **Sin ese `if`, pedirle a un socio que devuelva un libro que no tiene le borra otro libro cualquiera.** Un bug perfecto: silencioso, con datos plausibles, imposible de encontrar leyendo.

---

## Métodos — recorrer y buscar

**Recorrer:** `forEach` ejecuta algo por cada elemento. Es el reemplazo del `for` a mano.

```typescript
animales.forEach(animal => animal.hacerSonido());
//               ↑ el parámetro es cada elemento, uno por vez
```

**Buscar** — cuatro métodos, cuatro preguntas distintas:

```typescript
const notas: number[] = [8, 6, 10, 4];

notas.includes(10);              // ¿está el 10?              → true
notas.indexOf(10);               // ¿en qué posición está?    → 2
notas.find(n => n >= 9);         // dame el PRIMERO que cumpla → 10
notas.some(n => n < 6);          // ¿hay ALGUNO que cumpla?   → true
notas.every(n => n >= 4);        // ¿cumplen TODOS?           → true
```

| Método | La pregunta que contesta | Devuelve |
|---|---|---|
| `includes(x)` | ¿Está este valor exacto? | `boolean` |
| `indexOf(x)` | ¿En qué posición está? | Número, o **`-1`** |
| `find(fn)` | ¿Cuál es el primero que cumple? | El elemento, o `undefined` |
| `some(fn)` | ¿Hay al menos uno? | `boolean` |
| `every(fn)` | ¿Cumplen todos? | `boolean` |

> **`includes` vs `some`:** `includes` compara con `===` un valor exacto. `some` evalúa una **condición**. Si buscás un objeto por alguna de sus propiedades, va `some` o `find`, nunca `includes`.

---

## Métodos — transformar

Estos **no modifican el original**: devuelven un array **nuevo**.

```typescript
const precios: number[] = [100, 250, 80, 500];

const conIva = precios.map(p => p * 1.21);
// [121, 302.5, 96.8, 605]   ← array NUEVO, mismo largo

const caros = precios.filter(p => p > 200);
// [250, 500]                ← array NUEVO, más corto

const total = precios.reduce((acum, p) => acum + p, 0);
// 930                       ← NO es un array: es un solo valor
```

| Método | Qué hace | Largo del resultado |
|---|---|---|
| `map(fn)` | **Transforma** cada elemento | **Igual** al original |
| `filter(fn)` | **Selecciona** los que cumplen | **Menor o igual** |
| `reduce(fn, inicial)` | **Acumula** todo en un solo valor | Un valor, no un array |

**`reduce` merece un párrafo,** porque es el que más cuesta y lo vas a usar hoy:

```typescript
precios.reduce((acumulado, precio) => acumulado + precio, 0);
//              ↑ lo que llevo       ↑ el actual              ↑ desde dónde arranco
```

Lleva una "bolsa" (`acumulado`) que arranca en `0`, y por cada elemento decide qué meter en la bolsa. Al final, devuelve la bolsa.

Es exactamente esto, escrito en una línea:

```typescript
let acumulado = 0;
precios.forEach(precio => { acumulado = acumulado + precio; });
```

---

## Los que modifican y los que no

Ésta es **la tabla que más te va a servir**, porque el error acá es silencioso:

| Modifican el original | Devuelven uno nuevo |
|---|---|
| `push`, `pop` | `map` |
| `shift`, `unshift` | `filter` |
| `splice`, `sort`, `reverse` | `slice`, `concat` |

**El error clásico:**

```typescript
const precios = [100, 250, 80];

precios.map(p => p * 1.21);        // ⚠️ no pasó nada
console.log(precios);              // [100, 250, 80] — intacto

const conIva = precios.map(p => p * 1.21);   // ✅ hay que guardarlo
```

`map` **no cambia** `precios`. Fabrica un array nuevo y te lo devuelve — y si no lo guardás en ninguna variable, se pierde.

> **La forma de acordarte:** los que modifican son los que **hacen algo** (empujar, sacar, ordenar). Los que devuelven uno nuevo son los que **responden algo** (mapeame esto, filtrame aquello).
>
> Y por eso `sort()` es traicionero: parece que "responde el array ordenado", pero **te ordena el original**. Si necesitás conservarlo, copialo primero con `slice()`.

---

## Checkpoint — arrays

Contestá las cinco sin mirar para arriba.

1. `const libros: Libro[] = []` — ¿por qué el `= []` y no dejarlo sin inicializar?
2. Un array tiene `length` 5. ¿Cuál es el índice del último elemento?
3. ¿Qué devuelve `indexOf()` cuando no encuentra nada, y por qué es peligroso?
4. `precios.filter(p => p > 100)` — después de esa línea, ¿cambió `precios`?
5. Quiero saber si **alguna** nota es menor a 4. ¿`find`, `some` o `includes`?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **1.** Sin `= []`, el atributo arranca en `undefined`, y `undefined.push(...)` rompe el programa. Un array vacío **sí es un array**; `undefined` no es nada.
>
> **2.** El índice **4**, o sea `length - 1`. `length` cuenta desde uno, los índices desde cero.
>
> **3.** Devuelve **`-1`**. Es peligroso porque `-1` es un número válido para otros métodos: `splice(-1, 1)` borra el último elemento en vez de no hacer nada. **Siempre chequeá `!== -1` antes de usarlo.**
>
> **4.** **No.** `filter` devuelve un array nuevo y deja el original intacto. Si no guardás el resultado, se pierde.
>
> **5.** **`some`.** `includes` compara un valor exacto y acá hay una **condición**. `find` también sirve, pero te devuelve la nota en vez de un `true`/`false` — y si la nota fuera `0`, `if (notas.find(...))` daría falso por un motivo equivocado.
>
> **La 5 es la que más se falla**, y por eso la pongo última: elegir el método correcto no es memorizar la lista, es **saber qué pregunta estás haciendo**. `¿Hay alguno?` → `some`. `¿Cuál es?` → `find`. `¿Está este exacto?` → `includes`.

---

## Y ahora sí — arrancamos

Con eso ya tenés todo lo que hace falta. Guardá dos ideas de este repaso, porque las vas a usar en toda la clase de hoy:

1. **Un array de un tipo acepta a todas sus hijas** (`Animal[]` acepta perros y gatos). Es donde el polimorfismo se vuelve útil.
2. **El nombre y el objeto son dos cosas distintas.** Dos variables pueden apuntar al mismo objeto.

La segunda es, literalmente, el tema de la segunda mitad de la clase.

---
## Razoná — la pregunta que dejamos abierta dos veces

Al final de la clase de repaso te dejé esto:

> *"`Socio` **tiene** libros y `Auto` **tiene** un motor. Esa relación —'tiene un'— no es herencia. **¿Qué es?**"*

Y al final de la clase 4, otra vez:

> *"`Auto` y `Motor`. ¿Herencia? ¿Un auto **es un** motor?"*

**Pregunta:** ya sabés que no es herencia. Entonces, cuando escribís esto:

```typescript
class Socio extends Persona {
  private librosPrestados: Libro[] = [];
}
```

¿Qué **nombre** tiene esa segunda relación, la que hay entre `Socio` y `Libro`?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> Se llama **composición** o **agregación**, según cuán fuerte sea el vínculo. Hoy vamos a ver la diferencia — y es una diferencia con consecuencias reales, no un detalle de vocabulario.
>
> Pero antes quiero que veas **por qué** te la debía hace dos clases.
>
> La herencia tiene una palabra clave que la hace visible: `extends`. La implementación también: `implements`. Las ves de una porque están escritas.
>
> La composición **no tiene palabra clave**. Está escondida adentro de un tipo de atributo:
>
> ```typescript
> private librosPrestados: Libro[] = [];
> //                       ↑ acá está la relación, y no la anuncia nadie
> ```
>
> Por eso es la más difícil de detectar leyendo código, y **por eso hoy la vamos a dibujar.**
>
> En un diagrama, esa relación es una flecha bien visible. En el código, son ocho caracteres perdidos en el medio de un archivo.

---

## Para qué sirve dibujar

Tenés escritas, entre la biblioteca y la veterinaria, unas diez clases. Si te pido que me expliques cómo se relacionan entre sí, tenés dos opciones:

| Explicándolo con el código | Explicándolo con un diagrama |
|---|---|
| Abrir ocho archivos | Mirar una hoja |
| Buscar los `extends` y los `implements` | Seguir las flechas |
| Adivinar las relaciones "tiene un" mirando atributos | Verlas dibujadas |
| Diez minutos | Treinta segundos |

Y hay algo más importante que la velocidad.

**El diagrama se hace ANTES del código.** No es documentación de algo que ya escribiste: es la etapa donde decidís **quién es quién** y **quién conoce a quién**, mientras todavía es barato equivocarse.

Borrar una flecha mal puesta cuesta un segundo. Cambiar una jerarquía de herencia mal elegida, cuando ya tenés 300 líneas escritas, cuesta una tarde.

> **La frase para llevarte:** el papel es el único lugar donde refactorizar es gratis.

---

## La caja de clase

Toda clase se dibuja igual: **una caja partida en tres pisos.**

> [IMAGEN: un rectángulo vertical dividido por dos líneas horizontales en tres compartimentos. El compartimento de arriba, más chico, contiene centrado y en negrita el texto "Libro". El del medio contiene tres líneas de texto alineadas a la izquierda: "- titulo: string", "- autor: string", "- estaPrestado: boolean". El de abajo contiene dos líneas: "+ prestar(): boolean", "+ estaDisponible(): boolean". Al costado derecho, tres etiquetas con flechas finas apuntando a cada compartimento, que dicen respectivamente "NOMBRE", "ATRIBUTOS" y "MÉTODOS".]

| Piso | Qué va | Formato |
|---|---|---|
| Arriba | El **nombre** de la clase | Sustantivo, PascalCase |
| Medio | Los **atributos** | `visibilidad nombre: tipo` |
| Abajo | Los **métodos** | `visibilidad nombre(params): retorno` |

Dos reglas que se olvidan siempre:

- **El tipo va después de los dos puntos**, igual que en TypeScript. `titulo: string`, nunca `string titulo`.
- **Los métodos llevan paréntesis siempre**, aunque no reciban nada. Es lo único que distingue a simple vista un método de un atributo.

---

## Visibilidad — tres símbolos

Adelante de cada atributo y de cada método va un símbolo. Son sólo tres, y ya los conocés a los tres:

| Símbolo | En TypeScript | Quién puede tocarlo |
|---|---|---|
| `+` | `public` | Cualquiera, desde afuera |
| `-` | `private` | Sólo la propia clase |
| `#` | `protected` | La clase **y sus hijas** |

El `#` es el que más se olvida, así que un recurso para acordarte: **el numeral tiene más "paredes" que el guión, pero menos que ninguna.** Está en el medio, igual que `protected`.

> **Y ojo con esto, que es el error más común de todos:** si en tu diagrama casi todos los atributos tienen `+`, el diagrama te está avisando que el diseño está mal **antes de que escribas una línea**. Los atributos van `-` por defecto. El `+` es la excepción, no la regla.

---

## Pará y pensá — ¿qué falta en esta caja?

Alguien dibujó esta caja para representar la clase `Usuario` de tu primera tarea:

> [IMAGEN: un rectángulo dividido en tres compartimentos. Arriba: "Usuario". En el medio: "nombre: string", "email: string", "contrasenia: string". Abajo: "mostrarInfo", "login: boolean".]

**Pregunta:** tiene **tres** errores de notación. ¿Cuáles son? Y una más importante: **¿por qué el primero de ellos no es un detalle estético?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **1. No hay símbolos de visibilidad.** Ninguno de los cinco miembros dice si es `+`, `-` o `#`.
>
> **2. A los métodos les faltan los paréntesis.** Escrito así, `mostrarInfo` parece un atributo y `login: boolean` parece un booleano guardado, no un método que devuelve `true` o `false`.
>
> **3. A `login()` le falta el parámetro.** Debería ser `+ login(pass: string): boolean`. Un método sin sus parámetros no se puede implementar leyendo el diagrama.
>
> ---
>
> **Y ahora la pregunta importante: ¿por qué el error 1 no es estético?**
>
> Porque **la visibilidad es la mitad del diseño.** Un diagrama sin visibilidades no te dice si `contrasenia` está protegida o si cualquiera puede leerla desde afuera — y ésa era, literalmente, la consigna de ese ejercicio.
>
> Sin los símbolos, este diagrama y uno con **todo público** se dibujan igual. Y son sistemas completamente distintos.
>
> **Corregida queda así:**
>
> > [IMAGEN: un rectángulo dividido en tres compartimentos. Arriba: "Usuario". En el medio: "+ nombre: string", "+ email: string", "- contrasenia: string". Abajo: "+ mostrarInfo(): void", "+ login(pass: string): boolean".]
>
> Fijate que ahora se lee de un vistazo lo que antes había que ir a buscar al código: **la contraseña es la única con `-`.**

---

## Lo abstracto va en itálica

Una sola convención más y ya podés dibujar cualquier clase:

**Lo que es abstracto se escribe en itálica.** El nombre de la clase, si la clase es abstracta. El nombre del método, si el método es abstracto.

> [IMAGEN: dos rectángulos de tres compartimentos, uno al lado del otro. El de la izquierda tiene arriba la palabra "Figura" escrita en itálica; en el medio "# color: string"; abajo dos líneas: "+ describirse(): void" en texto normal y "+ calcularArea(): number" en itálica. El de la derecha tiene arriba "Circulo" en texto normal; en el medio "- radio: number"; abajo "+ calcularArea(): number" en texto normal. Debajo de ambos, una nota al pie que dice "itálica = abstracto".]

Con esa sola regla, el diagrama te contesta de un vistazo tres preguntas que en el código tenés que ir a buscar:

- **¿Se puede instanciar?** Si el nombre está en itálica, no.
- **¿Qué está obligada a implementar cada hija?** Los métodos en itálica.
- **¿Qué hereda ya resuelto?** Los métodos en texto normal.

> **Fijate en `Figura`:** `describirse()` en normal y `calcularArea()` en itálica, en la misma caja. Eso es exactamente lo que discutimos la clase pasada — la clase abstracta define **el qué** y delega **el cómo**. Y ahora se ve, en vez de tener que explicarlo.

---

## Ejercicio en vivo 1 — Dibujá tu propia clase

Tomá el `Libro` de la biblioteca — el tuyo, el que corregiste — y dibujá su caja completa.

```typescript
export default class Libro {
  private titulo: string;
  private autor: string;
  private isbn: string;
  private estaPrestado: boolean = false;

  constructor(titulo: string, autor: string, isbn: string) { /* ... */ }

  prestar(): boolean { /* ... */ }
  devolver(): void { /* ... */ }
  estaDisponible(): boolean { /* ... */ }
  getTitulo(): string { /* ... */ }
}
```

**Y contestame dos cosas:**

1. El **constructor**, ¿va en el diagrama? ¿En qué piso?
2. `estaPrestado` es privado y no tiene getter propio. Entonces, **¿para qué existe `estaDisponible()`**, si devuelve casi lo mismo? ¿Por qué no poner `estaPrestado` en `+`?

---

## Ejercicio en vivo 1 — Resolución

> [IMAGEN: un rectángulo dividido en tres compartimentos. Arriba, centrado y en negrita: "Libro". En el medio, cuatro líneas: "- titulo: string", "- autor: string", "- isbn: string", "- estaPrestado: boolean". Abajo, cinco líneas: "+ constructor(titulo: string, autor: string, isbn: string)", "+ prestar(): boolean", "+ devolver(): void", "+ estaDisponible(): boolean", "+ getTitulo(): string".]

**1. El constructor sí va, en el piso de los métodos.** Es un método más — el único que no lleva tipo de retorno, porque siempre devuelve una instancia de la clase. Suele ir primero de la lista.

*(En diagramas muy grandes se omite para no ensuciar. Pero mientras estés aprendiendo, ponelo: es el que te dice qué datos necesita el objeto para nacer.)*

**2. Por qué `estaDisponible()` y no `+ estaPrestado`** — acá está lo bueno.

Mirá las dos versiones dibujadas:

| Diseño A | Diseño B |
|---|---|
| `+ estaPrestado: boolean` | `- estaPrestado: boolean` + `+ estaDisponible(): boolean` |
| Cualquiera puede **escribirlo** | Sólo se puede **preguntar** |
| `libro.estaPrestado = false` sin devolverlo | Para cambiarlo hay que pasar por `prestar()` / `devolver()` |

Con el diseño A, cualquiera "devuelve" un libro sin devolverlo, y las reglas que escribiste adentro de `devolver()` **se saltean**.

**El atributo público es una puerta de entrada; el método es una puerta con guardia.**

Y fijate lo que hizo el diagrama: te mostró la diferencia entre los dos diseños **en el símbolo de un carácter**. Eso es lo que hace útil dibujar.

---

## Las cuatro flechas

Las cajas solas no dicen nada. **El sistema son las flechas.**

Son cuatro, y las cuatro las vas a reconocer por dos cosas: **cómo es la línea** y **qué tiene en la punta**.

| Relación | Línea | Punta | Se lee |
|---|---|---|---|
| **Herencia** | Llena | Triángulo **vacío** | "es un" |
| **Implementación** | Punteada | Triángulo **vacío** | "puede / se compromete a" |
| **Composición** | Llena | Rombo **lleno** | "tiene un", fuerte |
| **Agregación** | Llena | Rombo **vacío** | "tiene un", débil |

> [IMAGEN: cuatro filas, cada una con dos cajitas rectangulares simples conectadas por una flecha, y una etiqueta de texto a la derecha. Fila 1: caja "Perro" a la izquierda, línea llena hacia la caja "Animal" a la derecha, terminando en un triángulo hueco pegado a "Animal"; etiqueta "HERENCIA — es un". Fila 2: caja "Auto" a la izquierda, línea punteada hacia la caja "Encendible" a la derecha, terminando en un triángulo hueco pegado a "Encendible"; etiqueta "IMPLEMENTACIÓN — puede". Fila 3: caja "Auto" a la izquierda con un rombo relleno pegado a su borde derecho, línea llena desde ese rombo hasta la caja "Motor"; etiqueta "COMPOSICIÓN — tiene un, fuerte". Fila 4: caja "Auto" a la izquierda con un rombo hueco pegado a su borde derecho, línea llena desde ese rombo hasta la caja "Conductor"; etiqueta "AGREGACIÓN — tiene un, débil".]

**Dos reglas de dirección que se equivocan siempre:**

- El **triángulo** se dibuja pegado **al padre** (o a la interfaz). Apunta hacia arriba, hacia lo general.
- El **rombo** se dibuja pegado **al todo**, no a la parte. El rombo está del lado del que *contiene*.

> Truco para el rombo: **el rombo es la boca del que se come al otro.** El auto se "come" al motor → el rombo va del lado del auto.

---

## Flecha 1 — Herencia

Es la que ya venís usando desde la clase 2.

```typescript
class Animal { }
class Perro extends Animal { }
```

> [IMAGEN: arriba, una caja de tres compartimentos con el nombre "Animal", el atributo "# nombre: string" y el método "+ describirse(): void". Debajo, separadas horizontalmente, dos cajas: "Perro" con atributo "- raza: string" y método "+ hacerSonido(): void", y "Gato" con atributo "- color: string" y método "+ hacerSonido(): void". De cada caja de abajo sale una línea llena hacia arriba que termina en un triángulo hueco apoyado sobre el borde inferior de la caja "Animal". Las dos líneas pueden unirse en un tramo común antes de llegar al triángulo.]

**El test:** *"un perro **es un** animal"*. Si la frase suena bien, es herencia.

Y el test al revés, que es el que más sirve: *"¿todo perro es un animal?"* **Sí.** Si la respuesta fuera "casi siempre" o "depende", **no es herencia.**

---

## Flecha 2 — Implementación

La de la clase pasada.

```typescript
interface Vacunable {
  vacunar(vacuna: string): void;
}

class Perro extends Animal implements Vacunable { }
```

> [IMAGEN: arriba a la derecha, una caja de dos compartimentos (sin piso de atributos) con el nombre "Vacunable" escrito en itálica y con la palabra «interface» arriba del nombre, entre comillas angulares dobles; abajo, el método "+ vacunar(vacuna: string): void" también en itálica. Arriba a la izquierda, una caja "Animal" con el nombre en itálica. Abajo y en el medio, una caja "Perro". De "Perro" sale una línea llena hacia "Animal" que termina en triángulo hueco, y otra línea punteada hacia "Vacunable" que también termina en triángulo hueco.]

**Cómo se distinguen de un vistazo:** las dos terminan en triángulo hueco. La diferencia es **la línea**.

| | Línea | Qué te da |
|---|---|---|
| Herencia | **Llena** | Código heredado, ya escrito |
| Implementación | **Punteada** | Sólo una obligación. Cero código. |

> **Y una regla para acordarte cuál es cuál:** la línea **llena** transfiere algo sólido — código real. La línea **punteada** transfiere aire — una promesa que la clase tiene que cumplir por su cuenta.
>
> Fijate que la interfaz se dibuja con `«interface»` arriba del nombre. Sin eso, una interfaz y una clase abstracta se ven casi igual — las dos van en itálica.

---

## Checkpoint — ¿cuál de las dos flechas?

Para cada par, decidí si la flecha es **herencia** (llena) o **implementación** (punteada). Y en cada caso, **decí por qué en una frase**.

1. `Gerente` y `Empleado`
2. `Tarjeta` y `MedioDePago`
3. `Circulo` y `Figura`
4. `Video` y `Reproducible`
5. `Socio` y `Persona`

> [RESPUESTA: ocultar, permitir ver con un click]
>
> | | Flecha | Por qué |
> |---|---|---|
> | 1. Gerente → Empleado | **Herencia** (llena) | Un gerente **es un** empleado. Y hereda código real: nombre, legajo, `mostrarDatos()`. |
> | 2. Tarjeta → MedioDePago | **Implementación** (punteada) | `MedioDePago` no tiene código para dar. Es sólo el contrato `pagar()`. |
> | 3. Circulo → Figura | **Herencia** (llena) | Un círculo **es una** figura, y hereda `describirse()` ya escrito. |
> | 4. Video → Reproducible | **Implementación** (punteada) | "Reproducible" no es una cosa que el video **sea**: es algo que **puede hacer**. |
> | 5. Socio → Persona | **Herencia** (llena) | Un socio **es una** persona, y hereda `nombre` y `dni`. |
>
> **El patrón:** fijate en los **nombres**.
>
> Los casos de herencia son todos **sustantivos**: Empleado, Figura, Persona. Cosas que se **es**.
>
> Los casos de implementación terminan en **-able / -ible**: MedioDePago (una capacidad), Reproducible. Cosas que se **pueden hacer**.
>
> Es la misma regla de nombres que ya usaste para elegir entre clase abstracta e interfaz. **El diagrama no agrega criterios nuevos: dibuja los que ya tenías.**

---

## El problema que queda

Volvé a tu `Socio`:

```typescript
class Socio extends Persona {
  private librosPrestados: Libro[] = [];
}
```

La flecha hacia `Persona` ya sabés dibujarla: herencia.

Pero entre `Socio` y `Libro` **hay una relación real** — el socio conoce libros, los guarda, los usa — y **no es ninguna de las dos** que vimos:

- ¿Un socio **es un** libro? No.
- ¿Un socio **puede** libro? No tiene ni sentido.

Es la tercera forma de relación, la que te debía: **"tiene un"**.

Y ahí es donde aparecen los dos rombos. Porque "tiene un" **no siempre significa lo mismo**.

---

## Razoná — ¿cuál de los dos "tiene un"?

Mirá estas dos frases. Las dos son "tiene un":

- Un **auto tiene un motor**.
- Un **auto tiene un conductor**.

**Pregunta:** algo cambia entre las dos, y es lo suficientemente importante como para que UML les dé símbolos distintos.

**¿Qué es?** Pista: pensá qué pasa el día que el auto va al desguace.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Si destruís el auto:**
>
> - El **motor** se va con él. Ese motor fue fabricado para ese auto, vive adentro de ese auto y no tiene vida propia afuera. **La parte muere con el todo.**
> - El **conductor** se baja y sigue existiendo. Se compra otro auto, o maneja el de un amigo. **La parte sobrevive al todo.**
>
> Esa es toda la diferencia, y tiene nombre:
>
> | | Relación | Símbolo | La parte, sin el todo |
> |---|---|---|---|
> | Auto → Motor | **Composición** | Rombo **lleno** ♦ | No existe |
> | Auto → Conductor | **Agregación** | Rombo **vacío** ◇ | Sigue existiendo |
>
> **Cómo acordarte cuál es cuál:** el rombo **lleno** es el vínculo **lleno**, fuerte, de vida o muerte. El rombo **vacío** es el vínculo flojo — se puede vaciar y no pasa nada.

---

## El test de la destrucción

Es la única pregunta que necesitás. Guardala:

> ### Si destruyo el todo, ¿la parte sigue existiendo?
>
> - **No** → composición (rombo lleno ♦)
> - **Sí** → agregación (rombo vacío ◇)

Practicalo con estos, en voz alta:

| Todo | Parte | ¿Sobrevive la parte? | Relación |
|---|---|---|---|
| Casa | Habitación | No | Composición ♦ |
| Universidad | Alumno | Sí | Agregación ◇ |
| Pedido | Ítem del pedido | No | Composición ♦ |
| Biblioteca | Libro | Sí | Agregación ◇ |
| Libro | Capítulo | No | Composición ♦ |
| Equipo | Jugador | Sí | Agregación ◇ |

> **Una aclaración honesta, porque te la vas a cruzar:** en muchos casos reales la respuesta es discutible, y depende del dominio. ¿Un empleado sobrevive a la empresa? Como persona sí; como empleado, no.
>
> **Cuando dude, la pregunta no es "cuál es la verdad" sino "quién crea a la parte".** Eso lo vemos en la próxima slide, y es el criterio que no falla.

---

## Composición y agregación en código

Acá está la diferencia que importa, y es una sola línea:

```typescript
// COMPOSICIÓN — el todo CREA a la parte
class Auto {
  private motor: Motor;

  constructor(marca: string) {
    this.motor = new Motor(1600);   // ← el auto fabrica su propio motor
  }
}
```

```typescript
// AGREGACIÓN — el todo RECIBE a la parte
class Auto {
  private conductor: Conductor;

  constructor(marca: string, conductor: Conductor) {
    this.conductor = conductor;      // ← el conductor ya existía. Se lo pasan.
  }
}
```

**La regla, en dos palabras:**

| | Quién crea la parte | Dónde aparece | Símbolo |
|---|---|---|---|
| **Composición** | El todo, con `new` adentro | `new Motor()` en el constructor | ♦ |
| **Agregación** | Alguien de afuera | La parte llega **por parámetro** | ◇ |

> **Y por qué esto es más confiable que el test de la destrucción:** el test es filosófico y a veces se discute. Esto es **una línea de código que está o no está**.
>
> ¿Hay un `new` de la parte adentro del todo? Composición. ¿La parte entra por el constructor? Agregación. Se mira y se decide.

---

## Predecí — dos autos, un conductor

```typescript
const juan = new Conductor("Juan");

const auto1 = new Auto("Fiat", juan);
const auto2 = new Auto("Ford", juan);
```

**Pregunta:** ¿cuántos objetos `Conductor` hay en memoria? ¿Y por qué eso **no se podría hacer** si `Conductor` estuviera compuesto en vez de agregado?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Hay un solo `Conductor`.** `juan` se creó una vez, y los dos autos guardan una referencia **al mismo objeto**. Si Juan se cambia el nombre, los dos autos ven el cambio.
>
> **Y ahora la segunda parte, que es la importante.**
>
> Si `Auto` hiciera composición —`this.conductor = new Conductor("Juan")` adentro del constructor— tendrías **dos Juanes distintos**, uno por auto. Dos objetos que representan a la misma persona y no se enteran uno del otro.
>
> Eso es exactamente lo que **no** querés cuando la parte es una entidad con vida propia.
>
> | | Composición | Agregación |
> |---|---|---|
> | ¿La parte se puede compartir? | **No.** Cada todo tiene la suya. | **Sí.** Varios todos, la misma parte. |
> | ¿Quién decide qué parte concreta usa? | El todo, adentro | **Quien crea el todo**, desde afuera |
>
> Y esa segunda fila tiene una consecuencia muy grande que todavía no vamos a nombrar:
>
> **cuando la parte entra por el constructor, quien arma el objeto puede pasarle una parte distinta.** Un motor de prueba. Un conductor falso. Otra implementación entera.
>
> Guardate esa idea. Cuando lleguemos a la unidad 2 vas a descubrir que acabás de escribir uno de los principios más importantes del diseño de software, y que sólo te faltaba el nombre.

---

## Ejercicio en vivo 2 — Auto, Motor y Conductor

Escribí las tres clases, en TypeScript, respetando las dos relaciones:

- `Motor`: `cilindrada` (privada), método `arrancar()`
- `Conductor`: `nombre` (privado), método `getNombre()`
- `Auto`: `marca` (privada), **compone** un `Motor` y **agrega** un `Conductor`. Método `encender()` que arranca el motor y avisa quién maneja.

Después dibujá el diagrama de los tres.

**Y contestame:** ¿por qué `Auto` no puede recibir el `Motor` por constructor **también**? ¿Qué se rompería del modelo si lo hiciera?

---

## Ejercicio en vivo 2 — Resolución

```typescript
class Motor {
  constructor(private cilindrada: number) {}

  arrancar(): void {
    console.log(`Motor ${this.cilindrada} en marcha`);
  }
}

class Conductor {
  constructor(private nombre: string) {}

  getNombre(): string {
    return this.nombre;
  }
}

class Auto {
  private motor: Motor;
  private conductor: Conductor;

  constructor(private marca: string, conductor: Conductor) {
    this.motor = new Motor(1600);   // COMPOSICIÓN: el auto crea su motor
    this.conductor = conductor;      // AGREGACIÓN: el conductor viene de afuera
  }

  encender(): void {
    this.motor.arrancar();
    console.log(`${this.marca} manejado por ${this.conductor.getNombre()}`);
  }
}

const juan = new Conductor("Juan");
const auto = new Auto("Fiat", juan);
auto.encender();
```

> [IMAGEN: tres cajas de tres compartimentos. En el centro-izquierda, la caja "Auto" con atributos "- marca: string", "- motor: Motor", "- conductor: Conductor" y método "+ encender(): void". Arriba a la derecha, la caja "Motor" con atributo "- cilindrada: number" y método "+ arrancar(): void". Abajo a la derecha, la caja "Conductor" con atributo "- nombre: string" y método "+ getNombre(): string". Desde el borde derecho de "Auto" sale una línea llena hacia "Motor" que arranca con un rombo RELLENO pegado a la caja "Auto"; sobre la línea, cerca de "Motor", el número "1". Desde el borde derecho de "Auto" sale otra línea llena hacia "Conductor" que arranca con un rombo HUECO pegado a la caja "Auto"; sobre esa línea, cerca de "Conductor", el texto "0..1".]

**Por qué el `Motor` no entra por constructor:**

Si escribieras `constructor(marca: string, motor: Motor, conductor: Conductor)`, dos cosas se romperían:

1. **El mismo motor podría estar en dos autos.** `new Auto("Fiat", m, juan)` y `new Auto("Ford", m, ana)` — y un motor físico no puede estar en dos autos a la vez. El modelo pasaría a permitir algo imposible en el mundo real.

2. **Podrías crear un auto sin motor**, o cambiárselo desde afuera en cualquier momento. La regla "todo auto nace con su motor y es ése" dejaría de estar garantizada por el código.

> **La idea de fondo:** la composición no es sólo una forma de dibujar la flecha. Es el código **impidiendo** un estado imposible.
>
> Es lo mismo que venimos haciendo desde el primer día con `private`: el objeto se defiende solo.

---

## Multiplicidad

Falta un dato. Las flechas dicen **qué** relación hay, pero no **cuántos**.

Un socio tiene libros. ¿Uno? ¿Tres? ¿Ninguno también vale?

Eso se escribe **en los extremos de la línea**, y son cuatro formas:

| Se escribe | Significa |
|---|---|
| `1` | Exactamente uno. Obligatorio. |
| `0..1` | Cero o uno. **Opcional.** |
| `1..*` | Uno o más. Al menos uno. |
| `*` | Cualquier cantidad, cero incluido. |

Y también existe el rango exacto: `0..3` — entre cero y tres.

> [IMAGEN: dos cajas rectangulares simples, "Socio" a la izquierda y "Libro" a la derecha, unidas por una línea llena horizontal que arranca con un rombo hueco pegado a la caja "Socio". Sobre el extremo izquierdo de la línea, junto a "Socio", el número "1". Sobre el extremo derecho, junto a "Libro", el texto "0..3". Debajo de la línea, centrada, la palabra "presta" en itálica con una pequeña punta de flecha que indica que se lee de izquierda a derecha.]

**Ese diagrama se lee así:** *"un socio presta entre cero y tres libros"*.

Y fijate el detalle: el `0..3` **es la regla de negocio de tu ejercicio**, la del máximo de 3 libros. Está dibujada. Cualquiera que mire ese diagrama sabe la regla sin abrir el código.

> **Por eso la multiplicidad no es decoración.** El `0..3` del diagrama es el `if (this.librosPrestados.length >= this.MAXIMO)` del código. **Son la misma regla, en dos idiomas.**

---

## Pará y pensá — poné las multiplicidades

Estas son relaciones que ya escribiste vos. Poné la multiplicidad de cada extremo y, en cada caso, **justificá el extremo que te parezca más discutible**.

1. `Veterinaria` — `Animal`
2. `Auto` — `Motor`
3. `Pedido` — `ItemPedido`
4. `Persona` — `DNI`

> [RESPUESTA: ocultar, permitir ver con un click]
>
> | Relación | Multiplicidad | El extremo discutible |
> |---|---|---|
> | Veterinaria — Animal | `1` — `*` | El `*` del lado del animal: una veterinaria **recién abierta tiene cero animales** y sigue siendo válida. Por eso `*` y no `1..*`. |
> | Auto — Motor | `1` — `1` | Ninguno, y por eso está bueno: es el caso limpio. Un auto, un motor, sin excepciones. |
> | Pedido — ItemPedido | `1` — `1..*` | El `1..*`: **un pedido vacío no es un pedido.** Si permitieras `*`, estarías modelando pedidos de nada. |
> | Persona — DNI | `1` — `1` | Discutible en serio: un recién nacido todavía no tiene DNI. Si tu sistema los registra, es `0..1`. |
>
> **Lo que quiero que veas:** en tres de los cuatro casos, elegir entre `*` y `1..*` **no es una decisión técnica**. Es una pregunta sobre el negocio.
>
> ¿Puede existir un pedido sin ítems? ¿Una veterinaria sin animales? Esas preguntas **no las contesta el programador**: las contesta el que te pidió el sistema.
>
> Y si nadie te las contesta, las escribís en el diagrama como suposición y las preguntás. **Un diagrama es también una lista de preguntas.**

---

## Cómo elegir la flecha — el árbol de decisión

Todo lo de hoy, en tres preguntas, en orden:

**1. ¿"B es un A"?**
   → Sí, y A tiene código real para dar → **herencia** (línea llena + triángulo)
   → Sí, pero A es sólo un contrato sin código → **implementación** (línea punteada + triángulo)

**2. ¿"A tiene un B"?** → Es un rombo. Pasá a la 3.

**3. ¿Quién crea el B?**
   → Lo crea A adentro, con `new` → **composición** (rombo lleno ♦)
   → Se lo pasan desde afuera → **agregación** (rombo vacío ◇)

> [IMAGEN: un diagrama de flujo vertical con forma de árbol. Arriba, un rombo de decisión con el texto "¿B es un A?". De su salida "SÍ" baja a un segundo rombo con el texto "¿A tiene código real para compartir?", que se abre en dos cajas terminales: "SÍ → HERENCIA (línea llena, triángulo hueco)" e "NO → IMPLEMENTACIÓN (línea punteada, triángulo hueco)". De la salida "NO" del primer rombo baja a un tercer rombo con el texto "¿Quién crea el B?", que se abre en dos cajas terminales: "Lo crea A adentro → COMPOSICIÓN (rombo relleno)" y "Se lo pasan de afuera → AGREGACIÓN (rombo hueco)".]

**Guardate este árbol.** Es literalmente lo que vas a necesitar en el parcial cuando te den un enunciado en prosa y te pidan el diagrama.

---

## Ejercicio en vivo 3 — La veterinaria, dibujada

Éste es el grande. Vas a dibujar el diagrama completo del sistema de veterinaria — **el mismo que escribiste vos de tarea la clase pasada.**

El sistema tiene:

- `Animal` **abstracta**: `nombre`, `edad`, `describirse()` concreto, `hacerSonido()` abstracto
- `Perro`, `Gato`, `Pajaro` que extienden `Animal`
- Interfaces `Vacunable` y `Adiestrable`
- `Perro` implementa las dos. `Gato` sólo `Vacunable`. `Pajaro` ninguna.
- `Veterinaria` con un registro `Animal[]`

**Dibujalo entero:** las cajas con sus tres pisos, las visibilidades, la itálica donde corresponda, las cuatro flechas y las multiplicidades.

**Y después contestá:** entre `Veterinaria` y `Animal`, ¿rombo lleno o vacío? **Justificalo con el criterio del `new`, no con la intuición.**

---

## Ejercicio en vivo 3 — Resolución

> [IMAGEN: diagrama de clases completo en tres niveles.
>
> ARRIBA A LA DERECHA, dos cajas chicas de interfaz, una al lado de la otra. La primera tiene «interface» sobre el nombre "Vacunable" en itálica, y debajo el método "+ vacunar(vacuna: string): void" en itálica. La segunda tiene «interface» sobre el nombre "Adiestrable" en itálica, y debajo "+ entrenar(orden: string): void" en itálica.
>
> ARRIBA A LA IZQUIERDA, la caja "Animal" con el nombre en itálica; compartimento de atributos con "# nombre: string" y "# edad: number"; compartimento de métodos con "+ describirse(): void" y "+ getNombre(): string" en texto normal, y "+ hacerSonido(): void" en itálica.
>
> AL MEDIO, tres cajas alineadas horizontalmente: "Perro" con "- raza: string" y los métodos "+ hacerSonido(): void", "+ vacunar(vacuna: string): void", "+ entrenar(orden: string): void"; "Gato" con "- color: string" y los métodos "+ hacerSonido(): void", "+ vacunar(vacuna: string): void"; "Pajaro" con "- especie: string" y el método "+ hacerSonido(): void".
>
> FLECHAS DE HERENCIA: de cada una de las tres cajas del medio sale una línea llena hacia arriba que termina en un triángulo hueco apoyado en el borde inferior de "Animal".
>
> FLECHAS DE IMPLEMENTACIÓN: de "Perro" salen dos líneas punteadas hacia arriba a la derecha, una a "Vacunable" y otra a "Adiestrable", ambas terminando en triángulo hueco. De "Gato" sale una sola línea punteada hacia "Vacunable", terminando en triángulo hueco.
>
> ABAJO A LA IZQUIERDA, la caja "Veterinaria" con atributos "- nombre: string" y "- registro: Animal[]", y métodos "+ registrar(animal: Animal): void", "+ atenderATodos(): void", "+ vacunarATodos(vacuna: string): void".
>
> RELACIÓN FINAL: desde el borde superior de "Veterinaria" sale una línea llena hacia la caja "Animal", que arranca con un rombo HUECO pegado a "Veterinaria". Sobre el extremo de la línea junto a "Veterinaria" va el número "1", y sobre el extremo junto a "Animal" va un asterisco "*".]

**La respuesta al rombo: es HUECO — agregación.** Y el criterio es el del `new`:

```typescript
registrar(animal: Animal): void {
  this.registro.push(animal);      // ← el animal LLEGA. No se crea acá.
}

vet.registrar(new Perro("Fido", 3, "Labrador"));
//            ↑ el new está AFUERA de Veterinaria
```

La veterinaria **nunca escribe `new Perro()`**. Los animales existen antes de entrar y siguen existiendo si la veterinaria cierra. **Agregación.**

**Tres cosas para que mires del diagrama entero:**

1. **Las tres flechas de herencia van al mismo lugar.** Eso es el `Animal[]` funcionando: si los tres apuntan a `Animal`, los tres entran en un array de `Animal`. **El polimorfismo se ve dibujado.**

2. **`Pajaro` no tiene ninguna línea punteada.** Y ahí está, en un vistazo, todo el problema del `vacunarATodos()` que te trabó: la caja `Pajaro` no llega a `Vacunable`. **Lo que en código te obligó a un `if`, en el diagrama es un espacio en blanco.**

3. **Las líneas punteadas se cruzan con la jerarquía.** `Perro` sube a `Animal` y también a dos interfaces. Eso es exactamente lo que la clase pasada llamamos *capacidades cruzadas* — y el dibujo lo muestra sin que haya que explicarlo.

> **Esto es lo que gana un diagrama:** el problema que te bloqueó una tarde escribiendo código, acá se ve en tres segundos.

---

## Checkpoint — las cuatro, de memoria

Sin mirar para arriba. Para cada una: **cómo es la línea, qué tiene en la punta, y de qué lado va ese símbolo.**

1. Herencia
2. Implementación
3. Composición
4. Agregación

Y la de desempate: **¿qué símbolo va del lado del "todo", y por qué del lado del todo y no de la parte?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> | | Línea | Punta | De qué lado va el símbolo |
> |---|---|---|---|
> | **Herencia** | Llena | Triángulo hueco | Pegado al **padre** |
> | **Implementación** | Punteada | Triángulo hueco | Pegado a la **interfaz** |
> | **Composición** | Llena | Rombo **relleno** | Pegado al **todo** |
> | **Agregación** | Llena | Rombo **hueco** | Pegado al **todo** |
>
> **El rombo va del lado del todo.** Y el motivo es que el rombo **no describe a la parte: describe al todo.**
>
> Un `Motor` no es "un motor compuesto". Es un motor común. Lo que la relación dice es que **el `Auto` lo posee de forma fuerte**. La afirmación es sobre el auto, así que el símbolo se apoya en el auto.
>
> Es la misma lógica que el triángulo: el triángulo se apoya en el padre porque marca **quién es lo general**. En UML, el símbolo siempre se pega del lado del que manda la relación.

---

## Ejercicio en vivo 4 — Del diagrama al código

Hasta ahora fuimos del código al diagrama. Ahora al revés, que es como se trabaja de verdad.

**No hay enunciado en palabras. Sólo el dibujo.** Escribí las clases en TypeScript.

> [IMAGEN: diagrama de clases con cuatro cajas.
>
> ARRIBA, la caja "Reproducible" con «interface» sobre el nombre en itálica, y en el compartimento de métodos "+ play(): void" y "+ pause(): void", ambos en itálica.
>
> A LA IZQUIERDA, la caja "Playlist" con atributos "- nombre: string" y "- canciones: Cancion[]", y métodos "+ agregar(c: Cancion): void" y "+ reproducirTodo(): void".
>
> AL CENTRO, la caja "Cancion" con atributos "- titulo: string", "- duracion: number", "- artista: Artista", y métodos "+ play(): void", "+ pause(): void", "+ getTitulo(): string".
>
> A LA DERECHA, la caja "Artista" con atributo "- nombre: string" y método "+ getNombre(): string".
>
> FLECHAS: de "Cancion" sale una línea punteada hacia arriba a "Reproducible", terminando en triángulo hueco. De "Playlist" sale una línea llena hacia "Cancion" que arranca con un rombo HUECO pegado a "Playlist"; multiplicidad "1" del lado de Playlist y "*" del lado de Cancion. De "Cancion" sale una línea llena hacia "Artista" que arranca con un rombo HUECO pegado a "Cancion"; multiplicidad "*" del lado de Cancion y "1" del lado de Artista.]

**Y contestame, que es la parte que importa:** los dos rombos son huecos. **¿Por qué ninguno de los dos es lleno?** Dalo vuelta: ¿qué tendría que cambiar en el enunciado para que el rombo entre `Playlist` y `Cancion` pasara a ser relleno?

---

## Ejercicio en vivo 4 — Resolución

```typescript
interface Reproducible {
  play(): void;
  pause(): void;
}

class Artista {
  constructor(private nombre: string) {}

  getNombre(): string {
    return this.nombre;
  }
}

class Cancion implements Reproducible {
  private artista: Artista;

  constructor(
    private titulo: string,
    private duracion: number,
    artista: Artista            // ← llega de afuera: agregación
  ) {
    this.artista = artista;
  }

  play(): void  { console.log(`Sonando: ${this.titulo}`); }
  pause(): void { console.log(`Pausa: ${this.titulo}`); }

  getTitulo(): string { return this.titulo; }
}

class Playlist {
  private canciones: Cancion[] = [];

  constructor(private nombre: string) {}

  agregar(c: Cancion): void {    // ← la canción llega de afuera: agregación
    this.canciones.push(c);
  }

  reproducirTodo(): void {
    this.canciones.forEach(c => c.play());
  }
}
```

**Por qué los dos rombos son huecos:**

- **Playlist → Cancion:** si borrás la playlist, **las canciones siguen existiendo**. Están en otras playlists, en tu biblioteca, en el disco. Y encima **la misma canción está en varias playlists a la vez** — que es justamente lo que la composición no permite.

- **Cancion → Artista:** el artista existe sin la canción, y **un artista tiene muchas canciones**. Fijate la multiplicidad `*` del lado de `Cancion`: eso ya te está diciendo que hay muchas canciones apuntando al mismo artista. **Un rombo relleno con `*` del lado de la parte es casi siempre un error**, porque significaría que muchos "todos" son dueños exclusivos de la misma parte. No se puede.

**Y qué tendría que cambiar para que fuera relleno:**

Que la playlist **creara** sus canciones y que no se pudieran compartir. Por ejemplo, si `Playlist` guardara *copias privadas*:

```typescript
agregar(titulo: string, duracion: number, a: Artista): void {
  this.canciones.push(new Cancion(titulo, duracion, a));   // ← el new adentro
}
```

Ahí el `new` está adentro de `Playlist`, cada canción pertenece a una sola playlist y desaparece con ella. **Rombo relleno.**

> **Fijate qué pasó recién:** cambiar el rombo de hueco a relleno **cambió la firma del método**. Pasó de recibir un `Cancion` a recibir los datos para construirla.
>
> Eso es lo que quiero que te lleves de hoy: **la flecha del diagrama y la forma del constructor son la misma decisión.** No son dos trabajos, es uno.

---

## Recap — lo de hoy

| Concepto | La regla en una línea |
|---|---|
| **Caja** | Tres pisos: nombre, atributos, métodos. Los métodos siempre con paréntesis. |
| **Visibilidad** | `+` público, `-` privado, `#` protegido. Por defecto, `-`. |
| **Itálica** | Clase o método abstracto. |
| **Herencia** | Línea llena + triángulo hueco, pegado al padre. "Es un". |
| **Implementación** | Línea **punteada** + triángulo hueco, pegado a la interfaz. "Puede". |
| **Composición** | Rombo **relleno** del lado del todo. El todo hace `new` de la parte. |
| **Agregación** | Rombo **hueco** del lado del todo. La parte llega por parámetro. |
| **Multiplicidad** | `1`, `0..1`, `1..*`, `*`. Es la regla de negocio, dibujada. |

**Y la idea que atraviesa todo:**

> El diagrama no es un dibujo del código. **Es la decisión que se toma antes del código** — y cada símbolo tiene una consecuencia concreta en cómo se escribe el constructor.
>
> Rombo relleno → `new` adentro.
> Rombo hueco → parámetro.
> Triángulo → `extends` o `implements`.
>
> **Dibujar bien es diseñar bien. No es dibujar bonito.**

---

## Después de clase

Siete ejercicios. El primero es de repaso; del segundo al sexto son de lectura y notación; el séptimo es el que de verdad practica lo que se toma en el parcial.

**Una regla para los que llevan diagrama (del 2 al 7):** va **en papel, a mano**. No busques una herramienta. En el parcial vas a tener una hoja y una birome, y dibujar a mano es más rápido de lo que creés.

---

## Después de clase — Ejercicio 1: Repaso de arrays

**Consigna:** este código tiene **cuatro** problemas. Encontralos, explicá cada uno y reescribilo.

```typescript
class ListaDeTareas {
  private tareas: string[];

  agregar(tarea: string): void {
    this.tareas.push(tarea);
  }

  eliminar(tarea: string): void {
    const i = this.tareas.indexOf(tarea);
    this.tareas.splice(i, 1);
  }

  mostrarTodas(): void {
    for (let i = 0; i <= this.tareas.length; i++) {
      console.log(this.tareas[i]);
    }
  }

  enMayusculas(): void {
    this.tareas.map(t => t.toUpperCase());
  }
}
```

**Y contestá:** de los cuatro, **¿cuál es el más peligroso y por qué?** Pista: no es el que rompe el programa.

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Problema 1 — `tareas` nunca se inicializa.**
> Arranca en `undefined`, así que el primer `push()` revienta con *"Cannot read properties of undefined"*. Va `private tareas: string[] = [];`.
>
> **Problema 2 — `eliminar()` no chequea el `-1`.**
> Si la tarea no está en la lista, `indexOf` devuelve `-1`, y `splice(-1, 1)` **borra el último elemento**. Le pedís borrar algo que no existe y te borra otra cosa.
>
> **Problema 3 — el `for` usa `<=`.**
> Con `length` 3, la última vuelta pide el índice 3, que no existe → imprime `undefined`. Es el error de uno.
>
> **Problema 4 — `map()` sin guardar el resultado.**
> `map` no modifica el original: devuelve un array nuevo que acá **se tira a la basura**. El método no hace absolutamente nada.
>
> ---
>
> **El más peligroso es el 2**, y por lejos.
>
> Mirá por qué, comparándolos:
>
> | | Qué pasa | Cuándo te enterás |
> |---|---|---|
> | 1 — sin inicializar | El programa **explota** | Al instante, con un error clarísimo |
> | 3 — el `<=` | Imprime un `undefined` de más | Enseguida, se ve en la consola |
> | 4 — el `map` | No hace nada | Rápido: llamás al método y no cambia nada |
> | 2 — el `-1` | **Borra el dato equivocado, sin avisar** | **Nunca** |
>
> Los tres primeros son ruidosos: rompen, o se notan mirando. El segundo **hace algo plausible**. La lista queda con la cantidad correcta de elementos, todos válidos, y nadie sospecha nada — hasta que alguien pregunta dónde está su última tarea.
>
> > **La lección: el bug más grave no es el que rompe el programa. Es el que deja los datos mal y sigue andando.**
>
> Un programa que explota te avisa. Uno que corrompe datos en silencio, no.
>
> **Corregido:**
>
> ```typescript
> class ListaDeTareas {
>   private tareas: string[] = [];                    // 1
>
>   agregar(tarea: string): void {
>     this.tareas.push(tarea);
>   }
>
>   eliminar(tarea: string): void {
>     const i = this.tareas.indexOf(tarea);
>     if (i === -1) {                                 // 2
>       console.log(`"${tarea}" no está en la lista`);
>       return;
>     }
>     this.tareas.splice(i, 1);
>   }
>
>   mostrarTodas(): void {
>     this.tareas.forEach(t => console.log(t));       // 3
>   }
>
>   enMayusculas(): string[] {                        // 4
>     return this.tareas.map(t => t.toUpperCase());
>   }
> }
> ```
>
> **Fijate el arreglo del 3:** en vez de corregir el `<=` por `<`, saqué el `for` entero y puse `forEach`. **Arreglar el síntoma te deja el error latente para la próxima vez que escribas un `for`. Sacar el índice de la ecuación lo elimina para siempre.**
>
> Y en el 4 el método pasó a **devolver** el array (`: string[]`) en vez de `void`. Ése era el punto: un método que transforma tiene que entregarte el resultado, o no sirve para nada.

---

## Después de clase — Ejercicio 2: Del diagrama al código

**Consigna:** escribí en TypeScript las clases que representa este diagrama. Respetá visibilidades, tipos y relaciones.

> [IMAGEN: diagrama con tres cajas. ARRIBA, la caja "Empleado" con el nombre en itálica; atributos "# nombre: string" y "# sueldoBase: number"; métodos "+ mostrarDatos(): void" en texto normal y "+ calcularSueldo(): number" en itálica. ABAJO A LA IZQUIERDA, la caja "Vendedor" con atributo "- comision: number" y método "+ calcularSueldo(): number". ABAJO A LA DERECHA, la caja "Gerente" con atributo "- bono: number" y método "+ calcularSueldo(): number". De cada caja de abajo sale una línea llena hacia arriba terminada en triángulo hueco apoyado en "Empleado". A LA DERECHA DEL TODO, separada, la caja "Empresa" con atributos "- razonSocial: string" y "- empleados: Empleado[]", y método "+ contratar(e: Empleado): void". De "Empresa" sale una línea llena hacia "Empleado" que arranca con un rombo HUECO pegado a "Empresa"; multiplicidad "1" del lado de Empresa y "*" del lado de Empleado.]

**Y contestá:** ¿por qué `calcularSueldo()` está en itálica en `Empleado` pero en texto normal en las dos hijas? ¿Qué pasaría si en `Empleado` estuviera en texto normal?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> ```typescript
> abstract class Empleado {
>   constructor(protected nombre: string, protected sueldoBase: number) {}
>
>   mostrarDatos(): void {
>     console.log(`${this.nombre} — cobra $${this.calcularSueldo()}`);
>   }
>
>   abstract calcularSueldo(): number;
> }
>
> class Vendedor extends Empleado {
>   constructor(nombre: string, sueldoBase: number, private comision: number) {
>     super(nombre, sueldoBase);
>   }
>
>   calcularSueldo(): number {
>     return this.sueldoBase + this.comision;
>   }
> }
>
> class Gerente extends Empleado {
>   constructor(nombre: string, sueldoBase: number, private bono: number) {
>     super(nombre, sueldoBase);
>   }
>
>   calcularSueldo(): number {
>     return this.sueldoBase + this.bono;
>   }
> }
>
> class Empresa {
>   private empleados: Empleado[] = [];
>
>   constructor(private razonSocial: string) {}
>
>   contratar(e: Empleado): void {     // rombo hueco: el empleado llega de afuera
>     this.empleados.push(e);
>   }
> }
> ```
>
> **Por qué la itálica cambia:**
>
> En `Empleado` el método está en itálica porque es **abstracto**: existe la obligación, no el código. Un "empleado genérico" no tiene forma de cobrar — es el mismo `return 0` inventado de la clase 3.
>
> En las hijas está en texto normal porque **ahí sí hay código**. Cada una sabe su fórmula.
>
> **Y si en `Empleado` estuviera en normal:** significaría que `Empleado` tiene una implementación propia de `calcularSueldo()`. O sea:
>
> - `Empleado` se podría instanciar.
> - Una hija que se olvide de implementarlo **heredaría esa fórmula en silencio**, y nadie se enteraría.
>
> Es exactamente el bug que las clases abstractas vinieron a matar. **La itálica es el símbolo de "el compilador te va a frenar".**

---

## Después de clase — Ejercicio 3: Dibujá lo que ya escribiste

**Consigna:** agarrá **tu propia** solución del ejercicio de medios de pago (`MedioDePago`, `Efectivo`, `Tarjeta`, `Cripto`) y dibujá el diagrama completo, con visibilidades, itálicas y flechas.

Agregá también la función `cobrar(medio: MedioDePago, monto: number)`.

**Y contestá:** `cobrar()` no es un método de ninguna clase. **¿Cómo la representarías en el diagrama?** Y más importante: **¿por qué el hecho de que no entre bien en ningún lado te está diciendo algo sobre el diseño?**

> [RESPUESTA: ocultar, permitir ver con un click]
>
> > [IMAGEN: diagrama con cuatro cajas. ARRIBA, la caja "MedioDePago" con «interface» sobre el nombre en itálica, y en el compartimento de métodos "+ pagar(monto: number): boolean" en itálica. ABAJO, tres cajas alineadas horizontalmente: "Efectivo", sin atributos, con método "+ pagar(monto: number): boolean"; "Tarjeta" con atributo "- limite: number" y método "+ pagar(monto: number): boolean"; "Cripto" con atributo "- cotizacion: number" y método "+ pagar(monto: number): boolean". De cada una de las tres sale una línea punteada hacia arriba terminada en triángulo hueco apoyado en "MedioDePago".]
>
> **Sobre `cobrar()`:** en UML una función suelta no tiene caja propia. Hay dos salidas honestas:
>
> 1. Ponerla como método de la clase que la usa de verdad — una `Caja`, un `Carrito`, un `Sistema`. Y ahí la relación con `MedioDePago` sería **agregación** (el medio llega por parámetro).
> 2. Dejarla afuera con una nota, si es realmente una función auxiliar.
>
> **Y ahora la parte que importa.**
>
> Que `cobrar()` no encuentre dónde vivir **es una señal**. Está diciendo que en tu modelo falta el objeto que hace la venta. Alguien cobra: una caja registradora, un sistema de facturación, un carrito. Ese objeto no lo modelaste.
>
> > **Ésa es una de las cosas más útiles que hace un diagrama: mostrarte lo que falta.**
>
> Con el código suelto no lo hubieras notado nunca — una función en el aire compila igual. En un diagrama, una función sin caja **queda flotando**, y lo ves.
>
> Cuando dibujes y algo no entre en ningún lado, no lo fuerces. **Preguntate qué clase te está faltando.**

---

## Después de clase — Ejercicio 4: ¿Rombo lleno o vacío?

**Consigna:** para cada par, decidí el tipo de rombo. Escribí **las dos justificaciones**: el test de la destrucción y el criterio del `new`.

1. `Casa` — `Habitacion`
2. `Curso` — `Alumno`
3. `Factura` — `LineaDeFactura`
4. `Equipo` — `Jugador`
5. `Cuerpo` — `Corazon`
6. `Biblioteca` — `Socio`

> [RESPUESTA: ocultar, permitir ver con un click]
>
> | Par | Rombo | Test de la destrucción | Criterio del `new` |
> |---|---|---|---|
> | Casa — Habitacion | ♦ Lleno | Se demuele la casa, la habitación no existe en ningún lado | `this.habitaciones.push(new Habitacion(...))` adentro de `Casa` |
> | Curso — Alumno | ◇ Vacío | Se cierra el curso, el alumno sigue siendo alumno | `curso.inscribir(alumno)` — el alumno ya existía |
> | Factura — LineaDeFactura | ♦ Lleno | Se anula la factura, sus líneas no significan nada | `factura.agregarLinea(producto, cantidad)` crea la línea adentro |
> | Equipo — Jugador | ◇ Vacío | Se disuelve el equipo, el jugador se va a otro | `equipo.fichar(jugador)` — llega de afuera |
> | Cuerpo — Corazon | ♦ Lleno | El caso más literal de todos | El cuerpo no "recibe" un corazón: nace con él |
> | Biblioteca — Socio | ◇ Vacío | Cierra la biblioteca, la persona sigue viva | `biblioteca.asociar(persona)` |
>
> **El patrón que se repite:** los rombos llenos son **partes internas** que no tienen identidad propia — una habitación no se muda, una línea de factura no existe sola. Los vacíos son **entidades con vida propia** que entran y salen: personas, jugadores, alumnos.
>
> **Y una pista muy práctica para el parcial:** cuando la parte es una **persona**, casi siempre es agregación. Las personas no se crean ni se destruyen con el sistema que las registra.
>
> *(El caso 5 es una trampa amable: es composición en el sentido biológico, pero si estuvieras modelando un sistema de trasplantes, el corazón **sí** sobrevive al cuerpo y pasa a ser agregación. **El dominio manda, siempre.** Si escribiste eso, está mejor que la respuesta de la tabla.)*

---

## Después de clase — Ejercicio 5: Detectá los errores del diagrama

**Consigna:** este diagrama tiene **cinco** errores. Encontralos y explicá cada uno.

> [IMAGEN: diagrama con cuatro cajas y varias flechas.
>
> ARRIBA A LA IZQUIERDA, la caja "Animal" con el nombre en TEXTO NORMAL (no itálica); atributos "+ nombre: string" y "+ edad: number"; métodos "+ describirse(): void" y "+ hacerSonido(): void", los dos en texto normal.
>
> ARRIBA A LA DERECHA, la caja "Vacunable" con «interface» sobre el nombre, y en el compartimento del medio el atributo "- fechaUltimaVacuna: Date", y en el de abajo el método "+ vacunar(vacuna: string): void".
>
> ABAJO A LA IZQUIERDA, la caja "Perro" con atributo "- raza: string" y método "+ hacerSonido". Sale de ella una línea PUNTEADA hacia "Animal" terminada en triángulo hueco, y una línea LLENA hacia "Vacunable" terminada en triángulo hueco.
>
> ABAJO A LA DERECHA, la caja "Veterinaria" con atributo "- registro: Animal[]". De "Animal" sale una línea llena hacia "Veterinaria" que arranca con un rombo RELLENO pegado a la caja "ANIMAL"; sobre el extremo junto a "Veterinaria" va el número "1" y sobre el extremo junto a "Animal" va "1..*".]

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **1. Las dos flechas de `Perro` están intercambiadas.** La que va a `Animal` (una clase) tiene que ser **llena**, y la que va a `Vacunable` (una interfaz) tiene que ser **punteada**. Están al revés.
>
> **2. `Animal` debería estar en itálica, y `hacerSonido()` también.** Si `Animal` no es abstracta, se puede hacer `new Animal()` — y ya sabemos qué pasa: un animal genérico que hace un sonido inventado.
>
> **3. Los atributos de `Animal` están en `+`.** Deberían ser `#` (protegidos): las hijas los necesitan, pero nadie de afuera. Tal como está, cualquiera hace `perro.edad = -5`.
>
> **4. La interfaz `Vacunable` tiene un atributo.** Una interfaz es un contrato de comportamiento: no guarda estado. Si hace falta guardar `fechaUltimaVacuna` de verdad, eso pide una **clase abstracta**, no una interfaz. *(Éste es exactamente el error (a) del ejercicio de la clase pasada — el mismo, dibujado.)*
>
> **5. El rombo está del lado equivocado.** La veterinaria contiene animales, no al revés. El rombo va pegado a **`Veterinaria`**. Y además tendría que ser **hueco**: los animales llegan de afuera con `registrar()`.
>
> **Bonus, y suma:** a `hacerSonido()` en `Perro` le faltan los paréntesis, y el `1..*` obligaría a que toda veterinaria tenga al menos un animal — debería ser `*`.
>
> **Fijate un detalle:** los errores 2, 3 y 4 son errores que ya sabías detectar **en código**. El único nuevo es el 5. **Dibujar no te pide criterios nuevos: te pide los mismos, en otro idioma.**

---

## Después de clase — Ejercicio 6: Multiplicidades

**Consigna:** poné la multiplicidad de los dos extremos de cada relación, y **justificá cada elección con una frase sobre el negocio** — no sobre el código.

1. `Cuenta bancaria` — `Titular`
2. `Post` — `Comentario`
3. `Alumno` — `Materia`
4. `Empleado` — `Jefe`

> [RESPUESTA: ocultar, permitir ver con un click]
>
> | Relación | Multiplicidad | La frase del negocio |
> |---|---|---|
> | Cuenta — Titular | `*` — `1..*` | Una cuenta tiene al menos un titular (nunca cero), y puede ser conjunta. Y un titular puede tener varias cuentas. |
> | Post — Comentario | `1` — `*` | Un post puede no tener ningún comentario, y sigue siendo un post. Cada comentario pertenece a un solo post. |
> | Alumno — Materia | `*` — `*` | Un alumno cursa varias materias y una materia tiene varios alumnos. **Muchos a muchos.** |
> | Empleado — Jefe | `*` — `0..1` | Cada empleado tiene a lo sumo un jefe. El `0..1` es clave: **alguien arriba de todo no tiene jefe.** |
>
> **Tres cosas para mirar:**
>
> **El `0..1` del caso 4** es el que más se olvida. Si ponés `1`, tu modelo dice que el director general también tiene jefe, y el sistema no va a poder representar a la persona que está arriba de todo.
>
> **El `*` — `*` del caso 3** es el único de los cuatro que **no se puede escribir con un array y listo**. Si `Alumno` tiene `materias: Materia[]` y `Materia` tiene `alumnos: Alumno[]`, tenés la misma información guardada dos veces — y el día que se desincronizan, tenés un problema. La solución es una clase en el medio (`Inscripcion`), que además es donde vive la nota. **Lo vas a necesitar en el parcial.**
>
> **Y la diferencia entre `*` y `1..*`:** no es un detalle. Poner `1..*` donde va `*` significa que tu sistema **no puede representar el estado inicial** — la veterinaria el día que abre, el post recién publicado, la playlist vacía. Casi siempre, el estado inicial es cero.

---

## Después de clase — Ejercicio 7: Integrador — El restaurante

**Consigna:** modelá el sistema de un restaurante. **El diagrama va PRIMERO**, en papel, antes de escribir una sola línea de código. Después, escribilo en TypeScript.

**El enunciado, en prosa:**

> Un restaurante toma pedidos. Cada **pedido** se hace en una **mesa**, lo atiende un **mozo** y tiene uno o más **ítems**. Cada ítem indica una **cantidad** y apunta a un **producto** del menú, que tiene nombre y precio. El pedido sabe calcular su total.
>
> Los **empleados** del restaurante tienen nombre y legajo, y cada tipo cobra distinto: el **mozo** cobra sueldo base más propinas, el **cocinero** cobra sueldo base más un plus por antigüedad. Ningún empleado "genérico" trabaja en el restaurante.
>
> Además, algunos empleados están habilitados para **cerrar la caja** al final del día. Los mozos sí; los cocineros no.
>
> Un pedido se paga con un **medio de pago** — el mismo que ya modelaste.

**Antes de dibujar, contestá estas tres:**

1. Entre `Pedido` e `ItemPedido`, ¿qué rombo? ¿Y entre `ItemPedido` y `Producto`?
2. "Cerrar la caja" que algunos tienen y otros no, **¿clase abstracta o interfaz?** ¿Por qué?
3. ¿Cuál es la multiplicidad entre `Mozo` y `Pedido`?

> [RESPUESTA: ocultar, permitir ver con un click]
>
> **Las tres preguntas primero:**
>
> **1.** `Pedido` ♦→ `ItemPedido` (**composición**): si se anula el pedido, sus ítems no existen en ningún lado. El pedido los crea. Y `ItemPedido` ◇→ `Producto` (**agregación**): la milanesa del menú existe con o sin ese pedido, y **el mismo producto aparece en cientos de ítems**.
>
> **2.** **Interfaz** (`Cerrable` o `CierraCaja`). Es una capacidad que tienen **algunos** empleados y otros no. Si fuera clase abstracta, tendrías que meterla en la jerarquía — y entonces el cocinero heredaría un `cerrarCaja()` que no debería tener. Es el mismo caso que `Vacunable`.
>
> **3.** `Mozo` `1` — `*` `Pedido`. Un mozo atiende muchos pedidos; cada pedido tiene un solo mozo. Y es `*`, no `1..*`: **un mozo que recién entró al turno no atendió ningún pedido todavía.**
>
> ---
>
> **El diagrama:**
>
> > [IMAGEN: diagrama de clases del restaurante, en tres zonas.
> >
> > ZONA IZQUIERDA (la jerarquía de empleados): arriba, la caja "Empleado" con el nombre en itálica, atributos "# nombre: string" y "# sueldoBase: number", y métodos "+ mostrarDatos(): void" en normal y "+ calcularSueldo(): number" en itálica. Debajo, dos cajas: "Mozo" con atributo "- propinas: number" y métodos "+ calcularSueldo(): number" y "+ cerrarCaja(): void"; y "Cocinero" con atributo "- antiguedad: number" y método "+ calcularSueldo(): number". De cada una sale una línea llena hacia "Empleado" terminada en triángulo hueco. Arriba a la izquierda del todo, la caja de interfaz "Cerrable" con «interface» sobre el nombre en itálica y el método "+ cerrarCaja(): void" en itálica; de "Mozo" sale una línea punteada hacia ella, terminada en triángulo hueco.
> >
> > ZONA CENTRAL (el pedido): la caja "Pedido" con atributos "- fecha: Date", "- mesa: Mesa", "- mozo: Mozo", "- items: ItemPedido[]" y métodos "+ agregarItem(p: Producto, cant: number): void", "+ calcularTotal(): number", "+ pagar(medio: MedioDePago): boolean". Debajo, la caja "ItemPedido" con atributos "- cantidad: number" y "- producto: Producto" y método "+ subtotal(): number". De "Pedido" baja una línea llena hasta "ItemPedido" que arranca con un rombo RELLENO pegado a "Pedido"; multiplicidad "1" junto a Pedido y "1..*" junto a ItemPedido.
> >
> > ZONA DERECHA: la caja "Producto" con atributos "- nombre: string" y "- precio: number" y método "+ getPrecio(): number". De "ItemPedido" sale una línea llena hacia "Producto" que arranca con un rombo HUECO pegado a "ItemPedido"; multiplicidad "*" junto a ItemPedido y "1" junto a Producto. Arriba, la caja "Mesa" con atributo "- numero: number". De "Pedido" sale una línea llena hacia "Mesa" que arranca con un rombo HUECO pegado a "Pedido"; multiplicidad "*" junto a Pedido y "1" junto a Mesa. Abajo a la derecha, la caja de interfaz "MedioDePago" con «interface» sobre el nombre en itálica y el método "+ pagar(monto: number): boolean" en itálica; de "Pedido" sale una línea llena hacia ella que arranca con un rombo HUECO pegado a "Pedido", con multiplicidad "*" junto a Pedido y "1" junto a MedioDePago.
> >
> > CONEXIÓN ENTRE ZONAS: de "Pedido" sale una línea llena hacia "Mozo" que arranca con un rombo HUECO pegado a "Pedido"; multiplicidad "*" junto a Pedido y "1" junto a Mozo.]
>
> **El código:**
>
> ```typescript
> // ---------- Empleados ----------
> interface Cerrable {
>   cerrarCaja(): void;
> }
>
> abstract class Empleado {
>   constructor(protected nombre: string, protected sueldoBase: number) {}
>
>   mostrarDatos(): void {
>     console.log(`${this.nombre} — $${this.calcularSueldo()}`);
>   }
>
>   abstract calcularSueldo(): number;
> }
>
> class Mozo extends Empleado implements Cerrable {
>   constructor(nombre: string, sueldoBase: number, private propinas: number) {
>     super(nombre, sueldoBase);
>   }
>
>   calcularSueldo(): number { return this.sueldoBase + this.propinas; }
>   cerrarCaja(): void       { console.log(`${this.nombre} cerró la caja`); }
> }
>
> class Cocinero extends Empleado {
>   constructor(nombre: string, sueldoBase: number, private antiguedad: number) {
>     super(nombre, sueldoBase);
>   }
>
>   calcularSueldo(): number { return this.sueldoBase + this.antiguedad * 5000; }
> }
>
> // ---------- Menú y mesas ----------
> class Producto {
>   constructor(private nombre: string, private precio: number) {}
>   getPrecio(): number  { return this.precio; }
>   getNombre(): string  { return this.nombre; }
> }
>
> class Mesa {
>   constructor(private numero: number) {}
>   getNumero(): number { return this.numero; }
> }
>
> // ---------- El pedido ----------
> class ItemPedido {
>   constructor(private producto: Producto, private cantidad: number) {}
>
>   subtotal(): number {
>     return this.producto.getPrecio() * this.cantidad;
>   }
> }
>
> class Pedido {
>   private items: ItemPedido[] = [];      // COMPOSICIÓN
>
>   constructor(
>     private mesa: Mesa,                  // AGREGACIÓN: la mesa ya existía
>     private mozo: Mozo                   // AGREGACIÓN: el mozo ya existía
>   ) {}
>
>   agregarItem(producto: Producto, cantidad: number): void {
>     this.items.push(new ItemPedido(producto, cantidad));   // ← el new adentro
>   }
>
>   calcularTotal(): number {
>     return this.items.reduce((total, item) => total + item.subtotal(), 0);
>   }
>
>   pagar(medio: MedioDePago): boolean {   // AGREGACIÓN: el medio llega de afuera
>     return medio.pagar(this.calcularTotal());
>   }
> }
> ```
>
> ---
>
> **Las tres cosas que quiero que mires:**
>
> **1. `agregarItem()` recibe el producto y la cantidad, no un `ItemPedido`.** Ésa es la composición escrita: el `new ItemPedido(...)` está **adentro** de `Pedido`. Nadie puede fabricar un ítem por su cuenta y metérselo.
>
> Compará las dos firmas y vas a ver que la flecha está en la firma:
>
> ```typescript
> agregarItem(producto: Producto, cantidad: number): void   // ♦ composición
> agregarItem(item: ItemPedido): void                        // ◇ agregación
> ```
>
> **2. `Pedido` recibe `mesa` y `mozo` por constructor.** Rombo hueco las dos. La mesa 4 existe desde antes que abra el restaurante y va a seguir existiendo mañana.
>
> **3. `pagar(medio: MedioDePago)` recibe la interfaz, no una clase concreta.** El pedido no sabe ni le importa si le pagan con tarjeta o con cripto. **Es la misma función `cobrar()` que te faltaba en la tarea de la clase 4** — sólo que ahora tiene dónde vivir: adentro de `Pedido`.
>
> ¿Te acordás de la pregunta del ejercicio 3, la de "¿dónde meto `cobrar()`?"? **Ésta es la respuesta.** Faltaba modelar el pedido.

---

## Para pensar hasta la clase que viene

1. En la veterinaria, `atenderATodos()` recorre el registro. Si en vez de tres animales hubiera diez mil, y quisieras buscar **uno por su nombre**, ¿cómo lo harías con un `Animal[]`? ¿Te gusta la respuesta?
2. `Producto` tiene `precio`. El **IVA** es 21% para todos los productos del sistema, no cambia por producto. ¿Dónde lo guardarías? ¿Adentro de cada `Producto`? ¿No te suena a desperdicio guardar el mismo número mil veces?
3. Si tuvieras que llevar la cuenta de **cuántos pedidos** se hicieron hoy en total, ¿de quién sería ese dato? No es de un pedido en particular… pero es de los pedidos.

**La clase que viene:** miembros de clase (`static`), constantes, enums y casteos. Las preguntas 2 y 3 son exactamente el tema — y la 1 la contestamos en la clase de colecciones.

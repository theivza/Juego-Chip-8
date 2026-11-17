# Documentación del desarrollo de mi videojuego para CHIP-8

Este documento está hecho para personas que se interesen por aprender o conocer cómo se hace un videojuego utilizando el lenguaje de desarrollo Octo para CHIP-8.

## Introducción

### CHIP-8
**CHIP-8** es un lenguaje de programación interpretado, desarrollado por Joseph Weisbecker a mediados de la década de 1970. Fue creado para facilitar el desarrollo de videojuegos en los dispositivos de aquella época.

Actualmente, CHIP-8 sirve como una herramienta de aprendizaje para introducirse en el desarrollo de videojuegos, además de permitir conocer cómo se desarrollaban estos en sus primeros años y las limitaciones técnicas que existían. Con el tiempo, se han creado versiones extendidas que incorporan mayores capacidades.

### Octo
**Octo** es un ensamblador de alto nivel creado para facilitar la programación de videojuegos para la máquina virtual CHIP-8. Es el lenguaje que utilicé para desarrollar mi propio videojuego para CHIP-8.

## Entorno de desarrollo

El entorno de desarrollo que utilicé fue **[Octo](https://johnearnest.github.io/Octo/)**. Su interfaz es sencilla. En la parte superior se encuentran los botones y herramientas principales.

El logo de Octo te lleva al repositorio de su creador, donde también puedes encontrar la documentación que utilicé para aprender el lenguaje, dentro de la carpeta llamada [docs](https://github.com/JohnEarnest/Octo/tree/gh-pages/docs).

El botón **Run** inicia el emulador de CHIP-8. Allí puedes comprobar si tu videojuego funciona correctamente o si presenta errores.

El botón **Open** te permite abrir archivos. Puedes cargar una ROM para ejecutarla en el emulador y visualizar su código en hexadecimal, o abrir un archivo `.txt` o `.8o` que contenga código escrito en Octo.

El botón **Share** genera un enlace que permite compartir tu código fuente para que otras personas puedan consultarlo y probarlo.

En **Examples** puedes encontrar ROMs de ejemplo que puedes ejecutar y cuyo código fuente puedes consultar para aprender de ellos.

El botón **Speed** permite aumentar la velocidad de ejecución de las instrucciones. Sin embargo, también existen instrucciones que permiten controlar la velocidad a la que se ejecuta el programa.

En la parte derecha encontrarás un manual con el que puedes aprender el lenguaje y el botón **Toolbox**, que contiene herramientas que pueden ayudarte durante el desarrollo de tu videojuego.

Dentro de **Toolbox**, en el apartado **Binary Tools**, puedes exportar y compilar tu videojuego. En **File Output** puedes establecer el nombre del archivo de salida y seleccionar entre los diferentes formatos disponibles para guardarlo.

En la parte inferior de la interfaz se muestra el espacio disponible para el programa y la cantidad de bytes que está utilizando actualmente. En ese mismo lugar también aparecen los mensajes de error cuando existe algún problema con el código, aunque estos no siempre indican claramente cuál es el problema.

> [!NOTE]
> Los juegos desarrollados para CHIP-8 utilizan el formato `.ch8`. Si utilizas un emulador fuera del entorno de Octo, tendrás que exportar tu videojuego en ese formato para poder ejecutarlo.

## Instrucciones básicas de octo

Ahora una explicación sobre las instrucciones básicas de octo, primero tenemos que saber que existen 16 registros `v0`, `v1`, `v2`, hasta `v9` y `va`, `vb`, hasta `vf`, este último es
recomendable no modificarlo ya que ciertas instrucciones pueden alterar el valor de `vf` ya que se usa para detector o indicador de que paso algo en una instrucción, es un registro especial.

El flujo principal del programa comienza en : main. A partir de esta etiqueta, el programa ejecuta las instrucciones que se encuentran debajo de ella. Desde aquí también se pueden llamar a las distintas funciones que componen el videojuego.

### Asignación de valores

La primera instrucción básica es la asignación, `:=` se usa para asignarle un valor a una variable, un ejemplo de la sintaxis puede ser `v0 := 5` aquí le asignamos al
registro `v0` el valor de 5 utilizando el operador `:=`, también es posible asignarle el valor de otro registro de esta manera `v0 := v1`, aquí el registro `v0` ahora
tendría el mismo valor que `v1`. Los registros tienen un límite de almacenamiento de 8 bits de 0 a 255.

### Suma

La segunda instrucción básica es la suma `+=` se usa para sumarle cierto valor a una registro, un ejemplo de la sintaxis es `v0 += 3` imaginemos que el registro `v0`
tenia el valor de 5 con `+=` hicimos que su valor aumentara 3 así que `v0` habría terminado con el valor de 8, también se puede sumar al primer registro el valor de un
segundo registro por ejemplo de esta forma `v0 += v1`, imaginemos que `v0` tenia el valor de 4 y `v1` tenia el valor de 2, entonces el resultado seria que `v0` tendría un valor de 6. 
Si se supera el límite de 255 en una suma el valor dará la "vuelta", por ejemplo si `v0` vale 255 y le sumas 1, `v0` ahora valdría 0.

### Resta

La tercera instrucción es la resta `-=` se usa para restarle el valor a un registro, por ejemplo `v0 -= 2` imaginemos que `v0` vale 5, entonces la instrucción `v0 -= 2` 
le habría restado 2, dejando su valor en 3. Aquí también el valor se da "vuelta" si se supera el limite inferior de 0, si a un registro le intentas restar un valor 
que lo dejaría por debajo de 0 dará la vuelta y subirá su valor a 255 y intentara restar desde ahí, por ejemplo si a 0 le restas 1 quedara en 255 y si le restas 2, terminara en 254.

### Condición

La cuarta instrucción básica es `if` la cual ejecuta la instrucción o la instrucciones siguientes a ella si es que se cumple la condición que tiene, primero tienes dos formas
la primera `if condición then instrucción` ejecutara la instruccion inmediata siguiente, en cambio su segunda forma `if condición begin instrucción1, instrucción2, ... end`
te permite ejecutar todas las instrucciónes siguientes hasta el `end`. Las condiciones utilizan los siguientes operadores: 

#### Operadores

- `==` comprueba si los dos valores son iguales.

- `!=` comprueba si los dos valores son diferentes.

- `>` compara si el primer valor es mayor que el segundo.

- `<` compara si el primer valor es menor que el segundo.

- `>=` comprueba si el primer valor es mayor que el segundo o si son iguales, si una de las dos cosas es verdad se ejecutara la siguiente instrucción o las siguientes instrucciones.

- `<=` comprueba si el primer valor es menor al segundo o son iguales, pasa lo mismo que con el anterior operador, en esta instrucción si una de las dos condiciones es verdad se ejecutara la siguiente instrucción o las siguientes instrucciones.

---

Ejemplos de sintaxis, la primera forma `if v0 > 5 then v0 := 0` si el valor de `v0` es mayor que 5 como 6 por ejemplo entonces ejecutara una sola instrucción la de después de `then` que es `v0 := 0`, así que el registro `v0` terminara con el valor de 0.

Ejemplo de la segunda forma:
```
if v0 == 10 begin
v1 := 5
v2 -= 2
v3 += 3
end
```
Esta instrucción si se cumple la condición de que `v0` vale 10 ejecutara las siguientes instrucciones hasta el end.

## Desarrollo de mi videojuego

Ahora comenzare a explicar como funciona el código de mi videojuego, explicare la ***[versión 1.1](https://github.com/theivza/Juego-Chip-8/blob/main/Codigo-Fuente/YusepeCombat_1.1v.8o)*** que esta en la carpeta de Codigo-Fuente.

### Creación de sprites

Primero explicare un poco sobre como se crean y dibujan los sprites en pantalla, para crear un prime primero se le tiene que asignar una etiqueta por ejemplo `: nombresprite`,
hay distintas formas de crear el sprite como en binario o hexadecimal, yo mostrare como se hace en binario. La forma que tiene es `0b00000000` cada 0 que cambiamos por 1
será un pixel iluminado, por ejemplo `: nombresprite 0b11100000 0b11100000 0b11100000` cuando lo dibujemos en pantalla tendra la forma de un cubo 3x3 pixeles. Tienes que saber que
los sprites tienen un ancho maximo de 8 pixeles y 15 de alto, también puedes colocar los `0b00000000` del sprite en filas hacia abajo y seguiran formando parte del sprite.

Ahora la instrucción `sprite vx vy NN`, es la que dibuja el sprite en pantalla, pero antes de eso tienes que hacer que apunte al sprite que quieras, por que imagina que tienes
muchos sprites, tienes que apuntar al que quieres dibujar y esto se hace con la instrucción `i := nombresprite`, con esa instrucción apuntas al sprite que quieres dibujar
ahora volviendo a la instrucción de `sprite` le siguen 2 registros, el primero indica el eje x, y el segundo el eje y, la esquina superior izquierda de la pantalla es (0, 0)
cuando aumentas el valor del eje y, el `sprite` bajara, recuerda que la pantalla es 64x32, pero las coordenadas son de 0 a 63 en el eje x y de 0 a 31 en el y. En la instrucción de `sprite`
después de las coordenadas debe ir la altura del sprite o la cantidad de filas, como prefieras verlo, básicamente si en la creación de tu sprite pusiste por ejemplo 8 filas de `0b00000000` entonces para que se
imprima completo debes poner 8 al final algo asi `sprite va vb 8` para que se imprima correctamente. Los sprites comienzan a imprimirse desde la esquina superior izquierda hacia la derecha y abajo.
Si un sprite supera las coordenadas limite, se dará la "vuelta" según la coordenada que haya superado y se dibujara la parte que se haya pasado del otro lado

Los sprites que uso en mi videojuego los hice todos en binario, pondré un ejemplo de uno de los sprites que uso:

```
: jugador
0b00111000
0b01010000
0b01111000
0b11111100
0b01111000
0b01001000
#Este es el sprite que uso para representar al jugador tiene 6 de altura así que para dibujarlo se utilizara las siguientes instrucciones

: main
#Aquí inicia el programa a ejecutar instrucciones
i := jugador
#Estamos apuntando al sprite que queremos dibujar
va := 10
#Le asignamos un valor a un registro para usarlo para las coordenadas donde dibujaremos el sprite
sprite va va 6
#Podemos usar el mismo registro para las coordenadas si queremos que sean las mismas en ambos ejes, y tenemos que poner 6, 1 por cada fila como son 6 entonces 6 al final
```
El resultado se debería ver así.

<img width="638" height="317" alt="Captura de pantalla 2026-11-17 011130" src="https://github.com/user-attachments/assets/ddfb3b29-67a6-4006-8a2c-c6ae2d1f8dec" />

### Funciones

Aquí explicare las distintas funciones que tiene mi videojuego, que hacen y como funcionan.

Antes de eso explicare como usar `:alias nombre vx`, la instrucción `:alias` se permite asignarle a un registro un nombre personalizado para llamarlo, puede ser complicado
acordarse que asignaste a cada registro específicamente así que pues usar el alias que le asignes en su lugar, hace que el código sea más fácil de leer. El alias debe establecerse arriba
de cualquier instrucción que lo vaya a usar para que funcione y se tome en cuenta.

Los alias que yo uso son los siguientes:

```
:alias vx va
:alias vy vb
:alias accion v4
:alias vidaJugador v5
:alias vidaEnemigo v6
:alias azar v7
:alias ataqueJugador v8
:alias ataqueEnemigo v9
:alias defensaJugador vc
:alias defensaEnemigo vd
:alias enemigosDerrotados ve
```

---

#### : pantalla

`: pantalla` Es la instrucción que uso para después dibujar distintas pantallas, la uso como base para las demas.
Su código es:

```
: pantalla
i := borde1
vx := 0
vy := 0
loop
sprite vx vy 2
vx += 8
while vx != 64
again

vy := 31
vx := 0
loop
sprite vx vy 1
vx += 8
while vx != 64
again

i := borde2
vx := 0
vy := 2
loop
sprite vx vy 1
vy += 1
while vy != 31
again

vx := 62
vy := 2
loop
sprite vx vy 1
vy += 1
while vy != 31
again

i := borde3
vx := 2
vy := 2
sprite vx vy 6

i := borde4
vx := 56
vy := 2
sprite vx vy 6
;
```

Las funciones se inician con una etiqueta por ejemplo `: nombrefuncion` y terminan o se cierran con `;`, cuando se quiere llamar se puede simplemente escribir el nombre de la función para llamarla.

Utilizo varios sprites que hice antes y los voy posicionando en los lugares justos para dibujar la pantalla como quiero, en una parte utilizo la instrucción `loop...again`
esta instrucción lo que hace es crear un bucle, ejecuta todas las instrucciones debajo de `loop` hasta `again` donde al llegar a este ultimo se empiezan a repetir las instrucciones
desde el inicio, pero existe `while condicion` que sirve para romper el `loop`, mientras la condición del `while` se siga cumpliendo el `loop` continuara, pero cuando
`while` no se cumple se salta la instrucción siguiente a el, que normalmente es un `again`, terminando así con el bucle y siguiendo el programa.

### : acciones

```
: acciones
v1 := 1
i := hex v1
vx := 2
vy := 27
sprite vx vy 5
v1 := 10
i := hex v1
vx += 8
sprite vx vy 5
i := T
vx += 6
sprite vx vy 5
i := K
vx += 6
sprite vx vy 5

v1 := 2
i := hex v1
vx := 38
sprite vx vy 5
vx += 8
v1 := 13
i := hex v1
sprite vx vy 5
vx += 6
v1 := 14
i := hex v1
sprite vx vy 5
vx += 6
v1 := 15
i := hex v1
sprite vx vy 5
;
```

La función `: acciones`, lo que hace es imprimir los distintos sprites que acompañan el videojuego visualmente, en algunas partes se usa la instrucción `i := hex v1`
el valor que se le asigna a la variable que se le sigue a `hex` es el valor que tomara, octo y CHIP-8 incluyen por defecto un `sprite` para los números del 0 al 9 y de la letra A a la F
los sprites tienen un tamaño de 5 de altura, entonces lo que hace `hex` es acceder a esos sprites y seleccionar uno de acuerdo al valor que se le fue asignado, el 0 será el `sprite` de si mismo pero
llegamos al 10 este seleccionara el `sprite` de la letra A, el 11 el de la B y asi hasta el 15 con la F. 

La función lo que hace es dibujar en la parte inferior lo siguiente:

<img width="633" height="70" alt="Captura de pantalla 2026-11-17 014031" src="https://github.com/user-attachments/assets/4bd20ac2-a357-4ad6-aafd-d51ee03f3430" />

Que sirve como referencia de los controles del videojuego.

### : Djugador

```
: Djugador
vx := 2
vy := 1
i := jugador
sprite vx vy 6
;
```

Una función simple que dibuja el `sprite` del jugador cerca de la esquina superior izquierda de la pantalla.

### : Denemigo

```
: Denemigo
if enemigosDerrotados == 1 then i := enemigo1
if enemigosDerrotados == 2 then i := enemigo2
if enemigosDerrotados == 3 then i := enemigo3
if enemigosDerrotados == 4 then i := enemigo4
if enemigosDerrotados == 5 then i := enemigo5
vx := 55
vy := 0
sprite vx vy 7
;
```

Esta función también dibuja un sprite pero el del enemigo y cerca de la izquierda arriba, tiene unas instrucciones `if` al inicio que sirven para seleccionar el `sprite`
que se imprimira, toma en cuenta la variable de enemigosDerrotados para saber cual debe imprimir en base a los enemigos que han sido derrotados por el jugador.

### : estadisticasEnemigo

```
: estadisticasEnemigo
if enemigosDerrotados == 2 begin
vidaEnemigo := 6
ataqueEnemigo := 3
defensaEnemigo := 2
end
if enemigosDerrotados == 3 begin
vidaEnemigo := 10
ataqueEnemigo := 2
defensaEnemigo := 2
end
if enemigosDerrotados == 4 begin
vidaEnemigo := 10
ataqueEnemigo := 4
defensaEnemigo := 1
end
if enemigosDerrotados == 5 begin
vidaEnemigo := 15
ataqueEnemigo := 5
defensaEnemigo := 2
end
;
```

Esta función lo que hace es que al ser llamada, es usar instrucciones `if` para en base al valor de la variable enemigosDerrotados, asignar ciertos parámetros a las 
estadísticas del enemigo, aqui se utiliza la instruccion `begin`, para poder ejecutar varias instrucciones con un solo `if`.

### : digits

```
: digits 0 0 0
```

Esta función más bien lo que hace es reservar 3 bytes de memoria, que se utilizaran en la siguiente función que explicare.

### : numeros

```
: numeros
i := digits
bcd v0

i := digits
load v2

if v1 > 0 begin
i := hex v1
sprite vx vy 5
end
vx += 6

i := hex v2
sprite vx vy 5
;
```

Esta función lo que hace es apuntar hacia el lugar de memoria de `: digits`, la instruccion `bcd`, divide el valor que recibe en centenas, descenas y unidades y las guarda en 3 bytes distintos cada uno, en la dirección de memoria
donde este apuntando  `i`, luego `load v2` lo que hace es cargarlos de `v0` hasta `v2`, el registro `v0` guardaría las centenas, `v1` descenas y `v2` unidades.

Luego la instrucción `if v1 > 0 begin` lo que hace es fijarse si hay decenas, para ver si las dibuja en pantalla, esta función utiliza el valor que se le asigno a `v0` antes de ser llamada.
Esta función es la principal que dibuja números en la pantalla.

### : Dcorazon

```
: Dcorazon
i := vida1
vx := 28
vy := 0
sprite vx vy 8
i := vida2
vx := 29
vy := 1
sprite vx vy 3
;
```

Esta función simplemente dibuja más o menos en el centro arriba de la pantalla un corazón para indicar que los números de arriba es la vida.

### : stop

```
: stop
	loop
		vf := delay
		if vf != 0 then
	again
;
```

Esta es una función que se utiliza para detener el programa y controlar la velocidad con la que avanza en ciertas partes, `delay` sirve como reloj va a 60Hz, disminuye 60 veces por segundo,
se puede usar asignándole un valor a un registro y luego `delay := vf`, dare un ejemplo de como funciona digamos que se le asigno 1, entonces pausa 1/60 de segundo, si queremos
medio segundo entonces le asignamos 30, que seria que pausa 30/60 de segundo, aunque en realidad lo que pasa es que se queda en un bucle donde va disminuyendo su valor hasta 0 y termina en bucle.

### : saltoJugador

```
: saltoJugador
i := jugador
sprite vx vy 6
vf := 20
delay := vf
stop
sprite vx vy 6
vy -= 1
sprite vx vy 6
vf := 20
delay := vf
stop
sprite vx vy 6
vy += 1
;
```

Existe la función `: saltoJugador` que utiliza la función anterior para controlar la velocidad, con la que se ejecutan sus instrucciones, y tambien esta función aprovecha
lo que mencione antes de que si un `sprite` es dibujado 2 veces en el mismo lugar se borra. Esta función hace una pequeña animación como de salto con el sprite del jugador.

### : temp

`: temp 0`

Esta función solo tiene eso, que cumple la misma función de digits reservar un byte que luego servirá para guardar un valor de forma segura, que será utilizado en otra función.

### : accionAtaque

```
: accionAtaque

vx := 12
vy := 14
v0 := ataqueJugador
numeros

vx := 2
vy := 13
i := espada
sprite vx vy 8

if azar == 0 begin
vf := 60
delay := vf
stop
vx := 54
i := escudo
sprite vx vy 8

vx := 41
vy := 14
v0 := defensaEnemigo
numeros

vf := 60
delay := vf
stop
vx := 12
vy := 14
v0 := ataqueJugador
numeros

v0 := ataqueJugador
vx := 12
v0 -= defensaEnemigo
if v0 > 127 then v0 := 0
i := temp
save v0
numeros

else
v0 := ataqueJugador
i := temp
save v0
end

vf := 60
delay := vf
stop
vx := 43
vy := 1
v0 := vidaEnemigo
numeros
i := temp
load v0
vidaEnemigo -= v0

if vidaEnemigo > 127 then vidaEnemigo := 0

vx := 43
v0 := vidaEnemigo
numeros
;
```

Esta función es una de las más importantes en el videojuego y una de las principales, es la que se llama cuando el jugador decide atacar. Dibuja varios sprites para acompañar
visualmente el juego, también usa varias veces la función de `stop`, y otros sprites, tiene una lógica simple y la mayoría es dibujo, pero en este fragmento:

````
if azar == 0 begin
.
.
.
v0 := ataqueJugador
vx := 12
v0 -= defensaEnemigo
if v0 > 127 then v0 := 0
i := temp
save v0
numeros

else
v0 := ataqueJugador
i := temp
save v0
end
.
.
.
````

Lo que hace es guardar el valor de `ataqueJugador` en la variable `v0`, luego le resta la `defensaEnemigo`, comprueba que no haya pasado que el valor haya dado "vuelta" y si ocurrio le signa 0 de valor
luego apunta a la etiqueta donde se reservo un byte y guarda con `save v0` el valor de `v0` en ese byte.

El if toma en cuenta la acción del enemigo, que esta en `azar` para saber si se defendio y seguir el procedimiento en ese caso, en cambio si no se defendio salta al else, donde se guarda en `save` el valor real sin restar `defensaEnemigo` de `ataqueJugador`.
Las instrucciones de después de ese fragmento recuperan el valor guardado en el byte de `temp` utilizando la instrucción `load v0` para luego restarlo a `vidaEnemigo`.
si es que el valor quedo debajo de 0 y dio la vuelta el `if vidaEnemigo > 127 then vidaEnemigo := 0` lo establece en 0.

### : accionEnemigo

```
: accionEnemigo

vx := 41
vy := 14
v0 := ataqueEnemigo
numeros

vx := 54
vy := 13
i := espada
sprite vx vy 8

if accion == 2 begin
vf := 60
delay := vf
stop
vx := 2
i := escudo
sprite vx vy 8

vx := 9
vy := 14
v0 := defensaJugador
numeros

vf := 60
delay := vf
stop
vx := 41
vy := 14
v0  := ataqueEnemigo
numeros

v0 := ataqueEnemigo
vx := 41
v0 -= defensaJugador
if v0 > 127 then v0 := 0
i := temp
save v0
numeros

else
v0 := ataqueEnemigo
i := temp
save v0
end

vf := 60
delay := vf
stop
vx := 11
vy := 1
v0 := vidaJugador
numeros
i := temp
load v0
vidaJugador -= v0

if vidaJugador > 127 then vidaJugador := 0

vx := 11
v0 :=  vidaJugador
numeros
;
```

Esta función sigue el mismo procedimiento que la función de ataque del jugador pero para el enemigo.

### : dobleDefensa

```
: dobleDefensa
vx := 2
vy := 13
i := escudo
sprite vx vy 8
vx := 54
sprite vx vy 8
;
```

Es una función dibuja 2 sprites de escudos a cada lado, solo es llamada en el caso donde tanto el enemigo como el jugador se defendieron y no atacaron.

### : refrescar

```
: refrescar
acciones
Djugador
Denemigo
vx := 11
vy := 1
v0 := vidaJugador
numeros
vx := 43
v0 := vidaEnemigo
numeros
Dcorazon
;
```

Esta función llama a otras funciones y coloca en su posición la vida del jugador y enemigo, esta función lo que hace es básicamente colocar la pantalla de juego y llamar a las funciones que imprimen los elementos que lleva.

### : victory

```
: victory
clear
pantalla
i := jugador
vx := 10
vy := 10
sprite vx vy 6
i := V
vx := 18
sprite vx vy 5
i := I
vx := 24
sprite vx vy 5
v1 := 12
i := hex v1
vx := 28
sprite vx vy 5
i := T
vx := 33
sprite vx vy 5
i := O
vx := 39
sprite vx vy 5
i := R
vx := 44
sprite vx vy 5
i := Y
vx := 49
sprite vx vy 5

v1 := 3
i := hex v1
vx := 15
vy := 23
sprite vx vy 5
i := R
vx := 23
sprite vx vy 5
v1 := 14
i := hex v1
vx := 28
sprite vx vy 5
i := T
vx := 33
sprite vx vy 5
i := R
vx := 39
sprite vx vy 5
i := Y
vx := 44
sprite vx vy 5

loop
v2 := key
if v2 == 3 then accion := 3
while accion != 3
again
jump main
;
```

Esta función llama a pantalla ya que la usa como base para los demas elementos visuales que dibuja y coloca, es la pantalla de victoria cuando el jugador gana, al final espera que se aplaste la tecla 3, para salir de esa pantalla, y llama a `main` se utiliza el `jump` para que la ejecución de todo el programa se vaya a `main` y no quiera regresar o tenga referencia de donde vino. Usa la instrucción `clear` al inicio para limpiar toda la pantalla lo que hace es apagar todos los pixeles encendidos de la pantalla.

### : defeat

```
: defeat
clear 
pantalla
i := jugador
vx := 10
vy := 10
sprite vx vy 6
v1 := 13
i := hex v1
vx := 18
sprite vx vy 5
v1 := 14
i := hex v1
vx := 23
sprite vx vy 5
v1 := 15
i := hex v1
vx := 28
sprite vx vy 5
v1 := 14
i := hex v1
vx := 33
sprite vx vy 5
v1 := 10
i := hex v1
vx := 38
sprite vx vy 5
i := T
vx := 43
sprite vx vy 5

v1 := 3
i := hex v1
vx := 15
vy := 23
sprite vx vy 5
i := R
vx := 23
sprite vx vy 5
v1 := 14
i := hex v1
vx := 28
sprite vx vy 5
i := T
vx := 33
sprite vx vy 5
i := R
vx := 39
sprite vx vy 5
i := Y
vx := 44
sprite vx vy 5

loop
v2 := key
if v2 == 3 then accion := 3
while accion != 3
again
jump main
;
```

Esta función dibuja la pantalla que sale cuando el jugador pierde, su funcionamiento es igual al de la `victory` solo que dibuja otros elementos visuales.

### : start

```
: start
pantalla
accion := 0
i := Y
vx := 6
vy := 8
sprite vx vy 5
i := U
vx := 13
sprite vx vy 5
v1 := 5
i := hex v1
vx := 19
sprite vx vy 5
v1 := 14
i := hex v1
vx := 25
sprite vx vy 5
i := P
vx := 31
sprite vx vy 5
i := hex v1
vx := 37
sprite vx vy 5

v1 := 12
i := hex v1
vx := 22
vy := 15
sprite vx vy 5
i := O
vx := 28
sprite vx vy 5
i := M
vx := 34
sprite vx vy 5
v1 := 11
i := hex v1
vx := 41
sprite vx vy 5
v1 := 10
i := hex v1
vx := 47
sprite vx vy 5
i := T
vx := 53
sprite vx vy 5

vx := 14
vy := 24
v1 := 3
i := hex v1
sprite vx vy 5
v1 := 5
i := hex v1
vx := 21
sprite vx vy 5
i := T
vx := 26
sprite vx vy 5
v1 := 10
i := hex v1
vx := 33
sprite vx vy 5
i := R
vx := 39
sprite vx vy 5
i := T
vx := 45
sprite vx vy 5

vx := 5
vy := 23

v2 := 3
loop
saltoJugador
if v2 key then accion := 3
while accion != 3
again
;
```

La función `start` es la primera pantalla que sale al iniciar el videojuego, dibuja los elementos visuales que llevara, pero instrucción final para esperar a que el jugador presione el 3 para avanzar es distinta
mientras que `v2 := key` detiene el juego hasta que se presione una tecla para seguir, `v2 key` espera a que la tecla con el valor que se le fue asignado al registro este presionada para 
digamos ponerse en true o que se cumpla la condición de que se presione la tecla con el valor del registro, y que ejecute en este caso la instrucción del `if`, mientras espera
a que la tecla este presionada el `loop` o bucle sigue funcionando por eso puede no detectar la tecla si no se mantiene precionada.

### : sigueinteEnemigo

```
: siguienteEnemigo
pantalla
vx := 5
vy := 10
i := jugador
sprite vx vy 6

v0 := 14
i := hex v0
vx := 14
sprite vx vy 5
i := N
vx := 20
sprite vx vy 5
v0 := 14
i := hex v0
vx := 27
sprite vx vy 5
i := M
vx := 33
sprite vx vy 5
i := I
vx := 40
sprite vx vy 5
i := G
vx := 45
sprite vx vy 5
i := O
vx := 51
sprite vx vy 5

v0 := enemigosDerrotados
v0 -= 1
vx := 15
vy := 20
numeros
v0 := enemigosDerrotados
vx := 32
numeros

i := flecha
vx := 29
sprite vx vy 5

vf := 60
delay := vf
stop
;
```

Es una función que dibuja la pantalla de transición entre enemigos y la mantiene un poco de tiempo antes de continuar.

## Main

Ahora si la parte principal que controla el orden de lo que sucede `:  main`.

```
: main
clear
start
accion := 0
vidaJugador := 10
vidaEnemigo := 8
ataqueJugador := 3
ataqueEnemigo := 2
defensaJugador := 2
defensaEnemigo := 1
enemigosDerrotados := 1

loop
accion := 0
clear
refrescar

loop
v2 := key
if v2 == 1 then accion := 1 
if v2 == 2 then accion := 2 
while accion == 0 
again
#accion
#1 ataque
#2 defensa
#azar
#0 defensa
#1 ataque

if accion == 1 begin
azar := random 1
accionAtaque
end

if vidaEnemigo != 0 begin 
if accion == 2 then azar := random 1
if azar == 1 then accionEnemigo
end

if vidaJugador == 0 begin
vf := 60
delay := vf
stop
defeat
end

if azar == 0 then if accion == 2 begin
dobleDefensa
end

if vidaEnemigo == 0 begin
vf := 60
delay := vf
stop
clear
enemigosDerrotados += 1
if enemigosDerrotados == 6 then victory
estadisticasEnemigo
siguienteEnemigo
vidaJugador += 4
end

vf := 120
delay := vf
stop
again
```

Primero al inicio se limpia la pantalla por que otras funciones traer a esta parte devuelta y antes de dibujar una nueva pantalla la limpio, se llama a la función `start` para imprimir esa pantalla
y luego se le asignan los valores a las distintas variables además de limpiarse para la siguiente partida.

Al principio hay un `loop` que actualiza la pantalla y asigna a `accion` el valor de 0, luego sigue otro `loop` que espera a recibir la entrada de una tecla, para saber si el jugador atacara o se defendera.

Luego si el jugador ataco, se genera la acción que hará el enemigo al azar, luego se llama a la funcion de accionAtaque.

Si la vida del enemigo no es 0, sigue el `if`, si el jugador de defendió se genera al azar la acciona del enemigo, si el valor de `azar` es 1 se tomara como que el enemigo quiere atacar y se llamara a la función `accionEnemigo`

Si la vida del jugador es 0 se llamara a la función `defeat`, significara que el jugador ha perdido la partida.

Si tanto el jugador como el enemigo de defendieron se llamara a la función dobleDefensa.

Si la vida del enemigo es 0, entonces signfica que el enemigo ha sido derrotado, entonces se le aumentara a `enemigosDerrotados` el valor de 1, si los enemigosDerrotados son iguales a 6, significara
que el jugador derroto a todos los enemigos y que gano, se llamara a la pantalla de `victory`, pero en el caso de que aun no gano, se llamaran a las funciones `estadisticasEnemigo`
para asignarle al enemigo sus nuevos parámetros, a la función `siguienteEnemigo` para hacer la transición al siguiente enemigo, y se le sumara al jugador 4 de vida por haber derrotado a un enemigo.

y se detendra un momento el juego llamando a `stop` antes de volver a iniciar el `loop`.


---

## Archivos del proyecto

Puedes encontrar el código fuente de las distintas versiones de mi videojuego en la siguiente carpeta:

[Ver código fuente](https://github.com/theivza/Juego-Chip-8/tree/main/Codigo-Fuente)

El videojuego fue desarrollado utilizando el entorno de programación [Octo](https://johnearnest.github.io/Octo/).

También puedes encontrar la versión final del videojuego, compilada en formato `.ch8`, aquí:

[Descargar videojuego](https://github.com/theivza/Juego-Chip-8/blob/main/ROM/YusepeCombat.ch8)

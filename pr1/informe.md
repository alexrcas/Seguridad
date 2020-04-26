# Práctica 1
Alexis Rodríguez Casañas

## 1. Generando hash
#### 1.1. Genera una contraseña usando la librería “Openssl”. Ahora, genera la misma contraseña utilizando un salt. ¿Qué diferencia hay?
![](https://i.ibb.co/LRkH87Y/1.png)

En el primer caso la contraseña varía cada vez que la generamos, mientras que si utilizamos un salt siempre se genera la misma.

## 2. Ataques de fuerza bruta
#### 2.1. Utilizando el script adjunto a esta práctica (hay que darle permisos de ejecución) para romper por ataque de fuerza bruta los siguientes hashes

![](https://i.ibb.co/LxbTJLX/image.png)

| Nº de caracteres | Salt | Hash | Contraseña | Tiempo |
|------------------|------|------|------------|--------|
|   2              |  iT    |  iTtle2zsSnkjY    |  ab          | 0.025s       |
|          2        |   Za   |   ZaTXT5zGz.IuM   |  vT          |   13.26s     |

#### 2.2. Modifica el script anterior para romper los siguientes hashes
[](https://i.ibb.co/ftYBKg8/image.png)

Para la cadena de **4 caracteres** el tiempo de espera era demasiado largo y el programa no arrojaba salida después de **1 hora**. Por ello, decidí hacer rápidamente una versión del script en Python, un lenguaje más optimizado para el cómputo, utilizando el módulo *Crypto*, el cual usa exactamente las mismas librerías que la herramienta *OpenSSL*. El script es casi idéntico al proporcionado en *bash*, cambiando prácticamente solo la sintaxis.
![](https://i.ibb.co/HVjqWZ1/image.png)

Como se observa en la siguiente tabla, el rendimiento con Python es muy superior.
![](https://i.ibb.co/7VLcrvj/image.png)

| Nº de caracteres | Salt | Hash | Contraseña | Tiempo |
|------------------|------|------|------------|--------|
|   3              |  cr    |  crbZpEDVRIy7Q    |   ull         |   14m 57s (Bash)     |
|          4        |   yp   |   yp7TQPXS8Ooho   |   ssi9         |    18s  (Python)  |

### Extra
Ya que he implementado el script en *Python*, me ha parecido una buena ocasión para poner a prueba lo aprendido en la asignatura de *Computación en la nube* y probar a utilizar técnicas de programación paralela. Para ello, he utilizado el estándar *MPI*, del cual *Python* posee un bind.

El trabajo de generar y probar claves se divide de forma proporcional entre los cuatro núcleos del procesador de mi máquina, quedando el script como se muestra en la siguiente imagen.
![imagen](https://i.ibb.co/sHXBQX4/mpi4code.png)

El tiempo de ejecución de esta versión se reduce notablemente (aproximadamente un 90%)
![imagen](https://i.ibb.co/Sr4gTjY/mpi4.png)

| Versión | Salt | Hash | Contraseña | Tiempo |
|------------------|------|------|------------|--------|
|   Estándar              |  yp    |  yp7TQPXS8Ooho    |   ssi9         |   18s     |
|          Paralela        |   yp   |   yp7TQPXS8Ooho   |   ssi9         |    2s    |


Por desgracia, no dispongo de acceso a máquinas con más núcleos para probar contraseñas con un mayor número de caracteres.

#### 2.3. Calcula todas las combinaciones posibles para encontrar contraseñas de 3, 4, 5, 6, 7 y 8 caracteres utilizando el alfabeto del script. Estima el tiempo medio para calcular estas combinaciones (aproximado)
El número posible de combinaciones de tamaño fijo viene dado por el número de estados que puede tomar nuestra variable elevado al número de variables que tenemos. Esto se ve muy claro en el sistema binario, donde para saber hasta qué número podemos contar con *N* bits simplemente hacemos $2^N$.
El alfabeto usado tiene 70 caracteres, es decir, cada variable pueden tomar 70 estados diferentes. Esto queda representado gráficamente de la siguiente manera.
Nótese que el crecimiento **no es lineal**, sino que el eje *Y* se ha dispuesto en **escala logarítmica** ya que de otra forma era imposible apreciar los primeros valores.
![](https://i.ibb.co/X2SHN4d/image.png)

El tiempo estimado varía enormemente según el hardware y la técnica utilizada. Existen en internet multitud de tablas precalculadas que muestran la evolución de este tiempo con respecto al número de caracteres. Una de ellas es la siguiente. 

|Nº de caracteres  | Combinaciones posibles            | Tiempo estimado |
|------------------|-----------------------------------|-----------------|
|   3              |   $70^3 = 343.000$                |     1s          |
|   4              |   $70^4 = 24.010.000$             |     1m 30s      |
|   5              |   $70^5 = 1.680.700.000$          |     2h 5m       |
|   6              |   $70^6 = 117.649.000.000$        |     8 días      |
|   7              |  $70^7 = 8.235.430.000.000$       |     2 años      |
|   8              |   $70^8 = 576.480.100.000.000$    |    200 años     |

Creo que más importante que la exactitud del tiempo, es observar el crecimiento exponencial que sufre cada vez que se agrega un solo caracter más.

#### 2.4. Modifica el script anterior para romper los siguientes hashes:
Script proporcionado modificado para leer de diccionario
![](https://i.ibb.co/wdhdZb8/image.png)

Obteniendo una contraseña mediante este método.

![](https://i.ibb.co/TtG6vTb/image.png)

Otro método: ataque de diccionario (*rockyou*) utilizando *John the Ripper*. Se aprecia que es infinitamente más rápido que el script modificado.

![](https://i.ibb.co/FxSdKDC/image.png)

 Utilizando el script modificado
|Salt     | Hash              | Contraseña | Tiempo |
|---------|-------------------|------------|--------|
|   LK    |  LK94jNJvCbURI    |   mckenzie |   15s  |
|    HA   |   HA3rjIgQVtuag   |   soccer10 | + 10m  |

Utilizando John the Ripper
|Salt     | Hash              | Contraseña | Tiempo |
|---------|-------------------|------------|--------|
|   LK    |  LK94jNJvCbURI    |   mckenzie |   0.3s  |
|    HA   |   HA3rjIgQVtuag   |   soccer10 |  0.3s  |


#### 2.5. Utiliza herramientas como John the Ripper para romper los siguientes hashes:
Obteniendo las contraseñas con *John the Ripper*
![](https://i.ibb.co/1Xtqq20/image.png)

|Salt     | Hash              | Contraseña | Tiempo |
|---------|-------------------|------------|--------|
|   wE    |  wEJjaGhgmQzbI    |   12345678 |   23s  |
|    uP   |   uPFsobeDFz6so   |   cryptull | 1m 41s  |

### 3. Shadow Password
#### 3.1. Analizar en detalle el formato de las entradas de los dos ficheros /etc/passwd y /etc/shadow
El fichero contiene blablabla...

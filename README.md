# Docker Compose
## Idiomas
### `LANG`, `LANGUAGE` y `LC_ALL`

Para configurar `LANG`, `LANGUAGE` y `LC_ALL` en `docker-compose.yml`, debes definirlas como variables de entorno dentro de la sección `environment` de tu servicio. Esto asegura que el idioma y la configuración regional se establezcan correctamente dentro del contenedor de la aplicación.

#### ¿Cómo hacerlo?
1. Abre tu archivo `docker-compose.yml`.
2. Busca la sección `services` y el servicio para el que quieres configurar el idioma.
3. Añade la sección `environment`, si aún no existe.
4. Define las variables de entorno dentro de la sección `environment` con los valores en español.

#### Ejemplo práctico
```YML
version: '3.9'

services:
  mi-aplicacion:
    image: mi-imagen-de-aplicacion # Advertencia: Este es un "nombre" (mi-imagen-de-aplicacion) de ejemplo, hay que cambiar este "nombre" a la de la imagen del proyecto.
    environment:
      # Idioma
      - LANG=es_ES.UTF-8
      - LANGUAGE=es_ES:es
      - LC_ALL=es_ES.UTF-8
```

#### Explicación de las variables
* **<ins>`LANG`</ins>:** Establece la configuración regional predeterminada para la mayoría de los programas.
* **<ins>`LANGUAGE`</ins>:** Se utiliza principalmente para la traducción de mensajes de interfaz de usuario y puede tener múltiples valores.
* **<ins>`LC_ALL`</ins>:** Anula todas las otras variables `LC_*` y `LANG`. Al establecerla, se aplica a todas las categorías de configuración regional.

#### ¿Por qué utilizarlo?
##### `DateTimeFormat.MonthNames` del lenguaje de programación `C#`
La matriz `DateTimeFormat.MonthNames` de `C#` considera el idioma y la configuración regional del sistema porque el objeto `DateTimeFormatInfo` que la contiene está diseñado para proporcionar información específica de la referencia cultural para el formato de fecha y hora. Esto permite que la misma aplicación muestre nombres de meses en español, inglés, o cualquier otro idioma soportado, basándose en la configuración del sistema donde se ejecuta el código, lo que garantiza que los nombres de los meses y el formato de fecha sean apropiados para el usuario final.
* **<ins>Especificidad cultural</ins>:** `DateTimeFormatInfo` es una clase que contiene propiedades para el formato de fecha y hora, como los nombres de los meses, días de la semana, y formatos de números específicos de cada cultura (por ejemplo, `es-CL` para español de Chile o `en-US` para inglés de Estados Unidos de América).
* **<ins>Adaptación automática</ins>:** Cuando se utiliza sin especificar una cultura, `C#` utiliza la cultura actual del sistema operativo como valor predeterminado. Esto significa que `DateTimeFormat.MonthNames` contendrá automáticamente los nombres de los meses correspondientes a esa cultura.
* **<ins>Uso explícito</ins>:** También es posible sobrescribir este comportamiento y especificar manualmente una cultura diferente al formatear una fecha, por ejemplo, utilizando `ToString("MMMM", new CultureInfo("es-CL"))` para forzar el nombre del mes en inglés.

## Zona horaria
Para configurar la zona horaria en `docker-compose.yml`, hay que definir la variable de entorno `TZ` dentro de la sección `environment` del servicio.

### ¿Cómo hacerlo?
1. Abre tu archivo `docker-compose.yml`.
2. Busca la sección `services` y el servicio para el que quieres configurar la zona horaria.
3. Añade la sección `environment`, si aún no existe.
4. Define la variable de entorno dentro de la sección `environment` con la zona horaria que necesites.

### Ejemplo práctico
```YML
version: '3.9'

services:
  mi-aplicacion:
    image: mi-imagen-de-aplicacion # Advertencia: Este es un "nombre" (mi-imagen-de-aplicacion) de ejemplo, hay que cambiar este "nombre" a la de la imagen del proyecto.
    environment:
      # Zona horaria
      - TZ=America/Santiago # Zona horaria de Santiago de Chile
```

### Explicación de las variables
* **<ins>`TZ`</ins>:** Establece la zona horaria del contenedor.

## Configuración del almacenamiento persistente
Para configurar el almacenamiento persistente en `docker-compose.yml`, hay que agregar `'/path/to/host/uptime/:/app/data'` dentro de la sección `volumes` del servicio.
* **<ins>`/path/to/host/uptime/`</ins>:** Es la ruta exacta en tu máquina real (el host) donde se guardarán los archivos.
* **`:`:** El separador que indica "conectar con".
* **<ins>`/app/data`</ins>:** Es la ruta interna dentro del contenedor de `Docker` donde la aplicación almacena su información.

### ¿Por qué se usa?
* **<ins>Persistencia de datos</ins>:** Si el contenedor se elimina o se actualiza, la información generada por la aplicación no se pierde porque está respaldada en tu disco duro.
* **<ins>Uso común</ins>:** Se utiliza frecuentemente para herramientas de monitoreo como `Uptime Kuma` u otros servicios, de modo que tu historial de registros y configuraciones persista.

## Ejemplo de archivo `docker-compose.yml` (lo más completo posible)
```YML
version: '3.9'

services:
  mi-aplicacion:
    image: mi-imagen-de-aplicacion # Advertencia: Este es un "nombre" (mi-imagen-de-aplicacion) de ejemplo, hay que cambiar este "nombre" a la de la imagen del proyecto.
    restart: always
    environment:
      # Zona horaria
      - TZ=America/Santiago # Zona horaria de Santiago de Chile
      # Idioma
      - LANG=es_ES.UTF-8
      - LANGUAGE=es_ES:es
      - LC_ALL=es_ES.UTF-8
    volumes:
      - '/path/to/host/uptime/:/app/data'
```

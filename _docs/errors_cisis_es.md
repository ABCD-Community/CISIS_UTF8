
        <a name="Mensajes_de_error_principal.2C_devuelto_por_wxis"></a>

## Principales mensajes de error devueltos por wxis

**códigos de retorno**

```
        0-exitoso
        1-Bloqueo de entrada de datos denegada
        Bloqueo de 2 registros denegados
        Actualización de 3 registros denegados
        4-Registro no bloqueado
        5-La identificación del bloque no coincide
        6-Actualización invertida denegada
        7-No se alcanzó el tiempo de caducidad del bloqueo
```


### WXIS|compiled error|undefined message|4/6|

Accediendo a la base de datos. El error `compiled error|undefined message|4/6` no da mala información porque ocurrió en un script compilado, lo que elimina los mensajes de error. Por lo tanto, puede haber varias causas, por ejemplo: 1 - No se encontró el script de llamada (causa más común) 2 - Si es posible, ejecute el script sin compilar para verificar su correcto funcionamiento. Intente colocar cgi-bin (o scripts, o cgi-local) después de IsisScript y así sucesivamente, o cambie la extensión del nombre del script de xic a xis en el cuadro de dirección del navegador (si el script está disponible y se llama script is "abrir" en la barra de navegación).

### WXIS|execution error|field

Buscando en la base de datos. Probablemente la base de datos contenga algo de "suciedad". Por ejemplo: campos sin contenido; personajes de control; espacios al final (o al principio) de los campos.



```
        1) Hacer una copia de seguridad de la base de datos;
        2) Ejecute la utilidad MXCP para crear una nueva base de datos sin errores.
        3) Reemplace la base vieja por la nueva.
        4) Generar el invertido para la nueva base;
```

### WXIS|execution error|file|tmpnam|

Buscando en la base de datos. Los derechos de acceso no permiten escribir en el directorio CGI. Normalmente, por motivos de seguridad, no se permite escribir en el directorio de ejecución de CGI. Luego use CI_TEMPDIR en el script cipar. Por ejemplo:

```
        CI_TEMPDIR=c:\windows\temp, o CI_TEMPDIR=/tmp
```

### WXIS|execution error|search|temporary file

Falta el derecho de acceso de escritura. Cambie los derechos para permitir escribir en la carpeta CGI, o agregue la declaración CI_TEMPDIR=path_pasta_temp al cip del script.


### WXIS|execution error|format syntax|@FMT.PFT|

No se puede ejecutar el archivo de formato FMT.PFT. Verifique la ruta y la existencia del archivo de formato, incluido el caso para los sistemas * nix



### WXIS|fatal error|unavoidable|flupdat/Typemfr

Accediendo a la base de datos. Existencia de caracteres no permitidos (por ejemplo, caracteres de control) en la base de datos. Ejecute MXCP con la cláusula CLEAN en la base de datos.



```
    1) hacer una copia de seguridad de la base de datos;
    2) ejecute el comando mxcp base_in create=base_out clean log=base;
    3) copiar base_out sobre base_in: copiar base_out.* base_in.*
```

### WXIS|fatal error|unavoidable |mstsetup allomaxv

Al invertir la base de datos. Problema en versiones anteriores a la versión WXIS 4.0 del 26 de septiembre de 2000. Descargue e instale la versión 4.0 anterior a la versión del 26 de septiembre.


### WXIS|fatal error|unavoidable|b5_run/offset|

Buscando en la base de datos. Problemas con el invertido, por ejemplo, cuando apunta a mfn que no existen en la base de datos. Regenerar o reemplazar la base de datos invertida. Ocurrencia común cuando el archivo maestro pierde el sincronismo con el archivo de base de datos invertido, por ejemplo, cuando se actualiza sin realizar una "actualización" del archivo invertido.



### WXIS|fatal error|unavoidable|b63_run/write/6

Buscando en la base de datos. La búsqueda no pudo escribir el archivo temporal, probablemente debido a la falta de derechos de acceso. Utilice CI_TEMPDIR en cipar. Por ejemplo: CI_TEMPDIR=c:\windows\temp. Eliminar archivos temporales. Verifique los derechos de acceso a la alfombra de archivos temporales para el usuario.



### WXIS|fatal error|unavoidable|cntread/read/cnt|

Buscando en la base de datos. Es probable que la base de datos esté en otro formato de plataforma. Por ejemplo, en un sistema Linux, intente leer la base del sistema Windows. Intercambiar los archivos de la base de datos y convertirlos al sistema operativo de la plataforma. Hay dos formas principales de llevar a cabo este intercambio:



```
        1-Exportar ISO-2709 desde la plataforma original, con importación y generación invertida en la plataforma final
        2-Uso de las aplicaciones CRUNCHMF (archivo maestro) y CRUNCHIF (archivo invertido) que generan los archivos finales de la plataforma en el original (cross-generation). Error similar a: <b>WXIS|error fatal|inevitable|relectura/verificación/base|</b>
```

### WXIS|fatal error|unavoidable|msopn/w| 

Intentando escribir en la base de datos. El sistema operativo bloqueó el archivo maestro (MST), o al menos no lo dejó disponible para escritura. Copie los archivos MST y XRF (copia de trabajo) y ejecute MKXRF en la copia. Si todo está bien, copie sobre el original. El último término del mensaje significa MaSter OPeN para Write.

### WXIS|fatal error|unavoidable|recisis0/xrf

Intentando escribir en la base de datos. No puede guardar ni crear un archivo. Los derechos de acceso al directorio no permiten escritura o el archivo no existe (en caso de grabación). Cambie los derechos de acceso para permitir la escritura. Compruebe si el archivo a guardar existe.



### WXIS|fatal error|unavoidable|recread/check/base|

Acceso a la base de datos para lectura. Lee bases y convierte desde diferentes plataformas. Exportar datos a través de ISO-2709 y regenerar invertidos, o realizar CRUNCH?F para la base de datos Error similar a: `### WXIS|fatal|error|unavoidable|cntread/read/cnt|`



### WXIS|fatal error|unavoidable|recread/xropn/w|

Falta de derechos de acceso a la escritura. Compruebe y ajuste los derechos de acceso de descripción para activar de descripción en el directorio y los archivos de la base de datos. El último término del mensaje significa XRf OPeN para escritura. otras posibilidades: ajustar permisos para R&W para directorios "bases" y "htdocs" y R&W&X para "cgi-bin".



### WXIS|fatal error|unavoidable|cifm1/select/endselect|

Al mostrar un registro. Formato con errores de sintaxis en SELECT... CASE... ELSECASE... ENDSEL. Revisa y corrige el formato.



### WXIS|fatal error|unavoidable|CIIFL/invalid link record/key|

Generando el archivo invertido con wxis1660. No complete la generación invertida y bloquee la base de datos. Campo de datos de secuencia continua de más de 60 caracteres. Edite el registro y corrija el elemento de datos en el campo.



### WXIS|fatal error|unavoidable|recupdat/crec/nxtmfp/MSBSIZ|

La columna NXTMFP del registro de control del archivo maestro está indicando un número mayor a 500. En el registro de control, NXTMFP indica la primera posición libre dentro del último bloque asignado en el archivo maestro. Los bloques tienen una longitud de 512 bytes, con un registro que comienza en cualquier posición libre entre 0 y 498 e incluye uno o más bloques. Ningún registro puede comenzar entre el byte 500 y el byte 510.



Hay dos formas de resolver este problema:

- Cree un archivo ISO desde la base que tenga este error. Después de eso, crea e invierte la base nuevamente.
- Use la aplicación MXCP aplicando la propiedad limpia **para** corregir la ubicación del bloque. Reinvertir la base después de este procedimiento

### WXIS|fatal error|unavoidable|recwrite/check/even|

Este error ocurre al usar MKXRF despuéde CTLMFN cuando la columna NXTMFP del registro de control base apunta a un número mayor a 500. El procedimiento utilizado para solucionar este problema es el mismo que se describe en el error `WXIS|fatal error|inevitable|recupdat/crec /nxtmfp/MSBSIZ|`

### cntwrit/cnopn/w

Intente escribir en el archivo invertido, pero no tenga los derechos de acceso necesarios. Verifique y cambie los derechos de acceso de la escritura en el directorio y los archivos invertidos. Por ejemplo, en DOS use el comentario attrib: attrib -r *.* El final del último mensaje le dice a CNt OPeN que Write On Unix/Linux use el comando chmod según sea necesario.



### fatal: iso_loadbuff/kj

Error normalmente relacionado con la falta de delimitador de línea (en ISO generalmente en el último registro). Si usando hexdump (hexdump -C <iname>) se confirma la falta de un delimitador de línea (en DOS CR+LF, en Linux LF y en MacOS CR) al final del archivo, simplemente incluya el delimitador al final del file , de la siguiente manera, por ejemplo:



```
        echo > dummylf # Crea un archivo con solo el delimitador de línea
        cat <iname> dummylf > result.iso # Incrustar el delimitador en un nuevo archivo con el contenido original
```

### fatal: msrt/loadxrf/overflow

La base está mal construida, al menos en el sentido estricto de CISIS, en particular para el programa MSRT. Más específicamente, el archivo XRF tiene más bloques de los que debería. Reconstruya la base de datos con el comando:

```
     <i>mx base_in create=base_out -all now</i>
```

Por ejemplo, BASE_IN con 846 registros y 17 408 bytes en XRF durante la ejecución de MSRT provoca un desbordamiento en el área de memoria xrf dentro del programa, porque los 846 registros necesitan 7 bloques xrf (3584 bytes), que son asignados por el programa. Pero cargar los 17.408 bytes de BASE_IN (que serían 34 bloques) da como resultado un desbordamiento. En el archivo XRF, cada bloque (512 bytes) apunta a 127 registros.

### fatal: mxtb/maxhash/overflow

MXTB no tiene suficiente memoria para ejecutarse. Utilice el parámetro "class=n" para asignar más memoria. Si resulta en un error de asignación (/alloc) disminuya el valor n



### fatal: posting/ifpnxtby

Archivo invertido con problema. Genere el archivo invertido nuevamente.



### fatal: dbxopen:dbn(1)

El sistema operativo (MS-DOS) devolvió el código de error 1: número de función no válido. Es probable que no haya suficiente búfer o archivos para abrir la base de datos



### fatal: dbxopen:dbn(2)

El sistema operativo (MS-DOS) devuelve el código de error 2: Archivo no encontrado. Verifique la ubicación de la base de datos o regenere esa base de datos



### fatal: dbxopen:dbn(4)

El sistema operativo (MS-DOS) devolvió el código de error 4: Demasiados archivos abiertos En CONFIG.SYS Aumente la cantidad de archivos y búferes a 90 o más (requiere reiniciar el sistema). Una base de datos que se lee representa 2 archivos abiertos. Cada índice al que se accede implica hasta 6 archivos abiertos, por lo que puede superar fácilmente el número máximo de archivos abiertos por el sistema operativo.



### fatal: dbxopen:dbn(65)

El sistema operativo (MS-DOS) devolvió el código de error 65: Acceso denegado. Verifique (y corrija) los permisos de archivo. Attrib para DOS, o en entorno de red comprobar los derechos de acceso



### fatal: dbxopen:dbn(68)

En el símbolo del sistema de MS-DOS o Windows, abra Base de datos. El sistema operativo (MS-DOS) devolvió el código de error 68: el nombre de la red superó el límite. Se debe aumentar el tamaño del almacén de nombres de la red. Esto se hace aumentando el valor de ARCHIVOS en el archivo CONFIG.SYS (requiere reiniciar el sistema)



### fatal: dirread/check/mfrl

Se leyó un directorio, se verificó el registro y hubo un problema debido a la falta de consistencia entre MST y XRF. Determinar el registro del problema y eliminarlo



### fatal: ifload/CIIFL/keys not sorted

Invirtiendo la base de datos. A partir de Linux 6.2 es necesario ajustar la variable de sistema LC_ALL, como se muestra: export LC_ALL=POSIX, ya que esta versión de Linux ha cambiado el valor por defecto. Configure la variable del sistema: LC_ALL=POSIX. Linux Red Hat 6.2 o superior (compruebe la versión del kernel).



### fatal: mstsetup/allomxv/.xrf

Base de datos mayor que el límite de capacidad del programa MSRT. Utilice una versión actualizada de la aplicación. Usando MSRT hay un tamaño máximo de clasificación



### fatal: recread/check/nvf

Número de entradas del directorio (campos variables). Cree una nueva base sin los registros dañados, o intente usar la utilidad MKXRF (es posible perder los registros con un problema). El XRF indica que el registro se encuentra en una dirección MST, sin embargo, esta dirección es inconsistente.



### fatal: recwrite/deleted

Está grabando registro borrado y provoca una inconsistencia de lo invertido con el maestro. Genere el archivo invertido nuevamente.



### fatal: iso_getval

Problema común al transferir de UNIX a DOS o viceversa desde un archivo ISO en modo binario. Rehacer la transferencia FTP en modo ASCII Como opción, use: mx seq=dbn.iso lw=99999 pft=v1/ now >dbn1.iso. En los sistemas *nix, utilice el comando dos2unix o tr -d "\015".



### fatal: recread/check/base

Registro dañado: compensación del segmento de datos. Cree una nueva base sin los registros dañados, o intente usar la utilidad MKXRF (es posible que pierda los registros con un problema). El XRF indica que el registro se encuentra en una dirección MST, sin embargo, esta dirección es inconsistente.



### fatal: recread/check/mfrl

Problema de tamaño del registro. Cree una nueva base sin los registros dañados, o intente usar la utilidad MKXRF (es posible que pierda los registros con un problema). El XRF indica que el registro se encuentra en una dirección MST, sin embargo, esta dirección es inconsistente.



### fatal: recread/read

No se puede leer el registro de la base de datos. Crea una nueva base sin registros dañados.



### gizmread/GIZMTAG1/nocc

Base de datos de gizmo sin el campo 1. Inserte el campo 1 en la base de datos de gizmo.


### loadfile/overflow

Error al ejecutar un archivo de formato (PFT), probablemente porque es demasiado grande. Aumenta el valor de FMTL. Por ejemplo: mx base_in join=base_j,260=v480 fmtl=20000 lw=9000 pft=@format.pft create=base_out -all now tell=2000.



### rec write/xropn/w

No es posible registrar información en la base. Comprobar y ajustar los derechos de acceso.



### recalloc/allomaxv/rechsize+nbyte

En mx puede deberse a escribir con caracteres en MAYÚSCULAS. En la versión 3 de WWWIsis se debe verificar la restricción en cuanto al tamaño de la base, verifique la versión de WWWIsis.



### recwmast/write/rec

Puede ser que el archivo maestro supere el límite de tamaño de 512 Mbytes. Genere una base de datos con el parámetro mstxl igual a 4, luego agregue a la base. Para generar la base como se indica, haga:



```
    1) echo mstxl=4 > xpar.par;
    2) mx cipar=xpar.par create=recuento base=0
    3) agregar registros a la base
```

Para crear agregando registros directamente, use cipar en la operación.

### recwrite/mfblink

El sistema no puede escribir en el archivo maestro debido a la falta de integridad entre MST y XRF. La solución es crear otro maestro usando MX.



### refcgets/buffup

Archivo secuencial con línea demasiado larga. Utilice una versión más reciente de la aplicación MX.



### rejected link 66/440/1/1/

Generación o actualización de archivos invertidos. Al generar y actualizar el archivo invertido se detecta un término inválido que no se puede registrar en el archivo invertido (terminal en blanco, debajo del ascii 33, etc). Verifique el registro de caracteres no válidos y elimínelos o modifíquelos. Por ejemplo, MXCP se puede usar con la cláusula CLEAN para eliminar caracteres no válidos. Clave de descifrado: mfn/field/occurrence/cnt - 66/440/11/1.


### writbsiz/write

Falta de espacio en disco o, según la lista, algunos archivos han excedido el límite de 2G bytes. Libere espacio en disco si es necesario, o "divida" la base de datos en dos o más bases de datos. El uso de proc R es una posibilidad para una situación de base de datos muy grande

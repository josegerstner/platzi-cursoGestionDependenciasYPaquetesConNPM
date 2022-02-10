# Platzi - Curso de gestión de dependencias y paquetes con NPM  
  
## Inicializar un proyecto con NPM  
  
Para inicializar un proyecto debemos utilizar el comando:   
```
npm init  
```  
Luego de seguir los pasos se crea el **package.json**.  
  
También podemos inicializar el proyecto con el comando:  
```
npm init -y
```
(_-y o -yes_): esto sirve para generar el documento automáticamente sin que te pida todos los datos.  
  
Otra manera de hacerlo es preconfigurando:  
```  
npm set init.author.email "alguien@email.com"
npm set init.author.name "Nombre Apellido"
npm set init.license "MIT"
npm init -y
```
<br />  
  
## Instalación de dependencias  
  
Podemos instalar dependencias mediante el comando:  
```
npm install dependencia
```
En lugar de la palabra **install** también podemos usar la letra **i**.  
Podemos indicar dónde usaremos esa dependencia con **--save**.  
```
npm install dependencia1 --save
npm install dependencia2 --save-dev
```
> Como default npm toma los paquetes instalados como _--save_, por lo que no es necesario ponerlo.  
  
Si ponemos el **--save** (o no) significa que instalaremos una depencia necesaria para producción. Se agrega al package.json en la parte de "dependencies".  
  
Si ponemos **--save-dev** significa que instalaremos el paquete sólo de forma local en el ambiente de desarrollo. Se agrega al package.json en la parte de "devDependencies".  
  
Otra manera de agregar una dependencia a desarrollo es usando **-D** en lugar de **--save-dev**. También en lugar de **--save**, podemos poner **-S** para agregar la dependencia a "dependencies" en el package.json.  
  
Podemos instalar paquetes **opcionales** usando **-O**. Esto se agregará al package.json en la parte de "optionalDependencies".  
  
Para emular la instalación de un paquete, podemos usar:  
```
npm i paquete --dry-run
```
Esto mostrará todo el proceso como si se hubiera instalado, pero no lo instalará.  
  

## NODEMON  
> Nodemon es un _demonio_ que se encarga de escuchar los cambios del proyecto mientras se sigue ejecutando el servidor de node. En otras palabras, permite que no tengamos que reiniciar el server cada vez que hacemos cambios en el proyecto.  

Para instalarlo de manera global, debemos instalarlo con npm i -g:
```
npm install -g nodemon
```
**NOTA: Debemos tener permisos de administrador para poder instalar dependencias de manera global. En Windows hay que ejecutarlo con permisos de administrador, en Linux/Mac anteponiendo la palabra sudo en la terminal antes del comando.**
  <br />  
> **Forzar la instalación de dependencias**  
> Para forzar la instalación de dependencias podemos usar **-f** o **--force**.  
  
Se puede forzar la instalación de una versión específica de un paquete:  
```
npm install paquete@version
```
  
- **npm list**: lista los paquetes que tenemos instalados en el proyecto.
- **npm outdate**: muestra si hay alguna actualización para los paquetes de mi proyecto.
- **npm outdate --dd**: lo mismo que el comando anterior, pero muestra un output.
- **npm update**: actualiza mis paquetes.
- **npm install paquete@latest**: instala la última versión de un paquete. Si el paquete ya lo tengo instalado, actualiza a la última versión.
- **npm uninstall paquete**: elimina un paquete del proyecto.
- **npm uninstall paquete --no-save**: elmina un paquete del proyecto, pero sin sacarlo del _package.json_.
  
## CARET  
Luego de instalar dependencias, en el package.json se suelen mostrar las versiones de "dependencies" con el signo **^** (caret), en el formato _^versionmayor.versionmenor.arreglosbugs_. En este formato se puede decir que las versiones mayores no necesariamente tienen que ser compatibles entre sí. Dentro de una versión mayor, tenemos versiones menores que son compatibles con la versión mayor, por ejemplo nuevas funcionalidades, funciones deprecadas, etc. Luego los arreglos de bugs, que no afectarían demasiado, pero que son necesarias para corregir errores. Esto significa que cuando llevemos el proyecto a otra máquina y hagamos _npm install_ se instalarán las últimas versiones mayores de los paquetes que tengamos con caret. A veces hay conflictos entre versiones mayores, por lo que se puede recomendar trabajar con una versión específica de algunos paquetes. Para estos casos, es necesario quitar el caret de esos paquetes para no encontrarnos con problemas de incompatibilidad.  
  
## SCRIPTS
Se pueden configurar scripts en el package.json, en la parte que dice "scripts".  
___
## Comandos para solución de errores
- **npm comando --dd**: se ejecuta el comando de una manera _verbosa_. Esto significa que nos mostrará el output de todo lo que pasa atrás de su ejecución.
- **npm cache clear --force**: elimina el caché del proyecto.
- **npm cache verify**: para verificar que verdaderamente se borró el caché.
  
> A veces para solucionar errores, podemos simplemente borrar la carpeta **node_modules** con el comando
>```
>rm -rf node_modules
>```
> y reinstalar las dependencias con _npm install_.  
  
## Seguridad  
  
- **npm audit**: audita el proyecto indicando si hay vulnerabilidades.  
- **npm audit --json**: audita el proyecto indicando si hay vulnerabilidades pero en formato JSON.
- **npm update paquete --depth numero**: actualiza un paquete con una profundidad de numero.
  
___  
## Crear paquete
Inicializamos nuestro proyecto con **npm init** y confuguramos todo como queremos.  
En nuestro ejemplo (random-messages), tenemos una carpeta src con un index.js y otra carpeta bin con un global.js. En el index.js tenemos que tener nuestra funcionalidad exportada (randomMsg):
```
module.exports = { randomMsg }
```
En el global.js indicaremos que usaremos node. Además iremos a buscar el archivo index.js a la carpeta src y por último ejecutaremos nuestra función:
```
#!/usr/bin/env node
let random = require('../src/index.js')

random.randomMsg()
```
  
En el package.json debemos agregar:
```
"bin": {
    "random-msg": "./bin/global.js"
  },
  "preferGlobal": true
```
- "bin": indicamos que es el binario del paquete, al instalarlo vendrá a buscar la configuración aquí.  
- "random-msg": es el comando que utilizaremos en este caso, pero podríamos poner cualquier cosa.
- "preferGlobal": para indicar que queremos instalar el paquete de manera global en el sistema.
  
## Subir paquete a NPM
> **NOTA: Antes que nada tenemos que tener cuenta en la página de NPM**.  
  
- **npm adduser**: loguea un usuario npm en el equipo.
- **npm publish**: publica el paquete con los datos del npm user (corroborar que no haya otro paquete con el mismo name).
  
- **npm version _actualizacion_**: este comando actualiza la versión de nuestro paquete, que está en formato **major**.**minor**.**patch**.
```node
// version inicial 1.0.0
// patch actualiza el último número (1.0.1)
npm version patch
// minor actualiza el número del medio (1.1.1)
npm version minor
# major actualiza el primer número (2.1.1)
npm version major
```

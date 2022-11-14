# curso-de-vitejs
Vite es un servidor de archivos estatico

Alternativas para webpack pueden ser: 
 - esbuild: extremely fast javascript bundler
 - parcel
 - rollup
 - VITE


 etapas para ejecutar vite: 
 - pre-bundling: agrega compatibilidad
 - dependency resolving
 - hot module replacement
 - integracion con typescript
 - soporte a webworkers y web assembly


 como se configura: 

    - sirve para vanilla, vue, react, preact, lit, svelte ( y sus correspondientes en ts)
    - considerar usar la node version 16.13.2 y npm 8.1.2 para esta carpeta
    - npm create vite@latest || yarn create vite || pnpm create vite
    - 

Para importar css en vite se hace muy simple y ya se hace con todos los archivos
 - Se  usa @import


pre-procesadores css en vite
- vite lo reconoce por defecto
- incluso, se puede importar un archivo de sass en css .-.
- imcluso un archivo less


css modules en vite
- usado en el ejemplo: button.module.css
- import buttonStyles from './button.module.css'

importar imagenes:
- se cargan como modulos, todo en vite son modulos de ecmascript

importar objestos JSON
- si se puede importar por partes en lugar de traerlo todo para cada vez que se importe, ejemplo: import { user } from './data.json'

hay una opcion para importar todos los archivos de una sola carpeta con los global imports
- import.meta.glob(regex) para importar varios modulos al mismo tiempo

Ejemplo: 
const modules = import.meta.glob('./modules/*.js');

for(const path in modules){
  async function fetchModule(){
    const module = await modules[path]();
    module.load();
  }
  fetchModule();
  // modules[path]().then((module)=>{
  //   module.load();
  // })
}

console.info(modules);
- read more at: https://vitejs.dev/guide/features.html#glob-import

vite typescript: 
- no hay que hacer nada para trabajar con typescript, solo se debe crear el archivo con la extension de ts
- se debe crear un archivo de configuracion de typescript
- para tener en cuenta, se debe de incuir la extension del archivo en el import

Cada que se haga un cambio en tsconfig es importante limpiar la cache del navegador

Vite config: 
- existe un archivo vite.config.js para tomar el control manual de vite.

vite.config.js
import { defineConfig } from 'vite' // existen otras formas de sobreescribir las configuraciones de vite

export default defineConfig({
    server: {
        port: <new port>
    }
})

 Environment variables en vit incluyendo modos: 
 - tenemos un modo de desarollo y un modo de produccion
 - para acceder a variables de entorno tienen que contener el prefijo VITE_, seguido podemos llamar la funcion loadEnv desde el paquete principal de vite: 

 vite.config: const env = loadEnv(mode, process.cwd())

 Multipage sites: 
 - Normalment en un proyecto de vite vamos a tener un punto de entrada principal, pero podemos tener una pagina completamente independiente en otra ubicacion con sus propias dependencia. usaremos rollup para definir el nuevo punto de entrada de la aplicacion en el vite.config.js

```js
//vite.config.js
import { defineConfig } from 'vite' // existen otras formas de sobreescribir las configuraciones de vite
import { resolve } from 'path' // we can use nodejs funcions due we are in a nodejs context

export default defineConfig(({ command, mode }) => {
  const port = 3000

  const env = loadEnv(mode, process.cwd())
  console.log(env.VITE_NAME) // always starts with VITE_

  // mode can be development or production

  return {
    server: {
      port // will apply new port defined before
    },
    // we can access rollup configuration using build.rollupOptions
    build: {
      rollupOptions: {
        input: {
          main: resolve(__dirname, 'index.html'),
          help: resolve(__dirname, 'help', 'help.html')
        }
      }
    }
  }
})


```
 
Con microfrontends podemos tener pequenas aplicaciones que se construyen en diferentes frameworks o librerias que se les permite vivir en un contexto relativo

construir librerias con vite:

```js
//vite.config.js
import { defineConfig } from 'vite' // existen otras formas de sobreescribir las configuraciones de vite
import { resolve } from 'path' // we can use nodejs funcions due we are in a nodejs context

export default defineConfig(({ command, mode }) => {
  const port = 3000

  const env = loadEnv(mode, process.cwd())
  console.log(env.VITE_NAME) // always starts with VITE_

  // mode can be development or production

  return {
    server: {
      port // will apply new port defined before
    },
    // we can access rollup configuration using build.rollupOptions
    build: {
      lib: {
        entry: resolve(__dirname, 'lib', 'main.js'),
        name: 'demo',
        filename: (format ) => `demo.${format}.js`
      }
    }
  }
})


```

Las librerias se pueden distribuir de formas diferentes pero una de ellas es NPM

Soporte para react: 
- la configuracion para react se hace desde el momento en que se crea el proyecto con vite

Vite esta desarrollado por el mismo equipo de vuejs, para crear la configuracion inicial de vuejs se hace desde el comando de configuracion inicial


Vite es una herramienta de tercera generacion

¿Qué tecnología usa Vite para el procesamiento de CSS?
built-in

¿Qué son los CSS Modules?
estandar web para cargar modulos css en un performance similar al de javascript


9
Una manera de usarlo en React es:

CSS o SCSS Modules
El objetivo de esto es conseguir que el CSS que declaremos sea relativo y único hacia el componente que lo estamos importando.

¿Cómo hacer que una clase de CSS sea única, dándole un nombre único y diferente? se consigue con modules CSS que apoyado en un bundle permite generar un identificador único para cada clase del CSS

A nuestros archivos de CSS debemos agregarle un .module.css
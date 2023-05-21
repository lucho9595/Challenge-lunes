# movie-app - Challenge

## Instalacion

In the project directory, you can run:

## `git clone https://github.com/lucho9595/Challenge-lunes`

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, check out the [documentation](https://nuxtjs.org).

## Funcionalidades

Tiene un Landing page donde apretando el boton podes iniciar seccion, el cual te lleva a la ruta de Login y el unico acceso a ese login es con el siguiente usuario:

email: admin@admin.com
password: admin123

tiene una autentificacion dummy, el cual al no ingresar estos datos te sale un alert el cual te informa que ingrese las credenciales correctas para ir a la ruta de movie.

En movie la van a aparecer todas las peliculas con su nombre, establecidas en 10 por pagina con un paginado que se visualiza a lo ultimo.

Al hacer click en cada imagen accedes al detalle de cada pelicula y un boton para volver hacia la ruta de movie.

## Tecnologías usadas en el proyecto:

- Nuxt
- Vue,JS
- Bootstrap


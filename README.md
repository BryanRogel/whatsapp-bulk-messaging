# whatsapp-bulk-message

## Project setup

Crear un archivo .env con las siguientes variables

```
VUE_APP_ACCESS_TOKEN = 'token'

VUE_APP_API_URL = 'https://graph.facebook.com/version/id/messages'
```

Y el json de ejemplo:

{ "numeros_telefono": [ 12345678, 12345678 ] }

```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

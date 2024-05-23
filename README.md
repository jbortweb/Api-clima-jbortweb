# Buscador de Clima

Esta aplicación permite a los usuarios consultar el clima actual en diversas ciudades y países de Europa, utilizando datos en tiempo real de la API de OpenWeatherMap.

## Demo

Puedes ver la aplicación en acción [aquí](https://climajbortweb.netlify.app/).

## Tecnologías Utilizadas

- **Vue 3**: Framework progresivo de JavaScript para construir interfaces de usuario.
- **Axios**: Cliente HTTP basado en promesas para realizar las solicitudes a la API.
- **OpenWeatherMap API**: Fuente de datos para obtener el clima actual.

## Características

- **Selección de Ciudad y País**: Los usuarios pueden seleccionar una ciudad y un país para consultar el clima.
- **Visualización del Clima**: Muestra la temperatura actual, mínima y máxima.
- **Reactividad**: Uso de la reactividad de Vue para actualizar la interfaz de usuario en tiempo real.
- **Manejo de Errores**: Muestra alertas en caso de errores o datos faltantes.

## Instalación

Para ejecutar este proyecto localmente, sigue estos pasos:

1. Clona el repositorio:
    ```sh
    git clone https://github.com/tuusuario/buscador-clima.git
    ```

2. Navega al directorio del proyecto:
    ```sh
    cd buscador-clima
    ```

3. Instala las dependencias:
    ```sh
    npm install
    ```

4. Ejecuta la aplicación en modo desarrollo:
    ```sh
    npm run dev
    ```

5. Abre [http://localhost:5173](http://localhost:5173) en tu navegador para ver la aplicación.

## Uso

### Consultar el Clima

1. Completa los campos de ciudad y país.
2. Haz clic en "Consultar Clima".
3. Visualiza los resultados, incluyendo la temperatura actual, mínima y máxima.

## Código de Ejemplo

Aquí tienes un fragmento del código utilizado en el proyecto:

```javascript
import { ref, computed } from 'vue'
import axios from 'axios'

export default function useClima() {

    const clima = ref({})
    const cargando = ref(false)
    const error = ref('')

    const obtenerClima = async ({ciudad, pais}) => {
        const key = import.meta.env.VITE_API_KEY
        cargando.value = true
        clima.value = {}
        error.value = ''
        try {
            const url = `https://api.openweathermap.org/geo/1.0/direct?q=${ciudad},${pais}&limit=1&appid=${key}`
            const {data} = await axios(url)
            const { lat, lon } = data[0]
            const urlClima = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${key}`
            const {data: resultado} = await axios(urlClima)
            clima.value = resultado
        } catch {
            error.value = 'Ciudad No Encontrada'
        } finally {
            cargando.value = false
        }
    }

    const mostrarClima = computed(() => {
        return Object.values(clima.value).length > 0
    })

    const formatearTemperatura = temperatura => parseInt(temperatura - 273.15)

    return {
        obtenerClima,
        clima,
        mostrarClima,
        formatearTemperatura,
        cargando,
        error
    }
}
 ```
### Contribuciones
¡Las contribuciones son bienvenidas! Si tienes alguna sugerencia o mejora, no dudes en hacer un fork del repositorio y enviar un pull request. También puedes abrir un issue para reportar errores o hacer preguntas.

### Licencia
Este proyecto está bajo la licencia MIT.
¡Gracias por visitar y utilizar el Buscador de Clima! 🚀

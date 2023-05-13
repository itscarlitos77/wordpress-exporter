# INSTALACIÓN DE WORDPRESS-EXPORTER PARA EXPORTAR MÉTRICAS A PROMETHEUS
Pequeño fork hecho por Carlos Monge Ponce para el proyecto de fin de grado del IES Calderón de la Barca. Basado en https://github.com/aorfanos/wordpress-exporter

# PRIMER PASO
Clonamos este repositorio a una ubicación, preferiblemente donde se tengan los archivos de docker o docker compose.

```console
git clone https://github.com/itscarlitos77/wordpress-exporter.git
```

# SEGUNDO PASO
Accededmos al directorio creado, se llamará "wordpress-exporter". Una vez dentro, vamos a crear la imagen del contenedor. Usaremos el siguiente comando:
```console
docker build -t <nombre de la imagen> .
```
En nuestro caso lo llamamos wordpress-exporter para mayor comodidad

# TERCER PASO
Ahora, incluiremos las siguientes lineas de código en nuestro archivo de "prometheus.yml":
```console
scrape_configs:
  - job_name: wordpress_exporter
    honor_timestamps: true
    scrape_interval: 60s
    scrape_timeout: 15s
    metrics_path: /metrics
    scheme: http
    static_configs:
      - targets: ["<IP del exportador>:11011"]
```

# CUARTO PASO (ARRANCAR DIRECTAMENTE EL CONTENEDOR)
Si quieres arrancar directamente el contenedor:
```console
docker start <nombre de la imagen>
```
# CUARTO PASO (CON ARCHIVO DOCKER-COMPOSE.YML)
En el archivo de docker-compose.yml, incluiremos la siguientes líneas para incluir la imagen junto con unos parámetros:
```console  
  wordpress-exporter:
    image: wordpress-exporter:latest
    container_name: wordpress-exporter
    command: >
      -auth.user wexporter
      -auth.pass "wexporter"
      -host http://wordpress:2000
      -auth.basic true
    ports:
      - 11011:11011
```
Ahora solo deberías arrancar el archivo. Para comprobar su funcionamiento, en este cas, introduciremos la ruta "localhost:11011/metrics" en nuestro navegador.

## MÉTRICAS PROPORCIONADAS

| Nombre                   |Tipo       | Descripción                 |
|--------------------------|-----------|-----------------------------|
| wordpress_post_count     | Indicador |    WordPress posts count    |
| wordpress_category_count | Indicador |   WordPress category count  |
| wordpress_tag_count      | Indicador |     WordPress tags count    |
| wordpress_page_count     | Indicador |    WordPress pages count    |
| wordpress_comment_count  | Indicador |   WordPress comments count  |
| wordpress_media_count    | Indicador | WordPress media files count |
| wordpress_user_count     | Indicador |    WordPress users count    |
| wordpress_taxonomy_count | Indicador |    WordPress taxonomy count |
| wordpress_theme_count    | Indicador |    WordPress theme count    |
| wordpress_plugin_count   | Indicador |    Wordpress plugin count   |

## NOTA
Puedes modificar la ruta de búsqueda de métricas de wordpress en el archivo que se encuentra en:
```console
wordpress-exporter/utils/prometheus.go
```

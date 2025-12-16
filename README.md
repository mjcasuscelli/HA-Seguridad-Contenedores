Orquestación Docker Compose

Arquitectura de 3 capas - Frontend, API, Base de Datos orquestada mediante Docker Compose.
Aplica mínimo privilegio.

Contenedores con directiva read_only: Esto impide que un atacante que logre penetrar en la aplicación pueda modificar archivos del sistema, instalar herramientas de hacking o persistir malware en el disco. No puede instalar paquetes.

Manejo de escritura en memoria: Tmpfs: Dado que el sistema de archivos es de solo lectura, se mapearon volúmenes temporales en memoria (tmpfs) para las rutas críticas que requieren escritura legítima (como /tmp, /var/run, /var/cache/nginx). Esto asegura la funcionalidad sin comprometer la seguridad del disco.

Gestión de Secretos: Las credenciales de la base de datos (usuario y contraseña) no se encuentran hardcodeadas en las imágenes de Docker (Dockerfile). Se inyectan únicamente en tiempo de ejecución (Runtime) a través de variables de entorno en el docker-compose.yml, desacoplando el código de la configuración sensible.

Usuario No-Root: Ninguna aplicación corre con privilegios de administrador. Si comprometen la API, no tienen control total del servidor.


Se crearon dos redes virtuales: 
frontend-net: Pública, expone el servicio web.
backend-net: Interna (internal: true). BD sin acceso web, ni acceso fuera del cluster. Reducción de zona de ataque.
healtchecks: Nativos en bd, api espera que la base este iniciada.

Las imagenes ya existen en DockerHub

El Docker Compose las va a buscar directamente.

docker pull mjcasuscelli/db:1.0.2
docker pull mjcasuscelli/frontend:1.0.2
docker pull mjcasuscelli/api:1.0.2

Estas imagenes estan firmadas. 

Requiere tener instalado docker 

cd ruta/a/tu/carpeta/
git clone https://github.com/mjcasuscelli/HA-Seguridad-Contenedores.git
cd HA-Seguridad-Contenedores/
docker compose up -d
La parte up levanta el sistema y la -d (detached) lo hace en segundo plano para no bloquear tu terminal

docker compose ps
para ver el estado de los contenedores

deberia estar:
app-frontend, app-api, app-db
todos en UP en la columna status

Frontend:  http://localhost - página de bienvenida
api: http://localhost:8081 - mensaje de texto confirmando la fecha y hora

docker compose down -> para apagar los contenedores.



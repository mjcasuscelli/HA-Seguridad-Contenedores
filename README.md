Orquestación Docker Compose

Arquitectura de 3 capas - Frontend, API, Base de Datos orquestada mediante Docker Compose.
Aplica mínimo privilegio.

Contenedores con directiva read_only: Esto impide que un atacante que logre penetrar en la aplicación pueda modificar archivos del sistema, instalar herramientas de hacking o persistir malware en el disco.

Manejo de escritura en memoria: Tmpfs: Dado que el sistema de archivos es de solo lectura, se mapearon volúmenes temporales en memoria (tmpfs) para las rutas críticas que requieren escritura legítima (como /tmp, /var/run, /var/cache/nginx). Esto asegura la funcionalidad sin comprometer la seguridad del disco.

Gestión de Secretos: Las credenciales de la base de datos (usuario y contraseña) no se encuentran hardcodeadas en las imágenes de Docker (Dockerfile). Se inyectan únicamente en tiempo de ejecución (Runtime) a través de variables de entorno en el docker-compose.yml, desacoplando el código de la configuración sensible.

Se crearon dos redes virtuales: 
frontend-net: Pública, expone el servicio web.
backend-net: Interna (internal: true). BD sin acceso web, ni acceso fuera del cluster. Reducción de zona de ataque.
healtchecks: Nativos en bd, api espera que la base este iniciada.


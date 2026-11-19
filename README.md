Servidor FTP Seguro con ProFTPD y Docker
Proyecto de despliegue, configuración y securización de un servidor de transferencia de archivos (FTP) utilizando ProFTPD dentro de un contenedor Docker. Incluye la configuración de acceso anónimo, usuarios virtuales y cifrado TLS.

Tecnologías y Herramientas
Contenedores: Docker
Servidor FTP: ProFTPD (sobre Ubuntu)
Seguridad: TLS/SSL (OpenSSL)
Cliente FTP: FileZilla (interfaz gráfica) y tnftp (línea de comandos en WSL)

Funcionalidades y Conceptos Aplicados
El proyecto se divide en varias configuraciones clave sobre el archivo proftpd.conf:

1.Despliegue y Modo Pasivo:
Creación de un Dockerfile para levantar el servidor ProFTPD.
Configuración del modo pasivo (PassivePorts 50000 50034 y MasqueradeAddress) para sortear cortafuegos y permitir la transferencia de datos.

2.Acceso Anónimo:
Configuración de un directorio <Anonymous /srv/ftp/distancia> con permisos de solo lectura (los usuarios pueden descargar, pero no escribir).
Límite de conexiones simultáneas y uso de RequireValidShell off.

3.Usuarios Virtuales:
Creación de usuarios virtuales utilizando la herramienta ftpasswd (sin necesidad de crear usuarios reales en el sistema operativo).
Uso de AuthUserFile y enjaulamiento de los usuarios en su propio directorio home mediante DefaultRoot ~.

4.Conexión Segura (FTPS):
Configuración del módulo mod_tls (tls.conf) para forzar conexiones cifradas mediante certificados SSL, garantizando que las credenciales y los archivos viajen de forma segura.

Cómo ejecutar el proyecto
Asegúrate de tener Docker instalado.
Construye la imagen: docker build -t mi-servidor-ftp .
Arranca el contenedor mapeando los puertos (control y pasivos):
docker run -d --name mi-contenedor-ftp -p 21:21 -p 50000-50034:50000-50034 mi-servidor-ftp
Conéctate usando un cliente FTP (como FileZilla) a localhost con el usuario anónimo o virtual que hayas configurado.

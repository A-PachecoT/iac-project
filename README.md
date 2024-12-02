# Solución a la práctica calificada 5
**Nombre:** André Joaquín Pacheco Taboada
**Código:** 20222189G
**Correo:** andre.pacheco.t@uni.pe
**Curso:** Desarrollo de Software
**Fecha:** 02/12/2024

## Ejercicio 1
Para este ejercicio se creó el playbook [main.yml](ansible/ejercicio1/main.yml) que se encarga de configurar la máquina virtual con los siguientes requerimientos:
- Actualizar e instalar paquetes
    Usé la misma tarea que se implementó en el laboratorio.
- Configurar la zona horaria y locales a America/Lima
    Para esto se usó el módulo `timezone` para configurar la zona horaria y el módulo `locale_gen` para configurar los locales.
    Referencias:
    https://gist.github.com/ZeroDeth/4900c65af505f5398bd6db94cf4a54d4
    https://docs.ansible.com/ansible/latest/collections/community/general/locale_gen_module.html
    Nota: le puse become: true para activar escalado de privilegios, cosa que vi en la documentación de Ansible.
- Crear un usuario y grupo con permisos de administrador, con contraseña cifrada.
    Para esto se usó el módulo `user` con el parámetro `pwdhash` para establecer la contraseña del usuario.
    ```
    pwdhash: "{{ 'contracifrada' | password_hash('sha512') }}"
    ```
    Nota: por defecto el hash es sha512, pero igual lo especifiqué para mayor transparencia.
    Referencias:
    https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html
    https://docs.ansible.com/ansible/latest/collections/ansible/builtin/password_hash_filter.html

## Ejercicio 2
Para este ejercicio se creó el playbook [main.yml](ansible/ejercicio2/main.yml) que implementa un servidor web seguro con los siguientes componentes:

- Instalación y configuración de Nginx
    Se utiliza el módulo `apt` para instalar Nginx y se configura con SSL mediante una plantilla personalizada.

- Generación de certificados SSL autofirmados
    Se crea un directorio SSL y se generan certificados autofirmados usando OpenSSL con una validez de 365 días.
    ```yaml
    command: openssl req -x509 -nodes -days 365 -newkey rsa:2048 
            -keyout /etc/nginx/ssl/nginx.key 
            -out /etc/nginx/ssl/nginx.crt
    ```

- Configuración de Nginx con SSL
    Se utiliza una [plantilla](templates/nginx-ssl.conf.j2) que:
    - Redirecciona todo el tráfico HTTP a HTTPS
    - Configura los certificados SSL
    - Implementa protocolos seguros (TLSv1.2 y TLSv1.3)
    - Define cifrados seguros recomendados

- Configuración del firewall UFW
    Se configura el firewall para permitir solo el tráfico necesario:
    - SSH (puerto 22)
    - HTTP (puerto 80)
    - HTTPS (puerto 443)
    La política por defecto es denegar todo el tráfico no especificado.


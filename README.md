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


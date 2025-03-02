# AnsibleModulo-GetEnt
# Documentación del Módulo Getent en Ansible

## Introducción
El módulo `getent` en Ansible se utiliza para recuperar entradas de bases de datos del sistema compatibles con el comando `getent`, como passwd, group, hosts y más. Esto es útil para consultar información del sistema de manera dinámica dentro de los playbooks.

## Instalación de Ansible
Para utilizar el módulo `getent`, asegúrate de que Ansible está instalado en tu sistema. Si no está instalado, puedes configurarlo con:

```bash
sudo apt update
sudo apt install ansible -y
```

Los servidores de destino también deben tener instalado `libc-bin` o un paquete equivalente que proporcione `getent`. En distribuciones basadas en Debian, puedes asegurarte con:

```yaml
- name: Asegurar que getent está disponible
  hosts: all
  become: yes
  tasks:
    - name: Instalar el paquete necesario
      ansible.builtin.apt:
        name: libc-bin
        state: present
        update_cache: yes
```

## Uso del Módulo `getent`
El módulo `getent` recupera información de bases de datos del sistema como passwd, group y hosts. Su sintaxis básica es:

```yaml
- name: Obtener información de un grupo del sistema
  ansible.builtin.getent:
    database: "group"
    key: "sudo"
```

### Opciones Principales:
| Opción | Descripción |
|---------|-------------|
| `database` | Especifica la base de datos a consultar (ej. passwd, group, hosts) |
| `key` | Entrada específica a buscar (opcional) |
| `split` | Indica si dividir los resultados en una lista (por defecto: yes) |

## Ejemplos de Consultas
### 1️⃣ Obtener Información de un Usuario
```yaml
- name: Obtener detalles de un usuario específico
  ansible.builtin.getent:
    database: "passwd"
    key: "usuario"
```

### 2️⃣ Obtener Información de un Grupo
```yaml
- name: Obtener detalles del grupo sudo
  ansible.builtin.getent:
    database: "group"
    key: "sudo"
```

### 3️⃣ Obtener Entradas de Hosts
```yaml
- name: Obtener detalles de un host específico
  ansible.builtin.getent:
    database: "hosts"
    key: "localhost"
```

## Ejemplo Completo: Consultar y Mostrar Información del Sistema
Este playbook consulta datos de usuarios y grupos y muestra los resultados:

```yaml
- name: Recopilar información del sistema
  hosts: all
  tasks:
    - name: Obtener todas las cuentas de usuario
      ansible.builtin.getent:
        database: "passwd"
      register: usuarios

    - name: Obtener todos los grupos del sistema
      ansible.builtin.getent:
        database: "group"
      register: grupos

    - name: Mostrar datos de los usuarios recopilados
      debug:
        var: usuarios

    - name: Mostrar datos de los grupos recopilados
      debug:
        var: grupos
```
![image](https://github.com/user-attachments/assets/f7678f94-9d03-4345-997c-f716a7b48474)


## Conclusión
El módulo `getent` en Ansible permite recuperar información del sistema de manera dinámica. Es especialmente útil para auditorías, validación de configuraciones y gestión de accesos de forma dinámica.



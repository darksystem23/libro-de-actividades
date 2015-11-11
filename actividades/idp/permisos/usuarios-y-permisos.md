#Usuarios y permisos

![logo-permisos](./images/logo-permisos.jpeg)

* Entregar documento en formato ODT o PDF con el informe de la actividad realizadas y las capturas solicitadas.

#1. SO Windows 7
* Configurar la MV:
    * Modo de red VBox en modo puente.
    * Nombre de equipo: primer-apellido-del-alumno (Esto lo podemos consultar/modificar en `Inicio -> Equipo -> Btn derecho -> Propiedades`).
    * IP estática: 172.19.XX.11
    * Máscara: 255.255.0.0
    * Gateway: 172.19.0.1
    * DNS1: 8.8.4.4
* Ir a `miPC -> Botón derecho administrar -> Gestión de usuarios`.
* ¿Cuántos usuarios hay que no aparecen en la ventana de inicio al sistema? ¿Por qué?

##1.1 Usando el GUI Windows
* Crear el grupo `jedis` y dentro los usuarios `jedi1` y `jedi2`.
* Los miembros del grupo `jedis` ponerlos dentro del grupo administradores, para que puedan actuar como superusuarios.
* Capturar imagen. Para comprobar que los usuarios y grupos se han creado correctamente vamos a 
`Equipo -> Botón Derecho -> Administrar -> Usuarios y grupos`.
* Para cada miembro del grupo profesores:
    * Crear la carpeta `C:\Users\jedi1\private`.
    * Crear la carpeta `C:\Users\jedi1\group`.
    * Crear la carpeta `C:\Users\jedi1\public`.

![logo-grupos-permisos](./images/logo-grupos-permisos.jpeg)

> **INFORMACIÓN: Permisos NTFS**
>
> En Windows las carpetas HEREDAN los permisos de su carpeta padre. Para desactivar esta función 
en una carpeta determinada, haremos lo siguiente:
>
> * Botón derecho sobre la carpeta -> propiedades -> seguridad.
> * Opciones avanzadas -> cambiar permisos.
> * Desactivar herencia "Incluir permisos heredables" -> Quitar -> Aceptar.
> * Aplicar y Aceptar.
>
> Para modificar los permisos de una carpeta vamos a `Botón derecho -> Propiedades -> Seguridad -> Editar`.

* Vamos a modificar los permisos de la siguiente forma:
    * private: El usuario propietario tendrá control total y nadie más tendrá permisos.
    * group: grupo `jedis` permisos de lectura, y el usuario propietario control total.
    * public: todos tienen permiso de lectura, y el usuario propietario control total.
* Capturar imagen del resultado de la asignación de permisos.

Veamos un ejemplo de permisos para la carpeta public:

![win-permisos-gui-public-dir](./images/win-permisos-gui-public-dir.png)

##1.2 Usando los comandos Windows

* Vamos a usar los comandos de la nueva shell de Windows, llamada PowerShell. 
Para ello buscamos en el menú PowerShell -> (botón derecho) -> Iniciar como Administrador.
Si no lo hacemos como administrador, no tendremos los privilegios necesarios, 
y no podremos crear los usuarios.

> **EJEMPLO**
>
> Veamos un ejemplo de creación de usuarios en PowerShell:
> * Cambiar nombre-pc por el nombre del PC de cada uno.
> * Cambiar "alumno1" por el nombre del usuario que deseamos crear.
> * Cambiar "123456" por el valor de password que querramos.
>
> ```
> PS C:\> [ADSI]$server="WinNT://nombre-pc"
> PS C:\> $usu1=$server.Create("User","alumno1")
> PS C:\> $usu1
> distinguishedName :
> Path : WinNT://GLOBOMANTICS/nombre-pc/alumno1
> PS C:\> $usu1.SetPassword("123456")
> PS C:\> $usu1.SetInfo()
> ```
>
> Veamos un ejemplo para crear grupo y añadir usuario al mismo:
> * Cambiar nombre-pc por el nombre del PC de cada uno.
> * Cambiar "alumnos" por el nombre del grupo que deseamos crear.
> * Cambiar "alumno1" por el nombre del usuario que deseamos añadir al grupo.
>
> ```
> PS C:\> [ADSI]$server="WinNT://nombre-pc"
> PS C:\> $grupo=$server.Create("Group","alumnos")
> PS C:\> $grupo
> PS C:\> $grupo.SetInfo()
> PS C:\> $usu1 = [adsi]"WinNT://nombre-PC/alumno1,user"
> PS C:\> $grupo.Add($usu1.path)
> ```
>
> Veamos un ejemplo para añadir usuario a un grupo ya existente:
> ```
> PS> $computerName = $env:COMPUTERNAME
> PS> $groupName = "alumnos"
> PS> $grupo = [adsi]"WinNT://$computerName/$groupName,group"
> PS> $grupo
> PS> $usu2 = [adsi]"WinNT://nombre-PC/alumno2,user"
> PS> $grupo.Add($usu2.path)
> ```

* Ahora vistos los ejemplos, vamos a crear el grupo `siths`.
* Pondremos a los usuarios `sith1` y `sith2`, dentro de los grupos `siths` y `usuarios`.

> Al ponerlos dentro del grupo Usuarios conseguimos que se muestren los iconos en la ventana de 
inicio de sesión del sistema.
>
> [Más información sobre la creación de usuarios con PowerShell](https://www.petri.com/create-local-accounts-with-powershell)

* Para cada usuario del grupo `siths` hay que:
    * Crear la carpeta `C:\Users\sith\private`
    * Crear la carpeta `C:\Users\sith1\group`
    * Crear la carpeta `C:\Users\sith1\public`

> Veamos un ejemplo de permisos por comandos:
>
> ```
> (Comprobar los permisos actuales de public)
> PS C:\> icacls public
>
> (Dar permisos de lecturas al grupo todos)
> PS C:\> icacls public /grant todos:R
>
> (Dar permisos control total al usuario alumno1)
> PS C:\> icacls public /grant alumno1:F
>
> (Quitar los permisos heredados a la carpeta public)
> PS C:\> icacls public /inheritance:r
>
> (Comprobar los permisos actuales de public)
> PS C:\> icacls public
>
> (Para quitar el acceso al directorio para todos los usuarios del dominio:)
> PS C:\> icacls directorio /remove \"Usuarios del dominio"
> ```
> 

* Modificar los permisos de la siguiente forma:
    * private: Sólo el usuario propietario tendrá permisos lectura/escritura.
    * group: grupo `siths` permisos de lectura, y usuario propietario permisos de lectura y escritura.
    * public: todos tienen permiso de lectura, y el usuario propietario tiene permisos de lectura y escritura.


#2. SO GNU/Linux OpenSUSE
* Configurar la MV:
    * Modo de red VBox en modo puente.
    * Nombre de equipo: primer-apellido-del-alumno (Debian/Ubuntu modificar /et/hostname y /etc/hosts, OpenSUSE usar Yast2)

> **INFORMACIÓN**
>
> Para configurar la red sin entorno gráfico en Debian/Ubuntu, modificaremos el 
contenido del fichero /etc/network/interfaces con lo siguiente:
>
> ```
> auto eth0
> iface eth0 inet static
> address 172.16.108.XX
> netmask 255.255.0.0
> gateway 172.16.1.1
> dns-nameservers 172.16.1.1
> ```
> 
> Para configurar la red mediante entorno gráfico podemos usar NetworkManager en Debian/Ubuntu y Yast en OpenSuse.
>

* Ir al gestor de usuarios:
    * OpenSUSE: Ir a Yast -> Gestión de Usuarios.
    * Debian/Ubuntu: Ir a `Aplicaciones -> Herramientas -> Configuración del sistema -> Preferencias -> Cuentas de usuarios`, 
o bien ir a `Sistemas -> Administración -> Usuarios y Grupos`.
* ¿Cuántos usuarios hay que no aparecen en la ventana de inicio al sistema? ¿Por qué?

##2.1 Usando el GUI GNU/Linux

Capturar imagen de las siguientes acciones:
* Crear el grupo `jedis` y dentro los usuarios `jedi1` y `jedi2`.
* Para cada usuario del grupo profesores:
    * Crear la carpeta `/home/jedi1/private`.
    * Crear la carpeta `/home/jedi1/group`.
    * Crear la carpeta `/home/jedi1/public`.
    
Veamos un ejemplo de permisos por el entorno GUI:

![linux-permisos-gui](./images/linux-permisos-gui.png)

* Modificar los permisos de las carpetas de la siguiente forma:
    * private: Sólo el usuario propietario tendrá permisos lectura/escritura.
    * group: grupo `jedis` permisos de lectura, y usuario propietario permisos de lectura y escritura.
    * public: todos tienen permiso de lectura, y el usuario propietario tiene permisos de lectura y escritura.

##2.2 Sudoers (Grupo privilegiado)
Vamos a dar privilegios de superusuario a los miembros del grupo `jedis`.

> El comando `sudo` nos permite ejecutar comandos como si fuéramos el administrador del equipo.
Pero dicho comando sólo lo pueden ejecutar algunos elegidos.

* Vamos a configurar a los usuarios del grupo de `jedis` para poder tener privilegios de uso del comando sudo.

* Para ello podemos hacerlo de dos formas:
    * (A) Abrir el editor de la configuración sudo (/etc/sudoers) haciendo `visudo` (Debian/Ubuntu). Y añadimos la línea `%jedis ALL = (root) NOPASSWD:ALL`
            %alumnos ALL = (root) NOPASSWD:/sbin/shutdown, /sbin/fdisk -l, /sbin/dhclient
        (B) Usar Yast en OpenSUSE. Veamos imagen de ejemplo:

sudoers

    Guardar y salir
    Ahora los usuarios del grupo profesores ya pueden ejecutar el comando sudo, para realizar todas las tareas administrativas. Y los usuarios del grupo alumnos sólo para algunos comandos.


Veamos un ejemplo de un usuario sin privilegios que intenta usar el comando sudo:

sudo-error


##2.3 Usando los comandos
> Recordar los comandos adduser, addgroup.

Capturar imagen de las siguientes acciones:
* Crear el grupo `siths`.
* Crear los usuarios `sith1` y `sith2` dentro del grupo anterior.
* Ejecutar el comando `cat /etc/passwd`. Así vemos todos los usuarios 
definidos el el sistema. Algunos son usados por personas físicas, y otros 
son internos para uso de aplicaciones o del sistema operativo.

* Para cada usuario del grupo `siths` hacer:

    Crear la carpeta /home/alumno1/private
    Crear la carpeta /home/alumno1/group
    Crear la carpeta /home/alumno1/public



Modificar los permisos de las carpetas:

    [INFO] Recordar los comandos chown (Cambiar propietario), chgrp (Cambiar grupo propietario), chmod (Cambiar permisos de acceso).
    private: Sólo el usuario alumno1 tendrá permisos lectura/escritura.
    group: grupo alumnos permisos de lectura, y usuario alumno1 permisos de lectura y escritura.
    public: todos tienen permiso de lectura, y el usuario alumno1 tiene permisos de lectura y escritura.



ANEXO
A.1 Personalización de usuarios GNU/Linux
En OpenSUSE vemos que cuando queremos invocar el comando "ifconfig" con un usuario normal debemos hacerlo con la ruta absoluta "/sbin/ifconfig".

path

[INFO] Existe una variable llamada PATH, configurada para cada usuario de forma difierente. Dicha variable de entorno contiene las rutas de los ejecutables/comandos. No es necesario escribir la ruta completa para invocar a los comandos/programas que estén en alguna de estas rutas.

Para cambiarlo añadimos las siguientes líneas al final del fichero "/home/nombre-de-usuario/.profile":

PATH=$PATH:/sbin
export PATH

Para que los cambios tengan efecto debemos cerrar la sesión.

[INFO] En otras distribuciones se usa el fichero de configuración "/home/nombre-de-usuario/.bashrc"


A.2 Emulador de consola portable para Windows

Cmder (http://bliker.github.io/cmder/) is a software package created out of pure frustration over the absence of nice console emulators on Windows. It is based on amazing software, and spiced up with the Monokai color scheme and a custom prompt layout. Looking sexy from the start.


A.3 Crear usuarios en Windows

En la consola cmd se pueden crear usuarios mediante el comando:
net user nombre-usuario clave-usuario /add
net localgroup nombre-grupo nombre-usuario /add
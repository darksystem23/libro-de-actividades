
*(Actividad nueva para e curso 2015-2016)*

#Servidor LDAP - OpenSUSE

> Enlaces de interés:
> * Teoría
>     * [Presentación: ¿Qué es LDAP?](http://www.youtube.com/watch?v=CXe0Wxqep_g)
>     * [Presentación: Los ficheros LDIF](http://www.youtube.com/watch?v=ccFT94M-c4Y)
> * OpenSUSE
>     * [Configurar_LDAP_usando_YaST](https://es.opensuse.org/Configurar_LDAP_usando_YaST)
>     * [Ingreso_de_usuarios_y_grupos_en_LDAP_usando_YaST](https://es.opensuse.org/Ingreso_de_usuarios_y_grupos_en_LDAP_usando_YaST)
>     * [Configurar servidor LDAP en OpenSUSE con Yast](http://www.youtube.com/watch?v=NsQ1zPpoVBc)
> * Otros
>     * [LD01: Instalar Servidor OpenLDAP](http://www.youtube.com/watch?v=E0mIYO_vbx8)
>     * Min 38: Crear config dir a partir de config text.
>     * [Tool Openfile](http://www.openfiler.com/)
>     * Tool Zentyal

Vamos a usar una MV OpenSUSE 13.2
* Servidor LDAP:
    * IP estática del servidor 172.18.XX.51 (Donde XX es su número de puesto).
    * Nombre equipo: `ldap-server-XX`
* Cliente LDAP:    
    * IP estática del cliente 172.18.XX.52
    * Nombre equipo: `ldap-client-XX`


#1. Instalación Servidor LDAP
Comenzamos la instalación del servidor LDAP:
* Proceder a la instalación del módulo Yast para ldap (`yast-auth-server`)

Proceder a la instalación del [servidor LDAP](https://es.opensuse.org/Configurar_LDAP_usando_YaST)
* Ir a Yast -> Servidor de autenticación.
* Tipo de servidor: autónomo
* Configuración TLS: NO habilitar
* Configurar el servidor LDAP, usando como DN el siguiente: `dc=nombredealumnoXX, dc=curso1516`.
Donde XX es el número del puesto de cada uno.
* Asegurarse que tenemos definido el nombre DNS de la máquina `nombredealumnoXX.curso1516` y
`nombrealumnoXX`,  en el fichero /etc/hosts con su IP correspondiente.
* Una vez instalado, comprobar a parar y reiniciar el servicio de forma manual. En OpenSUSE usaremos
el comando `systemctl status ldap`, `systemctl stop ldap`, `systemctl  start ldap`.
* Comprobar también que el servicio se inicia automáticamente al reiniciar la máquina. 
* Instalar alguna de las herramientas cliente LDAP de OpenSUSE con Yast.

> Mediante el cliente LDAP podremos escribir y/o consultar información en la base de datos LDAP.

#2. Introducir datos en LDAP


Realizar las siguientes tareas:
* Consultar Vídeo [LPIC-2 202 LDAP Client Usage](http://www.youtube.com/embed/ZAHj93YWY84).
* Crear las unidades organizativas: `grupos` y `usuarios`.
* Crear dentro de ou=grupos, los grupos de `jedis` y `siths`.
* Crear dentro de ou=usuarios, varios usuarios `jedi11`, `jedi12`, `sith21`, `sith22`.
* Usar cliente LDAP desde la máquina local y comprobar que podemos acceder al contenido del servidor LDAP.
* Usar cliente LDAP desde otra máquina y comprobar que podemos acceder al contenido del servidor LDAP.

Vemos un ejemplo de un árbol de datos en LDAP:

![arbol](./images/arbol.png)
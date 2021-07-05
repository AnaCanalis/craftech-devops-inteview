# AWS Diagrama de red
En la imagen _Aws Network Diagram Canalis_ se observa la propuesta para el ejercicio 1 de la entrevista técnica de Craftech.
El diagrama de red está planteado para garantizar **alta disponibilidad y cargas variables**.
Cuando los _usuarios_ quieren acceder a la aplicación, lo solicitan al _Route 53_ utilizando un DNS a través del _Internet Gateway_. Se utilizará un servicio de _Firewall_ para proteger la red de actividades maliciosas.
El load balancer será el encargado de distribuir el tráfico entrante entre las diferentes zonas de disponibilidad. 
##Aspectos generales
El esquema contiene una _subred pública_ y _subred privada_.
En la **subred pública** contamos con un _NAT Gateway_. Se utiliza para proteger recursos privados del público y a su vez que las instancias privadas puedan acceder a internet (para por ejemplo descargar software o interactuar con APIs externas)
En la **subred privada** se encuentran tres grandes bloques:
* Frontend
* Backend
* Database
Tanto frontend como backend, poseen instancias EC2 dentro de un _security group_ para que puedan comunicarse entre sí. Son de modalidad _auto scalling_ por lo que si se necesitan más recursos, automáticamente se generarán nuevas instancias EC2.
Un nuevo loadbalancer, distribuye tráfico a las instancias EC2 del backend. Desde el backend se hacen peticiones a microservicios como _Amazon SNS_ para enviar mensajes a los clientes, un servicio externo de _correo_ y ubicaciones a través de _maps_.
Luego se accede al bloque Database que contiene una _base de datos relacional_ Aurora y una _no relacional_ Dynamo DB.
###Regiones y Zonas de disponibilidad
La arquitectura dentro de AWS está planteada en dos _regiones_ diferentes. A la izquierda EE UU Este (Norte de Virginia) y a la derecha Canadá (Central).
####Región de Estados Unidos
Esta será la región **activa**. Consta con dos zonas de disponibilidad. 
####Región Canadá Central
Esta región se encuentra **pasiva**. El objetivo de tener montada nuestra aplicación en otra región y en otro país, es que ante posibles problemas de Amazon, catástrofes naturales, climáticas, etcétera en la región o país de Estados Unidos, podremos poner en funcionamiento la presente región y los usuarios/clientes podrán seguir accediendo a la aplicación sin percibir estos conflictos. Por este motivo es que el planteamiento inicial es con instancias EC2 mínimas pero con un load balancer montado y auto scalling en caso de necesitar redireccionar el tráfico hacia aquí.
Las bases de datos están conectadas con las bases de datos de la región activa de manera asíncrona para almacenar y tener una copia de seguridad en caso de necesitar utilizar la región Canadá. 


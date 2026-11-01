Es un protocolo que permite configurar dinámicamente clientes para que se conecten a una red.

Asigna direcciones de forma dinámica, y mantiene un control de las direcciones en desuso.

Anteriormente existieron ARP y BOOTP (fue una mejora de este, y es compatible)


---
### Mensajes

#### DHCP Discover: 
El cliente busca servidores disponibles
Envía este mensaje por BroadCast, ya que no tiene IP, ni conoce la del servidor.

#### DHCP Offer:
Respuesta del servidor con IP disponible (dirección, GW, DNS, tiempo, ...)
(Puede haber más de una respuesta)

#### DHCPREQUEST:
El cliente solicita los parámetros que le ofrece el servidor, se envía por BC, ya que sigue sin configurar (además así se enteran el resto de servidores de a cuál responde).

Una vez está configurado, volverá a hacer este mensaje para renovar, UC al mismo servidor.

#### DHCPACK:
El servidor confirma los parámetros de configuración al cliente.
Los envía por BC, porque el cliente todavía no está configurado y para que se entere toda la red.

#### DHCPNACK:
El servidor rechaza la solicitud, en caso de que no se pueda asignar, envía este mensaje para que el cliente pida otra.

#### DHCPDECLINE:
El cliente rechaza la configuración (mala config, o IP en uso)
El servidor marca esa IP como mala o en uso para no ofrecerla a otro cliente, el servidor ofrece otra dirección.

#### DHCPRELEASE:
El cliente informa al servidor de que no va a continuar utilizando la dirección (para que se pueda asignar a otro cliente).

Aquí sí está configurado, con lo que UC.


Es un protocolo que permite configurar dinámicamente clientes para que se conecten a una red.

Asigna direcciones de forma dinámica, y mantiene un control de las direcciones en desuso.

Anteriormente existieron ARP y BOOTP (fue una mejora de este, y es compatible)

Puerto Servidor: 67
Puerto Cliente: 68

---
### Mensajes

#### DHCP Discover: 
El cliente busca servidores disponibles
Envía este mensaje por BroadCast, ya que ni tiene IP ni conoce la del servidor.

#### DHCP Offer:
Respuesta del servidor con IP disponible (dirección, GW, DNS, tiempo, ...)
(Puede haber más de una respuesta)

#### DHCPREQUEST:
El cliente solicita los parámetros que le ofrece el servidor, se envía por BC, ya que sigue sin configurar (además así se enteran el resto de servidores de a cuál responde).

Una vez está configurado, volverá a hacer este mensaje para renovar, UC al mismo servidor.

#### DHCPACK:
El servidor confirma los parámetros de configuración al cliente.
Los envía por BC, porque el cliente todavía no está configurado y para que se entere toda la red.
Tras este mensaje, el cliente se configura

#### DHCPNACK:
El servidor rechaza la solicitud, en caso de que no se pueda asignar, envía este mensaje para que el cliente pida otra.

#### DHCPDECLINE:
El cliente rechaza la configuración (mala config, o IP en uso)
El servidor marca esa IP como mala o en uso para no ofrecerla a otro cliente, el servidor ofrece otra dirección.

#### DHCPRELEASE:
El cliente informa al servidor de que no va a continuar utilizando la dirección (para que se pueda asignar a otro cliente).

Aquí sí está configurado, con lo que UC.

---
### Renovación de IP
Puesto que las concesiones de IP son dinámicas, cada cierto tiempo el cliente tendrá que renovarla (para que no se gasten todas en clientes que se fueron de la red).
Existen 2 tiempos

**Tiempo Máximo**: el máximo tiempo que puede dar una IP a un cliente.
**Tiempo por Defecto**: Si el cliente no especifica el tiempo, le dará este.

Cuando el cliente pida un tiempo entre estos 2, se lo dará, si pide mayor al máximo, le dará el máximo.

Las asignaciones de dirección tienen 3 tiempos (independientemente de cuánto sea).

- **T1**: 1/2 del total, el cliente tratará de renovar su configuración (Request), UC al mismo servidor, si responde, se reinicia. (confirma con ACK por UC)
- **T2**: 7/8 del total, asume que el servidor no responde, envía REQUEST para que escuche **cualquier** servidor (BC). (confirma con ACK por BC)
- **T3**: se acaba el tiempo de concesión, pierde la configuración.

---
### Relay Agent
(Agente de retransmisión)
Puesto que los BC no son enrutables, existen los relay agent, que lo reenvía a través de subredes.

En primer lugar cuando encuentra un DISCOVER, lo encapsula en un nuevo paquete IP, modifica el **origen** (para que se lo devuelva directamente a él) y el destino (para que llegue al servidor).
Especifica en giaddr en qué subred está para que el servidor DHCP lo sepa.

Ahora que tenemos un paquete UC, podemos enviarlo a diferentes subredes. 

Cuando lo envía al servidor DHCP, este comprueba el campo **giaddr** y busca una dirección en el pool para esa subred.

Envía el OFFER (por UC) con la configuración al Relay Agent, en **giaddr** pone la IP del Relay Agent.

Relay Agent extrae el mensaje DHCP y lo envía al cliente. 

Hace el mismo proceso para los REQUEST y ACK

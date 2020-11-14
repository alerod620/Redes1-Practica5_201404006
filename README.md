# Practica 5 - Redes 1

## COMPONENTES 
* 1 Router
* 1 EtherSwitch
* 2 Switch
* 4 Máquina Virtual
* 1 Cloud

## TOPOLOGÍA DE RED
![image](https://user-images.githubusercontent.com/61027811/98431714-88059480-207d-11eb-909f-eb027141e708.png)

## CONFIGURACIÓN

### SUBREDES
A continuación se muestra el cálculo de las subredes, utilizando como dirección de red la dirección 192.168.0.0./24. Para ello se inicia con la subred que posee mayor número de host a la subred con menor número de host.

| DEPARTAMENTO | HOST |
|:------:|:-----------:|
| Profesores | 52 |
| Estudiantes | 82 |

#### PASOS :

1. Identificar la mascara actual

> 192.168.1.0/24
> **Máscara 255.255.255.0**

2. Aplicar la formula 
```
2^m - 2 >= Host 
8^2 Host
M = 7
2^7 = 126
126 > 82 
```
3. Obtener la nueva máscara
```
    11111111.11111111.11111111.10000000
       255  .   255  .   255  .   168
```

4. Salto de red: ***constante 256***

```
  256 - 128 = 128
```
  Esto quiere decir que a partir de la 128 inicia la siguiente.
  
Se realiza el mismo procedimiento para las siguientes subredes, utilizando siempre la mascara original y se obtiene el siguiente resultado:


| NO | HOST SOLICITADO | HOST ENCONTRADOS | DIRECCION DE RED | MAX | MASCARA PUNTEADA | PRIMERA IP | ULTIMA IP | DIRECCION DE BROADCAST | 
|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:-----------:|
| 2 | 82 | 126 | 192.168.0.0 | /25 | 255.255.255.128 | 192.168.0.1 | 192.168.0.126 | 192.168.0.127 |
| 1 | 52 | 62 | 192.168.0.128 | /26 | 255.255.255.192 | 192.168.0.129 | 192.168.0.190 | 192.168.0.191 |



### VLAN
* Al iniciar la consola del EtherSwitch se utiliza el comando ***confing terminal*** y luego el comando ***vlan 'vlan'*** para configurar la *VLAN* deseada.
*	Se utiliza ***name 'nombre'*** para darle un nombre a la *VLAN* y el comando ***end*** para salir de la configuración.

| VLAN | Summary |
|:------:|:-----------:|
| 20 | ![image](https://user-images.githubusercontent.com/61027811/98424204-a0ae8400-2056-11eb-8372-a3ca683d8a44.png) |
| 50 | ![image](https://user-images.githubusercontent.com/61027811/98424152-78bf2080-2056-11eb-9cfb-2414f8419e2f.png) |
| 70 | ![image](https://user-images.githubusercontent.com/61027811/98424174-8bd1f080-2056-11eb-811c-aff7c10f016d.png) |

#### ROUTER
Al tener creadas las vlan ahora se procede a crear las subintefaces con los siguientes comandos:

> Int f#/#.#

> Encapsulation dot1q #vlan

> Ip address <Gateway> <Mask>
    
> Exit

Por ejemplo para la vlan 50 se le configuro la ip:`192.168.0.1` y la mascara de red:`255.255.255.128`.

![image](https://user-images.githubusercontent.com/61027811/98432755-192d3900-2087-11eb-849b-fbaac2e4a1c3.png)


### MODO TRUNCAL

#### ETHERSWITCH

* Se inicializa la configuración de la terminal y se ingresa el comando ***int 'interfaz'*** para indicar que *interfaz* se va a configurar.
*	Con el comando ***switchport mode trunk*** se indica que la *interfaz* se configurará en modo truncal.
*	Luego se utiliza ***switchport trunk allowed vlan 1,20,50,70,1002-1005*** para indicar las *VLANs* con las que se trabajarán.
* Esto se realiza para cada *interfaz* que esté habilitado en cada EtherSwitch.

|  | Imagen |
|:------:|:-----------:|
| ESW1 | ![image](https://user-images.githubusercontent.com/61027811/98431422-ba61c280-207a-11eb-8dac-cbd0f781c359.png) |

#### SWITCH

* Solo se ingresa a la configuración del switch y se cambia la *VLAN* para cada puerto.

![image](https://user-images.githubusercontent.com/61027811/98431807-504b1c80-207e-11eb-8b47-d1cfdaeaa82e.png)


## TOPOLOGÍA 2

![image](https://user-images.githubusercontent.com/53104989/99133868-4aef5400-25e1-11eb-9f27-35310cb899df.png)

## COMPONENTES 
* 3 Router
* 1 EtherSwitch
* 2 Switch
* 2 Máquina Virtual
* 1 Cloud

### Configuraciones de topología 2 
### EIGRP 
Protocolo de enrutamiento de Gateway interio.
Se configuro en los dispositivos R2, R3 y R4.

> conf t

> router eigrp 10

> network 10.10.0.0 0.0.0.255

> network 20.10.0.0 0.0.0.255

> network 192.168.15.0 0.0.0.255

> end

### VRRP
Virtual router redundance protocol.
Se configuro en los dispositivos R3 y R4.

## Enrutamiento 
###RIP: protocolo de informacion de entrutamiento.
En R2 se configuro el protocolo RIP hacia las redes de la topología, asi como a la red que conecta las dos topologías 8.8.8.0

![image](https://user-images.githubusercontent.com/53104989/99139467-8e0bf000-25fe-11eb-973e-5739fdd527ea.png)

## Coneción entre Topologías
La conexión entre topologías se realizó mediante vpn, utilizando una maquina virtual con Ubuntu 18, de Google cloud.

![image](https://user-images.githubusercontent.com/53104989/99140125-720b4d00-2604-11eb-8b7b-0801534fd2a4.png)

Se creo, en la máquina virtual, una regla de firewall para la entrada y salida que permite el trafico de cualquier rango de ip en el puerto 1194.

![image](https://user-images.githubusercontent.com/53104989/99140166-df1ee280-2604-11eb-9059-a6d3ce366e8c.png)

Se instalo el archivo de configuraciones de la vpn con el siguiente comando:

> sudo wget https://cubaelectronica.com/OpenVPN/openvpn-install.sh && sudo bash openvpn-install.sh 

Nos conectamos a la red vpn mediante el archivo de cliente que nos genera la maquina virtual, esto nos asigna una ip en la vpn por la cual nos comunicaremos hacia la otra computadora con la otra topología.

![image](https://user-images.githubusercontent.com/53104989/99140222-75eb9f00-2605-11eb-8aba-13e9d42a8c78.png)

En GNS3 la conexión se realiza mediante una nube 

![image](https://user-images.githubusercontent.com/53104989/99140232-81d76100-2605-11eb-88d7-6c8ea95a68a8.png)

La cual se configura de la siguiente manera:

![image](https://user-images.githubusercontent.com/53104989/99140238-8c91f600-2605-11eb-8ad7-b09c269ca2ee.png)

* local port: puerto de conexión de la computadora donde se realiza la configuración.
* remote host: ip de la computadora con la otra topología.
* remote port: puerto de conexión de la computadora con la otra topología.

**los puertos tienen que estar intercambiados en las configuraciones de las dos computadoras, ya que el de conexión de una será el remote de la otra**

La conexión se realiza mediate el UDP tunels el cual se conecta a la interfaz del rauter donde se realiza el ruteo para acceder a la red de la otra topología.

![image](https://user-images.githubusercontent.com/53104989/99140246-a8959780-2605-11eb-8b42-7e0b39622231.png)

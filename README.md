# Practica 5 - Redes 1

## COMPONENTES 
* 1 Router
* 1 EtherSwitch
* 2 Switch
* 4 Máquina Virtual
* 1 Cloud

## TOPOLOGÍA DE RED
![image](https://user-images.githubusercontent.com/61027811/98425567-33e9b880-205b-11eb-88f1-0406793b81ee.png)

## CONFIGURACIÓN

### SUBREDES
* Se utilizó la página http://www.vlsmcalc.net/ para el cálculo de las subredes, utilizando como dirección de red la dirección 192.168.0.0./24.
![image](https://user-images.githubusercontent.com/61027811/98426334-08b49880-205e-11eb-814b-0b3fed03fffb.png)

### VLAN
* La configuración de las vlans se realizan únicamente en el EtherSwitch 1.
* Al iniciar la consola del EtherSwitch se utiliza el comando ***confing terminal*** y luego el comando ***vlan 'vlan'*** para configurar la *VLAN* deseada.
*	Se utiliza ***name 'nombre'*** para darle un nombre a la *VLAN* y el comando ***end*** para salir de la configuración.

| VLAN | Summary |
|:------:|:-----------:|
| 20 | ![image](https://user-images.githubusercontent.com/61027811/98424204-a0ae8400-2056-11eb-8372-a3ca683d8a44.png) |
| 50 | ![image](https://user-images.githubusercontent.com/61027811/98424152-78bf2080-2056-11eb-9cfb-2414f8419e2f.png) |
| 70 | ![image](https://user-images.githubusercontent.com/61027811/98424174-8bd1f080-2056-11eb-811c-aff7c10f016d.png) |

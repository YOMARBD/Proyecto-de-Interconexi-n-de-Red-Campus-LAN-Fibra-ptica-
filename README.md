# Proyecto-de-Interconexion-de-Red-Campus-LAN-Fibra-optica-
Diseño y configuración de una red Campus LAN con fibra óptica monomodo y switch multicapa en Cisco Packet Tracer.

# 🌐 Proyecto de Interconexión de Red Campus LAN (Fibra Óptica & Capa 3)

##  Descripción del Proyecto
Este proyecto presenta el diseño, subneteo y configuración lógica de una infraestructura de red **Campus LAN** distribuida en dos bloques estructurales (Edificio 001 y Edificio 002) con una separación física de **150 metros**. 

La solución resuelve dos desafíos críticos de ingeniería de redes: la atenuación del medio físico debido a la distancia y el enrutamiento subredes utilizando un enfoque híbrido entre un Router central y un Switch Multicapa (Capa 3).

---

## Tecnologías y Herramientas Utilizadas
* **Simulador:** Cisco Packet Tracer v9.0.0+
* **Enrutamiento:** Router Cisco 2911 / ISR Series
* **Conmutación Avanzada:** Multilayer Switch Cisco Catalyst 3650
* **Conmutación LAN:** Switch Cisco Catalyst 2960 / Genérico
* **Medios Físicos:** 
  * Fibra Óptica Monomodo (Single-mode FX) para el Backbone Inter-edificios (150m).
  * Cable de Cobre de Par Trenzado (UTP Cat 6) para la Distribución Horizontal Interna.


## Esquema de Direccionamiento Lógico (Subneteo FLSM)
Se aplicó una segmentación manual utilizando Máscaras de Subred de Longitud Fija (**FLSM**) bajo el prefijo `/26` para optimizar el espacio de direcciones IP y aislar los dominios de difusión.

|       Subred        |     
| **192.168.1.0/26**  |   
| **192.168.1.64/26** | 

|       Rango IP Utilizable    |
|   192.168.1.1 - 192.168.1.62 |
| 192.168.1.65 - 192.168.1.126 |

 | Máscara de Subred |
 |  255.255.255.192  | 
 |  255.255.255.192  |

 | Gateway Asignado | 
 |    192.168.1.1   |
 |   192.168.1.65   |

 | Propósito / Ubicación  |
 | Edificio 001 (Hosts Locales) |
 | Edificio 002 (Switch Multicapa & Hosts) |


---

##  Justificación e Ingeniería del Diseño

### 1. Implementación de Fibra Óptica Monomodo (Capa 1 OSI)
El estándar de cableado estructurado ethernet sobre cobre (UTP) limita la distancia máxima de transmisión a **100 metros** debido a la atenuación de la señal eléctrica. Al existir una distancia de **150 metros** entre edificios, se justificó técnicamente el uso de un tendido de **Fibra Óptica Monomodo** operando a velocidad Gigabit (1000Base-FX), eliminando la degradación de señal y asegurando inmunidad total contra interferencias electromagnéticas externas.

## Comandos Clave de Configuración (Cisco IOS)

### En el Router6 (Enlace hacia Edificio 002)
```cisco
Router6# configure terminal
Router6(config)# interface GigabitEthernet0/1/0
Router6(config-if)# ip address 192.168.1.65 255.255.255.192
Router6(config-if)# no shutdown
```

### En el Router6 (Enlace hacia Edificio 001 - Central)
```cisco
Router6# configure terminal
Router6(config)# interface GigabitEthernet0/0
Router6(config-if)# ip address 192.168.1.1 255.255.255.192
Router6(config-if)# no shutdown
```

### Activación de puerto del switch
```cisco
Switch(config)#in gi1/1/1
Switch(config-if)#no shutdown
Switch(config-if)#exit
```

---

## Pruebas de Conectividad (Verificación)
La topología fue validada exitosamente mediante pruebas de ICMP (*Ping*), logrando una convergencia del **100% de éxito con 0% de pérdida de paquetes** en comunicaciones extremo a extremo entre los diferentes segmentos de red (ej. de `192.168.1.10` a `192.168.1.70`).

---
Documentación desarrollada por **Yomar Edito Barrios de la Cruz**.


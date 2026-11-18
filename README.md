# Homelab Proxmox

Laboratorio personal de virtualización, redes y administración de
sistemas, desarrollado como complemento práctico a mis estudios de
**Administración de Sistemas Informáticos en Red (ASIR)**.

El laboratorio está montado sobre un portátil reutilizado y dedicado a
esta función. Utiliza **Proxmox VE** como hipervisor y está diseñado
para practicar con máquinas virtuales, contenedores Linux, redes
segmentadas y servicios de infraestructura.

> **Estado del proyecto:** la instalación base de Proxmox y la
> configuración de red del host están hechas. La documentación técnica
> se publica de forma progresiva; en cada apartado indico su estado real
> para distinguir lo implementado de lo que sigue pendiente.

## Objetivos

- Practicar virtualización y administración de sistemas Linux y Windows.
- Diseñar y configurar redes virtuales aisladas.
- Trabajar con routing, firewall, NAT, DHCP y DNS.
- Desplegar máquinas virtuales y contenedores LXC según las necesidades
  de aislamiento y recursos.
- Automatizar tareas de aprovisionamiento cuando sea posible.
- Documentar la configuración, las pruebas, los problemas encontrados y
  sus soluciones para que el entorno pueda reproducirse.

## Hardware

| Componente | Detalle |
|---|---|
| Equipo | Portátil dedicado al laboratorio |
| CPU | Intel Core i7 (12 hilos) |
| RAM | 32 GB DDR4 |
| Almacenamiento | Aproximadamente 1 TB |
| Hipervisor | Proxmox VE 9.2 |

## Infraestructura y red

La red está planteada en dos segmentos:

- **Red física (`vmbr0`):** conecta el host Proxmox y la interfaz WAN
  del firewall con la red doméstica.
- **Red virtual aislada (`vmbr1`):** bridge interno sin conexión física
  directa, destinado a las máquinas del laboratorio que estarán detrás
  del firewall.

`vmbr0` es el bridge conectado a la interfaz física `nic0`. `vmbr1` es
un bridge interno, sin dirección IP ni puertos físicos asociados.

### Direccionamiento

| Elemento | Dirección | Estado / función |
|---|---|---|
| Router doméstico | `192.168.1.1/24` | Gateway de la red física |
| Host Proxmox (`homelab`) | `192.168.1.50/24` | Configurado: IP estática en `vmbr0` |
| OPNsense WAN | `192.168.1.40/24` | Previsto: interfaz hacia la red física |
| OPNsense LAN | `10.10.10.1/24` | Previsto: gateway de la red aislada |
| Servidor de laboratorio | `10.10.10.10/24` | Previsto |
| Cliente de laboratorio | `10.10.10.100/24` | Previsto |

### Topología objetivo

```text
             Internet
                 |
       Router doméstico
          192.168.1.1
                 |
        Red física / vmbr0
           /          \
          /            \
 Proxmox host       OPNsense WAN
 192.168.1.50        192.168.1.40
                         |
                    OPNsense LAN
                     10.10.10.1
                         |
                  Red aislada / vmbr1
                     /         \
                    /           \
                Servidor       Cliente
              10.10.10.10   10.10.10.100
```

> Las direcciones de OPNsense, servidor y cliente son el direccionamiento
> previsto. Se actualizarán los estados y el diagrama conforme se
> verifiquen las configuraciones definitivas.

## Estado y hoja de ruta

- [x] Instalar Proxmox VE.
- [x] Configurar la IP estática del host.
- [x] Crear el bridge interno `vmbr1` para la futura red aislada.
- [ ] Completar y documentar la VM de OPNsense (WAN, LAN, routing y firewall).
- [ ] Desplegar y documentar máquinas virtuales y contenedores LXC con Debian.
- [ ] Preparar plantillas y aprovisionamiento con cloud-init (usuario, clave SSH e IP).
- [ ] Configurar Windows Server, Active Directory y directivas GPO.
- [ ] Implementar y probar VLAN, DHCP y DNS.

## Documentación

Las guías se publicarán por bloques, incluyendo los comandos relevantes,
las decisiones de configuración y las comprobaciones realizadas.

| Documento | Contenido | Estado |
|---|---|---|
| `01-instalacion-proxmox.md` | Instalación inicial y configuración del host | Pendiente |
| `02-red-y-bridges.md` | Interfaces de red, `vmbr0` y `vmbr1` | Pendiente |
| `03-opnsense.md` | Despliegue del firewall, interfaces y reglas | Pendiente |
| `04-debian-lxc-cloud-init.md` | Máquinas Debian, contenedores y cloud-init | Pendiente |
| `05-windows-server-ad-gpo.md` | Windows Server, Active Directory y GPO | Pendiente |
| `06-vlan-dhcp-dns.md` | Segmentación de red y servicios de infraestructura | Pendiente |

## Estructura del repositorio

```text
.
├── README.md
└── configs/
    └── interfaces    # /etc/network/interfaces del host Proxmox
```

## Aprendizajes y resolución de incidencias

Este apartado recogerá incidencias reales encontradas durante el montaje
y la administración del laboratorio, junto con el diagnóstico, la
solución aplicada y las pruebas realizadas para verificarla.

La intención es documentar no solo el resultado final, sino también el
proceso de resolución de problemas.

## Autor

Estudiante de 2.º de Administración de Sistemas Informáticos en Red
(ASIR) en La Salle Gràcia (Barcelona).

Interés profesional en administración de sistemas, redes,
infraestructura y ciberseguridad.

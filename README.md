# Homelab Proxmox

Laboratorio personal de virtualización, redes y administración de
sistemas, desarrollado como complemento práctico a mis estudios de
**Administración de Sistemas Informáticos en Red (ASIR)**.

El laboratorio está montado sobre un portátil reutilizado y dedicado a
esta función. Utiliza **Proxmox VE** como hipervisor y está diseñado
para practicar con máquinas virtuales, contenedores Linux, redes
segmentadas y servicios de infraestructura.

> **Estado del proyecto:** el entorno físico y la instalación base de
> Proxmox ya están preparados. Estoy organizando y publicando la
> documentación técnica en GitHub *a posteriori*, a partir de la
> configuración y las pruebas realizadas durante el montaje. Por tanto,
> que una guía todavía no esté publicada no significa necesariamente que
> esa parte del laboratorio no se haya trabajado. En cada apartado
> indicaré su estado real para distinguir lo implementado de lo que
> sigue pendiente.

## Objetivos

-   Practicar virtualización y administración de sistemas Linux y
    Windows.
-   Diseñar y configurar redes virtuales aisladas.
-   Trabajar con routing, firewall, NAT, DHCP y DNS.
-   Desplegar máquinas virtuales y contenedores LXC según las
    necesidades de aislamiento y recursos.
-   Automatizar tareas de aprovisionamiento cuando sea posible.
-   Documentar la configuración, las pruebas, los problemas encontrados
    y sus soluciones para que el entorno pueda reproducirse.

## Hardware

  Componente       Detalle
  ---------------- ----------------------------------
  Equipo           Portátil dedicado al laboratorio
  CPU              Intel Core i7 (12 hilos)
  RAM              32 GB DDR4
  Almacenamiento   Aproximadamente 1 TB
  Hipervisor       Proxmox VE 9.2

## Infraestructura y red

La red está planteada en dos segmentos:

-   **Red física (vmbr0):** conecta el host Proxmox y la interfaz WAN
    del firewall con la red doméstica.
-   **Red virtual aislada (vmbr1):** bridge interno sin conexión física
    directa, destinado a las máquinas del laboratorio que estarán detrás
    del firewall.

### Configuración del host y direccionamiento previsto

  -----------------------------------------------------------------------
  Elemento                Dirección               Estado / función
  ----------------------- ----------------------- -----------------------
  Router doméstico        `192.168.1.1/24`        Gateway de la red
                                                  física

  Host Proxmox            `192.168.1.50/24`       IP estática en `vmbr0`
  (`homelab`)                                     

  OPNsense WAN            `192.168.1.40/24`       Interfaz hacia la red
                                                  física

  OPNsense LAN            `10.10.10.1/24`         Gateway previsto de la
                                                  red aislada

  Servidor de laboratorio `10.10.10.10/24`        Dirección prevista

  Cliente de laboratorio  `10.10.10.100/24`       Dirección prevista
  -----------------------------------------------------------------------

`vmbr0` es el bridge conectado a la interfaz física `nic0`. `vmbr1` es
un bridge interno, sin dirección IP ni puertos físicos asociados,
destinado a la red aislada.

### Topología objetivo

``` text
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

> Las direcciones de OPNsense, servidor y cliente representan el
> direccionamiento previsto para la topología. Se actualizarán los
> estados y el diagrama conforme se documenten y verifiquen las
> configuraciones definitivas.

## Estado y hoja de ruta

La siguiente lista refleja el seguimiento del proyecto. La documentación
se está completando progresivamente y se actualizará cuando cada
configuración esté comprobada.

-   [x] Instalar Proxmox VE.
-   [x] Configurar la IP estática del host.
-   [x] Crear el bridge interno `vmbr1` para la futura red aislada.
- [ ] Completar y documentar la VM de OPNsense (WAN, LAN, routing y
    firewall).
- [ ] Desplegar y documentar máquinas virtuales y contenedores LXC con
    Debian.
-   [ ] Preparar plantillas y aprovisionamiento con cloud-init (usuario,
    clave SSH e IP).
-   [ ] Configurar Windows Server, Active Directory y directivas GPO.
-   [ ] Implementar y probar VLAN, DHCP y DNS.

## Documentación

Estoy trasladando a este repositorio la documentación del laboratorio.
Las guías se publicarán por bloques, incluyendo los comandos relevantes,
las decisiones de configuración y las comprobaciones realizadas.

  -----------------------------------------------------------------------
  Documento                           Contenido
  ----------------------------------- -----------------------------------
  `01-instalacion-proxmox.md`         Instalación inicial y configuración
                                      del host

  `02-red-y-bridges.md`               Interfaces de red, `vmbr0` y
                                      `vmbr1`

  `03-opnsense.md`                    Despliegue del firewall, interfaces
                                      y reglas

  `04-debian-lxc-cloud-init.md`       Máquinas Debian, contenedores y
                                      cloud-init

  `05-windows-server-ad-gpo.md`       Windows Server, Active Directory y
                                      GPO

  `06-vlan-dhcp-dns.md`               Segmentación de red y servicios de
                                      infraestructura
  -----------------------------------------------------------------------

## Estructura del repositorio

``` text
.
├── README.md
├── docs/
│   ├── 01-instalacion-proxmox.md
│   ├── 02-red-y-bridges.md
│   ├── 03-opnsense.md
│   ├── 04-debian-lxc-cloud-init.md
│   ├── 05-windows-server-ad-gpo.md
│   └── 06-vlan-dhcp-dns.md
├── diagrams/
│   └── network-topology.png
├── screenshots/
└── configs/
    └── interfaces
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

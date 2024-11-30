# NPF: filtro de paquetes con estado que soporta NAT, conjuntos IP, etc.

[![Estado de la Construcción](https://travis-ci.com/rmind/npf.svg?branch=master)](https://travis-ci.com/rmind/npf)

NPF es un filtro de paquetes de capa 3 que soporta inspección de paquetes con estado,
IPv6, NAT, conjuntos IP, extensiones y mucho más.
Utiliza [BPF](https://en.wikipedia.org/wiki/Berkeley_Packet_Filter) como su
motor principal y fue diseñado con un enfoque en alto rendimiento, escalabilidad,
multi-threading y modularidad. NPF fue escrito desde cero en 2009. Está
escrito en C99 y distribuido bajo la licencia BSD de 2 cláusulas.

NPF se proporciona como una **biblioteca de espacio de usuario** para ser utilizada en una aplicación personalizada
para procesar paquetes. Puede ejecutarse en Linux, típicamente, en combinación con
frameworks como [Data Plane Development Kit (DPDK)](https://www.dpdk.org/)
o [netmap](https://www.freebsd.org/cgi/man.cgi?query=netmap&sektion=4).

## Características

NPF ofrece el conjunto tradicional de características proporcionadas por los filtros de paquetes.
Algunas características clave son:
- Inspección con estado (seguimiento de conexiones).
  - Incluye el [seguimiento completo del estado TCP](https://www.usenix.org/events/sec01/invitedtalks/rooij.pdf).
- Traducción de direcciones de red (NAT):
  - Traducción estática (sin estado) y dinámica (con estado).
  - NAPT y otras formas de traducción de puertos (por ejemplo, reenvío de puertos).
  - NAT de entrada y salida, así como NAT bidireccional.
  - Traducción de red a red, incluyendo NETMAP y NPTv6.
- Capacidad de NAT de grado operador (CG-NAT): conocido por servir más de un millón de conexiones.
- Tablas para conjuntos IP eficientes, incluyendo soporte para _coincidencia de prefijo más largo_.
- Puertas de enlace de nivel de aplicación (por ejemplo, para soportar traceroute).
- NPF usa [BPF con compilación just-in-time (JIT)](https://github.com/rmind/bpfjit).
- Procedimientos de reglas y un marco para extensiones NPF (plugins).
- Las extensiones incluyen:
  - Limitación de velocidad (control de tráfico).
  - Normalización de tráfico.
  - Registro de paquetes.
- Integración con [Data Plane Development Kit](https://dpdk.org/).

Para un conjunto completo de características y su descripción, consulte la documentación de NPF
y otras páginas del manual.

## Uso

Puede probar **[NPF-Router](app)** como una aplicación demo de NPF+DPDK, ejecutando una
red de prueba virtual con contenedores Docker.

## Documentación

Ver en [Github Pages](http://rmind.github.io/npf).
Fuente en el directorio [docs](docs).

## Dependencias

- [libnv](https://github.com/rmind/nvlist): `git clone https://github.com/rmind/nvlist`
- [thmap](https://github.com/rmind/thmap): `git clone https://github.com/rmind/thmap`
- [libqsbr](https://github.com/rmind/libqsbr): `git clone https://github.com/rmind/libqsbr`
- [liblpm](https://github.com/rmind/liblpm): `git clone https://github.com/rmind/liblpm`
- [bpfjit](https://github.com/rmind/bpfjit): `git clone https://github.com/rmind/bpfjit`
- [libcdb](https://github.com/rmind/libcdb): `git clone https://github.com/rmind/libcdb`

Cada repositorio proporciona los archivos de construcción para paquetes RPM (`cd pkg && make rpm`)
y DEB (`cd pkg && make deb`). También puede consultar el
archivo [Travis](.travis.yml) para ver un ejemplo de cómo construir todo.

## Estructura del código fuente

    app/                - Aplicación demo NPF-Router (NPF + DPDK + Docker)
    docs/               - fuente de documentación
    src/                - directorio raíz del código fuente
        kern/           - el componente del kernel (biblioteca npfkern)
        lib/            - bibliotecas de extensión que complementan npfctl
        libnpf/         - biblioteca para gestionar la configuración NPF
        npfctl/         - interfaz de línea de comandos para controlar NPF
        npftest/        - pruebas unitarias y herramienta para depurar NPF
    pkg/                - archivos de empaquetado (RPM y DEB)
    misc/               - scripts auxiliares

## Paquetes

Para construir los paquetes de la biblioteca libnpf (enlazar usando las banderas `-lnpf` y `-lnpfkern`):
* RPM (probado en RHEL/CentOS 7): `cd pkg && make rpm`
* DEB (probado en Debian 9): `cd pkg && make deb`

## Quién está usando NPF?

<table>
  <tr height="150">
    <th width="150"><a href="https://en.outscale.com"><img src="https://fr.outscale.com/wp-content/uploads/2018/07/Logo_Outscale_Bleu_RGB.png" alt="Outscale" align="middle"></a></th>
    <th width="150"><a href="https://innofield.com"><img src="https://innofield.com/wp-content/uploads/2014/07/innofield_logo_sticky.gif" alt="innofield AG" align="middle" width="80%"></a></th>
    <th width="150"><a href="https://www.netbsd.org"><img src="https://www.netbsd.org/images/NetBSD.png" alt="NetBSD" align="middle" width="70%"></a></th>
    <th width="150"><a href="https://bisonrouter.com"><img src="https://bisonrouter.com/img/logo.d1687d60.svg" alt="BisonRouter" align="middle"></a></th>
  </tr>
</table>

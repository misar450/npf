Aquí tienes la traducción revisada, considerando cada término técnico cuidadosamente y sin traducir aquellos que no corresponden:

---

# NPF: filtro de paquetes stateful con soporte para NAT, conjuntos de IP, etc.

[![Estado de compilación](https://travis-ci.com/rmind/npf.svg?branch=master)](https://travis-ci.com/rmind/npf)

NPF es un filtro de paquetes de nivel 3 (*layer 3 packet filter*) que soporta inspección de paquetes *stateful*, IPv6, NAT, conjuntos de IP (*IP sets*), extensiones y más.  
Utiliza [BPF (Berkeley Packet Filter)](https://en.wikipedia.org/wiki/Berkeley_Packet_Filter) como su motor central y fue diseñado con enfoque en alto rendimiento, escalabilidad, procesamiento multihilo (*multi-threading*) y modularidad.  
NPF fue desarrollado desde cero en 2009, está escrito en C99 y se distribuye bajo la licencia BSD de 2 cláusulas.

NPF se proporciona como una **biblioteca en espacio de usuario** (*userspace library*) diseñada para ser utilizada en aplicaciones personalizadas de procesamiento de paquetes.  
Puede ejecutarse en Linux, normalmente junto con frameworks como [DPDK (Data Plane Development Kit)](https://www.dpdk.org/) o [netmap](https://www.freebsd.org/cgi/man.cgi?query=netmap&sektion=4).

---

## Características

NPF ofrece una gama completa de funciones tradicionales proporcionadas por filtros de paquetes. Algunas características clave son:

- **Inspección stateful** (seguimiento de conexiones):
  - Incluye el [seguimiento completo del estado TCP](https://www.usenix.org/events/sec01/invitedtalks/rooij.pdf).
- **Network Address Translation (NAT)**:
  - Traducción estática (*stateless*) y dinámica (*stateful*).
  - NAPT y otras formas de traducción de puertos (e.g., redirección de puertos).
  - NAT de entrada (*inbound*), salida (*outbound*) y bidireccional.
  - Traducción red-a-red, incluyendo NETMAP y NPTv6.
- **Carrier-grade NAT (CG-NAT)**: capacidad para manejar más de un millón de conexiones simultáneas.
- **Tablas para conjuntos de IP (IP sets)** con soporte para _longest prefix match_.
- **Gateways a nivel de aplicación (Application Level Gateways)**, como soporte para traceroute.
- Uso de [BPF con compilación just-in-time (JIT)](https://github.com/rmind/bpfjit).
- Procedimientos de reglas y un marco para extensiones de NPF (*plugins*).
- Extensiones disponibles:
  - Limitación de velocidad (*rate limiting*).
  - Normalización del tráfico.
  - Registro de paquetes.
- Integración con [DPDK](https://dpdk.org/).

Para un listado completo de características, consulta la documentación de NPF.

---

## Uso

Puedes probar **[NPF-Router](app)** como una aplicación de demostración que combina NPF con DPDK, ejecutando una red virtual de prueba con contenedores Docker.

---

## Documentación

Disponible en [Github Pages](http://rmind.github.io/npf).  
El código fuente de la documentación está en el directorio [docs](docs).

---

## Dependencias

Clona los siguientes repositorios para obtener las bibliotecas necesarias:

- [libnv](https://github.com/rmind/nvlist): `git clone https://github.com/rmind/nvlist`
- [thmap](https://github.com/rmind/thmap): `git clone https://github.com/rmind/thmap`
- [libqsbr](https://github.com/rmind/libqsbr): `git clone https://github.com/rmind/libqsbr`
- [liblpm](https://github.com/rmind/liblpm): `git clone https://github.com/rmind/liblpm`
- [bpfjit](https://github.com/rmind/bpfjit): `git clone https://github.com/rmind/bpfjit`
- [libcdb](https://github.com/rmind/libcdb): `git clone https://github.com/rmind/libcdb`

Cada repositorio incluye archivos para construir paquetes RPM (`cd pkg && make rpm`) y DEB (`cd pkg && make deb`).  
Consulta el archivo [Travis](.travis.yml) para ejemplos de construcción.

---

## Estructura del código fuente

    app/                - Aplicación de demostración NPF-Router (NPF + DPDK + Docker).
    docs/               - Documentación.
    src/                - Directorio raíz del código fuente.
        kern/           - Componente del kernel (biblioteca npfkern).
        lib/            - Bibliotecas complementarias para npfctl.
        libnpf/         - Biblioteca para gestionar configuraciones de NPF.
        npfctl/         - Interfaz de línea de comandos para controlar NPF.
        npftest/        - Pruebas unitarias y herramienta de depuración.
    pkg/                - Archivos de empaquetado (RPM y DEB).
    misc/               - Scripts auxiliares.

---

## Construcción de paquetes

Para construir la biblioteca libnpf (enlace con las banderas `-lnpf` y `-lnpfkern`):  

- **RPM** (probado en RHEL/CentOS 7): `cd pkg && make rpm`.  
- **DEB** (probado en Debian 9): `cd pkg && make deb`.  

---

## ¿Quién usa NPF?

<table>
  <tr height="150">
    <th width="150"><a href="https://en.outscale.com"><img src="https://fr.outscale.com/wp-content/uploads/2018/07/Logo_Outscale_Bleu_RGB.png" alt="Outscale" align="middle"></a></th>
    <th width="150"><a href="https://innofield.com"><img src="https://innofield.com/wp-content/uploads/2014/07/innofield_logo_sticky.gif" alt="innofield AG" align="middle" width="80%"></a></th>
    <th width="150"><a href="https://www.netbsd.org"><img src="https://www.netbsd.org/images/NetBSD.png" alt="NetBSD" align="middle" width="70%"></a></th>
    <th width="150"><a href="https://bisonrouter.com"><img src="https://bisonrouter.com/img/logo.d1687d60.svg" alt="BisonRouter" align="middle"></a></th>
  </tr>
</table>

---

¿Ahora es más adecuado?

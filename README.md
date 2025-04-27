# **Tp3 Modo Protegido**

## **UEFI**

### **¿Que es UEFI y como se usa?**

La interfaz de firmware extensible unificada​ o UEFI (del inglés unified extensible firmware interface) es una especificación que define una interfaz entre el sistema operativo y el firmware. UEFI reemplaza la antigua interfaz del sistema básico de entrada y salida (BIOS) estándar presentado en los PC IBM como ROM BIOS de PC IBM.
  
Se usa principalmente interactuando con su menú de configuración durante el arranque del sistema (generalmente presionando teclas como F2, F10, F12, SUPR o ESC, dependiendo del fabricante). Estando ahí, se pueden modificar opciones como el orden de arranque, configuraciones de seguridad, habilitar/deshabilitar componentes de hardware, actualizar el propio firmware UEFI, y configurar parámetros de rendimiento o perfiles de memoria RAM.

### **Casos de BUGS de UEFI**

Investigadores de ESET han descubierto una vulnerabilidad que permite eludir el UEFI Secure Boot y que afecta a la mayoría de los sistemas basados en UEFI. Esta vulnerabilidad, asignada CVE-2024-7344, se encontró en una aplicación UEFI firmada por el certificado UEFI de terceros de Microsoft Microsoft Corporation UEFI CA 2011. La explotación de esta vulnerabilidad conduce a la ejecución de código no fiable durante el arranque del sistema, lo que permite a los atacantes potenciales desplegar fácilmente bootkits UEFI maliciosos (como Bootkitty o BlackLotus) incluso en sistemas con UEFI Secure Boot habilitado, independientemente del sistema operativo instalado.

La aplicación UEFI afectada forma parte de varias suites de software de recuperación de sistemas en tiempo real desarrolladas por Howyar Technologies Inc., Greenware Technologies, Radix Technologies Ltd., SANFONG Inc., Wasay Software Technology Inc., Computer Education System Inc. y Signal Computer GmbH. 

La vulnerabilidad está causada por el uso de un PE loader personalizado en lugar de utilizar las funciones estándar y seguras de UEFI LoadImage y StartImage. Como resultado, la aplicación permite la carga de cualquier binario UEFI - incluso uno sin firmar - desde un archivo especialmente diseñado llamado cloak.dat, durante el arranque del sistema, independientemente del estado de arranque seguro UEFI.

### **CSME y Intel MEBx**

**Converged Security and Management Engine (CSME)** es un subsistema crítico integrado en los chipsets de Intel que proporciona capacidades de seguridad y gestión. Este componente es el núcleo que impulsa la tecnología Intel® Active Management Technology (Intel® AMT), una funcionalidad que permite la gestión remota de sistemas incluso cuando están apagados o el sistema operativo no está operativo. El CSME opera de manera independiente del procesador principal, utilizando su propio firmware y memoria, lo que lo convierte en una parte esencial de la arquitectura de la plataforma Intel vPro®. Entre sus funciones clave se encuentra garantizar el acceso remoto seguro y las capacidades de gestión, como la comunicación fuera de banda (out-of-band), que son fundamentales para que los administradores de TI puedan mantener y solucionar problemas en los sistemas de forma remota.

**Intel Management Engine BIOS Extension (MEBx)** es una extensión de la BIOS que permite a los usuarios configurar el Management Engine, incluyendo los ajustes relacionados con Intel® AMT. Se accede típicamente durante el proceso de arranque presionando una combinación de teclas específica, como Ctrl+P, lo que abre una interfaz donde se pueden establecer configuraciones de red, parámetros de seguridad (como la contraseña del CSME) y habilitar funciones de AMT. El MEBx es un componente estándar en sistemas con capacidades de Intel AMT y está implícito en las referencias a la configuración manual y el acceso al CSME durante el arranque.

## **Coreboot**

### **¿Que es Coreboot?**

**Coreboot**, anteriormente conocido como **LinuxBIOS**, es un proyecto iniciado en 1999 en el Advanced Computing Laboratory en Los Alamos National Laboratory, con el objetivo de crear un firmware que arranque rápidamente y maneje errores de manera inteligente. Su diseño filosófico se centra en realizar solo las tareas mínimas necesarias para inicializar el hardware, como configurar el controlador de memoria, la CPU y los periféricos, y luego transferir el control a un "payload", que puede ser un cargador de arranque como SeaBIOS (para servicios PCBIOS), edk2 (para UEFI), GRUB2 (usado por muchas distribuciones Linux) o depthcharge (común en Chromebooks).

Esta separación de responsabilidades permite maximizar la reutilización de las rutinas de inicialización de hardware, adaptándose a una amplia gama de casos de uso, desde interfaces estándar hasta flujos de arranque completamente personalizados. Coreboot está licenciado bajo la **GNU** General Public License versión 2 (GPLv2), lo que garantiza su naturaleza de software libre y fomenta la colaboración comunitaria. Es compatible con múltiples arquitecturas de CPU, incluyendo IA-32, x86-64, ARM, ARM64, MIPS y RISC-V, y soporta plataformas específicas como AMD Geode, Intel Atom y Ryzen Embedded, entre otras.

### **¿Que productos lo incorporan?**

Coreboot se utiliza en una variedad de dispositivos, especialmente en notebooks y PCs diseñadas para usuarios que priorizan la libertad de software y la seguridad. A continuación, se detalla una lista de productos y fabricantes que lo incorporan, basada en información de sitios oficiales y foros especializados:

|  **Fabricante** |**Productos**   | **Informacion** |
|---|---|---|
|  Google |  Chromebooks (todos menos los tres primeros modelos) |  Mayor despliegue de coreboot, usado en dispositivos Chrome OS. | 
| Purism  | Notebooks Librem (como Librem 13 v2, Librem 15 v3)  |  Enfocados en privacidad, con coreboot preinstalado y soporte para PureBoot. |  
|  Minifree Ltd | Notebooks y desktops con Libreboot  | Ofrecen Libreboot preinstalado, junto con Debian Linux u otros BSD  | 
|  Lenovo Thinkpad | Modelos como T480, X220, X230  | Compatible con coreboot, especialmente en distribuciones como Libreboot y Skulls.  | 
|  StarLabs Systems |  Notebooks |  Ofrecen por defecto la alternativa de coreboot | 

### **Ventajas de la utilización de coreboot**

El uso de coreboot ofrece múltiples beneficios, especialmente para usuarios técnicos, entusiastas de software libre y organizaciones que buscan mayor control y seguridad en sus sistemas. A continuación, se enumeran las ventajas:
- **Rendimiento y eficiencia:** Coreboot está diseñado para arrancar rápidamente, realizando solo las tareas esenciales de inicialización. Por ejemplo, la versión x86 ejecuta solo diez instrucciones antes de pasar al modo de 32 bits, similar a los sistemas UEFI modernos, lo que reduce significativamente los tiempos de arranque.

- **Seguridad y transparencia:** Escrito principalmente en C, con menos del 1% en ensamblador, coreboot es más fácil de auditar que los BIOS tradicionales basados en ensamblador, lo que mejora la seguridad al reducir la superficie de ataque. Además, al ser de código abierto permite a los usuarios y desarrolladores revisar, modificar y mejorar el firmware, aumentando la transparencia. Esto es especialmente valioso para detectar vulnerabilidades y garantizar la integridad del sistema.

- **Flexibilidad y personalización:** Permite el uso de diferentes "payloads", como SeaBIOS para arranque legacy, iPXE para arranque por red, o edk2 para entornos UEFI, lo que ofrece flexibilidad para adaptarse a diversas necesidades. Los usuarios pueden personalizar aspectos como pantallas de arranque (en formato JPG), consolas de depuración accesibles vía puertos seriales, USB, SPI o incluso el altavoz de la PC, y recuperar registros de arranque una vez que el sistema operativo esté en funcionamiento.

- **Reutilización y mantenimiento:** Maximiza la reutilización de rutinas de inicialización de hardware a través de la separación de preocupaciones entre coreboot y el payload, lo que facilita su uso en diferentes escenarios, desde dispositivos estándar hasta aplicaciones especializadas. Evita la fragmentación manteniendo todo en un solo árbol de código fuente



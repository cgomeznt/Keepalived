## Balanceador de Carga con Keepalived

[Keepalived](http://http://www.keepalived.org// "http://www.http://www.keepalived.org/") es un software de enrutamiento escrito en C. El objetivo principal de este proyecto es proporcionar instalaciones simples y robustas para el balanceo de carga, alta disponibilidad para los sistemas Linux y utilizar infraestructuras basadas en Linux. El framework de loadbalancing se basa en la bien conocida y ampliamente utilizado Linux Virtual Server (IPVS), módulo del kernel que proporciona Layer4 loadbalancing. Keepalived implementa un conjunto de chequeos para mantener y gestionar un grupo de servidores en equilibrio de carga de acuerdo a su salud de forma dinámica y adaptativa. Por otro lado la alta disponibilidad se consigue por el protocolo VRRP. VRRP es fundamental para la conmutación por error del router. Además, [Keepalived](http://http://www.keepalived.org// "http://www.http://www.keepalived.org/") implementa un conjunto de procedimientos VRRP, proporcionado a bajo nivel y las interacciones de protocolo de alta velocidad. El framework Keepalived se pueden utilizar independientemente o todos juntos para proporcionar infraestructuras robustas.

[Keepalived](http://http://www.keepalived.org// "http://www.http://www.keepalived.org/") es software libre; usted puede redistribuirlo y / o modificarlo bajo los términos de la Licencia Pública General de GNU publicada por la Fundación para el Software Libre; ya sea la versión 2 de la Licencia, o (a su elección) cualquier versión posterior.

En estos links se pueden encontrar cada uno de los parametros para el archivo de configuración
[Keepalived](http://http://www.keepalived.org// "http://www.http://www.keepalived.org/")

Aqui vamos hacer dos laboratorios:
- [Failover con Keepalived (Muy utilizado, unicamente como failover)](guia/failover.rst)
- [Balanceador de Carga con Keepalived](guia/balance.rst)


Más información, consulte [Keepalived](http://http://www.keepalived.org// "http://www.http://www.keepalived.org/"), la documentación /usr/share/doc/keepalived/ y el man de  keepalived (8) y keepalived.conf (5).

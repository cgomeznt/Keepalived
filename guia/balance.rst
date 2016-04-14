Balanceador de Carga con Keepalived
====================================

`Keepalived <http://www.keepalived.org/>`_ utiliza el módulo del núcleo IP del servidor virtual (IPVS) para proporcionar equilibrio de carga de la capa de transporte (capa 4), desviando las peticiones de servicios basados ​​en la red a los miembros individuales de un clúster de servidores. IPVS supervisa el estado de cada servidor y utiliza el Virtual Router Redundancy Protocol (VRRP) para implementar la alta disponibilidad.


**Dos formas de hacer balanceador de carga con Keepalive**

* `Balanceador de Carga en modo NAT. <nat.rst>`_

* `Balanceador de Carga en modo DR. <dr.rst>`_


Más información, consulte `Keepalived <http://www.keepalived.org/>`_, la documentación /usr/share/doc/keepalived/ y el man de  keepalived (8) y keepalived.conf (5).

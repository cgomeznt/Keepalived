Balanceador de Carga en modo NAT
=================================

En el siguiente ejemplo se utiliza Keepalived en modo NAT para implementar un **failover** y **balanceo de carga** en dos servidores. Un servidor actúa como MASTER, el otro actúa como BACKUP, el servidor MASTER tiene una prioridad más alta que el BACKUP. Cada uno de los servidores del balanceador con Keepalived tiene dos interfaces de red, donde una de la interfaz (eth0) está conectada hacia una red externa (192.168.1.0/24) y la otra interfaz (eth1) está conectado a una red interna (10.0.0.0/24) en donde se encuentran los tres servidores web.

Si se fijan en `Failover con Keepalived (Muy utilizado, unicamente como failover) <failover.rst>`_ es igual a lo que implementamos aqui, pero en este caso solo le vamos asociar el **virtual server** a la IP Virtual del vrrp externo, para que ejecute el balanceo de carga.

.. figure:: ../images/nat.png

Aqui se omiten pasos que estan publicados en `Failover con Keepalived (Muy utilizado, unicamente como failover) <failover.rst>`_ debe enterder que primero configuramos la capa del failover y luego la de balanceo.

Como estará configurado el laboratorio
+++++++++++++++++++++++++++++++++++++++

- 192.168.1.20 -- IPV - IP virtual externa.
- 10.0.0.20 -- IPV - IP virtual interna.
- eth0-192.168.1.21 / eth1-10.0.0.21 -- Ubuntu 14.04.4, MASTER -- Keepalived
- eth0-192.168.1.22 / eth1-10.0.0.22 -- Ubuntu 14.04.4, BACKUP -- Keepalived
- 10.0.0.50 -- Ubuntu 14.04.4, server1 -- apache2
- 10.0.0.51 -- Ubuntu 14.04.4, server2 -- apache2
- 10.0.0.52 -- Ubuntu 14.04.4, server3 -- apache2

Configuración de los servidores con keepalived
++++++++++++++++++++++++++++++++++++++++++++++

La siguiente configuración en ``/etc/keepalived/keepalived.conf`` en el servidor MASTER::

	global_defs {
	   notification_email {
		 cgomez.stack@gmail.com
	   }
	   notification_email_from master@laboratorio.com
	   smtp_server mysmtp.laboratorio.com
	   smtp_connect_timeout 30
	}

	vrrp_sync_group VRRP1 {
	#   Groupo de VRRP instancias para hacer siempre el failover juntos
		group {
		    externa
		    interna
		    }
	}

	vrrp_instance externa {
		state MASTER
		interface eth0
		virtual_router_id 21
		priority 150
		advert_int 1
		authentication {
		    auth_type PASS
		    auth_pass Venezuela21
		}
	#   La virtual IP para la vrrp externa
		virtual_ipaddress {
		    192.168.1.20/24
		}
	}

	vrrp_instance interna {
		state MASTER
		interface eth1
		virtual_router_id 22
		priority 150
		advert_int 1
		authentication {
		    auth_type PASS
		    auth_pass Venezuela21
		}
	#   La virtual IP para la vrrp interna
		virtual_ipaddress {
		    10.0.0.20/24
		}
	}

	# Definimos un virtual server HTTP, sobre la virtual IP 192.168.1.20
	virtual_server 192.168.1.20 80 {
		delay_loop 10
		protocol TCP
	#   Usa round-robin para balanceo
		lb_algo rr
	#   Usa NAT para ocultar los servidores backend, los web servers
		lb_kind NAT
	#   Aplica time out a sesiones persistentes despues de 2 horas.
		persistence_timeout 7200

		real_server 10.0.0.50 80 {
		    weight 1
		    TCP_CHECK {
		      connect_timeout 5
		      connect_port 80
		    }
		}

		real_server 10.0.0.51 80 {
		    weight 1
		    TCP_CHECK {
		      connect_timeout 5
		      connect_port 80
		    }
		}
		real_server 10.0.0.52 80 {
		    weight 1
		    TCP_CHECK {
		      connect_timeout 5
		      connect_port 80
		    }
		}
	}

La siguiente configuración en ``/etc/keepalived/keepalived.conf`` en el servidor BACKUP, es igual que la del MASTER, solo cambia (notification_email_from, state, priority. ::

	global_defs {
	   notification_email {
		 cgomez.stack@gmail.com
	   }
	   notification_email_from backup@laboratorio.com
	   smtp_server mysmtp.laboratorio.com
	   smtp_connect_timeout 30
	}

	vrrp_sync_group VRRP1 {
	#   Groupo de VRRP instancias para hacer siempre el failover juntos, esto en el Keepalived con NAT no lo utilizamos
		group {
		    externa
		    interna
		    }
	}

	vrrp_instance externa {
		state BACKUP
		interface eth0
		virtual_router_id 21
		priority 100
		advert_int 1
		authentication {
		    auth_type PASS
		    auth_pass Venezuela21
		}
	#   La virtual IP para la vrrp externa
		virtual_ipaddress {
		    192.168.1.20/24
		}
	}

	vrrp_instance interna {
		state BACKUP
		interface eth1
		virtual_router_id 22
		priority 100
		advert_int 1
		authentication {
		    auth_type PASS
		    auth_pass Venezuela21
		}
	#   La virtual IP para la vrrp interna
		virtual_ipaddress {
		    10.0.0.20/24
		}
	}

	# Definimos un virtual server HTTP, sobre la virtual IP 192.168.1.20
	virtual_server 192.168.1.20 80 {
		delay_loop 10
		protocol TCP
	#   Usa round-robin para balanceo
		lb_algo rr
	#   Usa NAT para ocultar los servidores backend, los web servers
		lb_kind NAT
	#   Aplica time out a sesiones persistentes despues de 2 horas.
		persistence_timeout 7200

		real_server 10.0.0.50 80 {
		    weight 1
		    TCP_CHECK {
		      connect_timeout 5
		      connect_port 80
		    }
		}

		real_server 10.0.0.51 80 {
		    weight 1
		    TCP_CHECK {
		      connect_timeout 5
		      connect_port 80
		    }
		}
		real_server 10.0.0.52 80 {
		    weight 1
		    TCP_CHECK {
		      connect_timeout 5
		      connect_port 80
		    }
		}
	}


Más información, consulte `Keepalived <http://www.keepalived.org/>`_, la documentación /usr/share/doc/keepalived/ y el man de  keepalived (8) y keepalived.conf (5).

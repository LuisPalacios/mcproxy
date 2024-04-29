<!--vim: set textwidth=80 formatoptions+=t wrapmargin=5 -->

Introduction
============
Mcproxy is an IGMP/MLD Proxy daemon for Linux.

IGMP/MLD proxies offer the possibility option to combine local multicast
networks with a larger multicast infrastructure. In contrast to multicast
routers, proxies are lightweight and do not require the support of a multicast
routing protocol such as PIM or DVMRP. A common use case is a local stub
networks that interconnects with a remote multicast routing domain, e.g. via a
tunnel. But it can also be used in PMIPv6 domain to enable multicast for
sources
([pmipv6-source-draft](http://tools.ietf.org/html/draft-ietf-multimob-pmipv6-source))
and listeners ([RFC 6224](http://tools.ietf.org/html/rfc6224)). The Mcproxy
meets the requirements of the IGMP/MLD proxying standard ([RFC
        4605](http://tools.ietf.org/html/rfc4605)) and has additional
functionalities.  The multicast proxy can be instantiated multiple times, is
dynamically configurable at runtime, supports multiple upstreams and
peering-interfaces for a non hierarchical interconnection of multicast proxies.

If you use Mcproxy in a scientific context, please use the following [citation](http://inet.cpt.haw-hamburg.de/publications/sww-pmpps-14.html).

Requirements
============
*  A g++ version equal or higher **4.8** is required.

*  To generate a Makefile, cmake must be installed. 

        apt install cmake -y

*  To use the IPv6 functionality the kernel has to be configured and compiled
with the experimental kernel feature _IPv6: multicast routing_.  For more
details go to chapter [Startup](#startup).

*  To use more then one proxy instance for IPv4 and IPv6 the kernel has to  be
configured and compiled with the experimental kernel feature _IP: multicast
policy routing_ and _IPv6: multicast policy routing_.  For more details go to
chapter [Startup](#startup).

*  To build the documentation, doxygen must be installed. This can be done with
the following command:

        apt-get install doxygen

*  The Mcproxy has to be started with root privileges.

*  A Linux kernel version greater than version 2.6.32 is required.


Compilation
===========

Clone

```zsh
git clone https://github.com/LuisPalacios/mcproxy.git
```

Build

```zsh
cd mcproxy
cmake .
cmake --build .
```

Installation
============

It will be installed under `/usr/local/bin/mcproxy-bin`

```zsh
% sudo make install
[100%] Built target mcproxy-bin
Install the project...
-- Install configuration: ""
-- Installing: /usr/local/bin/mcproxy-bin
```


Documentation
=============
Mcproxy includes a HTML documentation. The documentation will be located in the
docs/ directory after the execution of:

```zsh
make doc
```

Startup
=======

At first you should check the available kernel features of your system. Type
the following command:

```zsh
% sudo mcproxy-bin -c
```

If a kernel feature you need is missing you have to reconfigure and recompile
your linux kernel. In the debug folder is a [README](debug/README.md#kernel) file
which could help you with this problem.

To run the Mcproxy you need to create a valid configuration file.  There is an
example in the project folder ([mcproxy.conf](mcproxy/mcproxy.conf)).

*  Prepare the configuration file: `/etc/mcproxy.conf`

```conf
######################################
##-- mcproxy configuration script --##
######################################

#protocol MLDv2; #IPv6
protocol IGMPv3; #IPv4

pinstance myProxy: vlan2 ==> vlan100;

#
# This confiugration example creates
# a multicast proxy for ipv4 with the
# upstream vlan2 and one downstream vlan100
#
#              |
#              |
#       +------+-----+
#       |    vlan2   |
#       |            |
#       |   mcProxy  |
#       |            |
#       |   vlan100  |
#       +------+-----+
#              |
#              |
#
```


*  Run the Mcproxy in the background:

```zsh
sudo nohup /usr/local/bin/mcproxy -f /etc/mcproxy.conf &
```

*  Run mcprocy with all available status and debug messages:

```zsh
sudo /usr/local/bin/mcproxy -dsvv -f /etc/mcproxy.conf
```

For more information see `mcproxy-bin -h` or visit our project page.


Contact
=======

Project page: [http://mcproxy.realmv6.org/](http://mcproxy.realmv6.org/)

Mailing list: multicast-proxy@googlegroups.com


Acknowledgement
===============
Álvaro Fernández Rojas, 
Florian Ecard,
Hai Shalom,
Zhi Chen


---
title: DHCPv6 configureren voor virtuele Linux-machines
titlesuffix: Azure Load Balancer
description: Klik hier voor meer informatie over het DHCPv6 configureren voor virtuele Linux-machines.
services: load-balancer
documentationcenter: na
author: KumudD
keywords: IPv6-, azure-load balancer, dual-stack, openbaar IP-adres, systeemeigen IPv6-, mobiele, iot
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2019
ms.author: kumud
ms.openlocfilehash: 66777ec314e95d81a4be57082f06ef16dc170186
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60516556"
---
# <a name="configure-dhcpv6-for-linux-vms"></a>DHCPv6 configureren voor virtuele Linux-machines


Enkele van de Linux-installatiekopieën voor virtuele machines in de Azure Marketplace is geen Dynamic Host Configuration Protocol versie 6 (DHCPv6) standaard geconfigureerd. Ter ondersteuning van IPv6, moet DHCPv6 worden geconfigureerd in de distributie van Linux-besturingssysteem die u gebruikt. Omdat ze verschillende pakketten gebruiken, de verschillende Linux-distributies DHCPv6 configureren op verschillende manieren.

> [!NOTE]
> Recente SUSE Linux- en CoreOS-installatiekopieën in de Azure Marketplace zijn vooraf geconfigureerd met DHCPv6. Er is geen aanvullende wijzigingen zijn vereist wanneer u deze installatiekopieën gebruiken.

Dit document wordt beschreven hoe u DHCPv6 inschakelen, zodat uw virtuele Linux-machine een IPv6-adres verkrijgt.

> [!WARNING]
> Door het onjuist bewerken van netwerk-configuratiebestanden, verliest u toegang tot het netwerk met uw virtuele machine. Het is raadzaam dat u uw wijzigingen in de configuratie op niet-productiesystemen testen. De instructies in dit artikel zijn getest op de nieuwste versies van de Linux-installatiekopieën in de Azure Marketplace. Raadpleeg de documentatie voor uw eigen versie van Linux voor meer gedetailleerde instructies.

## <a name="ubuntu"></a>Ubuntu

1. Bewerk de */etc/dhcp/dhclient6.conf* bestand en voeg de volgende regel toe:

        timeout 10;

2. Wijzig de netwerkconfiguratie voor de eth0-interface met de volgende configuratie:

   * Op **Ubuntu 12.04 en 14.04**, bewerk de */etc/network/interfaces.d/eth0.cfg* bestand. 
   * Op **Ubuntu 16.04**, bewerk de */etc/network/interfaces.d/50-cloud-init.cfg* bestand.

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Vernieuwen van het IPv6-adres:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```
Vanaf Ubuntu 17.10, het standaardmechanisme voor de configuratie van netwerk is [NETPLAN]( https://netplan.io).  Bij installatie/instantiëring NETPLAN netwerkconfiguratie leest uit YAML-configuratiebestanden op deze locatie: / {lib,etc,run}/netplan/*.yaml.

Neem een *dhcp6:true* -instructie voor elke ethernet-interface in uw configuratie.  Bijvoorbeeld:
  
        network:
          version: 2
          ethernets:
            eno1:
              dhcp6: true

Tijdens het opstarten van vroege, de netplan 'netwerk renderer' schrijft configuratie/uitvoert met de hand uit controle over apparaten van de opgegeven VPN-daemon voor naslaginformatie over NETPLAN, Zie https://netplan.io/reference.
 
## <a name="debian"></a>Debian

1. Bewerk de */etc/dhcp/dhclient6.conf* bestand en voeg de volgende regel toe:

        timeout 10;

2. Bewerk de */etc/network/interfaces* bestand en voeg de volgende configuratie toe:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Vernieuwen van het IPv6-adres:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel-centos-and-oracle-linux"></a>RHEL, CentOS en Oracle Linux

1. Bewerk de */etc/sysconfig/network* bestand, en voeg de volgende parameter toe:

        NETWORKING_IPV6=yes

2. Bewerk de */etc/sysconfig/network-scripts/ifcfg-eth0* bestand, en voeg de volgende twee parameters toe:

        IPV6INIT=yes
        DHCPV6C=yes

3. Vernieuwen van het IPv6-adres:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11-and-opensuse-13"></a>SLES 11 en openSUSE 13

Recente SUSE Linux Enterprise Server (SLES) en openSUSE-installatiekopieën in Azure zijn vooraf geconfigureerd met DHCPv6. Er is geen aanvullende wijzigingen zijn vereist wanneer u deze installatiekopieën gebruiken. Hebt u een virtuele machine die gebaseerd op een oudere of aangepaste SUSE-installatiekopie, het volgende doen:

1. Installeer de `dhcp-client` pakket, indien nodig:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Bewerk de */etc/sysconfig/network/ifcfg-eth0* bestand, en voeg de volgende parameter toe:

        DHCLIENT6_MODE='managed'

3. Vernieuwen van het IPv6-adres:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 en openSUSE Leap

Recente installatiekopieën voor SLES en openSUSE in Azure zijn vooraf geconfigureerd met DHCPv6. Er is geen aanvullende wijzigingen zijn vereist wanneer u deze installatiekopieën gebruiken. Hebt u een virtuele machine die gebaseerd op een oudere of aangepaste SUSE-installatiekopie, het volgende doen:

1. Bewerk de */etc/sysconfig/network/ifcfg-eth0* -bestand en vervang de `#BOOTPROTO='dhcp4'` parameter met de volgende waarde:

        BOOTPROTO='dhcp'

2. Aan de */etc/sysconfig/network/ifcfg-eth0* bestand, voeg de volgende parameter toe:

        DHCLIENT6_MODE='managed'

3. Vernieuwen van het IPv6-adres:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Recente CoreOS-installatiekopieën in Azure zijn vooraf geconfigureerd met DHCPv6. Er is geen aanvullende wijzigingen zijn vereist wanneer u deze installatiekopieën gebruiken. Hebt u een virtuele machine op basis van een oudere of aangepaste CoreOS-installatiekopie, het volgende doen:

1. Bewerk de */etc/systemd/network/10_dhcp.network* bestand:

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Vernieuwen van het IPv6-adres:

    ```bash
    sudo systemctl restart systemd-networkd
    ```

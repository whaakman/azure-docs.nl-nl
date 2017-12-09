---
title: DHCPv6 configureren voor virtuele Linux-machines | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van DHCPv6 voor virtuele Linux-machines.
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
keywords: IPv6-, azure load balancer, dual-stack, openbare IP-adres, systeemeigen ipv6, mobiele, iot
ms.assetid: b32719b6-00e8-4cd0-ba7f-e60e8146084b
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: b46c2107dcfda5f02407e08daf08bd42d722dfda
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="configure-dhcpv6-for-linux-vms"></a>DHCPv6 configureren voor virtuele Linux-machines

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Sommige van de installatiekopieën van Linux virtuele machines in Azure Marketplace hebben geen Dynamic Host Configuration Protocol versie 6 (DHCPv6) standaard geconfigureerd. Ter ondersteuning van IPv6 moet DHCPv6 worden geconfigureerd in de distributie van Linux-besturingssysteem die u gebruikt. De verschillende Linux-distributies configureren DHCPv6 in tal van manieren omdat ze verschillende pakketten gebruiken.

> [!NOTE]
> Recente SUSE Linux- en virtuele CoreOS-installatiekopieën in Azure Marketplace zijn vooraf geconfigureerd met DHCPv6. Er zijn geen aanvullende wijzigingen zijn vereist wanneer u deze installatiekopieën gebruiken.

Dit document wordt beschreven hoe DHCPv6 inschakelen zodat uw virtuele Linux-machine verkrijgt een IPv6-adres.

> [!WARNING]
> Door het onjuist bewerken van netwerk-configuratiebestanden, verliest u toegang tot het netwerk met uw virtuele machine. Het is raadzaam om uw configuratiewijzigingen op niet-productieve systemen te testen. De instructies in dit artikel zijn getest op de nieuwste versies van de Linux-installatiekopieën in Azure Marketplace. Raadpleeg de documentatie voor uw eigen versie van Linux voor gedetailleerde instructies.

## <a name="ubuntu"></a>Ubuntu

1. Bewerk de */etc/dhcp/dhclient6.conf* bestand en voeg de volgende regel:

        timeout 10;

2. Bewerk de netwerkconfiguratie voor de interface eth0 met de volgende configuratie:

   * Op **Ubuntu 12.04 en 14.04**, bewerk de */etc/network/interfaces.d/eth0.cfg* bestand. 
   * Op **Ubuntu 16.04**, bewerk de */etc/network/interfaces.d/50-cloud-init.cfg* bestand.

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Het IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Bewerk de */etc/dhcp/dhclient6.conf* bestand en voeg de volgende regel:

        timeout 10;

2. Bewerk de */etc/network/interfaces* bestand en voeg de volgende configuratie:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Het IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel-centos-and-oracle-linux"></a>RHEL, CentOS en Oracle Linux

1. Bewerk de */etc/sysconfig/network* bestand en voeg de volgende parameter:

        NETWORKING_IPV6=yes

2. Bewerk de */etc/sysconfig/network-scripts/ifcfg-eth0* bestand en voeg de volgende twee parameters:

        IPV6INIT=yes
        DHCPV6C=yes

3. Het IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11-and-opensuse-13"></a>SLES 11 en openSUSE 13

Recente SUSE Linux Enterprise Server (SLES) en openSUSE afbeeldingen in Azure is vooraf geconfigureerd met DHCPv6. Er zijn geen aanvullende wijzigingen zijn vereist wanneer u deze installatiekopieën gebruiken. Als u een virtuele machine die gebaseerd op een installatiekopie van een oudere of aangepaste SUSE hebt, het volgende doen:

1. Installeer de `dhcp-client` inpakken, indien nodig:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Bewerk de */etc/sysconfig/network/ifcfg-eth0* bestand en voeg de volgende parameter:

        DHCLIENT6_MODE='managed'

3. Het IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 en openSUSE Leap

Recente SLES en openSUSE afbeeldingen in Azure is vooraf geconfigureerd met DHCPv6. Er zijn geen aanvullende wijzigingen zijn vereist wanneer u deze installatiekopieën gebruiken. Als u een virtuele machine die gebaseerd op een installatiekopie van een oudere of aangepaste SUSE hebt, het volgende doen:

1. Bewerk de */etc/sysconfig/network/ifcfg-eth0* bestand en vervang de `#BOOTPROTO='dhcp4'` parameter met de volgende waarde:

        BOOTPROTO='dhcp'

2. Aan de */etc/sysconfig/network/ifcfg-eth0* bestand, het toevoegen van de volgende parameter:

        DHCLIENT6_MODE='managed'

3. Het IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Recente virtuele CoreOS-afbeeldingen in Azure is vooraf geconfigureerd met DHCPv6. Er zijn geen aanvullende wijzigingen zijn vereist wanneer u deze installatiekopieën gebruiken. Als u een virtuele machine op basis van een virtuele CoreOS oudere of aangepaste installatiekopie hebt, het volgende doen:

1. Bewerk de */etc/systemd/network/10_dhcp.network* bestand:

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Het IPv6-adres verlengen:

    ```bash
    sudo systemctl restart systemd-networkd
    ```

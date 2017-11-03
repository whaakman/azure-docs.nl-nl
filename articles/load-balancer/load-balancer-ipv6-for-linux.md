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
ms.openlocfilehash: 84558cb6e3a5524969f590eb0272a64ad8839ab5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-dhcpv6-for-linux-vms"></a>DHCPv6 configureren voor Linux VM's

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Sommige van de installatiekopieën van het Linux-virtuele machine in Azure Marketplace hebben geen DHCPv6 standaard geconfigureerd. Ter ondersteuning van IPv6 moet DHCPv6 worden geconfigureerd in binnen de distributie van Linux-besturingssysteem die u gebruikt. Verschillende Linux-distributies zijn verschillende manieren van het configureren van DHCPv6 omdat ze verschillende pakketten gebruiken.

> [!NOTE]
> Recente SUSE Linux- en virtuele CoreOS-installatiekopieën in Azure Marketplace zijn vooraf geconfigureerd met DHCPv6. Er zijn geen aanvullende wijzigingen vereist wanneer u deze afbeeldingen.

Dit document wordt beschreven hoe DHCPv6 inschakelen zodat uw virtuele Linux-machine verkrijgt een IPv6-adres.

> [!WARNING]
> Onjuist bewerken van netwerkbestanden configuratie kan leiden tot verliest u toegang tot uw virtuele machine. Het is raadzaam om uw configuratiewijzigingen op niet-productieve systemen te testen. De instructies in dit artikel zijn getest op de nieuwste versies van de Linux-installatiekopieën in Azure Marketplace. Raadpleeg de documentatie voor uw specifieke versie van Linux voor gedetailleerde instructies.

## <a name="ubuntu"></a>Ubuntu

1. Bewerk het bestand `/etc/dhcp/dhclient6.conf` en voeg de volgende regel:

        timeout 10;

2. Bewerk de netwerkconfiguratie voor de interface eth0 met de volgende configuratie:

   * Op **Ubuntu 12.04 en 14.04**, bewerk het bestand`/etc/network/interfaces.d/eth0.cfg`
   * Op **Ubuntu 16.04**, bewerk het bestand`/etc/network/interfaces.d/50-cloud-init.cfg`

         iface eth0 inet6 auto
             up sleep 5
             up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Bewerk het bestand `/etc/dhcp/dhclient6.conf` en voeg de volgende regel:

        timeout 10;

2. Bewerk het bestand `/etc/network/interfaces` en voeg de volgende configuratie:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Bewerk het bestand `/etc/sysconfig/network` en voeg de volgende parameter:

        NETWORKING_IPV6=yes

2. Bewerk het bestand `/etc/sysconfig/network-scripts/ifcfg-eth0` en voeg de volgende twee parameters:

        IPV6INIT=yes
        DHCPV6C=yes

3. IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Recente SLES en openSUSE afbeeldingen in Azure is vooraf geconfigureerd met DHCPv6. Er zijn geen aanvullende wijzigingen vereist wanneer u deze afbeeldingen. Als u een virtuele machine op basis van een installatiekopie van een oudere of aangepaste SUSE hebt, gebruikt u de volgende stappen uit:

1. Installeer de `dhcp-client` inpakken, indien nodig:

    ```bash
    sudo zypper install dhcp-client
    ```

2. Bewerk het bestand `/etc/sysconfig/network/ifcfg-eth0` en voeg de volgende parameter:

        DHCLIENT6_MODE='managed'

3. Het IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 en openSUSE Leap

Recente SLES en openSUSE afbeeldingen in Azure is vooraf geconfigureerd met DHCPv6. Er zijn geen aanvullende wijzigingen vereist wanneer u deze afbeeldingen. Als u een virtuele machine op basis van een installatiekopie van een oudere of aangepaste SUSE hebt, gebruikt u de volgende stappen uit:

1. Bewerk het bestand `/etc/sysconfig/network/ifcfg-eth0` en vervang deze parameter

        #BOOTPROTO='dhcp4'

    met de volgende waarde:

        BOOTPROTO='dhcp'

2. Voeg de volgende parameter `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Het IPv6-adres verlengen:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Recente virtuele CoreOS-afbeeldingen in Azure is vooraf geconfigureerd met DHCPv6. Er zijn geen aanvullende wijzigingen vereist wanneer u deze afbeeldingen. Als u een virtuele machine op basis van een virtuele CoreOS oudere of aangepaste installatiekopie hebt, gebruikt u de volgende stappen uit:

1. Bewerk het bestand`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Het IPv6-adres verlengen:

    ```bash
    sudo systemctl restart systemd-networkd
    ```

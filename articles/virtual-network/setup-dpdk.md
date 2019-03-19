---
title: DPDK in een Azure Linux-VM | Microsoft Docs
description: Informatie over het instellen van DPDK in een virtuele Linux-machine.
services: virtual-network
documentationcenter: na
author: laxmanrb
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2018
ms.author: labattul
ms.openlocfilehash: c5cb840035c5d0d5694982324c7237c58001e689
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57993873"
---
# <a name="set-up-dpdk-in-a-linux-virtual-machine"></a>DPDK instellen in een virtuele Linux-machine

Gegevens vlak Development Kit (DPDK) op Azure biedt een snellere gebruikersruimte pakketverwerken framework voor prestatie-intensieve toepassingen. Dit framework omzeilt van de virtuele machine kernel-netwerkstack.

In de verwerking van typische pakketten die gebruikmaakt van de netwerkstack kernel, is het proces interrupt gebaseerde. Wanneer de network interface inkomende pakketten ontvangt, is er een kernel-interrupt naar proces die het pakket en een context van de kernelruimte aan de gebruiker overschakelen. DPDK elimineert overschakelen van context en de methode interrupt gebaseerde en vervangen door een gebruikersruimte implementatie dat gebruikt modus stuurprogramma's voor snelle pakket te verwerken pollen.

DPDK bestaat uit sets gebruikersruimte bibliotheken die toegang tot bronnen op lagere niveaus bieden. Deze resources kunnen opnemen hardware, logische kernen, geheugen-management en poll-modus stuurprogramma's voor netwerkinterfacekaarten.

DPDK kunt uitvoeren op Azure virtual machines die ondersteuning voor meerdere besturingssysteem distributies bieden. DPDK biedt prestatie differentiatie in het netwerk functie virtualization implementaties te stimuleren. Deze implementaties kunnen duren voordat de vorm van virtuele netwerkapparaten (NVA's), zoals virtuele routers, firewalls, VPN's, load balancers, ontwikkelde pakket kernen en denial-of-service (DDoS)-toepassingen.

## <a name="benefit"></a>Voordeel

**Hogere pakketten per seconde (PPS)**: Het aantal cyclus overslaan van de kernel en met inachtneming van pakketten in de gebruikersruimte worden beperkt door het elimineren van context switches. Het verbetert ook het aantal pakketten dat per seconde in virtuele Azure Linux-machines worden verwerkt.


## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

De volgende distributies uit de galerie met Azure worden ondersteund:

| Linux-besturingssysteem     | Kernelversie        |
|--------------|----------------       |
| Ubuntu 16.04 | 4.15.0-1015-azure     |
| Ubuntu 18.04 | 4.15.0-1015-azure     |
| SLES 15      | 4.12.14-5.5-azure     |
| RHEL 7.5     | 3.10.0-862.9.1.el7    |
| CentOS 7.5   | 3.10.0-862.3.3.el7    |

**Aangepaste kernel-ondersteuning**

Zie voor een Linux-kernel-versie die niet wordt vermeld, [Patches voor het bouwen van een Azure-afgestemd op Linux-kernel](https://github.com/microsoft/azure-linux-kernel). Voor meer informatie, u kunt ook contact opnemen met [ azuredpdk@microsoft.com ](mailto:azuredpdk@microsoft.com). 

## <a name="region-support"></a>Ondersteuning voor regio

Alle Azure-regio's ondersteunen DPDK.

## <a name="prerequisites"></a>Vereisten

Versneld netwerken moet zijn ingeschakeld op een virtuele Linux-machine. De virtuele machine moet ten minste twee netwerkinterfaces, één interface voor het beheer hebben. Meer informatie over het [maken van een virtuele Linux-machine met versneld netwerkondersteuning ingeschakeld](create-vm-accelerated-networking-cli.md).

## <a name="install-dpdk-dependencies"></a>Installeer DPDK afhankelijkheden

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo add-apt-repository ppa:canonical-server/dpdk-azure -y
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev libmnl-dev
```

### <a name="rhel75centos-75"></a>7.5 RHEL7.5/CentOS

```bash
yum -y groupinstall "Infiniband Support"
sudo dracut --add-drivers "mlx4_en mlx4_ib mlx5_ib" -f
yum install -y gcc kernel-devel-`uname -r` numactl-devel.x86_64 librdmacm-devel libmnl-devel
```

### <a name="sles-15"></a>SLES 15

**Azure kernel**

```bash
zypper  \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-azure kernel-devel-azure gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

**Standaard-kernel**

```bash
zypper \
  --no-gpg-checks \
  --non-interactive \
  --gpg-auto-import-keys install kernel-default-devel gcc make libnuma-devel numactl librdmacm1 rdma-core-devel
```

## <a name="set-up-the-virtual-machine-environment-once"></a>Instellen van de virtuele machine-omgeving (één keer)

1. [Download de meest recente DPDK](https://core.dpdk.org/download). Versie 18.02 of hoger is vereist voor Azure.
2. De standaard-configuratie met bouwen `make config T=x86_64-native-linuxapp-gcc`.
3. Mellanox PMDs inschakelen in de gegenereerde configuratie met `sed -ri 's,(MLX._PMD=)n,\1y,' build/.config`.
4. Compileren met `make`.
5. Installeren met `make install DESTDIR=<output folder>`.

## <a name="configure-the-runtime-environment"></a>De runtime-omgeving configureren

Na het opnieuw opstarten, moet u de volgende opdrachten één keer uitgevoerd:

1. Hugepages

   * Hugepage configureren door het uitvoeren van de volgende opdracht eenmaal voor alle numanodes:

     ```bash
     echo 1024 | sudo tee
     /sys/devices/system/node/node*/hugepages/hugepages-2048kB/nr_hugepages
     ```

   * Maak een map voor koppelen met `mkdir /mnt/huge`.
   * Koppelpunt hugepages met `mount -t hugetlbfs nodev /mnt/huge`.
   * Controleer dat hugepages zijn gereserveerd met `grep Huge /proc/meminfo`.

     > [!NOTE]
     > Er is een manier om het bestand wormgaten wijzigen zodat hugepages bij het opstarten zijn gereserveerd door het volgende op de [instructies](https://dpdk.org/doc/guides/linux_gsg/sys_reqs.html#use-of-hugepages-in-the-linux-environment) voor de DPDK. De instructies zijn aan de onderkant van de pagina. Wanneer u een Azure Linux-machine gebruikt, wijzigt u bestanden onder **/etc/config/grub.d** in plaats daarvan te reserveren hugepages tijdens opnieuw opstarten.

2. MAC- en IP-adressen: Gebruik `ifconfig –a` om het MAC- en IP-adres van de netwerkinterfaces weer te geven. De *VF* netwerkinterface en *NETVSC* netwerkinterface hebben hetzelfde MAC-adres, maar alleen de *NETVSC* netwerkinterface heeft een IP-adres. VF-interfaces worden uitgevoerd als onderliggende-interfaces van NETVSC interfaces.

3. PCI-adressen

   * Gebruik `ethtool -i <vf interface name>` om erachter te komen welke PCI-adres moet worden gebruikt voor *VF*.
   * Als *eth0* versnelde netwerken is ingeschakeld, zorg ervoor dat testpmd niet per ongeluk overnemen van het apparaat voor het pci van VF voor *eth0*. Als de toepassing DPDK ten opzichte van de netwerkinterface management heeft per ongeluk en zorgt ervoor dat u de SSH-verbinding wordt verbroken, gebruikt u de seriële console om te stoppen van de toepassing DPDK. U kunt ook de seriële console gebruiken om te stoppen en starten van de virtuele machine.

4. Load *ibuverbs* op elke opnieuw opstarten met `modprobe -a ib_uverbs`. Voor SLES 15, ook laden *mlx4_ib* met `modprobe -a mlx4_ib`.

## <a name="failsafe-pmd"></a>Failsafe PMD

DPDK toepassingen moeten worden uitgevoerd via de failsafe PMD die wordt weergegeven in Azure. Als de toepassing wordt uitgevoerd boven op de PMD VF, het ontvangt geen **alle** pakketten die bestemd zijn voor de virtuele machine, aangezien sommige pakketten worden weergegeven via de interface van synthetische. 

Als u een toepassing DPDK via de failsafe PMD uitvoert, zorgt u ervoor dat de toepassing alle pakketten die bestemd zijn voor deze ontvangt. Deze ook zorgt ervoor dat de toepassing blijft actief in de modus DPDK, zelfs als de VF wordt ingetrokken wanneer de host wordt onderhouden. Zie voor meer informatie over failsafe PMD [failsafe poll-modus-stuurprogrammabibliotheek](https://doc.dpdk.org/guides/nics/fail_safe.html).

## <a name="run-testpmd"></a>Testpmd uitvoeren

Gebruiken om uit te voeren testpmd in de hoofdmap modus, `sudo` voordat de *testpmd* opdracht.

### <a name="basic-sanity-check-failsafe-adapter-initialization"></a>Basic: Controle, initialisatie van de adapter failsafe

1. Voer de volgende opdrachten om een losse poort testpmd-toepassing te starten:

   ```bash
   testpmd -w <pci address from previous step> \
     --vdev="net_vdev_netvsc0,iface=eth1" \
     -- -i \
     --port-topology=chained
    ```

2. Voer de volgende opdrachten om een dubbele poort testpmd-toepassing te starten:

   ```bash
   testpmd -w <pci address nic1> \
   -w <pci address nic2> \
   --vdev="net_vdev_netvsc0,iface=eth1" \
   --vdev="net_vdev_netvsc1,iface=eth2" \
   -- -i
   ```

   Als u testpmd met meer dan twee NIC's, de `--vdev` argument volgt dit patroon: `net_vdev_netvsc<id>,iface=<vf’s pairing eth>`.

3.  Nadat deze gestart, voert u `show port info all` om te controleren van informatie over de poort. U ziet nu een of twee DPDK-poorten die net_failsafe zijn (niet *net_mlx4*).
4.  Gebruik `start <port> /stop <port>` verkeer starten.

De voorgaande opdrachten start *testpmd* in de interactieve modus, die wordt aanbevolen om testpmd opdrachten uit te proberen.

### <a name="basic-single-sendersingle-receiver"></a>Basic: Één afzender/één ontvanger

De pakketten statistieken per seconde wordt periodiek afdrukken door de volgende opdrachten:

1. Voer de volgende opdracht aan de zijde TX:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device you plan to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
   ```

2. Aan de zijde RX, voer de volgende opdracht:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device you plan to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=rxonly \
     --eth-peer=<port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
   ```

Wanneer u de vorige opdrachten op een virtuele machine uitvoert, veranderen *IP_SRC_ADDR* en *IP_DST_ADDR* in `app/test-pmd/txonly.c` zodat deze overeenkomt met het IP-adres van de virtuele machines voordat u compileren. Anders worden de pakketten verwijderd voordat de ontvanger wordt bereikt.

### <a name="advanced-single-sendersingle-forwarder"></a>Geavanceerd: Één afzender/één doorstuurserver
De pakketten statistieken per seconde wordt periodiek afdrukken door de volgende opdrachten:

1. Voer de volgende opdracht aan de zijde TX:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address of the device you plan to use> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     -- --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     --eth-peer=<port id>,<receiver peer MAC address> \
     --stats-period <display interval in seconds>
    ```

2. Voer de volgende opdracht aan de zijde FWD:

   ```bash
   testpmd \
     -l <core-list> \
     -n <num of mem channels> \
     -w <pci address NIC1> \
     -w <pci address NIC2> \
     --vdev="net_vdev_netvsc<id>,iface=<the iface to attach to>" \
     --vdev="net_vdev_netvsc<2nd id>,iface=<2nd iface to attach to>" (you need as many --vdev arguments as the number of devices used by testpmd, in this case) \
     -- --nb-cores <number of cores to use for test pmd> \
     --forward-mode=io \
     --eth-peer=<recv port id>,<sender peer MAC address> \
     --stats-period <display interval in seconds>
    ```

Wanneer u de vorige opdrachten op een virtuele machine uitvoert, veranderen *IP_SRC_ADDR* en *IP_DST_ADDR* in `app/test-pmd/txonly.c` zodat deze overeenkomt met het IP-adres van de virtuele machines voordat u compileren. Anders worden de pakketten verwijderd voordat de doorstuurserver is bereikt. Kunt u zich niet aan een derde machine doorgestuurd verkeer ontvangen hebben omdat de *testpmd* doorstuurserver wijzigt niet de laag-3-adressen, tenzij u enkele codewijzigingen.

## <a name="references"></a>Verwijzingen

* [Opties voor EAL](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#eal-command-line-options)
* [Testpmd opdrachten](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#testpmd-command-line-options)

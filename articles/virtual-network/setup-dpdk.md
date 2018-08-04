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
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/27/2018
ms.author: labattul
ms.openlocfilehash: d09d2bf7d14508b0b8eac955eb2589c0e7384903
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506512"
---
# <a name="setup-dpdk-in-a-linux-virtual-machine"></a>Setup DPDK in een virtuele Linux-machine

Gegevens vlak Development Kit (DPDK) op Azure biedt een snellere gebruiker ruimte pakket framework voor gegevensverwerking voor prestatie-intensieve toepassingen die niet van de virtuele machine kernel-netwerkstack gebruikmaken.

Typische pakket verwerkt met behulp van de kernel-netwerkstack is interrupts bestuurd. Telkens wanneer die de network interface inkomende pakketten ontvangt, is er een kernel-interrupt voor het verwerken van de pakket- en context overschakelen van de kernelruimte aan gebruikersruimte. DPDK elimineert overschakelen van context en de interrupt gestuurde methode en vervangen door een gebruiker ruimte implementatie met behulp van poll-modus stuurprogramma's voor snelle pakket te verwerken.

DPDK bestaat uit de set van gebruiker ruimte bibliotheken bieden toegang tot resources zoals hardware, logische kernen, geheugen lager niveau en pollen modus stuurprogramma's voor netwerkinterfacekaarten.

DPDK kunt uitvoeren in virtuele machines van Azure, ondersteuning van meerdere besturingssysteem distributies. DPDK biedt een prestatie-differentiatie in sparen netwerkfunctie virtualization-implementaties in de vorm van virtuele netwerkapparaten (NVA), zoals een virtuele-router, firewall, VPN, load balancer, ontwikkelde pakket core en denial-of-service ( DDoS)-toepassingen.

## <a name="benefit"></a>Voordeel

**Hogere pakketten per seconde (PPS)**: voor het overslaan van de kernel en met inachtneming van pakketten in de gebruikersruimte vermindert het aantal cyclus door het elimineren van context-switch en verbetert het aantal pakketten dat per seconde in virtuele Azure Linux-machines wordt verwerkt.


## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

De volgende distributies uit de galerie met Azure worden ondersteund:

| Linux-besturingssysteem     | Kernelversie        |
|--------------|----------------       |
| Ubuntu 16.04 | 4.15.0-1015-Azure     |
| Ubuntu 18.04 | 4.15.0-1015-Azure     |
| SLES 15      | 4.12.14-5.5-Azure     |
| RHEL 7.5     | 3.10.0-862.9.1.el7    |
| CentOS 7.5   | 3.10.0-862.3.3.el7    |

**Aangepaste kernel-ondersteuning**

Raadpleeg [Patches voor het bouwen van een Azure-afgestemd op Linux-kernel](https://github.com/microsoft/azure-linux-kernel) voor een Linux-kernel-versie niet wordt vermeld, of voor nadere informatie, neem dan contact op met [ azuredpdk@microsoft.com ](mailto:azuredpdk@microsoft.com). 

## <a name="region-support"></a>Ondersteuning voor regio

Alle Azure-regio's ondersteunen DPDK.

## <a name="prerequisites"></a>Vereiste onderdelen

Versneld netwerken moet zijn ingeschakeld op een virtuele Linux-machine. De virtuele machine moet ten minste twee netwerkinterfaces, één interface voor het beheer hebben. Meer informatie over het [maken van een virtuele Linux-machine met versneld netwerkondersteuning ingeschakeld](create-vm-accelerated-networking-cli.md).

## <a name="install-dpdk-dependencies"></a>Installeer DPDK afhankelijkheden

### <a name="ubuntu-1804"></a>Ubuntu 18.04

```bash
sudo add-apt-repository ppa:canonical-server/dpdk-azure -y
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo apt-get update
sudo apt-get install -y librdmacm-dev librdmacm1 build-essential libnuma-dev
```

### <a name="rhel75centos-75"></a>7.5 RHEL7.5/CentOS

```bash
yum -y groupinstall "Infiniband Support"
sudo dracut --add-drivers "mlx4_en mlx4_ib mlx5_ib" -f
yum install -y gcc kernel-devel-`uname -r` numactl-devel.x86_64 librdmacm-devel
```

### <a name="sles-15"></a>SLES 15

**Azure-kernel**

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

## <a name="setup-virtual-machine-environment-once"></a>Setup-omgeving met virtuele machines (één keer)

1. [Download de meest recente DPDK](https://core.dpdk.org/download). Versie 18.02 of hoger is vereist voor Azure.
2. Installeer de *libnuma dev* inpakken met `sudo apt-get install libnuma-dev`.
3. Bouw eerst de standaard-config `make config T=x86_64-native-linuxapp-gcc`.
4. Mellanox PMDs inschakelen in de gegenereerde configuratie met `sed -ri 's,(MLX._PMD=)n,\1y,' build/.config`.
5. Compileren met `make`.
6. Installeren met `make install DESTDIR=<output folder>`.

# <a name="configure-runtime-environment"></a>Runtime-omgeving configureren

Voer de volgende opdrachten één keer, na het opnieuw opstarten:

1. Hugepages

   * Hugepage configureren door het uitvoeren van de volgende opdracht eenmaal voor alle numanodes:

     ```bash
     echo 1024 | sudo tee
     /sys/devices/system/node/node*/hugepages/hugepages-2048kB/nr_hugepages
     ```

   *  Maak een map voor koppelen met `mkdir /mnt/huge`.
   *  Koppelpunt hugepages met `mount -t hugetlbfs nodev /mnt/huge`.
   *  Selectievakje hugepages zijn gereserveerd met `grep Huge /proc/meminfo`.

     > [!NOTE]
     > Er is een manier om het bestand wormgaten wijzigen zodat grote pagina's bij het opstarten zijn gereserveerd door het volgende op de [instructies](http://dpdk.org/doc/guides/linux_gsg/sys_reqs.html#use-of-hugepages-in-the-linux-environment) voor DPDK. De instructie is aan de onderkant van de pagina. Wanneer wordt uitgevoerd in een Azure Linux-machine, wijzigen in /etc/config/grub.d bestanden in plaats daarvan moet worden gereserveerd hugepages tijdens opnieuw opstarten.

2. MAC- en IP-adressen: Gebruik `ifconfig –a` om het MAC- en IP-adres van de netwerkinterfaces weer te geven. De *VF* netwerkinterface en *NETVSC* netwerkinterface hebben hetzelfde MAC-adres, maar alleen de *NETVSC* netwerkinterface heeft een IP-adres.

3. PCI-adressen

   * Ontdek welke PCI-adres moet worden gebruikt voor *VF* met `ethtool -i <vf interface name>`.
   * Zorg ervoor dat testpmd niet per ongeluk overnemen van het apparaat voor het pci van VF voor *eth0*als *eth0* versnelde netwerken ingeschakeld. Als DPDK toepassing per ongeluk via de beheerinterface van het netwerk heeft genomen en zorgt ervoor verlies van uw SSH-verbinding dat, gebruikt u de seriële console kill DPDK toepassing, of als u wilt stoppen en starten van de virtuele machine.

4. Load *ibuverbs* op elke opnieuw opstarten met `modprobe -a ib_uverbs`. Laden voor SLES 15, *mlx4_ib* met 'modprobe - een mlx4_ib'.

## <a name="failsafe-pmd"></a>Failsafe PMD

DPDK toepassingen moeten worden uitgevoerd via de failsafe PMD die wordt weergegeven in Azure. Als de toepassing wordt uitgevoerd boven op de PMD VF, profiteert deze niet **alle** pakketten dat is bestemd voor de virtuele machine, aangezien sommige pakketten worden weergegeven via de interface van synthetische. Uitvoeren via de failsafe die PMD garandeert dat de toepassing alle pakketten dat is bestemd voor het ontvangt en zorgt er ook voor de toepassing wordt voortgezet om uit te voeren in de modus DPDK, zelfs als de VF wordt ingetrokken wanneer de host wordt onderhouden. Raadpleeg voor meer informatie over failsafe PMD [failsafe poll-modus-stuurprogrammabibliotheek](http://doc.dpdk.org/guides/nics/fail_safe.html).

## <a name="run-testpmd"></a>Testpmd uitvoeren

Gebruik `sudo` voordat de *testpmd* opdracht uit te voeren in de basis-modus.

### <a name="basic-sanity-check-failsafe-adapter-initialization"></a>Basic: Controle, initialisatie van de adapter failsafe

1. Voer de volgende opdrachten om een toepassing met één poort te starten:

   ```bash
   testpmd -w <pci address from previous step> \
     --vdev="net_vdev_netvsc0,iface=eth1" \
     -i \
     --port-topology=chained
    ```

2. Voer de volgende opdrachten om een toepassing met dubbele poort te starten:

   ```bash
   testpmd -w <pci address nic1> \
   -w <pci address nic2> \
   --vdev="net_vdev_netvsc0,iface=eth1" \
   --vdev="net_vdev_netvsc1,iface=eth2" \
   -i
   ```

   Als met meer dan 2 NIC's, de `--vdev` argument volgt dit patroon: `net_vdev_netvsc<id>,iface=<vf’s pairing eth>`.

3.  Nadat is gestart, voert `show port info all` om te controleren van informatie over de poort. U ziet nu een of twee DPDK-poorten die net_failsafe zijn (niet *net_mlx4*).
4.  Gebruik `start <port> /stop <port>` verkeer starten.

De voorgaande opdrachten start *testpmd* in de interactieve modus, die wordt aanbevolen, voor het uitproberen van sommige testpmd-opdrachten.

### <a name="basic-single-sendersingle-receiver"></a>Basic: Één afzender/één ontvanger

De pakketten statistieken per seconde wordt periodiek afdrukken door de volgende opdrachten:

1. Voer de volgende opdracht aan de zijde TX:

   ```bash
   Testpmd \
     –l <core-mask> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev=”net_vdev_netvsc<id>,iface=<the iface to attach to>” \
     --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     –eth-peer=<port id>,<peer MAC address> \
     --stats-period <display interval in seconds>
   ```

2. Aan de zijde RX, voer de volgende opdracht:

   ```bash
   Testpmd \
     –l <core-mask> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev net_vdev_netvsc<id>,iface=<the iface to attach to>” \
     --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=rxonly \
     –eth-peer=<port id>,<peer MAC address> \
     --stats-period <display interval in seconds>
   ```

Als u de vorige opdrachten uitvoert op een virtuele machine, wijzigen *IP_SRC_ADDR* en *IP_DST_ADDR* in `app/test-pmd/txonly.c` zodat deze overeenkomt met het IP-adres van de virtuele machines voordat u compileren. Anders worden de pakketten verwijderd voordat de ontvanger wordt bereikt.

### <a name="advanced-single-sendersingle-forwarder"></a>Geavanceerd: Één afzender/één doorstuurserver
De pakketten statistieken per seconde wordt periodiek afdrukken door de volgende opdrachten:

1. Voer de volgende opdracht aan de zijde TX:

   ```bash
   Testpmd \
     –l <core-mask> \
     -n <num of mem channels> \
     -w <pci address of the device intended to use> \
     --vdev net_vdev_netvsc<id>,iface=<the iface to attach to>” \
     --port-topology=chained \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=txonly \
     –eth-peer=<port id>,<peer MAC address> \
     --stats-period <display interval in seconds>
    ```

2. Voer de volgende opdracht aan de zijde FWD:

   ```bash
   Testpmd \
     –l <core-mask> \
     -n <num of mem channels> \
     -w <pci address NIC1> \
     -w <pci address NIC2> \
     --vdev=”net_vdev_netvsc<id>,iface=<the iface to attach to>” \
     --vdev=” net_vdev_netvsc<2nd id>,iface=<2nd iface to attach to>” (you need as many --vdev arguments as the number of devices used by testpmd, in this case) \
     --nb-cores <number of cores to use for test pmd> \
     --forward-mode=io \
     –eth-peer=<recv port id>,<peer MAC address> \
     --stats-period <display interval in seconds>
    ```

Als u de vorige opdrachten uitvoert op een virtuele machine, wijzigen *IP_SRC_ADDR* en *IP_DST_ADDR* in `app/test-pmd/txonly.c` zodat deze overeenkomt met het IP-adres van de virtuele machines voordat u compileren. Anders worden de pakketten verwijderd voordat de doorstuurserver is bereikt. Kunt u zich niet aan een derde machine doorgestuurd verkeer ontvangen hebben omdat de *testpmd* doorstuurserver wijzigt niet de laag-3-adressen, tenzij u enkele codewijzigingen.

## <a name="references"></a>Verwijzingen

* [Opties voor EAL](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#eal-command-line-options)
* [Testpmd opdrachten](https://dpdk.org/doc/guides/testpmd_app_ug/run_app.html#testpmd-command-line-options)

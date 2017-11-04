---
title: Optimaliseren van doorvoer van VM-netwerk | Microsoft Docs
description: Informatie over het optimaliseren van doorvoer van virtuele machine van Azure-netwerk.
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: steveesp
ms.openlocfilehash: 914747983d4d974810836be66d6c6af343f58b60
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Netwerkdoorvoer voor Azure virtual machines optimaliseren

Azure virtuele machines (VM) hebben netwerk standaardinstellingen die verder kunnen worden geoptimaliseerd voor netwerkdoorvoer. In dit artikel wordt beschreven hoe netwerkdoorvoer optimaliseren voor Microsoft Azure Windows en Linux-virtuele machines, inclusief belangrijke distributies zoals Ubuntu en CentOS van Red Hat.

## <a name="windows-vm"></a>Windows VM

Als uw virtuele machine van Windows wordt ondersteund met [versnelde netwerken](virtual-network-create-vm-accelerated-networking.md), het inschakelen van deze functie is de optimale configuratie voor de doorvoer. Met behulp van ontvangen Side Scaling (RSS) bereiken hogere maximale doorvoer dan een virtuele machine zonder RSS voor alle andere Windows-VM. RSS mogelijk in een virtuele machine van Windows standaard uitgeschakeld. Voer de volgende stappen uit om te bepalen of RSS is ingeschakeld en in te schakelen als deze uitgeschakeld.

1. Voer de `Get-NetAdapterRss` PowerShell-opdracht om te zien als de RSS is ingeschakeld voor een netwerkadapter. In de volgende voorbeelduitvoer geretourneerd door de `Get-NetAdapterRss`, RSS is niet ingeschakeld.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. Voer de volgende opdracht om in te schakelen van RSS:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    De vorige opdracht heeft geen uitvoer. De opdracht de NIC-instellingen, waardoor tijdelijke verbinding wordt verbroken voor ongeveer een minuut gewijzigd. Er verschijnt een dialoogvenster Reconnecting tijdens de verbinding wordt verbroken. Verbinding is doorgaans hersteld nadat de derde poging.
3. Controleer of RSS is ingeschakeld in de virtuele machine door te voeren de `Get-NetAdapterRss` opdracht opnieuw. Als dit lukt, wordt de volgende voorbeelduitvoer wordt geretourneerd:

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Virtuele Linux-machine

RSS is altijd ingeschakeld in een Azure Linux VM standaard. Linux-kernels die zijn uitgebracht sinds januari 2017 nieuwe netwerk Optimalisatieopties opnemen waarmee u een Linux-VM voor hogere netwerkdoorvoer.

### <a name="ubuntu"></a>Ubuntu

Om de optimalisatie, eerst een update naar de laatste ondersteunde versie, vanaf juni 2017 die:
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
Nadat de update voltooid is, voert u de volgende opdrachten om op te halen van de meest recente kernel:

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

Optionele opdracht:

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a>Ubuntu Azure Preview-kernel
> [!WARNING]
> Deze Azure Linux Preview kernel mogelijk niet dezelfde mate van beschikbaarheid en betrouwbaarheid als Marketplace-installatiekopieën en kernels die in het algemeen beschikbaarheid release. De functie wordt niet ondersteund, kan hebben beperkte mogelijkheden en zijn mogelijk niet zo betrouwbaar is als de standaard-kernel. Gebruik deze kernel niet voor productieworkloads.

De prestaties aanzienlijk doorvoer kan worden bereikt door het installeren van de voorgestelde Azure Linux-kernel. Als deze kernel, voegt u deze regel toe aan /etc/apt/sources.list

```bash
#add this to the end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

Voer deze opdrachten als hoofdmap.
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

Om de optimalisatie, eerst een update naar de laatste ondersteunde versie, vanaf juli-2017 is:
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
Nadat de update voltooid is, installeert u de meest recente Linux Integration Services (LIS).
De optimalisatie van doorvoer wordt LIS, vanaf 4.2.2-2. Voer de volgende opdrachten voor het installeren van LIS:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

Om de optimalisatie, eerst een update naar de laatste ondersteunde versie, vanaf juli-2017 is:
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
Nadat de update voltooid is, installeert u de meest recente Linux Integration Services (LIS).
De optimalisatie van doorvoer wordt LIS, vanaf 4.2. Voer de volgende opdrachten om te downloaden en installeren van LIS:

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Meer informatie over Linux Integration Services versie 4.2 voor Hyper-V door de [downloadpagina](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Volgende stappen
* Nu de virtuele machine is geoptimaliseerd, gaat u naar het resultaat met [bandbreedte/doorvoer testen van de virtuele machine van Azure](virtual-network-bandwidth-testing.md) voor uw scenario.
* Klik hier als u meer wilt weten met [Azure Virtual Network Veelgestelde vragen (FAQ)](virtual-networks-faq.md)

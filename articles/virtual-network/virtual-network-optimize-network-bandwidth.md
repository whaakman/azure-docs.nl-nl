---
title: VM-netwerkdoorvoer optimaliseren | Microsoft Docs
description: Informatie over het optimaliseren van de netwerkdoorvoer van virtuele machine van Azure.
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2017
ms.author: steveesp
ms.openlocfilehash: 50d7ca73e5e18f88f5d789e12fc7f26908e8b8f0
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025545"
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Netwerkdoorvoer van virtuele machines van Azure optimaliseren

Azure virtuele machines (VM) hebben standaard-netwerkinstellingen die verder kunnen worden geoptimaliseerd voor de netwerkdoorvoer. In dit artikel wordt beschreven hoe u netwerkdoorvoer optimaliseren voor Microsoft Azure Windows en Linux-VM's, met inbegrip van grote distributies zoals Ubuntu, CentOS en Red Hat.

## <a name="windows-vm"></a>Windows-VM

Als uw Windows-VM ondersteunt [versnelde netwerken](create-vm-accelerated-networking-powershell.md), inschakelen van deze functie is de optimale configuratie voor de doorvoer. Met behulp van ontvangen aan clientzijde schalen (RSS) bereiken hogere maximale doorvoer dan een virtuele machine zonder RSS voor alle andere Windows-VM's. RSS kan worden standaard uitgeschakeld in een Windows-VM. Om te bepalen of RSS is ingeschakeld en inschakelen als deze momenteel uitgeschakeld, voert u de volgende stappen uit:

1. Of RSS is ingeschakeld voor een netwerkadapter met de `Get-NetAdapterRss` PowerShell-opdracht. In de volgende voorbeelduitvoer geretourneerd door de `Get-NetAdapterRss`, RSS is niet ingeschakeld.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                 : False
    ```
2. Om in te schakelen RSS, voer de volgende opdracht:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    De vorige opdracht heeft geen uitvoer. De opdracht gewijzigd NIC-instellingen, veroorzaakt door tijdelijke verbindingsverlies voor ongeveer een minuut. Er verschijnt een dialoogvenster Reconnecting tijdens de verbinding is verbroken. Doorgaans wordt de verbinding hersteld nadat de derde poging.
3. Bevestig dat RSS is ingeschakeld in de virtuele machine door in te voeren de `Get-NetAdapterRss` opdracht opnieuw uit. Als dit lukt, wordt de volgende voorbeelduitvoer wordt geretourneerd:

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                  : True
    ```

## <a name="linux-vm"></a>Linux-VM

RSS is in een Azure Linux VM standaard altijd ingeschakeld. Linux kernels die zijn uitgebracht sinds oktober 2017 nieuwe netwerk optimalisaties opties opnemen waarmee u een Linux-VM voor een hogere netwerkdoorvoer.

### <a name="ubuntu-for-new-deployments"></a>Ubuntu voor nieuwe implementaties

De kernel Ubuntu Azure biedt de beste prestaties van het netwerk op Azure en de standaard-kernel is al sinds 21 September 2017. Om deze kernel, eerst de nieuwste ondersteunde versie van 16.04-LTS, als volgt installeren:

```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```

Nadat het aanmaken voltooid is, voert u de volgende opdrachten om op te halen van de meest recente updates. Deze stappen werken ook voor virtuele machines op dit moment met de Ubuntu Azure-kernel.

```bash
#run as root or preface with sudo
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

De volgende optionele opdrachtenset mogelijk nuttig zijn voor bestaande Ubuntu-implementaties die de Azure-kernel al hebben, maar die niet meer bijgewerkt met fouten.

```bash
#optional steps may be helpful in existing deployments with the Azure kernel
#run as root or preface with sudo
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

#### <a name="ubuntu-azure-kernel-upgrade-for-existing-vms"></a>Ubuntu Azure kernel-upgrade voor bestaande VM 's

Prestaties van de belangrijke netwerkdoorvoer kan worden bereikt door te upgraden naar de Azure Linux-kernel. Om te controleren of u deze kernel hebt, Controleer uw kernelversie.

```bash
#Azure kernel name ends with "-azure"
uname -r

#sample output on Azure kernel:
#4.13.0-1007-azure
```

Als uw virtuele machine niet de Azure-kernel hebt heeft, wordt het versienummer meestal begint met "4.4." Als de virtuele machine niet de Azure-kernel heeft, voer de volgende opdrachten als root:

```bash
#run as root or preface with sudo
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

Om de meest recente optimalisaties, is het raadzaam een virtuele machine maken met de nieuwste ondersteunde versie door de volgende parameters op te geven:

```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.4",
"Version": "latest"
```

Nieuwe en bestaande VM's kunnen profiteren van de meest recente Linux Integration Services (LIS) installeren. De optimalisatie van doorvoer is LIS, vanaf 4.2.2-2, hoewel latere versies verdere verbeteringen bevatten. Voer de volgende opdrachten voor het installeren van de meest recente LIS:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

Om de optimalisaties, is het raadzaam een virtuele machine maken met de nieuwste ondersteunde versie door de volgende parameters op te geven:

```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7-RAW"
"Version": "latest"
```

Nieuwe en bestaande VM's kunnen profiteren van de meest recente Linux Integration Services (LIS) installeren. De optimalisatie van doorvoer is LIS, vanaf 4.2. Voer de volgende opdrachten om te downloaden en installeren van LIS:

```bash
mkdir lis4.2.3-5
cd lis4.2.3-5
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.3-5.tar.gz
tar xvzf lis-rpms-4.2.3-5.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Meer informatie over Linux Integration Services versie 4.2 voor Hyper-V via de [downloadpagina](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Volgende stappen
* Zie het geoptimaliseerde resultaat met [bandbreedte/doorvoer testen van virtuele Azure-machine](virtual-network-bandwidth-testing.md) voor uw scenario.
* Meer informatie over hoe u [bandbreedte is toegewezen aan virtuele machines](virtual-machine-network-throughput.md)
* Leer meer met [Azure Virtual Network Veelgestelde vragen (FAQ)](virtual-networks-faq.md)

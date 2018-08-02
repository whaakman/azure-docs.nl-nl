---
title: Overzicht van Azure Linux-VM-Agent | Microsoft Docs
description: Informatie over het installeren en configureren van Linux-Agent (waagent) voor het beheren van de interactie van de virtuele machine met Azure-Infrastructuurcontroller.
services: virtual-machines-linux
documentationcenter: ''
author: zroiy
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: roiyz
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d3cba4e884b64d47c4e3109b23aebab7dd9da45f
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412157"
---
# <a name="understanding-and-using-the-azure-linux-agent"></a>Informatie over en het gebruik van de Azure Linux Agent

De Microsoft Azure Linux Agent (waagent) beheert Linux en FreeBSD inrichten en VM-interactie met de Azure-Infrastructuurcontroller. Naast de Linux-Agent inrichting functies bieden, biedt Azure ook de mogelijkheid van het gebruik van cloud-init voor sommige Linux OSes. De Linux-Agent biedt de volgende functionaliteit voor Linux en FreeBSD IaaS implementaties:

> [!NOTE]
> Zie voor meer informatie de [Leesmij-bestand](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
> 
> 

* **Inrichting van installatiekopieën**
  
  * Het maken van een gebruikersaccount
  * Configureren van SSH-verificatietypen
  * Implementatie van de openbare SSH-sleutels en paren van sleutels
  * Naam van de host instellen
  * Publiceren van de hostnaam voor het DNS-platform
  * Vingerafdruk voor SSH-host rapporteren aan het platform
  * Resourcebeheer voor schijf
  * Opmaak en koppelen van de bronschijf
  * Configureren van wisselruimte
* **Netwerken**
  
  * Routes voor een betere compatibiliteit met platform DHCP-servers beheren
  * Zorgt ervoor dat de stabiliteit van de naam van de netwerk-interface
* **Kernel**
  
  * Hiermee configureert u de virtuele NUMA (uitschakelen voor de kernel <`2.6.37`)
  * Hyper-V entropie voor /dev/random verbruikt
  * Hiermee configureert u SCSI time-outs voor het root-apparaat (die mogelijk externe)
* **Diagnostics**
  
  * Console-omleiding naar de seriële poort
* **SCVMM-implementaties**
  
  * Detecteert en de VMM-agent voor Linux bootstrapt bij het uitvoeren in een omgeving met System Center Virtual Machine Manager 2012 R2
* **VM-extensie**
  
  * Onderdelen die zijn gemaakt door Microsoft en Partners in Linux-VM (IaaS) om in te schakelen van software en automatisering van de configuratie invoeren
  * Referentie-implementatie op VM-extensie [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>Communicatie
De gegevensstroom van het platform aan de agent vindt plaats via twee kanalen:

* Een opstarttijd gekoppeld DVD voor IaaS-implementaties. Deze DVD bevat een OVF-compatibele configuratie-bestand met alle Inrichtingsgegevens dan de werkelijke SSH keypairs.
* Een TCP-eindpunt blootstellen van een REST-API gebruikt om op te halen van de implementatie en topologieconfiguratie.

## <a name="requirements"></a>Vereisten
De volgende systemen zijn getest en bekend is dat ze werken met de Azure Linux Agent:

> [!NOTE]
> Deze lijst kan verschillen van de officiële lijst met ondersteunde systemen op het Microsoft Azure-Platform, zoals hier wordt beschreven: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)
> 
> 

* CoreOS
* CentOS 6.3+
* Red Hat Enterprise Linux 6.7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Andere ondersteunde systemen:

* FreeBSD 10 + (Azure Linux Agent v2.0.10 +)

De Linux-agent is afhankelijk van sommige systeempakketten alleen juist werken:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Hulpprogramma's voor bestandssysteem: sfdisk, fdisk, mkfs, gescheiden
* Wachtwoord-hulpprogramma's: chpasswd, sudo
* Tekst voor het verwerken van hulpprogramma's: sed, grep
* Hulpprogramma's voor het netwerk: ip-route
* Kernel-ondersteuning voor het koppelen van bestandssystemen UDF.

## <a name="installation"></a>Installatie
Installatie met behulp van een RPM- of DEB-pakket uit de opslagplaats voor uw distributie van pakket is de voorkeursmethode voor het installeren en upgraden van de Azure Linux Agent. Alle de [distributie-providers die zijn goedgekeurd](../linux/endorsed-distros.md) het pakket van Azure Linux agent integreren in hun opslagplaatsen en installatiekopieën.

Raadpleeg de documentatie in de [Azure Linux Agent-opslagplaats op GitHub](https://github.com/Azure/WALinuxAgent) voor geavanceerde installatie-opties, zoals het installeren van de bron of naar aangepaste locaties of voorvoegsels.

## <a name="command-line-options"></a>Opdrachtregelopties
### <a name="flags"></a>Vlaggen
* uitgebreide: detailniveau van de opgegeven opdracht verhogen
* afdwingen: interactieve bevestiging voor bepaalde opdrachten overslaan

### <a name="commands"></a>Opdrachten
* Help: geeft een lijst van de ondersteunde opdrachten en vlaggen.
* inrichting: poging voor opschonen van het systeem en maken deze geschikt is voor beëindiging. De volgende bewerking verwijdert:
  
  * Alle SSH-hostsleutels (als Provisioning.RegenerateSshHostKeyPair 'y' in het configuratiebestand is)
  * Configuratie van de naamserver in /etc/resolv.conf
  * Hoofdwachtwoord uit/etc/shadow (als Provisioning.DeleteRootPassword 'y' in het configuratiebestand is)
  * In de cache leases voor DHCP-client
  * Herstel van hostnaam naar localhost.localdomain

> [!WARNING]
> Opheffen van inrichting is geen garantie dat de installatiekopie uitgeschakeld van alle gevoelige informatie en geschikt voor verdere distributie is.
> 
> 

* inrichting + gebruiker: alles in - inrichting (hierboven) wordt uitgevoerd en ook Hiermee verwijdert u de laatste ingerichte gebruikersaccount (verkregen van /var/lib/waagent) en gegevens die zijn gekoppeld. Deze parameter is bij het inrichten van een installatiekopie die eerder is ingericht op Azure, zodat deze kan worden vastgelegd en hergebruikt ongedaan maken.
* versie: geeft de versie van waagent
* serialconsole: Hiermee configureert u WORMGATEN als u wilt markeren ttyS0 (de eerste seriële poort) als de opstart-console. Dit zorgt ervoor dat de kernel wordt logboeken worden verzonden naar de seriële poort en beschikbaar gesteld voor foutopsporing.
* daemon: waagent uitvoeren als een daemon voor het beheren van interactie met het platform. Dit argument wordt opgegeven voor waagent in het waagent init-script.
* Start: waagent als achtergrondproces uitgevoerd

## <a name="configuration"></a>Configuratie
Een configuratiebestand (/ etc/waagent.conf) bepaalt de acties van waagent. Hieronder ziet u een voorbeeld-configuratiebestand:

    ```
    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.AllowResetSysUser=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None
    AutoUpdate.Enabled=y
    ```

De volgende die verschillende configuratie-opties worden beschreven. Configuratie-opties zijn van de drie typen; Booleaans, tekenreeks of geheel getal zijn. De Booleaanse configuratieopties kunnen worden opgegeven als "y" of "n". De speciale trefwoord 'None' als de volgende gegevens kunnen worden gebruikt voor bepaalde tekenreeks type configuratie-items:

**Provisioning.Enabled:**  
```
Type: Boolean  
Default: y
```
Hierdoor kan de gebruiker of de inrichting functionaliteit in de agent uit te schakelen. Geldige waarden zijn "y" of "n". Als inrichting is uitgeschakeld, SSH-host en de gebruiker sleutels in de afbeelding worden bewaard en een configuratie die is opgegeven in de Azure Inrichtings-API wordt genegeerd.

> [!NOTE]
> De `Provisioning.Enabled` parameter standaard ingesteld op "n" op Ubuntu Cloud afbeeldingen die cloud-init voor het inrichten gebruiken.
> 
> 

**Provisioning.DeleteRootPassword:**  
```
Type: Boolean  
Default: n
```
Als is ingesteld, het hoofdwachtwoord in het bestand/etc/shadow tijdens het inrichtingsproces is gewist.

**Provisioning.RegenerateSshHostKeyPair:**  
```
Type: Boolean  
Default: y
```
Als is ingesteld, alle SSH-host-sleutelparen (ecdsa, dsa en rsa) worden verwijderd tijdens het inrichtingsproces uit/etc/ssh /. En wordt een enkele nieuwe sleutelpaar gegenereerd.

Het versleutelingstype voor het nieuwe sleutelpaar kan worden geconfigureerd door de vermelding Provisioning.SshHostKeyPairType. SSH-sleutelparen voor ontbrekende versleutelingstypen die maken bepaalde distributies opnieuw wanneer de SSH-daemon opnieuw wordt gestart (bijvoorbeeld bij opnieuw opstarten).

**Provisioning.SshHostKeyPairType:**  
```
Type: String  
Default: rsa
```
Dit kan worden ingesteld op een type van de versleuteling algoritme die wordt ondersteund door de SSH-daemon op de virtuele machine. Normaal gesproken ondersteunde waarden zijn 'rsa', "dsa" en 'ecdsa'. "putty.exe" op Windows biedt geen ondersteuning voor 'ecdsa'. Als u van plan bent putty.exe gebruikt in Windows om het verbinding maken met een Linux-distributie, gebruikt u dus 'rsa' of 'dsa'.

**Provisioning.MonitorHostName:**  
```
Type: Boolean  
Default: y
```
Als set in waagent de virtuele Linux-machine voor wijzigingen van de hostnaam bewaakt (zoals geretourneerd door de opdracht 'hostname') en automatisch bijwerken van de configuratie van netwerken in de afbeelding in overeenstemming met de wijziging. Om te pushen de wijziging naar de DNS-servers,-netwerken opnieuw in de virtuele machine wordt gestart. Dit resulteert in het kort verlies van verbinding met Internet.

**Provisioning.DecodeCustomData**  
```
Type: Boolean  
Default: n
```
Als de set in waagent decodeert CustomData van Base64.

**Provisioning.ExecuteCustomData**  
```
Type: Boolean  
Default: n
```
Als de set in waagent voert CustomData na het inrichten.

**Provisioning.AllowResetSysUser**
```
Type: Boolean
Default: n
```
Met deze optie kunt het wachtwoord voor de gebruiker sys opnieuw moet worden ingesteld; Standaard is uitgeschakeld.

**Provisioning.PasswordCryptId**  
```
Type: String  
Default: 6
```
Het algoritme dat wordt gebruikt door crypt bij het genereren van wachtwoord-hash.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  

**Provisioning.PasswordCryptSaltLength**  
```
Type: String  
Default: 10
```
De lengte van willekeurige salt gebruikt bij het genereren van wachtwoord-hash.

**ResourceDisk.Format:**  
```
Type: Boolean  
Default: y
```
Als u het wachtwoord instelt, wordt de bronschijf geleverd door het platform opgemaakt en in waagent geladen als het type bestandssysteem is aangevraagd door de gebruiker in "ResourceDisk.Filesystem" iets anders dan 'ntfs is'. Een enkele partitie van het type Linux (83) is beschikbaar op de schijf. Deze partitie is niet geformatteerd als deze met succes kan worden gekoppeld.

**ResourceDisk.Filesystem:**  
```
Type: String  
Default: ext4
```
Hiermee geeft u het type bestandssysteem voor de resource-schijf. Ondersteunde waarden variëren per Linux-distributie. Als de tekenreeks is de X-en vervolgens mkfs. X moet aanwezig zijn op de Linux-installatiekopie. Installatiekopieën voor SLES 11 moeten meestal 'ext3' gebruiken. FreeBSD-installatiekopieën moeten hier 'ufs2' gebruiken.

**ResourceDisk.MountPoint:**  
```
Type: String  
Default: /mnt/resource 
```
Hiermee geeft u het pad waar de resource-schijf is gekoppeld. De resource-schijf is een *tijdelijke* schijf en kan worden leeggemaakt wanneer de inrichting van de virtuele machine is beëindigd.

**ResourceDisk.MountOptions**  
```
Type: String  
Default: None
```
Hiermee geeft u de koppelingsopties schijf moeten worden doorgegeven aan de opdracht mount -o. Dit is een door komma's gescheiden lijst met waarden, bijvoorbeeld. 'nodev, nosuid'. Zie mount(8) voor meer informatie.

**ResourceDisk.EnableSwap:**  
```
Type: Boolean  
Default: n
```
Als instellen, een wisselbestand (/ swapfile) is gemaakt op de bronschijf en toegevoegd aan de wisselruimte van het systeem.

**ResourceDisk.SwapSizeMB:**  
```
Type: Integer  
Default: 0
```
De grootte van het wisselbestand in megabytes.

**Logs.Verbose:**  
```
Type: Boolean  
Default: n
```
Als is ingesteld, gegevens opnemen in logboek wordt versterkt. Waagent meldt zich aan /var/log/waagent.log en maakt gebruik van de functionaliteit van de logrotate systeem voor het draaien van Logboeken.

**OS.EnableRDMA**  
```
Type: Boolean  
Default: n
```
Als is ingesteld, de agent probeert te installeren en vervolgens een RDMA-kernel-stuurprogramma te laden dat overeenkomt met de versie van de firmware van de onderliggende hardware.

**OS.RootDeviceScsiTimeout:**  
```
Type: Integer  
Default: 300
```
Deze instelling configureert u de SCSI-time-out in seconden op de OS-schijf- en gegevensstations. Als niet is ingesteld, het systeem standaardwaarden worden gebruikt.

**OS.OpensslPath:**  
```
Type: String  
Default: None
```
Deze instelling kan worden gebruikt om op te geven van een alternatief pad voor de openssl binaire moet worden gebruikt voor cryptografische bewerkingen.

**HttpProxy.Host, HttpProxy.Port**  
```
Type: String  
Default: None
```
Als deze proxyserver is ingesteld, de agent gebruikt voor toegang tot het internet. 

**AutoUpdate.Enabled**
```
Type: Boolean
Default: y
```
In- of uitschakelen van automatische updates voor de doelstatus verwerken; Standaard is ingeschakeld.



## <a name="ubuntu-cloud-images"></a>Cloud Ubuntu-installatiekopieën
Gebruikmaken van Ubuntu Cloudinstallatiekopieën [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) om uit te voeren veel configuratietaken die anders zouden worden beheerd door de Azure Linux Agent. De volgende punten zijn van toepassing:

* **Provisioning.Enabled** standaard ingesteld op "n" op Ubuntu Cloud afbeeldingen die cloud-init gebruiken voor het inrichtingsproces taken uitvoeren.
* De volgende configuratieparameters hebben geen effect op Ubuntu Cloudinstallatiekopieën die cloud-init gebruiken voor het beheren van de bronschijf en wisselruimte:
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**

* Zie voor meer informatie de volgende bronnen voor het configureren van het koppelpunt van de resource-schijf en wisselruimte op Ubuntu Cloudinstallatiekopieën tijdens het inrichten van:
  
  * [Ubuntu-Wiki: Wisselen partities configureren](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Aangepaste gegevens injecteren in een virtuele Machine van Azure](../windows/classic/inject-custom-data.md)


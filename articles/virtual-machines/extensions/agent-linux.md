---
title: Overzicht van Azure Linux VM-Agent | Microsoft Docs
description: Informatie over het installeren en configureren van Linux-Agent (waagent) voor het beheren van uw virtuele machine interactie met Azure-Infrastructuurcontroller.
services: virtual-machines-linux
documentationcenter: ''
author: danis
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
ms.author: danis
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2fe93cba2c8b295925ce4cfa8c3017ee1373261
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942765"
---
# <a name="understanding-and-using-the-azure-linux-agent"></a>Uitleg en het gebruik van de Azure Linux Agent

De Microsoft Azure Linux Agent (waagent) beheert Linux en FreeBSD inrichting en VM interactie met de Azure-Infrastructuurcontroller. Naast de Linux-Agent inrichting functies bieden, biedt Azure ook de mogelijkheid van het gebruik van cloud-init voor sommige Linux OSes. De Linux-Agent biedt de volgende functionaliteit voor Linux en FreeBSD IaaS implementaties:

> [!NOTE]
> Zie voor meer informatie de [Leesmij](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
> 
> 

* **Afbeelding inrichten**
  
  * Het maken van een gebruikersaccount
  * SSH-verificatietypen configureren
  * Implementatie van openbare SSH-sleutels en sleutelparen
  * Instellen van de hostnaam
  * Publiceren van de hostnaam voor het DNS-platform
  * Reporting SSH host vingerafdruk sleutel voor het platform
  * Resource Schijfbeheer
  * Opmaak en de resource-schijf koppelen
  * Wisselruimte configureren
* **Netwerken**
  
  * Routes voor een betere compatibiliteit met platform DHCP-servers beheert
  * Zorgt ervoor dat de stabiliteit van de naam van de interface
* **Kernel**
  
  * Hiermee configureert u virtuele NUMA (uitschakelen voor kernel <`2.6.37`)
  * Hyper-V entropie voor /dev/random verbruikt
  * Hiermee configureert u SCSI time-outs voor het root-apparaat (die mogelijk externe)
* **Diagnostics**
  
  * Console-omleiding naar de seriële poort
* **SCVMM-implementaties**
  
  * Detecteert en de VMM-agent voor Linux bootstraps wanneer in een omgeving met System Center Virtual Machine Manager 2012 R2 wordt uitgevoerd
* **VM-extensie**
  
  * Onderdeel geschreven door Microsoft en Partners in Linux VM (IaaS) om in te schakelen, software en configuratie automation invoeren
  * Implementatie van de VM-extensie verwijzing op [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>Communicatie
De informatiestroom van het platform voor de agent wordt uitgevoerd via twee kanalen:

* Een opstarttijd gekoppeld DVD voor IaaS-implementaties. Deze DVD bevat een OVF-compatibele configuratiebestand dat alle Inrichtingsgegevens dan de werkelijke SSH keypairs bevat.
* Een TCP-eindpunt blootstellen van een REST-API wordt gebruikt voor implementatie en topologieconfiguratie.

## <a name="requirements"></a>Vereisten
De volgende systemen zijn getest en bekend zijn bij het werken met de Azure Linux Agent:

> [!NOTE]
> Deze lijst kan afwijken van de officiële lijst van ondersteunde systemen op de Microsoft Azure-Platform, zoals hier wordt beschreven: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)
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

De Linux-agent is afhankelijk van sommige systeempakketten juist werken als:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Hulpprogramma's voor FileSystem: sfdisk fdisk, mkfs, gescheiden
* Hulpprogramma's voor wachtwoord: chpasswd, sudo
* Tekst voor het verwerken van hulpprogramma's: ype, grep
* Hulpprogramma's voor netwerk: IP-route
* Kernel-ondersteuning voor het koppelen van de UDF-bestandssystemen.

## <a name="installation"></a>Installatie
Installatie met een RPM of een pakket DEB vanuit uw distributiepunt pakket opslagplaats is de voorkeursmethode voor installatie en upgrade van de Azure Linux Agent. Alle de [goedgekeurd door providers van distributie](../linux/endorsed-distros.md) het Azure Linux agentpakket integreren in hun installatiekopieën en -opslagplaatsen.

Raadpleeg de documentatie in de [Azure Linux Agent opslagplaats op GitHub](https://github.com/Azure/WALinuxAgent) voor Geavanceerde installatieopties, zoals het installeren van de bron of naar aangepaste locaties of voorvoegsels.

## <a name="command-line-options"></a>Opdrachtregelopties
### <a name="flags"></a>Vlaggen
* uitgebreide: uitgebreidheid van de opgegeven opdracht verhogen
* afdwingen: interactieve bevestiging voor bepaalde opdrachten overslaan

### <a name="commands"></a>Opdrachten
* Help: bevat de ondersteunde opdrachten en vlaggen.
* identiteitsgegevens: poging tot het opschonen van het systeem, waardoor het geschikt is voor reprovisioning. De volgende bewerking verwijderen:
  
  * Alle SSH host-sleutels (als Provisioning.RegenerateSshHostKeyPair 'y' in het configuratiebestand is)
  * Configuratie van de naamserver in /etc/resolv.conf
  * Basis voor het wachtwoord van/etc/shadow (als Provisioning.DeleteRootPassword 'y' in het configuratiebestand is)
  * In de cache leases voor DHCP-client
  * Herstel van hostnaam naar localhost.localdomain

> [!WARNING]
> Opheffen van inrichting wordt niet gegarandeerd dat de installatiekopie van het gewiste van alle gevoelige informatie en geschikt is voor het opnieuw distribueren.
> 
> 

* identiteitsgegevens + gebruiker: alles in - deprovision (boven) uitvoert en ook worden verwijderd van de laatste ingerichte gebruikersaccount (verkregen uit /var/lib/waagent) en gegevens die zijn gekoppeld. Deze parameter is bij het inrichten van een afbeelding die eerder is ingericht op Azure zodat kan worden vastgelegd en hergebruikt ongedaan.
* versie: de versie van waagent weergeven
* serialconsole: GRUB ttyS0 markeert configureert (de eerste seriële poort) als het opstart-console. Dit zorgt ervoor dat de kernel wanneer logboeken worden verzonden naar de seriële poort en beschikbaar gesteld voor foutopsporing.
* daemon: waagent als een daemon voor het beheren van interactie met het platform worden uitgevoerd. Dit argument is opgegeven voor waagent in het waagent init-script.
* Start: waagent als een achtergrondproces uitvoeren

## <a name="configuration"></a>Configuratie
Een configuratiebestand (/ etc/waagent.conf) bepaalt de acties van waagent. Hieronder ziet u een voorbeeldconfiguratiebestand:

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

De volgende die diverse configuratieopties worden beschreven. Configuratie-opties zijn van de drie typen; Booleaanse waarde, String of Integer. De opties voor Boole-waarde kunnen worden opgegeven als "y" of "n". De speciale trefwoord 'None' voor sommige tekenreeks type configuratie-items worden gebruikt als de volgende details:

**Provisioning.Enabled:**  
```
Type: Boolean  
Default: y
```
Hierdoor kan de gebruiker of de inrichting functionaliteit in de agent uit te schakelen. Geldige waarden zijn "y" of "n". Inrichting is uitgeschakeld, SSH-host en de gebruiker sleutels in de afbeelding behouden blijven als een willekeurige configuratie is opgegeven in de Azure API-inrichting wordt genegeerd.

> [!NOTE]
> De `Provisioning.Enabled` parameter de standaardwaarde is "n" Ubuntu Cloud afbeeldingen die cloud init voor het inrichten gebruiken.
> 
> 

**Provisioning.DeleteRootPassword:**  
```
Type: Boolean  
Default: n
```
Als de set, het root-wachtwoord in het bestand/etc/shadow tijdens het inrichtingsproces worden gewist.

**Provisioning.RegenerateSshHostKeyPair:**  
```
Type: Boolean  
Default: y
```
Als de set, alle SSH host sleutelparen (ecdsa dsa en rsa) tijdens het inrichtingsproces van/etc/ssh/worden verwijderd. En een enkele nieuwe sleutelpaar gegenereerd.

Het versleutelingstype voor het nieuwe sleutelpaar kan worden geconfigureerd door de vermelding Provisioning.SshHostKeyPairType. SSH-sleutelparen voor ontbrekende versleutelingstypen die maken een aantal distributies opnieuw wanneer de SSH-daemon opnieuw wordt gestart (bijvoorbeeld, na het opnieuw opstarten).

**Provisioning.SshHostKeyPairType:**  
```
Type: String  
Default: rsa
```
Dit kan worden ingesteld op een versleutelingstype van de algoritme die wordt ondersteund door de SSH-daemon op de virtuele machine. De doorgaans ondersteunde waarden zijn 'rsa', 'dsa' en 'ecdsa'. 'putty.exe' in Windows biedt geen ondersteuning voor 'ecdsa'. Dus als u van plan bent putty.exe gebruiken op Windows verbinding maken met een Linux-implementatie gebruiken 'rsa' of 'dsa'.

**Provisioning.MonitorHostName:**  
```
Type: Boolean  
Default: y
```
Als de set, waagent bewaakt de virtuele Linux-machine voor wijzigingen van de hostnaam (zoals geretourneerd door de opdracht 'hostname') en automatisch bijwerken van de netwerkconfiguratie in de afbeelding in overeenstemming met de wijziging. Om de naamswijziging push naar de DNS-servers, netwerken opnieuw gestart in de virtuele machine. Dit resulteert in het kort verlies van verbinding met Internet.

**Provisioning.DecodeCustomData**  
```
Type: Boolean  
Default: n
```
Als de set, waagent decodeert CustomData van Base64.

**Provisioning.ExecuteCustomData**  
```
Type: Boolean  
Default: n
```
Als de set, waagent voert CustomData na het inrichten.

**Provisioning.AllowResetSysUser**
```
Type: Boolean
Default: n
```
Met deze optie kunt het wachtwoord voor de gebruiker sys worden gereset; Standaard is uitgeschakeld.

**Provisioning.PasswordCryptId**  
```
Type: String  
Default: 6
```
Door crypt gebruikt bij het genereren van wachtwoord-hash-algoritme.  
 1 - MD5  
 2a - Blowfish  
 5 - SHA-256  
 6 - SHA-512  

**Provisioning.PasswordCryptSaltLength**  
```
Type: String  
Default: 10
```
Lengte van willekeurige salt gebruikt bij het genereren van wachtwoord-hash.

**ResourceDisk.Format:**  
```
Type: Boolean  
Default: y
```
Als u het wachtwoord instelt, wordt de resource-schijf die door het platform opgemaakt en door waagent gekoppeld als het type bestandssysteem is aangevraagd door de gebruiker in 'ResourceDisk.Filesystem' iets anders dan 'ntfs'. Een enkele partitie van het type Linux (83) worden beschikbaar gemaakt op de schijf. Deze partitie is niet geformatteerd als met succes worden gekoppeld.

**ResourceDisk.Filesystem:**  
```
Type: String  
Default: ext4
```
Hiermee geeft u het type bestandssysteem voor de resource-schijf. Ondersteunde waarden verschillen per Linux-distributie. Als de tekenreeks is de X-en vervolgens mkfs. X moet aanwezig zijn op de Linux-installatiekopie. SLES 11 afbeeldingen moeten doorgaans 'ext3' gebruiken. Installatiekopieën van FreeBSD moeten 'ufs2' hier gebruiken.

**ResourceDisk.MountPoint:**  
```
Type: String  
Default: /mnt/resource 
```
Hiermee geeft u het pad waar de resource-schijf is gekoppeld. De resource-schijf is een *tijdelijke* schijfruimte en kan worden leeggemaakt wanneer de virtuele machine is gemaakt.

**ResourceDisk.MountOptions**  
```
Type: String  
Default: None
```
Hiermee geeft u een schijf koppelingsopties worden doorgegeven aan de opdracht mount -o. Dit is een door komma's gescheiden lijst met waarden, bijvoorbeeld. 'nodev, nosuid'. Zie mount(8) voor meer informatie.

**ResourceDisk.EnableSwap:**  
```
Type: Boolean  
Default: n
```
Als is ingesteld, een wisselbestand (/ swapfile) is gemaakt op de schijf resource en toegevoegd aan de wisselruimte van het systeem.

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
Als de set, logboek uitgebreidheid wordt versterkt. Waagent /var/log/waagent.log zich aanmeldt en maakt gebruik van de functionaliteit van de logrotate systeem om te draaien Logboeken.

**OS.EnableRDMA**  
```
Type: Boolean  
Default: n
```
Als is ingesteld, de agent probeert te installeren en vervolgens een RDMA-kernel-stuurprogramma te laden die overeenkomt met de versie van de firmware van de onderliggende hardware.

**OS.RootDeviceScsiTimeout:**  
```
Type: Integer  
Default: 300
```
Deze instelling configureert de SCSI-time-out in seconden op de OS-schijf- en stations. Als dat niet is ingesteld, het systeem standaardwaarden worden gebruikt.

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
In- of uitschakelen van automatische updates voor de doel-status verwerking; Standaard is ingeschakeld.



## <a name="ubuntu-cloud-images"></a>Ubuntu Cloud installatiekopieën
Ubuntu Cloud installatiekopieën gebruiken [cloud init](https://launchpad.net/ubuntu/+source/cloud-init) veel configuratietaken die anders zou worden beheerd door de Azure Linux Agent uit te voeren. Er gelden de volgende verschillen:

* **Provisioning.Enabled** de standaardwaarde is "n" Ubuntu Cloud afbeeldingen die gebruikmaken van cloud-init inrichting taken uit te voeren.
* De volgende configuratieparameters hebben geen effect op Ubuntu Cloud installatiekopieën die cloud init gebruiken voor het beheren van de resource-schijf en wisselruimte:
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**

* Zie voor meer informatie de volgende bronnen voor het configureren van het koppelpunt van de resource-schijf en wisselruimte op Ubuntu Cloud installatiekopieën tijdens het inrichten:
  
  * [Ubuntu-Wiki: Swap-partities configureren](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Injecteren van aangepaste gegevens in Azure een virtuele Machine](../windows/classic/inject-custom-data.md)


---
title: Snelstartgids - een virtuele VMware-machine op een Privécloud maken
description: Beschrijft het maken en een VMware-VM op CloudSimple Private Cloud
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/03/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 33354ce09ad6ba1a9a7c08a8cd3b945f3788011a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595690"
---
# <a name="create-vmware-virtual-machines-on-your-private-cloud"></a>Virtuele VMware-machines maken op uw Privécloud

Voor het maken van virtuele machines in uw persoonlijke Cloud, beginnen met het openen van de portal CloudSimple vanuit Azure portal.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Toegang tot de CloudSimple-portal

1. Selecteer **Alle services**.
2. Zoeken naar **CloudSimple Services**.
3. Selecteer de CloudSimple-service op die u wilt maken van uw Privécloud.
4. Uit de **overzicht** pagina, klikt u op **gaat u naar de portal CloudSimple** openen van een nieuw browsertabblad voor CloudSimple-portal.  Als u hierom wordt gevraagd, aanmelden met uw Azure-referenties aanmelden.  

    ![Starten van CloudSimple-portal](media/launch-cloudsimple-portal.png)

## <a name="launch-vcenter-web-ui"></a>Starten van de vCenter-web-ui

U kunt vCenter voor het instellen van virtuele machines en het beleid nu starten.

Voor toegang tot vCenter, start u vanuit de portal CloudSimple. Op de startpagina onder **algemene taken**, klikt u op **vSphere-Client starten**.  Selecteer de Privécloud en klik vervolgens op **vSphere-Client starten** op de Privécloud.

   ![VSphere-Client starten](media/launch-vcenter-from-cloudsimple-portal.png)

## <a name="upload-an-iso-or-vsphere-template"></a>Uploaden van een ISO- of vSphere-sjabloon

> [!WARNING]
> Gebruik de vSphere HTML5-client voor ISO-uploads.  Met behulp van Flash-client kan leiden tot een fout.

1. Verkrijg de ISO- of vSphere-sjabloon die u uploaden naar vCenter wilt naar een virtuele machine maken en deze beschikbaar op uw lokale systeem.

2. In vCenter, klikt u op de **schijf** pictogram en selecteer **vsanDatastore**. Klik op **bestanden** en klik vervolgens op **nieuwe map**.

    ![vCenter ISO](media/vcenter-create-folder.png)

3. Maak een map voor het opslaan van ISO-bestanden.

4. Navigeer naar de nieuwe map gemaakt en klik op **bestanden uploaden**. Volg de aanwijzingen op het scherm instructies voor het uploaden van de ISO.

## <a name="create-a-virtual-machine-in-vcenter"></a>Maak een virtuele Machine in VMware vCenter

1. In vCenter, klikt u op de **Hosts en Clusters** pictogram.

2. Met de rechtermuisknop op **werkbelasting** en selecteer **nieuwe virtuele Machine**.
    
    ![Virtuele machine maken](media/create-vcenter-virtual-machine-01.png)

3. Selecteer **maken van nieuwe virtuele machine** en klikt u op **volgende**.

    ![Wizard nieuwe virtuele machine](media/create-vcenter-virtual-machine-02.png)

4. De naam van de virtuele machine, selecteer de **werkbelasting van de virtuele machine** map en klik op **volgende**.

    ![Naam en map selecteren](media/create-vcenter-virtual-machine-03.png)

5. Selecteer de **werkbelasting** rekenresource en klikt u op **volgende**.

    ![Compute-resource selecteren](media/create-vcenter-virtual-machine-04.png)

6. Selecteer **vsanDatastore** en klikt u op **volgende**.

    ![Opslag selecteren](media/create-vcenter-virtual-machine-05.png)

7. Laat de standaardselectie voor de compatibiliteit van ESXi 6.5 en klik op **volgende**.

    ![Selecteer compatibiliteit](media/create-vcenter-virtual-machine-06.png)

8. Selecteer het gastbesturingssysteem van de ISO van voor de virtuele machine en klik op **volgende**.

    ![Gast-OS aanpassen](media/create-vcenter-virtual-machine-07.png)

9. Harde schijf en netwerkopties selecteren. Selecteer nieuwe CD/DVD-station **Datastore ISO-bestand**.  Als u dat verkeer van het openbare IP-adres aan deze virtuele machine is toegestaan wilt, selecteert u het netwerk als **vm-1**.

    ![Selecteer hardware aanpassing](media/create-vcenter-virtual-machine-08.png)

10. Een selectievenster wordt geopend. Selecteer het bestand dat u eerder hebt geüpload naar de map ISO's en -sjablonen en klikt u op **OK**.

    ![ISO selecteren](media/create-vcenter-virtual-machine-10.png)

11. Controleer de instellingen en klik op **OK** te maken van de virtuele machine.

    ![Opties controleren](media/create-vcenter-virtual-machine-11.png)

De virtuele machine wordt nu toegevoegd aan de werkbelasting van compute-resources en gereed is voor gebruik. 

![Nieuwe virtuele machine in VMware vCenter](media/create-vcenter-virtual-machine-12.png)

De eenvoudige configuratie is nu voltooid. U kunt starten met behulp van uw Privécloud vergelijkbaar met hoe u uw on-premises virtuele machine-infrastructuur gebruiken.

Volgende secties bevatten optionele informatie over het instellen van DNS en DHCP-servers voor Private Cloud workloads en het wijzigen van de standaard-netwerkconfiguratie.

## <a name="add-users-and-identity-sources-to-vcenter-optional"></a>Gebruikers en identiteiten bronnen toevoegen aan vCenter (optioneel)

CloudSimple wijst een standaardgebruikersaccount van vCenter met gebruikersnaam **cloudowner@cloudsimple.local** . Er zijn geen extra accountconfiguratie is vereist om aan de slag.  Beheerders CloudSimple normaal toegewezen de bevoegdheden die ze nodig hebben normale bewerkingen uit te voeren.  Instellen van uw on-premises active directory of Azure AD als een [extra identiteitsbron](https://docs.azure.cloudsimple.com/set-vcenter-identity/) op uw Privécloud.

## <a name="create-a-dns-and-dhcp-server-optional"></a>Maak een DNS- en DHCP-server (optioneel)

Toepassingen en workloads die worden uitgevoerd in een Privécloud-omgeving moeten naamomzetting en DHCP-services voor een lookup- en IP-adrestoewijzing. Een juiste DHCP en DNS-infrastructuur is vereist om deze services te leveren. U kunt een virtuele machine configureren in vCenter om deze services in uw omgeving Private Cloud te leveren.

### <a name="prerequisites"></a>Vereisten

* Een gedistribueerde poortgroep met VLAN geconfigureerd

* Route instellen voor on-premises of op basis van een Internet-DNS-servers

* Virtuele-machinesjabloon of ISO naar een virtuele machine maken

De volgende koppelingen bieden richtlijnen over het instellen van DHCP en DNS-servers op Linux- en Windows.

### <a name="linux-based-dns-server-setup"></a>Installatie op basis van Linux-DNS-Server

Linux biedt verschillende pakketten voor het instellen van DNS-servers.  Hier volgt een koppeling naar instructies voor het instellen van een open-source BIND DNS-server.

[Voorbeeld van de installatie](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-centos-7)

### <a name="windows-based-setup"></a>Windows-installatie

Deze Microsoft-artikelen wordt beschreven hoe u een Windows-server als een DNS-server en als een DHCP-server.
<br>
[WindowsServer-DNS-server](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

[Windows Server as DHCP Server](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-top)

## <a name="customize-networking-configuration-optional"></a>Aanpassen van de netwerkconfiguratie (optioneel)

De netwerk-pagina's in de portal CloudSimple kunnen u de configuratie voor de firewall-tabellen en openbare IP-adressen voor virtuele machines op te geven.

### <a name="allocate-public-ips"></a>Openbare IP-adressen toewijzen

1. Navigeer naar **netwerk > openbare IP-adres** in de portal CloudSimple.
2. Klik op **openbaar IP-adres toewijzen**.
3. Voer een unieke naam voor de IP-adres toe.
4. Selecteer de locatie van uw Privécloud.
5. Gebruik de schuifregelaar naar de time-out voor inactiviteit desgewenst wijzigen.
6. Voer in het lokale IP-adres waarvoor u wilt een openbaar IP-adres toewijzen.
7. Voer een bijbehorende DNS-naam desgewenst.
8. Klik op **Gereed**.

    ![Openbare IP](media/quick-create-pc-public-ip.png)

De taak van het toewijzen van het openbare IP-adres is gestart. U kunt de status van de taak controleren op de **activiteit > taken** pagina. Wanneer de toewijzing is voltooid, wordt het nieuwe item wordt weergegeven op de pagina openbaar IP-adressen.

De virtuele machine waaraan dit IP-adres moet worden toegewezen moet worden geconfigureerd met het lokale adres hierboven opgegeven. De procedure voor het configureren van een IP-adres is specifiek voor het besturingssysteem van de virtuele machine. Raadpleeg de documentatie voor uw besturingssysteem voor de virtuele machine voor de juiste procedure.

#### <a name="example"></a>Voorbeeld

Dit zijn bijvoorbeeld de details voor Ubuntu 16.04.

Voeg de statische methode toe aan de inet-familie-adresconfiguratie in het bestand ```/etc/network/interfaces```. Het adres, IP-masker en gateway waarden te wijzigen. In dit voorbeeld gebruiken we de eth0 interface, interne IP-adres 192.168.24.10, gatewayadres 192.168.24.1 en IP-masker 255.255.255.0. 

Bewerk de ```interfaces``` bestand.

```
sudo vi /etc/network/interfaces
```

Bijwerken van de volgende sectie in ```interfaces``` bestand.

```
auto eth0
iface eth0 inet static
address 192.168.24.10
netmask 255.255.255.0
gateway 192.168.24.1
dns-nameservers 8.8.8.8
dns-domain acme.com
dns-search acme.com
```

De interface uitschakelen.

```
sudo ifdown eth0
```

De interface opnieuw inschakelen.

```
sudo ifup eth0
```

Is standaard al het binnenkomende verkeer vanaf Internet **geweigerd**. Als u wilt een andere poort te openen, maakt u een [firewall tabel](https://docs.azure.cloudsimple.com/firewall/).

Controleer na het configureren van een interne IP-adres als het statische IP-adres, of dat u het Internet vanaf de virtuele machine kan bereiken.

```
ping 8.8.8.8
```

Controleer of dat u kunt bereiken de virtuele machine via Internet met behulp van het openbare IP-adres.

Zorg ervoor dat alle firewallregels (iptable) op de virtuele machine niet poort 80 blokkeert inkomende.

```
netstat -an | grep 80
```

Start een http-server op poort 80 luistert.
       
```
python2.7 -m SimpleHTTPServer 80
```

of

```
python3 -m http.server 80
```

Start een browser op het bureaublad en wijs poort 80 voor het openbare IP-adres om door te bladeren in de bestanden op uw virtuele machine. 

### <a name="default-cloudsimple-firewall-rules-for-public-ip"></a>Standaard CloudSimple firewall-regels voor openbare IP-adres

* VPN-verkeer: Al het verkeer tussen (van/naar) de VPN-verbinding en alle werkbelasting netwerken en beheernetwerk is toegestaan.
* Intern verkeer privécloud: Alle Oost-west-verkeer tussen (van/naar) netwerken van de werkbelasting en het beheernetwerk (hierboven) is toegestaan.
* Verkeer van Internet:
    * Al het binnenkomende verkeer vanaf het Internet wordt geweigerd voor werkbelasting-netwerken en het beheernetwerk.
    * Alle uitgaande verkeer naar Internet vanuit werkbelasting netwerken of het beheernetwerk is toegestaan.

U kunt de manier waarop die uw verkeer wordt beveiligd, ook wijzigen met behulp van de functie voor Firewall-regels. Zie voor meer informatie, [firewall tabellen en regels instellen](https://docs.azure.cloudsimple.com/firewall/).

## <a name="install-solutions-optional"></a>Installeer oplossingen (optioneel)
U kunt oplossingen installeren op uw CloudSimple Private Cloud te kunnen profiteren van uw Privécloud vCenter-omgeving. U kunt back-up, herstel na noodgevallen, replicatie en andere functies instellen om te beveiligen van uw virtuele machines. Voorbeelden zijn onder meer de VMware-Site Recovery Manager (VMware SRM) en Veeam back-up en replicatie.

Voor het installeren van een oplossing, moet u extra bevoegdheden aanvragen voor een beperkte periode. Zie [escaleren bevoegdheden](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges).

## <a name="next-steps"></a>Volgende stappen

* [Virtuele VMware-machines op Azure gebruiken](quickstart-create-vmware-virtual-machine.md)
* [Verbinding maken met on-premises netwerk met behulp van Azure ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection)
* [VPN-gateways op CloudSimple netwerk instellen](https://docs.azure.cloudsimple.com/vpn-gateway)

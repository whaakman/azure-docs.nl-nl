---
title: SAP Business één op Azure Virtual Machines | Microsoft Docs
description: SAP Business één op Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 101710b5a57faa37be77ff4b059fa0d494f4e617
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56878061"
---
# <a name="sap-business-one-on-azure-virtual-machines"></a>SAP Business één op virtuele Machines van Azure
Dit document biedt richtlijnen voor het implementeren van SAP Business One on Azure Virtual Machines. De documentatie is geen vervanging voor documentatie voor de installatie van bedrijf voor SAP. De documentatie zijn voor algemene richtlijnen voor de planning en implementatie voor de Azure-infrastructuur voor het uitvoeren van een zakelijke toepassingen op.

Business One ondersteunt twee verschillende databases:
- SQL Server - Zie [SAP Opmerking #928839 - Inhoudsrelease: Planning voor Microsoft SQL Server](https://launchpad.support.sap.com/#/notes/928839)
- SAP HANA - voor exacte SAP Business One ondersteuningsmatrix voor SAP HANA, bekijk de [SAP-Product beschikbaarheid Matrix](https://support.sap.com/pam)

SQL Server, de basisimplementatie overwegingen met betrekking tot zoals beschreven in de [Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms-guide) is van toepassing. voor SAP HANA, worden de overwegingen met betrekking tot vermeld in dit document.

## <a name="prerequisites"></a>Vereisten
Voor het gebruik van deze handleiding, moet u basiskennis hebt van de volgende Azure-onderdelen:

- [Azure virtuele machines op Windows](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
- [Azure virtuele machines op Linux](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure-netwerken en virtuele netwerken beheren met PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-virtual-network)
- [Azure-netwerken en virtuele netwerken met CLI](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure-schijven beheren met de Azure CLI](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Zelfs als u geïnteresseerd in bedrijven een bent alleen het document [Azure Virtual Machines planning en implementatie van SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) mag een goede bron van informatie.

De veronderstelling is dat u als de implementatie van SAP Business One-exemplaar:

- Bekend bent met het installeren van SAP HANA op een bepaalde infrastructuur, zoals een virtuele machine
- Vertrouwde installatie van de SAP Business One toepassing op een infrastructuur, zoals virtuele Azure-machines
- Bekend bent met het gebruik van SAP Business One en het gekozen DBMS-systeem
- Bekend bent met het implementeren van infrastructuur in Azure

Alle deze gebieden worden niet in dit document worden behandeld.

Naast de documentatie voor Azure moet u rekening houden met de belangrijkste SAP-opmerkingen, die verwijzen naar Business One of die centraal notities van SAP voor bedrijven een:

- [528296 - algemeen overzicht Opmerking voor een SAP Business-Releases en verwante producten](https://launchpad.support.sap.com/#/notes/528296)
- [2216195 - release-opmerkingen van de Updates voor SAP Business één 9.2, versie voor SAP HANA](https://launchpad.support.sap.com/#/notes/2216195)
- [2483583 - centraal Opmerking voor SAP Business één 9.3](https://launchpad.support.sap.com/#/notes/2483583)
- [2483615 - release-Opmerking Updates voor SAP Business één 9.3](https://launchpad.support.sap.com/#/notes/2483615)
- [2483595 - collectieve Opmerking voor SAP Business één 9.3 algemene problemen](https://launchpad.support.sap.com/#/notes/2483595)
- [2027458 - collectieve Consulting Opmerking voor SAP HANA-Related onderwerpen van SAP Business One, versie voor SAP HANA](https://launchpad.support.sap.com/#/notes/2027458)


## <a name="business-one-architecture"></a>Een architectuur voor bedrijven
Business One is een toepassing met twee lagen:

- Een clientlaag met een 'fat'-client
- Een database-laag met de database-schema voor een tenant

Een beter overzicht welke onderdelen worden uitgevoerd in het gedeelte van de client en welke onderdelen worden uitgevoerd in de server deel wordt beschreven in de [SAP Business een Administrator's Guide](https://help.sap.com/http.svc/rc/879bd9289df34a47af838e67d74ea302/9.3/en-US/AdministratorGuide_SQL.pdf) 

Omdat er zware latentie kritieke interactie tussen de clientlaag en de DBMS-laag, worden beide lagen moeten zich bevinden in Azure bij het implementeren in Azure. het is normaal dat de gebruikers vervolgens extern bureaublad-services in een of meerdere virtuele machines met een extern bureaublad-services voor de Business One clientonderdelen service.

### <a name="sizing-vms-for-sap-business-one"></a>Grootte van virtuele machines voor SAP Business een

Met betrekking tot de grootte van de client of meer virtuele machines, de resourcevereisten worden beschreven door SAP in het document [SAP Business één Hardware Requirements Guide](https://help.sap.com/http.svc/rc/011000358700000244612011e/9.3/en-US/B1_Hardware_Requirements_Guide.pdf). Voor Azure moet u zich concentreren en aan de vereisten vermeld in het hoofdstuk 2.4 van het document te berekenen.

Als Azure virtual machines voor het hosten van de Business One clientonderdelen en de DBMS-host, worden alleen VM's waarop SAP NetWeaver ondersteund zijn toegestaan. Als u wilt zoeken op de lijst met SAP NetWeaver ondersteunde Azure-VM's, lezen [SAP Opmerking #1928533](https://launchpad.support.sap.com/#/notes/1928533).

Uitvoeren van SAP HANA als DBMS back-end voor een bedrijf alleen virtuele machines, die voor bedrijven op HANA in de lijst staan de [HANA certifeid IaaS-platform lijst](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure%23SAP%20Business%20One) worden ondersteund voor HANA. De Business One clientonderdelen worden niet beïnvloed door deze sterkere beperking voor de SAP HANA DBMS-systeem.

### <a name="operating-system-releases-to-use-for-sap-business-one"></a>Besturingssysteemversies wilt gebruiken voor SAP Business One

In principe is het altijd het beste de meest recente versies van het besturingssysteem gebruiken. Met name in de Linux-ruimte is nieuwe functionaliteit van Azure met verschillende kleine meer recente versies van Suse- en Red Hat geïntroduceerd. Aan de Windows wordt met behulp van Windows Server 2016 sterk aanbevolen.


## <a name="deploying-infrastructure-in-azure-for-sap-business-one"></a>Azure-infrastructuur voor SAP Business One implementeren
In de volgende paar hoofdstukken stuks de infrastructuur van dit onderwerp voor het implementeren van SAP.

### <a name="azure-network-infrastructure"></a>De infrastructuur van Azure-netwerk
De netwerkinfrastructuur die u wilt implementeren in Azure, is afhankelijk van of u een enkel Business One systeem zelf implementeren. Of of u nu een hoster die als host fungeert voor tientallen Business One systemen voor klanten. Ook kunnen er kleine wijzigingen in het ontwerp op of hoe u verbinding met Azure maken. Doorlopen verschillende mogelijkheden, een ontwerp waar u een VPN-verbinding naar Azure hebt en waar u uw Active Directory via uitbreiden [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) in Azure.

![Eenvoudige netwerkconfiguratie met Business One](./media/business-one-azure/simple-network-with-VPN.PNG)

De vereenvoudigde configuratie weergegeven introduceert meerdere exemplaren van beveiliging waarmee beheer- en limiet routering. Deze begint met 

- De router/firewall op de on-premises klant.
- Het volgende exemplaar is de [Azure Network Security Group](https://docs.microsoft.com/azure/virtual-network/security-overview) die u gebruiken kunt om te introduceren van Routering en beveiligingsregels voor de Azure-VNet die u in uw SAP Business één configuratie worden uitgevoerd.
- Om te voorkomen dat gebruikers van Business One-client ook de server kunnen zien met de zakelijke één server, die de database wordt uitgevoerd, scheidt u de virtuele machine die als host fungeert voor de bedrijven een client en de business server in twee verschillende subnetten binnen het VNet.
- U zou Azure NSG toegewezen aan de twee verschillende subnetten opnieuw gebruiken om te beperken van toegang tot de zakelijke één server.

Een meer geavanceerde versie van de configuratie van een Azure-netwerk is gebaseerd op de Azure [beschreven aanbevolen procedures van hub en spoke-architectuur](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke). Het architectuurpatroon van de hub en spoke zou de eerste vereenvoudigde configuratie naar een als volgt wijzigen:


![Hub en spoke-configuratie met Business One](./media/business-one-azure/hub-spoke-network-with-VPN.PNG)

Voor gevallen waarin de gebruikers verbinding via het internet zonder particuliere connectiviteit in Azure maken, moet het ontwerp van het netwerk in Azure worden uitgelijnd met de principes die worden beschreven in de Azure-referentiearchitectuur voor [DMZ tussen Azure en het Internet](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-dmz).

### <a name="business-one-database-server"></a>Zakelijke één database-server
SQL Server en SAP HANA zijn beschikbaar voor het databasetype. Onafhankelijk van de DBMS-systemen, moet u het document lezen [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) om op te halen van een algemeen begrip van de DBMS-implementaties in azure VM's en de bijbehorende netwerken en opslag onderwerpen.

Hoewel de FDA al in de specifieke en algemene databasedocumenten, moet u uzelf bekend zijn met:

- [De beschikbaarheid van Windows virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) en [de beschikbaarheid van virtuele Linux-machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability)
- [SLA voor virtuele Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)

Deze documenten kunt u om te bepalen wat de selectie van opslagtypen en configuratie voor hoge beschikbaarheid.

In principe dient u:

- Premium SSD's gebruiken via standaard harde schijven. Zie voor meer informatie over de typen beschikbare schijfruimte, dan het artikel [een schijf selecteren](../../windows/disks-types.md)
- Azure Managed disks via niet-beheerde schijven gebruiken
- Zorg ervoor dat u voldoende IOPS en i/o-doorvoer geconfigureerd met de schijfconfiguratie hebt
- / Hana/gegevens en /hana/log volume combineren om te krijgen van een doeltreffende opslag kosten configuratie


#### <a name="sql-server-as-dbms"></a>SQL Server als DBMS-systemen
Ga voor SQL Server implementeren als DBMS-systemen voor Business One, aan het document [SQL Server Azure virtuele Machines DBMS-implementatie voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_sqlserver). 

Er zijn geen schattingen ruwe grootte voor de DBMS-zijde voor SQL Server:

| Aantal gebruikers | vCPU's | Geheugen | Voorbeeld van de VM-typen |
| --- | --- | --- | --- |
| maximaal 20 | 4 | 16 GB | D4s_v3, E4s_v3 |
| maximaal 40 | 8 | 32 GB | D8s_v3, E8s_v3 |
| maximaal 80 | 16 | 64 GB | D16s_v3, E16s_v3 |
| maximaal 150 | 32 | 128 GB | D32s_v3, E32s_v3 |

De hierboven vermelde sizing geeft waar te beginnen met een idee. Kan het zijn dat u meer of minder bronnen moet, in welk geval een aanpassing op azure is eenvoudig. Een wijziging tussen VM-typen is mogelijk met alleen een opnieuw opstarten van de virtuele machine.

#### <a name="sap-hana-as-dbms"></a>SAP HANA als DBMS-systemen
De overwegingen van het document met behulp van SAP HANA DBMS de volgende secties u moet volgen [SAP HANA op Azure-bedieningshandleiding](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).

Voor hoge beschikbaarheid en noodherstelconfiguraties om SAP HANA als een database voor Business One in Azure, moet u de documentatie lezen [SAP HANA met hoge beschikbaarheid voor Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview) en de documentatie waarnaar wordt verwezen naar van dat document.

Voor SAP HANA back-up en herstellen van strategieën, moet u het document lezen [back-uphandleiding voor SAP HANA op Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide) en de documentatie waarnaar wordt verwezen van dat document.

 
### <a name="business-one-client-server"></a>Business één client-server
Voor deze onderdelen zijn opslagoverwegingen niet het belangrijkste aandachtspunt. echter, wilt u beschikken over een betrouwbaar platform. Daarom moet u Azure Premium Storage gebruiken voor deze virtuele machine, zelfs voor de basis-VHD. Grootte van de virtuele machine, met de gegevens die zijn opgegeven in [SAP Business één Hardware Requirements Guide](https://help.sap.com/http.svc/rc/011000358700000244612011e/9.3/en-US/B1_Hardware_Requirements_Guide.pdf). Voor Azure moet u zich concentreren en aan de vereisten vermeld in het hoofdstuk 2.4 van het document te berekenen. Als u de vereisten voor berekenen, moet u ze te vergelijken met de volgende documenten kunnen de ideale VM zoeken:

- [Grootten voor Windows virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)
- [SAP-notitie 1928533 #](https://launchpad.support.sap.com/#/notes/1928533)

Vergelijk aantal CPU's en geheugen die nodig zijn voor wat wordt beschreven door Microsoft. Ook de netwerkdoorvoer in waarmee u rekening moet houden bij het kiezen van de virtuele machines.









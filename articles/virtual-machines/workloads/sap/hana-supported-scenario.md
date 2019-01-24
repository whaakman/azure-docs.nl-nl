---
title: Ondersteunde scenario's voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Scenario's ondersteund en informatie over hun architectuur voor SAP HANA op Azure (grote instanties)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/06/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4e8253238bf5edb5e0ea3f89fe67d6aa39f4a2d7
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54855452"
---
# <a name="supported-scenarios-for-hana-large-instances"></a>Ondersteunde scenario's voor HANA grote instanties
Dit document beschrijft de ondersteunde scenario's, samen met de details van de architectuur voor de HANA grote instanties (HLI).

>[!NOTE]
>Als uw vereiste scenario hier niet wordt vermeld, moet u contact op met Microsoft-Service Management-team voor het evalueren van uw vereisten.
Voordat u doorgaat met het inrichten van HLI eenheden, het ontwerp met SAP of uw implementatiepartner service te valideren.

## <a name="terms-and-definitions"></a>Termen en definities
We bekijken de termen en definities gebruikt in het document.

- SID: Systeem-id voor HANA-systeem.
- HLI: Hana grote instanties.
- DR: Een site voor noodherstel.
- Normaal DR: De installatie van een systeem met een toegewezen resource voor herstel na Noodgevallen doel alleen gebruikt.
- Meerdere doeleinden DR: Een systeem op DR-site die zijn geconfigureerd voor het gebruik van niet-productieomgeving, samen met de productie-exemplaar dat is geconfigureerd voor het gebruik van de gebeurtenis voor herstel na Noodgevallen. 
- Single SID:  Een systeem met één instantie geïnstalleerd.
- Multi-SID: Een systeem met meerdere exemplaren die zijn geconfigureerd. Ook wel een MCOS omgeving genoemd.


## <a name="overview"></a>Overzicht
De HANA grote instanties ondersteuning voor de verschillende architecturen om uit te voeren van uw zakelijke vereisten. De volgende lijst bevat informatie over de scenario's en hun configuratie-informatie. 

Het architectuurontwerp van de afgeleide is puur vanuit het perspectief van de infrastructuur en moet u SAP of uw implementatie-partners raadplegen voor de HANA-implementatie. Als uw scenario's niet weergegeven worden, neem dan contact op met de Microsoft-accountteam om te controleren van de architectuur en een oplossing voor u het afleiden.

**Deze architecturen zijn volledig compatibel met het ontwerp van de TDI (gegevensintegratie op maat gemaakt) en ondersteund door SAP.**

Dit document beschrijft de details van de twee onderdelen in elke ondersteunde architectuur:

- Ethernet
- Storage

### <a name="ethernet"></a>Ethernet

Elke server ingericht afkomstig is vooraf geconfigureerd met de sets Ethernet-interfaces. Hier volgen de details van de ethernet-interfaces op elke eenheid HLI geconfigureerd.

- **A**: Deze interface wordt gebruikt voor/door de clienttoegang.
- **B**: Deze interface wordt gebruikt voor de communicatie naar knooppunten. Deze interface is geconfigureerd op alle servers (ongeacht de topologie aangevraagd), maar alleen gebruikt voor de 
- scale-out-scenario's.
- **C**: Deze interface wordt gebruikt voor het knooppunt de opslagverbinding.
- **D**: Deze interface wordt gebruikt voor het knooppunt iSCSI-apparaatverbinding voor de installatie van stonith instellen. Deze interface is alleen geconfigureerd wanneer de installatie HSR wordt aangevraagd.  

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Knooppunt naar knooppunt |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | STONITH |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Knooppunt naar knooppunt |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | STONITH |

U gebruikt de interfaces op basis van de topologie die is geconfigureerd op de HLI-eenheid. De interface "B" wordt bijvoorbeeld instellen voor knooppunt naar communicatie, wat nuttig is wanneer u een scale-out-topologie die is geconfigureerd. In het geval van een configuratie met één knooppunt omhoog, wordt deze interface niet gebruikt. Controleer uw vereiste scenario's (verderop in dit document) voor meer informatie over het gebruik van de interface. 

Indien nodig, kunt u extra NIC-kaarten op uw eigen definiëren. De configuratie op de bestaande NIC's kan echter niet worden gewijzigd.

>[!NOTE]
>Mogelijk vindt u nog steeds meer netwerkinterfaces die zijn van fysieke interfaces of u de binding. U moet rekening houden met de bovengenoemde interfaces voor uw aanvraag wordt gebruikt, rest kan worden genegeerd / of niet te worden gematigd door middel van.

De verdeling van eenheden met twee IP-adressen die zijn toegewezen moet er als volgt uitzien:

- Ethernet 'A' moet een IP-adres toegewezen dat valt buiten het bereik voor het adres van Server-IP-adresgroep die u naar Microsoft verzonden. Dit IP-adres moet worden gebruikt voor het onderhouden van in/etc/hosts van het besturingssysteem.

- Ethernet 'C' moet een IP-adres toegewezen dat wordt gebruikt voor communicatie met NFS hebben. Daarom deze adressen doen **niet** moet worden behouden in etc/hosts kan het exemplaar-exemplaar verkeer binnen de tenant.

Een bladeconfiguratie met twee IP-adressen die zijn toegewezen is niet geschikt voor gevallen van de implementatie van HANA-Systeemreplicatie of HANA scale-out. Als met twee IP-adressen alleen worden toegewezen en die voor de implementatie van een dergelijke configuratie, neem contact op met SAP HANA op Azure Service Management om op te halen van een derde IP-adres in een derde VLAN toegewezen. Voor HANA grote instantie eenheden met drie IP-adressen die zijn toegewezen op drie NIC-poorten, zijn de volgende regels voor het gebruik van toepassing:

- Ethernet 'A' moet een IP-adres toegewezen dat valt buiten het bereik voor het adres van Server-IP-adresgroep die u naar Microsoft verzonden. Dit IP-adres wordt daarom niet worden gebruikt voor het onderhouden van in/etc/hosts van het besturingssysteem.

- Ethernet "B" mag uitsluitend worden gebruikt in etc/hosts voor de communicatie tussen de verschillende exemplaren worden bijgehouden. Deze adressen zou ook de IP-adressen die moeten worden bijgehouden in scale-out HANA configuraties als IP-adressen die HANA wordt gebruikt voor de configuratie tussen knooppunten.

- Ethernet 'C' moet een IP-adres toegewezen dat wordt gebruikt voor communicatie met NFS-opslag hebben. Dit type adressen moet daarom niet worden beheerd in etc/hosts.

- Ethernet "D" moet exclusief worden gebruikt voor toegang tot het apparaat stonith instellen voor pacemaker. Deze interface is vereist wanneer u HANA System Replication (HSR) configureren en wilt een automatische failover op het besturingssysteem met behulp van een SBD op basis van apparaat.


### <a name="storage"></a>Storage
Opslag vooraf is geconfigureerd op basis van de topologie die is aangevraagd. De volumes groter en koppelpunt afhankelijk van het aantal servers, -SKU's en -topologie die zijn geconfigureerd. Controleer uw vereiste scenario's (verderop in dit document) voor meer informatie. Als u meer opslag nodig is, kunt u deze kunt kopen in één TB verhoging.

>[!NOTE]
>Hetkoppelpunt/usr/sap/<SID> is een symbolische koppeling naar het koppelpunt/hana/gedeeld.


## <a name="supported-scenarios"></a>Ondersteunde scenario 's

In de architectuurdiagrammen worden volgende notaties gebruikt voor de afbeeldingen:

![Legends.PNG](media/hana-supported-scenario/Legends.PNG)

De volgende lijst bevat de ondersteunde scenario's:

1. Één knooppunt met een SID
2. Één knooppunt MCOS
3. Één knooppunt met DR (normaal)
4. Één knooppunt met DR (Multipurpose)
5. HSR met stonith instellen
6. HSR met DR (normale / Multipurpose) 
7. Host automatische failover (1 + 1) 
8. Scale-out met stand-by
9. Scale-out zonder stand-by
10. Scale-out met herstel na Noodgevallen



## <a name="1-single-node-with-one-sid"></a>1. Één knooppunt met een SID

Deze topologie biedt ondersteuning voor één knooppunt in een schaalset van configuratie met één SID.

### <a name="architecture-diagram"></a>Architectuurdiagram  

![Single-knooppunt-met-1-SID.png](media/hana-supported-scenario/Single-node-with-one-SID.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Geconfigureerd, maar niet wordt gebruikt |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Geconfigureerd, maar niet wordt gebruikt |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Geconfigureerd, maar niet wordt gebruikt |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Geconfigureerd, maar niet wordt gebruikt |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|/Hana/Shared/SID | HANA installeren | 
|/hana/data/SID/mnt00001 | Bestanden installeren | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren | 
|/Hana/logbackups/SID | Opnieuw uitvoeren van Logboeken |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.

## <a name="2-single-node-mcos"></a>2. Één knooppunt MCOS

Deze topologie biedt ondersteuning voor één knooppunt in een schaalset van configuratie met meerdere SID's.

### <a name="architecture-diagram"></a>Architectuurdiagram  

![single-node-mcos.png](media/hana-supported-scenario/single-node-mcos.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Geconfigureerd, maar niet wordt gebruikt |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Geconfigureerd, maar niet wordt gebruikt |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Geconfigureerd, maar niet wordt gebruikt |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Geconfigureerd, maar niet wordt gebruikt |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|/Hana/Shared/SID1 | HANA-installatie voor SID1 | 
|/hana/data/SID1/mnt00001 | Gegevensbestanden installeren voor SID1 | 
|/hana/log/SID1/mnt00001 | Logboekbestanden installeren voor SID1 | 
|/hana/logbackups/SID1 | Logboeken voor SID1 opnieuw |
|/hana/shared/SID2 | HANA-installatie voor SID2 | 
|/hana/data/SID2/mnt00001 | Gegevensbestanden installeren voor SID2 | 
|/hana/log/SID2/mnt00001 | Logboekbestanden installeren voor SID2 | 
|/hana/logbackups/SID2 | Logboeken voor SID2 opnieuw |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Verdeling van volume grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database-grootten in het geheugen worden ondersteund met multisid-omgeving.

## <a name="3-single-node-with-dr-normal"></a>3. Één knooppunt met DR (normaal)
 
Deze topologie biedt ondersteuning voor één knooppunt in een schaalset van configuratie met één of meerdere SID's met de replicatie op basis van opslag aan de DR-site voor een primaire SID. In het diagram wordt slechts één beveiligings-id wordt weergegeven op de primaire site, maar multisid (MCOS) wordt ook ondersteund.

### <a name="architecture-diagram"></a>Architectuurdiagram  

![Één knooppunt met dr.png](media/hana-supported-scenario/Single-node-with-dr.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Geconfigureerd, maar niet wordt gebruikt |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Geconfigureerd, maar niet wordt gebruikt |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Geconfigureerd, maar niet wordt gebruikt |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Geconfigureerd, maar niet wordt gebruikt |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|/Hana/Shared/SID | HANA-installatie voor beveiligings-id | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor beveiligings-id | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor SID | 
|/Hana/logbackups/SID | Opnieuw uitvoeren van Logboeken voor beveiligings-id |


### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Voor MCOS: Verdeling van volume grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database-grootten in het geheugen worden ondersteund met multisid-omgeving.
- Aan de DR: De volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Is vereist voor de installatie van HANA') voor de installatie van de HANA-exemplaar op de eenheid voor DR HLI voor productie. 
- Aan de DR: De gegevens, logbackups en gedeelde volumes (gemarkeerd als 'Storage-replicatie') worden gerepliceerd via momentopname van de productiesite. Deze volumes worden gekoppeld tijdens de failover-tijd. Lees voor meer informatie het document [failover noodherstelprocedure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) voor meer informatie.
- Opstartvolume voor **SKU-Type ik klasse** worden gerepliceerd naar de DR-knooppunt.


## <a name="4-single-node-with-dr-multipurpose"></a>4. Één knooppunt met DR (Multipurpose)
 
Deze topologie biedt ondersteuning voor één knooppunt in een schaalset van configuratie met één of meerdere SID's met de replicatie op basis van opslag aan de DR-site voor een primaire SID. In het diagram wordt slechts één beveiligings-id wordt weergegeven op de primaire site, maar multisid (MCOS) wordt ook ondersteund. HLI eenheid wordt gebruikt voor QA-exemplaar op de DR-site, terwijl de productiebewerkingen worden uitgevoerd vanaf de primaire site. QA-exemplaar op de DR-site is op het moment van DR-failover (of failover-test), offline gezet.

### <a name="architecture-diagram"></a>Architectuurdiagram  

![Single-knooppunt-met-dr-multipurpose.png](media/hana-supported-scenario/single-node-with-dr-multipurpose.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Geconfigureerd, maar niet wordt gebruikt |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Geconfigureerd, maar niet wordt gebruikt |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Geconfigureerd, maar niet wordt gebruikt |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Geconfigureerd, maar niet wordt gebruikt |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|**Op de primaire site**|
|/Hana/Shared/SID | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw |
|**Op de DR-site**|
|/Hana/Shared/SID | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/hana/shared/QA-SID | HANA-installatie voor QA-SID | 
|/hana/data/QA-SID/mnt00001 | Bestanden installeren voor QA-SID | 
|/hana/log/QA-SID/mnt00001 | Logboekbestanden installeren voor QA SID |
|/hana/logbackups/QA-SID | Logboeken voor SID-QA opnieuw |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Voor MCOS: Verdeling van volume grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database-grootten in het geheugen worden ondersteund met multisid-omgeving.
- Aan de DR: De volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Is vereist voor de installatie van HANA') voor de installatie van de HANA-exemplaar op de eenheid voor DR HLI voor productie. 
- Aan de DR: De gegevens, logbackups en gedeelde volumes (gemarkeerd als 'Storage-replicatie') worden gerepliceerd via momentopname van de productiesite. Deze volumes worden gekoppeld tijdens de failover-tijd. Lees voor meer informatie het document [failover noodherstelprocedure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) voor meer informatie. 
- Aan de DR: De gegevens, logbackups, log, gedeelde volumes voor QA (gemarkeerd als 'QA exemplaar installatie') zijn geconfigureerd voor de installatie van de QA-exemplaar.
- Opstartvolume voor **SKU-Type ik klasse** worden gerepliceerd naar de DR-knooppunt.

## <a name="5-hsr-with-stonith"></a>5. HSR met stonith instellen
 
Deze topologie ondersteunen twee knooppunten voor de configuratie van HANA System Replication (HSR). Deze configuratie wordt alleen ondersteund voor één HANA-instanties op een knooppunt. Middelen, MCOS scenario's worden niet ondersteund.

**Vanaf nu wordt deze architectuur alleen ondersteund voor SUSE-besturingssysteem.**


### <a name="architecture-diagram"></a>Architectuurdiagram  

![HSR met STONITH.png](media/hana-supported-scenario/HSR-with-STONITH.png)



### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Geconfigureerd, maar niet wordt gebruikt |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Gebruikt voor stonith instellen |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Geconfigureerd, maar niet wordt gebruikt |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Gebruikt voor stonith instellen |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|**Op het primaire knooppunt**|
|/Hana/Shared/SID | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw |
|**Op het tweede knooppunt**|
|/Hana/Shared/SID | HANA voor secundaire SID installeren | 
|/hana/data/SID/mnt00001 | Gegevensbestanden installeren voor secundaire SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor secundaire SID | 
|/Hana/logbackups/SID | Logboeken voor secundaire SID opnieuw |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Voor MCOS: Verdeling van volume grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database-grootten in het geheugen worden ondersteund met multisid-omgeving.
- STONITH INSTELLEN: Een SBD is geconfigureerd voor de installatie van de stonith instellen. Er is echter een gebruik van stonith instellen optioneel.


## <a name="6-hsr-with-dr"></a>6. HSR met herstel na Noodgevallen
 
Deze topologie ondersteunen twee knooppunten voor de configuratie van HANA System Replication (HSR). Zowel de normale en multipurpose DR wordt ondersteund. Deze configuraties worden alleen ondersteund voor één HANA-instanties op een knooppunt. Betekent MCOS scenario's worden niet ondersteund met deze configuraties.

In het diagram, multipurpose scenario wordt beschreven indien op de DR-site, HLI eenheid wordt gebruikt voor QA exemplaar tijdens productiebewerkingen worden uitgevoerd vanaf de primaire site. QA-exemplaar op de DR-site is op het moment van DR-failover (of failover-test), offline gezet. 



### <a name="architecture-diagram"></a>Architectuurdiagram  

![HSR met DR.png](media/hana-supported-scenario/HSR-with-DR.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Geconfigureerd, maar niet wordt gebruikt |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Gebruikt voor stonith instellen |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Geconfigureerd, maar niet wordt gebruikt |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Gebruikt voor stonith instellen |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|**Op het primaire knooppunt op de primaire site**|
|/Hana/Shared/SID | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw |
|**Op het tweede knooppunt op de primaire site**|
|/Hana/Shared/SID | HANA voor secundaire SID installeren | 
|/hana/data/SID/mnt00001 | Gegevensbestanden installeren voor secundaire SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor secundaire SID | 
|/Hana/logbackups/SID | Logboeken voor secundaire SID opnieuw |
|**Op de DR-site**|
|/Hana/Shared/SID | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/hana/shared/QA-SID | HANA-installatie voor QA-SID | 
|/hana/data/QA-SID/mnt00001 | Bestanden installeren voor QA-SID | 
|/hana/log/QA-SID/mnt00001 | Logboekbestanden installeren voor QA SID |
|/hana/logbackups/QA-SID | Logboeken voor SID-QA opnieuw |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Voor MCOS: Verdeling van volume grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database-grootten in het geheugen worden ondersteund met multisid-omgeving.
- STONITH INSTELLEN: Een SBD is geconfigureerd voor de installatie van de stonith instellen. Er is echter een gebruik van stonith instellen optioneel.
- Aan de DR: **Er zijn twee sets met opslagvolumes die zijn vereist** voor replicatie van primaire en secundaire knooppunt.
- Aan de DR: De volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Is vereist voor de installatie van HANA') voor de installatie van de HANA-exemplaar op de eenheid voor DR HLI voor productie. 
- Aan de DR: De gegevens, logbackups en gedeelde volumes (gemarkeerd als 'Storage-replicatie') worden gerepliceerd via momentopname van de productiesite. Deze volumes worden gekoppeld tijdens de failover-tijd. Lees voor meer informatie het document [failover noodherstelprocedure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) voor meer informatie. 
- Aan de DR: De gegevens, logbackups, log, gedeelde volumes voor QA (gemarkeerd als 'QA exemplaar installatie') zijn geconfigureerd voor de installatie van de QA-exemplaar.
- Opstartvolume voor **SKU-Type ik klasse** worden gerepliceerd naar de DR-knooppunt.


## <a name="7-host-auto-failover-11"></a>7. Host automatische failover (1 + 1)
 
Deze topologie biedt ondersteuning voor twee knooppunten in een host automatisch failover-configuratie. Er is een knooppunt met de master-/ werkrol en andere als een stand-by. **SAP biedt ondersteuning voor dit scenario alleen voor s/4 HANA.** Raadpleeg OSS-opmerking "[2408419 - SAP S/4HANA - ondersteuning voor meerdere knooppunten](https://launchpad.support.sap.com/#/notes/2408419)' voor meer informatie.



### <a name="architecture-diagram"></a>Architectuurdiagram  

![SCA](media/hana-supported-scenario/scaleup-with-standby.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Het knooppunt om communicatie tussen knooppunten te |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Geconfigureerd, maar niet wordt gebruikt |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Het knooppunt om communicatie tussen knooppunten te |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Geconfigureerd, maar niet wordt gebruikt |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|**Op de hoofd- en stand-by-knooppunten**|
|/ hana/gedeeld | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw |



### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- In stand-by: De volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Is vereist voor de installatie van HANA') voor de installatie van de HANA-exemplaar op de stand-by-eenheid.
 

## <a name="8-scale-out-with-standby"></a>8. Scale-out met stand-by
 
Deze topologie biedt ondersteuning voor meerdere knooppunten in een scale-out-configuratie. Er is een knooppunt met masterfunctie, een of meer knooppunten met werkrol en een of meer knooppunten als stand-by. Echter, kunnen er slechts één hoofdknooppunt op een willekeurig moment tijd wordt opgelost.


### <a name="architecture-diagram"></a>Architectuurdiagram  

![scaleout-nm-standby.png](media/hana-supported-scenario/scaleout-nm-standby.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Het knooppunt om communicatie tussen knooppunten te |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Geconfigureerd, maar niet wordt gebruikt |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Het knooppunt om communicatie tussen knooppunten te |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Geconfigureerd, maar niet wordt gebruikt |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|**Op de master-, werknemer- en stand-by-knooppunten**|
|/ hana/gedeeld | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw |


## <a name="9-scale-out-without-standby"></a>9. Scale-out zonder stand-by
 
Deze topologie biedt ondersteuning voor meerdere knooppunten in een scale-out-configuratie. Er is één knooppunt met masterfunctie en één of modus knooppunten met de werkrol. Echter, kunnen er slechts één hoofdknooppunt op een willekeurig moment tijd wordt opgelost.


### <a name="architecture-diagram"></a>Architectuurdiagram  

![scaleout-nm.png](media/hana-supported-scenario/scaleout-nm.png)


### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Het knooppunt om communicatie tussen knooppunten te |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Geconfigureerd, maar niet wordt gebruikt |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Het knooppunt om communicatie tussen knooppunten te |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Geconfigureerd, maar niet wordt gebruikt |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|**Op de hoofd- en worker-knooppunten**|
|/ hana/gedeeld | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw |


### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.

## <a name="10-scale-out-with-dr"></a>10. Scale-out met herstel na Noodgevallen
 
Deze topologie biedt ondersteuning voor meerdere knooppunten in een scale-out met een DR. Normale en multipurpose DR wordt ondersteund. In het diagram, zijn alleen de één doel DR wordt weergegeven. U kunt deze topologie met of zonder de stand-by-knooppunt op te vragen.


### <a name="architecture-diagram"></a>Architectuurdiagram  

![scaleout met dr.png](media/hana-supported-scenario/scaleout-with-dr.png)


### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces vooraf zijn geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | Naam van SUSE-besturingssysteem | Naam van RHEL-besturingssysteem | Use-case|
| --- | --- | --- | --- | --- |
| A | IK TYP | eth0.tenant | eno1.tenant | HLI-client |
| B | IK TYP | eth2.tenant | eno3.tenant | Het knooppunt om communicatie tussen knooppunten te |
| C | IK TYP | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | IK TYP | eth4.tenant | eno4.tenant | Geconfigureerd, maar niet wordt gebruikt |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | HLI-client |
| B | TYPE II | vlan<tenantNo+2> | team0.tenant+2 | Het knooppunt om communicatie tussen knooppunten te |
| C | TYPE II | vlan<tenantNo+1> | team0.tenant+1 | Knooppunt naar opslag |
| D | TYPE II | vlan<tenantNo+3> | team0.tenant+3 | Geconfigureerd, maar niet wordt gebruikt |

### <a name="storage"></a>Storage
De volgende quorumbron: vooraf zijn geconfigureerd:

| Mountpoint | Use-case | 
| --- | --- |
|**Op het primaire knooppunt**|
|/ hana/gedeeld | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw |
|**Op het knooppunt voor herstel na Noodgevallen**|
|/ hana/gedeeld | HANA installeren voor de productie-SID | 
|/hana/data/SID/mnt00001 | Bestanden installeren voor de productie-SID | 
|/hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 


### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
-  Aan de DR: De volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Is vereist voor de installatie van HANA') voor de installatie van de HANA-exemplaar op de eenheid voor DR HLI voor productie. 
- Aan de DR: De gegevens, logbackups en gedeelde volumes (gemarkeerd als 'Storage-replicatie') worden gerepliceerd via momentopname van de productiesite. Deze volumes worden gekoppeld tijdens de failover-tijd. Lees voor meer informatie het document [failover noodherstelprocedure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) voor meer informatie. 
- Opstartvolume voor **SKU-Type ik klasse** worden gerepliceerd naar de DR-knooppunt.


## <a name="next-steps"></a>Volgende stappen
- Raadpleeg [infrastructuur en connectiviteit](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity) voor HLI
- Raadpleeg [hoge beschikbaarheid en herstel na noodgevallen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) voor HLI

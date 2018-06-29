---
title: Ondersteunde scenario's voor SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Scenario's ondersteund en de details van de architectuur voor SAP HANA in Azure (grote exemplaren)
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
ms.date: 06/27/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8927b2a32956f73e75ac7b157ebad6bf6596ea88
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063626"
---
# <a name="supported-scenarios-for-hana-large-instances"></a>Ondersteunde scenario's voor grote HANA-exemplaren
Dit document beschrijft de ondersteunde scenario's samen met de details van de architectuur voor de HANA grote exemplaren (HLI).

>[!NOTE]
>Als uw vereiste scenario wordt hier niet vermeld, moet u contact op met Microsoft Service Management-team voor het evalueren van uw vereisten.
Voordat u doorgaat met de inrichting HLI-eenheid, het ontwerp met SAP of uw service-implementatiepartner te valideren.

## <a name="terms-and-definitions"></a>Termen en definities
We bekijken de termen en definities gebruikt in het document.

- SID: Systeemidentificatie voor HANA-systeem.
- HLI: Hana grote exemplaren.
- DR: Een disaster recovery site.
- Normaal DR: een systeeminstellingen met een speciale resource voor DR-doel alleen worden gebruikt.
- Multipurpose DR: Een systeem op DR-site die zijn geconfigureerd voor het gebruik van niet-productieomgeving samen met de productie-exemplaar dat is geconfigureerd om te gebruiken voor DR-gebeurtenis. 
- Één SID: Een systeem met één exemplaar is geïnstalleerd.
- Meerdere SID: Een systeem met meerdere exemplaren die zijn geconfigureerd. Ook wel een omgeving MCOS genoemd.


## <a name="overview"></a>Overzicht
De grote exemplaren HANA ondersteuning voor de verschillende soorten architecturen voor het uitvoeren van uw zakelijke vereisten. De volgende lijst bevat informatie over de scenario's en hun configuratie-informatie. 

Het architectuurontwerp van de afgeleide is uitsluitend vanuit het perspectief van de infrastructuur en SAP of partners voor uw implementatie moet te raadplegen voor de implementatie HANA. Als uw scenario's niet weergegeven worden, neem dan contact op met het Microsoft-accountteam om te bekijken van de architectuur en zijn afgeleid van een oplossing voor u.

**Deze architecturen volledig compatibel zijn met het ontwerp TDI (gegevensintegratie aangepast) en ondersteund door SAP.**

Dit document beschrijft de details van de twee onderdelen in elke ondersteunde architectuur:

- Ethernet
- Storage

### <a name="ethernet"></a>Ethernet

Elke server ingericht is vooraf geconfigureerd met de sets van ethernet. Hier volgen de details van de Ethernet-geconfigureerd op elke HLI eenheden.

- **Een**: dit wordt gebruikt voor/door de clienttoegang.
- **B**: dit wordt gebruikt voor de communicatie knooppunt naar. Dit is geconfigureerd op de servers (ongeacht de topologie aangevraagd), maar alleen voor de scale-out-scenario's worden gebruikt.
- **C**: deze interface wordt gebruikt voor het knooppunt voor de opslagconnectiviteit.
- **D**: deze interface wordt gebruikt voor het knooppunt voor iSCSI-apparaatverbinding voor STONITH setup. Deze interface is alleen geconfigureerd wanneer de installatie van de HSR wordt aangevraagd.  

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Knooppunt naar knooppunt |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | STONITH |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Knooppunt naar knooppunt |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | STONITH |

U de interfaces op basis van de topologie die is geconfigureerd op de eenheid HLI gebruiken. De interface "B" is bijvoorbeeld voor knooppunt naar communicatie, hetgeen nuttig is wanneer u een scale-out-topologie geconfigureerd hebt ingesteld. In het geval van een configuratie met één knooppunt omhoog schalen, is deze interface niet gebruikt. Bekijk uw vereiste scenario's (verderop in dit document) voor meer informatie over het gebruik van de interface. 

Indien nodig, kunt u extra NIC-kaarten op uw eigen definiëren. De configuratie op de bestaande NIC's kan echter niet worden gewijzigd.

>[!NOTE]
>Nog steeds misschien vindt u aanvullende interfaces die fysieke interfaces of de binding. U moet rekening houden met de bovengenoemde interfaces voor uw gebruikte aanvraag, rest kan worden genegeerd / of niet te worden getemperd met.

De verdeling van eenheden met twee IP-adressen toegewezen moet eruitzien als:

Ethernet "A" moet een IP-adres toegewezen valt buiten het bereik van groep met IP-adres dat u naar Microsoft verzonden. Dit IP-adres moet worden gebruikt voor het onderhouden van in/etc/hosts van het besturingssysteem.

Ethernet "B", moet een IP-adres toegewezen dat wordt gebruikt voor communicatie met NFS hebben. Daarom deze adressen komen **niet** moet worden behouden in etc/hosts om verkeer van de exemplaar-exemplaar binnen de tenant.

Een bladeconfiguratie met twee IP-adressen toegewezen is niet geschikt is voor implementatie gevallen van HANA System Replication of HANA scale-out. Als twee IP-adressen toegewezen alleen gelet en implementeren van een dergelijke configuratie, neem contact op met de SAP HANA op Azure Service Management ophalen van een derde IP-adres in een derde willen VLAN toegewezen. Voor grote exemplaar HANA eenheden met drie IP-adressen toegewezen op drie NIC-poorten, gelden de volgende informatie over het gebruiksregels:

- Ethernet "A" moet een IP-adres toegewezen valt buiten het bereik van groep met IP-adres dat u naar Microsoft verzonden. Dit IP-adres wordt daarom niet worden gebruikt voor het onderhouden van in/etc/hosts van het besturingssysteem.

- Ethernet "B", moet een IP-adres toegewezen dat wordt gebruikt voor communicatie met NFS opslag hebben. Dit type adressen moet daarom niet worden beheerd in etc/hosts.

- Ethernet 'C' mag uitsluitend worden gebruikt in etc/hosts voor de communicatie tussen de verschillende exemplaren worden bijgehouden. Deze adressen is ook de IP-adressen die moeten worden behouden in scale-out HANA configuraties als IP-adressen die HANA wordt gebruikt voor de configuratie tussen knooppunten.

- Ethernet "D" mag uitsluitend worden gebruikt voor toegang tot een apparaat STONITH voor pacemaker heeft. Dit is vereist wanneer u HANA System Replication (HSR) configureren en wilt automatische failover op het besturingssysteem met behulp van een apparaat op basis van SBD bereiken.


### <a name="storage"></a>Storage
Opslag is vooraf geconfigureerd op basis van de topologie die is aangevraagd. De grootten en koppelpunt afhankelijk van het aantal servers, SKU's en -topologie geconfigureerd. Bekijk uw vereiste scenario's (verderop in dit document) voor meer informatie. Als meer opslagruimte vereist is, kunt u deze met een TB verhoging kopen.

>[!NOTE]
>Het koppelpunt/usr/sap/<SID> is een symbolische koppeling naar het koppelpunt/hana/gedeeld.


## <a name="supported-scenarios"></a>Ondersteunde scenario 's

In de diagrammen architectuur worden de volgende notaties gebruikt voor afbeeldingen:

![Legends.PNG](media/hana-supported-scenario/Legends.PNG)

De volgende lijst bevat de ondersteunde scenario's:

1. Één knooppunt met een SID
2. Één knooppunt MCOS
3. Één knooppunt met DR (normaal)
4. Één knooppunt met DR (Multipurpose)
5. HSR met STONITH
6. HSR met DR (normale / Multipurpose) 
7. Host automatische failover (1 + 1) 
8. Uitschalen met stand-by
9. Zonder de stand-by uitbreiden
10. Uitschalen met DR



## <a name="1-single-node-with-one-sid"></a>1. Één knooppunt met een SID

Deze topologie biedt ondersteuning voor één knooppunt in een schaal van een SID-configuratie.

### <a name="architecture-diagram"></a>Architectuurdiagram  

![Single-knooppunt-aan-1-SID.png](media/hana-supported-scenario/Single-node-with-one-SID.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Geconfigureerde maar niet in gebruik |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Geconfigureerde maar niet in gebruik |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Geconfigureerde maar niet in gebruik |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Geconfigureerde maar niet in gebruik |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|/Hana/Shared/SID | HANA installeren | 
|/Hana/Data/SID/mnt00001 | Gegevensbestanden installeren | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren | 
|/Hana/logbackups/SID | Logboeken opnieuw uitvoeren |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.

## <a name="2-single-node-mcos"></a>2. Één knooppunt MCOS

Deze topologie biedt ondersteuning voor één knooppunt in een schaal van de configuratie met meerdere SID's.

### <a name="architecture-diagram"></a>Architectuurdiagram  

![Single-knooppunt-mcos.png](media/hana-supported-scenario/single-node-mcos.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Geconfigureerde maar niet in gebruik |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Geconfigureerde maar niet in gebruik |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Geconfigureerde maar niet in gebruik |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Geconfigureerde maar niet in gebruik |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|/Hana/Shared/SID1 | HANA installatie voor SID1 | 
|/Hana/Data/SID1/mnt00001 | Gegevensbestanden installeren voor SID1 | 
|/Hana/log/SID1/mnt00001 | Logboekbestanden installeren voor SID1 | 
|/Hana/logbackups/SID1 | Opnieuw uitvoeren van Logboeken voor SID1 |
|/Hana/Shared/SID2 | HANA installatie voor SID2 | 
|/Hana/Data/SID2/mnt00001 | Gegevensbestanden installeren voor SID2 | 
|/Hana/log/SID2/mnt00001 | Logboekbestanden installeren voor SID2 | 
|/Hana/logbackups/SID2 | Opnieuw uitvoeren van Logboeken voor SID2 |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Verdeling van volume-grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database groottes in het geheugen worden ondersteund met multisid omgeving.

## <a name="3-single-node-with-dr-normal"></a>3. Één knooppunt met DR (normaal)
 
Deze topologie biedt ondersteuning voor één knooppunt in een schaal van de configuratie met één of meerdere SID's met de replicatie op basis van opslag naar de DR-site voor een primaire-SID. In het diagram slechts één SID wordt beschreven op de primaire site, maar multisid (MCOS) wordt ook ondersteund.

### <a name="architecture-diagram"></a>Architectuurdiagram  

![Één knooppunt met dr.png](media/hana-supported-scenario/Single-node-with-dr.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Geconfigureerde maar niet in gebruik |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Geconfigureerde maar niet in gebruik |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Geconfigureerde maar niet in gebruik |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Geconfigureerde maar niet in gebruik |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|/Hana/Shared/SID | HANA installeren voor SID | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor SID | 
|/Hana/logbackups/SID | Opnieuw uitvoeren van Logboeken voor SID |


### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Voor MCOS: Verdeling van Volume-grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database groottes in het geheugen worden ondersteund met multisid omgeving.
- Op de DR: de volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Vereist voor de installatie van HANA') voor de productie HANA exemplaar installatie op de HLI DR-eenheid. 
- Op de DR: de gegevens, logbackups en gedeelde volumes (gemarkeerd als 'Opslagreplicatie') worden gerepliceerd via momentopname van de productiesite. Deze volumes worden gekoppeld tijdens de failover-tijd. Raadpleeg [failover noodherstelprocedure](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) voor meer informatie.
- Opstartvolume voor **SKU Type ik klasse** worden gerepliceerd naar DR-knooppunt.


## <a name="4-single-node-with-dr-multipurpose"></a>4. Één knooppunt met DR (Multipurpose)
 
Deze topologie biedt ondersteuning voor één knooppunt in een schaal van de configuratie met één of meerdere SID's met de replicatie op basis van opslag naar de DR-site voor een primaire-SID. In het diagram slechts één SID wordt beschreven op de primaire site, maar multisid (MCOS) wordt ook ondersteund. HLI eenheid wordt gebruikt voor QA-exemplaar op de site DR terwijl productiebewerkingen worden uitgevoerd vanaf de primaire site. QA-exemplaar op DR-site is op het moment van failover DR (of failover testen) offline gezet.

### <a name="architecture-diagram"></a>Architectuurdiagram  

![Single-knooppunt-met-dr-multipurpose.png](media/hana-supported-scenario/single-node-with-dr-multipurpose.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Geconfigureerde maar niet in gebruik |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Geconfigureerde maar niet in gebruik |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Geconfigureerde maar niet in gebruik |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Geconfigureerde maar niet in gebruik |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|**Op de primaire site**|
|/Hana/Shared/SID | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw uitvoeren |
|**Op de DR-site**|
|/Hana/Shared/SID | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/Shared/QA-SID | HANA installeren voor QA-SID | 
|/Hana/Data/QA-SID/mnt00001 | Bestanden installeren voor QA-SID | 
|/Hana/log/QA-SID/mnt00001 | Logboekbestanden installeren voor QA SID |
|/Hana/logbackups/QA-SID | Opnieuw uitvoeren van Logboeken voor QA-SID |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Voor MCOS: Verdeling van Volume-grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database groottes in het geheugen worden ondersteund met multisid omgeving.
- Op de DR: de volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Vereist voor de installatie van HANA') voor de productie HANA exemplaar installatie op de HLI DR-eenheid. 
- Op de DR: de gegevens, logbackups en gedeelde volumes (gemarkeerd als 'Opslagreplicatie') worden gerepliceerd via momentopname van de productiesite. Deze volumes worden gekoppeld tijdens de failover-tijd. Raadpleeg [failover noodherstelprocedure](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) voor meer informatie. 
- Op de DR: de gegevens, logbackups, logboek, gedeelde volumes voor QA (gemarkeerd als 'QA exemplaar installatie') zijn geconfigureerd voor de installatie van de QA-exemplaar.
- Opstartvolume voor **SKU Type ik klasse** worden gerepliceerd naar DR-knooppunt.

## <a name="5-hsr-with-stonith"></a>5. HSR met STONITH
 
Deze topologie ondersteuning voor twee knooppunten voor de configuratie van de HANA System Replication (HSR). 

**Vanaf nu wordt deze architectuur alleen ondersteund voor SUSE-besturingssysteem.**


### <a name="architecture-diagram"></a>Architectuurdiagram  

![HSR met STONITH.png](media/hana-supported-scenario/HSR-with-STONITH.png)



### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Geconfigureerde maar niet in gebruik |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Gebruikt voor STONITH |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Geconfigureerde maar niet in gebruik |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Gebruikt voor STONITH |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|**Op het primaire knooppunt**|
|/Hana/Shared/SID | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw uitvoeren |
|**Op het tweede knooppunt**|
|/Hana/Shared/SID | HANA voor secundaire SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor secundaire SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor secundaire SID | 
|/Hana/logbackups/SID | Opnieuw uitvoeren van Logboeken voor secundaire SID |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Voor MCOS: Verdeling van Volume-grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database groottes in het geheugen worden ondersteund met multisid omgeving.
- STONITH: Een SBD is geconfigureerd voor de installatie van de STONITH. Er is echter een gebruik van STONITH optioneel.


## <a name="6-hsr-with-dr"></a>6. HSR met DR
 
Deze topologie ondersteuning voor twee knooppunten voor de configuratie van de HANA System Replication (HSR). Zowel de DR normaal en meerdere doeleinden wordt ondersteund. 

In het diagram multipurpose scenario wordt beschreven indien op de DR-site, HLI eenheid wordt gebruikt voor QA exemplaar terwijl productiebewerkingen worden uitgevoerd vanaf de primaire site. QA-exemplaar op DR-site is op het moment van failover DR (of failover testen) offline gezet. 



### <a name="architecture-diagram"></a>Architectuurdiagram  

![HSR met DR.png](media/hana-supported-scenario/HSR-with-DR.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Geconfigureerde maar niet in gebruik |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Gebruikt voor STONITH |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Geconfigureerde maar niet in gebruik |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Gebruikt voor STONITH |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|**Op het primaire knooppunt op de primaire site**|
|/Hana/Shared/SID | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw uitvoeren |
|**Op het tweede knooppunt op de primaire site**|
|/Hana/Shared/SID | HANA voor secundaire SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor secundaire SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor secundaire SID | 
|/Hana/logbackups/SID | Opnieuw uitvoeren van Logboeken voor secundaire SID |
|**Op de DR-site**|
|/Hana/Shared/SID | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/Shared/QA-SID | HANA installeren voor QA-SID | 
|/Hana/Data/QA-SID/mnt00001 | Bestanden installeren voor QA-SID | 
|/Hana/log/QA-SID/mnt00001 | Logboekbestanden installeren voor QA SID |
|/Hana/logbackups/QA-SID | Opnieuw uitvoeren van Logboeken voor QA-SID |

### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Voor MCOS: Verdeling van Volume-grootte is gebaseerd op de grootte van de database in het geheugen. Raadpleeg de [overzicht en architectuur](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) sectie voor meer informatie over welke database groottes in het geheugen worden ondersteund met multisid omgeving.
- STONITH: Een SBD is geconfigureerd voor de installatie van de STONITH. Er is echter een gebruik van STONITH optioneel.
- Op de DR: **zijn twee sets met opslagvolumes vereist** voor replicatie van primaire en secundaire knooppunt.
- Op de DR: de volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Vereist voor de installatie van HANA') voor de productie HANA exemplaar installatie op de HLI DR-eenheid. 
- Op de DR: de gegevens, logbackups en gedeelde volumes (gemarkeerd als 'Opslagreplicatie') worden gerepliceerd via momentopname van de productiesite. Deze volumes worden gekoppeld tijdens de failover-tijd. Raadpleeg [failover noodherstelprocedure](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) voor meer informatie. 
- Op de DR: de gegevens, logbackups, logboek, gedeelde volumes voor QA (gemarkeerd als 'QA exemplaar installatie') zijn geconfigureerd voor de installatie van de QA-exemplaar.
- Opstartvolume voor **SKU Type ik klasse** worden gerepliceerd naar DR-knooppunt.


## <a name="7-host-auto-failover-11"></a>7. Host automatische failover (1 + 1)
 
Deze topologie ondersteunt twee knooppunten in een host automatische failover-configuratie. Er is een knooppunt met master/werkrol en andere als een stand-by. **Dit scenario ondersteunt SAP alleen voor S/4 HANA.** Raadpleeg OSS-Opmerking '[2408419 - SAP S/4HANA - ondersteuning voor meerdere knooppunten](https://launchpad.support.sap.com/#/notes/2408419)' voor meer informatie.



### <a name="architecture-diagram"></a>Architectuurdiagram  

![SCA](media/hana-supported-scenario/scaleup-with-standby.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Knooppunt voor communicatie tussen knooppunten |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Geconfigureerde maar niet in gebruik |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Knooppunt voor communicatie tussen knooppunten |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Geconfigureerde maar niet in gebruik |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|**Op de master en stand-by-knooppunten**|
|/ hana/gedeeld | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw uitvoeren |



### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
- Stand-by: de volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Vereist voor de installatie van HANA') voor de installatie HANA exemplaar op de stand-by-eenheid.
 

## <a name="8-scale-out-with-standby"></a>8. Uitschalen met stand-by
 
Deze topologie biedt ondersteuning voor meerdere knooppunten in een scale-out-configuratie. Er is een knooppunt met masterfunctie, een of meer knooppunten met werkrol en een of meer knooppunten als stand-by. Echter, kunnen er slechts één hoofdknooppunt op elk gewenst moment tijd.


### <a name="architecture-diagram"></a>Architectuurdiagram  

![scaleout-nm-standby.png](media/hana-supported-scenario/scaleout-nm-standby.png)

### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Knooppunt voor communicatie tussen knooppunten |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Geconfigureerde maar niet in gebruik |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Knooppunt voor communicatie tussen knooppunten |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Geconfigureerde maar niet in gebruik |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|**Op de master, worker en stand-by-knooppunten**|
|/ hana/gedeeld | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw uitvoeren |


## <a name="9-scale-out-without-standby"></a>9. Zonder de stand-by uitbreiden
 
Deze topologie biedt ondersteuning voor meerdere knooppunten in een scale-out-configuratie. Er is een knooppunt met master-functie en één of knooppunten van de modus met werkrol. Echter, kunnen er slechts één hoofdknooppunt op elk gewenst moment tijd.


### <a name="architecture-diagram"></a>Architectuurdiagram  

![scaleout nm.png](media/hana-supported-scenario/scaleout-nm.png)


### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Knooppunt voor communicatie tussen knooppunten |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Geconfigureerde maar niet in gebruik |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Knooppunt voor communicatie tussen knooppunten |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Geconfigureerde maar niet in gebruik |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|**Op de hoofd- en worker-knooppunten**|
|/ hana/gedeeld | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw uitvoeren |


### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.

## <a name="10-scale-out-with-dr"></a>10. Uitschalen met DR
 
Deze topologie biedt ondersteuning voor meerdere knooppunten in een scale-out met een Noodherstel. Normaal en multipurpose DR wordt ondersteund. In het diagram wordt alleen de één doeleinde DR afgebeeld. U kunt deze topologie met of zonder het stand-by-knooppunt aanvragen.


### <a name="architecture-diagram"></a>Architectuurdiagram  

![scaleout met dr.png](media/hana-supported-scenario/scaleout-with-dr.png)


### <a name="ethernet"></a>Ethernet
De volgende netwerkinterfaces zijn vooraf geconfigureerd:

| LOGISCHE INTERFACES NIC | SKU-TYPE | De naam met SUSE OS | De naam met RHEL OS | Gebruiksvoorbeeld|
| --- | --- | --- | --- | --- |
| A | TYPE I | eth0.tenant | eno1.tenant | Client HLI |
| B | TYPE I | eth2.tenant | eno3.tenant | Knooppunt voor communicatie tussen knooppunten |
| C | TYPE I | eth1.tenant | eno2.tenant | Knooppunt naar opslag |
| D | TYPE I | eth4.tenant | eno4.tenant | Geconfigureerde maar niet in gebruik |
| A | TYPE II | VLAN<tenantNo> | team0.tenant | Client HLI |
| B | TYPE II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Knooppunt voor communicatie tussen knooppunten |
| C | TYPE II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Knooppunt naar opslag |
| D | TYPE II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Geconfigureerde maar niet in gebruik |

### <a name="storage"></a>Storage
De volgende quorumbron: zijn vooraf geconfigureerd:

| Koppelpunt | Gebruiksvoorbeeld | 
| --- | --- |
|**Op het primaire knooppunt**|
|/ hana/gedeeld | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 
|/Hana/logbackups/SID | Logboeken voor productie SID opnieuw uitvoeren |
|**Op het knooppunt voor Noodherstel**|
|/ hana/gedeeld | HANA voor productie SID installeren | 
|/Hana/Data/SID/mnt00001 | Bestanden installeren voor productie SID | 
|/Hana/log/SID/mnt00001 | Logboekbestanden installeren voor productie SID | 


### <a name="key-considerations"></a>Belangrijkste overwegingen
- /usr/SAP/SID is een symbolische koppeling naar /hana/shared/SID.
-  Op de DR: de volumes en stel de volgende parameter zijn geconfigureerd (gemarkeerd als 'Vereist voor de installatie van HANA') voor de productie HANA exemplaar installatie op de HLI DR-eenheid. 
- Op de DR: de gegevens, logbackups en gedeelde volumes (gemarkeerd als 'Opslagreplicatie') worden gerepliceerd via momentopname van de productiesite. Deze volumes worden gekoppeld tijdens de failover-tijd. Raadpleeg [failover noodherstelprocedure](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) voor meer informatie. 
- Opstartvolume voor **SKU Type ik klasse** worden gerepliceerd naar DR-knooppunt.


## <a name="next-steps"></a>Volgende stappen
- Raadpleeg [infrastructuur en connectiviteit](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity) voor HLI
- Raadpleeg [hoge beschikbaarheid en herstel na noodgevallen](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) voor HLI
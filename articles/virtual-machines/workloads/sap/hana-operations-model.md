---
title: Operationeel model van SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Operationeel model van SAP HANA op Azure (grote instanties).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 36a648e2d46cce96a8ff663f45ccf45326898a84
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417003"
---
# <a name="operations-model-and-responsibilities"></a>Operationeel model en -verantwoordelijkheden

De service die wordt geleverd met SAP HANA op Azure (grote instanties) is afgestemd op Azure IaaS-services. U krijgt een exemplaar van een HANA grote instantie met een geïnstalleerd besturingssysteem dat is geoptimaliseerd voor SAP HANA. Met Azure IaaS VM's, de meeste van de taken van de beperking van het besturingssysteem, extra software te installeren, HANA installeren, die het besturingssysteem en HANA en bijwerken van het besturingssysteem en HANA is uw verantwoordelijkheid. Microsoft biedt geen verplichten om updates van het besturingssysteem of updates voor HANA op u.

![De verantwoordelijkheden van SAP HANA op Azure (grote instanties)](./media/hana-overview-architecture/image2-responsibilities.png)

In het diagram wordt weergegeven, is SAP HANA op Azure (grote instanties) een multitenant IaaS bieden. Voor het overgrote deel is de verdeling van de verantwoordelijkheid van de grens van de OS-infrastructuur. Microsoft is verantwoordelijk voor alle aspecten van de service onder de lijn van het besturingssysteem. U bent verantwoordelijk voor alle aspecten van de service boven de regel. Het besturingssysteem is uw verantwoordelijkheid. U kunt echter ook doorgaan met de meest recente on-premises-methoden die u gebruikmaken van mogelijk voor naleving, beveiliging, beheer van toepassingen, basis en OS-beheer. De systemen worden weergegeven alsof ze zich in uw netwerk in alle opzichten.

Deze service is geoptimaliseerd voor SAP HANA, zodat er gebieden waar u nodig hebt om te werken met Microsoft voor het gebruik van de mogelijkheden van de onderliggende infrastructuur voor de beste resultaten.

De volgende lijst vindt u meer details over op elk van de lagen en uw verantwoordelijkheden:

**Netwerken**: Alle interne netwerken voor het uitvoeren van SAP HANA grote instantie-stempel. Uw eigen verantwoordelijkheid omvat toegang tot opslag, verbinding tussen de exemplaren (voor scale-out en andere functies), verbinding met het landschap van en verbinding met Azure waar de SAP-toepassingslaag wordt gehost in VM's. Dit omvat ook WAN-verbinding tussen Azure-datacenters voor disaster recovery doeleinden-replicatie. Alle netwerken worden gepartitioneerd op basis van de tenant en kwaliteit van de service toegepast hebben.

**Opslag**: De gevirtualiseerde gepartitioneerd opslag voor alle volumes die nodig zijn voor de SAP HANA-servers, evenals voor momentopnamen. 

**Servers**: De toegewezen fysieke servers om uit te voeren van de SAP HANA-databases wordt toegewezen aan tenants. De servers van het Type ik klasse van SKU's zijn hardware geabstraheerd. Configuratie van de server is met de volgende typen servers, die worden verzameld en onderhouden in profielen die kunnen worden verplaatst van een fysieke hardware voor een andere fysieke hardware. Dergelijke een (handmatig) verplaatsen van een profiel door bewerkingen kan een en ander Azure-service herstel worden vergeleken. De servers van de SKU's Type II-klasse bieden niet een dergelijke mogelijkheid.

**SDDC**: De management-software die wordt gebruikt voor het beheren van gegevens wordt gecentreerd als software gedefinieerde entiteiten. Hierdoor kan Microsoft tot resources van schaal, beschikbaarheid en prestaties van toepassingen.

**O/S**: Het besturingssysteem kiest u (SUSE Linux of Red Hat Linux) die wordt uitgevoerd op de servers. De installatiekopieën van het besturingssysteem die u worden geleverd met zijn opgegeven door de leverancier van de afzonderlijke Linux naar Microsoft voor het uitvoeren van SAP HANA. U moet een abonnement met de Linux-leverancier voor de specifieke geoptimaliseerd voor SAP HANA-installatiekopie hebben. U bent verantwoordelijk voor het registreren van de afbeeldingen met de leverancier van het besturingssysteem. 

Vanaf het moment van oproepdienstoverdracht door Microsoft bent u verantwoordelijk voor alle verdere patchen van de Linux-besturingssysteem. Deze patches bevat aanvullende pakketten die mogelijk noodzakelijk voor een geslaagde installatie van de SAP HANA en die niet zijn opgenomen door de specifieke Linux-leverancier in hun SAP HANA geoptimaliseerd voor installatiekopieën van het besturingssysteem. (Zie de documentatie voor installatie van HANA en SAP-opmerkingen van SAP voor meer informatie.) 

U bent verantwoordelijk voor het patchen van het besturingssysteem door de storing of optimalisatie van het besturingssysteem en de stuurprogramma's relatief ten opzichte van de specifieke serverhardware. Ook bent u verantwoordelijk voor het beveiligings- of functioneel patchen van het besturingssysteem. 

Uw eigen verantwoordelijkheid bevat ook bewaken en plannen van capaciteit van:

- Gebruik van CPU-resources.
- Het geheugengebruik.
- Volumes op schijven die betrekking hebben op vrije ruimte, IOPS en latentie.
- Volume netwerkverkeer tussen HANA grote instantie en de SAP-toepassingslaag.

De onderliggende infrastructuur van HANA grote instantie biedt functionaliteit voor back-up en herstel van het volume met het besturingssysteem. Met deze functionaliteit is ook uw eigen verantwoordelijkheid.

**Middleware**: De SAP HANA-exemplaar, voornamelijk. Bent u verantwoordelijk voor beheer, bewerkingen en bewaking. De opgegeven functionaliteit kunt u storage-momentopnamen gebruiken voor back-up en herstel en disaster recovery-doeleinden. Deze mogelijkheden worden geleverd door de infrastructuur. Uw verantwoordelijkheden ook het ontwerpen van hoge beschikbaarheid of herstel na noodgeval met deze mogelijkheden gebruik te maken van deze en te bewaken om te bepalen of storage-momentopnamen is uitgevoerd.

**Gegevens**: Uw gegevens die worden beheerd door SAP HANA en andere gegevens zoals back-ups van bestanden die zijn opgeslagen op volumes of het bestand deelt. Uw verantwoordelijkheden zijn onder andere vrije schijfruimte controleren en beheren van de inhoud op de volumes. Ook bent u verantwoordelijk voor het bewaken van de voltooiing van uitvoering van back-ups van volumes op schijven en storage-momentopnamen. Geslaagde uitvoering van de replicatie van gegevens naar disaster recovery sites is de verantwoordelijkheid van Microsoft.

**Toepassingen:** De toepassingsexemplaren SAP of, in het geval van niet-SAP-toepassingen, het niveau van de toepassing van deze toepassingen. Uw verantwoordelijkheden zijn onder andere implementatie, beheer, bewerkingen en bewaking van deze toepassingen. U bent verantwoordelijk voor het plannen van capaciteit van CPU-verbruik van resources, geheugenverbruik, gebruik van Azure-opslag en netwerkbandbreedte in virtuele netwerken. Ook bent u verantwoordelijk voor de capaciteitsplanning voor bronverbruik van virtuele netwerken met SAP HANA op Azure (grote instanties).

**WAN's**: De verbindingen die u tot stand van on-premises naar Azure-implementaties voor werkbelastingen brengen. Alle klanten met HANA grote instantie Azure ExpressRoute gebruiken voor verbindingen. Deze verbinding maakt geen deel uit van de SAP HANA op Azure (grote instanties)-oplossing. U bent verantwoordelijk voor het instellen van deze verbinding.

**Archief**: Misschien wilt u het archiveren van kopieën van gegevens met behulp van uw eigen methoden in de storage-accounts. Archiveren is vereist, beheer, naleving, kosten en bewerkingen. U bent verantwoordelijk voor het genereren archive-exemplaren en back-ups op Azure en opslaat in een compatibele manier.

Zie de [SLA voor SAP HANA op Azure (grote instanties)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/).

**Volgende stappen**
- Raadpleeg [SAP HANA (grote instanties)-architectuur op Azure](hana-architecture.md)

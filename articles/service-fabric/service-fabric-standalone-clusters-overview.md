---
title: Zelfstandige Service Fabric-clusters overzicht | Microsoft Docs
description: Service Fabric-clusters worden uitgevoerd op Windows Server en Linux, wat betekent dat u zult kunnen implementeren en host-Service Fabric-toepassingen overal kunt u Windows Server of Linux uitvoeren.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/01/2019
ms.author: dekapur
ms.openlocfilehash: 5d3421c4d48a6bf8416a774c4b4e5e7852a83f06
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967338"
---
# <a name="overview-of-service-fabric-standalone-clusters"></a>Overzicht van Service Fabric zelfstandige clusters

Een Service Fabric-cluster is een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een clusterknooppunt genoemd. Clusters kunnen worden geschaald naar duizenden knooppunten. Als u nieuwe knooppunten aan het cluster toevoegt, rebalances Service Fabric de partitiereplica's service en de exemplaren in het grotere aantal knooppunten. Algemene verbetert de prestaties van toepassingen en conflicten voor toegang tot het geheugen vermindert. Als de knooppunten in het cluster niet efficiënt worden gebruikt, kunt u het aantal knooppunten in het cluster verminderen. Service Fabric rebalances opnieuw de partitiereplica's en exemplaren voor het kleinere aantal knooppunten om beter gebruik van de hardware op elk knooppunt.

Het knooppunttype bepaalt de grootte, het aantal en de eigenschappen voor een set knooppunten in het cluster. Elk knooppunttype kan dan onafhankelijk omhoog of omlaag worden geschaald, verschillende open poorten bevatten en diverse capaciteitsstatistieken hebben. Knooppunttypen worden gebruikt voor het definiëren van rollen voor een set clusterknooppunten, zoals 'front-end' of 'back-end'. Uw cluster kan meer dan één knooppunttype hebben, maar voor productieclusters moet het primaire knooppunttype ten minste vijf VM's bevatten (of ten minste drie VM's voor testclusters). [Service Fabric-systeemservices](service-fabric-technical-overview.md#system-services) worden op de knooppunten van het primaire knooppunttype geplaatst.

Het proces voor het maken van een Service Fabric-cluster on-premises is vergelijkbaar met het proces voor het maken van een cluster in elke cloud van uw keuze met een set van virtuele machines. De eerste stappen voor het inrichten van de virtuele machines worden beheerst door de cloudprovider of on-premises-omgeving die u gebruikt. Zodra u een set van virtuele machines met de netwerkverbinding tussen beide zijn ingeschakeld hebt, klikt u vervolgens de stappen voor het instellen van het Service Fabric-pakket, de clusterinstellingen bewerken en maken van het cluster worden uitgevoerd en management scripts zijn identiek. Dit zorgt ervoor dat uw kennis en ervaring van het besturingssysteem en het beheren van Service Fabric-clusters worden overgedragen wanneer u nieuwe hostingomgevingen als doel.

## <a name="cluster-security"></a>Clusterbeveiliging
Een Service Fabric-cluster is een resource waarvan u eigenaar bent.  Het is uw verantwoordelijkheid voor het beveiligen van uw clusters om te voorkomen dat onbevoegde gebruikers verbinding maken met deze. Een beveiligd cluster is vooral belangrijk wanneer u bij het uitvoeren van productieworkloads op het cluster.

### <a name="node-to-node-security"></a>Beveiliging van knooppunt-naar-knooppunt
Beveiliging van knooppunt-naar-knooppunt voor beveiliging van communicatie tussen de virtuele machines of computers in een cluster. In dit scenario voor beveiliging zorgt ervoor dat alleen computers die zijn gemachtigd om lid van het cluster te kunnen deelnemen die als host fungeert voor toepassingen en services in het cluster. Service Fabric maakt gebruik van X.509-certificaten voor het beveiligen van een cluster en beveiligingsfuncties van de toepassing opgeven.  Een clustercertificaat is vereist voor het beveiligen van verkeer het cluster en het cluster en de server-verificatie.  Zelf-ondertekend-certificaten kunnen worden gebruikt voor testclusters, maar een certificaat van een vertrouwde certificeringsinstantie moet worden gebruikt voor het beveiligen van productieclusters.

Windows-beveiliging kan ook worden ingeschakeld voor een zelfstandige cluster van Windows. Als u Windows Server 2012 R2 en Windows Active Directory hebt, raden wij aan dat u Windows-beveiliging met beheerde serviceaccounts voor groepen. Gebruik anders Windows-beveiliging met Windows-accounts.

Lees voor meer informatie, [beveiliging van knooppunt-naar-knooppunt](service-fabric-cluster-security.md#node-to-node-security)

### <a name="client-to-node-security"></a>Beveiliging van de client-naar-knooppunt
Beveiliging van de client-naar-knooppunt clients te verifiëren en helpt veilige communicatie tussen een client en de afzonderlijke knooppunten in het cluster. Dit type beveiliging zorgt ervoor dat alleen gemachtigde gebruikers toegang krijgen tot het cluster en de toepassingen die zijn geïmplementeerd op het cluster. Clients worden uniek geïdentificeerd door een van de beveiligingsreferenties van de X.509-certificaat. Een willekeurig aantal optionele clientcertificaten kan worden gebruikt om beheerder of gebruiker clients met het cluster te verifiëren.

Naast de clientcertificaten, worden Azure Active Directory ook geconfigureerd om te verifiëren van clients met het cluster.

Lees voor meer informatie, [beveiliging van de Client-naar-knooppunt](service-fabric-cluster-security.md#client-to-node-security)

### <a name="role-based-access-control-rbac"></a>RBAC (op rollen gebaseerd toegangsbeheer)
Service Fabric biedt ook ondersteuning voor toegangsbeheer om te beperken van toegang tot bepaalde bewerkingen voor een cluster voor verschillende groepen gebruikers. Zo kunt u het cluster beter te beveiligen. Twee access control-typen worden ondersteund voor clients die verbinding met een cluster maken: Beheerdersrol en gebruikersrol.  

Lees voor meer informatie, [Role-Based Access Control (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac).

## <a name="scaling"></a>Schalen

Toepassingsbehoeften veranderen verloop van tijd. Mogelijk moet u verhogen clusterbronnen om te voldoen aan een grotere werkbelasting of netwerk verkeer of verkleinen van clusterbronnen wanneer de vraag afneemt. Na het maken van een Service Fabric-cluster, kunt u het cluster horizontaal schalen (het aantal knooppunten wijzigen) of verticaal (de resources van de knooppunten wijzigen). U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster. Als het cluster wordt geschaald, wordt uw toepassingen automatisch ook schalen.

Lees voor meer informatie, [zelfstandige clusters schalen](service-fabric-cluster-scaling-standalone.md).

## <a name="upgrading"></a>Upgraden

Een zelfstandige cluster is een resource die u helemaal zelf. U bent verantwoordelijk voor het patchen van het onderliggende besturingssysteem en het initialiseren van de fabric-upgrades. U kunt instellen van uw cluster runtime automatische upgrades, ontvangen wanneer een nieuwe versie door Microsoft worden uitgegeven of kies een ondersteunde runtime-versie die u wilt. Naast het fabric-upgrades, kunt u ook vullen van het besturingssysteem en configuratie van het cluster, zoals certificaten of toepassingspoorten bijwerken. 

Lees voor meer informatie, [upgrade zelfstandige clusters](service-fabric-cluster-upgrade-standalone.md).

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen
U zijn kunt maken van clusters op virtuele machines of op computers met deze besturingssystemen (Linux is nog niet ondersteund):

* Windows Server 2012 R2
* Windows Server 2016 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [beveiligen](service-fabric-cluster-security.md), [schalen](service-fabric-cluster-scaling-standalone.md), en [upgraden](service-fabric-cluster-upgrade-standalone.md) zelfstandige clusters.

Meer informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md).
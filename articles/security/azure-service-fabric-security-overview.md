---
title: Overzicht van Azure Service Fabric-beveiliging | Microsoft Docs
description: Dit artikel bevat een overzicht van Azure Service Fabric-beveiliging.
services: security
documentationcenter: na
author: unifycloud
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 629b6fba9ced5fa2ccf22f473fe25c87d1cc4818
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436807"
---
# <a name="azure-service-fabric-security-overview"></a>Overzicht van Azure Service Fabric-beveiliging
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) is een platform voor gedistribueerde systemen waarmee u eenvoudig kunnen worden verpakt, implementeren en beheren van schaalbare en betrouwbare microservices. Service Fabric is afhankelijk van de uitdagingen van ontwikkelen en beheren van cloudtoepassingen. Ontwikkelaars en beheerders kunnen complexe infrastructuurproblemen vermijden en zich richten op het implementeren van essentiële en intensieve werkbelastingen die schaalbare en betrouwbare zijn.

In dit artikel wordt een overzicht van de beveiligingsoverwegingen voor de implementatie van een Service Fabric.

## <a name="secure-your-cluster"></a>Een cluster beveiligen
Azure Service Fabric coördineert services in een computercluster. Clusters moeten worden beveiligd om te voorkomen dat onbevoegde gebruikers verbinding kunnen maken, met name wanneer ze bij het uitvoeren van productieworkloads. Hoewel het mogelijk te maken van een niet-beveiligd cluster, kan hierdoor anonieme gebruikers verbinding maken met het cluster (als-beheereindpunten met het openbare internet beschikbaar worden gemaakt).

Voor clusters die worden uitgevoerd als zelfstandig of in Azure, zijn twee scenario's de beveiliging van knooppunt-naar-knooppunt en client-naar-knooppunt-beveiliging. U kunt verschillende technologieën gebruiken voor het implementeren van deze scenario's.

### <a name="node-to-node-security"></a>Beveiliging van knooppunt-naar-knooppunt
Beveiliging van knooppunt-naar-knooppunt is van toepassing op de communicatie tussen de virtuele machines of machines in een cluster. Met de beveiliging van knooppunt-naar-knooppunt kan kunnen alleen computers die zijn gemachtigd om lid van het cluster te deelnemen aan die als host fungeert voor toepassingen en services in het cluster.

Clusters die worden uitgevoerd op Azure of een zelfstandige clusters die worden uitgevoerd op Windows kunnen beide gebruiken [beveiliging van het certificaat](https://msdn.microsoft.com/library/ff649801.aspx) of [Windows security](https://msdn.microsoft.com/library/ff649396.aspx) voor Windows Server-machines.

#### <a name="node-to-node-certificate-security"></a>Knooppunt-naar-knooppunt Certificaatbeveiliging

Service Fabric maakt gebruik van x.509-certificaten die u opgeeft wanneer u een cluster maakt. Zie voor een kort overzicht van wat deze certificaten zijn en hoe u kunt verkrijgen of ze maken [werken met certificaten](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

U configureren Certificaatbeveiliging bij het maken van het cluster via Azure portal, Azure Resource Manager-sjablonen of een zelfstandige JSON-sjabloon. U kunt een primaire certificaat en een optionele secundaire certificaat dat wordt gebruikt voor het certificaat rollovers opgeven. De primaire en secundaire certificaten die u opgeeft moet verschillen van de beheerder client en de alleen-lezen-clientcertificaten gebruikt die u opgeeft voor [client-naar-knooppunt security](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>Beveiliging van de client-naar-knooppunt
U configureren client-naar-knooppunt beveiliging met behulp van de client-id's. Als u wilt de vertrouwensrelatie tussen een client en een cluster, moet u het cluster als u wilt weten welke client identiteiten die kunnen worden vertrouwd.

Service Fabric ondersteunt twee toegangstypen voor clients die zijn verbonden met een Service Fabric-cluster:

-   **Beheerder**: volledige toegang tot de mogelijkheden voor Computerbeheer, met inbegrip van lezen/schrijven-mogelijkheden.
-   **Gebruiker**: alleen leestoegang tot de mogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.

Met behulp van toegangsbeheer, kunnen clusterbeheerders toegang beperken tot bepaalde typen bewerkingen voor een cluster. Dit maakt het cluster beter te beveiligen.

#### <a name="client-to-node-certificate-security"></a>Client-naar-knooppunt Certificaatbeveiliging

U configureren client-naar-knooppunt Certificaatbeveiliging wanneer u een cluster via Azure portal, Resource Manager-sjablonen of een zelfstandige JSON-sjabloon maakt. U moet een certificaat voor de Beheerclient en/of een clientcertificaat voor gebruikers opgeven. Zorg ervoor dat deze certificaten verschillen van de primaire en secundaire certificaten die u voor de beveiliging van knooppunt-naar-knooppunt opgeeft.

Clients die verbinding met het cluster maken met behulp van het certificaat van de beheerder hebben volledige toegang tot de mogelijkheden voor beheer. Clients die verbinding met het cluster maken met behulp van het clientcertificaat voor alleen-lezen-gebruiker hebben alleen leestoegang tot de mogelijkheden voor beheer. Met andere woorden, worden deze certificaten gebruikt voor op rollen gebaseerd toegangsbeheer (RBAC).

Zie voor informatie over het Certificaatbeveiliging configureren in een cluster, [een cluster instellen met behulp van een Azure Resource Manager-sjabloon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

#### <a name="client-to-node-azure-active-directory-security"></a>Client-naar-knooppunt Azure Active Directory-beveiligingsgroep

Clusters die worden uitgevoerd op Azure kunnen ook toegang tot de eindpunten voor beheer beveiligen met behulp van Azure Active Directory (Azure AD). Zie voor meer informatie over het maken van de benodigde Azure Active Directory-artefacten, hoe u deze in te vullen tijdens het maken en hoe u verbinding maken met deze clusters [een cluster instellen met behulp van een Azure Resource Manager-sjabloon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Azure AD kan organisaties (bekend als tenants) voor het beheren van toegang tot toepassingen. Er zijn toepassingen met een web gebaseerde aanmelding bij de gebruikersinterface en toepassingen met een systeemeigen client-ervaring.

Een Service Fabric-cluster biedt verschillende toegangspunten bij de management-functionaliteit, met inbegrip van het web gebaseerde Service Fabric Explorer en Visual Studio. Als gevolg hiervan, het maken van twee Azure AD-toepassingen voor het beheren van toegang tot het cluster: een webtoepassing en een systeemeigen toepassing.

Voor clusters met Azure, wordt u aangeraden dat u Azure AD-beveiligingsgroep hebt gebruikt voor verificatie van clients en certificaten voor de beveiliging van knooppunt-naar-knooppunt.

Voor zelfstandige WindowsServer-clusters met Windows Server 2012 R2 en Active Directory, raden wij aan dat u Windows-beveiliging met een groep beheerde serviceaccounts (gmsa's). Gebruik anders Windows-beveiliging met Windows-accounts.

## <a name="understand-monitoring-and-diagnostics-in-service-fabric"></a>Informatie over controle en diagnostische gegevens in Service Fabric
[Controle en diagnose](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) essentieel zijn voor het ontwikkelen, testen en implementeren van toepassingen en services in elke omgeving. Service Fabric-oplossingen werken het beste wanneer u controle en diagnostische gegevens om ervoor te zorgen dat toepassingen en services werken zoals verwacht in een lokale ontwikkelingsomgeving of in de productieomgeving implementeren.

Vanuit het beveiligingsoogpunt zijn de belangrijkste doelstellingen van controle en diagnostische gegevens:

-   Detecteren en onderzoeken van problemen met hardware en de infrastructuur die kunnen worden veroorzaakt door een beveiligingsgebeurtenis.
-   Software- en app-problemen die mogelijk een indicator compromise (IoC) gedetecteerd.
-   Krijg inzicht in resourceverbruik om te voorkomen dat onbedoelde DOS-aanval.

De werkstroom van bewaking en diagnostiek bestaat uit drie stappen:

1.  **Gebeurtenisgeneratie**: genereren van gebeurtenissen bevat gebeurtenissen (Logboeken, traceringen, aangepaste gebeurtenissen) op het niveau van de infrastructuur (cluster) en de toepassing/service-niveau. Meer informatie over [gebeurtenissen op infrastructuurniveau](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) en [gebeurtenissen op toepassingsniveau](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) om te begrijpen wat wordt geleverd en hoe u verder instrumentatie toevoegen.

2.  **Gebeurtenis-aggregatie**: gegenereerde gebeurtenissen moeten worden verzameld en samengevoegd voordat ze kunnen worden weergegeven. Wordt doorgaans aangeraden [Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (vergelijkbaar met logboekverzameling op basis van een agent) of [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (in-process logboekverzameling).

3.  **Analyse**: gebeurtenissen moeten worden zinvoller visualiseren en toegankelijk is in een indeling, om toe te staan voor analyse en weergeven. Er zijn verschillende platforms voor de analyse en visualisatie van gegevens voor controle en diagnostische gegevens. Het is raadzaam [Azure Log Analytics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) en [Azure Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) omdat ze probleemloos worden geïntegreerd met Service Fabric.

U kunt ook [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) voor het bewaken van veel van de Azure-resources waarop een Service Fabric-cluster is gebouwd.

Een watchdog is een afzonderlijke service die u kunt bekijken van status en laden in services en de gezondheid van het rapport voor alles wat in de hiërarchie van de health-model. Met behulp van een watchdog kan u helpen voorkomen van fouten die niet kunnen worden gedetecteerd op basis van de weergave van een service. 

Watchdogs zijn ook een goede plaats code kunt hosten die corrigerende acties zonder tussenkomst van de gebruiker uitvoert. Een voorbeeld is opschonen van de logboekbestanden in de opslag op bepaalde tijdsintervallen. U vindt een voorbeeld watchdog-service-implementatie op [voorbeeld van Azure Service Fabric-watchdog](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="secure-communication-by-using-certificates"></a>Veilige communicatie met behulp van certificaten
Certificaten bij het beveiligen de communicatie tussen de verschillende knooppunten van uw zelfstandige Windows-cluster. U kunt ook clients die verbinding met dit cluster maakt verifiëren met behulp van X.509-certificaten. Dit zorgt ervoor dat alleen geautoriseerde gebruikers toegang het cluster tot kunnen. U wordt aangeraden een certificaat op het cluster in te schakelen wanneer u deze maakt.

Digitale x.509-certificaten worden meestal gebruikt voor verificatie van clients en servers. Ze worden ook gebruikt voor het versleutelen en digitaal ondertekenen van berichten.

De volgende tabel vindt u de certificaten die u nodig hebt van de cluster-instellingen:

|Instelling voor gegevens |Beschrijving|
|-------------------------------|-----------|
|ClusterCertificate|    Dit certificaat is vereist om de communicatie tussen de knooppunten op een cluster te beveiligen. U kunt twee clustercertificaten: een primair certificaat en een secundaire voor een upgrade.|
|ServerCertificate| Dit certificaat wordt weergegeven op de client als er wordt geprobeerd verbinding maken met dit cluster. U kunt twee servercertificaten gebruiken: een primair certificaat en een secundaire voor een upgrade.|
|ClientCertificateThumbprints|  Dit is een set certificaten worden geïnstalleerd op de geverifieerde clients.|
|ClientCertificateCommonNames|  Dit is de algemene naam van het eerste certificaat voor CertificateCommonName. CertificateIssuerThumbprint is de vingerafdruk voor de verlener van dit certificaat.|
|ReverseProxyCertificate|   Dit is een optionele certificaat dat u opgeven kunt als u wilt beveiligen uw [omgekeerde proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Zie voor meer informatie over het beveiligen van certificaten, [een zelfstandige cluster beveiligen op Windows met behulp van X.509-certificaten](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="understand-role-based-access-control"></a>Informatie over op rollen gebaseerd toegangsbeheer
U opgeven de functies van de client-beheerder en gebruiker op het moment van de cluster te maken door afzonderlijke identiteiten (met inbegrip van certificaten) voor elk. Zie voor meer informatie over de standaardinstellingen voor het beheer van toegang en het wijzigen van de standaardinstellingen, [rollen gebaseerd toegangsbeheer voor Service Fabric-clients](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-clusters-by-using-windows-security"></a>Zelfstandige clusters beveiligen met behulp van Windows-beveiliging
Om te voorkomen dat onbevoegde toegang tot een Service Fabric-cluster, moet u het cluster beveiligen. Beveiliging is vooral belangrijk wanneer het cluster wordt uitgevoerd voor werkbelastingen voor productie. U kunt beveiliging van knooppunt-naar-knooppunt en client-naar-knooppunt configureren met behulp van Windows-beveiliging in het bestand ClusterConfig.JSON.

Wanneer Service Fabric moet worden uitgevoerd onder een gMSA, beveiliging van knooppunt-naar-knooppunt te configureren door [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security). Voor het bouwen van vertrouwensrelaties tussen knooppunten, moet u doen op de hoogte van elkaar.

Als u gebruiken een computergroep in Active Directory-domein wilt, moet u knooppunt-naar-knooppunt beveiliging door in te stellen ClusterIdentity configureren. Zie voor meer informatie, [maken van een Machinegroep in Active Directory](https://msdn.microsoft.com/library/aa545347).

U configureren client-naar-knooppunt beveiliging met behulp van ClientIdentities. U moet het cluster zo dat die herkent welke client-id's kunnen worden vertrouwd. U kunt vertrouwen op twee manieren:

-   Geef de groep Domeingebruikers die verbinding kunnen maken.
-   Geef de domein-knooppunt-gebruikers die verbinding kunnen maken.

## <a name="configure-application-security-in-service-fabric"></a>Toepassingsbeveiliging configureren in Service Fabric
### <a name="manage-secrets-in-service-fabric-applications"></a>Geheimen in Service Fabric-toepassingen beheren
Geheimen kunnen gevoelige informatie, zoals storage-verbindingsreeksen, wachtwoorden, of andere waarden die niet moeten worden verwerkt als tekst zonder opmaak zijn.

U kunt [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) voor het beheren van sleutels en geheimen. Echter, het gebruik van geheimen in een toepassing niet afhankelijk van een bepaalde cloudplatform. U kunt toepassingen naar een cluster dat wordt gehost overal implementeren. Er zijn vier belangrijke stappen in deze stroom:

1.  Een versleutelingscertificaat gegevens ophalen.
2.  Het certificaat installeren op uw cluster.
3.  Versleutelen van geheime waarden bij het implementeren van een toepassing met het certificaat en injecteren van een service Settings.xml-configuratiebestand.
4.  Versleutelde waarden buiten het Settings.xml door ze te ontsleutelen met de dezelfde versleutelingscertificaat gelezen.

Zie voor meer informatie, [geheimen in Service Fabric-toepassingen beheren](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-an-application"></a>Beveiligingsbeleid configureren voor een toepassing
Met behulp van Azure Service Fabric-beveiliging, kunt u beveiligde toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts. Service Fabric-beveiliging helpt ook bij de resources die toepassingen op het moment van implementatie onder de gebruikersaccounts, bijvoorbeeld gebruiken, bestanden, mappen en certificaten beveiligen. Hierdoor actieve toepassingen, zelfs in een gedeelde omgeving, beter te beveiligen.

Taken voor het configureren van beleidsregels voor veiligheid zijn onder andere:

-   Het beleid voor een toegangspunt voor service-instellingen configureren
-   PowerShell-opdrachten starten vanaf een setup-toegangspunt
-   Met behulp van de console-omleiding voor lokale foutopsporing
-   Configureren van een beleid voor servicepakketten code
-   Toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten

## <a name="secure-communication-for-services"></a>Beveiligde communicatie voor services
Security is een van de belangrijkste aspecten van de communicatie. Het framework van de toepassing betrouwbare Services biedt een aantal vooraf gedefinieerde communicatie-stacks en hulpprogramma's die u gebruiken kunt om beveiliging te verbeteren. Zie voor meer informatie, [beveiligde communicatie van de service voor externe toegang voor een service](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication).

## <a name="next-steps"></a>Volgende stappen
- Raadpleeg voor algemene informatie over de clusterbeveiliging [maken van een Service Fabric-cluster met behulp van Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) en [maken van een Service Fabric-cluster met behulp van de Azure-portal](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Zie voor meer informatie over beveiliging in Service Fabric-clusters, [Service Fabric-clusterbeveiligingsscenario's](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).

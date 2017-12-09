---
title: Overzicht van Azure service fabric-beveiliging | Microsoft Docs
description: Dit artikel bevat een overzicht van Azure Service Fabric-beveiliging.
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: 64717da922701aabd27e15a67e8da1b0acb30b77
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-service-fabric-security-overview"></a>Overzicht van Azure Service Fabric-beveiliging
[Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) is een platform voor gedistribueerde systemen waarmee u gemakkelijk verpakken, implementeren en beheren van schaalbare en betrouwbare microservices. Service Fabric heeft betrekking op de aanzienlijke uitdagingen bij het ontwikkelen en beheren van cloudtoepassingen. Ontwikkelaars en beheerders kunnen complexe infrastructuurproblemen voorkomen en zich concentreren op het implementeren van bedrijfsspecifieke, veeleisende werkbelastingen die schaalbaar, betrouwbaar en beheerbaar zijn.

Dit overzicht van Azure Service Fabric-beveiliging artikel is gericht op de volgende gebieden:

-   Uw cluster beveiligen
-   Informatie over controle en diagnostische gegevens
-   Maken van de meer beveiligde omgevingen met behulp van certificaten
-   Met behulp van op rollen gebaseerde toegangsbeheer (RBAC)
-   Clusters beveiligen met behulp van Windows-beveiliging
-   Toepassingsbeveiliging configureren in Service Fabric
-   Beveiligen van communicatie voor services in Azure Service Fabric 

## <a name="secure-your-cluster"></a>Een cluster beveiligen
Azure Service Fabric ingedeeld services in een cluster van machines. Clusters moeten worden beveiligd om te voorkomen dat onbevoegde gebruikers verbinding kunnen maken, vooral wanneer er productie-workloads worden uitgevoerd. Hoewel het mogelijk om een niet-beveiligde cluster te maken, is dit mogelijk anonieme gebruikers verbinding kunnen maken (als eindpunten voor beheer met het openbare internet worden getoond).

Deze sectie biedt een overzicht van de beveiliging scenario's voor clusters die worden uitgevoerd als zelfstandig product of op Azure. Hierin worden ook de verschillende technologieën die worden gebruikt voor het implementeren van de scenario's. De cluster security scenario's:

-   Knooppunt naar beveiliging
-   Beveiliging van de client-naar-knooppunt

### <a name="node-to-node-security"></a>Knooppunt naar beveiliging
Knooppunt naar beveiliging beveiligt de communicatie tussen de virtuele machines of machines in een cluster. Met knooppunten voor beveiliging, kunnen alleen de computers die zijn gemachtigd om lid van het cluster te deelnemen die als host fungeert voor toepassingen en services in het cluster.

Clusters die worden uitgevoerd op Azure of zelfstandige clusters die worden uitgevoerd op Windows gebruikt een [beveiliging van het certificaat](https://msdn.microsoft.com/library/ff649801.aspx) of [Windows-beveiliging](https://msdn.microsoft.com/library/ff649396.aspx) voor Windows Server-machines.

**Beveiliging van een knooppunt naar certificate begrijpen**

Service Fabric maakt gebruik van x.509-certificaten die u opgeeft wanneer u een cluster maakt. Zie voor een snel overzicht van wat deze certificaten zijn en hoe u kunt verkrijgen of ze maken [werken met certificaten](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/working-with-certificates).

U kunt Certificaatbeveiliging configureren wanneer u het cluster, hetzij via de Azure-portal, Azure Resource Manager-sjablonen of een zelfstandige JSON-sjabloon maakt. U kunt een primaire certificaat en een optionele secundaire certificaat dat wordt gebruikt voor het certificaat rollovers opgeven. De primaire en secundaire certificaten die u opgeeft moet anders zijn dan de Beheerclient en alleen-lezen-clientcertificaten die u opgeeft voor [clientknooppunt beveiliging](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).

### <a name="client-to-node-security"></a>Beveiliging van de client-naar-knooppunt
U configureren clientknooppunt beveiliging met behulp van identiteiten van de client. Als u wilt een vertrouwensrelatie tussen een client en een cluster, moet u het cluster om te weten welke client identiteiten die kunnen worden vertrouwd. Dit kan op twee verschillende manieren doen:

-   Geef de groep Domeingebruikers die verbinding kunnen maken. 
-   Geef de domein-knooppunt gebruikers die verbinding kunnen maken.

Service Fabric ondersteunt twee verschillende toegangsrechten besturingselementtypen voor clients die zijn verbonden met een Service Fabric-cluster:

-   Beheerder
-   Gebruiker

Met behulp van toegangsbeheer kunnen clusterbeheerders toegang beperken tot bepaalde soorten bewerkingen voor een cluster. Hierdoor kunt u het cluster beter te beveiligen.

 Beheerders hebben volledige toegang tot de beheerfuncties (inclusief lezen/schrijven-mogelijkheden). Gebruikers hebben standaard alleen leestoegang tot beheermogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.

**Certificaatbeveiliging van de client naar het knooppunt begrijpen**

U configureren de beveiliging van client-naar-node certificate wanneer u een cluster met de Azure-portal, Resource Manager-sjablonen of een zelfstandige JSON-sjabloon maakt. U moet een clientcertificaat voor de beheerder en/of een clientcertificaat voor gebruikers opgeven. 

De client en de gebruiker beheerclientcertificaten die u opgeeft moet verschillen van de primaire en secundaire certificaten die u voor een knooppunt naar beveiliging opgeeft.

Clients die verbinding met het cluster met behulp van de admin-certificaat hebben volledige toegang tot de beheerfuncties. Clients die verbinding met het cluster met behulp van het clientcertificaat voor de gebruiker alleen-lezen hebben alleen leestoegang tot de beheerfuncties. Met andere woorden, worden deze certificaten gebruikt voor RBAC.

Zie voor meer informatie over het configureren van Certificaatbeveiliging in een cluster, [een cluster met behulp van een Azure Resource Manager-sjabloon instellen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

**Beveiliging van de client naar het knooppunt Azure Active Directory op Azure begrijpen**

Clusters die worden uitgevoerd op Azure kunnen ook toegang tot de beheer-eindpunten beveiligen met behulp van Azure Active Directory (Azure AD). Zie voor meer informatie over het maken van de benodigde Azure Active Directory-artefacten, hoe deze in te vullen tijdens het maken van het cluster en verbinding maken met deze clusters [een cluster met behulp van een Azure Resource Manager-sjabloon instellen](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Azure AD kan organisaties (bekend als tenants) voor het beheren van de gebruikerstoegang tot toepassingen. Er zijn toepassingen met een webconsole-in de gebruikersinterface en toepassingen met een native Clientervaring.

Een Service Fabric-cluster biedt verschillende toegangspunten naar de beheerfunctionaliteit, met inbegrip van het web gebaseerde Service Fabric Explorer en Visual Studio. Daardoor hebt u twee Azure AD-toepassingen toegang tot het cluster: een webtoepassing en een systeemeigen toepassing.

Azure-clusters, wordt u aangeraden u Azure AD-beveiligingsgroep gebruiken om clients en certificaten voor knooppunt naar beveiliging te verifiëren.

Voor zelfstandige WindowsServer-clusters met Windows Server 2012 R2 en Active Directory, wordt u aangeraden dat u Windows-beveiliging met serviceaccounts voor een groep beheerd.  Gebruik anders Windows-beveiliging met Windows-accounts.

## <a name="understand-monitoring-and-diagnostics-in-azure-service-fabric"></a>Controle en diagnostische gegevens in Azure Service Fabric begrijpen
[Controle en diagnostische gegevens](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) cruciaal zijn voor het ontwikkelen, testen en implementeren van toepassingen en services in elke omgeving. Service Fabric-oplossingen werken het beste wanneer u controle en diagnostische gegevens om ervoor te zorgen dat toepassingen en services werken zoals verwacht in een lokale ontwikkelingsomgeving of in de productieomgeving implementeren.

Vanuit het beveiligingsoogpunt van zijn de belangrijkste doelstellingen van controle en diagnostische gegevens:

-   Detecteren en onderzoeken van hardware- en infrastructuur voor problemen die kunnen worden veroorzaakt door een beveiligingsgebeurtenis.
-   Software- en app-problemen die een indicator van inbreuk (IoC) kunnen worden gedetecteerd.
-   Begrijpen brongebruik om te voorkomen dat onbedoeld DOS-aanval.

De algemene werkstroom van controle en diagnostische gegevens bestaat uit drie stappen:

-   **Genereren van gebeurtenis:** genereren van gebeurtenis bevat gebeurtenissen (Logboeken, traceringen, aangepaste gebeurtenissen) op de infrastructuur (cluster) en de toepassing/service-niveau. Lees meer over [niveau van de infrastructuur gebeurtenissen](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-infra) en [gebeurtenissen op toepassingsniveau](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) om te begrijpen wat is opgegeven en het toevoegen van meer instrumentation.

-   **Aggregatie van gebeurtenis:** gegenereerde gebeurtenissen moeten worden verzameld en geaggregeerd voordat ze kunnen worden weergegeven. Doorgaans aangeraden [Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) (vergelijkbaar met logboekverzameling op basis van een agent) of [EventFlow](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow) (in-process logboekgegevens verzameld).

-   **Analyse:** gebeurtenissen moeten worden gevisualiseerde en toegankelijk zijn in sommige indeling, om toe te staan voor analyse en weergeven. Er zijn verschillende platforms voor de analyse- en visualisatie van controle en diagnostische gegevens. De twee die wij adviseren [Operations Management Suite](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-oms) en [Azure Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights) vanwege de goede integratie met Service Fabric.

U kunt ook [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) voor het bewaken van veel van de Azure-resources waarop een Service Fabric-cluster is gebouwd.

Een watchdog is een afzonderlijke service dat health, laden op services en health rapporteren voor alles in het model health hiërarchie kunt bekijken. Met behulp van een watchdog, kunt u voorkomen dat op fouten die niet kunnen worden gedetecteerd op basis van de weergave van een service. 

Watchdogs zijn ook een goede plaats naar host-code die corrigerende acties zonder tussenkomst van de gebruiker (bijvoorbeeld het opschonen van logboekbestanden in de opslag op bepaalde tijdsintervallen) uitvoert. U vindt een steekproef watchdog service-implementatie op [Azure Service Fabric watchdog voorbeeld](https://azure.microsoft.com/resources/samples/service-fabric-watchdog-service/).

## <a name="understand-how-to-secure-communication-by-using-certificates"></a>Begrijpen hoe om communicatie te beveiligen met behulp van certificaten
Certificaten kunnen u beveiligen de communicatie tussen de verschillende knooppunten van uw zelfstandige Windows-cluster. U kunt ook clients die zijn verbonden met dit cluster verifiëren met behulp van X.509-certificaten. Dit zorgt ervoor dat alleen geautoriseerde gebruikers toegang het cluster tot kunnen. U wordt aangeraden een certificaat op het cluster in te schakelen wanneer u dit hebt gemaakt.

### <a name="x509-certificates-and-service-fabric"></a>X.509-certificaten en Service Fabric
Digitale x.509-certificaten worden meestal gebruikt voor verificatie van clients en servers. Ze worden ook gebruikt voor het versleutelen en digitaal ondertekenen van berichten.

De volgende tabel vindt u de certificaten die u nodig hebt van uw cluster-instellingen:

|Certificaat informatie instelling |Beschrijving|
|-------------------------------|-----------|
|ClusterCertificate|    Dit certificaat is vereist om de communicatie tussen de knooppunten op een cluster te beveiligen. U kunt twee verschillende certificaten: een primaire certificaat en een secundaire voor een upgrade.|
|ServerCertificate| Dit certificaat wordt geverifieerd op de client als er wordt geprobeerd verbinding maken met dit cluster. U kunt twee verschillende servercertificaten: een primaire certificaat en een secundaire voor een upgrade.|
|ClientCertificateThumbprints|  Dit is een set van certificaten op de geverifieerde clients worden geïnstalleerd.|
|ClientCertificateCommonNames|  Dit is de algemene naam van het certificaat van de eerste client voor CertificateCommonName. CertificateIssuerThumbprint is de vingerafdruk voor de verlener van dit certificaat.|
|ReverseProxyCertificate|   Dit is een optioneel certificaat dat kan worden opgegeven voor het beveiligen van uw [omgekeerde proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).|

Zie voor meer informatie over het beveiligen van certificaten [een zelfstandige cluster op Windows met behulp van X.509-certificaten beveiligen](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security).

## <a name="understand-role-based-access-control"></a>Toegangsbeheer op basis van rollen begrijpen
Toegangsbeheer kan de Clusterbeheerder om te beperken van toegang tot bepaalde clusterbewerkingen voor verschillende groepen gebruikers, waardoor het cluster beter te beveiligen. Twee verschillende access controletypen worden ondersteund voor clients die zijn verbonden met een cluster: 

- Beheerdersrol
- gebruikersrol

Beheerders hebben volledige toegang tot de beheerfuncties (inclusief lezen/schrijven-mogelijkheden). Gebruikers hebben standaard alleen leestoegang tot beheermogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.

U opgeven de functies van de client beheerder en gebruiker op het moment van cluster maken door afzonderlijke identiteiten (met inbegrip van certificaten) voor elk. Zie voor meer informatie over de standaardinstellingen voor het beheer van toegang en het wijzigen van de standaardinstellingen, [toegangsbeheer op basis van rollen voor Service Fabric-clients](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

## <a name="secure-standalone-cluster-by-using-windows-security"></a>Zelfstandige cluster beveiligen met behulp van Windows-beveiliging
Om te voorkomen dat onbevoegde toegang tot een Service Fabric-cluster, moet u het cluster te beveiligen. Beveiliging is vooral belangrijk bij het cluster wordt uitgevoerd voor productie-workloads. Wordt beschreven hoe u de beveiliging van client-naar-knooppunt en knooppunt naar configureren met behulp van Windows-beveiliging in het bestand ClusterConfig.JSON.

**Windows-beveiliging configureren met behulp van gMSA**

Wanneer het Service Fabric moet worden uitgevoerd onder de gMSA, u knooppunt naar beveiliging configureren door [ClustergMSAIdentity](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security). Als u wilt maken van vertrouwensrelaties tussen knooppunten, moeten ze worden aangebracht op de hoogte van elkaar.

U configureren clientknooppunt beveiliging met behulp van ClientIdentities. Als u wilt een vertrouwensrelatie tussen een client en het cluster, moet u het cluster herkend welke client-id's kunnen worden vertrouwd.

**Windows-beveiliging configureren met behulp van een Machinegroep**

Als u gebruiken een Machinegroep in Active Directory-domein wilt, wordt het knooppunt naar beveiliging configureren door ClusterIdentity. Zie voor meer informatie [maken van een Machinegroep in Active Directory](https://msdn.microsoft.com/library/aa545347).

U configureren clientknooppunt beveiliging met behulp van ClientIdentities. Als u wilt een vertrouwensrelatie tussen een client en het cluster, moet u het cluster herkend door de client-identiteiten die kan worden vertrouwd door het cluster configureren. U kunt een vertrouwensrelatie in twee verschillende manieren:

-   Geef de groep Domeingebruikers die verbinding kunnen maken.
-   Geef de domein-knooppunt gebruikers die verbinding kunnen maken.

## <a name="configure-application-security-in-service-fabric"></a>Toepassingsbeveiliging configureren in Service Fabric
### <a name="manage-secrets-in-service-fabric-applications"></a>Geheimen in Service Fabric-toepassingen beheren
Deze methode kan worden beheerd geheimen in een Service Fabric-toepassing. Geheimen mag gevoelige informatie, zoals de storage-verbindingsreeksen, wachtwoorden of andere waarden die niet moeten worden behandeld als tekst zonder opmaak.

Deze benadering wordt [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) voor het beheren van sleutels en geheimen. Met behulp van geheimen in een toepassing is echter cloud platform-networkdirect. Dit betekent dat toepassingen kunnen worden geïmplementeerd naar een cluster die overal wordt gehost. Er zijn vier hoofdstappen in deze overdracht:

-   Verkrijgen van een certificaat voor het uitwisselen van gegevens.
-   Installeer het certificaat op het cluster.
-   Versleutelen geheime waarden bij het implementeren van een toepassing met het certificaat en een service Settings.xml configuratiebestand injecteren.
-   Versleutelde waarden buiten Settings.xml door ze te ontsleutelen met hetzelfde certificaat uitwisselen gelezen.

>[!NOTE]
>Meer informatie over [geheimen in Service Fabric-toepassingen beheren](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="configure-security-policies-for-your-application"></a>Beveiligingsbeleid configureren voor uw toepassing
Met behulp van Azure Service Fabric-beveiliging, kunt u beveiligde toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts. Service Fabric-beveiliging kunt u ook de resources die worden gebruikt door toepassingen op het moment van implementatie onder de gebruikersaccounts--bijvoorbeeld, bestanden, mappen en certificaten beveiligen. Hierdoor waarop toepassingen worden uitgevoerd, zelfs in een gedeelde gehoste omgeving beter te beveiligen.

De stappen omvatten:

-   Het beleid voor een toegangspunt voor service-instellingen configureren.
-   PowerShell-opdrachten starten vanuit een setup-toegangspunt.
-   Via de console voor het opsporen van lokale.
-   Een beleid voor servicepakketten code te configureren.
-   Een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten toe te wijzen.

## <a name="secure-communication-for-services-in-azure-service-fabric-security"></a>Veilige communicatie voor services in Azure Service Fabric-beveiliging
Beveiliging is een van de belangrijkste aspecten van de communicatie. Het framework Reliable Services biedt enkele vooraf gedefinieerde communicatie stacks en hulpprogramma's die kunnen worden gebruikt om beveiliging te verbeteren.

-   [Beveiligen van een service als u service voor externe toegang](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication)
-   [Beveiligen van een service wanneer u een WCF-communicatie-stack](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-secure-communication#help-secure-a-service-when-youre-using-a-wcf-based-communication-stack)

## <a name="next-steps"></a>Volgende stappen
- Raadpleeg voor algemene informatie over de clusterbeveiliging van de [Service Fabric-cluster maken met behulp van Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) en [Azure-portal](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-portal).
- Zie voor meer informatie over clusterbeveiliging in Service Fabric, [Service Fabric-clusterbeveiliging](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security).

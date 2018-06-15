---
title: Meer informatie over Azure Service Fabric-Toepassingsbeveiliging | Microsoft Docs
description: Een overzicht van hoe u veilig microservices toepassingen uitvoeren in de Service-infrastructuur. Ontdek hoe u services en opstarten script onder verschillende beveiligingsaccounts uitvoeren, verifiëren en autoriseren van gebruikers, beheren toepassing geheimen, beveiligde communicatie van de service, gebruik van een API-gateway en een voor beveiligde toepassingsgegevens in rust.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/16/2018
ms.author: ryanwi
ms.openlocfilehash: fa6d46186ad833b68e60c24f742d210b7845759a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207907"
---
# <a name="service-fabric-application-and-service-security"></a>Service Fabric-toepassing en Servicebeveiliging
Een architectuur microservices kunt brengen [veel voordelen](service-fabric-overview-microservices.md). Het beheren van de beveiliging van microservices, is echter een challenge en anders dan traditionele monolithische toepassingen beveiliging beheren. 

De toepassing wordt doorgaans uitgevoerd op een of meer servers in een netwerk met een monoliet en is het eenvoudiger om de blootgestelde poorten en API's en IP-adres te identificeren. Er is vaak een perimeternetwerk of grens en een database te beveiligen. Als dat systeem vanwege een inbreuk op de beveiliging of een aanval is aangetast, is het waarschijnlijk dat alles in het systeem beschikbaar voor de aanvaller zal zijn. Het systeem is met microservices, hoe complexer.  Services zijn gedecentraliseerde en verdeeld over veel hosts en host host te migreren.  Met de juiste beveiliging u een bevoegdheden die een aanvaller kan ophalen en de hoeveelheid gegevens die beschikbaar zijn in een één-aanval beperken door één service schendingen veroorzaken.  Communicatie is geen interne, maar er gebeurt via een netwerk, en er zijn veel blootgestelde poorten en interacties tussen services. Weten wat deze service-bewerkingen zijn en wanneer ze gebeuren is essentieel voor de Toepassingsbeveiliging van uw.

In dit artikel is niet een handleiding voor microservices beveiliging, zijn er veel resources beschikbaar online, maar hierin wordt beschreven hoe verschillende aspecten van de beveiliging kunnen worden uitgevoerd in Service Fabric.

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
Vaak is het nodig zijn voor resources en API's die worden weergegeven door een service moeten worden beperkt tot bepaalde vertrouwde gebruikers of -clients. Verificatie is het proces van het op betrouwbare wijze vaststelling identiteit van een gebruiker.  Autorisatie is het proces dat kan API's of services beschikbaar zijn voor sommige gebruikers, maar andere niet geverifieerd.

### <a name="authentication"></a>Verificatie
De eerste stap bij de vertrouwensrelatie voor een API-niveau beslissingen is verificatie. Verificatie is het proces van het op betrouwbare wijze vaststelling identiteit van een gebruiker.  In scenario's voor microservice, verificatie, wordt doorgaans afgehandeld Centraal. Als u een API-Gateway gebruikt, kunt u [verificatie offload](/azure/architecture/patterns/gateway-offloading) naar de gateway. Als u deze methode gebruikt, ervoor zorgen dat de afzonderlijke services rechtstreeks (zonder de API-Gateway) kunnen niet worden bereikt als extra beveiliging aanwezig is om berichten te verifiëren of ze afkomstig zijn van de gateway of niet.

Als services kunnen rechtstreeks worden geopend, zoals een verificatieservice Azure Active Directory of een specifieke verificatie microservice fungeert als een beveiligingstokenservice (STS) kan worden gebruikt om gebruikers te verifiëren. Vertrouwensrelatie beslissingen worden gedeeld tussen services met beveiligingstokens of cookies. 

Voor ASP.NET Core, de belangrijkste mechanisme voor [verifiëren van gebruikers](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/) is het lidmaatschap van ASP.NET Core identiteit systeem. ASP.NET Core identiteit slaat gebruikersgegevens (inclusief aanmelden, rollen en claims) in een gegevensopslag die is geconfigureerd door de ontwikkelaar. ASP.NET Core identiteit biedt ondersteuning voor verificatie met twee factoren.  Externe verificatieproviders worden ook ondersteund, zodat gebruikers zich aanmelden kunnen bij het gebruik van bestaande verificatieprocessen van providers, zoals Microsoft, Google, Facebook of Twitter. 

### <a name="authorization"></a>Autorisatie
Na verificatie services moet gebruikerstoegang verlenen of kunt vaststellen wat een gebruiker is kunnen doen. Dit proces kan een service maken API's beschikbaar zijn voor sommige geverifieerde gebruikers, maar niet voor alle. Autorisatie is een rechthoekige en onafhankelijk van verificatie, het proces is van de vaststelling die een gebruiker is. Verificatie kan een of meer identiteiten voor de huidige gebruiker maken.

[ASP.NET Core autorisatie](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/authorization-net-microservices-web-applications) kan worden gedaan op basis van rollen voor gebruikers of op basis van een aangepast beleid, waaronder het inspecteren van claims of andere methodiek.

## <a name="restrict-and-secure-access-using-an-api-gateway"></a>Beperk en beveiligde toegang via een API-gateway
Cloudtoepassingen hebben meestal een gateway in de front-end nodig om een centraal ingangspunt te bieden voor gebruikers, apparaten of andere toepassingen. Een [API-gateway](/azure/architecture/microservices/gateway) tussen clients en -services en vormt de toegang tot alle services die uw toepassing kan leveren. Het fungeert als omgekeerde proxy routering van aanvragen van clients tot services. Het kan ook verschillende algemene taken, zoals verificatie en autorisatie, SSL-beëindiging en snelheidsbeperking uitvoeren. Als u een gateway niet implementeert, moeten clients geen aanvragen verzenden rechtstreeks aan de front-end-services.

In de Service Fabric, een gateway worden alle stateless Services, zoals een [ASP.NET Core toepassing](service-fabric-reliable-services-communication-aspnetcore.md), of een andere service die zijn ontworpen voor inkomend verkeer, zoals [Træfik](https://docs.traefik.io/), [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IoT-Hub](https://docs.microsoft.com/azure/iot-hub/), of [Azure API Management](https://docs.microsoft.com/azure/api-management).

API Management wordt rechtstreeks geïntegreerd met Service Fabric, zodat u voor het publiceren van API's met een groot aantal regels voor het doorsturen naar uw back-end Service Fabric-services.  U kunt beveiligde toegang tot de back-end-services, voorkomen van DOS-aanvallen via beperking of API-sleutels, JWT-tokens, certificaten, en andere referenties te verifiëren. Voor meer informatie lezen [Service Fabric met Azure API Management-overzicht](service-fabric-api-management-overview.md).

## <a name="manage-application-secrets"></a>Toepassingsgeheimen beheren
Geheimen mag gevoelige informatie, zoals de storage-verbindingsreeksen, wachtwoorden of andere waarden die niet moeten worden behandeld als tekst zonder opmaak. In dit artikel maakt gebruik van Azure Sleutelkluis sleutels en geheimen beheren. Echter, *met* geheimen in een toepassing is cloud platform-networkdirect naar toepassingen kunnen worden geïmplementeerd naar een cluster overal worden gehost.

De aanbevolen manier voor het beheren van configuratie-instellingen is via [service configuratiepakketten][config-package]. Configuratiepakketten zijn samengestelde en bij te werken via beheerde rollende upgrades met health-validatie en voor automatisch terugdraaien. Dit is voorkeur naar globale configuratie vermindert de kans op een globale Services uitvallen. Versleutelde geheimen zijn geen uitzondering. Service Fabric bevat ingebouwde functies voor het versleutelen en ontsleutelen van waarden in een pakket Settings.xml configuratiebestand met behulp van certificaatversleuteling.

Het volgende diagram illustreert de basisprincipes voor beheer van de geheime in een Service Fabric-toepassing:

![overzicht van geheime management][overview]

Er zijn vier hoofdstappen in deze overdracht:

1. Verkrijgen van een certificaat voor het uitwisselen van gegevens.
2. Installeer het certificaat in het cluster.
3. Versleutelen geheime waarden bij het implementeren van een toepassing met het certificaat en een service Settings.xml configuratiebestand injecteren.
4. Versleutelde waarden buiten Settings.xml lezen door te ontsleutelen met hetzelfde certificaat uitwisselen. 

[Azure Sleutelkluis] [ key-vault-get-started] wordt hier gebruikt als een locatie voor de veilige opslag voor certificaten en als een manier om certificaten zijn geïnstalleerd op de Service Fabric-clusters in Azure. Als u niet in Azure implementeert, hoeft u geen Sleutelkluis gebruiken voor het beheren van geheimen in Service Fabric-toepassingen.

Zie voor een voorbeeld [toepassing geheimen beheren](service-fabric-application-secret-management.md).

## <a name="secure-the-hosting-environment"></a>De hostomgeving beveiligen
U kunt toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts beveiligen met behulp van Azure Service Fabric. Service Fabric helpt ook bij de resources die worden gebruikt door toepassingen op het moment van implementatie onder de gebruikersaccounts--bijvoorbeeld, bestanden, mappen en certificaten beveiligen. Hierdoor waarop toepassingen worden uitgevoerd, zelfs in een gedeelde gehoste omgeving veiliger van elkaar.

Het toepassingsmanifest verklaart de beveiligings-principals (gebruikers en groepen) vereist uitvoeren van de service (s) en de beveiligde bronnen.  Deze beveiliging principals wordt verwezen in het beleid, bijvoorbeeld het run as,-eindpunt binden, delen of toegang beveiligingsbeleid van het pakket.  Beleid wordt vervolgens toegepast op servicebronnen in de **ServiceManifestImport** sectie van het toepassingsmanifest.

Als u principals definieert, kunt u ook definiëren en gebruikersgroepen maken, zodat een of meer gebruikers kunnen worden toegevoegd aan elke groep samen worden beheerd. Dit is handig wanneer er meerdere gebruikers voor verschillende toegangspunten en deze moeten bepaalde algemene rechten die beschikbaar op het groepeerniveau van de zijn.

Service Fabric-toepassingen worden standaard uitgevoerd onder het account waaronder het Fabric.exe-proces wordt uitgevoerd onder. Service Fabric biedt tevens de mogelijkheid toepassingen uitvoeren in een lokale gebruikersaccount of het lokale systeemaccount gebruikt, is opgegeven in het manifest van de toepassing. Zie voor meer informatie [een service uitvoeren als een lokale gebruikersaccount of de lokale systeemaccount](service-fabric-application-runas-security.md).  U kunt ook [een opstartscript service uitvoeren als een lokale gebruiker of systeem-account](service-fabric-run-script-at-service-startup.md).

Wanneer u Service Fabric op een zelfstandige-cluster van Windows uitvoert, kunt u een service uit onder uitvoeren [Active Directory-domeinaccounts](service-fabric-run-service-as-ad-user-or-group.md) of [groep beheerde serviceaccounts](service-fabric-run-service-as-gmsa.md).

## <a name="secure-containers"></a>Beveiligde containers
Service Fabric biedt een mechanisme voor services binnen een container voor toegang tot een certificaat dat is geïnstalleerd op de knooppunten in een Windows- of Linux-cluster (versie 5.7 of hoger). Deze PFX-certificaat kan worden gebruikt voor het verifiëren van de toepassing of service of beveiligde communicatie met andere services. Zie voor meer informatie [een certificaat importeren in een container](service-fabric-securing-containers.md).

Service Fabric ondersteunt bovendien ook gMSA (groep beheerde serviceaccounts) voor Windows-containers. Zie voor meer informatie [gMSA voor Windows-containers instellen](service-fabric-setup-gmsa-for-windows-containers.md).

## <a name="secure-service-communication"></a>Beveiligen van servicecommunicatie
In Service Fabric, een service wordt uitgevoerd ergens in een Service Fabric-cluster, doorgaans verdeeld over meerdere virtuele machines. Service Fabric biedt verschillende opties voor het beveiligen van uw service-communicatie.

U kunt inschakelen HTTPS-eindpunten in uw [ASP.NET Core of Java](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) webservices.

U kunt beveiligde verbinding maken tussen de omgekeerde proxy en services, waardoor u een beveiligd kanaal voor end-to-end. Verbinding maken met veilige services wordt alleen ondersteund als omgekeerde proxy is geconfigureerd om te luisteren op HTTPS. Voor meer informatie over het configureren van de omgekeerde proxy [omgekeerde proxy in Azure Service Fabric](service-fabric-reverseproxy.md).  [Verbinding maken met een beveiligde service](service-fabric-reverseproxy-configure-secure-communication.md) wordt beschreven hoe u een beveiligde verbinding tussen de omgekeerde proxy en services.

Het framework Reliable Services biedt enkele vooraf gedefinieerde communicatie stacks en hulpprogramma's die u gebruiken kunt om beveiliging te verbeteren. Informatie over het verbeteren van beveiliging wanneer u service voor externe toegang (in [C#](service-fabric-reliable-services-secure-communication.md) of [Java](service-fabric-reliable-services-secure-communication-java.md)) of met behulp van [WCF](service-fabric-reliable-services-secure-communication-wcf.md).

## <a name="encrypt-application-data-at-rest"></a>Toepassingsgegevens in rust versleutelen
Elke [knooppunttype](service-fabric-cluster-nodetypes.md) in een Service Fabric-cluster worden uitgevoerd in Azure wordt ondersteund door een [virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). U kunt met behulp van een Azure Resource Manager-sjabloon gegevensschijven koppelen aan de schaalsets die gezamenlijk het Service Fabric-cluster vormen.  Als uw services gegevens op een schijf bijgesloten gegevens opgeslagen, kunt u [coderen die gegevensschijven](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md) om de toepassingsgegevens van uw te beveiligen.

<!--TO DO: Enable BitLocker on Windows standalone clusters?
TO DO: Encrypt disks on Linux clusters?-->


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* [Een setup-script uitvoeren op de service is gestart](service-fabric-run-script-at-service-startup.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)
* [Meer informatie over clusterbeveiliging](service-fabric-cluster-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-and-service-manifests.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-and-service-security/overview.png
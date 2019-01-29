---
title: Meer informatie over de beveiliging van Azure Service Fabric-toepassingen | Microsoft Docs
description: Een overzicht van hoe u veilig microservices-toepassingen uitvoeren op Service Fabric. Informatie over het uitvoeren van services en opstarten script onder verschillende beveiligingsaccounts, verifiëren en autoriseren van gebruikers, toepassingsgeheimen beheren, beveiligde servicecommunicatie, gebruikt u een API-gateway en veilige toepassingsgegevens in rust.
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
ms.openlocfilehash: f83f7afa4173316f127c76f20967054bf13c9a6b
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55097894"
---
# <a name="service-fabric-application-and-service-security"></a>Service Fabric-toepassing en service-beveiliging
Een architectuur met microservices kunt brengen [veel voordelen](service-fabric-overview-microservices.md). Beheren van de beveiliging van microservices, is echter een uitdaging en anders dan de traditionele monolithische toepassingen beveiliging beheren. 

De toepassing wordt doorgaans uitgevoerd op een of meer servers in een netwerk met een zichzelf en is het eenvoudiger om de blootgestelde poorten en API's en IP-adres te identificeren. Er is vaak een perimeternetwerk of grens en een database om te beveiligen. Als dit systeem is geknoeid vanwege een schending van de beveiliging of een aanval, is het waarschijnlijk dat alles in het systeem beschikbaar voor de aanvaller zijn. Met microservices is het systeem complexer.  Services zijn gedecentraliseerde en verdeeld over een groot aantal hosts en migreren van host naar de host.  Met de juiste beveiliging kan u een de bevoegdheden die de aanvaller krijgt en de hoeveelheid gegevens die beschikbaar zijn in een één-aanval beperken door inbreuk op één service.  Communicatie is geen interne, maar er gebeurt via een netwerk, en er zijn veel blootgestelde poorten en interacties tussen services. Weten wat deze service-interacties zijn en wanneer ze zich voordoen is essentieel voor de beveiliging van uw toepassingen.

In dit artikel is geen handleiding en microservices security, er zijn veel van deze bronnen online beschikbaar, maar hierin wordt beschreven hoe verschillende aspecten van beveiliging kunnen worden uitgevoerd in Service Fabric.

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
Het is vaak nodig zijn voor resources en API's die worden weergegeven door een service moeten worden beperkt tot bepaalde vertrouwde gebruikers of -clients. Verificatie is het proces van het op betrouwbare wijze vaststelling van de identiteit van een gebruiker.  Autorisatie is het proces dat kunt u API's of services beschikbaar zijn voor sommige geverifieerd, maar niet voor andere gebruikers.

### <a name="authentication"></a>Verificatie
De eerste stap bij het nemen van beslissingen voor API-niveau vertrouwensrelatie is verificatie. Verificatie is het proces van het op betrouwbare wijze vaststelling van de identiteit van een gebruiker.  In scenario's microservices, verificatie, wordt doorgaans afgehandeld Centraal. Als u een API-Gateway gebruikt, kunt u [verificatie-offload](/azure/architecture/patterns/gateway-offloading) naar de gateway. Als u deze methode gebruikt, zorg ervoor dat de afzonderlijke services rechtstreeks (zonder de API-Gateway) kunnen niet worden bereikt, tenzij de extra beveiliging is ingesteld om berichten te verifiëren of ze afkomstig zijn van de gateway of niet.

Als services kunnen rechtstreeks worden geopend, wordt een authentication-service zoals Azure Active Directory of een toegewezen verificatie microservice die fungeert als een security token service (STS) kan worden gebruikt om gebruikers te verifiëren. Beslissingen voor de vertrouwensrelatie worden gedeeld tussen services met beveiligingstokens of cookies. 

Voor ASP.NET Core, het primaire mechanisme voor [verifiëren van gebruikers](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/) is het lidmaatschapssysteem van ASP.NET Core-identiteit. ASP.NET Core-identiteit slaat gebruikersgegevens (zoals aanmeldingsgegevens, rollen en claims) in een gegevensarchief dat is geconfigureerd door de ontwikkelaar. ASP.NET Core-identiteit biedt ondersteuning voor verificatie met twee factoren.  Externe verificatieproviders worden ook ondersteund, zodat gebruikers kunnen zich aanmelden met bestaande verificatieprocessen van leveranciers zoals Microsoft, Google, Facebook of Twitter. 

### <a name="authorization"></a>Autorisatie
Na verificatie services moeten gebruikerstoegang verlenen of kunt vaststellen wat een gebruiker kan doen. Dit proces staat een service voor het maken van API's beschikbaar voor geverifieerde gebruikers, maar niet voor alle. Autorisatie is een rechthoekige en onafhankelijk van verificatie, het proces is van de vaststelling van wie een gebruiker is. Verificatie kan een of meer id's voor de huidige gebruiker maken.

[ASP.NET Core-autorisatie](/dotnet/standard/microservices-architecture/secure-net-microservices-web-applications/authorization-net-microservices-web-applications) kan worden gedaan op basis van gebruikers, rollen of op basis van aangepaste beleid, waaronder het inspecteren van claims of andere methodiek.

## <a name="restrict-and-secure-access-using-an-api-gateway"></a>Beperk en veilige toegang met behulp van een API-gateway
Cloudtoepassingen hebben meestal een gateway in de front-end nodig om een centraal ingangspunt te bieden voor gebruikers, apparaten of andere toepassingen. Een [API-gateway](/azure/architecture/microservices/gateway) tussen clients en -services en is het toegangspunt voor alle services die uw toepassing kan leveren. Het fungeert als een omgekeerde proxy routeren van aanvragen van clients met services. Er kan ook verschillende algemene taken, zoals verificatie en autorisatie, SSL-beëindiging en gelden enkele beperkingen uitvoeren. Als u een gateway niet implementeert, moeten clients aanvragen verzenden rechtstreeks aan de front-end-services.

In Service Fabric, een gateway kan worden elke stateless service, zoals een [ASP.NET Core-toepassing](service-fabric-reliable-services-communication-aspnetcore.md), of een andere service die is ontworpen voor inkomend verkeer, zoals [Traefik](https://docs.traefik.io/), [Event Hubs](https://docs.microsoft.com/azure/event-hubs/), [IoT-Hub](https://docs.microsoft.com/azure/iot-hub/), of [Azure API Management](https://docs.microsoft.com/azure/api-management).

API Management wordt rechtstreeks geïntegreerd met Service Fabric, zodat u kunt voor het publiceren van API's met een uitgebreide set regels voor doorsturen naar uw back-end-Service Fabric-services.  U kunt beveiligde toegang tot de back-endservices, DOS-aanvallen te voorkomen dat met behulp van de beperking of Controleer of de API-sleutels, JWT-tokens, certificaten, en andere referenties. Voor meer informatie lezen [Service Fabric met Azure API Management-overzicht](service-fabric-api-management-overview.md).

## <a name="manage-application-secrets"></a>Toepassingsgeheimen beheren
Geheimen kunnen gevoelige informatie, zoals storage-verbindingsreeksen, wachtwoorden, of andere waarden die niet moeten worden verwerkt als tekst zonder opmaak zijn. In dit artikel maakt gebruik van Azure Key Vault om sleutels en geheimen te beheren. Echter, *met behulp van* geheimen in een toepassing is cloud platform-agnostische om toe te staan van toepassingen op een cluster die overal gehost kunnen worden geïmplementeerd.

De aanbevolen manier voor het beheren van configuratie-instellingen is via [service configuratiepakketten][config-package]. Configuratiepakketten zijn samengesteld en bij te werken via beheerde rolling upgrades met health-validatie en automatisch ongedaan maken. Dit is voorkeur aan de globale configuratie vermindert de kans op een wereldwijde service-onderbreking. Versleutelde geheimen zijn geen uitzondering. Service Fabric heeft ingebouwde functies voor het versleutelen en ontsleutelen van waarden in een pakket Settings.xml configuratiebestand met behulp van certificaatversleuteling.

Het volgende diagram illustreert de basisprincipes voor geheimenbeheer in een Service Fabric-toepassing:

![overzicht van geheimenbeheer][overview]

Er zijn vier belangrijke stappen in deze stroom:

1. Een versleutelingscertificaat gegevens verkrijgen.
2. Installeer het certificaat in uw cluster.
3. Versleutelen van geheime waarden bij het implementeren van een toepassing met het certificaat en injecteren van een service Settings.xml-configuratiebestand.
4. Versleutelde waarden buiten het Settings.xml lezen door te ontsleutelen met de dezelfde versleutelingscertificaat. 

[Azure Key Vault] [ key-vault-get-started] wordt hier gebruikt als een locatie voor de veilige opslag voor certificaten en als een manier om certificaten zijn geïnstalleerd op de Service Fabric-clusters in Azure. Als u niet naar Azure implementeert, hoeft u geen Key Vault gebruikt om geheimen in Service Fabric-toepassingen te beheren.

Zie voor een voorbeeld [toepassingsgeheimen beheren](service-fabric-application-secret-management.md).

## <a name="secure-the-hosting-environment"></a>De hosting-omgeving beveiligen
Met behulp van Azure Service Fabric, kunt u toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts beveiligen. Service Fabric helpt ook bij het beveiligen van de resources die worden gebruikt door toepassingen op het moment van implementatie onder de gebruikersaccounts, bijvoorbeeld, bestanden, mappen en certificaten. Hierdoor actieve toepassingen, zelfs in een gedeelde omgeving, beter te beveiligen van elkaar.

Het toepassingsmanifest verklaart de beveiligings-principals (gebruikers en groepen) vereist uitvoeren van de service (s) en beveiligde bronnen.  Deze beveiliging principals wordt verwezen in het beleid, bijvoorbeeld het run as,-eindpunt binden, het pakket delen of toegang beveiligingsbeleid.  Beleid wordt vervolgens toegepast op service-resources in de **ServiceManifestImport** gedeelte van het toepassingsmanifest.

Bij het melden van beveiligings-principals, kunt u ook definiëren en gebruikersgroepen maken, zodat een of meer gebruikers kunnen worden toegevoegd aan elke groep samen worden beheerd. Dit is handig als er meerdere gebruikers voor verschillende toegangspunten zijn en moeten bepaalde algemene rechten die beschikbaar op het groepeerniveau van de zijn hebt.

Standaard wordt Service Fabric-toepassingen uitvoeren onder het account dat de Fabric.exe-proces wordt uitgevoerd onder. Service Fabric biedt tevens de mogelijkheid om uit te voeren toepassingen onder een lokaal gebruikersaccount of het lokale systeemaccount gebruikt, die is opgegeven in het manifest van de toepassing. Zie voor meer informatie, [een service uitvoeren als een lokale gebruikersaccount of local system-account](service-fabric-application-runas-security.md).  U kunt ook [een opstartscript service uitgevoerd als een lokale gebruiker of systeemaccount](service-fabric-run-script-at-service-startup.md).

Wanneer u Service Fabric op een zelfstandige cluster van Windows uitvoert, kunt u een service onder uitvoeren [Active Directory-domeinaccounts](service-fabric-run-service-as-ad-user-or-group.md) of [groep beheerde serviceaccounts](service-fabric-run-service-as-gmsa.md).

## <a name="secure-containers"></a>Beveiligde containers
Service Fabric biedt een mechanisme voor services binnen een container voor toegang tot een certificaat dat is geïnstalleerd op de knooppunten in een Windows- of Linux-cluster (versie 5.7 of hoger). Dit PFX-certificaat kan worden gebruikt voor het verifiëren van de toepassing of service of beveiligde communicatie met andere services. Zie voor meer informatie, [een certificaat importeren in een container](service-fabric-securing-containers.md).

Service Fabric ondersteunt bovendien ook gMSA (group Managed Service Accounts) voor Windows-containers. Zie voor meer informatie, [gMSA instellen voor Windows-containers](service-fabric-setup-gmsa-for-windows-containers.md).

## <a name="secure-service-communication"></a>Beveiligde servicecommunicatie
In Service Fabric, een service wordt uitgevoerd ergens in een Service Fabric-cluster, meestal verdeeld over meerdere virtuele machines. Service Fabric biedt verschillende opties voor het beveiligen van uw service-communicatie.

Kunt u HTTPS-eindpunten in uw [ASP.NET Core- of Java](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) webservices.

U kunt beveiligde verbinding tussen de reverse proxy- en -services, waardoor u een beveiligd kanaal voor end-to-end maken. Verbinding maken met veilige services wordt alleen ondersteund als omgekeerde proxy is geconfigureerd om te luisteren op HTTPS. Lees voor informatie over het configureren van de omgekeerde proxy [omgekeerde proxy in Azure Service Fabric](service-fabric-reverseproxy.md).  [Verbinding maken met een beveiligd service](service-fabric-reverseproxy-configure-secure-communication.md) staat beschreven hoe u beveiligde verbinding tussen de reverse proxy- en -services.

Het framework van de toepassing betrouwbare Services biedt een aantal vooraf gedefinieerde communicatie-stacks en hulpprogramma's die u gebruiken kunt om beveiliging te verbeteren. Meer informatie over de beveiliging te verbeteren wanneer u externe toegang tot service (in [ C# ](service-fabric-reliable-services-secure-communication.md) of [Java](service-fabric-reliable-services-secure-communication-java.md)) of met behulp van [WCF](service-fabric-reliable-services-secure-communication-wcf.md).

## <a name="encrypt-application-data-at-rest"></a>Toepassingsgegevens in rust versleutelen
Elke [knooppunttype](service-fabric-cluster-nodetypes.md) in een Service Fabric-cluster worden uitgevoerd in Azure wordt ondersteund door een [virtuele-machineschaalset](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). U kunt met behulp van een Azure Resource Manager-sjabloon gegevensschijven koppelen aan de schaalsets die gezamenlijk het Service Fabric-cluster vormen.  Als uw services worden gegevens opslaan in een gekoppelde schijf, kunt u [die gegevensschijven versleutelen](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md) om de toepassingsgegevens van uw te beveiligen.

<!--TO DO: Enable BitLocker on Windows standalone clusters?
TO DO: Encrypt disks on Linux clusters?-->


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* [Voer een installatiescript uit bij het opstarten van de service](service-fabric-run-script-at-service-startup.md)
* [Resources specificeren in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een app implementeren](service-fabric-deploy-remove-applications.md)
* [Meer informatie over clusterbeveiliging](service-fabric-cluster-security.md)

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[config-package]: service-fabric-application-and-service-manifests.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md

<!-- Images -->
[overview]:./media/service-fabric-application-and-service-security/overview.png
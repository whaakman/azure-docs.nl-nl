---
title: Azure Service Fabric aanbevolen beveiligingsprocedures | Microsoft Docs
description: Dit artikel bevat een set met aanbevolen procedures voor beveiliging van de Azure Service Fabric.
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
ms.date: 11/01/2017
ms.author: tomsh
ms.openlocfilehash: a8b76e2895edcdbbddafbee7116e163d1789c06d
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-service-fabric-security-best-practices"></a>Azure Service Fabric best practices voor beveiliging
Een toepassing in Azure implementeren is snel, eenvoudig en rendabele. Voordat u uw cloudtoepassing in productie implementeert, controleert u onze lijst met essentiële en aanbevolen procedures voor het implementeren van beveiligde clusters in uw toepassing.

Azure Service Fabric is een gedistribueerde systemen platform waarmee u gemakkelijk pakket, implementeren en beheren van schaalbare en betrouwbare microservices. Service Fabric biedt ook een oplossing voor de grote uitdaging van het ontwikkelen en beheren van cloudtoepassingen. Ontwikkelaars en beheerders kunnen complexe infrastructuurproblemen voorkomen en zich concentreren op het implementeren van bedrijfsspecifieke, veeleisende werkbelastingen die schaalbaar, betrouwbaar en beheerbaar zijn. 

Voor elke beste kunnen worden uitgelegd:

-   Wat de beste handelswijze is.
-   Waarom moet u de aanbevolen procedure implementeren.
-   Wat kan gebeuren als u de aanbevolen procedure niet implementeert.
-   U leert hoe de aanbevolen procedure implementeren.

Wij raden de volgende Azure Service Fabric-security best practices:

-   Azure Resource Manager-sjablonen en de Service Fabric PowerShell-module gebruiken om beveiligde clusters te maken.
-   X.509-certificaten gebruiken.
-   Beveiligingsbeleid configureren.
-   De beveiligingsconfiguratie voor Reliable Actors implementeren.
-   SSL configureren voor Azure Service Fabric.
-   Netwerkisolatie en beveiliging gebruiken met Azure Service Fabric.
-   Azure Sleutelkluis configureren voor beveiliging.
-   Gebruikers toewijzen aan rollen.


## <a name="best-practices-for-securing-your-clusters"></a>Aanbevolen procedures voor het beveiligen van clusters

Gebruik altijd een beveiligde cluster:
-   Clusterbeveiliging implementeren met behulp van certificaten.
-   Client-toegang (admin en alleen-lezen) met behulp van Azure Active Directory (Azure AD).

Geautomatiseerde implementaties gebruiken:
-   Scripts gebruiken voor het genereren, implementeren en de geheimen overschakelen.
-   Opslaan van de geheimen in Azure Sleutelkluis en Azure AD gebruiken voor alle andere clienttoegang.
-   Verificatie vereisen voor menselijke toegang tot de geheimen.

Houd ook rekening met de volgende configuratieopties:
-   Perimeternetwerken (ook wel bekend als gedemilitariseerde zones, DMZ's en gescreend subnetten) maken met behulp van Azure Netwerkbeveiligingsgroepen (nsg's).
-   Toegang tot cluster virtuele machines (VM's) of uw cluster beheren met behulp van de landingspagina servers verbinding met extern bureaublad.

Uw clusters, moeten worden beveiligd om te voorkomen dat onbevoegde gebruikers verbinding maken, vooral wanneer een cluster wordt uitgevoerd in de productieomgeving. Hoewel het mogelijk te maken van een niet-beveiligde cluster, anonieme gebruikers verbinding kunnen maken met uw cluster als het cluster eindpunten voor beheer met het openbare internet beschrijft.

Er zijn drie [scenario's](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) voor het implementeren van clusterbeveiliging met behulp van verschillende technologieën:

-   Knooppunt naar beveiliging: dit scenario communicatie tussen de virtuele machines en de computers in het cluster beveiligt. Deze vorm van beveiliging zorgt ervoor dat alleen de computers die gemachtigd zijn om te worden toegevoegd aan het cluster toepassingen en services in het cluster hosten kunnen.
In dit scenario, de clusters die worden uitgevoerd op Azure of zelfstandige clusters die worden uitgevoerd op Windows, gebruikt een [beveiliging van het certificaat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) of [Windows-beveiliging](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) voor Windows Server-machines.
-   Beveiliging van de client-naar-knooppunt: in dit scenario communicatie tussen een Service Fabric-client en de afzonderlijke knooppunten in het cluster beveiligt.
-   Op rollen gebaseerde toegangsbeheer (RBAC): in dit scenario gebruikt afzonderlijke identiteiten (certificaten, Azure AD, enzovoort) voor elke beheerder en client-gebruikersrol die toegang heeft tot het cluster. U kunt de identiteiten van de rol opgeven bij het maken van het cluster.

>[!NOTE]
>**Beveiligingsaanbeveling voor Azure clusters:** beveiliging voor verificatie van clients en certificaten voor knooppunt naar beveiliging met Azure AD.

Zie voor het configureren van een zelfstandige Windows-cluster [instellingen configureren voor een zelfstandige Windows-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

Azure Resource Manager-sjablonen en de Service Fabric PowerShell-module gebruiken om een beveiligde cluster te maken.
Zie voor stapsgewijze instructies voor het maken van een beveiligde Service Fabric-cluster met behulp van Azure Resource Manager-sjablonen [maken van een Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

De Azure Resource Manager-sjabloon gebruikt:
-   Uw cluster aanpassen met behulp van de sjabloon voor het configureren van beheerde storage voor VM virtuele harde schijven (VHD's).
-   Station wijzigingen aan de resourcegroep met behulp van de sjabloon voor het beheer van eenvoudige configuratie en controle.

De clusterconfiguratie behandelen als code:
-   Uitgebreid worden bij het controleren van de implementatieconfiguraties van uw.
-   Vermijd het gebruik van impliciete opdrachten naar uw resources rechtstreeks worden gewijzigd.

Veel aspecten van de [Service Fabric-toepassing lifecycle](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) kan worden geautomatiseerd. De [Service Fabric PowerShell-module](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) automatiseert het algemene taken voor het implementeren, bijwerken, verwijderen en testen van Azure Service Fabric-toepassingen. Beheerde API's en HTTP-APIs voor toepassingsbeheer zijn ook beschikbaar.

## <a name="use-x509-certificates"></a>X.509-certificaten gebruiken
Altijd clusters beveiligen met behulp van X.509-certificaten of Windows-beveiliging. Beveiliging is alleen geconfigureerd tijdens het maken van een cluster. Het is niet mogelijk om in te schakelen beveiliging nadat het cluster is gemaakt.

Om op te geven een [cluster certificaat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security), stel de waarde van de **ClusterCredentialType** X509 eigenschap. Als een servercertificaat voor externe verbindingen opgeven, stelt u de **ServerCredentialType** X509 eigenschap.

Bovendien Volg deze procedures:
-   De certificaten voor productieclusters maken met behulp van een juist geconfigureerde service voor Windows Server-certificaat. U kunt ook de certificaten verkrijgen via een erkende certificeringsinstantie (CA).
-   Gebruik nooit een tijdelijke en certificaat voor productieclusters testen als het certificaat is gemaakt met behulp van MakeCert.exe of een vergelijkbaar hulpprogramma.
-   Een zelfondertekend certificaat gebruiken voor testclusters, maar niet voor productieclusters.

Als het cluster niet-beveiligd is, kan iedereen anoniem verbinding met het cluster en beheerbewerkingen uitvoeren. Daarom moet u altijd productieclusters beveiligen met behulp van x.509-certificaten of Windows-beveiliging.

Zie voor meer informatie over het gebruik van X.509-certificaten, [toevoegen of verwijderen van certificaten voor een Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Beveiligingsbeleid configureren
Service Fabric beveiligt ook de bronnen die worden gebruikt door toepassingen. Resources, zoals bestanden, mappen, en certificaten zijn opgeslagen onder de gebruikersaccounts wanneer de toepassing wordt geïmplementeerd. Deze functie kunt actieve toepassingen veiliger van elkaar, zelfs in een gedeelde gehoste omgeving.

-   Gebruik van een gebruiker of groep voor Active Directory-domein: uitvoeren van de service onder de referenties voor een Active Directory-gebruiker of groep account. Zorg ervoor dat het gebruik van Active Directory on-premises binnen uw domein en niet Azure Active Directory. Toegang tot andere bronnen in het domein die machtigingen hebben gekregen met behulp van een domeingebruiker of -groep. Bijvoorbeeld, bronnen zoals bestandsshares.

-   Toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten: Geef de **SecurityAccessPolicy** eigenschap toe te passen een **RunAs** beleid aan een service bij het servicemanifest eindpunt resources declareert met HTTP. Poorten die zijn toegewezen aan de HTTP-eindpunten zijn correct toegangscontrole lijsten voor het RunAs-gebruikersaccount dat de service wordt uitgevoerd onder. Wanneer het beleid niet is ingesteld, kunt http.sys heeft geen toegang tot de service en u fouten met aanroepen van de client.

Zie voor meer informatie over het gebruik van beleidsregels voor veiligheid in Service Fabric-cluster, [beveiligingsbeleid voor uw toepassing configureren](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="implement-the-reliable-actors-security-configuration"></a>De beveiligingsconfiguratie voor Reliable Actors implementeren
Service Fabric Reliable Actors is een implementatie van het ontwerppatroon actor. De beslissing om te gebruiken van een specifiek patroon is net als bij een ontwerppatroon software gebaseerd op of een softwareprobleem overeenkomt met het patroon.

In het algemeen met het ontwerppatroon actor kunt model oplossingen voor de volgende problemen met software of security scenario's:
-   De ruimte van uw probleem betrekking heeft op een groot aantal (duizendtallen of meer) van kleine, onafhankelijke en geïsoleerde eenheden van de status en logica.
-   U werkt met één thread-objecten die geen aanzienlijke interactie met externe onderdelen, inclusief de status opvragen van een set van actoren is vereist.
-   Uw actor-exemplaren blokkeren niet aanroepfuncties met onvoorspelbare vertragingen door uitgifte van i/o-bewerkingen.

In Service Fabric zijn actoren geïmplementeerd in het kader van de toepassing Reliable Actors. Dit framework is gebaseerd op het patroon actor en gebouwd boven [Service Fabric Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction). Elke betrouwbare actor-service die u schrijft is een gepartitioneerde stateful betrouwbare service.

Elke actor wordt gedefinieerd als een exemplaar van een actortype identiek is aan de manier waarop die.NET-object een exemplaar van een .NET-type is. Bijvoorbeeld, een **actortype** dat implementeert de functionaliteit van een Rekenmachine kan hebben veel actoren van dat type die worden gedistribueerd op verschillende knooppunten in een cluster. Elk van de gedistribueerde actoren wordt een unieke gekenmerkt door een actor-id.

[Replicator beveiligingsconfiguraties](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) worden gebruikt voor het Beveilig het communicatiekanaal dat wordt gebruikt tijdens de replicatie. Deze configuratie kan services niet zien elkaars replicatieverkeer en zorgt ervoor dat de maximaal beschikbare gegevens veilig is. Standaard wordt een lege beveiligingsconfiguratiesectie voorkomen dat replicatiebeveiliging.
Replicator configuraties configureren de replicator die verantwoordelijk is voor het maken van de status van de Actor State-Provider uiterst betrouwbaar.

## <a name="configure-ssl-for-azure-service-fabric"></a>SSL configureren voor Azure Service Fabric
Het verificatieproces server [verifieert](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) management eindpunten van het cluster naar een management-client. De management-client vervolgens herkent dat deze met het echte cluster communiceert. Dit certificaat biedt ook een [SSL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) voor de HTTPS-API en voor Service Fabric Explorer via HTTPS.
U moet een aangepaste domeinnaam voor uw cluster. Wanneer u een certificaat bij een certificeringsinstantie aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

SSL configureren voor een toepassing, moet u eerst een SSL-certificaat dat is ondertekend door een Certificeringsinstantie verkrijgen. De CA is een vertrouwde derde partij die om veiligheidsredenen SSL-certificaten verleent. Als u een SSL-certificaat nog geen hebt, moet u aanvragen bij een bedrijf dat SSL-certificaten verkoopt.

Het certificaat moet voldoen aan de volgende vereisten voor SSL-certificaten in Azure:
-   Het certificaat moet een persoonlijke sleutel bevatten.

-   Het certificaat moet worden gemaakt voor sleuteluitwisseling en kunnen worden geëxporteerd naar een persoonlijke gegevens exchange (.pfx)-bestand.

-   De onderwerpnaam van het certificaat moet overeenkomen met de domeinnaam die wordt gebruikt voor toegang tot uw cloudservice.

    - Verkrijgen van een aangepaste domeinnaam te gebruiken voor toegang tot uw cloudservice.
    - Een certificaat aanvragen bij een CA met een onderwerpnaam die overeenkomt met de aangepaste domeinnaam van uw service. Bijvoorbeeld, als uw aangepaste domeinnaam is __contoso__**.com**, het certificaat van uw CA moet de onderwerpnaam hebben **. contoso.com** of __www__ **. contoso.com**.

    >[!NOTE]
    >U kunt een SSL-certificaat kan niet verkrijgen van een Certificeringsinstantie voor de __cloudapp__**.net** domein.
    
-   Het certificaat moet ten minste 2048-bits codering gebruiken.

Het HTTP-protocol is onveilig en eavesdropping aangevallen. Gegevens die worden verzonden via HTTP wordt verzonden als tekst zonder opmaak vanuit de webbrowser op de webserver of tussen de andere eindpunten. Aanvallers kunnen onderscheppen en gevoelige gegevens die worden verzonden via HTTP, zoals creditcardgegevens en aanmeldingen account bekijken. Wanneer gegevens worden verzonden of via een browser via HTTPS geboekt, SSL zorgt ervoor dat gevoelige informatie versleuteld en beveiligd tegen worden onderschept.

Zie voor meer informatie over het gebruik van SSL-certificaten, [SSL configureren voor Azure-toepassingen](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="use-network-isolation-and-security-with-azure-service-fabric"></a>Netwerkisolatie en beveiliging te gebruiken met Azure Service Fabric
Een beveiligde 3 nodetype-cluster instellen met behulp van de [Azure Resource Manager-sjabloon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) als voorbeeld. Het inkomend en uitgaand netwerkverkeer beheren met behulp van de sjabloon en de Netwerkbeveiligingsgroepen.

De sjabloon heeft een NSG voor elk van de virtuele-machineschaalsets en moet worden gebruikt voor het verkeer van en naar de set. De regels zijn standaard geconfigureerd voor alle verkeer toestaan die nodig zijn voor de systeemservices en de toepassing poorten opgegeven in de sjabloon. Bekijk deze regels en breng eventuele wijzigingen aan uw behoeften voldoet, waaronder het toevoegen van nieuwe regels voor uw toepassingen.

Zie voor meer informatie [algemene netwerken scenario's voor Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-azure-key-vault-for-security"></a>Instellen van Azure Sleutelkluis voor beveiliging
Service Fabric gebruikt certificaten voor verificatie en versleuteling voor het beveiligen van een cluster en de toepassingen.

Service Fabric gebruikt X.509-certificaten voor het beveiligen van een cluster en voor het bieden van beveiligingsfuncties van toepassing. Gebruik van Azure Sleutelkluis [certificaten beheren](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) voor Service Fabric-clusters in Azure. De resourceprovider van Azure die de clusters worden gemaakt, haalt de certificaten van een sleutelkluis. De provider installeert vervolgens de certificaten op de virtuele machines wanneer het cluster is geïmplementeerd in Azure.

Een certificaat relatie bestaat tussen [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), de Service Fabric-cluster en de resourceprovider die gebruikmaakt van de certificaten. Wanneer het cluster wordt gemaakt, wordt informatie over de relatie van het certificaat wordt opgeslagen in een sleutelkluis.

Er zijn twee basisstappen voor het instellen van een sleutelkluis:
1. Maak een resourcegroep specifiek voor uw sleutelkluis.

    Het is raadzaam dat u de sleutelkluis in een eigen resourcegroep geplaatst. Deze actie voorkomt u dat het verlies van uw sleutels en geheimen als andere resourcegroepen worden verwijderd, zoals opslag, rekencapaciteit of de groep met uw cluster. De resourcegroep met de sleutelkluis moet in dezelfde regio bevinden als het cluster dat wordt gebruikt.

2. Een sleutelkluis maken in de nieuwe resourcegroep.

    De sleutelkluis moet zijn ingeschakeld voor implementatie. De resourceprovider voor compute kunt vervolgens de certificaten ophalen uit de kluis en deze installeren op de VM-exemplaren.

Zie voor meer informatie over het instellen van een sleutelkluis, [aan de slag met Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-to-roles"></a>Gebruikers toewijzen aan rollen
Nadat u de toepassingen voor uw cluster hebt gemaakt, uw gebruikers toewijzen aan de rollen die worden ondersteund door Service Fabric: alleen-lezen en de beheerder. U kunt deze rollen toewijzen met behulp van de Azure-portal.

>[!NOTE]
> Zie voor meer informatie over het gebruik van functies in Service Fabric [toegangsbeheer op basis van rollen voor Service Fabric-clients](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric ondersteunt twee toegangstypen voor clients die zijn verbonden met een [Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm): beheerder en gebruiker. De Clusterbeheerder kunt Toegangsbeheer gebruiken om te beperken van toegang tot bepaalde bewerkingen voor verschillende groepen gebruikers voor een cluster. Toegangsbeheer maakt het cluster beter te beveiligen.

## <a name="next-steps"></a>Volgende stappen
- Instellen van uw Service Fabric [ontwikkelomgeving](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- Meer informatie over [Service Fabric-ondersteuningsopties](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).

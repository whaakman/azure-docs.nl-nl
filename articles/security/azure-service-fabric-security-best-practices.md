---
title: Azure Service Fabric best practices voor beveiliging | Microsoft Docs
description: Dit artikel bevat een set van aanbevolen procedures voor Azure Service Fabric-beveiliging.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2019
ms.author: tomsh
ms.openlocfilehash: 8bafc4a95ca9af4567ed70c190a72f3b351da47c
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114702"
---
# <a name="azure-service-fabric-security-best-practices"></a>Azure Service Fabric best practices voor beveiliging
Implementeren van een toepassing op Azure is een snelle, eenvoudige en kosteneffectieve. Voordat u uw cloudtoepassing in productie implementeert, Bekijk onze lijst met essentiële en aanbevolen procedures voor het implementeren van veilige clusters in uw toepassing.

Azure Service Fabric is een gedistribueerde systemen platform waarmee u gemakkelijk pakket, implementeren en beheren van schaalbare en betrouwbare microservices. Service Fabric biedt ook een oplossing voor de grote uitdaging van het ontwikkelen en beheren van cloudtoepassingen. Ontwikkelaars en beheerders kunnen complexe infrastructuurproblemen voorkomen en zich concentreren op het implementeren van bedrijfsspecifieke, veeleisende werkbelastingen die schaalbaar, betrouwbaar en beheerbaar zijn.

Voor elke best practice wordt we het volgende uitgelegd:

-   Wat de beste manier is.
-   Waarom moet u de aanbevolen procedure implementeren.
-   Wat kan gebeuren als u de aanbevolen procedure niet implementeren.
-   Hoe kunt u meer informatie voor het implementeren van de aanbevolen procedure.

We raden aan de volgende Azure Service Fabric-beveiliging aanbevolen procedures:

-   Azure Resource Manager-sjablonen en de Service Fabric PowerShell-module gebruiken om beveiligde clusters te maken.
-   X.509-certificaten gebruiken.
-   Beveiligingsbeleid configureren.
-   De beveiligingsconfiguratie Reliable Actors implementeren.
-   SSL configureren voor Azure Service Fabric.
-   Gebruik netwerkisolatie en beveiliging met Azure Service Fabric.
-   Azure Key Vault voor beveiliging configureren.
-   Gebruikers toewijzen aan rollen.


## <a name="best-practices-for-securing-your-clusters"></a>Aanbevolen procedures voor het beveiligen van uw clusters

Gebruik altijd een beveiligd cluster:
-   Beveiliging-clusters implementeren met behulp van certificaten.
-   Client-toegang (admin en alleen-lezen) met behulp van Azure Active Directory (Azure AD).

Gebruik van geautomatiseerde implementaties:
-   Scripts gebruiken om te genereren, implementeren en de geheimen worden meegenomen.
-   De geheimen in Azure Key Vault Store en Azure AD gebruiken voor alle andere clienttoegang.
-   Verificatie vereisen voor menselijke toegang tot de geheimen.

Daarnaast kunt u de volgende configuratieopties:
-   Perimeternetwerken (ook wel bekend als gedemilitariseerde zones, DMZ's en gescreend subnet) maken met behulp van Azure voor Netwerkbeveiligingsgroepen (nsg's).
-   Toegang tot virtuele machines (VM's) of uw cluster beheren met behulp van Vliegende servers met verbinding met extern bureaublad.

Om te voorkomen dat onbevoegde gebruikers verbinding maken, met name als een cluster wordt uitgevoerd in de productieomgeving moeten uw clusters worden beveiligd. Hoewel het mogelijk te maken van een niet-beveiligd cluster, anonieme gebruikers verbinding kunnen maken met uw cluster als-beheereindpunten met het openbare internet wordt aangegeven dat het cluster.

Er zijn drie [scenario's](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security) voor het implementeren van beveiliging van clusters met behulp van verschillende technologieën:

-   Beveiliging van knooppunt-naar-knooppunt: In dit scenario voor beveiliging van communicatie tussen de virtuele machines en computers in het cluster. Deze vorm van beveiliging zorgt ervoor dat alleen de computers die zijn gemachtigd voor deelname aan het cluster, toepassingen en services in het cluster hosten kunnen.
In dit scenario, de clusters die worden uitgevoerd op Azure of een zelfstandige clusters die worden uitgevoerd op Windows, kunnen een gebruiken [beveiliging van het certificaat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security) of [Windows security](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-windows-security) voor Windows Server-machines.
-   Beveiliging van de client-naar-knooppunt: In dit scenario voor beveiliging van communicatie tussen een Service Fabric-client en de afzonderlijke knooppunten in het cluster.
-   Role-Based Access Control (RBAC): In dit scenario maakt gebruik van afzonderlijke identiteiten (certificaten, Azure AD, enzovoort) voor elke beheerder en client-gebruikersrol die toegang heeft tot het cluster. De rol-id's wordt u bij het maken van het cluster.

>[!NOTE]
>**Beveiligingsaanbeveling voor clusters met Azure:** Azure AD-beveiliging gebruiken om clients en certificaten voor de beveiliging van knooppunt-naar-knooppunt te verifiëren.

Zie configureren van een zelfstandige Windows-cluster [-instellingen configureren voor een zelfstandige Windows-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest).

Gebruik Azure Resource Manager-sjablonen en de Service Fabric PowerShell-module om een beveiligd cluster te maken.
Zie voor stapsgewijze instructies voor het maken van een beveiligd Service Fabric-cluster met behulp van Azure Resource Manager-sjablonen, [het maken van een Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm).

Gebruik de Azure Resource Manager-sjabloon:
-   Uw cluster aanpassen met behulp van de sjabloon voor het configureren van beheerde opslag voor virtuele machine virtuele harde schijven (VHD's).
-   Wijzigingen aan de resourcegroep met behulp van de sjabloon voor het beheer van eenvoudige configuratie en controle.

De configuratie van het cluster worden behandeld als code:
-   Uitgebreid worden tijdens het controleren van de implementatieconfiguraties van uw.
-   Vermijd het gebruik van de impliciete opdrachten het direct aanpassen van uw resources.

Veel aspecten van de [Service Fabric-toepassingslevenscyclus](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle) kan worden geautomatiseerd. De [Service Fabric PowerShell-module](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications#upload-the-application-package) algemene taken voor het implementeren, bijwerken, verwijderen en testen van Azure Service Fabric-toepassingen worden geautomatiseerd. Beheerde API's en HTTP-APIs voor toepassingsbeheer zijn ook beschikbaar.

## <a name="use-x509-certificates"></a>Gebruik van x.509-certificaten
Altijd uw clusters beveiligen met behulp van X.509-certificaten of Windows-beveiliging. Beveiliging wordt alleen geconfigureerd tijdens het maken van een cluster. Het is niet mogelijk om in te schakelen op beveiliging nadat het cluster is gemaakt.

Om op te geven een [clustercertificaat](https://docs.microsoft.com/azure/service-fabric/service-fabric-windows-cluster-x509-security), stel de waarde van de **ClusterCredentialType** eigenschap X509. Als een servercertificaat voor externe verbindingen opgeven, stelt u de **ServerCredentialType** eigenschap X509.

Bovendien, volgt u deze procedures:
-   De certificaten voor productieclusters maken met behulp van een goed geconfigureerde Windows Server-certificaatservice. U kunt ook de certificaten verkrijgen van een erkende certificeringsinstantie (CA).
-   Gebruik nooit een tijdelijke of certificaat voor productieclusters testen als het certificaat is gemaakt met behulp van MakeCert.exe of een vergelijkbaar hulpprogramma.
-   Gebruik een zelfondertekend certificaat voor testclusters, maar niet voor productieclusters.

Als het cluster onveilig is, kan iedereen anoniem verbinding maken met het cluster en beheerbewerkingen uitvoeren. Daarom moet u altijd productieclusters beveiligen met behulp van X.509-certificaten of Windows-beveiliging.

Zie voor meer informatie over het gebruik van X.509-certificaten, [toevoegen of verwijderen van certificaten voor een Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure).

## <a name="configure-security-policies"></a>Beveiligingsbeleid configureren
Service Fabric beveiligt ook de bronnen die worden gebruikt door toepassingen. Resources, zoals bestanden, mappen, en certificaten worden opgeslagen onder de gebruikersaccounts wanneer de toepassing wordt geïmplementeerd. Deze functie kunt actieve toepassingen beter te beveiligen van elkaar, zelfs in een gedeelde omgeving.

-   Gebruik een Active Directory-domeingroep of gebruiker: De service met de referenties voor een Active Directory-gebruiker of groepsaccount uitvoeren. Zorg ervoor dat on-premises Active Directory in uw domein en niet Azure Active Directory gebruiken. Toegang tot andere resources in het domein die machtigingen zijn verleend met behulp van een domeingebruiker of groep. Bijvoorbeeld, resources, zoals bestandsshares.

-   Een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten toewijzen: Geef de **SecurityAccessPolicy** eigenschap om toe te passen een **RunAs** beleid aan een service wanneer het servicemanifest bronnen met HTTP-eindpunt verklaart. Poorten die zijn toegewezen aan de HTTP-eindpunten zijn lijsten met correct toegangscontrole voor het RunAs-gebruikersaccount dat de service wordt uitgevoerd onder. Wanneer het beleid niet is ingesteld, http.sys heeft geen toegang tot de service en krijgt u fouten met aanroepen van de client.

Zie voor meer informatie over het gebruik van beleidsregels voor veiligheid in een Service Fabric-cluster, [beveiligingsbeleid configureren voor uw toepassing](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-runas-security).

## <a name="implement-the-reliable-actors-security-configuration"></a>Implementeren van de beveiligingsconfiguratie Reliable Actors
Service Fabric Reliable Actors is een implementatie van het ontwerppatroon actor. De beslissing om te gebruiken een specifieke patroon is net als bij een ontwerppatroon software gebaseerd op of een software-probleem overeenkomt met het patroon.

Gebruik het ontwerppatroon voor de actor in het algemeen model oplossingen voor de volgende problemen met software- of scenario's voor netwerkbeveiliging bij:
-   De adresruimte van uw probleem betrekking heeft op een groot aantal (duizenden of meerdere) van klein, onafhankelijk en gescheiden eenheden van de status en logica.
-   U werkt met één thread objecten waarvoor geen aanzienlijke interactie met externe componenten, inclusief de status opvragen van een set van actoren is vereist.
-   Uw actorexemplaren blokkeren niet aanroepfuncties met onvoorspelbare vertragingen door uitgifte van i/o-bewerkingen.

In Service Fabric, zijn actors geïmplementeerd in het kader van Reliable Actors-toepassing. Dit framework is gebaseerd op de actor-patroon en gebouwd boven [Service Fabric Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction). Elke reliable actor-service die u schrijft is een gepartitioneerde stateful betrouwbare service.

Elke actor wordt gedefinieerd als een exemplaar van een actortype, identiek zijn aan de manier waarop die een .NET-object een exemplaar van een .NET-type is. Bijvoorbeeld, een **actortype** dat implementeert de functionaliteit van een Rekenmachine kan hebben veel actoren van dat type die op verschillende knooppunten in een cluster worden gedistribueerd. Elk van de gedistribueerde actoren wordt een unieke gekenmerkt door een actor-id.

[Replicatie-beveiligingsconfiguraties](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-kvsactorstateprovider-configuration) worden gebruikt voor het beveiligen van het communicatiekanaal dat wordt gebruikt tijdens de replicatie. Deze configuratie wordt voorkomen dat services elkaars replicatieverkeer zien en zorgt ervoor dat de maximaal beschikbare gegevens veilig is. Standaard wordt een lege beveiligingsconfiguratiesectie voorkomen dat replicatiebeveiliging.
Configuraties voor replicatie configureren de replicatie dat verantwoordelijk is voor het maken van de status van de Actor State-Provider zeer betrouwbare.

## <a name="configure-ssl-for-azure-service-fabric"></a>SSL configureren voor Azure Service Fabric
Het verificatieproces server [verifieert](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) eindpunten voor clusterbeheer aan een Beheerclient. De management-client vervolgens herkent dat deze met de echt cluster communiceert. Dit certificaat biedt ook een [SSL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) voor de HTTPS-API en voor Service Fabric Explorer via HTTPS.
U hebt voor uw cluster een aangepaste domeinnaam nodig. Wanneer u een certificaat bij een certificeringsinstantie aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

SSL configureren voor een toepassing, moet u eerst een SSL-certificaat dat is ondertekend door een Certificeringsinstantie verkrijgen. De CA is een vertrouwde derde partij die certificaten voor SSL. Als u nog een SSL-certificaat hebt, moet u aanvragen bij een bedrijf dat SSL-certificaten wordt verkocht.

Het certificaat moet voldoen aan de volgende vereisten voor SSL-certificaten in Azure:
-   Het certificaat moet een persoonlijke sleutel bevatten.

-   Het certificaat moet worden gemaakt voor sleuteluitwisseling en kunnen worden geëxporteerd naar een personal information exchange (PFX)-bestand.

-   Naam van het onderwerp van het certificaat moet overeenkomen met de domeinnaam die wordt gebruikt voor toegang tot uw cloudservice.

    - Een aangepaste domeinnaam te gebruiken voor toegang tot uw cloudservice aan te schaffen.
    - Een certificaat aanvragen van een CA met een onderwerpnaam die overeenkomt met de aangepaste domeinnaam van uw service. Bijvoorbeeld, als uw aangepaste domeinnaam is __contoso__**.com**, het certificaat van uw Certificeringsinstantie moet de onderwerpnaam **. contoso.com** of __www__ **. contoso.com**.

    >[!NOTE]
    >U kunt een SSL-certificaat kan niet verkrijgen van een Certificeringsinstantie voor de __cloudapp__**.net** domein.

-   Het certificaat moet minimaal 2048-bits versleuteling gebruiken.

Het HTTP-protocol is onveilig en afluisteren te maken krijgen. Gegevens die worden verzonden via HTTP wordt verzonden als tekst zonder opmaak vanuit de webbrowser naar de webserver of tussen de andere eindpunten. Aanvallers kunnen worden onderschept en gevoelige gegevens die worden verzonden via HTTP, zoals creditcardgegevens en account aanmeldingen weergeven. Wanneer gegevens worden verzonden of via een browser via HTTPS geboekt, SSL zorgt ervoor dat gevoelige gegevens versleuteld en beveiligd tegen worden onderschept.

Zie voor meer informatie over het gebruik van SSL-certificaten, [SSL configureren voor Azure-toepassingen](https://docs.microsoft.com/azure/cloud-services/cloud-services-configure-ssl-certificate).

## <a name="use-network-isolation-and-security-with-azure-service-fabric"></a>Netwerkisolatie en beveiliging met Azure Service Fabric gebruiken
Een beveiligd cluster 3 nodetype instellen met behulp van de [Azure Resource Manager-sjabloon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) als een voorbeeld. Beheer de binnenkomend en uitgaand verkeer met behulp van de sjabloon en Netwerkbeveiligingsgroepen.

De sjabloon heeft een NSG voor elk van de virtuele-machineschaalsets en wordt gebruikt voor het beheren van het verkeer naar en uit de set. De regels zijn standaard waarmee al het verkeer die nodig zijn voor de systeemservices en de toepassingspoorten opgegeven in de sjabloon geconfigureerd. Controleer deze regels en het aanbrengen van wijzigingen in aan uw behoeften voldoen, zoals het toevoegen van nieuwe regels voor uw toepassingen.

Zie voor meer informatie, [algemene netwerkscenario's voor Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking).

## <a name="set-up-azure-key-vault-for-security"></a>Azure Key Vault instellen voor beveiliging
Service Fabric certificaten gebruikt voor verificatie en versleuteling voor het beveiligen van een cluster en de bijbehorende toepassingen.

Service Fabric maakt gebruik van X.509-certificaten om een cluster beveiligen en om beveiligingsfuncties van toepassing. Gebruik van Azure Key Vault [certificaten beheren](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-update-certs-azure) voor Service Fabric-clusters in Azure. De resourceprovider van Azure waarmee u clusters maakt, haalt de certificaten van een key vault. De provider installeert vervolgens de certificaten op de virtuele machines wanneer het cluster is geïmplementeerd op Azure.

Een certificaat-relatie bestaat tussen [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), de Service Fabric-cluster en de resource-provider die gebruikmaakt van de certificaten. Wanneer het cluster is gemaakt, wordt informatie over de relatie certificaat opgeslagen in een key vault.

Er zijn twee eenvoudige stappen voor het instellen van een key vault:
1. Maak een resourcegroep specifiek voor uw key vault.

    Het is raadzaam dat u de key vault in een eigen resourcegroep plaatsen. Deze actie voorkomt u dat het verlies van gegevens van uw sleutels en geheimen als andere resourcegroepen worden verwijderd, zoals opslag-, compute- of de groep met uw cluster. De resourcegroep waarin uw key vault moet zich in dezelfde regio als het cluster dat wordt gebruikt.

2. Een key vault maakt in de nieuwe resourcegroep.

    De key vault moet zijn ingeschakeld voor implementatie. De compute resourceprovider kunt vervolgens de certificaten ophalen uit de kluis en te installeren op de VM-exemplaren.

Zie voor meer informatie over het instellen van een key vault, [wat is Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).

## <a name="assign-users-to-roles"></a>Gebruikers aan rollen toewijzen
Nadat u de toepassingen voor uw cluster hebt gemaakt, uw gebruikers toewijzen aan de rollen die worden ondersteund door Service Fabric: alleen-lezen en -beheerder. U kunt deze rollen toewijzen met behulp van de Azure-portal.

>[!NOTE]
> Zie voor meer informatie over het gebruik van functies in Service Fabric [Role-Based Access Control voor Service Fabric-clients](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-security-roles).

Azure Service Fabric ondersteunt twee toegangstypen voor clients die zijn verbonden met een [Service Fabric-cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm): beheerder en gebruiker. De Clusterbeheerder kunt Toegangsbeheer gebruiken om te beperken van toegang tot bepaalde bewerkingen voor een cluster voor verschillende groepen gebruikers. Toegangsbeheer maakt het cluster beter te beveiligen.

## <a name="next-steps"></a>Volgende stappen

- [Controlelijst voor service Fabric-beveiliging](azure-service-fabric-security-checklist.md)
- Instellen van uw Service Fabric [ontwikkelomgeving](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started).
- Meer informatie over [ondersteuningsopties voor Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-support).

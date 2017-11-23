---
title: Microsoft Azure-gegevens van versleuteling op Rest | Microsoft Docs
description: Dit artikel bevat een overzicht van Microsoft Azure data encryption in rust, de algemene mogelijkheden en algemene overwegingen.
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: TomSh
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: yurid
ms.openlocfilehash: b02afa77ce99f576fed76b398642ba3f3ce2ba98
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="azure-data-encryption-at-rest"></a>Azure Data Encryption in rust
Er zijn meerdere hulpprogramma's in Microsoft Azure ter bescherming van gegevens op basis van de beveiliging en naleving behoeften van uw bedrijf. Dit artikel is gericht op:
- Hoe gegevens in rust zijn beveiligd via Microsoft Azure
- Worden de verschillende onderdelen die deel uitmaken van de data protection-implementatie
- Voordelen en nadelen van de verschillende Sleutelbeheer beveiliging benaderingen beoordeelt. 

Codering in rust is een algemene beveiligingsvereiste. Een voordeel van Microsoft Azure is dat organisaties versleuteling in rust zonder de kosten voor implementatie en beheer en het risico van een aangepaste sleutel beheersysteem kunnen bereiken. Organisaties hebben de mogelijkheid laten Azure versleuteling in rust volledig te beheren. Bovendien hebben organisaties verschillende opties voor het beheren van nauw versleuteling of versleutelingssleutels.

## <a name="what-is-encryption-at-rest"></a>Wat is versleuteling in rust?
Codering in rust verwijst naar de cryptografische codering (versleuteling) van gegevens wanneer deze wordt bewaard. De versleuteling bij rust ontwerpen in Azure gebruik symmetrische codering voor het versleutelen en ontsleutelen van grote hoeveelheden gegevens snel volgens een eenvoudige conceptuele model:

- Een sleutel symmetrische codering wordt gebruikt om gegevens te versleutelen zoals persistent wordt gemaakt 
- Dezelfde coderingssleutel wordt voor het ontsleutelen van die gegevens, zoals deze is readied voor gebruik in het geheugen gebruikt
- Kan de gegevens worden gepartitioneerd en verschillende sleutels kunnen worden gebruikt voor elke partitie
- Sleutels moeten worden opgeslagen op een veilige locatie met beleid voor toegangsbeheer beperken van toegang tot bepaalde identiteiten en logboekregistratie sleutelgebruik. De gegevensversleutelingssleutels vaak worden versleuteld met asymmetrische versleuteling toegang verder te beperken (beschreven in de *sleutel hiërarchie*verderop in dit artikel)

De bovenstaande beschrijving van de standaardelementen op hoog niveau van versleuteling in rust. In de praktijk vereisen belangrijke scenario's voor beheer en controle, evenals de schaal en beschikbaarheid garanties, extra constructies. Microsoft Azure-versleuteling in de Rest-concepten en -onderdelen worden hieronder beschreven.

## <a name="the-purpose-of-encryption-at-rest"></a>Het doel van versleuteling in rust
Codering in rust is bedoeld om gegevens te beveiligen voor gegevens in rust (zoals hierboven beschreven.) Aanvallen tegen gegevens in rust zijn pogingen voor fysieke toegang tot de hardware waarop de gegevens worden opgeslagen en vervolgens de gegevens in gevaar brengen. Bij een aanval harde schijf van de server kan hebben is kunnen stoten tijdens het onderhoud zodat een aanvaller de harde schijf te verwijderen. Later zou de aanvaller de harde schijf in een computer onder hun beheer proberen toegang tot de gegevens plaatsen. 

Codering in rust is ontworpen om te voorkomen dat de aanvaller toegang hebben tot de niet-versleutelde gegevens doordat de gegevens worden versleuteld op de schijf. Als een aanvaller versleuteld verkrijgen van een harde schijf met een dergelijke gegevens en geen toegang tot de versleutelingssleutels, zou de aanvaller de gegevens zonder goede moeite niet beschadigen. Een aanvaller zou hebben geprobeerd aanvallen tegen de versleutelde gegevens die zijn te veel complexere en resource verbruikt dan de toegang tot gegevens op een harde schijf niet-versleuteld in een dergelijk scenario. Om deze reden versleuteling in rust wordt nadrukkelijk aanbevolen en een hoge prioriteit vereiste voor veel organisaties. 

In sommige gevallen is versleuteling in rust ook vereist voor een organisatie behoefte aan gegevens bestuur en naleving inspanningen. Industrie- en government voorschriften, zoals HIPAA, PCI en FedRAMP, indelen, specifieke garanties met betrekking tot vereisten voor beveiliging en versleuteling van gegevens. Voor veel van deze voorschriften is versleuteling in rust een verplichte meting compatibele gegevensbeheer en-beveiliging vereist. 

Naast de naleving en regelgeving moet versleuteling in rust worden beschouwd als een defense-in-depth-platforms. Microsoft biedt een compatibel platform voor services, toepassingen en gegevens, uitgebreide faciliteit en fysieke beveiliging, data access-beheer en controle, is het belangrijk te bevatten extra beveiligingsmaatregelen 'overlappende' in geval een van de andere veiligheidsmaatregelen mislukt. Codering in rust is dergelijke een extra defense-mechanisme.

Microsoft hecht veel belang aan het bieden van versleuteling op de overige opties op cloudservices om klanten geschikte beheerbaarheid van versleutelingssleutels bieden en toegang tot de logboeken die worden weergegeven wanneer de versleutelingssleutels worden gebruikt. Bovendien werkt op het doel van het maken van alle klantgegevens die standaard in rust versleuteld met Microsoft.

## <a name="azure-encryption-at-rest-components"></a>Versleuteling van Azure in Rest-onderdelen

Zoals eerder beschreven, is het doel van versleuteling in rust dat gegevens die worden bewaard op de schijf worden versleuteld met een codering met geheime sleutel. Als u wilt bereiken die doel beveiligde sleutel maken, opslag, worden access control en beheer van de versleutelingssleutels opgegeven. Hoewel details variëren kunnen, Azure-services versleuteling op Rest-implementaties kunnen worden beschreven in termen van de onderstaande concepten die vervolgens in het volgende diagram worden geïllustreerd.

![Onderdelen](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig1.png)

### <a name="azure-key-vault"></a>Azure Key Vault

De opslaglocatie van de versleutelingssleutels en toegangsbeheer voor die sleutels is de kern van een versleuteling op rest-model. De toetsen moeten zeer beveiligde maar beheerbaar is voor de opgegeven gebruikers en beschikbaar zijn voor specifieke services. Voor Azure-services, Azure Sleutelkluis is de oplossing aanbevolen sleutelopslag en biedt een algemene beheerervaring op services. Sleutels worden opgeslagen en beheerd in sleutelkluizen, en toegang tot een sleutelkluis kan worden opgegeven voor gebruikers of services. Azure Sleutelkluis ondersteunt klant het maken van sleutels of importeren van de klant sleutels voor versleuteling door de klant beheerd belangrijke scenario's.

### <a name="azure-active-directory"></a>Azure Active Directory

Machtigingen voor het gebruik van de sleutels die zijn opgeslagen in Azure Sleutelkluis wilt beheren of toegang tot deze voor versleuteling op Rest versleuteling en ontsleuteling, kunnen worden toegewezen aan Azure Active Directory-accounts. 

### <a name="key-hierarchy"></a>Sleutel hiërarchie

Meer dan één versleutelingssleutel wordt gebruikt in een versleuteling op rest-implementatie. Asymmetrische codering is nuttig voor het tot stand brengen van de vertrouwensrelatie en verificatie nodig voor toegang tot de sleutel en beheer. Symmetrische codering is efficiënter voor bulksgewijze versleuteling en ontsleuteling, waardoor het sterkere versleuteling wilt gebruiken en betere prestaties. Bovendien verlaagt beperking van het gebruik van een enkele versleutelingssleutel het risico dat de sleutel wordt in gevaar komt en de kosten van nogmaals te worden versleuteld wanneer een sleutel moet worden vervangen. Als u wilt gebruikmaken van de voordelen van asymmetrische en symmetrische codering en beperken van het gebruik en de blootstelling van één sleutel, Azure versleutelingen op rest modellen sleutel hiërarchie met bestaat uit de volgende typen sleutels gebruiken:

- **Gegevensversleutelingsleutel (DEK)** – een symmetrische AES256-sleutel gebruikt voor het versleutelen van een partitie of een blok gegevens.  Een enkele bron kan zijn veel partities en veel gegevensversleutelingssleutels. Elk blok van gegevens te versleutelen met een andere sleutel maakt crypto geanalyseerd moeilijker. Toegang tot DEKs is nodig voor de resource provider of de toepassing-exemplaar dat is versleutelen en ontsleutelen van een specifiek blok. Wanneer een DEK is vervangen door een nieuwe sleutel moet alleen de gegevens in de bijbehorende blok opnieuw versleuteld met de nieuwe sleutel zijn.
- **Versleuteling KEK Key Key ()** – een asymmetrische versleutelingssleutel gebruikt voor het versleutelen van de versleutelingssleutels gegevens. Een coderingssleutel Key kunnen de gegevensversleutelingssleutels zelf worden versleuteld en beheerd. De entiteit die toegang tot de KEK heeft mogelijk anders dan de entiteit die de DEK vereist. Hierdoor kan een entiteit voor toegang tot de DEK om beperkte toegang van elk DEK aan specifieke partitie een broker. Aangezien de KEK is vereist voor het ontsleutelen van de DEKs, is de KEK in feite een potentieel waarmee DEKs kunnen worden effectief verwijderd door het verwijderen van de KEK.

De sleutels voor versleuteling, versleuteld met de sleutel versleutelingssleutels afzonderlijk worden opgeslagen en alleen een entity met toegang tot de coderingssleutel Key kunt krijgen tot alle gegevensversleutelingssleutels versleuteld met die sleutel. Verschillende modellen van opslag van sleutels worden ondersteund. Elk model later in de volgende sectie uitvoeriger behandeld.

## <a name="data-encryption-models"></a>Versleuteling gegevensmodellen

Informatie over de verschillende modellen voor versleuteling, en de voor- en nadelen is het essentieel om te begrijpen hoe de versleuteling in rust voor het implementeren van de verschillende resourceproviders in Azure. Deze definities worden gedeeld door alle resourceproviders in Azure om ervoor te zorgen algemene taal en taxonomie. 

Er zijn drie scenario's voor serverzijde versleuteling:

- Serverzijde codering met sleutels Service beheerd
    - De versleuteling en ontsleuteling bewerkingen uitvoeren die Azure Resourceproviders
    - Microsoft beheert de sleutels
    - Volledige cloud-functionaliteit

- Server-side '-codering met behulp van de klant beheerd sleutels in Azure Sleutelkluis
    - De versleuteling en ontsleuteling bewerkingen uitvoeren die Azure Resourceproviders
    - Controleert de klant sleutels via Azure Sleutelkluis
    - Volledige cloud-functionaliteit

- Met behulp van de sleutels door de klant beheerd op door de klant bewaakte hardware serverzijde-versleuteling
    - De versleuteling en ontsleuteling bewerkingen uitvoeren die Azure Resourceproviders
    - Controleert de klant sleutels op door de klant bewaakte hardware
    - Volledige cloud-functionaliteit

Voor client-side '-versleuteling, het volgende overwegen:

- Azure-services zien te ontsleutelen gegevens niet
- Klanten beheren en opslaan van sleutels lokale (of in andere secure winkels). Sleutels zijn niet beschikbaar voor Azure-services
- Cloud verminderde functionaliteit

De ondersteunde versleuteling modellen in Azure splitsen in twee groepen: 'Client codering' en 'Server-side codering' als eerder is vermeld. Houd er rekening mee dat, onafhankelijk van de versleuteling bij rust model gebruikt, Azure-services altijd het gebruik van een beveiligde transport, zoals het TLS- of HTTPS raden. Daarom transport-versleuteling moet worden opgelost door het transportprotocol en mag geen een belangrijke factor om te bepalen welke versleuteling in rest-model te gebruiken.

### <a name="client-encryption-model"></a>Model voor client-versleuteling

Client versleuteling model verwijst naar de codering die buiten de Resource Provider of Azure door de service of de aanroepende toepassing wordt uitgevoerd. De versleuteling kan worden uitgevoerd door de servicetoepassing in Azure of door een toepassing die wordt uitgevoerd in het datacenter van de klant. In beide gevallen wanneer de mogelijkheden van dit model versleuteling, ontvangt de Azure Resource Provider een versleutelde gegevens zonder de mogelijkheid te ontsleutelen van de gegevens in elke manier toegang hebben tot de versleutelingssleutels-blob. In dit model, de key management wordt uitgevoerd door de aanroepende servicetoepassing en ondoorzichtig met de Azure-service is.

![Client](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig2.png)

### <a name="server-side-encryption-model"></a>De model-serverzijde versleuteling

Serverzijde versleuteling modellen verwijzen naar de codering die door de Azure-service wordt uitgevoerd. In dit model voert de Resourceprovider de bewerkingen versleutelen en ontsleutelen. Bijvoorbeeld, Azure Storage gegevens kan ontvangen in tekst zonder opmaak bewerkingen en voert de versleuteling en ontsleuteling intern. De Resourceprovider mogelijk gebruik van de versleutelingssleutels die worden beheerd door Microsoft of door de klant, afhankelijk van de opgegeven configuratie. 

![Server](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig3.png)

### <a name="server-side-encryption-key-management-models"></a>Versleuteling serverzijde Sleutelbeheer modellen

Elk van de versleuteling serverzijde op rest modellen impliceert opvallende kenmerken van Sleutelbeheer. Dit omvat waar en hoe versleutelingssleutels zijn gemaakt en evenals de toegangsmodellen en de procedures van de sleutel worden gedraaid opgeslagen. 

#### <a name="server-side-encryption-using-service-managed-keys"></a>Serverzijde codering met sleutels service beheerd

Voor veel klanten is de essentiële eis om ervoor te zorgen dat de gegevens worden versleuteld wanneer deze zich in rust. Dit model kan serverzijde codering met sleutels voor Service beheerd waardoor klanten de specifieke resource (Storage-Account, SQL-database, etc.) markeren voor versleuteling en alle Sleutelbeheer aspecten zoals sleuteluitgave, draaiing en back-up wordt naar Microsoft. De meeste Azure-Services die ondersteuning bieden voor versleuteling in rust doorgaans ondersteuning voor dit model offloading van het beheer van de versleutelingssleutels in Azure. De Azure-resourceprovider maakt de sleutels, worden deze in de veilige opslag en haalt wanneer dit nodig is. Dit betekent dat de service volledige toegang tot de sleutels heeft en de service volledig beheer over de levenscyclus van Referentiebeheer heeft.

![Beheerd](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig4.png)

Serverzijde versleuteling met behulp van service beheerd sleutels dus snel adressen nodig om versleuteling in rust met weinig overhead aan de klant. Zodra deze beschikbaar wordt een klant doorgaans Hiermee opent u de Azure-portal voor het doelabonnement en de resourceprovider en controleert een vak die aangeeft dat ze zouden graag de gegevens worden versleuteld. In sommige bronbeheer is serverzijde codering met sleutels service beheerd standaard ingeschakeld. 

Codering met sleutels die door Microsoft beheerde serverzijde betekent dat de service heeft volledige toegang tot het opslaan en de sleutels beheert. Terwijl sommige klanten de sleutels beheren omdat ze van mening bent dat ze een betere beveiliging kunnen controleren of wilt kunnen, kunnen de kosten en het risico dat samenhangt met een aangepaste sleutel opslagoplossing moeten worden beschouwd bij het evalueren van dit model. In veel gevallen kan een organisatie bepalen dat resourcebeperkingen of risico's van een on-premises-oplossing, kan dit groter is dan het risico van het cloudbeheer van de versleuteling bij rust sleutels.  Echter kan dit model niet voldoende zijn voor organisaties die moeten voldoen om te bepalen voor het maken en de levenscyclus van de versleutelingssleutels of hebben andere personeel beheren van een service versleutelingssleutels dan die van de service (dat wil zeggen, scheiding van beheren sleutelbeheer van het algemene model voor het beheer voor de service).

##### <a name="key-access"></a>Toegang tot de sleutel

Wanneer serverzijde codering met sleutels Service beheerd wordt gebruikt, de sleutel maken, opslag en toegang tot service worden beheerd door de service. Normaal gesproken de fundamentele Azure resourceproviders de versleutelingssleutels gegevens worden opgeslagen in een archief dat bijna de gegevens en snel beschikbaar en toegankelijk is tijdens de versleutelingssleutels voor de sleutel worden opgeslagen in een beveiligd archief van de interne.

**Voordelen**

- Eenvoudige instellingen
- Microsoft beheert de belangrijkste rotatie, back-up en redundantie
- De klant beschikt niet over de kosten die zijn gekoppeld aan de implementatie of het risico van een aangepaste Sleutelbeheer schema.

**Nadelen**

- Geen controle van de klant over de versleutelingssleutels (sleutelspecificatie, levenscyclus, intrekken, enz.)
- Er is geen mogelijkheid scheiden sleutelbeheer van algemene model voor het beheer voor de service

#### <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Server-side '-codering met behulp van de klant beheerde sleutels in Azure Sleutelkluis 

Voor scenario's waarin de vereiste voor het versleutelen van het data-at-rest en beheer de versleuteling sleutels klanten serverzijde versleuteling met behulp van de klant beheerd sleutels in de Sleutelkluis gebruiken. Sommige services mogelijk de hoofdmap coderingssleutel Key opslaan in Azure Sleutelkluis en de versleutelingssleutel van de versleutelde gegevens opslaan op een interne locatie dichter in de gegevens. In dat scenario klanten hun eigen sleutels meenemen naar Sleutelkluis (BYOK: Breng uw eigen sleutel), of nieuwe te genereren en ze gebruiken voor het versleutelen van de gewenste bronnen. Terwijl de Resourceprovider de versleuteling en ontsleuteling bewerkingen de geconfigureerde sleutel gebruikt als de hoofdsleutel voor alle versleutelingsbewerkingen voert. 

##### <a name="key-access"></a>Toegang tot de sleutel

Het model serverzijde versleuteling met beheer van de klant sleutels in Azure Key Vault omvat de service die toegang tot de sleutels te coderen en decoderen naar behoefte. Versleuteling bij rust sleutels worden gedaan toegankelijk is voor een service via een beleid voor toegangsbeheer. Dit beleid verleent de service-identiteit-toegang voor het ontvangen van de sleutel. Een Azure-service uitgevoerd namens een bijbehorende-abonnement kan worden geconfigureerd met een identiteit in het desbetreffende abonnement. De service kunt Azure Active Directory-verificatie uitvoeren en een verificatietoken voor zichzelf te identificeren als deze service namens het abonnement ontvangen. Dat token kan vervolgens worden gepresenteerd voor Sleutelkluis ophalen van een sleutel die deze toegang is verleend.

Voor bewerkingen met behulp van coderingssleutels, een service-identiteit toegang kan krijgen tot een van de volgende bewerkingen: ontsleutelen, versleutelen, unwrapKey, wrapKey, controleren, meld u aan, ophalen, weergeven, bijwerken, maken, importeren, verwijderen, back-up en herstellen.

Als u wilt ophalen van de service-identiteit in het Resource Manager-service-exemplaar wordt uitgevoerd voor een sleutel voor gebruik in versleutelen of ontsleutelen van gegevens in rust UnwrapKey (voor de sleutel voor ontsleuteling) en hebben WrapKey (in te voegen in de sleutelkluis een sleutel bij het maken van een nieuwe sleutel).


>[!NOTE] 
>Zie voor meer informatie over Sleutelkluis autorisatie het veilig uw sleutelkluis-pagina in de [Azure Key Vault documentatie](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault). 

**Voordelen**

- Volledige controle over de sleutels gebruikt – versleuteling sleutels worden beheerd in de Sleutelkluis van de klant onder het beheer van de klant.
- Mogelijkheid voor het versleutelen van meerdere services met één master
- Sleutelbeheer van algemene model voor het beheer voor de service kunt scheiden
- Service en de locatie van de sleutel kunt over regio's definiëren

**Nadelen**

- De klant heeft volledige verantwoordelijkheid voor het beheer van toegang tot de sleutel
- De klant heeft volledige verantwoordelijkheid voor het beheer van levenscyclus
- Extra overhead voor installatie en configuratie

#### <a name="server-side-encryption-using-service-managed-keys-in-customer-controlled-hardware"></a>Server-side '-codering met behulp van service beheerde sleutels in de hardware van de klant beheerd

Sommige Azure-services inschakelen voor scenario's waarin de vereiste voor het versleutelen van de gegevens in rust en beheren van de sleutels in een eigen opslagplaats buiten het beheer van Microsoft, het model van de Sleutelbeheer Host uw eigen sleutel (HYOK). In dit model, de service moet de sleutel ophalen vanaf een externe site en zijn daarom van invloed op prestaties en beschikbaarheid garanties configuratie complexere is. Bovendien zijn de algehele beveiligingsgaranties van dit model vergelijkbaar met wanneer de sleutels beheerd in Azure Key Vault klant zijn omdat de service toegang tot de DEK tijdens de versleuteling en ontsleuteling bewerkingen heeft.  Dit model is daardoor niet geschikt is voor de meeste organisaties tenzij ze specifieke Sleutelbeheer vereisten waardoor deze hebben. Als gevolg van deze beperkingen bieden de meeste Azure-Services geen ondersteuning voor serverzijde versleuteling met behulp van de server beheerd sleutels in de hardware van de klant beheerd.

##### <a name="key-access"></a>Toegang tot de sleutel

Wanneer sleutels van de service die wordt beheerd in de hardware van de klant beheerd met serverzijde-versleuteling wordt gebruikt behouden de sleutels op een systeem dat is geconfigureerd door de klant. Azure-services die ondersteuning bieden voor dit model bieden een manier om een beveiligde verbinding maken met een klant sleutelarchief opgegeven.

**Voordelen**

- Volledige controle over de basissleutel gebruikte: versleuteling sleutels worden beheerd door een klant opgegeven store
- Mogelijkheid voor het versleutelen van meerdere services met één master
- Sleutelbeheer van algemene model voor het beheer voor de service kunt scheiden
- Service en de locatie van de sleutel kunt over regio's definiëren

**Nadelen**

- Volledige verantwoordelijkheid voor opslag van sleutels, beveiliging, prestaties en beschikbaarheid
- Volledige verantwoordelijkheid voor het beheer van toegang tot de sleutel
- Volledige verantwoordelijkheid voor het beheer van levenscyclus
- Aanzienlijke-installatie, configuratie en lopende onderhoudskosten
- Verbeterde afhankelijkheid van de beschikbaarheid van het netwerk tussen het datacenter van de klant en de Azure-datacenters.

## <a name="encryption-at-rest-in-microsoft-cloud-services"></a>Versleuteling in rust in de Microsoft cloud-services

Microsoft Cloud-services worden gebruikt in alle drie modellen: IaaS, PaaS, SaaS. Hieronder hebt u voorbeelden van hoe ze op elk model passen:

- Software-services genoemd Software als een Server of SaaS waarvoor de toepassing geleverd door de cloud, zoals Office 365.
- Welke klanten gebruikmaken van de cloud in hun toepassingen met behulp van de cloud voor zaken platform-services zoals opslag, analytics en service bus-functionaliteit.
- Infrastructuurservices of -infrastructuur als een Service (IaaS) in welke klant implementeren van besturingssystemen en toepassingen die worden gehost in de cloud en mogelijk andere cloudservices.

### <a name="encryption-at-rest-for-saas-customers"></a>Codering in rust voor SaaS-klanten

Software als een Service (SaaS)-klanten hebben doorgaans versleuteling in rust ingeschakeld of beschikbaar is in elke service. Office 365 beschikt over verschillende opties voor klanten om te controleren of versleuteling in rust inschakelen. Zie voor informatie over Office 365-services gegevens Versleutelingstechnologieën voor Office 365.

### <a name="encryption-at-rest-for-paas-customers"></a>Codering in rust voor PaaS-klanten

Platform als een Service (PaaS) klantgegevens bevindt zich doorgaans in een toepassing kan worden uitgevoerd-omgeving en alle Resourceproviders Azure gebruikt voor het opslaan van gegevens van de klant. Als u wilt zien van de versleuteling in rust onderzoeken opties die beschikbaar zijn in de onderstaande tabel voor de opslag- en -platforms die u gebruikt. Waar wordt ondersteund, worden koppelingen naar instructies voor het inschakelen van versleuteling in rust zijn beschikbaar voor elke resourceprovider. 

### <a name="encryption-at-rest-for-iaas-customers"></a>Codering in rust voor IaaS-klanten

Infrastructuur als een Service (IaaS)-klanten zijn tal van services en toepassingen in gebruik. IaaS-services kunnen inschakelen versleuteling in rust in hun Azure gehoste virtuele machines en VHD's met behulp van Azure Disk Encryption. 

#### <a name="encrypted-storage"></a>Versleutelde opslag

Zoals PaaS, kunnen IaaS-oplossingen gebruikmaken van andere Azure-services waarmee gegevens in rust versleuteld opgeslagen. In dergelijke gevallen kunt u de versleuteling op Rest-ondersteuning inschakelen worden geleverd door elke verbruikte Azure-service. De onderstaande tabel het inventariseren van de primaire opslag, services en platforms van toepassing en het model van versleuteling in rust ondersteund. Waar wordt ondersteund, vindt u koppelingen naar instructies over het inschakelen van versleuteling in rust. 

#### <a name="encrypted-compute"></a>Versleutelde compute

Een volledige codering op Rest-oplossing is vereist dat de gegevens nooit in versleuteld wordt bewaard. In gebruik op een server die de gegevens in het geheugen geladen worden gegevens lokaal vastgehouden op verschillende manieren, met inbegrip van het wisselbestand van Windows, een crashdump en eventuele logboekregistratie die de toepassing mag uitvoeren. IaaS-toepassingen kunnen Azure Disk Encryption gebruikmaken van een Azure IaaS virtuele machine (Windows of Linux) en een virtuele schijf om te controleren of dat deze gegevens in rust versleuteld. 

#### <a name="custom-encryption-at-rest"></a>Aangepaste versleuteling in rust

Het wordt aanbevolen dat waar mogelijk, IaaS toepassingen gebruikmaken van Azure Disk Encryption en versleuteling op Rest-opties die zijn opgegeven door verbruikte Azure-services. In sommige gevallen, zoals onregelmatige versleutelingsvereisten of niet-Azure op basis van opslag, een ontwikkelaar van een IaaS-toepassing moet mogelijk voor het implementeren van versleuteling op rest-zelf. Ontwikkelaars van IaaS-oplossingen beter integreren met Azure verwachtingen voor beheer en de klant dankzij het gebruik van bepaalde onderdelen van Azure. Ontwikkelaars moeten in het bijzonder de Azure Sleutelkluis-service gebruiken voor veilige sleutels, de opslag te bieden, evenals hun klanten te bieden met consistente sleutelbeheeropties met die van de meeste Azure-platformservices. Aangepaste oplossingen moeten bovendien Azure Managed Service-identiteiten gebruiken voor het inschakelen van de service-accounts voor toegang tot de versleutelingssleutels. Zie voor informatie voor ontwikkelaars over Azure Sleutelkluis en de Service-identiteiten beheerd hun respectieve SDK's.

## <a name="azure-resource-providers-encryption-model-support"></a>Ondersteuning voor Azure-resource providers codering model

Microsoft Azure-Services elke ondersteuning voor een of meer van de versleuteling op rest-modellen. Voor sommige services echter een of meer van de modellen versleuteling mogelijk niet van toepassing. Services kunnen bovendien ondersteuning voor deze scenario's op verschillende schema's vrijgeven. Deze sectie beschrijft de versleuteling bij rust ondersteuning op het moment van schrijven van dit voor elk van de belangrijke gegevens voor Azure storage-services.

### <a name="azure-disk-encryption"></a>Azure schijfversleuteling

De klant met behulp van Azure-infrastructuur als een Service (IaaS)-onderdelen versleuteling in rust voor hun IaaS VM's en schijven via Azure Disk Encryption kunnen bereiken. Zie voor meer informatie over Azure-schijf versleuteling de [documentatie voor Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

#### <a name="azure-storage"></a>Azure-opslag

Azure Blob- en -bestanden ondersteunt versleuteling in rust voor serverzijde versleutelde scenario's, evenals versleuteld klantgegevens (client-side '-versleuteling).

- Serverzijde: klanten die gebruikmaken van Azure blob-opslag kunnen versleuteling in rust op elke Azure-opslagaccount resource inschakelen. Eenmaal ingeschakeld serverzijde versleuteling is transparant uitgevoerd tot de toepassing. Zie [Azure Storage Service: versleuteling van gegevens in rust](https://docs.microsoft.com/azure/storage/storage-service-encryption) voor meer informatie.
- Client-side: versleuteling aan clientzijde van Azure Blobs wordt ondersteund. Wanneer met behulp van versleuteling aan clientzijde klanten coderen van de gegevens en de gegevens als een gecodeerde blob uploaden. Sleutelbeheer wordt gedaan door de klant. Zie [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) voor meer informatie.


#### <a name="sql-azure"></a>SQL Azure

SQL Azure ondersteunt momenteel versleuteling in rust voor servicezijde Microsoft beheerd en versleuteling van de client-side '-scenario's.

Ondersteuning voor server op dat moment wordt de versleuteling geleverd via de functie voor SQL Transparent Data Encryption aangeroepen. Wanneer een klant SQL Azure kunt TDE sleutel automatisch worden gemaakt en ze worden beheerd. Codering in rust kan worden ingeschakeld op het niveau van de database en de server. Vanaf juni 2017 [Transparent Data Encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) standaard op nieuwe databases wordt ingeschakeld.

Client-side '-versleuteling van gegevens van de SQL Azure wordt ondersteund door de [altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx) functie. Versleutelde gebruikt altijd een sleutel die zijn gemaakt en opgeslagen door de client. Klanten kunnen de hoofdsleutel opslaan in een certificaatarchief van de Windows Azure Key Vault of een lokale Hardware Security Module. Met behulp van SQL Server Management Studio, kiezen SQL-gebruikers welke sleutel die ze gebruiken willen voor het versleutelen van die kolom.

|                                  |                |                     | **Versleuteling Model**             |                              |        |
|----------------------------------|----------------|---------------------|------------------------------|------------------------------|--------|
|                                  |                |                     |                              |                              | **Client** |
|                                  | **Sleutelbeheer** | **Service beheerd sleutel** | **De klant beheerd in de Sleutelkluis** | **Beheer van de klant On-premises** |        |
| **Opslag- en -Databases**            |                |                     |                              |                              |        |
| Schijf (IaaS)                      |                | -                   | Ja                          | Ja*                         | -      |
| SQL Server (IaaS)                |                | Ja                 | Ja                          | Ja                          | Ja    |
| Azure SQL (PaaS)                 |                | Ja                 | Preview                      | -                            | Ja    |
| Azure-opslag (blokkeren/pagina-BLOB's) |                | Ja                 | Preview                      | -                            | Ja    |
| Azure-opslag (bestanden)            |                | Ja                 | -                            | -                            | -      |
| Azure-opslag (tabellen, wachtrijen)   |                | -                   | -                            | -                            | Ja    |
| Cosmos-DB (Document DB)          |                | Ja                 | -                            | -                            | -      |
| StorSimple                       |                | Ja                 | -                            | -                            | Ja    |
| Back-up                           |                | -                   | -                            | -                            | Ja    |
| **Intelligence en analyses**       |                |                     |                              |                              |        |
| Azure Data Factory               |                | Ja                 | -                            | -                            | -      |
| Azure Machine Learning           |                | -                   | Preview                      | -                            | -      |
| Azure Stream Analytics           |                | Ja                 | -                            | -                            | -      |
| HDInsights (Azure Blob-opslag)  |                | Ja                 | -                            | -                            | -      |
| HDInsights (Data Lake Storage)   |                | Ja                 | -                            | -                            | -      |
| Azure Data Lake Store            |                | Ja                 | Ja                          | -                            | -      |
| Azure Data Catalog               |                | Ja                 | -                            | -                            | -      |
| Power BI                         |                | Ja                 | -                            | -                            | -      |
| **IoT-Services**                     |                |                     |                              |                              |        |
| IoT Hub                          |                | -                   | -                            | -                            | Ja    |
| Service Bus                      |                | Ja (laag Premium)              | -                            | -                            | Ja    |
| Event Hubs                       |                | Ja             | -                            | -                            | -      |


## <a name="conclusion"></a>Conclusie

Bescherming van klantgegevens opgeslagen in Azure-Services is van groot belang aan Microsoft. Alle Azure gehoste services zijn belangrijk voor versleuteling bij rust opties. Fundamentele services zoals Azure Storage, Azure SQL en sleutel analytics en services voor bedrijfsinformatie een al versleuteling wilt gebruiken op de Rest-opties. Sommige van deze services sleutels van de klant beheerd en clientzijde versleuteling ondersteunen, evenals service beheerde sleutels en versleuteling. Microsoft Azure-services zijn grotendeels verbeteren van versleuteling Rest beschikbaarheid en nieuwe opties zijn gepland voor de preview en algemene beschikbaarheid in de komende maanden.


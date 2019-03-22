---
title: Microsoft Azure Data Encryption-at-Rest | Microsoft Docs
description: Dit artikel bevat een overzicht van Microsoft Azure data versleuteling in rust, de algemene functies en algemene overwegingen.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2018
ms.author: barclayn
ms.openlocfilehash: 4ced712b1b2716d85f0366ea892460053db598b8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58076901"
---
# <a name="azure-data-encryption-at-rest"></a>Azure Data Encryption-at-Rest

Microsoft Azure bevat hulpprogramma's voor het beveiligen van gegevens op basis van de beveiligings- en behoeften van uw bedrijf. In dit document gaat in op:

- Beveiligen van gegevens in rust in Microsoft Azure
- De verschillende onderdelen die deel uitmaken van de implementatie van de bescherming van gegevens wordt beschreven
- Beoordelingen voor- en nadelen van de verschillende Sleutelbeheer protection-methoden. 

Versleuteling-at-Rest is een algemene beveiligingsvereiste. In Azure, kunnen organisaties data-at-rest zonder de risico's of kosten van een oplossing voor aangepaste Sleutelbeheer coderen. Organisaties hebben de mogelijkheid om zodat Azure versleuteling-at-Rest volledig te beheren. Organisaties hebben ook verschillende opties voor het beheren van nauw versleuteling of versleutelingssleutels.

## <a name="what-is-encryption-at-rest"></a>Wat is versleuteling-at-rest?

Versleuteling-at-Rest wordt de codering (codering) van de gegevens wanneer deze worden opgeslagen. De versleuteling-at-Rest ontwerpen in Azure symmetrische versleuteling gebruiken voor het versleutelen en ontsleutelen van grote hoeveelheden gegevens snel op basis van een eenvoudige conceptuele model:

- Een versleutelingssleutel symmetrische wordt gebruikt voor het versleutelen van gegevens als ze worden geschreven naar opslag. 
- De dezelfde versleutelingssleutel wordt gebruikt voor het ontsleutelen van die gegevens zoals deze is readied voor gebruik in het geheugen.
- Gegevens kunnen worden gepartitioneerd en verschillende sleutels kunnen worden gebruikt voor elke partitie.
- Sleutels moeten worden opgeslagen op een veilige locatie met toegangsbeheer op basis van identiteit en controlebeleid. Sleutels voor gegevenscodering zijn vaak versleuteld met asymmetrische codering om toegang verder te beperken.

Belangrijke scenario's voor beheer en controle, evenals de schaal en beschikbaarheid garanties, is extra constructies vereist in de praktijk. Microsoft Azure versleuteling-at-Rest-concepten en -onderdelen worden hieronder beschreven.

## <a name="the-purpose-of-encryption-at-rest"></a>Het doel van versleuteling-at-rest

Versleuteling van inactieve beschikt u over gegevensbeveiliging voor opgeslagen gegevens (at-rest). Aanvallen tegen de gegevens in rust zijn pogingen om op te halen van fysieke toegang tot de hardware waarop de gegevens worden opgeslagen en vervolgens de gegevens in gevaar brengen. In een dergelijke aanval, de harde schijf van een server kan hebben is kunnen stoten tijdens onderhoud waardoor een aanvaller kan te verwijderen van de harde schijf. Later zou de aanvaller de harde schijf in een computer, onder hun beheer toegang tot de gegevens wilt plaatsen. 

Versleuteling-at-rest is ontworpen om te voorkomen dat de aanvaller heeft toegang tot de niet-versleutelde gegevens door ervoor te zorgen dat de gegevens worden versleuteld op de schijf. Als een aanvaller een harde schijf met de versleutelde gegevens, maar niet de versleutelingssleutels verkrijgt, moet de aanvaller de versleuteling voor het lezen van de gegevens verslaan. Deze aanval is veel meer complexe en resource gebruiken dan de toegang tot niet-versleutelde gegevens op een harde schijf. Om deze reden versleuteling-at-rest wordt ten zeerste aangeraden en een hoge prioriteit vereiste voor veel organisaties. 

Versleuteling-at-rest kan ook worden vereist door de noodzaak van een organisatie inspanningen voor het beheer en naleving van gegevens. Industrie en de overheid regelgeving zoals HIPAA, PCI en FedRAMP, indelen, specifieke garanties met betrekking tot vereisten voor beveiliging en versleuteling van gegevens. Versleuteling-at-rest is een verplichte meting vereist is voor enkele van de regelgeving wordt nageleefd.

Naast het voldoen aan het nalevings- en regelgeving, biedt versleuteling-at-rest in ingrijpende beveiliging. Microsoft Azure biedt een compatibel platform voor services, toepassingen en gegevens. Het biedt ook uitgebreide faciliteit en fysieke beveiliging, toegangsbeheer voor gegevens en controle. Het is echter belangrijk dat u extra "overlappende" beveiligingsmaatregelen bieden als een van de andere veiligheidsmaatregelen uitvalt en een beveiligingsmaatregel versleuteling at-rest biedt

Microsoft waakt versleuteling-at-rest-opties in cloudservices en zodat klanten de controle van versleutelingssleutels en logboeken van de sleutel gebruiken. Microsoft werkt ook voor het versleutelen van alle gegevens van de klant at-rest standaard.

## <a name="azure-encryption-at-rest-components"></a>Azure versleuteling-at-Rest-onderdelen

Zoals eerder beschreven, is het doel van versleuteling-at-rest dat gegevens die is opgeslagen op schijf worden versleuteld met een codering met geheime sleutel. Als u wilt bereiken die doel beveiligde sleutel maken, moeten opslag, access control en beheer van de versleutelingssleutels worden opgegeven. Hoewel details variëren kunnen, kunnen Azure-services versleuteling op Rest-implementaties worden beschreven in de voorwaarden die worden weergegeven in het volgende diagram.

![Onderdelen](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig1.png)

### <a name="azure-key-vault"></a>Azure Key Vault

De opslaglocatie van de versleutelingssleutels en het beheer van toegang tot die sleutels is essentieel voor een versleuteling-at-rest-model. De sleutels moeten zeer goed beveiligd maar beheerbaar is voor opgegeven gebruikers en beschikbaar zijn voor specifieke services. Voor Azure-services, Azure Key Vault is de oplossing aanbevolen sleutelopslag en biedt een algemene beheerervaring voor services. Sleutels worden opgeslagen en beheerd in de sleutelkluizen, en toegang tot een key vault kan worden verleend aan gebruikers of services. Azure Key Vault ondersteunt klant het maken van sleutels of het importeren van de klant sleutels voor versleuteling door de klant beheerde sleutel scenario's.

### <a name="azure-active-directory"></a>Azure Active Directory

Machtigingen voor het gebruik van de sleutels die zijn opgeslagen in Azure Key Vault, om te beheren of toegang tot deze voor versleuteling op Rest versleuteling en ontsleuteling, kunnen worden besteed aan Azure Active Directory-accounts. 

### <a name="key-hierarchy"></a>Belangrijkste hiërarchie

Meer dan één versleutelingssleutel wordt gebruikt in een versleuteling-at-rest-implementatie. Asymmetrische codering is handig voor het tot stand brengen van de vertrouwensrelatie en de verificatiemethode die nodig zijn voor toegang tot de sleutel en het beheer. Symmetrische codering is efficiënter voor bulksgewijze versleuteling en ontsleuteling, zodat voor nog betere versleuteling en betere prestaties. Beperk het gebruik van een enkele versleutelingssleutel verkleint u het risico dat de sleutel inbreuk wordt gemaakt en de kosten van opnieuw te versleutelen wanneer een sleutel moet worden vervangen. Azure coderingen op rest-modellen gebruiken een sleutel hiërarchie die bestaat uit de volgende soorten sleutels:

- **Gegevens Gegevensversleutelingsleutel (DEK)** : een symmetrische AES256-sleutel die wordt gebruikt voor het versleutelen van een partitie of het gegevensblok.  Een enkele resource mogelijk veel partities en veel sleutels voor gegevenscodering. Elk blok van gegevens versleutelen met een andere sleutel maakt voor crypto-analyse aanvallen moeilijker. Toegang tot DEKs is nodig voor de resource provider of de toepassing-exemplaar dat is versleutelen en ontsleutelen van een bepaald blok. Wanneer een DEK is vervangen door een nieuwe sleutel moet alleen de gegevens in de bijbehorende blok opnieuw versleuteld met de nieuwe sleutel zijn.
- **Sleutel voor Key-versleuteling (KEK)** – een asymmetrische versleutelingssleutel die wordt gebruikt voor het versleutelen van de sleutels voor gegevenscodering. Gebruik van een coderingssleutel kunnen de sleutels voor gegevenscodering zelf worden versleuteld en beheerd. De entiteit die toegang tot de KEK-sleutel heeft is mogelijk anders dan de entiteit die de DEK is vereist. Een entiteit kan toegang tot de DEK te beperken de toegang van elke DEK naar een specifieke partitie broker. Omdat de KEK-sleutel is vereist voor het ontsleutelen van de DEKs, is de KEK-sleutel in feite een single point waarmee DEKs kan worden effectief verwijderd door het verwijderen van de KEK-sleutel.

De sleutels voor gegevenscodering, versleuteld met de sleutel-versleutelingssleutels afzonderlijk worden opgeslagen en alleen een entiteit met toegang tot de Key-versleutelingssleutel krijgt alle sleutels voor gegevenscodering versleuteld met deze sleutel. Verschillende modellen van de opslag van sleutels worden ondersteund. Elk model in meer detail verderop in de volgende sectie wordt besproken.

## <a name="data-encryption-models"></a>Versleuteling van gegevensmodellen

Een goed begrip van de verschillende modellen voor versleuteling en hun voor- en nadelen is het essentieel om te begrijpen hoe de verschillende resourceproviders in Azure implementeren voor versleuteling-at-Rest. Deze definities worden in Azure om ervoor te zorgen algemene taal en taxonomie verdeeld over alle resourceproviders. 

Er zijn drie scenario's voor versleuteling op de server:

- Server-side-versleuteling door de Service beheerde sleutels
    - De versleuteling en ontsleuteling bewerkingen uitvoeren die Azure Resourceproviders
    - Microsoft beheert de sleutels
    - Volledige cloudfunctionaliteit

- Server-side-versleuteling door de klant beheerde sleutels in Azure Key Vault
    - De versleuteling en ontsleuteling bewerkingen uitvoeren die Azure Resourceproviders
    - Hiermee bepaalt u klant sleutels via Azure Key Vault
    - Volledige cloudfunctionaliteit

- Server-side-versleuteling met behulp van de klant beheerde sleutels op de klant beheerde hardware
    - De versleuteling en ontsleuteling bewerkingen uitvoeren die Azure Resourceproviders
    - Hiermee bepaalt u klant sleutels op de klant beheerde hardware
    - Volledige cloudfunctionaliteit

Voor client-side-versleuteling, houd rekening met het volgende:

- Azure-services zien geen ontsleutelde gegevens.
- Klanten beheren en opslaan van sleutels on-premises (of in andere beveiligde winkels). Sleutels zijn niet beschikbaar voor Azure-services
- Verminderde cloudfunctionaliteit

De modellen ondersteunde versleuteling in Azure splitsen in twee groepen: 'Client versleuteling' en 'Server-side Encryption' als eerder is vermeld. Onafhankelijk van de versleuteling-at-rest model dat wordt gebruikt, Azure services altijd raden het gebruik van een veilige transportmethoden zoals TLS- of HTTPS. Daarom codering in transport moet worden opgelost door het transportprotocol en mag geen een belangrijke factor bij het bepalen van welke versleuteling-at-rest-model te gebruiken.

### <a name="client-encryption-model"></a>Model voor client-versleuteling

Client-coderingsmodel verwijst naar versleuteling die door de service of de aanroepende toepassing buiten de Resourceprovider of Azure wordt uitgevoerd. De versleuteling kan worden uitgevoerd door de servicetoepassing in Azure, of door een toepassing die wordt uitgevoerd in het datacenter van de klant. In beide gevallen moet bij het gebruik van dit model versleuteling, ontvangt de Resourceprovider van Azure een versleutelde blob van gegevens zonder de mogelijkheid te decoderen van de gegevens op geen enkele manier toegang hebben tot de versleutelingssleutels. In dit model, de sleutelbeheer wordt uitgevoerd door de aanroepende servicetoepassing en is niet transparant voor de Azure-service.

![Client](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig2.png)

### <a name="server-side-encryption-model"></a>Versleuteling voor server-side '-model

Server-side Encryption modellen verwijzen naar versleuteling die wordt uitgevoerd door de Azure-service. In dit model voert de Resourceprovider de bewerkingen voor versleutelen en ontsleutelen. Bijvoorbeeld Azure Storage kan ontvangen van gegevens in operations in tekst zonder opmaak en neemt de versleuteling en ontsleuteling intern. De Resourceprovider kunt versleutelingssleutels die worden beheerd door Microsoft of door de klant, afhankelijk van de opgegeven configuratie. 

![Server](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig3.png)

### <a name="server-side-encryption-key-management-models"></a>Server-side encryption Sleutelbeheer modellen

Elk van de server-side versleuteling-at-rest modellen impliceert onderscheidende kenmerken van Sleutelbeheer. Dit omvat waar en hoe versleutelingssleutels worden gemaakt en, de toegangsmodellen en de procedures rouleren van de sleutel opgeslagen. 

#### <a name="server-side-encryption-using-service-managed-keys"></a>Server-side-versleuteling door de service beheerde sleutels

Voor veel klanten is de essentiële vereiste om ervoor te zorgen dat de gegevens worden versleuteld wanneer deze zich in rust. Server-side encryption service beheerde sleutels kunnen met dit model door zodat klanten kunnen het markeren van de specifieke resource (Storage-Account, SQL-database, enzovoort) voor versleuteling en het verlaten van alle Sleutelbeheer aspecten zoals sleutel uitgifte, draaien en back-up naar Microsoft . De meeste Azure-Services die ondersteuning bieden voor versleuteling van inactieve doorgaans ondersteuning voor het beheer van de versleutelingssleutels naar Azure-offloading van dit model. De resourceprovider van Azure worden de sleutels gemaakt, plaatst ze in de veilige opslag en worden opgehaald wanneer ze nodig zijn. Dit betekent dat de service volledige toegang tot de sleutels heeft en de service volledige controle over het beheer van de levenscyclus van referenties heeft.

![Beheerd](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig4.png)

Server-side-versleuteling door service beheerde sleutels dus snel adressen hoeft te zijn van versleuteling at-rest met weinig overhead aan de klant. Wanneer deze beschikbaar een klant Hiermee opent u de Azure-portal voor het doelabonnement en de resourceprovider doorgaans en controleert of een vak waarmee wordt aangegeven, zijn ze graag de gegevens moeten worden versleuteld. In sommige Resource Managers is versleuteling aan serverzijde met de service beheerde sleutels standaard ingeschakeld.

Versleuteling aan serverzijde met Microsoft beheerde sleutels betekent dat de service heeft volledige toegang tot het opslaan en beheren van de sleutels. Hoewel sommige klanten de sleutels beheren omdat ze denkt dat ze meer beveiliging toegang wilt kunnen, kunnen de kosten en risico's die zijn gekoppeld aan een oplossing voor opslag van aangepaste sleutels moeten worden overwogen bij het evalueren van dit model. In veel gevallen kan een organisatie bepalen dat resourcebeperkingen of -risico's van een on-premises-oplossing groter dan het risico van cloudbeheer van de versleuteling-at-rest-sleutels zijn kunnen.  Echter, dit model kan niet voldoende zijn voor organisaties die beschikken over de vereisten voor het beheren van het maken en de levenscyclus van de versleutelingssleutels of hebt u verschillende personeel van een service-versleutelingssleutels dan die voor het beheren van de service (dat wil zeggen, de scheiding beheren van Sleutelbeheer van het algemene model voor het beheren van de service).

##### <a name="key-access"></a>Toegang tot de sleutel

Als u versleuteling op de Server met de service beheerde sleutels gebruikt, worden de sleutel maken, opslag en toegang tot de service al beheerd door de service. Normaal gesproken de fundamentele Azure-resource-providers worden de versleutelingssleutels gegevens opgeslagen in een store, die zich in de buurt van de gegevens en snel beschikbaar en toegankelijk is tijdens de versleutelingssleutels voor de sleutel worden opgeslagen in een veilige, interne opslag.

**Voordelen**

- Eenvoudige installatie
- Microsoft beheert rouleren van de sleutel, back-up en redundantie
- De klant beschikt niet over de kosten die gepaard gaan met de implementatie of het risico van een aangepaste Sleutelbeheer-schema.

**Nadelen**

- Geen klant controle over de versleutelingssleutels (sleutelspecificatie, levenscyclus, intrekken, enz.)
- Er is geen mogelijkheid om te segregeer sleutelbeheer van algemene model voor het beheren van de service

#### <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Server-side-versleuteling door de klant beheerde sleutels in Azure Key Vault 

Voor scenario's waarin de vereiste voor het versleutelen van de gegevens in rust en beheren van de versleuteling gebruiken sleutels klanten serverzijde versleuteling door de klant beheerde sleutels in Key Vault. Sommige services mogelijk de hoofdmap Key-versleutelingssleutel in Azure Key Vault opslaan en de versleutelingssleutel van de versleutelde gegevens opslaan in een interne locatie zo dicht mogelijk tot de gegevens. In die scenario klanten hun eigen sleutels te brengen naar Key Vault (BYOK-Bring Your Own Key), of nieuwe labels te genereren en ze gebruiken voor het versleutelen van de gewenste resources. Terwijl de Resource Provider de versleuteling en ontsleuteling bewerkingen voert wordt de geconfigureerde sleutel gebruikt als de hoofdsleutel voor alle versleutelingsbewerkingen. 

##### <a name="key-access"></a>Toegang tot de sleutel

Het model versleuteling op de server met de klant beheerde sleutels in Azure Key Vault bestaat uit de service toegang tot de sleutels voor het versleutelen en ontsleutelen van indien nodig. Versleuteling-at-rest-sleutels worden aangebracht toegankelijk is voor een service via een beleid voor toegangsbeheer. Dit beleid de toegang van de service-identiteit voor het ontvangen van de sleutel, hebben. Een Azure-service uitgevoerd namens een bijbehorende-abonnement kan worden geconfigureerd met een identiteit in het desbetreffende abonnement. De service kunt uitvoeren van Azure Active Directory-verificatie en ontvangen van een verificatietoken zichzelf te identificeren als deze service fungeert namens het abonnement. Dit token kan vervolgens worden gepresenteerd aan Key Vault voor het ophalen van een sleutel die deze toegang is verleend.

Voor bewerkingen met behulp van versleutelingssleutels, een service-identiteit kan toegang worden verleend aan een van de volgende bewerkingen: ontsleutelen, versleutelen, sleutel uitpakken, sleutel inpakken, controleren, ondertekenen, ophalen, lijst, bijwerken, maken, importeren, verwijderen, back-up en herstellen.

Het ophalen van een sleutel voor gebruik in versleutelen of ontsleutelen van gegevens in rust de service-identiteit die het Resource Manager-service-exemplaar wordt uitgevoerd als ze beschikken UnwrapKey (voor de sleutel voor ontsleuteling) en de sleutel inpakken (om een sleutel invoegen in key vault bij het maken van een nieuwe sleutel).


>[!NOTE] 
>Zie voor meer informatie over Key Vault autorisatie de veilige uw sleutelkluis-pagina in de [Azure Key Vault-documentatie](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault). 

**Voordelen**

- Volledige controle over de sleutels gebruikt: versleuteling sleutels worden beheerd in de Sleutelkluis van de klant onder beheer van de klant.
- Mogelijkheid voor het versleutelen van meerdere services met één master
- Kan segregeer sleutelbeheer van algemene model voor het beheren van de service
- Service en de locatie van de sleutel kunt definiëren in regio 's

**Nadelen**

- Klant heeft volledige verantwoordelijkheid voor het beheer van toegang tot de sleutel
- Klant heeft volledige verantwoordelijkheid voor het beheer van levenscyclus
- Extra overhead voor installatie en configuratie

#### <a name="server-side-encryption-using-service-managed-keys-in-customer-controlled-hardware"></a>Server-side-versleuteling door de service beheerde sleutels in de klant beheerde hardware

Sommige Azure-services inschakelen voor het model van de key management Host Your Own Key (HYOK). In deze modus management is handig in situaties waar nodig voor het versleutelen van de gegevens in rust en beheren van de sleutels in een eigen opslagplaats buiten de controle van Microsoft is. In dit model, moet de service de sleutel ophalen van een externe site. Prestaties en beschikbaarheid garanties worden beïnvloed en configuratie is wat ingewikkelder. Bovendien zijn de algehele beveiliging garanties van dit model vergelijkbaar met wanneer de sleutels klant wordt beheerd in Azure Key Vault zijn omdat de service toegang tot de DEK tijdens de bewerkingen voor versleuteling en ontsleuteling heeft.  Dit model is als gevolg hiervan niet geschikt is voor de meeste organisaties tenzij ze beschikken over vereisten voor specifieke Sleutelbeheer. Vanwege deze beperkingen bieden de meeste Azure-Services geen ondersteuning voor serverzijde versleuteling door de server beheerde sleutels in de klant beheerde hardware.

##### <a name="key-access"></a>Toegang tot de sleutel

Wanneer de server-side-versleuteling door de service beheerde sleutels in de klant beheerde hardware wordt gebruikt worden de sleutels onderhouden op een systeem dat is geconfigureerd door de klant. Azure-services die ondersteuning bieden voor dit model bieden een manier tot stand brengen van een beveiligde verbinding met een klant sleutelarchief opgegeven.

**Voordelen**

- Volledige controle over de hoofdsleutel gebruikt: versleuteling sleutels worden beheerd door een klant-store
- Mogelijkheid voor het versleutelen van meerdere services met één master
- Kan segregeer sleutelbeheer van algemene model voor het beheren van de service
- Service en de locatie van de sleutel kunt definiëren in regio 's

**Nadelen**

- Volledige verantwoordelijkheid voor opslag van sleutels, beveiliging, prestaties en beschikbaarheid
- Volledige verantwoordelijkheid voor het beheer van toegang tot de sleutel
- Volledige verantwoordelijkheid voor het beheer van levenscyclus
- Aanzienlijke installatie, configuratie en constant onderhoudskosten
- Verbeterde afhankelijkheid van de beschikbaarheid van het netwerk tussen het datacenter van de klant en de Azure-datacenters.

## <a name="encryption-at-rest-in-microsoft-cloud-services"></a>Versleuteling-at-rest in Microsoft cloud-services

Microsoft Cloud-services worden gebruikt in alle drie modellen: IaaS, PaaS, SaaS. Hieronder bevat voorbeelden van hoe ze op elk model passen:

- Softwareservices, aangeduid als Software als een Server of SaaS, die u opgegeven door de cloud, zoals Office 365-toepassing hebt.
- Platform-services welke klanten gebruikmaken van de cloud in hun toepassingen, met de cloud voor zaken zoals opslag-, analyse- en service bus-functionaliteit.
- Infrastructuurservices of Infrastructure as a Service (IaaS) in welke klant wordt geïmplementeerd voor besturingssystemen en toepassingen die worden gehost in de cloud en mogelijk gebruik te maken van andere cloudservices.

### <a name="encryption-at-rest-for-saas-customers"></a>Versleuteling-at-rest voor SaaS-klanten

Software als een Service (SaaS)-klanten hebben doorgaans versleuteling ' at rest ' ingeschakeld of beschikbaar zijn in elke service. Office 365 beschikt over verschillende opties voor klanten om te controleren of het inschakelen van versleuteling-at-rest. Zie voor meer informatie over Office 365-services [versleuteling in Office 365](https://docs.microsoft.com/office365/securitycompliance/encryption).

### <a name="encryption-at-rest-for-paas-customers"></a>Versleuteling-at-rest voor PaaS-klanten

Platform als de gegevens van de klant van een Service (PaaS) bevindt zich doorgaans in de uitvoeringsomgeving van een toepassing en een Azure-Resourceproviders gebruikt voor het opslaan van gegevens van de klant. Als u wilt zien van de versleuteling-at-rest-opties die beschikbaar zijn, controleert u de onderstaande tabel voor de opslag- en platforms die u gebruikt. Waar dit wordt ondersteund, vindt u koppelingen naar instructies over het inschakelen van versleuteling-at-Rest voor elke resourceprovider. 

### <a name="encryption-at-rest-for-iaas-customers"></a>Versleuteling-at-rest voor IaaS-klanten

Infrastructuur als een Service (IaaS)-klanten kan verschillende services en toepassingen hebben gebruikt. IaaS-services kunnen inschakelen versleuteling-at-rest in hun Azure gehoste virtuele machines en VHD's met Azure Disk Encryption. 

#### <a name="encrypted-storage"></a>Versleutelde opslag

Als PaaS, IaaS-oplossingen kunnen gebruikmaken van andere Azure-services die opslaan van gegevens in rust versleuteld. In dergelijke gevallen kunt u de versleuteling-at-Rest-ondersteuning worden geleverd door verschillende verbruikte Azure-Services. De onderstaande tabel worden de primaire opslag, services, en toepassingsplatforms en het model van versleuteling-at-Rest wordt ondersteund. Waar dit wordt ondersteund, vindt u koppelingen naar instructies over het inschakelen van versleuteling-at-Rest. 

#### <a name="encrypted-compute"></a>Versleutelde compute

Een volledige versleuteling-at-Rest-oplossing is vereist dat de gegevens nooit worden opgeslagen in niet-gecodeerde vorm. Terwijl in gebruik is, op een server die het laden van de gegevens in het geheugen, worden gegevens lokaal vastgehouden op verschillende manieren, met inbegrip van het wisselbestand van Windows, een crashdump en eventuele logboekregistratie die de toepassing kan uitvoeren. IaaS-toepassingen kunnen zodat deze gegevens in rust versleuteld met Azure Disk Encryption op een Azure IaaS virtuele machine (Windows of Linux) en de virtuele schijf gebruiken. 

#### <a name="custom-encryption-at-rest"></a>Aangepaste versleuteling-at-rest

Het wordt aanbevolen dat waar mogelijk, IaaS toepassingen gebruikmaken van Azure Disk Encryption en versleuteling geleverd door verbruikte Azure-services Rest-opties. In sommige gevallen, zoals versleutelingsvereisten onregelmatige of niet-Azure op basis van opslag, een ontwikkelaar van een IaaS-toepassing moet mogelijk voor het implementeren van versleuteling-at-rest-zelf. Ontwikkelaars van IaaS-oplossingen beter integreren met Azure verwachtingen voor beheer en de klant door gebruik te maken van bepaalde Azure-onderdelen. Ontwikkelaars moeten met name de Azure Key Vault-service gebruiken om beveiligde sleutel opslag bieden, evenals hun klanten leveren met consistente sleutelbeheeropties met die van de meeste Azure-platform. Aangepaste oplossingen moeten bovendien Azure-Managed Service-identiteiten gebruiken om in te schakelen van service-accounts voor toegang tot versleutelingssleutels. Zie voor informatie over Azure Key Vault en beheerde Service-identiteiten voor ontwikkelaars, hun respectieve SDK's.

## <a name="azure-resource-providers-encryption-model-support"></a>Ondersteuning voor Azure-resource providers versleuteling model

Microsoft Azure-Services elke ondersteuning voor een of meer van de versleuteling-at-rest-modellen. Voor sommige services echter een of meer van de modellen versleuteling mogelijk niet van toepassing. Voor services die ondersteuning bieden voor belangrijke scenario's door de klant beheerde, ondersteunen ze slechts een subset van de typen sleutels die ondersteuning biedt voor Azure Key Vault voor sleutels key-versleuteling. Services kunnen bovendien ondersteuning voor deze scenario's en sleuteltypen op verschillende planningen vrijgeven. Deze sectie beschrijft de versleuteling-at-rest-ondersteuning op het moment van dit artikel is geschreven voor elk van de belangrijkste Azure services voor gegevensopslag.

### <a name="azure-disk-encryption"></a>Azure disk encryption

Elke klant met behulp van Azure-infrastructuur als een Service (IaaS)-functies versleuteling-at-rest voor hun IaaS-VM's en schijven via Azure Disk Encryption kunnen bereiken. Zie voor meer informatie over Azure Disk encryption de [documentatie voor Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

#### <a name="azure-storage"></a>Azure Storage

Alle services van Azure Storage (Blob storage, Queue storage, Table storage en Azure Files) ondersteuning voor serverzijde versleuteling-at-rest, met bepaalde services door de klant beheerde sleutels en client-side-versleuteling ondersteunen.  

- Serverzijde: Alle Azure Storage-Services inschakelen-serverzijde codering standaard door de service beheerde sleutels, die is transparant voor de toepassing. Zie voor meer informatie, [Azure Storage-Serviceversleuteling voor Data-at-Rest](https://docs.microsoft.com/azure/storage/storage-service-encryption). Azure Blob storage en Azure Files ook ondersteuning voor RSA 2048-bits door de klant beheerde sleutels in Azure Key Vault. Zie voor meer informatie, [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).
- Client-side: Ondersteuning voor versleuteling van de client-side Azure Blobs, tabellen en wachtrijen. Wanneer u versleuteling op de client, wordt klanten versleutelen van de gegevens en de gegevens als een versleutelde blob uploaden. Sleutelbeheer wordt uitgevoerd door de klant. Zie voor meer informatie, [Client-Side-versleuteling en Microsoft Azure Storage in Azure Key Vault](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).


#### <a name="azure-sql-database"></a>Azure SQL Database

Azure SQL Database ondersteunt momenteel versleuteling-at-rest voor beheerd door Microsoft servicezijde en versleuteling van de client-side '-scenario's.

Ondersteuning voor serverversleuteling is momenteel beschikbaar via de functie voor SQL Transparent Data Encryption met de naam. Wanneer een klant met een Azure SQL Database kan worden automatisch TDE sleutel gemaakt en beheerd voor hen. Versleuteling-at-rest kan worden ingeschakeld op het niveau van de database en server. Vanaf juni 2017, [transparante gegevensversleuteling (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) is standaard ingeschakeld op nieuwe databases. Azure SQL Database ondersteunt RSA 2048-bits door de klant beheerde sleutels in Azure Key Vault. Zie voor meer informatie, [Transparent Data Encryption met Bring Your Own Key-ondersteuning voor Azure SQL Database en Data Warehouse](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-byok-azure-sql?view=azuresqldb-current).

Client-side-versleuteling van gegevens uit een Azure SQL Database is alleen beschikbaar via de [Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx) functie. Versleutelde maakt altijd gebruik van een sleutel die is gemaakt en opgeslagen door de client. Klanten kunnen de hoofdsleutel opslaan in een Windows-certificaatarchief, Azure Key Vault of een lokale Hardware Security Module. Met behulp van SQL Server Management Studio, kiezen SQL-gebruikers welke sleutel ze graag willen gebruiken voor het versleutelen van die kolom.

|                                  |                    | **Model voor versleuteling en sleutelbeheer** |                    |
|----------------------------------|--------------------|-----------------------------------------|--------------------|
|                                  | **Met behulp van de Service beheerde sleutel voor serverzijde**     | **Server-Side met behulp van de klant beheerd in Key Vault**             | **Client-Side met behulp van Client worden beheerd**      |
| **Opslag en Databases**        |                    |                    |                    |
| Disk (IaaS)                      | -                  | Ja, RSA 2048-bits  | -                  |
| SQL Server (IaaS)                | Ja                | Ja, RSA 2048-bits  | Ja                |
| Azure SQL (Database/Data Warehouse) | Ja                | Ja, RSA 2048-bits  | Ja                |
| Azure SQL (Database beheerd exemplaar) | Ja                | Preview-versie, RSA 2048-bits  | Ja                |
| Azure-opslag (blok-/ pagina-Blobs) | Ja                | Ja, RSA 2048-bits  | Ja                |
| Azure Storage (bestanden)            | Ja                | Ja, RSA 2048-bits  | -                  |
| Azure Storage (tabellen, wachtrijen)   | Ja                | -                  | Ja                |
| Cosmos DB (Documentdb)          | Ja                | -                  | -                  |
| StorSimple                       | Ja                | -                  | Ja                |
| Backup                           | Ja                | -                  | Ja                |
| **Intelligence en analyse**   |                    |                    |                    |
| Azure Data Factory               | Ja                | -                  | -                  |
| Azure Machine Learning           | -                  | Preview-versie, RSA 2048-bits | -                  |
| Azure Stream Analytics           | Ja                | -                  | -                  |
| HDInsight (Azure Blob Storage)   | Ja                | -                  | -                  |
| HDInsight (Data Lake Storage)    | Ja                | -                  | -                  |
| Apache Kafka voor HDInsight       | Ja                | Alle RSA lengtes Preview-versie | -                  |
| Azure Data Lake Store            | Ja                | Ja, RSA 2048-bits  | -                  |
| Azure Data Catalog               | Ja                | -                  | -                  |
| Power BI                         | Ja                | -                  | -                  |
| **IoT-Services**                 |                    |                    |                    |
| IoT Hub                          | -                  | -                  | Ja                |
| Service Bus                      | Ja                | -                  | Ja                |
| Event Hubs                       | Ja                | -                  | -                  |
| Event Grid                       | Ja                | -                  | -                  |


## <a name="conclusion"></a>Conclusie

Bescherming van gegevens van de klant die zijn opgeslagen in Azure-Services is van cruciaal belang naar Microsoft. Alle Azure gehoste services vinden het belangrijk dat versleuteling op Rest-opties zijn. Fundamentele services zoals Azure Storage, Azure SQL Database, en belangrijke analysescenario en informatieservices maken al bieden van versleuteling op Rest-opties. Enkele van deze services ondersteunen klantgegevens worden sleutels en clientzijde, evenals service beheerde sleutels en versleuteling. Microsoft Azure-services zijn grotendeels verbeteren van de versleuteling op beschikbaarheid van Rest en nieuwe opties zijn gepland voor de Preview-versie en de algemene beschikbaarheid in de komende maanden.

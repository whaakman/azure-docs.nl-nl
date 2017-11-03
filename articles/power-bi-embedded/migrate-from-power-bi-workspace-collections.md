---
title: Power BI-Werkruimteverzameling inhoud migreren naar Power BI Embedded | Microsoft Docs
description: Informatie over het migreren van Power BI werkruimte verzamelingen naar Power BI Embedded en gebruikmaken van ontwikkelingen voor het insluiten in apps.
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: 069f31c8213bd0d8586f7ca50e543acfdad8a2b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-migrate-power-bi-workspace-collection-content-to-power-bi-embedded"></a>Power BI-Werkruimteverzameling inhoud migreren naar Power BI Embedded

Informatie over het migreren van Power BI werkruimte verzamelingen naar Power BI Embedded. In dit artikel bevat richtlijnen voor het migreren van de Azure Power BI werkruimte verzamelingen naar Power BI Embedded. We ook kijken wat ze kunnen verwachten voor wijzigingen in de toepassing.

De Power BI werkruimte verzamelingen resource blijft beschikbaar voor een beperkte periode na algemene beschikbaarheid van de Power BI Premium release. Klanten onder een Enterprise Agreement hebben toegang tot hun bestaande werkruimte verzamelingen via de vervaldatum van hun bestaande overeenkomsten. Klanten die verzamelingen van Power BI werkruimte via directe of CSP kanalen verkregen Profiteer van toegang tot een jaar na algemene beschikbaarheid van Power BI Premium.

> [!IMPORTANT]
> Tijdens de migratie een afhankelijkheid van de Power BI-service duurt, er is niet een afhankelijkheid op Power BI voor de gebruikers van uw toepassing bij gebruik van een **insluiten token**. Ze hoeft niet aanmelden voor Power BI om weer te geven van de ingesloten inhoud in uw toepassing. U kunt deze benadering insluiten insluiten van Power BI voor uw klanten.

![Power BI Embedded stroom](media/migrate-from-power-bi-workspace-collections/powerbi-embed-flow.png)

## <a name="prepare-for-the-migration"></a>Voorbereiden voor de migratie

Er zijn enkele dingen die u doen om voor te bereiden moet voor het migreren van Power BI werkruimte verzamelingen service via naar Power BI Embedded. U moet een tenant die beschikbaar is, samen met een gebruiker die een licentie voor Power BI Pro heeft.

1. Zorg ervoor dat u toegang hebt tot een tenant van Azure Active Directory (Azure AD).

    Welke tenant te gebruiken?

    * Gebruik uw bestaande zakelijke Power BI-tenant?
    * Gebruik een afzonderlijke tenant voor uw toepassing?
    * Gebruik een afzonderlijke tenant voor elke klant?

    Als u besluit om te maken van een nieuwe tenant voor uw toepassing of elke klant, Zie het volgende:

    * [Een Azure Active Directory-tenant maken](https://powerbi.microsoft.com/documentation/powerbi-developer-create-an-azure-active-directory-tenant/)
    * [Een Azure Active Directory-tenant verkrijgen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant).

2. Een gebruiker binnen deze nieuwe tenant die als uw toepassing 'master'-account fungeert maken. Account moet zich aanmelden voor Power BI en moet beschikken over een Power BI Pro licentie toegewezen.

## <a name="accounts-within-azure-ad"></a>Accounts in Azure AD

De volgende accounts moeten bestaan binnen uw tenant.

> [!NOTE]
> Deze accounts moeten Power BI Pro licenties hebt om te kunnen gebruiken van App-werkruimten.

1. Een beheerder van de tenant.

    Het wordt aanbevolen dat de insluiten app werkruimte de tenantbeheerder vermeld als een lid heeft.

2. Accounts voor analisten die inhoud maken.

    Deze gebruikers moeten worden toegewezen aan de app werkruimten, indien nodig.

3. Een toepassing *master* gebruikersaccount of serviceaccount.

    De back-end toepassingen slaat de referenties voor dit account. Gebruik de *master* account voor het verkrijgen van een Azure AD-token voor gebruik met de Power BI REST-API's. Dit account wordt gebruikt voor het genereren van de insluittoken voor de toepassing. De *master* account moet een beheerder zijn van de app werkruimten die zijn gemaakt voor het insluiten van.

    **Dit account is slechts een normaal gebruikersaccount in uw organisatie die wordt gebruikt voor de doeleinden voor het insluiten van.**

## <a name="app-registration-and-permissions"></a>App-registratie en machtigingen

Als u de REST API-aanroepen, moet u een toepassing registreren met Azure AD. Aanvullende configuratie is toegepast in de Microsoft Azure-portal naast de registratiepagina van de Power BI-app. Zie voor meer informatie [registreren van een Azure AD-app Power BI-inhoud insluiten](https://powerbi.microsoft.com/documentation/powerbi-developer-register-app/).

Aangeraden wordt om de toepassing met behulp van de toepassing te registreren **master** account.

## <a name="create-app-workspaces-required"></a>Maken van app-werkruimten (vereist)

Als uw toepassing onderhoud aan meerdere klanten, kunt u profiteren van app-werkruimten om betere isolatie te bieden. Dashboards en rapporten zou worden geïsoleerd tussen uw klanten. U kunt vervolgens een Power BI-account per app werkruimte verder isoleren van toepassingen mogelijk tussen uw klanten, maar u kunt slechts één account gebruiken voor het eenvoudig te houden.

> [!IMPORTANT]
> U kunt een persoonlijke werkruimte niet gebruiken (een 'Mijn werkruimte') om te profiteren van het insluiten van uw klanten.

U moet een gebruiker die een licentie Pro heeft om te kunnen maken van een app-werkruimte in Power BI. De Power BI-gebruiker die de App-werkruimte maakt de beheerder die werkruimte standaard is. **De toepassing *master* account moet een beheerder zijn van de werkruimte.**

## <a name="content-migration"></a>Migratie van inhoud

Migreren van de inhoud van uw werkruimte-verzamelingen naar Power BI Embedded gelijktijdig naar uw huidige oplossing kunnen worden uitgevoerd en u hoeft geen downtime.

Een **hulpprogramma voor migratie van** is beschikbaar voor u moet gebruiken om u te helpen bij het kopiëren van inhoud van Power BI werkruimte verzamelingen naar Power BI Embedded. Met name wanneer er veel rapporten. Zie voor meer informatie [hulpprogramma voor migratie van Power BI Embedded](migrate-tool.md).

Migratie van inhoud afhankelijk is hoofdzakelijk twee API's.

1. Downloaden PBIX - deze API kan PBIX-bestanden die worden geüpload naar Power BI na oktober 2016 downloaden.
2. Een PBIX importeren PBIX - deze API geüpload naar Power BI.

Zie voor een aantal codefragmenten gerelateerd, [codefragmenten voor het migreren van inhoud in Power BI Embedded](migrate-code-snippets.md).

### <a name="report-types"></a>Rapporttypen

Er zijn verschillende soorten rapporten, elke vereisen een stroom van verschillende migratiescenario.

#### <a name="cached-dataset-and-report"></a>In de cache gegevensset en het rapport

In de cache gegevenssets verwijzen naar PBIX-bestanden die gegevens in plaats van een live-verbinding of DirectQuery verbinding had geïmporteerd.

**Stroom**

1. Download PBIX-API aanroepen vanuit uw Power BI-Werkruimteverzameling-werkruimte.
2. PBIX opslaan.
3. Importeren PBIX-aanroep voor uw Power BI Embedded werkruimte.

#### <a name="directquery-dataset-and-report"></a>DirectQuery gegevensset en het rapport

**Stroom**

1. Aanroepen van GET-https://api.powerbi.com/v1.0/collections/ {collection_id}/workspaces/{wid}/datasets/{dataset_id}/Default.GetBoundGatewayDataSources en ontvangen verbindingsreeks op te slaan.
2. Download PBIX-API aanroepen vanuit uw Power BI-Werkruimteverzameling-werkruimte.
3. PBIX opslaan.
4. Importeren PBIX-aanroep voor uw Power BI Embedded werkruimte.
5. Verbindingsreeks bijwerken door aan te roepen - https://api.powerbi.com/v1.0/myorg/datasets/ {dataset_id}/Default.SetAllConnections plaatsen
6. GW-ID en een datasource-ID ophalen door aan te roepen: https://api.powerbi.com/v1.0/myorg/datasets/ {dataset_id}/Default.GetBoundGatewayDataSources OPVRAGEN
7. Bijwerken van de referenties van gebruiker door aan te roepen - PATCH https://api.powerbi.com/v1.0/myorg/gateways/ {gateway_id} /datasources/ {datasource_id}

#### <a name="old-dataset-and-reports"></a>Oude gegevensset en rapporten

Rapporten worden geüpload, voordat oktober 2016 geen ondersteuning voor de functie PBIX downloaden.

**Stroom**

1. PBIX ophalen van uw ontwikkelomgeving (uw interne bronbeheer).
2. Importeren PBIX-aanroep voor uw Power BI Embedded werkruimte.

#### <a name="push-dataset-and-report"></a>Push-gegevensset en het rapport

Download PBIX biedt geen ondersteuning voor *Push API* gegevenssets. API-gegevensset gegevens kunnen niet worden overgezet van Power BI werkruimte verzamelingen pushen naar Power BI Embedded.

**Stroom**

1. Aanroepen 'Gegevensset maken' API met gegevensset Json om gegevensset voor uw Power BI Embedded werkruimte te maken.
2. Rapport voor het gemaakte gegevensset * opnieuw worden opgebouwd.

Het is mogelijk gebruik van deze tips voor het migreren van de push-bewerking api rapport van Power BI werkruimte verzamelingen naar Power BI Embedded door het volgende:

1. Sommige dummy PBIX uploaden naar uw Power BI-Werkruimteverzameling-werkruimte.
2. De push-bewerking klonen api rapporteren en bindt dit aan de dummy PBIX uit stap 1.
3. API-rapport met de dummy PBIX push downloaden.
4. Dummy PBIX uploaden naar uw Power BI Embedded werkruimte.
5. Push-gegevensset maken in uw Power BI Embedded werkruimte.
6. Wijs de binding rapport voor de push-api-gegevensset.

## <a name="create-and-upload-new-reports"></a>Maken en nieuwe rapporten uploaden

U kunt naast de inhoud die u hebt gemigreerd van Power BI werkruimte verzamelingen, rapporten en gegevenssets die een Power BI Desktop maken en vervolgens publiceert deze rapporten naar een app-werkruimte. De eindgebruiker publiceren van de rapporten moet een licentie voor Power BI Pro hebben om te kunnen publiceren naar een app-werkruimte.

## <a name="rebuild-your-application"></a>Uw toepassing opnieuw bouwt

1. Wijzigen van uw toepassing met de Power BI REST-API's en de Rapportlocatie in powerbi.com.

2. Opnieuw samenstellen uw verificatieprocessen/AuthZ verificatie met behulp van de *master* account voor uw toepassing. U kunt profiteren van het gebruik van een [insluiten token](https://msdn.microsoft.com/library/mt784614.aspx) zodat deze gebruiker te handelen namens andere gebruikers.

3. Sluit uw rapporten uit Power BI Embedded in uw toepassing. Zie voor meer informatie [insluiten uw Power BI-dashboards, rapporten en tegels](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

## <a name="map-your-users-to-a-power-bi-user"></a>Uw gebruikers toewijzen aan een Power BI-gebruiker

In uw toepassing, wijst u gebruikers die u in de toepassing naar beheert een *master* Power BI-referentie voor de doeleinden van uw toepassing. De referenties voor dit Power BI *master* account zijn opgeslagen in uw toepassing en worden gebruikt voor het maken van sluit tokens.

## <a name="what-to-do-when-you-are-ready-for-production"></a>Wat te doen wanneer u gereed voor productie bent

Wanneer u gereed om te verplaatsen naar de productie bent, moet u het volgende doen:

- Als u van een afzonderlijke tenant voor ontwikkeling gebruikmaakt, moet u ervoor zorgen dat uw app werkruimten, samen met dashboards en rapporten, zijn beschikbaar in uw productieomgeving. Zorg ervoor dat u de toepassing in Azure AD hebt gemaakt voor uw productie-tenant en de juiste app-machtigingen toegewezen, zoals aangegeven in stap 1.

- Schaf een capaciteit die past bij uw behoeften. U kunt de [ingesloten analytics capaciteitsplanning technisch document](https://aka.ms/pbiewhitepaper) voor meer informatie over wat u mogelijk nodig hebt. Wanneer u klaar om aan te schaffen bent, kunt u een Power BI Embedded bron binnen de Azure-portal kunt aanschaffen.

- De werkruimte App bewerken en toewijzen aan een capaciteit onder Geavanceerde.

    ![App-werkruimte toewijzen aan een capaciteit in powerbi.com](media/migrate-from-power-bi-workspace-collections/embedded-capacity.png)

- Uw bijgewerkte toepassing implementeert naar productie en beginnen met het insluiten van rapporten uit Power BI Embedded.

## <a name="after-migration"></a>Na de migratie

Sommige opschoning is vereist in Power BI werkruimte verzamelingen.

- Verwijder alle werkruimten afmeldt bij de geïmplementeerde oplossing binnen de Azure-service van Power BI werkruimte verzamelingen.
- Verwijder eventuele werkruimte verzamelingen die zijn opgeslagen in Azure.

## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd! Uw toepassing is nu gemigreerd naar Power BI Embedded. Zie voor meer informatie over het insluiten van uw Power BI-dashboards, rapporten en gegevenssets [insluiten uw Power BI-dashboards, rapporten en tegels](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Nog vragen? [Probeer de Power BI-Community vragen](http://community.powerbi.com/)
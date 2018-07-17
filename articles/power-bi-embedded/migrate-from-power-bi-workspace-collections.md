---
title: Inhoud van Power BI-Werkruimteverzameling migreren naar Power BI Embedded | Microsoft Docs
description: Informatie over het migreren van Power BI-Werkruimteverzamelingen naar Power BI Embedded en maak gebruik van gebruikmaakt van ontwikkelingen voor het insluiten van inhoud in apps.
services: power-bi-embedded
documentationcenter: ''
author: markingmyname
manager: kfile
editor: ''
tags: ''
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: maghan
ms.openlocfilehash: de20d532112ca73f34f7cb603d043579c28179d6
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071229"
---
# <a name="how-to-migrate-power-bi-workspace-collection-content-to-power-bi-embedded"></a>Inhoud van Power BI-Werkruimteverzameling migreren naar Power BI Embedded

Informatie over het migreren van Power BI-Werkruimteverzamelingen naar Power BI Embedded. In dit artikel bevat richtlijnen voor het migreren van de Azure Power BI-Werkruimteverzamelingen naar Power BI Embedded. We ook kijken wat u kunt verwachten bij wijzigingen in de toepassing.

De Power BI Workspace Collections-resource nog steeds beschikbaar voor een beperkte periode na de release voor algemene beschikbaarheid van Power BI Premium. Klanten met een Enterprise Agreement hebben toegang tot hun bestaande werkruimteverzamelingen tot de vervaldatum van hun bestaande overeenkomsten. Klanten die Power BI Workspace Collections via directe of CSP-kanalen verkregen Profiteer van toegang voor één jaar na de algemene beschikbaarheid van Power BI Premium.

> [!IMPORTANT]
> Tijdens de migratie wordt afhankelijk van de Power BI-service, er is een afhankelijkheid op Power BI voor de gebruikers van uw toepassing bij het gebruik van een **insluittoken**. Ze hoeft niet te registreren voor Power BI om de ingesloten inhoud in uw toepassing weer te geven. U kunt deze aanpak voor het insluiten van inhoud voor Power BI insluiten voor uw klanten.

![Stroom van Power BI Embedded](media/migrate-from-power-bi-workspace-collections/powerbi-embed-flow.png)

## <a name="prepare-for-the-migration"></a>Voorbereiden voor de migratie

Er zijn enkele dingen die u doen om voor te bereiden moet voor het migreren van Power BI Workspace Collections-service via naar Power BI Embedded. U moet een tenant beschikbaar, samen met een gebruiker die een Power BI Pro-licentie heeft.

1. Zorg ervoor dat u toegang hebt tot een tenant Azure Active Directory (Azure AD).

    Welke tenant te gebruiken?

    * Gebruik uw bestaande zakelijke Power BI-tenant?
    * Een afzonderlijke tenant voor uw toepassing gebruiken?
    * Een afzonderlijke tenant gebruiken voor elke klant?

    Als u besluit te maken van een nieuwe tenant voor uw toepassing, of elke klant, ziet u een van de volgende:

    * [Een Azure Active Directory-tenant maken](https://powerbi.microsoft.com/documentation/powerbi-developer-create-an-azure-active-directory-tenant/)
    * [Een Azure Active Directory-tenant verkrijgen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant).

2. Een gebruiker binnen deze nieuwe tenant die als uw toepassing 'master'-account fungeert maken. Account moet zich aanmelden voor Power BI en moet beschikken over een Power BI Pro licentie zijn toegewezen.

## <a name="accounts-within-azure-ad"></a>Accounts in Azure AD

De volgende accounts moeten binnen uw tenant aanwezig.

> [!NOTE]
> Deze accounts moeten Power BI Pro-licenties hebben om te kunnen gebruiken van App-werkruimten.

1. Een tenantbeheerder.

    Het wordt aanbevolen dat de ingesloten app-werkruimte als lid van een tenant-beheerder heeft.

2. Accounts voor analisten die inhoud maken.

    Deze gebruikers moeten worden toegewezen aan de app-werkruimten, indien nodig.

3. Een toepassing *master* gebruikersaccount of de service-account.

    De toepassingen back-end slaat de referenties voor dit account. Gebruik de *master* account voor het verkrijgen van een Azure AD-token voor gebruik met de Power BI REST-API's. Dit account wordt gebruikt voor het genereren van het insluittoken voor de toepassing. De *master* account moet een beheerder van de app-werkruimten die zijn gemaakt voor het insluiten van inhoud.

    **Dit account is alleen een normaal gebruikersaccount in uw organisatie die wordt gebruikt voor het doel van het insluiten van inhoud.**

## <a name="app-registration-and-permissions"></a>App-registratie en machtigingen

Als u REST API-aanroepen, moet u een toepassing registreren met Azure AD. Aanvullende configuratie is toegepast in de Microsoft Azure-portal naast de registratiepagina van de Power BI-app. Zie voor meer informatie, [registreren van een Azure AD-app voor het insluiten van Power BI-inhoud](https://powerbi.microsoft.com/documentation/powerbi-developer-register-app/).

De aanbeveling is het registreren van de toepassing met behulp van de toepassing **master** account.

## <a name="create-app-workspaces-required"></a>App-werkruimten (vereist) maken

Als uw toepassing onderhoud aan meerdere klanten, kunt u profiteren van app-werkruimten om betere isolatie te bieden. Dashboards en rapporten zou worden geïsoleerd van uw klanten. U kunt vervolgens Power BI-account per app-werkruimte gebruiken voor het isoleren van verdere toepassingen mogelijk tussen uw klanten, maar u kunt slechts één account gebruiken om het eenvoudig te laten.

> [!IMPORTANT]
> U kunt een persoonlijke werkruimte niet gebruiken (een 'Mijn werkruimte') om te profiteren van het insluiten van uw klanten.

U moet een gebruiker die een Pro-licentie heeft om te kunnen maken van een app-werkruimte in Power BI. De Power BI-gebruiker die wordt gemaakt van de App-werkruimte is een beheerder van deze werkruimte standaard. **De toepassing *master* account moet een beheerder van de werkruimte.**

## <a name="content-migration"></a>Migratie van inhoud

Uw inhoud migreren van uw werkruimteverzamelingen naar Power BI Embedded parallel met uw huidige oplossing kan worden uitgevoerd en vereist geen uitvaltijd.

Een **hulpprogramma voor migratie van** is beschikbaar voor u kunt gebruiken om u te helpen bij het kopiëren van inhoud van Power BI-Werkruimteverzamelingen naar Power BI Embedded. Met name als er veel rapporten zijn. Zie voor meer informatie, [hulpprogramma voor migratie van Power BI Embedded](migrate-tool.md).

Migratie van inhoud gebeurt hoofdzakelijk via twee API's.

1. Download PBIX: deze API kan PBIX-bestanden die na oktober 2016 zijn geüpload naar Power BI kunt downloaden.
2. Import PBIX: deze API worden alle PBIX geüpload naar Power BI.

Zie voor een aantal gerelateerde codefragmenten, [codefragmenten voor het migreren van inhoud in Power BI Embedded](migrate-code-snippets.md).

### <a name="report-types"></a>Rapporttypen

Er zijn verschillende soorten rapporten, elke vereisen een verschillende migratiestroom.

#### <a name="cached-dataset-and-report"></a>In de cache opgeslagen gegevensset en rapport

In de cache opgeslagen gegevenssets verwijzen naar PBIX-bestanden die gegevens in plaats van een live-verbinding of DirectQuery-verbinding had geïmporteerd.

**Stroom**

1. Download PBIX API aanroepen vanuit uw werkruimte Power BI-Werkruimteverzameling.
2. Sla de PBIX op.
3. Roep de Import PBIX voor uw Power BI Embedded-werkruimte.

#### <a name="directquery-dataset-and-report"></a>DirectQuery-gegevensset en rapport

**Stroom**

1. Roep GET `https://api.powerbi.com/v1.0/collections/{collection_id}/workspaces/{wid}/datasets/{dataset_id}/Default.GetBoundGatewayDataSources` en sla de verbindingsreeks is ontvangen.
2. Download PBIX API aanroepen vanuit uw werkruimte Power BI-Werkruimteverzameling.
3. Sla de PBIX op.
4. Roep de Import PBIX voor uw Power BI Embedded-werkruimte.
5. Verbindingsreeks bijwerken door aanroepen - POST  `https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.SetAllConnections`
6. GW-ID en datasource-ID door aan te roepen ophalen: ophalen `https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.GetBoundGatewayDataSources`
7. De referenties van gebruiker bijwerken door aanroepen - PATCH `https://api.powerbi.com/v1.0/myorg/gateways/{gateway_id}/datasources/{datasource_id}`

#### <a name="old-dataset-and-reports"></a>Oude gegevensset en rapporten

De rapporten geüpload voordat oktober 2016 bieden geen ondersteuning voor de Download PBIX-functie. 

**Stroom**

1. PBIX ophalen uit uw ontwikkelomgeving (uw interne bronbeheer).
2. Roep de Import PBIX voor uw Power BI Embedded-werkruimte.

#### <a name="push-dataset-and-report"></a>Pushgegevensset en rapport

Download PBIX biedt geen ondersteuning voor *Push API* gegevenssets. Push API-gegevenssets kunnen niet worden overgezet van Power BI-Werkruimteverzamelingen naar Power BI Embedded.

**Stroom**

1. Roep de API 'Create dataset' met de gegevensset Json om de gegevensset voor uw Power BI Embedded-werkruimte te maken.
2. Rapport voor de gemaakte gegevensset * opnieuw.

Het is mogelijk met behulp van aantal tijdelijke oplossingen voor het migreren van de push api-rapport van Power BI-Werkruimteverzamelingen naar Power BI Embedded door te proberen het volgende:

1. Sommige dummy PBIX uploaden naar uw Power BI-Werkruimteverzameling-werkruimte.
2. Kloon het push api rapport en koppel het aan het PBIX uit stap 1.
3. Download het push API-rapport met het PBIX.
4. Upload het PBIX aan uw Power BI Embedded-werkruimte.
5. Maak een pushgegevensset in uw Power BI Embedded-werkruimte.
6. Koppel het rapport naar het push api-gegevensset opnieuw.

## <a name="create-and-upload-new-reports"></a>Maken en nieuwe rapporten uploaden

U kunt naast de inhoud van Power BI Workspace Collections migreren, rapporten en gegevenssets met Power BI Desktop maken en deze rapporten vervolgens publiceren naar een app-werkruimte. De eindgebruiker de rapporten publiceren, moet beschikken over een Power BI Pro licentie om te kunnen publiceren naar een app-werkruimte.

## <a name="rebuild-your-application"></a>Uw toepassing opnieuw bouwen

1. Pas uw toepassing met de Power BI REST-API's en de Rapportlocatie in powerbi.com.

2. Opnieuw opbouwen uw AuthN/AuthZ-verificatie met de *master* rekening voor uw toepassing. U kunt profiteren van het gebruik van een [insluittoken](https://msdn.microsoft.com/library/mt784614.aspx) om toe te staan dat de gebruiker handelen namens andere gebruikers.

3. Sluit uw rapporten uit Power BI Embedded in uw toepassing. Zie voor meer informatie, [uw Power BI-dashboards, rapporten en tegels insluiten](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

## <a name="map-your-users-to-a-power-bi-user"></a>Uw gebruikers toewijzen aan een Power BI-gebruiker

In uw toepassing, wijst u gebruikers die u in de toepassing naar beheert een *master* Power BI-referentie voor de doeleinden van uw toepassing. De referenties voor dit Power BI *master* account zijn opgeslagen in uw toepassing en worden gebruikt voor het maken van insluittokens.

## <a name="what-to-do-when-you-are-ready-for-production"></a>Wat te doen wanneer u gereed voor productie bent

Wanneer u klaar om te verplaatsen naar productie bent, moet u het volgende doen:

- Als u een afzonderlijke tenant voor ontwikkeling gebruikt, moet u controleren of dat de app-werkruimten, dashboards en rapporten zijn beschikbaar in uw productieomgeving. Zorg ervoor dat u de toepassing voor uw productietenant in Azure AD hebt gemaakt en de juiste app-machtigingen toegewezen, zoals aangegeven in stap 1.

- Koop de capaciteit die past bij uw behoeften. U kunt de [ingesloten analysecapaciteitsplanning](https://aka.ms/pbiewhitepaper) voor meer informatie over wat u mogelijk nodig hebt. Wanneer u klaar om aan te schaffen bent, kunt u een Power BI Embedded-resource in Azure portal kunt kopen.

- Bewerk de App-werkruimte en wijs deze toe aan een capaciteit onder Geavanceerd.

    ![App-werkruimte toewijzen aan een capaciteit in powerbi.com](media/migrate-from-power-bi-workspace-collections/embedded-capacity.png)

- Implementeer uw bijgewerkte toepassing naar productie en begin met het insluiten van rapporten van Power BI Embedded.

## <a name="after-migration"></a>Na de migratie

Sommige opschonen is in Power BI Workspace Collections vereist.

- Verwijder alle werkruimten uit de geïmplementeerde oplossing in de Azure-service van Power BI Workspace Collections.
- Verwijder eventuele Werkruimteverzamelingen die zijn opgeslagen in Azure.

## <a name="next-steps"></a>Volgende stappen

Gefeliciteerd! Uw toepassing is gemigreerd naar Power BI Embedded. Zie voor meer informatie over het insluiten van Power BI-dashboards, rapporten en gegevenssets [uw Power BI-dashboards, rapporten en tegels insluiten](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Nog vragen? [Misschien dat de Power BI-Community](http://community.powerbi.com/)
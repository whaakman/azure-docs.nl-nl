---
title: Cortana intelligence | Microsoft Docs
description: Een Cortana intelligence-aanbieding publiceert.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 74cde720af012b3355b813cf6da2b2bdf10b9b8e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51257204"
---
# <a name="publish-a-cortana-intelligence-offer-using-the-cloud-partner-portal"></a>Een Cortana Intelligence-aanbieding met behulp van de Cloud Partner-Portal publiceren

In dit artikel wordt beschreven hoe u een Cortana Intelligence-aanbieding met behulp van de Cloud Partner-Portal publiceren.

## <a name="prerequisites"></a>Vereisten

De Cloud Partner-Portal biedt ondersteuning voor toegang tot de portal, waarmee medewerkers samen te werken aan het publiceren van een aanbieding op basis van rollen. Zie voor meer informatie, [Cloud Portal beheren-gebruikers](./cloud-partner-portal-manage-users.md).

Voordat een aanbieding kan worden gepubliceerd namens een publisher-account, een van de personen met \"eigenaar\" rol moet instemmen met de [gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/website-terms-of-use/), [privacyverklaring van Microsoft](https://www.microsoft.com/privacystatement/default.aspx), en [Microsoft Azure Certified-Programmaovereenkomst](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).

## <a name="to-start-creating-a-cortana-inteligence-offer"></a>Om te beginnen met het maken van een aanbieding Cortana Inteligence

Als alle vereisten wordt voldaan, bent u klaar om te beginnen met het ontwerpen van uw aanbieding Cortana Inteligence.

1.  Aanmelden bij de [cloud partner-portal](http://cloudpartner.azure.com/).
2.  Selecteer op de linker navigatiebalk **+ nieuwe aanbieding**.
3. Selecteer **Cortana Intelligence**.
4. Selecteer de **Editor** tabblad de **nieuwe aanbieding** weergave voor het configureren van de volgende opties:
    -   Aanbiedingsinstellingen
    -   Technische informatie
    -   StoreFront Details
    -   Contactpersonen

    Elk van de voorgaande opties weergeven van een formulier dat u invullen.  Vereiste velden voor elk formulier zijn gemarkeerd met een rood sterretje (\*).

## <a name="to-configure-offer-settings"></a>Het configureren van instellingen bieden

De instellingen bieden biedt algemene informatie over de aanbieding, zoals een aanbiedings-ID, de uitgevers-ID en de aanbiedingsnaam.

### <a name="offer-id"></a>Aanbiedings-id

Dit is een unieke id voor de aanbieding binnen een publisher-profiel.
Deze ID wordt weergegeven in de product-URL's. De id kan alleen bestaan uit alfanumerieke tekens in kleine letters en streepjes (-). De ID kan mag niet eindigen met een streepje en een maximum van 50 tekens. 
>[!Note]
>Dit veld wordt vergrendeld wanneer een aanbieding live meteen.

**Voorbeeld:**

Als de uitgever **contoso** maakt u een aanbieding met de aanbiedings-ID *voorbeeld Cortana Intelligence*, wordt het weergegeven in AppSource als 'https://appsource.microsoft.com/marketplace/apps/**contoso**.*Voorbeeld van Cortana Intelligence*? tab = Overview '.

### <a name="publisher-id"></a>Uitgevers-ID

Een vervolgkeuzelijst met uitgever profielen. Gebruiken om te kiezen van de uitgeverprofiel dat u wilt publiceren, deze aanbieding onder.

>[!Note]
>Dit veld wordt vergrendeld wanneer een aanbieding live meteen.

### <a name="name"></a>Naam

De weergavenaam voor uw aanbieding. Deze naam wordt weergegeven [AppSource](https://appsource.microsoft.com). De naam mag maximaal 50 tekens bevatten.

Als u klaar bent met de informatie die nodig is in de instellingen bieden, selecteer **opslaan** om door te gaan met de technische informatie deel van de aanbieding. optie.

## <a name="to-configure-technical-info"></a>Technische gegevens configureren

Gebruik deze weergave om technische informatie die wordt weergegeven op de pagina van uw aanbieding te geven. De volgende velden van toepassing op deze weergave.

### <a name="partner-mpn-id"></a>MPN-partner-Id

Als u een geregistreerde Microsoft-partner bent, gaat u naar de [partners website](https://partners.microsoft.com/) en meld u aan om op te halen van uw organisatie Partner-id. Voer deze ID voor de **Partner MPN Id**.

### <a name="analytics-components-used"></a>Analyse-onderdelen die worden gebruikt

Selecteer de onderdelen die gebruikmaakt van uw aanbieding. Selecteer ten minste 1 onderdeel. Selecteer **Microsoft R** alleen als u een van de licenties MEVR SQL 2016 R Services, HDInsight Premium, of MEVR die worden uitgevoerd op virtuele machines.

### <a name="additional-components-used"></a>Extra onderdelen die worden gebruikt

Selecteer alle andere onderdelen die gebruikmaakt van uw oplossing.

### <a name="customer-name-using-solution"></a>Naam van de klant met behulp van oplossing

Geef de naam van de klant die van deze oplossing in productie gebruikmaakt. 

>[!Note]
>Deze informatie niet worden gepubliceerd op AppSource. Het wordt alleen gebruikt voor de oplossing voor evaluatie.

### <a name="azure-consumption-requirement-met"></a>Azure-verbruik aan deze eis wordt voldaan?

Aangeven of ten minste $1 K per maand per klant van Azure-Suite-onderdelen in de oplossing wordt gegenereerd of als de oplossing maakt gebruik van Microsoft-R.

>[!Note]
>Deze informatie niet worden gepubliceerd op AppSource. Het wordt alleen gebruikt voor de oplossing voor evaluatie.

### <a name="demo-video-url"></a>Demo van Video-URL

Geef een URL voor een demonstratie video voor de oplossing/app dat uw team met betrekking tot de pre-sales klanten kunt weergeven. 

>[!Note]
>Deze informatie niet worden gepubliceerd op AppSource. Het wordt alleen gebruikt voor de oplossing voor evaluatie.

#### <a name="demo-video-guidelines"></a>Video demo-richtlijnen

- Videolengte moet minder dan 15 minuten.
- De video moet minimaal worden bewerkt op te nemen van de functionaliteit van de oplossing. 
- De video worden de belangrijkste aspecten van de gebruiker gerichte functionaliteit gemarkeerd en is gericht op de integratie van geavanceerde analyses. 
- Demo van video's **niet** worden openbaar aan klanten beschikbaar gesteld, maar worden verwacht vertegenwoordiger van hoe u de oplossing *zou* weergegeven aan een potentiële klant. Ze moeten als zodanig worden uitgeschreven en herhaalbare.
- Gebruik een hulpmiddel voor het opnemen van scherm dat Hiermee exporteert u een standard video-indeling (bijvoorbeeld MPEG) voor het maken van uw video. 

De volgende instructies laten zien hoe u een video die gebruikmaken van Skype voor bedrijven maakt. 

1. [Beginnen met een vergadering](https://support.office.com/article/Start-a-Skype-for-Business-conference-call-8dc8ac52-91ac-4db9-8672-11551fdaf997)
2. [Uw bureaublad delen](https://support.office.com/article/Share-your-screen-in-Skype-for-Business-2d436dc9-d092-4ef1-83f1-dd9f7a7cd3fc)
3. [Beginnen met opnemen](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232)
4. Na het stoppen van opname [de opname-manager gebruiken voor het opnemen van uw publiceren](https://support.office.com/article/Recording-Manager-save-and-publish-59a3beb7-c700-40cf-ab21-bc82a2b06351)

Upload uw video-opnamen naar een service waarmee u voor het genereren van een gedeelde-URL. Bijvoorbeeld, een [Gast-koppeling in OneDrive of Sharepoint](https://support.office.com/article/Share-sites-or-documents-with-people-outside-your-organization-80e49744-e30f-44db-8d51-16661b1d4232).

### <a name="supported-regions"></a>Ondersteunde regio’s

Selecteer alle regio's die ondersteuning biedt voor uw oplossing. Selecteer ten minste 1 regio.

### <a name="power-bi-desktop-file-pbix"></a>Power BI desktop-bestand (.pbix)

Indien van toepassing, upload een pbix-bestand. Zorg ervoor dat gegevens worden geïmporteerd in het bestand niet extern worden verwezen. Het ingesloten rapport maken we voor u.

### <a name="solution-architecture"></a>Oplossingsarchitectuur

Upload een document waarin wordt uitgelegd van de architectuur van uw oplossing. Zie voor het uploaden van een oplossing architectuur diagram richtlijn [Advanced Analytics werkstroom oplossingssjabloon](https://partnersprofilesint.blob.core.windows.net/partner-assets/documents/AppSource%20Solution%20Publishing%20Guide%20Docs/Advanced%20Analytics%20Solution%20Workflow%20Template%20V.2017.3.23.pptx).

>[!Note]
>Deze informatie niet worden gepubliceerd op AppSource. Het wordt alleen gebruikt voor de oplossing voor evaluatie.

### <a name="select-segments"></a>Selecteer segmenten

Selecteer alle gerelateerde bedrijfstakken. Als uw app voor een niet-vermelde segment is, voert u de segmentnaam van het in de **anderen** veld. U bent beperkt tot drie woorden voor het segment.

### <a name="select-business-processes"></a>Selecteer bedrijfsprocessen

Selecteer de bedrijfsprocessen die het beste uw oplossing te beschrijven. Als uw app voor alle niet-vermelde processen is, voert u de procesnaam van het in de **anderen** veld. U bent beperkt tot drie woorden voor het proces.

### <a name="trial-info"></a>Proefversie Info

-   **URL van de SaaS-proefversie:** Voer de URL voor de proefversie van uw app.
-   **Test Drive proefversie-URL:** Voer de URL voor de Test Drive-ervaring van uw app.

Zie voor meer informatie over proefversies **App-type** in de volgende sectie van dit artikel.

Als u klaar bent met de informatie die nodig is in de technische informatie, selecteer **opslaan** om door te gaan met de Details van de winkel deel van de aanbieding. 

## <a name="to-configure-storefront-details"></a>Om gegevens van de webwinkel te configureren

### <a name="offer-summary"></a>Overzicht van aanbieding

Dit is een overzicht van de toegevoegde waarde van uw aanbieding. Deze wordt weergegeven op de pagina voor zoeken van uw aanbieding. De maximale lengte van de samenvatting is 100 tekens.

### <a name="offer-description"></a>Beschrijving van aanbieding

Dit is de beschrijving die wordt weergegeven op de detailpagina van uw app.
De maximale lengte van de beschrijving is 1300 tekens.

Houd er rekening mee dat dit veld html-inhoud met labels, zoals duurt \<p\>, \<h1\>, \<h2\>, \<li\>, enz., waarmee u de inhoud nog veel meer maken netjes. Publiceren van portal-team werkt over het toevoegen van een functie waarmee een om te zien van een Preview-versie van hun storefront details voor iteratief en de inhoud meer netjes: in de tussentijd kunt u elke kunt u een online realtime HTML-hulpprogramma's zoals [ http://htmledit.squarefree.com ](http://htmledit.squarefree.com/) om te zien hoe uw beschrijving eruitzien.

De voorgestelde indeling van de beschrijving is te hebben splitst u de tekst in op basis van de subsecties propositions, elk met gemarkeerd met een subkop. Bezoekers meestal in een oogopslag over het veld "overzicht bieden" en subkoppen om op te halen basisvertalingen van wat de app-adressen en waarom moeten ze rekening houden met de app in slechts een kort overzicht. Het is dus belangrijk dat u krijgt de gebruiker\'aandacht geven ze een reden te lezen op de details ophalen.

#### <a name="partner-examples"></a>Voorbeelden van partner

- [Optimalisatie van de voorraad Neal Analytics](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview)
- [Persoonlijke instellingen Plexure detailhandel](https://appsource.microsoft.com/product/web-apps/plexure.c82dc2fc-817b-487e-ae83-1658c1bc8ff2?tab=Overview)
- [AvePoint Citizen Services](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview)

### <a name="industries"></a>Bedrijfstakken

Selecteer de branche dat uw app is het beste uitgelijnd. Als uw app is gekoppeld aan meerdere branches, laat u dit veld leeg.

### <a name="categories"></a>Categorieën

Selecteer de categorieën die relevant voor uw app zijn. Maximaal twee categorieën selecteren.

### <a name="app-type"></a>App-type

Selecteer het type van de proefversie die door uw app worden ondersteund op AppSource. Kies op van de volgende tests:
- **Gratis** betekent dat uw app is gratis.
- **Proefversie** betekent dat klanten kunnen proberen de gedurende een bepaalde periode.
- **Aanvraag voor de proefversie** betekent dat klanten kunnen vragen om een evaluatieversie van de app.

Partners kunnen twee soorten proefversie ervaringen bieden op AppSource.

- De **proefversie** optie, ook wel genoemd **klant onder leiding van experimenten (CLT)** kan bestaan uit een van de volgende typen: 
    - SaaS-proef
        - De klant kunt navigeren naar een Url die u of uw partner bieden. De klant verloopt via AAD federatieve eenmalige aanmelding om een tijd testervaring vakken.
        - Dit is een SaaS-app met meerdere tenants die worden geïsoleerd van de gebruiker/configuratiegegevens van andere gebruikers. Of deze ervaring biedt alleen een subset die gebruikmaakt van alleen-lezen bewerkingen.

        **Voorbeelden:**

        - [AFS Retail POP-uitvoering](https://appsource.microsoft.com/product/web-apps/afs.fa9fc926-3bc3-43dd-becd-3ef41b52c10b?tab=Overview)
        - [Services voor burgers AvePoint](https://appsource.microsoft.com/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview). (Deze app biedt een persoonlijke ervaring met duidelijke paden voor een geselecteerde set gebruiker Persona's.)

     - Test Drive
        - Uw (of van uw partner)-oplossing of een subset van deze kan worden verpakt met behulp van Azure Resource Manager-sjablonen die AppSource kunt instantiëren en. AppSource kunt u de app in een partner-abonnement beheren met de tijd boxing en onderhouden van warme/koude groep exemplaren enz.
        - We kunnen sjablonen en voorbeeldcode om u te helpen als u deze optie kiest bieden.

        **Voorbeelden:**

        - [Optimalisatie van de voorraad Neal Analytics](https://appsource.microsoft.com/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview&tag=CISHome)

- De **aanvraag voor proefversie** (**Partner geleid proefversies / PLT**) optie moet klanten om op te vullen van een formulier contactgegevens voor het vervolg van de partner. De partner follow-up en biedt een demonstratie of een proefversie van de app. Zie voor meer informatie, [AppSource testervaring scenario](https://aka.ms/trialexperienceforwebapps) video voor een overzicht op hoog niveau.

>[!Note]
>Gegevens ziet u dat **klant onder leiding van experimenten** hebben een hogere leadgeneratie mogelijke dan **Partner geleid proefversies**.


### <a name="help-link-for-your-app"></a>Help-koppeling voor uw app

Geef een URL naar een pagina met help-informatie voor uw app.

### <a name="supported-countriesregions"></a>Ondersteunde landen/regio 's

Bepaalt de landen/regio's waar uw aanbieding is beschikbaar voor evaluatie van dit veld.

![Ondersteunde landen/regio 's](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot3.png)

### <a name="supported-languages"></a>Ondersteunde talen

Selecteer de talen die ondersteuning biedt voor uw app. Als uw app biedt ondersteuning voor talen die niet in deze lijst staat, uw aanbieding publiceren en een e-mail verzenden naar: <appsource@microsoft.com> en laat ons weten over de ondersteuning voor andere talen.

### <a name="app-version"></a>App-versie

Voer het versienummer voor uw app.

### <a name="products-your-app-works-with"></a>Uw app met werkt producten

Specifieke producten vermeld die geschikt is voor uw app. U kunt maximaal 3 producten weergeven. Als u een product, selecteer de **plusteken** (naast Nieuw) en een open tekstveld voor u is gemaakt. Voer de naam van een product die geschikt is voor uw app.

### <a name="hide-key"></a>Sleutel verbergen

Dit is een sleutel in combinatie met de voorbeeld-URL van de aanbieding te verbergen, de aanbieding van openbaar weer te geven. Het is niet een wachtwoord. U kunt een willekeurige alfanumerieke tekenreeks invoeren.

### <a name="offer-logo-small"></a>Logo van de aanbieding (klein)

Dit logo wordt weergegeven op de pagina voor zoeken van uw app. De afbeelding PNG-indeling is de enige indeling die toegestaan. Grootte van de installatiekopie is 48 x 48 pixels.

### <a name="offer-logo-large"></a>Aanbieding logo (groot)

Dit logo wordt weergegeven op de detailpagina van uw app. De afbeelding PNG-indeling is de enige indeling die toegestaan. Grootte van de installatiekopie is 48 pixels x 48 pixels.

### <a name="video"></a>Video

U kunt maximaal vier video's uploaden. Voor elke video die u wilt uploaden:
- Geef de naam van de video
- Geef een URL (YouTube of Vimeo alleen)
- Geef een miniatuur om te koppelen aan de video. De miniatuur moet de PNG-indeling gebruiken en de grootte moet pixels 1280 X 720 pixels. 

Selecteren om toe te voegen nieuwe video(s), **+ nieuw**, zoals weergegeven in de volgende schermopname.

![Nieuwe video toevoegen](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot4.png)

### <a name="document"></a>Document

U kunt maximaal drie documenten uploaden. Elk document moet de PDF-indeling gebruiken. Een nieuw document toevoegen:

- Selecteer **+ nieuwe**
- Voer de naam van een document
- Selecteer **uploaden** om een document te uploaden.

![Nieuw document toevoegen](./media/cloud-partner-portal-publish-cortana-intelligence-app/CISScreenshot5.png)

### <a name="privacy-policy"></a>Privacybeleid

Voer de URL voor het privacybeleid van uw app

### <a name="terms-of-use"></a>Gebruiksvoorwaarden

Voer de gebruiksvoorwaarden van uw app. AppSource-klanten moeten deze voorwaarden accepteren voordat ze uw app kunnen uitproberen.

>[!Note]
>Dit veld accepteert inhoud, zoals html-inhoud met behulp van HTML met tags < p\>, < h1\>, en < li\>. U kunt deze tags gebruiken om de opmaak van uw inhoud. 

### <a name="lead-destination"></a>Bestemming leiden

Selecteer een CRM-systeem waar de leads worden opgeslagen. 

Selecteer **Azure Table** als u een van deze systemen CRM: Salesforce, Marketo of Microsoft Dynamics CRM. 

Voor meer informatie over het CRM-systeem die u wilt gebruiken, selecteert u een van de volgende koppelingen voor ondersteunde systemen.

-   [Azure-tabel](./cloud-partner-portal-lead-management-instructions-azure-table.md)
-   [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md)
-   [Microsoft Dynamics CRM](./cloud-partner-portal-lead-management-instructions-dynamics.md)
-   [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md)

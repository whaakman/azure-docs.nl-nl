---
title: Instellen van uw toepassing in de Portal voor publiceren | Microsoft Docs
description: De instructies in het instellen van uw toepassing in de Cloud-Portal voor publiceren.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 7b5bb0bf8ff5fac10c47cf5bdf9564903f4d6b94
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54448480"
---
# <a name="setup-your-application-in-the-publishing-portal"></a>Instellen van uw toepassing in de Portal voor publiceren

U bent nu klaar voor het instellen van uw toepassing in de portal voor publiceren.

## <a name="login-and-create-a-new-offer"></a>Aanmelden en een nieuwe aanbieding maken

1. Aanmelden bij de [Cloud Partner-Portal](http://cloudpartner.azure.com/).
2. In de linker navigatiebalk, klikt u op '+ nieuw bieden' en selecteer "Dynamics 365 voor Customer Engagement."

![Een nieuwe aanbieding selecteren](./media/CRMScreenShot14.png)

3. Een nieuwe aanbieding 'Editor' weergave wordt nu voor u geopend en we zijn klaar om door te gaan ontwerpen.

![Venster voor de nieuwe aanbieding](./media/CRMScreenShot15.png)

4. De 'formulieren"die moeten worden ingevuld zijn zichtbaar aan de linkerkant in de weergave 'Editor'. Elke "vorm" bestaat uit een set velden die moeten worden ingevuld. Vereiste velden zijn gemarkeerd met een rood sterretje (\*).

Er zijn vier belangrijkste formulieren voor het ontwerpen van een Dynamics 365 voor Customer Engagement-aanbieding

* Aanbiedingsinstellingen
* Technische informatie
* StoreFront Details
* Contactpersonen

## <a name="fill-out-the-offer-settings-form"></a>Vul het formulier instellingen bieden

Het formulier van de instellingen voor aanbieding is een eenvoudige methode om de instellingen van de aanbieding te geven. De verschillende velden worden hieronder beschreven.

### <a name="offer-id"></a>Aanbiedings-id

Dit is een unieke id voor de aanbieding binnen een publisher-profiel. Deze ID zijn in URL's voor product zichtbaar. De id kan alleen bestaan uit alfanumerieke tekens in kleine letters en streepjes (-). De ID kan mag niet eindigen met een streepje en een maximum van 50 tekens. Dit veld is vergrendeld zodra een aanbieding live meteen.

bijvoorbeeld, als een uitgever **'contoso'** uitgevers maakt u een aanbieding met aanbiedings-ID **'voorbeeld-Web-App'**, wordt het weergegeven in AppSource als "https://appsource.microsoft.com/marketplace/apps/contoso.sample-WebApp?tab=Overview"

### <a name="publisher-id"></a>Uitgevers-ID

Deze vervolgkeuzelijst kunt u de uitgeverprofiel dat u wilt publiceren, deze aanbieding onder kiezen. Dit veld is vergrendeld zodra een aanbieding live meteen.

### <a name="name"></a>Name

Dit is de weergavenaam voor uw aanbieding. Dit is de naam die wordt weergegeven in de [AppSource](https://appsource.microsoft.com/). De naam mag maximaal 50 tekens bevatten.

Klik op 'Opslaan' uw voortgang opslaan. Volgende stap zou zijn om toe te voegen technische informatie voor uw aanbieding.

## <a name="fill-out-the-technical-info-form"></a>Vul het formulier technische informatie


Het formulier technische informatie is waar vult u in informatie die specifiek is voor uw Dynamics 365 voor Customer Engagement-oplossing. De muisaanwijzer boven de wordt meer informatie met zich mee. Zie het voorbeeld hieronder.

![Scherm voor technische informatie](./media/CRMScreenShot16.png)

### <a name="application-info"></a>Informace aplikace

De meeste uitgevers laat deze velden met de standaardwaarden, gebruiker, Nee, geen, en de URL van een lege toepassing configuratie aan de hand van de bovenstaande schermafbeelding.

### <a name="crm-package"></a>CRM-pakket

![Informatie over CRM-pakket](./media/CRMScreenShot17.png)

Hier volgt een uitleg voor deze velden:

* Naam van uw pakket: Naam van het bestand u gemaakt in de bovenstaande stap bij het maken van het zip-bestand dat is uw CRM AppSource-pakket. In het bovenstaande voorbeeld is ' Microsoft\_SamplePackage.zip '.
* URL van de locatie van uw pakket: Dit is de URL van de Azure Storage-account met de naam van het pakket opgegeven hierboven. Het is de URL in stap 9 van de voorgaande sectie hebt gemaakt.
* Is er meer dan één crm-pakket in uw pakketbestand: Selecteer Ja **alleen** als u ondersteuning voor meerdere versies van crm met verschillende pakketten bieden. Dit is voor de meeste partners "Nee". Als u Ja selecteert, moet u AppSource om pakketten te maken voor elke versie van uw oplossing. _Opmerking: Dit wordt niet gevraagd om als er meerdere **zip** bestanden. Als u meerdere solution.zip bestanden, maar slechts één versie hebt, moet u nog steeds selecteren 'Nee '. Het hulpprogramma verpakking wordt Breng deze samen in automatisch._

### <a name="crm-package-availability"></a>Beschikbaarheid van CRM-pakket

In deze sectie, selecteer welke regio's van uw pakket wordt beschikbaar gesteld aan CRM. Zie de koppeling voor informatie op welke regio's welke landen bedienen: [http://o365datacentermap.azurewebsites.net/](http://o365datacentermap.azurewebsites.net/)

Opmerking: Implementeren naar Duitsland 'Onafhankelijke en VS (overheid)-Cloud' onafhankelijke vereist speciale MACHTIGINGEN en validatie tijdens de certificering

## <a name="storefront-details"></a>StoreFront Details

### <a name="offer-summary"></a>Overzicht van aanbieding

Dit is een overzicht van de toegevoegde waarde van uw aanbieding. Deze wordt weergegeven op de pagina voor zoeken van uw aanbieding. Dit moet een maximum van 100 tekens.

### <a name="offer-description"></a>Beschrijving van aanbieding

Dit is de beschrijving die wordt weergegeven op de pagina met details van uw app. Maximaal toegestane aantal is 1300 tekens

### <a name="industries"></a>Bedrijfstakken

Selecteer de branche dat uw app is het beste uitgelijnd. Als uw app is gekoppeld aan meerdere branches, kunt u dit leeg laten.

### <a name="categories"></a>Categorieën

Selecteer de categorieën die relevant voor uw app zijn. Selecteer maximaal 3.

### <a name="app-type"></a>App-type

Selecteer het type van de proefversie waarmee uw app wordt ingeschakeld op AppSource. 'Gratis' betekent dat uw app is gratis. 'Proefversie' betekent dat klanten uw app gedurende een korte periode op AppSource kunnen uitproberen. 'Voor proefversie aanvragen' wordt niet ondersteund voor Dynamics 365 voor Customer Engagement-apps. Selecteer deze optie niet.

### <a name="help-link-for-your-app"></a>Help-koppeling voor uw app

Voer de URL naar een pagina waarmee verwante informatie voor uw app heeft.

### <a name="supported-countriesregions"></a>Ondersteunde landen/regio 's

Bepaalt de landen/regio waarin uw aanbieding is beschikbaar voor evaluatie van dit veld.

### <a name="supported-languages"></a>Ondersteunde talen

Selecteer de talen die ondersteuning biedt voor uw app. Als uw app biedt ondersteuning voor extra talen die niet in deze lijst staat, blijven uw aanbieding publiceren en e-mail verzenden naar: [ appsource@microsoft.com ](mailto:appsource@microsoft.com) en laat ons weten.

### <a name="app-version"></a>App-versie

Voer in het versienummer voor uw app

### <a name="app-release-date"></a>App-Releasedatum

Voer de releasedatum voor uw app

### <a name="products-your-app-works-with-max-3"></a>Producten die uw app met (Max. 3 werkt)

Lijst-specifieke producten die geschikt is voor uw app. U kunt maximaal drie producten weergeven. Als u een product, klikt u op het plusteken (naast Nieuw) en een nieuw open tekstveld wordt gemaakt voor u de naam van een product dat uw app met werkt invoeren.

### <a name="search-keywords-max-3"></a>Trefwoorden (Max. 3)

AppSource kan een klant kan op basis van trefwoorden zoeken. U kunt de set waarvoor uw toepassing worden weergegeven op de klanten trefwoorden invoeren.

Bijvoorbeeld als de toepassing 'Mijn e-mailen Service' e-mailberichten is, postadres, e-mailservice mogelijk bepaalde trefwoorden. Kies woorden die gebruikers waarschijnlijk wordt gebruikt om te zoeken naar voor uw app in het zoekvak AppSource.

### <a name="hide-key"></a>Sleutel verbergen

Dit is een sleutel die wordt gecombineerd met de voorbeeld-URL van de aanbieding te verbergen in openbaar weer te geven. Het is niet een wachtwoord. U kunt een willekeurige tekenreeks hier invoeren.

### <a name="offer-logo-png-format-48x48"></a>Logo van de aanbieding (png-indeling, 48 x 48)

Dit wordt weergegeven op de pagina voor zoeken van uw app. **Alleen de PNG-indeling is toegestaan.** Uploaden van een PNG-afbeelding met een resolutie van 48PX\*48PX

### <a name="offer-logo-png-format-216x216"></a>Logo van de aanbieding (png-indeling, 216 x 216)

Dit wordt weergegeven op de detailpagina van uw app. **Alleen de PNG-indeling is toegestaan.** Uploaden van een PNG-afbeelding met een resolutie van 216PX\*216PX

### <a name="videos"></a>Video's

U kunt maximaal vier video's uploaden. Voor elke video die u wilt uploaden, moet u in de video naam, URL (YouTube of Vimeo alleen) en miniatuur om te koppelen aan de video te vullen. Miniatuur moeten PNG-indeling hebben en moet 1280PX\*720PX. Als u wilt toevoegen van nieuwe video(s), klikt u op het plusteken. Video's Trefwoordbeheer wordt weergegeven op de detailpagina van uw app.

### <a name="documents"></a>Documenten

U kunt maximaal drie documenten in PDF-indeling uploaden. Voor elk document dat u wilt uploaden, moet u de naam van het document invullen en uploadt het document. Document moet zich in pdf-indeling.

Nieuwe documenten toevoegen, klikt u op het plusteken (+)

### <a name="screenshots"></a>Schermopnamen

Dit zijn schermafbeeldingen die wordt weergegeven op de detailpagina van AppSource voor uw app.

### <a name="privacy-policy"></a>Privacybeleid

Voer de URL naar het privacybeleid van uw app

### <a name="terms-of-use"></a>Gebruiksvoorwaarden

Voer de gebruiksvoorwaarden van uw app. AppSource-klanten moeten deze voorwaarden accepteren voordat ze kunnen uw app uitproberen

### <a name="support-url"></a>URL voor ondersteuning

Voer de URL voor ondersteuning voor uw app.

### <a name="lead-destination"></a>Bestemming leiden

Selecteer een CRM-systeem indien u leiden worden, opgeslagen. Selecteer 'Azure Table' hier hebt u een van de volgende CRM-systemen: Salesforce, Marketo, Microsoft Dynamics CRM. De CRM-systeem die u hier selecteert, wordt waar we de details van eindgebruikers die uw Apps weer op AppSource (leads proberen) worden geschreven. Afhankelijk van de CRM-systeem dat u selecteert, klikt u op de bijbehorende URL hieronder voor meer informatie over het uitvoeren van de volgende set met velden

* [Azure-tabel](./cloud-partner-portal-lead-management-instructions-azure-table.md)
* [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md)
* [Microsoft Dynamics CRM](./cloud-partner-portal-lead-management-instructions-dynamics.md)
* [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md)

## <a name="storefront-details"></a>StoreFront Details

De gegevens van de contactpersoon worden voor de interne communicatie tussen de partner en Microsoft alleen gebruikt. Opmerking: Het is belangrijk dat u een e-mailadres dat wordt bewaakt in deze velden. We gebruiken dit e-mailbericht om te communiceren met u op uw voortgang bij het publiceren op AppSource. Alleen de URL voor ondersteuning is zichtbaar voor klanten.

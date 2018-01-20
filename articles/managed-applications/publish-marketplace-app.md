---
title: Azure beheerde toepassingen in de Marketplace | Microsoft Docs
description: Beschrijving van Azure beheerde toepassingen die beschikbaar via de Marketplace zijn.
services: azure-resource-manager
author: tfitzmac
manager: timlt
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/18/2018
ms.author: tomfitz
ms.openlocfilehash: fccc2dbb7623f4ceb0d3decc7037f75a05858910
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Beheerde Azure-toepassingen in de Marketplace

Aanbieders Azure kunnen gebruikmaken van beheerde toepassingen te bieden hun oplossingen voor alle klanten van Azure Marketplace. De leveranciers kunnen opnemen beheerde serviceproviders (MSPs), onafhankelijke softwareleveranciers (ISV's) en systeemintegrators (SIs). Beheerde toepassingen verminderd het onderhoud en het onderhoud van de overhead voor klanten. Leveranciers verkopen infrastructuur- en software via de marketplace. Ze kunnen services en operationele ondersteuning koppelen aan beheerde toepassingen. Zie voor meer informatie [beheerde toepassingsoverzicht](overview.md).

Dit artikel wordt uitgelegd hoe u een toepassing publiceren in de marketplace en grote schaal beschikbaar maken voor klanten.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Vereisten voor het publiceren van een beheerde toepassing

Als u dit artikel, moet u al het ZIP-bestand voor de definitie van de beheerde toepassing hebben. Zie voor meer informatie [-catalogus-servicetoepassing maken](publish-service-catalog-app.md).

Bovendien zijn er verschillende zakelijke vereisten. Dit zijn:

* Uw bedrijf of de vestiging moet zich bevinden in een land waar verkoop worden ondersteund door de marketplace.
* Het product moet op een manier die compatibel is met factureringsmodellen ondersteund door de marketplace beschikken.
* Technische ondersteuning beschikbaar maken voor klanten in commercieel redelijke wijze. Ondersteuning van de gratis, worden betaald, of via community ondersteunen.
* Licentie uw software en eventuele afhankelijkheden software van derden.
* Geef de inhoud die voldoet aan de criteria voor uw aanbieding worden weergegeven in de Marketplace en in de Azure-portal.
* Ga akkoord met de voorwaarden van de Azure Marketplace deelname beleidsregels en uitgever overeenkomst.
* Ga akkoord met de gebruiksvoorwaarden van, de privacyverklaring van Microsoft en de gecertificeerd programma Agreement voor Microsoft Azure.

## <a name="become-a-publisher"></a>Door een

Als een publisher in Azure Marketplace, moet u het volgende doen:

1. Maken van een Microsoft-ID - uw Microsoft-account maken met een e-mailadres behoort aan het domein van uw bedrijf, maar niet aan een enkele persoon. Dit e-mailadres wordt gebruikt voor de Microsoft Developer Center en de Cloud Partner-Portal. Zie voor meer informatie [Azure Marketplace Publisher Guide](https://aka.ms/sellerguide).
1. Indienen [Azure Marketplace benoeming formulier](https://aka.ms/ampnomination) - voor **oplossing die u wilt publiceren?**, selecteer **beheerde toepassing**. Zodra het formulier wordt verzonden, wordt de Marketplace-voorbereidingsteam controleert de toepassing en valideert de aanvraag. Het goedkeuringsproces kan één tot drie dagen duren. Wanneer uw benoeming is goedgekeurd, ontvangt u een promotiecode om te zien van de registratie-kosten voor het developer center. Als u dit doet **niet** vult u het formulier benoeming van Marketplace u wordt gevraagd om te betalen registratie $99.
1. Registreren in [Developer Center](https://developer.microsoft.com) -Microsoft wordt gecontroleerd of uw organisatie een geldige rechtspersoon met een geldig BTW-nummer voor het land waar het is geregistreerd. Het goedkeuringsproces kan 5 tot 10 dagen duren. Om te voorkomen de kosten van de registratie, gebruikt u de promotiecode die u in de e-mail van het proces benoeming ontvangen. Zie voor meer informatie [Azure Marketplace Publisher Guide](https://aka.ms/sellerguide).
1. Meld u aan bij [Cloud Partner-Portal](https://cloudpartner.azure.com) - In het profiel publisher uw Developer Center-account koppelen aan het profiel van de uitgever Marketplace. Zie voor meer informatie [Azure Marketplace Publisher Guide](https://aka.ms/sellerguide).

## <a name="create-a-new-azure-application-offer"></a>Een nieuwe aanbieding voor Azure-toepassing maken

Nadat uw partner-portal-account is gemaakt, bent u gereed voor het maken van uw aanbieding beheerde toepassing.

### <a name="set-up-an-offer"></a>Instellen van een aanbieding

De aanbieding voor een beheerde toepassing komt overeen met een product aanbieden van een uitgever-klasse. Als u een nieuw type toepassing die u beschikbaar wilt maken in de marketplace hebt, kunt u dit instellen als een nieuwe aanbieding. Een aanbieding is een verzameling van SKU's. Elke aanbieding wordt weergegeven als een eigen entiteit in de marketplace.

1. Aanmelden bij de [Cloud Partner-portal](https://cloudpartner.azure.com/).

1. Selecteer in het navigatievenster aan de linkerkant **+ een nieuwe aanbieding** > **Azure-toepassingen**.

1. In de **Editor** weergave ziet u de vereiste formulieren. Elk formulier wordt verderop in dit artikel beschreven.

## <a name="offer-settings-form"></a>Formulier voor instellingen van aanbieding

De velden voor de **instellingen bieden** formulier zijn:

* **ID bieden**: deze unieke id identificeert de aanbieding binnen een publisher-profiel. Deze ID wordt weergegeven in de product-URL's, Resource Manager-sjablonen en facturering rapporteert. Deze kan alleen bestaan uit alfanumerieke tekens in kleine letters of streepjes (-). De ID mag niet eindigen op een streepje. Het is beperkt tot een maximum van 50 tekens. Nadat een aanbieding live gaat, wordt dit veld is vergrendeld.
* **Uitgevers-ID**: Gebruik deze vervolgkeuzelijst om te kiezen uitgever u het gewenste profiel voor het publiceren van deze aanbieding onder. Nadat een aanbieding live gaat, wordt dit veld is vergrendeld.
* **Naam**: deze weergavenaam voor uw aanbieding wordt weergegeven in de Marketplace en in de portal. Er kunnen maximaal 50 tekens. Een herkenbare naam merk voor het product bevatten. Neem geen hier de naam van uw bedrijf tenzij hoe deze wordt gebracht. Als u deze aanbieding bent marketing op uw eigen website, zorg ervoor dat de naam exact hoe deze wordt weergegeven op uw website.

Wanneer u klaar bent, selecteer **opslaan** uw voortgang opslaan.

## <a name="skus-form"></a>Formulier SKU 's

De volgende stap is het toevoegen van SKU's voor uw aanbieding.

Een SKU is de kleinste over een aanbieding. U kunt een SKU binnen dezelfde productklasse (aanbieden) om onderscheid te gebruiken tussen:

* Andere functies die worden ondersteund
* Hiermee wordt aangegeven of de aanbieding wordt beheerd of onbeheerd
* Facturering modellen die worden ondersteund

Een SKU wordt weergegeven onder de bovenliggende aanbieding in de marketplace. Deze wordt weergegeven als een eigen over entiteit in de Azure-portal.

1. Selecteer **SKU's** > **nieuwe SKU**.

1. Voer een **SKU-ID**. Een SKU-ID is een unieke id voor de SKU binnen een aanbieding. Deze ID wordt weergegeven in de product-URL's, Resource Manager-sjablonen en facturering rapporteert. Deze kan alleen bestaan uit alfanumerieke tekens in kleine letters of streepjes (-). De ID mag niet eindigen op een streepje en beperkt tot een maximum van 50 tekens. Nadat een aanbieding live gaat, wordt dit veld is vergrendeld. U kunt meerdere SKU's binnen een aanbieding hebben. U moet een SKU voor elke installatiekopie die u wilt publiceren.

1. Vul de **SKU Details** sectie op de volgende notatie:

   Vul de volgende velden:

   * **Titel**: Voer een titel voor de SKU. Deze titel wordt weergegeven in de galerie voor dit item.
   * **Samenvatting**: Voer een korte samenvatting voor de SKU. Deze tekst wordt weergegeven onder de titel.
   * **Beschrijving**: Voer een gedetailleerde beschrijving van de SKU.
   * **SKU-Type**: de toegestane waarden zijn *beheerde toepassingen* en *Oplossingssjablonen*. Selecteer voor deze aanvraag *beheerde toepassing*.
   * **Land/regio beschikbaarheid**: Selecteer de landen waar de beheerde toepassing beschikbaar is.
   * **Prijzen**: Geef een prijs voor beheer van de toepassing. Selecteer de beschikbare landen voordat u de prijs.

1. Een nieuw pakket toevoegt. Vul de **pakketgegevens** sectie op de volgende notatie:

   Vul de volgende velden:

   * **Huidige versie**: Voer een versie voor het pakket dat u uploadt. Deze moet de indeling `{number}.{number}.{number}{number}`.
   * **Selecteer een pakketbestand**: dit pakket bevat twee vereiste bestanden in een ZIP-pakket zijn gecomprimeerd. Een bestand is een Resource Manager-sjabloon die de resources te implementeren voor de beheerde toepassing definieert. Het andere bestand definieert het [gebruikersinterface](create-uidefinition-overview.md) voor consumenten implementeert de beheerde toepassing via de portal. In de gebruikersinterface geeft u de elementen waarmee consumenten parameterwaarden opgeven.
   * **PrincipalId**: deze eigenschap is de Azure Active Directory (Azure AD)-ID van een gebruiker, groep of toepassing die wordt verleend tot de resources in het abonnement van de klant. De roldefinitie worden de machtigingen beschreven.
   * **Roldefinitie**: deze eigenschap is een lijst met alle ingebouwde rollen gebaseerd toegangsbeheer (RBAC) rollen verstrekt door Azure AD. U kunt de functie die het meest geschikt is om te gebruiken voor het beheren van bronnen namens de klant selecteren.

U kunt meerdere autorisaties toevoegen. Het is raadzaam dat u een AD-gebruikersgroep maken en geef de bijbehorende ID in **PrincipalId**. Op deze manier kunt u meer gebruikers toevoegen aan de gebruikersgroep zonder de noodzaak voor het bijwerken van de SKU.

Zie voor meer informatie over RBAC [aan de slag met RBAC in Azure portal](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Formulier Marketplace

Het formulier Marketplace vraagt om de velden die worden weergegeven op de [Azure Marketplace](https://azuremarketplace.microsoft.com) en klik op de [Azure-portal](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Preview abonnement-id 's

Geef een lijst met Azure-abonnement-id's die u toegang krijgen de aanbieding tot kunt nadat deze gepubliceerd. U kunt deze abonnementen wit vermeld de voorbeeld-aanbieding testen voordat u deze maakt live. U kunt een witte lijst van maximaal 100 abonnementen in de Partnerportal compileren.

### <a name="suggested-categories"></a>Aanbevolen categorieën

Selecteer maximaal vijf categorieën uit de lijst die uw aanbieding kan best aan gekoppeld zijn. Deze categorieën worden gebruikt voor uw aanbieding worden toegewezen aan de productcategorieën die beschikbaar zijn in de [Azure Marketplace](https://azuremarketplace.microsoft.com) en de [Azure-portal](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Marketplace

De volgende velden worden weergegeven in de samenvatting van uw beheerde toepassing:

![Samenvatting van Marketplace](./media/publish-marketplace-app/publishvm10.png)

De **overzicht** tabblad voor uw beheerde toepassing de volgende velden worden weergegeven:

![Overzicht van Marketplace](./media/publish-marketplace-app/publishvm11.png)

De **plannen + prijzen** tabblad voor uw beheerde toepassing de volgende velden worden weergegeven:

![Marketplace-abonnementen](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Azure Portal

De volgende velden worden weergegeven in de samenvatting van uw beheerde toepassing:

![Portal-overzicht](./media/publish-marketplace-app/publishvm12.png)

De volgende velden worden weergegeven in het overzicht voor de beheerde toepassing:

![Overzicht van de portal](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>Richtlijnen voor het logo

Volg deze richtlijnen voor een logo dat u in de Cloud Partner-portal uploaden:

*   Het ontwerp van de Azure heeft een eenvoudige kleurenpalet. Beperk het aantal primaire en secundaire kleuren op uw logo.
*   Themakleur van de portal zijn wit en zwarte. Deze kleuren als de achtergrondkleur die niet worden gebruikt voor het logo. Gebruik een kleur die uw logo opvallende meldingen in de portal maakt. Het wordt aangeraden de eenvoudige primaire kleuren. *Als u een transparante achtergrond gebruikt, zorg ervoor dat het logo en tekst worden niet wit, zwart of blauw.*
*   Gebruik een kleurovergang achtergrond niet op het logo.
*   Plaats geen tekst in het logo, zelfs niet uw bedrijf of de naam. Het uiterlijk van uw logo moet platte en kleurovergangen voorkomen.
*   Zorg ervoor dat het logo niet is gespreid.

#### <a name="hero-logo"></a>Hero-logo

Het logo hero is optioneel. De publisher kunt niet een logo hero uploaden. Nadat het pictogram hero is geüpload, kan niet worden verwijderd. Op dat moment wordt moet de partner volgen na de Marketplace-richtlijnen voor hero pictogrammen.

Volg deze richtlijnen voor het logo hero pictogram:

*   De weergavenaam van de uitgever, de titel van het abonnement en de lange samenvatting aanbieding worden in wit weergegeven. Daarom niet voor de achtergrond van het pictogram hero een lichte kleur te gebruiken. Een zwarte, wit of transparante achtergrond is niet toegestaan voor hero pictogrammen.
*   Nadat de aanbieding wordt weergegeven, worden elementen programmatisch insluiten binnen het hero-logo. De ingesloten elementen zijn de weergavenaam van de uitgever, de titel plan, de aanbieding lange samenvatting en de **maken** knop. Als gevolg daarvan kan niets invoert terwijl u het logo hero ontwerpt. Laat lege ruimte aan de rechterkant, omdat de tekst programmatisch is opgenomen in deze ruimte. De lege ruimte voor de tekst moet 415 x 100 pixels aan de rechterkant. Het wordt verschoven met 370 pixels vanaf de linkerkant.

    ![Voorbeeld van Hero-logo](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>Ondersteuning voor formulier

Vul de **ondersteunen** formulier met ondersteuning voor contactpersonen van uw bedrijf. Deze informatie kan worden engineering, contactpersonen en contactpersonen voor ondersteuning van klant.

## <a name="publish-an-offer"></a>Een aanbieding publiceren

Nadat u alle secties invult, selecteert u **publiceren** om het proces die uw aanbieding beschikbaar aan klanten stelt te starten.

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van beheerde toepassingen](overview.md) voor een inleiding tot beheerde toepassingen.
* Zie voor meer informatie over het publiceren van een Servicecatalogus beheerde toepassing [maken en publiceren van een Servicecatalogus beheerde toepassing](publish-service-catalog-app.md).
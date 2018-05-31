---
title: Door Azure beheerde toepassingen in Marketplace | Microsoft Docs
description: Hier vindt u een beschrijving van door Azure beheerde toepassingen die beschikbaar zijn via Marketplace.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.date: 03/15/2018
ms.author: tomfitz
ms.openlocfilehash: 39797bb4fe2b0576cd5696d7111826dcf807ff5c
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/18/2018
ms.locfileid: "34304528"
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Door Azure beheerde toepassingen in Marketplace

Leveranciers gebruiken door Azure beheerde toepassingen om hun oplossingen aan te bieden aan alle klanten van Azure Marketplace. Voorbeelden van leveranciers zijn MSP's (Managed Service Providers), ISV's (Independent Software Vendors) en SI's (systeemintegrators). Beheerde toepassingen verminderen de overhead voor onderhoud voor klanten. Leveranciers verkopen infrastructuur en software via Marketplace. Ze kunnen services en operationele ondersteuning koppelen aan beheerde toepassingen. Zie [Overzicht van door Azure beheerde toepassingen](overview.md) voor meer informatie.

In dit artikel wordt uitgelegd hoe u een toepassing publiceert naar Marketplace en deze op grote schaal beschikbaar maakt voor klanten.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Vereisten voor het publiceren van een beheerde toepassing

Om dit artikel te kunnen voltooien, moet u over het ZIP-bestand beschikken met de definitie van uw beheerde toepassing. Zie [Een beheerde toepassing voor intern verbruik publiceren](publish-service-catalog-app.md) voor meer informatie.

Bovendien zijn er verschillende zakelijke vereisten. Dit zijn:

* Uw bedrijf (of een dochteronderneming) moet zijn gevestigd in een land waar transacties via Marketplace worden ondersteund.
* Het product moet beschikken over een licentie die compatibel is met factureringsmodellen die worden ondersteund door Marketplace.
* Klanten moeten op een commercieel redelijke wijze toegang hebben tot technische ondersteuning. Deze ondersteuning kan gratis zijn, betaald of worden aangeboden via een community.
* Neem een licentie op uw software en op eventuele afhankelijke software van derden.
* Bied inhoud aan die voldoet aan de criteria die van kracht zijn om uw aanbieding op te nemen in Marketplace en in Azure Portal.
* Ga akkoord met de voorwaarden van het deelnamebeleid en de overeenkomst voor uitgevers van Azure Marketplace.
* Ga akkoord met de gebruiksvoorwaarden, de Microsoft-privacyverklaring en de overeenkomst inzake het Microsoft Azure Certified-programma.

## <a name="become-a-publisher"></a>Registreren als uitgever

Voer de volgende stappen uit om een uitgever te worden in Azure Marketplace:

1. Een Microsoft-id maken: maak een Microsoft-account met een e-mailadres dat deel uitmaakt van het domein van uw bedrijf, maar dat niet aan een bepaalde persoon is gekoppeld. Dit e-mailadres wordt gebruikt voor zowel het Microsoft Developer Center als de Cloud Partner-portal. Zie de [handleiding van Azure Marketplace voor uitgevers](https://aka.ms/sellerguide) voor meer informatie.
1. [Azure Marketplace-aanvraagformulier](https://aka.ms/ampnomination) versturen: selecteer **Managed Application** voor **Solution that you intend to publish?**. Zodra het formulier is verzonden, wordt de aanvraag bekeken en gevalideerd door het Marketplace Onboarding Team. Het goedkeuringsproces kan één tot drie dagen duren. Wanneer uw aanvraag is goedgekeurd, ontvangt u een promotiecode waarmee u de registratiekosten voor het Developer Center kunt voldoen. Als u het Azure Marketplace-aanvraagformulier **niet** invult, moet een registratiebijdrage van $99 betalen.
1. Registreren in [Developer Center](http://dev.windows.com/registration?accountprogram=azure): Microsoft controleert of uw organisatie een geldige rechtspersoon is met een geldig btw-nummer voor het land waar de organisatie is geregistreerd. Het goedkeuringsproces kan vijf tot tien dagen duren. U kunt de registratiekosten voldoen met behulp van de promotiecode die u in de e-mail over de aanvraag hebt ontvangen. Zie de [handleiding van Azure Marketplace voor uitgevers](https://aka.ms/sellerguide) voor meer informatie.
1. Aanmelden bij de [Cloud Partner-portal](https://cloudpartner.azure.com): koppel het Developer Center-account in uw uitgeversprofiel aan het uitgeversprofiel van Marketplace. Zie de [handleiding van Azure Marketplace voor uitgevers](https://aka.ms/sellerguide) voor meer informatie.

## <a name="create-a-new-azure-application-offer"></a>Een nieuwe aanbieding voor een Azure-toepassing maken

Als het account voor de partnerportal is gemaakt, kunt u een aanbieding voor uw beheerde toepassing gaan maken.

### <a name="set-up-an-offer"></a>Een aanbieding samenstellen

De aanbieding voor een beheerde toepassing komt overeen met de klasse van een productaanbieding van een uitgever. Als u een nieuw type toepassing hebt dat u beschikbaar wilt maken in Marketplace, stelt u een nieuwe aanbieding samen. Een aanbieding is een verzameling SKU's. Elke aanbieding wordt als een eigen entiteit weergegeven in Marketplace.

1. Meld u aan bij de [Cloud Partner-portal](https://cloudpartner.azure.com/).

1. Selecteer in het navigatievenster aan de linkerkant **+ New offer** > **Azure Applications**.

1. In de **Editor**-weergave ziet u de vereiste formulieren. De formulieren worden verderop in dit artikel beschreven.

## <a name="offer-settings-form"></a>Offer Settings-formulier

Dit zijn de velden voor het formulier **Offer Settings**:

* **Offer ID**: deze unieke id identificeert de aanbieding binnen een uitgeversprofiel. Deze id wordt weergegeven in product-URL's, Resource Manager-sjablonen en factureringsrapporten. De id kan alleen bestaan uit alfanumerieke tekens in kleine letters en streepjes (-). De id mag niet eindigen op een streepje. Het maximum aantal tekens is 50. Zodra een aanbieding is gepubliceerd, kan dit veld niet meer worden gewijzigd.
* **Publisher ID**: gebruik deze vervolgkeuzelijst om het uitgeversprofiel te kiezen dat u wilt gebruiken voor het publiceren van deze aanbieding. Zodra een aanbieding is gepubliceerd, kan dit veld niet meer worden gewijzigd.
* **Name**: deze weergavenaam voor uw aanbieding wordt weergegeven in Marketplace en in de portal. De naam mag maximaal 50 tekens bevatten. Gebruik een merknaam voor het product die makkelijk te herkennen is. Laat de naam van uw bedrijf weg, tenzij u het product als zodanig wilt publiceren. Als u deze aanbieding al op uw eigen website onder de aandacht wordt gebracht, zorg er dan voor dat de naam exact overeenkomt met de naam op uw website.

Als u klaar bent, selecteert u **Save** om verder te gaan.

## <a name="skus-form"></a>Formulier voor SKU's

De volgende stap is het toevoegen van SKU's voor uw aanbieding.

Een SKU is de kleinst mogelijke eenheid van een aanbieding die kan worden gekocht. U kunt een SKU binnen dezelfde productklasse (aanbieding) gebruiken om onderscheid te maken tussen:

* Verschillende functies die worden ondersteund
* Een beheerde of onbeheerde aanbieding
* Factureringsmodellen die worden ondersteund

Een SKU wordt in Marketplace weergegeven onder de bovenliggende aanbieding. Een SKU wordt als een afzonderlijk verkrijgbare entiteit weergegeven in Azure Portal.

1. Selecteer **SKUs** > **New SKU**.

1. Geef een waarde op voor **SKU ID**. Een SKU-id is een unieke id voor de SKU binnen een aanbieding. Deze id wordt weergegeven in product-URL's, Resource Manager-sjablonen en factureringsrapporten. De id kan alleen bestaan uit alfanumerieke tekens in kleine letters en streepjes (-). De id mag niet eindigen op een streepje en kan uit maximaal 50 tekens bestaan. Zodra een aanbieding is gepubliceerd, kan dit veld niet meer worden gewijzigd. U kunt meerdere SKU's hebben binnen een aanbieding. U hebt een SKU nodig voor elke installatiekopie die u wilt publiceren.

1. Vul de sectie **SKU Details** in op het volgende formulier:

   Vul de volgende velden in:

   * **Title**: voer een titel in voor deze SKU. Deze titel wordt weergegeven in de galerie voor dit item.
   * **Summary**: voer een korte samenvatting in voor deze SKU. Deze tekst wordt onder de titel weergegeven.
   * **Description**: voer een gedetailleerde beschrijving van de SKU in.
   * **SKU Type**: de toegestane waarden zijn *Managed Application* en *Solution Templates*. Selecteer voor deze aanvraag *Managed Application*.
   * **Country/Region availability**: selecteer de landen waar de beheerde toepassing beschikbaar is.
   * **Pricing**: geef een prijs op voor beheer van de toepassing. Selecteer de beschikbare landen voordat u de prijs instelt.

1. Voeg een nieuw pakket toe. Vul de sectie **Package Details** in op het volgende formulier:

   Vul de volgende velden in:

   * **Current Version**: voer een versie in voor het pakket dat u uploadt. Gebruik hierbij de notatie `{number}.{number}.{number}{number}`.
   * **Select a package file**: dit pakket bevat twee vereiste bestanden die in één ZIP-pakket zijn gecomprimeerd. Het ene bestand is een Resource Manager-sjabloon die de resources definieert die voor de beheerde toepassing moeten worden geïmplementeerd. Het andere bestand definieert de [gebruikersinterface](create-uidefinition-overview.md) voor consumenten die de beheerde toepassing implementeren via de portal. In de gebruikersinterface geeft u elementen op waarmee consumenten parameterwaarden kunnen opgeven.
   * **PrincipalId**: deze eigenschap is de id van Azure Active Directory (Azure AD) van een gebruiker, groep of toepassing die toegang krijgt tot de resources in het abonnement van de klant. In de roldefinitie worden de machtigingen beschreven.
   * **Role Definition**: deze eigenschap bestaat uit een lijst met alle ingebouwde rollen voor op rollen gebaseerd toegangsbeheer (RBAC) die worden aangeboden door Azure AD. U kunt de rol selecteren die het meest geschikt is voor het beheren van resources namens de klant.

U kunt meerdere autorisaties toevoegen. Het wordt aangeraden om een AD-gebruikersgroep te maken en de id van deze groep op te geven voor **PrincipalId**. Op deze manier kunt u meer gebruikers toevoegen aan de gebruikersgroep zonder dat u de SKU hoeft bij te werken.

Zie [Aan de slag met toegangsbeheer op basis van rollen in Azure Portal](../role-based-access-control/overview.md) voor meer informatie over RBAC.

## <a name="marketplace-form"></a>Marketplace-formulier

U gebruikt het formulier Marketplace om aan te geven welke velden moeten worden weergegeven in [Azure Marketplace](https://azuremarketplace.microsoft.com) en in [Azure Portal](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Preview van abonnement-id's

Geef een lijst met id's van Azure-abonnementen op die toegang hebben tot de aanbieding nadat deze is gepubliceerd. U kunt deze gebruiken met abonnementen op de whitelist om de preview-aanbieding te testen voordat deze live gaat. U kunt een whitelist van maximaal 100 abonnementen samenstellen in de partnerportal.

### <a name="suggested-categories"></a>Voorgestelde categorieën

Selecteer maximaal vijf categorieën uit de lijst waaraan uw aanbieding het best kan worden gekoppeld. Deze categorieën worden gebruikt om uw aanbieding toe te wijzen aan de productcategorieën die beschikbaar zijn in [Azure Marketplace](https://azuremarketplace.microsoft.com) en [Azure Portal](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Marketplace

De samenvatting van uw beheerde toepassing bevat de volgende velden:

![Samenvatting van Marketplace](./media/publish-marketplace-app/publishvm10.png)

Het tabblad **Overview** voor uw beheerde toepassing bevat de volgende velden:

![Overzicht van Marketplace](./media/publish-marketplace-app/publishvm11.png)

Het tabblad **Plans + Pricing** voor uw beheerde toepassing bevat de volgende velden:

![Marketplace-abonnementen](./media/publish-marketplace-app/publishvm15.png)

#### <a name="azure-portal"></a>Azure Portal

De samenvatting van uw beheerde toepassing bevat de volgende velden:

![Portal-samenvatting](./media/publish-marketplace-app/publishvm12.png)

Het overzicht voor uw beheerde toepassing bevat de volgende velden:

![Overzicht van portal](./media/publish-marketplace-app/publishvm13.png)

#### <a name="logo-guidelines"></a>Logorichtlijnen

Volg deze richtlijnen voor logo's die u uploadt in de Cloud Partner-portal:

*   Het Azure-ontwerp heeft een eenvoudig kleurenpalet. Beperk het aantal primaire en secundaire kleuren in uw logo.
*   De themakleuren van de portal zijn wit en zwart. Gebruik deze kleuren niet als de achtergrondkleur voor uw logo. Gebruik een kleur waardoor uw logo opvalt in de portal. We adviseren eenvoudige primaire kleuren. *Als u een transparante achtergrond gebruikt, moeten het logo en de tekst niet wit, zwart of blauw zijn.*
*   Gebruik geen achtergrond met een kleurovergang in het logo.
*   Plaats geen tekst in het logo, ook niet de naam van uw bedrijf of merk. Het uiterlijk van uw logo moet plat zijn, zonder kleurovergangen.
*   Zorg ervoor dat het logo niet is uitgerekt.

#### <a name="hero-logo"></a>Hero-logo

Het hero-logo is optioneel. De uitgever kan ervoor kiezen om geen hero-logo te uploaden. Als er een hero-pictogram is geüpload, kan dit niet meer worden verwijderd. Op dat moment moet de partner de Marketplace-richtlijnen voor hero-pictogrammen volgen.

Volg deze richtlijnen voor het hero-logopictogram:

*   De weergavenaam van de uitgever, de titel van het abonnement en de lange samenvatting van de aanbieding worden in wit weergegeven. Gebruik dan ook geen lichte kleur voor de achtergrond van het hero-pictogram. Een zwarte, witte of transparante achtergrond is niet toegestaan voor hero-pictogrammen.
*   Zodra de aanbieding wordt vermeld, worden elementen programmatisch ingesloten binnen het hero-logo. De ingesloten elementen zijn de weergavenaam van de uitgever, de titel van het abonnement, de lange samenvatting van de aanbieding en de knop **Maken**. U hoeft dan ook geen tekst in te voeren wanneer u het hero-logo ontwerpt. Laat aan de rechterkant ruimte vrij, aangezien de tekst programmatisch daar wordt opgenomen. De lege ruimte voor de tekst moet 415 x 100 pixels zijn aan de rechterkant. De ruimte wordt met 370 pixels vanaf de linkerkant verschoven.

    ![Voorbeeld van hero-logo](./media/publish-marketplace-app/publishvm14.png)

## <a name="support-form"></a>Support-formulier

Vul het formulier **Support** in met gegevens van contactpersonen voor ondersteuning van uw bedrijf. Dit kunnen bijvoorbeeld contactpersonen zijn voor engineering en voor klantenondersteuning.

## <a name="publish-an-offer"></a>Een aanbieding publiceren

Als u alle secties hebt ingevuld, selecteert u **Publish** om het proces te starten waarmee uw aanbieding beschikbaar komt voor klanten.

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van beheerde toepassingen](overview.md) voor een inleiding tot beheerde toepassingen.
* Zie [Een beheerde toepassing voor intern verbruik publiceren](publish-service-catalog-app.md) voor meer informatie over het publiceren van een beheerde toepassing in een servicecatalogus.
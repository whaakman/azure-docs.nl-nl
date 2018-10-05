---
title: Een Azure beheerde toepassing in Azure Marketplace publiceren
description: Een Azure beheerde toepassing in Azure Marketplace publiceren
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: bc044c8b59c939163336ecab01546fc26a7a2643
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48809961"
---
<a name="publish-an-azure-managed-application-to-azure-marketplace"></a>Een Azure beheerde toepassing in Azure Marketplace publiceren 
========================================================

Dit artikel worden de verschillende stappen voor het publiceren van een aanbieding van beheerde toepassingen op Azure Marketplace.

<a name="pre-requisites-for-publishing-a-managed-application"></a>Vereisten voor het publiceren van een beheerde toepassing 
---------------------------------------------------

Vereisten voor vermelding op Azure Marketplace

1.  Technisch

    -   [Azure Resource Manager-sjablonen maken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

    -   Azure Quickstart-sjablonen:

        -   <https://azure.microsoft.com/documentation/templates/>

        -   <https://github.com/azure/azure-quickstart-templates>

    -   UI-definitie maken

        -   [Gebruikersinterface definition-bestand maken](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

2.  Niet-technische (zakelijke vereisten)

    -   Uw bedrijf (of een dochteronderneming) moet zich bevinden in een land van verkopen oorsprong wordt ondersteund door de Azure Marketplace

    -   Uw product een licentie moet worden verleend op een manier die compatibel is met de factureringsmodellen ondersteund door de Azure Marketplace

    -   U bent verantwoordelijk voor technische ondersteuning bieden aan uw klanten: gratis, betaald, of via communityondersteuning.

    -   U bent verantwoordelijk voor licentieverlening van uw software en eventuele afhankelijkheden van software van derden.

    -   U moet bevatten inhoud die voldoet aan criteria voor uw aanbieding wilt laten vermelden op Azure Marketplace en in de Azure Management Portal

    -   U moet akkoord gaan met de voorwaarden van de Deelnamebeleid voor Azure Marketplace en de overeenkomst voor uitgevers

    -   U moet akkoord gaan om te voldoen aan de gebruiksvoorwaarden en privacyverklaring van Microsoft overeenkomst van Microsoft Azure Certified-programma.

<a name="how-to-create-a-new-azure-application-offer"></a>Over het maken van een nieuwe aanbieding voor Azure-toepassing 
-------------------------------------------

Als alle vereisten wordt voldaan, bent u klaar om te beginnen met het ontwerpen van uw aanbieding beheerde toepassing. Voordat we beginnen, een snel overzicht van een aanbieding en een SKU

**Aanbieding**

Een Azure-toepassing-aanbieding komt overeen met een klasse van het product van een uitgever. Hebt u een nieuwe oplossing/toepassing in Azure Marketplace worden gepubliceerd, is een nieuwe aanbieding voor de manier om te gaan. Een aanbieding is een verzameling SKU's. Elke aanbieding wordt weergegeven als een eigen entiteit in Azure Marketplace.

**SKU**

Een SKU is een aanbieding met de kleinste die kunnen worden gekocht eenheid. Vanuit de dezelfde productklasse (aanbieding), kunnen SKU's u onderscheid maken tussen de verschillende functies die worden ondersteund, of de aanbieding is beheerd of niet-beheerde en factureringsmodellen.

Meerdere SKU's toevoegen als u ondersteuning voor verschillende factureringsmodellen: zoals Breng uw eigen licentie (BYOL), betalen (betalen per gebruik), enzovoort. 

Voeg meerdere SKU's als elke SKU een eigen functieset ondersteunt, en verschillend zijn geprijsd.

Een SKU wordt weergegeven onder de bovenliggende aanbieding in Azure Marketplace terwijl deze weergegeven als een eigen die kunnen worden gekocht entiteit op azure.com wordt.

1.  Aanmelden bij de [Cloud Partner-Portal](http://cloudpartner.azure.com).

2.  Klik in de linker navigatiebalk op \"+ nieuwe aanbieding\" en selecteer \"Azure-toepassingen\".

    ![Nieuwe aanbieding](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  Een nieuwe aanbieding \"Editor\" weergave wordt nu geopend en kunt u gaan ontwerpen.

4.  De \"formulieren\" die moeten worden ingevuld zijn zichtbaar aan de linkerkant binnen de \"Editor\" weergeven. Elke \"formulier\" bestaat uit een set velden die moeten worden ingevuld. Vereiste velden zijn gemarkeerd met een rode akterisksymbool (\*).

    > Er zijn 4 hoofdformulieren voor het ontwerpen van een beheerde toepassing

    -   Aanbiedingsinstellingen

    -   Voorraadeenheden

    -   Marketplace

    -   Ondersteuning

<a name="how-to-fill-out-the-offer-settings-form"></a>Het invullen van het formulier instellingen bieden 
---------------------------------------

Het formulier van de instellingen voor aanbieding is een eenvoudige methode om de instellingen van de aanbieding te geven.
De verschillende velden worden hieronder beschreven.

**Aanbiedings-ID**

Een unieke id voor de aanbieding binnen een publisher-profiel.
Deze is zichtbaar in de URL's op product: Resource Manager-sjablonen en facturering rapporten. Alleen kleine letters alfanumerieke tekens of streepjes (-) zijn toegestaan. Eindigen met een streepje mag niet of langer zijn dan een maximum van 50 tekens. Dit veld is vergrendeld zodra een aanbieding live meteen.

**Uitgevers-ID**

Deze vervolgkeuzelijst kunt u de uitgeverprofiel dat u wilt publiceren, deze aanbieding onder kiezen. Dit veld is vergrendeld zodra een aanbieding live meteen.

**Naam**

De weergavenaam voor uw aanbieding. De naam die wordt weergegeven in Azure Marketplace en in Azure portal. De naam mag maximaal 50 tekens bevatten. Richtlijnen hier is een herkenbare naam merk voor uw product op te nemen. Geen hier de naam van uw bedrijf, tenzij dat is hoe deze wordt gebracht. Als u deze aanbieding worden marketing op uw eigen website, controleert u of de naam precies hoe deze wordt weergegeven in uw website.

Klik op \"opslaan\" uw voortgang opslaan. Volgende stap zou zijn om toe te voegen van SKU's voor uw aanbieding.

<a name="how-to-create-skus"></a>Over het maken van SKU 's 
------------------

Klik op de \"SKU's\" formulier. Hier ziet u een optie voor het \"Voeg een SKU toe\" te klikken op waarmee u een \"SKU-ID\".

![nieuwe aanbieding voor SKU 's](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

\"SKU-ID\" is de unieke id voor de SKU in een aanbieding. Deze ID in URL's voor product, ARM-sjablonen, zichtbaar zijn en facturering rapporten. De id kan alleen bestaan uit alfanumerieke tekens in kleine letters en streepjes (-).
Deze kan mag niet eindigen met een streepje en een maximum van 50 tekens. Dit veld is vergrendeld zodra een aanbieding live meteen. U kunt meerdere SKU's hebben binnen een aanbieding. Een SKU moet u voor elke afbeelding die u van plan bent om te publiceren.

Zodra een SKU is toegevoegd, wordt deze weergegeven in de lijst met SKU's binnen de \"SKU's\" formulier. Klik op de naam van de SKU toegang te krijgen tot de details van die bepaalde SKU. Hier volgen enkele details voor een aantal velden.

Nadat u hebt geklikt op \"nieuwe Voorraadeenheid\", moet u het volgende formulier invullen:

![Nieuwe aanbieding - nieuwe Voorraadeenheid](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)

### <a name="how-to-fill-sku-details-section"></a>Hoe u sectie Sku-Details 

**Titel** -biedt een titel voor deze Sku. Dit is wat wordt weergegeven in de galerie voor dit item.

**Samenvatting** -bevat een korte samenvatting voor deze sku. Deze tekst wordt direct onder de titel weergegeven.

**Beschrijving** -wordt een gedetailleerde beschrijving over de SKU.

**SKU-Type** -de toegestane waarden zijn \"beheerde toepassingen\" en \"Oplossingssjablonen\". In dit geval, selecteert u \"beheerde toepassingen\".

### <a name="how-to-fill-package-details-section"></a>Hoe u pakketgegevens sectie 

De sectie pakket heeft de volgende velden die moeten worden ingevuld

![Nieuwe aanbieding - nieuwe SKU-pakket](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_package.png)

**Huidige versie** -biedt een versie van het pakket dat u uploadt.
Deze moet de indeling.

**Pakketbestand** -het pakket bevat de volgende bestanden die in een ZIP-bestand zijn gecomprimeerd.

applianceMainTemplate.json - de implementatie-sjabloonbestand dat wordt gebruikt voor het implementeren van de oplossing/toepassing en de resources die zijn gedefinieerd in het maken. U vindt meer informatie over hoe u de auteur van implementatie-sjabloonbestanden hier: <https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template>

applianceCreateUIDefinition.json - dit bestand wordt gebruikt door de Portal op azure.com voor het genereren van de gebruikersinterface voor het inrichten van deze oplossing/toepassing. U vindt meer informatie over het maken van dit bestand hier: <https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview>

mainTemplate.json - het sjabloonbestand die alleen de bron Microsoft.Solution/appliances bevat. De belangrijkste eigenschappen van deze resource om te worden op de hoogte van zijn als volgt:

-   \"type\" -de waarde moet \"Marketplace\" in het geval van Marketplace-beheerde toepassingen-scenario
-   \"ManagedResourceGroupId\": de resourcegroep in de klant\'s-abonnement waarin de resources die zijn gedefinieerd in de applianceMainTemplate.json worden geïmplementeerd.
-   \"PublisherPackageId\": de tekenreeks die een unieke identificatie van het pakket. 

De waarde moet als volgt samengesteld: u kunt een samenvoeging van \[publisherId\].\[ OfferId\]-preview\[SKUID\].\[ PackageVersion\].
Elk van deze waarden kan worden verkregen, zoals hieronder weergegeven.

Dit pakket bevat geen geneste sjablonen of scripts die zijn vereist voor het inrichten van is deze toepassing. De mainTemplate.json, applianceMainTemplate.json en applianceCreateUIDefinition.json moet aanwezig zijn in de hoofdmap zijn.

**Autorisaties** -deze eigenschap wordt gedefinieerd wie er toegang en welk niveau van toegang tot de resources in de abonnementen van klanten. Hierdoor kunnen de uitgever voor het beheren van de toepassing namens de klant.

-   PrincipalId - de Azure Active directory-ID van een gebruiker, groep of toepassing die wordt verleend bepaalde machtigingen (zoals is beschreven in de definitie van de rol) op de resources in het klantabonnement.

-   Roldefinitie - een lijst van alle de ingebouwde RBAC-rollen geleverd door Azure AD. U kunt de meest geschikte rol waarmee u kunt voor het beheren van de resources namens de klant.

Meerdere autorisaties kunnen worden toegevoegd. Het wordt echter aanbevolen om te maken van een Active Directory-gebruikersgroep en geef de ID in de \"PrincipalId\."  Hiermee schakelt u het toevoegen van meer gebruikers aan de groep zonder te hoeven bijwerken van de SKU.

Meer informatie over RBAC vindt u hier: <https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is>

<a name="marketplace-form"></a>Marketplace-formulier
----------------

Het formulier marketplace binnen een azure-toepassing-aanbieding wordt gevraagd voor velden die worden weergegeven op [Azure Marketplace](https://azuremarketplace.microsoft.com) en klik op [Azure Portal](https://portal.azure.com/). Hier volgen enkele details voor een aantal velden.

#### <a name="preview-subscription-ids"></a>Preview-versie abonnement-id 's

Voer hier een lijst van Azure-abonnement-id's die u toegang hebben tot de aanbieding wilt nadat deze is gepubliceerd. Deze abonnementen wit vermeld, kunt u de voorbeeld-aanbieding voordat u het testen live. De partner-portal kunt u witte lijst met maximaal 100 abonnementen.

#### <a name="suggested-categories"></a>Aanbevolen categorieën

Selecteer maximaal vijf categorieën in de opgegeven lijst dat uw aanbieding beste is gekoppeld. De geselecteerde categorieën worden gebruikt om uw aanbieding worden toegewezen aan de productcategorieën die beschikbaar zijn in [Azure Marketplace](https://azuremarketplace.microsoft.com) en [Azure Portal](https://portal.azure.com/).

Hier volgen enkele van de locaties die de gegevens die u in dit formulier opgeeft wordt weergegeven in.

##### <a name="azure-marketplace"></a>Azure Marketplace

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)

![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)

![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)

##### <a name="portal-at-azurecom"></a>Portal op azure.com

![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)

![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)

##### <a name="logo-guidelines"></a>Richtlijnen voor het logo

Alle de logo's die in de Cloud Partner-Portal zijn geüpload moeten Volg de onderstaande richtlijnen:

-   Het Azure-ontwerp heeft een eenvoudig kleurenpalet. Houd het aantal primaire en secundaire kleuren op uw logo laag.

-   De themakleuren van de portal op azure.com zijn wit en zwart. Vermijd het gebruik van deze kleuren als de achtergrondkleur van uw logo's. Gebruik een kleur die uw logo's opvallende in de portal op azure.com.
    We adviseren eenvoudige primaire kleuren. **Als u transparante achtergrond gebruikt, zorg ervoor dat de logo's en de tekst niet wit, zwarte of blauwe wordt weergegeven.**

-   Gebruik niet een kleurovergang achtergrond op het logo.

-   Plaats tekst, zelfs in uw bedrijf, of een naam, het logo huisstijl.
    Het uiterlijk van uw logo moet \'platte\' en vermijd kleurovergangen.

-   Vermijd het logo stretching.

##### <a name="hero-logo"></a>Hero-Logo

De Hero-logo is optioneel. De publisher kunt niet een Hero-logo uploaden. Maar eenmaal geüploade de hero-pictogram kan niet worden verwijderd. In dat geval moet de Azure Marketplace-richtlijnen voor de Hero-pictogrammen volgt u de partner.

###### <a name="guidelines-for-the-hero-logo-icon"></a>Richtlijnen voor het pictogram van Hero-logo

-   De weergavenaam van de uitgever, plan titel en de lange samenvatting aanbieding worden weergegeven in de kleur wit. Voorkom dat een lichte kleur op de achtergrond van het Hero-pictogram. Zwart, wit en transparante achtergrond zijn niet toegestaan voor de Hero-pictogrammen.

-   Wanneer de aanbieding wordt weergegeven, worden de weergavenaam van de uitgever, plan titel, de aanbieding voor lange samenvatting en de knop maken via een programma ingesloten in het Hero-logo. U hoeft niet te invoeren bij het ontwerpen van de Hero-logo. Laat de eigenschap spaties aan de rechterkant voor publisher weergavenaam, plan titel, de aanbieding lange samenvatting en enzovoort. Ze zijn via een programma inbegrepen.
    Opslagruimten leeg voor de tekst die moet worden 415 x 100 aan de rechterkant en gecompenseerd door 370 pixels vanaf de linkerkant.

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

<a name="support-form"></a>Ondersteuning voor formulier
------------

Het volgende formulier om in te vullen is het ondersteuningsformulier. In dit formulier dient voor contactpersonen voor ondersteuning van uw bedrijf.  Enkele voorbeelden zijn uw technische informatie en de contactgegevens voor de ondersteuning van de klant.

<a name="how-to-publish-an-offer"></a>Een aanbieding publiceren
-----------------------

Nu dat uw aanbieding is opgesteld, wordt de volgende stap is het publiceren van de aanbieding op Azure Marketplace. Klik op de \"publiceren\" knop start u het proces van het maken van uw aanbieding live.

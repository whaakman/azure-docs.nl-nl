---
title: Publiceren van een oplossingssjabloon | Microsoft Docs
description: Een oplossingssjabloon publiceren in Azure Marketplace.
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
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: 5320b8d5ca7456a6f1b0fdd1372c9f39ac1edfb0
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219406"
---
# <a name="publish-a-solution-template-to-azure-marketplace"></a>Een oplossingssjabloon publiceren in Azure Marketplace

Dit artikel bevat de stappen voor het publiceren van een oplossingssjabloon bieden op Azure Marketplace.

## <a name="prerequisites"></a>Vereisten

De volgende technische en niet-technische vereisten gelden voor het weergeven van een oplossingssjabloon op Azure Marketplace.

### <a name="technical"></a>Technisch

- [Informatie over Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

- Azure Quickstart-sjablonen:

    - [Documentatie voor Azure-quickstart-sjabloon](https://azure.microsoft.com/documentation/templates/)

    - [Azure Quick Start-documentatie op GitHub](https://github.com/azure/azure-quickstart-templates)

 - [Gebruikersinterface in Azure portal-gebruiker een bestand maken](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

### <a name="non-technical-business-requirements"></a>Niet-technische (zakelijke vereisten)

-   Uw bedrijf (of een dochteronderneming) moet zich bevinden in een verkoop van land/regio ondersteund via de Azure Marketplace.

-   Uw product moet op een manier die compatibel is met de factureringsmodellen ondersteund door de Azure Marketplace in licentie worden gegeven.

-   U bent verantwoordelijk voor het maken van technische ondersteuning beschikbaar voor klanten in een commercieel redelijke manier of gratis en betaalde of via communityondersteuning.

-   U bent zelf verantwoordelijk voor licentiëring van uw software en eventuele afhankelijkheden voor software van derden.

-   Geef de inhoud die voldoet aan criteria voor uw aanbieding wilt laten vermelden op Azure Marketplace en in de Azure Management Portal.

-   Ga akkoord met de voorwaarden van de Deelnamebeleid voor Azure Marketplace en de overeenkomst voor uitgevers.

-   Ga akkoord met de gebruiksvoorwaarden, de Microsoft-privacyverklaring en de overeenkomst inzake het Microsoft Azure Certified-programma.

## <a name="before-you-begin"></a>Voordat u begint

Als u aan alle vereisten, kunt u beginnen met het ontwerpen van uw aanbieding oplossingssjabloon. Voordat u begint, Controleer u de volgende aanbieding en SKU-gegevens.

**Aanbieding**

Een Azure-toepassing-aanbieding komt overeen met een klasse van het product van een uitgever. Als u een nieuw type oplossing/toepassing die u graag beschikbaar zijn in Azure Marketplace hebt, is een nieuwe aanbieding voor de beste benadering. Een aanbieding is een verzameling SKU's. Elke aanbieding wordt weergegeven als een eigen entiteit in Azure Marketplace.

**SKU**

Een SKU is de kleinst mogelijke eenheid van een aanbieding die kan worden gekocht. Vanuit de dezelfde productklasse (aanbieding), kunnen u onderscheid maken tussen verschillende functies van de ondersteunde SKU's. Bijvoorbeeld, de aanbieding wordt beheerd of niet-beheerde en verschillende factureringsmodellen worden ondersteund.

Voeg meerdere SKU's in de volgende scenario's:
- Wilt u ondersteuning voor verschillende factureringsmodellen, zoals Breng uw eigen licentie (BYOL) of betalen (betalen per gebruik).
- Elke SKU ondersteunt een reeks verschillende functies en de prijs van elke reeks functies is anders.

Een SKU wordt weergegeven onder de bovenliggende aanbieding in Azure Marketplace en wordt weergegeven als een eigen entiteit die kunnen worden gekocht in Azure portal.

## <a name="to-create-a-new-offer"></a>Een nieuwe aanbieding maken

1.  Aanmelden bij de [Cloud Partner-Portal](http://cloudpartner.azure.com/).

2.  Selecteer op de linker navigatiebalk **+ nieuwe aanbieding**, en selecteer vervolgens **Azure-toepassingen**.

    ![Een nieuwe aanbieding maken](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  Onder **nieuwe aanbieding**, selecteer **Editor**.

    ![Een nieuwe aanbieding-editor](./media/cloud-partner-portal-publish-managed-app/newOffer_OfferSettings.png)

4.  Onder **Editor**, biedt u informatie in de volgende weergaven:
    - Aanbiedingsinstellingen
    - Voorraadeenheden
    - Marketplace
    - Ondersteuning

Elke weergave bevat een set velden voor u om in te vullen. Vereiste velden worden aangegeven met een rode akterisksymbool (\*)

## <a name="to-configure-offer-settings"></a>Het configureren van instellingen bieden

1. Configureer de volgende **identiteit bieden** velden in de instellingen bieden.

    **Aanbiedings-ID**

     Een unieke id voor de aanbieding binnen een publisher-profiel. Deze ID wordt weergegeven in de URL's voor product, ARM-sjablonen en facturering rapporten. U kunt alleen kleine letters, alfanumerieke tekens en streepjes (-). De ID kan mag niet eindigen met een streepje en een maximum van 50 tekens. 
    >[!Note]
    >Dit veld wordt vergrendeld wanneer een aanbieding live meteen.

    **Uitgevers-ID**

    Een vervolgkeuzelijst voor uitgeverprofiel. Kies het profiel dat u wilt publiceren van de aanbieding onder. 
    >[!Note]
    >Dit veld wordt vergrendeld wanneer een aanbieding live meteen.

    **Naam**

    De weergavenaam voor uw aanbieding. Deze naam wordt weergegeven in Azure Marketplace en in Azure Portal. De naam mag maximaal 50 tekens bevatten. Gebruik de volgende richtlijnen voor de aanbiedingsnaam van:
    -  Gebruik een merknaam voor het product die makkelijk te herkennen is. 
    - Neem hier de naam van uw bedrijf niet, tenzij dat is hoe de aanbieding is gebracht.
    - Als u bent deze aanbieding op uw eigen website marketing, zorg er dan voor dat de naam is identiek aan de naam op uw website.

2. Selecteer **opslaan** instellingen bieden voltooid.
voor uw aanbieding.

## <a name="to-create-skus"></a>SKU's maken
------------------

1. Selecteer **SKU's**. 

    ![Nieuwe Voorraadeenheid](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

    SKU-ID is een unieke id voor de SKU in een aanbieding. Deze ID wordt weergegeven in de URL's voor product, ARM-sjablonen en facturering rapporten. De SKU-ID:
    - Kan alleen een maximum van 50 tekens hebben.
    - Kan alleen worden samengesteld uit kleine letters, alfanumerieke tekens en streepjes (-).
    - De id mag niet eindigen op een streepje.

    >[!Note]
    >Nadat een SKU is toegevoegd, wordt deze weergegeven in de lijst van SKU's in de weergave van de SKU's. Selecteer de SKU-naam voor de SKU-informatie. 

2. Selecteer **nieuwe Voorraadeenheid** wordt weergegeven in de volgende schermopname gegevens op te geven. 

    ![SKU-details](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)


### <a name="sku-settings"></a>SKU-instellingen

Geef de volgende instellingen voor de SKU.

- **Titel** -een titel voor de SKU. Deze titel wordt weergegeven in de galerie voor dit item.

- **Samenvatting** : een korte samenvatting beschrijving van de SKU. (Maximale lengte is 100 tekens.)

- **Beschrijving** : een gedetailleerde beschrijving van de SKU.

- **SKU-Type** -een vervolgkeuzelijst met deze waarden: 'Managed-toepassing (Preview)' en 'Oplossingssjabloon'. Selecteer voor dit scenario **oplossingssjabloon**.

- **Beschikbaarheid in de cloud** -de locatie van de SKU. De standaardwaarde is de openbare Azure.

### <a name="package-details"></a>Pakketdetails

Nadat u de SKU-instellingen hebt voltooid, moet u de volgende pakketgegevens bevatten.

![Pakketdetails](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_ST_package.png)

- **Huidige versie** -de versie van het pakket dat u zult uploaden. Deze moet de indeling -...

- **Pakketbestand** -dit pakket bevat de volgende bestanden, die zijn opgeslagen in een ZIP-bestand.

    -   MainTemplate.json - de implementatie-sjabloonbestand dat wordt gebruikt voor het implementeren van de oplossing/toepassing en de resources die zijn gedefinieerd voor de oplossing te maken. Zie voor meer informatie, [over het ontwerpen van implementatie-sjabloonbestanden](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)

    -   createUIDefinition.json - dit bestand wordt gebruikt door de Azure-Portal voor het genereren van de gebruikersinterface voor het inrichten van deze oplossing/toepassing. Zie voor meer informatie, [maken-Azure portal gebruikersinterface voor uw beheerde toepassing](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

    >[!IMPORTANT]
    >Dit pakket bevat geen andere geneste sjablonen of scripts die nodig zijn voor het inrichten van deze toepassing. De mainTemplate.json en createUIDefinition.json moeten zich in de hoofdmap.

## <a name="to-configure-the-marketplace"></a>Het configureren van de Marketplace

Gebruik de Marketplace-weergave voor het configureren van de velden die worden weergegeven voor de aanbieding op [Azure Marketplace](https://azuremarketplace.microsoft.com) en klik op [Azure Portal](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Preview-versie abonnement-id 's

De lijst met Azure-abonnement-id's die u toegang hebben tot de aanbieding wilt wanneer de aanbieding wordt gepubliceerd. Deze abonnementen wit vermeld, kunt u voor het testen van de voorbeeld-aanbieding voordat u het live. De partner-portal kunt u witte lijst met maximaal 100 abonnementen.

### <a name="suggested-categories"></a>Aanbevolen categorieën

Selecteer maximaal 5 categorieën in de opgegeven lijst dat uw aanbieding beste is gekoppeld. De geselecteerde categorieën worden gebruikt om uw aanbieding worden toegewezen aan de productcategorieën die beschikbaar zijn in [Azure Marketplace](https://azuremarketplace.microsoft.com) en [Azure Portal](https://portal.azure.com/).

De volgende voorbeelden ziet de marketplace-gegevens in de Azure Marketplace en de Azure-Portal.

**Azure Marketplace**

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)


**Azure Portal**


![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)


![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)


### <a name="logo-guidelines"></a>Richtlijnen voor het logo

Volg deze richtlijnen voor het logo's uploaden naar de Cloud Partner-Portal:

-   Het Azure-ontwerp heeft een eenvoudig kleurenpalet. Houd het aantal primaire en secundaire kleuren op uw logo laag.

-   De themakleuren van het Azure-portal zijn wit en zwart. Vermijd het gebruik van deze kleuren als de achtergrondkleur van uw logo's. Gebruik een kleur die uw logo's opvallende in Azure portal. We adviseren eenvoudige primaire kleuren.

    >[!Note] 
    >Als u van een transparante achtergrond gebruikmaakt, moet u ervoor zorgen dat de logo's / tekst worden niet wit, zwart-wit of blauw.

-   Gebruik geen achtergrond met een kleurovergang in het logo.

-   Plaats tekst op het logo. Dit omvat uw bedrijf of een merk. Het uiterlijk van uw logo moet *platte* en vermijd kleurovergangen.

-   Het logo mag niet worden uitgebreid.

#### <a name="hero-logo"></a>Hero-Logo

De Hero-logo is optioneel. De publisher kunt niet een Hero-logo uploaden. Worden echter, nadat het logo is geüpload, deze kan niet verwijderd. De partner moet voldoen aan de Azure Marketplace-richtlijnen voor de Hero-pictogrammen.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Richtlijnen voor het pictogram van Hero-logo

-   De weergavenaam van de uitgever, plan titel en de lange samenvatting van de aanbieding worden weergegeven met een witte gekleurde lettertype. Vermijd het gebruik van een lichte kleur op de achtergrond. Zwart-wit en transparante achtergrond zijn niet toegestaan voor de Hero-pictogrammen.

-   De uitgever weergavenaam, plan titel, de aanbieding voor lange samenvatting en de knop maken zijn ingesloten via een programma in de Hero-logo wanneer van de aanbieding weergegeven. Voer de tekst bij het ontwerpen van de Hero-logo. Laat een lege ruimte aan de rechterkant van het logo. Deze ruimte moet 415 x 100 pixels en wordt gecompenseerd door 370 pixels vanaf de linkerkant.

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

## <a name="to-configure-support"></a>Het configureren van ondersteuning

Gebruik de weergave van de ondersteuning voor de volgende informatie:

- Contactpersonen van uw bedrijf, zoals technische ondersteuning.
- Contactpersonen voor ondersteuning van klant.

## <a name="to-publish-the-offer"></a>De aanbieding publiceren

De laatste stap is het publiceren van de aanbieding. Selecteer **Publiceren**.

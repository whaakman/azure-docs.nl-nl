---
title: SaaS-toepassing technische Publicatiehandleiding voor | Microsoft Docs
description: Wordt uitgelegd hoe u SaaS-toepassingen-aanbiedingen publiceren naar de juiste marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: pbutlerm
ms.openlocfilehash: 7e648d6d3bd8d8ac14fb3f9723f4667c1cc33ac6
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620180"
---
<a name="saas-application-technical-publishing-guide"></a>SaaS-toepassing technische Publicatiehandleiding voor
===========================================

Welkom bij de SaaS-toepassingen technische Publicatiehandleiding voor! Deze handleiding is ontworpen om u te helpen candidate en bestaande uitgevers in de lijst van hun toepassingen en services in AppSource of Azure Marketplace met behulp van de SaaS-toepassingen bieden.

Voor een overzicht van alle andere Marketplace-aanbiedingen, raadpleegt u de [Marketplace Publisher Guide](https://aka.ms/sellerguide).


<a name="what-are-pre-requisites-for-publishing-a-saas-app"></a>Wat zijn de vereisten voor het publiceren van een SaaS-App?
-------------------------------------------------

De [Cloud Partner-Portal](https://cloudpartner.azure.com) biedt toegang tot de portal zodat meerdere personen samen te werken voor het publiceren van een aanbieding op basis van rollen. Zie voor meer informatie, [gebruikers beheren](./cloud-partner-portal-manage-users.md). 

Voordat een aanbieding kan worden gepubliceerd namens een publisher-account, een van de personen met *eigenaar* rol moet instemmen met de [gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/website-terms-of-use/), [Microsoft PrivacyStatement](https://www.microsoft.com/privacystatement/default.aspx), en [Microsoft Azure Certified-ProgramAgreement](https://azure.microsoft.com/support/legal/marketplace/certified-program-agreement/).


<a name="creating-an-offer"></a>Het maken van een aanbieding
-----------------

In deze sectie beschrijft het proces van het maken van een nieuwe SaaS-App-aanbieding.

![SaaS biedt een overzicht](media/cpp-creating-saas-offers/saas-offer-overview.png)

De aanbieding voor SaaS-App bestaat uit vijf secties wordt beschreven in de volgende tabel:

| **Sectie van de aanbieding**  | **Beschrijving**                                                                                                     |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Aanbiedingsinstellingen     | Helpt bij het definiëren van een unieke naam en ID voor de SaaS-App                                                                         |
| Technische informatie     | Helpt bij het configureren van het type SaaS-oplossing, en Geef details van de verbinding voor uw toepassing                            |
| Test Drive         | Optionele sectie waarmee u voor het definiëren van een service waarmee klanten kunnen voor het testen van uw aanbieding voordat ze kopen. |
| StoreFront Details | Bevat de marketing, juridische, lead management en aanbieding secties:   <br/> -De Marketing-sectie kunt u de beschrijving en logo's vereist zijn voor de aanbieding moet juist worden weergegeven in de marketplace-gebruikersportal in te voeren.  <br/> -De Lead Management kunt u voor het definiëren van een centrale locatie waar de eindgebruiker leads die worden gegenereerd in de gebruikersportal van Azure Marketplace-End.  <br/> -De juridische sectie kunt u privacybeleid en de gebruiksvoorwaarden juridische documentatie invoeren.  |
| Contactpersoon            | Hiermee kunt u contactgegevens voor ondersteuning van de aanbieding opgeven.                                                                  |
|  |  |


### <a name="creating-a-new-offer"></a>Het maken van een nieuwe aanbieding

Nadat u zich aanmeldt bij de [Cloud Partner-Portal](https://cloudpartner.azure.com), selecteer de **nieuwe aanbieding** item op de menubalk linksboven, waarin een menu met beschikbare aanbiedingen filteren. De volgende afbeelding toont een voorbeeld van deze aanbiedingen:

![Nieuwe aanbieding voor SaaS](media/cpp-creating-saas-offers/saas-new-offer.png)

Selecteer de aanbieding die u hebt goedgekeurd voor in de lijst en een nieuwe aanbieding formulier wordt geopend.

![Formulier voor nieuwe SaaS-aanbieding](media/cpp-creating-saas-offers/saas-new-offer-2.png)

De volgende tabel beschrijft de velden van aanbod:

| **Velden van aanbod** | **Beschrijving**                                                                                            |
|------------------|----------------------------------------------------------------------------------------------------------- |
| Aanbiedings-id         | Een unieke id voor de aanbieding binnen een publisher-profiel. Deze ID zijn in URL's voor product en facturering rapporten zichtbaar. De id kan alleen bestaan uit alfanumerieke tekens in kleine letters en streepjes (-). De ID kan mag niet eindigen met een streepje en een maximum van 50 tekens. Houd er rekening mee dat dit veld is vergrendeld zodra een aanbieding live meteen. Bijvoorbeeld, als een uitgever, Contoso, publiceert een aanbieding met aanbieding-ID voorbeeld-vm, wordt deze weergegeven in Azure marketplace als: [https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview](https://azuremarketplace.microsoft.com/) |
| Uitgevers-ID     | De uitgevers-ID is de unieke id in de Marketplace. Al uw aanbiedingen moeten worden gekoppeld met uw uitgever-ID. De uitgevers-ID kan niet worden gewijzigd wanneer de aanbieding is opgeslagen.                                                                                       |
| Naam             | Dit is de weergavenaam voor uw aanbieding. Dit is de naam die wordt weergegeven in Azure Marketplace en in Azure Portal. De naam mag maximaal 50 tekens bevatten. Richtlijnen hier is een herkenbare naam merk voor uw product op te nemen. Neem hier de naam van uw bedrijf niet, tenzij dat is hoe deze wordt gebracht. Als u deze aanbieding worden marketing op uw eigen website, controleert u of de naam precies hoe deze wordt weergegeven in uw website.             |
|  |  |

Klik op **opslaan** uw voortgang opslaan. De volgende sectie worden beschreven toe te voegen plannen om uw aanbieding.


### <a name="technical-information"></a>Technische informatie

De sectie technische informatie kunt u de volgende informatie op te geven:

![SaaS biedt technische gegevens](media/cpp-creating-saas-offers/saas-new-offer-technical-info.png)

De meest belangrijke beslissing is of u het gaat om een SaaS-lijst of commerce ingeschakeld. Als u een SaaS-aanbieding, moet u kiezen tussen:

-   Gratis - Geef een URL van de SaaS-toepassing waar klanten uw app kunnen krijgen.
-   Gratis proefversie - Geef een URL van de SaaS-toepassing waar klanten de proefversie uitvoeren kunnen voordat ze de aanbieding.
-   Contact met mij - alleen relevant als u een beheersysteem lead is verbonden hebt, deze optie kunnen klanten vragen contact met u worden opgenomen en een potentiële klant wordt gedeeld met u opnemen.

Als u een SaaS-app die op Azure Marketplace en wil inschakelen commerce via Microsoft-transacties, selecteer **verkopen via Azure**.  
Zie voor meer informatie over het verbinden van uw SaaS-app [SaaS - verkopen via Azure](./cloud-partner-portal-saas-offer-subscriptions.md).


### <a name="test-drive"></a>Test Drive

Het maken van een evaluatieversie voor uw klanten is een aanbevolen procedure om te controleren of dat ze met vertrouwen kunnen kopen. Test Drive is van de proefversie opties die beschikbaar is, de meest effectief bij het genereren van hoge kwaliteit potentiële klanten en hogere conversie van deze leads.

Daardoor kunnen klanten met een praktische, zelfgestuurd evaluatieversie van de belangrijke functies en voordelen, gedemonstreerd in een implementatiescenario voor het werkelijke van uw product.

![SaaS-aanbieding Test Drive](media/cpp-creating-saas-offers/saas-new-offer-test-drive.png)


#### <a name="how-a-test-drive-works"></a>De werking van een Test Drive

Een potentiële klant wordt gezocht en detecteert de toepassing op de Marketplace. De klant zich heeft aangemeld en gaat akkoord met de gebruiksvoorwaarden. Op dit moment ontvangt de klant uw vooraf geconfigureerde omgeving om te proberen voor een vast aantal uren, terwijl u een zeer gekwalificeerde lead te ontvangen.

![Test Drive 2 voor SaaS-aanbieding](media/cpp-creating-saas-offers/saas-new-offer-test-drive-2.png)

Ongeacht hoe complex uw toepassing is, kunt uw Microsoft-Test Drive u uw product tot leven brengen voor de klant. Er zijn drie verschillende typen Test Drives beschikbaar, elk op basis van het type van het product, scenario en u zich op marketplace.

-   **Azure Resource Manager** -een Azure ARM-Test Drive is een sjabloon voor de implementatie met alle van de Azure-resources die deel uitmaken van een oplossing wordt gemaakt door de uitgever. Producten die geschikt zijn voor dit type Test Drive zijn die alleen Azure-bronnen.
-   **Logische App** -A Logic App Test Drive is een sjabloon voor de implementatie die is bedoeld om alle complexe architecturen omvatten. Alle Dynamics-toepassingen of aangepaste producten moeten gebruiken voor dit type Test Drive.
-   **Power BI** -een Power BI Test Drive bestaat uit een ingesloten koppeling naar een op maat gemaakte dashboard. Alle producten die wil ter illustratie van dat een interactieve Power BI-visualisatie dit type Test Drive moet gebruiken. U moet uploaden, is de URL van uw ingesloten Power BI.

De belangrijkste publishing stappen voor het toevoegen van een test Drive zijn:

1.  Definieer uw Test Drive-scenario
2.  Bouw en/of wijzigen van de Resource Manager-sjabloon
3.  Stapsgewijze handleiding van uw Test Drive maken
4.  Uw aanbod opnieuw publiceren

Zie voor meer informatie, [Test Drive](./what-is-test-drive.md).


### <a name="storefront-details"></a>StoreFront Details

SaaS-Apps moet de eerste twee secties van een overzicht en een beschrijving te verstrekken over uw app.

![SaaS aanbiedingsdetails storefront](media/cpp-creating-saas-offers/saas-new-offer-storefront-details.png)

De volgende tabel beschrijft de aanbieding\'s Storefront Details:

| **Velden van aanbod**        | **Beschrijving**                                                                                          |
|-------------------------| ---------------------------------------------------------------------------------------------------------|
| Overzicht van aanbieding           | Samenvatting van de toegevoegde waarde van uw aanbieding. Deze wordt weergegeven op de pagina voor zoeken van uw aanbieding. Dit moet een maximum van 100 tekens.   |
| Beschrijving van aanbieding       | De beschrijving die op de detailpagina van uw toepassing verschijnen zal. Maximaal toegestane aantal is 1300 tekens *Opmerking* dit veld accepteert HTML-inhoud met tags, zoals & ltp\>, & lth1\>, & lth2\>, & ltli\>, enz., waarmee u de inhoud nog veel meer netjes te maken. Publiceren van portal-team werkt over het toevoegen van een functie waarmee een om te zien van een Preview-versie van hun storefront details voor iteratief en de inhoud meer netjes: in de tussentijd kunt u elke kunt u een online realtime HTML-hulpprogramma's zoals http://htmledit.squarefree.com om te zien hoe uw beschrijving eruit zouden zien. |
| Bedrijfstakken              | Selecteer de branches die uw aanbieding beste wordt uitgelijnd. Als uw app is gekoppeld aan meerdere branches, kunt u maximaal twee. |
| Aanbevolen categorieën    | Selecteer de categorieën die uw aanbieding beste wordt uitgelijnd. U kunt maximaal drie categorieën selecteren.     |
| Toepassingsversie     | Voer het versienummer van uw toepassing                                                                |
| Trefwoorden (Max. 3) | Voer maximaal drie trefwoorden die klanten gebruiken kunnen om uw toepassing niet vinden in de Marketplace storefront-website. |
|  |  |


### <a name="marketing-artifacts"></a>Marketing artefacten

De marketing artefacten sectie kunt u de Azure Marketplace-activa, zoals logo's, video's, schermopnamen en documenten marketing definiëren:

![SaaS biedt marketing artefacten](media/cpp-creating-saas-offers/saas-new-offer-marketing-artifacts.png)

De volgende tabel beschrijft de Marketing-velden:

| **Velden van aanbod** | **Beschrijving**                                                                                                          |
|------------------| ------------------------------------------------------------------------------------------------------------------------ |
| Logo 's            | Als u een **verkopen via Azure** SaaS-app, moet u alle logo's bieden. Als u slechts een aanbieding, zijn alleen 2 logo's zijn vereist. De logo's uploaden van de [Cloud Partner-Portal](https://cloudpartner.azure.com) moet gebruiken de volgende richtlijnen:     <br/> -Houd het aantal primaire en secundaire kleuren op uw logo laag. Het Azure-ontwerp heeft een eenvoudig kleurenpalet.     <br/> -Vermijd het gebruik van zwart of als de achtergrondkleur van het logo van uw wit. De themakleuren van het Azure-Portal zijn zwart-wit. In plaats daarvan gebruik kleur die uw logo opvallende in de Azure-Portal zou maken. We adviseren eenvoudige primaire kleuren. Als u een transparante achtergrond gebruikt, zorg ervoor dat het logo en de tekst niet zwart-wit of blauwe wordt weergegeven.     <br/> -Gebruik niet een kleurovergang achtergrond op het logo.     <br/> -Voorkomen dat tekst, zelfs uw bedrijf of de naam van het merk, geplaatst op het logo. Het uiterlijk van uw logo moet 'vast' en vermijd kleurovergangen.    <br/> -De logoafbeelding moet niet worden uitgebreid.                   |
| Video's           | Hiermee kunt u koppelingen van video's van uw aanbieding toevoegen. U kunt koppelingen naar YouTube en/of Vimeo-video's worden weergegeven, samen met uw aanbieding aan klanten. U moet ook een miniatuur van de video met een PNG-afbeelding van 1280 x 720 pixels invoeren. U kunt maximaal vier video's per aanbieding hebben. |
| Documenten        | Hiermee kunt u marketing documenten toevoegen aan uw aanbieding. Alle documenten in PDF-indeling moeten worden en u kunt maximaal drie documenten per aanbieding hebben.                                                                                                                                                      |
| Schermopnamen      | Hiermee kunt u toevoegen van schermopnamen van uw aanbieding. Er is een maximum van vijf schermafbeeldingen die per aanbieding kunnen worden toegevoegd. De maximale afbeeldingsgrootte is 1280 x 720 pixels.                                                                                                                                             |
| Nuttige koppelingen     | Hiermee kunt u toevoegen van externe URL's voor uw aanbod, wijs architectuurdiagrammen of andere websites die van een klant zou willen zien.                                                                                                                                                              |
|  |  |

De volgende afbeelding ziet u hoe de informatie wordt weergegeven in een zoekresultaat Marketplace:

![SaaS biedt marketing publisher info](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info.png)

De volgende afbeelding ziet u hoe de aanbieding wordt weergegeven in de Marketplace nadat een klant op de tegel met kleinere aanbieding klikt:

![SaaS biedt marketing publisher info2](media/cpp-creating-saas-offers/saas-new-offer-marketing-publisher-info-2.png)


### <a name="lead-management-and-legal-information"></a>Beheer van potentiële klanten en juridische informatie

De juridische sectie kunt u om in te stellen van de aanbieding juridische documentatie.
Er zijn twee juridische documenten die zijn vereist voor elke aanbieding SaaS-toepassing: privacybeleid en de gebruiksvoorwaarden. Zie voor meer informatie  
[Configureren van leads klant](./cloud-partner-portal-get-customer-leads.md).

![SaaS biedt marketing juridische informatie](media/cpp-creating-saas-offers/saas-new-offer-legal-info.png)

De volgende tabel beschrijft de eigenschappen van de juridische sectie:

| **Velden van aanbod**   | **Beschrijving**                                                                                 |
|--------------------| ----------------------------------------------------------------------------------------------- |
| URL privacybeleid | De URL naar het privacybeleid van uw bedrijf.                                                       |
| Gebruiksvoorwaarden       | De gebruiksvoorwaarden voor uw aanbieding. Typ of kopieer en plak de gebruiksvoorwaarden hier. Basisgebruik HTML is toegestaan, zoals die in dit veld html-inhoud met tags, zoals & ltp wordt\>, & lth1\>, & lth2\>, & ltli\>, enzovoort. *Belangrijke* is het raadzaam dat u bekijken en voorbeeld van de HTML-code die u hebt gemaakt in een browser voordat u de aanbieding verzendt. |
|  |  |


### <a name="contact-information"></a>Contactgegevens

In deze sectie voert u de contactpersonen voor ondersteuning van uw bedrijf die verantwoordelijk zijn voor de ondersteuning voor klanten met behulp van deze aanbieding.
Er zijn drie hoofdgebieden: contact op met de technische contactpersoon voor ondersteuning en ondersteuning voor URL's:

![SaaS biedt marketing contactgegevens](media/cpp-creating-saas-offers/saas-new-offer-contact-info.png)


| **Neem contact op met**         | **Beschrijving**                                                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Technische contactpersoon | Geef de naam, e-mailadres en telefoonnummer van een technische contactpersoon die Microsoft kan contact opnemen met ondersteuning en zakelijke problemen. |
| Contactpersoon ondersteuning     | Geef de naam, e-mailadres, telefoonnummer en ondersteuning voor URL die uw klanten kunnen contact opnemen met wanneer ze ondersteuningsaanvragen hebben.                  |
|  |  |


Als uw aanbieding klaar is en u publiceren bereikt, wordt de aanbieding verloopt via certificering. We testen uw SaaS-app via een handmatige verificatie rond het testen van uw URL als u een aanbieding hebt, of eindpunten als u hebt verkopen via Azure geselecteerd. Tijdens deze handmatige goedkeuring bepalen wij ook de juiste webwinkel die uw app op (AppSource, Azure Marketplace of beide weergegeven moet).

<a name="updating-the-offer"></a>De aanbieding wordt bijgewerkt
------------------

Er zijn verschillende soorten updates die u doen voor uw aanbieding wilt mogelijk nadat deze is gepubliceerd en actief is. Eventuele wijzigingen die u in uw nieuwe versie van uw aanbieding aanbrengt moet worden opgeslagen en opnieuw om deze weer in de Marketplace worden gepubliceerd.

<a name="deleting-an-existing-offer"></a>Een bestaande aanbieding verwijderen...
--------------------------

Mogelijk wilt u uw aanbieding uit Marketplace verwijderen. Aanbieding verwijderen zorgt u ervoor dat nieuwe klanten niet meer kunnen kopen of implementeren van uw aanbieding, maar deze geen invloed op bestaande klanten heeft. Aanbieding beëindigen is het proces van beëindiging van de service en/of licentieovereenkomst tussen u en uw bestaande klanten.

Richtlijnen en beleidsregels met betrekking tot de aanbieding verwijderen en beëindigen worden bepaald door de Microsoft Marketplace-overeenkomst voor uitgevers (Zie sectie 7) en de beleidsregels voor deelname aan (Zie de sectie 6.2). Deze sectie bevat informatie met betrekking tot het verwijderen van andere ondersteunde scenario's en de stappen die u voor hen kunt nemen.

### <a name="delete-the-live-offer"></a>De Live-aanbieding verwijderen

Er zijn diverse aspecten die worden afgehandeld als een aanvraag moeten voor het verwijderen van een live-aanbieding. Volg de stappen hieronder om begeleiding van het ondersteuningsteam om te verwijderen van een live-aanbieding van Azure Marketplace:

1.  Een ondersteuningsticket via deze koppeling genereren
2.  Selecteer in de lijst probleemtype **beheren van aanbiedingen**, en selecteer in de lijst met categorieën **wijzigen van een aanbieding en/of SKU al in de productieomgeving**.
3.  Dien de aanvraag

Het ondersteuningsteam leidt u door de verwijdering van de aanbieding.

> [!NOTE] 
> Verwijderen van een aanbieding, heeft dit geen invloed op de huidige aankopen van deze aanbieding. Deze aankopen van klanten blijven werken als voorheen.
De aanbieding wordt niet zijn echter beschikbaar voor alle nieuwe aankopen nadat de verwijdering voltooid is.

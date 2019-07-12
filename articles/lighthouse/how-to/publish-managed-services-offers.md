---
title: Een beheerde services aanbieding publiceren op Azure Marketplace
description: Informatie over het publiceren van een beheerde service-aanbieding dat uitvoeren van Onboarding van klanten naar Azure Resourcemanagement overgedragen.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: bb2f26a170bbd60eb927bd00f6def7d033fafee9
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809846"
---
# <a name="publish-a-managed-services-offer-to-azure-marketplace"></a>Een beheerde services aanbieding publiceren op Azure Marketplace

In dit artikel leert u hoe u een openbare of particuliere beheerde services aanbieding publiceren [Azure Marketplace](https://azuremarketplace.microsoft.com) met behulp van de [Cloud Partner-Portal](https://cloudpartner.azure.com/), waardoor de klant die aankopen in de aanbieding worden uitgevoerd gedelegeerd resourcebeheer voor Azure. 

> [!NOTE]
> U moet beschikken over een geldige [-account in de Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) maken en publiceren van aanbiedingen. Als u geen account hebt, de [aanmeldingsproces](https://aka.ms/joinmarketplace) leidt u door de stappen voor het maken van een account in Partnercentrum en registratie via de commerciële Marketplace-programma. Uw Microsoft Partner Network (MPN)-ID worden [automatisch gekoppeld](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started) met de aanbiedingen die u voor het volgen van uw invloed op klant-engagements publiceren.
>
> Als u niet dat een aanbieding publiceren op Azure Marketplace wilt, kunt u Onboarding van klanten handmatig met behulp van Azure Resource Manager-sjablonen. Zie voor meer informatie, [onboarding een klant naar Azure Resourcemanagement overgedragen](onboard-customer.md).

Publiceren van een Managed Services-aanbieding is vergelijkbaar met een ander type aanbieding publiceren op Azure Marketplace. Zie voor meer informatie over dit proces, [Azure Marketplace en AppSource Publicatiehandleiding voor](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide) en [Azure beheren en de AppSource-Marketplace-aanbiedingen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers).

> [!IMPORTANT]
> Elk abonnement van een aanbieding van beheerde services bevat een **Manifest Details** sectie, waar u de Azure Active Directory (Azure AD)-entiteiten in uw tenant die toegang hebben tot de gedelegeerde resourcegroepen en/of abonnementen voor definiëren klanten die kopen van het abonnement. Het is belangrijk dat u er rekening mee dat een groep (of gebruiker of service-principal) die u hier opgeeft, dezelfde machtigingen voor elke klant die het abonnement koopt hebben. Om te wijzen aan verschillende groepen om te werken met elke klant, moet u een aparte privé plan dat is exclusief voor elke klant te publiceren.

## <a name="create-your-offer-in-the-cloud-partner-portal"></a>Uw aanbieding maken in de Cloud Partner-Portal

1. Aanmelden bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Selecteer in het navigatiemenu links **nieuwe aanbieding**en selecteer vervolgens **beheerde services**.
3. U ziet een **Editor** sectie voor uw aanbod met vier onderdelen in te vullen: **Aanbiedingsinstellingen**, **plannen**, **Marketplace**, en **ondersteuning**. Lees verder voor instructies over het voltooien van deze secties.

## <a name="enter-offer-settings"></a>Voer de instellingen van aanbieding

In de **Aanbiedingsinstellingen** sectie, geeft u de volgende:

|Veld  |Description  |
|---------|---------|
|**Aanbiedings-ID**     | Een unieke id van uw aanbieding (binnen uw uitgeverprofiel). Deze ID mag alleen kleine letters, alfanumerieke tekens, streepjes en onderstrepingstekens bevatten, met een maximum van 50 tekens. Houd er rekening mee dat de aanbiedings-ID mogelijk zichtbaar voor klanten in plaatsen, zoals in de product-URL's en facturering rapporten. Nadat u de aanbieding publiceert, kunt u deze waarde niet wijzigen.        |
|**Uitgevers-ID**     | De uitgevers-ID dat gekoppeld aan de aanbieding is. Als u meer dan één uitgevers-ID hebt, kunt u de versie die u wilt gebruiken voor deze aanbieding.       |
|**Name**     | De naam (maximaal 50 tekens) die zichtbaar voor klanten van uw aanbieding in Azure Marketplace en in de Azure-portal. Gebruik een herkenbare naam voor de merk die klanten begrijpt, als u deze aanbieding via uw eigen website promoot, moet u exact dezelfde naam hier gebruiken.        |

Wanneer u bent klaar, selecteer **opslaan**. Nu u klaar om te verplaatsen bent naar de **plannen** sectie.

## <a name="create-plans"></a>Maak plannen

Ieder aanbod beschikken over een of meer abonnementen (soms ook wel SKU's genoemd). U kunt meerdere abonnementen voor de ondersteuning van verschillende functiesets tegen verschillende prijzen of voor het aanpassen van een specifiek plan voor een beperkt publiek van specifieke klanten toevoegen. Klanten kunnen de abonnementen die beschikbaar voor deze onder de aanbieding bovenliggende zijn weergeven.

Selecteer in de sectie plannen voor elk abonnement dat u maken wilt, **nieuw Plan**. Voer vervolgens een **serviceplan-ID**. Deze ID mag alleen kleine letters, alfanumerieke tekens, streepjes en onderstrepingstekens bevatten, met een maximum van 50 tekens. De plan-ID is mogelijk zichtbaar voor klanten in plaatsen, zoals in de product-URL's en facturering rapporten. Nadat u de aanbieding publiceert, kunt u deze waarde niet wijzigen.

Voer vervolgens de volgende secties in het **Plan Details** sectie:

|Veld  |Description  |
|---------|---------|
|**Titel**     | Beschrijvende naam voor het abonnement om weer te geven. Maximale lengte van maximaal 50 tekens.        |
|**Samenvatting**     | Beknopte beschrijving van het plan om weer te geven onder de titel. Maximale lengte van 100 tekens.        |
|**Beschrijving**     | Beschrijvende tekst die een meer gedetailleerde uitleg van het plan bevat.         |
|**Factureringsmodel**     | Er zijn 2 factureringsmodellen die hier worden weergegeven, maar u moet kiezen **hun eigen licentie mee** voor beheerde services biedt. Dit betekent dat u uw klanten direct voor kosten met betrekking tot deze aanbieding wordt gefactureerd en Microsoft niet in rekening eventuele kosten aan u.   |
|**Is dit een privé-abonnement?**     | Geeft aan of de SKU privé of openbaar is. De standaardwaarde is **Nee** (openbare). Als u deze selectie laat, zal uw abonnement niet worden beperkt aan specifieke klanten (of aan een bepaald aantal klanten); Nadat u een openbare-abonnement hebt gepubliceerd, kan niet u deze later wijzigen in een particuliere. Als u dit plan alleen op specifieke klanten, selecteer **Ja**. Als u dit doet, moet u bepalen voor welke klanten door te geven van hun abonnement aanleveren. Dit kunnen één door een (voor maximaal 10 abonnementen) of door het uploaden van een CSV-bestand (voor maximaal 20.000 abonnementen) zijn. Zorg dat u hier uw eigen abonnementen bevatten, zodat u kunt testen en valideren van de aanbieding. Zie voor meer informatie, [privé-SKU's en abonnementen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus).  |

Voltooi ten slotte de **Manifest Details** sectie. Hiermee maakt u een manifest met autorisatie-informatie voor het beheren van resources van de klant. De informatie die u opgeeft als volgt overgedragen die nodig zijn voor de onboarding uw klanten voor Azure resourcemanagement. Zoals hierboven vermeld, gelden deze machtigingen geldt voor elke klant die het abonnement aanschaft, zodat als u beperken van toegang tot een specifieke klant wilt, u moet voor het publiceren van een persoonlijke plan voor eigen gebruik.

- Geef eerst een **versie** voor het manifest. Gebruik de indeling *n.n.n* (bijvoorbeeld 1.2.5).
- Voer vervolgens uw **Tenant-ID**. Dit is een GUID die is gekoppeld aan de Azure Active Directory-tenant-ID van uw organisatie (dat wil zeggen, de tenant waarmee u werkt in om uw klanten resources te beheren). Als u geen binnen handbereik hebt, kunt u deze vinden door de muiswijzer op de naam van uw account op de bovenste rechterkant van de Azure-portal of door te selecteren **schakelen tussen mappen**. 
- Voeg een of meer **autorisatie** vermeldingen voor uw abonnement. Autorisaties definiëren de entiteiten die toegang hebben tot resources en abonnementen voor klanten die het abonnement te kopen. U moet deze gegevens opgeven voor toegang tot resources namens de klant met behulp van Azure-gedelegeerde resourcemanagement.
  Voor elke autorisatie, bieden de volgende. Vervolgens kunt u selecteren **nieuwe autorisatie** zo vaak als nodig is om toe te voegen meer gebruikers/roldefinities.
  - **Azure AD-Object-ID**: De Azure AD-id van een gebruiker, groep of toepassing die wordt toegekend bepaalde machtigingen (zoals is beschreven in de definitie van de rol) aan uw klanten resources.
  - **Weergavenaam van de Azure AD-Object**: Een beschrijvende naam voor de klant leert wat het doel van deze machtiging. De klant zien deze naam bij het overdragen van resources.
  - **Role Definition**: Selecteer een van de beschikbare ingebouwde rollen in de lijst met Azure AD. Deze rol, bepaalt de machtigingen die de gebruiker in de **Azure AD-Object-ID** veld, heeft op de resources van uw klanten. Zie voor informatie over deze rollen [ingebouwde rollen](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).
  - **Toewijsbare rollen**: Als u beheerder van gebruikerstoegang in hebt geselecteerd de **roldefinitie** voor deze machtiging, kunt u een of meer toegewezen rollen hier toevoegen. De gebruiker in de **Azure AD-Object-ID** veld is mogelijk om toe te wijzen deze **toewijsbare rollen** naar [beheerde identiteiten](https://docs.microsoft.com/azure/managed-applications/publish-managed-identity). Houd er rekening mee dat er geen andere machtigingen die doorgaans gepaard gaan met de rol Administrator voor gebruikerstoegang op deze gebruiker toepast. (Als u een van deze gebruiker roldefinitie niet Administrator voor gebruikerstoegang hebt geselecteerd, in dit veld heeft geen effect.)

> [!TIP]
> In de meeste gevallen moet u machtigingen toewijzen aan een Azure AD-gebruikersgroep of de service principal, in plaats van een serie van afzonderlijke gebruikersaccounts. Hiermee kunt u toegang toevoegen of verwijderen voor individuele gebruikers zonder te hoeven bijwerken en opnieuw publiceren van het abonnement als uw toegangsvereisten worden gewijzigd.

Wanneer u klaar bent met het plannen voor het toevoegen, selecteert u **opslaan**, gaat u verder met de **Marketplace** sectie.

## <a name="provide-marketplace-text-and-images"></a>Bieden van Marketplace-tekst en afbeeldingen

De **Marketplace** sectie is waarin u de tekst en afbeeldingen die klanten wordt weergegeven in Azure Marketplace en de Azure-portal opgeven.

Geef gegevens voor de volgende velden in de **overzicht** sectie:

|Veld  |Description  |
|---------|---------|
|**Titel**     |  De titel van de aanbieding, vaak de lang, formele naam. Deze titel wordt prominent weergegeven in de marketplace. Maximale lengte van maximaal 50 tekens. In de meeste gevallen dit moet hetzelfde zijn als de **naam** u hebt opgegeven de **instellingen bieden** sectie.       |
|**Samenvatting**     | Korte doel of de functie van uw aanbieding. Dit wordt meestal weergegeven onder de titel. Maximale lengte van 100 tekens.        |
|**Lange samenvatting**     | Een langer samenvatting van het doel of de functie van uw aanbieding. Mag maximaal 256 tekens bevatten.        |
|**Beschrijving**     | Meer informatie over uw aanbieding. Dit veld heeft een maximale lengte van 3000 tekens en biedt ondersteuning voor eenvoudige HTML-opmaak.        |
|**Marketing-id**     | Een unieke beschrijvende URL-id. Deze wordt gebruikt in Marketplace-URL's voor deze aanbieding. Bijvoorbeeld, als uw uitgevers-ID is *contoso* en uw marketing-id is *sampleApp*, de URL voor uw aanbieding in Azure Marketplace *https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp* .        |
|**Preview-versie abonnement-id 's**     | 1 tot 100 abonnements-id's toevoegen. De klanten die zijn gekoppeld aan deze abonnementen is mogelijk om weer te geven van de aanbieding in Azure Marketplace, voordat u deze meteen live. Het is raadzaam om uw eigen abonnementen hier met inbegrip van, zodat u een voorbeeld van hoe uw aanbieding wordt weergegeven in de Azure Marketplace vooraleer het beschikbaar voor klanten.  (Ondersteuning van Microsoft en de engineering-teams is ook mogelijk om uw aanbieding in deze previewperiode weer te geven.)   |
|**Nuttige koppelingen**     | URL's met betrekking tot uw aanbieding, zoals documentatie, opmerkingen bij de release, veelgestelde vragen, enzovoort.        |
|**Aanbevolen categorieën (maximaal 5)**     | Een of meer categorieën (maximaal vijf) die van toepassing op uw aanbieding zijn. Deze categorieën help klanten uw aanbieding in Azure Marketplace en de Azure-portal detecteren.        |

In de **Marketing artefacten** sectie, u kunt uploaden logo's en andere elementen moet worden weergegeven in uw aanbieding. U kunt eventueel schermafbeeldingen uploaden of koppelingen naar video's waarmee klanten inzicht in uw aanbieding.

Vier logo-grootten zijn vereist: **Klein (40, 40)** , **Gemiddeld (90 x 90)** , **grote (115 x 115)** , en **Wide (255 x 155)** . Volg deze richtlijnen voor uw logo's:

- Het Azure-ontwerp heeft een eenvoudig kleurenpalet. Beperk het aantal primaire en secundaire kleuren in uw logo.
- De themakleuren van de portal zijn wit en zwart. Gebruik deze kleuren niet als de achtergrondkleur voor uw logo. Gebruik een kleur waardoor uw logo opvalt in de portal. We adviseren eenvoudige primaire kleuren.
- Als u een transparante achtergrond gebruikt, zorg ervoor dat het logo en tekst worden niet wit, zwart of blauw.
- Het uiterlijk van uw logo moet plat zijn, zonder kleurovergangen. Gebruik geen achtergrond met een kleurovergang in het logo.
- Plaats geen tekst in het logo, ook niet de naam van uw bedrijf of merk.
- Zorg ervoor dat het logo niet is uitgerekt.

De **Hero (815 x 290)** logo is optioneel maar aanbevolen. Als u een hero-logo, volgt u deze richtlijnen:

- Geen tekst bevatten in de hero-logo en laat 415 pixels van de lege ruimte aan de rechterkant van het logo. Dit is vereist voor laat ruimte voor tekstelementen die via een programma wordt ingesloten: uw weergavenaam uitgever, plan de titel, lange samenvatting bieden.
- Het logo van uw hero mogelijk niet de zwarte en witte en transparant. Zorg ervoor dat uw achtergrondkleur is te licht is, niet omdat de ingesloten tekst wordt weergegeven in wit.
- Nadat u uw aanbieding met een hero-pictogram publiceren, verwijderen niet (maar u deze met een andere versie bijwerken kunt indien gewenst).

In de **leiden Management** sectie, kunt u de CRM-systeem waar de leads worden opgeslagen, indien gewenst. 

Ten slotte, Geef uw **URL privacybeleid** en **gebruiksvoorwaarden** in de **juridische** sectie. U kunt hier ook opgeven of niet gebruik van de [Standard Contract](https://docs.microsoft.com/azure/marketplace/standard-contract) voor deze aanbieding.

Zorg ervoor dat u uw wijzigingen opslaan voordat u aan bij de **ondersteuning** sectie.

## <a name="add-support-info"></a>Ondersteuningsinformatie toevoegen

In de **ondersteunen** sectie, de naam, e-mailadres en telefoonnummer opgeven voor een technische contactpersoon en een klant contact opnemen met ondersteuning. U moet ook bieden ondersteuning voor URL's. Microsoft kan deze gegevens worden gebruikt als we moeten contact met u opnemen over bedrijf en ondersteuning bij problemen.

Als u deze gegevens hebt toegevoegd, selecteert u **opslaan.**

## <a name="publish-your-offer"></a>Uw aanbieding publiceren

Bent u tevreden is met alle van de gegevens die u hebt opgegeven, wordt uw volgende stap is het publiceren van de aanbieding op Azure Marketplace. Selecteer de **publiceren** knop om te zorgen dat uw aanbieding initiëren live. Zie voor meer informatie over dit proces [aanbiedingen publiceren op Azure Marketplace en AppSource](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-publish-offer).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [ervaringen voor cross-tenant](../concepts/cross-tenant-management-experience.md).
- [Weergeven en beheren van klanten](view-manage-customers.md) door te gaan naar **mijn klanten** in Azure portal.

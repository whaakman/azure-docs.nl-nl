---
title: Een nieuwe SaaS-aanbieding maken in de commerciële Marketplace
description: Het maken van een nieuw Software als een Service (SaaS)-aanbieding voor de aanbieding of te verkopen in de Azure Marketplace, AppSource, of via het programma Cloud Solution Provider (CSP) is met behulp van de commerciële Marketplace-portal voor Microsoft Partner Center.
author: mattwojo
manager: evansma
ms.author: mattwoj
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: f2787cd74525e7676befb133a6106ce83d9c2a20
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072637"
---
# <a name="create-a-new-saas-offer"></a>Een nieuwe SaaS-aanbieding maken

Om te beginnen met het maken van Software, zoals een Service (SaaS) biedt, zorg ervoor dat u eerste [maken van een Partner Center-account](./create-account.md) en open de [commerciële Marketplace-dashboard](https://partner.microsoft.com/dashboard/commercial-marketplace/offers), met de **overzicht** tabblad is geselecteerd.

![Commerciële Marketplace-dashboard op Partnercentrum](./media/new-offer-overview.png)

Selecteer de + **maakt u een nieuw...** knop en selecteer vervolgens de **Software als een Service** menu-item. 

Als u een van de andere aanbiedingstypen selecteert, wordt u omgeleid naar de oudere [Cloud Partner-Portal](https://cloudpartner.azure.com/).  Alleen SaaS-aanbiedingen zijn beschikbaar in de portal commerciële Marketplace op Partner Center op dit moment. 

![Venster van de aanbieding op Partner Center maken](./media/new-offer-click.png)

De **nieuwe aanbieding** in het dialoogvenster wordt weergegeven. 

![Dialoogvenster nieuwe aanbieding](./media/new-offer-popup.png)


## <a name="offer-id-and-name"></a>Aanbiedings-ID en naam

- **Offer ID**: Maak een unieke id voor elke aanbieding in uw account. Deze ID is zichtbaar voor klanten in het URL-adres voor de marketplace-aanbieding en Azure Resource Manager-sjablonen (indien van toepassing). Aanbiedings-ID moet in kleine letters, alfanumerieke tekens (inclusief afbreekstreepjes en onderstrepingstekens bevatten, maar er zijn geen witruimte). Dit is beperkt tot 50 tekens bevatten en kan niet worden bijgewerkt nadat u hebt geselecteerd maken.  
Voorbeeld: test-aanbieding-1
<br>Dit resulteert in de URL: `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`

- **Naam van aanbieding**: De officiële naam van uw aanbieding SaaS-toepassing, consistent zijn tussen publicaties, advertenties en websites.  Deze naam kan worden handelsmerk.  De naam mag geen spatie, emoji's bevatten (tenzij ze het symbool voor handelsmerk of copyright zijn) bieden en moet worden beperkt tot 50 tekens.
<br>Voorbeeld: 1-aanbieding voor testen&#8482;

Selecteer **Maken**.  Een **overzicht bieden** -pagina wordt gemaakt voor deze aanbieding.  

![Overzicht van de Partner Center bieden](./media/commercial-marketplace-offer-overview.png)

## <a name="offer-overview"></a>Overzicht van de aanbieding

De **overzicht bieden** pagina bevat: 

- De **publicatiestatus** geeft een visuele weergave van de stappen die nodig zijn voor het publiceren van deze aanbieding en hoelang elke stap duurt om te voltooien. Onvolledige publishing pictogrammen in de stap wordt grijs weergegeven. 

- De **overzicht bieden** menu bevat een lijst met koppelingen voor het uitvoeren van bewerkingen op deze aanbieding. Deze lijst met bewerkingen veranderen op basis van de selectie die u voor uw aanbieding.  
    - Als de aanbieding een concept-concept verwijderen is 
    - Als de aanbieding is live: Stop aanbieding verkopen 
    - Als de aanbieding beschikbaar als preview-Go live is 
    - Als u dit nog niet hebt voltooid publisher Meld u af: annuleren publiceren

## <a name="offer-setup"></a>Installatie van de aanbieding

De **bieden setup** tabblad om de volgende informatie wordt gevraagd. Selecteer **opslaan** na het voltooien van deze velden.

- **Wilt u verkopen via Microsoft?** (Ja/Nee)
    - **Ja**, u wilt verkopen van uw aanbieding met behulp van Microsoft, met Microsoft die als host fungeert marketplace transacties namens; of 
    - **Geen**, u liever om uw aanbieding via de marktplaatsen, net weer te geven voor het verwerken van eventuele financiële transacties onafhankelijk van Microsoft.    

### <a name="sell-through-microsoft"></a>Verkopen via Microsoft

Verkopen via Microsoft voorziet in betere klant detectie en overname, kan Microsoft host marketplace transacties uit uw naam en maakt gebruik van de Microsoft wereldwijd beschikbaar commerce-functionaliteit.

#### <a name="saas-offer-requirements"></a>Vereisten voor SaaS-aanbieding

Om de lijst met Software als een Service (SaaS) met commerciële Marketplace op Partner Center biedt, wordt de volgende criteria voldaan:

- Uw aanbieding moet gebruiken [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) voor de verificatie en identiteitsbeheer.
- Uw aanbieding moet gebruiken [SaaS-API's voor vervulling](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) om te integreren met de Azure Marketplace.
- Zie voor meer uitgebreide vereisten, de [Publicatiehandleiding voor SaaS bieden](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide).

#### <a name="saas-on-azure-billing-infrastructure-costs"></a>SaaS in Azure Billing infrastructuurkosten
Als de SaaS-aanbieding wordt gehost in Azure, u, als de uitgever moet rekening houden voor de infrastructuur van Azure-gebruikskosten en softwarelicentiekosten als een item eenmalige kosten. Deze kosten wordt weergegeven als een vaste maandelijkse kosten voor de klant. Gebruik Azure-infrastructuur wordt beheerd en kosten in rekening gebracht, kunt u de partner direct. Werkelijke infrastructuur gebruikskosten zijn niet zichtbaar voor de klant. Uitgevers is doorgaans ervoor kiezen te bundelen gebruikskosten voor Azure-infrastructuur in de software-licentie prijzen. 

Software licentiekosten als een maandelijkse, terugkerende site-gebaseerde vast tarief abonnementskosten worden weergegeven en wordt geen datalimiet of op basis van verbruik.

|**Uw licentie kosten**|**$100 per maand**|
|:---|:---|
|Gebruik van Azure-kosten (D1/1-Core)|Rechtstreeks op de publisher, niet de klant in rekening gebracht|
|De klant wordt gefactureerd op basis van Microsoft|$100,00 per maand (Publisher moet rekening voor de kosten van een gemaakte of Pass Through-infrastructuur in het licentietarief)|

- In dit scenario Microsoft $100,00 voor uw softwarelicentie kosten in rekening gebracht en betaalt van $80,00 met de publisher.
- Partners die zijn voor de **servicevergoeding voor Marketplace verminderd** ziet u een lagere transactiekosten in rekening op de SaaS-aanbiedingen van mei 2019 tot en met juni 2020. In dit scenario Microsoft $100,00 voor uw softwarelicentie kosten in rekening gebracht en betaalt van $90,00 met de publisher.

> [!NOTE]
> **Servicevergoeding voor Marketplace verminderd**: SaaS biedt voor bepaalde dat u hebt gepubliceerd op onze commerciële Marketplace, Microsoft beperkt de servicevergoeding voor Marketplace van 20% (zoals beschreven in de Microsoft-overeenkomst voor uitgevers) tot 10%. In de volgorde van uw aanbieding in aanmerking komt, moet ten minste één van uw aanbiedingen zijn aangewezen door Microsoft als het IP-CO-sell ready of IP-gezamenlijke verkoop prioriteit.  In aanmerking te komen moet ten minste vijf (5) werkdagen vóór het einde van elke kalendermaand worden voldaan om te kunnen ontvangen van deze minder servicevergoeding voor Marketplace gedurende de maand.  De kosten verlaagd Marketplace Service geldt niet voor virtuele machines, beheerde Apps of andere producten beschikbaar gesteld via onze commerciële Marketplace.  De kosten verlaagd Marketplace-Service kan alleen worden gekwalificeerde aanbiedingen voor die worden verzameld door Microsoft tussen 1 mei 2019 en 30 juni 2020-licentiekosten.  Na deze tijd wordt de servicevergoeding voor Marketplace terug naar de normale hoeveelheid. 

|**Microsoft facturen**|**$100 per maand**|
|:---|:---|
|Microsoft betaalt u 80% van de kosten van uw licentie <br>**Voor gekwalificeerde SaaS-apps betaalt Microsoft 90% van de kosten van uw licentie*|$80,00 per maand <br>*$* 90,00 per maand *|


#### <a name="csp-program-opt-in"></a>CSP-programma Opt-in
De [Cloud Solution Provider (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) kunt u software-aanbiedingen voor miljoenen gekwalificeerde klanten van Microsoft met minimale investeringen marketing en verkoop.

- **Kanalen: Mijn aanbieding beschikbaar maken in het CSP-programma** (selectievakje)

Selecteren van uw aanbieding beschikbaar maken in het CSP-programma, kunt Cloud Solution Providers te verkopen van uw product als onderdeel van een oplossing voor gebundelde aan hun klanten. 

### <a name="list-through-microsoft"></a>Lijst met behulp van Microsoft

Promoot uw bedrijf met Microsoft door het maken van een marketplace-aanbieding. Betekent dit niet handelen met behulp van Microsoft en weergeven van uw aanbieding alleen selecteren dat Microsoft niet aan rechtstreeks transacties voor software-licentie deelnemen. Er zijn geen transactiekosten verbonden en de 100% van de software licensing kosten die worden verzameld van de klant worden bewaard. De uitgever is echter verantwoordelijk voor de ondersteuning van alle aspecten van de software-licentie transactie, inclusief maar niet beperkt tot: order uitvoering, softwaremeter, facturering, facturering, betaling en verzameling. 

- **Hoe wilt u potentiële klanten om te communiceren met deze aanbieding aanbieding?**

##### <a name="get-it-now-free"></a>Deze nu (gratis)
Lijst met uw aanbieding aan klanten gratis door te geven van een geldige URL (die begint met http of https), waar ze krijgen uw app tot toegang.  Bijvoorbeeld: `https://contoso.com/saas-app`

##### <a name="free-trial"></a>Gratis proefversie
Geef uw aanbieding aan klanten op basis van een gratis proefversie door te geven van een geldige URL (die begint met http of https), waar ze krijgen uw app tot toegang.  Bijvoorbeeld: `https://contoso.com/trial/saas-app`

##### <a name="contact-me"></a>Contact opnemen
Neem contact op met klantgegevens verzamelen door verbinding te maken van uw systeem Customer Relationship Management (CRM). De klant wordt gevraagd om toestemming om hun gegevens te delen. Deze details van de klant, samen met de aanbiedingsnaam van de, -ID en marketplace bron waar ze gevonden dat uw aanbieding, verzonden naar de CRM-systeem die u hebt geconfigureerd. Zie voor meer informatie over het configureren van uw CRM [Connect lead management](#connect-lead-management). 

## <a name="example-marketplace-offer-listing"></a>Voorbeeld van de marketplace bieden aanbieding

![Voorbeeld van de lijst met opmerkingen bij de marketplace-aanbieding](./media/marketplace-offer.svg)

## <a name="enable-a-test-drive"></a>Een test uit te schakelen

Een test drive is een uitstekende manier om uw aanbieding aan potentiële klanten door zodat ze de optie ' proberen voordat u aanschaft ', wat resulteert in betere conversie en de generatie van maximaal verkoopleads presenteren. [Meer informatie over proeven.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

- **Een test uit te schakelen** (selectievakje) 

Test drive inschakelt, wordt u gevraagd een demo-omgeving voor klanten om te proberen uw aanbieding voor een vaste periode configureren. 

### <a name="type-of-test-drive"></a>Type test drive

- **[Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)** : Een implementatiesjabloon met alle Azure-resources die deel uitmaken van uw oplossing. Producten die geschikt zijn voor dit scenario gebruikt u alleen Azure-resources.
- **[Dynamics 365 for Business Central](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)** : Microsoft host en onderhoudt de test drive-service (met inbegrip van de inrichting en implementatie) voor een centrale zakelijke enterprise resource planning systeem (financiën, bewerkingen, Toelevering, CRM-, enz.).  
- **[Dynamics 365 voor Customer Engagement](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)** : Microsoft host en onderhoudt de test drive-service (met inbegrip van de inrichting en implementatie) voor een Customer Engagement-systeem (verkoop, service, project-service, buitendienst, enz.).  
- **[Dynamics 365 for Operations](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)** : Microsoft host en onderhoudt de test drive-service (met inbegrip van de inrichting en implementatie) voor een Finance and Operations enterprise resource planning systeem (financiën, bewerkingen, productie, Toelevering, enz.). 
- **[Logische app](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)** : Een sjabloon voor de implementatie dat alle complexe architecturen. Alle aangepaste producten moeten dit type Test Drive gebruiken.
- **[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)** : Een ingesloten koppeling naar een op maat gemaakte dashboard. Producten die u wilt tonen dat een interactieve Power BI-visualisatie dit type Test Drive moet gebruiken. U moet hier uploaden, is de URL van uw ingesloten Power BI.

#### <a name="additional-test-drive-resources"></a>Aanvullende test drive-resources
- [Test Drive technische Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Probeer de aanbevolen procedures voor Marketing](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Station overzicht één Pager testen](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Verbinding maken met beheer van leads

Verbinding maken met klanten rechtstreeks door uw aanbieding in de marktplaatsen in de lijst en ten slotte verbindt uw systeem Customer Relationship Management (CRM), zodat u klantgegevens ontvangt kunt onmiddellijk nadat een klant uitdrukking van belang of implementeert uw product.

- **Kies een doel lead** (vervolgkeuzelijst): Geef de gegevens van de verbinding met de CRM-systeem waar u graag leads klant verzenden. 

Partner Center ondersteunt de volgende CRM-systemen voor het beheer van leads. Selecteer de koppeling voor installatie-instructies.

- Azure Blob-bieden e-mailadres contactpersoon, containernaam en verbindingsreeks voor opslag. 
- [Azure-tabel](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-azure-table) – e-mailadres contactpersoon en storage-account-verbindingsreeks opgeven. 
- [Dynamics CRM Online](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-dynamics) – e-mailadres contactpersoon, URL en verificatiemodus (Office 365 of Azure Active Directory).
- [HTTPS-eindpunt](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-https) – e-mailadres contactpersoon- en HTTPS-eindpunt-URL opgeven. 
- [Marketo](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-marketo) – e-mailadres contactpersoon, formulier-ID, Munchkin account-ID en-id op.
- [SalesForce](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-lead-management-instructions-salesforce) -e-mailadres contactpersoon en organisatie-ID. 

#### <a name="additional-lead-management-resources"></a>Aanvullende lead management-resources
- [Veelgestelde vragen over management leiden](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Veelvoorkomende configuratiefouten van de potentiële klant](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Beheer overzicht één Pager leiden](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf)

Houd er rekening mee te **opslaan** voordat u doorgaat met de volgende sectie.

## <a name="properties"></a>Properties
De **eigenschappen** tabblad vraagt u de categorieën en branches gebruikt voor het groeperen van uw aanbieding op de marktplaatsen, de ondersteuning van uw aanbieding en de appversie van uw contracten definiëren. 

Selecteer **opslaan** na het voltooien van deze velden. 

### <a name="category"></a>Category
Selecteer een minimum van één (1) en maximaal drie (3) de categorieën die is gebruikt voor het groeperen van uw aanbieding in de juiste marketplace zoekgebieden. Aanroepen van hoe uw aanbieding biedt ondersteuning voor deze categorieën in de beschrijving van de aanbieding. 

### <a name="industry"></a>Industrie
Selecteer maximaal twee (2) de branches die is gebruikt voor het groeperen van uw aanbieding in de juiste marketplace zoekgebieden. Als uw aanbieding niet specifiek zijn voor een industrie is, selecteert u niet een. Aanroepen van hoe uw aanbieding biedt ondersteuning voor de geselecteerde branches in de beschrijving van de aanbieding. 

### <a name="app-version"></a>App-versie
Dit is een optioneel veld in de AppSource-marketplace gebruikt om u te identificeren van het versienummer van uw aanbieding. 

### <a name="standard-contract"></a>Standaardcontract

- **Standard Contract gebruiken?**

Voor het vereenvoudigen van het aankoopproces voor klanten en juridische vanwege de complexiteit van softwareleveranciers, biedt Microsoft een Standard Contract-sjabloon om een transactie in de marketplace te vergemakkelijken. 

In plaats van het samenstellen van aangepaste voorwaarden en bepalingen, kunt Azure Marketplace-uitgevers bieden van hun software onder het standaardcontract, klanten alleen hoeven om te screenen en één keer te accepteren. 

Het standaardcontract vindt u hier: https://go.microsoft.com/fwlink/?linkid=2041178.

#### <a name="terms-of-use"></a>Gebruiksvoorwaarden

Als uw licentievoorwaarden verschillen van het standaardcontract, kunt u aangeven of u uw eigen juridische voorwaarden van het gebruik hier invoeren. U kunt ook maximaal 10.000 tekens van de tekst in dit veld invoeren. Als uw gebruiksvoorwaarden een langere beschrijving vereist, voert u een enkele URL-koppeling in dit veld waar de aanvullende licentievoorwaarden kunnen worden gevonden. Aan klanten wordt deze weergegeven als een actieve koppeling.

Klanten moeten deze voorwaarden accepteren voordat ze uw app kunnen uitproberen. 

Houd er rekening mee te **opslaan** voordat u doorgaat met de volgende sectie.

## <a name="offer-listing"></a>Bieden van aanbieding

De vermelding voor de aanbieding tabblad geeft de talen (en markten) waar uw aanbieding beschikbaar is, is Engels (Verenigde Staten) momenteel de enige locatie beschikbaar is. Deze pagina bevat ook de status van de aanbieding taalspecifieke en de datum/tijd waarop deze is toegevoegd. U moet de gegevens van marketplace (bieden naam, beschrijving, zoektermen, enz.) definiëren voor elke taal / markt.

### <a name="offer-listings"></a>Vermeldingen voor aanbiedingen

Vindt u informatie moet worden weergegeven in de marketplace, met inbegrip van de beschrijvingen van uw aanbieding en marketing activa.

- **Naam** (vereist): De naam die hier zijn gedefinieerd wordt weergegeven als de titel van uw aanbieding op de marketplace(s) die u hebt gekozen. De naam is vooraf ingevuld op basis van uw vorige **nieuwe aanbieding** vermelding.  Dit kan worden handelsmerk.  Dit mag geen spatie, emoji's bevatten (tenzij ze het merk en copyrightgegevens symbolen zijn) en moet worden beperkt tot 50 tekens.
- **Samenvatting** (vereist): Geef een korte beschrijving van uw aanbieding moet worden gebruikt in de zoekresultaten van marketplace aanbieding(en). In dit veld kunnen maximaal 100 tekens van de tekst worden ingevoerd.
- **Beschrijving** (vereist): Geef een beschrijving van uw aanbieding moet worden weergegeven in het overzicht van de aanbieding(en) marketplace. U kunt overwegen om een toegevoegde waarde, de belangrijkste voordelen, categorie of bepaalde industrie voldoen koppelingen, aankopen binnen Apps verkoopkansen, de vereiste informatie en een koppeling voor meer informatie.
In dit veld kunnen maximaal 3000 tekens van de tekst worden ingevoerd. Zie voor meer tips voor het artikel [Schrijf een geweldige app-beschrijving](https://docs.microsoft.com/windows/uwp/publish/write-a-great-app-description).
- **Trefwoorden zoeken**: Voer maximaal drie trefwoorden die klanten gebruiken kunnen om uw aanbieding niet vinden in de marketplace(s).
- **Aan de slag instructies** (vereist): Wordt uitgelegd hoe u kunt configureren en gebruiken van uw app voor potentiële klanten.  In deze Quick Start kunt bevatten koppelingen naar meer gedetailleerde online documentatie. In dit veld kunnen maximaal 3000 tekens van de tekst worden ingevoerd. 

#### <a name="links"></a>Koppelingen

- **Privacybeleid** (vereist): Koppeling naar het privacybeleid van uw organisatie. U bent verantwoordelijk voor het ervoor te zorgen dat uw app voldoet aan de privacywetten en regelgevingen en voor het ontwikkelen van een geldige privacybeleid
- **CSP-programma marketingmaterialen** (optioneel): Moet u een koppeling naar het marketingmateriaal als u ervoor kiest om uit te breiden van uw aanbieding aan de [Cloud Solution Provider (CSP)](https://docs.microsoft.com/azure/marketplace/cloud-solution-providers) programma. CSP breidt uw aanbieding aan een breder scala aan gekwalificeerde klanten door in te schakelen van CSP-partners te bundelen, handel en wederverkoop van uw aanbieding. Deze resellers nodig toegang tot materiaal voor de marketing van uw aanbieding. Zie voor meer informatie, [Go-To-Market Services](https://partner.microsoft.com/reach-customers/gtm).
- **Nuttige koppelingen** (optioneel): Optioneel aanvullende online documenten over uw app of verwante services die worden vermeld door te geven een **titel** en **URL**. Als u meer nuttige koppelingen toevoegen door te klikken op **+ toevoegen van een URL**.

#### <a name="contact-information"></a>Contactgegevens

- **Contactpersonen**: Voor elke contactpersoon van de klant, geeft u een werknemer **naam** , **telefoonnummer**, en **e** adres.  (Deze *niet* openbaar worden weergegeven). Een **URL ondersteuning** is ook vereist voor de **Ondersteuningscontactpersoon** groep.  (Deze informatie *wordt* openbaar worden weergegeven).

**Neem contact op met ondersteuning** (vereist): Voor algemene ondersteuningsvragen.

**Neem contact op met technische** (vereist): Voor technische vragen.

**Kanaal Manager contact** (vereist): Voor reseller vragen met betrekking tot het CSP-programma.

#### <a name="files-and-images"></a>Bestanden en installatiekopieën

- **Documenten** (vereist): Gerelateerde marketing documenten voor uw aanbieding in PDF-indeling, met een minimum van één (1) en maximaal drie (3) de documenten per aanbieding toevoegen.
- **Afbeeldingen** (optioneel): Er zijn meerdere plaatsen waar van uw aanbieding logo's kunnen worden weergegeven in de marketplace(s), vereisen de volgende grootten: klein: 48 x 48 pixels _(vereist),_ gemiddeld: 90 x 90 pixels groot: 216 x 216 pixels _(vereist),_ breed: 255 x 115 pixels en Hero: 815 x 290 pixels. Alle installatiekopieën moeten zijn. PNG-indeling.
- **Schermafbeeldingen** (vereist): Voeg schermafbeeldingen aan uw aanbieding te tonen. Maximaal vijf (5) schermafbeeldingen kan worden toegevoegd en moet worden aangepast op 1280 x 720 pixels. Alle installatiekopieën moeten zijn. PNG-indeling.
- **Video's** (optioneel): Koppelingen toevoegen naar video's aan uw aanbieding te tonen. U kunt koppelingen naar YouTube en/of Vimeo-video's worden weergegeven, samen met uw aanbieding aan klanten. U moet ook een miniatuur van de video, voer formaat op 1280 x 720 pixels in PNG-indeling. U kunt maximaal vier video's per aanbieding weergeven.

Houd er rekening mee te **opslaan** voordat u doorgaat met de volgende sectie.

#### <a name="additional-marketplace-listing-resources"></a>Resources voor aanvullende marketplace-aanbieding

- [Aanbevolen procedures voor marketplace bieden aanbiedingen](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)


## <a name="preview"></a>Preview

De **Preview** tabblad kunt u voor het definiëren van een beperkte **Preview doelgroep** voor het vrijgeven van uw aanbod publiceren van uw aanbieding live naar de breder publiek van marketplace.

> [!IMPORTANT]
> U moet selecteren **live gaan** voordat uw aanbieding wordt gepubliceerd live naar de openbare marketplace-doelgroep na het controleren van uw aanbieding in Preview.

- **Definieer een publiek Preview-versie: Een enkele AAD/MSA-account e-mail per regel, samen met een optionele beschrijving toevoegen.**

Handmatig toevoegen van maximaal tien (10) e-mailadressen of live twintig (20) als een CSV-bestand uploaden voor bestaande Microsoft Account (MSA) of Azure Active Directory (AAD)-accounts om te helpen bij het valideren van uw aanbieding voordat u publiceert. U kunt een doelgroep die preview-toegang tot uw aanbieding kunnen worden voordat deze wordt gepubliceerd naar de marketplace(s) wilt definiëren door deze accounts toe te voegen. Als uw aanbieding al gepubliceerd is, kunt u een preview-doelgroep kunt nog steeds definiëren voor het testen van wijzigingen of updates voor uw aanbieding.

> [!NOTE]
> De preview-doelgroep verschilt van een particuliere doelgroep. Een preview-publiek toegang heeft tot uw aanbieding _voorafgaande_ live wordt gepubliceerd in de marktplaatsen. U kunt ook een plan maken en deze beschikbaar te maken voor alleen een particuliere doelgroep. In de **aanbieding van plan bent** tabblad kunt u een particuliere doelgroep met definiëren de **dit is een persoonlijke plan** selectievakje. U kunt vervolgens een particuliere doelgroep van maximaal 20.000 klanten met behulp van Azure-Tenant-id's definiëren.

## <a name="technical-configuration"></a>Technische configuratie

De **technische configuratie** tabblad definieert de technische details (URL-pad, webhook, tenant-ID en app-ID) gebruikt voor verbinding met uw aanbieding. Deze verbinding kan we uw aanbieding inrichten als een resource in Azure-abonnement van de klant als ze ervoor kiezen om toegang te verkrijgen.

- **URL van openingspagina** (vereist): De site-URL die klanten worden omgeleid naar het land op na het aanschaffen van uw aanbieding uit marketplace definiëren. Deze URL worden ook het eindpunt dat de API's voor het vereenvoudigen van handel met Microsoft-verbinding wordt ontvangen.

- **Verbinding webhook** (vereist): Voor alle asynchrone gebeurtenissen die door Microsoft nodig heeft om u te sturen namens de klant (voorbeeld: Azure-abonnement is geworden ongeldige), moet u een webhook verbinding opgeven. Als u nog een webhook-systeem voldaan hebt, wordt de eenvoudigste configuratie is dat de logische App van een HTTP-eindpunt die luisteren naar gebeurtenissen worden gepubliceerd naar deze en klik vervolgens op de juiste wijze verwerkt (bijvoorbeeld https:\//prod-1westus.logic.azure.com:443/work). Zie voor meer informatie, [aanroepen, trigger of nesten van werkstromen met HTTP-eindpunten in logische apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-http-endpoint).

- **Azure AD-tenant-ID** (vereist): In Azure portal, vereisen we dat u [maken van een Azure Active Directory (AD)-app](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) zodat de verbinding tussen de twee services kan worden gevalideerd, zich achter een geverifieerde communicatie. Om te zoeken de [tenant-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-id), gaat u naar uw Azure Active Directory en selecteer **eigenschappen**, zoekt de **map-ID** dat staat vermeld (bijvoorbeeld) 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD-app-ID** (vereist): U moet ook uw [toepassings-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key) en een verificatiesleutel nodig. Als u deze waarden, gaat u naar uw Azure Active Directory en selecteer **App-registraties**, zoekt de **toepassings-ID** dat staat vermeld (bijvoorbeeld 50c464d3-4930-494c-963c-1e951d15360e). Als u de verificatiesleutel zoekt, gaat u naar **instellingen** en selecteer **sleutels**. U moet voor een beschrijving en de duur en wordt vervolgens een numerieke waarde worden opgegeven.

 Houd er rekening mee dat de Azure-toepassings-ID is gekoppeld aan de uitgevers-ID, dus zorg ervoor dat dezelfde toepassings-ID wordt gebruikt in alle uw aanbiedingen.

## <a name="plan-overview"></a>Overzicht van het plan

De **Plan overzicht** tabblad kunt u diverse opties voor plan in de dezelfde aanbieding. Deze abonnementen (soms ook wel SKU's genoemd) kunnen verschillen in termen van versie, verkopen of lagen van de service. U moet ten minste één abonnement om te kunnen verkopen van uw aanbieding op marketplace instellen.

Nadat u hebt gemaakt, ziet u de namen van uw abonnement,-id's, prijsmodellen, beschikbaarheid (openbaar of privé), huidige status, en alle beschikbare acties publiceren.

**Acties** beschikbaar in de **Plan overzicht** variëren, afhankelijk van de huidige status van uw abonnement en kunnen omvatten:

- Als de status van het plan is **Draft** – concept verwijderen
- Als de status van het plan is **Live** : Stop verkopen plan of particuliere doelgroep synchroniseren

**Nieuw plan maken** (het minimum van een plan voor diegenen die selecteren om te verkopen via Microsoft)

- **Plan-ID:** Maak een unieke plan-ID voor elk abonnement in deze aanbieding. Deze ID is zichtbaar voor klanten in de product-URL en Azure Resource Manager-sjablonen (indien van toepassing). Gebruik alleen kleine letters, alfanumerieke tekens, streepjes of onderstrepingstekens bevatten. Een maximum van 50 tekens zijn toegestaan voor dit abonnement-ID. Houd er rekening mee dat de ID kan niet worden gewijzigd nadat u hebt gemaakt.
- **Naam van het plan:** Klanten zien deze naam wanneer u beslist dat van plan bent om te selecteren in uw aanbieding. De naam van een unieke aanbieding voor elk plan maken in deze aanbieding. Naam van het plan wordt gebruikt om te plannen voor software die mogelijk een deel van de dezelfde aanbieding (bijvoorbeeld onderscheiden Naam van aanbieding: Windows Server; plans: Windows Server 2016, Windows Server 2019).

### <a name="plan-listing"></a>Overzicht plannen

De **aanbieding van plan bent** tabblad geeft de talen (en markten) waar het abonnement beschikbaar is, heeft een Engels (Verenigde Staten) is momenteel de enige locatie beschikbaar is. Deze pagina bevat ook de status van de aanbieding taalspecifieke en de datum/tijd waarop deze is toegevoegd. U moet de gegevens van marketplace (bieden naam, beschrijving, zoektermen, enz.) definiëren voor elke taal / markt.

#### <a name="plan-listing-details"></a>Plan de details van aanbieding

Als u een van de talen van het abonnement wordt weergegeven de **aanbieding van plan bent** informatie, met inbegrip van **naam** en **beschrijving.**

- **Naam**: Vooraf ingevuld op basis van uw voorbeeld **nieuw plan** vermelding en worden weergegeven als de titel van de "Software aanbieding' weergegeven in de marketplace.
- **Beschrijving:** Deze beschrijving is een kans om uit te leggen wat dit softwareabonnement unieke maakt en eventuele verschillen van andere softwareabonnementen in uw aanbieding. Mag maximaal 500 tekens bevatten.

Selecteer **opslaan** na het voltooien van deze velden.

#### <a name="plan-pricing-and-availability"></a>Prijzen en beschikbaarheid plannen

De **prijzen en de beschikbaarheid** tabblad kunt u configureren de markten die dit plan beschikbaar zijn in de gewenste verdiensten model, prijzen en facturering term. Bovendien kunt u aangeven of om het abonnement zichtbaar voor iedereen of alleen naar specifieke klanten (een particuliere doelgroep).

#### <a name="markets"></a>Markten

- **Bewerken markten** (optioneel)

Elke plan moet beschikbaar zijn in ten minste één markt. Selecteer het selectievakje voor elke markt-locatie waar u wilt dit plan beschikbaar te maken. Een zoekvak en een knop voor het selecteren van "Belasting overgeschreven" landen, waarbij Microsoft verkoop- en gebruiksbelasting namens u terugwijst worden opgenomen om u te helpen. 


Als u al prijzen hebt ingesteld voor uw abonnement in Amerikaanse dollar (USD) en het toevoegen van een andere locatie van de markt, wordt de prijs voor de nieuwe markt worden berekend op basis van de huidige exchange-tarieven. De prijs voor elke markt voordat u publiceert, moet u altijd controleren. Prijzen kan worden gecontroleerd met behulp van de koppeling 'Exporteren prijzen (xlsx)' na het opslaan van uw wijzigingen.

#### <a name="pricing"></a>Prijzen

- **Prijsmodel**: Vast bedrag of op basis van Seat

**Vast bedrag:** Toegang tot uw aanbieding met een enkele maandelijkse of jaarlijkse prijs geldt een proportionele prijs inschakelen. Dit wordt soms als site-gebaseerde prijzen genoemd.

**Aantal seats op basis van:** Toegang tot uw aanbieding inschakelen met de prijs op basis van het aantal gebruikers toegang krijgen tot de aanbieding of bezet *seats*. Dit model op basis van een seat kunt u om in te stellen het minimum en maximum aantal toegestane seats op basis van de prijs. Op deze manier kunnen verschillende prijzen worden geconfigureerd op basis van het aantal gebruikers door het configureren van meerdere abonnementen.  Deze velden zijn optioneel. Als dit leeg blijft, wordt het aantal seats beschouwd als niet met een limiet (minimaal 1) en de maximale hoeveelheid zo veel als het systeem kan ondersteunen. Deze velden kunnen worden bewerkt als onderdeel van een update voor uw abonnement.

Zodra gepubliceerd, kan de facturering prijzen model keuze kan niet worden gewijzigd. Alle abonnementen voor de dezelfde aanbieding moeten bovendien de dezelfde prijsmodel delen.

- **Facturering term**: Maandelijkse of jaarlijkse

Selecteer de frequentie waarmee klanten de weergegeven prijs moeten betalen. Ten minste één maandelijks of jaarlijks prijs moet worden opgegeven of beide opties kunnen beschikbaar worden gesteld aan klanten.

- **Prijs**: USD per maand of USD per jaar

Prijzen instellen in de lokale valuta (USD = Amerikaanse Dollar) worden geconverteerd naar de lokale valuta van alle geselecteerde markten met behulp van de huidige wisselkoersen beschikbaar tijdens de installatie. Deze prijzen vóór publicatie valideren door het exporteren van het werkblad prijzen en controleren van de prijs in elke markt. Als u wilt het aangepaste prijzen instellen op een afzonderlijke markt, wijzigen en de prijscategorie werkblad te importeren. U bent verantwoordelijk voor het valideren van deze prijzen en eigenaar van deze instellingen.
**Uw prijswijzigingen voor het exporteren van gegevens over prijzen inschakelen, moet u eerst opslaan.*

Controleer uw prijzen zorgvuldig door voordat u publiceert, omdat er enkele beperkingen zijn op welke wijzigen kunt nadat een abonnement is gepubliceerd:

- Zodra een plan is gepubliceerd, kan het prijsmodel kan niet worden gewijzigd.
- Zodra een termijn van de facturering is gepubliceerd voor een abonnement, kan niet deze later verwijderd.
- Zodra een prijs voor een markt in uw plan is gepubliceerd, kan niet deze later worden gewijzigd.

### <a name="plan-audience"></a>Doelgroep plannen

U hebt de optie voor het configureren van elk abonnement zichtbaar voor iedereen of alleen een specifieke doelgroep van uw keuze. U kunt het lidmaatschap van deze beperkte doelgroep met behulp van Azure AD-tenant id's toewijzen.

#### <a name="privacy"></a>Privacy

- **Dit is een persoonlijke plan** (optionele selectievakje)

Schakel dit selectievakje in als u uw plan privé en alleen zichtbaar voor de beperkte doelgroep van uw keuze. Zodra gepubliceerd als een persoonlijke abonnement, kunt u de doelgroep bijwerken of kiezen om het abonnement beschikbaar maken voor iedereen. Zodra een plan als voor iedereen zichtbaar is gepubliceerd, moet het voor iedereen zichtbaar blijven. Worden (het plan kan niet geconfigureerd als een persoonlijke plan opnieuw).

- **Beperkte doelgroep (Tenant-id's)**

De doelgroep die toegang tot dit persoonlijke plan hebben toewijzen. Toegang is toegewezen met behulp van tenant-id's met de optie om op te nemen van een beschrijving van elke tenant-ID die is toegewezen. Maximaal 10 tenant-id's kan worden toegevoegd of 20.000 klanten tenant-id's als een CSV-werkbladbestand importeert.

Er is een weergave van een organisatie, met de ID wordt weergegeven als een GUID (Globally Unique Identifier, een 128-bits geheel getal dat wordt gebruikt om resources te identificeren) voor een tenant. Een tenant is een toegewezen exemplaar van Azure AD dat een organisatie of app-ontwikkelaar ontvangt wanneer deze een relatie start met Microsoft, door zich bijvoorbeeld aan te melden voor Azure, Microsoft Intune of Microsoft 365. Elke Azure AD-tenant is uniek en werkt afzonderlijk van andere Azure AD-tenants. Om te controleren of de tenant, moet u zich aanmelden bij Azure portal met het account dat u wilt gebruiken voor het beheren van uw toepassing. Als u een tenant hebt, wordt u automatisch aangemeld en ziet u de naam van de tenant direct onder de accountnaam. Houd de muisaanwijzer op uw accountnaam rechtsboven in Azure Portal om uw naam, e-mailadres, directory en tenant-id (een GUID), en uw domein te zien. Als uw account is gekoppeld aan meerdere tenants, kunt u de accountnaam selecteren om een menu te openen waarmee u kunt schakelen tussen de tenants. Elke tenant heeft zijn eigen tenant-id. U kunt ook zoeken van de tenant-ID van uw organisatie met behulp van een domein naam URL op: [ https://www.whatismytenantid.com ](https://www.whatismytenantid.com).

SaaS-aanbiedingen gebruiken tenant-id's voor het definiëren van een particuliere doelgroep, kunnen andere typen Azure-abonnement-id's (die ook worden weergegeven als GUID's) gebruiken.

> [!NOTE]
> De particuliere doelgroep (of een beperkt publiek) verschilt van de doelgroep van een Preview-versie. In de **[Preview](#preview)** tabblad kunt u een doelgroep Preview-versie. Een preview-publiek toegang heeft tot uw aanbieding *voorafgaande* op de aanbieding die live wordt gepubliceerd in de marketplace. Terwijl de aanduiding particuliere doelgroep is alleen van toepassing op een specifiek plan, de doelgroep preview vindt u alle abonnementen (particulier of niet), maar alleen in de beperkte preview-periode terwijl het plan is getest en gevalideerd.

## <a name="example-list-of-plans-within-a-marketplace-offer"></a>Voorbeeld van de lijst met abonnementen binnen een marketplace-aanbieding

![Voorbeeld van de marketplace-abonnement met opmerkingen bij de aanbieding](./media/marketplace-plan.svg)

## <a name="test-drive"></a>Test drive

De **uitproberen** tabblad kunt u voor het instellen van een demonstratie (of 'test drive') die biedt klanten de mogelijkheid om te proberen uw aanbieding voordat het doorvoeren van kopen. Meer informatie in het artikel [wat is Test Drive?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive). Als u niet meer wilt voor een test uit voor uw aanbieding, Ga terug naar de **[bieden setup](#offer-setup)** pagina en schakel het selectievakje **Enable test drive**.

### <a name="technical-configuration"></a>Technische configuratie
De volgende typen proeven zijn beschikbaar, elk met hun eigen technische configuratievereisten.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Logische app](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (technische configuratie niet vereist)

#### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Technische configuratie voor Azure Resource Manager kunt uitproberen

Een implementatiesjabloon met alle Azure-resources die deel uitmaken van uw oplossing. Producten die geschikt zijn voor dit scenario gebruikt u alleen Azure-resources. Meer informatie over het instellen van een [Azure Resource Manager test drive](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Regio's** (vereist): Er zijn momenteel 26 Azure ondersteunde regio's waar uw test drive kan beschikbaar worden gesteld. Doorgaans wilt u uw test drive beschikbaar maken in de regio's waar u verwacht het grootste aantal klanten dat, zodat ze de dichtstbijzijnde regio voor de beste prestaties kunnen selecteren. U moet om ervoor te zorgen dat uw abonnement is toegestaan voor het implementeren van alle van de resources die nodig zijn in elk van de regio's die u selecteert.

- **Exemplaren**: Selecteer het type (warm of koud) en het nummer van exemplaren van beschikbaar, die zal worden vermenigvuldigd met het aantal regio's waar uw aanbieding beschikbaar is.

**Hot**: Dit type exemplaar dat is geïmplementeerd en wacht op toegang per geselecteerde regio. Klanten kunnen onmiddellijk toegang krijgen tot *warm* exemplaren van een test uit in plaats van dat moet worden gewacht voor een implementatie. Het verschil is dat deze instanties altijd worden uitgevoerd op uw Azure-abonnement, zodat ze een grotere uptime kosten wordt gebracht. Het is raadzaam dat ten minste één *warm* exemplaar, zoals de meeste klanten niet wilt wachten tot volledige implementaties, wat resulteert in een inleverbibliotheek in klanten te worden gebruikt als er geen *warm* exemplaar beschikbaar is.

**Koude**: Dit type exemplaar geeft het totale aantal instanties dat kan mogelijk per regio worden geïmplementeerd. Koude exemplaren vereisen dat de hele Test Drive Resource Manager-sjabloon om te implementeren wanneer een klant aanvraagt of test drive, dus *koude* exemplaren worden veel langzamer dan laden *warm* exemplaren. Een afweging is dat u hoeft alleen te betalen voor de duur van test drive, is het *niet* altijd wordt uitgevoerd op uw Azure-abonnement als met een *warm* exemplaar.

- **Test drive Azure Resource Manager-sjabloon**: Upload het ZIP met uw Azure Resource Manager-sjabloon.  Meer informatie over het maken van een Azure Resource Manager-sjabloon in dit artikel [maken en implementeren van Azure Resource Manager-sjablonen met behulp van de Azure-portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

- **Duur van test drive** (vereist): Geef de hoeveelheid tijd die de Test Drive actief is, in totaal aantal uren blijft. De Test Drive wordt automatisch beëindigd na afloop van deze periode. Deze duur kan alleen worden ingesteld door een geheel getal van uur (bijvoorbeeld) uren "2", is "1.5" niet geldig).

#### <a name="technical-configuration-for-dynamics-365-test-drive"></a>U kunt uitproberen technische configuratie voor Dynamics 365

Microsoft kan de complexiteit van het instellen van een test uit door de host en onderhouden van de service-inrichting en implementatie met behulp van dit type proefrit kunt verwijderen. De configuratie voor dit type gehoste test drive is hetzelfde, ongeacht of het station test een publiek Business Central, Customer Engagement of bewerkingen is gericht.

- **Max. gelijktijdige proeven** (vereist): Stel het maximum aantal klanten die uw test drive in één keer kunt gebruiken. Elke gelijktijdige gebruiker wordt een Dynamics 365-licentie gebruiken tijdens de test drive actief is, wordt dus u ervoor zorgen moet dat u voldoende licenties beschikbaar voor ondersteuning van de ingestelde maximumlimiet hebt. Aanbevolen waarde van 3 tot 5.

- **Duur van test drive** (vereist): Geef de hoeveelheid tijd die de Test Drive actief blijven met het definiëren van het aantal uren. Na dit aantal uur, wordt de sessie beëindigen en een van de licenties niet meer gebruiken. U wordt aangeraden een waarde van 2 tot 24 uur, afhankelijk van de complexiteit van uw aanbieding. Deze duur kan alleen worden ingesteld door een geheel getal van uur (bijvoorbeeld) uren "2", is "1.5" niet geldig).  De gebruiker kan een nieuwe sessie aanvragen als ze weinig tijd en toegang kunt krijgen tot de proefrit opnieuw.

- **Instantie-URL** (vereist): De URL waar de klant de test drive wordt gestart. Normaal gesproken de URL van uw Dynamics 365-instantie voor het uitvoeren van uw app met voorbeeldgegevens die zijn geïnstalleerd (bijvoorbeeld https://testdrive.crm.dynamics.com).

- **URL van de Web-API-exemplaar** (vereist): Ophalen van de URL van de Web-API voor uw exemplaar van Dynamics 365 door te melden bij uw Microsoft 365-account en te navigeren naar **instellingen** \&gt; **Aanpassing** \&gt; **Bronnen voor ontwikkelaars** \&gt; **Exemplaar Web-API (basis-URL)** , Kopieer de URL is hier beschikbaar (bijvoorbeeld https://testdrive.crm.dynamics.com/api/data/v9.0).

- **De naam van rol** (vereist): U hebt gedefinieerd in uw aangepaste test drive voor Dynamics 365-naam van de beveiligingsrol bieden. Dit zal worden toegewezen aan de gebruiker tijdens de test-station (bijvoorbeeld test-drive-rol).

#### <a name="technical-configuration-for-logic-app-test-drive"></a>Technische configuratie voor de logische app uitproberen

Alle aangepaste producten moeten dit type test drive-implementatiesjabloon die tal van complexe architecturen omvat gebruiken. Voor meer informatie over het instellen van logische App testen, gaat u naar [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) en [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) op GitHub.

- **Regio** (vereist, enkelvoudige selectie vervolgkeuzelijst): Er zijn momenteel 26 Azure ondersteunde regio's waar uw test drive kan beschikbaar worden gesteld. De resources voor uw logische app wordt geïmplementeerd in de regio die u selecteert. Als uw logische App een aangepaste resources die zijn opgeslagen in een bepaalde regio, moet u dat die regio hier is geselecteerd. De beste manier om dit te doen is volledig uw logische App lokaal op uw Azure-abonnement in de portal implementeren en controleren of deze correct werkt voordat u deze selectie.

- **Max. gelijktijdige proeven** (vereist): Stel het maximum aantal klanten die uw test drive in één keer kunt gebruiken. Deze test drives al zijn geïmplementeerd, zodat klanten kunnen onmiddellijk toegang tot deze zonder te wachten op een implementatie.

- **Duur van test drive** (vereist): Geef de hoeveelheid tijd die de Test Drive actief is, in totaal aantal uren blijft. Test drive wordt automatisch beëindigd na afloop van deze periode.

- **Naam van de Azure-resourcegroep** (vereist): Voer de [Azure-resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) naam waar uw logische App test drive wordt opgeslagen.

- **Azure logic app-naam** (vereist): Voer de naam van de logische app waarmee de proefrit worden toegewezen aan de gebruiker. Deze logische app moet worden opgeslagen in de bovenstaande Azure-resources-groep.

- **Inrichting logic app-naam** (vereist): Voer de naam van de logische app die de proefrit deprovisions wanneer de klant voltooid is. Deze logische app moet worden opgeslagen in de bovenstaande Azure-resources-groep.

#### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Technische configuratie niet vereist voor Power BI-proeven

Producten die u tonen dat een interactieve Power BI-visualisatie kunt een ingesloten koppeling gebruiken wilt voor het delen van een op maat gemaakte dashboard als hun test drive er kunnen geen nieuwe technische configuratie vereist. Meer informatie over het instellen van[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview) sjabloon-apps.

### <a name="deployment-subscription-details"></a>Implementatie-abonnementsgegevens

Als u wilt implementeren de Test Drive uit uw naam, maken en een afzonderlijke, unieke Azure-abonnement. (Niet vereist voor Power BI test drives).

- **Azure-abonnement-ID** (vereist voor Azure Resource Manager en Logic apps): Voer de abonnement-ID voor het verlenen van toegang tot de services van uw Azure-account voor Resourcegebruik reporting en facturering. Het is raadzaam dat u rekening houden met [het maken van een afzonderlijke Azure-abonnement](https://docs.microsoft.com/azure/billing/billing-create-subscription) voor test-stations gebruiken als u er nog geen hebt. U kunt uw Azure-abonnement-ID vinden door aan te melden bij de [Azure-portal](https://portal.azure.com/) en te navigeren naar de **abonnementen** tabblad van het menu aan de linkerkant. Het tabblad te selecteren, wordt uw abonnements-ID (bijvoorbeeld ' a83645ac-1234-5ab6-6789-1h234g764ghty") weergegeven.

- **Azure AD-tenant-ID** (vereist): Voer uw Azure Active Directory (AD) [tenant-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-id). Als u wilt deze ID vinden, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com/), selecteert u het Active Directory-tabblad in de links in het menu, selecteer **eigenschappen** , zoekt de **map-ID** dat staat vermeld (bijvoorbeeld) 50c464d3-4930-494c-963c-1e951d15360e). U kunt ook zoeken van de tenant-ID van uw organisatie met behulp van de URL van uw domein op: [ https://www.whatismytenantid.com ](https://www.whatismytenantid.com).

- **Azure AD-tenantnaam** (vereist voor Dynamic 365): Voer de naam van uw Azure Active Directory (AD). Als u deze naam zoekt, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com/), in de rechterbovenhoek de tenantnaam van uw worden vermeld in de accountnaam van uw.

- **Azure AD-app-ID** (vereist): Voer uw Azure Active Directory (AD) [toepassings-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key). Als u wilt deze ID vinden, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com/), selecteert u het Active Directory-tabblad in de links in het menu, selecteer **App-registraties**, zoekt de **toepassings-ID** getal vermeld (bijvoorbeeld 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD-app-sleutel** (vereist): Voer uw Azure Active Directory (AD) [Toepassingssleutel](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-application-id-and-authentication-key). Als u wilt deze ID vinden, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com/), selecteert u het Active Directory-tabblad in de links in het menu, selecteer **App-registraties** en selecteer vervolgens **instellingen**  >  **Sleutels**.

Houd er rekening mee te **opslaan** voordat u doorgaat met de volgende sectie.

### <a name="test-drive-listings-optional"></a>Test drive-aanbiedingen (optioneel)

De **Test Drive aanbiedingen** optie te vinden onder de **uitproberen** tabblad geeft de talen (en markten) waar uw test drive beschikbaar is, Engels (Verenigde Staten) is momenteel de enige locatie beschikbaar . Deze pagina bevat ook de status van de aanbieding taalspecifieke en de datum/tijd waarop deze is toegevoegd. U moet voor het definiëren van de test drive details (beschrijving, gebruikershandleiding, video's, enzovoort) voor elke taal/markt.

- **Beschrijving** (vereist): Uw test drive, wat worden gedemonstreerd, beschrijven doelstellingen voor de gebruiker om te experimenteren met functies om te verkennen en eventuele relevante informatie om u te helpen bij de gebruiker te bepalen of aan te schaffen van uw aanbieding. In dit veld kunnen maximaal 3000 tekens van de tekst worden ingevoerd. 

- **Toegang tot informatie** (vereist voor Azure Resource Manager en de logische drives test): Wordt uitgelegd wat een klant moet weten om te openen en gebruiken van deze test drive. Een scenario voor het gebruik van uw aanbieding en precies wat de klant moet weten voor toegang tot functies in de gehele test drive doorlopen. In dit veld kunnen maximaal 10.000 tekens van de tekst worden ingevoerd.

- **Gebruikershandleiding** (vereist): Een uitgebreid overzicht van uw test drive-ervaring. De handleiding moet betrekking hebben op precies wat u wilt de klant ondervindt test drive krijgen en fungeren als een verwijzing voor eventuele vragen die ze kunnen hebben. Het bestand moet zich in PDF-indeling en worden met de naam (max. 255 tekens) na het uploaden.

- **Video's: Video's toevoegen** (optioneel): Video's kunnen worden geüpload naar YouTube of Vimeo en hier wordt verwezen met een koppeling en miniatuur-installatiekopie (533 x 324 pixels) dat een klant een Walkthrough van informatie zodat ze beter begrip van de test-station, waaronder het gebruik van de functies van is uw inzicht in scenario hun voordelen markeren en bieden.
  - **Naam** (vereist)
  - **URL (YouTube of Vimeo alleen)** (vereist)
  - **Miniatuur (533 x 324px)** : Bestand van de installatiekopie moet zich in de PNG-indeling.

Selecteer **opslaan** na het voltooien van deze velden.

## <a name="publish"></a>Publiceren

#### <a name="submit-offer-to-preview"></a>Indienen om een voorbeeld te bieden

Nadat u alle vereiste secties van de aanbieding hebt voltooid, selecteert u **publiceren** in de rechterbovenhoek van de portal. Kunt u zich opnieuw gerichte naar de **controleren en te publiceren** pagina. 

Als dit de eerste keer is deze aanbieding publiceren, kunt u het volgende doen:

- Zie de voltooiingsstatus voor elke sectie van de aanbieding.
    - *Niet gestart* : betekent dat de sectie niet is gewijzigd en moet worden voltooid.
    - *Onvolledige* : betekent dat de sectie bevat fouten die moeten worden hersteld of moet u meer gegevens worden opgegeven. Ga terug naar de sectie (s) en deze bijwerken.
    - *Volledige* : betekent dat de sectie is voltooid, worden alle vereiste gegevens is opgegeven en er geen fouten zijn. Alle secties van de aanbieding moeten zich in een status voltooid voordat u de aanbieding kunt indienen.
- Vindt u instructies aan het CA-team testen om ervoor te zorgen dat uw app naast de aanvullende opmerkingen die nuttig is voor het begrijpen van uw app correct is getest.
- De aanbieding voor publicatie verzenden door te selecteren **indienen**. We sturen u een e-mailbericht zodat u weet wanneer er een previewversie van de aanbieding is beschikbaar voor u om te controleren en goedkeuren. U moet terugkeren naar de Partner Center en selecteer **Go live** voor de aanbieding voor uw aanbieding publiceren naar de openbare (of als een persoonlijke aan de particuliere doelgroep aanbieden).

## <a name="next-steps"></a>Volgende stappen

- [Een bestaande aanbieding op Marketplace commercieel bijwerken](./update-existing-offer.md)

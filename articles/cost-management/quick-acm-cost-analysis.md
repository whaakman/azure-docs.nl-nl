---
title: Snelstartgids - Azure-kosten met kostenanalyse verkennen | Microsoft Docs
description: Deze snelstart helpt u kostenanalyse te gebruiken om de kosten van Azure voor uw bedrijf te verkennen en te analyseren.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/11/2019
ms.topic: quickstart
ms.service: cost-management
manager: micflan
ms.custom: seodec18
ms.openlocfilehash: d07a52a3cd07c69b9b7e2ef43331ddd9fb455b10
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827820"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>Quickstart: Verken en analyseer de kosten met kostenanalyse

Voordat u de kosten van Azure goed kunt beheren en optimaliseren, moet u de oorsprong van de kosten in uw bedrijf weten. Het is ook handig om te weten hoeveel geld uw services kosten, en ter ondersteuning van die omgevingen en systemen. Inzicht in de volle breedte van alle kosten is noodzakelijk om nauwkeurig de uitgavenpatronen van uw bedrijf te leren kennen. U kunt de bestedingslimiet patronen om af te dwingen kosten mechanismen, zoals budgetten.

In deze snelstart gebruikt u kostenanalyse om de kosten van Azure voor uw bedrijf te verkennen en te analyseren. U kunt de totale kosten per bedrijf weergeven, zodat u begrijpt waar de kosten worden opgebouwd en uitgavenpatronen kunt identificeren. U kunt de totale kosten bekijken om geschatte kostentrends per maand, per kwartaal en zelfs per jaar naast een budget te leggen. Een budget helpt bij het in acht nemen van financiële beperkingen. En een budget wordt gebruikt om dagelijkse en maandelijkse kosten te bekijken om onregelmatigheden in de uitgaven te isoleren. Plus, u kunt de gegevens van het huidige rapport downloaden voor verdere analyse of om in een extern systeem te gebruiken.

In deze snelstart leert u de volgende zaken:

- Kosten in kostenanalyse beoordelen
- Kostenweergaven aanpassen
- Gegevens van kostenanalyse downloaden


## <a name="prerequisites"></a>Vereisten

Kostenanalyse biedt ondersteuning voor verschillende soorten typen Azure-account. Zie voor de volledige lijst met ondersteunde accounttypen [Gegevens van Azure Cost Management begrijpen](understand-cost-mgt-data.md). Als u kostengegevens wilt weergeven, hebt u minimaal leestoegang voor uw Azure-account nodig.

Voor [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) moet u minimaal leestoegang hebben tot een of meer van de volgende bereiken om kostengegevens te kunnen bekijken.

- Factureringsaccount
- Afdeling
- Inschrijvingsaccount
- Beheergroep
- Subscription
- Resource group

Zie voor meer informatie over het toewijzen van toegang tot gegevens van Azure Cost Management [toegang tot gegevens toewijzen](assign-access-acm-data.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

- Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="review-costs-in-cost-analysis"></a>Kosten in kostenanalyse beoordelen

Als u wilt uw kosten in kostenanalyse bekijken, opent u het bereik in de Azure portal en selecteer **Cost analysis** in het menu. Bijvoorbeeld, gaat u naar **abonnementen**, selecteer een abonnement in de lijst en selecteer vervolgens **Cost analysis** in het menu. Gebruik de **bereik** pill om over te schakelen naar een ander bereik in kostenanalyse. Zie voor meer informatie over bereiken [begrijpen en werk met een bereik](understand-work-scopes.md).

Het bereik dat u selecteert, wordt gebruikt in Cost Management voor gegevens consolidatie en beheren van toegang tot kostengegevens. Wanneer u gebruikmaakt van bereiken, moet u geen meervoudige selectie maken. In plaats daarvan selecteert u een groter bereik, die anderen tot rollen, en filter vervolgens omlaag naar de geneste bereiken die u nodig hebt. Deze aanpak is belangrijk om te begrijpen omdat sommige mensen mogelijk geen toegang hebt tot een enkel bovenliggend bereik bevat informatie over meerdere geneste bereiken.

De initiële kosten analysis-weergave bevat de volgende gebieden.

**Kosten-weergave samengevoegd**: Hiermee geeft u de configuratie van de vooraf gedefinieerde kosten analysis bekijken. Elke weergave bevat datumbereik, granulatie, groeperen op, en filterinstellingen. De standaardweergave toont de totale kosten voor de huidige factureringsperiode, maar u kunt wijzigen in andere ingebouwde weergaven. Zie voor meer informatie, [aanpassen weergaven kosten](#customize-cost-views).

**Werkelijke kosten**: Toont de totale kosten voor gebruik en de aankoop voor de huidige maand, zoals ze worden samengevoegd en wordt weergegeven op uw factuur.

**Prognose**: Toont de totale geraamde kosten voor een periode die u kiest. (Prognose is in Preview-versie.)

**Budget**: Geeft de geplande bestedingslimiet voor het geselecteerde bereik, indien beschikbaar.

**Samengevoegde granulariteit**: Toont de totale cumulatieve dagelijkse kosten, vanaf het begin van de factureringsperiode. Nadat u een [budget hebt gemaakt](tutorial-acm-create-budgets.md) voor uw factureringsrekening of abonnement, kunt u snel uw uitgaventrend ten opzichte van het budget bekijken. Beweeg de muisaanwijzer over een datum om de opgebouwde kosten voor die dag te zien.

**Draaitabel (ringdiagrammen)** : Dynamische draaigrafieken, behalve dat de totale kosten door een gemeenschappelijke set eigenschappen voor standard bieden. Ze weergegeven de grootste naar kleinste kosten voor de huidige maand. U kunt draaigrafieken op elk moment wijzigen door een andere pivot te selecteren. Kosten worden gecategoriseerd door de service (metercategorie), de locatie (regio) en onderliggend bereik standaard. Bijvoorbeeld, inschrijvingsaccounts zijn onder facturering accounts, resourcegroepen zijn onder abonnementen en resources zijn onder de resourcegroepen.

![Oorspronkelijke weergave van Kostenanalyse in Azure Portal](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>Kostenweergaven aanpassen

Kostenanalyse heeft vier ingebouwde weergaven, geoptimaliseerd voor de meest voorkomende doelen:

Weergave | Antwoorden op vragen zoals
--- | ---
Totale kosten | Hoeveel heb ik besteed aan het tot nu toe voor deze maand? Blijf ik binnen mijn budget?
Dagelijkse kosten | Er zijn een toename in de kosten per dag voor de afgelopen 30 dagen?
Kosten per service | Hoe heeft mijn maandelijks gebruik gedurende de afgelopen drie facturen variëren?
Kosten per resource | Welke bronnen de meeste dusver deze maand kosten?

![Weergaveselectie van een voorbeeld van de selectie voor deze maand](./media/quick-acm-cost-analysis/view-selector.png)

Er zijn echter veel gevallen waar u een meer gedetailleerde analyse nodig hebt. Het aanpassen begint bovenaan de pagina, met de selectie van de datum.

Kostenanalyse toont standaard de gegevens voor de huidige maand. De datum-selector snel overschakelen naar algemene datumbereiken gebruiken. Voorbeelden hiervan zijn de afgelopen zeven dagen, de laatste maand, het huidige jaar of een aangepast datumbereik. Betalen per gebruik-abonnementen omvatten ook datumbereiken op basis van de factureringsperiode is niet aan de kalendermaand, zoals de huidige factureringsperiode of laatste factuur gebonden. Gebruik de **< vorige** en **volgende >** koppelingen aan de bovenkant van het menu om door te gaan naar de volgende of vorige periode, respectievelijk. Bijvoorbeeld, **< vorige** wordt overschakelen van de **afgelopen 7 dagen** naar **8-14 dagen geleden** of **15-21 dagen geleden**.

![Datumselector met een voorbeeldselectie voor deze maand](./media/quick-acm-cost-analysis/date-selector.png)

Kostenanalyse toont standaard **opgebouwde** kosten. Totale kosten omvatten alle kosten voor elke dag plus de voorgaande dagen, voor een voortdurend groeiende weergave van uw dagelijkse totale kosten. Deze weergave is geoptimaliseerd om u te laten zien hoe u het doet in verhouding met het budget voor het geselecteerde tijdsbereik.

Gebruik de weergave diagram voor het identificeren van mogelijke schendingen van het budget. Wanneer er een mogelijk beveiligingsprobleem met budget, wordt verwachte budgetoverschrijding weergegeven in het rood. Een indicatorsymbool wordt ook weergegeven in de grafiek. De muisaanwijzer boven het symbool bevat de geschatte datum van de schending van het budget.

![Voorbeeld tonen potentiële budget inbreuk](./media/quick-acm-cost-analysis/budget-breach.png)

Er is ook de **dagelijkse** weergave die de kosten voor elke dag toont. De dagelijkse weergave toont geen groeitrend. De weergave is ontworpen om u onregelmatigheden als pieken of dalen in de kosten per dag te laten zien. Als u een budget hebt geselecteerd, geeft de dagelijkse weergave ook een schatting van uw dagelijkse budget.

Als uw dagelijkse kosten consistent boven het geschatte dagelijkse budget, kunt u verwachten dat u gaat uw maandelijkse budget Overtref. De geschatte dagelijkse budget is een manier om u te helpen u uw budget op een lager niveau visualiseren. Wanneer u schommelingen hebt in uw dagelijkse kosten dan is de vergelijking tussen het geschatte dagelijkse budget en uw maandelijkse budget minder nauwkeurig.

Hier volgt een dagelijks overzicht van recente uitgaven met uitgaven voorspellen ingeschakeld.
![Dagelijkse weergave voor voorbeeld van de dagelijkse kosten voor de huidige maand](./media/quick-acm-cost-analysis/daily-view.png)

Wanneer uitschakelt de uitgaven voorspellen, ziet u geen geraamde uitgaven voor datums in de toekomst. Wanneer u kosten voor afgelopen perioden bekijkt, weergeven geen prognose kosten ook kosten.

Over het algemeen kunt u verwachten dat gegevens of meldingen voor de verbruikte resources binnen 8 tot 12 uur weergegeven.


**Groeperen op** topinzenders in het algemene eigenschappen voor het opsplitsen van kosten en identificeren. Als u wilt groeperen op resourcetags, bijvoorbeeld, selecteer de tagsleutel die u groeperen wilt op. Kosten worden opgedeeld per elke tagwaarde, met een extra segment voor resources die geen dat label wordt toegepast.

De meeste [ondersteuning voor Azure-resources taggen](../azure-resource-manager/tag-support.md). Sommige labels zijn echter niet beschikbaar in kostenbeheer en facturering. Bovendien worden resourcegroeptags niet ondersteund. Cost Management ondersteunt resourcetags alleen vanaf de datum waarop de tags rechtstreeks worden toegepast op de resource. Bekijk de [over het bekijken van beleid voor resourcetags bij Azure Cost Management](https://www.youtube.com/watch?v=nHQYcYGKuyw) video voor meer informatie over het gebruik van Azure-tag-beleid voor het verbeteren van de kosten voor gegevens zichtbaar.

Hier volgt een overzicht van de kosten voor Azure-service voor de huidige maand.

![Gegroepeerde, dagelijks samengevoegde weergave met een voorbeeld van Azure-servicekosten van de afgelopen maand](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

Kostenanalyse toont alle kosten voor gebruik en de aankoop zoals ze worden samengevoegd en wordt weergegeven op uw factuur, ook wel bekend als **werkelijke kosten**. Werkelijke kosten weergeven is ideaal voor het afstemmen van uw factuur. Aankoop pieken in de kosten kunnen zijn in verontrustend wanneer u voor afwijkingen en andere wijzigingen in de kosten voor de uitgavelimiet gaten houden uit. Als u wilt samenvoegen om pieken veroorzaakt door een reservering kopen kosten, Ga naar **afgeschreven kosten**. 

![Wisselen tussen de werkelijke en afgeschreven kosten om te zien van de reservering aanschaft verspreid over de term en toegewezen aan de resources die u de reservering gebruikt](./media/quick-acm-cost-analysis/metric-picker.png)

Afgeschreven kosten een uitsplitsing van aankopen in de reservering in segmenten van dagelijkse en ze in de loop van de reserveringstermijn verspreid. Bijvoorbeeld, in plaats van een $-365 aankopen doen op 1 januari ervaart, ziet u een $-1 elke dag vanaf 1 januari tot en met 31 December kopen. Naast eenvoudige afschrijving worden deze kosten ook opnieuw toegewezen en die zijn gekoppeld met behulp van de specifieke bronnen waarmee u de reservering. Bijvoorbeeld, als die dagelijks afschrijving van $1 is gesplitst tussen twee virtuele machines, ziet u twee $0,50 kosten in rekening gebracht voor die dag. Als onderdeel van de reservering wordt niet voor die dag gebruikt, ziet u een $0,50 in rekening gebracht die zijn gekoppeld aan de betreffende virtuele machine en een andere $0,50 in rekening gebracht met het type kosten in rekening gebracht `UnusedReservation`. Houd er rekening mee dat kosten niet-gebruikte reservering alleen wanneer weergave kosten afgeschreven kunnen worden weergegeven.

Vanwege de wijziging in hoe de kosten worden weergegeven, is het belangrijk te weten dat werkelijke kosten en weergaven van de afgeschreven kosten verschillende totale cijfers wordt weergegeven. In het algemeen is de totale kosten van maanden bij een aankoop van de reservering wordt verlaagd wanneer kijken naar de afgeschreven kosten en maanden na de aankoop van de reservering wordt verhoogd. Afschrijving is alleen beschikbaar voor aankopen in de reservering en niet van toepassing op Azure Marketplace-aankopen op dit moment.

De volgende afbeelding toont resource namen van groepen. U kunt groeperen op tag weergeven van de totale kosten per tag of gebruik de **kosten per resource** weer te geven van alle tags voor een bepaalde resource.

![Volledige gegevens van de huidige weergave, waarbij de resourcegroepnamen worden weergegeven](./media/quick-acm-cost-analysis/full-data-set.png)

Wanneer u kosten door een specifiek kenmerk groepeert, worden de bovenste 10 kostenposten weergegeven van de hoogste naar laagste. Als er meer dan 10, de Actiefste negen kosten leden worden weergegeven in een **anderen** groep. Deze groep bevat alle resterende groepen samen. Als u op label groeperen wilt, een **Untagged** groep wordt weergegeven voor de kosten die niet de labelsleutel toegepast. **Niet-gecodeerde** is altijd laatst, zelfs als kosten zonder tag hoger dan gelabelde kosten zijn. Kosten zonder tag wordt deel uitmaken van **anderen**, als 10 of meer tagwaarden bestaat.

Klassieke virtuele machines, netwerken en opslagbronnen delen niet gedetailleerde factureringsgegevens. Ze worden samengevoegd als **Klassieke services** wanneer kosten worden gegroepeerd.

Grafieken onder de verschillende groepen weergeven van de belangrijkste grafiek, zodat u een bredere beeld van uw totale kosten voor de geselecteerde periode en filters. Selecteer een eigenschap of tag om geaggregeerde kosten voor een dimensie weer te geven.

![Voorbeeld van grafieken weergeven](./media/quick-acm-cost-analysis/pivot-charts.png)

U kunt de volledige gegevensset voor elke weergave bekijken. Welke selecties of filters die u van toepassing zijn van invloed zijn op de gegevens die worden gepresenteerd. Als u wilt zien van de volledige gegevensset, selecteert u de **grafiektype** lijst en selecteer vervolgens **tabel** weergeven.

![Gegevens voor de huidige weergave in tabelvorm](./media/quick-acm-cost-analysis/chart-type-table-view.png)


## <a name="understanding-grouping-and-filtering-options"></a>Understanding groepering en opties voor het filteren

De volgende tabel bevat enkele van de meest voorkomende groeperen en filteren van opties en wanneer u deze moet gebruiken.

| Eigenschap | Wanneer gebruikt u dit? |
| --- | --- |
| **Factureringsperiode** | Kosten per factuur maand opsplitsen. Deze optie is belangrijk voor betalen per gebruik en ontwikkeling/test-abonnementen, die niet zijn gekoppeld aan kalendermaanden. EA/MCA accounts kunnen kalendermaanden in de datumkiezer of maandelijkse granulariteit gebruiken om uit te voeren hetzelfde doel. |
| **Type van de kosten in rekening gebracht** | Gebruik, inkoop, restitutie en niet-gebruikte reservering kosten opsplitsen. Reservering-aankopen en terugbetalingen zijn beschikbaar alleen bij gebruik van de kosten van de actie en niet bij het gebruik van de afgeschreven kosten. Niet-gebruikte reservering kosten zijn alleen beschikbaar wanneer afgeschreven kosten kijken. |
| **Cloud** | Kosten per AWS en Azure opsplitsen. AWS-kosten zijn alleen beschikbaar via beheergroepen, externe facturering accounts en externe abonnementen. |
| **Afdeling** / **sectie factuur** | Kosten van EA-afdeling of MCA factuur sectie opsplitsen. Deze optie is alleen beschikbaar voor EA/MCA facturering accounts en facturering profielen MCA. |
| **Voor apparaatregistratie-account** | Kosten per EA accounteigenaar opsplitsen. Deze optie is alleen beschikbaar voor EA-accounts voor facturering en afdelingen. |
| **Frequentie** | Splitsen op basis van gebruik, eenmalige en terugkerende kosten. |
| **Meter** | Kosten onderverdeeld per meter voor gebruik van Azure. Deze optie is alleen beschikbaar voor gebruik van Azure. Alle aankopen en het gebruik van Marketplace weergegeven als **niet opgegeven** of **niet-toegewezen**. |
| **Uitgevertype** | AWS en Azure Marketplace-kosten opsplitsen. |
| **Reservering** | Kosten door reservering opsplitsen. Gebruik waarin een reservering niet wordt weergegeven als **niet opgegeven**. |
| **Resource** | Kosten per resource opsplitsen. Alle aankopen in de weergegeven als **niet opgegeven**, omdat deze worden toegepast op een factureringsaccount EA/betalen per gebruik of MCA facturering van het niveau van profiel.  |
| **Resourcegroep** | Kosten per resourcegroep opsplitsen. Deze optie is alleen beschikbaar voor niet-klassieke gebruik. Gebruik van klassieke resources weergegeven als **andere**, en aankopen weergegeven als **niet opgegeven**. |
| **Resourcetype** | Kosten per resourcetype opsplitsen. Deze optie is alleen beschikbaar voor niet-klassieke gebruik. Gebruik van klassieke resources weergegeven als **andere**, en aankopen weergegeven als **niet opgegeven**. |
| **Servicenaam** of **metercategorie** | Kosten onderverdeeld per Azure-service. Deze optie is alleen beschikbaar voor gebruik van Azure. Alle aankopen en het gebruik van Marketplace weergegeven als **niet opgegeven** of **niet-toegewezen**. |
| **Servicelaag** of **subcategorie van de Meter** | Kosten door gebruik van Azure-meter subclassificatie opsplitsen. Deze optie is alleen beschikbaar voor gebruik van Azure. Alle aankopen en het gebruik van Marketplace weergegeven als **niet opgegeven** of **niet-toegewezen**. |
| **Abonnement** | Kosten onderverdeeld per abonnement. Alle aankopen weergeven als **niet opgegeven**. |
| **Tag** | Kosten onderverdeeld per tagwaarden voor een specifieke tag-sleutel. |

Zie voor meer informatie over voorwaarden, [inzicht in de termen die worden gebruikt in de Azure-gebruik en kosten-](../billing/billing-understand-your-usage.md).


## <a name="saving-and-sharing-customized-views"></a>Opslaan en delen van aangepaste weergaven

Opslaan en delen van aangepaste weergaven met anderen door kostenanalyse naar de Azure portal-dashboard vast te maken of een koppeling naar kostenanalyse kopiëren. 

Als u wilt vastmaken cost analysis, selecteer het speldpictogram in de rechterbovenhoek. Kostenanalyse vast te maken, wordt alleen de grafiek of de tabel hoofdweergave opgeslagen. Deel het dashboard voor anderen toegang geven tot de tegel. Houd er rekening mee dat dit alleen de configuratie van het dashboard deelt, en niet anderen toegang tot de onderliggende gegevens verlenen. Als u geen toegang tot kosten hebt, maar u toegang tot een gedeeld dashboard hebt, ziet u een bericht 'toegang geweigerd'.

Als u wilt delen een koppeling naar de kosten van analyse, selecteer **delen** aan de bovenkant van de blade. Een aangepaste URL wordt weergegeven, die deze specifieke weergave voor deze specifieke scope wordt geopend. Als u geen kosten voor toegang en deze URL hebt, ziet u een bericht 'toegang geweigerd'. 

Bekijk voor meer informatie over het verlenen van toegang tot de kosten voor elke ondersteunde scope [begrijpen en werk met een bereik](understand-work-scopes.md).

## <a name="automation-and-offline-analysis"></a>Automatisering en offline-analyse

Er zijn tijden wanneer u nodig hebt voor het downloaden van de gegevens voor verdere analyse, het account samenvoegen met uw eigen gegevens, of integreren in uw eigen systemen. Cost Management biedt een aantal andere opties. Als een beginpunt maken als u een ad-hoc Geavanceerd overzicht, zoals wat u binnen kostenanalyse krijgt, moet de weergave die u nodig hebt. Vervolgens downloaden door te selecteren **exporteren** en selecteren **gegevens downloaden naar CSV** of **downloaden van gegevens naar Excel**. De Excel-download bevat aanvullende context voor de weergave die u gebruikt voor het genereren van het downloaden, zoals bereik, configuratie, totaal query en datum gegenereerd.

Als u de volledige, unaggregated gegevensset nodig, het downloaden van het factureringsaccount. In de lijst met services in het linkernavigatievenster van de portal, ga dan naar **kostenbeheer en facturering**. Selecteer uw factureringsrekening, indien van toepassing. Ga naar **gebruik en kosten**, en selecteer vervolgens de **downloaden** pictogram voor de gewenste factureringsperiode.

Een soortgelijke benadering uitvoeren voor het automatiseren van ontvangst van kostengegevens. Gebruik de [Query-API](/rest/api/cost-management/query) voor uitgebreidere analyse met dynamisch filteren, groeperen en aggregatie of gebruik de [UsageDetails API](/rest/api/consumption/usageDetails) unaggregated voor de volledige gegevensset. De algemene beschikbaarheid (GA) versie van deze API's is 2019-01-01. Gebruik **2019-04-01-preview** toegang te krijgen tot de Preview-versie van de reservering en Marketplace-aankopen binnen deze API's. 

Hieronder volgt bijvoorbeeld een samengevoegde weergave van de afgeschreven kosten onderverdeeld op basis van type kosten in rekening gebracht (syntaxis, kopen of restitutie), publisher type (Azure of Marketplace), de resourcegroep (leeg voor aankopen) en de reservering (leeg als niet van toepassing).

```
POST https://management.azure.com/{scope}/providers/Microsoft.CostManagement/query?api-version=2019-04-01-preview
Content-Type: application/json
 
{
  "type": "AmortizedCost",
  "timeframe": "Custom",
  "timePeriod": { "from": "2019-04-01", "to": "2019-04-30" },
  "dataset": {
    "granularity": "None",
    "aggregation": {
      "totalCost": { "name": "PreTaxCost", "function": "Sum" }
    },
    "grouping": [
      { "type": "dimension", "name": "ChargeType" },
      { "type": "dimension", "name": "PublisherType" },
      { "type": "dimension", "name": "Frequency" },
      { "type": "dimension", "name": "ResourceGroup" },
      { "type": "dimension", "name": "SubscriptionName" },
      { "type": "dimension", "name": "SubscriptionId" },
      { "type": "dimension", "name": "ReservationName" },
      { "type": "dimension", "name": "ReservationId" },
    ]
  },
}
```

En als u niet de aggregatie nodig hebt en de voorkeur geeft aan de volledige, onbewerkte gegevensset:

```
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?metric=AmortizedCost&$filter=properties/usageStart+ge+'2019-04-01'+AND+properties/usageEnd+le+'2019-04-30'&api-version=2019-04-01-preview
```

Als u de werkelijke kosten weer te geven van aankopen, zoals ze worden samengevoegd, wijzigen moet **type**/**metric** naar **ActualCost**. Zie voor meer informatie over deze API's, de [Query](/rest/api/cost-management/query) en [UsageDetails](/rest/api/consumption/usageDetails) API-documentatie. Houd er rekening mee dat de gepubliceerde documenten voor de GA-versie zijn. Maar ze werken op dezelfde manier voor het *2019-04-01-preview* API-versie buiten het nieuwe type/metriek kenmerk en de namen van gewijzigde eigenschappen. (Meer informatie over de namen van eigenschappen hieronder).
 
Management-API's work kosten over alle bereiken bovenstaande resources: resourcegroep, abonnement en beheergroep via Azure RBAC toegang EA facturering accounts (inschrijvingen), afdelingen en inschrijvingsaccounts via EA-portal-toegang. Meer informatie over bereiken, met inbegrip van hoe u om te bepalen van uw bereik-ID of beheren van toegang, in [begrijpen en werk met een bereik](understand-work-scopes.md).

## <a name="next-steps"></a>Volgende stappen

Ga door naar de eerste zelfstudie voor informatie over het maken en beheren van een budget.

> [!div class="nextstepaction"]
> [Budgetten maken en beheren](tutorial-acm-create-budgets.md)

---
title: Azure-kosten met Cost analysis verkennen | Microsoft Docs
description: Dit artikel helpt u kostenanalyse gebruiken om te verkennen en analyseren van uw Azure-kosten.
services: billing
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 08/16/2018
ms.topic: conceptual
ms.service: billing
manager: dougeby
ms.custom: ''
ms.openlocfilehash: eeaf02853f8ffe9ca67dbf31afc687afb7dee242
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180772"
---
# <a name="explore-and-analyze-costs-with-cost-analysis"></a>Verken en analyseer de kosten met Cost analysis

Voordat u kunt correct beheren en optimaliseren van uw Azure-kosten, moet u inzicht in de oorsprong van kosten binnen uw organisatie. Het is ook handig om te weten hoeveel geld uw services kosten, en ter ondersteuning van welke omgevingen en systemen. Inzicht in het volledige spectrum van de kosten is essentieel om nauwkeurig te begrijpen organisatie bestedingslimiet patronen die kunnen worden gebruikt om af te dwingen kosten mechanismen, zoals budgetten.

In dit artikel gebruikt u kostenanalyse te verkennen en analyseren van de kosten voor de organisatie. U kunt samengevoegde kosten weergeven door uw organisatie om te begrijpen waar de kosten worden opgebouwd en identificeren van trends in uitgaven. U weergeven samengevoegde kosten na verloop van tijd kunt u schatten maandelijks, elk kwartaal, of zelfs jaarlijks kostentrends ten opzichte van een budget. Een budget helpt om te voldoen aan financiële beperkingen of dagelijkse of maandelijkse kosten voor het isoleren van de bestedingslimiet onregelmatigheden weergeven. En u kunt downloaden van het huidige rapport gegevens voor verdere analyse, of in een extern systeem wilt gebruiken.

## <a name="requirements"></a>Vereisten

Kostenanalyse is beschikbaar voor alle Enterprise Agreement (EA)-klanten. U moet lezen-toegang hebben tot ten minste één van de volgende bereiken om gegevens van cost weer te geven.

- EA Azure billing account (inschrijving)
- Azure EA-abonnement
- Azure resourcegroep van de EA-abonnement

## <a name="review-costs-in-cost-analysis"></a>Kosten in kostenanalyse

Als u wilt uw kosten met kostenanalyse bekijken, opent u Azure portal en navigeer naar **kostenbeheer en facturering** &gt; **facturering accounts** &gt; uw EA account Factureringselecteren&gt; onder Cost Management, selecteer **Cost analysis**.

De initiële kosten analysis-weergave bevat de volgende gebieden:

**Totaal aantal** – ziet u de totale kosten voor de huidige maand.

**Budget** : toont de geplande uitgavenlimiet voor de geselecteerde scope, indien beschikbaar.

**Cumulatieve kosten** – geeft het totale aantal uitvoeren dagelijks uitgaven, vanaf het begin van de maand. Als u hebt [een budget gemaakt](billing-cost-management-budget-scenario.md#create-the-azure-budget) voor uw facturering account of -abonnement, kunt u snel uw uitgaventrend ten opzichte van het budget te zien. Beweeg de muisaanwijzer over een datum aan de totale kosten voor die dag.

**Draaitabel (ringdiagrammen)** : dynamische draaigrafieken bieden. Ze opsplitsen de totale kosten door een gemeenschappelijke set standaardeigenschappen. De grafieken geven de meest naar minst kosten betalen voor de huidige maand. U kunt grafieken op elk gewenst moment wijzigen door een andere draaigrafiek te selecteren. Kosten worden standaard opgedeeld per service (metercategorie), de locatie (regio) en onderliggend bereik (bijvoorbeeld inschrijvingsaccounts onder facturering accounts, resourcegroepen onder abonnementen en resources in resourcegroepen).

![Beginweergave van kostenanalyse](./media/billing-cost-analysis/cost-analysis-01.png)



## <a name="customizing-cost-views"></a>Kosten weergaven aanpassen

De standaardweergave biedt snelle antwoorden op veelgestelde vragen zoals:

- Hoeveel ik breng?
- Wordt ik binnen mijn budget blijven?

Er zijn echter veel gevallen waarin u meer gedetailleerde analyse. Aanpassing begint aan de bovenkant van de pagina met de Datumselectie.

Standaard toont kostenanalyse gegevens voor de huidige maand. Gebruik de kiezer datum snel overschakelen naar de laatste maand, deze maand, dit kwartaal, dit jaar agenda of een aangepast datumbereik van uw keuze. De afgelopen maand selecteren, is de snelste manier om te analyseren van uw laatste factuur voor Azure en eenvoudig in overeenstemming brengen kosten in rekening gebracht. Het huidige kwartaal en per jaar opties helpen de kosten op de langere termijn budgetten bijhouden. Of u kunt een meer verfijnde of bredere datumbereik selecteren van één dag in de afgelopen zeven dagen of alles tot een jaar vóór de huidige maand geleden.

![Selectie van datum](./media/billing-cost-analysis/date-selector.png)

Ook standaard kosten analysis toont **samengevoegde** kosten. Totale kosten omvatten alle kosten voor elke dag dat naast de voorgaande dagen, voor een voortdurend groeiende weergave van uw dagelijkse, worden de totale kosten. In deze weergave is geoptimaliseerd om weer te geven hoe u trends op basis van een budget voor het geselecteerde tijdsbereik.

Er is ook de **dagelijkse** weergeven. Er worden kosten weergegeven voor elke dag en een van de groeitrend niet weergeven. De dagelijkse weergave is geoptimaliseerd om onregelmatigheden als kosten piek weergeven of dip van dag tot dag. Wanneer u een budget selecteert, wordt ook een schatting van hoe uw dagelijkse budget eruit kan zien in de dagelijkse weergave bevat. Als uw dagelijkse kosten consistent meer dan de geschatte dagelijkse budget, en vervolgens u verwachten kunt dat gaat u uw maandelijkse budget overschrijdt. De geschatte dagelijkse budget is gewoon een manier om u te helpen u uw budget op een lager niveau visualiseren. Wanneer u schommelingen in dagelijkse kosten hebt, klikt u vervolgens is de geschatte dagelijkse budgetvergelijking aan uw maandelijkse budget minder nauwkeurig.

![Dagelijkse weergeven](./media/billing-cost-analysis/daily-view.png)

U kunt **groeperen op** een categorie groep wijzigen van gegevens die worden weergegeven in het bovenste gedeelte van de totale-grafiek te selecteren. Groepering kunt u snel zien hoe uw uitgaven wordt gecategoriseerd door resourcetype. Hier volgt een overzicht van de kosten voor de weergave van een laatste maand Azure-service.

![Gegroepeerde dagelijkse samengevoegde weergave](./media/billing-cost-analysis/grouped-daily-accum-view.png)

Grafieken zijn onder de bovenste totale weergave. Gebruik ze om te zien van de weergaven voor verschillende groeperen en filteren van categorieën. Wanneer u een categorie selecteert, wordt de volledige set gegevens voor totale weergave aan de onderkant van de weergave. Hier volgt een voorbeeld van resourcegroepen.

![Volledige gegevens voor de huidige weergave](./media/billing-cost-analysis/full-data-set.png)

### <a name="download-cost-analysis-data"></a>Gegevens van cost analysis downloaden

Wanneer u **downloaden** informatie uit analysis kosten, wordt een CSV-bestand gegenereerd voor alle gegevens op dit moment wordt weergegeven in de Azure-portal. Als u alle filters of groepering hebt toegepast, zijn klikt u vervolgens ze opgenomen in het bestand. Sommige onderliggende gegevens voor de bovenste totale grafiek die actief wordt niet in de portal weergegeven is ook opgenomen in het CSV-bestand.

## <a name="next-steps"></a>Volgende stappen

Weer andere [Azure-facturering en documentatie over het beheer van kosten](billing-cost-management-budget-scenario.md).

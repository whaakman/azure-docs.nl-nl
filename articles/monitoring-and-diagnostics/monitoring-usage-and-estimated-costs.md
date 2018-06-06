---
title: Bewaking van gebruik en de geschatte kosten in Azure Monitor | Microsoft Docs
description: Overzicht van het proces van het gebruik van Azure Monitor gebruiks- en de geschatte kosten van pagina
author: dalekoetke
manager: carmonmills
editor: mrbullwinkle
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: Dale.Koetke;mbullwin
ms.openlocfilehash: 4e6b3a2e8769c6e7e93071aed27b81c87ae336ca
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34715554"
---
# <a name="monitoring-usage-and-estimated-costs"></a>Bewaking en de geschatte kosten

In de hub Monitor van de Azure portal, de **gebruik en de geschatte kosten** pagina wordt uitgelegd dat het gebruik van kernbewaking functies zoals [waarschuwingen, metrische gegevens, meldingen](https://azure.microsoft.com/pricing/details/monitor/), [Azure Log Analytics ](https://azure.microsoft.com/pricing/details/log-analytics/), en [Azure Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). Dit omvat ook gebruik van logboekanalyse die zijn aangeschaft via de inzichten en Analytics bieden voor klanten op de prijscategorie plannen April 2018 beschikbaar is.

Op deze pagina kunnen gebruikers bekijken hun Resourcegebruik voor de afgelopen 31 dagen geaggregeerd per abonnement. Inzoomen-modules weergeven trends in gebruik gedurende de periode 31 dagen. Grote hoeveelheden gegevens moet worden gecombineerd voor deze schatting, dus zorg geduld terwijl de pagina wordt geladen.

In dit voorbeeld ziet u bewaking gebruiks- en een schatting van de resulterende kosten:

![Gebruik en de geschatte kosten van het portal-schermafbeelding](./media/monitoring-usage-and-estimated-costs/001.png)

Selecteer de koppeling in de maandelijkse gebruiksgegevens kolom openen van een diagram dat aangeeft van trends in gebruik gedurende de afgelopen 31 dagen:

![Inbegrepen per knooppunt balk schermafbeelding van de grafiek](./media/monitoring-usage-and-estimated-costs/002.png)

Hier is een andere vergelijkbare gebruik en kosten samenvatting. Dit voorbeeld ziet een abonnement in het nieuwe April 2018 op basis van verbruik prijsmodel. Let op het ontbreken van een facturering op basis van een knooppunt. Gegevensopname en retentie voor logboekanalyse en Application Insights wordt nu een nieuwe algemene meter gerapporteerd.

![Gebruik en de geschatte kosten portal schermafbeelding - April 2018 prijzen](./media/monitoring-usage-and-estimated-costs/003.png)

## <a name="new-pricing-model"></a>Nieuw prijsmodel

In April 2018, een [nieuwe bewaking prijsmodel werd uitgebracht](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/).  Deze functies cloud-vriendelijk, op basis van verbruik prijzen. U betaalt alleen voor wat u, zonder verbintenissen op basis van een knooppunt gebruikt. Details van het nieuwe prijsmodel zijn beschikbaar voor [waarschuwingen, metrische gegevens, meldingen](https://azure.microsoft.com/pricing/details/monitor/), [logboekanalyse](https://azure.microsoft.com/pricing/details/log-analytics/) en [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/). 

Voor de voorbereiding op klanten logboekanalyse of Application Insights na 2 April 2018, is het nieuwe prijsmodel de enige optie. Voor klanten die al gebruikmaken van deze services, verplaatsen naar het nieuwe prijsmodel is optioneel.

## <a name="assessing-the-impact-of-the-new-pricing-model"></a>De gevolgen van het nieuwe prijsmodel beoordelen
Het nieuwe prijsmodel heeft verschillende effecten voor elke klant op basis van hun bewaking gebruikspatronen. Voor klanten die zijn met logboekanalyse of Application Insights voordat 2 April 2018, de **gebruik en de geschatte kosten** pagina in de Azure-Monitor maakt een schatting van eventuele wijzigingen in de kosten als ze naar het nieuwe prijsmodel. Het biedt de manier waarop een abonnement verplaatsen naar het nieuwe model. Voor de meeste klanten is het nieuwe prijsmodel voordeligste. Voor klanten met gebruikspatronen vooral hoog gegevens of in hogere kosten regio's, kan dit niet het geval zijn.

Om te zien van een schatting maken van uw kosten voor de abonnementen die u hebt gekozen op de **gebruik en de geschatte kosten** pagina, selecteert u de blauwe banner boven aan de pagina. Het is raadzaam te doen dit een abonnement op een tijdstip, omdat dit het niveau waarop het nieuwe prijsmodel kan worden vastgesteld.

![Gebruik en de geschatte kosten in nieuwe prijscategorie model-schermopname controleren](./media/monitoring-usage-and-estimated-costs/004.png)

De nieuwe pagina ziet u een vergelijkbare versie van de vorige pagina met een groen banner:

![Gebruik en de geschatte kosten in de huidige prijscategorie model-schermopname controleren](./media/monitoring-usage-and-estimated-costs/005.png)

De pagina bevat ook een andere set meters die met het nieuwe prijsmodel overeenkomen. Deze lijst is een voorbeeld:

- Inzicht en Analytics\Overage per knooppunt
- Inzicht en Analytics\Included per knooppunt
- Overschrijding toepassingsgegevens Insights\Basic
- Toepassingsgegevens Insights\Included

Het nieuwe prijsmodel geen opgenomen gegevens op basis van een knooppunt toewijzingen. Daarom deze gegevens opneemt meters worden gecombineerd tot een nieuwe algemene gegevens opname meter aangeroepen **gedeeld Services\Data opname**. 

Er is een andere wijziging gegevens ingenomen in logboekanalyse of Application Insights in regio's met hogere kosten. Gegevens voor deze gebieden hoge kosten worden weergegeven met de nieuwe landinstellingen meters. Een voorbeeld is **gegevensopname (ons West Central)**.

> [!NOTE]
> De geschatte kosten komen niet in de account-niveau per knooppunt rechten van het abonnement Operations Management Suite (OMS) factor per abonnement. Raadpleeg uw accountvertegenwoordiger in dat geval voor een diepgaandere bespreking van de nieuwe prijsmodel.

## <a name="new-pricing-model-and-operations-management-suite-subscription-entitlements"></a>Nieuwe prijzen model en rechten voor Operations Management Suite-abonnement

Klanten die zijn gekocht van Microsoft Operations Management Suite E1 en E2 komen in aanmerking voor de per knooppunt gegevens opname rechten voor [logboekanalyse](https://www.microsoft.com/cloud-platform/operations-management-suite) en [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-pricing#the-price-plans). Deze rechten voor Log Analytics-werkruimten of Application Insights-resources in een bepaald abonnement ontvangen: 

- Het abonnement prijsmodel moet blijven in het model van de pre-April 2018.
- Log Analytics-werkruimten moeten de "Per knooppunt (OMS)' prijscategorie gebruiken.
- Application Insights-resources moeten de prijzen plan 'onderneming' gebruiken.

Afhankelijk van het aantal knooppunten van het pakket dat uw organisatie heeft aangeschaft, enkele verplaatsen abonnementen op het nieuwe prijsmodel kunnen nuttig zijn, maar dit moet zorgvuldig gebeuren. In het algemeen is het raadzaam gewoon blijven in de pre-April 2018 model zoals hierboven is beschreven.

> [!WARNING]
> Als uw organisatie heeft aangeschaft voor de Microsoft Operations Management Suite E1 en E2, is het beste aan uw abonnementen in de pre-April prijsmodel voor 2018 behouden. 
>

## <a name="changes-when-youre-moving-to-the-new-pricing-model"></a>Verandert wanneer u naar het nieuwe prijsmodel verplaatsen wilt

Het nieuwe prijsmodel vereenvoudigt Log Analytics en Application Insights prijzen opties tot slechts één laag (of -abonnement). Het verplaatsen van een abonnement naar het nieuwe prijscategorie model wordt:

- De prijscategorie voor elke logboekanalyse wijzigen in een nieuwe Per GB-laag (genaamd 'pergb2018' in Azure Resource Manager)
- Een Application Insights-resources in het plan Enterprise wordt gewijzigd naar het basisniveau.

De schatting van de kosten worden de effecten van deze wijzigingen.

> [!WARNING]
> Belangrijke opmerking als u Azure Resource Manager of PowerShell gebruiken voor het implementeren van hier [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-template-workspace-configuration) of [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-powershell) prijzen in een abonnement dat u hebt verplaatst naar het nieuwe model. Als u een laag/prijsstelling dan de 'pergb2018' voor logboekanalyse of 'Basic' voor Application Insights opgeeft, plaats mislukt de implementatie vanwege een ongeldige prijzen laag/plan, geven deze slaagt **maar alleen het geldige wordt gebruikt laag/plan prijzen** (dit geldt niet voor de Log Analytics gratis laag waar een ongeldig prijscategorie laag bericht wordt gegenereerd).
>

## <a name="moving-to-the-new-pricing-model"></a>Verplaatsen naar het nieuwe prijsmodel

Als u hebt besloten vast te stellen de nieuw prijsmodel voor een abonnement, selecteert u de **prijzen modelselectie** optie boven aan de **gebruik en de geschatte kosten** pagina:

![Gebruik en de geschatte kosten in nieuwe prijscategorie model-schermopname controleren](./media/monitoring-usage-and-estimated-costs/006.png)

De **modelselectie prijzen** pagina wordt geopend. Het bevat een overzicht van elk van de abonnementen die u op de vorige pagina weergegeven:

![Schermafbeelding van de selectie prijscategorie model](./media/monitoring-usage-and-estimated-costs/007.png)

Selecteer het selectievakje in om een abonnement naar het nieuwe prijsmodel verplaatst, en selecteer vervolgens **opslaan**. U kunt verplaatsen terug naar het oudere prijsmodel op dezelfde manier. Houd er rekening mee dat eigenaar van het abonnement of Inzender-rechten zijn nodig voor het wijzigen van het prijsmodel.

## <a name="automate-moving-to-the-new-pricing-model"></a>Verplaatsen naar het nieuwe prijsmodel automatiseren

De onderstaande scripts moeten de Azure PowerShell-Module. Om te controleren als u de nieuwste versie hebt, Zie [Installeer Azure PowerShell-module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.1.0).

Zodra u de nieuwste versie van Azure PowerShell hebt, dan moet u eerst om uit te voeren ``Connect-AzureRmAccount``.

``` PowerShell
# To check if your subscription is eligible to adjust pricing models.
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Een resultaat True onder isGrandFatherableSubscription geeft aan dat prijsmodel voor dit abonnement kan worden verplaatst tussen prijsmodellen. Het ontbreken van een waarde onder optedInDate betekent dat dit abonnement is momenteel ingesteld op het oude prijsmodel.

```
isGrandFatherableSubscription optedInDate
----------------------------- -----------
                         True            
```

Dit abonnement migreren naar het nieuwe prijsmodel uitvoeren:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
```

Om te bevestigen dat de wijziging geslaagd opnieuw uitvoeren is:

```PowerShell
$ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
```

Als de migratie voltooid is, moet nu uw resultaat eruitzien als:

```
isGrandFatherableSubscription optedInDate                      
----------------------------- -----------                      
                         True 2018-05-31T13:52:43.3592081+00:00
```

De optInDate bevat nu een tijdstempel van wanneer dit abonnement heeft gekozen voor het nieuwe prijsmodel.

Als u terugkeren naar het oude prijsmodel wilt, moet u uitvoeren:

```PowerShell
 $ResourceID ="/subscriptions/<Subscription-ID-Here>/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action rollbacktolegacypricingmodel `
 -Force
```

Als u de vorige script met vervolgens opnieuw ``-Action listmigrationdate``, ziet u nu een lege optedInDate-waarde die aangeeft uw abonnement is hersteld in de legacy model prijzen.

Als u meerdere abonnementen, die u wilt migreren die worden gehost onder dezelfde tenant hebt, kunt u uw eigen variant met behulp van de onderdelen van de volgende scripts maken:

```PowerShell
#Query tenant and create an array comprised of all of your tenants subscription ids
$TenantId = <Your-tenant-id>
$Tenant =Get-AzureRMSubscription -TenantId $TenantId
$Subscriptions = $Tenant.Id
```

Als u wilt controleren of de abonnementen in uw tenant in aanmerking komen voor het nieuwe prijsmodel zijn, kunt u het volgende uitvoeren:

```PowerShell
Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force
}
```

Het script kan worden verfijnd verdere door het maken van een script dat drie matrices genereert. Één matrix die bestaat uit alle abonnement-id's waarvoor ```isGrandFatherableSubscription``` ingesteld op True en optedInDate momenteel geen waarde hebben. Een tweede matrix van abonnementen die momenteel op het nieuwe prijsmodel. En een derde matrix ingevuld met de abonnement-id's in uw tenant die niet in aanmerking komen voor het nieuwe prijsmodel:

```PowerShell
[System.Collections.ArrayList]$Eligible= @{}
[System.Collections.ArrayList]$NewPricingEnabled = @{}
[System.Collections.ArrayList]$NotEligible = @{}

Foreach ($id in $Subscriptions)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
$Result= Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action listmigrationdate `
 -Force

     if ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $False)
     {
     $Eligible.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $True -and [bool]$Result.optedInDate -eq $True)
     {
     $NewPricingEnabled.Add($id)
     }

     elseif ($Result.isGrandFatherableSubscription -eq $False)
     {
     $NotEligible.add($id)
     }
}
```

> [!NOTE]
> Afhankelijk van het aantal abonnementen kan het bovenstaande script even duren om uit te voeren. Het PowerShell-venster echo vanwege het gebruik van de methode .add() stijgende waarden toe als items worden toegevoegd aan elke array.

Nu dat u uw abonnementen is onderverdeeld in drie matrices hebt zorgvuldig u de resultaten. U kunt een back-up van de inhoud van de matrices maken zodat u gemakkelijk kunt terugvallen uw wijzigingen u moet in de toekomst. Als u had besloten, die u wilt converteren van alle in aanmerking komende abonnementen die momenteel op het oude prijsmodel naar de nieuwe model die met deze taak kan nu worden bewerkstelligd prijzen zijn met:

```PowerShell
Foreach ($id in $Eligible)
{
$ResourceID ="/subscriptions/$id/providers/microsoft.insights"
Invoke-AzureRmResourceAction `
 -ResourceId $ResourceID `
 -ApiVersion "2017-10-01" `
 -Action migratetonewpricingmodel `
 -Force
}

```
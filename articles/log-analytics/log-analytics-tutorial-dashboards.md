---
title: Maken en delen van dashboards van gegevens van de Azure Log Analytics | Microsoft Docs
description: Deze zelfstudie helpt u begrijpen hoe logboekanalyse dashboards Visualiseer al uw zoekopdrachten opgeslagen logboek zodat u een identiteitsonderdelen om weer te geven van uw omgeving.
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 272945134b534a5ded794379ce5e96b0902a4227
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Maken en dashboards van logboekanalyse gegevens delen

Log Analytics dashboards kunnen Visualiseer al uw zoekopdrachten opgeslagen logboek zodat u de mogelijkheid om te zoeken, correleren en delen van IT operationele gegevens in de organisatie.  Deze zelfstudie bevat informatie over het maken van een zoekopdracht logboek die wordt gebruikt ter ondersteuning van een gedeelde dashboard dat wordt geopend door uw IT-team bewerkingen ondersteuning.  Procedures voor:

> [!div class="checklist"]
> * Een gedeelde dashboard maken in de Azure portal
> * Een zoekopdracht op prestaties logboek visualiseren 
> * Een zoekactie van het logboek toevoegen aan een gedeelde dashboard 
> * Een tegel in een gedeelde dashboard aanpassen

Als u het voorbeeld in deze zelfstudie, moet u een bestaande virtuele machine hebben [verbonden met de werkruimte voor logboekanalyse](log-analytics-quick-collect-azurevm.md).  
 
## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure portal
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-shared-dashboard"></a>Een gedeelde dashboard maken

Het eerste wat u ziet nadat u zich aanmeldt bij de Microsoft Azure portal is een [dashboard](../azure-portal/azure-portal-dashboards.md).<br> ![Azure-portaldashboard](media/log-analytics-tutorial-dashboards/log-analytics-portal-dashboard.png)

Hier kunt u overbrengen samen operationele gegevens die is zeer belangrijk voor IT over alle Azure-resources, met inbegrip van telemetrie van Azure-logboekanalyse.  Voordat we stap in een logboek zoekopdracht visualiseren, laten we eerst een dashboard maken en delen.  Deze manier kunnen wij de manier krijgen voordat we onze voorbeeld prestaties logboek zoeken, die worden weergegeven als een lijndiagram en toe te voegen aan het dashboard.  

Als u een dashboard, selecteert de **nieuwe dashboard** knop naast de naam van het dashboard.<br> ![Nieuw dashboard maken in Azure portal](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-01.png)

Deze actie wordt een nieuw, leeg, persoonlijke dashboard gemaakt en wordt u aanpassing modus kunt u uw dashboard name en toevoegen of tegels opnieuw rangschikken. De naam van het dashboard bewerken en geef *voorbeelddashboard* voor deze zelfstudie en selecteer vervolgens **gedaan aanpassen**.<br><br> ![Opslaan van aangepaste Azure-dashboard](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-02.png)

Wanneer u een dashboard maakt, is het persoonlijke standaard, wat betekent dat u de enige bent die deze kunnen zien. Als u deze zichtbaar voor anderen, gebruikt u de **Share** knop die wordt weergegeven naast de andere opdrachten in het dashboard.<br> ![Delen van een nieuw dashboard in Azure-portal](media/log-analytics-tutorial-dashboards/log-analytics-share-dashboard.png) 

U wordt gevraagd een abonnement en resourcegroep voor uw dashboard worden gepubliceerd om te kiezen. Voor het gemak ervaring handleidingen u naar een patroon waar u dashboards in een resourcegroep plaatsen opgeroepen het publiceren van de portal **dashboards**.  Controleer of het geselecteerde abonnement en klik vervolgens op **publiceren**.  Toegang tot de informatie die wordt weergegeven in het dashboard wordt gecontroleerd met [Azure Resource-gebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md).   

## <a name="visualize-a-log-search"></a>Een zoekopdracht logboek visualiseren

U kunt eenvoudige query's op één regel vanuit de portal logboek zoeken in de Azure portal maken. De portal logboek zoeken kan worden gebruikt zonder het starten van een externe portal en kunt u het uitvoeren van een breed scala aan functies met logboek zoekopdrachten inclusief maken van regels voor waarschuwingen, computergroepen maken en exporteren van de resultaten van de query. 

De [geavanceerde analyses portal](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-the-Analytics-portal) is een speciale portal geavanceerde functionaliteit niet beschikbaar in de portal logboek zoeken. Het onderdeel biedt de mogelijkheid om te bewerken van een query op meerdere regels, voert u selectief code, afhankelijk van de context Intellisense en slimme Analytics. In de portal Advanced Analytics, wordt u een Prestatieweergave grafisch maken, opslaan voor toekomstige zoeken en vastmaken aan het gedeelde dashboard eerder hebt gemaakt.   

U start de Advanced Analytics-portal via een koppeling in de portal logboek zoeken.<br> ![Start de Advanced Analytics-portal](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-01.png)

Voer de volgende query om terug te keren alleen processor gebruik records voor Windows- en Linux-computers, gegroepeerd op de Computer en TimeGenerated en weergegeven in een visual grafiek in de portal Analytics:

```
Perf | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1m), Computer | render timechart
```

Opslaan van de query door de **opslaan query** knop van de rechterbovenhoek.<br> ![Query opslaan van Advanced Analytics-portal](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-02.png)<br><br> In de **Query opslaan** Configuratiescherm, Geef een naam, zoals *Azure Virtual machines - processorgebruik* en klik vervolgens op **opslaan**.  Op deze manier kunt u een bibliotheek met algemene query's te zoeken met en zonder te hoeven volledig opnieuw schrijven wijzigen.  Dit ten slotte vastmaken aan het gedeelde dashboard eerder hebt gemaakt door het selecteren van **pincode grafiek voor uw Azure-dashboard** knop van de middelste rechtsboven aan de pagina.  

Nu dat we een query vastgemaakt aan het dashboard hebben, zult u er een algemene naam en een opmerking hieronder.<br> ![Voorbeeld van de Azure-dashboard](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-01.png)<br><br>  Er moet de naam van het herkenbare die gemakkelijk kan worden begrepen door weergeven.  Met de rechtermuisknop op de tegel en selecteer **bewerken tegel**.  Wanneer u klaar bent met de titel en de subtitel van de tegel aanpassen, klikt u op **Update**.  Een banner wordt weergegeven waarin u wijzigingen publiceren of negeren.  Klik op **wijzigingen publiceren** en sluit vervolgens de **tegel bewerken** besturingselementvenster.  

![Voltooide configuratie van voorbeelddashboard](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een dashboard in de Azure portal maken en toevoegen van een zoekopdracht logboek met.  Voorafgaande aan de volgende zelfstudie voor meer informatie over de verschillende antwoorden die u kunt implementeren, gebaseerd op logboek zoekresultaten.  

> [!div class="nextstepaction"]
> [Reageer op gebeurtenissen met Log Analytics waarschuwingen](log-analytics-tutorial-response.md)
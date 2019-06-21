---
title: Dashboards van Azure Log Analytics-gegevens maken en delen | Microsoft Docs
description: In deze zelfstudie helpt u begrijpen hoe Log Analytics-dashboards visualiseren al uw opgeslagen logboeken-query's, zodat u een één lens naar uw omgeving.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 93cda8680bc665055d449e86c24d6565f6fc525f
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296459"
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Dashboards van Log Analytics-gegevens maken en delen

Log Analytics dashboards kunnen visualiseren al uw opgeslagen logboeken-query's, zodat u de mogelijkheid om te zoeken, correleren en operationele IT-gegevens in de organisatie delen.  In deze zelfstudie bevat informatie over het maken van een logboekquery die wordt gebruikt ter ondersteuning van een gedeeld dashboard dat wordt gebruikt door het ondersteuningsteam van uw IT-bewerkingen.  In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een gedeeld dashboard maken in de Azure-portal
> * Een query voor prestaties visualiseren 
> * Query voor een toevoegen aan een gedeeld dashboard 
> * Een tegel in een gedeeld dashboard aanpassen

Om het voorbeeld in deze zelfstudie uit te voeren, moet u een bestaande virtuele machine [hebben verbonden met de Log Analytics-werkruimte](quick-collect-azurevm.md).  
 
## <a name="sign-in-to-azure-portal"></a>Meld u aan bij Azure Portal
Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-shared-dashboard"></a>Een gedeeld dashboard maken
Selecteer **Dashboard** openen van de standaard [dashboard](../../azure-portal/azure-portal-dashboards.md). Uw dashboard ziet er anders dan in het volgende voorbeeld.

![Azure portal-dashboard](media/tutorial-logs-dashboards/log-analytics-portal-dashboard.png)

Hier verzamelt u de belangrijkste operationele IT-gegevens van al uw Azure-resources, waaronder telemetrie van Azure Log Analytics.  Voordat een query voor visualiseren, laten we eerst een dashboard maken en delen.  We kunnen vervolgens concentreren op onze logboekquery voorbeeld van de prestaties, die wordt weergegeven als een lijndiagram en toevoegen aan het dashboard.  

Om een dashboard te maken, selecteert u de knop **Nieuw dashboard** naast de naam van het huidige dashboard.

![Nieuw dashboard maken in Azure portal](media/tutorial-logs-dashboards/log-analytics-create-dashboard-01.png)

Met deze actie wordt een nieuw, leeg, persoonlijk dashboard gemaakt en in de aanpassingsmodus gezet. In deze modus kunt u het dashboard een naam geven en tegels toevoegen of opnieuw rangschikken. De naam van het dashboard bewerken en geef *voorbeelddashboard* voor deze zelfstudie, en selecteer vervolgens **aanpassen voltooid**.<br><br> ![Aangepast Azure-dashboard opslaan](media/tutorial-logs-dashboards/log-analytics-create-dashboard-02.png)

Wanneer u een dashboard maakt, is het standaard persoonlijk. Dat betekent dat u de enige bent die het kan zien. Als u het zichtbaar wilt maken voor anderen, gebruik dan de knop **Delen** die naast de andere dashboardopdrachten wordt weergegeven.

![Delen van een nieuw dashboard in Azure portal](media/tutorial-logs-dashboards/log-analytics-share-dashboard.png) 

U wordt gevraagd een abonnement en resourcegroep te kiezen waarnaar uw dashboard zal worden gepubliceerd. Voor het gemak helpt de publicatie-ervaring van de portal u naar een patroon waar u dashboards kunt plaatsen in een resourcegroep die **dashboards** wordt genoemd.  Controleer het geselecteerde abonnement en klik op **Publiceren**.  Toegang tot de informatie die wordt weergegeven in het dashboard wordt geregeld met op [Azure-resources gebaseerd toegangsbeheer](../../role-based-access-control/role-assignments-portal.md).   

## <a name="visualize-a-log-query"></a>Een query voor visualiseren
[Log Analytics](../log-query/get-started-portal.md) is een speciale portal gebruikt om te werken met Logboeken-query's en de resultaten ervan. Voorbeelden van functies zijn de mogelijkheid om een query van meerdere regels te bewerken, code selectief uit te voeren, contextafhankelijke Intellisense en slimme analyse. In deze zelfstudie gebruikt u Log Analytics voor het maken van een prestatieweergave in grafische vorm, voor een toekomstige query, opslaan en vastmaken aan het gedeelde dashboard eerder hebt gemaakt.

Log Analytics openen door te selecteren **logboeken** in het menu van Azure Monitor. Het begint al met een nieuwe lege query.

![Startpagina](media/tutorial-logs-dashboards/homepage.png)

Voer de volgende query uit om terug te keren processor processorgebruikrecords voor zowel Windows als Linux-computers, gegroepeerd op Computer en TimeGenerated, en weergegeven in een visuele grafiek. Klik op **uitvoeren** uitvoeren van de query en het resulterende diagram weergeven.

```Kusto
Perf 
| where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" 
| summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1hr), Computer 
| render timechart
```

Opslaan van de query door de **opslaan** knop vanaf de bovenkant van de pagina.

![Query opslaan](media/tutorial-logs-dashboards/save-query.png)

In de **Query opslaan** Configuratiescherm, Geef een naam, zoals *Azure VM's - processorgebruik* en een categorie zoals *Dashboards* en klik vervolgens op **opslaan** .  Op deze manier kunt u een bibliotheek met algemene query's die u gebruikt en wijzigen.  Ten slotte deze vast aan het gedeelde dashboard eerder hebt gemaakt door het selecteren van de **pincode** knop in de rechterbovenhoek van de pagina en selecteer vervolgens de naam van het dashboard.

Nu u een query hebt die is vastgemaakt aan het dashboard, ziet u dat deze een algemene naam heeft met een opmerking eronder.

![Voorbeeld van Azure-dashboard](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-01.png)

 U gaat de naam wijzigen in iets met meer betekenis voor gebruikers die de grafiek willen bekijken.  Klik op de knop bewerken voor het aanpassen van de titel en subtitel van de tegel en klik vervolgens op **Update**.  Er wordt een banner weergegeven waarin u wordt gevraagd of u de wijzigingen wilt publiceren of negeren.  Klik op **een kopie opslaan**.  

![Voltooide configuratie van voorbeelddashboard](media/tutorial-logs-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een dashboard maken in Azure portal en een query voor aan toe te voegen.  Ga verder met de volgende zelfstudie voor meer informatie over de verschillende antwoorden die u kunt implementeren op basis van log queryresultaten.  

> [!div class="nextstepaction"]
> [Reageren op gebeurtenissen met Log Analytics-waarschuwingen](tutorial-response.md)

---
title: Dashboards van Azure Log Analytics-gegevens maken en delen | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Log Analytics-dashboards al uw opgeslagen logboekzoekopdrachten kunt visualiseren, zodat u door één lens naar uw omgeving kunt kijken.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: 2c7c1e71830eb3fe4f7240195dac828ac9dfed3a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951558"
---
# <a name="create-and-share-dashboards-of-log-analytics-data"></a>Dashboards van Log Analytics-gegevens maken en delen

Met Log Analytics-dashboards kunt u al uw opgeslagen logboekzoekopdrachten visualiseren, zodat u operationele IT-gegevens in de organisatie kunt zoeken, met elkaar in verband kunt brengen en kunt delen.  Deze zelfstudie bevat informatie over het maken van een logboekzoekopdracht die wordt gebruikt ter ondersteuning van een gedeeld dashboard waartoe uw IT-team toegang heeft.  In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een gedeeld dashboard maken in de Azure-portal
> * Een zoekopdracht voor het prestatielogboek visualiseren 
> * Een zoekactie toevoegen aan een gedeeld dashboard 
> * Een tegel in een gedeeld dashboard aanpassen

Om het voorbeeld in deze zelfstudie uit te voeren, moet u een bestaande virtuele machine [hebben verbonden met de Log Analytics-werkruimte](log-analytics-quick-collect-azurevm.md).  
 
## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-shared-dashboard"></a>Een gedeeld dashboard maken

Het eerste wat u ziet nadat u zich hebt aangemeld bij Microsoft Azure Portal is een [dashboard](../azure-portal/azure-portal-dashboards.md).<br> ![Azure Portal-dashboard](media/log-analytics-tutorial-dashboards/log-analytics-portal-dashboard.png)

Hier verzamelt u de belangrijkste operationele IT-gegevens van al uw Azure-resources, waaronder telemetrie van Azure Log Analytics.  Voordat u een logboekzoekopdracht gaat visualiseren, gaat u een dashboard maken en delen.  Dan is dat vast gedaan voordat u met de voorbeeldzoekopdracht voor het prestatielogboek aan de slag gaat. U gaat de gegevens weergeven als een lijndiagram en toevoegen aan het dashboard.  

Om een dashboard te maken, selecteert u de knop **Nieuw dashboard** naast de naam van het huidige dashboard.<br> ![Nieuw dashboard maken in Azure Portal](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-01.png)

Met deze actie wordt een nieuw, leeg, persoonlijk dashboard gemaakt en in de aanpassingsmodus gezet. In deze modus kunt u het dashboard een naam geven en tegels toevoegen of opnieuw rangschikken. Bewerk de naam van het dashboard en geef *Voorbeelddashboard* op voor deze zelfstudie. selecteer vervolgens **Aanpassen voltooid**.<br><br> ![Aangepast Azure-dashboard opslaan](media/log-analytics-tutorial-dashboards/log-analytics-create-dashboard-02.png)

Wanneer u een dashboard maakt, is het standaard persoonlijk. Dat betekent dat u de enige bent die het kan zien. Als u het zichtbaar wilt maken voor anderen, gebruik dan de knop **Delen** die naast de andere dashboardopdrachten wordt weergegeven.<br> ![Een nieuw dashboard in Azure Portal delen](media/log-analytics-tutorial-dashboards/log-analytics-share-dashboard.png) 

U wordt gevraagd een abonnement en resourcegroep te kiezen waarnaar uw dashboard zal worden gepubliceerd. Voor het gemak helpt de publicatie-ervaring van de portal u naar een patroon waar u dashboards kunt plaatsen in een resourcegroep die **dashboards** wordt genoemd.  Controleer het geselecteerde abonnement en klik op **Publiceren**.  Toegang tot de informatie die wordt weergegeven in het dashboard wordt geregeld met op [Azure-resources gebaseerd toegangsbeheer](../role-based-access-control/role-assignments-portal.md).   

## <a name="visualize-a-log-search"></a>Een logboekzoekopdracht visualiseren

Vanuit de portal Zoeken in logboeken van Azure Portal kunt u eenvoudige query's van één regel maken. U kunt de portal Zoeken in logboeken gebruiken zonder een externe portal te starten. Vanuit deze portal kunt u tal van functies met logboekzoekopdrachten uitvoeren. Zo kunt u waarschuwingsregels en computergroepen maken en de resultaten van de query exporteren. 

De [Log Analytics](../log-analytics/query-language/get-started-analytics-portal.md)-portal is een speciale portal die geavanceerde functionaliteit biedt die niet beschikbaar is in de portal Zoeken in logboeken. Voorbeelden van functies zijn de mogelijkheid om een query van meerdere regels te bewerken, code selectief uit te voeren, contextafhankelijke Intellisense en slimme analyse. In de portal Advanced Analytics maakt u een grafische prestatieweergave en slaat u deze op voor een toekomstige zoekopdracht. U maakt de weergave vast aan het gedeelde dashboard dat u eerder hebt gemaakt.   

U start de portal Advanced Analytics via een koppeling in de portal Zoeken in logboeken.<br> ![De portal Advanced Analytics starten](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-01.png)

Voer in de portal Advanced Analytics de volgende query uit om alleen records over processorverbruik te retourneren voor zowel Windows- als Linux-computers, gegroepeerd op Computer en TimeGenerated, en weergegeven in een visuele grafiek:

```
Perf | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1m), Computer | render timechart
```

Sla de query op door in de rechterbovenhoek de knop **Query opslaan** te selecteren.<br> ![De query opslaan in de portal Advanced Analytics](media/log-analytics-tutorial-dashboards/log-analytics-advancedportal-02.png)<br><br> Geef in het dialoogvenster **Query opslaan** een naam op, bijvoorbeeld *Azure-VM's - processorgebruik* en klik op **Opslaan**.  Op die manier kunt u een bibliotheek met veelgebruikte query's maken waarmee u kunt zoeken of die u kunt aanpassen zodat u de query's niet telkens opnieuw hoeft te schrijven.  Tot slot maakt u deze vast aan het gedeelde dashboard dat u eerder hebt gemaakt. Daarvoor selecteert u de knop **Grafiek vastmaken aan Azure-dashboard** in de rechterbovenhoek van de pagina.  

Nu u een query hebt die is vastgemaakt aan het dashboard, ziet u dat deze een algemene naam heeft met een opmerking eronder.<br> ![Voorbeeld van Azure-dashboard](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-01.png)<br><br>  U gaat de naam wijzigen in iets met meer betekenis voor gebruikers die de grafiek willen bekijken.  Klik met de rechtermuisknop op de tegel en selecteer **Tegel bewerken**.  Wanneer u de titel en subtitel van de tegel hebt aangepast, klikt u op **Bijwerken**.  Er wordt een banner weergegeven waarin u wordt gevraagd of u de wijzigingen wilt publiceren of negeren.  Klik op **Wijzigingen publiceren** en sluit het dialoogvenster **Tegel bewerken**.  

![Voltooide configuratie van voorbeelddashboard](media/log-analytics-tutorial-dashboards/log-analytics-modify-dashboard-02.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een dashboard maakt in Azure Portal en hoe u daar een logboekzoekopdracht aan toevoegt.  Ga verder met de volgende zelfstudie voor meer informatie over de verschillende reacties die u op basis van de resultaten van de logboekzoekopdracht kunt implementeren.  

> [!div class="nextstepaction"]
> [Reageren op gebeurtenissen met Log Analytics-waarschuwingen](log-analytics-tutorial-response.md)
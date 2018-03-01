---
title: Reageren op gebeurtenissen met Azure Log Analytics-waarschuwingen | Microsoft Docs
description: Deze zelfstudie biedt u meer inzicht in waarschuwingen in Log Analytics om belangrijke informatie te herkennen in de OMS-opslagplaats en u proactief te informeren over problemen of acties aan te roepen om te proberen deze problemen te corrigeren.
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
ms.date: 09/20/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: fcfaa849f67ffcfa69672d116837e96d318c2124
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Reageren op gebeurtenissen met Log Analytics-waarschuwingen
Waarschuwingen in Log Analytics geven belangrijke informatie in uw Log Analytics-opslagplaats aan. Ze worden gemaakt door waarschuwingsregels die automatisch met regelmatige tussenpozen zoekopdrachten in logboeken uitvoeren. Als de resultaten van de logboekzoekactie aan bepaalde criteria voldoen, wordt een waarschuwingsrecord gemaakt en kan dit worden geconfigureerd voor het uitvoeren van een automatische reactie.  Deze zelfstudie is een vervolg van de zelfstudie [Dashboards van Log Analytics-gegevens maken en delen](log-analytics-tutorial-dashboards.md).   

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een waarschuwingsregel maken
> * Een waarschuwingsregel configureren voor het verzenden van een e-mailmelding

Om het voorbeeld in deze zelfstudie uit te voeren, moet u een bestaande virtuele machine [hebben verbonden met de Log Analytics-werkruimte](log-analytics-quick-collect-azurevm.md).  

## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-alerts"></a>Waarschuwingen maken

Waarschuwingen worden gemaakt door waarschuwingsregels die automatisch met regelmatige tussenpozen zoekopdrachten in logboeken uitvoeren.  U kunt waarschuwingen maken op basis van specifieke prestatiemetrieken of wanneer bepaalde gebeurtenissen worden gemaakt, de afwezigheid van een gebeurtenis of een aantal gebeurtenissen die binnen een bepaalde periode worden gemaakt.  Waarschuwingen kunnen bijvoorbeeld worden gebruikt om u te waarschuwen wanneer het gemiddelde CPU-gebruik een bepaalde drempelwaarde overschrijdt of een gebeurtenis wordt gegenereerd wanneer een specifieke Windows-service of Linux-daemon niet wordt uitgevoerd.   Als de resultaten van de logboekzoekopdracht aan bepaalde criteria voldoen, wordt een waarschuwingsrecord gemaakt. De regel kan vervolgens automatisch een of meer acties uitvoeren om u proactief te informeren over de waarschuwing of een ander proces aanroepen. 

In het volgende voorbeeld maakt u een waarschuwingsregel voor een metrische meting waarmee een waarschuwing wordt gemaakt voor elk computerobject in de query met een waarde die hoger is dan de drempelwaarde van 90%.

1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
2. Start de OMS-portal door OMS-portal te selecteren en selecteer op de pagina **Overzicht** de optie **Zoeken in logboeken**.  
3. Selecteer boven in de portal **Favorieten** en selecteer in het dialoogvenster **Opgeslagen zoekopdrachten** aan de rechterkant de query *Azure VM's - processorgebruik*.  
4. Klik boven aan de pagina op **Waarschuwing** om het scherm **Waarschuwingsregel toevoegen** te openen.  
5. Configureer de waarschuwingsregel met de volgende informatie:  
   a. Geef een **Naam** op voor de waarschuwing, zoals *VM-processorgebruik overschrijdt > 90*  
   b. Geef voor **Tijdvenster** een tijdsbereik op voor de query, zoals *30*.  De query retourneert alleen de records die zijn gemaakt binnen dit tijdsbereik.  
   c. **Waarschuwingsfrequentie** geeft aan hoe vaak de query moet worden uitgevoerd.  Geef voor dit voorbeeld *5* minuten op, dat binnen ons opgegeven tijdvenster valt.  
   d. Selecteer **Meting van metrische gegevens** en voer *90* in voor **Geaggregeerde waarde** en voer *3* in voor **Waarschuwing activeren op basis van**   
   e. Schakel onder **Acties** e-mailmeldingen uit.
6. Klik op **Opslaan** om de waarschuwingsregel te voltooien. Deze wordt onmiddellijk uitgevoerd.<br><br> ![Voorbeeld van waarschuwingsregel](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Waarschuwingsrecords die worden gemaakt door waarschuwingsregels in Log Analytics zijn van het type **Waarschuwing** en hebben **OMS** als SourceSystem.<br><br> ![Voorbeeld van gegenereerde waarschuwingsgebeurtenissen](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Waarschuwingsacties
U kunt geavanceerde acties met waarschuwingen uitvoeren, zoals het maken van een e-mailmelding, het starten van een [Automation-runbook](../automation/automation-runbook-types.md), het gebruiken van een webhook voor het maken van een incidentrecord in uw ITSM-incidentbeheersysteem of het gebruik van de oplossing [IT Service Management-connector](log-analytics-itsmc-overview.md) als reactie wanneer aan de waarschuwingscriteria wordt voldaan.   

Met een e-mailactie wordt een e-mailbericht met details van de waarschuwing verzonden naar een of meer ontvangers. U kunt het onderwerp van het e-mailbericht opgeven, maar de inhoud ervan is een standaardindeling die wordt samengesteld door Log Analytics.  We gaan de waarschuwingsregel die u eerder hebt gemaakt bijwerken en deze configureren voor het verzenden van een e-mailmelding in plaats van actief te controleren op het waarschuwingsrecord met een logboekzoekopdracht.     

1. Selecteer in de OMS-portal in het bovenste menu de optie **Instellingen** en selecteer vervolgens **Waarschuwingen**.
2. Klik in de lijst met waarschuwingsregels op het potloodpictogram naast de waarschuwing die u eerder hebt gemaakt.
3. Schakel onder de sectie **Acties** e-mailmeldingen in.
4. Geef een **Onderwerp** op voor het e-mailbericht, zoals *Processorgebruik overschrijdt drempelwaarde > 90*.
5. Voeg adressen van een of meer ontvangers toe in het veld **Ontvangers**.  Als u meer dan één adres opgeeft, scheidt u de adressen van elkaar met een puntkomma (;).
6. Klik op **Opslaan** om de waarschuwingsregel te voltooien. Deze wordt onmiddellijk uitgevoerd.<br><br> ![Waarschuwingsregel met e-mailmelding](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe waarschuwingsregels een probleem proactief kunnen identificeren en kunnen reageren op een probleem wanneer deze met regelmatige tussenpozen logboekzoekopdrachten uitvoeren en aan bepaalde criteria voldoen.  

Volg deze link om voorbeelden te zien van vooraf gemaakte Log Analytics-scripts.  

> [!div class="nextstepaction"]
> [Voorbeelden van Log Analytics-scripts](powershell-samples.md)
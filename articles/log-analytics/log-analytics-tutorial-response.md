---
title: Reageer op gebeurtenissen die met Azure Log Analytics waarschuwingen | Microsoft Docs
description: Deze zelfstudie helpt u begrijpen waarschuwingen in logboekanalyse belangrijke informatie in de OMS-opslagplaats te identificeren en proactief zullen u informeren over problemen of acties uit om te proberen op te lossen ze worden aangeroepen.
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
ms.openlocfilehash: 3ab8d32eb4b3f2748249f40139de76c8e7f4d971
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Reageer op gebeurtenissen met Log Analytics waarschuwingen
Waarschuwingen in logboekanalyse identificeren belangrijke informatie in de opslagplaats Log Analytics.  Ze zijn gemaakt door regels voor waarschuwingen die automatisch logboek zoekopdrachten met regelmatige tussenpozen worden uitgevoerd en als de resultaten van de zoekactie logboek aan bepaalde criteria voldoen een waarschuwing record gemaakt en kan worden geconfigureerd voor het uitvoeren van een geautomatiseerd antwoord.  Deze zelfstudie is een vervolg van de de [dashboards maken en delen van gegevens van logboekanalyse](log-analytics-tutorial-dashboards.md) zelfstudie.   

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een waarschuwingsregel maken
> * Een regel voor het verzenden van een e-mailmelding voor waarschuwingen configureren

Als u het voorbeeld in deze zelfstudie, moet u een bestaande virtuele machine hebben [verbonden met de werkruimte voor logboekanalyse](log-analytics-quick-collect-azurevm.md).  

## <a name="log-in-to-azure-portal"></a>Aanmelden bij Azure portal
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-alerts"></a>Waarschuwingen maken

Waarschuwingen worden gemaakt door regels voor waarschuwingen die automatisch logboek zoekopdrachten met regelmatige tussenpozen worden uitgevoerd.  Kunt u waarschuwingen op basis van specifieke parameters of wanneer bepaalde gebeurtenissen worden gemaakt, afwezigheid van een gebeurtenis of een aantal gebeurtenissen worden gemaakt binnen een bepaalde periode.  Waarschuwingen kunnen bijvoorbeeld worden gebruikt om u te waarschuwen wanneer een bepaalde drempelwaarde voor gemiddelde CPU-gebruik overschrijdt of een gebeurtenis wordt gegenereerd wanneer een specifieke Windows-service of Linux-daemon wordt niet uitgevoerd.   Een waarschuwing record wordt gemaakt als de resultaten van de zoekopdracht logboek aan bepaalde criteria voldoen. De regel kan een of meer acties voor het proactief zullen u informeren over de waarschuwing of een ander proces aanroepen vervolgens automatisch uitgevoerd. 

In het volgende voorbeeld maken we een waarschuwingsregel metrische meting waarmee een waarschuwing voor elke computerobject wordt gemaakt in de query met een waarde die groter is dan een drempelwaarde 90%.

1. Klik in de Azure-portal op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**...
2. Start de OMS-portal door te selecteren van OMS-Portal en op de **overzicht** pagina **logboek zoeken**.  
3. Selecteer **Favorieten** vanaf de bovenkant van de portal en in de **opgeslagen zoekacties** rechterdeelvenster, selecteert u de query *Azure Virtual machines - processorgebruik*.  
4. Klik op **waarschuwing** boven aan de pagina opent de **waarschuwingsregel toevoegen** scherm.  
5. De waarschuwingsregel te configureren met de volgende informatie:  
   a. Geef een **naam** voor de waarschuwing, zoals *VM processorgebruik overschreden > 90*  
   b. Voor **tijdvenster**, Geef een periode voor de query, zoals *30*.  De query retourneert alleen de records die zijn gemaakt binnen dit bereik van de huidige tijd.  
   c. **Waarschuwing frequentie** geeft aan hoe vaak de query moet worden uitgevoerd.  Geef voor dit voorbeeld *5* minuten, die in onze opgegeven tijdvenster wordt uitgevoerd.  
   d. Selecteer **metriek meting** en voer *90* voor **waarde geaggregeerd** en voer *3* voor **Trigger waarschuwing op basis van**   
   e. Onder **acties**, e-mailmeldingen uitschakelen.
6. Klik op **opslaan** voltooien van de waarschuwingsregel. Begint onmiddellijk uitgevoerd.<br><br> ![Voorbeeld van de waarschuwingsregel](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Waarschuwing records gemaakt door regels voor waarschuwingen in Log Analytics is een Type **waarschuwing** en een SourceSystem van **OMS**.<br><br> ![Voorbeeld van een waarschuwing gebeurtenissen die worden gegenereerd](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Waarschuwingsacties
U kunt geavanceerde bewerkingen met waarschuwingen, zoals het maken van een e-mailmelding starten uitvoeren een [Automation-runbook](../automation/automation-runbook-types.md), een webhook gebruiken voor het maken van een incident-record in uw ITSM incidentbeheer systeem of met de [IT Service Management-Connector oplossing](log-analytics-itsmc-overview.md) als antwoord wanneer de waarschuwing criteria wordt voldaan.   

Een e-mailbericht met details van de waarschuwing verzenden acties e-mailbericht naar een of meer geadresseerden. Kunt u het onderwerp van het e-mailbericht, maar de inhoud is een standaardindeling samengesteld door logboekanalyse.  Laten we de waarschuwingsregel eerder hebt gemaakt en configureert u het e-updatemelding u in plaats van actief bewaking voor de waarschuwing record met een zoekopdracht logboek.     

1. In de OMS-portal op het bovenste menu selecteren **instellingen** en selecteer vervolgens **waarschuwingen**.
2. In de lijst met regels voor waarschuwingen, klikt u op het potloodpictogram naast de waarschuwing die eerder hebt gemaakt.
3. Onder **acties** sectie, e-mailmeldingen inschakelen.
4. Geef een **onderwerp** voor het e-mailbericht, zoals *Processor-gebruik overschrijdt drempelwaarde > 90*.
5. Toevoegen van adressen met een of meer e-mailadressen in de **ontvangers** veld.  Als u meer dan één adres opgeeft, scheidt u de adressen met een puntkomma (;).
6. Klik op **opslaan** voltooien van de waarschuwingsregel. Begint onmiddellijk uitgevoerd.<br><br> ![Waarschuwingsregel met e-mailmeldingen](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt geleerd hoe waarschuwing regels proactief kunnen identificeren en reageren op een probleem wanneer ze worden uitgevoerd logboek zoekopdrachten op intervallen gepland en aan een bepaalde criteria voldoen.  

Volg deze link om te zien van vooraf samengestelde logboekanalyse scriptvoorbeelden.  

> [!div class="nextstepaction"]
> [Meld u Analytics scriptvoorbeelden](powershell-samples.md)
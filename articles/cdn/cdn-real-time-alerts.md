---
title: Realtime waarschuwingen van Azure CDN | Microsoft Docs
description: Realtime-waarschuwingen in Microsoft Azure CDN. Realtime waarschuwingen bevatten meldingen over de prestaties van de eindpunten in uw CDN-profiel.
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 51dce1680be5f5f4387c2ba02827195bcdbe9b48
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Realtime-waarschuwingen in Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
Dit document wordt uitgelegd in Microsoft Azure CDN realtime-waarschuwingen. Deze functionaliteit biedt realtime meldingen over de prestaties van de eindpunten in uw CDN-profiel.  U kunt e-mailadres of op basis van HTTP-waarschuwingen instellen:

* Bandbreedte
* Statuscodes
* Cache-statussen
* Verbindingen

## <a name="creating-a-real-time-alert"></a>Maken van een realtime waarschuwing
1. In de [Azure-portal](https://portal.azure.com), blader naar uw CDN-profiel.
   
    ![CDN-profiel](./media/cdn-real-time-alerts/cdn-profile-blade.png)
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop CDN-profiel beheren](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    Hiermee opent u de CDN-beheerportal.
3. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **realtime statistieken** doel.  Klik op **realtime-waarschuwingen**.
   
    ![CDN-beheerportal](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    De lijst met bestaande waarschuwing configuraties (indien aanwezig) wordt weergegeven.
4. Klik op de **waarschuwing toevoegen** knop.
   
    ![Knop Waarschuwing toevoegen](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    Een formulier voor het maken van een nieuwe waarschuwing wordt weergegeven.
   
    ![Nieuwe waarschuwing formulier](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. Als u wilt dat deze waarschuwing geactiveerd wanneer u klikt op **opslaan**, Controleer de **waarschuwing ingeschakeld** selectievakje.
6. Voer een beschrijvende naam voor de waarschuwing in de **naam** veld.
7. In de **mediatype** vervolgkeuzelijst **HTTP Large Object**.
   
    ![Het mediatype met HTTP-Large Object geselecteerd](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > U moet selecteren **HTTP Large Object** als de **mediatype**.  De andere opties worden niet gebruikt door **Azure CDN van Verizon**.  Fout bij selecteren **HTTP Large Object** zorgt ervoor dat de waarschuwing nooit worden geactiveerd.
   > 
   > 
8. Maak een **expressie** controleren door te selecteren een **metriek**, **Operator**, en **activeren waarde**.
   
   * Voor **metriek**, selecteer het type van de gewenste bewaakte voorwaarde.  **Mbps bandbreedte** is de hoeveelheid bandbreedte in megabits per seconde.  **Totaal aantal verbindingen** is het aantal gelijktijdige HTTP-verbindingen naar onze randservers.  Zie voor definities van de verschillende cache statussen en statuscodes [Azure CDN Cache statuscodes](https://msdn.microsoft.com/library/mt759237.aspx) en [Azure CDN HTTP-statuscodes](https://msdn.microsoft.com/library/mt759238.aspx)
   * **Operator** is de rekenkundige operator die zorgt voor de relatie tussen de metrische gegevens en de trigger-waarde.
   * **Activeren van de waarde** is de drempelwaarde in die moet worden voldaan voordat een melding wordt verzonden.
     
     In het volgende voorbeeld wordt geeft de gemaakte expressie aan dat een melding wordt verzonden als het aantal 404 statuscodes groter dan 25 is.
     
     ![Realtime waarschuwing voorbeeldexpressie](./media/cdn-real-time-alerts/cdn-expression.png)
9. Voor **Interval**, opgeven hoe vaak u wilt de geëvalueerde expressie.
10. In de **meldingen op basis van** vervolgkeuzelijst, selecteer deze optie als u een melding krijgen wilt wanneer de expressie waar is.
    
    * **Start-voorwaarde** geeft aan dat er een melding wordt verzonden wanneer de opgegeven voorwaarde voor het eerst wordt gedetecteerd.
    * **End-voorwaarde** geeft aan dat er een melding wordt verzonden wanneer de opgegeven voorwaarde wordt niet meer gedetecteerd. Deze melding kan alleen worden geactiveerd nadat ons netwerk bewakingssysteem gedetecteerd dat de opgegeven voorwaarde opgetreden.
    * **Continue** geeft aan dat een melding wordt verzonden elke keer dat het netwerk bewakingssysteem de opgegeven voorwaarde detecteert. Houd er rekening mee dat het netwerk bewakingssysteem slechts één keer per interval voor de opgegeven voorwaarde controleert.
    * **Voorwaarde begin- en** geeft aan dat een melding wordt verzonden met de eerste keer dat de opgegeven voorwaarde wordt gedetecteerd en nog een keer als de voorwaarde is niet meer gedetecteerd.
1. Als u meldingen ontvangen via e-mail wilt, controleert u de **melden via E-mail** selectievakje.  
    
    ![Delen per E-mail formulier](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    In de **naar** en voer het e-mailadres dat u waar u meldingen verzonden. Voor **onderwerp** en **hoofdtekst**, mag u laat de standaardwaarde of kunt u aanpassen op het bericht met de **beschikbare trefwoorden** lijst dynamisch invoegen waarschuwingsgegevens wanneer het bericht Er wordt verzonden.
    
    > [!NOTE]
    > U kunt de e-mailmelding testen door te klikken op de **testen melding** knop, maar alleen als de configuratie van de waarschuwing is opgeslagen.
    > 
    > 
12. Als u wilt dat meldingen worden gepubliceerd op een webserver, controleert u de **hoogte door HTTP Post** selectievakje.
    
    ![Melden door HTTP Post-formulier](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    In de **Url** en voer de URL die u waar u het HTTP-bericht verzonden. In de **Headers** tekstvak de HTTP-headers worden verzonden in de aanvraag invoeren.  Voor **hoofdtekst**, kunt u het bericht aanpassen met behulp van de **beschikbare trefwoorden** lijst waarschuwingsgegevens dynamisch invoegen wanneer het bericht is verzonden.  **Headers** en **hoofdtekst** standaard op een XML-nettolading vergelijkbaar met het volgende voorbeeld:
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > U kunt de melding HTTP Post testen door te klikken op de **testen melding** knop, maar alleen als de configuratie van de waarschuwing is opgeslagen.
    > 
    > 
13. Klik op de **opslaan** om op te slaan van uw configuratie van de waarschuwing.  Als u dit selectievakje inschakelt **waarschuwing ingeschakeld** in stap 5, de waarschuwing is nu actief.

## <a name="next-steps"></a>Volgende stappen
* Analyseren [realtime statistieken in Azure CDN](cdn-real-time-stats.md)
* Meer gedetailleerde met dig [geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
* Analyseren [gebruikspatronen](cdn-analyze-usage-patterns.md)


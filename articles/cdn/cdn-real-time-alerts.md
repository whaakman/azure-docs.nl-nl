---
title: Realtime waarschuwingen van Azure CDN | Microsoft Docs
description: Realtime waarschuwingen in Microsoft Azure CDN. Realtime waarschuwingen geven meldingen over de prestaties van de eindpunten in uw CDN-profiel.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: 1e85b809-e1a9-4473-b835-69d1b4ed3393
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e20161147aa16456e31aff2bd3cc6337c3690e89
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917852"
---
# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Realtime waarschuwingen in Microsoft Azure CDN
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Overzicht
Dit document wordt uitgelegd realtime waarschuwingen in Microsoft Azure CDN. Deze functionaliteit biedt realtime meldingen over de prestaties van de eindpunten in uw CDN-profiel.  U kunt e-mailadres of op basis van HTTP-waarschuwingen instellen:

* Bandbreedte
* Statuscodes
* Statusbepaling van de cache
* Verbindingen

## <a name="creating-a-real-time-alert"></a>Het maken van een realtime waarschuwing
1. In de [Azure-portal](https://portal.azure.com), blader naar uw CDN-profiel.
   
    ![CDN-profiel](./media/cdn-real-time-alerts/cdn-profile-blade.png)
1. Klik in de blade CDN-profiel op de **beheren** knop.
   
    ![Knop voor CDN-profiel beheren](./media/cdn-real-time-alerts/cdn-manage-btn.png)
   
    De CDN-beheerportal wordt geopend.
3. Beweeg de muisaanwijzer over de **Analytics** tabblad en klik vervolgens Beweeg de muisaanwijzer over de **realtime statistieken** flyout.  Klik op **realtime waarschuwingen**.
   
    ![CDN-beheerportal](./media/cdn-real-time-alerts/cdn-premium-portal.png)
   
    De lijst met bestaande waarschuwing configuraties (indien aanwezig) wordt weergegeven.
4. Klik op de **waarschuwing toevoegen** knop.
   
    ![Knop Waarschuwing toevoegen](./media/cdn-real-time-alerts/cdn-add-alert.png)
   
    Een formulier voor het maken van een nieuwe waarschuwing wordt weergegeven.
   
    ![Nieuwe waarschuwing formulier](./media/cdn-real-time-alerts/cdn-new-alert.png)
5. Als u wilt dat deze waarschuwing geactiveerd wanneer u klikt op **opslaan**, Controleer de **waarschuwing ingeschakeld** selectievakje.
6. Voer een beschrijvende naam voor de waarschuwing in de **naam** veld.
7. In de **mediatype** vervolgkeuzelijst **HTTP Large Object**.
   
    ![Mediatype met HTTP-Large Object geselecteerd](./media/cdn-real-time-alerts/cdn-http-large.png)
   
   > [!IMPORTANT]
   > U moet selecteren **HTTP Large Object** als de **mediatype**.  Andere opties worden niet gebruikt door **Azure CDN van Verizon**.  Fout bij selecteren **HTTP Large Object** zorgt ervoor dat de waarschuwing nooit wordt geactiveerd.
   > 
   > 
8. Maakt een **expressie** om te controleren door het selecteren van een **metrische gegevens**, **Operator**, en **waarde activeren**.
   
   * Voor **Metric**, selecteer het type van de voorwaarde die u wilt dat bewaakte.  **Mbps bandbreedte** is de hoeveelheid bandbreedtegebruik in megabits per seconde.  **Totaal aantal verbindingen** is het aantal gelijktijdige HTTP-verbindingen met onze edge-servers.  Zie voor definities van de verschillende cachestatussen en statuscodes [statuscodes voor Azure CDN-Cache](/previous-versions/azure/mt759237(v=azure.100)) en [Azure CDN HTTP-statuscodes](/previous-versions/azure/mt759238(v=azure.100))
   * **Operator** is de rekenkundige operator die de relatie tussen de metrische gegevens en de trigger-waarde bepaalt.
   * **Waarde activeren** is de drempelwaarde-waarde die moet worden voldaan voordat een melding wordt verzonden.
     
     In het volgende voorbeeld wordt geeft de gemaakte expressie aan dat een melding wordt verzonden wanneer het aantal 404 statuscodes groter dan 25 is.
     
     ![Voorbeeld van een realtime waarschuwing expressie](./media/cdn-real-time-alerts/cdn-expression.png)
9. Voor **Interval**, opgeven hoe vaak u wilt de geëvalueerde expressie.
10. In de **op de hoogte stellen op** vervolgkeuzelijst, selecteer deze optie als u worden gewaarschuwd wilt wanneer de expressie waar is.
    
    * **Start-voorwaarde** geeft aan dat er een melding wordt verzonden wanneer de opgegeven voorwaarde voor het eerst wordt gedetecteerd.
    * **End-voorwaarde** geeft aan dat een melding wordt verzonden wanneer de opgegeven voorwaarde wordt niet meer gedetecteerd. Deze melding kan alleen worden geactiveerd nadat ons netwerk bewakingssysteem gedetecteerd dat de opgegeven voorwaarde is opgetreden.
    * **Continue** geeft aan dat een melding wordt verzonden elke keer dat het netwerk bewaken systeem de opgegeven voorwaarde detecteert. Houd er rekening mee dat het netwerk bewaken systeem slechts één keer per interval voor de opgegeven voorwaarde wordt gecontroleerd.
    * **Voorwaarde-begin- en** geeft aan dat een melding wordt verzonden met de eerste keer dat de opgegeven voorwaarde wordt gedetecteerd en opnieuw als de voorwaarde is niet meer gedetecteerd.
1. Als u meldingen ontvangen via e-mail wilt, controleert u de **waarschuwen via E-mail** selectievakje.  
    
    ![Een melding per E-mail formulier](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    In de **naar** en voer het e-mailadres dat u waar u meldingen verzonden. Voor **onderwerp** en **hoofdtekst**, mag u de standaardwaarde laten staan of kunt u aanpassen op het bericht met de **beschikbaar trefwoorden** lijst dynamisch invoegen waarschuwingsgegevens wanneer het bericht wordt verzonden.
    
    > [!NOTE]
    > U kunt de e-mailmelding testen door te klikken op de **testen melding** knop, maar alleen nadat de configuratie van de waarschuwing is opgeslagen.
    > 
    > 
12. Als u wilt dat meldingen worden geboekt met een webserver, controleert u de **melden door HTTP Post** selectievakje.
    
    ![Op de hoogte stellen via HTTP Post-formulier](./media/cdn-real-time-alerts/cdn-notify-http.png)
    
    In de **Url** en voer de URL die u waar u het HTTP-bericht geplaatst. In de **Headers** tekstvak de HTTP-headers worden verzonden in de aanvraag invoert.  Voor **hoofdtekst**, kunt u het bericht aanpassen met behulp van de **beschikbaar trefwoorden** lijst waarschuwingsgegevens dynamisch invoegen wanneer het bericht is verzonden.  **Headers** en **hoofdtekst** standaard ingesteld op een XML-nettolading die vergelijkbaar is met het volgende voorbeeld:
    
    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```
    
    > [!NOTE]
    > U kunt de HTTP Post-melding testen door te klikken op de **testen melding** knop, maar alleen nadat de configuratie van de waarschuwing is opgeslagen.
    > 
    > 
13. Klik op de **opslaan** om op te slaan van de configuratie van de waarschuwing.  Als u dit selectievakje inschakelt **waarschuwing ingeschakeld** in stap 5, de waarschuwing is nu actief.

## <a name="next-steps"></a>Volgende stappen
* Analyseren [realtime statistieken in Azure CDN](cdn-real-time-stats.md)
* Dieper graven met [geavanceerde HTTP-rapporten](cdn-advanced-http-reports.md)
* Analyseren [gebruikspatronen](cdn-analyze-usage-patterns.md)


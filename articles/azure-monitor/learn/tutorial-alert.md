---
title: Waarschuwingen verzenden vanuit Azure Application Insights | Microsoft Docs
description: Zelfstudie over het verzenden van waarschuwingen als reactie op fouten in uw toepassing met behulp van Azure Application Insights.
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 300f0ddc8b738b5fd8578ed0b33cc15000c1098a
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101782"
---
# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>Toepassingsstatus bewaken en waarschuwingen erover verzenden met Azure Application Insights

Met Azure Application Insights kunt u uw toepassing bewaken en waarschuwingen ontvangen wanneer deze niet beschikbaar is, wanneer er fouten optreden, of wanneer er sprake is van prestatieproblemen.  In deze zelfstudie wordt u begeleid bij het maken van tests voor het continu controleren van de beschikbaarheid van uw toepassing en het verzenden van verschillende soorten waarschuwingen als reactie op gedetecteerde problemen.  In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Beschikbaarheidstest maken voor het continu controleren van de reacties van de toepassing
> * E-mail verzenden naar beheerders wanneer zich een probleem voordoet
> * Waarschuwingen maken op basis van metrische prestatiegegevens 
> * Een logische app gebruiken om op schema samengevatte telemetrie te verzenden.


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - ASP.NET-ontwikkeling en webontwikkeling
    - Azure-ontwikkeling
    - Implementeer een .NET-toepassing in Azure en [schakel de Application Insights-SDK](../../azure-monitor/app/asp-net.md)in. 


## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-availability-test"></a>Beschikbaarheidstest maken
Met beschikbaarheidstests in Application Insights kunt u uw toepassing automatisch testen vanuit verschillende locaties over de hele wereld.   In deze zelfstudie voert u een eenvoudige test uit om te controleren of de toepassing beschikbaar is.  U kunt ook een volledig overzicht maken om de werking van de toepassing gedetailleerd te testen. 

1. Selecteer **Application Insights** en selecteer vervolgens uw abonnement.  
1. Selecteer in het menu **Onderzoeken** de optie **Beschikbaarheid**, en klik vervolgens op **Test toevoegen**.
 
    ![Beschikbaarheidstest toevoegen](media/tutorial-alert/add-test.png)

2. Typ een naam voor de test en laat de andere standaardinstellingen ongewijzigd.  Hiermee wordt de startpagina van de toepassing elke 5 minuten opgevraagd vanuit 5 verschillende geografische locaties. 
3. Selecteer **Waarschuwingen** om het paneel **Waarschuwingen** te openen. Hier kunt u de details voor de gewenste reactie definiëren, indien de test mislukt. Typ een e-mailadres waarnaar een melding moet worden verzonden, wanneer wordt voldaan aan de waarschuwingscriteria.  U kunt optioneel ook het adres van een webhook typen dat moet worden aangeroepen wanneer wordt voldaan aan de waarschuwingscriteria.

    ![Test maken](media/tutorial-alert/create-test.png)
 
4. Ga terug naar het testpaneel. Na enkele minuten beginnen de resultaten van de beschikbaarheidstest binnen te komen.  Klik op de testnaam om de details van elke locatie weer te geven.  In het spreidingsdiagram ziet u het slagen en de duur van elke test.

    ![Testdetails](media/tutorial-alert/test-details.png)

5.  U kunt inzoomen op de details van elke gewenste test door op de bijbehorende stip te klikken in het spreidingsdiagram.  In het onderstaande voorbeeld ziet u de details van een mislukte aanvraag.

    ![Testresultaat](media/tutorial-alert/test-result.png)
  
6. Als is voldaan aan waarschuwingscriteria, wordt een e-mailbericht zoals het onderstaande verzonden naar het adres dat u hebt opgegeven.

    ![Waarschuwings-e-mail](media/tutorial-alert/alert-mail.png)


## <a name="create-an-alert-from-metrics"></a>Een waarschuwing maken vanuit metrische gegevens
Naast het verzenden van waarschuwingen vanuit een beschikbaarheidstest, kunt u een waarschuwing maken vanuit metrische prestatiegegevens die worden verzameld voor de toepassing.

2. Selecteer in het menu **Configureren** de optie **Waarschuwingen**.  Hiermee wordt het paneel Azure-waarschuwingen geopend.  Mogelijk zijn hier andere waarschuwingsregels geconfigureerd voor andere services.
3. Klik op **Waarschuwing voor metrische gegevens toevoegen**.  Hiermee wordt het paneel geopend voor het maken van een nieuwe waarschuwingsregel.

    ![Metrische waarschuwing toevoegen](media/tutorial-alert/add-metric-alert.png)

4. Typ een **Naam** voor de waarschuwingsregel en selecteer uw toepassing in de vervolgkeuzelijst voor **Resource**.
5. Selecteer een **Metrisch gegeven** om een steekproef op uit te voeren.  Er wordt een grafiek weergegeven om de waarde van deze aanvraag in de afgelopen 24 uur aan te geven.  Dit helpt u om de voorwaarde voor dit metrisch gegeven in te stellen.

    ![Waarschuwingsregel toevoegen](media/tutorial-alert/add-alert-01.png)

6. Geen een **Voorwaarde** en **Drempelwaarde** op voor de waarschuwing. Dit is het aantal keer dat het metrische gegeven moet worden overschreden om een waarschuwing te activeren. 
6. Schakel onder **Melden via** het vak **Eigenaars, bijdragers en lezers van e-mail** in om een e-mailbericht naar deze gebruikers te verzenden wanneer wordt voldaan aan de waarschuwingscriteria, en voeg de e-mailadressen van eventuele extra ontvangers toe.  U kunt hier ook een webhook of logische app opgeven die wordt uitgevoerd wanneer wordt voldaan aan de voorwaarde.  Deze kunnen worden gebruikt om te proberen het gedetecteerde probleem te verhelpen of 

    ![Waarschuwingsregel toevoegen](media/tutorial-alert/add-alert-02.png)


## <a name="proactively-send-information"></a>Proactief informatie verzenden
Waarschuwingen worden gemaakt als reactie op een bepaalde set problemen die zijn geïdentificeerd in uw toepassing. Doorgaans gebruikt u waarschuwingen alleen bij kritieke voorwaarden waarvoor onmiddellijke aandacht is vereist.  U kunt proactief informatie ontvangen over de toepassing met behulp van een logische app die automatisch wordt uitgevoerd volgens een schema.  U kunt bijvoorbeeld dagelijks een e-mailbericht laten verzenden naar beheerders met daarin een informatieoverzicht over verdere evaluaties die zijn vereist.

Zie [Application Insights-processen automatiseren met behulp van Logic Apps](../../azure-monitor/app/automate-with-logic-apps.md) voor details over het maken van een logische app met Application Insights

## <a name="next-steps"></a>Volgende stappen
Nu u hebt geleerd hoe u waarschuwingen voor problemen kunt genereren, gaat u verder met de volgende zelfstudie waarin u leert hoe u hoe kunt analyseren hoe uw gebruikers de toepassing gebruiken.

> [!div class="nextstepaction"]
> [Gebruikers begrijpen](../../azure-monitor/learn/tutorial-users.md)
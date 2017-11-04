---
title: Meldingen verzenden vanuit Azure Application Insights | Microsoft Docs
description: Zelfstudie voor het verzenden van meldingen in antwoord op fouten in uw toepassing met behulp van Azure Application Insights.
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 39e2f136e30ebb6dcfc003c435382f3384af1052
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>Monitor en waarschuwing van toepassingsstatus met Azure Application Insights

Azure Application Insights kunt u uw toepassing bewaken en u waarschuwingen ontvangt wanneer het is niet beschikbaar is, ondervindt fouten of prestatieproblemen lijdt.  Deze zelfstudie leert u het proces voor het maken van de tests uit om de beschikbaarheid van uw toepassing continu controleren en om verschillende soorten waarschuwingen verzenden in antwoord op heeft problemen gedetecteerd.  Procedures voor:

> [!div class="checklist"]
> * Beschikbaarheidstest om te controleren continu het antwoord van de toepassing maken
> * E-mail verzonden naar beheerders wanneer er een probleem optreedt
> * Waarschuwingen op basis van maatstaven voor prestaties maken 
> * Gebruik een logische App samengevatte telemetrie verzenden volgens een schema.


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - ASP.NET-ontwikkeling en webontwikkeling
    - Azure-ontwikkeling
    - Een .NET-toepassing in Azure implementeert en [inschakelen van de Application Insights-SDK](app-insights-asp-net.md). 


## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-availability-test"></a>Beschikbaarheidstest maken
Beschikbaarheidstests in Application Insights kunnen u automatisch de toepassing te testen vanaf verschillende locaties over de hele wereld.   In deze zelfstudie voert u een eenvoudige test om ervoor te zorgen dat de toepassing beschikbaar is.  U kunt ook volledige procedure als u wilt testen, de gedetailleerde bewerking maken. 

1. Selecteer **Application Insights** en selecteer vervolgens uw abonnement.  
1. Selecteer **beschikbaarheid** onder de **onderzoeken** menu en klik vervolgens op **toevoegen test**.
 
    ![Beschikbaarheidstest toevoegen](media/app-insights-tutorial-alert/add-test.png)

2. Typ een naam voor de test en de andere standaardinstellingen laten staan.  Hiermee vraagt de startpagina van de toepassing om de 5 minuten van 5 verschillende geografische locaties. 
3. Selecteer **waarschuwingen** openen de **waarschuwingen** Configuratiescherm waarin u details voor informatie over het reageren als de test mislukt kunt definiëren. Type in een e-mailadres om te verzenden wanneer de waarschuwing criteria wordt voldaan.  U kan eventueel typt u het adres van een webhook aanroepen wanneer de waarschuwing criteria wordt voldaan.

    ![Test maken](media/app-insights-tutorial-alert/create-test.png)
 
4. Terug naar het paneel test en na een paar minuten moet u eerst de resultaten van de beschikbaarheidstest.  Klik op de naam van test voor meer informatie vanaf elke locatie.  De spreidingsdiagram geeft de geslaagde pogingen en de duur van elke test.

    ![Details van test](media/app-insights-tutorial-alert/test-details.png)

5.  U kunt inzoomen de details van een bepaalde test door te klikken op het punt in het spreidingsdiagram.  Het volgende voorbeeld ziet de details voor een mislukte aanvraag.

    ![Testresultaat](media/app-insights-tutorial-alert/test-result.png)
  
6. Als de waarschuwing criteria wordt voldaan, wordt een e-mail vergelijkbaar met hieronder verzonden naar het adres dat u hebt opgegeven.

    ![Waarschuwing mail](media/app-insights-tutorial-alert/alert-mail.png)


## <a name="create-an-alert-from-metrics"></a>Maken van een waarschuwing van metrische gegevens
Naast het verzenden van meldingen vanuit een beschikbaarheidstest, kunt u een waarschuwing van elk maatstaven voor prestaties die worden verzameld voor uw toepassing.

2. Selecteer **waarschuwingen** van de **configureren** menu.  Hiermee opent u het deelvenster waarschuwingen van Azure.  Er zijn andere regels voor waarschuwingen die hier zijn geconfigureerd voor andere services.
3. Klik op **metrische waarschuwing toevoegen**.  Hiermee opent u het deelvenster voor het maken van een nieuwe waarschuwingsregel.

    ![Metrische waarschuwing toevoegen](media/app-insights-tutorial-alert/add-metric-alert.png)

4. Typ in een **naam** voor de waarschuwing regel en selecteer uw toepassing in de vervolgkeuzelijst voor **Resource**.
5. Selecteer een **metriek** met het volgende voorbeeld.  Een grafiek wordt weergegeven om aan te geven van de waarde van deze aanvraag in de afgelopen 24 uur.  Dit helpt u bij het instellen van de voorwaarde voor de metriek.

    ![toevoegen van waarschuwingsregel](media/app-insights-tutorial-alert/add-alert-01.png)

6. Geef een **voorwaarde** en **drempelwaarde** voor de waarschuwing. Dit is het aantal keren dat de meetwaarde moet worden overschreden voor een waarschuwing moet worden gemaakt. 
6. Onder **melden** controleren de **e-eigenaren, bijdragers en lezers** vak een e-mail verzenden naar deze gebruikers wanneer de waarschuwing voorwaarde wordt voldaan en de e-mailadres van meer geadresseerden toevoegen.  U kunt ook opgeven in een webhook of een logische app hier die wordt uitgevoerd als de voorwaarde wordt voldaan.  Deze kunnen worden gebruikt om te beperken van het gedetecteerde probleem of 

    ![toevoegen van waarschuwingsregel](media/app-insights-tutorial-alert/add-alert-02.png)


## <a name="proactively-send-information"></a>Proactief informatie verzenden
Waarschuwingen in reactie op een bepaalde set van problemen die worden beschreven in uw toepassing worden gemaakt en u waarschuwingen voor kritieke voorwaarden die onmiddellijke aandacht doorgaans reserveren.  Proactief krijgt u informatie over uw toepassing met een logische App die automatisch wordt uitgevoerd volgens een schema.  U kunt een e-mailbericht verzonden naar beheerders dagelijks met overzichtsgegevens die verdere evaluatie vereist bijvoorbeeld kunnen hebben.

Zie voor meer informatie over het maken van een logische App met Application Insights [automatiseren Application Insights verwerkt met behulp van Logic Apps](automate-with-logic-apps.md)

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt geleerd hoe u een waarschuwing bij problemen, Ga naar de volgende zelfstudie voor informatie over het analyseren hoe gebruikers communiceert met uw toepassing.

> [!div class="nextstepaction"]
> [Gebruikers begrijpen](app-insights-tutorial-users.md)
---
title: Waarschuwingen verzenden vanuit Azure Application Insights | Microsoft Docs
description: Zelfstudie over het verzenden van waarschuwingen als reactie op fouten in uw toepassing met behulp van Azure Application Insights.
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/10/2019
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 05285a177827cd0dd1e0e39e779a395ccfdfc0cd
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/15/2019
ms.locfileid: "59578761"
---
# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>Toepassingsstatus bewaken en waarschuwingen erover verzenden met Azure Application Insights

Met Azure Application Insights kunt u uw toepassing bewaken en waarschuwingen ontvangen wanneer deze niet beschikbaar is, wanneer er fouten optreden, of wanneer er sprake is van prestatieproblemen.  In deze zelfstudie doorloopt u het proces voor het maken van tests uit om continu te controleren de beschikbaarheid van uw toepassing.

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Beschikbaarheidstest maken voor het continu controleren van de reacties van de toepassing
> * E-mail verzenden naar beheerders wanneer zich een probleem voordoet

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

Maak een [Application Insights-resource](https://docs.microsoft.com/azure/azure-monitor/learn/dotnetcore-quick-start#enable-application-insights).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-availability-test"></a>Beschikbaarheidstest maken

Met beschikbaarheidstests in Application Insights kunt u uw toepassing automatisch testen vanuit verschillende locaties over de hele wereld.   In deze zelfstudie, moet u een url-test om ervoor te zorgen dat uw web-App beschikbaar is uitvoeren.  U kunt ook een volledig overzicht maken om de werking van de toepassing gedetailleerd te testen. 

1. Selecteer **Application Insights** en selecteer vervolgens uw abonnement.  

2. Selecteer **beschikbaarheid** onder de **onderzoeken** menu en klik vervolgens op **maken test**.

    ![Beschikbaarheidstest toevoegen](media/tutorial-alert/add-test-001.png)

3. Typ een naam voor de test en laat de andere standaardinstellingen ongewijzigd.  Deze selectie wordt geactiveerd aanvragen voor de url van de toepassing om de 5 minuten van vijf verschillende geografische locaties.

4. Selecteer **waarschuwingen** openen de **waarschuwingen** vervolgkeuzelijst waar u de details voor informatie over het reageren op als de test mislukt kunt definiëren. Kies **bijna realtime** en zet de status op **ingeschakeld.**

    Typ een e-mailadres waarnaar een melding moet worden verzonden, wanneer wordt voldaan aan de waarschuwingscriteria.  U kunt optioneel ook het adres van een webhook typen dat moet worden aangeroepen wanneer wordt voldaan aan de waarschuwingscriteria.

    ![Test maken](media/tutorial-alert/create-test-001.png)

5. Ga terug naar het deelvenster test, selecteer het beletselteken en waarschuwing om in te voeren van de configuratie voor de waarschuwing voor bijna realtime bewerken.

    ![Waarschuwing bewerken](media/tutorial-alert/edit-alert-001.png)

6. Mislukte locaties ingesteld op groter dan of gelijk aan 3. Maak een [actiegroep](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) configureren die wordt een melding wanneer uw waarschuwingsdrempel is geschonden.

    ![De gebruikersinterface van de waarschuwing opslaan](media/tutorial-alert/save-alert-001.png)

7. Als u de waarschuwing hebt geconfigureerd, klikt u op de testnaam van de om de details van elke locatie weer te geven. Tests kunnen worden weergegeven in beide graph- en spreidingsdiagrammen plot de notatie voor het visualiseren van de geslaagde/mislukte voor een bepaalde periode.

    ![Testdetails](media/tutorial-alert/test-details-001.png)

8. U kunt inzoomen op de details van elke test door te klikken op het punt in het spreidingsdiagram. De detailweergave van het end-to-end-transactie wordt geopend. In het onderstaande voorbeeld ziet u de details van een mislukte aanvraag.

    ![Testresultaat](media/tutorial-alert/test-result-001.png)
  
## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u waarschuwingen voor problemen kunt genereren, gaat u verder met de volgende zelfstudie waarin u leert hoe u hoe kunt analyseren hoe uw gebruikers de toepassing gebruiken.

> [!div class="nextstepaction"]
> [Gebruikers begrijpen](../../azure-monitor/learn/tutorial-users.md)
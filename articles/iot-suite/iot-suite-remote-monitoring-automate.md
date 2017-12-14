---
title: Problemen bij apparaten in de oplossing voor externe controle - Azure detecteren | Microsoft Docs
description: Deze zelfstudie laat zien hoe u regels en acties automatisch detecteren op basis van drempelwaarden apparaat problemen in de oplossing voor externe controle.
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 12/12/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: e00c4ab2fc8bb13a765f7c2154555607dddfc651
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="detect-issues-using-threshold-based-rules"></a>Problemen met behulp van regels op basis van drempelwaarden detecteren

Deze zelfstudie ziet de mogelijkheden van de motor regels in de oplossing voor externe controle. De zelfstudie maakt ter introductie van het volgen van deze mogelijkheden gebruik van een scenario in de Contoso IoT-toepassing.

Contoso heeft een regel die een kritieke waarschuwing wordt gegenereerd wanneer de druk gemeld door een **Koelunit** apparaat is groter dan 250 PSI. Als operator die u wilt identificeren **Koelunit** apparaten waarvoor problematisch sensoren door te zoeken naar eerste Druk pieken. Deze om apparaten te identificeren, moet u een regel voor het genereren van een waarschuwing wanneer de druk groter is dan 150 PSI maken.

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * De regels in uw oplossing weergeven
> * Een nieuwe regel maken
> * Een bestaande regel bewerken
> * Een regel verwijderen

## <a name="prerequisites"></a>Vereisten

Volg deze zelfstudie, moet u een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement.

Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren van de vooraf geconfigureerde oplossing voor externe controle](iot-suite-remote-monitoring-deploy.md) zelfstudie.

## <a name="view-the-rules-in-your-solution"></a>De regels in uw oplossing weergeven

De **regels en acties** pagina in de oplossing geeft een lijst met de huidige regels:

![Regels en acties pagina](media/iot-suite-remote-monitoring-automate/rulesactions.png)

Om weer te geven alleen de regels die betrekking hebben op **Koelunit** apparaten, een filter toepassen:

![De lijst met regels filteren](media/iot-suite-remote-monitoring-automate/rulesactionsfilter.png)

U kunt meer informatie over een regel bekijken en bewerken wanneer u deze in de lijst selecteert:

![Details van de regel weergeven](media/iot-suite-remote-monitoring-automate/rulesactionsdetail.png)

Als u wilt uitschakelen, inschakelen of verwijderen van een of meer regels, selecteert u meerdere regels in de lijst:

![Meerdere regels selecteren](media/iot-suite-remote-monitoring-automate/rulesactionsmultiselect.png)

## <a name="create-a-new-rule"></a>Een nieuwe regel maken

Toevoegen van een nieuwe regel waarvoor wordt een waarschuwing gegenereerd wanneer de druk in een **Koelunit** apparaat groter is dan 150 PSI, kiest u **nieuwe regel**:

![Regel maken](media/iot-suite-remote-monitoring-automate/rulesactionsnewrule.png)

De volgende waarden gebruiken om de regel te maken:

| Instelling          | Waarde                                 |
| ---------------- | ------------------------------------- |
| Naam             | Koelunit waarschuwing                       |
| Bron           | **Chillers** apparaatgroep             |
| Trigger-veld    | pressure                              |
| Trigger-operator | Groter dan                          |
| Trigger-waarde    | 150                                   |
| Ernstniveau   | Waarschuwing                               |
| Beschrijving      | Druk Koelunit heeft 150 PSI overschreden |

Kies voor het opslaan van de nieuwe regel **toepassen**.

U kunt weergeven wanneer de regel wordt geactiveerd op de **regels en acties** pagina of op de **Dashboard** pagina.

## <a name="edit-an-existing-rule"></a>Een bestaande regel bewerken

Als u wilt een wijziging aanbrengt aan een bestaande regel, selecteert u deze in de lijst met regels. Klik in de **regel Detail** Configuratiescherm kiezen **bewerkingsmodus**.

![Regel voor het bewerken](media/iot-suite-remote-monitoring-automate/rulesactionsedit.png)

## <a name="disable-a-rule"></a>Een regel uitschakelen

Voor het tijdelijk uitschakelen op een regel, kunt u deze uitschakelen in de lijst met regels. Kies de regel uit te schakelen en kies vervolgens **uitschakelen**. De **Status** van de regel in de lijst met wijzigingen om aan te geven van de regel is uitgeschakeld. U kunt een regel die u eerder hebt uitgeschakeld met dezelfde procedure opnieuw inschakelen.

![Regel uitschakelen](media/iot-suite-remote-monitoring-automate/rulesactionsdisable.png)

U kunt inschakelen en uitschakelen van meerdere regels op hetzelfde moment als u meerdere regels in de lijst selecteert.

## <a name="delete-a-rule"></a>Een regel verwijderen

Als permanent wilt verwijderen een regel, kiest u de regel in de lijst met regels en kies vervolgens **verwijderen**.

Als u meerdere regels in de lijst selecteert, kunt u meerdere regels tegelijk verwijderen.

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie hebt u geleerd hoe naar:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * De regels in uw oplossing weergeven
> * Een nieuwe regel maken
> * Een bestaande regel bewerken
> * Een regel verwijderen

U hebt geleerd hoe u problemen met regels op basis van drempelwaarden detecteren, de voorgestelde volgende stappen zijn voor meer informatie over hoe:

* [Uw apparaten beheren en configureren](./iot-suite-remote-monitoring-manage.md).
* [Problemen oplossen en het oplossen van problemen met apparaat](./iot-suite-remote-monitoring-maintain.md).
* [Testen van uw oplossing met gesimuleerde apparaten](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->
---
title: Problemen met apparaat detecteren in Azure-externe controle oplossing - | Microsoft Docs
description: Deze zelfstudie laat zien hoe u regels en acties automatisch detecteren op basis van drempelwaarden apparaat problemen in de oplossing voor externe controle.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-suite
ms.date: 05/01/2018
ms.topic: conceptual
ms.openlocfilehash: df1ba7909c64e8ccc24bcf3584bd28b2629f49ff
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627310"
---
# <a name="detect-issues-using-threshold-based-rules"></a>Problemen met behulp van regels op basis van drempelwaarden detecteren

Deze zelfstudie ziet de mogelijkheden van de motor regels in de oplossing voor externe controle. De zelfstudie maakt ter introductie van het volgen van deze mogelijkheden gebruik van een scenario in de Contoso IoT-toepassing.

Contoso heeft een regel die een kritieke waarschuwing wordt gegenereerd wanneer de druk gemeld door een **Koelunit** apparaat is groter dan 250 PSI. Als operator die u wilt identificeren **Koelunit** apparaten waarvoor problematisch sensoren door te zoeken naar eerste Druk pieken. Deze om apparaten te identificeren, moet u een regel voor het genereren van een waarschuwing wanneer de druk groter is dan 150 PSI maken.

U hebt ook is gemeld dat een kritieke waarschuwing moet worden geactiveerd wanneer de gemiddelde vochtigheid van de **Koelunit** apparaat in de afgelopen vijf minuten is groter dan 80% en de temperatuur van de **Koelunit** apparaat in de afgelopen vijf minuten is groter dan 75 graden fahrenheit.

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * De regels in uw oplossing weergeven
> * Een nieuwe regel maken
> * Een nieuwe regel maken met meerdere voorwaarden
> * Een bestaande regel bewerken
> * Een regel verwijderen

## <a name="prerequisites"></a>Vereisten

Volg deze zelfstudie, moet u een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement.

Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren Remote Monitoring solution accelerator](iot-accelerators-remote-monitoring-deploy.md) zelfstudie.

## <a name="view-the-rules-in-your-solution"></a>De regels in uw oplossing weergeven

De **regels** pagina in de oplossing geeft een lijst met de huidige regels:

![Regels en acties pagina](./media/iot-accelerators-remote-monitoring-automate/rulesactions_v2.png)

Om weer te geven alleen de regels die betrekking hebben op **Koelunit** apparaten, een filter toepassen:

![De lijst met regels filteren](./media/iot-accelerators-remote-monitoring-automate/rulesactionsfilter_v2.png)

U kunt meer informatie over een regel bekijken en bewerken wanneer u deze in de lijst selecteert:

![Details van de regel weergeven](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdetail_v2.png)

Als u wilt uitschakelen, inschakelen of verwijderen van een of meer regels, selecteert u meerdere regels in de lijst:

![Meerdere regels selecteren](./media/iot-accelerators-remote-monitoring-automate/rulesactionsmultiselect_v2.png)

## <a name="create-a-new-rule"></a>Een nieuwe regel maken

Toevoegen van een nieuwe regel waarvoor wordt een waarschuwing gegenereerd wanneer de druk in een **Koelunit** apparaat groter is dan 150 PSI, kiest u **nieuwe regel**:

![Regel maken](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_v2.png)

De volgende waarden gebruiken om de regel te maken:

| Instelling          | Waarde                                 |
| ---------------- | ------------------------------------- |
| Regelnaam        | Koelunit waarschuwing                       |
| Beschrijving      | Druk Koelunit heeft 150 PSI overschreden |
| Apparaatgroep     | **Chillers** apparaatgroep             |
| Berekening      | Chatberichten                               |
| Veld voorwaarde 1| pressure                              |
| 1-voorwaardeoperator | Groter dan                      |
| Waarde van 1 voorwaarde    | 150                               |
| Serverity niveau  | Waarschuwing                               |

Kies voor het opslaan van de nieuwe regel **toepassen**.

U kunt weergeven wanneer de regel wordt geactiveerd op de **regels** pagina of op de **Dashboard** pagina.

## <a name="create-a-new-rule-with-multiple-conditions"></a>Een nieuwe regel maken met meerdere voorwaarden

Een nieuwe regel maken met meerdere voorwaarden die een kritieke genereert een waarschuwing wanneer de gemiddelde vochtigheid van de **Koelunit** apparaat in de afgelopen vijf minuten is groter dan 80% en de temperatuur van de **Koelunit** apparaat in de afgelopen vijf minuten is groter dan 75 graden fahrenheit, kiest u **nieuwe regel**:

![Mult-regel maken](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_v2.png)

De volgende waarden gebruiken om de regel te maken:

| Instelling          | Waarde                                 |
| ---------------- | ------------------------------------- |
| Regelnaam        | Koelunit Vochtigheids- en kritieke temp    |
| Beschrijving      | Vochtigheids- en temperatuurtelemetrie zijn essentieel |
| Apparaatgroep     | **Chillers** apparaatgroep             |
| Berekening      | Gemiddeld                               |
| Periode      | 5                                     |
| Veld voorwaarde 1| Vochtigheid                              |
| 1-voorwaardeoperator | Groter dan                      |
| Waarde van 1 voorwaarde    | 80                               |
| Serverity niveau  | Kritiek                              |

De tweede als voorwaarde wilt toevoegen, klikt u op plusteken (+ voorwaarde toevoegen).

![2-voorwaarde maken](./media/iot-accelerators-remote-monitoring-automate/rulesactionsnewrule_mult_cond2_v2.png)

Gebruik de volgende waarden op de nieuwe voorwaarde:

| Instelling          | Waarde                                 |
| ---------------- | ------------------------------------- |
| Veld voorwaarde 2| temperatuur                           |
| Voorwaarde 2-operator | Groter dan                      |
| Waarde van de voorwaarde 2    | 75                                |

Kies voor het opslaan van de nieuwe regel **toepassen**.

U kunt weergeven wanneer de regel wordt geactiveerd op de **regels** pagina of op de **Dashboard** pagina.

## <a name="edit-an-existing-rule"></a>Een bestaande regel bewerken

Als u wilt een wijziging aanbrengt aan een bestaande regel, selecteert u deze in de lijst met regels.

![Regel voor het bewerken](./media/iot-accelerators-remote-monitoring-automate/rulesactionsedit_v2.png)

<!--## Disable a rule

To temporarily switch off a rule, you can disable it in the list of rules. Choose the rule to disable, and then choose **Disable**. The **Status** of the rule in the list changes to indicate the rule is now disabled. You can re-enable a rule that you previously disabled using the same procedure.

![Disable rule](./media/iot-accelerators-remote-monitoring-automate/rulesactionsdisable.png)

You can enable and disable multiple rules at the same time if you select multiple rules in the list.-->

<!--## Delete a rule

To permanently delete a rule, choose the rule in the list of rules and then choose **Delete**.

You can delete multiple rules at the same time if you select multiple rules in the list.-->

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie hebt u geleerd hoe naar:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * De regels in uw oplossing weergeven
> * Een nieuwe regel maken
> * Een bestaande regel bewerken
> * Een regel verwijderen

U hebt geleerd hoe u problemen met regels op basis van drempelwaarden detecteren, de voorgestelde volgende stappen zijn voor meer informatie over hoe:

* [Uw apparaten beheren en configureren](iot-accelerators-remote-monitoring-manage.md).
* [Problemen oplossen en het oplossen van problemen met apparaat](iot-accelerators-remote-monitoring-maintain.md).
* [Testen van uw oplossing met gesimuleerde apparaten](iot-accelerators-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->
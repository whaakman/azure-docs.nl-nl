---
title: Vertraging van de volgende actie in werkstromen - Azure Logic Apps
description: Wachten tot de volgende actie uitvoeren in logic app workflows met behulp van de vertraging of vertraging tot acties in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
tags: connectors
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 27475fb3f086dbc5166a473e9d657d2dab723938
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66297636"
---
# <a name="delay-running-the-next-action-in-azure-logic-apps"></a>Vertraging voor het uitvoeren van de volgende actie in Azure Logic Apps

Als u wilt uw logische app een hoeveelheid tijd voordat het uitvoeren van de volgende actie wachten, kunt u de ingebouwde toevoegen **vertraging - plannen** actie voordat u een actie in uw logic app-werkstroom. U kunt de ingebouwde toevoegen **vertragen tot - plannen** actie moet worden gewacht totdat een specifieke datum en tijd voordat u de volgende actie uitvoert. Zie voor meer informatie over de ingebouwde schema acties en triggers [planning en uitvoering terugkerende, geautomatiseerde, taken en werkstromen met Azure Logic Apps](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

* **vertraging**: Wachten op het opgegeven aantal tijdseenheden, zoals seconden, minuten, uren, dagen, weken of maanden, voordat u de volgende actie uitgevoerd.

* **Vertragen tot**: Wachten tot de opgegeven datum en tijd voordat de volgende actie wordt uitgevoerd.

Hier volgen enkele manieren voorbeeld om het gebruik van deze acties:

* Wachten tot een weekdag voor een statusupdate verzenden via e-mail.

* Uw werkstroom uitstellen totdat een HTTP-aanroep is voltooid voordat deze wordt hervat en het ophalen van gegevens.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [zich aanmelden voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Basiskennis over [logische apps](../logic-apps/logic-apps-overview.md). Voordat u een actie gebruiken kunt, moet eerst uw logische app starten met een trigger. U kunt een trigger die u wilt en andere acties toevoegen voordat u een vertragingsactie toevoegen. In dit onderwerp wordt gebruikgemaakt van een Office 365 Outlook-trigger. Als u geen ervaring met logische apps, krijgt u informatie [over het maken van uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-delay"></a>

## <a name="add-the-delay-action"></a>Voeg de actie vertragen

1. Kies in de Logic App Designer onder de stap waarin u wilt toevoegen van de vertragingsactie **nieuwe stap**.

   Als u wilt toevoegen de vertragingsactie tussen fasen, gaat u de aanwijzer over de pijl die verbinding maakt de stappen. Kies het plusteken (+) dat wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Typ 'vertraging' als filter in het zoekvak. Selecteer deze actie uit de lijst met acties: **Delay**

   !["Vertraging"-actie toevoegen](./media/connectors-native-delay/add-delay-action.png)

1. Geef de hoeveelheid tijd moet worden gewacht voordat de volgende actie uitgevoerd.

   ![Hoeveelheid tijd voor de vertraging instellen](./media/connectors-native-delay/delay-time-intervals.png)

   | Eigenschap | JSON-naam | Vereist | Type | Description |
   |----------|-----------|----------|------|-------------|
   | Count | count | Ja | Geheel getal | Het aantal tijdseenheden vertraging |
   | Eenheid | Eenheid | Ja | String | De eenheid van de tijd, bijvoorbeeld: `Second`, `Minute`, `Hour`, `Day`, `Week`, of `Month` |
   ||||||

1. Voeg eventuele andere vereiste acties uit te voeren in uw werkstroom.

1. Wanneer u klaar bent, kunt u uw logische app opslaan.

<a name="add-delay-until"></a>

## <a name="add-the-delay-until-action"></a>De vertraging toevoegen-tot actie

1. Kies in de Logic App Designer onder de stap waarin u wilt toevoegen van de vertragingsactie **nieuwe stap**.

   Als u wilt toevoegen de vertragingsactie tussen fasen, gaat u de aanwijzer over de pijl die verbinding maakt de stappen. Kies het plusteken (+) dat wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Typ 'vertraging' als filter in het zoekvak. Selecteer deze actie uit de lijst met acties: **Vertragen tot**

   ![De actie 'Vertragen tot' toevoegen](./media/connectors-native-delay/add-delay-until-action.png)

1. Geef de datum en tijd voor als u wilt doorgaan met de werkstroom.

   ![Tijdstempel voor het einde van de vertraging opgeven](./media/connectors-native-delay/delay-until-timestamp.png)

   | Eigenschap | JSON-naam | Vereist | Type | Description |
   |----------|-----------|----------|------|-------------|
   | Tijdstempel | timestamp | Ja | String | De einddatum en -tijd voor het hervatten van de werkstroom met behulp van deze indeling: <p>JJJJ-MM-ddTHH <p>Bijvoorbeeld als u wilt dat 18 September 2017 om 14:00 uur, geeft ' 2017-09-18T14:00:00Z '. <p>**Opmerking:** Deze tijd-indeling moet volgen de [ISO 8601 datum tijdsspecificatie](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) in [UTC-datum-tijdnotatie](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), maar zonder een [UTC-offset](https://en.wikipedia.org/wiki/UTC_offset). Zonder een tijdzone, moet u de letter 'Z' toevoegen aan het einde zonder spaties. Deze "Z" verwijst naar het equivalent [nautische tijd](https://en.wikipedia.org/wiki/Nautical_time). |
   ||||||

1. Voeg eventuele andere vereiste acties uit te voeren in uw werkstroom.

1. Wanneer u klaar bent, kunt u uw logische app opslaan.

## <a name="next-steps"></a>Volgende stappen

* [Maken, plannen en uitvoeren van terugkerende taken en werkstromen met de trigger met terugkeerpatroon](../connectors/connectors-native-recurrence.md)
* [Connectors voor Logic Apps](../connectors/apis-list.md)
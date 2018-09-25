---
title: Migreren van Azure Scheduler naar Azure Logic Apps
description: Leer hoe u taken in Azure Scheduler, is buiten gebruik gesteld, kunt vervangen door Azure Logic Apps
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/20/2018
ms.openlocfilehash: c841f29adbe9911193227cced2856d953d820b08
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46997270"
---
# <a name="migrate-azure-scheduler-jobs-to-azure-logic-apps"></a>Azure Scheduler-taken migreren naar Azure Logic Apps

> [!IMPORTANT]
> Met Azure Logic Apps is bezig met het vervangen van Azure Scheduler, is buiten gebruik gesteld. Volg voor het plannen van taken, in dit artikel voor het verplaatsen naar Azure Logic Apps in plaats daarvan.

Dit artikel wordt beschreven hoe u eenmalige en terugkerende taken kunt plannen door geautomatiseerde werkstromen maken met Azure Logic Apps, in plaats van met Azure Scheduler. Wanneer u geplande taken met Logic Apps maakt, krijgt u tot deze voordelen:

* U hoeft te bekommeren het concept van een *taakverzameling* omdat elke logische app een afzonderlijke Azure-resources is.

* U kunt meerdere eenmalige taken uitvoeren met behulp van een enkele logische app.

* De service Azure Logic Apps biedt ondersteuning voor tijdzone- en wintertijd (Zomertijd).

Zie voor meer informatie, [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md) of maak uw eerste logische app in deze Quick Start: [maken van uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

* Gebruik voor het activeren van uw logische app door HTTP-verzoeken te sturen, een hulpprogramma zoals de [Postman bureaublad-app](https://www.getpostman.com/apps).

## <a name="schedule-one-time-jobs"></a>Eenmalige taken plannen

U kunt meerdere eenmalige taken uitvoeren door alleen een één logische app te maken. 

### <a name="create-your-logic-app"></a>Uw logische app maken

1. In de [Azure-portal](https://portal.azure.com), een lege logische app maken in Logic App Designer. 

   Ga als volgt voor de eenvoudige stappen [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Typ 'wanneer een http-aanvraag' als filter in het zoekvak. Selecteer in de lijst met triggers deze trigger: **wanneer een HTTP-aanvraag wordt ontvangen** 

   ![Trigger 'Aanvragen' toevoegen](./media/migrate-from-scheduler-to-logic-apps/request-trigger.png)

1. U kunt een JSON-schema, die helpt de ontwerper van logische App de structuur voor de invoer van de inkomende aanvraag begrijpen en maakt de uitvoer eenvoudiger voor u later in uw werkstroom te selecteren (optioneel) opgeven voor de aanvraagactivering.

   Geef een schema op het schema in te voeren de **hoofdtekst van de JSON-Schema aanvragen** vak, bijvoorbeeld: 

   ![Schema van de aanvraag](./media/migrate-from-scheduler-to-logic-apps/request-schema.png)

   Als u een schema hebt, maar u een voorbeeld-nettolading in JSON-indeling hebt, kunt u een schema genereren uit deze nettolading.

   1. Kies in de aanvraagtrigger **voorbeeldnettolading gebruiken voor het genereren van schema**.

   1. Onder **typt of plakt u een voorbeeld-JSON-nettolading**, bieden de voorbeeldnettolading in en kies vervolgens **gedaan**, bijvoorbeeld:

      ![Voorbeeld van payload](./media/migrate-from-scheduler-to-logic-apps/sample-payload.png)

1. Kies onder de trigger **volgende stap**. 

1. Voer 'vertragen tot' als filter in het zoekvak. Onder de lijst met acties, selecteert u deze actie: **vertragen tot**

   Deze actie onderbreekt de werkstroom van uw logische app tot een opgegeven datum en tijd.

   ![De actie 'Vertragen tot' toevoegen](./media/migrate-from-scheduler-to-logic-apps/delay-until.png)

1. Voer het tijdstempel voor als u wilt om de werkstroom van de logische app te starten. 

   Wanneer u klikt u in de **Timestamp** vak de lijst met dynamische inhoud wordt weergegeven, zodat u eventueel een uitvoer uit de trigger selecteren kunt.

   ![Geef details "Vertragen tot"](./media/migrate-from-scheduler-to-logic-apps/delay-until-details.png)

1. Voeg eventuele andere vereiste acties uit voeren door te selecteren te in [~ meer dan 200 connectoren](../connectors/apis-list.md). 

   U kunt bijvoorbeeld een HTTP-actie die een aanvraag verzendt naar een URL of acties die werken met Storage-wachtrijen, Service Bus-wachtrijen of Service Bus-onderwerpen zijn: 

   ![HTTP-actie](./media/migrate-from-scheduler-to-logic-apps/request-http-action.png)

1. Wanneer u klaar bent, kunt u uw logische app opslaan.

   ![Uw logische app opslaan](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

   Wanneer u uw logische app voor de eerste keer opslaat, het eindpunt-URL voor de aanvraagtrigger van uw logische app wordt weergegeven in de **URL voor HTTP POST** vak. 
   Als u wilt uw logische app aanroepen en het verzenden van invoer voor uw logische app voor verwerking, moet u deze URL gebruiken als de bestemming van de aanroep.

   ![Opslaan van de aanvraag trigger eindpunt-URL](./media/migrate-from-scheduler-to-logic-apps/request-endpoint-url.png)

1. Kopieer en bewaar deze eindpunt-URL, zodat u later een handmatige aanvraag die wordt geactiveerd uw logische app kunt verzenden. 

## <a name="start-a-one-time-job"></a>Een eenmalige taak starten

Als u handmatig uitvoeren of activeren van een eenmalige taak, een aanroep van de eindpunt-URL voor de trigger voor uw logische app de aanvraag te verzenden. In deze aanroep, geeft u de invoer of de payload te verzenden, die u mogelijk eerder hebben beschreven door een schema op te geven. 

Bijvoorbeeld, met behulp van de Postman-app, u kunt een POST-aanvraag maken met de instellingen die vergelijkbaar is met dit voorbeeld, en kies vervolgens **verzenden** om de aanvraag te doen.

| Aanvraagmethode | URL | Hoofdtekst | Headers |
|----------------|-----|------|---------| 
| **POST** | <*eindpunt-URL*> | **onbewerkte** <p>**JSON(Application/JSON)** <p>In de **onbewerkte** voert u de nettolading die u wilt verzenden in de aanvraag. <p>**Houd er rekening mee**: deze instelling automatisch configureert de **Headers** waarden. | **Sleutel**: Content-Type <br>**Waarde**: application/json
 |||| 

![Aanvraag voor het activeren van uw logische app handmatig verzenden](./media/migrate-from-scheduler-to-logic-apps/postman-send-post-request.png)

Nadat u de aanroep verzenden, het antwoord van de logische app wordt weergegeven onder de **onbewerkte** vak op de **hoofdtekst** tabblad. 

<a name="workflow-run-id"></a>

> [!IMPORTANT]
>
> Als u annuleren van de taak later wilt, kiest u de **Headers** tabblad. Zoeken en kopieer de **x-ms-werkstroom-run-id** headerwaarde in het antwoord. 
>
> ![Antwoord](./media/migrate-from-scheduler-to-logic-apps/postman-response.png)

## <a name="cancel-a-one-time-job"></a>Een eenmalige taak annuleren

In Logic Apps, wordt elke eenmalige taak uitgevoerd als één logische app exemplaar wordt uitgevoerd. Als u wilt een eenmalige taak annuleren, kunt u [Werkstroomuitvoeringen - annuleren](https://docs.microsoft.com/rest/api/logic/workflowruns/cancel) in de REST API voor Logic Apps. Als u een aanroep van de trigger verzendt, geeft u de [werkstroom uitgevoerd ID](#workflow-run-id).

## <a name="schedule-recurring-jobs"></a>Plan terugkerende taken

### <a name="create-your-logic-app"></a>Uw logische app maken

1. In de [Azure-portal](https://portal.azure.com), een lege logische app maken in Logic App Designer. 

   Ga als volgt voor de eenvoudige stappen [Snelstartgids: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Typ "terugkeerpatroon" als filter in het zoekvak. Selecteer in de lijst met triggers deze trigger: **terugkeerpatroon** 

   !["Terugkeerpatroon" trigger toevoegen](./media/migrate-from-scheduler-to-logic-apps/recurrence-trigger.png)

1. Een meer geavanceerde schema instellen als u wilt.

   ![Geavanceerd schema](./media/migrate-from-scheduler-to-logic-apps/recurrence-advanced-schedule.png)

   Zie voor meer informatie over geavanceerde planningsopties [maken en uitvoeren van terugkerende taken en werkstromen met Azure Logic Apps](../connectors/connectors-native-recurrence.md)

1. Toevoegen van andere acties die u wilt dat door te selecteren in [meer dan 200 connectoren](../connectors/apis-list.md). Kies onder de trigger **volgende stap**. Zoek en selecteer de gewenste acties.

   U kunt bijvoorbeeld een HTTP-actie die een aanvraag verzendt naar een URL of acties die werken met Storage-wachtrijen, Service Bus-wachtrijen of Service Bus-onderwerpen zijn: 

   ![HTTP-actie](./media/migrate-from-scheduler-to-logic-apps/recurrence-http-action.png)

1. Wanneer u klaar bent, kunt u uw logische app opslaan.

   ![Uw logische app opslaan](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

## <a name="advanced-setup"></a>Geavanceerde instellingen

Hier volgen andere manieren waarop u uw taken kunt aanpassen.

### <a name="retry-policy"></a>Beleid voor opnieuw proberen

Voor het beheren van de manier waarop een actie probeert uit te voeren in uw logische app wanneer er storingen optreden, kunt u instellen de [beleid voor opnieuw proberen](../logic-apps/logic-apps-exception-handling.md#retry-policies) in van de actie-instellingen, bijvoorbeeld:

1. Openen van de actie (**...** ) menu en selecteer **instellingen**.

   ![Actie-instellingen openen](./media/migrate-from-scheduler-to-logic-apps/action-settings.png)

1. Selecteer het gewenste beleid voor opnieuw proberen. Zie voor meer informatie over elk beleid [beleid voor opnieuw proberen](../logic-apps/logic-apps-exception-handling.md#retry-policies).

   ![Selecteer beleid voor opnieuw proberen](./media/migrate-from-scheduler-to-logic-apps/retry-policy.png)

## <a name="handle-exceptions-and-errors"></a>Afhandelen van uitzonderingen en fouten

Als de standaardactie kan niet worden uitgevoerd, kunt u in Azure Scheduler, een alternatief actie die het probleem verhelpt uitvoeren. U kunt dezelfde taak ook uitvoeren in Azure Logic Apps.

1. In Logic App Designer boven de actie die u wilt verwerken, de aanwijzer over de pijl tussen fasen en selecteer en **een parallelle vertakking toevoegen**. 

   ![Parallelle vertakking toevoegen](./media/migrate-from-scheduler-to-logic-apps/add-parallel-branch.png)

1. Zoek en selecteer de actie die u wilt uitvoeren in plaats daarvan als alternatieve actie.

   ![Parallelle actie toevoegen](./media/migrate-from-scheduler-to-logic-apps/add-parallel-action.png)

1. Open op de alternatieve actie, de (**...** ) menu en selecteer **uitvoeren na configureren**.

   ![Uitvoeren na configureren](./media/migrate-from-scheduler-to-logic-apps/configure-run-after.png)

1. Schakel het selectievakje voor de **is geslaagd** eigenschap. Selecteer deze eigenschappen: **is mislukt**, **wordt overgeslagen**, en **is een time-out**

   !["Uitvoeren na" eigenschappen instellen](./media/migrate-from-scheduler-to-logic-apps/select-run-after-properties.png)

1. Wanneer u klaar bent, kiest u **gedaan**.

Zie voor meer informatie over de afhandeling van uitzonderingen, [afhandelen van fouten en uitzonderingen - eigenschap RunAfter](../logic-apps/logic-apps-exception-handling.md#catch-and-handle-failures-with-the-runafter-property).

## <a name="faq"></a>Veelgestelde vragen

<a name="retire-date"></a> 

**Q**: wanneer wordt Azure Scheduler buiten gebruik gesteld? <br>
**Een**: Azure Scheduler is gepland om te worden buiten gebruik stellen en met 30 September 2019.

**Q**: Wat gebeurt er met mijn Scheduler-taakverzamelingen en taken nadat de service beëindigen? <br>
**Een**: alle Scheduler-taakverzamelingen en taken worden verwijderd uit het systeem.

**Q**: heb ik een back-up of andere taken uitvoeren voordat u migreert van mijn Scheduler-taken naar logische Apps? <br>
**Een**: als een best practice altijd back-up van uw werk. Controleer of de logische apps die u hebt gemaakt zoals verwacht voordat verwijderen of uitschakelen van de Scheduler-taken worden uitgevoerd. 

**Q**: Is er een hulpprogramma waarmee u me kan helpen migreren van mijn taken van Scheduler naar Logic Apps? <br>
**Een**: elke Scheduler-taak is uniek is, zodat een niet-gedifferentieerde hulpprogramma bestaat niet. Verschillende scripts zijn echter beschikbaar voor u om te wijzigen voor uw behoeften. Voor de beschikbaarheid van script, kom later hier terug.

**Q**: waar kan ik ondersteuning krijgen voor het migreren van mijn Scheduler-taken? <br>
**Een**: Hier volgen enkele manieren ondersteuning krijgen: 

**Azure Portal**

Als uw Azure-abonnement een betaald abonnement heeft, kunt u een aanvraag voor technische ondersteuning maken in Azure portal. Anders kunt u een andere ondersteuningsoptie selecteren.

1. Op de [Azure-portal](https://portal.azure.com) hoofdmenu, selecteer **Help en ondersteuning**.

1. Onder **ondersteunen**, selecteer **nieuwe ondersteuningsaanvraag**. Geef de volgende gegevens voor uw aanvraag:

   | Instelling | Waarde |
   |---------|-------|
   | **Probleemtype** | **Technische** | 
   | **Abonnement** | <*uw Azure-abonnement*> | 
   | **Service** | Onder **controle en beheer**, selecteer **Scheduler**. | 
   ||| 

1. Selecteer de gewenste Ondersteuningsoptie. Als u een betaald abonnement hebt, kiest u **volgende**.

**Community**

* [Forum voor Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-scheduler)

## <a name="next-steps"></a>Volgende stappen

* [Regelmatig actieve taken en werkstromen maken met Azure Logic Apps](../connectors/connectors-native-recurrence.md)
* [Zelfstudie: Verkeer met een schema op basis van logische app controleren](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
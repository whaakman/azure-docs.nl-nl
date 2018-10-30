---
title: Verzenden en ontvangen van berichten met Azure Service Bus - Azure Logic Apps | Microsoft Docs
description: Instellen van enterprise cloud-berichten met Azure Service Bus in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 68378c87e18df874059579445352b8fd1b2b6c13
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232712"
---
# <a name="exchange-messages-in-the-cloud-with-azure-service-bus-and-azure-logic-apps"></a>Exchange-berichten in de cloud met Azure Service Bus en Azure Logic Apps

Met Azure Logic Apps en de Azure Service Bus-connector, kunt u geautomatiseerde taken en werkstromen voor het overbrengen van gegevens, zoals verkoop en inkooporders, tijdschriften, en inventaris verplaatsingen van het type voor toepassingen voor uw organisatie maken. De connector niet alleen bewaakt, verzendt, en beheert van berichten, maar voert ook de acties met wachtrijen, -sessies, onderwerpen, abonnementen, enzovoort, bijvoorbeeld:

* Monitor wanneer berichten binnenkomen (automatisch voltooien) of ontvangen (peek-lock) in wachtrijen, onderwerpen en onderwerpabonnementen zijn. 
* Berichten worden verzonden.
* Maak en verwijder berichtabonnementen.
* Berichten in wachtrijen en onderwerpabonnementen beheren, bijvoorbeeld: get, uitgesteld ophalen, voltooid, uitstellen, dit en dead-letter uitvoeren.
* Vergrendelingen op berichten en sessies in wachtrijen en onderwerpabonnementen vernieuwen.
* Sluit sessies in wachtrijen en onderwerpen.

U kunt triggers die te antwoorden krijgen van Service Bus en de uitvoer beschikbaar voor andere acties in uw logische apps gebruiken. U kunt ook andere acties waarmee de uitvoer van de Service Bus-acties hebben. Als u geen ervaring met Service Bus en Logic Apps, raadpleegt u [wat is Azure Service Bus?](../service-bus-messaging/service-bus-messaging-overview.md) en [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Een Service Bus-naamruimte en een Berichtentiteit, zoals een wachtrij. Als u deze items niet hebt, krijgt u informatie over het [uw Service Bus-naamruimte en een wachtrij maken](../service-bus-messaging/service-bus-create-namespace-portal.md). 

  Deze items moeten zich in hetzelfde Azure-abonnement als uw logische apps die gebruikmaken van deze items.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u Service Bus gebruiken. Uw logische app moet zich in hetzelfde Azure-abonnement als uw servicebus. Om te beginnen met een Service Bus-trigger, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een Service Bus-bewerking beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

<a name="permissions-connection-string"></a>

## <a name="check-permissions"></a>Controleer de machtigingen

Controleer of uw logische app machtigingen voor toegang tot uw Service Bus-naamruimte heeft. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Ga naar uw Service Bus *naamruimte*. Op de pagina naamruimte onder **instellingen**, selecteer **beleid voor gedeelde toegang**. Onder **Claims**, Controleer of u hebt **beheren** machtigingen voor die naamruimte

   ![Machtigingen beheren voor uw Service Bus-naamruimte](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

3. De verbindingsreeks voor uw Service Bus-naamruimte ophalen. U hebt deze tekenreeks nodig wanneer u uw verbindingsgegevens in uw logische app invoeren.

   1. Selecteer **RootManageSharedAccessKey**. 
   
   1. Kies de knop kopiëren naast uw primaire verbindingsreeks. De verbindingsreeks voor later gebruik opslaan.

      ![Service Bus-naamruimte-verbindingsreeks kopiëren](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Zoeken om te controleren of de verbindingsreeks gekoppeld aan uw Service Bus-naamruimte of een Berichtentiteit, zoals een wachtrij is, de verbindingsreeks voor de `EntityPath`  parameter. Als u deze parameter kunt vinden, de verbindingsreeks is voor een specifieke entiteit en is niet de juiste tekenreeks die moet worden gebruikt met uw logische app.

## <a name="add-trigger-or-action"></a>Trigger of actie toevoegen

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Om toe te voegen een *trigger* naar een lege, logische app, in het zoekvak 'Azure Service Bus' Voer als filter. Selecteer de gewenste trigger onder de lijst met triggers. 

   Bijvoorbeeld, als u wilt uw logische app wordt geactiveerd wanneer een nieuw item wordt verzonden naar een Service Bus-wachtrij, selecteer deze trigger: **wanneer een bericht wordt ontvangen in een wachtrij (automatisch voltooien)**

   ![Service Bus-trigger selecteren](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   > [!NOTE]
   > Sommige triggers als resultaat geven of berichten, bijvoorbeeld de trigger **als een of meer berichten aankomen in een wachtrij (automatisch voltooien)**. Wanneer deze triggers worden geactiveerd, retourneren ze tussen één en het aantal berichten dat is opgegeven door een van de trigger **bericht maximumaantal** eigenschap.

   *Alle Service Bus-triggers zijn long-polling triggers*, wat betekent dat wanneer de trigger wordt geactiveerd, wordt de trigger alle berichten worden verwerkt en wordt vervolgens gewacht 30 seconden voor meer berichten worden weergegeven in het abonnement wachtrij of onderwerp. 
   Als er geen berichten in 30 seconden weergegeven, wordt de trigger uitvoeren wordt overgeslagen. 
   Anders, blijft de trigger berichten lezen, totdat het abonnement wachtrij of onderwerp leeg is. De volgende trigger poll is gebaseerd op het terugkeerpatroon die is opgegeven in de eigenschappen van de trigger.

1. Om toe te voegen een *actie* aan een bestaande logische app, als volgt te werk: 

   1. Kies onder de laatste stap waar u een actie toevoegen, **nieuwe stap**. 

      Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
      Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

   1. Typ 'Azure Service Bus' als filter in het zoekvak. 
   Selecteer de actie die u wilt onder de lijst met acties. 
 
      Bijvoorbeeld: Selecteer deze actie: **bericht verzenden**

      ![Service Bus-actie selecteren](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png) 

1. Als u uw logische app naar uw Service Bus-naamruimte voor de eerste keer verbinding, vraagt de ontwerper van logische App nu u om uw verbindingsgegevens. 

   1. Geef een naam op voor uw verbinding en selecteer uw Service Bus-naamruimte.

      ![Service Bus-verbinding, deel 1 maken](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-1.png)

      Om door te voeren in plaats daarvan de connection string, kies **handmatig verbindingsgegevens invoeren**. 
      Als u de verbindingsreeks hebt, krijgt u informatie [over het vinden van de verbindingsreeks](#permissions-connection-string).

   1. Selecteer nu uw Service Bus-beleid en kies vervolgens **maken**.

      ![Service Bus-verbinding, deel 2 maken](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-2.png)

1. Selecteer voor dit voorbeeld wordt de Berichtentiteit die u, zoals een wachtrij of onderwerp wilt. In dit voorbeeld selecteert u uw Service Bus-wachtrij. 
   
   ![Service Bus-wachtrij selecteren](./media/connectors-create-api-azure-service-bus/service-bus-select-queue.png)

1. Geef de benodigde informatie voor uw trigger of actie. In dit voorbeeld gaat u als volgt de relevante stappen uit voor uw trigger of actie: 

   * **Voor de voorbeeld-trigger**: de polling-interval en frequentie voor het controleren van de wachtrij instellen.

     ![Polling-interval instellen](./media/connectors-create-api-azure-service-bus/service-bus-trigger-details.png)

     Wanneer u klaar bent, doorgaan met het ontwikkelen van uw logische app-werkstroom door toe te voegen van de acties die u wilt. U kunt bijvoorbeeld een actie die e-mail wordt verzonden zodra een nieuw bericht binnenkomt toevoegen.
     Wanneer de trigger wordt de wachtrij gecontroleerd en wordt een nieuw bericht gevonden, wordt de geselecteerde acties voor het bericht gevonden in uw logische app uitgevoerd.

   * **Voor de actie voorbeeld**: de inhoud van het bericht en andere details invoeren. 

     ![Geef de inhoud van het bericht en details](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)

     Wanneer u klaar bent, doorgaan met het ontwikkelen van uw logische app-werkstroom door toe te voegen van eventuele andere vereiste acties die u wilt. U kunt bijvoorbeeld een actie die wordt verzonden e-mailbericht waaruit blijkt dat het bericht is verzonden toevoegen.

1. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/servicebus/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
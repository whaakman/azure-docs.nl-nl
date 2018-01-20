---
title: Batch-berichten verwerken als een groep of een verzameling - Azure Logic Apps | Microsoft Docs
description: Verzenden en ontvangen van berichten voor batchverwerking in logic apps
keywords: batch, batchproces
author: jonfancey
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/7/2017
ms.author: LADocs; estfan; jonfan
ms.openlocfilehash: de519084a4f172ad984c78727123835eeb9deaef
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="send-receive-and-batch-process-messages-in-logic-apps"></a>Verzenden, ontvangen en verwerken van berichten in logic apps batch

Voor het verwerken van berichten samen in groepen, u kunt verzenden gegevensitems of berichten naar een *batch*, en vervolgens deze items als een batch te verwerken. Deze methode is handig als u wilt controleren of gegevensitems worden gegroepeerd in een bepaalde manier en samen worden verwerkt. 

Kunt u logic apps die items als een batch met behulp van ontvangen de **Batch** trigger. Vervolgens kunt u maken logic apps die items naar een batch te met behulp van verzenden de **Batch** in te grijpen.

Dit onderwerp wordt beschreven hoe u een batchen oplossing kunt maken met het uitvoeren van deze taken: 

* [Maken van een logische app dat ontvangt en verzamelt items als batch](#batch-receiver). Deze 'batch ontvanger' logische app geeft de batch-naam en release criteria om te voldoen aan voordat u de ontvanger logische app vrijgegeven en worden items verwerkt. 

* [Maken van een logische app waarmee items worden verzonden naar een batch](#batch-sender). Deze 'batch afzender' logische app geeft aan waar items, die een bestaande batch ontvanger logic app moet verzenden. U kunt ook een unieke sleutel, zoals een klantnummer 'partitie' of delen, de doel-batch in subsets op basis van die sleutel opgeven. Op die manier worden alle items met die sleutel verzameld en verwerkt samen. 

## <a name="requirements"></a>Vereisten

Wilt u dit voorbeeld gebruiken, moet u deze items:

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). U kunt eventueel ook direct kiezen voor [een Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Elementaire kennis over [logic apps maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Een e-mailaccount met een [e-provider wordt ondersteund door Azure Logic Apps](../connectors/apis-list.md)

<a name="batch-receiver"></a>

## <a name="create-logic-apps-that-receive-messages-as-a-batch"></a>Logic apps die berichten te als een batch ontvangen maken

Voordat u berichten naar een batch verzenden kunt, moet u eerst een 'batch ontvanger' logische app met maken de **Batch** trigger. Op die manier kunt u deze ontvanger logic app bij het maken van de afzender logische app. Voor de ontvanger geeft u de batchnaam van de, release criteria en andere instellingen. 

Afzender logische apps moeten weten waar items, terwijl de ontvanger logische apps hoeft niet te weten over de afzenders.

1. In de [Azure-portal](https://portal.azure.com), een logische app maken met deze naam: 'BatchReceiver' 

2. In Logic Apps Designer, voegt u de **Batch** trigger uw logische app-werkstroom op te starten. Voer in het zoekvak 'batch' als filter. Selecteer deze trigger: **Batch: Batch-berichten**

   ![Batch-trigger toevoegen](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Geef een naam voor de batch en Geef criteria op voor het vrijgeven van de batch, bijvoorbeeld:

   * **Batch-naam**: de naam die wordt gebruikt voor het identificeren van de batch is 'TestBatch' in dit voorbeeld.
   * **Release Criteria**: de batch release criteria, die kunnen worden gebaseerd op het aantal berichten, planning, of beide.
   
     ![Geef details op Batch trigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-release-criteria.png)

   * **Aantal berichten**: het aantal berichten voor het opslaan als een batch voordat voor verwerking, namelijk '5' in dit voorbeeld zijn vrijgegeven.

     ![Geef details op Batch trigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-count-based.png)

   * **Planning**: het schema van de batch release voor verwerking 'om de 5 minuten' in dit voorbeeld is.

     ![Geef details op Batch trigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-schedule-based.png)


4. Voeg een andere actie die een e-mailbericht wordt verzonden wanneer de batch-trigger wordt geactiveerd. Telkens wanneer die de batch vijf items of de afgelopen vijf minuten heeft verzendt de logische app een e-mailbericht.

   1. Kies onder de trigger batch **+ een nieuwe stap** > **een actie toevoegen**.

   2. Voer in het zoekvak 'e' als filter.
   Op basis van uw e-mailprovider, selecteert u een e-connector.
   
      Als u een account voor werk of school hebt, Selecteer bijvoorbeeld de Outlook van Office 365-connector. 
      Als u een Gmail-account hebt, selecteert u de connector Gmail.

   3. Deze actie voor de connector selecteert: **{*e-mailprovider*}-e-mailbericht verzenden**

      Bijvoorbeeld:

      ![Selecteer de actie 'E-mailbericht verzenden' voor uw e-mailprovider](./media/logic-apps-batch-process-send-receive-messages/add-send-email-action.png)

5. Als u niet eerder verbinding kunt voor uw e-mailprovider maken, moet u uw e-referenties opgeven voor verificatie wanneer u wordt gevraagd. Meer informatie over [verifiëren van de referenties van uw e-mailadres](../logic-apps/quickstart-create-first-logic-app-workflow.md).

6. Stel de eigenschappen voor de actie die u zojuist hebt toegevoegd.

   * Voer het e-mailadres van de ontvanger in het vak **Aan** in. 
   Voor testdoeleinden kunt u uw eigen e-mailadres gebruiken.

   * In de **onderwerp** vak, wanneer de **dynamische inhoud** lijst wordt weergegeven, selecteert u de **partitienaam** veld.

     ![Selecteer in de lijst 'Dynamische inhoud', 'Partitienaam'](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     In een volgende sectie, kunt u een unieke partitiesleutel die verdeelt de doel-batch in logische groepen waarbij u kunt om berichten te verzenden. 
     Elke set heeft een uniek nummer dat wordt gegenereerd door de afzender logische app. 
     Deze mogelijkheid kunt u één batch met meerdere subsets gebruikt, en elke subset met de naam die u opgeeft.

   * In de **hoofdtekst** vak, wanneer de **dynamische inhoud** lijst wordt weergegeven, selecteert u de **bericht-Id** veld.

     ![Selecteer voor 'Hoofdtekst', 'bericht-Id](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

     Omdat de invoer voor de e-mailbericht verzendactie een matrix is, voegt de ontwerpfunctie automatisch een **voor elk** lus rond de **e-mailbericht verzenden** in te grijpen. 
     Deze lus voert de interne actie op elk item in de batch. 
     Dus met de batch-trigger is ingesteld op vijf items, beschikt u over tijd vijf e-mails die de trigger wordt geactiveerd.

7.  Nu dat u een batch ontvanger logische app hebt gemaakt, sla uw logische app.

    ![Uw logische app opslaan](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

    > [!IMPORTANT]
    > Een partitie heeft een limiet van 5000 berichten of 80 MB. Als beide voorwaarden is voldaan, kan de batch worden vrijgegeven, zelfs als niet aan de gebruiker gedefinieerde voorwaarde wordt voldaan.


<a name="batch-sender"></a>

## <a name="create-logic-apps-that-send-messages-to-a-batch"></a>Logic apps die berichten naar een batch verzenden maken

Maak nu een of meer logische apps die items naar de batch gedefinieerd door de ontvanger logische app verzendt. Voor de afzender geeft u de ontvanger logische app en de batchnaam van de, inhoud van het bericht en eventuele andere instellingen. U kunt eventueel een unieke partitiesleutel voor het delen van de batch in deelverzamelingen voor het verzamelen van items met die sleutel opgeven.

Afzender logische apps moeten weten waar items, terwijl de ontvanger logische apps hoeft niet te weten over de afzenders.

1. Een andere logische app maken met deze naam: 'BatchSender'

   1. Voer in het zoekvak "recurrence" als filter. 
   Selecteer deze trigger: **schema - terugkeerpatroon**

      ![Toevoegen van de trigger "Terugkeerpatroon plannen"](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-receiver.png)

   2. Stel de frequentie en het interval om uit te voeren van de afzender logische app elke minuut.

      ![Frequentie en het interval voor de trigger terugkeerpatroon ingesteld](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-receiver-details.png)

2. Voeg een nieuwe stap voor het verzenden van berichten naar een batch.

   1. Kies onder de trigger terugkeerpatroon **+ een nieuwe stap** > **een actie toevoegen**.

   2. Voer in het zoekvak 'batch' als filter. 

   3. Deze actie selecteert: **berichten verzenden naar batch: Kies een werkstroom Logic Apps met batch-trigger**

      ![Selecteer 'Verzenden van berichten in een batch moet'](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   4. Nu uw 'BatchReceiver' logische app die u eerder hebt gemaakt, die nu wordt weergegeven als een actie selecteren.

      !['Batch ontvanger' logische app selecteren](./media/logic-apps-batch-process-send-receive-messages/send-batch-select-batch-receiver.png)

      > [!NOTE]
      > De lijst bevat ook andere logic apps waarvoor de batch-triggers.

3. De Batcheigenschappen instellen.

   * **Batch-naam**: de batchnaam gedefinieerd door de ontvanger logische app, die 'TestBatch' in dit voorbeeld en wordt gevalideerd tijdens runtime.

     > [!IMPORTANT]
     > Zorg ervoor dat u de batchnaam, die moet overeenkomen met de batchnaam die wordt opgegeven door de ontvanger logische app niet te wijzigen.
     > Als de batchnaam van de wijzigt, wordt de afzender logische app mislukken.

   * **Inhoud bericht**: de inhoud van het bericht dat u wilt verzenden. 
   Voor dit voorbeeld voegen deze expressie die de huidige datum en tijd worden ingevoegd in de inhoud van het bericht dat u de batch verzenden:

     1. Wanneer de **dynamische inhoud** lijst wordt weergegeven, kies **expressie**. 
     2. Voer de expressie **utcnow()**, en kies **OK**. 

        ![Kies in 'Bericht inhoud', 'Expressie'. Voer 'utcnow()'.](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details.png)

4. Nu ingesteld op een partitie voor de batch. Kies in de actie 'BatchReceiver' **geavanceerde opties weergeven**.

   * **De naam van partitie**: een optionele unieke partitiesleutel moet worden gebruikt voor het verdelen van de doel-batch. Voor dit voorbeeld voegt u een expressie die een willekeurig getal tussen een en vijf genereert.
   
     1. Wanneer de **dynamische inhoud** lijst wordt weergegeven, kies **expressie**.
     2. Voer deze expressie: **rand(1,6)**

        ![Instellen van een partitie voor de doel-batch](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-partition-advanced-options.png)

        Dit **RNG** functie genereert een getal tussen 1 en 5. 
        U zijn zodat deze batch verdelen in vijf genummerde partities, waarmee deze expressie voor het dynamisch wordt ingesteld.

   * **Bericht-Id**: een optionele bericht-id en een gegenereerde GUID wanneer leeg is. 
   In dit voorbeeld laat u dit vak leeg.

5. Sla uw logische app. Uw afzender logische app nu er ongeveer als volgt in dit voorbeeld:

   ![Sla uw logische app van afzender](./media/logic-apps-batch-process-send-receive-messages/send-batch-receiver-details-finished.png)

## <a name="test-your-logic-apps"></a>Uw logische apps testen

Laat uw logische apps uitgevoerd voor een paar minuten voor het testen van uw batchen oplossing. U start binnenkort opvragen van e-mailberichten in groepen van vijf allemaal met dezelfde partitiesleutel.

Uw BatchSender logische app uitgevoerd elke minuut, genereert een willekeurig getal tussen 1 en 5 en maakt gebruik van deze gegenereerd nummer als de partitiesleutel voor de doel-batch waarin berichten worden verzonden. Telkens wanneer die de batch vijf items met dezelfde partitiesleutel bevat uw BatchReceiver logische app wordt gestart en e-mail voor elk bericht verzendt.

> [!IMPORTANT]
> Wanneer u klaar bent testen, zorg ervoor dat de BatchSender logische app om te stoppen met het verzenden van berichten en te voorkomen dat uw postvak in overbelasting uit te schakelen.

## <a name="next-steps"></a>Volgende stappen

* [Maken van logic app-definities met behulp van JSON](../logic-apps/logic-apps-author-definitions.md)
* [Een zonder server-App in Visual Studio met Azure Logic Apps en functies](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Afhandeling van uitzonderingen en foutenregistratie voor logic apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

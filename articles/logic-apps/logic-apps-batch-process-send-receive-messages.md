---
title: Batch-berichten verwerken als een groep of een verzameling - Azure Logic Apps | Microsoft Docs
description: Berichten verzenden en ontvangen als batches in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, jonfan, LADocs
ms.topic: article
ms.date: 08/19/2018
ms.openlocfilehash: f60cb79324cad194877402203dbd1706727468d0
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54330718"
---
# <a name="send-receive-and-batch-process-messages-in-azure-logic-apps"></a>Verzenden, ontvangen en verwerken van berichten in Azure Logic Apps voor batch

Als u wilt verzenden en berichten bij elkaar op een specifieke manier verwerken als groepen, kunt u een batchverwerkingsindeling oplossing die worden verzameld van berichten in een *batch* totdat uw opgegeven criteria wordt voldaan voor het vrijgeven van en de batch-berichten worden verwerkt. Batchverwerking, kunt u beperken hoe vaak uw logische app berichten worden verwerkt. Dit artikel wordt beschreven hoe u een batchverwerkingsindeling oplossing kunt maken met het maken van twee logische apps binnen hetzelfde Azure-abonnement, Azure-regio, en deze specifieke volgorde te volgen: 

* De ["batch ontvanger"](#batch-receiver) logische app, die worden geaccepteerd en berichten worden verzameld in een batch tot aan de opgegeven criteria wordt voldaan voor vrijgeven en deze berichten worden verwerkt.

  Zorg ervoor dat u eerst de batch-ontvanger maken zodat u de batch-bestemming later selecteren kunt bij het maken van de batch-afzender.

* Een of meer ['batch zender'](#batch-sender) logic apps, die de berichten naar de ontvanger van de eerder gemaakte batch verzenden. 

   U kunt ook een unieke sleutel, zoals een klantnummer opgeven die *partities* of de doel-batch in logische subsets op basis van deze sleutel wordt verdeeld. Op die manier kunnen kunt de app ontvanger alle items met dezelfde sleutel verzamelen en ze samen worden verwerkt.

Zorg ervoor dat uw batch-ontvanger en de batch-afzender delen hetzelfde Azure-abonnement *en* Azure-regio. Als ze dit niet doet, kunt u de batch-ontvanger niet selecteren bij het maken van de afzender batch omdat ze niet zichtbaar zijn voor elkaar.

## <a name="prerequisites"></a>Vereisten

Als u wilt dit voorbeeld volgen, moet u deze items:

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). Of, [zich aanmelden voor een abonnement op gebruiksbasis](https://azure.microsoft.com/pricing/purchase-options/).

* Een e-mailaccount met een [e-mailprovider ondersteund door Azure Logic Apps](../connectors/apis-list.md)

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md) 

* Voor het gebruik van Visual Studio in plaats van de Azure-portal, zorg ervoor dat [Visual Studio instellen voor het werken met logische Apps](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

<a name="batch-receiver"></a>

## <a name="create-batch-receiver"></a>Maken van batch-ontvanger

Voordat u berichten naar een batch verzenden kunt, moet eerst die partij bestaan als de doellocatie waar u die berichten verzenden. Dus eerst moet u de 'batch ontvanger' logische app, die met begint de **Batch** trigger. Op die manier kunnen bij het maken van de 'zender batch' logische app, kunt u de batch-ontvanger logische app selecteren. De batch-ontvanger blijft verzamelen van berichten, tot aan de opgegeven criteria wordt voldaan voor vrijgeven en deze berichten worden verwerkt. Terwijl de batch-ontvangers niet hoeft te weten niets van afzenders van batch, moeten de afzenders van batch de doellocatie waar ze de berichten verzenden weten. 

1. In de [Azure-portal](https://portal.azure.com) of Visual Studio, een logische app maken met deze naam: "BatchReceiver" 

2. In Logic Apps Designer voegen de **Batch** trigger, die de werkstroom van uw logische app wordt gestart. Typ 'batch' als filter in het zoekvak. Selecteer deze trigger: **Berichten batchgewijs**

   !['Batch berichten' trigger toevoegen](./media/logic-apps-batch-process-send-receive-messages/add-batch-receiver-trigger.png)

3. Deze eigenschappen instelt voor de ontvanger batch: 

   | Eigenschap | Description | 
   |----------|-------------|
   | **Batchmodus** | - **Inline**: Voor het definiëren van releasecriteria in de batchtrigger <br>- **Integratieaccount**: Voor het definiëren van configuraties met meerdere release criteria via een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md). U kunt deze configuraties allemaal op één locatie in plaats van in afzonderlijke logische apps te onderhouden met een integratieaccount. | 
   | **Batchnaam** | De naam voor uw batch-, die in dit voorbeeld 'TestBatch', en is alleen bedoeld voor **Inline** batchmodus |  
   | **Releasecriteria** | Is alleen bedoeld voor **Inline** batch-modus en selecteert de criteria om te voldoen aan voordat elke batch wordt verwerkt: <p>- **Bericht op basis van aantal**: Het aantal berichten voor het verzamelen van 10 berichten in de batch, bijvoorbeeld: <br>- **Op basis van grootte**: De maximale batchgrootte in bytes, voor bijvoorbeeld 10 MB <br>- **Op basis van planning**: Het interval en frequentie van verschillende versies van de batch, bijvoorbeeld tien minuten. De minimale terugkeer is 60 seconden of 1 minuut. Fractionele minuten waarden zijn effectief naar boven afgerond op 1 minuut. Als een datum en tijd opgeven, kiest u **geavanceerde opties weergeven**. <br>- **Selecteer alle**: Gebruik de opgegeven criteria. | 
   ||| 

   In dit voorbeeld ziet u alle criteria, maar voor uw eigen tests, selecteert slechts één criterium:

   ![Geef de details van de Batch-trigger](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-criteria.png)

4. Nu toevoegen een of meer acties waarmee elke batch worden verwerkt. 

   Voor dit voorbeeld wordt een actie toevoegen die een e-mailbericht wordt verzonden wanneer de batchtrigger wordt geactiveerd. 
   De trigger wordt uitgevoerd en een e-mailbericht wordt verzonden wanneer de batch 10 berichten heeft, 10 MB is bereikt, of na 10 minuten doorgegeven.

   1. Kies onder de batchtrigger **nieuwe stap**.

   2. Voer 'e-mail verzenden' als filter in het zoekvak in.
   Op basis van uw e-mailprovider, selecteert u een e-connector.

      Bijvoorbeeld, als u hebt een persoonlijk account is, zoals @outlook.com of @hotmail.com, selecteert u de Outlook.com-connector. 
      Als u een Gmail-account hebt, selecteert u de Gmail-connector. 
      In dit voorbeeld maakt gebruik van Office 365 Outlook. 

   3. Selecteer deze actie: **Stuur een e-mail - <*e-mailprovider*>**

      Bijvoorbeeld:

      ![Selecteer de actie 'Een e-mail verzenden' voor uw e-mailprovider](./media/logic-apps-batch-process-send-receive-messages/batch-receiver-send-email-action.png)

5. Meld u aan bij uw e-mailaccount als dat wordt gevraagd. 

6. Stel de eigenschappen voor de actie die u hebt toegevoegd.

   * Voer het e-mailadres van de ontvanger in het vak **Aan** in. 
   Voor testdoeleinden kunt u uw eigen e-mailadres gebruiken.

   * In de **onderwerp** vak, wanneer de lijst met dynamische inhoud wordt weergegeven, selecteert u de **partitienaam** veld.

     ![Selecteer in de lijst met dynamische inhoud, "Partitienaam"](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details.png)

     U kunt later een unieke partitiesleutel waarmee de doel-batch verdeelt in logische subsets waar kunt u berichten verzenden opgeven in de batch-afzender. 
     Elke set is een uniek nummer dat wordt gegenereerd door de batch afzender logische app. 
     Deze mogelijkheid kunt u één batch met meerdere subsets gebruiken en het definiëren van een bepaalde subset met de naam die u opgeeft.

     > [!IMPORTANT]
     > Een partitie heeft een limiet van 5000 berichten of 80 MB. Als een van beide voorwaarden is voldaan, vrijgeven Logic Apps kan de batch, zelfs als niet aan de gedefinieerde release-voorwaarde wordt voldaan.

   * In de **hoofdtekst** vak, wanneer de lijst met dynamische inhoud wordt weergegeven, selecteert u de **bericht-Id** veld. 

     Ontwerper van logische Apps wordt automatisch een lus 'voor elke' rond de actie voor e-mail verzenden toegevoegd omdat deze actie de uitvoer van de vorige actie als wanneer u een verzameling, in plaats van een batch behandelt. 

     ![Selecteer voor 'Hoofdtekst', '-bericht-Id'](./media/logic-apps-batch-process-send-receive-messages/send-email-action-details-for-each.png)

7.  Sla uw logische app op. U hebt nu de ontvanger van een batch.

    ![Uw logische app opslaan](./media/logic-apps-batch-process-send-receive-messages/save-batch-receiver-logic-app.png)

8. Als u Visual Studio, zorg ervoor dat u [uw batch-ontvanger logische app implementeren in Azure](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). Anders kunt u niet de batch-ontvanger selecteren bij het maken van de batch-afzender.

<a name="batch-sender"></a>

## <a name="create-batch-sender"></a>Maken van batch-afzender

Maak nu een of meer batch afzender logische apps die berichten naar de batch-ontvanger logische app verzenden. In elke batch afzender geeft u de batch-ontvanger en de batchnaam van de, inhoud van het bericht en eventuele andere instellingen. U kunt eventueel een unieke partitiesleutel voor het verdelen van de batch in logische subsets voor het verzamelen van berichten met die sleutel opgeven. 

* Zorg ervoor dat u hebt al [gemaakt van uw batch-ontvanger](#batch-receiver) , zodat wanneer u uw batch-afzender maakt, kunt u de bestaande batch ontvanger als de doel-batch. Terwijl batch ontvangers niet hoeft te weten niets van afzenders van batch, weet batch afzenders om berichten te verzenden. 

* Zorg ervoor dat uw batch-ontvanger en de batch-afzender delen dezelfde Azure-regio *en* Azure-abonnement. Als ze dit niet doet, kunt u de batch-ontvanger niet selecteren bij het maken van de afzender batch omdat ze niet zichtbaar zijn voor elkaar.

1. Een andere logische app maken met deze naam. "BatchSender"

   1. Typ "terugkeerpatroon" als filter in het zoekvak. 
   Selecteer deze trigger: **Herhaling - schema**

      ![De trigger '--terugkeerschema' toevoegen](./media/logic-apps-batch-process-send-receive-messages/add-schedule-trigger-batch-sender.png)

   2. Stel de frequentie en interval om uit te voeren van de afzender logische app elke minuut.

      ![Frequentie en interval voor de trigger met terugkeerpatroon instellen](./media/logic-apps-batch-process-send-receive-messages/recurrence-trigger-batch-sender-details.png)

2. Voeg een nieuwe actie voor het verzenden van berichten aan een batch.

   1. Kies onder de trigger terugkeerpatroon **nieuwe stap**.

   2. Typ 'batch' als filter in het zoekvak. 
   Selecteer de **acties** lijst en selecteer vervolgens deze actie: **Een werkstroom voor logische Apps met batchtrigger - berichten verzenden naar de batch kiezen**

      ![Selecteer 'Kies een Logic Apps-werkstroom met batchtrigger'](./media/logic-apps-batch-process-send-receive-messages/send-messages-batch-action.png)

   3. Selecteer de logische app van uw batch-ontvanger die u eerder hebt gemaakt.

      ![Selecteer 'batch ontvanger' logische app](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch-receiver.png)

      > [!NOTE]
      > De lijst bevat ook een andere logische apps met triggers van batch. 
      > 
      > Als u Visual Studio, en u alle ontvangers batch ziet te selecteren, Controleer of u uw batch-ontvanger in Azure geïmplementeerd. Als u nog niet hebt, krijgt u informatie over het [uw batch-ontvanger logische app implementeren in Azure](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). 

   4. Selecteer deze actie: **Batch_messages - <*uw batch-ontvanger*>**

      ![Selecteer deze actie: "Batch_messages - < uw-logica-app >"](./media/logic-apps-batch-process-send-receive-messages/batch-sender-select-batch.png)

3. De batch van de afzender eigenschappen instellen:

   | Eigenschap | Description | 
   |----------|-------------| 
   | **Batchnaam** | De batchnaam die door de ontvanger logische app, die 'TestBatch' in dit voorbeeld is gedefinieerd <p>**Belangrijke**: De batchnaam van de wordt gevalideerd tijdens runtime en moet overeenkomen met de naam die is opgegeven door de ontvanger logische app. Wijzigen van de batchnaam zorgt ervoor dat de afzender batch mislukken. | 
   | **Inhoud van het bericht** | De inhoud van het bericht dat u wilt verzenden | 
   ||| 

   Voor dit voorbeeld voegt u deze expressie, wordt de huidige datum en tijd ingevoegd in de inhoud van het bericht dat u naar de batch verzendt:

   1. Klik in de **berichtinhoud** vak. 

   2. Wanneer de lijst met dynamische inhoud wordt weergegeven, kiest u **expressie**. 

   3. Voer de expressie `utcnow()`, en kies vervolgens **OK**. 

      ![In "Message Content", 'Expressie' kiest, "utcnow()" Voer en kies "OK".](./media/logic-apps-batch-process-send-receive-messages/batch-sender-details.png)

4. Nu instellen op een partitie voor de batch. Kies in de actie "BatchReceiver" **geavanceerde opties weergeven** en stel deze eigenschappen:

   | Eigenschap | Description | 
   |----------|-------------| 
   | **Naam van de partitie** | Een optionele unieke partitiesleutel te gebruiken voor het verdelen van de doel-batch in logische subsets en verzamelen van berichten op basis van die sleutel | 
   | **Bericht-Id** | Een optioneel bericht-id die een gegenereerde globally unique identifier (GUID) leeg is | 
   ||| 

   In dit voorbeeld in de **partitienaam** vak, voegt u een expressie die een willekeurig getal tussen 1 en 5 genereert. Laat de **bericht-Id** vak leeg zijn.
   
   1. Klik in de **partitienaam** vak zodat de lijst met dynamische inhoud wordt weergegeven. 

   2. Kies in de lijst met dynamische inhoud voor **Expressie**.
   
   3. Voer de expressie `rand(1,6)`, en kies vervolgens **OK**.

      ![Instellen van een partitie voor uw doel-batch](./media/logic-apps-batch-process-send-receive-messages/batch-sender-partition-advanced-options.png)

      Dit **rand** functie genereert een getal tussen 1 en 5. 
      U bent dus deze batch verdelen in vijf genummerde partities, die dynamisch Hiermee stelt u deze expressie.

5. Sla uw logische app op. De afzender logische app ziet er nu uit zoals in dit voorbeeld:

   ![Sla uw logische app van afzender](./media/logic-apps-batch-process-send-receive-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Test uw logische apps

Als u wilt de batchverwerkingsindeling oplossing testen, laat u uw logische apps met een paar minuten. Binnenkort beginnen u met het ophalen van e-mailberichten in groepen van vijf, allemaal met dezelfde partitiesleutel.

Uw logische app van de batch afzender per minuut wordt uitgevoerd, genereert een willekeurig getal tussen 1 en 5 en maakt gebruik van deze gegenereerd nummer als de partitiesleutel voor de doel-batch waarnaar moeten worden verzonden. Telkens wanneer die de batch vijf items met dezelfde partitiesleutel heeft, uw batch-ontvanger logische app wordt gestart en e-mail voor elk bericht wordt verzonden.

> [!IMPORTANT]
> Wanneer u klaar bent testen, zorg ervoor dat de BatchSender logische app als u wilt stoppen met het verzenden van berichten en te voorkomen dat overbelasting van uw postvak in te schakelen.

## <a name="next-steps"></a>Volgende stappen

* [Batch- en verzenden EDI-berichten](../logic-apps/logic-apps-scenario-edi-send-batch-messages.md)
* [Bouwen op definities voor logische Apps met behulp van JSON](../logic-apps/logic-apps-author-definitions.md)
* [Een serverloze app bouwen in Visual Studio met Azure Logic Apps en Functions](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Afhandeling van uitzonderingen en logboekregistratie van fouten voor logische apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

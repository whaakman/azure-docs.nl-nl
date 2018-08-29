---
title: Batch-EDI-berichten verwerken als een groep of een verzameling - Azure Logic Apps | Microsoft Docs
description: EDI-berichten voor batchverwerking in logische apps verzenden
services: logic-apps
ms.service: logic-apps
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, LADocs
ms.topic: article
ms.date: 08/19/2018
ms.openlocfilehash: 7e058b7cebb9c2cdc3fb8b97bf99554b2f26dd8c
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43121572"
---
# <a name="send-edi-messages-in-batches-to-trading-partners-with-azure-logic-apps"></a>EDI-berichten in batches verzenden naar het zakelijke partners met Azure Logic Apps

In scenario's (B2B) voor bedrijven, partners vaak in groepen-berichten uitwisselen of *batches*. Wanneer u een batchverwerkingsindeling oplossing met Logic Apps bouwt, kunt u berichten verzenden naar handelspartners en die berichten samen in batches verwerken. Dit artikel wordt beschreven hoe u kunt batch EDI-berichten verwerken, met behulp van X12 bijvoorbeeld door een 'batch zender' logische app en een 'batch ontvanger' logische app te maken. 

Batchverwerking X12 berichten werkt net als andere berichten; batching u een batchtrigger die berichten worden verzameld in een batch- en een batch-actie waarmee berichten worden verzonden naar de batch. Bovendien X12 batchverwerking bevat een X12 stap codering voordat de berichten gaat u naar de trading partner of andere bestemming. Zie voor meer informatie over de batchtrigger en de actie, [berichten batchgewijs verwerken](../logic-apps/logic-apps-batch-process-send-receive-messages.md).

In dit artikel hebt u een batchverwerkingsindeling oplossing bouwen door twee logische apps in de dezelfde Azure-abonnement, de Azure-regio en het volgende te maken in dit specifieke volgorde:

* Een ["batch ontvanger"](#receiver) logische app, die worden geaccepteerd en berichten worden verzameld in een batch tot aan de opgegeven criteria wordt voldaan voor vrijgeven en deze berichten worden verwerkt. In dit scenario wordt de ontvanger batch ook codeert de berichten in de batch met behulp van de opgegeven X12 overeenkomst of -partner-id's.

  Zorg ervoor dat u eerst de batch-ontvanger maken zodat u de batch-bestemming later selecteren kunt bij het maken van de batch-afzender.

* Een ['batch zender'](#sender) logische app, die de berichten worden verzonden naar de ontvanger van de eerder gemaakte batch. 

Zorg ervoor dat uw batch-ontvanger en de batch-afzender delen hetzelfde Azure-abonnement *en* Azure-regio. Als ze dit niet doet, kunt u de batch-ontvanger niet selecteren bij het maken van de afzender batch omdat ze niet zichtbaar zijn voor elkaar.

## <a name="prerequisites"></a>Vereisten

Als u wilt dit voorbeeld volgen, moet u deze items:

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). Of, [zich aanmelden voor een abonnement op gebruiksbasis](https://azure.microsoft.com/pricing/purchase-options/).

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Een bestaande [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) die is gekoppeld aan uw Azure-abonnement en is gekoppeld aan uw logische apps

* Ten minste twee bestaande [partners](../logic-apps/logic-apps-enterprise-integration-partners.md) in uw integratie-account. Elke partner moet de X12 (Standard Carrier Alpha Code) gebruiken als een bedrijfsidentiteit in de eigenschappen van de partner de kwalificatie.

* Een bestaande [X12 overeenkomst](../logic-apps/logic-apps-enterprise-integration-x12.md) in uw integratieaccount

* Voor het gebruik van Visual Studio in plaats van de Azure-portal, zorg ervoor dat [Visual Studio instellen voor het werken met logische Apps](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md).

<a name="receiver"></a>

## <a name="create-x12-batch-receiver"></a>Maken van X12 batch ontvanger

Voordat u berichten naar een batch verzenden kunt, moet eerst die partij bestaan als de doellocatie waar u die berichten verzenden. Dus eerst moet u de 'batch ontvanger' logische app, die met begint de **Batch** trigger. Op die manier kunnen bij het maken van de 'zender batch' logische app, kunt u de batch-ontvanger logische app selecteren. De batch-ontvanger blijft verzamelen van berichten, tot aan de opgegeven criteria wordt voldaan voor vrijgeven en deze berichten worden verwerkt. Terwijl de batch-ontvangers niet hoeft te weten niets van afzenders van batch, moeten de afzenders van batch de doellocatie waar ze de berichten verzenden weten. 

Voor dit batch-ontvanger, geeft u de batchmodus, de naam, de releasecriteria, X12 overeenkomst en andere instellingen. 

1. In de [Azure-portal](https://portal.azure.com) of Visual Studio, een logische app maken met deze naam: "BatchX12Messages"

2. [Uw logische app koppelen aan uw integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account).

3. In Logic Apps Designer voegen de **Batch** trigger, die de werkstroom van uw logische app wordt gestart. Typ 'batch' als filter in het zoekvak. Selecteer deze trigger: **Batch-berichten**

   ![Batch-trigger toevoegen](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

4. De batch ontvanger eigenschappen instellen: 

   | Eigenschap | Waarde | Opmerkingen | 
   |----------|-------|-------|
   | **Batchmodus** | Inline |  |  
   | **Batchnaam** | TestBatch | Alleen beschikbaar bij **Inline** batchmodus | 
   | **Releasecriteria** | Aantal berichten is gebaseerd, op basis van planning | Alleen beschikbaar bij **Inline** batchmodus | 
   | **Aantal berichten** | 10 | Alleen beschikbaar bij **bericht op basis van aantal** releasecriteria | 
   | **Interval** | 10 | Alleen beschikbaar bij **schema op basis van** releasecriteria | 
   | **Frequentie** | minuut | Alleen beschikbaar bij **schema op basis van** releasecriteria | 
   ||| 

   ![Geef de details van de batch-trigger](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-release-criteria.png)

   > [!NOTE]
   > In dit voorbeeld instellen niet met een partitie voor de batch, zodat elke batch maakt gebruik van dezelfde partitiesleutel. Zie voor meer informatie over partities, [berichten batchgewijs verwerken](../logic-apps/logic-apps-batch-process-send-receive-messages.md#batch-sender).

5. Nu een actie toevoegen die elke batch codeert: 

   1. Kies onder de batchtrigger **nieuwe stap**.

   2. Voer in het zoekvak 'X 12 batch' als filter en selecteer deze actie (alle versies): **Batch versleutelen <*versie*>-X12** 

      ![Selecteer X12 actie Batch versleutelen](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)

   3. Als u niet eerder verbinding met uw integratie-account maken, moet u de verbinding nu maken. Geef een naam op voor de verbinding, selecteert u de integratie-account u wilt en kies vervolgens **maken**.

      ![Verbinding maken tussen batch encoder en integratie-account](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encoder-connect-integration-account.png)

   4. Stel deze eigenschappen voor uw batch-encoder-actie:

      | Eigenschap | Beschrijving |
      |----------|-------------|
      | **Naam van X12 overeenkomst** | Open de lijst en selecteer uw bestaande overeenkomst. <p>Als de lijst leeg is, zorg ervoor dat u [uw logische app koppelen aan het integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md#link-account) waarvoor de overeenkomst die u wilt. | 
      | **BatchName** | In dit vak klikt, en nadat de lijst met dynamische inhoud wordt weergegeven, selecteert u de **batchnaam** token. | 
      | **%{PartitionName/** | In dit vak klikt, en nadat de lijst met dynamische inhoud wordt weergegeven, selecteert u de **partitienaam** token. | 
      | **items** | Sluit het vak van de details van item en vervolgens klikt u in dit vak. Nadat de lijst met dynamische inhoud wordt weergegeven, selecteert u de **batchgewijs Items** token. | 
      ||| 

      ![Batch-actiegegevens coderen](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

      Voor de **Items** vak:

      ![Batch coderen actie-items](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-items.png)

6. Sla uw logische app op. 

7. Als u Visual Studio, zorg ervoor dat u [uw batch-ontvanger logische app implementeren in Azure](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md#deploy-logic-app-to-azure). Anders kunt u niet de batch-ontvanger selecteren bij het maken van de batch-afzender.

### <a name="test-your-logic-app"></a>Uw logische app testen

Als u wilt controleren of uw batch-ontvanger werkt zoals verwacht, kunt u een HTTP-actie voor testdoeleinden toevoegen en een batch verzenden naar de [aanvragen Bin service](https://requestbin.fullcontact.com/). 

1. Onder de X12 coderen actie, kiest u **nieuwe stap**. 

2. Typ 'http' als filter in het zoekvak. Selecteer deze actie: **HTTP - HTTP**
    
   ![HTTP-actie selecteren](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-add-http-action.png)

3. Stel de eigenschappen voor de HTTP-actie:

   | Eigenschap | Beschrijving | 
   |----------|-------------|
   | **Methode** | Selecteer in deze lijst **POST**. | 
   | **URI** | Een URI voor de opslaglocatie van uw aanvraag genereren, en voer vervolgens deze URI in dit vak. | 
   | **Hoofdtekst** | In dit vak klikt, en nadat de lijst met dynamische inhoud wordt geopend, selecteert u de **hoofdtekst** token, dat wordt weergegeven in de sectie, **als Batch versleutelen op Overeenkomstnaam**. <p>Als er geen de **hoofdtekst** token, naast **als Batch versleutelen op Overeenkomstnaam**, selecteer **meer**. | 
   ||| 

   ![Geef details van HTTP-actie](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-add-http-action-details.png)

4. Sla uw logische app op. 

   Uw batch-ontvanger logische app ziet eruit zoals in dit voorbeeld: 

   ![Sla uw logische app van de batch-ontvanger](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receiver-finished.png)

<a name="sender"></a>

## <a name="create-x12-batch-sender"></a>Maken van X12 batch-afzender

Maak nu een of meer logische apps die berichten naar de batch-ontvanger logische app verzenden. In elke batch afzender geeft u de batch-ontvanger logische app en de batchnaam van de, inhoud van het bericht en eventuele andere instellingen. U kunt eventueel een unieke partitiesleutel voor het verdelen van de batch in subsets voor het verzamelen van berichten met die sleutel opgeven. 

* Zorg ervoor dat u hebt al [gemaakt van uw batch-ontvanger](#receiver) , zodat wanneer u uw batch-afzender maakt, kunt u de bestaande batch ontvanger als de doel-batch. Terwijl batch ontvangers niet hoeft te weten niets van afzenders van batch, weet batch afzenders om berichten te verzenden. 

* Zorg ervoor dat uw batch-ontvanger en de batch-afzender delen dezelfde Azure-regio *en* Azure-abonnement. Als ze dit niet doet, kunt u de batch-ontvanger niet selecteren bij het maken van de afzender batch omdat ze niet zichtbaar zijn voor elkaar.

1. Een andere logische app maken met deze naam: "SendX12MessagesToBatch" 

2. Typ 'wanneer een http-aanvraag' als filter in het zoekvak. Selecteer deze trigger: **wanneer een HTTP-aanvraag wordt ontvangen** 
   
   ![De aanvraag-trigger toevoegen](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

3. Een actie voor het verzenden van berichten aan een batch toevoegen.

   1. Kies onder de HTTP-aanvraagbewerking **nieuwe stap**.

   2. Typ 'batch' als filter in het zoekvak. 
   Selecteer de **acties** lijst en selecteer vervolgens deze actie: **kiest u een werkstroom voor Logic Apps met batchtrigger - berichten verzenden naar batches**

      ![Selecteer 'Kies een Logic Apps-werkstroom met batchtrigger'](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-trigger.png)

   3. Selecteer nu de 'BatchX12Messages' logische app die u eerder hebt gemaakt.

      ![Selecteer 'batch ontvanger' logische app](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-receiver.png)

   4. Selecteer deze actie: **Batch_messages - <*uw batch-ontvanger*>**

      ![Selecteer de actie "Batch_messages"](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-select-batch-messages-action.png)

4. De batch van de afzender eigenschappen instellen.

   | Eigenschap | Beschrijving | 
   |----------|-------------| 
   | **Batchnaam** | De batchnaam die door de ontvanger logische app, die 'TestBatch' in dit voorbeeld is gedefinieerd <p>**Belangrijke**: de batchnaam van de wordt gevalideerd tijdens runtime en moet overeenkomen met de naam die is opgegeven door de ontvanger logische app. Wijzigen van de batchnaam zorgt ervoor dat de afzender batch mislukken. | 
   | **Inhoud van het bericht** | De inhoud van het bericht dat u verzenden wilt, dit is de **hoofdtekst** token in dit voorbeeld | 
   ||| 
   
   ![Batch-eigenschappen instellen](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-set-batch-properties.png)

5. Sla uw logische app op. 

   De batch afzender logische app ziet eruit zoals in dit voorbeeld:

   ![Sla uw logische app van de batch-afzender](./media/logic-apps-scenario-EDI-send-batch-messages/batch-sender-finished.png)

## <a name="test-your-logic-apps"></a>Test uw logische apps

Als u wilt de batchverwerkingsindeling oplossing testen, post X12 berichten naar uw batch-afzender logische app van [Postman](https://www.getpostman.com/postman) of een vergelijkbaar hulpprogramma. Binnenkort u beginnen met het ophalen van X12 berichten in de opslaglocatie van uw aanvraag, elke 10 minuten of in batches van 10, allemaal met dezelfde partitiesleutel.

## <a name="next-steps"></a>Volgende stappen

* [Verwerken van berichten als batches](../logic-apps/logic-apps-batch-process-send-receive-messages.md) 

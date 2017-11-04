---
title: Batch-EDI-berichten verwerken als een groep of een verzameling - Azure Logic Apps | Microsoft Docs
description: EDI-berichten voor batchverwerking in logic apps verzenden
keywords: batch, batch verwerkt, batch coderen
author: divswa
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
ms.date: 09/21/2017
ms.author: LADocs; estfan; divswa
ms.openlocfilehash: 837cb0d9595da5b5bd4f01fb4576f75e98ab8912
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="send-x12-messages-in-batch-to-trading-partners"></a>Verzenden X12 berichten in een batch handelspartners

In scenario's (B2B) van bedrijven uitwisselen partners vaak berichten in groepen of batches. Om berichten te verzenden in groepen of batches bij handelspartners, kunt u een batch met meerdere items en vervolgens gebruik de X12 batch actie die items als een batch te verwerken.


Batchverwerking voor X12 berichten, zoals andere berichten, maakt gebruik van een batch-trigger en action. Ook voor X12, de batch verloopt via een X12 coderen stap voordat u verdergaat met de partner of een andere bestemming. Zie voor meer informatie over de batch-trigger en action [Batch verwerken van berichten](logic-apps-batch-process-send-receive-messages.md).

Dit onderwerp leest u hoe u X12 kunt verwerken berichten als batch door deze taken uitvoeren:
* [Maken van een logische app dat items ontvangt, en maakt u een batch](#receiver). Deze 'ontvanger' logische app worden deze acties uitgevoerd:
 
   * Hiermee geeft u de batch-naam en release criteria om te voldoen aan voordat de items als een batch zijn vrijgegeven.

   * Verwerkt of de items in de batch met behulp van de opgegeven X12 codeert identiteiten overeenkomst of partner.

* [Maken van een logische app waarmee items worden verzonden naar een batch](#sender). Deze 'afzender' logische app geeft aan waar items voor batches, dit een bestaande ontvanger logic app moeten verzenden.


## <a name="prerequisites"></a>Vereisten

Wilt u dit voorbeeld gebruiken, moet u deze items:

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). U kunt eventueel ook direct kiezen voor [een Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Een [integratie account](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement

* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die u hebt gedefinieerd in uw account integratie. Zorg ervoor dat elke partner gebruikmaakt van de X12 (standaard Carrier Alpha Code) met de kwalificatie in de eigenschappen van de partner als een bedrijfsidentiteit.

* Een [X12 overeenkomst](logic-apps-enterprise-integration-x12.md) die al is gedefinieerd in uw account integratie

<a name="receiver"></a>

## <a name="create-a-logic-app-that-receives-x12-messages-and-creates-a-batch"></a>Maken van een logische app die X12 ontvangt berichten en maakt u een batch

Voordat u berichten naar een batch verzenden kunt, moet u eerst een 'ontvanger' logische app met maken de **Batch** trigger. Op die manier kunt u deze ontvanger logic app bij het maken van de afzender logische app. Voor de ontvanger u Geef de batchnaam, criteria, X12 release overeenkomst en andere instellingen. 


1. In de [Azure-portal](https://portal.azure.com), een logische app maken met deze naam: 'BatchX12Messages'.

2. In Logic Apps Designer, voegt u de **Batch** trigger uw logische app-werkstroom op te starten. Voer in het zoekvak 'batch' als filter. Selecteer deze trigger: **Batch: Batch-berichten**

   ![Batch-trigger toevoegen](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

3. Geef een naam voor de batch en Geef criteria op voor het vrijgeven van de batch, bijvoorbeeld:

   * **Batch-naam**: de naam die wordt gebruikt voor het identificeren van de batch is 'TestBatch' in dit voorbeeld.

   * **Release Criteria**: de batch release criteria, die kunnen worden gebaseerd op het aantal berichten, planning, of beide.
   
     ![Geef details op Batch trigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-release-criteria.png)

   * **Aantal berichten**: het aantal berichten voor het opslaan als een batch voordat voor verwerking, namelijk '5' in dit voorbeeld zijn vrijgegeven.

     ![Geef details op Batch trigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-count-based.png)

   * **Planning**: het schema van de batch release voor verwerking 'elke 10 minuten' in dit voorbeeld is.

     ![Geef details op Batch trigger](./media/logic-apps-scenario-EDI-send-batch-messages/receive-batch-schedule-based.png)


4. Toevoegen van een andere actie die de gegroepeerde codeert of batches van berichten en maakt een X12 bericht batch verwerkt. 

   a. Kies **+ een nieuwe stap** > **een actie toevoegen**.

   b. Voer in het zoekvak 'X 12 batch' als filter en selecteer een actie voor **X12-Batch coderen**. De X12, zoals coderen connector, wordt er meerdere variaties voor batch codering in te grijpen. U kunt een van beide te selecteren.

   ![Selecteer X12 actie Batch coderen](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)
   
5. Stel de eigenschappen voor de actie die u zojuist hebt toegevoegd.

   * In de **naam van X12 overeenkomst** vak, selecteert u de overeenkomst in de vervolgkeuzelijst. Als de lijst leeg is, zorg er dan voor dat u geen verbinding gemaakt met uw account integratie.

   * In de **BatchName** de optie de **batchnaam** veld in de lijst met dynamische inhoud.
   
   * In de **PartitionName** de optie de **partitienaam** veld in de lijst met dynamische inhoud.

   * In de **Items** de optie de **batch opgenomen Items** uit de lijst met dynamische inhoud.

   ![Actie van de Batchdetails coderen](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

6. Voor testdoeleinden, een HTTP-actie voor het verzenden van de batch-bericht naar toevoegen [aanvragen Bin service](https://requestbin.fullcontact.com/). 

   1. Voer 'HTTP' als filter in het zoekvak. Deze actie selecteert: **HTTP - HTTP**
    
      ![Selecteer HTTP-actie](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action.png)

   2. Van de **methode** selecteert **POST**. Voor de **Uri** vak, een URI genereren voor de opslaglocatie van uw aanvraag en voert u deze URI. In de **hoofdtekst** vak wanneer de dynamische lijst wordt geopend, selecteer de **hoofdtekst** veld onder de **Batch coderen met de naam van de overeenkomst** sectie. Als er geen **hoofdtekst**, kies **Zie voor meer informatie** naast **Batch coderen met de naam van de overeenkomst**.

      ![Geef details op http-actie](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action-details.png)

7.  Nu dat u een ontvanger logische app hebt gemaakt, sla uw logische app.

    ![Uw logische app opslaan](./media/logic-apps-scenario-EDI-send-batch-messages/save-batch-receiver-logic-app.png)

    > [!IMPORTANT]
    > Een partitie heeft een limiet van 5000 berichten of 80 MB. Als beide voorwaarden is voldaan, kan de batch worden vrijgegeven, zelfs als niet aan de gebruiker gedefinieerde voorwaarde wordt voldaan.

<a name="sender"></a>

## <a name="create-a-logic-app-that-sends-x12-messages-to-a-batch"></a>Maken van een logische app waarmee X12 verzonden berichten aan een batch

Maak nu een of meer logische apps die items naar de batch gedefinieerd door de ontvanger logische app verzendt. Voor de afzender geeft u de ontvanger logische app en de batchnaam van de, inhoud van het bericht en eventuele andere instellingen. U kunt eventueel een unieke partitiesleutel voor het delen van de batch in deelverzamelingen voor het verzamelen van items met die sleutel opgeven.

Afzender logische apps moeten weten waar u het verzenden van items, terwijl een ontvanger logische apps hoeft niet te weten over de afzenders.


1. Een andere logische app maken met deze naam: 'X12MessageSender'. Deze trigger toevoegen aan uw logische app: **vraag / antwoord - aanvraag** 
   
   ![De aanvraag-trigger toevoegen](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

2. Voeg een nieuwe stap voor het verzenden van berichten naar een batch.

   1. Kies **+ een nieuwe stap** > **een actie toevoegen**.

   2. Voer in het zoekvak 'batch' als filter. 

3. Deze actie selecteert: **berichten verzenden naar batch: Kies een werkstroom Logic Apps met batch-trigger**

   ![Selecteer 'Verzenden van berichten in een batch moet'](./media/logic-apps-scenario-EDI-send-batch-messages/send-messages-batch-action.png)

4. Nu uw 'BatchX12Messages' logische app die u eerder hebt gemaakt, die nu wordt weergegeven als een actie selecteren.

   !['Batch ontvanger' logische app selecteren](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-receiver.png)

   > [!NOTE]
   > De lijst bevat ook andere logic apps waarvoor de batch-triggers.

5. De Batcheigenschappen instellen.

   * **Batch-naam**: de batchnaam gedefinieerd door de ontvanger logische app, die 'TestBatch' in dit voorbeeld en wordt gevalideerd tijdens runtime.

     > [!IMPORTANT]
     > Zorg ervoor dat u de batchnaam, die moet overeenkomen met de batchnaam die wordt opgegeven door de ontvanger logische app niet te wijzigen.
     > Als de batchnaam van de wijzigt, wordt de afzender logische app mislukken.

   * **Inhoud bericht**: de inhoud van het bericht dat u wilt verzenden naar de batch
   
   ![Eigenschappen van batch instellen](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-properties.png)

6. Sla uw logische app. Uw afzender logische app nu er ongeveer als volgt in dit voorbeeld:

   ![Sla uw logische app van afzender](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-finished.png)

## <a name="test-your-logic-apps"></a>Uw logische apps testen

Als u wilt testen van uw batchen oplossing post X12 berichten naar uw app afzender logica van [Postman](https://www.getpostman.com/postman) of vergelijkbare hulpprogramma. Snel, moet u eerst aan X12 berichten, hetzij als een batch van vijf items of elke 10 minuten in de opslaglocatie van uw aanvraag - alle met dezelfde partitiesleutel.

## <a name="next-steps"></a>Volgende stappen

* [Verwerken van berichten als batches](logic-apps-batch-process-send-receive-messages.md) 
* [Een zonder server-App in Visual Studio met Azure Logic Apps en functies](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Afhandeling van uitzonderingen en foutenregistratie voor logic apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

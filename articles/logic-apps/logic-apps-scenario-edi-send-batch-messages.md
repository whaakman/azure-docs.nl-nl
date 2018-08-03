---
title: Batch-EDI-berichten verwerken als een groep of een verzameling - Azure Logic Apps | Microsoft Docs
description: EDI-berichten voor batchverwerking in logische apps verzenden
keywords: verwerken van batch-, batch, batch versleutelen
author: divswa
manager: jeconnoc
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: LADocs; estfan; divswa
ms.openlocfilehash: fb15688968cb29039fc669ed6b8685ba64df9e81
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432130"
---
# <a name="send-x12-messages-in-batch-to-trading-partners"></a>Berichten verzenden X12 in een batch te handelspartners

In scenario's (B2B) voor bedrijven wisselen partners vaak berichten in batches of in groepen. Voor het verzenden van berichten in groepen of batch-bestanden voor handelspartners, kunt u een batch met meerdere items en gebruik de X12 batch de actie voor het verwerken van deze items als een batch.


Batchverwerking voor X12 berichten, zoals andere berichten, maakt gebruik van een batchtrigger en een actie. Ook voor X12, de batch verloopt via een X12 coderen stap voordat u verdergaat met de partner of een andere bestemming. Zie voor meer informatie over de batchtrigger en actie [berichten batchgewijs verwerken](logic-apps-batch-process-send-receive-messages.md).

Dit onderwerp leest u hoe u X12 kunt verwerken berichten als batch door het uitvoeren van deze taken:
* [Maak een logische app die items ontvangt en maakt u een batch](#receiver). Deze 'ontvanger' logische app de volgende bewerkingen uitgevoerd:
 
   * Hiermee geeft u de batch-naam en release criteria om te voldoen aan voordat de items die zijn vrijgegeven als een batch.

   * Verwerkt of de items in de batch met behulp van de opgegeven X12 codeert overeenkomst of -partner-id's.

* [Maak een logische app waarmee items worden verzonden naar een batch](#sender). Deze logische app van 'zender' geeft aan waar-items voor batchverwerking, dit een bestaande ontvanger logische app moeten verzendt.


## <a name="prerequisites"></a>Vereisten

Als u wilt dit voorbeeld volgen, moet u deze items:

* Een Azure-abonnement. Als u geen abonnement hebt, kunt u [beginnen met een gratis Azure-account](https://azure.microsoft.com/free/). U kunt eventueel ook direct kiezen voor [een Betalen per gebruik-abonnement](https://azure.microsoft.com/pricing/purchase-options/).

* Een [integratieaccount](logic-apps-enterprise-integration-create-integration-account.md) die al is gedefinieerd en die zijn gekoppeld aan uw Azure-abonnement

* Ten minste twee [partners](logic-apps-enterprise-integration-partners.md) die u hebt opgegeven in het integratieaccount. Zorg ervoor dat de X12 (Standard Carrier Alpha Code) maakt gebruik van elke partner kwalificatie in de eigenschappen van de partner als de bedrijfsidentiteit van een.

* Een [X12 overeenkomst](logic-apps-enterprise-integration-x12.md) die al gedefinieerd in uw integratieaccount

<a name="receiver"></a>

## <a name="create-a-logic-app-that-receives-x12-messages-and-creates-a-batch"></a>Hiermee maakt u een logica-app die X12 ontvangt berichten en maakt u een batch

Voordat u berichten naar een batch verzenden kunt, moet u eerst maken een 'ontvanger' logische app met de **Batch** trigger. Op die manier kunt u de deze ontvanger logische app selecteren bij het maken van de afzender logische app. Voor de ontvanger, u geeft u de batchnaam van de, releasecriteria, X12 overeenkomst en andere instellingen. 


1. In de [Azure-portal](https://portal.azure.com), een logische app maken met deze naam: 'BatchX12Messages'.

1. In Logic Apps Designer voegen de **Batch** trigger, die de werkstroom van uw logische app wordt gestart. Typ 'batch' als filter in het zoekvak. Selecteer deze trigger: **Batch: batchberichten**

   ![Batch-trigger toevoegen](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-receiver-trigger.png)

1. Geef een naam voor de batch en Geef criteria op voor het vrijgeven van de batch, bijvoorbeeld:

   * **Batch-naam**: de naam die wordt gebruikt voor het identificeren van de batch, namelijk 'TestBatch' in dit voorbeeld.

   * **Releasecriteria**: de criteria voor batch vrijgeven, die kunnen worden gebaseerd op het aantal berichten, planning, of beide.
   
     ![Geef de details van de Batch-trigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-release-criteria.png)

   * **Aantal berichten**: het aantal berichten om op te slaan als een batch voordat voor verwerking, namelijk '5' in dit voorbeeld zijn vrijgegeven.

     ![Geef de details van de Batch-trigger](./media/logic-apps-batch-process-send-receive-messages/receive-batch-count-based.png)

   * **Planning**: de batch release-planning voor verwerking, namelijk 'elke 10 minuten"in dit voorbeeld.

     ![Geef de details van de Batch-trigger](./media/logic-apps-scenario-EDI-send-batch-messages/receive-batch-schedule-based.png)


1. Voeg nog een actie die de gegroepeerde codeert of berichten batches en maakt u een X12 batchgewijs bericht. 

   a. Kies **+ nieuwe stap** > **een actie toevoegen**.

   b. Voer in het zoekvak 'X 12 batch' als filter en selecteer een actie voor **X12-Batch versleutelen**. Als de X12 connector coderen, wordt er meerdere varianten voor batch actie-codering. U kunt een van beide selecteren.

   ![Selecteer X12 actie Batch versleutelen](./media/logic-apps-scenario-EDI-send-batch-messages/add-batch-encode-action.png)
   
1. Stel de eigenschappen voor de actie die u zojuist hebt toegevoegd.

   * In de **naam van X12 overeenkomst** vak, selecteert u de overeenkomst in de vervolgkeuzelijst. Als de lijst leeg is, zorg ervoor dat u een verbinding naar uw integratie-account hebt gemaakt.

   * In de **BatchName** Schakel de **batchnaam** veld uit de lijst met dynamische inhoud.
   
   * In de **%{PartitionName/** Schakel de **partitienaam** veld uit de lijst met dynamische inhoud.

   * In de **Items** Schakel de **batchgewijs Items** uit de lijst met dynamische inhoud.

   ![Batch coderen actiegegevens](./media/logic-apps-scenario-EDI-send-batch-messages/batch-encode-action-details.png)

1. Voor testdoeleinden toevoegen een HTTP-actie voor het verzenden van de batch bericht [aanvragen Bin service](https://requestbin.fullcontact.com/). 

   1. Typ 'HTTP' als filter in het zoekvak in. Selecteer deze actie: **HTTP - HTTP**
    
      ![HTTP-actie selecteren](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action.png)

   1. Uit de **methode** in de lijst met **POST**. Voor de **Uri** vak, een URI voor de opslaglocatie van uw aanvraag genereren en voert u deze URI. In de **hoofdtekst** vak, wanneer de lijst met dynamische wordt geopend, selecteert u de **hoofdtekst** veld onder de **als Batch versleutelen op Overeenkomstnaam** sectie. Als er geen **hoofdtekst**, kiest u **meer** naast **als Batch versleutelen op Overeenkomstnaam**.

      ![Geef details van HTTP-actie](./media/logic-apps-scenario-EDI-send-batch-messages/batch-receive-add-http-action-details.png)

1.  Nu dat u een ontvanger logische app gemaakt, kunt u uw logische app opslaan.

    ![Uw logische app opslaan](./media/logic-apps-scenario-EDI-send-batch-messages/save-batch-receiver-logic-app.png)

    > [!IMPORTANT]
    > Een partitie heeft een limiet van 5000 berichten of 80 MB. Als een van beide voorwaarden is voldaan, kan de batch worden vrijgegeven, zelfs als niet aan de gebruiker gedefinieerde voorwaarde wordt voldaan.

<a name="sender"></a>

## <a name="create-a-logic-app-that-sends-x12-messages-to-a-batch"></a>Maken van een logische app die X12 verzendt berichten aan een batch

Maak nu een of meer logische apps die items naar de batch die zijn gedefinieerd door de ontvanger logische app verzendt. Voor de afzender geeft u de ontvanger logische app en de batchnaam, de inhoud van het bericht en de andere instellingen. U kunt eventueel een unieke partitiesleutel voor het verdelen van de batch in subsets voor het verzamelen van items met die sleutel opgeven.

Afzender logische apps moeten weten waar ze voor het verzenden van items, terwijl de ontvanger logische apps niet hoeft te weten niets van de afzenders te maken.


1. Een andere logische app maken met deze naam: 'X12MessageSender'. Deze trigger toevoegen aan uw logische app: **aanvraag / antwoord - aanvraag** 
   
   ![De aanvraag-trigger toevoegen](./media/logic-apps-scenario-EDI-send-batch-messages/add-request-trigger-sender.png)

1. Voeg een nieuwe stap voor het verzenden van berichten aan een batch.

   1. Kies **+ nieuwe stap** > **een actie toevoegen**.

   1. Typ 'batch' als filter in het zoekvak. 

1. Selecteer deze actie: **berichten verzenden naar batch: een werkstroom voor logische Apps met batchtrigger kiezen**

   ![Selecteer 'Berichten naar batches verzenden'](./media/logic-apps-scenario-EDI-send-batch-messages/send-messages-batch-action.png)

1. Selecteer nu uw 'BatchX12Messages' logische app die u eerder hebt gemaakt, die nu wordt weergegeven als een actie.

   ![Selecteer 'batch ontvanger' logische app](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-receiver.png)

   > [!NOTE]
   > De lijst bevat ook een andere logische apps met triggers van batch.

1. De Batcheigenschappen instellen.

   * **Batch-naam**: de batchnaam die door de ontvanger logische app, die 'TestBatch' in dit voorbeeld is en wordt gevalideerd tijdens runtime gedefinieerd.

     > [!IMPORTANT]
     > Zorg ervoor dat u de batchnaam, die moet overeenkomen met de batchnaam die opgegeven door de ontvanger logische app niet te wijzigen.
     > Wijzigen van de batchnaam zorgt ervoor dat de afzender logische app te laten mislukken.

   * **Bericht inhoud**: de inhoud van het bericht dat u wilt verzenden naar de batch
   
   ![Batch-eigenschappen instellen](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-select-batch-properties.png)

1. Sla uw logische app op. De afzender logische app ziet er nu uit zoals in dit voorbeeld:

   ![Sla uw logische app van afzender](./media/logic-apps-scenario-EDI-send-batch-messages/send-batch-finished.png)

## <a name="test-your-logic-apps"></a>Test uw logische apps

Als u wilt de batchverwerkingsindeling oplossing testen, post X12 berichten naar de afzender logische app [Postman](https://www.getpostman.com/postman) of een vergelijkbaar hulpprogramma. Snel, moet u zich eerst aan X12 berichten, hetzij als een batch van vijf items of elke 10 minuten in uw aanvraag bin - allemaal met dezelfde partitiesleutel.

## <a name="next-steps"></a>Volgende stappen

* [Verwerken van berichten als batches](logic-apps-batch-process-send-receive-messages.md) 
* [Een serverloze app bouwen in Visual Studio met Azure Logic Apps en Functions](../logic-apps/logic-apps-serverless-get-started-vs.md)
* [Afhandeling van uitzonderingen en logboekregistratie van fouten voor logische apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)

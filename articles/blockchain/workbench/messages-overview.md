---
title: Azure Blockchain Workbench berichten integratie, overzicht
description: Overzicht van het gebruik van berichten in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 02/01/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: 48e7de2798d9c34942df281febcc1d4ec443010d
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55662224"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>Azure Blockchain Workbench messaging-integratie

Naast het bieden van een REST-API, biedt Azure Blockchain Workbench ook integratie op basis van berichten. Workbench publiceert grootboek gerichte gebeurtenissen via Azure Event Grid, waarmee de downstream consument opname van gegevens of maatregelen nemen op basis van deze gebeurtenissen. Voor deze clients waarvoor betrouwbare uitwisseling van berichten, levert Azure Blockchain Workbench berichten naar een Azure Service Bus-eindpunt.

## <a name="input-apis"></a>Invoer-API 's

Als u starten van transacties van externe systemen gebruikers maken wilt, contracten, en overeenkomsten bijwerken, kunt u berichten van de invoer-API's gebruiken transacties uitvoeren op een grootboek. Zie [messaging integratie voorbeelden](https://aka.ms/blockchain-workbench-integration-sample) voor een voorbeeld waarin wordt gedemonstreerd invoer API's.

Hier volgen de momenteel beschikbare API's-invoer.

### <a name="create-user"></a>Gebruiker maken

Hiermee maakt u een nieuwe gebruiker.

De aanvraag moet de volgende velden:

| **Naam**             | **Beschrijving**                                      |
|----------------------|------------------------------------------------------|
| requestId            | Client wordt geleverd GUID                                |
| firstName            | De voornaam van de gebruiker                              |
| lastName             | De achternaam van de gebruiker                               |
| emailAddress         | E-mailadres van de gebruiker                           |
| externalId           | Azure AD-object-ID van de gebruiker                      |
| connectionId         | De unieke id voor de blockchain-verbinding |
| messageSchemaVersion | Messaging schemaversie                            |
| messageName          | **CreateUserRequest**                               |

Voorbeeld:

``` json
{
    "requestId": "e2264523-6147-41fc-bbbb-edba8e44562d",
    "firstName": "Ali",
    "lastName": "Alio",
    "emailAddress": "aa@contoso.com",
    "externalId": "6a9b7f65-ffff-442f-b3b8-58a35abd1bcd",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateUserRequest"
}
```

Blockchain Workbench retourneert een antwoord met de volgende velden:

| **Naam**              | **Beschrijving**                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------|
| requestId             | Client wordt geleverd GUID |
| userId                | ID van de gebruiker die is gemaakt |
| userChainIdentifier   | Adres van de gebruiker die is gemaakt op de blockchain-netwerk. In Ethereum, is het adres van de gebruiker **in de chain** adres. |
| connectionId          | De unieke id voor de blockchain-verbinding|
| messageSchemaVersion  | Messaging schemaversie |
| messageName           | **CreateUserUpdate** |
| status                | De status van de aanvraag voor het maken van gebruiker.  Als lukt, waarde is **succes**. Bij fouten, ligt **fout**.     |
| AdditionalInformation | Als u meer informatie vindt u op basis van de status |

Voorbeeld van de geslaagde **gebruiker maken** reactie van Blockchain Workbench:

``` json
{ 
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Success", 
    "additionalInformation": { } 
} 
```

Als de aanvraag mislukt is, wordt informatie over de fout zijn opgenomen in als u meer informatie.

``` json
{
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": null, 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Failure", 
    "additionalInformation": { 
        "errorCode": 4000, 
        "errorMessage": "User cannot be provisioned on connection." 
    }
}
```

### <a name="create-contract"></a>Contract maken

Hiermee maakt u een nieuwe overeenkomst.

De aanvraag moet de volgende velden:

| **Naam**             | **Beschrijving**                                                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId            | Client wordt geleverd GUID |
| userChainIdentifier  | Adres van de gebruiker die is gemaakt op de blockchain-netwerk. In Ethereum, is dit adres van de gebruiker **van keten** adres. |
| applicationName      | Naam van de toepassing |
| versie              | De versie van de toepassing. Vereist als er meerdere versies van de toepassing ingeschakeld. Anders wordt de versie is optioneel. Zie voor meer informatie over versiebeheer van de toepassing [Azure Blockchain Workbench toepassing versiebeheer](version-app.md). |
| workflowName         | Naam van de werkstroom |
| parameters           | Invoer van de parameters voor het contract maken |
| connectionId         | De unieke id voor de blockchain-verbinding |
| messageSchemaVersion | Messaging schemaversie |
| messageName          | **CreateContractRequest** |

Voorbeeld:

``` json
{ 
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211", 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "applicationName": "AssetTransfer",
    "version": "1.0",
    "workflowName": "AssetTransfer", 
    "parameters": [ 
        { 
            "name": "description", 
            "value": "a 1969 dodge charger" 
        }, 
        { 
            "name": "price", 
            "value": "12345" 
        } 
    ], 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateContractRequest" 
}
```

Blockchain Workbench retourneert een antwoord met de volgende velden:

| **Naam**                 | **Beschrijving**                                                                   |
|--------------------------|-----------------------------------------------------------------------------------|
| requestId                | Client wordt geleverd GUID                                                             |
| ContractId               | De unieke id voor het contract binnen Azure Blockchain Workbench |
| contractLedgerIdentifier | Adres van het contract op het grootboek                                            |
| connectionId             | De unieke id voor de blockchain-verbinding                               |
| messageSchemaVersion     | Messaging schemaversie                                                         |
| messageName              | **CreateContractUpdate**                                                      |
| status                   | De status van de aanvraag voor het contract maken.  Mogelijke waarden: **Verzonden**, **doorgevoerd**, **fout**.  |
| AdditionalInformation    | Als u meer informatie vindt u op basis van de status                              |

Voorbeeld van een ingediende **contract maken** reactie van Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Submitted"
    "additionalInformation": { }
}
```

Voorbeeld van een vastgelegde **contract maken** reactie van Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Committed",
    "additionalInformation": { }
}
```

Als de aanvraag mislukt is, wordt informatie over de fout zijn opgenomen in als u meer informatie.

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": null,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract cannot be provisioned on connection."
    }
}
```

### <a name="create-contract-action"></a>Contract actie maken

Maakt een nieuwe overeenkomst-actie.

De aanvraag moet de volgende velden:

| **Naam**                 | **Beschrijving**                                                                                                           |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId                | Client wordt geleverd GUID |
| userChainIdentifier      | Adres van de gebruiker die is gemaakt op de blockchain-netwerk. In Ethereum, is dit adres van de gebruiker **van keten** adres. |
| contractLedgerIdentifier | Adres van het contract op het grootboek |
| versie                  | De versie van de toepassing. Vereist als er meerdere versies van de toepassing ingeschakeld. Anders wordt de versie is optioneel. Zie voor meer informatie over versiebeheer van de toepassing [Azure Blockchain Workbench toepassing versiebeheer](version-app.md). |
| workflowFunctionName     | Naam van de Werkstroomfunctie |
| parameters               | Invoer van de parameters voor het contract maken |
| connectionId             | De unieke id voor de blockchain-verbinding |
| messageSchemaVersion     | Messaging schemaversie |
| messageName              | **CreateContractActionRequest** |

Voorbeeld:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398",
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "version": "1.0",
    "workflowFunctionName": "modify",
    "parameters": [
        {
            "name": "description",
            "value": "a 1969 dodge charger"
        },
        {
            "name": "price",
            "value": "12345"
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionRequest"
}
```

Blockchain Workbench retourneert een antwoord met de volgende velden:

| **Naam**              | **Beschrijving**                                                                   |
|-----------------------|-----------------------------------------------------------------------------------|
| requestId             | Client wordt geleverd GUID|
| ContractId            | De unieke id voor het contract binnen Azure Blockchain Workbench |
| connectionId          | De unieke id voor de blockchain-verbinding |
| messageSchemaVersion  | Messaging schemaversie |
| messageName           | **CreateContractActionUpdate** |
| status                | De status van de aanvraag van de actie contract. Mogelijke waarden: **Verzonden**, **doorgevoerd**, **fout**.                         |
| AdditionalInformation | Als u meer informatie vindt u op basis van de status |

Voorbeeld van een ingediende **contract actie maken** reactie van Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Submitted",
    "additionalInformation": { }
}
```

Voorbeeld van een vastgelegde **contract actie maken** reactie van Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Committed"
    "additionalInformation": { }
}
```

Als de aanvraag mislukt is, wordt informatie over de fout zijn opgenomen in als u meer informatie.

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract action cannot be provisioned on connection."
    }
}
```

### <a name="input-api-error-codes-and-messages"></a>Invoer API-foutcodes en berichten

**Foutcode 4000: Ongeldige aanvraag-fout**
- Ongeldige connectionId
- CreateUserRequest deserialisatie is mislukt
- CreateContractRequest deserialisatie is mislukt
- CreateContractActionRequest deserialisatie is mislukt
- Toepassing {geïdentificeerd door de naam van toepassing} bestaat niet
- Toepassing {geïdentificeerd door de naam van toepassing} heeft geen werkstroom
- UserChainIdentifier bestaat niet
- Contract {aangegeven met grootboek-id} bestaat niet
- Contract {aangegeven met grootboek-id} heeft geen functie {functie Werkstroomnaam}
- UserChainIdentifier bestaat niet

**Foutcode 4090: Foutbericht**
- Gebruiker bestaat al
- Er bestaat al een contract
- Er bestaat al een overeenkomst-actie

**Foutcode 5000: Interne serverfout**
- Uitzondering berichten

## <a name="event-notifications"></a>Gebeurtenismeldingen

Meldingen van gebeurtenissen kunnen worden gebruikt om gebruikers- en downstream-systemen van gebeurtenissen die in Blockchain Workbench en de blockchain-netwerk die is verbonden plaatsvinden met te informeren. Meldingen van gebeurtenissen kunnen worden gebruikt in de code of met hulpprogramma's voor Logic Apps en Flow gebruikt om de stroom van gegevens aan downstream-systemen te activeren.

Zie [Notification message verwijzing](#notification-message-reference) voor meer informatie over verschillende berichten die kunnen worden ontvangen.

### <a name="consuming-event-grid-events-with-azure-functions"></a>Event Grid-gebeurtenissen met Azure Functions

Als een gebruiker wil Event Grid gebruiken om te worden geïnformeerd over gebeurtenissen die Blockchain Workbench, kunt u gebeurtenissen van Event Grid gebruiken met behulp van Azure Functions.

1. Maak een **Azure Function-App** in Azure portal.
2. Maak een nieuwe functie.
3. Zoek de sjabloon voor Event Grid. Voor Basic-sjablooncode voor het lezen van het bericht wordt weergegeven. De code zo nodig wijzigen.
4. Sla de functie. 
5. Selecteer de Event Grid in de resourcegroep van de Blockchain Workbench.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Event Grid-gebeurtenissen met Logic Apps

1. Maak een nieuwe **Azure Logic App** in Azure portal.
2. Bij het openen van de logische App van Azure in de portal, wordt u gevraagd om te selecteren van een trigger. Selecteer **Azure Event Grid--als een resourcegebeurtenis**.
3. Wanneer de werkstroomontwerper wordt weergegeven, wordt u gevraagd aan te melden.
4. Selecteer het abonnement. Resource als **Microsoft.EventGrid.Topics**. Selecteer de **resourcenaam** van de naam van de resource in de resourcegroep Azure Blockchain Workbench.
5. Selecteer de Event Grid in de resourcegroep van de Blockchain Workbench.

## <a name="using-service-bus-topics-for-notifications"></a>Met behulp van Service Bus-onderwerpen voor meldingen

Service Bus-onderwerpen kunnen worden gebruikt om gebruikers te informeren over gebeurtenissen die Blockchain Workbench. 

1. Blader naar de Service Bus in de resourcegroep van de Workbench.
2. Selecteer **onderwerpen**.
3. Selecteer **uitgaande onderwerp**.
4. Maak een nieuw abonnement in dit onderwerp. Voor het ophalen van een sleutel.
5. Maak een programma, waarmee ze zich op gebeurtenissen van dit abonnement abonneren.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Verbruik van Service Bus-berichten met Logic Apps

1. Maak een nieuwe **Azure Logic App** in Azure portal.
2. Bij het openen van de logische App van Azure in de portal, wordt u gevraagd om te selecteren van een trigger. Type **Service Bus** in het zoekvak en selecteer de trigger die geschikt is voor het type interactie dat u wilt dat met de Service Bus. Bijvoorbeeld, **Service Bus - wanneer een bericht wordt ontvangen in een onderwerpabonnement (automatisch voltooien)**.
3. Wanneer de werkstroomontwerper wordt weergegeven, geeft u de verbindingsgegevens voor de Service Bus.
4. Selecteer uw abonnement en geeft u het onderwerp van **workbench-externe**.
5. De logica voor uw toepassing die gebruikmaakt van het bericht van deze trigger te ontwikkelen.

## <a name="notification-message-reference"></a>Naslaginformatie over melding

Afhankelijk van de **OperationName**, de meldingen hebben een van de volgende berichttypen.

### <a name="block-message"></a>Het blokkeringsbericht

Bevat informatie over afzonderlijke blokken. De *BlockMessage* bevat een sectie met gegevens blokkeren en een sectie met transactie-informatie.

| Name | Description |
|------|-------------|
| blokkeren | Bevat [informatie blokkeren](#block-information) |
| transacties | Bevat een verzameling [transactie-informatie](#transaction-information) voor het blok |
| connectionId | De unieke id voor de verbinding |
| messageSchemaVersion | Messaging schemaversie |
| messageName | **BlockMessage** |
| AdditionalInformation | Als u meer informatie |

#### <a name="block-information"></a>Blok-informatie

| Name              | Description |
|-------------------|-------------|
| BlockId           | De unieke id voor het blok in Azure Blockchain Workbench |
| blockNumber       | De unieke id voor een blok in het grootboek |
| blockHash         | De hash van het blok |
| previousBlockHash | De hash van het vorige blok |
| blockTimestamp    | De tijdstempel van het blok |

#### <a name="transaction-information"></a>Transactie-informatie

| Name               | Description |
|--------------------|-------------|
| transactionId      | De unieke id voor de transactie binnen Azure Blockchain Workbench |
| TransactionHash    | De hash van de transactie op het grootboek |
| uit               | De unieke id van het grootboek voor oorsprong van de transactie |
| tot                 | De unieke id van het grootboek voor het doel van de transactie |
| ProvisioningStatus | Hiermee geeft u de huidige status van het inrichtingsproces voor de transactie. Mogelijke waarden zijn: </br>0: de transactie is gemaakt door de API in de database</br>1: de transactie is verzonden naar het grootboek</br>2: de transactie is doorgevoerd naar het grootboek</br>3 of 4: de transactie kan niet worden doorgevoerd naar het grootboek</br>5 - de transactie is doorgevoerd naar het grootboek |

Voorbeeld van een *BlockMessage* van Blockchain Workbench:

``` json
{
    "block": {
        "blockId": 123
        "blockNumber": 1738312,
        "blockHash": "0x03a39411e25e25b47d0ec6433b73b488554a4a5f6b1a253e0ac8a200d13fffff",
        "previousBlockHash": null,
        "blockTimestamp": "2018-10-09T23:35:58Z",
    },
    "transactions": [
        {
            "transactionId": 234
            "transactionHash": "0xa4d9c95b581f299e41b8cc193dd742ef5a1d3a4ddf97bd11b80d123fec27ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": null,
            "provisioningStatus": 1
        },
        {
            "transactionId": 235
            "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
            "from": "0xadd97e1e595916e29ea94fda894941574000ffff",
            "to": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff",
            "provisioningStatus": 2
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "BlockMessage",
    "additionalInformation": {}
}
```

### <a name="contract-message"></a>Contract-bericht

Bevat informatie over een contract. Het bericht bevat een sectie met eigenschappen van het contract en een sectie met transactie-informatie. Alle transacties die het contract voor de desbetreffende blok hebt gewijzigd, worden opgenomen in de sectie transactie.

| Name | Description |
|------|-------------|
| BlockId | De unieke id voor het blok in Azure Blockchain Workbench |
| blockHash | Hash van het blok |
| modifyingTransactions | [Transacties die gewijzigd](#modifying-transaction-information) het contract |
| ContractId | De unieke id voor het contract binnen Azure Blockchain Workbench |
| contractLedgerIdentifier | De unieke id voor het contract op het grootboek |
| contractProperties | [Eigenschappen van het contract](#contract-properties) |
| isNewContract | Geeft aan of deze overeenkomst zojuist is gemaakt. Mogelijke waarden zijn: true: dit contract is een nieuwe opdracht gemaakt. ONWAAR: dit contract is een update van het contract. |
| connectionId | De unieke id voor de verbinding |
| messageSchemaVersion | Messaging schemaversie |
| messageName | **ContractMessage** |
| AdditionalInformation | Als u meer informatie |

#### <a name="modifying-transaction-information"></a>Transactie-informatie wijzigen

| Name               | Description |
|--------------------|-------------|
| transactionId | De unieke id voor de transactie binnen Azure Blockchain Workbench |
| TransactionHash | De hash van de transactie op het grootboek |
| uit | De unieke id van het grootboek voor oorsprong van de transactie |
| tot | De unieke id van het grootboek voor het doel van de transactie |

#### <a name="contract-properties"></a>Contract-eigenschappen

| Name               | Description |
|--------------------|-------------|
| workflowPropertyId | De unieke id voor de werkstroom-eigenschap binnen Azure Blockchain Workbench |
| naam | Naam van de werkstroom-eigenschap |
| waarde | Waarde van de werkstroom-eigenschap |

Voorbeeld van een *ContractMessage* van Blockchain Workbench:

``` json
{
    "blockId": 123,
    "blockhash": "0x03a39411e25e25b47d0ec6433b73b488554a4a5f6b1a253e0ac8a200d13fffff",
    "modifyingTransactions": [
        {
            "transactionId": 234,
            "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": "0xf8559473b3c7197d59212b401f5a9f07ffff"
        },
        {
            "transactionId": 235,
            "transactionHash": "0xa4d9c95b581f299e41b8cc193dd742ef5a1d3a4ddf97bd11b80d123fec27ffff",
            "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
            "to": "0xf8559473b3c7197d59212b401f5a9f07b429ffff"
        }
    ],
    "contractId": 111,
    "contractLedgerIdentifier": "0xf8559473b3c7197d59212b401f5a9f07b429ffff",
    "contractProperties": [
        {
            "workflowPropertyId": 1,
            "name": "State",
            "value": "0"
        },
        {
            "workflowPropertyId": 2,
            "name": "Description",
            "value": "1969 Dodge Charger"
        },
        {
            "workflowPropertyId": 3,
            "name": "AskingPrice",
            "value": "30000"
        },
        {
            "workflowPropertyId": 4,
            "name": "OfferPrice",
            "value": "0"
        },
        {
            "workflowPropertyId": 5,
            "name": "InstanceAppraiser",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 6,
            "name": "InstanceBuyer",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 7,
            "name": "InstanceInspector",
            "value": "0x0000000000000000000000000000000000000000"
        },
        {
            "workflowPropertyId": 8,
            "name": "InstanceOwner",
            "value": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff"
        },
        {
            "workflowPropertyId": 9,
            "name": "ClosingDayOptions",
            "value": "[21,48,69]"
        }
    ],
    "isNewContract": false,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "ContractMessage",
    "additionalInformation": {}
}
```

### <a name="event-message-contract-function-invocation"></a>Bericht van de gebeurtenis: Contract-functieaanroepen

Bevat informatie wanneer een overeenkomst-functie is aangeroepen, zoals de naam van de functie, invoer van de parameters en de oproepende functie van de functie.

| Name | Description |
|------|-------------|
| eventName                   | **ContractFunctionInvocation** |
| oproepende functie                      | [Beller-informatie](#caller-information) |
| ContractId                  | De unieke id voor het contract binnen Azure Blockchain Workbench |
| contractLedgerIdentifier    | De unieke id voor het contract op het grootboek |
| Functienaam                | Naam van de functie |
| parameters                  | [Informatie over parameters](#parameter-information) |
| Transactie                 | [Transactie-informatie](#eventmessage-transaction-information) |
| inTransactionSequenceNumber | Het volgnummer van de transactie in het blok |
| connectionId                | De unieke id voor de verbinding |
| messageSchemaVersion        | Messaging schemaversie |
| messageName                 | **EventMessage** |
| AdditionalInformation       | Als u meer informatie |

#### <a name="caller-information"></a>Beller-informatie

| Name | Description |
|------|-------------|
| type | Type van de oproepende functie, zoals een gebruiker of een contract |
| id | De unieke id voor de oproepende functie binnen Azure Blockchain Workbench |
| ledgerIdentifier | De unieke id voor de oproepende functie op het grootboek |

#### <a name="parameter-information"></a>Informatie over parameters

| Name | Description |
|------|-------------|
| naam | Parameternaam |
| waarde | Parameterwaarde |

#### <a name="event-message-transaction-information"></a>Gebeurtenis bericht transactie-informatie

| Name               | Description |
|--------------------|-------------|
| transactionId      | De unieke id voor de transactie binnen Azure Blockchain Workbench |
| TransactionHash    | De hash van de transactie op het grootboek |
| uit               | De unieke id van het grootboek voor oorsprong van de transactie |
| tot                 | De unieke id van het grootboek voor het doel van de transactie |

Voorbeeld van een *EventMessage ContractFunctionInvocation* van Blockchain Workbench:

``` json
{
    "eventName": "ContractFunctionInvocation",
    "caller": {
        "type": "User",
        "id": 21,
        "ledgerIdentifier": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60ffff"
    },
    "contractId": 34,
    "contractLedgerIdentifier": "0xf8559473b3c7197d59212b401f5a9f07b429ffff",
    "functionName": "Modify",
    "parameters": [
        {
            "name": "description",
            "value": "a new description"
        },
        {
            "name": "price",
            "value": "4567"
        }
    ],
    "transaction": {
        "transactionId": 234,
        "transactionHash": "0x5c1fddea83bf19d719e52a935ec8620437a0a6bdaa00ecb7c3d852cf92e1ffff",
        "from": "0xd85e7262dd96f3b8a48a8aaf3dcdda90f60dffff",
        "to": "0xf8559473b3c7197d59212b401f5a9f07b429ffff"
    },
    "inTransactionSequenceNumber": 1,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation": { }
}
```

### <a name="event-message-application-ingestion"></a>Bericht van de gebeurtenis: Opname van de toepassing

Bevat informatie wanneer een toepassing wordt geüpload naar de Workbench, zoals de naam en versie van de toepassing geüpload.

| Name | Description |
|------|-------------|
| eventName | **ApplicationIngestion** |
| applicationId | De unieke id voor de toepassing in Azure Blockchain Workbench |
| applicationName | De naam van de toepassing |
| applicationDisplayName | Weergavenaam van toepassing |
| applicationVersion | Toepassingsversie |
| applicationDefinitionLocation | URL op waar het configuratiebestand van de toepassing zich bevindt |
| contractCodes | Verzameling van [contract codes](#contract-code-information) voor de toepassing |
| ApplicationRoles | Verzameling van [toepassingsrollen](#application-role-information) voor de toepassing |
| applicationWorkflows | Verzameling van [werkstromen van bedrijfstoepassingen](#application-workflow-information) voor de toepassing |
| connectionId | De unieke id voor de verbinding |
| messageSchemaVersion | Messaging schemaversie |
| messageName | **EventMessage** |
| AdditionalInformation | Aanvullende informatie die hier beschikbaar zijn omvat de toepassing Werkstroomstatussen en overgang. |

#### <a name="contract-code-information"></a>Contract-informatie

| Name | Description |
|------|-------------|
| id | De unieke id voor het codebestand contract binnen Azure Blockchain Workbench |
| ledgerId | De unieke id voor het grootboek binnen Azure Blockchain Workbench |
| location | URL op waar het contract codebestand zich bevindt |

#### <a name="application-role-information"></a>Informatie over de rol van toepassingen

| Name | Description |
|------|-------------|
| id | De unieke id voor de toepassingsrol binnen Azure Blockchain Workbench |
| naam | Naam van de toepassingsrol |

#### <a name="application-workflow-information"></a>Informatie over de werkstroom van toepassing

| Name | Description |
|------|-------------|
| id | De unieke id voor de toepassingswerkstroom in Azure Blockchain Workbench |
| naam | Naam van de toepassing-werkstroom |
| displayName | Weergavenaam van de toepassing-werkstroom |
| functions | Verzameling van [functies voor de toepassingswerkstroom ](#workflow-function-information)|
| statussen | Verzameling van [statussen voor de toepassingswerkstroom](#workflow-state-information) |
| properties | Toepassing [informatie over de eigenschappen van werkstroom](#workflow-property-information) |

##### <a name="workflow-function-information"></a>Informatie over de functie van werkstroom

| Name | Description |
|------|-------------|
| id | De unieke id voor de functie van de werkstroom toepassing binnen Azure Blockchain Workbench |
| naam | Functienaam |
| parameters | Parameters voor de functie |

##### <a name="workflow-state-information"></a>Informatie over de status van de werkstroom

| Name | Description |
|------|-------------|
| naam | Provincienaam |
| displayName | Status weergavenaam |
| stijl | Status stijl (slagen of mislukken) |

##### <a name="workflow-property-information"></a>Informatie over werkstroom-eigenschap

| Name | Description |
|------|-------------|
| id | De unieke id voor de eigenschap van de werkstroom toepassing binnen Azure Blockchain Workbench |
| naam | Naam van eigenschap |
| type | Typ vlastnosti |

Voorbeeld van een *EventMessage ApplicationIngestion* van Blockchain Workbench:

``` json
{
    "eventName": "ApplicationIngestion",
    "applicationId": 31,
    "applicationName": "AssetTransfer",
    "applicationDisplayName": "Asset Transfer",
    "applicationVersion": “1.0”,
    "applicationDefinitionLocation": "http://url"
    "contractCodes": [
        {
            "id": 23,
            "ledgerId": 1,
            "location": "http://url"
        }
    ],
    "applicationRoles": [
            {
                "id": 134,
                "name": "Buyer"
            },
            {
                "id": 135,
                "name": "Seller"
            }
       ],
    "applicationWorkflows": [
        {
            "id": 89,
            "name": "AssetTransfer",
            "displayName": "Asset Transfer",
            "functions": [
                {
                    "id": 912,
                    "name": "",
                    "parameters": [
                        {
                            "name": "description",
                            "type": {
                                "name": "string"
                             }
                        },
                        {
                            "name": "price",
                            "type": {
                                "name": "int"
                            }
                        }
                    ]
                },
                {
                    "id": 913,
                    "name": "modify",
                    "parameters": [
                        {
                            "name": "description",
                            "type": {
                                "name": "string"
                             }
                        },
                        {
                            "name": "price",
                            "type": {
                                "name": "int"
                            }
                        }
                    ]
                }
            ],
            "states": [ 
                 {
                      "name": "Created",
                      "displayName": "Created",
                      "style" : "Success"
                 },
                 {
                      "name": "Terminated",
                      "displayName": "Terminated",
                      "style" : "Failure"
                 }
            ],
            "properties": [
                {
                    "id": 879,
                    "name": "Description",
                    "type": {
                                "name": "string"
                     }
                },
                {
                    "id": 880,
                    "name": "Price",
                    "type": {
                                "name": "int"
                     }
                }
            ]
        }
    ]
    "connectionId": [ ],
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation":
        {
            "states" :
            [
                {
                    "Name": "BuyerAccepted",
                    "Transitions": [
                        {
                            "DisplayName": "Accept"
                            "AllowedRoles": [ ],
                            "AllowedInstanceRoles": [ "InstanceOwner" ],
                            "Function": "Accept",
                            "NextStates": [ "SellerAccepted" ]
                        }
                    ]
                }
            ]
        }
}
```

### <a name="event-message-role-assignment"></a>Bericht van de gebeurtenis: Roltoewijzing

Bevat informatie wanneer een gebruiker een rol in Workbench, zoals wie de toewijzing van rollen en de naam van de rol en de bijbehorende toepassing uitgevoerd wordt toegewezen.

| Name | Description |
|------|-------------|
| eventName | **RoleAssignment** |
| applicationId | De unieke id voor de toepassing in Azure Blockchain Workbench |
| applicationName | De naam van de toepassing |
| applicationDisplayName | Weergavenaam van toepassing |
| applicationVersion | Toepassingsversie |
| applicationRole        | Informatie over de [toepassingsrol](#roleassignment-application-role) |
| toewijzer van               | Informatie over de [toewijzer van](#roleassignment-assigner) |
| toegewezen gebruiker               | Informatie over de [toegewezen gebruiker](#roleassignment-assignee) |
| connectionId           | De unieke id voor de verbinding |
| messageSchemaVersion   | Messaging schemaversie |
| messageName            | **EventMessage** |
| AdditionalInformation  | Als u meer informatie |

#### <a name="roleassignment-application-role"></a>RoleAssignment toepassingsrol

| Name | Description |
|------|-------------|
| id | De unieke id voor de toepassingsrol binnen Azure Blockchain Workbench |
| naam | Naam van de toepassingsrol |

#### <a name="roleassignment-assigner"></a>Toewijzer van RoleAssignment

| Name | Description |
|------|-------------|
| id | De unieke id van de gebruiker in Azure Blockchain Workbench |
| type | Type van de toewijzer van |
| chainIdentifier | De unieke id van de gebruiker op het grootboek |

#### <a name="roleassignment-assignee"></a>RoleAssignment toegewezen gebruiker

| Name | Description |
|------|-------------|
| id | De unieke id van de gebruiker in Azure Blockchain Workbench |
| type | Type van de toegewezen gebruiker |
| chainIdentifier | De unieke id van de gebruiker op het grootboek |

Voorbeeld van een *EventMessage RoleAssignment* van Blockchain Workbench:

``` json
{
    "eventName": "RoleAssignment",
    "applicationId": 31,
    "applicationName": "AssetTransfer",
    "applicationDisplayName": "Asset Transfer",
    "applicationVersion": “1.0”,
    "applicationRole": {
        "id": 134,
        "name": "Buyer"
    },
    "assigner": {
        "id": 1,
        "type": null,
        "chainIdentifier": "0xeFFC7766d38aC862d79706c3C5CEEf089564ffff"
    },
    "assignee": {
        "id": 3,
        "type": null,
        "chainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f24ffff"
    },
    "connectionId": [ ],
    "messageSchemaVersion": "1.0.0",
    "messageName": "EventMessage",
    "additionalInformation": { }
}
```

## <a name="next-steps"></a>Volgende stappen

- [Slimme contract integratiepatronen](integration-patterns.md)
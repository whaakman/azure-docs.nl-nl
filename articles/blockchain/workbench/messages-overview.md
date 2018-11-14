---
title: Azure Blockchain Workbench berichten integratie, overzicht
description: Overzicht van het gebruik van berichten in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 11/12/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: f8f3584475415cf9ca19458f6da78d34df37f438
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614358"
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
| aanvraag-id            | Client wordt geleverd GUID                                |
| Voornaam            | De voornaam van de gebruiker                              |
| lastName             | De achternaam van de gebruiker                               |
| EmailAddress         | E-mailadres van de gebruiker                           |
| externalId           | Azure AD-object-ID van de gebruiker                      |
| ConnectionId         | De unieke id voor de blockchain-verbinding |
| messageSchemaVersion | Schemaversie Messaging                            |
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
| aanvraag-id             | Client wordt geleverd GUID |
| userId                | ID van de gebruiker die is gemaakt |
| UserChainIdentifier   | Adres van de gebruiker die is gemaakt op de blockchain-netwerk. In Ethereum, is het adres van de gebruiker **in de chain** adres. |
| ConnectionId          | De unieke id voor de blockchain-verbinding|
| messageSchemaVersion  | Schemaversie Messaging |
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

Als de aanvraag is mislukt, worden de details over de fout opnemen in als u meer informatie.

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
| aanvraag-id            | Client wordt geleverd GUID |
| UserChainIdentifier  | Adres van de gebruiker die is gemaakt op de blockchain-netwerk. In Ethereum, is dit adres van de gebruiker **van keten** adres. |
| ApplicationName      | Naam van de toepassing |
| WorkflowName         | Naam van de werkstroom |
| parameters           | Invoer van de parameters voor het contract maken |
| ConnectionId         | De unieke id voor de blockchain-verbinding |
| messageSchemaVersion | Schemaversie Messaging |
| messageName          | **CreateContractRequest** |

Voorbeeld:

``` json
{ 
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211", 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "applicationName": "AssetTransfer", 
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
| aanvraag-id                | Client wordt geleverd GUID                                                             |
| ContractId               | De unieke id voor het contract binnen Azure Blockchain Workbench |
| ContractLedgerIdentifier | Adres van het contract op het grootboek                                            |
| ConnectionId             | De unieke id voor de blockchain-verbinding                               |
| messageSchemaVersion     | Schemaversie Messaging                                                         |
| messageName              | **CreateContractUpdate**                                                      |
| status                   | De status van de aanvraag voor het contract maken.  Mogelijke waarden: **ingediend**, **vastgelegd**, **fout**.  |
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

Als de aanvraag is mislukt, worden de details over de fout opnemen in als u meer informatie.

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
| aanvraag-id                | Client wordt geleverd GUID |
| UserChainIdentifier      | Adres van de gebruiker die is gemaakt op de blockchain-netwerk. In Ethereum, is dit van de gebruiker **van keten** adres. |
| ContractLedgerIdentifier | Adres van het contract op het grootboek |
| WorkflowFunctionName     | Naam van de Werkstroomfunctie |
| parameters               | Invoer van de parameters voor het contract maken |
| ConnectionId             | De unieke id voor de blockchain-verbinding |
| messageSchemaVersion     | Schemaversie Messaging |
| messageName              | **CreateContractActionRequest** |

Voorbeeld:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398",
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
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
| aanvraag-id             | Client wordt geleverd GUID|
| ContractId            | De unieke id voor het contract binnen Azure Blockchain Workbench |
| ConnectionId          | De unieke id voor de blockchain-verbinding |
| messageSchemaVersion  | Schemaversie Messaging |
| messageName           | **CreateContractActionUpdate** |
| status                | De status van de aanvraag van de actie contract. Mogelijke waarden: **ingediend**, **vastgelegd**, **fout**.                         |
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

Als de aanvraag is mislukt, worden de details over de fout opnemen in als u meer informatie.

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

**Foutcode 4000: ongeldige aanvraag-fout**
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

**Foutcode 4090: Conflict fout**
- Gebruiker bestaat al
- Er bestaat al een contract
- Er bestaat al een overeenkomst-actie

**Foutcode 5000: interne serverfout**
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

1.  Maak een nieuwe **Azure Logic App** in Azure portal.
2.  Bij het openen van de logische App van Azure in de portal, wordt u gevraagd om te selecteren van een trigger. Selecteer **Azure Event Grid--als een resourcegebeurtenis**.
3. Wanneer de werkstroomontwerper wordt weergegeven, wordt u gevraagd aan te melden.
4. Selecteer het abonnement. Resource als **Microsoft.EventGrid.Topics**. Selecteer de **resourcenaam** van de naam van de resource in de resourcegroep Azure Blockchain Workbench.
5. Selecteer de Event Grid in de resourcegroep van de Blockchain Workbench.

## <a name="using-service-bus-topics-for-notifications"></a>Met behulp van Service Bus-onderwerpen voor meldingen

Service Bus-onderwerpen kunnen worden gebruikt om gebruikers te informeren over gebeurtenissen die Blockchain Workbench. 

1.  Blader naar de Service Bus in de resourcegroep van de Workbench.
2.  Selecteer **onderwerpen**.
3.  Selecteer **workbench-externe**.
4.  Maak een nieuw abonnement in dit onderwerp. Voor het ophalen van een sleutel.
5.  Maak een programma, waarmee ze zich op gebeurtenissen van dit abonnement abonneren.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Verbruik van Service Bus-berichten met Logic Apps

1. Maak een nieuwe **Azure Logic App** in Azure portal.
2. Bij het openen van de logische App van Azure in de portal, wordt u gevraagd om te selecteren van een trigger. Type **Service Bus** in het zoekvak en selecteer de trigger die geschikt is voor het type interactie dat u wilt dat met de Service Bus. Bijvoorbeeld, **Service Bus - wanneer een bericht wordt ontvangen in een onderwerpabonnement (automatisch voltooien)**.
3. Wanneer de werkstroomontwerper wordt weergegeven, geeft u de verbindingsgegevens voor de Service Bus.
4. Selecteer uw abonnement en geeft u het onderwerp van **workbench-externe**.
5. De logica voor uw toepassing die gebruikmaakt van het bericht van deze trigger te ontwikkelen.

## <a name="notification-message-reference"></a>Naslaginformatie over melding

Afhankelijk van de **OperationName**, de meldingen hebben een van de volgende berichttypen.

### <a name="accountcreated"></a>AccountCreated

Geeft aan dat een nieuw account moet worden toegevoegd aan de opgegeven keten is aangevraagd.

| Naam    | Beschrijving  |
|----------|--------------|
| UserId  | ID van de gebruiker die is gemaakt. |
| ChainIdentifier | Adres van de gebruiker die is gemaakt op de blockchain-netwerk. In Ethereum, zou dit van de gebruiker **in de chain** adres. |

``` csharp
public class NewAccountRequest : MessageModelBase
{
  public int UserID { get; set; }
  public string ChainIdentifier { get; set; }
}
```

### <a name="contractinsertedorupdated"></a>ContractInsertedOrUpdated

Geeft aan dat een aanvraag invoegen of bijwerken van een overeenkomst op een gedistribueerd grootboek is ingevoerd.

| Naam | Beschrijving |
|-----|--------------|
| ChainID | De unieke id voor de keten die zijn gekoppeld aan de aanvraag |
| BlockId | De unieke id voor een blok in het grootboek |
| ContractId | Een unieke id voor het contract |
| ContractAddress |       Het adres van het contract op het grootboek |
| TransactionHash  |     De hash van de transactie op het grootboek |
| OriginatingAddress |   Het adres van de afzender van de transactie |
| actionName       |     De naam van de actie |
| IsUpdate        |      Wordt geïdentificeerd als dit een update is |
| Parameters       |     Een lijst met objecten die de naam, waarde en het gegevenstype van de parameters die zijn verzonden naar een actie identificeren |
| TopLevelInputParams |  Dit zijn de parameters van het contract op het hoogste niveau in scenario's waarin een overeenkomst is verbonden met een of meer overeenkomsten. |

``` csharp
public class ContractInsertOrUpdateRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public int ContractId { get; set; }
    public string ContractAddress { get; set; }
    public string TransactionHash { get; set; }
    public string OriginatingAddress { get; set; }
    public string ActionName { get; set; }
    public bool IsUpdate { get; set; }
    public List<ContractProperty> Parameters { get; set; }
    public bool IsTopLevelUpdate { get; set; }
    public List<ContractInputParameter> TopLevelInputParams { get; set; }
}
```

#### <a name="updatecontractaction"></a>UpdateContractAction

Geeft aan dat een aanvraag is gemaakt met het uitvoeren van een actie op een specifieke opdracht op een gedistribueerd grootboek.

| Naam                     | Beschrijving                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ContractActionId         | De unieke id voor deze actie contract |
| ChainIdentifier          | De unieke id voor de keten |
| ConnectionId             | De unieke id voor de verbinding |
| UserChainIdentifier      | Adres van de gebruiker die is gemaakt op de blockchain-netwerk. In Ethereum, is dit adres van de gebruiker **van keten** adres. |
| ContractLedgerIdentifier | Adres van het contract op het grootboek |
| WorkflowFunctionName     | Naam van de Werkstroomfunctie |
| WorkflowName             | Naam van de werkstroom |
| WorkflowBlobStorageURL   | De url van de overeenkomst in blob-opslag |
| ContractActionParameters | Parameters voor de overeenkomst-actie |
| TransactionHash          | De hash van de transactie op het grootboek |
| Inrichtingsstatus      | De huidige inrichting status van de actie.</br>0: gemaakt</br>1 – in proces</br>2 – voltooien</br> Volledige geeft aan dat een bevestiging van het grootboek dat deze is toegevoegd |

```csharp
public class ContractActionRequest : MessageModelBase
{
    public int ContractActionId { get; set; }
    public int ConnectionId { get; set; }
    public string UserChainIdentifier { get; set; }
    public string ContractLedgerIdentifier { get; set; }
    public string WorkflowFunctionName { get; set; }
    public string WorkflowName { get; set; }
    public string WorkflowBlobStorageURL { get; set; }
    public IEnumerable<ContractActionParameter> ContractActionParameters { get; set; }
    public string TransactionHash { get; set; }
    public int ProvisioningStatus { get; set; }
}
```

### <a name="updateuserbalance"></a>UpdateUserBalance

Geeft aan dat een aanvraag om bij te werken het saldo van de gebruiker op een specifieke gedistribueerd grootboek is gemaakt.

> [!NOTE]
> Dit bericht wordt alleen gegenereerd voor deze grootboeken waarvoor de financiering van accounts.
> 

| Naam    | Beschrijving                              |
|---------|------------------------------------------|
| Adres | Het adres van de gebruiker die is gefinancierd |
| Saldo | Het saldo van het saldo van de gebruiker         |
| ChainID | De unieke id voor de keten     |


``` csharp
public class UpdateUserBalanceRequest : MessageModelBase
{
    public string Address { get; set; }
    public decimal Balance { get; set; }
    public int ChainID { get; set; }
}
```

### <a name="insertblock"></a>InsertBlock

Bericht geeft aan dat een aanvraag voor een blok toevoegen aan een gedistribueerde grootboek is gemaakt.

| Naam           | Beschrijving                                                            |
|----------------|------------------------------------------------------------------------|
| ChainId        | De unieke id van de keten waaraan het blok is toegevoegd             |
| BlockId        | De unieke id voor het blok in Azure Blockchain Workbench |
| BlockHash      | De hash van het blok                                                 |
| BlockTimeStamp | De tijdstempel van het blok                                            |

``` csharp
public class InsertBlockRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string BlockHash { get; set; }
    public int BlockTimestamp { get; set; }
}
```

### <a name="inserttransaction"></a>InsertTransaction

Bericht geeft details op een aanvraag voor het toevoegen van een transactie van een gedistribueerde grootboek.

| Naam            | Beschrijving                                                            |
|-----------------|------------------------------------------------------------------------|
| ChainId         | De unieke id van de keten waaraan het blok is toegevoegd             |
| BlockId         | De unieke id voor het blok in Azure Blockchain Workbench |
| TransactionHash | De hash van de transactie                                           |
| Vanaf            | Het adres van de afzender van de transactie                      |
| Handeling              | Het adres van de beoogde ontvanger van de transactie              |
| Waarde           | De waarde die is opgenomen in de transactie                                 |
| IsAppBuilderTx  | Wordt geïdentificeerd als dit een Blockchain Workbench-transactie is                         |

``` csharp
public class InsertTransactionRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string TransactionHash { get; set; }
    public string From { get; set; }
    public string To { get; set; }
    public decimal Value { get; set; }
    public bool IsAppBuilderTx { get; set; }
}
```

### <a name="assigncontractchainidentifier"></a>AssignContractChainIdentifier

Biedt details over de toewijzing van een keten-id voor een contract. Bijvoorbeeld, in Ethereum blockchain, het adres van een overeenkomst op het grootboek.

| Naam            | Beschrijving                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| ContractId      | De unieke id voor het contract binnen Azure Blockchain Workbench |
| ChainIdentifier | Id van de overeenkomst in de keten                             |

``` csharp
public class AssignContractChainIdentifierRequest : MessageModelBase
{
    public int ContractId { get; set; }
    public string ChainIdentifier { get; set; }
}
```

## <a name="classes-used-by-message-types"></a>Klassen die worden gebruikt door berichttypen

### <a name="messagemodelbase"></a>MessageModelBase

Het basismodel voor alle berichten.

| Naam          | Beschrijving                          |
|---------------|--------------------------------------|
| OperationName | De naam van de bewerking           |
| aanvraag-id     | Unieke id voor de aanvraag |

``` csharp
public class MessageModelBase
{
    public string OperationName { get; set; }
    public string RequestId { get; set; }
}
```

### <a name="contractinputparameter"></a>ContractInputParameter

Bevat de naam, waarde en type van een parameter.

| Naam  | Beschrijving                 |
|-------|-----------------------------|
| Naam  | De naam van de parameter  |
| Waarde | De waarde van de parameter |
| Type  | Het type van de parameter  |

``` csharp
public class ContractInputParameter
{
    public string Name { get; set; }
    public string Value { get; set; }
    public string Type { get; set; }
}
```

#### <a name="contractproperty"></a>ContractProperty

Bevat de ID, naam, waarde en type van een eigenschap.

| Naam  | Beschrijving                |
|-------|----------------------------|
| Id    | De ID van de eigenschap    |
| Naam  | De naam van de eigenschap  |
| Waarde | De waarde van de eigenschap |
| Type  | Het type van de eigenschap  |

``` csharp
public class ContractProperty
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Value { get; set; }
    public string DataType { get; set; }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Slimme contract integratiepatronen](integration-patterns.md)
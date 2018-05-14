---
title: Overzicht van Azure Blockchain Workbench berichten
description: Overzicht van het gebruik van berichten in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 8d9b9c74ead54280d82bf2b0f9a2232723b2af63
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="azure-blockchain-workbench-messages-overview"></a>Overzicht van Azure Blockchain Workbench berichten

Naast het bieden van een REST-API biedt Azure Blockchain Workbench ook integratie op basis van berichten. Workbench publiceert grootboek gerichte gebeurtenissen via Azure gebeurtenis raster, waarmee de downstream consument opnemen van gegevens of actie onderneemt op basis van deze gebeurtenissen. Voor deze clients waarvoor betrouwbare messaging, biedt Azure Blockchain Workbench berichten met een Azure Service Bus-eindpunt.

Ontwikkelaars hebben ook geïnteresseerd in de mogelijkheid om externe systemen communiceren initiëren transacties voor het maken van gebruikers, contracten maken en bijwerken van opdrachten op een grootboek uitgedrukt. Terwijl deze functionaliteit is momenteel niet worden weergegeven in de openbare preview, een steekproef die wel die mogelijkheid biedt kan worden gevonden op [ http://aka.ms/blockchain-workbench-integration-sample ](http://aka.ms/blockchain-workbench-integration-sample).


## <a name="event-notifications"></a>Gebeurtenismeldingen

Gebeurtenismeldingen kunnen worden gebruikt om gebruikers- en downstream systemen van gebeurtenissen die in de Workbench en het blockchain-netwerk dat is verbonden plaatsvinden met te informeren. Gebeurtenismeldingen worden gebruikt in de code of worden gebruikt met hulpprogramma's zoals Logic Apps en stroom voor het activeren van de stroom van gegevens naar downstream-systemen.

Zie [Notification message verwijzing](#notification-message-reference) voor meer informatie over verschillende berichten die kunnen worden ontvangen.

### <a name="consuming-event-grid-events-with-azure-functions"></a>Gebeurtenis raster gebeurtenissen met de Azure Functions gebruiken

Als een gebruiker wil gebeurtenis raster gebruiken om te worden geïnformeerd over de gebeurtenissen die in de Blockchain Workbench gebeuren, kunt u gebeurtenissen van gebeurtenis raster verbruiken met behulp van Azure Functions.

1. Maak een **Azure functie-App** in de Azure portal.
2. Maak een nieuwe functie.
3. De sjabloon voor de gebeurtenis raster vinden. Basic sjablooncode voor het lezen van het bericht wordt weergegeven. De code zo nodig wijzigen.
4. De functie opslaan. 
5. Selecteer het raster gebeurtenis uit de resourcegroep Blockchain-Workbench.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Gebeurtenissen van de gebeurtenis raster met Logic Apps gebruiken

1.  Maak een nieuwe **Azure Logic App** in de Azure portal.
2.  Bij het openen van de Azure logische App in de portal, wordt u gevraagd om te selecteren van een trigger. Selecteer **Azure gebeurtenis raster--wanneer er een gebeurtenis optreedt**.
3. Wanneer de workflow designer wordt weergegeven, wordt u gevraagd aan te melden.
4. Selecteer het abonnement. Resource als **Microsoft.EventGrid.Topics**. Selecteer de **resourcenaam** van de naam van de bron van de resourcegroep Azure Blockchain Workbench.
5. Selecteer het raster gebeurtenis uit de resourcegroep Blockchain-Workbench.

## <a name="using-service-bus-topics-for-notifications"></a>Met behulp van Service Bus-onderwerpen voor meldingen

Service Bus-onderwerpen kan worden gebruikt voor de gebruikers te informeren over de gebeurtenissen die Blockchain Workbench. 

1.  Blader naar de Service Bus in de Workbench-resourcegroep.
2.  Selecteer **onderwerpen**.
3.  Selecteer **workbench-externe**.
4.  Maak een nieuw abonnement naar dit onderwerp. Voor het ophalen van een sleutel.
5.  Maak een programma, gebeurtenissen is lid van dit abonnement.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Verbruik van Service Bus-berichten met Logic Apps

1. Maak een nieuwe **Azure Logic App** in de Azure portal.
2.  Bij het openen van de Azure logische App in de portal, wordt u gevraagd om te selecteren van een trigger. Type **Service Bus** in het zoekvak en selecteer de trigger die geschikt is voor het type van interactie dat u wilt dat met de Service Bus. Bijvoorbeeld: **Service Bus--wanneer een bericht wordt ontvangen in een onderwerpabonnement (automatisch aanvullen)**.
3. Wanneer de workflow designer wordt weergegeven, geeft u de verbindingsgegevens voor de Service Bus.
4. Selecteer uw abonnement en geef het onderwerp van **workbench-externe**.
5. Ontwikkelen voor de logica voor uw toepassing die gebruikmaakt van het bericht van deze trigger.

## <a name="notification-message-reference"></a>Naslaginformatie over melding

De meldingen worden, afhankelijk van de OperationName hebben een van de volgende berichttypen.

### <a name="accountcreated"></a>AccountCreated

Hiermee wordt aangegeven dat een nieuw account moet worden toegevoegd aan de opgegeven keten is aangevraagd.

| Naam    | Beschrijving  |
|----------|--------------|
| UserId  | ID van de gebruiker die is gemaakt |
| ChainIdentifier | Adres van de gebruiker die is gemaakt op het netwerk blockchain. In Ethereum, zou dit adres 'in de keten' van de gebruiker. |

``` csharp
public class NewAccountRequest : MessageModelBase
{
  public int UserID { get; set; }
  public string ChainIdentifier { get; set; }
}
```

### <a name="contractinsertedorupdated"></a>ContractInsertedOrUpdated

Hiermee wordt aangegeven dat een aanvraag invoegen of bijwerken van een contract op een gedistribueerde grootboek heeft aangebracht.

| Naam | Beschrijving |
|-----|--------------|
| ChainID | Een unieke id voor de keten is gekoppeld aan de aanvraag.|
  BlockId | De unieke id voor een blok op het grootboek.|
  ContractId | Een unieke id voor het contract.|
  ContractAddress |       Het adres van het contract op het grootboek.|
  TransactionHash  |     De hash van de transactie op het grootboek.|
  OriginatingAddress |   Het adres van de oorspronkelijke aanvrager van de transactie.|
  actionName       |     De naam van de actie.|
  IsUpdate        |      Hiermee wordt aangeduid of dit een update is.|
  Parameters       |     Een lijst met objecten die het type naam, waarde en gegevens van de parameters die worden verzonden naar een actie identificeren.|
  TopLevelInputParams |  Dit zijn de parameters van het contract op het hoogste niveau in scenario's waarin een contract is verbonden met een of meer overeenkomsten. |

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

Geeft aan dat een aanvraag heeft aangebracht tot het uitvoeren van een actie op een specifieke contract op een gedistribueerde grootboek.

| Naam                     | Beschrijving                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ContractActionId         | De unieke id voor deze actie contract                                                                                                                                |
| ChainIdentifier          | De unieke id voor de keten                                                                                                                                           |
| Verbindings-id             | De unieke id voor de verbinding                                                                                                                                      |
| UserChainIdentifier      | Adres van de gebruiker die is gemaakt op het netwerk blockchain. In Ethereum, zou dit adres 'in de keten' van de gebruiker.                                                     |
| ContractLedgerIdentifier | Adres van het contract op het grootboek.                                                                                                                                        |
| WorkflowFunctionName     | Naam van de Werkstroomfunctie.                                                                                                                                                |
| WorkflowName             | De naam van de werkstroom.                                                                                                                                                         |
| WorkflowBlobStorageURL   | De url van het contract in blob-opslag.                                                                                                                                      |
| ContractActionParameters | Parameters voor de contract-actie.                                                                                                                                           |
| TransactionHash          | De hash van de transactie op het grootboek.                                                                                                                                    |
| Inrichtingsstatus      | De huidige inrichting status van de actie.</br>0: gemaakt</br>1 – in proces</br>2 – voltooien</br> Volledige een bevestiging van het grootboek geeft aan dat dit is toegevoegd.                                               |
|                          |                                                                                                                                                                               |

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

Hiermee wordt aangegeven dat een aanvraag is gemaakt met het saldo van de gebruiker op een specifieke gedistribueerde grootboek bijwerken.

> [!NOTE]
> Dit bericht wordt alleen gegenereerd voor deze posten waarvoor de middelen van accounts.
> 

| Naam    | Beschrijving                              |
|---------|------------------------------------------|
| Adres | Het adres van de gebruiker die is basis. |
| Verdeling | Het saldo van het saldo van de gebruiker.         |
| ChainID | De unieke id voor de keten.     |


``` csharp
public class UpdateUserBalanceRequest : MessageModelBase
{
    public string Address { get; set; }
    public decimal Balance { get; set; }
    public int ChainID { get; set; }
}
```

### <a name="insertblock"></a>InsertBlock

Bericht geeft aan dat een aanvraag is gemaakt met een blok toevoegen aan een gedistribueerde grootboek.

| Naam           | Beschrijving                                                            |
|----------------|------------------------------------------------------------------------|
| ChainId        | De unieke id van de keten waaraan het blok is toegevoegd.             |
| BlockId        | De unieke id voor het blok in Azure Blockchain Workbench. |
| BlockHash      | De hash van het blok.                                                 |
| BlockTimeStamp | De tijdstempel van het blok.                                            |

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

Bericht worden details weergegeven over een aanvraag voor het toevoegen van een transactie van een gedistribueerde grootboek.

| Naam            | Beschrijving                                                            |
|-----------------|------------------------------------------------------------------------|
| ChainId         | De unieke id van de keten waaraan het blok is toegevoegd.             |
| BlockId         | De unieke id voor het blok in Azure Blockchain Workbench. |
| TransactionHash | De hash van de transactie.                                           |
| Vanaf            | Het adres van de oorspronkelijke aanvrager van de transactie.                      |
| Handeling              | Het adres van de beoogde ontvanger van de transactie.              |
| Waarde           | De waarde die is opgenomen in de transactie.                                 |
| IsAppBuilderTx  | Hiermee wordt aangeduid of dit een transactie Blockchain Workbench is.                         |

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

Biedt details over de toewijzing van een keten-id voor een contract. Bijvoorbeeld in Ethereum blockchain, het adres van een contract op het grootboek.

| Naam            | Beschrijving                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| ContractId      | Dit is de unieke id voor het contract in Azure Blockchain Workbench. |
| ChainIdentifier | Dit is de id voor het contract in de keten.                             |

``` csharp
public class AssignContractChainIdentifierRequest : MessageModelBase
{
    public int ContractId { get; set; }
    public string ChainIdentifier { get; set; }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench-architectuur](blockchain-workbench-architecture.md)
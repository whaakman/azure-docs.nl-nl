---
title: Een toepassing blockchain maken in Azure Blockchain Workbench
description: Het maken van een toepassing blockchain in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/17/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: a4b704f433f02afcff7b94f98c19a478caaa02b2
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808057"
---
# <a name="create-a-blockchain-application-in-azure-blockchain-workbench"></a>Een toepassing blockchain maken in Azure Blockchain Workbench

U kunt Azure Blockchain Workbench blockchain toepassingen maken die meerdere partijen werkstromen die zijn gedefinieerd door de configuratie en slimme Contractcode vertegenwoordigen.

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een toepassing blockchain configureren
> * Maken van een smartcard contract codebestand
> * Een toepassing blockchain aan Blockchain Workbench toevoegen
> * Leden toevoegen aan de toepassing blockchain

## <a name="prerequisites"></a>Vereisten

* Een Blockchain Workbench-implementatie. Zie voor meer informatie [Azure Blockchain Workbench-implementatie](blockchain-workbench-deploy.md) voor meer informatie over de implementatie.
* Azure Active Directory-gebruikers in de tenant gekoppeld aan Blockchain Workbench. Zie voor meer informatie [Azure AD-gebruikers toevoegen in Azure Blockchain Workbench](blockchain-workbench-manage-users.md#add-azure-ad-users).
* Een beheerdersaccount Blockchain Workbench. Zie voor meer informatie toevoegen [Blockchain Workbench beheerders in Azure Blockchain Workbench](blockchain-workbench-manage-users.md#manage-blockchain-workbench-administrators).

## <a name="hello-blockchain"></a>Hallo, Blockchain!

We gaan een eenvoudige toepassing waarin een aanvrager verzendt een aanvraag en een responder een antwoord op de aanvraag verzenden bouwen. Bijvoorbeeld, een aanvraag kan zijn, 'Hallo, hoe weet u?', en de antwoord-kunnen zijn, 'Ik ben geweldig!'. Zowel de aanvraag en het antwoord worden op de onderliggende blockchain geregistreerd. 

Volg de stappen voor het maken van bestanden voor de toepassing of kunt u [het voorbeeld downloaden vanuit GitHub](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/hello-blockchain). 

## <a name="configuration-file"></a>Configuratiebestand

Metagegevens van de configuratie definieert de werkstromen op hoog niveau en model van de interactie van de toepassing blockchain. Metagegevens van de configuratie vertegenwoordigt de fasen van de werkstroom en het model van de interactie van de toepassing blockchain.

1. Maak een bestand met de naam in uw favoriete editor `HelloBlockchain.json`.
2. De volgende JSON om te definiëren van de configuratie van de toepassing blockchain toevoegen.

    ``` json
    {
      "ApplicationName": "HelloBlockchain",
      "DisplayName": "Hello, Blockchain!",
      "Description": "A simple application to send request and get response",
      "ApplicationRoles": [
        {
          "Name": "Requestor",
          "Description": "A person sending a request."
        },
        {
          "Name": "Responder",
          "Description": "A person responding to a request"
        }
      ],
      "Workflows": [
        {
          "Name": "HelloBlockchain",
          "DisplayName": "Request Response",
          "Description": "A simple workflow to send a request and receive a response.",
          "Initiators": [ "Requestor" ],
          "StartState": "Request",
          "Properties": [
            {
              "Name": "State",
              "DisplayName": "State",
              "Description": "Holds the state of the contract.",
              "Type": {
                "Name": "state"
              }
            },
            {
              "Name": "Requestor",
              "DisplayName": "Requestor",
              "Description": "A person sending a request.",
              "Type": {
                "Name": "Requestor"
              }
            },
            {
              "Name": "Responder",
              "DisplayName": "Responder",
              "Description": "A person sending a response.",
              "Type": {
                "Name": "Responder"
              }
            },
            {
              "Name": "RequestMessage",
              "DisplayName": "Request Message",
              "Description": "A request message.",
              "Type": {
                "Name": "string"
              }
            },
            {
              "Name": "ResponseMessage",
              "DisplayName": "Response Message",
              "Description": "A response message.",
              "Type": {
                "Name": "string"
              }
            }
          ],
          "Constructor": {
            "Parameters": [
              {
                "Name": "message",
                "Description": "...",
                "DisplayName": "Request Message",
                "Type": {
                  "Name": "string"
                }
              }
            ]
          },
          "Functions": [
            {
              "Name": "SendRequest",
              "DisplayName": "Request",
              "Description": "...",
              "Parameters": [
                {
                  "Name": "requestMessage",
                  "Description": "...",
                  "DisplayName": "Request Message",
                  "Type": {
                    "Name": "string"
                  }
                }
              ]
            },
            {
              "Name": "SendResponse",
              "DisplayName": "Response",
              "Description": "...",
              "Parameters": [
                {
                  "Name": "responseMessage",
                  "Description": "...",
                  "DisplayName": "Response Message",
                  "Type": {
                    "Name": "string"
                  }
                }
              ]
            }
          ],
          "States": [
            {
              "Name": "Request",
              "DisplayName": "Request",
              "Description": "...",
              "PercentComplete": 50,
              "Value": 0,
              "Style": "Success",
              "Transitions": [
                {
                  "AllowedRoles": ["Responder"],
                  "AllowedInstanceRoles": [],
                  "Description": "...",
                  "Function": "SendResponse",
                  "NextStates": [ "Respond" ],
                  "DisplayName": "Send Response"
                }
              ]
            },
            {
              "Name": "Respond",
              "DisplayName": "Respond",
              "Description": "...",
              "PercentComplete": 90,
              "Value": 1,
              "Style": "Success",
              "Transitions": [
                {
                  "AllowedRoles": [],
                  "AllowedInstanceRoles": ["Requestor"],
                  "Description": "...",
                  "Function": "SendRequest",
                  "NextStates": [ "Request" ],
                  "DisplayName": "Send Request"
                }
              ]
            }
          ]
        }
      ]
    }
    ```

3. Sla het bestand `HelloBlockchain.json` op.

Het configuratiebestand bevat verschillende secties. Meer informatie over elke sectie zijn als volgt:

### <a name="application-metadata"></a>Metagegevens van de toepassing

Het begin van het configuratiebestand bevat informatie over de toepassing zoals toepassingsnaam en beschrijving.

### <a name="application-roles"></a>Toepassingsrollen

De sectie van de rollen toepassing definieert de rollen van de gebruiker die u kunnen uitvoeren of in de toepassing blockchain deelnemen. U definieert een aantal verschillende functies op basis van functionaliteit. Er is een verschil tussen de functionaliteit van een aanvrager als een entiteit die produceert aanvragen en een eindpunt als een entiteit die antwoorden produceert in het scenario van aanvragen en antwoorden.

### <a name="workflows"></a>Werkstromen

Werkstromen definiëren een of meer fasen en acties van het contract. In het scenario voor aanvragen en antwoorden is de eerste fase (status) van de werkstroom dat een aanvrager (rol) heeft een actie (transitie) (functie) van een aanvraag te verzenden. De volgende fase (status) is dat een responder (rol) heeft een actie (transitie) om een antwoord (functie) te verzenden. Werkstroom van een toepassing kan betrekking hebben op eigenschappen, functies, en statussen vereist beschrijven de stroom van een contract. 

Zie voor meer informatie over de inhoud van de configuratiebestanden [Azure Blockchain werkstroom configuratie verwijzing](blockchain-workbench-configuration-overview.md).

## <a name="smart-contract-code-file"></a>Smart contract codebestand

Smart contracten vertegenwoordigen de zakelijke logica van de toepassing blockchain. Op dit moment ondersteunt Blockchain Workbench Ethereum voor het blockchain-grootboek. Maakt gebruik van Ethereum [volheid](https://solidity.readthedocs.io) de programmeertaal voor het schrijven van zelf afdwingen van de bedrijfslogica voor smart contracten.

Smartcard contracten in volheid zijn vergelijkbaar met de klassen in objectgeoriënteerde talen. Elke opdracht bevat de status en functies voor het implementeren van fasen en acties van de smartcard contract.

Maak een bestand met de naam in uw favoriete editor `HelloBlockchain.sol`.

### <a name="version-pragma"></a>Versie pragma

Als een best practice erop wijzen dat de versie van volheid u ontwikkelt. De versie opgeven voorkomt incompatibel zijn met toekomstige versies van volheid. 

De volgende versie pragma toevoegen aan de bovenkant van `HelloBlockchain.sol` codebestand smart contract.


  ``` solidity
  pragma solidity ^0.4.20;
  ```

### <a name="base-class"></a>Basisklasse

**WorkbenchBase** basisklasse kunt Blockchain Workbench maken en bijwerken van het contract. De basisklasse is vereist voor Blockchain Workbench specifieke smart Contractcode. Het contract moet worden overgenomen van de **WorkbenchBase** basisklasse.

In `HelloBlockchain.sol` slimme codebestand contract, het toevoegen van de **WorkbenchBase** klasse aan het begin van het bestand. 

```
contract WorkbenchBase {
    event WorkbenchContractCreated(string applicationName, string workflowName, address originatingAddress);
    event WorkbenchContractUpdated(string applicationName, string workflowName, string action, address originatingAddress);

    string internal ApplicationName;
    string internal WorkflowName;

    function WorkbenchBase(string applicationName, string workflowName) internal {
        ApplicationName = applicationName;
        WorkflowName = workflowName;
    }

    function ContractCreated() internal {
        WorkbenchContractCreated(ApplicationName, WorkflowName, msg.sender);
    }

    function ContractUpdated(string action) internal {
        WorkbenchContractUpdated(ApplicationName, WorkflowName, action, msg.sender);
    }
}
```
De basisklasse bevat twee belangrijke functies:

|Functie van de basisklasse  | Doel  | Het aanroepen  |
|---------|---------|---------|
| ContractCreated() | Waarschuwt Blockchain Workbench een contract is gemaakt | Voordat u afsluit de contract-constructor |
| ContractUpdated() | Waarschuwt Blockchain Workbench een contractstatus is bijgewerkt | Voordat u afsluit een contract-functie |

### <a name="configuration-and-smart-contract-code-relationship"></a>Configuratie en slimme contract code relatie

Blockchain Workbench maakt gebruik van het configuratiebestand en codebestand smart contract een blockchain-toepassing maken. Er is een relatie tussen is gedefinieerd in de configuratie en de code in het contract van de smartcard. Contractdetails van het, functies, parameters en typen moet overeenkomen met de toepassing maken. Blockchain Workbench controleert de bestanden voorafgaand aan het maken van de toepassing. 

### <a name="contract"></a>Contract

Voor Blockchain Workbench, contracten moeten worden overgenomen van de **WorkbenchBase** basisklasse. Als u definieert het contract, moet u de naam van de toepassing en de naam van de werkstroom geven als argumenten.

Voeg de **contract** koptekst tot uw `HelloBlockchain.sol` codebestand smart contract. 

```
contract HelloBlockchain is WorkbenchBase('HelloBlockchain', 'HelloBlockchain') {
```

Het contract moet worden overgenomen van de **WorkbenchBase** basisklasse en in de parameters **ApplicationName** en de werkstroom **naam** zoals gedefinieerd in de configuratie het bestand. In dit geval wordt zijn de toepassingsnaam en de naam van de werkstroom hetzelfde.

### <a name="state-variables"></a>Variabelen voor de status

Variabelen voor de opslaan waarden van de status voor elk exemplaar van het contract. De status van variabelen in het contract moeten overeenkomen met de werkstroomeigenschappen die zijn gedefinieerd in het configuratiebestand.

De variabelen voor de toevoegen aan uw contract in uw `HelloBlockchain.sol` codebestand smart contract. 

```
    //Set of States
    enum StateType { Request, Respond}
    
    //List of properties
    StateType public  State;
    address public  Requestor;
    address public  Responder;
    
    string public RequestMessage;
    string public ResponseMessage;
```

### <a name="constructor"></a>Constructor

De constructor definieert invoerparameters voor een nieuw exemplaar van de smartcard contract van een werkstroom. De constructor is gedeclareerd als een functie met dezelfde naam als het contract. Vereiste parameters voor de constructor worden gedefinieerd als constructorparameters in het configuratiebestand. Het aantal, de volgorde en het type van de parameters moeten overeenkomen met in beide bestanden.

Schrijven in de constructorfunctie, eventuele bedrijfslogica dat u wilt uitvoeren voordat u het contract. Bijvoorbeeld, de variabelen voor de bij het starten van de waarden worden geïnitialiseerd.

Aanroepen voordat u afsluit de constructor-functie, de `ContractCreated()` functie. Deze functie waarschuwt Blockchain Workbench een contract is gemaakt.

De constructor-functie toevoegen aan uw contract in uw `HelloBlockchain.sol` codebestand smart contract. 

```
    // constructor function
    function HelloBlockchain(string message) public
    {
        Requestor = msg.sender;
        RequestMessage = message;
        State = StateType.Request;
    
        // call ContractCreated() to create an instance of this workflow
        ContractCreated();
    }
```

### <a name="functions"></a>Functions

De uitvoerbare eenheden van de bedrijfslogica binnen een contract zijn. Vereiste parameters voor de functie worden gedefinieerd als parameters van de functie in het configuratiebestand. Het aantal, de volgorde en het type van de parameters moeten overeenkomen met in beide bestanden. Functies zijn gekoppeld aan overgangen in een werkstroom Blockchain Workbench in het configuratiebestand. Een overgang is een actie uitgevoerd om te verplaatsen naar de volgende fase van de werkstroom van een toepassing zoals wordt bepaald door het contract.

Eventuele bedrijfslogica kunt schrijven die u wilt uitvoeren in de functie. Bijvoorbeeld, het wijzigen van de waarde van een variabele van de status.

Aanroepen voordat u de functie afsluit, de `ContractUpdated()` functie. De functie waarschuwt Blockchain Workbench contractstatus is bijgewerkt. Als u wilt de van statuswijzigingen in de functie ongedaan maken, roept u revert(). Terugkeren verwijderingen wijzigingen sinds de laatste aanroep ContractUpdated() status.

1. De volgende functies toevoegen aan uw contract in uw `HelloBlockchain.sol` codebestand smart contract. 

    ```
        // call this function to send a request
        function SendRequest(string requestMessage) public
        {
            if (Requestor != msg.sender)
            {
                revert();
            }
    
            RequestMessage = requestMessage;
            State = StateType.Request;
    
            // call ContractUpdated() to record this action
            ContractUpdated('SendRequest');
        }
    
        // call this function to send a response
        function SendResponse(string responseMessage) public
        {
            Responder = msg.sender;
    
            // call ContractUpdated() to record this action
            ResponseMessage = responseMessage;
            State = StateType.Respond;
            ContractUpdated('SendResponse');
        }
    }
    ```

2. Sla uw `HelloBlockchain.sol` codebestand smart contract.

## <a name="add-blockchain-application-to-blockchain-workbench"></a>Blockchain toepassing toevoegen aan Blockchain Workbench

U uploaden om toe te voegen in een toepassing blockchain Blockchain Workbench, de configuratie en slimme contract-bestanden voor het definiëren van de toepassing.

1. Ga naar het webadres Blockchain Workbench in een webbrowser. Bijvoorbeeld, `https://{workbench URL}.azurewebsites.net/` de webtoepassing wordt gemaakt wanneer u Blockchain Workbench implementeren. Zie voor meer informatie over het zoeken van de Blockchain Workbench webadres [Blockchain Workbench Web-URL](blockchain-workbench-deploy.md#blockchain-workbench-web-url)
2. Meld u aan als een [Blockchain Workbench beheerder](blockchain-workbench-manage-users.md#manage-blockchain-workbench-administrators).
3. Selecteer **toepassingen** > **nieuwe**. De **nieuwe toepassing** deelvenster wordt weergegeven.
4. Selecteer **uploaden van de configuratie van het contract** > **Bladeren** vinden de **HelloBlockchain.json** configuratiebestand dat u hebt gemaakt. Het configuratiebestand is automatisch gevalideerd. Selecteer de **weergeven** koppeling validatiefouten wordt weergegeven. Validatiefouten oplossen voordat u de toepassing implementeren.
5. Selecteer **uploaden van de Contractcode** > **Bladeren** vinden de **HelloBlockchain.sol** codebestand smart contract. De codebestand wordt automatisch gecontroleerd. Selecteer de **weergeven** koppeling validatiefouten wordt weergegeven. Validatiefouten oplossen voordat u de toepassing implementeren.
6. Selecteer **implementeren** om de blockchain-toepassing op basis van de configuratie en slimme contract bestanden te maken.

Implementatie van de toepassing blockchain duurt een paar minuten. Wanneer implementatie is voltooid, de nieuwe toepassing wordt weergegeven in **toepassingen**. 

> [!NOTE]
> U kunt ook blockchain toepassingen maken met behulp van de [REST-API van Azure Blockchain Workbench](https://docs.microsoft.com/rest/api/azure-blockchain-workbench). 

## <a name="add-blockchain-application-members"></a>Blockchain toepassing leden toevoegen

Leden van de toepassing toevoegen aan uw toepassing om te starten en er acties op opdrachten. Toepassing als leden wilt toevoegen, moet u een [Blockchain Workbench beheerder](blockchain-workbench-manage-users.md#manage-blockchain-workbench-administrators).

1. Selecteer **toepassingen** > **Hello, Blockchain!**.
2. Het aantal leden die zijn gekoppeld aan de toepassing wordt weergegeven in de rechterbovenhoek van de pagina. Voor een nieuwe toepassing, wordt het aantal leden niet nul zijn.
3. Selecteer de **leden** koppeling in de rechterbovenhoek van de pagina. Een huidige lijst met leden van de toepassing wordt weergegeven.
4. Selecteer in de lidmaatschapslijst **leden toevoegen**.
5. Selecteer of typ de naam van het lid die toe te voegen. Alleen worden Azure AD-gebruikers die zijn opgenomen in de tenant Blockchain Workbench weergegeven. Als de gebruiker niet wordt gevonden, moet u [Azure AD-gebruikers toevoegen](blockchain-workbench-manage-users.md#add-azure-ad-users).
6. Selecteer de **rol** voor het lid. Selecteer voor het eerste lid **aanvrager** als de rol.
7. Selecteer **toevoegen** het lid met de bijbehorende functieservices toevoegen aan de toepassing.
8. Een ander lid toevoegen aan de toepassing met de **Responder** rol.

Zie voor meer informatie over het beheren van gebruikers in de Blockchain Workbench [gebruikers beheren in Azure Blockchain Workbench](blockchain-workbench-manage-users.md)

## <a name="next-steps"></a>Volgende stappen

In dit artikel instructies als u een eenvoudige aanvraag en antwoord-toepassing hebt gemaakt. Voor informatie over het gebruik van de toepassing, blijven de volgende procedures voor artikel.

> [!div class="nextstepaction"]
> [Met behulp van een toepassing blockchain](blockchain-workbench-use.md)

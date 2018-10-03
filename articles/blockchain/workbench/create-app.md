---
title: Een blockchain-toepassing maken in Azure Blockchain Workbench
description: Het maken van een toepassing met blockchain in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: a7ca3f42874bc844bc0036e37a790ffebdc5f8d8
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242659"
---
# <a name="create-a-blockchain-application-in-azure-blockchain-workbench"></a>Een blockchain-toepassing maken in Azure Blockchain Workbench

U kunt Azure Blockchain Workbench gebruiken om te maken van blockchain-toepassingen die meerdere partijen werkstromen die zijn gedefinieerd door de configuratie en slimme Contractcode vertegenwoordigen.

In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een blockchain-toepassingen configureren
> * Maken van een codebestand slimme contract
> * Een blockchain Blockchain Workbench-toepassing toevoegen
> * Leden toevoegen aan de blockchain-toepassing

## <a name="prerequisites"></a>Vereisten

* Een Blockchain Workbench-implementatie. Zie voor meer informatie, [Azure Blockchain Workbench-implementatie](deploy.md) voor meer informatie over de implementatie.
* Azure Active Directory-gebruikers in de tenant die is gekoppeld met Blockchain Workbench. Zie voor meer informatie, [Azure AD-gebruikers toevoegen in Azure Blockchain Workbench](manage-users.md#add-azure-ad-users).
* Een Blockchain Workbench administrator-account. Zie voor meer informatie toevoegen [Blockchain Workbench beheerders in Azure Blockchain Workbench](manage-users.md#manage-blockchain-workbench-administrators).

## <a name="hello-blockchain"></a>Hallo, Blockchain!

We maken een eenvoudige toepassing waarin een aanvrager verzendt een aanvraag en een eindpunt verzenden van een antwoord op de aanvraag. Bijvoorbeeld, een aanvraag kan zijn, "Hallo, hoe gaat het?", en het antwoord kan worden, "Ik ben geweldige!". Zowel de aanvraag en het antwoord worden op de onderliggende blockchain geregistreerd. 

Volg de stappen voor het maken van bestanden voor de toepassing of u kunt [downloaden van het voorbeeld uit GitHub](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/hello-blockchain). 

## <a name="configuration-file"></a>Configuratiebestand

Metagegevens van de configuratie definieert de werkstromen op hoog niveau en het model van de interactie van de blockchain-toepassing. Metagegevens van de configuratie vertegenwoordigt de fasen van de werkstroom en het model van de interactie van de blockchain-toepassing.

1. Maak een bestand met de naam in uw favoriete editor `HelloBlockchain.json`.
2. Voeg de volgende JSON voor het definiëren van de configuratie van de blockchain-toepassing.

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

Het begin van het configuratiebestand bevat informatie over de toepassing met inbegrip van naam en beschrijving.

### <a name="application-roles"></a>Toepassingsrollen

Het gedeelte van de rollen toepassing definieert de gebruikersrollen die kunnen optreden of nemen in de blockchain-toepassing. U definieert een reeks afzonderlijke functies op basis van functionaliteit. Er is een verschil tussen de functionaliteit van een aanvrager als een entiteit die wordt geproduceerd aanvragen en een eindpunt als een entiteit die wordt geproduceerd antwoorden in het scenario voor request response.

### <a name="workflows"></a>Werkstromen

Werkstromen definiëren een of meer fasen en acties van het contract. In de request response-scenario is de eerste fase (status) van de werkstroom dat een aanvrager (rol) heeft een actie (overgang) om een aanvraag (functie) te verzenden. De volgende fase (status) is dat een eindpunt (rol) heeft een actie (overgang) om een antwoord (functie) te verzenden. Werkstroom van een toepassing kan betrekking hebben op eigenschappen, functies, en Staten vereist beschrijven de stroom van een contract. 

Zie voor meer informatie over de inhoud van configuratiebestanden, [naslaginformatie voor configuratie van Azure Blockchain werkstroom](configuration.md).

## <a name="smart-contract-code-file"></a>Slimme contract codebestand

Slimme contracten vertegenwoordigen de zakelijke logica van de blockchain-toepassing. Op dit moment ondersteunt Blockchain Workbench Ethereum voor het grootboek met blockchain. Maakt gebruik van Ethereum [Solidity](https://solidity.readthedocs.io) de programmeertaal voor het schrijven van zelf uitvoerende zakelijke logica voor slimme contracten.

Slimme contracten in Solidity zijn vergelijkbaar met de klassen in objectgeoriënteerde talen. Elke opdracht bevat functies voor het implementeren van fasen en acties van het contract slimme en status.

Maak een bestand met de naam in uw favoriete editor `HelloBlockchain.sol`.

### <a name="version-pragma"></a>Versie pragma

Als een best practice, geven de versie van Solidity die u ontwikkelt. De versie op te geven, kunt voorkomen van compatibiliteitsproblemen met toekomstige versies van Solidity. 

De volgende versie pragma toevoegen aan de bovenkant van `HelloBlockchain.sol` slimme contract codebestand.


  ``` solidity
  pragma solidity ^0.4.20;
  ```

### <a name="base-class"></a>Basisklasse

**WorkbenchBase** basisklasse kunt Blockchain Workbench maken en bijwerken van het contract. De basisklasse is vereist voor Blockchain Workbench specifieke slimme Contractcode. Uw contract moet worden overgenomen van de **WorkbenchBase** basisklasse.

In `HelloBlockchain.sol` slimme codebestand contract, voegt u de **WorkbenchBase** klasse aan het begin van het bestand. 

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

|Basisklasse functie  | Doel  | Wanneer u aan te roepen  |
|---------|---------|---------|
| ContractCreated() | Ontvangt een melding Blockchain Workbench een contract is gemaakt | Voordat u afsluit de contract-constructor |
| ContractUpdated() | Ontvangt een melding Blockchain Workbench een contractstatus is bijgewerkt | Voordat u de functie van een contract afsluiten |

### <a name="configuration-and-smart-contract-code-relationship"></a>Configuratie en slimme contract code relatie

Blockchain Workbench maakt gebruik van de configuratie van bestands- en slimme contract codebestand een blockchain-toepassing maken. Er is een relatie tussen is gedefinieerd in de configuratie en de code in het smart contract. Podrobnosti o kontraktu, functies, parameters en typen moeten overeenkomen met om de toepassing te maken. Blockchain Workbench controleert de bestanden voorafgaand aan het maken van de toepassing. 

### <a name="contract"></a>contract

Voor Blockchain Workbench, contracten wilt overnemen van de **WorkbenchBase** basisklasse. Bij het melden van de overeenkomst, moet u de naam van de en naam van de werkstroom door te geven als argumenten.

Toevoegen de **contract** header aan uw `HelloBlockchain.sol` slimme contract codebestand. 

```
contract HelloBlockchain is WorkbenchBase('HelloBlockchain', 'HelloBlockchain') {
```

Uw contract moet worden overgenomen van de **WorkbenchBase** basisklasse en de parameters doorgeven **ApplicationName** en de werkstroom **naam** zoals gedefinieerd in de configuratie het bestand. In dit geval zijn de toepassingsnaam en de naam van de werkstroom hetzelfde.

### <a name="state-variables"></a>Variabelen voor de status

Variabelen voor de opslaan waarden van de status voor elk exemplaar van het contract. De status van variabelen in uw contract moeten overeenkomen met de werkstroomeigenschappen die zijn gedefinieerd in het configuratiebestand.

De variabelen toevoegen aan uw contract in uw `HelloBlockchain.sol` slimme contract codebestand. 

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

De constructor definieert invoerparameters voor een nieuwe slimme contract exemplaar van een werkstroom. De constructor is gedeclareerd als een functie met dezelfde naam als het contract. Vereiste parameters voor de constructor worden gedefinieerd als parameters van constructor in het configuratiebestand. Het aantal, de volgorde en het type van de parameters moeten overeenkomen met in beide bestanden.

Schrijven in de constructorfunctie eventuele bedrijfslogica dat u wilt uitvoeren voordat ze het contract maken. Bijvoorbeeld, de variabelen bij het starten van de waarden worden geïnitialiseerd.

Voordat u de constructorfunctie afsluit, Roep de `ContractCreated()` functie. Deze functie ontvangt een melding Blockchain Workbench een contract is gemaakt.

De constructorfunctie toevoegen aan uw contract in uw `HelloBlockchain.sol` slimme contract codebestand. 

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

Functies zijn de uitvoerbare eenheden van bedrijfslogica binnen een contract. Vereiste parameters voor de functie worden gedefinieerd als parameters van de functie in het configuratiebestand. Het aantal, de volgorde en het type van de parameters moeten overeenkomen met in beide bestanden. Functies zijn gekoppeld aan de overgangen in een werkstroom Blockchain Workbench in het configuratiebestand. Een overgang is een actie uitgevoerd om te verplaatsen naar de volgende fase van de werkstroom van een toepassing zoals wordt bepaald door het contract.

Eventuele bedrijfslogica kunt schrijven die u wilt uitvoeren in de functie. Bijvoorbeeld, het wijzigen van de waarde van een variabele van de status.

Voordat u de functie wordt afgesloten, Roep de `ContractUpdated()` functie. De functie melding Blockchain Workbench contractstatus is bijgewerkt. Als u de van statuswijzigingen in de functie ongedaan wilt maken, roept u revert(). Terugkeren verwijderingen wijzigingen sinds de laatste aanroep ContractUpdated() staat.

1. Voeg de volgende functies toe aan uw contract in uw `HelloBlockchain.sol` slimme contract codebestand. 

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

2. Sla uw `HelloBlockchain.sol` slimme contract codebestand.

## <a name="add-blockchain-application-to-blockchain-workbench"></a>Blockchain toepassing toevoegen aan Blockchain Workbench

Als u wilt toevoegen een blockchain-toepassing Blockchain Workbench, uploadt u de configuratie en slimme contract bestanden voor het definiëren van de toepassing.

1. Navigeer in een webbrowser naar het webadres Blockchain Workbench. Bijvoorbeeld, `https://{workbench URL}.azurewebsites.net/` de web-App is gemaakt bij het implementeren van Blockchain Workbench. Zie voor meer informatie over het vinden van uw Blockchain Workbench webadres [Blockchain Workbench Web-URL](deploy.md#blockchain-workbench-web-url)
2. Meld u aan als een [Blockchain Workbench beheerder](manage-users.md#manage-blockchain-workbench-administrators).
3. Selecteer **toepassingen** > **nieuwe**. De **nieuwe toepassing** deelvenster wordt weergegeven.
4. Selecteer **uploaden van de configuratie van de overeenkomst** > **Bladeren** vinden de **HelloBlockchain.json** configuratiebestand dat u hebt gemaakt. Het configuratiebestand wordt automatisch gevalideerd. Selecteer de **weergeven** koppeling om weer te geven van validatiefouten. Validatiefouten oplossen voordat u de toepassing implementeren.
5. Selecteer **uploaden van de Contractcode** > **Bladeren** vinden de **HelloBlockchain.sol** slimme contract codebestand. Het codebestand wordt automatisch gevalideerd. Selecteer de **weergeven** koppeling om weer te geven van validatiefouten. Validatiefouten oplossen voordat u de toepassing implementeren.
6. Selecteer **implementeren** de blockchain-toepassing op basis van de configuratie en slimme contract bestanden maken.

Implementatie van de toepassing blockchain duurt een paar minuten. Wanneer de implementatie is voltooid, de nieuwe toepassing wordt weergegeven in **toepassingen**. 

> [!NOTE]
> U kunt ook blockchain-toepassingen maken met behulp van de [REST-API van Azure Blockchain Workbench](https://docs.microsoft.com/rest/api/azure-blockchain-workbench). 

## <a name="add-blockchain-application-members"></a>Leden van blockchain-toepassing toevoegen

Leden van de toepassing toevoegen aan uw toepassing te starten en actie ondernemen op opdrachten. Als u wilt toevoegen van leden van de toepassing, moet u een [Blockchain Workbench beheerder](manage-users.md#manage-blockchain-workbench-administrators).

1. Selecteer **toepassingen** > **Hallo, Blockchain!**.
2. Het aantal leden dat is gekoppeld aan de toepassing wordt weergegeven in de rechterbovenhoek van de pagina. Voor een nieuwe toepassing wordt het aantal leden nul zijn.
3. Selecteer de **leden** koppeling in de rechterbovenhoek van de pagina. Een huidige lijst met leden voor de toepassing wordt weergegeven.
4. Selecteer in de lijst met leden **leden toevoegen**.
5. Selecteer of typ de naam van het lid die u wilt toevoegen. Alleen de Azure AD-gebruikers die zijn opgenomen in de tenant Blockchain Workbench worden weergegeven. Als de gebruiker niet wordt gevonden, moet u [Azure AD-gebruikers toevoegen](manage-users.md#add-azure-ad-users).
6. Selecteer de **rol** voor het lid. Selecteer voor het eerste lid **aanvrager** als de rol.
7. Selecteer **toevoegen** het lid met de bijbehorende functieservices toevoegen aan de toepassing.
8. Een ander lid toevoegen aan de toepassing met de **Responder** rol.

Zie voor meer informatie over het beheren van gebruikers in Blockchain Workbench [gebruikers beheren in Azure Blockchain Workbench](manage-users.md)

## <a name="next-steps"></a>Volgende stappen

In dit artikel met instructies, hebt u een eenvoudige toepassing voor de aanvraag en respons gemaakt. Voor informatie over het gebruik van de toepassing, blijven de volgende procedures voor artikel.

> [!div class="nextstepaction"]
> [Met behulp van een blockchain-toepassing](use.md)

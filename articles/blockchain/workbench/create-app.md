---
title: Een blockchain-toepassing maken in Azure Blockchain Workbench
description: Zelfstudie over het maken van een blockchain-toepassing in Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/30/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: brendal
manager: femila
ms.openlocfilehash: b444ad799eaa356d654952c32ac58188de8d7131
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417382"
---
# <a name="tutorial-create-a-blockchain-application-in-azure-blockchain-workbench"></a>Zelfstudie: Een blockchain-toepassing maken in Azure Blockchain Workbench

U kunt Azure Blockchain Workbench gebruiken om een blockchain-toepassing te maken waarmee werkstromen voor meerdere partijen worden gerepresenteerd die door configuratie en slimme-contractcode worden gedefinieerd.

U leert het volgende:

> [!div class="checklist"]
> * Een blockchain-toepassing configureren
> * Maken van een smart contract codebestand
> * Een blockchain-toepassing toevoegen aan Blockchain Workbench
> * Leden toevoegen aan de blockchain-toepassing

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Een Blockchain Workbench-implementatie. Zie [Azure Blockchain Workbench-implementatie](deploy.md) voor meer informatie over de implementatie.
* Gebruikers van Azure Active Directory in de tenant die aan Blockchain Workbench is gekoppeld. Zie [Azure AD-gebruikers toevoegen in Azure Blockchain Workbench](manage-users.md#add-azure-ad-users) voor meer informatie.
* Een Blockchain Workbench-beheerdersaccount. Zie voor meer informatie toevoegen van [Blockchain Workbench beheerders in Azure Blockchain Workbench](manage-users.md#manage-blockchain-workbench-administrators).

## <a name="hello-blockchain"></a>Hallo Blockchain.

We maken een eenvoudige toepassing waarin een aanvrager een aanvraag verzendt en een eindpunt een antwoord verzendt op deze aanvraag.
Een aanvraag kan bijvoorbeeld zijn, "Hallo, hoe gaat het?", en het antwoord kan worden, "Het gaat geweldig!". Zowel de aanvraag als het antwoord worden op de onderliggende blockchain geregistreerd.

Volg de stappen voor het maken van bestanden voor de toepassing of u kunt deze [downloaden van het voorbeeld uit GitHub](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/hello-blockchain).

## <a name="configuration-file"></a>Configuratiebestand

Metagegevens van de configuratie definieren de werkstromen op hoog niveau en het model van de interactie van de blockchain-toepassing. Metagegevens van de configuratie beschrijven de fasen van de werkstroom en het model van de interactie van de blockchain-toepassing.

1. Maak een bestand met de naam `HelloBlockchain.json` in uw favoriete editor.
2. Plaats in dit bestand de volgende JSON voor het definiëren van de configuratie van de blockchain-toepassing.

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

Het configuratiebestand heeft meerdere secties. De details over de secties zijn als volgt:

### <a name="application-metadata"></a>Toepassingsmetagegevens

het begin van het configuratiebestand bevat informatie over de toepassing, waaronder de naam en een beschrijving.

### <a name="application-roles"></a>Toepassingsrollen

Het gedeelte van de toepassingsrollen definieert de gebruikersrollen die kunnen deelnemen in de blockchain-toepassing. U definieert een verzameling afzonderlijke rollen over functionaliteit. In het scenario voor request-response is er een verschil tussen de functionaliteit van een aanvrager als een entiteit die aanvragen genereert en een eindpunt als een entiteit die antwoorden produceert.

### <a name="workflows"></a>Werkstromen

Met werkstromen worden een of meer fasen en acties van het contract gedefinieerd. In het request-response scenario is de eerste fase (status) van de werkstroom dat een aanvrager (rol) een actie (overgang) onderneemt om een aanvraag (functie) te verzenden. De volgende fase (status) is dat een eindpunt (rol) een actie (overgang) onderneemt om een antwoord (functie) te verzenden. De werkstroom van een toepassing kan betrekking hebben op eigenschappen, functies, en statussen die vereist zijn voor het beschrijven van de werkstroom van een contract.

Zie [Naslaginformatie over Azure Blockchain Workbench-configuratie](configuration.md) voor meer informatie over de inhoud van configuratiebestanden.

## <a name="smart-contract-code-file"></a>Smart contract codebestand

Smart contracts vertegenwoordigen de zakelijke logica van de blockchain-toepassing. Op dit moment ondersteunt Blockchain Workbench Ethereum voor het blockchain-grootboek. Ethereum maakt gebruik van [Solidity](https://solidity.readthedocs.io) als programmeertaal voor het schrijven van zelf uitvoerende zakelijke logica voor smart contracts.

Smart contracts in Solidity zijn vergelijkbaar met de klassen in objectgeoriënteerde talen. Elke opdracht bevat functies voor het implementeren van fasen en acties van het smart contract.

Maak een bestand met de naam `HelloBlockchain.sol` in uw favoriete editor.

### <a name="version-pragma"></a>Versiepragma

Als een best practice geeft u de versie op van Solidity waarmee u ontwikkelt. Door de versie op te geven kunt u  compatibiliteitsproblemen met toekomstige versies van Solidity voorkomen.

Voeg de volgende versiepragma toe aan de bovenkant van bestand `HelloBlockchain.sol` met de slimme-contractcode.

``` solidity
pragma solidity >=0.4.25 <0.6.0;
```

### <a name="configuration-and-smart-contract-code-relationship"></a>Relatie tussen configuratie en slimme-contractcode

Blockchain Workbench maakt gebruik van het configuratiebestand en het smart contract codebestand om een blockchain-toepassing te maken. Er is een relatie tussen wat gedefinieerd is in de configuratie en de code in het smart contract. Details van het contract, functies, parameters en typen moeten overeenkomen om de toepassing te maken. Blockchain Workbench verifieert de bestanden voordat de toepassing wordt gemaakt.

### <a name="contract"></a>Contract

Voeg de kop van het **contract** toe aan bestand `HelloBlockchain.sol` met de slimme-contractcode.

``` solidity
contract HelloBlockchain {
```

### <a name="state-variables"></a>Statusvariabelen

Status-variabelen bewaren de status van elke instantie van het contract. De status-variabelen in uw contract moeten overeenkomen met de werkstroomeigenschappen die zijn gedefinieerd in het configuratiebestand.

Voeg de variabelen toe aan uw contract in uw `HelloBlockchain.sol` smart contract codebestand.

``` solidity
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

De constructor definieert invoerparameters voor een nieuwe instantie van een smart contract van een werkstroom. Vereiste parameters voor de constructor worden gedefinieerd als parameters van de constructor in het configuratiebestand. Het aantal, de volgorde en het type van de parameters moeten overeenkomen in beide bestanden.

In de constructorfunctie schrijft u eventuele bedrijfslogica die u wilt uitvoeren voordat het contract gemaakt wordt. Initialiseer bijvoorbeeld de statusvariabelen met beginwaarden.

Voeg de constructorfunctie toe aan uw contract in bestand `HelloBlockchain.sol` voor de slimme-contractcode.

``` solidity
    // constructor function
    constructor(string memory message) public
    {
        Requestor = msg.sender;
        RequestMessage = message;
        State = StateType.Request;
    }
```

### <a name="functions"></a>Functions

Functies zijn de uitvoerbare eenheden van de bedrijfslogica in een contract. Vereiste parameters voor de functie worden in het configuratiebestand gedefinieerd als functieparameters. Het aantal, de volgorde en het type van de parameters moeten overeenkomen in beide bestanden. Functies zijn gekoppeld aan de overgangen in een Blockchain Workbench-workflow zoals gedefinieerd in het configuratiebestand. Een overgang is een actie die wordt uitgevoerd om naar de volgende fase van de werkstroom te gaan van een toepassing zoals is bepaald door het contract.

U kunt elke bedrijfslogica laten uitvoeren in de functie. Bijvoorbeeld het wijzigen van de waarde van een variabele.

1. Voeg de volgende functies toe aan uw contract in bestand `HelloBlockchain.sol` voor de slimme-contractcode.

    ``` solidity
        // call this function to send a request
        function SendRequest(string memory requestMessage) public
        {
            if (Requestor != msg.sender)
            {
                revert();
            }
    
            RequestMessage = requestMessage;
            State = StateType.Request;
        }
    
        // call this function to send a response
        function SendResponse(string memory responseMessage) public
        {
            Responder = msg.sender;
    
            ResponseMessage = responseMessage;
            State = StateType.Respond;
        }
    }
    ```

2. Sla uw `HelloBlockchain.sol` smart contract codebestand op.

## <a name="add-blockchain-application-to-blockchain-workbench"></a>Blockchain-toepassing toevoegen aan Blockchain Workbench

Als u een blockchain-toepassing wilt toevoegen aan Blockchain Workbench, uploadt u de configuratie en smart contractbestanden voor het definiëren van de toepassing.

1. Navigeer in een webbrowser naar het webadres van Blockchain Workbench. bijvoorbeeld `https://{workbench URL}.azurewebsites.net/`. De webtoepassing wordt gemaakt als u Blockchain Workbench implementeert. Zie [Web-URL van Blockchain Workbench](deploy.md#blockchain-workbench-web-url) voor informatie over waar u het webadres van Blockchain Workbench kunt vinden
2. Meld u aan als een [Blockchain Workbench-beheerder](manage-users.md#manage-blockchain-workbench-administrators).
3. Selecteer **Toepassingen** > **Nieuw**. Het **nieuwe toepassing** deelvenster wordt weergegeven.
4. Selecteer **uploaden van de configuratie van het contract** > **Bladeren** om het **HelloBlockchain.json** configuratiebestand dat u hebt gemaakt te vinden. Het configuratiebestand wordt automatisch gevalideerd. Selecteer de **weergeven** koppeling om validatiefouten weer te geven. Los validatiefouten op voordat u de toepassing implementeert.
5. Selecteer **uploaden van de Contractcode** > **Bladeren** om het **HelloBlockchain.sol** smart contract codebestand te vinden. Het codebestand wordt automatisch gevalideerd. Selecteer de **weergeven** koppeling om validatiefouten weer te geven. Los validatiefouten op voordat u de toepassing implementeert.
6. Selecteer **implementeren** om de blockchain-toepassing op basis van de configuratie en smart contract bestanden te maken.

De implementatie van de blockchain-toepassing duurt een paar minuten. Wanneer de implementatie is voltooid, wordt de nieuwe toepassing weergegeven in **toepassingen**. 

> [!NOTE]
> U kunt blockchain-toepassingen ook maken met behulp van de [REST API voor Azure Blockchain Workbench](https://docs.microsoft.com/rest/api/azure-blockchain-workbench).

## <a name="add-blockchain-application-members"></a>Leden voor blockchain-toepassing toevoegen

Voeg leden van de toepassing toe aan uw toepassing om acties in het contract te starten en uit te voeren. Om leden van de toepassing toe te voegen moet u een [Blockchain Workbench beheerder](manage-users.md#manage-blockchain-workbench-administrators) zijn.

1. Selecteer **Toepassingen** > **Hallo Blockchain.** .
2. Het aantal leden dat is gekoppeld aan de toepassing wordt weergegeven in de rechterbovenhoek van de pagina. Voor een nieuwe toepassing zal het aantal leden nul zijn.
3. Selecteer de koppeling **leden** in de rechterbovenhoek van de pagina. De huidige ledenlijst voor de toepassing wordt weergegeven.
4. Selecteer **Leden toevoegen** in de ledenlijst.
5. Selecteer of typ de naam van het lid dat u wilt toevoegen. Alleen Azure AD-gebruikers die voorkomen in de Blockchain Workbench-tenant staan vermeld. Als de gebruiker niet wordt gevonden, moet u deze [Azure AD-gebruikers toevoegen](manage-users.md#add-azure-ad-users).
6. Selecteer de **Rol** voor het lid. Voor het eerste lid selecteert u **Aanvrager** als de rol.
7. Selecteer **toevoegen** om het lid met de bijbehorende functieservices toe te voegen aan de toepassing.
8. Voeg een ander lid toe aan de toepassing met de **Responder** rol.

Zie [Gebruikers beheren in Azure Blockchain Workbench](manage-users.md) voor meer informatie over het beheren van gebruikers in Blockchain Workbench

## <a name="next-steps"></a>Volgende stappen

In dit artikel met instructies hebt u een eenvoudige toepassing voor aanvraag en respons gemaakt. Voor informatie over het gebruik van de toepassing, ga door naar het volgende how-to artikel.

> [!div class="nextstepaction"]
> [Een blockchain-toepassing gebruiken](use.md)

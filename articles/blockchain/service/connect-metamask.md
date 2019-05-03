---
title: MetaMask verbinden met een netwerk met Azure Blockchain-Service
description: Verbinding maken met een Service van Azure Blockchain-netwerk met behulp van MetaMask en implementeren van een slimme contract.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: db029cee6edcd14d29c83964e5bf75aa45077e7e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026898"
---
# <a name="quickstart-use-metamask-to-connect-and-deploy-a-smart-contract"></a>Quickstart: MetaMask gebruiken voor verbinding maken en implementeren van een slimme contract

In deze snelstartgids gebruikt u MetaMask verbinding maken met een netwerk met Azure Blockchain-Service en Remix gebruiken voor het implementeren van een slimme contract. Metamask is een Browseruitbreiding voor het beheren van een wallet Ether en slimme contract acties uitvoeren.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Een Azure Blockchain-lid maken](create-member.md)
* Installeer [MetaMask browserextensie](https://metamask.io)
* Genereren van een MetaMask [wallet](https://metamask.zendesk.com/hc/en-us/articles/360015488971-New-to-MetaMask-Learn-How-to-Setup-MetaMask-the-First-Time)

## <a name="get-endpoint-address"></a>Adres van eindpunt ophalen

U moet het adres van het Azure Blockchain-Service-eindpunt verbinding maken met de blockchain-netwerk. U kunt het eindpunt van de adres- en toegangsbeheer sleutels vinden in Azure portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw Service voor Azure Blockchain-lid. Selecteer **transactie knooppunten** en de standaardkoppeling van de transactie-knooppunt.

    ![Standaard transactie knooppunt selecteren](./media/connect-metamask/transaction-nodes.png)

1. Selecteer **verbindingsreeksen > toegangssleutels**.
1. Kopieer het adres van het eindpunt van **HTTPS (toegangssleutel 1)**. U moet het adres voor de volgende sectie.

    ![Verbindingsreeks](./media/connect-metamask/connection-string.png)

## <a name="connect-metamask"></a>Verbinding maken met MetaMask

1. Uitbreiding van de browser MetaMask openen en meld u aan.
1. Selecteer in de vervolgkeuzelijst netwerk **aangepaste RPC**.

    ![Aangepaste RPC](./media/connect-metamask/custom-rpc.png)

1. In **nieuw netwerk > nieuwe RPC-URL**, voer het adres van uw eindpunt uit de vorige sectie hebt gekopieerd.
1. Selecteer **Opslaan**.

    Als de verbinding is geslaagd, wordt het particuliere netwerk weergegeven in de vervolgkeuzelijst van het netwerk.

    ![Nieuw netwerk](./media/connect-metamask/new-network.png)

## <a name="deploy-smart-contract"></a>Slimme contract implementeren

Remix is een browser gebaseerde Solidity-ontwikkelomgeving. Met MetaMask en Remix samen, kunt u implementeren en acties uitvoeren op een slimme contracten.

1. Ga in de browser naar `https://remix.ethereum.org`.
1. Selecteer **Uitvoeren**. 

    MetaMask stelt uw **omgeving** naar **geïnjecteerd Web3** en **Account** met uw netwerk.

    ![Tabblad uitvoeren](./media/connect-metamask/injected-web3.png)

1. Selecteer **nieuw bestand maken**.

    Noem het nieuwe bestand `simple.sol`.

    ![Bestand maken](./media/connect-metamask/create-file.png)

    Selecteer **OK**.

1. Plak in het volgende in de editor Remix **eenvoudig intelligente contract** code.

    ```solidity
    pragma solidity ^0.5.0;
             
    contract simple {
        uint balance;
                 
        constructor() public{
            balance = 0;
        }
                 
        function add(uint _num) public {
            balance += _num;
        }
                 
        function get() public view returns (uint){
            return balance;
        }
    }
    ```

    De **eenvoudige contract** declareert een status-variabele met de naam **saldo**. Er zijn twee functies die zijn gedefinieerd. De **toevoegen** functie voegt een getal tot **saldo**. De **ophalen** functie retourneert de waarde van **saldo**.

1. Samengesteld op basis van het contract, selecteer de **compileren > starten voor het compileren van**. Als dit lukt, wordt een groen vak met de naam van het servicecontract wordt weergegeven.

    ![Compileren](./media/connect-metamask/compile.png)

1. Voor het uitvoeren van het contract, selecteer de **uitvoeren** tabblad. Selecteer de **eenvoudige** vervolgens contract **implementeren**.

    ![Aangepaste RPC](./media/connect-metamask/deploy.png)

1. Een melding MetaMask weergegeven u onvoldoende middelen om uit te voeren van de transactie.

    Voor een openbare blockchain-netwerk moet u Ether om te betalen voor de transactiekosten. Omdat dit een particulier netwerk in een consortium, kunt u gas prijs instellen op nul.

1.  Selecteer **Gas kosten > Bewerken > Geavanceerd**, stel de **Gas prijs** op 0.

    ![Gas prijs](./media/connect-metamask/gas-price.png)

    Selecteer **Opslaan**.

1. Selecteer **bevestigen** de slimme contract implementeren op de blockchain.
1. In de **geïmplementeerd contracten** sectie uit, vouw de **eenvoudige** contract.

    ![Geïmplementeerde contract](./media/connect-metamask/deployed-contract.png)

    Er zijn twee acties **toevoegen** en **ophalen** die worden toegewezen aan de functies die zijn gedefinieerd in het contract.

1. Om uit te voeren een **toevoegen** transactie op de blockchain, voer een getal toevoegen en selecteer vervolgens **toevoegen**.
1. Net als bij de implementatie van het contract, een MetaMask melding weergegeven u onvoldoende middelen om uit te voeren van de transactie.

    Omdat dit een particulier netwerk in een consortium, kunnen we gas prijs ingesteld op nul.

1.  Selecteer **Gas kosten > Bewerken > Geavanceerd**, stel de **Gas prijs** op 0, en selecteer **opslaan**.
1. Selecteer **bevestigen** om uit te voeren van de transactie op de blockchain.
1. Selecteer **ophalen** actie. Dit is een aanroep knooppunt om gegevens te doorzoeken. Een transactie is niet nodig.
1. In het deelvenster Foutopsporing van Remix ziet u meer informatie over de transacties op de blockchain.

    ![Fouten opsporen in geschiedenis](./media/connect-metamask/debug.png)

    Ziet u de **eenvoudige** contract maken, transactie voor **simple.add**, en aanroepen van **simple.get**.

1. U ziet ook de transactiegeschiedenis in MetaMask. Open de browser MetaMask extensie.
1. In de **geschiedenis** sectie ziet u een logboek van het geïmplementeerde contract en transacties.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gebruikt u de browserextensie MetaMask verbinding maken met een knooppunt van de transactie Azure Blockchain-Service, het implementeren van een slimme contract en het verzenden van een transactie naar de blockchain. Probeer de volgende zelfstudie voor het implementeren en verzenden van een transactie met behulp van Truffle.

> [!div class="nextstepaction"]
> [Verzenden van een transactie](send-transaction.md)
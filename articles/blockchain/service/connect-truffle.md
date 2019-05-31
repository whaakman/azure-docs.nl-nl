---
title: Verbinding maken met behulp van Truffle
description: Verbinding maken met een Service van Azure Blockchain-netwerk met behulp van Truffle
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 8b1a701beac867c5f331ffa1ee1dee615961c6b3
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416302"
---
# <a name="quickstart-use-truffle-to-connect-to-an-azure-blockchain-service-network"></a>Quickstart: Gebruik Truffle verbinding maken met een netwerk met Azure Blockchain-Service

Truffle is een blockchain-ontwikkelomgeving die kunt u verbinding maken met een Azure Blockchain-knooppunt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Een Azure Blockchain-lid maken](create-member.md)
* Installeer [Truffle](https://github.com/trufflesuite/truffle). Truffle vereist verschillende hulpprogramma's om te worden geïnstalleerd met [Node.js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
* Installeer [Python 2.7.15](https://www.python.org/downloads/release/python-2715/). Python is nodig voor Web3.

## <a name="create-truffle-project"></a>Truffle-project maken

1. Open een Node.js-opdrachtprompt of de shell.
1. Wijzig de map waar u wilt maken van de projectmap Truffle.
1. Maak een map voor het project en wijzig het pad naar de nieuwe map. Bijvoorbeeld:

    ``` bash
    mkdir truffledemo
    cd truffledemo
    ```

1. Het project Truffle initialiseren.

    ``` bash
    truffle init
    ```

1. Installeer web3 Ethereum JavaScript-API in de projectmap. Op dit moment is versie web3 versie 1.0.0-beta.37 vereist.

    ``` bash
    npm install web3@1.0.0-beta.37
    ```

    U ontvangt mogelijk npm waarschuwingen tijdens de installatie.

1. Start de Truffle interactieve ontwikkeling-console.

    ``` bash
    truffle develop
    ```

    Truffle maakt een blockchain lokale ontwikkeling en biedt een interactieve console.

## <a name="connect-to-transaction-node"></a>Verbinding maken met transactieknooppunt

Gebruik *Web3* verbinding maken met het knooppunt transactie. U krijgt de *Web3* connection string vanuit de Azure-portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw Service voor Azure Blockchain-lid. Selecteer **transactie knooppunten** en de standaardkoppeling van de transactie-knooppunt.

    ![Standaard transactie knooppunt selecteren](./media/connect-truffle/transaction-nodes.png)

1. Selecteer **voorbeeldcode > Web3**.
1. Kopieer het JavaScript van **HTTPS (toegangssleutel 1)** . U moet de code voor Truffle van interactieve ontwikkeling-console.

    ![Web3 code](./media/connect-truffle/web3-code.png)

1. Plak de JavaScript-code uit de vorige stap in de console van de interactieve ontwikkeling Truffle. De code maakt een web3-object dat is verbonden met uw Azure Blockchain-transactie-knooppunt.

    Voorbeelduitvoer:

    ```bash
    truffle(develop)> var Web3 = require("Web3");
    truffle(develop)> var provider = new Web3.providers.HttpProvider("https://myblockchainmember.blockchain.azure.com:3200/hy5FMu5TaPR0Zg8GxiPwned");
    truffle(develop)> var web3 = new Web3(provider);
    ```

    U kunt methoden aanroepen op de **web3** object om te communiceren met uw transactie-knooppunt.

1. Roep de **getBlockNumber** methode om te retourneren van het nummer van de huidige blokkeren.

    ```bash
    web3.eth.getBlockNumber();
    ```

    Voorbeelduitvoer:

    ```bash
    truffle(develop)> web3.eth.getBlockNumber();
    18567
    ```
1. Sluit de console Truffle-ontwikkeling.

    ```bash
    .exit
    ```

## <a name="next-steps"></a>Volgende stappen

In deze quickstart maakt u een project Truffle verbinding maken met uw Azure Blockchain Service standaard transactie knooppunt gemaakt.

Probeer de volgende zelfstudie voor het gebruik van Truffle voor het verzenden van een transactie met uw consortium blockchain-netwerk.

> [!div class="nextstepaction"]
> [Verzenden van een transactie](send-transaction.md)
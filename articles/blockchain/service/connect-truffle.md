---
title: Verbinding maken met behulp van Truffle
description: Verbinding maken met een Service van Azure Blockchain-netwerk met behulp van Truffle
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 037f37d6a8e1c41579403dbf7c9dd265efbb5d10
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026954"
---
# <a name="quickstart-use-truffle-to-connect-to-a-an-azure-blockchain-service-network"></a>Quickstart: Truffle gebruiken voor verbinding met een een Blockchain-Service van Azure-netwerk

Truffle is een blockchain-ontwikkelomgeving die kunt u verbinding maken met een Azure Blockchain-knooppunt.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Een Azure Blockchain-lid maken](create-member.md)
* Truffle vereist verschillende hulpprogramma's om te worden geïnstalleerd met [Node.js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), en [Truffle](https://github.com/trufflesuite/truffle).

    Om in te stellen snel op Windows 10, installeer [Ubuntu in Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6) Installeer voor een Unix-Bash-shell terminal [Truffle](https://github.com/trufflesuite/truffle). Het Ubuntu op Windows-distributiepunt bevat Node.js en Git.

## <a name="create-truffle-project"></a>Truffle-project maken

1. Open een Bash-shell-terminal.
1. Wijzig de map waar u wilt maken van de projectmap Truffle. Bijvoorbeeld:

    ``` bash
    cd /mnt/c
    ```

1. Maak een map voor het project en wijzig het pad naar de nieuwe map. Bijvoorbeeld:

    ``` bash
    mkdir truffledemo
    cd truffledemo
    ```

1. Installeer web3 Ethereum JavaScript-API in de projectmap. Op dit moment is versie web3 versie 1.0.0-beta.37 vereist.

    ``` bash
    npm install web3@1.0.0-beta.37
    ```

    U ontvangt mogelijk npm waarschuwingen tijdens de installatie.

1. Het project Truffle initialiseren.

    ``` bash
    truffle init
    ```

1. Start de Truffle interactieve ontwikkeling-console.

    ``` bash
    truffle develop
    ```

    Truffle maakt een blockchain lokale ontwikkeling en biedt een interactieve console.

## <a name="connect-to-transaction-node"></a>Verbinding maken met transactie-knooppunt

We Web3 gebruiken om verbinding maken met de transactie-knooppunt. U kunt de Web3-verbindingsreeks ophalen uit de Azure-portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw Service voor Azure Blockchain-lid. Selecteer **transactie knooppunten** en de standaardkoppeling van de transactie-knooppunt.

    ![Standaard transactie knooppunt selecteren](./media/connect-truffle/transaction-nodes.png)

1. Selecteer **voorbeeldcode > Web3**.
1. Kopieer het JavaScript van **HTTPS (toegangssleutel 1)**. U moet de code voor Truffle van interactieve ontwikkeling-console.

    ![Web3 code](./media/connect-truffle/web3-code.png)

1. Plak de JavaScript-code uit de vorige stap in de console van de interactieve ontwikkeling Truffle. De code maakt een web3-object dat is verbonden met uw Azure Blockchain-transactie-knooppunt.

    Voorbeelduitvoer:

    ```bash
    truffle(develop)> var Web3 = require("Web3");
    truffle(develop)> var provider = new Web3.providers.HttpProvider("https://myblockchainmember.blockchain.azure.com:3200/hy5FMu5TaPR0Zg8GxiPwned");
    truffle(develop)> var web3 = new Web3(provider);
    truffle(develop)>
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
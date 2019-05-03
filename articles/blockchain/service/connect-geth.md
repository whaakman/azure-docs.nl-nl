---
title: Gebruik Geth verbinding maken met Azure Blockchain-Service
description: Verbinding maken met een Service van Azure Blockchain-netwerk met behulp van Geth
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 0716a9326a54ae31d4f355fe5f4c88488339b390
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026919"
---
# <a name="quickstart-use-geth-to-connect-to-a-transaction-node"></a>Quickstart: Geth gebruiken voor verbinding met een transactie-knooppunt

Geth is een gaat Ethereum-client die u gebruiken kunt om te koppelen aan een Geth-exemplaar op een knooppunt van de transactie Azure Blockchain-Service.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Installeer [Geth](https://github.com/ethereum/go-ethereum/wiki/geth)
* [Een Azure Blockchain-lid maken](create-member.md)

## <a name="get-the-geth-connection-string"></a>De verbindingsreeks Geth ophalen

U kunt de verbindingsreeks Geth vinden in de Azure-portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw Service voor Azure Blockchain-lid. Selecteer **transactie knooppunten** en de standaardkoppeling van de transactie-knooppunt.

    ![Standaard transactie knooppunt selecteren](./media/connect-geth/transaction-nodes.png)

1. Selecteer **verbindingsreeksen**.
1. Kopieer de verbindingsreeks van **HTTPS (toegangssleutel 1)**. In dat geval moet u de opdracht in voor de volgende sectie.

    ![Verbindingsreeks](./media/connect-geth/connection-string.png)

## <a name="connect-to-geth"></a>Verbinding maken met Geth

1. Open een opdrachtprompt of de shell.
1. Gebruik de Geth koppelen subopdracht koppelen aan het actieve exemplaar Geth op uw transactie-knooppunt. Plak de verbindingsreeks als een argument voor de subopdracht toevoegen. Bijvoorbeeld:

    ```
    geth attach <connection string>
    ```

1. Eenmaal verbinding hebben met het transactie-knooppunt Ethereum-console, kunt u de web3 JavaScript Dapp-API of de beheer-API kunt aanroepen.

    Bijvoorbeeld de volgende API gebruiken om de chainId erachter te komen.

    ```bash
    admin.nodeInfo.protocols.istanbul.config.chainId
    ```

    In dit voorbeeld is de chainId 297.

    ![Azure Blockchain-Service-optie](./media/connect-geth/geth-attach.png)

1. Als u wilt verbreken van de console, typt u `exit`.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gebruikt u de client Geth koppelen aan een Geth-exemplaar op een knooppunt van de transactie Azure Blockchain-Service. Probeer de volgende zelfstudie voor het implementeren en verzenden van een transactie met behulp van Truffle.

> [!div class="nextstepaction"]
> [Verzenden van een transactie](send-transaction.md)
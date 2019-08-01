---
title: Slimme contracten gebruiken op de Azure Block Chain-Service
description: Zelf studie over het gebruik van de Azure Block Chain-Service voor het implementeren van een slim contract en het uitvoeren van een functie via een trans actie.
services: azure-blockchain
author: PatAltimore
ms.author: patricka
ms.date: 07/31/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: chrisseg
ms.openlocfilehash: 1843bd66e11a6686c9ae81fb8e30c7b030e889b7
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705130"
---
# <a name="tutorial-use-smart-contracts-on-azure-blockchain-service"></a>Zelfstudie: Slimme contracten gebruiken op de Azure Block Chain-Service

In deze zelf studie gebruikt u de Azure Block Chain Development Kit voor Ethereum voor het maken en implementeren van een slim contract en voert u een slimme contract functie uit via een trans actie op een consortium Block chain-netwerk.

U gebruikt Azure Block Chain Development Kit voor Ethereum voor het volgende:

> [!div class="checklist"]
> * Verbinding maken met het block Chain-lid van het Azure Block Chain Service consortium
> * Een slim contract maken
> * Een slim contract implementeren
> * Een slimme contract functie uitvoeren via een trans actie
> * Query contract status

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Volledige [Snelstartgids: Maak een Block Chain-lid met behulp [van de Azure Portal](create-member.md) of Quick Start: Een Block Chain-lid van de Azure Block Chain-service maken met behulp van Azure CLI](create-member-cli.md)
* [Visual Studio Code](https://code.visualstudio.com/Download)
* [Azure Block Chain Development Kit voor Ethereum-uitbrei ding](https://marketplace.visualstudio.com/items?itemName=AzBlockchain.azure-blockchain)
* [Node.js](https://nodejs.org)
* [Git](https://git-scm.com)
* [Python](https://www.python.org/downloads/release/python-2715/). Voeg python. exe toe aan het pad. Python in het pad is vereist voor Azure Block Chain Development Kit.
* [Truffle](https://www.trufflesuite.com/docs/truffle/getting-started/installation)
* [Ganache CLI](https://github.com/trufflesuite/ganache-cli)

### <a name="verify-azure-blockchain-development-kit-environment"></a>De Azure Block Chain Development Kit-omgeving controleren

In azure Block Chain Development Kit wordt gecontroleerd of aan de vereisten van uw ontwikkel omgeving is voldaan. Controleren of uw ontwikkel omgeving:

Kies **in het palet met de VS code-opdracht Azure Block chain: Welkomst pagina**weer geven.

Azure Block Chain Development Kit voert een validatie script uit dat ongeveer een minuut in beslag neemt. U kunt de uitvoer weer geven door **Terminal > New Terminal**te selecteren. Selecteer in de menu balk van de Terminal het tabblad **uitvoer** en **Azure Block Chain** in de vervolg keuzelijst. Geslaagde validatie ziet eruit als in de volgende afbeelding:

![Geldige ontwikkel omgeving](./media/send-transaction/valid-environment.png)

 Als u een vereist hulp programma mist, wordt een nieuw tabblad met de naam **Azure Block Chain Development Kit** weer gegeven met de vereiste apps die moeten worden geïnstalleerd en koppelingen voor het downloaden van de hulpprogram ma's.

![Ontwikkel Kit vereiste apps](./media/send-transaction/required-apps.png)

## <a name="connect-to-consortium-member"></a>Verbinding maken met het consortium

U kunt verbinding maken met consortium leden met behulp van de Azure Block Chain Development Kit VS code extension. Wanneer u bent verbonden met een consortium, kunt u slimme contracten compileren, bouwen en implementeren in een Azure Block Chain Service consortium-lid.

Als u geen toegang hebt tot een lid van een Azure Block Chain Service consortium, voltooit [u de vereiste Snelstartgids: Maak een Block Chain-lid met behulp [van de Azure Portal](create-member.md) of Quick Start: Maak een Azure Block Chain Service Block Chain-lid met behulp van Azure CLI](create-member-cli.md).

1. Vouw in het deel venster Visual Studio code (VS code) Explorer de **Azure Block Chain** -extensie uit.
1. Selecteer **verbinding maken met consortium**.

   ![Verbinding maken met consortium](./media/send-transaction/connect-consortium.png)

    Als u wordt gevraagd om Azure-verificatie, volgt u de prompts voor verificatie met behulp van een browser.
1. Kies **verbinding maken met Azure Block Chain Service consortium** in de vervolg keuzelijst voor het opdracht palet.
1. Kies het abonnement en de resource groep die aan uw Azure Block Chain Service consortium-lid zijn gekoppeld.
1. Kies uw consortium in de lijst.

De consortium-en block Chain-leden worden weer gegeven in de Visual Studio Explorer-balk.

![Consortium weer gegeven in Verkenner](./media/send-transaction/consortium-node.png)

## <a name="create-a-smart-contract"></a>Een slim contract maken

De Azure Block Chain Development Kit voor Ethereum gebruikt Project sjablonen en Truffle-hulpprogram ma's om contracten te helpen, te bouwen en te implementeren.

1. Kies **in het palet met de VS code-opdracht Azure Block chain: Nieuw volheid-project**.
1. Kies **basis project maken**.
1. Maak een nieuwe map met `HelloBlockchain` de naam en **Selecteer Nieuw pad**voor het project.

De Azure Block Chain Development Kit maakt en initialiseert een nieuw solide project voor u. Het basis project bevat een voor beeld van een **HelloBlockchain** slim contract en alle benodigde bestanden voor het bouwen en implementeren van uw consortium in de Azure Block Chain-service. Het kan enkele minuten duren voordat het project is gemaakt. U kunt de voortgang in het Terminal paneel van VS code controleren door de uitvoer voor Azure Block chain te selecteren.

De project structuur ziet eruit als in het volgende voor beeld:

   ![Volheid project](./media/send-transaction/solidity-project.png)

## <a name="build-a-smart-contract"></a>Een slim contract bouwen

Slimme contracten bevinden zich in de map **contracten** van het project. U kunt slimme contracten compileren voordat u ze implementeert in een Block chain. Gebruik de opdracht **contracten bouwen** om alle slimme contracten in uw project te compileren.

1. Vouw in de VS code Explorer-zijbalk de map **contracten** in uw project uit.
1. Klik met de rechter muisknop op **HelloBlockchain. Sol** en kies **contracten maken** in het menu.

    ![Contracten samen stellen](./media/send-transaction/build-contracts.png)

Azure Block Chain Development Kit maakt gebruik van truffle om de slimme contracten te compileren.

![Uitvoer compileren](./media/send-transaction/compile-output.png)

## <a name="deploy-a-smart-contract"></a>Een slim contract implementeren

Truffle maakt gebruik van migratie scripts om uw contracten te implementeren in een Ethereum-netwerk. Migraties zijn Java script-bestanden die zich in de map migraties van het project bevinden.

1. Als u uw slimme contract wilt implementeren, klikt u met de rechter muisknop op **HelloBlockchain. Sol** en kiest u **contracten implementeren** in het menu.
1. Kies uw Azure Block Chain consortium-netwerk onder **van truffle-config. js**. Het consortium Block chain-netwerk is toegevoegd aan het Truffle-configuratie bestand van het project wanneer u het project hebt gemaakt.
1. Kies **instructie genereren**. Kies een bestands naam en sla het bestand voor de instructie op in de projectmap. Bijvoorbeeld `myblockchainmember.env`. Het instructie bestand wordt gebruikt voor het genereren van een persoonlijke sleutel Ethereum voor uw Block Chain-lid.

Azure Block Chain Development Kit maakt gebruik van truffle om het migratie script uit te voeren om de contracten te implementeren op de Block chain.

![Het contract is geïmplementeerd](./media/send-transaction/deploy-contract.png)

## <a name="call-a-contract-function"></a>Een contract functie aanroepen

De functie **SendRequest** van het **HelloBlockchain** -contract wijzigt de variabele **RequestMessage** -status. Het wijzigen van de status van een Block chain-netwerk wordt uitgevoerd via een trans actie. U kunt een script maken om de functie **SendRequest** uit te voeren via een trans actie.

1. Maak een nieuw bestand in de hoofdmap van uw Truffle-project en noem `sendrequest.js`het. Voeg de volgende Web3 java script-code toe aan het bestand.

    ```javascript
    var HelloBlockchain = artifacts.require("HelloBlockchain");
        
    module.exports = function(done) {
      console.log("Getting the deployed version of the HelloBlockchain smart contract")
      HelloBlockchain.deployed().then(function(instance) {
        console.log("Calling SendRequest function for contract ", instance.address);
        return instance.SendRequest("Hello, blockchain!");
      }).then(function(result) {
        console.log("Transaction hash: ", result.tx);
        console.log("Request complete");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

1. Wanneer Azure Block Chain Development Kit een project maakt, wordt het Truffle-configuratie bestand gegenereerd met de details van uw consortium Block chain-netwerk eindpunt. Open **Truffle-config. js** in uw project. In het configuratie bestand worden twee netwerken weer gegeven: een met de naam ontwikkeling en een met dezelfde namen als het consortium.
1. Gebruik in het Terminal venster van VS code Truffle om het script uit te voeren op uw consortium Block chain-netwerk. Selecteer in de menu balk van het Terminal venster het tabblad **Terminal** en **Power shell** in de vervolg keuzelijst.

    ```PowerShell
    truffle exec sendrequest.js --network <blockchain network>
    ```

    Vervang \<Block Chain-\> netwerk door de naam van het block chain-netwerk dat is gedefinieerd in **Truffle-config. js**.

Truffle voert het script uit op uw Block chain-netwerk.

![Scriptuitvoer](./media/send-transaction/execute-transaction.png)

Wanneer u de functie van een contract uitvoert via een trans actie, wordt de trans actie niet verwerkt totdat er een blok is gemaakt. Functies die via een trans actie moeten worden uitgevoerd, retour neren een trans actie-ID in plaats van een retour waarde.

## <a name="query-contract-state"></a>Query contract status

Met de functies van een slimme opdracht kan de huidige waarde van de status variabelen worden geretourneerd. We gaan een functie toevoegen om de waarde van een status variabele te retour neren.

1. Voeg in **HelloBlockchain. Sol**een **GetMessage** -functie toe aan het slimme **HelloBlockchain** -contract.

    ``` solidity
    function getMessage() public view returns (string memory)
    {
        if (State == StateType.Request)
            return RequestMessage;
        else
            return ResponseMessage;
    }
    ```

    De functie retourneert het bericht dat is opgeslagen in een status variabele op basis van de huidige status van het contract.

1. Klik met de rechter muisknop op **HelloBlockchain. Sol** en kies **contracten maken** in het menu om de wijzigingen in het slimme contract te compileren.
1. Als u wilt implementeren, klikt u met de rechter muisknop op **HelloBlockchain. Sol** en kiest u **contracten implementeren** in het menu.
1. Maak vervolgens een script dat wordt gebruikt om de functie **GetMessage** aan te roepen. Maak een nieuw bestand in de hoofdmap van uw Truffle-project en noem `getmessage.js`het. Voeg de volgende Web3 java script-code toe aan het bestand.

    ```javascript
    var HelloBlockchain = artifacts.require("HelloBlockchain");
    
    module.exports = function(done) {
      console.log("Getting the deployed version of the HelloBlockchain smart contract")
      HelloBlockchain.deployed().then(function(instance) {
        console.log("Calling getMessage function for contract ", instance.address);
        return instance.getMessage();
      }).then(function(result) {
        console.log("Request message value: ", result);
        console.log("Request complete");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

1. Gebruik in het Terminal venster van VS code Truffle om het script uit te voeren op uw Block chain-netwerk. Selecteer in de menu balk van het Terminal venster het tabblad **Terminal** en **Power shell** in de vervolg keuzelijst.

    ```bash
    truffle exec getmessage.js --network <blockchain network>
    ```

    Vervang \<Block Chain-\> netwerk door de naam van het block chain-netwerk dat is gedefinieerd in **Truffle-config. js**.

Het script voert een query uit op het slimme contract door de getMessage-functie aan te roepen. De huidige waarde van de status variabele **RequestMessage** wordt geretourneerd.

![Scriptuitvoer](./media/send-transaction/execute-get.png)

Let op: de waarde is niet **Hello, Block Chain!** . In plaats daarvan is de geretourneerde waarde een tijdelijke aanduiding. Wanneer u het contract wijzigt en implementeert, haalt het contract een nieuw contract adres op en worden aan de status variabelen waarden toegewezen in de Smart contract-constructor. Het Truffle-voor beeld- **2_deploy_contracts. js** -migratie script implementeert het slimme contract en geeft een tijdelijke aanduiding door als argument. De constructor stelt de **RequestMessage** -status variabele in op de waarde van de tijdelijke aanduiding en dat is wat er wordt geretourneerd.

1. Als u de **RequestMessage** -status variabele wilt instellen en de waarde wilt opvragen, voert u de scripts **SendRequest. js** en **GetMessage. js** opnieuw uit.

    ![Scriptuitvoer](./media/send-transaction/execute-set-get.png)

    **SendRequest. js** stelt de **RequestMessage** -status variabele in op **Hello, Block Chain!** en **GetMessage. js** vraagt het contract om de waarde van de **RequestMessage** -status variabele en retourneert **Hello, Block Chain!** .

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u deze niet meer nodig hebt, kunt u de resources verwijderen `myResourceGroup` door de resource groep te verwijderen die u hebt gemaakt in de Snelstartgids *een Block Chain-lid maken* .

De resource groep verwijderen:

1. In de Azure-portal, gaat u naar **resourcegroep** in het navigatiedeelvenster links en selecteert u de resourcegroep die u wilt verwijderen.
1. Selecteer **Resourcegroep verwijderen**. Bevestig de verwijdering door de naam van de resourcegroep in te voeren en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u een voor beeld van een volheid project gemaakt met behulp van de Azure Block Chain Development Kit. U hebt een slim contract gemaakt en geïmplementeerd. dit wordt een functie genoemd via een trans actie op een Block Chain consortium-netwerk dat wordt gehost op de Azure Block Chain-service.

> [!div class="nextstepaction"]
> [Block Chain-toepassingen ontwikkelen met behulp van de Azure Block Chain-Service](develop.md)

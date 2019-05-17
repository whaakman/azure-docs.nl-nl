---
title: Transacties met Azure Blockchain-Service verzenden
description: Zelfstudie over het gebruik van Azure Blockchain-Service op een slimme contract implementeren en een persoonlijke transactie verzenden.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 0b5e39e9cf2fc3ffe91db6587bc1ed1bab079e93
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65777331"
---
# <a name="tutorial-send-transactions-using-azure-blockchain-service"></a>Zelfstudie: Transacties met Azure Blockchain-Service verzenden

In deze zelfstudie hebt u voor het maken van de knooppunten van de transactie als u wilt testen contract en de transactie privacy.  U gebruikt Truffle te maken van een lokale ontwikkelingsomgeving en implementeren van een slimme contract en verzenden van een persoonlijke transactie.

U leert het volgende:

> [!div class="checklist"]
> * Transactie-knooppunten toevoegen
> * Truffle gebruiken voor het implementeren van een slimme contract
> * Verzenden van een transactie
> * Transactie-privacy valideren

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Volledige [een blockchain-lid met de Azure portal maken](create-member.md)
* Volledige [Quick Start: Truffle gebruiken voor verbinding met een netwerk consortium](connect-truffle.md)
* Truffle vereist verschillende hulpprogramma's om te worden geïnstalleerd met [Node.js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), en [Truffle](https://github.com/trufflesuite/truffle).

    Om in te stellen snel op Windows 10, installeer [Ubuntu in Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6) Installeer voor een Unix-Bash-shell terminal [Truffle](https://github.com/trufflesuite/truffle). Het Ubuntu op Windows-distributiepunt bevat Node.js en Git.

* Installeer [Visual Studio Code](https://code.visualstudio.com/Download)
* Installeer [Solidity van Visual Studio Code-extensie](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity)

## <a name="create-transaction-nodes"></a>Transactie-knooppunten maken

Standaard hebt u één knooppunt van de transactie. We gaan twee meer toevoegen. Een van de knooppunten maakt deel uit van de persoonlijke transactie. De andere is niet opgenomen in de persoonlijke transactie.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw Azure Blockchain-lid en selecteer **transactie knooppunten > toevoegen**.
1. Voltooien van de instellingen voor een nieuwe transactie-knooppunt met de naam `alpha`.

    ![Transactie-knooppunt maken](./media/send-transaction/create-node.png)

    | Instelling | Waarde | Beschrijving |
    |---------|-------|-------------|
    | Name | `alpha` | Naam van de transactie-knooppunt. De naam wordt gebruikt om te maken van de DNS-adres van het eindpunt van de transactie-knooppunt. Bijvoorbeeld `alpha-mymanagedledger.blockchain.azure.com`. |
    | Wachtwoord | sterk wachtwoord | Het wachtwoord wordt gebruikt voor toegang tot het eindpunt van de transactie-knooppunt met basisverificatie.

1. Selecteer **Maken**.

    Het duurt ongeveer 10 minuten een nieuwe transactie-knooppunt wordt ingericht.

1. Herhaal stappen 2 t/m 4 om toe te voegen een transactie-knooppunt met de naam `beta`.

U kunt doorgaan met de zelfstudie terwijl de knooppunten worden ingericht. Wanneer de inrichting is voltooid, hebt u drie knooppunten van de transactie.

## <a name="open-truffle-project"></a>Truffle project openen

1. Open een Bash-shell-terminal.
1. Het pad naar de projectmap Truffle wijzigen van de vereiste [Quick Start: Truffle gebruiken voor verbinding met een netwerk consortium](connect-truffle.md). Bijvoorbeeld:

    ```bash
    cd truffledemo
    ```

1. Start de Truffle interactieve ontwikkeling-console.

    ``` bash
    truffle develop
    ```

    Truffle maakt een blockchain lokale ontwikkeling en biedt een interactieve console.

## <a name="connect-to-transaction-node"></a>Verbinding maken met transactieknooppunt

Gebruik Web3 verbinding maken met het standaard-transactie-knooppunt en een account maken. U kunt de Web3-verbindingsreeks ophalen uit de Azure-portal.

1. In de Azure-portal, gaat u naar de standaard-transactie-knooppunt en selecteer **transactie knooppunten > voorbeeldcode > Web3**.
1. Kopieer het JavaScript van **HTTPS (toegangssleutel 1)** ![Web3 voorbeeldcode](./media/send-transaction/web3-code.png)

1. Plak de Web3 JavaScript-code voor het standaard-transactie-knooppunt in de console van de interactieve ontwikkeling Truffle. De code maakt een Web3-object dat is verbonden met uw Azure Blockchain-transactie-knooppunt.

    ```bash
    truffle(develop)> var Web3 = require("Web3");
    truffle(develop)> var provider = new Web3.providers.HttpProvider("https://myblockchainmember.blockchain.azure.com:3200/hy5FMu5TaPR0Zg8GxiPwned");
    truffle(develop)> var web3 = new Web3(provider);
    ```

    U kunt methoden aanroepen op het object Web3 om te communiceren met uw transactie-knooppunt.

1. Maak een nieuw account op het knooppunt van de transactie standaard. De parameter password vervangen door uw eigen sterk wachtwoord.

    ```bash
    web3.eth.personal.newAccount("1@myStrongPassword");
    ```

    Maak notitie van de accountadres dat is geretourneerd en wachtwoord die u hebt gebruikt voor de volgende sectie.

1. Sluit de Truffle-ontwikkelomgeving.

    ```bash
    .exit
    ```

## <a name="configure-truffle-project"></a>Configureer Truffle project

Voor het configureren van het project Truffle, moet u enkele transactie-informatie voor knooppunt vanuit Azure portal.

### <a name="transaction-node-public-key"></a>Openbare sleutel voor transactie-knooppunt

Elk knooppunt van de transactie is een openbare sleutel. De openbare sleutel kunt u een persoonlijke transactie verzenden naar het knooppunt. Als u wilt verzenden van een transactie in het knooppunt van de transactie standaard naar de *alpha* transactie-knooppunt, moet u de *alpha* openbare sleutel van het knooppunt transactie.

U kunt de openbare sleutel ophalen uit de lijst van de transactie-knooppunt. Kopieer de openbare sleutel voor het alpha-knooppunt en de waarde voor opslaan verderop in de zelfstudie.

![Lijst van knooppunt](./media/send-transaction/node-list.png)

### <a name="transaction-node-endpoint-addresses"></a>Eindpuntadressen voor transactie-knooppunt

1. In de Azure-portal, gaat u naar elk knooppunt transactie en selecteer **transactie knooppunten > verbindingsreeksen**.
1. Kopiëren en opslaan van de eindpunt-URL van **HTTPS (toegangssleutel 1)** voor elk knooppunt van de transactie. U moet de eindpuntadressen voor het configuratiebestand slimme contract later in de zelfstudie.

    ![Transactie-adres van eindpunt](./media/send-transaction/endpoint.png)

### <a name="edit-configuration-file"></a>Het configuratiebestand bewerken

1. Start Visual Studio Code en opent u het Truffle project directory map met de **bestand > map openen** menu.
1. Open het configuratiebestand Truffle `truffle-config.js`.
1. Vervang de inhoud van het bestand met de volgende configuratie-informatie. Variabelen met de adressen van eindpunten en accountgegevens toevoegen. De secties punthaak vervangen door waarden die u hebt verzameld in de vorige secties.

``` javascript
var defaultnode = "<default transaction node connection string>";
var alpha = "<alpha transaction node connection string>";
var beta = "<beta transaction node connection string>";

var myAccount = "<account address>";
var myPassword = "<account password>";

var Web3 = require("web3");
```

Voeg de configuratiecode aan de **module.exports** sectie van de configuratie.

```javascript
module.exports = {
  networks: {
    defaultnode: {
      provider:(() =>  {
      const AzureBlockchainProvider = new Web3.providers.HttpProvider(defaultnode);

      const web3 = new Web3(AzureBlockchainProvider);
      web3.eth.personal.unlockAccount(myAccount, myPassword);

      return AzureBlockchainProvider;
      })(),

      network_id: "*",
      gas: 0,
      gasPrice: 0,
      from: myAccount
    },
    alpha: {
      provider: new Web3.providers.HttpProvider(alpha),
      network_id: "*",
      gas: 0,
      gasPrice: 0
    },
    beta: {
      provider: new Web3.providers.HttpProvider(beta),
      network_id: "*",
      gas: 0,
      gasPrice: 0
    }
  }
}
```

## <a name="create-smart-contract"></a>Slimme contract maken

In de map **contracten**, maak een nieuw bestand met de naam `SimpleStorage.sol`. Voeg de volgende code toe.

```solidity
pragma solidity >=0.4.21 <0.6.0;

contract SimpleStorage {
    string public storedData;

    constructor(string memory initVal) public {
        storedData = initVal;
    }

    function set(string memory x) public {
        storedData = x;
    }

    function get() view public returns (string memory retVal) {
        return storedData;
    }
}
```

In de map **migraties**, maak een nieuw bestand met de naam `2_deploy_simplestorage.js`. Voeg de volgende code toe.

```solidity
var SimpleStorage = artifacts.require("SimpleStorage.sol");

module.exports = function(deployer) {

  // Pass 42 to the contract as the first constructor parameter
  deployer.deploy(SimpleStorage, "42", {privateFor: ["<alpha node public key>"], from:"<Account address>"})  
};
```

Vervang de waarden in de punthaken.

| Value | Description
|-------|-------------
| \<de openbare sleutel Alpha-knooppunt\> | Openbare sleutel van het knooppunt voor alpha
| \<Accountadres\> | Het adres van de account in het knooppunt van de transactie standaard gemaakt.

In dit voorbeeld wordt de aanvankelijke waarde van de **storeData** waarde is ingesteld op 42.

**privateFor** definieert de knooppunten waarop het contract beschikbaar is. In dit voorbeeld van het standaard transactie knooppunt account kunt cast-conversie persoonlijke transacties naar de **alpha** knooppunt. U moet openbare sleutels toevoegen voor alle persoonlijke Transactiedeelnemers. Als u geen **privateFor:** en **uit:**, de slimme contract transacties openbaar zijn en kan worden bekeken door alle leden van de consortium.

Sla alle bestanden door het selecteren van **bestand > opslaan alle**.

## <a name="deploy-smart-contract"></a>Slimme contract implementeren

Truffle gebruiken om te implementeren `SimpleStorage.sol` met standaard transactie knooppunt netwerk.

```bash
truffle migrate --network defaultnode
```

Truffle eerst worden gecompileerd en implementeert vervolgens de **SimpleStorage** slimme contract.

Voorbeelduitvoer:

```
pat@DESKTOP:/mnt/c/truffledemo$ truffle migrate --network defaultnode

2_deploy_simplestorage.js
=========================

   Deploying 'SimpleStorage'
   -------------------------
   > transaction hash:    0x3f695ff225e7d11a0239ffcaaab0d5f72adb545912693a77fbfc11c0dbe7ba72
   > Blocks: 2            Seconds: 12
   > contract address:    0x0b15c15C739c1F3C1e041ef70E0011e641C9D763
   > account:             0x1a0B9683B449A8FcAd294A01E881c90c734735C3
   > balance:             0
   > gas used:            0
   > gas price:           0 gwei
   > value sent:          0 ETH
   > total cost:          0 ETH


   > Saving migration to chain.
   > Saving artifacts
   -------------------------------------
   > Total cost:                   0 ETH


Summary
=======
> Total deployments:   2
> Final cost:          0 ETH
```

## <a name="validate-contract-privacy"></a>Contract privacy valideren

Vanwege privacy contract, kunnen alleen waarden van de overeenkomst van knooppunten die zijn gedeclareerd in worden opgevraagd **privateFor**. In dit voorbeeld kunnen we in het knooppunt van de transactie standaard query omdat het account in dat knooppunt bestaat. Met behulp van de console Truffle verbinding maken met het standaard-transactie-knooppunt.

```bash
truffle console --network defaultnode
```

Een opdracht die de waarde van het contract exemplaar retourneert uitvoeren.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

Als het uitvoeren van query's het standaard-transactie-knooppunt is geslaagd, wordt de waarde 42 geretourneerd.

Voorbeelduitvoer:

```
pat@DESKTOP-J41EP5S:/mnt/c/truffledemo$ truffle console --network defaultnode
truffle(defaultnode)> SimpleStorage.deployed().then(function(instance){return instance.get();})
'42'
```

Sluit de console.

```bash
.exit
```

Omdat we gedeclareerd **alpha** openbare sleutel van het knooppunt in **privateFor**, we kunnen opvragen de **alpha** knooppunt. Met behulp van de console Truffle verbinding maken met de **alpha** knooppunt.

```bash
truffle console --network alpha
```

Een opdracht die de waarde van het contract exemplaar retourneert uitvoeren.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

Als het uitvoeren van query's de **alpha** knooppunt is geslaagd, wordt de waarde 42 geretourneerd.

Voorbeelduitvoer:

```
pat@DESKTOP-J41EP5S:/mnt/c/truffledemo$ truffle console --network alpha
truffle(alpha)> SimpleStorage.deployed().then(function(instance){return instance.get();})
'42'
```

Sluit de console.

```bash
.exit
```

Omdat we niet heeft declareren **Bèta** openbare sleutel van het knooppunt in **privateFor**, we het niet mogelijk om op te vragen de **Bèta** knooppunt vanwege contract privacy. Met behulp van de console Truffle verbinding maken met de **Bèta** knooppunt.

```bash
truffle console --network beta
```

Een opdracht die de waarde van het contract exemplaar retourneert uitvoeren.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

Uitvoeren van query's de **Bèta** knooppunt is mislukt omdat het contract privé is.

Voorbeelduitvoer:

```
pat@DESKTOP-J41EP5S:/mnt/c/truffledemo$ truffle console --network beta
truffle(beta)> SimpleStorage.deployed().then(function(instance){return instance.get();})
Thrown:
Error: Returned values aren't valid, did it run Out of Gas?
    at XMLHttpRequest._onHttpResponseEnd (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:345:8)
    at XMLHttpRequest._setReadyState (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:219:8)
    at XMLHttpRequestEventTarget.dispatchEvent (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request-event-target.ts:44:13)
    at XMLHttpRequest.request.onreadystatechange (/mnt/c/truffledemo/node_modules/web3-providers-http/src/index.js:96:13)
```

Sluit de console.

```bash
.exit
```

## <a name="send-a-transaction"></a>Verzenden van een transactie

Maak een bestand met de naam `sampletx.js`. Opslaan in de hoofdmap van uw project.

Met dit script wordt het contract **storedData** variabelewaarde op 65. Voeg de code toe aan het nieuwe bestand.

```javascript
var SimpleStorage = artifacts.require("SimpleStorage");

module.exports = function(done) {
  console.log("Getting deployed version of SimpleStorage...")
  SimpleStorage.deployed().then(function(instance) {
    console.log("Setting value to 65...");
    return instance.set("65", {privateFor: ["<alpha node public key>"], from:"<Account address>"});
  }).then(function(result) {
    console.log("Transaction:", result.tx);
    console.log("Finished!");
    done();
  }).catch(function(e) {
    console.log(e);
    done();
  });
};
```

Vervang de waarden in de punthaken en sla het bestand.

| Value | Description
|-------|-------------
| \<de openbare sleutel Alpha-knooppunt\> | Openbare sleutel van het knooppunt voor alpha
| \<Accountadres\> | Het adres van de account in het knooppunt van de transactie standaard gemaakt.

**privateFor** definieert de knooppunten waarop de transactie beschikbaar is. In dit voorbeeld van het standaard transactie knooppunt account kunt cast-conversie persoonlijke transacties naar de **alpha** knooppunt. U moet openbare sleutels toevoegen voor alle persoonlijke Transactiedeelnemers.

Truffle gebruiken voor het uitvoeren van het script voor het standaard-transactie-knooppunt.

```bash
truffle exec sampletx.js --network defaultnode
```

Een opdracht die de waarde van het contract exemplaar retourneert uitvoeren.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

Als de transactie voltooid is, wordt de waarde 65 geretourneerd.

Voorbeelduitvoer:

```
Getting deployed version of SimpleStorage...
Setting value to 65...
Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
Finished!
```

Sluit de console.

```bash
.exit
```

## <a name="validate-transaction-privacy"></a>Transactie-privacy valideren

Vanwege de transactie-privacy kunnen transacties alleen worden uitgevoerd op knooppunten die zijn gedeclareerd in **privateFor**. In dit voorbeeld kunnen we transacties uitgevoerd, aangezien we gedeclareerd **alpha** openbare sleutel van het knooppunt in **privateFor**. Truffle gebruiken voor het uitvoeren van de transactie op de **alpha** knooppunt.

```bash
truffle exec sampletx.js --network alpha
```

Een opdracht die de waarde van het contract exemplaar retourneert uitvoeren.

```bash
SimpleStorage.deployed().then(function(instance){return instance.get();})
```

Als de transactie voltooid is, wordt de waarde 65 geretourneerd.

Voorbeelduitvoer:

```
Getting deployed version of SimpleStorage...
Setting value to 65...
Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
Finished!
```

Sluit de console.

```bash
.exit
```

In deze zelfstudie maakt u twee transactie knooppunten om te demonstreren contract en de transactie privacy toegevoegd. U gebruikt het standaardknooppunt voor het implementeren van een persoonlijke slimme contract. U privacy door een query uitvoeren op contractwaarden en presterende transacties op de blockchain getest.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, kunt u de resources verwijderen door het verwijderen van de `myResourceGroup` resourcegroep die u hebt gemaakt door de Azure Blockchain-Service.

De resourcegroep verwijderen:

1. In de Azure-portal, gaat u naar **resourcegroep** in het navigatiedeelvenster links en selecteert u de resourcegroep die u wilt verwijderen.
1. Selecteer **Resourcegroep verwijderen**. Bevestig de verwijdering door de naam van de resourcegroep in te voeren en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Blockchain-toepassingen met Azure Blockchain Service ontwikkelen](develop.md)

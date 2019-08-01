---
title: Trans acties verzenden met behulp van de Azure Block Chain-Service
description: Zelf studie over het gebruik van de Azure Block Chain-Service voor het implementeren van een slim contract en het verzenden van een persoonlijke trans actie.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/29/2019
ms.topic: tutorial
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 3cfbbdc5b95d1607738b132980320d2ff7c99788
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698384"
---
# <a name="tutorial-send-transactions-using-azure-blockchain-service"></a>Zelfstudie: Trans acties verzenden met behulp van de Azure Block Chain-Service

In deze zelf studie maakt u transactie knooppunten om de privacy van een contract en trans acties te testen.  U gebruikt Truffle om een lokale ontwikkel omgeving te maken en een slim contract te implementeren en een persoonlijke trans actie te verzenden.

U leert het volgende:

> [!div class="checklist"]
> * Transactie knooppunten toevoegen
> * Truffle gebruiken voor het implementeren van een slim contract
> * Een trans actie verzenden
> * Privacy van trans acties valideren

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Volt ooien [een Block Chain-lid maken met behulp van de Azure Portal](create-member.md)
* Volledige [Snelstartgids: Truffle gebruiken om verbinding te maken met een consortium netwerk](connect-truffle.md)
* Installeer [Truffle](https://github.com/trufflesuite/truffle). Truffle vereist verschillende hulpprogram ma's die moeten worden geïnstalleerd, waaronder [node. js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
* [Python 2.7.15](https://www.python.org/downloads/release/python-2715/)installeren. Python is vereist voor web3.
* [Visual Studio code](https://code.visualstudio.com/Download) installeren
* [Visual Studio code-extensie voor volheid](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity) installeren

## <a name="create-transaction-nodes"></a>Transactie knooppunten maken

Standaard hebt u één transactie knooppunt. We gaan nog twee meer toevoegen. Een van de knoop punten neemt deel uit van de persoonlijke trans actie. De andere is niet opgenomen in de persoonlijke trans actie.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Navigeer naar uw Azure Block Chain-lid en selecteer **transactie knooppunten > toevoegen**.
1. Voltooi de instellingen voor een nieuw transactie knooppunt met `alpha`de naam.

    ![Transactie knooppunt maken](./media/send-transaction/create-node.png)

    | Instelling | Waarde | Beschrijving |
    |---------|-------|-------------|
    | Name | `alpha` | Naam van het transactie knooppunt. De naam wordt gebruikt voor het maken van het DNS-adres voor het eind punt van het transactie knooppunt. Bijvoorbeeld `alpha-mymanagedledger.blockchain.azure.com`. |
    | Wachtwoord | Sterk wacht woord | Het wacht woord wordt gebruikt voor toegang tot het knoop punt van het transactie knooppunt met basis verificatie.

1. Selecteer **Maken**.

    Het inrichten van een nieuw transactie knooppunt duurt ongeveer 10 minuten.

1. Herhaal stap 2 tot en met 4 om een transactie knooppunt `beta`met de naam toe te voegen.

U kunt door gaan met de zelf studie wanneer de knoop punten worden ingericht. Wanneer het inrichten is voltooid, hebt u drie transactie knooppunten.

## <a name="open-truffle-console"></a>Truffle-console openen

1. Open een node. js-opdracht prompt of-shell.
1. Wijzig het pad naar de Truffle-project directory van de vereiste [Snelstartgids: Gebruik Truffle om verbinding te maken met een](connect-truffle.md)consortium netwerk. Bijvoorbeeld:

    ```bash
    cd truffledemo
    ```

1. Gebruik de Truffle-console om verbinding te maken met het standaard transactie knooppunt.

    ``` bash
    truffle console --network defaultnode
    ```

    Truffle maakt verbinding met het standaard transactie knooppunt en biedt een interactieve console.

## <a name="create-ethereum-account"></a>Ethereum-account maken

Gebruik Web3 om verbinding te maken met het standaard transactie knooppunt en maak een Ethereum-account. U kunt methoden voor het object Web3 aanroepen om te communiceren met uw trans actie-knoop punt.

1. Maak een nieuw account op het standaard transactie knooppunt. Vervang de wachtwoord parameter door uw eigen sterke wacht woord.

    ```bash
    web3.eth.personal.newAccount("1@myStrongPassword");
    ```

    Noteer het account adres dat wordt geretourneerd en het wacht woord. U hebt het adres en het wacht woord van het Ethereum-account nodig in de volgende sectie.

1. Sluit de Truffle-ontwikkel omgeving af.

    ```bash
    .exit
    ```

## <a name="configure-truffle-project"></a>Truffle-project configureren

Als u het Truffle-project wilt configureren, hebt u enkele informatie over het transactie knooppunt nodig van de Azure Portal.

### <a name="transaction-node-public-key"></a>Open bare sleutel van transactie knooppunt

Elk transactie knooppunt heeft een open bare sleutel. Met de open bare sleutel kunt u een persoonlijke trans actie verzenden naar het knoop punt. Als u een trans actie van het knoop punt standaard transactie naar het *Alfa* transactie knooppunt wilt verzenden, moet u de open bare sleutel van het *Alfa* trans actie-knoop punt hebben.

U kunt de open bare sleutel ophalen uit de lijst met transactie knooppunten. Kopieer de open bare sleutel voor het Alfa knooppunt en sla de waarde op verderop in de zelf studie.

![Lijst met transactie knooppunten](./media/send-transaction/node-list.png)

### <a name="transaction-node-endpoint-addresses"></a>Eindpunt adressen van het transactie knooppunt

1. Ga in het Azure Portal naar elk transactie knooppunt en selecteer **transactie knooppunten > verbindings reeksen**.
1. Kopieer de eind punt-URL van **https (toegangs sleutel 1)** voor elk transactie knooppunt en sla deze op. U hebt de eindpunt adressen voor het configuratie bestand van het slimme contract later in de zelf studie nodig.

    ![Adres van het transactie eindpunt](./media/send-transaction/endpoint.png)

### <a name="edit-configuration-file"></a>Configuratie bestand bewerken

1. Start Visual Studio code en open de map Truffle project directory in het menu **File > map openen** .
1. Open het configuratie bestand `truffle-config.js`Truffle.
1. Vervang de inhoud van het bestand door de volgende configuratie gegevens. Voeg variabelen met de eind punten en account gegevens toe. Vervang de secties met punt haken door de waarden die u hebt verzameld uit de vorige secties.

    ``` javascript
    var defaultnode = "<default transaction node connection string>";
    var alpha = "<alpha transaction node connection string>";
    var beta = "<beta transaction node connection string>";
    
    var myAccount = "<Ethereum account address>";
    var myPassword = "<Ethereum account password>";
    
    var Web3 = require("web3");
    
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
          gasPrice: 0,
          from: myAccount
        },
        alpha: {
          provider: new Web3.providers.HttpProvider(alpha),
          network_id: "*",
        },
        beta: {
          provider: new Web3.providers.HttpProvider(beta),
          network_id: "*",
        }
      },
      compilers: {
        solc: {
          evmVersion: "byzantium"
        }
      }
    }
    ```

1. Sla de wijzigingen op `truffle-config.js`op.

## <a name="create-smart-contract"></a>Slim contract maken

1. Maak in de map **contracten** een nieuw bestand met de `SimpleStorage.sol`naam. Voeg de volgende code toe.

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
    
1. Maak in de map migraties een nieuw bestand met de `2_deploy_simplestorage.js`naam. Voeg de volgende code toe.

    ```solidity
    var SimpleStorage = artifacts.require("SimpleStorage.sol");
    
    module.exports = function(deployer) {
    
      // Pass 42 to the contract as the first constructor parameter
      deployer.deploy(SimpleStorage, "42", {privateFor: ["<alpha node public key>"], from:"<Ethereum account address>"})  
    };
    ```

1. Vervang de waarden in de punt haken.

    | Waarde | Description
    |-------|-------------
    | \<alpha node public key\> | Open bare sleutel van het Alfa knooppunt
    | \<Ethereum account address\> | Het Ethereum-account adres dat is gemaakt in het standaard transactie knooppunt

    In dit voor beeld wordt de begin waarde van de waarde **Store data** ingesteld op 42.

    **privateFor** definieert de knoop punten waarvoor het contract beschikbaar is. In dit voor beeld kan het account van het standaard transactie knooppunt persoonlijke trans acties naar het **Alfa** knooppunt casten. U kunt open bare sleutels voor alle deel nemers aan de persoonlijke trans actie toevoegen. Als u PrivateFor niet opneemt **:** en **van:** , zijn de slimme contract transacties openbaar en kunnen alle leden van het consortium worden weer gegeven.

1. Sla alle bestanden op door **bestand te selecteren > alles**op te slaan.

## <a name="deploy-smart-contract"></a>Slim contract implementeren

Gebruik Truffle om te `SimpleStorage.sol` implementeren op het standaard transactie knooppunt netwerk.

```bash
truffle migrate --network defaultnode
```

Truffle maakt eerst een compilatie en implementeert vervolgens het slimme **SimpleStorage** -contract.

Voorbeelduitvoer:

```
admin@desktop:/mnt/c/truffledemo$ truffle migrate --network defaultnode

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

## <a name="validate-contract-privacy"></a>Privacy voor contracten valideren

Vanwege de privacy van het contract kunnen contract waarden alleen worden opgevraagd van de knoop punten die zijn gedeclareerd in **privateFor**. In dit voor beeld kunnen we een query uitvoeren op het standaard transactie knooppunt, omdat het account zich in dat knoop punt bevindt. 

1. Maak verbinding met het standaard transactie knooppunt met behulp van de Truffle-console.

    ```bash
    truffle console --network defaultnode
    ```

1. Voer in de Truffle-console code uit die de waarde van het contract exemplaar retourneert.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

    Als er een query wordt uitgevoerd op het standaard transactie knooppunt, wordt de waarde 42 geretourneerd. Bijvoorbeeld:

    ```
    admin@desktop:/mnt/c/truffledemo$ truffle console --network defaultnode
    truffle(defaultnode)> SimpleStorage.deployed().then(function(instance){return instance.get();})
    '42'
    ```

1. Sluit de Truffle-console af.

    ```bash
    .exit
    ```

Omdat we de open bare sleutel van het **Alfa** -knoop punt in **privateFor**zijn gedeclareerd, kunnen we een query uitvoeren op het **Alfa** knooppunt.

1. Maak verbinding met het **Alfa** knooppunt met behulp van de Truffle-console.

    ```bash
    truffle console --network alpha
    ```

1. Voer in de Truffle-console code uit die de waarde van het contract exemplaar retourneert.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

    Als er een query wordt uitgevoerd op het **Alfa** knooppunt, wordt de waarde 42 geretourneerd. Bijvoorbeeld:

    ```
    admin@desktop:/mnt/c/truffledemo$ truffle console --network alpha
    truffle(alpha)> SimpleStorage.deployed().then(function(instance){return instance.get();})
    '42'
    ```

1. Sluit de Truffle-console af.

    ```bash
    .exit
    ```

Omdat we de open bare sleutel van het **bèta** -knoop punt niet hebben gedeclareerd in **privateFor**, kunnen we geen query uitvoeren op het **bèta** -knoop punt vanwege de privacy van het contract.

1. Maak met behulp van de Truffle-console verbinding met het **beta** -knoop punt.

    ```bash
    truffle console --network beta
    ```

1. Voer een code uit die de waarde van het contract exemplaar retourneert.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

1. Het uitvoeren van een query op het **bèta** knooppunt is mislukt omdat het contract privé is. Bijvoorbeeld:

    ```
    admin@desktop:/mnt/c/truffledemo$ truffle console --network beta
    truffle(beta)> SimpleStorage.deployed().then(function(instance){return instance.get();})
    Thrown:
    Error: Returned values aren't valid, did it run Out of Gas?
        at XMLHttpRequest._onHttpResponseEnd (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:345:8)
        at XMLHttpRequest._setReadyState (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request.ts:219:8)
        at XMLHttpRequestEventTarget.dispatchEvent (/mnt/c/truffledemo/node_modules/xhr2-cookies/xml-http-request-event-target.ts:44:13)
        at XMLHttpRequest.request.onreadystatechange (/mnt/c/truffledemo/node_modules/web3-providers-http/src/index.js:96:13)
    ```

1. Sluit de Truffle-console af.

    ```bash
    .exit
    ```
    
## <a name="send-a-transaction"></a>Een trans actie verzenden

1. Maak een bestand met `sampletx.js`de naam. Sla het op in de hoofdmap van het project.
1. Met het volgende script wordt de waarde van de contract **storedData** -variabele ingesteld op 65. Voeg de code toe aan het nieuwe bestand.

    ```javascript
    var SimpleStorage = artifacts.require("SimpleStorage");
    
    module.exports = function(done) {
      console.log("Getting deployed version of SimpleStorage...")
      SimpleStorage.deployed().then(function(instance) {
        console.log("Setting value to 65...");
        return instance.set("65", {privateFor: ["<alpha node public key>"], from:"<Ethereum account address>"});
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

    Vervang de waarden in de punt haken en sla het bestand op.

    | Value | Description
    |-------|-------------
    | \<alpha node public key\> | Open bare sleutel van het Alfa knooppunt
    | \<Ethereum account address\> | Het Ethereum-account adres dat is gemaakt in het standaard transactie knooppunt.

    **privateFor** definieert de knoop punten waarop de trans actie beschikbaar is. In dit voor beeld kan het account van het standaard transactie knooppunt persoonlijke trans acties naar het **Alfa** knooppunt casten. U moet open bare sleutels voor alle deel nemers aan de persoonlijke trans actie toevoegen.

1. Gebruik Truffle om het script voor het standaard transactie knooppunt uit te voeren.

    ```bash
    truffle exec sampletx.js --network defaultnode
    ```

1. Voer in de Truffle-console code uit die de waarde van het contract exemplaar retourneert.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```

    Als de trans actie is geslaagd, wordt de waarde 65 geretourneerd. Bijvoorbeeld:
    
    ```
    Getting deployed version of SimpleStorage...
    Setting value to 65...
    Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
    Finished!
    ```

1. Sluit de Truffle-console af.

    ```bash
    .exit
    ```
    
## <a name="validate-transaction-privacy"></a>Privacy van trans acties valideren

Vanwege de privacy van de trans actie kunnen trans acties alleen worden uitgevoerd op knoop punten die zijn gedeclareerd in **privateFor**. In dit voor beeld kunnen we trans acties uitvoeren omdat de open bare sleutel van het **Alfa** -knoop punt in **privateFor**is gedeclareerd. 

1. Gebruik Truffle om de trans actie op het **alpha** -knoop punt uit te voeren.

    ```bash
    truffle exec sampletx.js --network alpha
    ```
    
1. Voer code uit die de waarde van het contract exemplaar retourneert.

    ```bash
    SimpleStorage.deployed().then(function(instance){return instance.get();})
    ```
    
    Als de trans actie is geslaagd, wordt de waarde 65 geretourneerd. Bijvoorbeeld:

    ```
    Getting deployed version of SimpleStorage...
    Setting value to 65...
    Transaction: 0x864e67744c2502ce75ef6e5e09d1bfeb5cdfb7b880428fceca84bc8fd44e6ce0
    Finished!
    ```
    
1. Sluit de Truffle-console af.

    ```bash
    .exit
    ```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u deze niet meer nodig hebt, kunt u de resources verwijderen `myResourceGroup` door de resource groep te verwijderen die u hebt gemaakt door de Azure Block Chain-service.

De resource groep verwijderen:

1. In de Azure-portal, gaat u naar **resourcegroep** in het navigatiedeelvenster links en selecteert u de resourcegroep die u wilt verwijderen.
1. Selecteer **Resourcegroep verwijderen**. Bevestig de verwijdering door de naam van de resourcegroep in te voeren en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u twee transactie knooppunten toegevoegd om de privacy van contracten en trans acties te demonstreren. U hebt het standaard knooppunt gebruikt voor het implementeren van een privé-contract. U hebt de privacy getest door de contract waarden te doorzoeken en trans acties uit te voeren op de Block chain.

> [!div class="nextstepaction"]
> [Block Chain-toepassingen ontwikkelen met behulp van de Azure Block Chain-Service](develop.md)

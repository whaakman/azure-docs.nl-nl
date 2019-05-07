---
title: Maak een Azure Blockchain-Service met behulp van Azure CLI
description: Azure Blockchain-Service gebruiken voor het maken van een blockchain-lid met behulp van Azure CLI.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: e1b7558ea83c8948a8984215e15040e4d929cb1b
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141382"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-azure-cli"></a>Quickstart: Maak een Azure Blockchain Service blockchain-lid met behulp van Azure CLI

Azure Blockchain-Service is een blockchain-platform dat u gebruiken kunt voor het uitvoeren van uw bedrijfslogica binnen een slimme contract. In deze Quick Start laat zien hoe u aan de slag met het maken van een blockchain-lid met behulp van Azure CLI.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell starten

Azure Cloud Shell is een gratis interactieve shell waarmee u de stappen in dit artikel kunt uitvoeren. In deze shell zijn algemene Azure-hulpprogramma's vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account.

Als u Cloud Shell wilt openen, selecteert u **Proberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/bash](https://shell.azure.com/bash) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op Enter om de code uit te voeren.

Als u liever om te installeren en de CLI lokaal gebruikt, vereist deze snelstartgids Azure CLI versie 2.0.51 of hoger. Voer `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](https://docs.microsoft.com/cli/azure/group). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-blockchain-member"></a>Een blockchain-lid maken

Een blockchain-lid maken in Azure Blockchain-Service die het Quorum grootboek-protocol in een nieuwe consortium wordt uitgevoerd.

Er zijn meerdere parameters en eigenschappen die u nodig hebt om door te geven. De volgende parameters vervangen door uw waarden.

| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources worden gemaakt. Gebruik de resourcegroep die u in de vorige sectie hebt gemaakt.
| **name** | Een unieke naam ter identificatie van uw Azure Blockchain Service blockchain-lid. De naam wordt gebruikt voor het adres van het openbare eindpunt. Bijvoorbeeld `myblockchainmember.blockchain.azure.com`.
| **location** | Azure-regio waar het lid van de blockchain wordt gemaakt. Bijvoorbeeld `eastus`. Kies de locatie die zich het dichtst bij uw gebruikers of uw andere Azure-toepassingen bevindt.
| **Wachtwoord** | Het wachtwoord voor het lid. Het wachtwoord voor het lid wordt gebruikt voor verificatie met de blockchain-lid van de openbare eindpunt basisverificatie wordt gebruikt.
| **consortium** | De naam van het consortium te koppelen of te maken.
| **consortiumManagementAccountPassword** | Het wachtwoord van het management consortium. Dit wordt gebruikt voor het lidmaatschap van een consortium.
| **skuName** | Laagtype. Gebruik S0 voor Standard- en B0 voor Basic.

```azurecli-interactive
az resource create --resource-group myResourceGroup --name myblockchainmember --resource-type Microsoft.Blockchain/blockchainMembers --is-full-object --properties "{ \"location\": \"eastus\", \"properties\": {\"password\": \"strongMemberAccountPassword@1\", \"protocol\": \"Quorum\", \"consortium\": \"myConsortiumName\", \"consortiumManagementAccountPassword\": \"strongConsortiumManagementPassword@1\" }, \"sku\": { \"name\": \"S0\" } }"
```

Het duurt ongeveer 10 minuten om het lid van de blockchain en ondersteunende resources te maken.

De volgende voorbeelduitvoer wordt weergegeven van een geslaagde bewerking maken.

```json
{
  "id": "/subscriptions/<subscriptionId>/resourceGroups/myResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/mymembername",
  "kind": null,
  "location": "eastus",
  "name": "mymembername",
  "properties": {
    "ConsortiumMemberDisplayName": "mymembername",
    "consortium": "myConsortiumName",
    "consortiumManagementAccountAddress": "0xfe5fbb9d1036298abf415282f52397ade5d5beef",
    "consortiumManagementAccountPassword": null,
    "consortiumRole": "ADMIN",
    "dns": "mymembername.blockchain.azure.com",
    "protocol": "Quorum",
    "provisioningState": "Succeeded",
    "userName": "mymembername",
    "validatorNodesSku": {
      "capacity": 2
    }
  },
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "S0",
    "tier": "Standard"
  },
  "type": "Microsoft.Blockchain/blockchainMembers"
}
```

## <a name="clean-up-resources"></a>Resources opschonen

Voor de volgende quickstart of zelfstudie kunt u de blockchain-lid dat u hebt gemaakt. Wanneer u niet meer nodig hebt, kunt u de resources verwijderen door het verwijderen van de `myResourceGroup` resourcegroep die u hebt gemaakt door de Azure Blockchain-Service.

Voer de volgende opdracht om te verwijderen van de resourcegroep en alle gerelateerde resources.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

Nu u een blockchain-lid hebt gemaakt, kunt u proberen een van de verbinding met transactie knooppunt snelstartgidsen voor [Geth](connect-geth.md), [MetaMask](connect-metamask.md), of [Truffle](connect-truffle.md).

> [!div class="nextstepaction"]
> [Truffle gebruiken voor verbinding met een een Blockchain-Service van Azure-netwerk](connect-truffle.md)

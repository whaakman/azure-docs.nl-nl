---
title: Azure Blockchain-Service met behulp van Azure CLI beheren
description: Over het maken en beheren van Azure Blockchain-Service met Azure CLI
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: seal
manager: femila
ms.openlocfilehash: 100d50443c7ed839e57d80ceea3b8b86904e4ba7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027868"
---
# <a name="manage-azure-blockchain-service-with-azure-cli"></a>Azure Blockchain-Service met Azure CLI beheren

Naast de Azure portal, kunt u Azure CLI snel maken en beheren van blockchain-leden en transactie-knooppunten voor uw Azure Blockchain-Service.

Zorg ervoor dat u de meest recente hebt geïnstalleerd [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) en aangemeld bij een Azure-account met `az login`.

Vervang in de volgende voorbeelden, voorbeeld `<parameter names>` door uw eigen waarden.

## <a name="create-blockchain-member"></a>Blockchain lid maken

In het voorbeeld wordt een blockchain-lid gemaakt in Azure Blockchain-Service die het Quorum grootboek-protocol in een nieuwe consortium wordt uitgevoerd.

```azurecli
az resource create --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --is-full-object --properties '{ "location": "<myBlockchainLocation>", "properties": {"password": "<myStrongPassword>", "protocol": "Quorum", "consortium": "<myConsortiumName>", "consortiumManagementAccountPassword": "<myConsortiumManagementAccountPassword>", "firewallRules": [ { "ruleName": "<myRuleName>", "startIpAddress": "<myStartIpAddress>", "endIpAddress": "<myEndIpAddress>" } ] }, "sku": { "name": "<skuName>" } }'
```

| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources worden gemaakt. |
| **De naam** | Een unieke naam ter identificatie van uw Azure Blockchain Service blockchain-lid. De naam wordt gebruikt voor het adres van het openbare eindpunt. Bijvoorbeeld `myblockchainmember.blockchain.azure.com`. |
| **location** | Azure-regio waar het lid van de blockchain wordt gemaakt. Bijvoorbeeld `eastus`. Kies de locatie die zich het dichtst bij uw gebruikers of uw andere Azure-toepassingen bevindt. |
| **Wachtwoord** | Het wachtwoord voor het lid. Het wachtwoord voor het lid wordt gebruikt voor verificatie met de blockchain-lid van de openbare eindpunt basisverificatie wordt gebruikt. Het wachtwoord moet voldoen aan drie van de volgende vier voldoen: de lengte moet tussen 12 en 72 tekens, 1 kleine letter, 1 hoofdletter, 1 cijfer en 1 speciaal teken dat geen getal sign(#), percentage (%), komma (,), star(*), back-dubbel aanhalingsteken () \`), dubbele quote("), daarom enkele aanhalingstekens, koppeltekens en puntkomma (;). |
| **protocol** | Openbare preview-versie biedt ondersteuning voor Quorum. |
| **consortium** | De naam van het consortium te koppelen of te maken. |
| **consortiumManagementAccountPassword** | Het wachtwoord van het management consortium. Het wachtwoord wordt gebruikt voor het lidmaatschap van een consortium. |
| **RuleName** | De naam van de regel voor opname in een IP-adresbereik. Optionele parameter voor de firewall-regels.|
| **startIpAddress** | Begin van het IP-adresbereik voor opname in de whitelist. Optionele parameter voor de firewall-regels. |
| **endIpAddress** | Einde van het IP-adresbereik voor opname in de whitelist. Optionele parameter voor de firewall-regels. |
| **skuName** | Laagtype. Gebruik S0 voor Standard- en B0 voor Basic. |

## <a name="change-blockchain-member-password"></a>Blockchain lid-wachtwoord wijzigen

Voorbeeld wijzigt van een lid van de blockchain-wachtwoord.

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --set properties.password="<myStrongPassword>" --remove properties.consortiumManagementAccountAddress
```
| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources worden gemaakt. |
| **De naam** | Naam ter identificatie van uw Azure Blockchain Service lid. |
| **Wachtwoord** | Het wachtwoord voor het lid. Het wachtwoord moet voldoen aan drie van de volgende vier voldoen: de lengte moet tussen 12 en 72 tekens, 1 kleine letter, 1 hoofdletter, 1 cijfer en 1 speciaal teken dat geen getal sign(#), percentage (%), komma (,), star(*), back-dubbel aanhalingsteken () \`), dubbele quote("), daarom enkele aanhalingstekens, koppeltekens en puntkomma (;). |


## <a name="create-transaction-node"></a>Transactie-knooppunt maken

Maak een transactie-knooppunt in een bestaand blockchain-lid. U kunt door toe te voegen knooppunten transactie, isolatie verhogen en verdelen. Bijvoorbeeld, kan er een eindpunt van het knooppunt transactie voor verschillende toepassingen.

```azurecli
az resource create --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --resource-type Microsoft.Blockchain/blockchainMembers  --is-full-object --properties '{ "location": "<myRegion>", "properties": { "password": "<myStrongPassword>", "firewallRules": [ { "ruleName": "<myRuleName>", "startIpAddress": "<myStartIpAddress>", "endIpAddress": "<myEndIpAddress>" } ] } }'
```

| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources worden gemaakt. |
| **De naam** | De naam van het lid van de Service van Azure Blockchain blockchain die ook de naam van het nieuwe transactie-knooppunt. |
| **location** | Azure-regio waar het lid van de blockchain wordt gemaakt. Bijvoorbeeld `eastus`. Kies de locatie die zich het dichtst bij uw gebruikers of uw andere Azure-toepassingen bevindt. |
| **Wachtwoord** | Het wachtwoord van het knooppunt transactie. Het wachtwoord moet voldoen aan drie van de volgende vier voldoen: de lengte moet tussen 12 en 72 tekens, 1 kleine letter, 1 hoofdletter, 1 cijfer en 1 speciaal teken dat geen getal sign(#), percentage (%), komma (,), star(*), back-dubbel aanhalingsteken () \`), dubbele quote("), daarom enkele aanhalingstekens, koppeltekens en puntkomma (;). |
| **RuleName** | De naam van de regel voor opname in een IP-adresbereik. Optionele parameter voor de firewall-regels. |
| **startIpAddress** | Begin van het IP-adresbereik voor opname in de whitelist. Optionele parameter voor de firewall-regels. |
| **endIpAddress** | Einde van het IP-adresbereik voor opname in de whitelist. Optionele parameter voor de firewall-regels.|

## <a name="change-transaction-node-password"></a>Wachtwoord wijzigen transactie-knooppunt

Voorbeeld van wijzigt een transactie knooppunt-wachtwoord.

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --resource-type Microsoft.Blockchain/blockchainMembers  --set properties.password="<myStrongPassword>"
```

| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources bestaan. |
| **De naam** | De naam van het lid van de Service van Azure Blockchain blockchain die ook de naam van het nieuwe transactie-knooppunt. |
| **Wachtwoord** | Het wachtwoord van het knooppunt transactie. Het wachtwoord moet voldoen aan drie van de volgende vier voldoen: de lengte moet tussen 12 en 72 tekens, 1 kleine letter, 1 hoofdletter, 1 cijfer en 1 speciaal teken dat geen getal sign(#), percentage (%), komma (,), star(*), back-dubbel aanhalingsteken () \`), dubbele quote("), daarom enkele aanhalingstekens, koppeltekens en puntkomma (;). |

## <a name="change-consortium-management-account-password"></a>Wachtwoord wijzigen consortium management-account

Het consortium-account wordt gebruikt voor het beheer van consortium lidmaatschap. Elk lid wordt uniek geïdentificeerd door een consortium-account en kunt u het wachtwoord van dit account met de volgende opdracht.

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --set properties.consortiumManagementAccountPassword="<myConsortiumManagementAccountPassword>" --remove properties.consortiumManagementAccountAddress
```

| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources worden gemaakt. |
| **De naam** | Naam ter identificatie van uw Azure Blockchain Service lid. |
| **consortiumManagementAccountPassword** | Het accountwachtwoord consortium voor beheer. Het wachtwoord moet voldoen aan drie van de volgende vier voldoen: de lengte moet tussen 12 en 72 tekens, 1 kleine letter, 1 hoofdletter, 1 cijfer en 1 speciaal teken dat geen getal sign(#), percentage (%), komma (,), star(*), back-dubbel aanhalingsteken () \`), dubbele quote("), daarom enkele aanhalingstekens, koppeltekens en puntkomma (;). |
  
## <a name="update-firewall-rules"></a>Firewallregels bijwerken

```azurecli
az resource update --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers --set properties.firewallRules='[ { "ruleName": "<myRuleName>", "startIpAddress": "<myStartIpAddress>", "endIpAddress": "<myEndIpAddress>" } ]' --remove properties.consortiumManagementAccountAddress
```

| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources bestaan. |
| **De naam** | Naam van het lid van de blockchain Azure Blockchain-Service. |
| **RuleName** | De naam van de regel voor opname in een IP-adresbereik. Optionele parameter voor de firewall-regels.|
| **startIpAddress** | Begin van het IP-adresbereik voor opname in de whitelist. Optionele parameter voor de firewall-regels.|
| **endIpAddress** | Einde van het IP-adresbereik voor opname in de whitelist. Optionele parameter voor de firewall-regels.|

## <a name="list-api-keys"></a>Lijst met API-sleutels

API-sleutels kunnen worden gebruikt voor toegang tot die vergelijkbaar is met de gebruikersnaam en wachtwoord. Er zijn twee API-sleutels voor de ondersteuning van rouleren van de sleutel. Gebruik de volgende opdracht om uw API-sleutels weer te geven.

```azurecli
az resource invoke-action --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --action "listApiKeys" --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources bestaan. |
| **De naam** | De naam van het lid van de Service van Azure Blockchain blockchain die ook de naam van het nieuwe transactie-knooppunt. |

## <a name="regenerate-api-keys"></a>API-sleutels opnieuw genereren

Gebruik de volgende opdracht in uw API-sleutels opnieuw genereren.

```azurecli
az resource invoke-action --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --action "regenerateApiKeys" --resource-type Microsoft.Blockchain/blockchainMembers --request-body '{"keyName":"<keyValue>"}'
```


| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources bestaan. |
| **De naam** | De naam van het lid van de Service van Azure Blockchain blockchain die ook de naam van het nieuwe transactie-knooppunt. |
| **keyName** | Vervang \<met sleutel-waardeparen\> met key1 of key2. |

## <a name="delete-a-transaction-node"></a>Een transactie-knooppunt verwijderen

Voorbeeld wordt een blockchain lid transactie knooppunt verwijderd.

```azurecli
az resource delete --resource-group <myResourceGroup> --name <myMemberName>/transactionNodes/<myTransactionNode> --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources bestaan. |
| **De naam** | De naam van het lid van de Service van Azure Blockchain blockchain die ook de naam van het nieuwe transactie knooppunt moet worden verwijderd. |

## <a name="delete-a-blockchain-member"></a>Een blockchain-lid verwijderen

Voorbeeld wordt een blockchain-lid.

```azurecli
az resource delete --resource-group <myResourceGroup> --name <myMemberName> --resource-type Microsoft.Blockchain/blockchainMembers
```

| Parameter | Description |
|---------|-------------|
| **resource-group** | Resourcegroepnaam waar Azure Blockchain-serviceresources bestaan. |
| **De naam** | Naam van het lid van de blockchain Azure Blockchain-Service moet worden verwijderd. |

## <a name="azure-active-directory"></a>Azure Active Directory

### <a name="grant-access-for-azure-ad-user"></a>Toegang verlenen voor Azure AD-gebruiker

```azurecli
az role assignment create --role <role> --assignee <assignee> --scope /subscriptions/<subId>/resourceGroups/<groupName>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>
```

| Parameter | Description |
|---------|-------------|
| **Rol** | De naam van de Azure AD-rol. |
| **assignee** | Azure AD user ID. Bijvoorbeeld: `user@contoso.com` |
| **scope** | Bereik van de roltoewijzing. Een van beide blockchain lid kan zijn van de transactie-knooppunt. |

**Voorbeeld:**

Knooppunt toegangsmachtigingen voor Azure AD-gebruiker tot blockchain **lid**:

```azurecli
az role assignment create \
  --role "myRole" \
  --assignee user@contoso.com \
  --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

**Voorbeeld:**

Knooppunt toegangsmachtigingen voor Azure AD-gebruiker tot blockchain **transactie knooppunt**:

```azurecli
az role assignment create \
  --role "MyRole" \
  --assignee user@contoso.com \
  --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1/transactionNodes/contosoTransactionNode1
```

### <a name="grant-node-access-for-azure-ad-group-or-application-role"></a>Knooppunt toegangsmachtigingen voor Azure AD-groep of toepassing rol

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id>
```
| Parameter | Description |
|---------|-------------|
| **Rol** | De naam van de Azure AD-rol. |
| **assignee-object-id** | Azure AD-groeps-ID of de toepassing-id. |
| **scope** | Bereik van de roltoewijzing. Een van beide blockchain lid kan zijn van de transactie-knooppunt. |

**Voorbeeld:**

Knooppunt toegangsmachtigingen voor **toepassingsrol**

```azurecli
az role assignment create \
  --role "myRole" \
  --assignee-object-id 22222222-2222-2222-2222-222222222222 \
  --scope /subscriptions/mySubscriptionId/resourceGroups/contosoResourceGroup/providers/Microsoft.Blockchain/blockchainMembers/contosoMember1
```

### <a name="remove-azure-ad-node-access"></a>Azure AD-knooppunt toegang verwijderen

```azurecli
az role assignment delete --role <myRole> --assignee <assignee> --scope /subscriptions/mySubscriptionId/resourceGroups/<myResourceGroup>/providers/Microsoft.Blockchain/blockchainMembers/<myMemberName>/transactionNodes/<myTransactionNode>
```

| Parameter | Description |
|---------|-------------|
| **Rol** | De naam van de Azure AD-rol. |
| **assignee** | Azure AD user ID. Bijvoorbeeld: `user@contoso.com` |
| **scope** | Bereik van de roltoewijzing. Een van beide blockchain lid kan zijn van de transactie-knooppunt. |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Azure Blockchain Service transactie knooppunten met de Azure-portal configureren](configure-transaction-nodes.md)

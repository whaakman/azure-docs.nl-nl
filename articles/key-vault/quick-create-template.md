---
title: 'Azure-Snelstart: een Azure-sleutelkluis en een geheim maken met behulp van Azure Resource Manager-sjabloon | Microsoft Docs'
description: Snelstart van het Azure-sleutelkluizen maken en geheimen toevoegen aan de kluizen met behulp van Azure Resource Manager-sjabloon.
services: key-vault
author: mumian
manager: dougeby
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/04/2019
ms.author: jgao
ms.openlocfilehash: 4b774e5b0a5c64a9af9a5a54ba264c6226558a24
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57880020"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-resource-manager-template"></a>Quickstart: Instellen en ophalen van een geheim uit Azure Key Vault met behulp van Resource Manager-sjabloon

[Azure Key Vault](./key-vault-overview.md) is een cloudservice die voorziet in een beveiligd archief geheimen, zoals sleutels, wachtwoorden, certificaten, en andere geheimen. In deze snelstart richt zich op het proces voor het implementeren van een Resource Manager-sjabloon voor het maken van een key vault en een geheim. Zie voor meer informatie over het ontwikkelen van Resource Manager-sjablonen [Resource Manager-documentatie](/azure/azure-resource-manager/) en de [sjabloonverwijzing](/azure/templates/microsoft.keyvault/allversions).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* Uw Azure AD-gebruikersobject-id is in de sjabloon nodig om machtigingen te kunnen configureren. Met de volgende procedure wordt de object-id (GUID) opgehaald.

    1. Voer de volgende Azure PowerShell of Azure CLI-opdracht door Selecteer **uitproberen**, en plak het script in het deelvenster shell. Als u het script, met de rechtermuisknop op de shell en selecteer vervolgens **plakken**. 

        ```azurecli-interactive
        echo "Enter your email address that is used to sign in to Azure:" &&
        read upn &&
        az ad user show --upn-or-object-id $upn --query "objectId" 
        ```

        ```azurepowershell-interactive
        $upn = Read-Host -Prompt "Enter your email address used to sign in to Azure"
        (Get-AzADUser -UserPrincipalName $upn).Id
        ```

    2. Noteer de object-id. U hebt deze nodig in de volgende sectie van deze quickstart.

## <a name="create-a-vault-and-a-secret"></a>Een kluis en een geheim maken

De sjabloon die wordt gebruikt in deze Quick Start is afkomstig van [Azure Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/101-key-vault-create/). Meer voorbeelden van Azure Key Vault sjabloon vindt [hier](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Keyvault).

1. Selecteer de volgende afbeelding om u aan te melden bij Azure en een sjabloon te openen. Met de sjabloon worden een sleutelkluis en een geheim gemaakt.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-key-vault-create%2Fazuredeploy.json"><img src="./media/quick-create-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Typ of selecteer de volgende waarden.  

    ![Resource Manager-sjabloon - sleutelkluisintegratie - portalimplementatie](./media/quick-create-template/create-key-vault-using-template-portal.png)

    Tenzij deze is opgegeven, wordt de standaardwaarde gebruiken om de sleutelkluis en een geheim te maken.

    * **Abonnement**: selecteer een Azure-abonnement.
    * **Resourcegroep**: Selecteer **nieuw**, voer een unieke naam voor de resourcegroep en klik vervolgens op **OK**. 
    * **Locatie**: selecteer een locatie.  Bijvoorbeeld **US - centraal**.
    * **Key Vault-naam**: Voer een unieke naam voor de key vault.  
    * **Tenant-id**: met de sjabloonfunctie wordt uw tenant-id automatisch opgehaald.  Wijzig de standaardwaarde niet
    * **AD-gebruikers-Id**: Voer uw Azure AD gebruiker object-ID die u hebt opgehaald via [vereisten](#prerequisites).
    * **Geheime naam**: Voer een naam voor de geheime sleutel die u in de key vault opslaat.  Bijvoorbeeld, **adminpassword**
    * **Geheime waarde**: Voer de geheime waarde.  Als u een wachtwoord opslaat, is het raadzaam het gegenereerde wachtwoord dat u hebt gemaakt in vereisten.
    * **Ik ga akkoord met de bovenstaande voorwaarden**: Selecteren.
3. Selecteer **Aankoop**.

## <a name="validate-the-deployment"></a>De implementatie valideren

U kunt Azure portal gebruiken om de key vault en het geheim te controleren of u de volgende Azure CLI of Azure PowerShell-script gebruiken om het geheim die is gemaakt.

```azurecli-interactive
echo "Enter your key vault name:" &&
read keyVaultName &&
az keyvault secret list --vault-name $keyVaultName
```

```azurepowershell-interactive
$keyVaultName = Read-Host -Prompt "Enter your key vault name"
Get-AzKeyVaultSecret -vaultName $keyVaultName
```

## <a name="clean-up-resources"></a>Resources opschonen

Andere Key Vault-snelstarts en zelfstudies bouwen voort op deze snelstart. Als u van plan bent om verder te gaan met volgende snelstarts en zelfstudies, kunt u deze resources intact laten.
Als u die niet meer nodig hebt, verwijdert u de resourcegroep. Hierdoor worden ook de sleutelkluis en de gerelateerde resources verwijderd. De resourcegroep verwijderen met behulp van Azure CLI of Azure Powershell:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName 
```
```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName 
```

## <a name="next-steps"></a>Volgende stappen

* [Startpagina van Azure Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Documentatie voor Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)
* [Azure SDK voor Node](https://docs.microsoft.com/javascript/api/overview/azure/key-vault)
* [Azure REST API-naslaginformatie](https://docs.microsoft.com/rest/api/keyvault/)

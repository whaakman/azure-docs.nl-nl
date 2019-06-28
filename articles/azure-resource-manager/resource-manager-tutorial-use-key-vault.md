---
title: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie | Microsoft Docs
description: Ontdek hoe u Azure Key Vault gebruikt om veilig parameterwaarden door te geven tijdens het implementeren van Resource Manager-sjablonen
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 05/23/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: b91a3488750795e8ee9df6602bb2b3df8a9b08ec
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67436580"
---
# <a name="tutorial-integrate-azure-key-vault-in-your-resource-manager-template-deployment"></a>Zelfstudie: Azure Key Vault integreren in uw sjabloonimplementatie Resource Manager

Informatie over het ophalen van geheimen vanuit een Azure key vault en de geheimen doorgeven als parameters bij het implementeren van Azure Resource Manager. De waarde van parameter wordt nooit weergegeven, omdat u verwijzen naar alleen de sleutelkluis-ID. Zie voor meer informatie, [Azure Key Vault gebruikt om door te geven beveiligde parameterwaarde tijdens de implementatie van](./resource-manager-keyvault-parameter.md).

In de [implementatievolgorde resource ingesteld](./resource-manager-tutorial-create-templates-with-dependent-resources.md) zelfstudie maakt u een virtuele machine (VM) maken. U moet de virtuele machine administrator-gebruikersnaam en wachtwoord opgeven. U kunt in plaats van het wachtwoord geven, het wachtwoord in een Azure-sleutelkluis vooraf opslaan en vervolgens de sjabloon om het wachtwoord van de key vault tijdens de implementatie aanpassen.

![Diagram weergeven van de integratie van Resource Manager-sjabloon met een key vault](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een sleutelkluis voorbereiden
> * Een snelstartsjabloon openen
> * Het parameterbestand bewerken
> * De sjabloon implementeren
> * De implementatie valideren
> * Resources opschonen

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/) met de [Resource Manager-Tools-extensie](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Als u wilt beveiligen, moet u een gegenereerde wachtwoord gebruiken voor het beheerdersaccount van de virtuele machine. Hier volgt een voorbeeld voor het genereren van een wachtwoord:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Controleren of de gegenereerd wachtwoord voldoet aan de vereisten voor het VM-wachtwoord. Elke Azure-service heeft eigen wachtwoordvereisten. Zie voor de wachtwoordvereisten VM [wat zijn de wachtwoordvereisten bij het maken van een virtuele machine?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="prepare-a-key-vault"></a>Een sleutelkluis voorbereiden

In deze sectie maakt u een sleutelkluis maken en een geheim toevoegen aan, zodat u het geheim ophalen kunt wanneer u de sjabloon implementeert. Er zijn veel manieren om een sleutelkluis te maken. In deze zelfstudie hebt u Azure PowerShell gebruiken om te implementeren een [Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json). Deze sjabloon doet het volgende:

* Hiermee maakt u een key vault met de `enabledForTemplateDeployment` eigenschap is ingeschakeld. Deze eigenschap moet worden *waar* voordat de sjabloonimplementatie proces toegang tot de geheimen die zijn gedefinieerd in de key vault.
* Een geheim toevoegen aan de sleutelkluis. Het geheim slaat het beheerderswachtwoord voor de virtuele machine.

> [!NOTE]
> Als de gebruiker die de implementatie van de virtuele-machinesjabloon, als u niet de eigenaar van bent of een bijdrager aan de sleutelkluis, de eigenaar of Inzender moet u toegang verlenen tot de *Microsoft.KeyVault/vaults/deploy/action* machtiging voor de key vault. Zie voor meer informatie, [Azure Key Vault gebruikt om door te geven van de waarde van een beveiligde parameter tijdens de implementatie van](./resource-manager-keyvault-parameter.md).

Als u wilt de volgende Azure PowerShell-script uitvoeren, selecteert u **uitproberen** Azure Cloud Shell te openen. Als u het script, met de rechtermuisknop op de shell-venster en selecteer vervolgens **plakken**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$upn = Read-Host -Prompt "Enter your user principal name (email address) used to sign in to Azure"
$secretValue = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$keyVaultName = $projectName
$adUserId = (Get-AzADUser -UserPrincipalName $upn).Id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -keyVaultName $keyVaultName -adUserId $adUserId -secretValue $secretValue
```

> [!IMPORTANT]
> * Naam van de resourcegroep is de naam van het project, maar met **rg** hieraan toegevoegd. Om gemakkelijker te [verwijdert u de resources die u hebt gemaakt in deze zelfstudie](#clean-up-resources), gebruik dezelfde naam en resourcegroep projectgroep naam wanneer u [implementeren van de volgende sjabloon](#deploy-the-template).
> * De standaardnaam voor de geheime sleutel is **vmAdminPassword**. De vastgelegd in de sjabloon.
> * Als u wilt inschakelen in de sjabloon om op te halen van de geheime sleutel, moet u een toegangsbeleid met de naam 'Toegang tot Azure Resource Manager voor sjabloonimplementatie inschakelen' voor de key vault inschakelen. Dit beleid is ingeschakeld in de sjabloon. Zie voor meer informatie over het toegangsbeleid [implementeren sleutelkluizen en geheimen](./resource-manager-keyvault-parameter.md#deploy-key-vaults-and-secrets).

De sjabloon bevat één uitvoerwaarde, met de naam *keyVaultId*. Noteer de waarde van de ID voor later gebruik wanneer u de virtuele machine implementeert. De indeling van de resource-ID is:

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

Wanneer u kopieert en plakt u de ID, kan deze worden onderverdeeld in meerdere regels. Samenvoegen van de regels en de extra spaties wissen.

Als u wilt controleren of de implementatie, de volgende PowerShell-opdracht worden uitgevoerd in het deelvenster met dezelfde shell om op te halen van het geheim in niet-versleutelde tekst. De opdracht werkt alleen in dezelfde shellsessie, omdat het gebruikmaakt van de variabele *$keyVaultName*, die is gedefinieerd in het voorgaande PowerShell-script.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

U hebt nu een key vault en een geheim voorbereid. De volgende secties laten zien hoe u een bestaande sjabloon om op te halen van de geheime sleutel tijdens de implementatie aanpassen.

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen. In plaats van een sjabloon helemaal vanaf de basis te maken, kunt u een voorbeeldsjabloon zoeken en aanpassen. De sjabloon die wordt gebruikt in deze zelfstudie wordt aangeroepen [een eenvoudige Windows-VM implementeren](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. Selecteer in Visual Studio Code **File** > **Open File**.

1. In de **bestandsnaam** vak, plak de volgende URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Selecteer **Openen** om het bestand te openen. Het scenario is hetzelfde als de waarde die wordt gebruikt in [zelfstudie: Azure Resource Manager-sjablonen met afhankelijke resources maken](./resource-manager-tutorial-create-templates-with-dependent-resources.md).
   In de sjabloon zijn vijf resources gedefinieerd:

   * `Microsoft.Storage/storageAccounts`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

   Is het handig om enige basiskennis van de sjabloon hebt voordat u deze aanpassen.

1. Selecteer **bestand** > **OpslaanAls**, en sla een kopie van het bestand op uw lokale computer met de naam van de *azuredeploy.json*.

1. Herhaal de stappen 1-3 te openen van de volgende URL en sla het bestand op als *azuredeploy.parameters.json*.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Het parameterbestand bewerken

U hoeft het sjabloonbestand niet te wijzigen.

1. Open in Visual Studio Code, *azuredeploy.parameters.json* als deze nog niet is geopend.
1. Update de `adminPassword` parameter:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    > [!IMPORTANT]
    > Vervang de waarde voor **id** met de resource-ID van de sleutelkluis die u in de vorige procedure hebt gemaakt.

    ![key vault en Resource Manager sjabloon voor virtuele machine-implementatiebestand parameters integreren](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)

1. Werk de volgende waarden bij:

    * **adminUsername**: De naam van het beheerdersaccount van de virtuele machine.
    * **dnsLabelPrefix**: Naam van de dnsLabelPrefix-waarde.

    Zie voor voorbeelden van de namen van de voorgaande afbeelding.

1. Sla de wijzigingen op.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Volg de instructies in [implementeren van de sjabloon](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Uploaden van beide *azuredeploy.json* en *azuredeploy.parameters.json* Cloud Shell en gebruik vervolgens de volgende PowerShell-script om de sjabloon te implementeren:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$location = Read-Host -Prompt "Enter the same location that is used for creating the key vault (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "$HOME/azuredeploy.json" `
    -TemplateParameterFile "$HOME/azuredeploy.parameters.json"
```

Wanneer u de sjabloon implementeert, gebruikt u dezelfde resourcegroep die u hebt gebruikt in de key vault. Deze aanpak maakt het eenvoudiger voor u voor het opschonen van de resources, omdat u nodig hebt om slechts één resourcegroep in plaats van twee te verwijderen.

## <a name="validate-the-deployment"></a>De implementatie valideren

Nadat u de virtuele machine hebt geïmplementeerd, test u de referenties aanmelden met behulp van het wachtwoord die zijn opgeslagen in de key vault.

1. Open de [Azure Portal](https://portal.azure.com).

1. Selecteer **resourcegroepen** >  **\<*YourResourceGroupName*>**  > **simpleWinVM** .
1. Selecteer **verbinding** aan de bovenkant.
1. Selecteer **RDP-bestand downloaden**, en volg de instructies om aan te melden bij de virtuele machine met behulp van het wachtwoord die zijn opgeslagen in de key vault.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u uw Azure-resources niet meer nodig hebt, kunt u opschonen van de resources die u hebt geïmplementeerd door de resourcegroep te verwijderen.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een geheim vanuit uw Azure key vault opgehaald. Vervolgens gebruikt u het geheim in de sjabloonimplementatie van uw. Voor meer informatie over het maken van gekoppelde sjablonen ziet u:

> [!div class="nextstepaction"]
> [Gekoppelde sjablonen maken](./resource-manager-tutorial-create-linked-templates.md)

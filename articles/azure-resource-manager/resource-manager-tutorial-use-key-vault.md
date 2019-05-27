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
ms.openlocfilehash: 0d78e6eaca708073c3a216507b320fe8783a25b6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239242"
---
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>Zelfstudie: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie

Ontdek hoe u geheime waarden uit Azure Key Vault ophaalt en deze als parameters doorgeeft tijdens de implementatie van Resource Manager. De waarde zelf wordt nooit getoond, omdat u alleen verwijst naar de sleutelkluis-id. Zie [Azure Key Vault gebruiken om veilig parameterwaarden door te geven tijdens de implementatie](./resource-manager-keyvault-parameter.md) voor meer informatie

In de [implementatievolgorde resource ingesteld](./resource-manager-tutorial-create-templates-with-dependent-resources.md) zelfstudie maakt u een virtuele machine maken. U moet de virtuele machine administrator-gebruikersnaam en wachtwoord opgeven. U kunt in plaats van het bieden van het wachtwoord, vooraf het wachtwoord in een Azure Key Vault opslaan en vervolgens de sjabloon om het wachtwoord van de key vault tijdens de implementatie aanpassen.

![Diagram van Resource Manager sjabloon Key Vault-integratie](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een sleutelkluis voorbereiden
> * Een quickstartsjabloon openen
> * Het parameterbestand bewerken
> * De sjabloon implementeren
> * De implementatie valideren
> * Resources opschonen

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/) met de [extensie Resource Manager Tools](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Gebruik een gegenereerd wachtwoord voor het beheerdersaccount van de virtuele machine om de beveiliging te verhogen. Hier volgt een voorbeeld voor het genereren van een wachtwoord:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Controleer of het gegenereerde wachtwoord voldoet aan de wachtwoordvereisten voor virtuele machines. Elke Azure-service heeft eigen wachtwoordvereisten. Zie [Wat zijn de wachtwoordvereisten bij het maken van een virtuele machine?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) voor meer informatie over VM-wachtwoordvereisten.

## <a name="prepare-a-key-vault"></a>Een sleutelkluis voorbereiden

In deze sectie maakt u een sleutelkluis maken en een geheim toevoegen aan de key vault, zodat u het geheim ophalen kunt wanneer u de sjabloon implementeert. Er zijn veel manieren om een sleutelkluis te maken. In deze zelfstudie hebt u Azure PowerShell gebruiken om te implementeren een [Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json). Dit sjabloon doet het volgende:

* Een sleutelkluis maken waarvoor de eigenschap `enabledForTemplateDeployment` is ingeschakeld. Deze eigenschap moet de waarde 'true' hebben, anders kan tijdens het sjabloonimplementatieproces geen toegang worden verkregen tot de geheimen die in deze sleutelkluis zijn gedefinieerd.
* Voeg een geheim toe aan de sleutelkluis.  In het geheim wordt het beheerderswachtwoord van de virtuele machine opgeslagen.

> [!NOTE]
> Als u (als de gebruiker die de virtuele-machinesjabloon gaat implementeren) niet de eigenaar of inzender bent van de sleutelkluis, moet de eigenaar of inzender van de sleutelkluis u toegang verlenen tot de machtiging Microsoft.KeyVault/vaults/deploy/action voor de sleutelkluis. Zie [Azure Key Vault gebruiken om veilig parameterwaarden door te geven tijdens de implementatie](./resource-manager-keyvault-parameter.md) voor meer informatie

Als u wilt het volgende PowerShell-script uitvoeren, selecteert u **uitproberen** naar de cloudshell te openen. Als u het script, met de rechtermuisknop op de shell-venster en selecteer vervolgens **plakken**.

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

Een aantal onderdelen van belangrijke informatie:

* Naam van de resourcegroep met de naam van het project is **rg** toegevoegd. Om gemakkelijker te [verwijdert u de resources die zijn gemaakt in deze zelfstudie](#clean-up-resources), gebruik dezelfde naam en resourcegroep projectgroep naam wanneer u [implementeren van de volgende sjabloon](#deploy-the-template).
* De standaardnaam voor de naam van het geheim is **vmAdminPassword**. Deze is vastgelegd in de sjabloon.
* Als u voor de sjabloon om op te halen van de geheime sleutel, moet u een toegangsbeleid met de naam inschakelen **inschakelen van toegang tot Azure Resource Manager voor sjabloonimplementatie** voor de key vault. Dit beleid is ingeschakeld in de sjabloon. Zie voor meer informatie over deze toegangsbeleid [implementeren sleutelkluizen en geheimen](./resource-manager-keyvault-parameter.md#deploy-key-vaults-and-secrets).

De sjabloon bevat één uitvoerwaarde met de naam **keyVaultId**. Noteer de waarde in. U hebt deze id nodig bij het implementeren van de virtuele machine. De indeling van de resource-id is:

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

Wanneer u kopieert en plakt u de ID, kan de ID worden onderverdeeld in meerdere regels. U moet de regels samenvoegen en de extra spaties wissen.

Als u wilt controleren of de implementatie, de volgende PowerShell-opdracht worden uitgevoerd in het deelvenster met dezelfde shell om op te halen van het geheim in niet-versleutelde tekst. De opdracht werkt alleen in dezelfde shellsessie omdat deze gebruikmaakt van een variabele $keyVaultName gedefinieerd in de vorige PowerShell-script.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

Nu hebt u voorbereid een key vault en een geheim in de volgende secties ziet u hoe het aanpassen van een bestaande sjabloon om op te halen van de geheime sleutel tijdens de implementatie.

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen. In plaats van een sjabloon helemaal vanaf de basis te maken, kunt u een voorbeeldsjabloon zoeken en aanpassen. De sjabloon die in deze zelfstudie wordt gebruikt, heet [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Een eenvoudige Windows-VM implementeren).

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

3. Selecteer **Openen** om het bestand te openen. Dit is hetzelfde scenario als in [Zelfstudie: Azure Resource Manager-sjablonen met afhankelijke resources maken](./resource-manager-tutorial-create-templates-with-dependent-resources.md).
4. Er worden vijf resources gedefinieerd met de sjabloon:

   * `Microsoft.Storage/storageAccounts`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Het is handig om enige basiskennis te hebben van de sjabloon voordat u deze gaat aanpassen.
5. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.
6. Herhaal stap 1-4 om de volgende URL te openen en sla het bestand dan op als **azuredeploy.parameters.json**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Het parameterbestand bewerken

U hoeft het sjabloonbestand niet te wijzigen.

1. Open **azuredeploy.parameters.json** in Visual Studio Code als het bestand nog niet is geopend.
2. Werk de parameter **adminPassword** bij naar:

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
    > Vervang de waarde van **id** met de resource-ID van uw key vault in de laatste procedure hebt gemaakt.

    ![Key Vault en Resource Manager-sjabloon integreren - virtuele-machine-implementatie - parameterbestand](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Geef waarden op voor:

    * **adminUsername**: geef het beheerdersaccount van de virtuele machine een naam.
    * **dnsLabelPrefix**: geef dnsLabelPrefix een naam.

    Bekijk een voorbeeld in de vorige schermafbeelding.

4. Sla de wijzigingen op.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Volg de instructies in [De sjabloon implementeren](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) om de sjabloon te implementeren. U moet beide uploaden **azuredeploy.json** en **azuredeploy.parameters.json** naar de cloudshell en gebruik vervolgens de volgende PowerShell-script om de sjabloon te implementeren:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$location = Read-Host -Prompt "Enter the same location that is used for creating the key vault (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "$HOME/azuredeploy.json" `
    -TemplateParameterFile "$HOME/azuredeploy.parameters.json"
```

Wanneer u de sjabloon implementeert, gebruikt u dezelfde resourcegroep als die van de sleutelkluis. Dit is handiger bij het opschonen van de resources. U hoeft dan slechts één resourcegroep te verwijderen in plaats van twee.

## <a name="validate-the-deployment"></a>De implementatie valideren

Wanneer u de virtuele machine hebt geïmplementeerd, test u of u zich kunt aanmelden met het wachtwoord dat in de sleutelkluis is opgeslagen.

1. Open de [Azure Portal](https://portal.azure.com).
2. Selecteer **Resourcegroepen**/**Naam van uw resourcegroep>** /**simpleWinVM**
3. Selecteer bovenaan de optie **Verbinding maken**.
4. Selecteer **RDP-bestand downloaden** en volg de instructies voor het aanmelden bij de virtuele machine, met het wachtwoord dat is opgeslagen in de sleutelkluis.

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een geheim opgehaald uit Azure Key Vault en hebt u dat geheim gebruikt in uw sjabloonimplementatie.  Voor meer informatie over het maken van gekoppelde sjablonen ziet u:

> [!div class="nextstepaction"]
> [Gekoppelde sjablonen maken](./resource-manager-tutorial-create-linked-templates.md)

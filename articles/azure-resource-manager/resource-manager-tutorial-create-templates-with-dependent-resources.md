---
title: Azure Resource Manager-sjablonen met afhankelijke resources maken | Microsoft Docs
description: Leer hoe u een Azure Resource Manager-sjabloon maakt met meerdere resources en hoe u deze via Azure Portal implementeert
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/09/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 50f1c81f08787181de2fe3a9f6fb97a96a2bd882
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114309"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Zelfstudie: Azure Resource Manager-sjablonen met afhankelijke resources maken

Leer hoe u een Azure Resource Manager-sjabloon maakt voor het implementeren van meerdere resources.  Nadat u de sjabloon hebt gemaakt, kunt u de sjabloon implementeren met behulp van de Cloud Shell van Azure Portal.

In deze zelfstudie hebt u een opslagaccount, een virtuele machine, een virtueel netwerk en enkele andere afhankelijke resources gemaakt. Sommige resources kunnen niet worden geïmplementeerd totdat er een andere resource bestaat. U kunt bijvoorbeeld niet een virtuele machine maken totdat het bijbehorende opslagaccount en de bijbehorende netwerkinterface bestaan. U definieert deze relatie door een resource afhankelijk van de andere resources te maken. Resource Manager evalueert de afhankelijkheden tussen resources en implementeert ze in de volgorde van afhankelijkheid. Als resources niet van elkaar afhankelijk zijn, worden deze door Resource Manager parallel geïmplementeerd. Zie [Define the order for deploying resources in Azure Resource Manager Templates](./resource-group-define-dependencies.md) (De volgorde voor het implementeren van resources definiëren in Azure Resource Manager-sjablonen) voor meer informatie.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Key Vault voorbereiden
> * Een snelstartsjabloon openen
> * De sjabloon verkennen
> * Het parameterbestand bewerken
> * De sjabloon implementeren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/) met de extensie Resource Manager Tools.  Zie [De extensie installeren](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="prepare-key-vault"></a>Key Vault voorbereiden

Als u een spraying-aanval op wachtwoorden wilt voorkomen, wordt het aangeraden om een automatisch gegenereerd wachtwoord te gebruiken voor het beheerdersaccount van de virtuele machine en om Key Vault te gebruiken voor het opslaan van het wachtwoord. Met de volgende procedure maakt u een sleutelkluis en een geheim om het wachtwoord op te slaan. Hiermee configureert u ook de machtigingen die nodig zijn voor de sjabloonimplementatie, om toegang te verkrijgen tot het geheim dat in de sleutelkluis is opgeslagen. Er is aanvullend toegangsbeleid nodig als de sleutelkluis onderdeel is van een ander Azure-abonnement. Zie [Azure Key Vault gebruiken om veilig parameterwaarden door te geven tijdens de implementatie](./resource-manager-keyvault-parameter.md) voor meer informatie.

1. Meld u aan bij [Azure Cloud Shell](https://shell.azure.com).
2. Schakel in de linkerbovenhoek over naar uw favoriete omgeving (**PowerShell** of **Bash**).
3. Voer de volgende Azure PowerShell- of Azure CLI-opdracht uit.  

    ```azurecli-interactive
    keyVaultName='<your-unique-vault-name>'
    resourceGroupName='<your-resource-group-name>'
    location='Central US'
    userPrincipalName='<your-email-address-associated-with-your-subscription>'
    
    # Create a resource group
    az group create --name $resourceGroupName --location $location
    
    # Create a Key Vault
    keyVault=$(az keyvault create \
      --name $keyVaultName \
      --resource-group $resourceGroupName \
      --location $location \
      --enabled-for-template-deployment true)
    keyVaultId=$(echo $keyVault | jq -r '.id')
    az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

    # Create a secret
    password=$(openssl rand -base64 32)
    az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: $keyVaultId."
    ```

    ```azurepowershell-interactive
    $keyVaultName = "<your-unique-vault-name>"
    $resourceGroupName="<your-resource-group-name>"
    $location='Central US'
    $userPrincipalName="<your-email-address-associated-with-your-subscription>"
    
    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
    # Create a Key Vault
    $keyVault = New-AzureRmKeyVault `
      -VaultName $keyVaultName `
      -resourceGroupName $resourceGroupName `
      -Location $location `
      -EnabledForTemplateDeployment
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list
      
    # Create a secret
    $password = openssl rand -base64 32
    
    $secretValue = ConvertTo-SecureString $password -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretValue
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: " $keyVault.ResourceID
    ```
4. Noteer de uitvoerwaarden. U hebt ze later in de zelfstudie nodig

> [!NOTE]
> Elke Azure-service heeft eigen wachtwoordvereisten. U vindt de vereisten voor virtuele Azure-machines bijvoorbeeld in 'Wat zijn de wachtwoordvereisten bij het maken van een VM?'

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen. In plaats van een sjabloon helemaal vanaf de basis te maken, kunt u een voorbeeldsjabloon zoeken en aanpassen. De sjabloon die in deze zelfstudie wordt gebruikt, heet [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Een eenvoudige Windows-VM implementeren).

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.
4. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.
5. Herhaal stap 1-4 om **https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json** te openen en sla het bestand dan op als **azuredeploy.parameters.json**.

## <a name="explore-the-template"></a>De sjabloon verkennen

Wanneer u de sjabloon in deze sectie verkent, probeert u om deze vragen te beantwoorden:

- Hoeveel Azure-resources zijn er in deze sjabloon gedefinieerd?
- Een van de resources is een Azure-opslagaccount.  Ziet de definitie eruit zoals de definitie die is gebruikt in de laatste zelfstudie?
- Kunt u de sjabloonverwijzingen vinden voor de resources die zijn gedefinieerd in deze sjabloon?
- Kunt u de afhankelijkheden van de resources vinden?

1. Vouw in Visual Studio Code de elementen samen totdat u alleen de elementen op het eerste niveau en tweede niveau binnen **resources** ziet:

    ![Azure Resource Manager-sjablonen in Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    Er worden vijf resources gedefinieerd door de sjabloon.
2. Vouw de eerste resource uit. Dit is een opslagaccount. De definitie is identiek aan de definitie die is gebruikt in het begin van de vorige zelfstudie.

    ![Azure Resource Manager-sjablonen in Visual Studio Code: definitie van opslagaccount](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

3. Vouw de tweede resource uit. Het resourcetype is **Microsoft.Network/publicIPAddresses**. Als u de sjabloonverwijzing wilt zoeken, bladert u naar [Sjabloonverwijzing](https://docs.microsoft.com/azure/templates/) en typt u **Openbaar IP-adres** of **Openbare IP-adressen** in het veld **Filteren op titel**. Vergelijk de resourcedefinitie met de sjabloonverwijzing.

    ![Azure Resource Manager-sjablonen in Visual Studio Code: definitie van openbaar IP-adres](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)
4. Herhaal de vorige stap om de sjabloonverwijzingen te zoeken voor de andere resources die zijn gedefinieerd in deze sjabloon.  Vergelijk de resourcedefinities met de verwijzingen.
5. Vouw de vierde resource uit:

    ![Azure Resource Manager-sjablonen in Visual Studio Code: dependson](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependson.png)

    Met het element dependsOn kunt u één resource als afhankelijk van een of meer resources definiëren. De resource in dit voorbeeld is een networkInterface.  Deze is afhankelijk van twee andere resources:

    * publicIPAddress
    * virtualNetwork

6. Vouw de vijfde resource uit. Deze resource is een virtuele machine. Deze is afhankelijk van twee andere resources:

    * storageAccount
    * networkInterface

Het volgende diagram illustreert de resources en de afhankelijkheidsgegevens voor deze sjabloon:

![Azure Resource Manager-sjablonen in Visual Studio Code: afhankelijkheidsdiagram](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Door de afhankelijkheden op te geven, kan Resource Manager de oplossing efficiënt implementeren. Het implementeert tegelijk het opslagaccount, een openbaar IP-adres en een virtueel netwerk omdat ze geen afhankelijkheden hebben. Nadat het openbare IP-adres en het virtuele netwerk zijn geïmplementeerd, wordt de netwerkinterface gemaakt. Wanneer alle andere resources zijn geïmplementeerd, implementeert Resource Manager de virtuele machine.

## <a name="edit-the-parameters-file"></a>Het parameterbestand bewerken

U hoeft het sjabloonbestand niet te wijzigen. U moet echter het parameterbestand bewerken om het beheerderswachtwoord uit de sleutelkluis op te halen.

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
    Vervang de **id** door de resource-id van de sleutelkluis die u in de afgelopen procedure hebt gemaakt. Dit is een van de zaken die wordt uitgevoerd. 

    ![Key Vault en Resource Manager-sjabloon integreren - virtuele-machine-implementatie - parameterbestand](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Geef waarden op voor:

    - **adminUsername**: geef het beheerdersaccount van de virtuele machine een naam.
    - **dnsLabelPrefix**: geef de dnsLablePrefix een naam.
4. Sla de wijzigingen op.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Er bestaan meerdere methoden voor het implementeren van sjablonen.  In deze zelfstudie gebruikt u Cloud Shell van Azure Portal.

1. Meld u aan bij [Cloud Shell](https://shell.azure.com). U kunt zich ook aanmelden bij de [Azure-portal](https://portal.azure.com) en dan in de rechterbovenhoek **Cloud Shell** selecteren, zoals weergegeven in de volgende afbeelding:

    ![Cloud Shell in Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
2. Selecteer **PowerShell** in de linkerbovenhoek van Cloud Shell en selecteer **Bevestigen**.  In deze zelfstudie gebruikt u PowerShell.
3. Selecteer **Bestand uploaden** vanuit Cloud Shell:

    ![Bestand uploaden in Cloud Shell in Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
4. Selecteer de bestanden die u eerder in de zelfstudie hebt opgeslagen. De standaardnamen zijn **azuredeploy.json** en **azuredeploy.paraemters.json**.  Als u bestanden met dezelfde bestandsnamen hebt, worden de oude bestanden zonder melding overschreven.
5. Voer vanaf Cloud Shell de volgende opdracht uit om te verifiëren of het bestand is geüpload. 

    ```bash
    ls
    ```

    ![Cloud Shell-lijstbestand in Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    De bestandsnaam die in de schermafbeelding wordt weergegeven is azuredeploy.json.

6. Voer vanaf Cloud Shell de volgende opdracht uit om de inhoud van het JSON-bestand te verifiëren:

    ```bash
    cat azuredeploy.json
    cat azuredeploy.parameters.json
    ```
7. Voer vanuit Cloud Shell de volgende PowerShell-opdrachten uit. In het voorbeeldscript wordt gebruikgemaakt van dezelfde resourcegroep als de groep die u voor de sleutelkluis hebt gemaakt. Door dezelfde resourcegroep te gebruiken, is het eenvoudiger om de resources op te schonen.

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $deploymentName = "<Enter a deployment name>"

    New-AzureRmResourceGroupDeployment -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json `
        -TemplateparameterFile azuredeploy.parameters.json
    ```
8. Voer de volgende PowerShell-opdracht uit om de nieuwe virtuele machine weer te geven:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName $resourceGroupName
    ```

    De naam van de virtuele machine is in code vastgelegd als **SimpleWinVM** in de sjabloon.

9. Meld u aan bij de virtuele machine om de referenties van de beheerder te testen. 

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie ontwikkelt en implementeert u een sjabloon voor het maken van een virtuele machine, een virtueel netwerk en de afhankelijke resources. Informatie over het implementeren van Azure-resources op basis van voorwaarden vindt u in:


> [!div class="nextstepaction"]
> [Voorwaarden gebruiken](./resource-manager-tutorial-use-conditions.md)


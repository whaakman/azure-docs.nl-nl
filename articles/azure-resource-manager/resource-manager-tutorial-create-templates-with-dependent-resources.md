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
ms.date: 07/20/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: bd559cb9f0140706a4b9735c642367e03616a14d
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188162"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Zelfstudie: Azure Resource Manager-sjablonen met afhankelijke resources maken

Leer hoe u een Azure Resource Manager-sjabloon maakt voor het implementeren van meerdere resources.  Nadat u de sjabloon hebt gemaakt, kunt u de sjabloon implementeren met behulp van de Cloud Shell van Azure Portal.

Sommige resources kunnen niet worden geïmplementeerd totdat er een andere resource bestaat. U kunt bijvoorbeeld niet een virtuele machine maken totdat het bijbehorende opslagaccount en de bijbehorende netwerkinterface bestaan. U definieert deze relatie door een resource afhankelijk van de andere resources te maken. Resource Manager evalueert de afhankelijkheden tussen resources en implementeert ze in de volgorde van afhankelijkheid. Als resources niet van elkaar afhankelijk zijn, worden deze door Resource Manager parallel geïmplementeerd. Zie [Define the order for deploying resources in Azure Resource Manager Templates](./resource-group-define-dependencies.md) (De volgorde voor het implementeren van resources definiëren in Azure Resource Manager-sjablonen) voor meer informatie.

> [!div class="checklist"]
> * Een snelstartsjabloon openen
> * De sjabloon verkennen
> * De sjabloon implementeren
> * Resources opschonen

Met de instructies in deze zelfstudie maakt u een virtuele machine, een virtueel netwerk en andere afhankelijke resources. 

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/).
* Extensie voor Azure Resource Manager-hulpprogramma's.  Zie [De extensie installeren](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen. In plaats van een sjabloon helemaal vanaf de basis te maken, kunt u een voorbeeldsjabloon zoeken en aanpassen. De sjabloon die in deze zelfstudie wordt gebruikt, heet [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Een eenvoudige Windows-VM implementeren).

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.
4. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.

## <a name="explore-the-template"></a>De sjabloon verkennen

1. Vouw in Visual Studio Code de elementen samen totdat u alleen de elementen op het eerste niveau en tweede niveau binnen **resources** ziet:

    ![Azure Resource Manager-sjablonen in Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    Er worden vijf resources gedefinieerd door de sjabloon.
2. Vouw het vierde element uit:

    ![Azure Resource Manager-sjablonen in Visual Studio Code: dependson](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependson.png)

    Met het element dependsOn kunt u één resource als afhankelijk van een of meer resources definiëren. De resource in dit voorbeeld is een networkInterface.  Deze is afhankelijk van twee andere resources:

    * publicIPAddress
    * virtualNetwork

3. Vouw het vijfde element uit. Deze resource is een virtuele machine. Deze is afhankelijk van twee andere resources:

    * storageAccount
    * networkInterface

Het volgende diagram illustreert de resources en de afhankelijkheidsgegevens voor deze sjabloon:

![Azure Resource Manager-sjablonen in Visual Studio Code: afhankelijkheidsdiagram](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Door de afhankelijkheden op te geven, kan Resource Manager de oplossing efficiënt implementeren. Het implementeert tegelijk het opslagaccount, een openbaar IP-adres en een virtueel netwerk omdat ze geen afhankelijkheden hebben. Nadat het openbare IP-adres en het virtuele netwerk zijn geïmplementeerd, wordt de netwerkinterface gemaakt. Wanneer alle andere resources zijn geïmplementeerd, implementeert Resource Manager de virtuele machine.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Er bestaan meerdere methoden voor het implementeren van sjablonen.  In deze zelfstudie gebruikt u Cloud Shell van Azure Portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com)
2. Selecteer **Cloud Shell** in de rechterbovenhoek, zoals weergegeven in de volgende afbeelding:

    ![Cloud Shell in Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
3. Selecteer **PowerShell** in de linkerbovenhoek van Cloud Shell.  In deze zelfstudie gebruikt u PowerShell.
4. Selecteer **Opnieuw starten**
5. Selecteer **Bestand uploaden** vanuit Cloud Shell:

    ![Bestand uploaden in Cloud Shell in Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
6. Selecteer het bestand dat u eerder in de zelfstudie hebt opgeslagen. De standaardnaam is **azuredeploy.json**.  Als u een bestand met dezelfde bestandsnaam hebt, wordt het oude bestand zonder melding overschreven.
7. Voer vanaf Cloud Shell de volgende opdracht uit om te verifiëren of het bestand is geüpload. 

    ```shell
    ls
    ```

    ![Cloud Shell-lijstbestand in Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    De bestandsnaam die in de schermafbeelding wordt weergegeven is azuredeploy.json.

8. Voer vanaf Cloud Shell de volgende opdracht uit om de inhoud van het JSON-bestand te verifiëren:

    ```shell
    cat azuredeploy.json
    ```
9. Voer vanuit Cloud Shell de volgende PowerShell-opdrachten uit:

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $location = "<Enter the Azure location>"
    $vmAdmin = "<Enter the admin username>"
    $vmPassword = "<Enter the password>"
    $dnsLabelPrefix = "<Enter the prefix>"

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    $vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
    New-AzureRmResourceGroupDeployment -Name mydeployment0710 -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json -adminUsername $vmAdmin -adminPassword $vmPW `
        -dnsLabelPrefix $dnsLabelPrefix
    ```
    Dit is de schermafbeelding voor een voorbeeldimplementatie:

    ![Sjabloon implementeren in Cloud Shell in Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-deploy-template.png)

    In de schermafbeelding worden de volgende waarden gebruikt:

    * **$resourceGroupName**: myresourcegroup0710. 
    * **$location**: eastus2
    * **&lt;DeployName>**: mydeployment0710
    * **&lt;TemplateFile>**: azuredeploy.json
    * **Template parameter**s:

        * **adminUsername**: JohnDole
        * **adminPassword**: Pass@word123
        * **dnsLabelPrefix**: myvm0710

10. Voer de volgende PowerShell-opdracht uit om de nieuwe virtuele machine weer te geven:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName <ResourceGroupName>
    ```

    De naam van de virtuele machine is in code vastgelegd als **SimpleWinVM** in de sjabloon.

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie ontwikkelt en implementeert u een sjabloon voor het maken van een virtuele machine, een virtueel netwerk en de afhankelijke resources. Zie [Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md) voor meer informatie over sjablonen.
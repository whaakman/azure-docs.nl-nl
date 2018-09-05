---
title: Visual Studio Code gebruiken om Azure Resource Manager-sjablonen te maken | Microsoft Docs
description: Gebruik Visual Studio Code en de Azure Resource Manager-extensie voor hulpprogramma's om te werken met Resource Manager-sjablonen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 08/24/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 540aabc9164e43776d2166926430f4512dd23f49
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43106046"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>Snelstart: Azure Resource Manager-sjablonen maken met Visual Studio Code

Leer hoe u Azure Resource Manager-sjablonen maakt met behulp van Visual Studio Code en de extensie voor Azure Resource Manager-hulpprogramma's. U kunt Resource Manager-sjablonen maken in Visual Studio Code zonder de extensie, maar de extensie biedt opties voor automatisch aanvullen die het ontwikkelen van sjablonen eenvoudiger maken. Zie [Overzicht van Azure Resource Manager](resource-group-overview.md) voor inzicht in de concepten die gerelateerd zijn aan het implementeren en beheren van uw Azure-oplossingen.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

- [Visual Studio Code](https://code.visualstudio.com/).
- Extensie voor Azure Resource Manager-hulpprogramma's. Gebruik de volgende stappen om deze te installeren:

    1. Open Visual Studio Code.
    2. Druk op **Ctrl+Shift+X** om het deelvenster Extensies te openen
    3. Zoek **Azure Resource Manager-hulpprogramma’s** en selecteer **Installeren**.
    4. Selecteer **Opnieuw laden** om de installatie van de extensie te voltooien.

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

In plaats van een sjabloon helemaal opnieuw te maken, opent u een sjabloon in [Azure-snelstartsjablonen](https://azure.microsoft.com/resources/templates/). Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen.

De in deze snelstart gebruikte sjabloon wordt [Create a standard storage account](https://azure.microsoft.com/resources/templates/101-storage-account-create/) (Standaardopslagaccount maken) genoemd. De sjabloon definieert een Azure Storage-accountresource.

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.
4. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.

## <a name="edit-the-template"></a>De sjabloon bewerken

Als u wilt weten hoe u een sjabloon met behulp van Visual Studio Code bewerkt, voegt u een extra element toe aan de uitvoersectie.

1. Voeg in Visual Studio Code een extra uitvoer toe aan de geëxporteerde sjabloon:

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    Als u klaar bent, ziet de uitvoersectie er als volgt uit:

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    Als u de code in Visual Studio Code hebt geplakt, kunt u het element **value** opnieuw typen om een idee te krijgen van de intellisense-mogelijkheden van de extensie voor Resource Manager-hulpprogramma's.

    ![Visual Studio Code-intellisense van Resource Manager-sjabloon](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Selecteer **Bestand**>**Opslaan** om het bestand op te slaan.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Er bestaan meerdere methoden voor het implementeren van sjablonen.  In deze snelstart gebruikt u Azure Cloud Shell van Azure Portal. Cloud Shell ondersteunt zowel Azure CLI als Azure PowerShell. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com)
2. Selecteer **Cloud Shell** in de rechterbovenhoek, zoals weergegeven in de volgende afbeelding:

    ![Cloud Shell in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell.png)

    Cloud Shell wordt geopend onderaan het scherm.

3. Linksboven in Cloud Shell wordt **PowerShell** of **Bash** weergegeven. Als u CLI wilt gebruiken, moet u een Bash-sessie openen. Als u PowerShell wilt uitvoeren, moet u een PowerShell-sessie openen. Als u wilt overschakelen, selecteert u de pijl-omlaag en selecteert u de interpreter. In de volgende afbeelding wordt het overschakelen van PowerShell naar Bash weergegeven.

    ![Cloud Shell CLI in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)

    U moet de shell opnieuw starten wanneer u overschakelt.
4. Selecteer **Upload/download files** en selecteer **Uploaden**.

    ![Bestand uploaden in Cloud Shell in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)

    U moet het sjabloonbestand uploaden voordat u het vanuit de shell kunt implementeren.
5. Selecteer het bestand dat u eerder in de snelstart hebt opgeslagen. De standaardnaam is **azuredeploy.json**.
6. Voer vanaf Cloud Shell de opdracht **ls** uit om te verifiëren of het bestand is geüpload. U kunt de opdracht **cat** gebruiken om de sjablooninhoud te verifiëren. In de volgende afbeelding ziet u dat de opdracht wordt uitgevoerd vanuit Bash.  U gebruikt dezelfde opdrachten van een PowerShell-sessie.

    ![Cloud Shell-lijstbestand in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)
7. Voer vanuit Cloud Shell de volgende opdrachten uit. Selecteer het tabblad om de PowerShell-code of de CLI-code weer te geven.

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```cli
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file <TemplateFileName>
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```powershell
    New-AzureRmResourceGroup -Name <ResourceGroupName> -Location <AzureLocation>

    New-AzureRmResourceGroupDeployment -ResourceGroupName <ResourceGroupName> -TemplateFile <TemplateFileName>
    ```
    
    ---

    In de volgende schermafbeelding ziet u een voorbeeld-CLI-implementatie:

    ![Sjabloon implementeren in Cloud Shell in Azure Portal](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    In de schermafbeelding worden de volgende waarden gebruikt:

    - **&lt;ResourceGroupName>**: myresourcegroup0709. De parameter komt tweemaal voor.  Zorg ervoor dat u dezelfde waarde gebruikt.
    - **&lt;AzureLocation>**: eastus2
    - **&lt;DeployName>**: mydeployment0709
    - **&lt;TemplateFile>**: azuredeploy.json

    In de uitvoer op de schermafbeelding ziet u dat de naam van het opslagaccount *3tqebj3slyfyestandardsa* is. 

7. Voer de volgende CLI- of PowerShell-opdracht uit om het nieuwe opslagaccount weer te geven:

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```
   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    
    ```powershell
    Get-AzureRmStorageAccount -ResourceGroupName <ResourceGroupName> -Name <StorageAccountName>
    ```
    
    ---

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

De primaire focus van deze zelfstudie is het gebruik van Visual Studio Code om een bestaande sjabloon van de Azure-snelstartsjablonen te bewerken. U hebt ook geleerd hoe u de sjabloon kunt implementeren met CLI of PowerShell vanuit Azure Cloud Shell. De sjablonen van de Azure-snelstartsjablonen voldoen mogelijk niet volledig aan uw behoeften. In de volgende zelfstudie leert u hoe u de informatie van de sjabloonverwijzing kunt vinden, zodat u een versleuteld Azure-opslagaccount kunt maken.

> [!div class="nextstepaction"]
> [Een versleuteld opslagaccount maken](./resource-manager-tutorial-create-encrypted-storage-accounts.md)

---
title: Meerdere resource-instanties maken met Azure Resource Manager | Microsoft Docs
description: Leer hoe u een Azure Resource Manager-sjabloon maakt om meerdere resource-instanties te maken.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/18/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 1cfccdf644b1748a96f7638e574c66eace8d113a
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456657"
---
# <a name="tutorial-create-multiple-resource-instances-using-resource-manager-templates"></a>Zelfstudie: Meerdere resource-instanties maken met Resource Manager-sjablonen

Leer hoe u een Azure Resource Manager-sjabloon herhaaldelijk gebruikt om meerdere instanties van een Azure-resource te maken. In de laatste zelfstudie hebt u een bestaande sjabloon gewijzigd om een versleuteld Azure Storage-account te maken. In deze zelfstudie wijzigt u dezelfde sjabloon om drie instanties van het opslagaccount te maken.

> [!div class="checklist"]
> * Een snelstartsjabloon openen
> * De sjabloon bewerken
> * De sjabloon implementeren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/) met de [extensie Resource Manager Tools](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

De in deze snelstart gebruikte sjabloon wordt [Create a standard storage account](https://azure.microsoft.com/resources/templates/101-storage-account-create/) (Standaardopslagaccount maken) genoemd. De sjabloon definieert een Azure Storage-accountresource.

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.
4. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.

## <a name="edit-the-template"></a>De sjabloon bewerken

Het doel van deze zelfstudie is het gebruik van resource-iteratie om drie opslagaccounts te maken.  Met de voorbeeldsjabloon wordt slechts één opslagaccount gemaakt. 

Breng vanuit Visual Studio Code de volgende vier wijzigingen aan:

![Azure Resource Manager meerdere instanties maken](./media/resource-manager-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Voeg een `copy`-element toe aan de resourcedefinitie van het opslagaccount. In het copy-element geeft u het aantal iteraties en een naam voor deze lus op. Het aantal iteraties moet een positief geheel getal zijn en mag niet hoger zijn dan 800.
2. De functie `copyIndex()` retourneert de huidige iteratie in de lus. `copyIndex()` is gebaseerd op nul. Als u de indexwaarde wilt verschuiven, kunt u een waarde doorgeven in de functie copyIndex(). Bijvoorbeeld *copyIndex(1)*.
3. Verwijder het element **variables**, want dit wordt niet meer gebruikt.
4. Verwijder het element **outputs**.

De voltooide sjabloon ziet er als volgt uit:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "apiVersion": "2018-02-01",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ]
}
```

Voor meer informatie over het maken van meerdere instanties raadpleegt u [Deploy multiple instances of a resource or property in Azure Resource Manager Templates](./resource-group-create-multiple.md) (Meerdere instanties van een resource of eigenschap implementeren in Azure Resource Manager-sjablonen)

## <a name="deploy-the-template"></a>De sjabloon implementeren

Raadpleeg de sectie [De sjabloon implementeren](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) in de snelstartgids van Visual Studio Code voor de implementatieprocedure.

Als u alle drie de opslagaccounts wilt weergeven, laat u de parameter --name weg:

# <a name="clitabcli"></a>[CLI](#tab/CLI)
```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az storage account list --resource-group $resourceGroupName
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName
```

---

Vergelijk de namen van de opslagaccounts met de naamdefinitie in de sjabloon.

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u meerdere instanties van opslagaccounts maakt. Tot nu toe hebt u één opslagaccount of meerdere instanties van een opslagaccount gemaakt. In de volgende zelfstudie ontwikkelt u een sjabloon met meerdere resources en meerdere resourcetypen. Sommige resources hebben afhankelijke resources.

> [!div class="nextstepaction"]
> [Afhankelijke resources maken](./resource-manager-tutorial-create-templates-with-dependent-resources.md)

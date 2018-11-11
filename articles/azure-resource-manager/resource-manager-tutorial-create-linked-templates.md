---
title: Gekoppelde Azure Resource Manager-sjablonen maken | Microsoft Docs
description: Informatie over het maken van gekoppelde Azure Resource Manager-sjablonen voor het maken van een virtuele machine
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/30/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: b08013941c1cf83b3eb006543d699eb7e1356ff0
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239981"
---
# <a name="tutorial-create-linked-azure-resource-manager-templates"></a>Zelfstudie: Azure Resource Manager-sjablonen maken

Informatie over het maken van gekoppelde Azure Resource Manager-sjablonen. Met gekoppelde sjablonen kunt u een sjabloon een andere sjabloon laten aanroepen. Dit is handig om sjablonen te modulariseren. In deze zelfstudie gebruikt u dezelfde sjabloon als in [Zelfstudie: Meerdere resource-instanties maken met Resource Manager-sjablonen](./resource-manager-tutorial-create-multiple-instances.md). Deze sjabloon maakt een virtuele machine, een virtueel netwerk en andere afhankelijke resources, waaronder een opslagaccount. U verplaatst de resource van het opslagaccount naar een gekoppelde sjabloon.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een snelstartsjabloon openen
> * De gekoppelde sjabloon maken
> * De gekoppelde sjabloon uploaden
> * Koppeling maken met de gekoppelde sjabloon
> * Afhankelijkheid configureren
> * De sjabloon implementeren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/) met de [extensie Resource Manager Tools](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Voor een verbeterde beveiliging gebruikt u een gegenereerd wachtwoord voor het beheerdersaccount van de virtuele machine. Hier volgt een voorbeeld voor het genereren van een wachtwoord:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault is ontworpen om cryptografische sleutels en andere geheimen te beveiligen. Zie [Zelfstudie: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie](./resource-manager-tutorial-use-key-vault.md) voor meer informatie. We raden u ook aan om uw wachtwoord elke drie maanden te wijzigen.

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen. In plaats van een sjabloon helemaal vanaf de basis te maken, kunt u een voorbeeldsjabloon zoeken en aanpassen. De sjabloon die in deze zelfstudie wordt gebruikt, heet [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Een eenvoudige Windows-VM implementeren). Dit is de sjabloon die ook is gebruikt in [Zelfstudie: Meerdere resource-instanties maken met Resource Manager-sjablonen](./resource-manager-tutorial-create-multiple-instances.md). U slaat twee kopieën op van dezelfde sjabloon. Deze worden gebruikt als:

* **De hoofdsjabloon**: voor het maken van alle resources, met uitzondering van het opslagaccount.
* **De gekoppelde sjabloon**: voor het maken van het opslagaccount.

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.
4. Er worden vijf resources gedefinieerd door de sjabloon:

    * `Microsoft.Storage/storageAccounts`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts). 
    * `Microsoft.Network/publicIPAddresses`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses). 
    * `Microsoft.Network/virtualNetworks`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks). 
    * `Microsoft.Network/networkInterfaces`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces). 
    * `Microsoft.Compute/virtualMachines`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Het is handig om enige basiskennis te hebben van de sjabloon voordat u deze gaat aanpassen.
5. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.
6. Selecteer **Bestand**>**Opslaan als** om nog een exemplaar te maken van het bestand met de naam **linkedTemplate.json**.

## <a name="create-the-linked-template"></a>De gekoppelde sjabloon maken

De gekoppelde sjabloon maakt een opslagaccount. De gekoppelde sjabloon is bijna identiek aan de zelfstandige sjabloon die een opslagaccount maakt. In deze zelfstudie moet de gekoppelde sjabloon een waarde teruggeven aan de hoofdsjabloon. Deze waarde is gedefinieerd in het element `outputs`.

1. Open linkedTemplate.json in Visual Studio Code als het bestand nog niet is geopend.
2. Breng de volgende wijzigingen aan:

    * Verwijder alle resources behalve het opslagaccount. U moet in totaal vier resources verwijderen.
    * Werk het element **outputs** zo bij dat het er als volgt uitziet:

        ```json
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
        ```
        **storageUri** is vereist voor de resourcedefinitie van de virtuele machine in de hoofdsjabloon.  U kunt de waarde als een uitvoerwaarde teruggeven aan de hoofdsjabloon.
    * Verwijder de parameters die nooit worden gebruikt. Deze parameters zijn onderstreept met een groene golvende lijn. U houdt nog één parameter over, de parameter **locatie**.
    * Verwijder het element **variables**. Ze zijn in deze zelfstudie niet nodig.
    * Voeg een parameter met de naam **storageAccountName** toe. De naam van het opslagaccount wordt als een parameter van de hoofdsjabloon doorgegeven aan de gekoppelde sjabloon.

    Wanneer u klaar bent, ziet de sjabloon er als volgt uit:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountName":{
            "type": "string",
            "metadata": {
              "description": "Azure Storage account name."
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
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
          }
        ],
        "outputs": {
            "storageUri": {
                "type": "string",
                "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
              }
        }
    }
    ```
3. Sla de wijzigingen op.

## <a name="upload-the-linked-template"></a>De gekoppelde sjabloon uploaden

De sjablonen moeten toegankelijk zijn vanaf de locatie waar u de implementatie uitvoert. Dit kan een Azure-opslagaccount, Github of Dropbox zijn. Als uw sjablonen gevoelige informatie bevatten, zorg er dan voor dat u de toegang tot deze gegevens beveiligt. In deze zelfstudie gebruikt u de implementatiemethode van de Cloud-shell die u ook hebt gebruikt in [Zelfstudie: Meerdere resource-instanties maken met Resource Manager-sjablonen](./resource-manager-tutorial-create-multiple-instances.md). De hoofdsjabloon (azuredeploy.json)-sjabloon is geüpload naar de shell. De gekoppelde sjabloon (linkedTemplate.json) moet ergens worden gedeeld.  Om de taken van deze zelfstudie te beperken, is de gekoppelde sjabloon die in de vorige sectie is gedefinieerd, al geüpload naar een [Azure-opslagaccount](https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json).

## <a name="call-the-linked-template"></a>De gekoppelde sjabloon aanroepen

De hoofdsjabloon heet azuredeploy.json.

1. Open azuredeploy.json in Visual Studio Code als het bestand nog niet is geopend.
2. Verwijder de resourcedefinitie van het opslagaccount uit de sjabloon.
3. Voeg het volgende json-codefragment toe aan de locatie waar u de definitie van het opslagaccount hebt neergezet:

    ```json
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri":"https://armtutorials.blob.core.windows.net/linkedtemplates/linkedStorageAccount.json"
          },
          "parameters": {
              "storageAccountName":{"value": "[variables('storageAccountName')]"},
              "location":{"value": "[parameters('location')]"}
          }
      }
    },
    ```

    Let op de volgende gegevens:

    * Een `Microsoft.Resources/deployments`-resource in de hoofdsjabloon wordt gebruikt om de sjabloon te koppelen aan een andere sjabloon.
    * De `deployments`-resource heet `linkedTemplate`. Deze naam wordt gebruikt voor [het configureren van afhankelijkheden](#configure-dependency).  
    * U kunt alleen de [incrementele](./deployment-modes.md) implementatiemodus gebruiken bij het aanroepen van gekoppelde sjablonen.
    * `templateLink/uri` bevat de URI van de gekoppelde sjabloon. De gekoppelde sjabloon is geüpload naar een gedeeld opslagaccount. U kunt de URI bijwerken als u de sjabloon uploadt naar een andere locatie op internet.
    * Gebruik `parameters` om waarden door te geven van de hoofdsjabloon naar de gekoppelde sjabloon.
4. Sla de wijzigingen op.

## <a name="configure-dependency"></a>Afhankelijkheid configureren

In de [Zelfstudie: Meerdere resource-instanties maken met Resource Manager-sjablonen](./resource-manager-tutorial-create-multiple-instances.md) is uitgelegd dat de VM-resource afhankelijk is van het opslagaccount:

![Afhankelijkheidsdiagram van Azure Resource Manager-sjablonen](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-visual-studio-code-dependency-diagram.png)

Omdat het opslagaccount nu is gedefinieerd in de gekoppelde sjabloon, moet u de volgende twee elementen van de `Microsoft.Compute/virtualMachines`-resource bijwerken.

* Configureer het `dependOn`-element opnieuw. De definitie van het opslagaccount is verplaatst naar de gekoppelde sjabloon.
* Configureer het `properties/diagnosticsProfile/bootDiagnostics/storageUri`-element opnieuw. In [De gekoppelde sjabloon maken](#create-the-linked-template) hebt u een uitvoerwaarde toegevoegd:

    ```json
    "outputs": {
        "storageUri": {
            "type": "string",
            "value": "[reference(parameters('storageAccountName')).primaryEndpoints.blob]"
            }
    }
    ```
    Deze waarde heeft de hoofdsjabloon nodig.

1. Open azuredeploy.json in Visual Studio Code als het bestand nog niet is geopend.
2. Vouw de definitie van de VM-resource uit en werk **dependsOn** bij zoals wordt weergegeven in de volgende schermafbeelding:

    ![Gekoppelde Azure Resource Manager-sjablonen configureren afhankelijkheid ](./media/resource-manager-tutorial-create-linked-templates/resource-manager-template-linked-templates-configure-dependency.png)

    *linkedTemplate* is de naam van de implementatieresource.  
3. Werk **properties/diagnosticsProfile/bootDiagnostics/storageUri** bij zoals is weergegeven in de vorige schermafbeelding.

Voor meer informatie raadpleegt u [Use linked and nested templates when deploying Azure resources](./resource-group-linked-templates.md) (Gekoppelde en geneste sjablonen gebruiken bij het implementeren van Azure-resources)

## <a name="deploy-the-template"></a>De sjabloon implementeren

Raadpleeg de sectie [De sjabloon implementeren](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) voor de implementatieprocedure. Voor een verbeterde beveiliging gebruikt u een gegenereerd wachtwoord voor het beheerdersaccount van de virtuele machine. Zie [Vereisten](#prerequisites).

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een gekoppelde sjabloon ontwikkeld en geïmplementeerd. Voor meer informatie over het gebruik van extensies van virtuele machines voor de uitvoering van post-implementatietaken raadpleegt u

> [!div class="nextstepaction"]
> [Extensies van virtuele machines implementeren](./deployment-manager-tutorial.md)

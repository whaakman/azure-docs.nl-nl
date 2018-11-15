---
title: SQL BACPAC-bestanden importeren met Azure Resource Manager-sjablonen | Microsoft Docs
description: Meer informatie over het gebruik van SQL Database-extensie om SQL BACPAC-bestanden te importeren met Azure Resource Manager-sjablonen
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 6349f3e6a30ce1b7a3162f54056901313327a101
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612534"
---
# <a name="tutorial-import-sql-bacpac-files-with-azure-resource-manager-templates"></a>Zelfstudie: SQL BACPAC-bestanden importeren met Azure Resource Manager-sjablonen

Leer hoe u Azure SQL Database-extensies gebruikt om een BACPAC-bestand te importeren. In deze zelfstudie maakt u een sjabloon voor het implementeren van een Azure SQL Server, een SQL Database en een BACPAC-bestand. Zie [# Zelfstudie: Extensies voor virtuele machines implementeren met Azure Resource Manager-sjablonen](./resource-manager-tutorial-deploy-vm-extensions.md) voor informatie over het implementeren van extensies van virtuele Azure-machines met behulp van Azure Resource Manager-sjablonen.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een BACPAC-bestand voorbereiden
> * Een snelstartsjabloon openen
> * De sjabloon bewerken
> * De sjabloon implementeren
> * De implementatie controleren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/) met de extensie Resource Manager Tools. Zie [De extensie installeren](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Gebruik een gegenereerd wachtwoord voor het beheerdersaccount van SQL Server om de beveiliging te verbeteren. Hier volgt een voorbeeld voor het genereren van een wachtwoord:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault is ontworpen om cryptografische sleutels en andere geheimen te beveiligen. Zie [Zelfstudie: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie](./resource-manager-tutorial-use-key-vault.md) voor meer informatie. We raden u ook aan om uw wachtwoord elke drie maanden te wijzigen.

## <a name="prepare-a-bacpac-file"></a>Een BACPAC-bestand voorbereiden

Een BACPAC-bestand wordt gedeeld in een [Azure Storage-account met openbare toegang](https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac). Zie [Een Azure SQL-database exporteren naar een BACPAC-bestand](../sql-database/sql-database-export.md) als u een eigen account wilt maken. Als u ervoor kiest het bestand naar uw eigen locatie te publiceren, moet u de sjabloon later in de zelfstudie bijwerken.

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen. In plaats van een sjabloon helemaal vanaf de basis te maken, kunt u een voorbeeldsjabloon zoeken en aanpassen. De sjabloon die in deze zelfstudie wordt gebruikt, heet [Een Azure SQL Server implementeren met Detectie van bedreigingen](https://azure.microsoft.com/resources/templates/201-sql-threat-detection-server-policy-optional-db/).

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-sql-threat-detection-server-policy-optional-db/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.

    Er worden drie resources gedefinieerd in de sjabloon:

    * `Microsoft.Sql/servers`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.sql/servers).
    * `Microsoft.SQL/servers/securityAlertPolicies`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/securityalertpolicies).
    * `Microsoft.SQL.servers/databases`.  Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases).
    Het is handig om enige basiskennis te hebben van de sjabloon voordat u deze gaat aanpassen.
4. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.

## <a name="edit-the-template"></a>De sjabloon bewerken

U moet twee extra resources toevoegen aan de sjabloon.

* Als u wilt toestaan dat de SQL database-extensie BACPAC-bestanden kan importeren, moet u toegang tot Azure-services toestaan. Voeg de volgende JSON toe aan de definitie van de SQL-server:

    ```json
    {
        "type": "firewallrules",
        "name": "AllowAllAzureIps",
        "location": "[parameters('location')]",
        "apiVersion": "2014-04-01",
        "dependsOn": [
            "[variables('databaseServerName')]"
        ],
        "properties": {
            "startIpAddress": "0.0.0.0",
            "endIpAddress": "0.0.0.0"
        }
    }
    ```

    De sjabloon zal er als volgt uitzien:

    ![Azure Resource Manager sql-extensies importeren BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-firewall.png)

* Voeg met de volgende JSON een SQL Database-extensieresource toe aan de databasedefinitie:

    ```json
    "resources": [
        {
            "name": "Import",
            "type": "extensions",
            "apiVersion": "2014-04-01",
            "dependsOn": [
                "[resourceId('Microsoft.Sql/servers/databases', variables('databaseServerName'), variables('databaseName'))]"
            ],
            "properties": {
                "storageKeyType": "SharedAccessKey",
                "storageKey": "?",
                "storageUri": "https://armtutorials.blob.core.windows.net/sqlextensionbacpac/SQLDatabaseExtension.bacpac",
                "administratorLogin": "[variables('databaseServerAdminLogin')]",
                "administratorLoginPassword": "[variables('databaseServerAdminLoginPassword')]",
                "operationMode": "Import",
            }
        }
    ]
    ```

    De sjabloon zal er als volgt uitzien:

    ![Azure Resource Manager sql-extensies importeren BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac.png)

    Zie de [verwijzing voor SQL Database-extensies](https://docs.microsoft.com/azure/templates/microsoft.sql/servers/databases/extensions) voor meer informatie over de resourcedefinitie. Hier volgen enkele belangrijke elementen:

    * **dependsOn**: de extensieresource moet worden gemaakt nadat de SQL-database is gemaakt.
    * **storageKeyType**: het type opslagsleutel dat moet worden gebruikt. De waarde kan `StorageAccessKey` of `SharedAccessKey` zijn. Omdat het opgegeven BACPAC-bestand in een Azure Storage-account met openbare toegang wordt gedeeld, wordt hier 'SharedAccessKey' gebruikt.
    * **storageKey**: de te gebruiken opslagsleutel. Als het type opslagsleutel SharedAccessKey is, moet deze worden voorafgegaan door een '?'.
    * **storageUri**: de te gebruiken uri. Als u ervoor kiest om het verstrekte BACPAC-bestand niet te gebruiken, moet u de waarden bijwerken.
    * **administratorLoginPassword**: het wachtwoord van de SQL-beheerder. Het wordt aanbevolen om een gegenereerd wachtwoord te gebruiken. Zie [Vereisten](#prerequisites).

## <a name="deploy-the-template"></a>De sjabloon implementeren

Raadpleeg de sectie [De sjabloon implementeren](./resource-manager-tutorial-create-multiple-instances.md#deploy-the-template) voor de implementatieprocedure. Gebruik in plaats daarvan het volgende PowerShell-implementatiescript:

```azurepowershell
$deploymentName = Read-Host -Prompt "Enter the name for this deployment"
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
$adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmResourceGroupDeployment -Name $deploymentName `
    -ResourceGroupName $resourceGroupName `
    -adminUser $adminUsername `
    -adminPassword $adminPassword `
    -TemplateFile azuredeploy.json
```

Het wordt aanbevolen om een gegenereerd wachtwoord te gebruiken. Zie [Vereisten](#prerequisites).

## <a name="verify-the-deployment"></a>De implementatie controleren

Selecteer in de portal de SQL-database in de geïmplementeerde resourcegroep. Selecteer **Query-editor (preview)** en voer vervolgens de beheerdersreferenties in. U ziet dat twee tabellen in de database worden geïmporteerd:

![Azure Resource Manager sql-extensies importeren BACPAC](./media/resource-manager-tutorial-deploy-sql-extensions-bacpac/resource-manager-tutorial-deploy-sql-extensions-bacpac-query-editor.png)

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een SQL Server en een SQL Database geïmplementeerd en een BACPAC-bestand geïmporteerd. Voor meer informatie over hoe u Azure-resources implementeert in meerdere regio's en hoe u veilige implementatiemethoden gebruikt, raadpleegt u

> [!div class="nextstepaction"]
> [Azure Deployment Manager gebruiken](./resource-manager-tutorial-deploy-vm-extensions.md)

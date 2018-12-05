---
title: Extensies voor virtuele machines implementeren met Azure Resource Manager-sjablonen | Microsoft Docs
description: Informatie over het implementeren van extensies voor virtuele machines met Azure Resource Manager-sjablonen
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
ms.openlocfilehash: 7e0b3fff0ed60d5eb77194e7f9081d35f2e38571
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52869634"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Zelfstudie: Extensies voor virtuele machines implementeren met Azure Resource Manager-sjablonen

Meer informatie over het gebruik van [extensies voor virtuele Azure-machines](../virtual-machines/extensions/features-windows.md) voor het uitvoeren van configuratie- en automatiseringstaken na de implementatie op virtuele Azure-machines. Er zijn veel verschillende VM-extensies beschikbaar voor gebruik met Azure-VM's. In deze zelfstudie implementeert u een aangepaste scriptextensie van een Resource Manager-sjabloon om een PowerShell-script uit te voeren op een Windows-VM.  Met het script wordt een webserver op de virtuele machine geïnstalleerd.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een PowerShell-script voorbereiden
> * Een quickstartsjabloon openen
> * De sjabloon bewerken
> * De sjabloon implementeren
> * De implementatie controleren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/) met de extensie Resource Manager Tools.  Zie [De extensie installeren](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Gebruik een gegenereerd wachtwoord voor het beheerdersaccount van de virtuele machine om de beveiliging te verhogen. Hier volgt een voorbeeld voor het genereren van een wachtwoord:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault is ontworpen om cryptografische sleutels en andere geheimen te beveiligen. Zie [Zelfstudie: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie](./resource-manager-tutorial-use-key-vault.md) voor meer informatie. We raden u ook aan om uw wachtwoord elke drie maanden te wijzigen.

## <a name="prepare-a-powershell-script"></a>Een PowerShell-script voorbereiden

Een PowerShell-script met de volgende inhoud wordt gedeeld via een [Azure Storage-account met openbare toegang](https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1):

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Als u ervoor kiest het bestand naar uw eigen locatie te publiceren, moet u het element [fileUri] in de sjabloon later in de zelfstudie bijwerken.

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen. In plaats van een sjabloon helemaal vanaf de basis te maken, kunt u een voorbeeldsjabloon zoeken en aanpassen. De sjabloon die in deze zelfstudie wordt gebruikt, heet [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Een eenvoudige Windows-VM implementeren).

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.
4. Er worden vijf resources gedefinieerd met de sjabloon:

    * `Microsoft.Storage/storageAccounts`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
    * `Microsoft.Network/publicIPAddresses`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
    * `Microsoft.Network/virtualNetworks`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
    * `Microsoft.Network/networkInterfaces`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
    * `Microsoft.Compute/virtualMachines`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Het is handig om enige basiskennis te hebben van de sjabloon voordat u deze gaat aanpassen.
5. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.

## <a name="edit-the-template"></a>De sjabloon bewerken

Voeg een VM-extensieresource toe aan de bestaande sjabloon met de volgende inhoud:

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(variables('vmName'),'/', 'InstallWebServer')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/',variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "autoUpgradeMinorVersion":true,
        "settings": {
            "fileUris": [
                "https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
        }
    }
}
```

Zie de [extensieverwijzing](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions) voor meer informatie over deze resourcedefinitie. Hier volgen enkele belangrijke elementen:

* **name**: omdat de extensiebron een onderliggende resource van het virtuele-machineobject is, moet de naam het voorvoegsel van de naam van de virtuele machine hebben. Zie [onderliggende resources](./resource-manager-templates-resources.md#child-resources).
* **dependsOn**: de extensiebron moet worden gemaakt nadat de virtuele machine is gemaakt.
* **fileUris**: dit zijn de locaties waar de scriptbestanden worden opgeslagen. Als u ervoor kiest om de meegeleverde locaties niet te gebruiken, moet u de waarden bijwerken.
* **commandToExecute**: dit is de opdracht om het script aan te roepen.  

## <a name="deploy-the-template"></a>De sjabloon implementeren

Raadpleeg de sectie [De sjabloon implementeren](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) voor de implementatieprocedure. Het wordt aanbevolen een gegenereerd wachtwoord te gebruiken voor het beheerdersaccount van de virtuele machine. Zie [Vereisten](#prerequisites).

## <a name="verify-the-deployment"></a>De implementatie controleren

Selecteer de VM in de portal en gebruik in het overzicht van de VM **Klik om te kopiëren** rechts van het IP-adres om het adres te kopiëren. Plak het adres vervolgens in een tabblad van de browser. De standaardwelkomstpagina van IIS wordt geopend en deze ziet er als volgt uit:

![Azure Resource Manager implementeert vm-extensies klantscript IIS-webserver](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een virtuele machine en een VM-extensie. Door de extensie is de IIS-webserver op de virtuele machine geïnstalleerd. Zie voor meer informatie over het gebruik van de SQL Database-extensie om een BACPAC-bestand te importeren:

> [!div class="nextstepaction"]
> [](./resource-manager-tutorial-deploy-vm-extensions.md)

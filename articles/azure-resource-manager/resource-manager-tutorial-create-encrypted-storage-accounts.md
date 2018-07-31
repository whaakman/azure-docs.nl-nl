---
title: Een Azure Resource Manager-sjabloon maken voor het implementeren van een versleuteld opslagaccount | Microsoft Docs
description: Visual Studio Code gebruiken om een sjabloon voor het implementeren van een versleuteld opslagaccount te maken.
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
ms.openlocfilehash: 7c78636a210ae90c5bfe1d0bfd35e4e05633f5cd
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188196"
---
# <a name="tutorial-create-an-azure-resource-manager-template-for-deploying-an-encrypted-storage-account"></a>Zelfstudie: een Azure Resource Manager-sjabloon maken voor het implementeren van een versleuteld opslagaccount

Uitleg over hoe u meer informatie kunt vinden om een Azure Resource Manager-sjabloon te voltooien.

In deze zelfstudie gebruikt u een basissjabloon uit Azure-snelstartsjablonen om een Azure Storage-account te maken.  U past de basissjabloon door middel van referentiemateriaal voor sjablonen aan om een versleuteld opslagaccount te maken.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een snelstartsjabloon openen
> * Inzicht in de sjabloonindeling
> * Parameters in een sjabloon gebruiken
> * Variabelen in een sjabloon gebruiken
> * De sjabloon bewerken
> * De sjabloon implementeren

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/).
* Extensie voor Azure Resource Manager-hulpprogramma's. Als u deze wilt installeren, gaat u naar [Install the Resource Manager Tools extension](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites) (Extensie voor Resource Manager-hulpprogramma's installeren).

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

De in deze snelstart gebruikte sjabloon wordt [Create a standard storage account](https://azure.microsoft.com/resources/templates/101-storage-account-create/) (Standaardopslagaccount maken) genoemd. De sjabloon definieert een Azure Storage-accountresource.

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen.
4. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.

## <a name="understand-the-format"></a>Inzicht in de indeling

Vouw vanuit VS Code de sjabloon samen naar het hoofdniveau. U hebt de eenvoudigste structuur met de volgende elementen:

![Eenvoudigste structuur voor Resource Manager-sjabloon](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-simplest-structure.png)

* **$schema**: geef de locatie van het JSON-schemabestand op dat de versie van de sjabloontaal beschrijft.
* **contentVersion**: geef een waarde op voor dit element om belangrijke wijzigingen in de sjabloon vast te leggen.
* **parameters**: geef de waarden op die worden geleverd wanneer de implementatie wordt uitgevoerd om resource-implementatie aan te passen.
* **variables**: geef de waarden op die worden gebruikt als JSON-fragmenten in de sjabloon voor het vereenvoudigen van sjabloontaalexpressies.
* **resources**: geef de resourcetypen op die worden geïmplementeerd of bijgewerkt in een resourcegroep.
* **outputs**: geef de waarden op die worden geretourneerd na de implementatie.

## <a name="use-parameters-in-template"></a>Parameters in een sjabloon gebruiken

Met parameters kunt u de implementatie aanpassen door waarden op te geven die voor een specifieke omgeving zijn aangepast. U gebruikt de parameters die in de sjabloon zijn gedefinieerd bij het instellen van waarden voor het opslagaccount.

![Parameters voor Resource Manager-sjablonen](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-parameters.png)

In deze sjabloon worden twee parameters gedefinieerd. Er wordt een sjabloonfunctie gebruikt in location.defaultValue:

```json
"defaultValue": "[resourceGroup().location]",
```

De functie resourceGroup() retourneert een object dat de huidige resourcegroep vertegenwoordigt. Zie [Azure Resource Manager-sjabloonfuncties](./resource-group-template-functions.md) voor een overzicht van sjabloonfuncties.

De parameters die zijn gedefinieerd in de sjabloon gebruiken:

```json
"location": "[parameters('location')]",
"name": "[parameters('storageAccountType')]"
```

## <a name="use-variables-in-template"></a>Variabelen in een sjabloon gebruiken

Met variabelen kunt u waarden maken die in uw sjablonen kunnen worden gebruikt. Variabelen helpen sjablonen minder complex te maken.

![Variabelen voor Resource Manager-sjablonen](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-variables.png)

Deze sjabloon definieert één variabele: *storageAccountName*. In de definitie worden twee sjabloonfuncties gebruikt:

- **concat()**: voegt tekenreeksen samen. Zie [concat](./resource-group-template-functions-string.md#concat) voor meer informatie.
- **uniqueString()**: hiermee maakt u een deterministische hash-tekenreeks gebaseerd op de waarden die zijn geleverd als parameters. Elk Azure-opslagaccount moet een unieke naam overal binnen Azure hebben. Deze functie biedt een unieke tekenreeks. Zie [Tekenreeksfuncties](./resource-group-template-functions-string.md) voor meer tekenreeksfuncties.

De variabele die is gedefinieerd in de sjabloon gebruiken:

```json
"name": "[variables('storageAccountName')]"
```

## <a name="edit-the-template"></a>De sjabloon bewerken

Als u de aan versleuteling gerelateerde configuratie voor het opslagaccount wilt vinden, kunt u de sjabloonverwijzing van het Azure-opslagaccount gebruiken.

1. Browse naar [Azure-sjablonen](https://docs.microsoft.com/azure/templates/).
2. Selecteer in de inhoudsopgave aan de linkerkant **Verwijzing**->**Opslag**->**Opslagaccounts**. De pagina bevat de informatie voor het definiëren van de gegevens van een opslagaccount.
3. Bekijk de aan versleuteling gerelateerde informatie.  
4. Voeg de volgende json toe in het element eigenschappen van de resourcedefinitie voor het opslagaccount:

    ```json
    "encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
            "blob": {
                "enabled": true
            }
        }
    }
    ```
    Met deze tegel wordt de versleutelingsfunctie van de Blob Storage-service ingeschakeld.

Het laatste element resources ziet er als volgt uit:

![Versleutelde resources van opslagaccount in Resource Manager-sjabloon](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resources.png)

## <a name="deploy-the-template"></a>De sjabloon implementeren

Er bestaan meerdere methoden voor het implementeren van sjablonen.  In deze zelfstudie gebruikt u Cloud Shell van Azure Portal. Cloud Shell ondersteunt zowel Azure CLI als Azure PowerShell. De hier gebruikte instructies maken gebruik van CLI.

1. Meld u aan bij [Azure Portal](https://portal.azure.com)
2. Selecteer **Cloud Shell** in de rechterbovenhoek, zoals weergegeven in de volgende afbeelding:

    ![Cloud Shell in Azure Portal](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell.png)

3. Selecteer de pijl-omlaag en selecteer vervolgens **Bash** als deze niet Bash is. U gebruikt Azure CLI in deze zelfstudie.

    ![Cloud Shell CLI in Azure Portal](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-choose-cli.png)
4. Selecteer **Opnieuw starten** om de shell opnieuw te starten.
5. Selecteer **Upload/download files** en selecteer **Uploaden**.

    ![Bestand uploaden in Cloud Shell in Azure Portal](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-upload-file.png)
6. Selecteer het bestand dat u eerder in de zelfstudie hebt opgeslagen. De standaardnaam is **azuredeploy.json**.
7. Voer vanaf Cloud Shell de opdracht **ls** uit om te verifiëren of het bestand is geüpload. U kunt de opdracht **cat** gebruiken om de sjablooninhoud te verifiëren.

    ![Cloud Shell-lijstbestand in Azure Portal](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-list-file.png)
8. Voer vanuit Cloud Shell de volgende opdrachten uit:

    ```cli
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file azuredeploy.json
    ```
    Dit is de schermafbeelding van een voorbeeldimplementatie:

    ![Sjabloon implementeren in Cloud Shell in Azure Portal](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-deploy-template.png)

    In de schermafbeelding worden de volgende waarden gebruikt:

    * **&lt;ResourceGroupName>**: myresourcegroup0719. De parameter komt tweemaal voor.  Zorg ervoor dat u dezelfde waarde gebruikt.
    * **&lt;AzureLocation>**: eastus2
    * **&lt;DeployName>**: mydeployment0719
    * **&lt;TemplateFile>**: azuredeploy.json

    In de uitvoer op de schermafbeelding ziet u dat de naam van het opslagaccount *fhqbfslikdqdsstandardsa* is. 

9. Voer de volgende PowerShell-opdracht uit om het nieuw gemaakte opslagaccount weer te geven:

    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```

    U ziet een resultaat vergelijkbaar met de volgende schermafbeelding, waarin wordt aangegeven dat versleuteling voor de blob-opslag is ingeschakeld.

    ![Versleuteld opslagaccount in Azure Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-account.png)

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u met sjabloonverwijzing een bestaande sjabloon kunt aanpassen. De gebruikte sjabloon in deze zelfstudie bevat slechts één Azure-resource.  In de volgende zelfstudie ontwikkelt u een sjabloon met meerdere resources.  Sommige resources hebben afhankelijke resources.

> [!div class="nextstepaction"]
> [Meerdere resources maken](./resource-manager-tutorial-create-templates-with-dependent-resources.md)

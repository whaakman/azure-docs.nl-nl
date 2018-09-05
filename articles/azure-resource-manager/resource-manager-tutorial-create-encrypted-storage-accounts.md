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
ms.date: 08/27/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 57d5f7039831c9fd617926f20f3ff001b22ef314
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43097882"
---
# <a name="tutorial-create-an-azure-resource-manager-template-for-deploying-an-encrypted-storage-account"></a>Zelfstudie: een Azure Resource Manager-sjabloon maken voor het implementeren van een versleuteld opslagaccount

Uitleg over hoe u meer informatie kunt vinden om een Azure Resource Manager-sjabloon te voltooien.

In deze zelfstudie gebruikt u een basissjabloon uit Azure-snelstartsjablonen om een Azure Storage-account te maken.  U past de basissjabloon door middel van referentiemateriaal voor sjablonen aan om een versleuteld opslagaccount te maken.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een snelstartsjabloon openen
> * Inzicht in de sjabloon
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
- **uniqueString()**: hiermee maakt u een deterministische hash-tekenreeks gebaseerd op de waarden die zijn geleverd als parameters. Elk Azure-opslagaccount moet een naam hebben die uniek is in heel Azure. Deze functie biedt een unieke tekenreeks. Zie [Tekenreeksfuncties](./resource-group-template-functions-string.md) voor meer tekenreeksfuncties.

De variabele die is gedefinieerd in de sjabloon gebruiken:

```json
"name": "[variables('storageAccountName')]"
```

## <a name="edit-the-template"></a>De sjabloon bewerken

Het doel van deze zelfstudie is om een sjabloon te definiëren voor het maken van een versleuteld opslagaccount.  Met de voorbeeldsjabloon wordt alleen een niet-versleuteld basisopslagaccount gemaakt. Als u de aan versleuteling gerelateerde configuratie wilt vinden, kunt u de sjabloonverwijzing van het Azure-opslagaccount gebruiken.

1. Browse naar [Azure-sjablonen](https://docs.microsoft.com/azure/templates/).
2. Selecteer in de inhoudsopgave aan de linkerkant **Verwijzing**->**Opslag**->**Opslagaccounts**. U kunt ook **opslag** invoeren in het veld **Filteren op**.  De pagina bevat het schema voor het definiëren van de gegevens van een opslagaccount.
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

Wijzig de sjabloon vanuit Visual Studio Code, zodat het uiteindelijke resources-element er als volgt uitziet:

![Versleutelde resources van opslagaccount in Resource Manager-sjabloon](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resources.png)

## <a name="deploy-the-template"></a>De sjabloon implementeren

Raadpleeg de sectie [De sjabloon implementeren](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) in de snelstartgids van Visual Studio Code voor de implementatieprocedure.

In de volgende schermafbeelding ziet u de CLI-opdracht voor het weergeven van het zojuist gemaakte opslagaccount, waarmee wordt aangegeven dat versleuteling is ingeschakeld voor de blobopslag.

![Versleuteld opslagaccount in Azure Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-account.png)

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u met sjabloonverwijzing een bestaande sjabloon kunt aanpassen. De gebruikte sjabloon in deze zelfstudie bevat slechts één Azure-resource.  In de volgende zelfstudie ontwikkelt u een sjabloon met meerdere resources. Sommige resources hebben afhankelijke resources.

> [!div class="nextstepaction"]
> [Meerdere resources maken](./resource-manager-tutorial-create-templates-with-dependent-resources.md)

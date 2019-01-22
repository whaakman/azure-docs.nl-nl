---
title: Een Azure Resource Manager-sjabloon maken en implementeren via Azure Portal | Microsoft Docs
description: Leer hoe u uw eerste Azure Resource Manager-sjabloon maakt via Azure Portal en hoe u deze implementeert.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 01/11/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: f989a006251313a8439432861477dc133374af35
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54304666"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>Quickstart: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal

Leer hoe u een Resource Manager-sjabloon kunt genereren met behulp van de Azure-portal, en hoe u de sjabloon vanuit de portal bewerkt en implementeert. Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren. Zie [Overzicht van Azure Resource Manager](resource-group-overview.md) voor inzicht in de concepten die gerelateerd zijn aan het implementeren en beheren van uw Azure-oplossingen.

Na het voltooien van de zelfstudie implementeert u een Azure Storage-account. Hetzelfde proces kan worden gebruikt voor het implementeren van andere Azure-resources.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="generate-a-template-using-the-portal"></a>Een sjabloon maken via de portal

Het maken van een geheel nieuwe Resource Manager-sjabloon is geen gemakkelijke taak, vooral als Azure-implementatie nieuw voor u is en u niet bekend bent met de JSON-indeling. Met behulp van de Azure-portal kunt u een resource configureren, bijvoorbeeld een Azure Storage-account. Voordat u de resource implementeert, kunt u uw configuratie exporteren naar een Resource Manager-sjabloon. U kunt de sjabloon opslaan voor later gebruik.

Veel ervaren sjabloonontwikkelaars gebruiken deze methode om werkende sjablonen te genereren wanneer ze proberen Azure-resources te implementeren waarmee ze niet vertrouwd zijn.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Een resource maken** > **Opslag** > **Opslagaccount - blob, bestand, tabel, wachtrij**.

    ![Een Azure Storage-account in Azure Portal maken](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. Voer de volgende informatie in:

    - **Resourcegroep**: Selecteer **Nieuwe maken** en geef een resourcegroepnaam naar keuze op. In de schermafbeelding is de naam van de resourcegroep *mystorage1016rg*. Een resourcegroep is een container voor Azure-resources. Een resourcegroep maakt het gemakkelijker Azure-resources te beheren.
    - **Naam**: geef uw opslagaccount een unieke naam. In de schermafbeelding is de naam *mystorage1016*.

    Voor de overige eigenschappen kunt u de standaardwaarden gebruiken.

    ![Een Azure Storage-accountconfiguratie in Azure Portal maken](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Sommige geëxporteerde sjablonen moeten worden bewerkt voordat u ze implementeren.

4. Selecteer **Controleren + maken** onderaan in het scherm.
5. Selecteer **Een sjabloon voor automatisering downloaden** onderaan in het scherm. In de portal wordt de gegenereerde sjabloon weergegeven:

    ![Een sjabloon genereren via de portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    In het hoofdvenster wordt de sjabloon getoond. Het is een JSON-bestand met vier elementen van het hoogste niveau: `schema`, `contentVersion`, `parameters`, `variables`, `resources` en `output`. Zie [Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md) voor meer informatie

    Er zijn zes parameters gedefinieerd. Een van deze parameters heet **storageAccountName**. In het tweede gemarkeerde gedeelte van de bovenstaande schermopname ziet u hoe u in de sjabloon naar deze parameter kunt verwijzen. In het volgende gedeelte bewerkt u de sjabloon zodat er voor het opslagaccount gebruik wordt gemaakt van een gegenereerde naam.

    In de sjabloon is er één Azure-resource gedefinieerd. Deze is van het type [Microsoft.Storage/storageAccounts]. Bekijk hoe de resource wordt gedefinieerd en wat de definitiestructuur is.
6. Selecteer **Download**. Sla **template.json** uit het gedownloade pakket op op uw computer. In het volgende gedeelte gebruikt u een implementatiehulpprogramma voor sjablonen om de sjabloon te bewerken.
7. Selecteer het tabblad **Parameter** om de waarden te bekijken die u voor de parameters hebt opgegeven. Schrijf deze waarden op. U hebt ze in het volgende gedeelte (bij het implementeren van de sjabloon) weer nodig.

    ![Een sjabloon genereren via de portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Met behulp van zowel de sjabloon- als de parameterbestanden kunt u een resource maken: in deze zelfstudie een Azure-opslagaccount.

## <a name="edit-and-deploy-the-template"></a>De sjabloon bewerken en implementeren

U kunt de Azure-portal gebruiken om eenvoudige wijzigingen door te voeren in sjablonen. In deze quickstart gebruikt u het portalhulpprogramma *Sjabloonimplementatie*. In deze zelfstudie wordt *Sjabloonimplementatie* gebruikt, zodat u de hele zelfstudie kunt doen met behulp van één interface: de Azure-portal. Als u een complexere sjabloon wilt bewerken, kunt u gebruikmaken van [Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md), dat meer bewerkingsfuncties heeft.

Azure vereist dat elke Azure-service een unieke naam heeft. De implementatie mislukt als u de naam van een opslagaccount invoert dat al bestaat. Om dit probleem te vermijden, voegt u een aanroep naar de sjabloonfunctie `uniquestring()` toe aan de sjabloon om een unieke naam voor de opslagaccount te genereren.

1. Selecteer in Azure Portal **Een resource maken**.
2. In **Marketplace doorzoeken** typt u **sjabloonimplementatie**. Druk vervolgens op **ENTER**.
3. Selecteer **Sjabloonimplementatie**.

    ![Azure Resource Manager-sjabloonbibliotheek](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
4. Selecteer **Maken**.
5. Selecteer **Bouw uw eigen sjabloon in de editor**.
6. Selecteer **Bestand laden** en volg de instructies voor het laden van het bestand template.json dat u in het vorige gedeelte hebt gedownload.
7. Voeg één variabele toe zoals wordt weergegeven in de volgende schermafbeelding:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
    ```
    ![Azure Resource Manager-sjablonen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

    Er worden hier twee sjabloonfuncties gebruikt: `concat()` en `uniqueString()`.

8. Verwijder de parameter **storageAccountName** die in de vorige schermafbeelding is gemarkeerd.
9. Werk het naamelement bij van de resource **Microsoft.Storage/storageAccounts** voor gebruik van de nieuw gedefinieerde variabele in plaats van de parameter:

    ```json
    "name": "[variables('storageAccountName')]",
    ```

    De uiteindelijke sjabloon moet er als volgt uitzien:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "location": {
                "type": "string"
            },
            "accountType": {
                "type": "string"
            },
            "kind": {
                "type": "string"
            },
            "accessTier": {
                "type": "string"
            },
            "supportsHttpsTrafficOnly": {
                "type": "bool"
            }
        },
        "variables": {
            "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
            {
                "name": "[variables('storageAccountName')]",
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2018-07-01",
                "location": "[parameters('location')]",
                "properties": {
                    "accessTier": "[parameters('accessTier')]",
                    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
                },
                "dependsOn": [],
                "sku": {
                    "name": "[parameters('accountType')]"
                },
                "kind": "[parameters('kind')]"
            }
        ],
        "outputs": {}
    }
    ```
7. Selecteer **Opslaan**.
8. Voer de volgende waarden in:

    - **Resourcegroep**: selecteer **Nieuwe maken** en geef de resourcegroep een unieke naam.
    - **Locatie**: selecteer een locatie voor de resourcegroep. Bijvoorbeeld **US - centraal**. 
    - **Locatie**: selecteer een locatie voor het opslagaccount. Bijvoorbeeld **US - centraal**.
    - **Accounttype**: voer voor deze quickstart **Standard_LRS** in.
    - **Soort**: voer voor deze quickstart **StorageV2** in.
    - **Toegangslaag**: voer voor deze quickstart **Dynamisch** in.
    - **Alleen HTTPS-verkeer ingeschakeld**.  Selecteer voor deze quickstart **true**.
    - **Ik ga akkoord met de bovenstaande voorwaarden**: (selecteren)

    Dit is een schermafbeelding van een voorbeeldimplementatie:

    ![Implementatie van Azure Resource Manager-sjablonen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

10. Selecteer **Aankoop**.
11. Selecteer het belpictogram (meldingen) boven in het scherm om de implementatiestatus te zien. U ziet **Implementatie wordt uitgevoerd**. Wacht tot de implementatie is voltooid.

    ![Notificatie van implementatie van Azure Resource Manager-sjablonen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

12. Selecteer **Ga naar de resourcegroep** in het deelvenster meldingen. U ziet een scherm dat vergelijkbaar is met:

    ![Resourcegroep voor implementatie van Azure Resource Manager-sjablonen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    U ziet dat de status van de implementatie is voltooid en er slechts één opslagaccount in de resourcegroep is. De naam van het opslagaccount is een unieke tekenreeks gegenereerd door de sjabloon. Meer informatie over Azure-opslagaccounts vindt u in [Quickstart: blobs uploaden, downloaden en vermelden met behulp van Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet het opslagaccount in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een sjabloon genereert in Azure Portal en hoe u de sjabloon via de portal implementeert. De gebruikte sjabloon in deze quickstart is een eenvoudige sjabloon met één Azure-resource. Als de sjabloon complex is, kunt u beter Visual Studio Code of Visual Studio gebruiken om de sjabloon te ontwikkelen. In de volgende quickstart ziet u hoe u sjablonen ook kunt implementeren met Azure PowerShell en de Azure-opdrachtregelinterface (CLI).

> [!div class="nextstepaction"]
> [Create templates by using Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md) (Sjablonen maken met Visual Studio Code)

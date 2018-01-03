---
title: Maken en publiceren van een Azure-service catalogus beheerde toepassing | Microsoft Docs
description: In dit artikel leest u hoe u een beheerde Azure-toepassing maakt die is bedoeld voor leden van uw organisatie.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: 46adcdf39625c85dc962a7541b68c5500cf920ee
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>Een beheerde toepassing voor intern verbruik publiceren

U kunt maken en publiceren van Azure [beheerde toepassingen](overview.md) die bestemd zijn voor leden van uw organisatie. Een IT-afdeling kan bijvoorbeeld beheerde toepassingen die ervoor zorgen aan de organisatie-standaarden dat publiceren. Deze beheerde toepassingen zijn beschikbaar via de Servicecatalogus, niet de Azure marketplace.

Voor het publiceren van een beheerde toepassingsservices voor de Servicecatalogus, moet u het volgende doen:

* Maak een sjabloon die de resources te implementeren met de beheerde toepassing definieert.
* Definieer de gebruikersinterface-elementen voor de portal bij het implementeren van de beheerde toepassing.
* Een ZIP-pakket met de vereiste sjabloon-bestanden maken.
* Bepaal welke gebruiker, groep of toepassing moet toegang tot de resourcegroep in het abonnement van de gebruiker.
* De definitie van de beheerde toepassing die verwijst naar het ZIP-pakket en aanvragen van toegang voor de identiteit maken.

Voor dit artikel bevat de beheerde toepassing alleen een opslagaccount. Het is bedoeld ter illustratie van de stappen voor het publiceren van een beheerde toepassing. Zie voor voorbeelden van volledige [voorbeeldprojecten voor Azure beheerde toepassingen](sample-projects.md).

## <a name="create-the-resource-template"></a>De resource-sjabloon maken

De definitie van elke beheerde toepassing bevat een bestand met de naam **mainTemplate.json**. In deze definieert u de Azure-resources te richten. De sjabloon is niet anders dan een reguliere Resource Manager-sjabloon.

Maak een bestand met de naam **mainTemplate.json**. De naam is hoofdlettergevoelig.

De volgende JSON toevoegen aan uw bestand. Definieert de parameters voor het maken van een opslagaccount, en geeft de eigenschappen voor het opslagaccount.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        },
        "storageAccountType": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString('storage'))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {
        "storageEndpoint": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
        }
    }
}
```

Sla het bestand mainTemplate.json.

## <a name="create-the-user-interface-definition"></a>De gebruiker interfacedefinitie maken

De Azure portal maakt gebruik van de **createUiDefinition.json** bestand voor het genereren van de gebruikersinterface voor gebruikers die de beheerde toepassing maken. U definiëren hoe gebruikers de gegevens voor elke parameter opgeven. U kunt opties kunt gebruiken, zoals een vervolgkeuzelijst, in het tekstvak, wachtwoord en andere hulpprogramma's voor invoer. Zie [Aan de slag met CreateUiDefinition](create-uidefinition-overview.md) voor meer informatie over het maken van een UI-definitiebestand voor een beheerde toepassing.

Maak een bestand met de naam **createUiDefinition.json**. De naam is hoofdlettergevoelig.

De volgende JSON toevoegen aan het bestand.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "storageConfig",
                "label": "Storage settings",
                "subLabel": {
                    "preValidation": "Configure the infrastructure settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccounts",
                        "type": "Microsoft.Storage.MultiStorageAccountCombo",
                        "label": {
                            "prefix": "Storage account name prefix",
                            "type": "Storage account type"
                        },
                        "defaultValue": {
                            "type": "Standard_LRS"
                        },
                        "constraints": {
                            "allowedTypes": [
                                "Premium_LRS",
                                "Standard_LRS",
                                "Standard_GRS"
                            ]
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "storageAccountNamePrefix": "[steps('storageConfig').storageAccounts.prefix]",
            "storageAccountType": "[steps('storageConfig').storageAccounts.type]",
            "location": "[location()]"
        }
    }
}
```

Sla het bestand createUIDefinition.json.

## <a name="package-the-files"></a>De bestanden van het pakket

De twee bestanden toevoegen aan een ZIP-bestand met de naam app.zip. De twee bestanden moeten zich op het hoogste niveau van het ZIP-bestand. Als u opslaan in een map, ontvangt u een fout opgetreden bij het maken van de definitie van de beheerde toepassing waarin wordt vermeld dat de vereiste bestanden zijn niet aanwezig. 

Het uploaden van het pakket naar een toegankelijke locatie vanaf waar deze kan worden gebruikt. 

```powershell
New-AzureRmResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob

$ctx = $storageAccount.Context

New-AzureStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzureStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>De definitie van de beheerde toepassing maken

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Een groep voor Azure Active Directory-gebruiker of toepassing maken

De volgende stap is het selecteren van een, gebruikersgroep of toepassing voor het beheren van bronnen namens de klant. Deze, gebruikersgroep of toepassing kunt u de juiste machtigingen heeft op de beheerde resourcegroep volgens de rol die is toegewezen. De rol kan ingebouwde rollen gebaseerd toegangsbeheer (RBAC)-functie zoals eigenaar of bijdrager zijn. Ook kunt u machtigen een afzonderlijke gebruiker om de resources te beheren, maar doorgaans u deze machtiging toewijzen aan een gebruikersgroep. Voor het maken van een nieuwe Active Directory-gebruikersgroep, Zie [een groep maken en leden toevoegen in Azure Active Directory](../active-directory/active-directory-groups-create-azure-portal.md).

U moet de object-ID van de gebruikersgroep moet worden gebruikt voor het beheren van de resources. 

![Groeps-ID ophalen](./media/publish-service-catalog-app/get-group-id.png)

### <a name="get-the-role-definition-id"></a>De roldefinitie-ID ophalen

Vervolgens moet u de roldefinitie-ID van de ingebouwde RBAC-rol die u toegang wilt verlenen aan de gebruiker, groep of toepassing. Meestal gebruikt u de rol van eigenaar of bijdrager of lezer. In de volgende opdracht ziet u hoe u de roldefinitie-id ophaalt voor de rol van eigenaar:

```powershell
$ownerID=(Get-AzureRmRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>De definitie van de beheerde toepassing maken

Als u nog geen een resourcegroep voor het opslaan van de definitie van de beheerde toepassing, maakt u een nu:

```powershell
New-AzureRmResourceGroup -Name appDefinitionGroup -Location westcentralus
```

Maak nu de definitieresource van de beheerde toepassing.

```powershell
$blob = Get-AzureStorageBlob -Container appcontainer -Blob app.zip -Context $ctx

New-AzureRmManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "westcentralus" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Azure Storage Account" `
  -Authorization "<group-id>:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

## <a name="create-the-managed-application-by-using-the-portal"></a>De beheerde toepassing maken met behulp van de portal

Nu gaan we de portal gebruiken om de beheerde toepassing te implementeren. Ziet u de gebruikersinterface die u hebt gemaakt in het pakket.

1. Ga naar de Azure-portal. Selecteer **+ nieuw** en zoek naar **Servicecatalogus**.

   ![Servicecatalogus zoeken](./media/publish-service-catalog-app/select-new.png)

1. Selecteer **Servicecatalogus beheerde toepassing**.

   ![Selecteer de Servicecatalogus](./media/publish-service-catalog-app/select-service-catalog.png)

1. Selecteer **Maken**.

   ![Selecteer maken](./media/publish-service-catalog-app/select-create.png)

1. Zoek de beheerde toepassing die u wilt maken in de lijst met beschikbare oplossingen en selecteert u deze. Selecteer **Maken**.

   ![De beheerde toepassing zoeken](./media/publish-service-catalog-app/find-application.png)

1. Geef algemene informatie die is vereist voor de beheerde toepassing. Geef het abonnement en een nieuwe resourcegroep op de beheerde toepassing bevat. Selecteer **West-Centraal VS** voor de locatie. Wanneer u klaar bent, selecteer **OK**.

   ![Geef parameters op beheerde toepassing](./media/publish-service-catalog-app/provide-basics.png)

1. Geef de waarde die specifiek voor de resources in de beheerde toepassing zijn. Wanneer u klaar bent, selecteer **OK**.

   ![Geef Resourceparameters op die](./media/publish-service-catalog-app/provide-resource-values.png)

1. De sjabloon valideert de waarden die u hebt opgegeven. Als de validatie slaagt, selecteert u **OK** implementatie te starten.

   ![Valideren van de beheerde toepassing](./media/publish-service-catalog-app/validate.png)

Nadat de implementatie is voltooid, wordt de beheerde toepassing in een resourcegroep met de naam applicationGroup bestaat. Het opslagaccount bestaat in een resourcegroep met de naam applicationGroup plus een hash string-waarde.

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van beheerde toepassingen](overview.md) voor een inleiding tot beheerde toepassingen.
* Bijvoorbeeld projecten, Zie [voorbeeldprojecten voor Azure beheerde toepassingen](sample-projects.md).
* Zie [Aan de slag met CreateUiDefinition](create-uidefinition-overview.md) voor meer informatie over het maken van een UI-definitiebestand voor een beheerde toepassing.

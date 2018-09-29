---
title: Door Azure beheerde toepassing in servicecatalogus maken en publiceren | Microsoft Docs
description: In dit artikel leest u hoe u een beheerde Azure-toepassing maakt die is bedoeld voor leden van uw organisatie.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.date: 06/08/2018
ms.author: tomfitz
ms.openlocfilehash: 3b1da6e9068be3c96cce5973f29344fe7e4b4872
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095837"
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>Een beheerde toepassing publiceren voor intern gebruik

U kunt door Azure [beheerde toepassingen](overview.md) maken en publiceren die bedoeld zijn voor leden van uw organisatie. Zo kan een IT-afdeling beheerde toepassingen publiceren die voldoen aan de organisatiestandaarden. Deze beheerde toepassingen zijn beschikbaar via de servicecatalogus, niet Azure Marketplace.

U moet het volgende doen om een beheerde toepassing te publiceren voor de servicecatalogus:

* Een sjabloon maken die de resources definieert die met de beheerde toepassing moeten worden geïmplementeerd.
* De elementen van de gebruikersinterface voor de portal definiëren bij het implementeren van de beheerde toepassing.
* Een ZIP-pakket met de vereiste sjabloonbestanden maken.
* Bepalen welke gebruiker, groep of toepassing toegang moet hebben tot de resourcegroep in het abonnement van de gebruiker.
* De definitie van de beheerde toepassing maken die verwijst naar het ZIP-pakket en toegang voor de identiteit aanvraagt.

De beheerde toepassing in dit artikel bevat alleen een opslagaccount. Het artikel is namelijk alleen bedoeld om de stappen voor het publiceren van een beheerde toepassing te laten zien. Zie [Voorbeeldprojecten voor door Azure beheerde toepassingen](sample-projects.md) voor uitgebreide voorbeelden.

Voor de PowerShell-voorbeelden in dit artikel hebt u Azure PowerShell versie 6.2 of hoger nodig. [Werk uw versie bij](/powershell/azure/install-azurerm-ps) als dat nodig is.

## <a name="create-the-resource-template"></a>De resourcesjabloon maken

De definitie van een beheerde toepassing bevat altijd een bestand met de naam **mainTemplate.json**. Hierin definieert u de Azure-resources die u wilt implementeren. De sjabloon is niet anders dan een reguliere Resource Manager-sjabloon.

Maak een bestand met de naam **mainTemplate.json**. De naam is hoofdlettergevoelig.

Voeg de volgende JSON-code toe aan uw bestand. Hiermee definieert u de parameters voor het maken van een opslagaccount, en geeft u de eigenschappen voor het opslagaccount op.

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
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]"
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

Sla het bestand mainTemplate.json op.

## <a name="create-the-user-interface-definition"></a>Definitie van gebruikersinterface maken

Azure Portal gebruikt het bestand **createUiDefinition.json** om de gebruikersinterface te genereren voor gebruikers die de beheerde toepassing maken. U definieert hoe gebruikers invoer opgeven voor elke parameter. U kunt opties gebruiken zoals een vervolgkeuzelijst, tekstvak, wachtwoordvak en andere invoermethoden. Zie [Aan de slag met CreateUiDefinition](create-uidefinition-overview.md) voor meer informatie over het maken van een UI-definitiebestand voor een beheerde toepassing.

Maak een bestand met de naam **createUiDefinition.json**. De naam is hoofdlettergevoelig.

Voeg de volgende JSON-code toe aan het bestand.

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

Sla het bestand createUiDefinition.json op.

## <a name="package-the-files"></a>De bestanden verpakken

Voeg de twee bestanden toe aan een ZIP-bestand met de naam app.zip. De twee bestanden moeten zich in de hoofdmap van het ZIP-bestand bevinden. Als u ze in een map opslaat, treedt er een fout op bij het maken van de definitie van de beheerde toepassing met de mededeling dat de vereiste bestanden niet zijn gevonden. 

Upload het pakket naar een toegankelijke locatie vanaf waar het pakket kan worden gebruikt. 

```powershell
New-AzureRmResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context

New-AzureStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzureStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>De definitie van de beheerde toepassing maken

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Een Azure Active Directory-gebruikersgroep of -toepassing maken

De volgende stap is het selecteren van een gebruikersgroep of toepassing om de resources namens de klant te beheren. Deze gebruikersgroep of toepassing heeft machtigingen voor de beheerde resourcegroep overeenkomstig de toegewezen rol. De rol kan elke ingebouwde rol van op rollen gebaseerd toegangsbeheer (RBAC) zijn, zoals Eigenaar of Inzender. U kunt ook een afzonderlijke gebruiker toestemming geven om de resources te beheren, maar meestal wijst u deze machtiging toe aan een gebruikersgroep. Zie [Een groep maken en leden toevoegen in Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) voor instructies voor het maken van een nieuwe Active Directory-gebruikersgroep.

U hebt de object-id van de gebruikersgroep nodig om de resources te beheren. 

```powershell
$groupID=(Get-AzureRmADGroup -DisplayName mygroup).Id
```

### <a name="get-the-role-definition-id"></a>De roldefinitie-id opvragen

Vervolgens hebt u de roldefinitie-id nodig van de ingebouwde RBAC-rol waarmee u toegang wilt verlenen aan de gebruiker, gebruikersgroep of toepassing. Meestal gebruikt u de rol van eigenaar, inzender of lezer. In de volgende opdracht ziet u hoe u de roldefinitie-id ophaalt voor de rol van eigenaar:

```powershell
$ownerID=(Get-AzureRmRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>De definitie van de beheerde toepassing maken

Als u nog geen resourcegroep hebt voor het opslaan van de definitie van de beheerde toepassing, maakt u er nu een:

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
  -Authorization "${groupID}:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

### <a name="make-sure-users-can-see-your-definition"></a>Ervoor zorgen dat gebruikers de definitie kunnen zien

U hebt toegang tot de definitie van de beheerde toepassing, maar u wilt controleren of andere gebruikers in uw organisatie hiertoe toegang hebben. Verleen hun minimaal de rol van Lezer voor de definitie. Mogelijk hebben ze dit toegangsniveau overgenomen van het abonnement of de resourcegroep. Als u wilt controleren wie toegang heeft tot de definitie en gebruikers of groepen wilt toevoegen, raadpleegt u [Op rollen gebaseerd toegangsbeheer gebruiken om de toegang tot de resources van uw Azure-abonnement te beheren](../role-based-access-control/role-assignments-portal.md).

## <a name="create-the-managed-application"></a>De beheerde toepassing maken

U kunt de beheerde toepassing implementeren via de portal, PowerShell of Azure CLI.

### <a name="powershell"></a>PowerShell

We gebruiken eerst PowerShell om de beheerde toepassing te implementeren.

```powershell
# Create resource group
New-AzureRmResourceGroup -Name applicationGroup -Location westcentralus

# Get ID of managed application definition
$appid=(Get-AzureRmManagedApplicationDefinition -ResourceGroupName appDefinitionGroup -Name ManagedStorage).ManagedApplicationDefinitionId

# Create the managed application
New-AzureRmManagedApplication `
  -Name storageApp `
  -Location westcentralus `
  -Kind ServiceCatalog `
  -ResourceGroupName applicationGroup `
  -ManagedApplicationDefinitionId $appid `
  -ManagedResourceGroupName "InfrastructureGroup" `
  -Parameter "{`"storageAccountNamePrefix`": {`"value`": `"demostorage`"}, `"storageAccountType`": {`"value`": `"Standard_LRS`"}}"
```

Uw beheerde toepassing en de beheerde infrastructuur zijn nu aanwezig in het abonnement.

### <a name="portal"></a>Portal

Vervolgens gebruiken we de portal om de beheerde toepassing te implementeren. U ziet de gebruikersinterface die u in het pakket hebt gemaakt.

1. Ga naar Azure Portal. Selecteer **+ Een resource maken** en zoek naar **servicecatalogus**.

   ![Zoeken naar servicecatalogus](./media/publish-service-catalog-app/create-new.png)

1. Selecteer **Beheerde toepassing voor de servicecatalogus**.

   ![Servicecatalogus selecteren](./media/publish-service-catalog-app/select-service-catalog-managed-app.png)

1. Selecteer **Maken**.

   ![Selecteer Maken](./media/publish-service-catalog-app/select-create.png)

1. Zoek de beheerde toepassing die u wilt maken in de lijst met beschikbare oplossingen en selecteer de toepassing. Selecteer **Maken**.

   ![De beheerde toepassing vinden](./media/publish-service-catalog-app/find-application.png)

   Als u de definitie van de beheerde toepassing niet kunt zien via de portal, moet u mogelijk uw portal-instellingen wijzigen. Selecteer het **Map- en abonnementsfilter**.

   ![Abonnementsfilter selecteren](./media/publish-service-catalog-app/select-filter.png)

   Controleer of het globaal abonnementsfilter het abonnement bevat dat de definitie van de beheerde toepassing bevat.

   ![Abonnementsfilter controleren](./media/publish-service-catalog-app/check-global-filter.png)

   Wanneer het abonnement is geselecteerd, begint u opnieuw met het maken van de beheerde toepassing voor de servicecatalogus. Deze moet nu worden weergegeven.

1. Geef basisgegevens op die vereist zijn voor de beheerde toepassing. Geef het abonnement en een nieuwe resourcegroep op voor de beheerde toepassing. Selecteer **US - west-centraal** als de locatie. Selecteer **Ok** wanneer u gereed bent.

   ![Parameters voor beheerde toepassing opgeven](./media/publish-service-catalog-app/add-basics.png)

1. Geef waarden op die specifiek zijn voor de resources in de beheerde toepassing. Selecteer **Ok** wanneer u gereed bent.

   ![Resourceparameters opgeven](./media/publish-service-catalog-app/add-storage-settings.png)

1. De sjabloon valideert de waarden die u hebt opgegeven. Als de validatie is gelukt, selecteert u **OK** om de implementatie te starten.

   ![Beheerde toepassing valideren](./media/publish-service-catalog-app/view-summary.png)

Als de implementatie is voltooid, maakt de beheerde toepassing deel uit van een resourcegroep met de naam applicationGroup. Het opslagaccount bevindt zich in een resourcegroep met de naam applicationGroup plus een gehashte tekenreekswaarde.

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van beheerde toepassingen](overview.md) voor een inleiding tot beheerde toepassingen.
* Zie [Voorbeeldprojecten voor door Azure beheerde toepassingen](sample-projects.md) voor voorbeeldprojecten.
* Zie [Aan de slag met CreateUiDefinition](create-uidefinition-overview.md) voor meer informatie over het maken van een UI-definitiebestand voor een beheerde toepassing.

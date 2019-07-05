---
title: Azure beheerde toepassingen met beheerde identiteit
description: Informatie over het configureren van een beheerde toepassing met een beheerde identiteit. Beheerde toepassingen voor het beheren van Azure-resources buiten de beheerde resourcegroep en een operationele identiteit van beheerde toepassingen voor het activiteitenlogboek bieden beheerde identiteit kan worden gebruikt voor het implementeren van beheerde toepassingen die zijn gekoppeld aan bestaande resources verlenen en andere services binnen Azure.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.reviewer: ''
ms.author: jobreen
author: jjbfour
ms.date: 05/13/2019
ms.openlocfilehash: 9fb5f7a4a62c2d323059f7c0b879482e93feef2f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434861"
---
# <a name="azure-managed-application-with-managed-identity"></a>Azure beheerde toepassingen met beheerde identiteit

> [!NOTE]
> Ondersteuning van beheerde identiteiten voor beheerde toepassingen is momenteel in preview. Gebruik de api-versie 2018-09-01-preview gebruikmaken van beheerde identiteit.

Informatie over het configureren van een beheerde toepassing bevat een beheerde identiteit. Beheerde identiteit kan worden gebruikt om toe te staan van de klant kan de beheerde App toegang verlenen tot extra bestaande resources. De identiteit wordt beheerd door het Azure-platform en vereist niet dat u in te richten of er geheimen draaien. Zie voor meer informatie over beheerde identiteiten in Azure Active Directory (AAD), [beheerde identiteiten voor een Azure-resources](../active-directory/managed-identities-azure-resources/overview.md).

Uw toepassing kan twee soorten identiteiten worden verleend:

- Een **systeem toegewezen identiteit** is gekoppeld aan uw toepassing en wordt verwijderd als uw app wordt verwijderd. Een app kan slechts één systeem toegewezen identiteit hebben.
- Een **gebruiker toegewezen identiteit** is een afzonderlijke Azure-resource die kan worden toegewezen aan uw app. Een app kan meerdere gebruiker toegewezen identiteiten hebben.

## <a name="how-to-use-managed-identity"></a>Het gebruik van beheerde identiteit

Beheerde identiteit kan veel scenario's voor beheerde toepassingen. Er zijn enkele algemene scenario's die kunnen worden opgelost:

- Implementatie van een beheerde toepassing gekoppeld aan bestaande Azure-resources. Een voorbeeld is de implementatie van een Azure virtuele machine (VM) in de beheerde toepassing die is gekoppeld aan een [bestaande netwerkinterface](../virtual-network/virtual-network-network-interface-vm.md).
- De beheerde toepassingen en uitgever toegang verlenen tot Azure-resources buiten de **beheerde resourcegroep**.
- Bieden een operationele identiteit van beheerde toepassingen voor activiteitenlogboek en andere services binnen Azure.

## <a name="adding-managed-identity"></a>Toevoegen van beheerde identiteit

Het maken van een beheerde toepassing met een beheerde identiteit vereist een extra eigenschap worden ingesteld op de Azure-resource. Het volgende voorbeeld wordt een voorbeeld **identiteit** eigenschap:

```json
{
"identity": {
    "type": "SystemAssigned, UserAssigned",
    "userAssignedIdentities": {
        "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity": {}
    }
}
```

Er zijn twee algemene manieren om te maken van een beheerde toepassing met **identiteit**: [CreateUIDefinition.json](./create-uidefinition-overview.md) en [Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md). Voor eenvoudige enkele scenario's maken, CreateUIDefinition moet worden gebruikt voor het inschakelen van de beheerde identiteit, omdat deze een rijkere ervaring biedt. Tijdens het afhandelen van geavanceerde of complexe systemen waarvoor automatische of meerdere implementaties van beheerde toepassingen, sjablonen kunnen worden gebruikt.

### <a name="using-createuidefinition"></a>Met behulp van CreateUIDefinition

Een beheerde toepassing kan worden geconfigureerd met de beheerde identiteit via de [CreateUIDefinition.json](./create-uidefinition-overview.md). In de [sectie uitvoer](./create-uidefinition-overview.md#outputs), de sleutel `managedIdentity` kan worden gebruikt voor de onderdrukking van de eigenschap id van de beheerde toepassingen. Schakelt u het onderstaande voorbeeld **systeem toegewezen** identiteit op de beheerde toepassing. Complexere identiteitsobjecten kunnen worden gevormd door de gebruiker om invoer vragen met behulp van CreateUIDefinition-elementen. Deze invoer kunnen worden gebruikt voor het maken van beheerde toepassingen met **gebruiker toegewezen identiteit**.

```json
"outputs": {
    "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
}
```

#### <a name="when-to-use-createuidefinition-for-managed-identity"></a>Wanneer u CreateUIDefinition voor beheerde identiteit

Hieronder volgen enkele aanbevelingen voor wanneer CreateUIDefinition worden gebruikt voor het inschakelen van de beheerde identiteit op beheerde toepassingen.

- Het maken van de beheerde toepassingen verloopt via Azure portal of marketplace.
- De beheerde identiteit complexe consument invoer is vereist.
- De beheerde identiteit is vereist op het maken van de beheerde toepassing.

#### <a name="systemassigned-createuidefinition"></a>SystemAssigned CreateUIDefinition

Een eenvoudige CreateUIDefinition waarmee de identiteit van de SystemAssigned voor de beheerde toepassing.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
        ],
        "outputs": {
            "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
        }
    }
}
```

#### <a name="userassigned-createuidefinition"></a>UserAssigned CreateUIDefinition

Een eenvoudige CreateUIDefinition waarmee een **gebruiker toegewezen identiteit** resource als invoer en de UserAssigned-identiteit voor de beheerde toepassing.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "manageIdentity",
                "label": "Identity",
                "subLabel": {
                    "preValidation": "Manage Identities",
                    "postValidation": "Done"
                },
                "bladeTitle": "Identity",
                "elements": [
                    {
                        "name": "userAssignedText",
                        "type": "Microsoft.Common.TextBox",
                        "label": "User assigned managed identity",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('manageIdentity').userAssignedText),':{}}}'))]"
        }
    }
}
```

De bovenstaande CreateUIDefinition.json genereert een gebruikerservaring maken waarvoor een tekstvak voor een gebruiker in te voeren de **gebruiker toegewezen identiteit** Azure-resource-ID. De gegenereerde ervaring zou er als volgt uitzien:

![Voorbeeld van een gebruiker toegewezen identiteit CreateUIDefinition](./media/publish-managed-identity/user-assigned-identity.png)

### <a name="using-azure-resource-manager-templates"></a>Met behulp van Azure Resource Manager-sjablonen

> [!NOTE]
> Beheerde toepassingen sjablonen op Marketplace worden automatisch gegenereerd voor de ervaring voor klanten via de Azure-portal maken.
> Voor deze scenario's de `managedIdentity` uitvoer toets op het CreateUIDefinition moet worden gebruikt om de identiteit ingeschakeld.

De beheerde identiteit kan ook worden ingeschakeld via Azure Resource Manager-sjablonen. Schakelt u het onderstaande voorbeeld **systeem toegewezen** identiteit op de beheerde toepassing. Complexere identiteitsobjecten kunnen met behulp van Azure Resource Manager-Sjabloonparameters voor invoer worden gevormd. Deze invoer kunnen worden gebruikt voor het maken van beheerde toepassingen met **gebruiker toegewezen identiteit**.

#### <a name="when-to-use-azure-resource-manager-templates-for-managed-identity"></a>Wanneer u Azure Resource Manager-sjablonen voor de beheerde identiteit

Hieronder volgen enkele aanbevelingen voor wanneer u Azure Resource Manager-sjablonen voor het inschakelen van de beheerde identiteit op beheerde toepassingen.

- Beheerde toepassingen kunnen via een programma worden geïmplementeerd op basis van een sjabloon.
- Aangepaste roltoewijzingen voor de beheerde identiteit zijn nodig voor het inrichten van de beheerde toepassing.
- De beheerde toepassing hoeft niet de Azure portal en marketplace maken stroom.

#### <a name="systemassigned-template"></a>SystemAssigned sjabloon

Een eenvoudige Azure Resource Manager-sjabloon die een beheerde toepassing met implementeert **systeem toegewezen** identiteit.

```json
"resources": [
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "SystemAssigned"
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

### <a name="userassigned-template"></a>UserAssigned sjabloon

Een eenvoudige Azure Resource Manager-sjabloon die een beheerde toepassing met implementeert een **gebruiker toegewezen identiteit**.

```json
"resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('managedIdentityName')]",
      "apiVersion": "2018-11-30",
      "location": "[parameters('location')]"
    },
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "UserAssigned",
            "userAssignedIdentities": {
                "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',parameters('managedIdentityName'))]": {}
            }
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

## <a name="granting-access-to-azure-resources"></a>Het verlenen van toegang tot Azure-resources

Wanneer een beheerde toepassingen is een identiteit die is verleend, is het kan dat deze toegang tot de bestaande azure-resources worden verleend. Dit proces kan worden gedaan via de interface van de Access control (IAM) in Azure portal. De naam van de beheerde toepassing of **gebruiker toegewezen identiteit** om toe te voegen een roltoewijzing kan worden doorzocht.

![Roltoewijzing voor beheerde toepassingen toevoegen](./media/publish-managed-identity/identity-role-assignment.png)

## <a name="linking-existing-azure-resources"></a>Koppelen van bestaande Azure-resources

> [!NOTE]
> Een **gebruiker toegewezen identiteit** moet [geconfigureerd](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) voordat de beheerde toepassing wordt geïmplementeerd. Gekoppelde resource-implementatie van beheerde toepassingen wordt bovendien alleen ondersteund voor de **marketplace** type.

Beheerde identiteit kan ook worden gebruikt om een beheerde toepassing waarvoor toegang tot bestaande resources tijdens de implementatie vereist te implementeren. Wanneer de beheerde toepassing is ingericht door de klant, **gebruiker toegewezen identiteiten** kunnen worden toegevoegd voor aanvullende machtigingen aan de **mainTemplate** implementatie.

### <a name="authoring-the-createuidefinition-with-a-linked-resource"></a>Het CreateUIDefinition met een gekoppelde resource ontwerpen

Bij het koppelen van de implementatie van de beheerde toepassing met bestaande bronnen, zowel de bestaande Azure-resource en een **gebruiker toegewezen identiteit** met de toepasselijke rol toewijzing voor die bron moet worden opgegeven.

 Een voorbeeld van een CreateUIDefinition waarvoor twee invoeren: een netwerkinterface resource-ID en een gebruiker toegewezen identiteit resource-id.

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
                "name": "managedApplicationSetting",
                "label": "Managed Application Settings",
                "subLabel": {
                    "preValidation": "Managed Application Settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Managed Application Settings",
                "elements": [
                    {
                        "name": "networkInterfaceId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "network interface resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Network/networkInterfaces/existingnetworkinterface",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.Network/networkInterfaces/networkinterface1",
                        "visible": true
                    },
                    {
                        "name": "userAssignedId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "user assigned identity resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/identity1",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "existingNetworkInterfaceId": "[steps('managedApplicationSetting').networkInterfaceId]",
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('managedApplicationSetting').userAssignedId),':{}}}'))]"
        }
    }
}
```

Deze CreateUIDefinition.json genereert een gebruikerservaring maken die twee velden bevat. Het eerste veld kan de gebruiker in te voeren in de Azure-resource-ID voor de resource wordt gekoppeld aan de implementatie van de beheerde toepassing. De tweede is voor een gebruiker in te voeren de **gebruiker toegewezen identiteit** Azure-resource-ID, die toegang tot de gekoppelde Azure-resource heeft. De gegenereerde ervaring zou er als volgt uitzien:

![Voorbeeld van CreateUIDefinition met twee invoeren: een netwerkinterface resource-ID en een gebruiker toegewezen identiteit resource-ID](./media/publish-managed-identity/network-interface-cuid.png)

### <a name="authoring-the-maintemplate-with-a-linked-resource"></a>De mainTemplate met een gekoppelde resource ontwerpen

Naast het CreateUIDefinition bijwerken, moet de belangrijkste sjabloon ook worden bijgewerkt voor het accepteren van de doorgegeven in de gekoppelde resource-ID. De belangrijkste sjabloon kan worden bijgewerkt voor het accepteren van de nieuwe uitvoer door een nieuwe parameter toe te voegen. Omdat de `managedIdentity` uitvoer overschrijft de waarde van de gegenereerde sjabloon van beheerde toepassingen, niet wordt doorgegeven aan de belangrijkste sjabloon en niet moeten worden opgenomen in de parametersectie.

Een belangrijke voorbeeldsjabloon die aan een bestaande netwerkinterface die is geleverd door het CreateUIDefinition Hiermee stelt u het netwerkprofiel.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "existingNetworkInterfaceId": { "type": "string" }
    },
    "variables": {
    },
    "resources": [
        {
            "apiVersion": "2016-04-30-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "myLinkedResourceVM",
            "location": "[resourceGroup().location]",
            "properties": {
                …,
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[parameters('existingNetworkInterfaceId')]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="consuming-the-managed-application-with-a-linked-resource"></a>De beheerde toepassing met een gekoppelde resource verbruikt

Zodra de beheerde toepassingen-pakket is gemaakt, kan de beheerde toepassing via Azure portal worden gebruikt. Voordat deze kan worden gebruikt, zijn er enkele vereiste stappen.

- Een exemplaar van de vereiste gekoppelde Azure-resource moet worden gemaakt.
- De **gebruiker toegewezen identiteit** moet [gemaakt en bepaalde roltoewijzingen](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) naar de gekoppelde resource.
- De bestaande gekoppelde resource-ID en de **gebruiker toegewezen identiteit** ID aan de CreateUIDefinition worden verstrekt.

## <a name="accessing-the-managed-identity-token"></a>Toegang tot het token beheerde identiteit

Het token van de beheerde toepassing kan nu worden geopend via de `listTokens` -api van de tenant van de uitgever. Een van de voorbeeldaanvraag kan er als volgt uitzien:

``` HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}/listTokens?api-version=2018-09-01-preview HTTP/1.1

{
    "authorizationAudience": "https://management.azure.com/",
    "userAssignedIdentities": [
        "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{userAssignedIdentityName}"
    ]
}
```

Parameters van de hoofdtekst van aanvraag:

Parameter | Vereist | Description
---|---|---
authorizationAudience | *no* | De App-ID-URI van de doelresource. Het is ook de `aud` claim (doelgroep) van de uitgegeven tokens. De standaardwaarde is "https://management.azure.com/"
userAssignedIdentities | *no* | De lijst met beheerde identiteiten om op te halen van een token voor de gebruiker toegewezen. Indien niet opgegeven, `listTokens` het token voor het systeem toegewezen beheerde identiteit wordt geretourneerd.


Een voorbeeldantwoord kan er als volgt uitzien:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json

{
    "value": [
        {
            "access_token": "eyJ0eXAi…",
            "expires_in": "2…",
            "expires_on": "1557…",
            "not_before": "1557…",
            "authorizationAudience": "https://management.azure.com/",
            "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}",
            "token_type": "Bearer"
        }
    ]
}
```

Het antwoord bevat een matrix van tokens onder de `value` eigenschap:

Parameter | Description
---|---
access_token | Het aangevraagde toegangstoken.
expires_in | Het aantal seconden dat het toegangstoken ongeldig is.
expires_on | De timespan wanneer het toegangstoken is verlopen. Dit wordt weergegeven als het aantal seconden van epoche.
not_before | De timespan wanneer het toegangstoken wordt van kracht. Dit wordt weergegeven als het aantal seconden van epoche.
authorizationAudience | De `aud` (doelgroep) het toegangstoken is een aanvraag voor. Dit is hetzelfde als is opgegeven de `listTokens` aanvraag.
resourceId | De Azure-resource-ID voor de uitgegeven tokens. Dit is de beheerde toepassings-ID of id van de gebruiker toegewezen identiteit.
token_type | Het type van het token.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Over het configureren van een beheerde toepassing met een aangepaste Provider](./custom-providers-overview.md)

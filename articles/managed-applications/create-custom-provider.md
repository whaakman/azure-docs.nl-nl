---
title: Resourceprovider maken met Azure aangepaste Providers Preview
description: Beschrijft hoe u een resourceprovider maken en implementeren van de aangepaste resource-typen.
services: managed-applications
author: MSEvanhi
ms.service: managed-applications
ms.topic: tutorial
ms.date: 05/01/2019
ms.author: evanhi
ms.openlocfilehash: e4196cf59537be5194ceb510a1b7b066c97de19a
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65410222"
---
# <a name="tutorial-create-custom-provider-and-deploy-custom-resources"></a>Zelfstudie: Aangepaste provider maken en implementeren van aangepaste resources

In deze zelfstudie hebt u uw eigen resourceprovider voor maken en implementeren aangepaste resourcetypen die resourceprovider. Zie voor meer informatie over aangepaste providers [Azure aangepaste Providers Preview overzicht](custom-providers-overview.md).

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in deze zelfstudie hebt voltooid, moet u voor het aanroepen van REST-bewerkingen. Er zijn [REST-aanvragen verzendt op verschillende manieren](/rest/api/azure/). Als u een hulpprogramma voor de REST-bewerkingen nog geen hebt, installeert u [ARMClient](https://github.com/projectkudu/ARMClient). Er is een open-source-opdrachtregelprogramma dat vereenvoudigt het aanroepen van de Azure Resource Manager-API.

## <a name="deploy-custom-provider"></a>Aangepaste provider implementeren

Als u de aangepaste provider instelt, implementeert u een [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/custom-providers/customprovider.json) aan uw Azure-abonnement.

Na de implementatie van de sjabloon voor heeft uw abonnement de volgende bronnen:

* Functie-App met de bewerkingen voor de resources en acties.
* Storage-Account voor het opslaan van gebruikers die zijn gemaakt via de aangepaste provider.
* Aangepaste Provider die de aangepaste resourcetypen en acties definieert. Het eindpunt van de functie-app wordt gebruikt voor het verzenden van aanvragen.
* Aangepaste resource in de aangepaste provider.

Voor het implementeren van de aangepaste provider met PowerShell, gebruikt u:

```azurepowershell-interactive
$rgName = "<resource-group-name>"
$funcName = "<function-app-name>"

New-AzResourceGroup -Name $rgName -Location eastus
New-AzResourceGroupDeployment -ResourceGroupName $rgName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/custom-providers/customprovider.json `
  -funcname $funcName
```

Of u kunt de oplossing met de volgende knop implementeren:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-docs-json-samples%2Fmaster%2Fcustom-providers%2Fcustomprovider.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="view-custom-provider-and-resource"></a>Aangepaste weergave-provider en de resource

In de portal is de aangepaste provider verborgen resourcetype. Om te bevestigen dat de resourceprovider is geïmplementeerd, gaat u naar de resourcegroep. Selecteer de optie voor **verborgen typen weergeven**.

![Verborgen typen weergeven](./media/create-custom-providers/show-hidden.png)

Als u wilt zien van het type aangepaste resource die u hebt geïmplementeerd, gebruikt de GET-bewerking op het resourcetype.

```
GET https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users?api-version=2018-09-01-preview
```

Met ARMClient, gebruiken:

```powershell
$subID = (Get-AzContext).Subscription.Id
$requestURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users?api-version=2018-09-01-preview"

armclient GET $requestURI
```

U krijgt de reactie:

```json
{
  "value": [
    {
      "properties": {
        "provisioningState": "Succeeded",
        "FullName": "Santa Claus",
        "Location": "NorthPole"
      },
      "id": "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/santa",
      "name": "santa",
      "type": "Microsoft.CustomProviders/resourceProviders/users"
    }
  ]
}
```

## <a name="call-action"></a>Oproepactie

Uw aangepaste provider heeft ook een actie met de naam **ping**. De code die de aanvraag verwerkt is in de functie-app geïmplementeerd. De ping-actie antwoorden met een begroeting.

Gebruik de POST-bewerking op uw aangepaste provider voor het verzenden van een ping-aanvraag.

```
POST https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/ping?api-version=2018-09-01-preview
```

Met ARMClient, gebruiken:

```powershell
$pingURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/ping?api-version=2018-09-01-preview"

armclient POST $pingURI
```

U krijgt de reactie:

```json
{
  "pingcontent": {
    "source": "<function-name>.azurewebsites.net"
  },
  "message": "hello <function-name>.azurewebsites.net"
}
```

## <a name="create-resource-type"></a>Brontype maken

U kunt de resource in een sjabloon implementeren voor het maken van het aangepaste brontype. Deze aanpak wordt weergegeven in de sjabloon die u in deze zelfstudie hebt geïmplementeerd. U kunt ook een PUT-aanvraag verzenden voor het brontype.

```
PUT https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/<resource-name>?api-version=2018-09-01-preview

{"properties":{"FullName": "Test User", "Location": "Earth"}}
```

Met ARMClient, gebruiken:

```powershell
$addURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users/testuser?api-version=2018-09-01-preview"
$requestBody = "{'properties':{'FullName': 'Test User', 'Location': 'Earth'}}"

armclient PUT $addURI $requestBody
```

U krijgt de reactie:

```json
{
  "properties": {
    "provisioningState": "Succeeded",
    "FullName": "Test User",
    "Location": "Earth"
  },
  "id": "/subscriptions/<sub-ID>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/testuser",
  "name": "testuser",
  "type": "Microsoft.CustomProviders/resourceProviders/users"
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor een inleiding tot aangepaste providers, [Azure aangepaste Providers Preview overzicht](custom-providers-overview.md).

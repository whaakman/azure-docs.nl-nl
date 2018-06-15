---
title: Veelvoorkomende fouten van de Azure-implementatie oplossen | Microsoft Docs
description: Beschrijft hoe u veelvoorkomende fouten oplossen wanneer u resources in Azure met Azure Resource Manager implementeert.
services: azure-resource-manager
documentationcenter: ''
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: Fout in de implementatie, azure-implementatie implementeren in azure
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/08/2018
ms.author: tomfitz
ms.openlocfilehash: 3ecc1a9557c7854a0771decb3cc7f7597bcd87dd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34360016"
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager oplossen

In dit artikel beschrijft een aantal veelvoorkomende fouten Azure-implementatie u mogelijk stuit en biedt informatie waarmee de fouten op te lossen. Als u de foutcode voor uw implementatiefout vinden kan, raadpleegt u [foutcode vinden](#find-error-code).

## <a name="error-codes"></a>Foutcodes

| Foutcode | Oplossing | Meer informatie |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Ga als volgt naamsbeperkingen voor opslagaccounts. | [Naam van het opslagaccount oplossen](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Controleer de eigenschappen van het opslagaccount beschikbaar. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AllocationFailed | Het cluster of de regio is geen beschikbare bronnen of het aangevraagde VM-grootte kan niet worden ondersteund. De aanvraag op een later tijdstip opnieuw proberen of vraag een andere VM-grootte. | [Inrichten en de toewijzing van problemen voor Linux](../virtual-machines/linux/troubleshoot-deployment-new-vm.md), [inrichten en de toewijzing van problemen voor Windows](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) en [toewijzingsfouten](../virtual-machines/windows/allocation-failure.md)|
| AnotherOperationInProgress | Wacht u totdat de gelijktijdige bewerking is voltooid. | |
| AuthorizationFailed | Uw account of de service-principal heeft niet voldoende toegangsrechten voor het voltooien van de implementatie. Controleer de uw account behoort tot rol en de toegang voor de implementatie-scope. | [Op rollen gebaseerde toegangsbeheer van Azure](../role-based-access-control/role-assignments-portal.md) |
| BadRequest | U verzonden implementatie-waarden die niet overeenkomen met wat er wordt verwacht door Resource Manager. Controleer de binnenste statusbericht voor meer informatie over het oplossen van problemen. | [Verwijzing naar de sjabloon](/azure/templates/) en [locaties ondersteund](resource-manager-templates-resources.md#location) |
| Conflict | U hebt aangevraagd een bewerking die niet is toegestaan in de huidige status van de resource. De schijfgrootte is mag bijvoorbeeld alleen bij het maken van een virtuele machine of wanneer de toewijzing van de VM ongedaan is gemaakt. | |
| DeploymentActive | Wacht voor gelijktijdige implementatie aan deze resourcegroep te voltooien. | |
| Implementatie mislukt | De implementatie mislukt-fout is een algemene fout die biedt geen informatie die u nodig hebt voor het oplossen van de fout. Raadpleeg de foutdetails voor een foutcode die vindt u meer informatie. | [Foutcode vinden](#find-error-code) |
| DeploymentQuotaExceeded | Als u de limiet van 800 implementaties per resourcegroep bereiken, kunt u implementaties verwijderd uit de geschiedenis die niet langer nodig zijn. U kunt items verwijderen uit de geschiedenis met [az implementatie verwijderen](/cli/azure/group/deployment#az_group_deployment_delete) voor Azure CLI of [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) in PowerShell. Verwijderen van een vermelding uit de implementatiegeschiedenis van de heeft geen invloed op de resources implementeren. | |
| DnsRecordInUse | De DNS-recordnaam moet uniek zijn. Geef een andere naam, of de bestaande record wijzigen. | |
| ImageNotFound | Controleer de instellingen van de VM-installatiekopie. |  |
| InUseSubnetCannotBeDeleted | U kunt deze fout kan optreden bij het bijwerken van een bron, maar de aanvraag wordt verwerkt door verwijderen en het maken van de resource. Zorg ervoor dat alle ongewijzigde waarden opgeven. | [Bron bijwerken](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Haal het toegangstoken voor de juiste tenant. U kunt alleen het token ophalen van de tenant die uw account hoort bij. | |
| InvalidContentLink | U hebt waarschijnlijk geprobeerd om te koppelen aan een geneste sjabloon die niet beschikbaar. Controleer de URI die u hebt opgegeven voor de geneste sjabloon. Als de sjabloon in een opslagaccount bestaat, zorg er dan voor dat de URI is toegankelijk. U moet mogelijk een SAS-token doorgeven. | [gekoppelde sjablonen](resource-group-linked-templates.md) |
| InvalidParameter | Een van de waarden die u hebt opgegeven voor een resource komt niet overeen met de verwachte waarde. Deze fout kan leiden tot uit veel verschillende voorwaarden. Bijvoorbeeld, een wachtwoord is mogelijk onvoldoende of een blob-naam is mogelijk onjuist. Controleer het foutbericht om te bepalen welke waarde moet worden gecorrigeerd. | |
| InvalidRequestContent | De waarden van uw implementatie die hetzij bevatten waarden die niet wordt verwacht of ontbreken vereiste waarden. Controleer de waarden voor het brontype. | [Verwijzing naar de sjabloon](/azure/templates/) |
| InvalidRequestFormat | Logboekregistratie voor foutopsporing in te schakelen wanneer de implementatie wordt uitgevoerd en controleer de inhoud van de aanvraag. | [Logboekregistratie voor foutopsporing](#enable-debug-logging) |
| InvalidResourceNamespace | Controleer de resourcenaamruimte die u hebt opgegeven in de **type** eigenschap. | [Verwijzing naar de sjabloon](/azure/templates/) |
| InvalidResourceReference | De resource nog niet bestaat of niet juist wordt verwezen. Controleer of u moet een afhankelijkheid toevoegen. Controleer uw gebruik van de **verwijzing** functie bevat de vereiste parameters voor uw scenario. | [Afhankelijkheden moeten worden opgelost](resource-manager-not-found-errors.md) |
| InvalidResourceType | Controleer de resource typt u hebt opgegeven in de **type** eigenschap. | [Verwijzing naar de sjabloon](/azure/templates/) |
| InvalidSubscriptionRegistrationState | Uw abonnement bij de resourceprovider registreren. | [Inschrijving oplossen](resource-manager-register-provider-errors.md) |
| InvalidTemplate | Controleer de sjabloonsyntaxis van de op fouten. | [Ongeldige sjabloon oplossen](resource-manager-invalid-template-errors.md) |
| InvalidTemplateCircularDependency | Verwijder overbodige afhankelijkheden. | [Circulaire afhankelijkheden moeten worden opgelost](resource-manager-invalid-template-errors.md#circular-dependency) |
| LinkedAuthorizationFailed | Controleer of uw account hoort bij dezelfde tenant als de resourcegroep die u implementeert. | |
| LinkedInvalidPropertyId | De resource-ID voor een bron is niet correct kan omzetten. Controleer dat u alle vereiste waarden voor de resource-ID opgeven, met inbegrip van de abonnements-ID, naam van een resourcegroep, resourcetype, bovenliggende Resourcenaam (indien nodig) en resourcenaam. | |
| LocationRequired | Geef een locatie voor uw resource. | [Locatie instellen](resource-manager-templates-resources.md#location) |
| MismatchingResourceSegments | Zorg ervoor dat de geneste resource heeft een juiste aantal segmenten in de naam en type. | [Resource segmenten oplossen](resource-manager-invalid-template-errors.md#incorrect-segment-lengths)
| MissingRegistrationForLocation | Controleer de registratiestatus van de resource-provider en de ondersteunde locaties. | [Inschrijving oplossen](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Uw abonnement bij de resourceprovider registreren. | [Inschrijving oplossen](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Registratiestatus resource provider. | [Inschrijving oplossen](resource-manager-register-provider-errors.md) |
| Niet gevonden | U probeert mogelijk voor het implementeren van een afhankelijke bron in combinatie met een bovenliggende resource. Controleer of u moet een afhankelijkheid toevoegen. | [Afhankelijkheden moeten worden opgelost](resource-manager-not-found-errors.md) |
| OperationNotAllowed | De implementatie wordt geprobeerd een bewerking die het quotum voor het abonnement, resourcegroep of regio overschrijdt. Indien mogelijk uw implementatie om te blijven binnen de quota herzien. Overweeg anders de aanvragen van een wijziging in uw quota's. | [Quota's oplossen](resource-manager-quota-errors.md) |
| ParentResourceNotFound | Zorg ervoor dat een bovenliggende resource bestaat voordat het maken van de onderliggende resources. | [Bovenliggende resource oplossen](resource-manager-parent-resource-errors.md) |
| PrivateIPAddressInReservedRange | Het opgegeven IP-adres bevat een adresbereik vereist door Azure. IP-adres om te voorkomen dat gereserveerde bereik wijzigen. | [IP-adressen](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | Het opgegeven IP-adres is buiten het subnetbereik. IP-adres valt binnen het subnetbereik wijzigen. | [IP-adressen](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | Sommige eigenschappen kunnen niet worden gewijzigd voor een geïmplementeerde bron. Bij het bijwerken van een resource, beperkt u uw wijzigingen in de toegestane eigenschappen. | [Bron bijwerken](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | Uw abonnement omvat een bronbeleid een actie die u probeert uit te voeren tijdens de implementatie wordt verhinderd. Zoek het beleid dat de actie wordt geblokkeerd. Indien mogelijk uw implementatie om te voldoen aan de beperkingen van het beleid wijzigen. | [Beleid oplossen](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ReservedResourceName | Geef de naam van een resource die geen een gereserveerde naam omvatten. | [Gereserveerde resourcenamen](resource-manager-reserved-resource-name.md) |
| ResourceGroupBeingDeleted | Wachttijd voor verwijderen is voltooid. | |
| ResourceGroupNotFound | Controleer de naam van de brongroep van doel voor de implementatie. Het moet al bestaan in uw abonnement. Controleer de context van uw abonnement. | [Azure CLI](/cli/azure/account?#az_account_set) [PowerShell](/powershell/module/azurerm.profile/set-azurermcontext) |
| ResourceNotFound | Uw implementatie verwijst naar een resource die niet kan worden omgezet. Controleer uw gebruik van de **verwijzing** functie bevat de vereiste parameters voor uw scenario. | [Verwijzingen oplossen](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | De implementatie is bij het maken van resources die groter is dan het quotum voor het abonnement, resourcegroep of regio. Indien mogelijk uw infrastructuur om te blijven binnen de quota herzien. Overweeg anders de aanvragen van een wijziging in uw quota's. | [Quota's oplossen](resource-manager-quota-errors.md) |
| SkuNotAvailable | Selecteer de SKU (zoals VM-grootte) die beschikbaar is voor de locatie die u hebt geselecteerd. | [SKU oplossen](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | Geef een unieke naam op voor het opslagaccount. | [Naam van het opslagaccount oplossen](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | Geef een unieke naam op voor het opslagaccount. | [Naam van het opslagaccount oplossen](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | Controleer het abonnement, de resourcegroep en de naam van het opslagaccount dat u probeert te gebruiken. | |
| SubnetsNotInSameVnet | Een virtuele machine kan slechts één virtueel netwerk hebben. Bij het implementeren van meerdere NIC's, zorg er dan voor dat ze deel uitmaken van hetzelfde virtuele netwerk. | [Meerdere NIC 's](../virtual-machines/windows/multiple-nics.md) |
| TemplateResourceCircularDependency | Verwijder overbodige afhankelijkheden. | [Circulaire afhankelijkheden moeten worden opgelost](resource-manager-invalid-template-errors.md#circular-dependency) |
| TooManyTargetResourceGroups | Verminder het aantal resourcegroepen voor een enkele implementatie. | [Implementatie in meerdere resourcegroepen](resource-manager-cross-resource-group-deployment.md) |

## <a name="find-error-code"></a>Foutcode vinden

Er zijn twee typen fouten die u kunt ontvangen:

* Validatiefouten
* Implementatiefouten

Validatiefouten worden veroorzaakt door scenario's die vóór de implementatie kunnen worden bepaald. Ze bevatten syntaxisfouten in de sjabloon of het implementeren van resources die uw abonnement quota's zou overschrijden. Implementatiefouten worden veroorzaakt door de voorwaarden die tijdens de implementatie optreden. Ze bevatten toegang wilt krijgen tot een resource die parallel wordt geïmplementeerd.

Beide soorten fouten retourneren een foutcode die u gebruikt voor het oplossen van de implementatie. Beide soorten fouten worden weergegeven in de [activiteitenlogboek](resource-group-audit.md). Validatiefouten worden echter niet weergegeven in de geschiedenis van uw implementatie omdat de implementatie is nooit gestart.

### <a name="validation-errors"></a>Validatiefouten

Bij het implementeren via de portal, ziet u een validatiefout opgetreden na het verzenden van uw waarden.

![Fout bij de portal validatie weergeven](./media/resource-manager-common-deployment-errors/validation-error.png)

Selecteer het bericht voor meer informatie. In de volgende afbeelding ziet u een **InvalidTemplateDeployment** fout en een bericht verschijnt dat een beleid voor implementatie wordt geblokkeerd.

![validatie details weergeven](./media/resource-manager-common-deployment-errors/validation-details.png)

### <a name="deployment-errors"></a>Implementatiefouten

Als de bewerking is geslaagd, maar niet tijdens de implementatie, ziet u de fout in de meldingen. Selecteer de melding.

![Fout bij wijzigingsbericht](./media/resource-manager-common-deployment-errors/notification.png)

Er is meer informatie over de implementatie. Selecteer de optie voor meer informatie over de fout.

![implementatie is mislukt](./media/resource-manager-common-deployment-errors/deployment-failed.png)

Ziet u de foutmelding en foutcodes. Er zijn twee foutcodes. De eerste foutcode (**implementatie mislukt**) is een algemene fout die geen biedt informatie die u nodig hebt voor het oplossen van de fout. De tweede foutcode (**StorageAccountNotFound**) bevat de details die u nodig hebt. 

![foutgegevens](./media/resource-manager-common-deployment-errors/error-details.png)

## <a name="enable-debug-logging"></a>Inschakelen van logboekregistratie voor foutopsporing

Soms moet u meer informatie over de aanvraag en -antwoord voor meer informatie over wat er mis ging. U kunt met behulp van PowerShell of Azure CLI aanvragen dat aanvullende informatie wordt geregistreerd tijdens een implementatie.

- PowerShell

   In PowerShell, stelt u de **DeploymentDebugLogLevel** parameter naar alle, ResponseContent of RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Controleer de aanvraag van inhoud met de volgende cmdlet:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Of het antwoord inhoud met:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Deze informatie kunt u bepalen of een waarde in de sjabloon niet correct wordt ingesteld.

- Azure-CLI

   Controleer de implementatiebewerkingen met de volgende opdracht:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- Geneste sjabloon

   Meld u foutopsporingsgegevens voor een geneste sjabloon met de **debugSetting** element.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```

## <a name="create-a-troubleshooting-template"></a>Maken van een sjabloon voor het oplossen van problemen

In sommige gevallen is de eenvoudigste manier om op te lossen, de sjabloon voor het testen van de onderdelen hiervan. U kunt maken als een vereenvoudigde sjabloon waarmee u zich kunt richten op het onderdeel dat u denkt dat de fout wordt veroorzaakt. Stel bijvoorbeeld dat u ontvangt een fout opgetreden bij het verwijzen naar een resource. In plaats van te moeten omgaan met een volledige sjabloon, een sjabloon die als resultaat geeft het onderdeel dat wordt veroorzaakt door het probleem te maken. Dit kunt u bepalen of u doorgeeft aan de juiste parameters met sjabloonfuncties correct en ophalen van de resource die u verwacht.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Of stel fouten implementatie die u van mening zijn gerelateerd bent aan afhankelijkheden niet correct instellen. Test uw sjabloon door in vereenvoudigde sjablonen op te splitsen. Maak eerst een sjabloon die slechts één bron (zoals een SQL-Server) implementeert. Als u zeker dat u die correct gedefinieerd resource hebt, kunt u een resource die afhankelijk zijn van deze (zoals een SQL-Database) toevoegen. Wanneer u deze twee bronnen onjuist is gedefinieerd hebt, moet u andere afhankelijke resources (zoals controlebeleid) toevoegen. Verwijder de resourcegroep om te controleren of u voldoende testen van de afhankelijkheden tussen elke testimplementatie.


## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het controleren van acties, [bewerkingen met Resource Manager controleren](resource-group-audit.md).
* Zie voor meer informatie over acties om de fouten tijdens de implementatie, [implementatiebewerkingen weergeven](resource-manager-deployment-operations.md).

---
title: Veelvoorkomende fouten voor Azure-implementatie oplossen | Microsoft Docs
description: Beschrijft hoe u veelvoorkomende fouten oplossen wanneer u resources implementeren op Azure met behulp van Azure Resource Manager.
services: azure-resource-manager
documentationcenter: ''
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: implementatiefout optreedt, azure-implementatie implementeren in azure
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2019
ms.author: tomfitz
ms.openlocfilehash: f6ebeb1d9953311ad1cb85d8ab33c83d5e92d687
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57405518"
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager

Dit artikel beschrijft een aantal veelvoorkomende fouten in de Azure-implementatie, en bevat informatie om op te lossen de fouten. Als u de foutcode niet voor uw implementatiefout optreedt vinden, raadpleegt u [foutcode vinden](#find-error-code).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="error-codes"></a>Foutcodes

| Foutcode | Oplossing | Meer informatie |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Ga als volgt naamsbeperkingen voor storage-accounts. | [De naam van opslagaccount oplossen](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Controleer de eigenschappen van het opslagaccount beschikbaar. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AllocationFailed | Het cluster of de regio geen beschikbare bronnen of kan de aangevraagde VM-grootte niet ondersteunen. De aanvraag opnieuw uitvoeren op een later tijdstip, of vraag een andere VM-grootte. | [Problemen met inrichten en de toewijzing voor Linux](../virtual-machines/linux/troubleshoot-deployment-new-vm.md), [problemen met inrichten en de toewijzing voor Windows](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) en [toewijzingsfouten oplossen](../virtual-machines/troubleshooting/allocation-failure.md)|
| AnotherOperationInProgress | Wachten op voor de gelijktijdige bewerking is voltooid. | |
| AuthorizationFailed | Uw account of service-principal beschikt niet over voldoende toegangsrechten voor het voltooien van de implementatie. Controleer de rol van die uw account behoort tot en de toegang voor de implementatie-scope.<br><br>U kunt deze fout kan optreden als een vereiste resourceprovider is niet geregistreerd. | [Azure Role-Based Access Control](../role-based-access-control/role-assignments-portal.md)<br><br>[Registratie oplossen](resource-manager-register-provider-errors.md) |
| BadRequest | U hebt verzonden implementatie waarden die niet overeenkomen met wat er door Resource Manager wordt verwacht. Controleer de binnenste statusbericht voor hulp bij het oplossen van problemen. | [Sjabloonverwijzing](/azure/templates/) en [ondersteunde locaties](resource-group-authoring-templates.md#resource-location) |
| Conflict | U vraagt om een bewerking die in de huidige status van de resource is niet toegestaan. Bijvoorbeeld, de schijfgrootte mag alleen als het maken van een virtuele machine of als de VM ongedaan is gemaakt. | |
| DeploymentActive | Wachten op voor gelijktijdige implementatie aan deze resourcegroep te voltooien. | |
| DeploymentFailed | De implementatie mislukt-fout is een algemene fout dat biedt geen informatie die u nodig hebt voor het oplossen van de fout. Zoek in de foutdetails voor een foutcode die vindt u meer informatie. | [Foutcode vinden](#find-error-code) |
| DeploymentQuotaExceeded | Als u de limiet van 800 implementaties per resourcegroep bereikt, verwijdert u implementaties uit de geschiedenis die niet meer nodig zijn. U kunt items verwijderen uit de geschiedenis met [az group deployment verwijderen](/cli/azure/group/deployment#az-group-deployment-delete) voor Azure CLI of [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) in PowerShell. Een vermelding verwijderen uit de implementatiegeschiedenis heeft geen invloed op de resources implementeren. | |
| DnsRecordInUse | De naam van de DNS-record moet uniek zijn. U kunt een andere naam of de bestaande record wijzigen. | |
| ImageNotFound | Controleer de instellingen van de VM-installatiekopie. |  |
| InUseSubnetCannotBeDeleted | U krijgt deze fout tijdens het bijwerken van een resource, maar de aanvraag wordt verwerkt door te verwijderen en het maken van de resource. Zorg ervoor dat alle ongewijzigd waarden op te geven. | [Bron bijwerken](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Toegangstoken verkrijgen voor de juiste tenant. U kunt alleen het token ophalen van de tenant die uw account tot behoort. | |
| InvalidContentLink | U hebt waarschijnlijk geprobeerd om te koppelen aan een geneste sjabloon die is niet beschikbaar. Controleer de URI die u hebt opgegeven voor de geneste sjabloon. Als de sjabloon in een storage-account bestaat, zorg er dan voor dat de URI is toegankelijk. Het is mogelijk om door te geven van een SAS-token. | [Gekoppelde sjablonen](resource-group-linked-templates.md) |
| InvalidParameter | Een van de waarden die u hebt opgegeven voor een resource komt niet overeen met de verwachte waarde. Deze fout kan worden veroorzaakt door veel verschillende voorwaarden. Bijvoorbeeld, een wachtwoord is mogelijk onvoldoende of een blobnaam is mogelijk onjuist. Controleer het foutbericht om te bepalen welke waarde moet worden gecorrigeerd. | |
| InvalidRequestContent | De waarden in de implementatie die een bevatten waarden die niet worden verwacht of er ontbreken vereiste waarden. Controleer of de waarden voor uw resourcetype. | [Sjabloonverwijzing](/azure/templates/) |
| InvalidRequestFormat | Logboekregistratie voor foutopsporing inschakelen bij het uitvoeren van de implementatie en controleer of de inhoud van de aanvraag. | [Logboekregistratie voor foutopsporing](#enable-debug-logging) |
| InvalidResourceNamespace | Controleer de resourcenaamruimte die u hebt opgegeven in de **type** eigenschap. | [Sjabloonverwijzing](/azure/templates/) |
| InvalidResourceReference | De resource nog niet bestaat of niet juist wordt verwezen. Controleer of u wilt toevoegen van een afhankelijkheid. Controleer uw gebruik van de **verwijzing** functie bevat de vereiste parameters voor uw scenario. | [Omzetten van afhankelijkheden](resource-manager-not-found-errors.md) |
| InvalidResourceType | Controleer de resource typt u hebt opgegeven in de **type** eigenschap. | [Sjabloonverwijzing](/azure/templates/) |
| InvalidSubscriptionRegistrationState | Uw abonnement met de resourceprovider registreren. | [Registratie oplossen](resource-manager-register-provider-errors.md) |
| InvalidTemplate | Controleer de sjabloonsyntaxis van de op fouten. | [Ongeldige sjabloon oplossen](resource-manager-invalid-template-errors.md) |
| InvalidTemplateCircularDependency | Verwijder overbodige afhankelijkheden. | [Circulaire afhankelijkheden oplossen](resource-manager-invalid-template-errors.md#circular-dependency) |
| LinkedAuthorizationFailed | Als uw account tot dezelfde tenant als de resourcegroep die u behoort naar implementeert controleren | |
| LinkedInvalidPropertyId | De resource-ID voor een resource is niet correct kan omzetten. Controleer dat u alle vereiste waarden voor de resource-ID opgeven, met inbegrip van de abonnements-ID, naam van de resourcegroep, resourcetype, bovenliggende Resourcenaam (indien nodig) en resourcenaam. | |
| LocationRequired | Geef een locatie voor uw resource. | [Locatie instellen](resource-group-authoring-templates.md#resource-location) |
| MismatchingResourceSegments | Zorg ervoor dat geneste resource is juist aantal segmenten in de naam en type. | [Resource-segmenten oplossen](resource-manager-invalid-template-errors.md#incorrect-segment-lengths)
| MissingRegistrationForLocation | Controleer de status van de registratie van resourceprovider en ondersteunde locaties. | [Registratie oplossen](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Uw abonnement met de resourceprovider registreren. | [Registratie oplossen](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Controleer de status van de registratie van resourceprovider. | [Registratie oplossen](resource-manager-register-provider-errors.md) |
| NotFound | Mogelijk probeert u een afhankelijke resource in combinatie met een bovenliggende resource implementeren. Controleer of u moet een afhankelijkheid toevoegen. | [Omzetten van afhankelijkheden](resource-manager-not-found-errors.md) |
| OperationNotAllowed | De implementatie, is er wordt geprobeerd een bewerking die het quotum voor het abonnement, resourcegroep of regio overschrijdt. Wijzig, indien mogelijk, uw implementatie om binnen de quota te blijven. Overweeg anders aanvragen van een wijziging in uw quota's. | [Quota's oplossen](resource-manager-quota-errors.md) |
| ParentResourceNotFound | Zorg ervoor dat een bovenliggende resource bestaat voor het maken van de onderliggende resources. | [Bovenliggende resource oplossen](resource-manager-parent-resource-errors.md) |
| PasswordTooLong | Als u een wachtwoord met te veel tekens hebt geselecteerd of mogelijk hebt omgezet in de password-waarde een beveiligde tekenreeks voordat deze wordt doorgegeven als parameter. Als de sjabloon bevat een **beveiligde tekenreeks** parameter, hoeft u niet de waarde te converteren naar een beveiligde tekenreeks. Geef de waarde van het wachtwoord op als tekst. |  |
| PrivateIPAddressInReservedRange | Het opgegeven IP-adres bevat een adresbereik dat is vereist voor Azure. IP-adres om te voorkomen dat gereserveerde bereik wijzigen. | [IP-adressen](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | Het opgegeven IP-adres is buiten het subnetbereik. IP-adres binnen het subnetbereik vallen wijzigen. | [IP-adressen](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | Sommige eigenschappen kunnen niet worden gewijzigd op een geïmplementeerde resource. Tijdens het bijwerken van een resource, beperkt u de wijzigingen in de eigenschappen van de toegestane. | [Bron bijwerken](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | Uw abonnement bevat een resourcebeleid waarmee wordt voorkomen dat een actie die u wilt uitvoeren tijdens de implementatie. Zoek het beleid dat de actie wordt geblokkeerd. Indien mogelijk, wijzigen van uw implementatie om te voldoen aan de beperkingen van het beleid. | [Beleid oplossen](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ReservedResourceName | Geef de naam van een resource die geen een gereserveerde naam. | [Gereserveerde resourcenamen](resource-manager-reserved-resource-name.md) |
| ResourceGroupBeingDeleted | Wachten op verwijderen om te voltooien. | |
| ResourceGroupNotFound | Controleer de naam van de doelresourcegroep voor de implementatie. Het moet al bestaan in uw abonnement. Controleer de context van uw abonnement. | [Azure CLI](/cli/azure/account?#az-account-set) [PowerShell](/powershell/module/Az.Accounts/Set-AzContext) |
| ResourceNotFound | Uw implementatie verwijst naar een resource die kan niet worden omgezet. Controleer uw gebruik van de **verwijzing** functie bevat de vereiste parameters voor uw scenario. | [Referenties oplossen](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | De implementatie is probeert te maken van resources die het quotum voor het abonnement, resourcegroep of regio. Wijzig, indien mogelijk, uw infrastructuur in binnen de quota blijven. Overweeg anders aanvragen van een wijziging in uw quota's. | [Quota's oplossen](resource-manager-quota-errors.md) |
| SkuNotAvailable | Selecteer de SKU (zoals VM-grootte) die beschikbaar is voor de locatie die u hebt geselecteerd. | [SKU oplossen](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | Geef een unieke naam op voor het opslagaccount. | [De naam van opslagaccount oplossen](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | Geef een unieke naam op voor het opslagaccount. | [De naam van opslagaccount oplossen](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | Controleer het abonnement, resourcegroep en de naam van het opslagaccount dat u probeert te gebruiken. | |
| SubnetsNotInSameVnet | Een virtuele machine kan slechts één virtueel netwerk hebben. Bij het implementeren van meerdere NIC's, zorg er dan voor dat ze deel uitmaken van hetzelfde virtuele netwerk. | [Meerdere NIC 's](../virtual-machines/windows/multiple-nics.md) |
| TemplateResourceCircularDependency | Verwijder overbodige afhankelijkheden. | [Circulaire afhankelijkheden oplossen](resource-manager-invalid-template-errors.md#circular-dependency) |
| TooManyTargetResourceGroups | Verminder het aantal resourcegroepen voor een enkele implementatie. | [Implementatie in meerdere resourcegroepen](resource-manager-cross-resource-group-deployment.md) |

## <a name="find-error-code"></a>Foutcode vinden

Er zijn twee typen fouten die u kunt ontvangen:

* validatiefouten
* fouten bij de implementatie

Validatiefouten voortvloeien uit scenario's die vóór de implementatie kunnen worden bepaald. Ze bevatten syntaxisfouten in de sjabloon of proberen resources te implementeren waarmee uw abonnementquota worden overschreden. Er ontstaan fouten bij de implementatie van voorwaarden die tijdens het implementatieproces optreden. Bijvoorbeeld wanneer deze toegang proberen te krijgen tot een resource die parallel wordt geïmplementeerd.

Beide typen fouten retourneren een foutcode die u gebruikt om de problemen met de implementatie op te lossen. Beide typen fouten worden weergegeven in de [activiteitenlogboek](resource-group-audit.md). Validatiefouten worden echter niet weergegeven in de implementatiegeschiedenis omdat de implementatie niet is gestart.

### <a name="validation-errors"></a>Validatiefouten

Wanneer u implementeert via de portal, ziet u een validatiefout na het indienen van uw waarden.

![Validatiefout van de portal weergeven](./media/resource-manager-common-deployment-errors/validation-error.png)

Selecteer het bericht voor meer informatie. In de volgende afbeelding ziet u een **InvalidTemplateDeployment** fout en het bericht weergegeven dat een beleid voor implementatie wordt geblokkeerd.

![Validatiedetails weergeven](./media/resource-manager-common-deployment-errors/validation-details.png)

### <a name="deployment-errors"></a>fouten bij de implementatie

Als de bewerking is gevalideerd, maar niet tijdens de implementatie, krijgt u een implementatiefout opgetreden.

Als u wilt zien van implementatie-foutcodes en berichten met PowerShell, gebruikt u:

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -DeploymentName exampledeployment -ResourceGroupName examplegroup).Properties.statusMessage
```

Als u wilt zien van implementatie-foutcodes en berichten met Azure CLI, gebruikt u:

```azurecli-interactive
az group deployment operation list --name exampledeployment -g examplegroup --query "[*].properties.statusMessage"
```

Selecteer de melding in de portal.

![Fout met melding](./media/resource-manager-common-deployment-errors/notification.png)

U vindt u meer over de implementatie. Selecteer de optie voor meer informatie over de fout.

![de implementatie is mislukt](./media/resource-manager-common-deployment-errors/deployment-failed.png)

Ziet u het foutbericht en foutcodes. Er zijn twee foutcodes. De eerste foutcode (**implementatie mislukt**) is een algemene fout dat biedt geen informatie die u nodig hebt voor het oplossen van de fout. De tweede foutcode (**StorageAccountNotFound**) bevat de details die u nodig hebt. 

![foutgegevens](./media/resource-manager-common-deployment-errors/error-details.png)

## <a name="enable-debug-logging"></a>Inschakelen van logboekregistratie voor foutopsporing

Soms moet u meer informatie over de aanvraag en respons voor meer informatie over wat er mis ging. Tijdens de implementatie, kunt u aanvragen dat aanvullende gegevens worden geregistreerd tijdens een implementatie. 

### <a name="powershell"></a>PowerShell

Stel in PowerShell, de **DeploymentDebugLogLevel** parameter op alle, obsah ResponseContent of RequestContent.

```powershell
New-AzResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName examplegroup `
  -TemplateFile c:\Azure\Templates\storage.json `
  -DeploymentDebugLogLevel All
```

Controleer de aanvraag van inhoud met de volgende cmdlet:

```powershell
(Get-AzResourceGroupDeploymentOperation `
-DeploymentName exampledeployment `
-ResourceGroupName examplegroup).Properties.request `
| ConvertTo-Json
```

Of het antwoord inhoud met:

```powershell
(Get-AzResourceGroupDeploymentOperation `
-DeploymentName exampledeployment `
-ResourceGroupName examplegroup).Properties.response `
| ConvertTo-Json
```

Deze informatie kunt u bepalen of een waarde in de sjabloon niet correct wordt ingesteld.

### <a name="azure-cli"></a>Azure-CLI

Op dit moment Azure CLI biedt geen ondersteuning voor inschakelen van logboekregistratie voor foutopsporing, maar u kunt logboekregistratie voor foutopsporing ophalen.

Controleer de implementatiebewerkingen met de volgende opdracht uit:

```azurecli
az group deployment operation list \
  --resource-group examplegroup \
  --name exampledeployment
```

Controleer de aanvraag van inhoud met de volgende opdracht:

```azurecli
az group deployment operation list \
  --name exampledeployment \
  -g examplegroup \
  --query [].properties.request
```

Controleer het antwoord inhoud met de volgende opdracht:

```azurecli
az group deployment operation list \
  --name exampledeployment \
  -g examplegroup \
  --query [].properties.response
```

### <a name="nested-template"></a>Geneste sjabloon

Als u wilt gegevens voor foutopsporing voor een geneste sjabloon, gebruiken de **debugSetting** element.

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

In sommige gevallen is de eenvoudigste manier om op te lossen van de sjabloon voor het testen van onderdelen van deze. U kunt maken een eenvoudige sjabloon waarmee u zich kunt richten op het onderdeel dat u denkt dat de fout wordt veroorzaakt. Stel bijvoorbeeld dat u ontvangt een fout op wanneer u verwijst naar een resource. In plaats van omgaan met een volledige sjabloon, een sjabloon die wordt geretourneerd van het onderdeel dat wordt veroorzaakt door het probleem te maken. U kunt bepalen of u bent doorgeven in de juiste parameters met behulp van de sjabloonfuncties correct en ophalen van de resource die u verwacht.

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

Of Stel dat u regelmatig fouten bij de implementatie die u denkt dat ze zijn gerelateerd aan de afhankelijkheden niet correct ingesteld. Test uw sjabloon door te delen in vereenvoudigde sjablonen. Maak eerst een sjabloon die alleen één resource (zoals een SQL-Server) implementeert. Als u zeker dat u hebt deze resource correct gedefinieerd weet, moet u een resource die afhankelijk zijn van het (zoals een SQL-Database) toevoegen. Wanneer u deze twee resources correct is gedefinieerd hebt, andere afhankelijke resources (zoals een controlebeleid) toevoegen. Verwijder de resourcegroep om te controleren of u voldoende testen van de afhankelijkheden tussen elke test-implementatie.


## <a name="next-steps"></a>Volgende stappen

* Als u wilt u een zelfstudie voor problemen oplossen, Zie [zelfstudie: Problemen met sjabloonimplementaties van Resource Manager-oplossen](./resource-manager-tutorial-troubleshoot.md)
* Zie voor meer informatie over het controleren van acties, [bewerkingen controleren met Resource Manager](resource-group-audit.md).
* Zie voor meer informatie over acties voor het bepalen van de fouten tijdens de implementatie, [implementatiebewerkingen bekijken](resource-manager-deployment-operations.md).

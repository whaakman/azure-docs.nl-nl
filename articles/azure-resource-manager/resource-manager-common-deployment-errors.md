---
title: Veelvoorkomende fouten van de Azure-implementatie oplossen | Microsoft Docs
description: Beschrijft hoe u veelvoorkomende fouten oplossen wanneer u resources in Azure met Azure Resource Manager implementeert.
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: Fout in de implementatie, azure-implementatie implementeren in azure
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: tomfitz
ms.openlocfilehash: ca7e3cb541948e6cc0b8d077616f3611e3ab2477
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager oplossen

In dit artikel beschrijft een aantal veelvoorkomende fouten Azure-implementatie u mogelijk stuit en biedt informatie waarmee de fouten op te lossen. Als u de foutcode voor uw implementatiefout vinden kan, raadpleegt u [foutcode vinden](#find-error-code).

## <a name="error-codes"></a>Foutcodes

| Foutcode | Oplossing | Meer informatie |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Ga als volgt naamsbeperkingen voor opslagaccounts. | [Naam van het opslagaccount oplossen](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Controleer de eigenschappen van het opslagaccount beschikbaar. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AllocationFailed | Het cluster of de regio is geen beschikbare bronnen of het aangevraagde VM-grootte kan niet worden ondersteund. De aanvraag op een later tijdstip opnieuw proberen of vraag een andere VM-grootte. | [Inrichten en de toewijzing van problemen voor Linux](../virtual-machines/linux/troubleshoot-deployment-new-vm.md) en [inrichten en de toewijzing van problemen voor Windows](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) |
| AnotherOperationInProgress | Wacht u totdat de gelijktijdige bewerking is voltooid. | |
| AuthorizationFailed | Uw account of de service-principal heeft niet voldoende toegangsrechten voor het voltooien van de implementatie. Controleer de uw account behoort tot rol en de toegang voor de implementatie-scope. | [Op rollen gebaseerde toegangsbeheer van Azure](../active-directory/role-based-access-control-configure.md) |
| BadRequest | U verzonden implementatie-waarden die niet overeenkomen met wat er wordt verwacht door Resource Manager. Controleer de binnenste statusbericht voor meer informatie over het oplossen van problemen. | [Verwijzing naar de sjabloon](/azure/templates/) en [locaties ondersteund](resource-manager-templates-resources.md#location) |
| Conflict | U hebt aangevraagd een bewerking die niet is toegestaan in de huidige status van de resource. De schijfgrootte is mag bijvoorbeeld alleen bij het maken van een virtuele machine of wanneer de toewijzing van de VM ongedaan is gemaakt. | |
| DeploymentActive | Wacht voor gelijktijdige implementatie aan deze resourcegroep te voltooien. | |
| DnsRecordInUse | De DNS-recordnaam moet uniek zijn. Geef een andere naam, of de bestaande record wijzigen. | |
| ImageNotFound | Controleer de instellingen van de VM-installatiekopie. |  |
| InUseSubnetCannotBeDeleted | U kunt deze fout kan optreden bij het bijwerken van een bron, maar de aanvraag wordt verwerkt door verwijderen en het maken van de resource. Zorg ervoor dat alle ongewijzigde waarden opgeven. | [Bron bijwerken](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Haal het toegangstoken voor de juiste tenant. U kunt alleen het token ophalen van de tenant die uw account hoort bij. | |
| InvalidContentLink | U hebt waarschijnlijk geprobeerd om te koppelen aan een geneste sjabloon die niet beschikbaar. Controleer de URI die u hebt opgegeven voor de geneste sjabloon. Als de sjabloon in een opslagaccount bestaat, zorg er dan voor dat de URI is toegankelijk. U moet mogelijk een SAS-token doorgeven. | [Gekoppelde sjablonen](resource-group-linked-templates.md) |
| InvalidParameter | Een van de waarden die u hebt opgegeven voor een resource komt niet overeen met de verwachte waarde. Deze fout kan leiden tot uit veel verschillende voorwaarden. Bijvoorbeeld, een wachtwoord is mogelijk onvoldoende of een blob-naam is mogelijk onjuist. Controleer het foutbericht om te bepalen welke waarde moet worden gecorrigeerd. | |
| InvalidRequestContent | De waarden van uw implementatie die hetzij bevatten waarden die niet wordt verwacht of ontbreken vereiste waarden. Controleer de waarden voor het brontype. | [Verwijzing naar de sjabloon](/azure/templates/) |
| InvalidRequestFormat | Logboekregistratie voor foutopsporing in te schakelen wanneer de implementatie wordt uitgevoerd en controleer de inhoud van de aanvraag. | [Logboekregistratie voor foutopsporing](resource-manager-troubleshoot-tips.md#enable-debug-logging) |
| InvalidResourceNamespace | Controleer de resourcenaamruimte die u hebt opgegeven in de **type** eigenschap. | [Verwijzing naar de sjabloon](/azure/templates/) |
| InvalidResourceReference | De resource nog niet bestaat of niet juist wordt verwezen. Controleer of u moet een afhankelijkheid toevoegen. Controleer uw gebruik van de **verwijzing** functie bevat de vereiste parameters voor uw scenario. | [Afhankelijkheden moeten worden opgelost](resource-manager-not-found-errors.md) |
| InvalidResourceType | Controleer de resource typt u hebt opgegeven in de **type** eigenschap. | [Verwijzing naar de sjabloon](/azure/templates/) |
| InvalidTemplate | Controleer de sjabloonsyntaxis van de op fouten. | [Ongeldige sjabloon oplossen](resource-manager-invalid-template-errors.md) |
| LinkedAuthorizationFailed | Controleer of uw account hoort bij dezelfde tenant als de resourcegroep die u implementeert. | |
| LinkedInvalidPropertyId | De resource-ID voor een bron is niet correct kan omzetten. Controleer dat u alle vereiste waarden voor de resource-ID opgeven, met inbegrip van de abonnements-ID, naam van een resourcegroep, resourcetype, bovenliggende Resourcenaam (indien nodig) en resourcenaam. | |
| LocationRequired | Geef een locatie voor uw resource. | [Locatie instellen](resource-manager-templates-resources.md#location) |
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
| StorageAccountAlreadyExists | Geef een unieke naam voor het opslagaccount. | [Naam van het opslagaccount oplossen](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | Geef een unieke naam voor het opslagaccount. | [Naam van het opslagaccount oplossen](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | Controleer het abonnement, de resourcegroep en de naam van het opslagaccount dat u probeert te gebruiken. | |
| SubnetsNotInSameVnet | Een virtuele machine kan slechts één virtueel netwerk hebben. Bij het implementeren van meerdere NIC's, zorg er dan voor dat ze deel uitmaken van hetzelfde virtuele netwerk. | [Meerdere NIC 's](../virtual-machines/windows/multiple-nics.md) |

## <a name="find-error-code"></a>Foutcode vinden

Wanneer er een fout tijdens de implementatie optreden, retourneert Resource Manager een foutcode. Hier ziet u het foutbericht via de portal, PowerShell of Azure CLI. Het buitenste foutbericht mogelijk te algemeen voor het oplossen van problemen. Zoek naar het interne bericht dat gedetailleerde informatie over de fout bevat. Zie voor meer informatie [bepalen foutcode](resource-manager-troubleshoot-tips.md#determine-error-code).

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het controleren van acties, [bewerkingen met Resource Manager controleren](resource-group-audit.md).
* Zie voor meer informatie over acties om de fouten tijdens de implementatie, [implementatiebewerkingen weergeven](resource-manager-deployment-operations.md).

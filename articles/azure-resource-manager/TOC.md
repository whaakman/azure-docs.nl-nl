# Overzicht
## [Wat is Resource Manager?](resource-group-overview.md)
## [Resourceproviders en -typen](resource-manager-supported-services.md)
## [Resource Manager en klassieke implementatie](resource-manager-deployment-model.md)
## [Abonnementsgovernance](resource-manager-subscription-governance.md)

# Aan de slag
## [Een sjabloon maken en implementeren](resource-manager-create-first-template.md)
## [Visual Studio-code-extensie voor sjablonen](resource-manager-vscode-extension.md)
## [Visual Studio met Resource Manager](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

# Procedures
## Sjablonen maken
### [Sjabloonsecties](resource-group-authoring-templates.md)
#### [Parameters](resource-manager-templates-parameters.md)
#### [Variabelen](resource-manager-templates-variables.md)
#### [Bronnen](resource-manager-templates-resources.md)
#### [Uitvoer](resource-manager-templates-outputs.md)
### [Gekoppelde en geneste sjablonen](resource-group-linked-templates.md)
### [Afhankelijkheid tussen resources definiëren](resource-group-define-dependencies.md)
### [Meerdere exemplaren maken](resource-group-create-multiple.md)
### [Bron bijwerken](/azure/architecture/building-blocks/extending-templates/update-resource)
### [Patronen voor het ontwerpen van sjablonen](best-practices-resource-manager-design-templates.md)


## Implementeren
### Azure PowerShell
#### [Sjabloon implementeren](resource-group-template-deploy.md)
#### [Persoonlijke sjabloon met SAS-token implementeren](resource-manager-powershell-sas-token.md)
#### [Sjabloon exporteren en opnieuw distribueren](resource-manager-export-template-powershell.md)
### Azure-CLI
#### [Sjabloon implementeren](resource-group-template-deploy-cli.md)
#### [Persoonlijke sjabloon met SAS-token implementeren](resource-manager-cli-sas-token.md)
#### [Sjabloon exporteren en opnieuw distribueren](resource-manager-export-template-cli.md)
### Azure Portal
#### [Resources implementeren](resource-group-template-deploy-portal.md)
#### [Sjabloon exporteren](resource-manager-export-template.md)
### [REST API](resource-group-template-deploy-rest.md)
### [Meerdere resourcegroepen of abonnementen](resource-manager-cross-resource-group-deployment.md)
### [Doorlopende integratie met Visual Studio Team Services](../vs-azure-tools-resource-groups-ci-in-vsts.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Beveiligde waarden doorgeven tijdens implementatie](resource-manager-keyvault-parameter.md)

## Beheren
### [Azure PowerShell](powershell-azure-resource-manager.md)
### [Azure-CLI](xplat-cli-azure-resource-manager.md)
### [Azure Portal](resource-group-portal.md)
### [REST API](resource-manager-rest-api.md)
### [Tags gebruiken om resources te organiseren](resource-group-using-tags.md)
### [Resources verplaatsen naar nieuwe groep of abonnement](resource-group-move-resources.md)
### [Abonnementen organiseren met beheergroepen](../billing/billing-enterprise-mgmt-group-overview.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
### [Voorbeelden governance](resource-manager-subscription-examples.md)
### [Beheerde toepassingen](../managed-applications/overview.md)

## Toegang beheren
### Een service-principal maken
#### [Azure PowerShell](resource-group-authenticate-service-principal.md)
#### [Azure-CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json)
#### [Azure Portal](resource-group-create-service-principal-portal.md)
### [Verificatie-API om toegang te krijgen tot abonnementen](resource-manager-api-authentication.md)
### [Resources vergrendelen](resource-group-lock-resources.md)

## Controleren
### [Activiteitenlogboeken bekijken](resource-group-audit.md)
### [Implementatiebewerkingen bekijken](resource-manager-deployment-operations.md)

## Problemen oplossen
### [Veelvoorkomende implementatiefouten](resource-manager-common-deployment-errors.md)
#### [AccountNameInvalid](resource-manager-storage-account-name-errors.md)
#### [InvalidTemplate](resource-manager-invalid-template-errors.md)
#### [Linux-implementatieproblemen](../virtual-machines/linux/troubleshoot-deploy-vm.md)
#### [NoRegisteredProviderFound](resource-manager-register-provider-errors.md)
#### [NotFound](resource-manager-not-found-errors.md)
#### [ParentResourceNotFound](resource-manager-parent-resource-errors.md)
#### [Problemen met inrichten en toewijzen voor Linux](../virtual-machines/linux/troubleshoot-deployment-new-vm.md)
#### [Problemen met inrichten en toewijzen voor Windows](../virtual-machines/windows/troubleshoot-deployment-new-vm.md)
#### [RequestDisallowedByPolicy](resource-manager-policy-requestdisallowedbypolicy-error.md)
#### [ReservedResourceName](resource-manager-reserved-resource-name.md)
#### [ResourceQuotaExceeded](resource-manager-quota-errors.md)
#### [SkuNotAvailable](resource-manager-sku-not-available-errors.md)
#### [Windows-implementatieproblemen](../virtual-machines/windows/troubleshoot-deploy-vm.md)
### [Inzicht in implementatiefouten](resource-manager-troubleshoot-tips.md)

# Verwijzing
## [Sjabloonindeling](/azure/templates/)
## [Sjabloonfuncties](resource-group-template-functions.md)
### [Matrix- en objectfuncties](resource-group-template-functions-array.md)
### [Vergelijkingsfuncties](resource-group-template-functions-comparison.md)
### [Implementatiefuncties](resource-group-template-functions-deployment.md)
### [Logische functies](resource-group-template-functions-logical.md)
### [Numerieke functies](resource-group-template-functions-numeric.md)
### [Resourcefuncties](resource-group-template-functions-resource.md)
### [Tekenreeksfuncties](resource-group-template-functions-string.md)
## [PowerShell](/powershell/module/azurerm.resources)
## [Azure-CLI](/cli/azure/resource)
## [.NET](/dotnet/api/microsoft.azure.management.resourcemanager)
## [Java](/java/api/com.microsoft.azure.management.resources)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/resourcemanagement.html)
## [REST](/rest/api/resources/)

# Resources
## [Azure-roadmap](https://azure.microsoft.com/roadmap/?category=monitoring-management)
## [Prijscalculator](https://azure.microsoft.com/pricing/calculator/)
## [Service-updates](https://azure.microsoft.com/updates/?product=azure-resource-manager)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-resource-manager)
## [Beperkingsaanvragen](resource-manager-request-limits.md)
## [Asynchrone bewerkingen bijhouden](resource-manager-async-operations.md)
## [Video's](https://azure.microsoft.com/documentation/videos/index/?services=azure-resource-manager)

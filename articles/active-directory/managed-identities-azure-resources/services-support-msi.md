---
title: Azure-Services die ondersteuning bieden voor beheerde identiteiten voor Azure-resources
description: Lijst met services die ondersteuning bieden voor beheerde identiteiten voor Azure-resources en Azure AD-verificatie
services: active-directory
author: priyamohanram
ms.author: priyamo
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2aa25d2e0fe4700eab69d383d1ec8b57fca86bd4
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56201547"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Services die ondersteuning bieden voor beheerde identiteiten voor Azure-resources

Beheerde identiteiten voor Azure-resources bieden Azure-services met een automatisch beheerde identiteit in Azure Active Directory. Met behulp van een beheerde identiteit, kunt u verifiëren met een service die ondersteuning biedt voor Azure AD-verificatie zonder referenties in uw code. We zijn bezig met integratie van beheerde identiteiten voor Azure-resources en Azure AD-verificatie in Azure. Controleer regelmatig of er updates.

> [!NOTE]
> Beheerde identiteiten voor Azure-resources is de nieuwe naam voor de service die eerder de naam Managed Service Identity (MSI) had.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure-services die beheerde identiteiten voor Azure-resources ondersteunen

De volgende Azure-services bieden ondersteuning voor beheerde identiteiten voor Azure-resources:

### <a name="azure-virtual-machines"></a>Azure Virtual Machines

|ID-type beheerd |  Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government|Azure Duitsland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Door het systeem toegewezen | Beschikbaar | Preview | Preview | Preview | Preview |
| Door de gebruiker toegewezen | Preview | Preview | Preview | Preview | Preview

Raadpleeg de volgende lijst voor het configureren van beheerde identiteit voor Azure Virtual Machines (in regio's indien beschikbaar):

- [Azure Portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure-CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-sjablonen](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Azure Virtual Machine Scale Sets

|ID-type beheerd |  Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government|Azure Duitsland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Door het systeem toegewezen | Beschikbaar | Preview | Preview | Preview |
| Door de gebruiker toegewezen | Preview | Preview | Preview | Preview

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure Virtual Machine Scale Sets (in regio's indien beschikbaar):

- [Azure Portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure-CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-sjablonen](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure App Service

|ID-type beheerd |  Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government|Azure Duitsland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Door het systeem toegewezen | Beschikbaar | Beschikbaar | Beschikbaar | Beschikbaar |
| Door de gebruiker toegewezen | Preview | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar

Raadpleeg de volgende lijst voor het configureren van beheerde identiteit voor Azure App Service (in regio's indien beschikbaar):

- [Azure Portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure-CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager-sjabloon](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-functions"></a>Azure Functions

ID-type beheerd |  Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government|Azure Duitsland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Door het systeem toegewezen | Beschikbaar | Beschikbaar | Beschikbaar | Beschikbaar |
| Door de gebruiker toegewezen | Preview | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar

Raadpleeg de volgende lijst voor het configureren van beheerde identiteit voor Azure Functions (in regio's indien beschikbaar):

- [Azure Portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure-CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager-sjabloon](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure Logic Apps

ID-type beheerd |  Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government|Azure Duitsland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Door het systeem toegewezen | Beschikbaar | Beschikbaar | Beschikbaar | Beschikbaar |
| Door de gebruiker toegewezen | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure Logic Apps (in regio's indien beschikbaar):

- [Azure Portal](/azure/logic-apps/create-managed-service-identity#azure-portal)
- [Azure Resource Manager-sjabloon](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure Data Factory V2

ID-type beheerd |  Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government|Azure Duitsland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Door het systeem toegewezen | Beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |
| Door de gebruiker toegewezen | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure Data Factory V2 (in regio's indien beschikbaar):

- [Azure Portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API Management

ID-type beheerd |  Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government|Azure Duitsland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Door het systeem toegewezen | Beschikbaar | Beschikbaar | Niet beschikbaar | Niet beschikbaar |
| Door de gebruiker toegewezen | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure API Management (in regio's indien beschikbaar):

- [Azure Resource Manager-sjabloon](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure Container Instances

ID-type beheerd |  Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government|Azure Duitsland|Azure China 21Vianet|
| --- | --- | --- | --- | --- |
| Door het systeem toegewezen | Linux: Preview<br>Windows: Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |
| Door de gebruiker toegewezen | Linux: Preview<br>Windows: Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure Container Instances (in regio's indien beschikbaar):

- [Azure-CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure Resource Manager-sjabloon](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)


## <a name="azure-services-that-support-azure-ad-authentication"></a>Azure-services die ondersteuning voor Azure AD-verificatie

De volgende services ondersteuning bieden voor Azure AD-verificatie, en zijn getest met clientservices die gebruikmaken van beheerde identiteiten voor Azure-resources.

| Service | Resource-id | Status | Toegang toewijzen |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | `https://management.azure.com/` | Beschikbaar | [Azure Portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure-CLI](howto-assign-access-CLI.md) <br>[Azure Resource Manager-sjabloon](../../role-based-access-control/role-assignments-template.md) |
| Azure Key Vault | `https://vault.azure.net` | Beschikbaar |  
| Azure Data Lake | `https://datalake.azure.net/` | Beschikbaar |
| Azure SQL | `https://database.windows.net/` | Beschikbaar |
| Azure Event Hubs | `https://eventhubs.azure.net` | Preview |
| Azure Service Bus | `https://servicebus.azure.net` | Preview |
| Azure Storage | `https://storage.azure.com/` | Preview |

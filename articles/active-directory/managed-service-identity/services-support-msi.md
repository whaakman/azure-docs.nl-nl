---
title: Azure-Services die ondersteuning bieden voor beheerde Service-identiteit
description: Lijst met services die ondersteuning bieden voor beheerde Service-identiteit en Azure AD-verificatie
services: active-directory
author: daveba
ms.author: daveba
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: f63832723a2c33b88d0e5fc9c6a38a0cad63fa38
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259174"
---
# <a name="services-that-support-managed-service-identity"></a>Services die ondersteuning bieden voor beheerde Service-identiteit 

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. Met behulp van een beheerde identiteit, kunt u verifiëren met een service die ondersteuning biedt voor Azure AD-verificatie zonder referenties in uw code. We zijn bezig met beheerde Service-identiteit en Azure AD-verificatie integreren in Azure. Controleer regelmatig of er updates.

## <a name="azure-services-that-support-managed-service-identity"></a>Azure-services die ondersteuning bieden voor beheerde Service-identiteit

De volgende Azure-services ondersteunen beheerde Service-identiteit.

| Service | Systeem toegewezen status | Door gebruiker toegewezen status| Configureren | Een token verkrijgen |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Virtual Machines | Preview | Preview | [Azure Portal](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[Azure-CLI](qs-configure-cli-windows-vm.md)<br>[Azure Resource Manager-sjablonen](qs-configure-template-windows-vm.md)<br>[REST](qs-configure-rest-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Azure Virtual Machine Scale Sets | Preview | Preview | [Azure Portal](qs-configure-portal-windows-vmss.md)<br>[PowerShell](qs-configure-powershell-windows-vmss.md)<br>[Azure-CLI](qs-configure-cli-windows-vmss.md)<br>[Azure Resource Manager-sjablonen](qs-configure-template-windows-vmss.md)<br>[REST](qs-configure-rest-vmss.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell)
| Azure App Service | Beschikbaar | Niet beschikbaar | [Azure Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure-CLI](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Azure Resource Manager-sjabloon](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell)  |
| Azure Functions | Beschikbaar | Niet beschikbaar | [Azure Portal](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[Azure-CLI](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Azure Resource Manager-sjabloon](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell) |
| Azure Data Factory V2 | Preview | Niet beschikbaar | [Azure Portal](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |


## <a name="azure-services-that-support-azure-ad-authentication"></a>Azure-services die ondersteuning voor Azure AD-verificatie

De volgende services ondersteuning bieden voor Azure AD-verificatie, en zijn getest met clientservices die gebruikmaken van beheerde Service-identiteit.

| Service | Resource-id | Status | Date | Toegang toewijzen |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Beschikbaar | September 2017 | [Azure Portal](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[Azure-CLI](howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net | Beschikbaar | September 2017 | |
| Azure Data Lake | https://datalake.azure.net/ | Beschikbaar | September 2017 | |
| Azure SQL | https://database.windows.net/ | Beschikbaar | Oktober 2017 | |
| Azure Event Hubs | https://eventhubs.azure.net | Beschikbaar | December 2017 | |
| Azure Service Bus | https://servicebus.azure.net | Beschikbaar | December 2017 | |
| Azure Storage | https://storage.azure.com/ | Preview | Mei 2018 | |

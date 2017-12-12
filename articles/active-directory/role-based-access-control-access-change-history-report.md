---
title: Toegang tot rapportage - Azure RBAC | Microsoft Docs
description: Genereer een rapport met een lijst met alle wijzigingen in de toegang tot uw Azure-abonnementen met toegangsbeheer op basis van rollen gedurende de afgelopen 90 dagen.
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c430e1206e6e97f2c7fb7d2a6ff0dd6e65ee8bbf
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="create-an-access-report-for-role-based-access-control"></a>Een access-rapport maken voor toegangsbeheer op basis van rollen
Elk gewenst moment iemand verleent of trekt u toegang binnen uw abonnementen, worden de wijzigingen geregistreerd in Azure gebeurtenissen. U kunt toegang tot Geschiedenisrapporten om te zien alle wijzigingen voor de afgelopen negentig dagen maken.

## <a name="create-a-report-with-azure-powershell"></a>Een rapport maken met Azure PowerShell
Gebruik voor het maken van een geschiedenisrapport voor gewijzigde van toegang in PowerShell de [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) opdracht.

Wanneer u deze opdracht aanroept, kunt u opgeven welke eigenschap van de toewijzingen die u weergegeven, inclusief het volgende wilt:

| Eigenschap | Beschrijving |
| --- | --- |
| **Actie** |Hiermee wordt aangegeven of toegang is toegekend of ingetrokken |
| **Aanroeper** |De eigenaar die verantwoordelijk is voor de wijziging toegang |
| **PrincipalId** | De unieke id van de gebruiker, groep of toepassing die de rol is toegewezen |
| **Primaire naam** |De naam van de gebruiker, groep of toepassing |
| **PrincipalType** |Hiermee wordt aangegeven of de toewijzing voor een gebruiker, groep of toepassing is |
| **RoleDefinitionId** |De GUID van de rol die is toegekend of ingetrokken |
| **Rolnaam** |De rol die is toegekend of ingetrokken |
| **Bereik** | De unieke id van het abonnement, resourcegroep of resource die de toewijzing is van toepassing op | 
| **ScopeName** |De naam van het abonnement, resourcegroep of resource |
| **ScopeType** |Hiermee wordt aangegeven of de toewijzing is op het abonnement, resourcegroep of resource-bereik |
| **Tijdstempel** |De datum en tijd op dat toegang is gewijzigd |

Deze opdracht worden alle wijzigingen in het abonnement voor de afgelopen zeven dagen:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![PowerShell Get-AzureRMAuthorizationChangeLog - schermafbeelding](./media/role-based-access-control-configure/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Een rapport maken met Azure CLI
Gebruik voor het maken van een access-geschiedenisrapport van wijziging in de Azure-opdrachtregelinterface (CLI) de `azure role assignment changelog list` opdracht.

## <a name="export-to-a-spreadsheet"></a>Naar een spreadsheet exporteren
Als u het rapport opslaan of manipuleren van de gegevens, de toegang wijzigingen in een CSV-bestand wilt exporteren. U kunt vervolgens het rapport weergeven in een werkblad voor revisie.

![Changelog weergegeven als werkblad - schermafbeelding](./media/role-based-access-control-configure/change-history-spreadsheet.png)

## <a name="next-steps"></a>Volgende stappen
* Werken met [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md)
* Meer informatie over het beheren van [Azure RBAC met powershell](role-based-access-control-manage-access-powershell.md)


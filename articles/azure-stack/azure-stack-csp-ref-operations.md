---
title: Registreren van tenants voor het gebruik bijhouden in Azure Stack | Microsoft Docs
description: Meer informatie over de bewerkingen die worden gebruikt voor het beheren van de tenant-registraties en het tenantgebruik wordt bijgehouden in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: mabrigg
ms.reviewer: alfredop
ms.lastreviewed: 01/08/2019
ms.openlocfilehash: 6f2a2eb9902e8567b5fa27ed93dd8be2fe3a01b3
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56587069"
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Tenant-registratie in Azure Stack beheren

*Van toepassing op: Azure Stack-geïntegreerde systemen*

In dit artikel bevat informatie over de werking van de registratie. U kunt deze bewerkingen te gebruiken:
- Tenant-registraties beheren
- Gebruik bijhouden van tenant beheren

Hier vindt u meer informatie over het toevoegen, lijst of tenant toewijzingen verwijderen. U kunt PowerShell of de facturering API-eindpunten gebruiken voor het beheren van uw gebruik bijhouden. Hier vindt u meer informatie over het toevoegen, lijst of tenant toewijzingen verwijderen. U kunt PowerShell of de facturering API-eindpunten gebruiken voor het beheren van uw gebruik bijhouden.

## <a name="add-tenant-to-registration"></a>Tenant registratie toevoegen

Als u wilt toevoegen van een nieuwe tenant aan uw registratie gebruikt u de bewerking. Tenantgebruik wordt vermeld onder een Azure-abonnement verbonden met de tenant Azure Active Directory (Azure AD).

U kunt de bewerking ook gebruiken als u wilt wijzigen van het abonnement dat is gekoppeld aan een tenant. Aanroepen van PUT/New-AzureRMResource als u wilt overschrijven van de vorige toewijzing.

U kunt een enkel Azure-abonnement koppelen aan een tenant. Als u probeert een tweede abonnement toevoegen aan een bestaande tenant, is het eerste abonnement overschreven.

### <a name="use-api-profiles"></a>Gebruik API-profielen

De cmdlets van de registratie is vereist dat u een API-profiel opgeven bij het uitvoeren van PowerShell. API-profielen vertegenwoordigen een verzameling Azure-resource-providers en hun API-versies. Ze helpen u de juiste versie van de API gebruiken bij interactie met meerdere Azure-clouds. Bijvoorbeeld, werken u met meerdere clouds bij het werken met de globale Azure en Azure Stack. Profielen Geef een naam die overeenkomt met de datum waarop de release. U moet gebruiken de **03-09-2017** profiel.

Zie voor meer informatie over Azure Stack en API-profielen, [beheren API-versieprofielen in Azure Stack](user/azure-stack-version-profiles.md). Zie voor instructies over het ophalen van en uitvoeren met API-profiel met PowerShell [gebruik API-versieprofielen voor PowerShell in Azure Stack](user/azure-stack-version-profiles-powershell.md).

### <a name="parameters"></a>Parameters

| Parameter                  | Description |
|---                         | --- |
| registrationSubscriptionID | De Azure-abonnement dat is gebruikt voor de registratie. |
| customerSubscriptionID     | De Azure-abonnement (niet Azure Stack) die horen bij de klant worden geregistreerd. Moet worden gemaakt van de aanbieding van Cloud Service Provider (CSP) tot en met de Partner Center. Als een klant meer dan één tenant heeft, moet u een abonnement voor de tenant voor aanmelding bij Azure Stack gemaakt. |
| resourceGroup              | De resourcegroep in Azure waarop uw registratie zijn opgeslagen. |
| registrationName           | De naam van de registratie van uw Azure Stack. Er is een object dat is opgeslagen in Azure. De naam is meestal in de vorm azurestack-CloudID, waarbij CloudID is in de Cloud-ID van uw Azure Stack-implementatie. |

> [!Note]  
> Tenants moeten worden geregistreerd met elke Azure-Stack die ze gebruiken. Als een tenant maakt gebruik van meer dan één Azure Stack, moet u de eerste registraties van elke implementatie bijwerken met de tenantabonnement.

### <a name="powershell"></a>PowerShell

Gebruik de cmdlet New-AzureRmResource om toe te voegen een tenant. [Verbinding maken met Azure Stack](azure-stack-powershell-configure-admin.md), en gebruik vervolgens de volgende cmdlet vanaf een opdrachtprompt:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>API-aanroep

**Bewerking**: PUT  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Antwoord**: 201-gemaakt  
**Antwoordtekst**: Leeg  

## <a name="list-all-registered-tenants"></a>Lijst van alle geregistreerde tenants

Haal een lijst van alle tenants die zijn toegevoegd aan een registratie.

 > [!Note]  
 > Als u geen tenants zijn geregistreerd, ontvangt u geen antwoord.

### <a name="parameters"></a>Parameters

| Parameter                  | Description          |
|---                         | ---                  |
| registrationSubscriptionId | De Azure-abonnement dat is gebruikt voor de registratie.   |
| resourceGroup              | De resourcegroep in Azure waarop uw registratie zijn opgeslagen.    |
| registrationName           | De naam van de registratie van uw Azure Stack. Er is een object dat is opgeslagen in Azure. De naam van de bevindt zich doorgaans in de vorm van **azurestack**-***CloudID***, waarbij ***CloudID*** is de Cloud-ID van uw Azure Stack-implementatie.   |

### <a name="powershell"></a>PowerShell

Gebruik de cmdlet Get-AzureRmResource om alle geregistreerde tenants weer te geven. [Verbinding maken met Azure Stack](azure-stack-powershell-configure-admin.md), en gebruik vervolgens de volgende cmdlet vanaf een opdrachtprompt:

```powershell
  Get-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API-aanroep

U kunt een lijst van alle toewijzingen van tenant met behulp van de GET-bewerking.

**Bewerking**: GET  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?  
api-version=2017-06-01 HTTP/1.1`  
**Antwoord**: 200  
**Antwoordtekst**: 

```JSON  
{
    "value": [{
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 1}",
            "name": " cspSubscriptionId 1",
            "type": "Microsoft.AzureStack\customerSubscriptions",
            "properties": { "tenantId": "tId1" }
        },
        {
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 2}",
            "name": " cspSubscriptionId2 ",
            "type": "Microsoft.AzureStack\customerSubscriptions",
            "properties": { "tenantId": "tId2" }
        }
    ],
    "nextLink": "{originalRequestUrl}?$skipToken={opaqueString}"
}
```

## <a name="remove-a-tenant-mapping"></a>Een tenanttoewijzing verwijderen

U kunt een tenant die is toegevoegd aan een registratie verwijderen. Als deze tenant is nog steeds resources in Azure Stack, worden hun gebruik wordt verrekend met het abonnement dat is gebruikt in de eerste registratie voor Azure Stack.

### <a name="parameters"></a>Parameters

| Parameter                  | Description          |
|---                         | ---                  |
| registrationSubscriptionId | Abonnements-ID voor de registratie.   |
| resourceGroup              | De resourcegroep voor de registratie.   |
| registrationName           | De naam van de registratie.  |
| customerSubscriptionId     | De klant abonnements-ID.  |

### <a name="powershell"></a>PowerShell

Gebruik de cmdlet Remove-AzureRmResource te verwijderen van een tenant. [Verbinding maken met Azure Stack](azure-stack-powershell-configure-admin.md), en gebruik vervolgens de volgende cmdlet vanaf een opdrachtprompt:

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API-aanroep

Toewijzingen van tenant met behulp van de bewerking verwijderen, kunt u verwijderen.

**Bewerking**: DELETE  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Antwoord**: 204 geen inhoud  
**Antwoordtekst**: Leeg

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over het ophalen van informatie over het gebruik van de resource van Azure Stack, [gebruik en facturering in Azure Stack](azure-stack-billing-and-chargeback.md).

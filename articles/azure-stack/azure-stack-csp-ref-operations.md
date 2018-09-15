---
title: Registreren van tenants voor het gebruik bijhouden in Azure Stack | Microsoft Docs
description: Meer informatie over de bewerkingen die worden gebruikt voor het beheren van de tenant-registraties en het tenantgebruik wordt bijgehouden in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2018
ms.author: sethm
ms.reviewer: alfredo
ms.openlocfilehash: 93830933115b19c6dc6b2981dca142f72a133fc0
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45629658"
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Tenant-registratie in Azure Stack beheren

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

In dit artikel bevat informatie over de bewerkingen die u gebruiken kunt voor het beheren van uw tenant-registraties en hoe tenantgebruik wordt bijgehouden. Hier vindt u meer informatie over het toevoegen, lijst of tenant toewijzingen verwijderen. U kunt PowerShell of de facturering API-eindpunten gebruiken voor het beheren van uw gebruik bijhouden.

## <a name="add-tenant-to-registration"></a>Tenant registratie toevoegen

Als u toevoegen van een nieuwe tenant aan uw registratie, wilt zodat hun gebruik wordt gerapporteerd bij een Azure-abonnement verbonden met de tenant Azure Active Directory (Azure AD) gebruikt u deze bewerking.

U kunt deze bewerking ook gebruiken als u wilt wijzigen van het abonnement dat is gekoppeld aan een tenant, kunt u New-PUT-AzureRMResource opnieuw aanroepen. De oude toewijzing wordt overschreven.

Houd er rekening mee dat slechts één Azure-abonnement gekoppeld aan een tenant worden kan. Als u probeert een tweede abonnement toevoegen aan een bestaande tenant, is het eerste abonnement overschreven. 

### <a name="use-api-profiles"></a>Gebruik API-profielen

De cmdlets in dit artikel is vereist dat u een API-profiel opgeven bij het uitvoeren van PowerShell. API-profielen vertegenwoordigen een verzameling Azure-resource-providers en hun API-versies. Ze helpen u de juiste versie van de API gebruiken bij interactie met meerdere Azure-clouds, bijvoorbeeld bij het werken met de globale Azure en Azure Stack. Profielen worden opgegeven door een naam die overeenkomt met de datum waarop de release. Met dit artikel, moet u gebruiken de **03-09-2017** profiel.

Zie voor meer informatie over Azure Stack en API-profielen, [beheren API-versieprofielen in Azure Stack](user/azure-stack-version-profiles.md). Zie voor instructies over het ophalen van en uitvoeren met API-profiel met PowerShell [gebruik API-versieprofielen voor PowerShell in Azure Stack](user/azure-stack-version-profiles-powershell.md).

### <a name="parameters"></a>Parameters

| Parameter                  | Beschrijving |
|---                         | --- |
| registrationSubscriptionID | De Azure-abonnement dat is gebruikt voor de registratie. |
| customerSubscriptionID     | De Azure-abonnement (niet Azure Stack) die horen bij de klant worden geregistreerd. Moet worden gemaakt van de aanbieding van Cloud Service Provider (CSP). Dit betekent via Partner Center in de praktijk. Als een klant meer dan één tenant heeft, kan dit abonnement moet worden gemaakt in de tenant die wordt gebruikt voor aanmelding bij Azure Stack. |
| ResourceGroup              | De resourcegroep in Azure waarop uw registratie zijn opgeslagen. |
| registrationName           | De naam van de registratie van uw Azure Stack. Er is een object dat is opgeslagen in Azure. De naam is meestal in de vorm azurestack-CloudID, waarbij CloudID is in de Cloud-ID van uw Azure Stack-implementatie. |

> [!Note]  
> Tenants moeten worden geregistreerd met elke Azure-Stack die ze gebruiken. Als een tenant maakt gebruik van meer dan één Azure Stack, moet u de eerste registraties van elke implementatie bijwerken met de tenantabonnement.

### <a name="powershell"></a>PowerShell

Gebruik de cmdlet New-AzureRmResource om bij te werken van de registratie-resource. Aanmelden bij Azure (`Add-AzureRmAccount`) met behulp van het account dat u voor de registratie gebruikt. Hier volgt een voorbeeld van het toevoegen van een tenant:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>API-aanroep

**Bewerking**: plaatsen  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Antwoord**: 201-gemaakt  
**Antwoordtekst**: leeg zijn  

## <a name="list-all-registered-tenants"></a>Lijst van alle geregistreerde tenants

Haal een lijst van alle tenants die zijn toegevoegd aan een registratie.

 > [!Note]  
 > Als u geen tenants zijn geregistreerd, ontvangt u geen antwoord.

### <a name="parameters"></a>Parameters

| Parameter                  | Beschrijving          |
|---                         | ---                  |
| registrationSubscriptionId | De Azure-abonnement dat is gebruikt voor de registratie.   |
| ResourceGroup              | De resourcegroep in Azure waarop uw registratie zijn opgeslagen.    |
| registrationName           | De naam van de registratie van uw Azure Stack. Er is een object dat is opgeslagen in Azure. De naam van de bevindt zich doorgaans in de vorm van **azurestack**-***CloudID***, waarbij ***CloudID*** is de Cloud-ID van uw Azure Stack-implementatie.   |

### <a name="powershell"></a>PowerShell

Gebruik de cmdlet Get-AzureRmResource om alle geregistreerde tenants weer te geven. Aanmelden bij Azure (`Add-AzureRmAccount`) met behulp van het account dat u voor de registratie gebruikt. Hier volgt een voorbeeld van het toevoegen van een tenant:

```powershell
  Get-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API-aanroep

U kunt een lijst van alle toewijzingen van tenant met behulp van de GET-bewerking ophalen

**Bewerking**: ophalen  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?  
api-version=2017-06-01 HTTP/1.1`  
**Antwoord**: 200  
**Antwoordtekst**: 

```JSON  
{
    "value": [{
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 1}”,
            "name": " cspSubscriptionId 1",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId1" }
        },
        {
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 2}”,
            "name": " cspSubscriptionId2 ",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId2" }
        }
    ],
    "nextLink": "{originalRequestUrl}?$skipToken={opaqueString}"
}
```

## <a name="remove-a-tenant-mapping"></a>Een tenanttoewijzing verwijderen

U kunt een tenant die is toegevoegd aan een registratie verwijderen. Als deze tenant is nog steeds resources in Azure Stack, worden hun gebruik wordt verrekend met het abonnement dat is gebruikt in de eerste registratie voor Azure Stack.

### <a name="parameters"></a>Parameters

| Parameter                  | Beschrijving          |
|---                         | ---                  |
| registrationSubscriptionId | Abonnements-ID voor de registratie.   |
| ResourceGroup              | De resourcegroep voor de registratie.   |
| registrationName           | De naam van de registratie.  |
| customerSubscriptionId     | De klant abonnements-ID.  |

### <a name="powershell"></a>PowerShell

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API-aanroep

Toewijzingen van tenant met behulp van de bewerking verwijderen, kunt u verwijderen.

**Bewerking**: verwijderen  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Antwoord**: 204 geen inhoud  
**Antwoordtekst**: leeg zijn

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over het ophalen van informatie over het gebruik van de resource van Azure Stack, [gebruik en facturering in Azure Stack](azure-stack-billing-and-chargeback.md).

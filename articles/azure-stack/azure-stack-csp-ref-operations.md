---
title: Registreren van tenants voor gebruik in Azure-Stack bijhouden | Microsoft Docs
description: Gegevens over de bewerkingen die worden gebruikt voor het beheren van de tenant registraties en hoe het gebruik van de tenant wordt bijgehouden in Azure-Stack.
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
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: fbdf4023bc70f1ad05dd52ac1eabe95b12be9be2
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Registratie in Azure-Stack tenant beheren

*Van toepassing op: Azure Stack geïntegreerd systemen*

Dit artikel bevat informatie over de bewerkingen die u gebruiken kunt voor het beheren van uw tenant-registraties en hoe het gebruik van de tenant wordt bijgehouden. U vindt meer informatie over het toevoegen, lijst of verwijder toewijzingen van de tenant. U kunt PowerShell of de eindpunten facturering API gebruiken voor het beheren van uw gebruik bijhouden.

## <a name="add-tenant-to-registration"></a>Tenant toevoegen aan de registratie

Als u een nieuwe tenant toevoegen aan uw registratie, wilt zodat hun gebruik wordt gerapporteerd onder een Azure-abonnement verbonden met de tenant van Azure Active Directory (Azure AD) gebruikt u deze bewerking.

U kunt deze bewerking ook gebruiken als u wilt wijzigen van het abonnement is gekoppeld aan een tenant, u kunt New-PUT-AzureRMResource opnieuw aanroepen. De oude toewijzing wordt overschreven.

Houd er rekening mee dat slechts één Azure-abonnement gekoppeld aan een tenant worden kan. Als u een tweede abonnement toevoegen aan een bestaande tenant probeert, is het eerste abonnement overschreven. 


| Parameter                  | Beschrijving |
|---                         | --- |
| registrationSubscriptionID | De Azure-abonnement dat is gebruikt voor de initiële registratie. |
| customerSubscriptionID     | De Azure-abonnement (geen Azure-Stack) die horen bij de klant worden geregistreerd. Moet worden gemaakt in de aanbieding Cloud Service Provider (CSP). In de praktijk betekent dit via Partner Center. Als een klant meer dan één tenant heeft, kan dit abonnement moet worden gemaakt in de tenant die wordt gebruikt voor aanmelding bij Azure-Stack. |
| resourceGroup              | De resourcegroep in Azure waarin uw registratie wordt opgeslagen. |
| registrationName           | De naam van de registratie van uw Azure-Stack. Er is een object dat is opgeslagen in Azure. De naam van de bevindt zich doorgaans in de vorm azurestack-CloudID, waarbij CloudID is in de Cloud-ID van uw Azure-Stack-implementatie. |

> [!Note]  
> Tenants moeten worden geregistreerd met elke Azure-Stack die ze gebruiken. Als een tenant maakt gebruik van meer dan één Azure-Stack, moet u de initiële registraties van elke implementatie bijwerken met de tenant-abonnement.

### <a name="powershell"></a>PowerShell

Gebruik de cmdlet New-AzureRmResource bijwerken van de bron van de registratie. Aanmelden bij Azure (`Connect-AzureRmAccount`) met het account dat u voor de initiële registratie gebruikt. Hier volgt een voorbeeld van het toevoegen van een tenant:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>API-aanroep

**Bewerking**: plaatsen  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Antwoord**: 201 gemaakt  
**Antwoordtekst**: leeg  

## <a name="list-all-registered-tenants"></a>Lijst van alle geregistreerde tenants

Een lijst van alle tenants die zijn toegevoegd aan een registratie ophalen.

 > [!Note]  
 > Als u geen tenants zijn geregistreerd, ontvangt u geen antwoord.

### <a name="parameters"></a>Parameters

| Parameter                  | Beschrijving          |
|---                         | ---                  |
| registrationSubscriptionId | De Azure-abonnement dat is gebruikt voor de initiële registratie.   |
| resourceGroup              | De resourcegroep in Azure waarin uw registratie wordt opgeslagen.    |
| registrationName           | De naam van de registratie van uw Azure-Stack. Er is een object dat is opgeslagen in Azure. De naam van de bevindt zich doorgaans in de vorm van **azurestack**-***CloudID***, waarbij ***CloudID*** is de Cloud-ID van uw Azure-Stack-implementatie.   |

### <a name="powershell"></a>PowerShell

Gebruik de cmdlet Get-AzureRmResovurce voor een lijst met alle geregistreerde tenants. Aanmelden bij Azure (`Connect-AzureRmAccount`) met het account dat u voor de initiële registratie gebruikt. Hier volgt een voorbeeld van het toevoegen van een tenant:

```powershell
  Get-AzureRmResovurce -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
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

## <a name="remove-a-tenant-mapping"></a>Verwijderen van een tenant toewijzen

U kunt een tenant die is toegevoegd aan een registratie verwijderen. Als deze tenant wordt steeds resources op Azure-Stack, wordt hun gebruik verrekend met het abonnement dat is gebruikt bij de eerste Azure-Stack-registratie.

### <a name="parameters"></a>Parameters

| Parameter                  | Beschrijving          |
|---                         | ---                  |
| registrationSubscriptionId | Abonnement-ID voor de registratie.   |
| resourceGroup              | De resourcegroep voor de registratie.   |
| registrationName           | De naam van de registratie.  |
| customerSubscriptionId     | De klant abonnements-ID.  |

### <a name="powershell"></a>PowerShell

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>API-aanroep

U kunt de toewijzingen van tenant met behulp van de bewerking DELETE verwijderen.

**Bewerking**: verwijderen  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Antwoord**: 204 geen inhoud  
**Antwoordtekst**: leeg

## <a name="next-steps"></a>Volgende stappen

 - Zie voor meer informatie over het ophalen van informatie over het gebruik van de bron van Azure-Stack, [gebruiks- en facturering in Azure Stack](/azure-stack-billing-and-chargeback.md).

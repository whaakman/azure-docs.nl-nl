---
title: Toevoegen van tenants voor het gebruik van en naar Azure Stack facturering | Microsoft Docs
description: Een eindgebruiker toevoegen de vereiste stappen aan Azure-Stack beheerd door een Cloud Service Provider.
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
ms.date: 03/08/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: 27473ce4057fdb06ab9faf0f46dede62b4ee2246
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37048836"
---
# <a name="add-tenant-for-usage-and-billing-to-azure-stack"></a>Toevoegen van de tenant voor informatie over het gebruik en facturering naar Azure-Stack

*Van toepassing op: Azure Stack geïntegreerd systemen*

In dit artikel wordt beschreven hoe u een eindgebruiker toevoegen aan Azure-Stack beheerd door een Cloud Service Provider (CSP). Als de nieuwe tenant resources gebruikt, wordt Azure Stack gebruik rapporteren aan haar CSP-abonnement.

CSP's bieden vaak services voor meerdere klanten (tenants) op de Stack van Azure-implementatie. Tenants toe te voegen aan de registratie van de Azure-Stack zorgt ervoor dat elke tenant gebruik wordt gerapporteerd en aan het overeenkomstige CSP-abonnement in rekening gebracht. Als u niet de stappen in dit artikel, wordt gebruik van de tenant verrekend met het abonnement dat is gebruikt bij de initiële registratie van Azure-Stack. Voordat u een end-klant aan Azure-Stack toevoegen kunt voor gebruik bijhouden en hun tenant te beheren, moet u Azure-Stack configureren als een CSP. Zie voor stappen en bronnen [gebruik en facturering voor Azure-Stack als een Cloudserviceprovider beheren](azure-stack-add-manage-billing-as-a-csp.md).

Het volgende diagram toont de stappen die een CSP volgen moet om een nieuwe klant Azure-Stack gebruiken en voor het instellen van gebruik bijhouden voor de klant inschakelen. Door de klant toe te voegen kunt u ook zich voor het beheren van resources in Azure-Stack. U hebt twee opties voor het beheren van hun bronnen:

1. U kunt de tenant van de klant end onderhouden en geef referenties op om het lokale Stack Azure-abonnement bij de klant.  
2. Of de klant kunt werken met hun abonnement lokaal en toevoegen van de CSP als gast met de van eigenaarsmachtigingen.  

**Stappen voor het toevoegen van een klant einde**

![Cloudserviceprovider ingesteld voor gebruik bijhouden en voor het beheren van de account van de klant beëindigen](media\azure-stack-csp-enable-billing-usage-tracking\process-csp-enable-billing.png)

## <a name="create-a-new-customer-in-partner-center"></a>Maak een nieuwe klant in Partner Center

In Partnercentrum, maakt u een nieuwe Azure-abonnement voor de klant. Zie voor instructies [toevoegen van een nieuwe klant](https://msdn.microsoft.com/partner-center/add-a-new-customer).


##  <a name="create-an-azure-subscription-for-the-end-customer"></a>Een Azure-abonnement voor de klant end maken

Nadat u een record van uw klant in Partner Center hebt gemaakt, kunt u deze abonnementen op producten in de catalogus verkopen. Zie voor instructies [maken, onderbreken of annuleren klantabonnementen](https://msdn.microsoft.com/partner-center/create-a-new-subscription).

## <a name="create-a-guest-user-in-the-end-customer-directory"></a>Maken van een gastgebruiker in de map van de klant einde

Als eindgebruikers hun eigen account beheert, een gastgebruiker maken in de directory en de gegevens verzenden. De eindgebruiker vervolgens wordt de Gast toevoegen en uitbreiden van de Gast-machtiging voor het **eigenaar** aan het Azure-Stack CSP-account.
 
## <a name="update-the-registration-with-the-end-customer-subscription"></a>De registratie wordt bijgewerkt met het einde klant-abonnement

Uw registratie bijwerken met de nieuwe klant-abonnement. Azure rapporten van de klant gebruik met behulp van de identiteit van de klant van Centraal Partner. Deze stap zorgt ervoor dat informatie over het gebruik van elke klant onder de afzonderlijke CSP-abonnement van de klant wordt gerapporteerd. Dit maakt gebruik van de gebruiker bijhouden en facturering veel eenvoudiger.

> [!Note]  
> Als u wilt deze stap uitvoert, moet u hebben [geregistreerd Azure Stack](azure-stack-register.md).

1. Open Windows PowerShell met een opdrachtprompt en voer:  
    `Add-AzureRmAccount`
2. Typ uw Azure-referenties.
3. In de PowerShell-sessie uitvoeren:

```powershell
    New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties <PSObject>
```
### <a name="new-azurermresource-powershell-parameters"></a>Nieuwe AzureRmResource PowerShell-parameters
| Parameter | Beschrijving |
| --- | --- | 
|registrationSubscriptionID | De Azure-abonnement dat is gebruikt voor de initiële registratie van de Azure-Stack. |
| customerSubscriptionID | De Azure-abonnement (geen Azure-Stack) die horen bij de klant worden geregistreerd. Moet worden gemaakt in de CSP-aanbieding; in de praktijk betekent dit via Partner Center. Als een klant meer dan één Azure Active Directory-tenant heeft, kan dit abonnement moet worden gemaakt in de tenant die wordt gebruikt voor aanmelding bij Azure-Stack.
| resourceGroup | De resourcegroep in Azure waarin uw registratie wordt opgeslagen. 
| registrationName | De naam van de registratie van uw Azure-Stack. Er is een object dat is opgeslagen in Azure. | 
| Eigenschappen | Hiermee geeft u eigenschappen voor de resource. Gebruik deze parameter om op te geven van de waarden van eigenschappen die specifiek voor het brontype zijn.


> [!Note]  
> Tenants moeten worden geregistreerd met elke Azure-Stack die ze gebruiken. Als er twee Azure Stack-implementaties en een tenant gaat gebruiken beide parameters, moet u de initiële registraties van elke implementatie bijwerken met de tenant-abonnement.

## <a name="onboard-tenant-to-azure-stack"></a>Ingebouwde tenant naar Azure-Stack

Configureer Azure Stack ter ondersteuning van gebruikers van meerdere Azure AD-tenants services in Azure-Stack gebruiken. Zie voor instructies [inschakelen in Azure-Stack multi-tenancymodus](azure-stack-enable-multitenancy.md).


## <a name="create-a-local-resource-in-the-end-customer-tenant-in-azure-stack"></a>Een lokale bron maken in de tenant van de klant einde in Azure-Stack

Nadat u de nieuwe klant hebt toegevoegd aan Azure-Stack of de tenant van de klant end uw gastaccount met rechten van de eigenaar is ingeschakeld, controleert u of u kunt een resource in de tenant te maken. Bijvoorbeeld, kunnen ze [virtuele Windows-machine maken met de Stack van Azure-portal](user\azure-stack-quick-windows-portal.md).

## <a name="next-steps"></a>Volgende stappen

 - De foutberichten worden weergegeven als ze worden geactiveerd tijdens het registratieproces Zie [Tenant-registratie foutberichten](azure-stack-csp-ref-infrastructure.md#usage-and-billing-error-codes).
 - Zie voor meer informatie over het ophalen van informatie over het gebruik van de bron van Azure-Stack, [gebruiks- en facturering in Azure Stack](/azure-stack-billing-and-chargeback.md).
 - Als u wilt controleren hoe een klant kan toevoegen, als de CSP, als de manager voor de Azure-Stack-tenant, Zie [inschakelen van een Cloud Service Provider voor het beheren van uw abonnement Azure Stack](user\azure-stack-csp-enable-billing-usage-tracking.md).

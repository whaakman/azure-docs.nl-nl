---
title: Resource provider API-versies ondersteund door de profielen in Azure Stack | Microsoft Docs
description: Meer informatie over de Azure Resource Manager-versie wordt ondersteund door de profielen in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 2769b78632e1a7f776359f2a4d768154c224aab5
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54264611"
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Resource provider API-versies ondersteund door de profielen in Azure Stack

U vindt de resourceprovider en de versienummers voor elke API-profiel dat door Azure Stack wordt gebruikt in dit artikel. De tabellen in dit artikel worden de versies die worden ondersteund voor elke resourceprovider en de API-versies van de profielen. Elke resourceprovider bevat een set van resourcetypen en specifieke versienummers.

De API-profiel maakt gebruik van drie naamgevingsregels:

 - **latest**
 - **yyyy-mm-dd-hybrid**
 - **yyyy-mm-dd-profile**

Zie voor een uitleg van de API-profielen en -versie worden uitgerold voor Azure Stack, [beheren API-versieprofielen in Azure Stack](azure-stack-version-profiles.md).

> [!Note]  
> De **nieuwste** API-profiel bevat de meest recente versie van de resourceprovider API en niet wordt vermeld in dit artikel.

## <a name="overview-of-2018--03-01-hybrid"></a>Overzicht van 2018-03-01-hybride

| Resourceprovider | API-versie |
|-----------------------------------------------|-----------------------------------------------------|
| Microsoft.Compute | 2017-03-30 |
| Microsoft.Network | 2017-10-01<br>VPN-Gateway moet 2017-03-01 |
| Microsoft.Storage (Data Plane) | 2017-04-17 |
| Microsoft.Storage (Controlelaag) | 2016-01-01 |
| Microsoft. Web | 2016-08-01<br>Dit is de meest recente (vanaf nu) in Azure |
| Microsoft.KeyVault | 2016-10-01 (niet wijzigen) |
| Microsoft.Resources (Azure Resource Manager zelf) | 2016-02-01 |
| Microsoft.Authorization (bewerkingen voor beleid) | 2015-11-01 |
| Microsoft.Insights | 2015-11-01 |
| Microsoft.Keyvault | 2016-10-01 |
| Beleid | 2016-10-01 |
| Resources | 2016-10-01 |
| Resources_Links | 2016-10-01 |
| Resources_Locks | 2016-10-01 |
| Abonnementen | 2016-10-01 |

Zie voor meer een lijst van de versies voor elk resourcetype voor de providers in het API-profiel, [Details voor de 2018-03-01-hybride](#details-for-the-2018-03-01-hybrid) profiel.

## <a name="overview-of-2018-03-01-hybrid"></a>Overzicht van 2018-03-01-hybride

| Resourceprovider | API-versie |
|------------------------------------------------|------------------------------|
| Microsoft.Compute | 30-03-2016 |
| Microsoft.Network | 2015-06-15 |
| Microsoft.Storage (Data Plane) | 2015-04-05  |
| Microsoft.Storage (Controlelaag) | 2016-01-01   |
| Microsoft.Websites | 2016-01-01 |
| Microsoft.KeyVault | 2016-10-01<br>(Niet wijzigen) |
| Microsoft.Resources<br>(Azure Resource Manager zelf) | 2016-02-01 |
| Microsoft.Authorization<Br>(bewerkingen voor beleid) | 2015-11-01 |
| Microsoft.Insights | 2015-11-01 |
| Microsoft.Keyvault | 2016-10-01 |
| Beleid | 2015-10-01-preview |
| Resources | 2016-02-01 |
| Resources_Links | 2016-09-01 |
| Resources_Locks | 2016-09-01 |
| Abonnementen | 2016-06-1 |

Voor een lijst van de versies van elk resourcetype voor de providers in het API-profiel, Zie de volgende sectie.

## <a name="details-for-the-2018-03-01-hybrid"></a>Details van de 2018-03-01-hybride

### <a name="microsoftauthorization"></a>Microsoft.Authorization

Op rollen gebaseerd toegangsbeheer kunt u de acties die gebruikers in uw organisatie op resources uitvoeren kunnen kunt beheren. Deze set bewerkingen kunt u rollen definiëren, rollen toewijzen aan gebruikers of groepen en informatie over machtigingen verkrijgen. Zie voor meer informatie, [autorisatie](/rest/api/authorization/).

| Resourcetypen | API-versies |
|---------------------|--------------------|
| Vergrendelingen | 2017-04-01 |
| Bewerkingen | 2015-07-01 |
| Machtigingen | 2015-07-01 |
| Beleidstoewijzingen | 12-01-2016 (2017-06-01-preview) |
| Definities voor beleid | 2016-12-01 |
| Providerbewerkingen | 2015-07-01-preview |
| Roltoewijzingen | 2015-07-01 |
| Roldefinities | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft.Commerce

| Resourcetype | API-versie |
|----------------------------------|----------------------|
| Gedelegeerde Providerabonnementen | 2015-06-01 - preview |
| Gedelegeerde gebruik statistische functies | 2015-06-01 - preview |
| Schatting Resource besteden | 2015-06-01-preview |
| Bewerkingen | 2015-06-01 - preview |
| De verzamelde gebruiksgegevens abonnee | 2015-06-01 - preview |
| Verzamelde gebruiksgegevens | 2015-06-01 - preview |

### <a name="microsoftcompute"></a>Microsoft.Compute

De Azure Compute API's bieden u programmatische toegang tot virtuele machines en hun ondersteunende resources. Zie voor meer informatie, [Azure Compute](/rest/api/compute/).

| Resourcetype | API-versie |
|---------------------------------------------------------------|-------------|
| Beschikbaarheidssets | 30-03-2016 |
| Locaties | 30-03-2016 |
| Locations/operations | 30-03-2016 |
| Locations/publishers | 30-03-2016 |
| Locaties/vormen van gebruik | 30-03-2016 |
| Locaties/vmSizes | 30-03-2016 |
| Bewerkingen | 30-03-2016 |
| Virtuele machines | 30-03-2016 |
| Virtuele Machines/extensies | 30-03-2016 |
| Virtual Machine Scale Sets | 30-03-2016 |
| Virtual Machine Scale Sets/extensies | 30-03-2016 |
| Virtual Machine Scale Sets /-netwerkinterfaces | 30-03-2016 |
| Virtual Machine Scale Sets/virtuele Machines | 30-03-2016 |
| Virtual Machines Scale Sets/virtualMachines/networkInterfaces | 30-03-2016 |

### <a name="microsoftgallery"></a>Microsoft.Gallery

| Resourcetype | API-versie |
|------------------|-------------|
| Curatie | 2015-04-01 |
| Curatie-inhoud | 2015-04-01 |
| Curatie-extractie | 2015-04-01 |
| Galerie-Items | 2015-04-01 |
| Bewerkingen | 2015-04-01 |
| Portal | 2015-04-01 |
| Search | 2015-04-01 |
| Voorstellen | 2015-04-01 |

### <a name="microsoftinsights"></a>Microsoft.Insights

| Resourcetypen | API-versies |
|--------------------|--------------------|
| Bewerkingen | 2015-04-01 |
| Evenementcategorieën | 2015-04-01 |
| Gebeurteniscategorieën | 2015-04-01 |
| Metrische definities | 2018-01-01 |
| Metrische gegevens | 2018-01-01 |
| Diagnostische instellingen | 2017-05-01-preview |
| Categorieën van diagnostische instellingen | 2017-05-01-preview |


### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Beheren van uw kluizen en de sleutels, geheimen en certificaten binnen uw sleutelkluizen. Zie voor meer informatie de [Azure Key Vault REST API-verwijzing](/rest/api/keyvault/).

| Resourcetypen | API-versies |
|-------------------------|--------------|
| Bewerkingen | 2016-10-01 |
| Kluizen | 2016-10-01 |
| Kluizen / toegangsbeleid | 2016-10-01 |
| Kluizen/geheimen | 2016-10-01 |

### <a name="microsoftnetwork"></a>Microsoft.Network

De operations-aanroepresultaat is een weergave van de lijst met beschikbare network cloud bewerkingen. Zie voor meer informatie, [bewerking REST-API](/rest/api/operation/).

| Resourcetypen | API-versies |
|---------------------------|--------------|
| Verbindingen | 2015-06-15 |
| DNS-zones | 2016-04-01 |
| Load balancers | 2015-06-15 |
| Lokale netwerkgateway | 2015-06-15 |
| Locaties | 2016-04-01 |
| Locatie/operationResults | 2016-04-01 |
| Locations/operations | 2016-04-01 |
| Locaties/vormen van gebruik | 2016-04-01 |
| Netwerkinterfaces | 2015-06-15 |
| Netwerkbeveiligingsgroepen | 2015-06-15 |
| Bewerkingen | 2015-06-15 |
| Openbaar IP-adres | 2015-06-15 |
| Routetabellen | 2015-06-15 |
| Gateway voor een virtueel netwerk | 2015-06-15 |
| Virtuele netwerken | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

Azure Resource Manager kunt u de infrastructuur voor uw Azure-oplossingen implementeren en beheren. U organiseert gerelateerde resources in resourcegroepen en implementeert uw resources met JSON-sjablonen. Zie voor een inleiding tot het implementeren en beheren van resources met Resource Manager, de [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

| Resourcetypen | API-versies |
|-----------------------------------------|-------------------|
| Toepassingsregistraties | 2015-01-01 |
| Resourcenaam controleren | 2016-09-01 |
| Gedelegeerde Providers | 2015-01-01 |
| Gedelegeerde Providers/aanbiedingen | 2015-01-01 |
| DelegatedProviders/offers/estimatePrice | 2015-01-01 |
| Implementaties | 2016-09-01 |
| Implementaties/operations | 2016-09-01 |
| De metagegevens van de extensies | 2015-01-01 |
| Koppelingen | 2016-09-01 |
| Locaties | 2015-01-01 |
| Aanbiedingen | 2015-01-01 |
| Bewerkingen | 2015-01-01 |
| Providers | 2017-08-01 |
| Resourcegroepen | 2016-09-01 |
| Resources | 2016-09-01 |
| Abonnementen | 2016-09-01 |
| Abonnementen /-locatie | 2016-09-01 |
| Resultaten van de abonnementen/bewerking | 2016-09-01 |
| Abonnementen/providers | 2017-08-01 |
| Abonnementen/resourcegroepen | 2016-09-01 |
| Abonnementen/resourceGroups/resources | 2016-09-01 |
| Abonnementen/resources | 2016-09-01 |
| Subscriptions/tagNames | 2016-09-01 |
| Subscriptions/tagNames/tagValues | 2016-09-01 |
| Tenants | 2017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage 

De Storage Resource Provider (SRP) kunt u uw storage-account en sleutels op programmatische wijze beheren. Zie voor meer informatie de [Azure Storage Resource Provider REST API-verwijzing](/rest/api/storagerp/).

| Resourcetypen | API-versies |
|-------------------------|--------------|
| Beschikbaarheid van de naam controleren | 2016-01-01 |
| Locaties | 2016-01-01 |
| Locaties/quota | 2016-01-01 |
| Bewerkingen | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Het gebruik van | 2016-01-01 |

## <a name="details-for-the-2018-03-01-hybrid"></a>Details van de 2018-03-01-hybride

### <a name="microsoft-authorization"></a>Microsoft-authorisatie

| Resourcetypen | API-versies |
|---------------------|---------------------------------|
| Vergrendelingen | 2017-04-01 |
| Bewerkingen | 2015-07-01 |
| Machtigingen | 2015-07-01 |
| Beleidstoewijzingen | 12-01-2016 (2017-06-01-preview) |
| Definities voor beleid | 2016-12-01 |
| Providerbewerkingen | 2015-07-01-preview |
| Roltoewijzingen | 2015-07-01 |
| Roldefinities | 2015-07-01 |

### <a name="microsoftcompute"></a>Microsoft.Compute

| Resourcetype | API-versie |
|---------------------------------------------------------------|-------------|
| Beschikbaarheidssets | 30-03-2016 |
| Locaties | 30-03-2016 |
| Locations/operations | 30-03-2016 |
| Locations/publishers | 30-03-2016 |
| Locaties/vormen van gebruik | 30-03-2016 |
| Locaties/vmSizes | 30-03-2016 |
| Bewerkingen | 30-03-2016 |
| Virtuele machines | 30-03-2016 |
| Virtuele Machines/extensies | 30-03-2016 |
| Virtual Machine Scale Sets | 30-03-2016 |
| Virtual Machine Scale Sets/extensies | 30-03-2016 |
| Virtual Machine Scale Sets /-netwerkinterfaces | 30-03-2016 |
| Virtual Machine Scale Sets/virtuele Machines | 30-03-2016 |
| Virtual Machines Scale Sets/virtualMachines/networkInterfaces | 30-03-2016 |

### <a name="microsoftnetwork"></a>Microsoft.Network

| Resourcetypen | API-versies |
|---------------------------|--------------|
| Verbindingen | 2015-06-15 |
| DNS-zones | 2016-04-01 |
| Load balancers | 2015-06-15 |
| Lokale netwerkgateway | 2015-06-15 |
| Locaties | 2016-04-01 |
| Locatie/operationResults | 2016-04-01 |
| Locations/operations | 2016-04-01 |
| Locaties/vormen van gebruik | 2016-04-01 |
| Netwerkinterfaces | 2015-06-15 |
| Netwerkbeveiligingsgroepen | 2015-06-15 |
| Bewerkingen | 2015-06-15 |
| Openbaar IP-adres | 2015-06-15 |
| Routetabellen | 2015-06-15 |
| Gateway voor een virtueel netwerk | 2015-06-15 |
| Virtuele netwerken | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

| Resourcetypen | API-versies |
|-----------------------------------------|--------------|
| Toepassingsregistraties | 2015-01-01 |
| Resourcenaam controleren | 2016-09-01 |
| Gedelegeerde Providers | 2015-01-01 |
| Gedelegeerde Providers/aanbiedingen | 2015-01-01 |
| DelegatedProviders/offers/estimatePrice | 2015-01-01 |
| Implementaties | 2016-09-01 |
| Implementaties/operations | 2016-09-01 |
| De metagegevens van de extensies | 2015-01-01 |
| Koppelingen | 2016-09-01 |
| Locaties | 2015-01-01 |
| Aanbiedingen | 2015-01-01 |
| Bewerkingen | 2015-01-01 |
| Providers | 2017-08-01 |
| Resourcegroepen | 2016-09-01 |
| Resources | 2016-09-01 |
| Abonnementen | 2016-09-01 |
| Abonnementen /-locatie | 2016-09-01 |
| Resultaten van de abonnementen/bewerking | 2016-09-01 |
| Abonnementen/providers | 2017-08-01 |
| Abonnementen/resourcegroepen | 2016-09-01 |
| Abonnementen/resourceGroups/resources | 2016-09-01 |
| Abonnementen/resources | 2016-09-01 |
| Subscriptions/tagNames | 2016-09-01 |
| Subscriptions/tagNames/tagValues | 2016-09-01 |
| Tenants | 2017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage

| Resourcetypen | API-versies |
|-------------------------|--------------|
| Beschikbaarheid van de naam controleren | 2016-01-01 |
| Locaties | 2016-01-01 |
| Locaties/quota | 2016-01-01 |
| Bewerkingen | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Het gebruik van | 2016-01-01 |

## <a name="next-steps"></a>Volgende stappen

* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md)  

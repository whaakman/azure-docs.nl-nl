---
title: Resource provider API-versies ondersteund door de profielen in Azure-Stack | Microsoft Docs
description: Meer informatie over de Azure Resource Manager-versie die wordt ondersteund door de profielen in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 1a516c890441c3b703d43f31816b7c37cac364fd
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054378"
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Resource provider API-versies ondersteund door de profielen in Azure-Stack

U vindt de resourceprovider en versienummers voor elk profiel API wordt gebruikt door Azure-Stack in dit artikel. De tabellen in dit artikel worden de versies die worden ondersteund voor elke resourceprovider en de API-versies van de profielen. Elke resourceprovider bevat een set van resourcetypen en specifieke versienummers.

De API-profiel maakt gebruik van drie naamgevingsregels:
 - meest recente
 - jjjj-mm-dd-hybride
 - jjjj-mm-dd-profiel

Zie voor een uitleg van de API-profielen en versie release uitgebracht voor Azure-Stack [versie-profielen API beheren in Azure-Stack](azure-stack-version-profiles.md).

> [!Note]  
> De **nieuwste** API profiel bevat de nieuwste versie van de resource-serviceprovider-API-versie en niet wordt vermeld in dit artikel.

## <a name="overview-of-2018--03-01-hybrid"></a>Overzicht van 2018: 03-01-hybride

| Resourceprovider | API-versie |
|-----------------------------------------------|-----------------------------------------------------|
| Microsoft.Compute | 2017-03-30 |
| Microsoft.Network | 2017-10-01<br>VPN-Gateway worden 2017-03-01 |
| Microsoft.Storage (vlak gegevens) | 2017-04-17 |
| Microsoft.Storage (besturingselement vlak) | 2016-01-01 |
| Microsoft Corporation. Web | 08-01-2016<br>Dit is de meest recente (vanaf nu) in Azure |
| Microsoft.KeyVault | 2016-10-01 (niet wijzigen) |
| Microsoft.Resources (Azure Resource Manager zelf) | 2016-01-02 |
| Microsoft.Authorization (beleid bewerkingen) | 11-01-2015 |
| Microsoft.Insights | 11-01-2015 |
| Microsoft.Keyvault | 2016-10-01 |
| Beleid | 2016-10-01 |
| Resources | 2016-10-01 |
| Resources_Links | 2016-10-01 |
| Resources_Locks | 2016-10-01 |
| Abonnementen | 2016-10-01 |

Zie voor meer een lijst van de versies voor elk resourcetype voor de providers in het profiel api, [Details voor de 2018-03-01-hybride](#details-for-the-2018-03-01-hybrid) profiel.

## <a name="overview-of-2017-03-09-profile"></a>Overzicht van 2017-03-09-profiel

| Resourceprovider | API-versie |
|------------------------------------------------|------------------------------|
| Microsoft.Compute | 2016-03-30 |
| Microsoft.Network | 2015-06-15 |
| Microsoft.Storage (vlak gegevens) | 2015-04-05  |
| Microsoft.Storage (besturingselement vlak) | 2016-01-01   |
| Microsoft.Websites | 2016-01-01 |
| Microsoft.KeyVault | 2016-10-01<br>(Niet wijzigen) |
| Microsoft.Resources<br>(Azure Resource Manager zelf) | 2016-01-02 |
| Microsoft.Authorization<Br>(beleid bewerkingen) | 11-01-2015 |
| Microsoft.Insights | 11-01-2015 |
| Microsoft.Keyvault | 2016-10-01 |
| Beleid | 2015-10-01-preview |
| Resources | 2016-01-02 |
| Resources_Links | 2016-09-01 |
| Resources_Locks | 2016-09-01 |
| Abonnementen | 2016-06-1 |

Zie voor meer een lijst van de versies voor elk resourcetype voor de providers in het profiel api, [Details voor de 2017-03-09-profiel](#details-for-the-2017-03-09-profile)

## <a name="details-for-the-2018-03-01-hybrid"></a>Details voor de 2018-03-01-hybride

### <a name="microsoftauthorization"></a>Microsoft.Authorization

Toegangsbeheer op basis van rollen kunt u de acties die gebruikers in uw organisatie op resources uitvoeren kunnen kunt beheren. Deze reeks bewerkingen kunt u rollen definiëren, rollen toewijzen aan gebruikers of groepen en het verkrijgen van informatie over machtigingen. Zie voor meer informatie [autorisatie](https://docs.microsoft.com/rest/api/authorization/).

| Resourcetypen | API-versies |
|---------------------|--------------------|
| Vergrendelingen | 2017-04-01 |
| Bewerkingen | 2015-07-01 |
| Machtigingen | 2015-07-01 |
| Beleidstoewijzingen | 2016-12-01 (2017-06-01-preview) |
| Definities voor beleid | 2016-12-01 |
| Bewerkingen van de provider | 2015-07-01-preview |
| Roltoewijzingen | 2015-07-01 |
| Roldefinities | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft.Commerce

| Resourcetype | API-versie |
|----------------------------------|----------------------|
| Gedelegeerde Provider abonnementen | 2015-06-01 - preview |
| Gedelegeerde gebruik statistische functies | 2015-06-01 - preview |
| Schatting Resource besteden | 2015-06-01-preview |
| Bewerkingen | 2015-06-01 - preview |
| Abonnee gebruik statistische functies | 2015-06-01 - preview |
| Verzamelde gebruiksgegevens | 2015-06-01 - preview |

### <a name="microsoftcompute"></a>Microsoft.Compute

De Azure Compute-API's bieden u programmatische toegang tot virtuele machines en hun ondersteunende bronnen. Zie voor meer informatie [Azure Compute](https://docs.microsoft.com/rest/api/compute/).

| Resourcetype | API-versie |
|---------------------------------------------------------------|-------------|
| Beschikbaarheidssets | 2016-03-30 |
| Locaties | 2016-03-30 |
| Locaties/operations | 2016-03-30 |
| Locaties/uitgevers | 2016-03-30 |
| Locaties/vormen van gebruik | 2016-03-30 |
| Locaties/vmSizes | 2016-03-30 |
| Bewerkingen | 2016-03-30 |
| Virtuele machines | 2016-03-30 |
| Virtuele Machines/extensies | 2016-03-30 |
| Virtual Machine Scale Sets | 2016-03-30 |
| Virtuele-Machineschaalset Sets/extensies | 2016-03-30 |
| Sets/netwerkinterfaces van virtuele-Machineschaalset | 2016-03-30 |
| Virtuele Machine Scale Sets/virtuele Machines | 2016-03-30 |
| Sets, virtuele machines/networkInterfaces van schaal van de virtuele Machines | 2016-03-30 |

### <a name="microsoftgallery"></a>Microsoft.Gallery

| Resourcetype | API-versie |
|------------------|-------------|
| Curatie | 2015-04-01 |
| Curatie-inhoud | 2015-04-01 |
| Curatie-extractie | 2015-04-01 |
| Galerij-Items | 2015-04-01 |
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
| Diagnostische instellingen voor categorieën | 2017-05-01-preview |


### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Het beheren van uw sleutel en de sleutels, geheimen en certificaten binnen uw sleutelkluizen kluizen. Zie voor meer informatie [naslaginformatie over REST API van Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/).

| Resourcetypen | API-versies |
|-------------------------|--------------|
| Bewerkingen | 2016-10-01 |
| kluizen | 2016-10-01 |
| Kluizen / toegangsbeleid | 2016-10-01 |
| Kluizen/geheimen | 2016-10-01 |

### <a name="microsoftnetwork"></a>Microsoft.Network

Bewerkingen aanroep resultaat is een weergave van de lijst beschikbare netwerk cloud-bewerkingen. Zie voor meer informatie [bewerking REST-API](https://docs.microsoft.com/rest/api/operation/).

| Resourcetypen | API-versies |
|---------------------------|--------------|
| Verbindingen | 2015-06-15 |
| DNS-Zones | 2016-04-01 |
| Load Balancers | 2015-06-15 |
| Lokale netwerkgateway | 2015-06-15 |
| Locaties | 2016-04-01 |
| Locatie/operationResults | 2016-04-01 |
| Locaties/operations | 2016-04-01 |
| Locaties/vormen van gebruik | 2016-04-01 |
| Netwerkinterfaces | 2015-06-15 |
| Netwerkbeveiligingsgroepen | 2015-06-15 |
| Bewerkingen | 2015-06-15 |
| Openbaar IP-adres | 2015-06-15 |
| Routetabellen | 2015-06-15 |
| Gateway voor een virtueel netwerk | 2015-06-15 |
| Virtuele netwerken | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

Azure Resource Manager kunt u de infrastructuur voor uw Azure-oplossingen implementeren en beheren. U verwante resources in resourcegroepen ordenen en implementeren van uw resources met JSON-sjablonen. Zie voor een inleiding tot het implementeren en beheren van resources met Resource Manager, [overzicht van Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

| Resourcetypen | API-versies |
|-----------------------------------------|-------------------|
| Registraties van toepassing | 2015-01-01 |
| Resourcenaam controleren | 2015-012016-09-01 |
| Gedelegeerde Providers | 2015-01-01 |
| Gedelegeerde Providers/aanbiedingen | 2015-01-01 |
| Aanbiedingen-DelegatedProviders/estimatePrice | 2015-01-01 |
| Implementaties | 2016-0209-01 |
| Implementaties/operations | 2016-0209-01 |
| Extensies metagegevens | 2015-01-01 |
| Koppelingen | 2015-012016-09-01 |
| Locaties | 2015-01-01 |
| Aanbiedingen | 2015-01-01 |
| Bewerkingen | 2015-01-01 |
| Providers | 2015-012017-08-01 |
| Resourcegroepen | 2015-012016-09-01 |
| Resources | 2015-012016-09-01 |
| Abonnementen | 2015-012016-09-01 |
| Abonnementen of de locatie | 2015-012016-09-01 |
| Resultaat van de abonnementen/bewerking | 2015-012016-09-01 |
| Abonnementen/providers | 2015-012017-08-01 |
| Abonnementen/resourcegroepen | 2015-012016-09-01 |
| ResourceGroups-abonnementen-resources | 2015-012016-09-01 |
| Abonnementen/resources | 2015-012016-09-01 |
| Abonnementen/tagNames | 2016-0609-01 |
| TagNames-abonnementen/tagValues | 2016-0609-01 |
| Tenants | 2015-012017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage 

Storage Resource Provider (SRP) kunt u uw opslagaccount en sleutels programmatisch te beheren. Zie voor meer informatie [Azure Storage Resource Provider REST API-verwijzing](https://docs.microsoft.com/rest/api/storagerp/).

| Resourcetypen | API-versies |
|-------------------------|--------------|
| Beschikbaarheid van de naam controleren | 2016-01-01 |
| Locaties | 2016-01-01 |
| Locaties/quota | 2016-01-01 |
| Bewerkingen | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Sleutelgebruik | 2016-01-01 |

## <a name="details-for-the-2017-03-09-profile"></a>Details voor de 2017-03-09-profiel

### <a name="microsoft-authorization"></a>Microsoft-autorisatie

| Resourcetypen | API-versies |
|---------------------|---------------------------------|
| Vergrendelingen | 2017-04-01 |
| Bewerkingen | 2015-07-01 |
| Machtigingen | 2015-07-01 |
| Beleidstoewijzingen | 2016-12-01 (2017-06-01-preview) |
| Definities voor beleid | 2016-12-01 |
| Bewerkingen van de provider | 2015-07-01-preview |
| Roltoewijzingen | 2015-07-01 |
| Roldefinities | 2015-07-01 |

### <a name="microsoftcompute"></a>Microsoft.Compute

| Resourcetype | API-versie |
|---------------------------------------------------------------|-------------|
| Beschikbaarheidssets | 2016-03-30 |
| Locaties | 2016-03-30 |
| Locaties/operations | 2016-03-30 |
| Locaties/uitgevers | 2016-03-30 |
| Locaties/vormen van gebruik | 2016-03-30 |
| Locaties/vmSizes | 2016-03-30 |
| Bewerkingen | 2016-03-30 |
| Virtuele machines | 2016-03-30 |
| Virtuele Machines/extensies | 2016-03-30 |
| Virtual Machine Scale Sets | 2016-03-30 |
| Virtuele-Machineschaalset Sets/extensies | 2016-03-30 |
| Sets/netwerkinterfaces van virtuele-Machineschaalset | 2016-03-30 |
| Virtuele Machine Scale Sets/virtuele Machines | 2016-03-30 |
| Sets, virtuele machines/networkInterfaces van schaal van de virtuele Machines | 2016-03-30 |

### <a name="microsoftnetwork"></a>Microsoft.Network

| Resourcetypen | API-versies |
|---------------------------|--------------|
| Verbindingen | 2015-06-15 |
| DNS-Zones | 2016-04-01 |
| Load Balancers | 2015-06-15 |
| Lokale netwerkgateway | 2015-06-15 |
| Locaties | 2016-04-01 |
| Locatie/operationResults | 2016-04-01 |
| Locaties/operations | 2016-04-01 |
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
| Registraties van toepassing | 2015-01-01 |
| Resourcenaam controleren | 2016-09-01 |
| Gedelegeerde Providers | 2015-01-01 |
| Gedelegeerde Providers/aanbiedingen | 2015-01-01 |
| Aanbiedingen-DelegatedProviders/estimatePrice | 2015-01-01 |
| Implementaties | 2016-09-01 |
| Implementaties/operations | 2016-09-01 |
| Extensies metagegevens | 2015-01-01 |
| Koppelingen | 2016-09-01 |
| Locaties | 2015-01-01 |
| Aanbiedingen | 2015-01-01 |
| Bewerkingen | 2015-01-01 |
| Providers | 2017-08-01 |
| Resourcegroepen | 2016-09-01 |
| Resources | 2016-09-01 |
| Abonnementen | 2016-09-01 |
| Abonnementen of de locatie | 2016-09-01 |
| Resultaat van de abonnementen/bewerking | 2016-09-01 |
| Abonnementen/providers | 2017-08-01 |
| Abonnementen/resourcegroepen | 2016-09-01 |
| ResourceGroups-abonnementen-resources | 2016-09-01 |
| Abonnementen/resources | 2016-09-01 |
| Subscriptiosn/tagNames | 2016-09-01 |
| TagNames-abonnementen/tagValues | 2016-09-01 |
| Tenants | 2017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage

| Resourcetypen | API-versies |
|-------------------------|--------------|
| Beschikbaarheid van de naam controleren | 2016-01-01 |
| Locaties | 2016-01-01 |
| Locaties/quota | 2016-01-01 |
| Bewerkingen | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Sleutelgebruik | 2016-01-01 |

## <a name="next-steps"></a>Volgende stappen

* [PowerShell voor Azure Stack installeren](azure-stack-powershell-install.md)
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md)  

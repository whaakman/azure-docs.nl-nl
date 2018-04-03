---
title: Resource provider API-versies ondersteund door de profielen in Azure-Stack | Microsoft Docs
description: Meer informatie over de Azure Resource Manager-versie die wordt ondersteund door de profielen in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 2E21C8DE-D540-4C1C-A0EF-1B7125DB7A6E
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 8cc5ce1ec113df7ce92c54022dbe1b6219c8c235
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Resource provider API-versies ondersteund door de profielen in Azure-Stack

Een Azure-resourceprovider biedt resources die u kunt implementeren en beheren via de Azure Resource Manager. Elke provider biedt bewerkingen voor het werken met resources. Sommige algemene resourceproviders omvatten Microsoft.Compute die virtuele machines levert, Microsoft.Storage die account opslagbronnen levert, en Microsoft.Web die betrekking hebben op web-apps levert. Zie [Resourceproviders en -typen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services) voor meer informatie.

De volgende tabel voor elke resourceprovider geeft aan van de ondersteunde versie van de API-versie voor Azure-Stack bij gebruik van profielen.

### <a name="microsoftauthorization"></a>Microsoft.Authorization

Toegangsbeheer op basis van rollen kunt u de acties die gebruikers in uw organisatie op resources uitvoeren kunnen kunt beheren. Deze reeks bewerkingen kunt u rollen definiëren, rollen toewijzen aan gebruikers of groepen en het verkrijgen van informatie over machtigingen. Zie voor meer informatie [autorisatie](https://docs.microsoft.com/rest/api/authorization/).

| Brontypen | API-versies |
|---------------------|--------------------|
| Vergrendelingen | 2017-04-01 |
| Bewerkingen | 2015-07-01 |
| Machtigingen | 2015-07-01 |
| Beleidstoewijzingen | 2016-12-01 (2017-06-01-preview) |
| Beleidsdefinities | 2016-12-01 |
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

De Azure Compute-API's bieden u programmatische toegang tot virtuele machines en hun ondersteunende bronnen. Zie voor meer informatie [Azure Compute](https://docs.microsoft.com/en-us/rest/api/compute/).

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
| Schaalsets voor virtuele machines | 2016-03-30 |
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

| Brontypen | API-versies |
|--------------------|--------------------|
| Waarschuwingsregels | 2016-03-01 |
| Gebeurteniscategorieën | 2017-03-01-preview |
| Evenementcategorieën | 2017-03-01-preview |
| Metrische definities | 2016-03-01 |
| Bewerkingen | 2015-04-01 |

### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Het beheren van uw sleutel en de sleutels, geheimen en certificaten binnen uw sleutelkluizen kluizen. Zie voor meer informatie [naslaginformatie over REST API van Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/).

| Brontypen | API-versies |
|-------------------------|--------------|
| Bewerkingen | 2016-10-01 |
| Kluizen | 2016-10-01 |
| Kluizen / toegangsbeleid | 2016-10-01 |
| Kluizen/geheimen | 2016-10-01 |

### <a name="microsoftkeyvaultadmin"></a>Microsoft.Keyvault.Admin

Het beheren van uw sleutel en de sleutels, geheimen en certificaten binnen uw sleutelkluizen kluizen. Zie voor meer informatie [naslaginformatie over REST API van Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/).

| Brontypen | API-versies |
|------------------|--------------------|
| Locaties | 2017-02-01-preview |
| Locaties/quota | 2017-02-01-preview |

### <a name="microsoftnetwork"></a>Microsoft.Network

Bewerkingen aanroep resultaat is een weergave van de lijst beschikbare netwerk cloud-bewerkingen. Zie voor meer informatie [bewerking REST-API](https://docs.microsoft.com/rest/api/operation/).

| Brontypen | API-versies |
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

| Brontypen | API-versies |
|-----------------------------------------|-------------------|
| Registraties van toepassing | 2015-01-01 |
| Resourcenaam controleren | 2015-012016-09-01 |
| Gedelegeerde Providers | 2015-01-01 |
| Gedelegeerde Providers/aanbiedingen | 2015-01-01 |
| DelegatedProviders/offers/estimatePrice | 2015-01-01 |
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

| Brontypen | API-versies |
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

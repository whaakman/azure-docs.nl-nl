---
title: Azure Monitor buiten gebruik stellen van klassieke implementatiemodel API's voor metrische gegevens en voor automatisch schalen
description: Metrische gegevens en voor automatisch schalen klassieke API's, ook wel Azure Service Management (ASM) of RDFE-implementatiemodel is buiten gebruik gesteld
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/19/2018
ms.author: robb
ms.component: ''
ms.openlocfilehash: 450eac3613df18997c5b0f9189555555da20cfcf
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585532"
---
# <a name="azure-monitor-retirement-of-classic-deployment-model-apis-for-metrics-and-autoscale"></a>Azure Monitor buiten gebruik stellen van klassieke implementatiemodel API's voor metrische gegevens en voor automatisch schalen

Azure Monitor (voorheen Azure Insights als voor het eerst uitgebracht) op dit moment biedt de mogelijkheid om te maken en beheren van instellingen voor automatisch schalen en metrische gegevens van klassieke virtuele machines en klassieke Cloud Services gebruiken. De oorspronkelijke set met klassieke implementatie op basis van model-API's worden **buiten gebruik gesteld na 30 juni 2019** in alle Azure openbare en persoonlijke clouds in alle regio's.   

Dezelfde bewerkingen worden ondersteund via een set van Azure Resource Manager gebaseerde API's voor een jaar. De Azure portal maakt gebruik van de nieuwe REST-API's voor zowel automatisch schalen en metrische gegevens. Een nieuwe SDK, PowerShell en CLI op basis van deze Resource Manager-API's zijn ook beschikbaar. Onze partnerservices voor het bewaken van de nieuwe gebruiken in Azure Monitor REST-API's op basis van Resource Manager.  

## <a name="who-is-not-affected"></a>Die wordt niet beïnvloed

Als u automatisch schalen via de Azure portal, beheert de [nieuwe SDK van Azure Monitor](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/), PowerShell, CLI of Resource Manager-sjablonen, is geen actie nodig.  

Als u metrische gegevens via de Azure-portal of via verschillende verbruikt [partner bewakingsservices](../../monitoring-and-diagnostics/monitoring-partners.md), is geen actie nodig. Microsoft werkt samen met partners om te migreren naar de nieuwe API's bewaken.

## <a name="who-is-affected"></a>Die worden beïnvloed

In dit artikel is van toepassing op u, als u de volgende onderdelen:

- **Klassieke Azure Insights-SDK** : als u de [klassieke Azure Insights-SDK](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/), overschakelen op het gebruik van de nieuwe Azure Monitor-SDK voor [.NET](https://github.com/azure/azure-libraries-for-net#download) of [Java](https://github.com/azure/azure-libraries-for-java#download). Download de [Azure Monitor SDK NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/).

- **Klassieke voor automatisch schalen** : als u belt de [instellingen van de klassieke voor automatisch schalen API's](https://msdn.microsoft.com/library/azure/mt348562.aspx) van uw op maat gemaakte hulpprogramma's of met behulp van de [klassieke Azure Insights-SDK](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/), moet u overstappen naar de [ Resource Manager Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/autoscalesettings).

- **Klassieke metrische gegevens** : als u verbruikt metrische gegevens met behulp van bent de [klassieke REST-API's](https://msdn.microsoft.com/library/azure/dn510374.aspx) of [klassieke Azure Insights-SDK](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/) van op maat gemaakte hulpprogramma's, moet u overstappen naar de [ Resource Manager Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/autoscalesettings). 

Als u niet zeker weet of uw code of aangepaste hulpprogramma's voor de klassieke API's aanroept, bekijkt u het volgende:

- Bekijk de URI waarnaar wordt verwezen in uw code of hulpprogramma. Gebruik de klassieke API's van de URI https://management.core.windows.net. U moet worden met behulp van de nieuwere URI voor de Resource Manager gebaseerde API's met begint https://management.azure.com/.

- Vergelijk de assemblynaam op uw computer. De oudere klassieke assembly loopt https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/.

- Als u verificatie via certificaat gebruikt voor toegang tot metrische gegevens of voor automatisch schalen API's, gebruikt u een klassieke eindpunt en de bibliotheek. De nieuwere Resource Manager-API's vereist Azure Active Directory-verificatie via een service-principal of een gebruiker-principal.

- Als u aanroepen waarnaar wordt verwezen in de documentatie bij elk van de volgende koppelingen, kunt u de oudere klassieke API's gebruiken.

  - [Windows.Azure.Management.Monitoring klassebibliotheek](https://docs.microsoft.com/previous-versions/azure/dn510414(v=azure.100))

  - [Bewaking van .NET (klassiek)](https://docs.microsoft.com/previous-versions/azure/reference/mt348562(v%3dazure.100))

  - [IMetricOperations-Interface](https://docs.microsoft.com/previous-versions/azure/reference/dn802395(v%3dazure.100))

## <a name="why-you-should-switch"></a>Waarom moet u overschakelen

Alle bestaande mogelijkheden voor automatisch schalen en metrische gegevens blijven werken via de nieuwe API's.  

Migreren via naar nieuwere API's worden geleverd met Resource Manager gebaseerde mogelijkheden, zoals ondersteuning voor consistente Role-Based Access Control (RBAC) in uw bewakingsservices. U krijgt ook aanvullende functionaliteit voor metrische gegevens: 

- ondersteuning voor dimensies
- consistente 1 minuut metrische granulariteit op alle services 
- beter uitvoeren van query 's
- hogere Gegevensretentie (93 dagen van metrische gegevens-vs. 30 dagen) 

Algemene, net als alle andere services in Azure, de Resource Manager gebaseerde Azure Monitor API's worden geleverd met betere prestaties, schaalbaarheid en betrouwbaarheid. 

## <a name="what-happens-if-you-do-not-migrate"></a>Wat gebeurt er als u niet migreren

### <a name="before-retirement"></a>Voordat u buiten gebruik stellen

Er is geen directe impact op uw Azure-services of hun workloads.  

### <a name="after-retirement"></a>Na het buiten gebruik stellen

De aanroepen voor het klassieke API's die eerder vermeld mislukken en retourneren foutberichten ongeveer de volgende query:

Voor automatisch schalen: *deze API is afgeschaft. De Azure portal, de SDK van Azure Monitor, PowerShell, CLI of Resource Manager-sjablonen gebruiken voor het beheren van instellingen voor automatisch schalen*.  

Voor metrische gegevens: *deze API is afgeschaft. Gebruik de Azure portal, Azure Monitor-SDK, PowerShell, CLI aan query voor metrische gegevens*.

## <a name="email-notifications"></a>E-mailmeldingen

Een melding buiten gebruik stellen is verzonden naar het e-mailadressen voor de volgende rollen voor account: 

- Beheerders van account en -service
- Medebeheerders  

Als u vragen hebt, contact met ons op MonitorClassicAPIhelp@microsoft.com.  

## <a name="references"></a>Verwijzingen

- [Nieuwere REST-API's voor Azure Monitor](https://docs.microsoft.com/rest/api/monitor/) 
- [Nieuwere Azure Monitor SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/)

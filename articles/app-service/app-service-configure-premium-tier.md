---
title: PremiumV2-laag - Azure App Service configureren | Microsoft Docs
description: Leer hoe u betere prestaties voor uw web, mobiele Apps en API-app in Azure App Service door te schalen naar de nieuwe prijscategorie PremiumV2-laag.
keywords: App Service, Azure App Service, schaal, schaalbaar, App Service-abonnement, kosten App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: ff00902b-9858-4bee-ab95-d3406018c688
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 5fe3d9369958984b76fd704583460a95868da85e
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114210"
---
# <a name="configure-premiumv2-tier-for-azure-app-service"></a>PremiumV2-laag voor Azure App Service configureren

De nieuwe **PremiumV2** prijscategorie biedt snellere processors, SSD-opslag en verdubbelt de geheugen-naar-core-verhouding van de bestaande Prijscategorieën. Met het voordeel van prestaties, kan u geld besparen door te voeren van uw apps op minder exemplaren. In dit artikel leert u hoe u het maken van een app in **PremiumV2** laag of een app opschalen **PremiumV2** laag.

## <a name="prerequisites"></a>Vereisten

Scale-up van een app aan **PremiumV2**, moet u beschikken over een Azure App Service-app die wordt uitgevoerd in een lager is dan prijscategorie **PremiumV2**, en de app moet worden uitgevoerd in een App Service-implementatie die ondersteuning biedt voor PremiumV2.

<a name="availability"></a>

## <a name="premiumv2-availability"></a>PremiumV2-beschikbaarheid

De **PremiumV2** laag is beschikbaar voor App Service op beide _Windows_ , evenals _Linux_.

**PremiumV2** is beschikbaar in de meeste Azure-regio's. Als u wilt zien als deze beschikbaar in uw regio is, kunt u de volgende Azure CLI-opdracht uitvoeren de [Azure Cloud Shell](../cloud-shell/overview.md):

```azurecli-interactive
az appservice list-locations --sku P1V2
```

<a name="create"></a>

## <a name="create-an-app-in-premiumv2-tier"></a>Een app maken in PremiumV2-laag

De prijscategorie van een App Service-app is gedefinieerd in de [App Service-plan](overview-hosting-plans.md) waarop deze wordt uitgevoerd. U kunt een App Service-plan maken door zelf of als onderdeel van het maken van apps.

Bij het configureren van de App Service-plan in de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, selecteer **prijscategorie**. 

Selecteer **productie**en selecteer vervolgens **P1V2**, **P2V2**, of **P3V2**, klikt u vervolgens op **toepassen**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

> [!IMPORTANT] 
> Als er geen **P1V2**, **P2V2**, en **P3V2** als opties, of als de opties worden grijs weergegeven out, klikt u vervolgens **PremiumV2** waarschijnlijk niet beschikbaar is in de onderliggende App Service-implementatie met de App Service-plan. Zie [kan worden uitgebreid van de combinatie van een niet-ondersteund resourcetype groep en regio](#unsupported) voor meer informatie.

## <a name="scale-up-an-existing-app-to-premiumv2-tier"></a>Een bestaande app te PremiumV2-laag omhoog schalen

Voordat u een bestaande App opschalen tot **PremiumV2** laag, zorg ervoor dat **PremiumV2** beschikbaar is. Zie voor meer informatie, [PremiumV2 beschikbaarheid](#availability). Als deze niet beschikbaar is, raadpleegt u [kan worden uitgebreid van de combinatie van een niet-ondersteund resourcetype groep en regio](#unsupported).

Afhankelijk van uw hostingomgeving omhoog schalen mogelijk extra stappen. 

In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, opent u de App Service-app-pagina.

Selecteer in het linkernavigatievenster van de App Service-app-pagina, **opschalen (App Service-plan)**.

![](media/app-service-configure-premium-tier/scale-up-tier-portal.png)

Selecteer **productie**en selecteer vervolgens **P1V2**, **P2V2**, of **P3V2**, klikt u vervolgens op **toepassen**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

Als de bewerking voltooid is, de overzichtspagina van uw app wordt aangegeven dat het nu een **PremiumV2** laag.

![](media/app-service-configure-premium-tier/finished.png)

### <a name="if-you-get-an-error"></a>Als u een fout optreedt

Sommige App Service-plannen kunnen niet omhoog schalen naar de prijscategorie PremiumV2 als PremiumV2 biedt geen ondersteuning voor de onderliggende App Service-implementatie.  Zie [kan worden uitgebreid van de combinatie van een niet-ondersteund resourcetype groep en regio](#unsupported) voor meer informatie.

<a name="unsupported"></a>

## <a name="scale-up-from-an-unsupported-resource-group-and-region-combination"></a>Omhoog schalen van de combinatie van een niet-ondersteund resourcetype groep en regio

Als uw app wordt uitgevoerd in een App Service-implementatie waarbij **PremiumV2** niet beschikbaar is, of als uw app wordt uitgevoerd in een regio die momenteel niet ondersteund door **PremiumV2**, moet u uw app om te opnieuw te implementeren profiteren van **PremiumV2**.  U hebt hiervoor twee opties:

- Maakt een **nieuwe** resource groep en maak vervolgens een **nieuwe** app en App Service-plan in de **nieuwe** resourcegroep, de gewenste Azure-regio kiezen tijdens het proces voor het maken.  U **moet** selecteert u de **PremiumV2** abonnement op het moment dat de nieuwe app service-plan is gemaakt.  Dit zorgt ervoor dat de combinatie van App Service-abonnement, resourcegroep en Azure-regio resulteert in de App Service-plan wordt gemaakt in een App Service-implementatie die ondersteuning biedt voor **PremiumV2**.  Code van uw toepassing vervolgens implementeren in de zojuist gemaakte app- en app service-plan. Indien gewenst kunt u vervolgens de App Service-plan omlaag schalen van **PremiumV2** kosten om op te slaan, en kunt u zich nog steeds met succes back omhoog schalen opnieuw in de toekomst met **PremiumV2**.
- Als uw app al wordt uitgevoerd in een bestaande **Premium** tier, en vervolgens kunt u uw app klonen met alle app-instellingen, verbindingsreeksen en configuratie van de implementatie in een nieuw app service-plan, die gebruikmaakt van **PremiumV2**.

    ![](media/app-service-configure-premium-tier/clone-app.png)

    In de **app klonen** pagina, kunt u een App Service-plan met **PremiumV2** in de regio u wilt en geef de app-instellingen en configuratie die u wilt klonen.

## <a name="automate-with-scripts"></a>Automatiseren met scripts

U kunt automatiseren, het maken van apps in de **PremiumV2** laag met scripts, met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure-CLI

De volgende opdracht maakt u een App Service-plan in _P1V2_. U kunt deze uitvoeren in de Cloud Shell. De opties voor `--sku` P1V2, zijn _P2V2_, en _P3V2_.

```azurecli-interactive
az appservice plan create \
    --resource-group <resource_group_name> \
    --name <app_service_plan_name> \
    --sku P1V2
```

### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

De volgende opdracht maakt u een App Service-plan in _P1V2_. De opties voor `-WorkerSize` zijn _kleine_, _gemiddeld_, en _grote_.

```PowerShell
New-AzAppServicePlan -ResourceGroupName <resource_group_name> `
    -Name <app_service_plan_name> `
    -Location <region_name> `
    -Tier "PremiumV2" `
    -WorkerSize "Small"
```
## <a name="more-resources"></a>Meer bronnen

[Een app in Azure omhoog schalen](web-sites-scale.md)  
[Het aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md)

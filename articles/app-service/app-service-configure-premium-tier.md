---
title: PremiumV2 laag configureren voor Azure App Service | Microsoft Docs
description: Informatie over het betere prestaties voor uw web-, mobiele en API-app in Azure App Service door naar de nieuwe PremiumV2 prijscategorie schalen.
keywords: App Service, Azure App Service, schaal, schaalbaar, App Service-abonnement, kosten App Service
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: ff00902b-9858-4bee-ab95-d3406018c688
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: cephalin
ms.openlocfilehash: 92cc8d8b0f67dde95ea2e3fc2f0f083bd8ac8aab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-premiumv2-tier-for-azure-app-service"></a>PremiumV2 laag configureren voor Azure App Service

De nieuwe **PremiumV2** prijscategorie biedt [Dv2-serie VMs](../virtual-machines/windows/sizes-general.md#dv2-series) met snellere processors, SSD-opslag en dubbele geheugen-core-verhouding vergeleken met **standaard** laag. In dit artikel leert u informatie over het maken van een app in **PremiumV2** servicetier of een app opschalen **PremiumV2** laag.

## <a name="prerequisites"></a>Vereisten

Scale-up van een web-app naar **PremiumV2**, moet u beschikken over een Web-App in Azure App Service die wordt uitgevoerd in een prijscategorie lager is dan **PremiumV2**.

<a name="availability"></a>

## <a name="premiumv2-availability"></a>PremiumV2 beschikbaarheid

De laag PremiumV2 is momenteel beschikbaar voor App-Service op _Windows_ alleen. Linux-containers zijn nog niet ondersteund.

PremiumV2 is al beschikbaar in de meeste Azure-regio's en groeiende. Om te zien of deze beschikbaar in uw regio is, kunt u de volgende Azure CLI-opdracht uitvoert in de [Azure Cloud Shell](../cloud-shell/overview.md):

```azurecli-interactive
az appservice list-locations --sku P1V2
```

Als er een fout opgetreden tijdens het maken van de app of App Service-abonnement maken, klikt u vervolgens **PremiumV2** waarschijnlijk niet beschikbaar voor uw regio naar keuze.

<a name="create"></a>

## <a name="create-an-app-in-premiumv2-tier"></a>Een app maken in PremiumV2 laag

De prijscategorie van een App Service-app is gedefinieerd in de [App Service-abonnement](azure-web-sites-web-hosting-plans-in-depth-overview.md) die op wordt uitgevoerd. U kunt een App Service-abonnement maken zelfstandig of als onderdeel van Web-App maken.

Bij het configureren van de App Service-abonnement in de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, selecteer **prijscategorie**. 

Kies een van de **PremiumV2** opties en klik op **Selecteer**.

![](media/app-service-configure-premium-tier/pick-premium-tier.png)

> [!IMPORTANT] 
> Als er geen **P1V2**, **P2V2**, en **P3V2** als opties, ofwel **PremiumV2** is niet beschikbaar in uw regio keuze, of u bent configureren van een Linux-App Service-abonnement biedt geen ondersteuning voor **PremiumV2**.

## <a name="scale-up-an-existing-app-to-premiumv2-tier"></a>Een bestaande app zodanig PremiumV2 laag opschalen

Voordat u een bestaande app zodanig te schalen **PremiumV2** servicetier, zorg ervoor dat **PremiumV2** is beschikbaar in uw regio. Zie voor informatie [PremiumV2 beschikbaarheid](#availability). Als deze niet beschikbaar in uw regio is, Zie [opschalen van een niet-ondersteunde regio](#unsupported).

Afhankelijk van uw hostingomgeving omhoog schalen mogelijk extra stappen nodig. 

In de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>, open de pagina van uw App Service-app.

Selecteer in het linkernavigatievenster van uw App Service-app-pagina **opschalen (App Service-abonnement)**.

![](media/app-service-configure-premium-tier/scale-up-tier-portal.png)

Selecteer een van de **PremiumV2** grootte en klik vervolgens op **Selecteer**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

Als de bewerking voltooid wordt, ziet u de pagina overzicht van uw app dat het is nu een **PremiumV2** laag.

![](media/app-service-configure-premium-tier/finished.png)

### <a name="if-you-get-an-error"></a>Als u een fout krijgt

Sommige App Service-abonnementen kunnen niet omhoog schalen aan de laag PremiumV2. Als uw bewerking scale-up u een fout biedt, moet u een nieuw App Service-plan voor uw app.

Maak een _Windows_ App Service-abonnement in dezelfde regio en resourcegroep bevinden als uw bestaande App Service-app. Volg de stappen in [een app maken in de laag PremiumV2](#create) worden ingesteld op **PremiumV2** laag. Indien gewenst, kunt u dezelfde scale-out configuratie gebruiken als uw App Service-abonnement (aantal exemplaren, automatisch schalen, enzovoort).

Open de pagina van uw App Service-app opnieuw. Selecteer in het linkernavigatievenster van uw App Service **wijziging App Service-abonnement**.

![](media/app-service-configure-premium-tier/change-plan.png)

Selecteer de App Service-abonnement dat u zojuist hebt gemaakt.

![](media/app-service-configure-premium-tier/select-plan.png)

Nadat de wijzigingsbewerking is voltooid, uw app wordt uitgevoerd **PremiumV2** laag.

<a name="unsupported"></a>

## <a name="scale-up-from-an-unsupported-region"></a>Omhoog schalen van een niet-ondersteunde regio

Als uw app wordt uitgevoerd in een regio waar **PremiumV2** is nog niet beschikbaar, kunt u uw app verplaatsen naar een andere regio om te profiteren van **PremiumV2**. U hebt hiervoor twee opties:

- Maken van een app in nieuwe **PremiumV2** plannen en implementeren van uw toepassingscode. Volg de stappen in [een app maken in de laag PremiumV2](#create) worden ingesteld op **PremiumV2** laag. Indien gewenst, kunt u dezelfde scale-out configuratie gebruiken als uw App Service-abonnement (aantal exemplaren, automatisch schalen, enzovoort).
- Als uw app wordt al uitgevoerd in een bestaand **Premium** servicetier, en vervolgens u uw app met alle app-instellingen, verbindingsreeksen en implementatieconfiguratie klonen kunt.

    ![](media/app-service-configure-premium-tier/clone-app.png)

    In de **kloon app** pagina kunt u een nieuw App Service-plan maken in de regio die u wilt en geef de instellingen die u wilt klonen.

## <a name="automate-with-scripts"></a>Automatiseren met behulp van scripts

U kunt automatiseren app maken in de **PremiumV2** laag met scripts, met behulp van de [Azure CLI](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

De volgende opdracht maakt u een App Service-abonnement in _P1V2_. U kunt deze uitvoeren in de Cloud-Shell. De opties voor `--sku` P1V2, zijn _P2V2_, en _P3V2_.

```azurecli-interactive
az appservice plan create \
    --resource-group <resource_group_name> \
    --name <app_service_plan_name> \
    --sku P1V2
```

### <a name="azure-powershell"></a>Azure PowerShell

De volgende opdracht maakt u een App Service-abonnement in _P1V2_. De opties voor `-WorkerSize` zijn _kleine_, _gemiddeld_, en _grote_.

```PowerShell
New-AzureRmAppServicePlan -ResourceGroupName <resource_group_name> `
    -Name <app_service_plan_name> `
    -Location <region_name> `
    -Tier "PremiumV2" `
    -WorkerSize "Small"
```
## <a name="more-resources"></a>Meer bronnen

[Een app in Azure opschalen](web-sites-scale.md)  
[Het aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md)
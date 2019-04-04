---
title: Azure Functions-Premium-abonnement (preview) | Microsoft Docs
description: Meer informatie en configuratie-opties (VNet, geen koude start, onbeperkte uitvoeringstijd) voor de Azure Functions-Premium-plan.
services: functions
author: jeffhollan
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: jehollan
ms.openlocfilehash: ca65b6a1691a870054682b36109f2bdc10d4ad98
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918702"
---
# <a name="azure-functions-premium-plan-preview"></a>Azure Functions-Premium-abonnement (preview)

De Azure Functions Premium-abonnement is een optie voor hosting voor Functions-apps. Het Premium-abonnement bevat functies zoals VNet-connectiviteit, geen koude start en premium-hardware.  Meerdere functie-apps kunnen worden geïmplementeerd op de dezelfde Premium-abonnement en het abonnement kunt u configureren compute-instantiegrootte, basisplan grootte en grootte van de maximale plan.  Zie voor een vergelijking van de Premium-abonnement en andere plan en die als host fungeert typen [schaal en hosting van de opties werken](functions-scale.md).

> [!NOTE]
> De Premium-plan preview biedt ondersteuning voor functies die worden uitgevoerd in .NET, knooppunt of Java via Windows-infrastructuur.

## <a name="create-a-premium-plan"></a>Een Premium-abonnement maken

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

U kunt ook een Premium-abonnement maken vanuit de Azure CLI

```azurecli-interactive
az functionapp plan create -g <resource-group> -n <plan-name> -l <region> --number-of-workers 1 --sku EP1
```

## <a name="features"></a>Functies

De volgende functies zijn beschikbaar voor functie-apps die zijn geïmplementeerd op een Premium-abonnement.

### <a name="pre-warmed-instances"></a>Vooraf verwarmde exemplaren

Als er geen gebeurtenissen en uitvoeringen vandaag nog in het abonnement Consumption optreden, kan uw app omlaag schalen naar nul exemplaren. Wanneer nieuwe gebeurtenissen afkomstig is, moet een nieuw exemplaar speciaal worden geconfigureerd met uw app die erop worden uitgevoerd.  Nieuwe instanties gespecialiseerd duurt enige tijd, afhankelijk van de app.  Deze extra latentie in de eerste aanroep wordt vaak aangeduid als app koude start.

U kunt uw app vooraf warm gelopen op een opgegeven aantal exemplaren hebben in het Premium-abonnement.  Vooraf verwarmde instanties kunnen u een app voor hoge belasting vooraf schalen. Wanneer de app wordt geschaald, wordt het eerst in de vooraf verwarmde exemplaren. Extra exemplaren blijven voor het bufferen van en warme onmiddellijk ter voorbereiding op de volgende schaalbewerking. Doordat een buffer van vooraf verwarmde exemplaren, kunt u effectief koude start latenties voorkomen.  Vooraf verwarmde exemplaren is een functie van de Premium-abonnement en u moet ten minste één exemplaar wordt uitgevoerd en beschikbaar zijn op alle keren dat het abonnement actief is.

U kunt het aantal vooraf verwarmde exemplaren configureren in Azure portal door te selecteren **uitschalen** in de **platformfuncties** tabblad.

![Instellingen voor elastisch schalen](./media/functions-premium-plan/scale-out.png)

U kunt ook vooraf verwarmde exemplaren voor een app configureren met de Azure CLI

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Verbinding met het particuliere netwerk

Azure Functions geïmplementeerd naar een Premium-abonnement profiteert van [nieuwe VNet-integratie voor web-apps](../app-service/web-sites-integrate-with-vnet.md#new-vnet-integration).  Wanneer geconfigureerd, wordt uw app kan communiceren met resources in uw VNet of beveiligd via service-eindpunten.  IP-beperkingen zijn ook beschikbaar in de app om te beperken van inkomend verkeer.

Bij het toewijzen van een subnet aan uw functie-app in een Premium-abonnement, moet u een subnet met voldoende IP-adressen voor elke mogelijke instantie. Hoewel het maximum aantal exemplaren tijdens de Preview-versie variëren kan, moet een IP-Adresblok met ten minste 100 beschikbare adressen.

Zie voor meer informatie, [uw functie-app integreren met een VNet](functions-create-vnet.md).

### <a name="rapid-elastic-scale"></a>Snelle elastisch schalen

Aanvullende rekeninstanties worden automatisch toegevoegd voor uw app met behulp van dezelfde snelle vergroten/verkleinen logica als in het abonnement Consumption.  Zie voor meer informatie over hoe schaal werkt, [functie schaal en hosting](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="unbounded-run-duration"></a>Uitvoeringsduur unbounded

Azure Functions in een verbruiksabonnement is beperkt tot 10 minuten voor een enkel kan worden uitgevoerd.  In het Premium-abonnement, wordt de uitvoeringsduur standaard tot 30 minuten om te voorkomen dat overmatig uitvoeringen. U kunt echter [wijzigen van de configuratie van de host.json](./functions-host-json.md#functiontimeout) zodat deze niet-gebonden voor Premium-plan-apps.

## <a name="plan-and-sku-settings"></a>Instellingen voor planning en SKU

Wanneer u het abonnement maakt, u twee instellingen configureren: het minimum aantal exemplaren (of de grootte van de planning) en de maximale burst-limiet.  De minimale exemplaren voor een Premium-abonnement 1 en de maximale burst tijdens de Preview-versie is 20.  Minimale instanties zijn gereserveerd en wordt altijd uitgevoerd.

> [!IMPORTANT]
> In rekening gebracht voor elk exemplaar dat is toegewezen in het minimum aantal exemplaren, ongeacht als functies worden uitgevoerd of niet.

Als uw app nodig exemplaren dan de grootte van uw abonnement heeft, kan deze uitschalen totdat het aantal exemplaren van treffers in de maximale burst-limiet blijven.  U wordt gefactureerd voor exemplaren dan de grootte van uw abonnement alleen terwijl ze worden uitgevoerd en gehuurde aan u.  We doen een best-effort op uw App uit tot de gedefinieerde maximumlimiet, terwijl de minimale planexemplaren voor uw app worden gegarandeerd.

U kunt de grootte van de planning configureren en maximumwaarden in Azure portal door geselecteerd de **uitschalen** opties in het plan of een functie-app geïmplementeerd in het abonnement (onder **platformfuncties**).

U kunt ook de maximale burst-limiet van de Azure CLI verhogen:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

### <a name="available-instance-skus"></a>Beschikbare instantie SKU 's

Bij het maken van onze schalen van uw abonnement, kunt u kiezen tussen drie exemplaargrootten.  U wordt gefactureerd voor het totale aantal kernen en geheugen verbruikt per seconde.  Uw app kunt automatisch uitschalen naar meerdere exemplaren, indien nodig.  

|SKU|Kerngeheugens|Geheugen|Opslag|
|--|--|--|--|
|EP1|1|3,5 GB|250 GB|
|EP2|2|7GB|250 GB|
|EP3|4|14GB|250 GB|

## <a name="regions"></a>Regio's

Hieronder vindt u de momenteel ondersteunde regio's voor de openbare preview.

|Regio|
|--|
|Australië - oost|
|Australië Souteast|
|Canada - midden|
|India - centraal|
|US - centraal|
|Azië - oost|
|US - oost 2|
|Frankrijk - centraal|
|Japan - west|
|Korea - centraal|
|Europa - noord|
|US - zuid-centraal|
|India - zuid|
|Azië - zuidoost|
|Verenigd Koninkrijk West|
|Europa -west|
|India - west|
|US - west|

## <a name="known-issues"></a>Bekende problemen

U kunt de status van de bekende problemen van volgen de [openbare preview-versie van GitHub](https://github.com/Azure/Azure-Functions/wiki/Premium-plan-known-issues).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over Azure Functions-schaal en hosting-opties](functions-scale.md)

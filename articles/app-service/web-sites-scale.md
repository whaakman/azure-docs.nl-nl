---
title: Functies en -capaciteit - Azure App Service opschalen | Microsoft Docs
description: Leer hoe u een app in Azure App Service voor capaciteit en onderdelen toevoegen kunt schalen.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 98d3d1f6fc0f2f30196f360811808579dfbab312
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53727476"
---
# <a name="scale-up-an-app-in-azure"></a>Een app in Azure omhoog schalen

> [!NOTE]
> De nieuwe **PremiumV2** laag kunt u snellere CPU's, SSD-opslag, en de geheugen-kernverhouding van de bestaande Prijscategorieën verdubbeld. Met het voordeel van prestaties, kan u geld besparen door te voeren van uw apps op minder exemplaren. Om te schalen tot **PremiumV2** categorie [configureren PremiumV2-laag voor App Service](app-service-configure-premium-tier.md).
>

Dit artikel leest u hoe uw app schalen in Azure App Service. Er zijn twee werkstromen voor vergroten/verkleinen, scale-en scale-out, en in dit artikel wordt uitgelegd de schaal van de werkstroom.

* [Omhoog schalen](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Get-meer CPU, geheugen, schijfruimte en extra functies, zoals toegewezen virtuele machines (VM's), aangepaste domeinen en certificaten, faseringssleuven, automatisch schalen en meer. U schalen omhoog door de prijscategorie van de App Service-plan waartoe uw app behoort te wijzigen.
* [Uitschalen](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Verhoog het aantal VM-exemplaren waarop uw app wordt uitgevoerd.
  U kunt uitschalen naar maximaal 20 exemplaren, afhankelijk van uw prijscategorie. [App Service-omgevingen](environment/intro.md) in **geïsoleerd** laag verder verhoogt het aantal scale-out naar 100 instanties. Zie voor meer informatie over het uitbreiden [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md). Daar vindt u informatie over het gebruik van automatisch schalen, dat is het aantal exemplaren automatisch op basis van vooraf gedefinieerde regels en schema's schalen.

De schaalinstellingen worden alleen de seconden toe te passen en gelden voor alle apps in uw [App Service-plan](../app-service/overview-hosting-plans.md).
Ze dat niet u uw code te wijzigen of opnieuw implementeren van uw toepassing.

Zie voor meer informatie over de prijzen en functies van afzonderlijke App Service-plannen [prijsinformatie voor App Service](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Voordat u een App Service-plan uit schakelt de **gratis** laag, moet u eerst verwijderen de [bestedingslimieten](https://azure.microsoft.com/pricing/spending-limits/) aanwezig is voor uw Azure-abonnement. Als u wilt weergeven of wijzigen van opties voor uw Microsoft Azure App Service-abonnement, Zie [Microsoft Azure-abonnementen][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Uw prijscategorie opschalen
1. Open in uw browser de [Azure-portal][portal].
2. Klik in uw App Service-app-pagina op **alle instellingen**, en klik vervolgens op **omhoog schalen**.
   
    ![Ga voor het schalen van uw Azure-app.][ChooseWHP]
3. De laag, en klik vervolgens op **toepassen**.
   
    De **meldingen** tabblad wordt een groen flash **SUCCES** nadat de bewerking voltooid is.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Verwante resources schalen
Als uw app, is afhankelijk van andere services, zoals Azure SQL Database of Azure Storage, kunt u deze resources afzonderlijk opschalen. Deze resources niet worden beheerd door de App Service-plan.

1. In **Essentials**, klikt u op de **resourcegroep** koppeling.
   
    ![Verwante bronnen van uw Azure-app opschalen](./media/web-sites-scale/RGEssentialsLink.png)
2. In de **samenvatting** deel uitmaakt van de **resourcegroep** pagina, klikt u op een resource die u wilt schalen. De volgende schermafbeelding ziet u een SQL-Database-resource en een Azure Storage-resource.
   
    ![Navigeer naar de pagina voor een resourcegroep voor het schalen van uw Azure-app](./media/web-sites-scale/ResourceGroup.png)
3. Voor de resource van een SQL-Database, klikt u op **instellingen** > **prijscategorie** voor het schalen van de prijscategorie.
   
    ![De SQL-Database back-end voor uw Azure-app opschalen](./media/web-sites-scale/ScaleDatabase.png)
   
    U kunt ook inschakelen [geo-replicatie](../sql-database/sql-database-geo-replication-overview.md) voor uw exemplaar van SQL-Database.
   
    Voor een Azure Storage-resource, klikt u op **instellingen** > **configuratie** uw opties voor opslag kan worden uitgebreid.
   
    ![De Azure Storage-account die worden gebruikt door uw Azure-app opschalen](./media/web-sites-scale/ScaleStorage.png)

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Prijscategorieën vergelijken
Zie voor gedetailleerde informatie, zoals VM-grootten voor elke prijscategorie [prijsinformatie voor App Service](https://azure.microsoft.com/pricing/details/web-sites/).
Zie voor een tabel van Servicelimieten, quotums en beperkingen en ondersteunde functies in elke laag [limieten voor App Service](../azure-subscription-service-limits.md#app-service-limits).

<a name="Next Steps"></a>

## <a name="next-steps"></a>Volgende stappen
* Voor informatie over prijzen, ondersteuning en SLA, gaat u naar de volgende koppelingen:
  
    [Prijsinformatie voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Ondersteuningsabonnementen voor Microsoft Azure](https://azure.microsoft.com/support/plans/)
  
    [Serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/)
  
    [Prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Virtuele Machine en Cloud Service Sizes for Microsoft Azure][vmsizes]
  
* Zie voor meer informatie over Azure App Service aanbevolen procedures, met inbegrip van het bouwen van een schaalbare en flexibele architectuur, [aanbevolen procedures: Azure App Service WebApps](https://azure.microsoft.com/blog/best-practices-windows-azure-websites-waws/).
* Zie de volgende bronnen voor video's over het schalen van App Service-apps:
  
  * [Wanneer Azure Websites - met Stefan Schackow schalen](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Functie voor automatisch schalen van Azure Websites, CPU of geplande - met Stefan Schackow](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [Hoe Azure Websites schalen - met Stefan Schackow](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:https://azure.microsoft.com/pricing/details/app-service/
[SQLaccountsbilling]:https://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:https://account.windowsazure.com/subscriptions
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png

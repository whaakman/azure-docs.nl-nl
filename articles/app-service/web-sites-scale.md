---
title: Een app in Azure opschalen | Microsoft Docs
description: Informatie over het schalen van een app in Azure App Service-capaciteit en functies toevoegen.
services: app-service
documentationcenter: 
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
ms.openlocfilehash: 248b96cc97367ca2cb3fd82c9824d43dfee43c0a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="scale-up-an-app-in-azure"></a>Een app in Azure opschalen

> [!NOTE]
> De nieuwe **PremiumV2** laag kunt u snellere CPU's, SSD-opslag, en de verhouding tussen het geheugen-core-dubbelklik dan de bestaande prijzen lagen. Maximaal schalen **PremiumV2** categorie [PremiumV2 configureren-laag voor App Service](app-service-configure-premium-tier.md).
>

In dit artikel laat zien hoe uw app schalen in Azure App Service. Er zijn twee werkstromen voor vergroten/verkleinen, scale-en scale-out en in dit artikel wordt uitgelegd de schaal van een workflow.

* [Omhoog schalen](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): ophalen van meer CPU, geheugen, schijfruimte en extra functies, zoals toegewezen virtuele machines (VM's), aangepaste domeinen en certificaten, sleuven, automatisch schalen en meer. U opschalen door het wijzigen van de prijscategorie van de App Service-plan waartoe uw app behoort.
* [Uitschalen](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): verhoog het aantal VM-exemplaren die uw app uitvoeren.
  U kunt uitschalen naar maximaal 20 instanties, afhankelijk van uw prijscategorie. [App Service-omgevingen](environment/intro.md) in **geïsoleerd** laag verdere verhoogt uw aantal scale-out op 100 exemplaren. Zie voor meer informatie over het uitbreiden van [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md). Er informatie u over het gebruik van automatisch schalen, die is het aantal exemplaren automatisch op basis van vooraf gedefinieerde regels en schema's.

De scale-instellingen worden alleen seconden toe te passen en gelden voor alle apps in uw [App Service-abonnement](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
Ze hoeft u uw code wijzigen of uw toepassing opnieuw distribueren.

Zie voor meer informatie over de prijzen en -functies van afzonderlijke App Service-abonnementen [App Service-prijsinformatie](https://azure.microsoft.com/pricing/details/web-sites/).  

> [!NOTE]
> Voordat u overschakelt van een App Service-abonnement van de **vrije** laag, moet u eerst verwijderen de [bestedingslimieten](https://azure.microsoft.com/pricing/spending-limits/) voor uw Azure-abonnement. Als u wilt weergeven of wijzigen van opties voor uw Microsoft Azure App Service-abonnement, Zie [Microsoft Azure-abonnementen][azuresubscriptions].
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Uw prijscategorie opschalen
1. Open in uw browser de [Azure-portal][portal].
2. Klik in de pagina van uw App Service-app op **alle instellingen**, en klik vervolgens op **omhoog schalen**.
   
    ![Navigeer om te schalen van uw app in Azure.][ChooseWHP]
3. Kies uw laag en klik vervolgens op **Selecteer**.
   
    De **meldingen** tabblad knippert een groene **geslaagd** nadat de bewerking voltooid is.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>Verwante bronnen schalen
Als uw app, is afhankelijk van andere services, zoals Azure SQL Database- of Azure Storage, kunt u deze resources afzonderlijk opschalen. Deze resources niet worden beheerd door de App Service-abonnement.

1. In **Essentials**, klikt u op de **resourcegroep** koppeling.
   
    ![Verwante bronnen van uw Azure-app opschalen](./media/web-sites-scale/RGEssentialsLink.png)
2. In de **samenvatting** deel uit van de **resourcegroep** pagina, klikt u op een resource die u wilt schalen. De volgende schermafbeelding ziet u een resource van de SQL-Database en een Azure Storage-resource.
   
    ![Navigeer naar de pagina van de resource-group schalen van uw app in Azure](./media/web-sites-scale/ResourceGroup.png)
3. Voor een SQL-Database-resource, klikt u op **instellingen** > **prijscategorie** de prijscategorie schalen.
   
    ![De SQL-Database-back-end voor uw app in Azure opschalen](./media/web-sites-scale/ScaleDatabase.png)
   
    Kunt u ook inschakelen [geo-replicatie](../sql-database/sql-database-geo-replication-overview.md) voor uw exemplaar van SQL-Database.
   
    Voor een Azure Storage-resource, klikt u op **instellingen** > **configuratie** schalen van uw opties voor opslag.
   
    ![De Azure Storage-account die wordt gebruikt door uw app in Azure opschalen](./media/web-sites-scale/ScaleStorage.png)

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Prijscategorieën vergelijken
Zie voor gedetailleerde informatie, zoals VM-grootten voor elke prijscategorie [App Service-prijsinformatie](https://azure.microsoft.com/pricing/details/web-sites/).
Zie voor een tabel van de Servicelimieten, quota's en -beperkingen en ondersteunde functies in elke laag [App Service-beperkingen](../azure-subscription-service-limits.md#app-service-limits).

> [!NOTE]
> Als u aan de slag met Azure App Service wilt voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/) waar u direct een tijdelijke en eenvoudige web-app kunt maken in App Service. U hebt geen creditcard vereist en er bent nergens toe verplicht.
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a>Volgende stappen
* Voor informatie over prijzen, ondersteuning en SLA, gaat u naar de volgende koppelingen:
  
    [Prijsinformatie voor gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [Microsoft Azure-ondersteuningsplannen](https://azure.microsoft.com/support/plans/)
  
    [Serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/)
  
    [Prijsinformatie voor SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)
  
    [Virtual Machine and Cloud Service Sizes for Microsoft Azure][vmsizes]
  
* Zie voor meer informatie over Azure App Service aanbevolen procedures voor het bouwen van een schaalbare en flexibele architectuur, inclusief [Best Practices: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).
* Zie de volgende bronnen voor video's over het schalen van App Service-apps:
  
  * [Wanneer Azure Websites - met Stefan Schackow schalen](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [Automatische schaling van Azure Websites, CPU of geplande - met Stefan Schackow](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [Hoe Azure Websites schaal - met Stefan Schackow](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:/pricing/details/app-service/
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png

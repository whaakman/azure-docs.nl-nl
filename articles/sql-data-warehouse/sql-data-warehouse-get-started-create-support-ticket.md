---
title: Een ondersteuningsticket maken voor SQL Data Warehouse | Microsoft Docs
description: Maak een ondersteuningsticket in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 058ff1229acee5d03db7c0305c5565ae95a85758
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Een ondersteuningsticket maken voor SQL Data Warehouse
Als u problemen ondervindt met SQL Data Warehouse, kunt u een ondersteuningsticket maken zodat ons technisch team u kan helpen.

> [!NOTE] 
> Met ingang van 20-12-2016 is de statuscontrole van resources in Azure Portal niet nauwkeurig. We doen ons best om dit probleem zo snel mogelijk te verhelpen. 


## <a name="create-a-support-ticket"></a>Een ondersteuningsticket maken
1. Open [Azure Portal][Azure portal].
2. Klik in het beginscherm op de tegel **Help en ondersteuning**.
   
    ![Help en ondersteuning](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. Klik in de blade Help en ondersteuning op **Een ondersteuningsverzoek maken**.
   
    ![Nieuw ondersteuningsverzoek](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. Selecteer het **Soort aanvraag**.
   
    ![Soort aanvraag](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > Elke SQL-server (bijvoorbeeld myserver.database.windows.net) heeft standaard een **DTU-quotum** van 45.000. Dit quotum is gewoon een veiligheidsbeperking. U kunt uw quotum verhogen door een ondersteuningsticket te maken en *Quotum* als het aanvraagtype te selecteren. Voor de berekening van de behoeften van uw DTU vermenigvuldigt u 7,5 met het totale aantal benodigde [DWU][DWU]. Als u bijvoorbeeld twee DW6000’s op één SQL server wilt hosten, moet u een DTU-quotum van 90.000 aanvragen.  U kunt uw huidige DTU-verbruik zien in de blade voor de SQL-server in de portal. Zowel onderbroken als niet-onderbroken databases tellen mee voor het DTU-quotum. 
   > 
   > 
5. Selecteer het **abonnement** waaronder de database valt met het probleem dat u wilt melden.
   
    ![Abonnement](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. Selecteer **SQL Data Warehouse** als de resource.
   
    ![Resource](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. Selecteer uw [Azure-ondersteuningsplan][Azure support plan].
   
   * Ondersteuning voor het **beheren van facturering, quota en abonnementen** is op alle ondersteuningsniveaus beschikbaar.
   * **Probleemoplossing** wordt ondersteund op de ondersteuningsniveaus [Developer][Developer], [Standard][Standard], [Professional Direct][Professional Direct] en [Premier][Premier]. Het gaat hierbij om problemen die klanten ondervinden tijdens het gebruik van Azure waarbij er een redelijke verwachting is dat het probleem is veroorzaakt door Microsoft.
   * **Begeleiding van ontwikkelaars** en **adviesdiensten** zijn beschikbaar op de ondersteuningsniveaus [Professional Direct][Professional Direct] en [Premier][Premier]. 
     
     Als u een Premier-ondersteuningsplan hebt, kunt u ook problemen met betrekking tot SQL Data Warehouse rapporteren in de [Microsoft Premier Online-portal][Microsoft Premier online portal].  Zie [Azure-ondersteuningsplannen][Azure support plan] voor meer informatie over de verschillende ondersteuningsplannen, waaronder bereik, reactietijden, prijzen enzovoort.  Zie [Veelgestelde vragen over ondersteuning van Azure][Azure support FAQs] voor veelgestelde vragen over de ondersteuning van Azure.  
     
     ![Ondersteuningsplan](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. Selecteer het **Probleemtype** en de **Categorie**. In dit voorbeeld hebben we Hulpprogramma's gekozen als het probleemtype en Clienthulpprogramma's als de categorie. 
   
    ![Probleemtype categorie](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. Beschrijf het probleem en kies de impact die het probleem heeft op de bedrijfsactiviteiten.
   
    ![Beschrijving van het probleem](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. Uw **contactgegevens** voor dit ondersteuningsticket worden vooraf ingevuld. U kunt deze indien nodig bijwerken.
    
    ![Contactgegevens](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. Klik op **Maken** om het ondersteuningsverzoek in te dienen.

## <a name="monitor-a-support-ticket"></a>Een ondersteuningsticket bewaken
Nadat u de ondersteuningsaanvraag hebt ingediend, zal het ondersteuningsteam van Azure contact met u opnemen. Als u de aanvraagstatus en -details wilt controleren, klikt u op het dashboard op **Ondersteuningsaanvragen beheren**.

![Status controleren](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Meer informatie
Daarnaast kunt u verbinding maken met de SQL Data Warehouse-community op [Stack Overflow][Stack Overflow] of op het [MSDN-forum Azure SQL Data Warehouse][Azure SQL Data Warehouse MSDN forum].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/


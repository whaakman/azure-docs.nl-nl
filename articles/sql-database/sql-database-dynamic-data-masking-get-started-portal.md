---
title: 'Azure-portal: SQL-Database dynamische-gegevensmaskering | Microsoft Docs'
description: Hoe u aan de slag met SQL-Database dynamische gegevensmaskering in de Azure Portal
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: "2"
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: ronitr; ronmat
ms.openlocfilehash: 15184e14d4e1e23b56126bbf9f972c1619dcba80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>Aan de slag met SQL-Database dynamische-gegevensmaskering met de Azure Portal

Dit onderwerp leest u hoe u implementeert [dynamische gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) met de Azure-portal. U kunt ook implementeren dat maskering met behulp van dynamische gegevens [cmdlets van Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) of de [REST-API](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Dynamische gegevens maskeren voor uw database met de Azure Portal instellen
1. Starten van de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Navigeer naar de instellingenblade van de database met de gevoelige gegevens die u wilt maskeren.
3. Klik op de **dynamische-Gegevensmaskering** gestart tegel de **dynamische-Gegevensmaskering** configuratie blade.
   
   * U kunt ook Schuif omlaag naar de **Operations** sectie en klik op **dynamische-Gegevensmaskering**.
     
     ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. In de **dynamische-Gegevensmaskering** configuratie blade ziet u mogelijk enkele databasekolommen die voor de engine voor aanbevelingen voor maskering is gemarkeerd. Om de aanbevelingen hebt geaccepteerd, klikt u op **masker toevoegen** voor een of meer kolommen en een masker wordt gemaakt op basis van het standaardtype voor deze kolom. U kunt de maskeringsfunctie wijzigen door te klikken op de maskeringsregel en bewerken van het maskeren veldindeling naar een andere indeling van uw keuze. Klik op **opslaan** uw instellingen op te slaan.
   
    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. Toevoegen van een masker voor elke kolom in de database, aan de bovenkant van de **dynamische-Gegevensmaskering** configuratie blade Klik **masker toevoegen** openen de **maskeren regel toevoegen** configuratie Blade
   
    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Selecteer de **Schema**, **tabel** en **kolom** voor het definiëren van het veld dat is opgegeven die wordt gemaskeerd.
7. Kies een **maskeren veldindeling** uit de lijst met gevoelige gegevens maskeren categorieën.
   
    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Klik op **opslaan** in de blade van de regel voor het bijwerken van de reeks regels in het beleid voor dynamische gegevensmaskering maskeren maskeren gegevens.
9. Geef de SQL-gebruikers of AAD-identiteiten die moeten worden uitgesloten van maskering en hebben toegang tot de ontmaskerd gevoelige gegevens. Dit moet een door puntkomma's gescheiden lijst met gebruikers. Houd er rekening mee dat gebruikers met beheerdersrechten altijd toegang tot de oorspronkelijke gegevens toegankelijk is hebben.
   
    ![Navigatiedeelvenster](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > Als u wilt ervoor zorgen dat de toepassingslaag gevoelige gegevens voor de bevoegde toepassingsgebruikers kunt weergeven, voeg de SQL-gebruiker of AAD-identiteit de toepassing wordt gebruikt om te zoeken in de database. Het is raadzaam dat deze lijst een minimum aantal bevoegde gebruikers om te beperken van blootstelling van de gevoelige gegevens bevatten.
   > 
   > 
10. Klik op **opslaan** in de blade van de configuratie voor het opslaan van de nieuwe of bijgewerkte maskeren gegevens maskeren beleid.


## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van dynamische gegevensmaskering [dynamische gegevensmaskering](sql-database-dynamic-data-masking-get-started.md).
* U kunt ook implementeren dat maskering met behulp van dynamische gegevens [cmdlets van Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) of de [REST-API](https://msdn.microsoft.com/library/dn505719.aspx).
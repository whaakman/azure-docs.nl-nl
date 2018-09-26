---
title: 'Azure-portal: SQL Database dynamische gegevensmaskering | Microsoft Docs'
description: Aan de slag met SQL Database dynamische gegevensmaskering in Azure portal
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 1ec0634c89148cee59f399e437b92a7d2c6b283b
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165718"
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>Aan de slag met SQL Database dynamische gegevensmaskering met Azure portal

Dit artikel leest u hoe u voor het implementeren van [dynamische gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) met de Azure-portal. U kunt ook implementeren dynamische gegevens maskeren met [Azure SQL Database-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.sql/) of de [REST-API](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Dynamische gegevensmaskering voor uw database met behulp van de Azure-portal instellen
1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de instellingenpagina van de database met de gevoelige gegevens die u wilt maskeren.
3. Klik op de **Dynamic Data Masking** tegel die wordt gestart de **Dynamic Data Masking** configuratiepagina.
   
   * U kunt ook, schuif omlaag naar de **Operations** sectie en klikt u op **Dynamic Data Masking**.
     
     ![Navigatievenster](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. In de **Dynamic Data Masking** configuratiepagina, ziet u enkele kolommen die de recommendations-engine is gemarkeerd voor maskeren. Als u wilt de aanbevelingen accepteren, klikt u op **masker toevoegen** voor een of meer kolommen en een subnetmasker is gemaakt op basis van het standaardtype voor deze kolom. U kunt de maskeringsfunctie wijzigen door te klikken op de maskeringsregel en bewerken van het maskeren veldindeling naar een andere indeling van uw keuze. Klik op **opslaan** uw instellingen op te slaan.
   
    ![Navigatievenster](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. Om toe te voegen een masker voor elke kolom in de database, aan de bovenkant van de **Dynamic Data Masking** configuratiepagina, klikt u op **masker toevoegen** openen de **maskeren regel toevoegen** configuratiepagina .
   
    ![Navigatievenster](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Selecteer de **Schema**, **tabel** en **kolom** voor het definiëren van het veld dat is opgegeven voor maskeren.
7. Kies een **notatie veld maskeren** uit de lijst met gevoelige gegevens maskeren categorieën.
   
    ![Navigatievenster](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Klik op **opslaan** in de pagina van de regel voor het bijwerken van de set met regels in het beleid voor dynamische gegevensmaskering maskeren voor gegevensmaskering.
9. Geef de SQL-gebruikers of AAD-identiteiten die moeten worden uitgesloten van maskeren en hebben toegang tot de ontmaskerd gevoelige gegevens. Dit moet een puntkomma's gescheiden lijst van gebruikers. Gebruikers met administrator-bevoegdheden hebben altijd toegang tot de oorspronkelijke gegevens toegankelijk maakt.
   
    ![Navigatievenster](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > Als u wilt ervoor zorgen dat de toepassingslaag gevoelige gegevens voor gebruikers van de toepassing in beschermde modus kan weergeven, voeg de SQL-gebruiker of de AAD-identiteit is die de toepassing gebruikt om te zoeken in de database. Het is raadzaam dat deze lijst een minimum aantal gebruikers met bevoegdheden om te beperken van blootstelling van gevoelige gegevens bevatten.
   > 
   > 
10. Klik op **opslaan** maskeren in de gegevens maskeren configuratiepagina voor het opslaan van de nieuwe of bijgewerkte beleid.


## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van dynamische gegevensmaskering [dynamische gegevensmaskering](sql-database-dynamic-data-masking-get-started.md).
* U kunt ook implementeren dynamische gegevens maskeren met [Azure SQL Database-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.sql/) of de [REST-API](https://msdn.microsoft.com/library/dn505719.aspx).

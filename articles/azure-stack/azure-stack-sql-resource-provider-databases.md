---
title: Met behulp van databases die worden geleverd door de resourceprovider van SQL-Adapter op Azure Stack | Microsoft Docs
description: Over het maken en beheren van de SQL-databases die zijn ingericht met behulp van de resourceprovider van SQL-Adapter
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: 6f56cdccd8f4cd690eb149c8f84a562a580325dc
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56164342"
---
# <a name="create-sql-databases"></a>SQL-databases maken

U kunt maken en beheren van databases in de gebruikersportal selfservice. Een Azure Stack-gebruiker moet een abonnement met een aanbieding met de SQL database-service.

1. Aanmelden bij de [Azure Stack](azure-stack-poc.md) gebruikersportal.

2. Selecteer **+ nieuw** &gt; **gegevens en opslag** &gt; **SQL Server-Database** &gt; **toevoegen**.

3. Onder **Create Database**, voer de vereiste gegevens, zoals **databasenaam** en **Max Size in MB**.

   >[!NOTE]
   >Grootte van de database moet ten minste 64 MB, die u kunt verhogen nadat u de database hebt geïmplementeerd.

   Configureer de andere instellingen zoals vereist voor uw omgeving.

4. Onder **Create Database**, selecteer **SKU**. Onder **selecteert u een SKU**, selecteer de SKU voor uw database.

   ![Database maken](./media/azure-stack-sql-rp-deploy/newsqldb.png)

   >[!NOTE]
   >Als hosting-servers worden toegevoegd aan Azure Stack, krijgt deze een SKU. Databases worden gemaakt in de groep van het host-servers in een SKU.

5. Selecteer **aanmelding**.
6. Onder **selecteert u een aanmelding**, kiest u een bestaande aanmelding of selecteer **en maak een nieuwe aanmelding**.
7. Onder **nieuwe aanmelding**, voer een naam in voor **databaseaanmelding** en een **wachtwoord**.

   >[!NOTE]
   >Deze instellingen zijn de SQL-verificatie-referentie op die voor uw toegang tot alleen deze database wordt gemaakt. De gebruikersnaam moet globaal uniek zijn. U kunt instellingen voor aanmelding voor andere databases die gebruikmaken van dezelfde SKU opnieuw gebruiken.

   ![Maak een nieuwe databaseaanmelding](./media/azure-stack-sql-rp-deploy/create-new-login.png)

8. Selecteer **OK** voor het voltooien van de implementatie van de database.

Onder **Essentials**, die wordt weergegeven nadat de database is geïmplementeerd, noteer de **Connection string**. U kunt deze tekenreeks gebruiken in alle toepassingen die toegang nodig heeft tot de SQL Server-database.

![De verbindingsreeks ophalen](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="sql-always-on-databases"></a>SQL Always On-databases

Standaard worden altijd op databases afgehandeld anders dan in een omgeving met zelfstandige servers. Zie voor meer informatie, [Maak kennis met SQL Server Always On-beschikbaarheidsgroepen op Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview).

### <a name="verify-sql-always-on-databases"></a>Controleer of SQL Always On-databases

De volgende schermafbeelding ziet u hoe u SQL Server Management Studio kunt gebruiken om te kijken naar de status van de database in SQL Always On.

![Status van de AlwaysOn-database](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)

AlwaysOn-databases moeten weergeven als gesynchroniseerd en beschikbaar zijn op alle SQL-exemplaren en weergegeven in Availability Groups. In de vorige schermafbeelding is het database-voorbeeld newdb1 en de status ervan **newdb1 (gesynchroniseerd)**.

### <a name="delete-an-alwayson-database"></a>Een AlwaysOn-database verwijderen

Wanneer u een SQL AlwaysOn-database uit de resourceprovider verwijderen, verwijderd SQL database van de primaire replica en uit de beschikbaarheidsgroep.

SQL wordt vervolgens de database in de herstelstatus wordt geplaatst op de andere replica's en de database niet verwijderen, tenzij geactiveerd. Als de database is niet verwijderd, gaat u de secundaire replica's in een status niet kan worden gesynchroniseerd.

## <a name="next-steps"></a>Volgende stappen

[De resourceprovider van SQL Server beheren](azure-stack-sql-resource-provider-maintain.md)

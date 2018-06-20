---
title: Met behulp van databases die worden geleverd door de bronprovider van SQL-Adapter op Azure-Stack | Microsoft Docs
description: Het maken en beheren van de SQL-databases die zijn ingericht met behulp van de resourceprovider voor SQL-Adapter
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
ms.date: 06/18/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: a82db16f2012672f6e2669f2fd8198b177f501f3
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264179"
---
# <a name="create-sql-databases"></a>SQL-databases maken

U kunt maken en beheren van selfservice-databases in de gebruikersportal. Een Azure-Stack-gebruiker moet een abonnement met een aanbieding met de SQL database-service.

1. Aanmelden bij de [Azure Stack](azure-stack-poc.md) gebruikersportal.

2. Selecteer **+ nieuw** &gt; **gegevens en opslag '** &gt; **SQL Server-Database** &gt; **toevoegen**.

3. Onder **Create Database**, voer de vereiste informatie, zoals **databasenaam** en **Max Size in MB**.

   >[!NOTE]
   >De databasegrootte moet ten minste 64 MB, die u kunt verhogen nadat u de database hebt geïmplementeerd.

   Configureer de andere instellingen zoals vereist voor uw omgeving.

4. Onder **Create Database**, selecteer **SKU**. Onder **selecteert u een SKU**, selecteer de SKU voor uw database.

   ![Database maken](./media/azure-stack-sql-rp-deploy/newsqldb.png)

   >[!NOTE]
   >Als u hosting-servers worden toegevoegd aan Azure-Stack, waaraan ze een SKU zijn toegewezen. Databases zijn gemaakt in de groep servers in een SKU hosten.

5. Selecteer **aanmelding**.
6. Onder **selecteert u een aanmelding**, kies een bestaande aanmelding of selecteer **+ Maak een nieuwe aanmelding**.
7. Onder **New Login**, voer een naam voor **databaseaanmelding** en een **wachtwoord**.

   >[!NOTE]
   >Deze instellingen zijn voor de SQL-verificatie-referentie die voor uw toegang tot alleen deze database wordt gemaakt. Aanmeldingsnaam voor de gebruiker moet wereldwijd uniek zijn. U kunt instellingen voor aanmelding voor andere databases die gebruikmaken van dezelfde SKU hergebruiken.

   ![Maak een nieuwe databaseaanmelding](./media/azure-stack-sql-rp-deploy/create-new-login.png)

8. Selecteer **OK** om te implementeren van de database.

Onder **Essentials**, die wordt weergegeven nadat de database is geïmplementeerd, dient u de **verbindingsreeks**. U kunt deze tekenreeks gebruiken in elke toepassing die u moet toegang tot de SQL Server-database.

![De verbindingsreeks ophalen](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="sql-always-on-databases"></a>SQL Always On databases

Standaard zijn altijd aan databases anders afgehandeld dan in een omgeving met zelfstandige servers. Zie voor meer informatie [introductie van SQL Server altijd op beschikbaarheidsgroepen op Azure virtuele machines](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview).

### <a name="verify-sql-always-on-databases"></a>Controleer of SQL Always On databases

De volgende schermafbeelding ziet u hoe u SQL Server Management Studio kunt gebruiken om te kijken naar de databasestatus van de in SQL Always On.

![Status van AlwaysOn-database](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)

AlwaysOn-databases moeten weergeven als gesynchroniseerd en beschikbaar zijn op alle SQL-exemplaren en weergegeven in Availability Groups. In de vorige schermafbeelding is in het voorbeeld database newdb1 en de status is **newdb1 (gesynchroniseerd)**.

### <a name="delete-an-alwayson-database"></a>Verwijderen van een database met AlwaysOn

Als u een SQL AlwaysOn-database van de resourceprovider verwijdert, verwijderd SQL database van de primaire replica en uit de beschikbaarheidsgroep.

SQL vervolgens plaatst de database in de herstelstatus op de andere replica's en de database niet verwijderen tenzij geactiveerd. Als de database is niet verwijderd, gaat u de secundaire replica's in een status niet kan worden gesynchroniseerd.

## <a name="next-steps"></a>Volgende stappen

[De SQL Server-resourceprovider onderhouden](azure-stack-sql-resource-provider-maintain.md)

---
title: Met behulp van databases die worden geleverd door de SQL-Adapter RP op Azure-Stack | Microsoft Docs
description: Het maken en beheren van de SQL-databases die zijn ingericht met behulp van de Resource Provider voor SQL-Adapter
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: 0cc08c37e879b00f8cd9a4046a4c81c55dab167c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-sql-databases"></a>SQL-databases maken

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Selfservice-databases zijn opgenomen in de portal van de gebruikerservaring. Een gebruiker moet een abonnement dat is een aanbieding met de database-service.

1. Aanmelden bij de [Azure Stack](azure-stack-poc.md) gebruikersportal (serviceadministrators kunt ook het beheerportal gebruiken).

2. Klik op **+ nieuw** &gt; **gegevens en opslag '** &gt; **SQL Server-Database (preview)** &gt; **toevoegen**.

3. Vul in het formulier met informatie over de database, met inbegrip van een **databasenaam**, **maximumgrootte**, en de andere parameters zo nodig wijzigen. U wordt gevraagd om door te gaan een SKU voor uw database. Als hosting-servers worden toegevoegd, waaraan ze een SKU zijn toegewezen. Databases zijn gemaakt in die toepassingen hosten van de servers die gezamenlijk de SKU.

  ![Nieuwe database](./media/azure-stack-sql-rp-deploy/newsqldb.png)

  >[!NOTE]
  > Grootte van de database moet ten minste 64 MB. Deze kan worden verhoogd met instellingen.

4. Vul de aanmeldingsinstellingen: **databaseaanmelding**, en **wachtwoord**. Deze instellingen zijn voor de SQL-verificatie-referentie die voor uw toegang tot alleen deze database wordt gemaakt. Aanmeldingsnaam voor de gebruiker moet wereldwijd uniek zijn. Maak een nieuwe aanmelding instelling of een bestaande set selecteren. U kunt instellingen voor aanmelding voor andere databases met dezelfde SKU hergebruiken.

    ![Maak een nieuwe databaseaanmelding](./media/azure-stack-sql-rp-deploy/create-new-login.png)


5. Het formulier verzendt en wachten op de implementatie te voltooien.

    In de resulterende blade ziet u het veld "Connection string". U kunt deze tekenreeks in een toepassing waarvoor toegang tot SQL Server (bijvoorbeeld een web-app) gebruiken in uw Azure-Stack.

    ![De verbindingsreeks ophalen](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="delete-sql-databases"></a>SQL-databases verwijderen
Vanuit de portal

>[!NOTE]
>
>Wanneer een SQL AlwaysOn-database wordt verwijderd uit de RP, uit de primaire en AlwaysOn-beschikbaarheidsgroep is verwijderd maar door ontwerp SQL AG plaatst de database in herstelstatus in elke replica en wordt de database niet verwijderen tenzij geactiveerd. Als een database wordt niet verwijderd, wordt de secundaire replica's gaat naar de status niet kan worden gesynchroniseerd. Een nieuwe database opnieuw wordt toegevoegd aan de AG met dezelfde via RP nog steeds werkt. Zie ![verwijderen van een secundaire database](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server)

## <a name="manage-database-credentials"></a>Databasereferenties beheren
U kunt bijwerken databasereferenties (aanmeldingsinstellingen).

## <a name="verify-sql-alwayson-databases"></a>Controleer of de SQL AlwaysOn-databases
AlwaysOn-databases moeten worden weergegeven als gesynchroniseerd en beschikbaar zijn op alle exemplaren en in de beschikbaarheidsgroep. De database na een failover naadloos verbinding moet maken. U kunt SQL Server Management Studio gebruiken om te controleren dat een database wordt gesynchroniseerd:

![Controleer of AlwaysOn](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)

---
title: Persoonlijke gegevens beheren in Microsoft Azure | Microsoft Docs
description: richtlijnen voor het corrigeren, bijwerken, verwijderen en exporteren van persoonlijke gegevens in Azure Active Directory en Azure SQL Database
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: barclayn
ms.openlocfilehash: 97e8d23d90821489575b3ac6e8f1e810b7ce25a9
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="manage-personal-data-in-microsoft-azure"></a>Persoonlijke gegevens beheren in Microsoft Azure

In dit artikel biedt richtlijnen voor het corrigeren, bijwerken, verwijderen en exporteren van persoonlijke gegevens in Azure Active Directory en Azure SQL Database.

## <a name="scenario"></a>Scenario

Een bedrijf Dublin gebaseerde biedt one-stop winkelen voor hoge einde bestemming bruiloften in Ierland en over de hele wereld voor zowel een lokale en internationale klanten. Ze hebben kantoren, klanten, werknemers en leveranciers over de hele wereld om de plaatsen die ze bieden volledig te verwerken.

Tussen veel andere items het bedrijf houdt van geen r.s.v.p. 's die voedselallergieën en via de voeding voorkeuren bevatten. Bruiloft gasten kunnen worden geregistreerd voor verschillende activiteiten zoals horseback fiets, surfen, is aangewezen ritjes, enz., en zelfs communiceren met elkaar op een centrale webpagina tijdens de maanden voorafgaand aan de gebeurtenis. Het bedrijf verzamelt persoonlijke gegevens van werknemers, leveranciers, klanten en bruiloft gasten. Vanwege de internationale aard van het bedrijf moet het bedrijf met meerdere niveaus van regelgeving voldoen.

## <a name="problem-statement"></a>Probleemformulering

- Beheerders van gegevens moet kunnen juiste onnauwkeurig persoonlijke informatie en update onvolledig of veranderende persoonlijke gegevens.

- Beheerders van gegevens moet kunnen persoonlijke gegevens op verzoek van een onderwerp gegevens verwijderen.

- Beheerders van gegevens moet gegevens exporteren en opgeven in een onderwerp van de gegevens in een algemene, gestructureerde indeling op zijn of haar verzoek.

## <a name="company-goals"></a>Bedrijfsdoelstellingen

- Onjuiste of onvolledige klant, bruiloft Gast werknemer en persoonlijke gegevens van de leverancier moeten worden gecorrigeerd of bijgewerkt in Azure Active Directory en Azure SQL Database.

- Persoonlijke gegevens, moet op verzoek van een onderwerp gegevens in Azure Active Directory en Azure SQL Database worden verwijderd.

- Persoonlijke gegevens moet in een algemene, gestructureerde indeling op verzoek van een onderwerp gegevens worden geëxporteerd.

## <a name="solutions"></a>Oplossingen

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a>Azure Active Directory: onjuiste of onvolledige persoonlijke gegevens herstellen/corrigeren en persoonlijke gegevens/gebruikersprofielen wissen en verwijderen

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) van Microsoft cloud-gebaseerde, multitenant directory en identity management-service is.
U kunt corrigeren, bijwerken of verwijderen van klanten en werknemers gebruikersprofielen en gebruikersgegevens werk die persoonlijke gegevens bevatten, zoals naam, werk titel, adres of telefoonnummer, een gebruiker in uw [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) omgeving met behulp van de [Azure-portal](https://portal.azure.com/).

U moet zich aanmelden met een account met globale beheerdersrechten voor de map.

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a>Hoe corrigeren of bijwerken van gebruikersprofiel en werk ik gegevens in Azure Active Directory?

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.

2. Selecteer **meer services**, voer **gebruikers en groepen** in het tekstvak in en selecteer vervolgens **Enter**.

    ![Media/image1.png](media/manage-personal-data-azure/image001.png)

3. Op de **gebruikers en groepen** blade Selecteer **gebruikers**.

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. Op de **gebruikers en groepen - gebruikers** blade, selecteert u een gebruiker in de lijst en selecteer vervolgens het volgende op de blade voor de geselecteerde gebruiker **profiel** om weer te geven van de gebruikersgegevens voor het profiel dat moet worden gecorrigeerd of bijgewerkt.

    ![Media/image3.png](media/manage-personal-data-azure/image005.png)

5. Corrigeer of werkt u de gegevens en selecteer vervolgens in de opdrachtbalk **opslaan.**

6.  Selecteer op de blade voor de geselecteerde gebruiker **Info werken** om weer te geven van gebruikersgegevens werk die moet worden gecorrigeerd of bijgewerkt.

    ![Media/image4.png](media/manage-personal-data-azure/image007.png)

7. Corrigeer of bijwerken van de gegevens voor het werk van gebruiker en selecteer vervolgens in de opdrachtbalk **opslaan.**

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a>Hoe verwijder ik een gebruikersprofiel in Azure Active Directory?

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.

2. Selecteer **meer services**, voer **gebruikers en groepen** in het tekstvak in en selecteer vervolgens **Enter**.

    ![](media/manage-personal-data-azure/image001.png)

3. Op de **gebruikers en groepen** blade Selecteer **gebruikers**.

    ![Media/image2.png](media/manage-personal-data-azure/image003.png)

4. Selecteer op de blade **Gebruikers en groepen - Gebruikers** een gebruiker in de lijst.

    ![Media/image3.png](media/manage-personal-data-azure/image007.png)

5. Selecteer op de blade voor de geselecteerde gebruiker **overzicht**, en selecteer vervolgens in de opdrachtbalk **verwijderen**.

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a>SQL Database: herstellen/Corrigeer onjuiste of onvolledige persoonsgegevens; wissen en verwijderen van persoonsgegevens; exporteren van persoonlijke gegevens 

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) is een cloud-database die kan softwareontwikkelaars maken en onderhouden van toepassingen.

Persoonlijke gegevens kunnen worden bijgewerkt [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) met behulp van standaard SQL-query's, en kan ook worden verwijderd. Bovendien kunnen persoonlijke gegevens worden geëxporteerd vanuit SQL-Database met een aantal methoden, met inbegrip van de Azure SQL-Server importeren en exporteren van de wizard en in verschillende indelingen, waaronder een Bacpac-bestand.

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a>Hoe ik corrigeren, bijwerken of persoonlijke gegevens in SQL-Database wissen?

Als u wilt weten hoe u op te lossen of persoonlijke gegevens in SQL-Database bijwerken, gaat u naar de [Update (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [tekst bijwerken](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [bijwerken met algemene tabelexpressie](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), of [Update schrijven tekst](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) documentatie.

Als u wilt weten hoe u om persoonlijke gegevens in SQL-Database te verwijderen, gaat u naar de [verwijderen (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) documentatie.

#### <a name="how-do-i-export-personal-data-to-a-bacpac-file-in-sql-database"></a>Hoe exporteer persoonlijke gegevens naar een Bacpac-bestand in SQL-Database?

Een Bacpac-bestand bevat de gegevens van de SQL-Database en de metagegevens en een zip-bestand met een Bacpac-extensie is. U kunt dit doen met behulp van de [Azure-portal](https://portal.azure.com/), het opdrachtregelprogramma SQLPackage, SQL Server Management Studio (SSMS) of PowerShell.

Voor informatie over het exporteren van gegevens naar een Bacpac-bestand, gaat u naar de [een Azure SQL database naar een Bacpac-bestand exporteren](https://docs.microsoft.com/azure/sql-database/sql-database-export) pagina met gedetailleerde instructies voor elke methode die hierboven worden genoemd.

Hoe exporteer persoonlijke gegevens uit SQL-Database met de SQL Server Wizard importeren en exporteren?

Deze wizard kunt u gegevens kopiëren van een bron naar doel. Voor een inleiding tot de wizard, met inbegrip van het ophalen, machtigingen informatie en hoe u hulp bij het hulpprogramma, gaat u naar de [importeren en exporteren van gegevens met de SQL Server Wizard importeren en exporteren](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) webpagina.

Voor een overzicht van stappen voor de wizard, gaat u naar de [stappen in de SQL Server Wizard importeren en exporteren](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) webpagina.

## <a name="next-steps"></a>Volgende stappen

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)


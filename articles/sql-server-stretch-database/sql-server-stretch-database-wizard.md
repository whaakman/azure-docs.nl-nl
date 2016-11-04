---
title: Aan de slag door de Enable Database voor Stretch-wizard uit te voeren| Microsoft Docs
description: Ontdek hoe u een database kunt configureren voor Stretch Database door de Enable Database  voor Stretch-wizard uit te voeren.
services: sql-server-stretch-database
documentationcenter: ''
author: douglaslMS
manager: ''
editor: ''

ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/05/2016
ms.author: douglasl

---
# Ga aan de slag door de Enable Database voor Stretch-wizard uit te voeren
Voer de Enable Database  voor Stretch-wizard uit om een database te configureren voor Stretch Database.  In dit onderwerp wordt uitgelegd welke gegevens u moet invoeren en welke keuzes u in de wizard moet maken.

Zie [Stretch Database](sql-server-stretch-database-overview.md) voor meer informatie over Stretch Database.

> [!NOTE]
> Later, wanneer u Stretch Database uitschakelt, houd er dan rekening mee dat het uitschakelen van Stretch Database voor een tabel of voor een database er niet voor zorgt dat het externe object wordt verwijderd. Als u de externe tabel of de externe database wilt verwijderen, moet u deze verwijderen met behulp van de Azure-beheerportal. Voor de externe objecten blijven Azure-kosten in rekening worden gebracht totdat u deze handmatig verwijdert. 
> 
> 

## De wizard starten
1. In SQL Server Management Studio, in Object Explorer, selecteert u de database waarvoor u Stretch wilt inschakelen.
2. Klik hier met de rechtermuisknop\-op en selecteer **Taken**. Selecteer vervolgens **Stretch** en daarna  **Inschakelen** om de wizard te starten.

## <a name="Intro"></a>Inleiding
Controleer het doel van de wizard en de vereisten.

De belangrijke vereisten zijn:

* U moet een beheerder zijn om database-instellingen te wijzigen.
* U moet beschikken over een Microsoft Azure-abonnement.
* De SQL Server moet kunnen communiceren met de externe Azure-server.

![Introductiepagina van de Stretch Database-wizard][StretchWizardImage1]

## <a name="Tables"></a>Selecteer tabellen
Selecteer de tabellen die u wilt inschakelen voor Stretch.

Tabellen met een groot aantal rijen worden boven aan de gesorteerde lijst weergegeven. Voordat de Wizard de lijst met tabellen weergeeft, analyseert u deze om te zoeken naar gegevenstypen die momenteel niet worden ondersteund door Stretch Database.

![Selecteer de tabellenpagina van de Stretch Database-wizard][StretchWizardImage2]

| Kolom | Beschrijving |
| --- | --- |
| (geen titel) |Vink het selectievakje in deze kolom aan om de geselecteerde tabel voor Stretch in te schakelen. |
| **Naam** |De naam van de kolom in de tabel. |
| (geen titel) |Een symbool in deze kolom kan mogelijk staan voor een waarschuwing die niet\' voorkomt dat u de geselecteerde tabel kunt inschakelen voor Stretch. Het kan ook staan voor een probleem met een blokkering, dat verhindert dat u de geselecteerde tabel kunt inschakelen voor Stretch \-, bijvoorbeeld omdat de tabel een niet-ondersteund gegevenstype gebruikt. Houd de cursor boven  het symbool om meer informatie in de knopinfo weer te geven. Zie voor meer informatie [Beperkingen voor Stretch Database](sql-server-stretch-database-limitations.md). |
| **Stretched** |Geeft aan of de tabel al is ingeschakeld voor Stretch. |
| **Migreren** |U kunt een hele tabel migreren (**Gehele tabel**) of u kunt een filter op een bestaande kolom opgeven in de tabel. Als u een andere filterfunctie wilt gebruiken om rijen te selecteren die u wilt migreren, voert u de instructie ALTER TABLE uit om de filterfunctie te specificeren nadat u de wizard hebt afgesloten. Zie voor meer informatie over de filterfunctie [Rijen selecteren voor migratie met behulp van een filterfunctie](sql-server-stretch-database-predicate-function.md). Zie [Stretch Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md) of [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx) voor meer informatie over het toepassen van de functie. |
| **Rijen** |Aantal rijen in de tabel. |
| **Grootte (KB)** |Grootte van de tabel in KB. |

## <a name="Filter"></a>Geef optioneel een rijfilter op
Als u een filterfunctie wilt opgeven om de rijen die u wilt migreren te selecteren, doe dan het volgende op de pagina **Tabellen selecteren**.

1. In de lijst **Selecteer de tabellen die u wilt uitrekken**, klikt u op **Gehele tabel** in de rij voor de tabel. Het dialoogvenster **Selecteer rijen om uit te rekken** wordt geopend.
   
   ![Een filterfunctie definiëren][StretchWizardImage2a]
2. In het dialoogvenster **Selecteer rijen om uit te rekken** selecteert u **Rijen kiezen**.
3. In het **naamveld** vult u een naam in voor de filterfunctie.
4. Voor de **waar**-clausule kiest u een kolom uit de tabel, een operator en een waarde.
5. Klik op **Controleren** om de functie te testen. Als de functie resultaten uit de tabel geeft, dat wil zeggen, als er te migreren rijen zijn die voldoen aan de voorwaarde, geeft de test **Geslaagd** aan.
   
   > [!NOTE]
   > Het tekstvak waarin de filterquery wordt weergegeven is alleen-lezen. U kunt de query in het tekstvak niet bewerken.
   > 
   > 
6. Klik op Gereed om terug te keren naar de pagina **Tabellen selecteren**.

De filterfunctie wordt pas in SQL Server gemaakt wanneer u klaar bent met de wizard. Tot die tijd kunt u terugkeren naar de pagina **Tabellen selecteren** om de filterfunctie te wijzigen of deze een andere naam te geven.

![Selecteer de pagina Tabellen nadat u een filterfunctie hebt gedefinieerd][StretchWizardImage2b]

Als u een ander type filterfunctie wilt gebruiken om de rijen die u wilt migreren te selecteren, voert u een van de volgende acties uit.  

* Sluit de wizard af en voer de instructie ALTER TABLE uit om Stretch in te schakelen voor de tabel en een filterfunctie op te geven. Zie voor meer informatie [Stretch Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md).  
* Voer de instructie ALTER TABLE uit om een filterfunctie op te geven nadat u de wizard hebt afgesloten. Zie voor de vereiste stappen [Een filterfunctie toevoegen na het uitvoeren van de wizard](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="Configure"></a>Azure-implementatie configureren
1. Inloggen bij Microsoft Azure met een Microsoft-account.
   
   ![Inloggen bij de Azure - Stretch Database-wizard][StretchWizardImage3]
2. Selecteer het bestaande Azure-abonnement dat moet worden gebruikt voor de Stretch Database.
3. Selecteer een Azure-regio.
   
   * Als u een nieuwe server maakt, wordt de server gemaakt in deze regio.  
   * Als u bestaande servers in de geselecteerde regio hebt, geeft de wizard deze weer als u **Bestaande server** kiest.
   
   Om latentie te beperken, kiest u de Azure-regio waarin uw SQL Server zich bevindt. Zie [Azure-gebieden](https://azure.microsoft.com/regions/) voor meer informatie over de regio's.
4. Geef op of u een bestaande server wilt gebruiken of een nieuwe Azure-server wilt maken.
   
   Als de Active Directory op uw SQL Server is bij Azure Active Directory is gefedereerd, kunt u eventueel een federatieve serviceaccount voor SQL Server gebruiken om te communiceren met de externe Azure-server. Zie [ALTER DATABASE SET Options (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx) voor meer informatie over de vereisten voor deze optie.
   
   * **Nieuwe server maken**
     
     1. Maak een gebruikersnaam en wachtwoord voor de serverbeheerder.
     2. Gebruik eventueel een federatieve serviceaccount voor SQL Server om te communiceren met de externe Azure-server.
     
     ![Nieuwe Azure-server maken - Stretch Database-wizard][StretchWizardImage4]
   * **Bestaande server**
     
     1. Selecteer de bestaande Azure-server.
     2. Selecteer de verificatiemethode.
        
        * Als u **SQL Server-verificatie** selecteert, geeft u de gebruikersnaam en het wachtwoord van de beheerder op.
        * Selecteer **Active Directory Integrated Authentication** om een federatieve serviceaccount voor SQL Server te gebruiken om te communiceren met de externe Azure-server. Als de geselecteerde server niet is geïntegreerd met Azure Active Directory, wordt deze optie niet weergegeven.
     
     ![Bestaande Azure-server selecteren - Stretch Database-wizard][StretchWizardImage5]

## <a name="Credentials"></a>Beveiligde referenties
U hebt een databasehoofdsleutel nodig om de referenties te beschermen die de Stretch Database gebruikt om verbinding te maken met de externe database.  

Als er al een databasehoofdsleutel bestaat, voert u hiervoor het wachtwoord in.  

![Beveilig de referentiepagina van de Stretch Database-wizard][StretchWizardImage6b]

Als de database geen bestaande hoofdsleutel heeft, voert u een sterk wachtwoord in om een databasehoofdsleutel te maken.  

![Beveilig de referentiepagina van de Stretch Database-wizard][StretchWizardImage6]

Zie [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) en [Create a Database Master Key](https://msdn.microsoft.com/library/aa337551.aspx) voor meer informatie over de databasehoofdsleutel. Zie [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx) voor meer informatie over de referentie die de wizard maakt.

## <a name="Network"></a>Selecteer IP-adres
Gebruik het subnet-IP-adresbereik (aanbevolen) of het openbare IP-adres van uw SQL Server om een firewallregel op Azure te creëren waardoor SQL Server kan communiceren met de externe Azure-server.

Het IP-adressen/de IP-adressen die u op deze pagina opgeeft, laten de Azure-server weten dat deze binnenkomende gegevens, query's en beheerbewerkingen gestart door SQL Server moet doorlaten via de Azure-firewall. De wizard brengt geen wijzigingen aan in de firewall-instellingen op de SQL Server.

![Selecteer de IP-adrespagina van de Stretch Database-wizard][StretchWizardImage7]

## <a name="Summary"></a>Samenvatting
Bekijk de waarden die u hebt ingevoerd, de opties die u hebt geselecteerd in de wizard en de geschatte kosten in Azure. Selecteer vervolgens **Voltooien** om Stretch in te schakelen.

![Overzichtspagina van de Stretch Database-wizard][StretchWizardImage8]

## <a name="Results"></a>Resultaten
Bekijk de resultaten.

Zie [Gegevensmigratie bewaken en problemen oplossen (Stretch Database)](sql-server-stretch-database-monitor.md) om de status van de gegevensmigratie te bewaken.

![Pagina met resultaten van de Stretch Database-wizard][StretchWizardImage9]

## <a name="KnownIssues"></a>Problemen met de wizard oplossen
**De Stretch Database-wizard is mislukt.**
Als Stretch Database nog niet is ingeschakeld op serverniveau en u voert de wizard uit zonder de systeembeheerdersmachtigingen om deze in te schakelen, mislukt de wizard. Vraag de systeembeheerder om Stretch Database in te schakelen op het lokale serverexemplaar en voer de wizard opnieuw uit. Zie [Vereiste: toestemming om Stretch Database in te schakelen op de server](sql-server-stretch-database-enable-database.md#EnableTSQLServer) voor meer informatie.

## Volgende stappen
Aanvullende tabellen voor Stretch Database inschakelen. Controleer gegevensmigratie en beheer\-databases en tabellen waarvoor Stretch is ingeschakeld.

* [Stretch  Database  inschakelen voor een tabel](sql-server-stretch-database-enable-table.md) om aanvullende tabellen in te schakelen.
* [Gegevensmigratie bewaken en problemen oplossen](sql-server-stretch-database-monitor.md) voor de status van de gegevensmigratie.
* [Stretch Database pauzeren en  hervatten](sql-server-stretch-database-pause.md)
* [Stretch Database beheren en problemen oplossen](sql-server-stretch-database-manage.md)
* [Back-up maken van Stretch-databases](sql-server-stretch-database-backup.md)

## Zie ook
[Stretch Database inschakelen voor een database](sql-server-stretch-database-enable-database.md)

[Stretch Database inschakelen voor een tabel](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png



<!--HONumber=Sep16_HO3-->



---
title: Migratie en publiceren van webtoepassingen met een Azure-Cloud-Service vanuit Visual Studio | Microsoft Docs
description: Informatie over het migreren en publiceren van uw webtoepassing in een Azure-cloud-service met behulp van Visual Studio.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d5de4f5a7357cf5adde7773867356d47ad447bab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>How to: migreren en publiceren van webtoepassingen met een Azure-Cloud-Service vanuit Visual Studio
Als u wilt profiteren van de hosting-services en schaalbaarheid van Azure, is het raadzaam om te migreren en publiceren van uw webtoepassing in een Azure-cloud-service. U kunt een webtoepassing in Azure uitvoeren met minimale wijzigingen aan uw bestaande toepassing.

> [!NOTE]
> Dit onderwerp gaat over het implementeren van cloud-services, niet naar websites. Zie voor meer informatie over het implementeren van websites [een web-app in Azure App Service implementeren](app-service/app-service-deploy-local-git.md).
>
>

Zie de sectie voor een lijst van specifieke sjablonen die worden ondersteund voor zowel Visual C# en Visual Basic, **projectsjablonen ondersteund** verderop in dit onderwerp.

U moet eerst uw webtoepassing voor Azure vanuit Visual Studio inschakelen. De volgende afbeelding toont de belangrijkste stappen voor het publiceren van uw webtoepassing door een Azure-project moet worden gebruikt voor implementatie toe te voegen. Dit proces wordt een Azure-project met de vereiste Webrol toegevoegd aan uw oplossing. Op basis van het type webproject die u hebt, worden de Projecteigenschappen voor assembly's ook bijgewerkt als het servicepakket extra assembly's voor implementatie vereist.

![Publiceren van webtoepassingen met Microsoft Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

> [!NOTE]
> De **converteren**, **converteren naar Azure-Cloudserviceproject** opdracht wordt alleen weergegeven voor het webproject in uw oplossing. Bijvoorbeeld, is de opdracht niet beschikbaar is voor een Silverlight-project in uw oplossing.
> Wanneer u een servicepakket maakt of uw toepassing in Azure publiceren, zijn de waarschuwingen of fouten kunnen optreden. Deze waarschuwingen en fouten kunt u problemen oplossen voordat u in Azure implementeert. Bijvoorbeeld, verschijnt er een waarschuwing over een ontbrekende assembly. Zie voor meer informatie over het eventuele waarschuwingen als fouten behandelen [configureren van een Azure-Cloudserviceproject met Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Als u uw toepassing bouwen, lokaal met behulp van de rekenemulator uitvoeren of deze naar Azure publiceren, ziet u mogelijk de volgende fout in de **foutenlijst** venster: **het opgegeven pad, de bestandsnaam of beide zijn te lang**. Deze fout treedt op omdat de lengte van de naam van de volledige Azure-project te lang is. De lengte van de naam van het project, inclusief het volledige pad mag niet meer dan 146 tekens. Dit is bijvoorbeeld de naam van het volledige project inclusief bestandspad voor een Azure-project dat wordt gemaakt voor een Silverlight-toepassing: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Mogelijk moet uw oplossing verplaatsen naar een andere map met een korter pad op naar het Reduceer de lengte van de volledig gekwalificeerde naam.
>
>

Als u wilt migreren en publiceren van webtoepassingen vanuit Visual Studio naar Azure, volg deze stappen.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Een webtoepassing voor de implementatie van Azure inschakelen
### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Een webtoepassing voor de implementatie van Azure inschakelen
1. Om uw webtoepassing voor implementatie naar Azure, opent u het snelmenu voor een webproject in uw oplossing en kies Azure implementatieproject toevoegen.

    De volgende acties uitgevoerd:

   * Een Azure-project aangeroepen `<name of the web project>.Azure` wordt toegevoegd aan de oplossing voor uw toepassing.
   * Een Webrol voor het webproject wordt toegevoegd aan deze Azure-project.
   * De **lokale kopie** eigenschap is ingesteld op true voor assembly's die vereist voor MVC MVC 2, MVC 3 zijn, 4 en Silverlight Business-toepassingen. Deze assembly's wordt toegevoegd aan het servicepakket dat wordt gebruikt voor implementatie.

   > [!IMPORTANT]
   > Als u andere assembly's of bestanden die vereist voor deze webtoepassing zijn hebt, moet u de eigenschappen voor deze bestanden handmatig instellen. Zie de sectie voor informatie over het instellen van deze eigenschappen **bestanden opnemen in het servicepakket** verderop in dit artikel.
   >
   > [!NOTE]
   > Als een Webrol voor een specifieke webproject al in een Azure-project in de oplossing bestaat **converteren**, **converteren naar Azure-Cloudserviceproject** wordt niet weergegeven in het snelmenu voor dit webproject.
   >
   >

   Als u meerdere webprojecten in uw webtoepassing hebt en u wilt maken van webrollen voor elk webproject, voert u de stappen in deze procedure voor elke webproject. Hiermee maakt u afzonderlijke Azure projecten voor elke Webrol. Elke webproject kan afzonderlijk worden gepubliceerd. U kunt ook handmatig een andere Webrol toevoegen aan een bestaand Azure-project in uw webtoepassing. Om dit te doen, opent u het snelmenu voor het **rollen** map in uw Azure-project, kies **toevoegen**, klikt u vervolgens **Webrolproject in oplossing**, kiest u het project wilt toevoegen als een Webrol en kies vervolgens de **OK** knop.

## <a name="use-an-azure-sql-database-for-your-application"></a>Een Azure SQL-Database voor uw toepassing gebruiken
Als u een verbindingsreeks voor uw webtoepassing die gebruikmaakt van een SQL Server-database die op de locatie hebt, moet u deze verbindingsreeks naar een exemplaar van SQL-Database die Azure als host fungeert in plaats daarvan.

> [!IMPORTANT]
> Uw abonnement, moet u gebruikmaken van SQL-Database inschakelen. Als u toegang tot uw abonnement op de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), kunt u bepalen welke services voorziet in uw abonnement. De volgende instructies gelden voor de uitgebrachte [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885). Als u de [Azure-portal](http://portal.microsoft.com), gaat u verder met de volgende procedure.
>
>

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Met een exemplaar van SQL-Database in uw Webrol voor de verbindingsreeks
1. Maken van een exemplaar van SQL-Database in de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), volg de stappen in het volgende artikel: [maken van een SQL-databaseserver](http://go.microsoft.com/fwlink/?LinkId=225109).

   > [!NOTE]
   > Bij het instellen van de firewallregels voor uw exemplaar van SQL-Database, moet u de **andere Azure-services toegang tot deze server toestaan** selectievakje.
   >
   >
2. Volg de stappen in de volgende sectie in het volgende artikel voor het maken van een exemplaar van SQL-Database moet worden gebruikt voor de verbindingsreeks: [maken van een SQL-Database](http://go.microsoft.com/fwlink/?LinkId=225110).
3. Uitvoeren voor het kopiëren van de ADO.NET-verbindingsreeks moet worden gebruikt voor de verbindingsreeks de volgende stappen in de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).  

   1. Kies de **Database** knop en open vervolgens het knooppunt voor het abonnement dat u gebruikt voor het maken van uw exemplaar van SQL-Database.
   2. Als u wilt weergeven van de beschikbare exemplaren van SQL-Database, kiest u de **SQL-Databases** knooppunt.
   3. Als u wilt weergeven in de eigenschappen voor de database, kiest u de database. De **eigenschappen** weergegeven.

      > [!NOTE]
      > Als de **eigenschappen** weergave niet wordt weergegeven, moet u mogelijk om deze te openen met behulp van de scheidingsmarkering.
      >
      >
   4. Kies de knop met het weglatingsteken (...) naast weergave zodat de verbindingsreeksen.

      De **verbindingsreeksen** dialoogvenster wordt weergegeven.
   5. U kopieert de ADO.NET-verbindingsreeks, markeer de tekst en kies de toetsen Ctrl + C.
   6. Kies de het dialoogvenster te sluiten, de **sluiten** knop.
4. Ter vervanging van de verbindingsreeks in het bestand web.config op dit exemplaar van SQL-Database gebruiken, open het bestand web.config, markeert u de bestaande verbinding tekenreeks vermelding en kies vervolgens de toetsen Ctrl + V. De ADO.NET-verbindingsreeks voor het exemplaar van SQL Database vervangt de bestaande verbindingsreeks.
5. U moet ook de parameter toevoegen `MultipleActiveResultSets=True` de verbindingsreeks. De verbindingsreeks moet de volgende indeling hebben:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```
6. (Optioneel) Er is een alternatieve methode voor het wijzigen van de verbindingsreeks rechtstreeks in het web.config-bestand een sectie toevoegen aan een van de bestanden van een web.config-transformatie, afhankelijk van de buildconfiguratie die u gebruikt om uw servicepakket te maken. Open het bestand Web.Debug.Config of het bestand Web.Release.Config. Voeg de volgende sectie in dit bestand:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```
7. Sla het bestand dat u hebt gewijzigd en uw toepassing opnieuw publiceren.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Een exemplaar van SQL-Database gebruiken met behulp van de klassieke Azure portal
1. In de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885), kiest u het knooppunt van de SQL-Databases.

   * Als het exemplaar van SQL Database die u wilt gebruiken wordt weergegeven, kunt u kiezen om deze te openen.
   * Als u geen instanties dat nog niet hebt gemaakt, kiest u de koppeling en maak vervolgens een exemplaar.
2. Nadat u openen of maken van een database-exemplaar, kiest u de **verbindingsreeksen** koppeling.
3. Aan de onderkant van de pagina kiest u de koppeling naar de firewall-instellingen configureren en de standaardwaarden accepteren of configureer de waarden die u nodig hebt.
4. Kopieer de ADO.NET-verbindingsreeks, plakt u deze in het web.config-bestand via de oude verbindingsreeks voor de lokale-database en moet u toevoegen `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Publiceer een webtoepassing naar Azure
### <a name="to-publish-a-web-application-to-azure"></a>Voor het publiceren van een webtoepassing naar Azure
1. Test de toepassing in de lokale ontwikkeling omgeving met behulp van de Azure rekenemulator, open het snelmenu voor het Azure-project voor de Webserverrol en kiest u **instellen als opstartproject**. Kies vervolgens **Debug**, **foutopsporing starten** (toetsenbord: **F5**).

    De **starten van de Azure-omgeving foutopsporing** in het dialoogvenster wordt geopend en de toepassing wordt gestart in de browser. Zie de tabel in deze sectie voor meer informatie over het starten van elk type van de webtoepassing in de rekenemulator.
2. Als u de services voor uw toepassing te publiceren naar Azure instelt, moet u een Microsoft-account en een Azure-abonnement hebben. Gebruik de stappen in het volgende onderwerp voor het instellen van uw services: [voorbereiden publiceert of implementeert vanuit Visual Studio een Azure-toepassing](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
3. Voor het publiceren van de webtoepassing naar Azure, open het snelmenu voor het webproject en kies **publiceren naar Azure**.

    De **Publish Azure Application** in het dialoogvenster wordt geopend en Visual Studio het implementatieproces begint. Zie de sectie voor meer informatie over hoe u de toepassing publiceert, **publiceren van een Azure-toepassing vanuit Visual Studio** in [publiceren van een Cloudservice met de Azure-hulpprogramma's](vs-azure-tools-publishing-a-cloud-service.md).

   > [!NOTE]
   > U kunt ook de webtoepassing van de Azure-project publiceren. Om dit te doen, opent u het snelmenu voor het Azure-project en kiest **publiceren**.
   >
   >
4. Overzicht van de voortgang van de implementatie, ziet u de **Azure Activity Log** venster. Dit logboek wordt automatisch weergegeven wanneer het implementatieproces begint. U kunt het artikel in het gebeurtenissenlogboek om gedetailleerde informatie weer te geven kunt uitbreiden, zoals wordt weergegeven in de volgende afbeelding:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)
5. (Optioneel) Als u wilt het implementatieproces annuleren, opent u het snelmenu voor het artikel in het activiteitenlogboek en kies **annuleren en verwijder**. Dit het implementatieproces stopt en verwijdert deze de implementatieomgeving van Azure.

   > [!NOTE]
   > Als u wilt deze implementatieomgeving verwijdert nadat deze is geïmplementeerd, moet u de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).
   >
   >
6. (Optioneel) Nadat uw rolinstanties hebt gestart, Visual Studio automatisch de implementatieomgeving in toont de **Azure Compute** knooppunt in **Cloud Explorer** of **Server Explorer**. U kunt hier de status van de afzonderlijke rolinstanties bekijken.

    De volgende afbeelding ziet u de rolinstanties in **Server Explorer** als ze nog steeds in de status Bezig met initialiseren:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)
7. Voor toegang tot uw toepassing na de implementatie, klik op de pijl naast uw implementatie wanneer de status van **voltooid** wordt weergegeven in de **Azure Activity log**. Hiermee geeft de URL voor uw webtoepassing in Azure. Zie de volgende tabel voor meer informatie over het starten van een specifiek type van de webtoepassing van Azure.

    De volgende tabel bevat de details over het starten van specifieke webtoepassingen vanuit Azure of uitvoeren of fouten opsporen in een webtoepassing lokaal via de Azure Compute-Emulator:

   | Webtoepassingstype | Voer/Debug lokaal met behulp van de Rekenemulator | in Azure wordt uitgevoerd |
   | --- | --- | --- |
   | ASP.NET-webtoepassing |Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |Kies de URL-hyperlink weergegeven in de **implementatie** tabblad voor de **Azure Activity log** laden van de startpagina in de browser. |
   | ASP.NET MVC 2-webtoepassing |Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |Kies de URL-hyperlink weergegeven in de **implementatie** tabblad voor de **Azure Activity log** laden van de startpagina in de browser. |
   | ASP.NET MVC 3-webtoepassing |Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |Kies de URL-hyperlink weergegeven in de **implementatie** tabblad voor de **Azure Activity log** laden van de startpagina in de browser. |
   | ASP.NET MVC 4-webtoepassing |Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |Kies de URL-hyperlink weergegeven in de **implementatie** tabblad voor de **Azure Activity log** laden van de startpagina in de browser. |
   | Lege ASP.NET-webtoepassing |U moet een ASPX-pagina toevoegen in uw toepassing die u voor uw webproject als startpagina instellen. Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |Als u een standaard .aspx-pagina in uw toepassing hebt, kiest u de URL-hyperlink weergegeven in de **implementatie** tabblad voor de **Azure Activity log** en deze pagina wordt geladen in de browser. Als u een andere ASPX-pagina hebt, moet u navigeren naar deze specifieke pagina met de volgende notatie voor uw url:`<url for deployment>/<name of page>.aspx` |
   | Silverlight-toepassing |Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |Zo moet u navigeren naar de specifieke pagina voor uw toepassing met de volgende notatie voor uw url:`<url for deployment>/<name of page>.aspx` |
   | Silverlight-Business-toepassing |Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |Zo moet u navigeren naar de specifieke pagina voor uw toepassing met de volgende notatie voor uw url:`<url for deployment>/<name of page>.aspx` |
   | Silverlight-toepassing voor navigatie |Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |Zo moet u navigeren naar de specifieke pagina voor uw toepassing met de volgende notatie voor uw url:`<url for deployment>/<name of page>.aspx` |
   | WCF-Service-toepassing |U moet het .svc-bestand instellen als startpagina voor uw project WCF-Service. Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |U moet Ga naar het svc-bestand voor uw toepassing met de volgende notatie voor uw url:`<url for deployment>/<name of service file>.svc` |
   | WCF-werkstroom-servicetoepassing |U moet het .svc-bestand instellen als startpagina voor uw project WCF-Service. Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |U moet Ga naar het svc-bestand voor uw toepassing met de volgende notatie voor uw url:`<url for deployment>/<name of service file>.svc` |
   | ASP.NET dynamische entiteiten |Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |U moet de verbindingsreeks bijwerken (Zie volgende sectie). Ook moet u navigeren naar de specifieke pagina voor uw toepassing met de volgende notatie voor uw url:`<url for deployment>/<name of page>.aspx` |
   | ASP.NET dynamische gegevens Linq to SQL |Kies op de menubalk **Debug**, **foutopsporing starten** (toetsenbord: Kies de **F5** sleutel.). |U moet de stappen in deze procedure: een Azure SQL-database gebruiken voor uw toepassing (Zie eerdere sectie in dit onderwerp). Ook moet u navigeren naar de specifieke pagina voor uw toepassing met de volgende notatie voor uw url:`<url for deployment>/<name of page>.aspx` |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Bijwerken van een verbindingsreeks voor ASP.NET dynamische entiteiten
### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Een verbindingsreeks voor ASP.NET dynamische entiteiten bijwerken
1. Volg de stappen in de procedure voor het maken van een Azure SQL-database die kan worden gebruikt voor een dynamische entiteiten van ASP.NET-webtoepassing **een Azure SQL-database gebruiken voor uw toepassing** eerder in dit onderwerp.
2. Toevoegen van de tabellen en velden die u nodig hebt voor deze database uit de [klassieke Azure-portal](http://go.microsoft.com/fwlink/?LinkID=213885).
3. De verbindingsreeks voor dit type toepassing heeft de volgende indeling in het bestand web.config:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Update de *connectionString* waarde met de ADO.NET-verbindingsreeks voor uw Azure SQL database als volgt:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```
4. Sla het bestand web.config met de wijzigingen die u hebt aangebracht in de verbindingsreeks, op de menubalk Kies **bestand**, **opslaan web.config**.

## <a name="supported-project-templates"></a>Ondersteunde projectsjablonen
Voor het publiceren van een webtoepassing naar Azure moet de toepassing een van de projectsjablonen gebruiken voor C# of Visual Basic, die wordt vermeld in de onderstaande tabel.

| Groep Project-sjabloon | Project-sjabloon |
| --- | --- |
| Web |ASP.NET-webtoepassing |
| Web |ASP.NET MVC 2-webtoepassing |
| Web |ASP.NET MVC 3-webtoepassing |
| Web |MVC4 voor ASP.NET-webtoepassing |
| Web |Lege ASP.NET-webtoepassing |
| Web |Lege ASP.NET MVC 2-webtoepassing |
| Web |De webtoepassing van ASP.NET dynamische gegevensentiteiten |
| Web |ASP.NET dynamische gegevens Linq to SQL-webtoepassing |
| Silverlight |Silverlight-toepassing |
| Silverlight |Silverlight-Business-toepassing |
| Silverlight |Silverlight-toepassing voor navigatie |
| WCF |WCF-Service-toepassing |
| WCF |WCF-werkstroom-servicetoepassing |
| Werkstroom |WCF-werkstroom-servicetoepassing |

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over publiceren [voorbereiden te publiceren en implementeren van een Azure-toepassing vanuit Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Lees ook [instelling Up met de naam verificatiereferenties](vs-azure-tools-setting-up-named-authentication-credentials.md).

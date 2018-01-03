---
title: Migratie en publiceren van webtoepassingen met een Azure-Cloud-Service vanuit Visual Studio | Microsoft Docs
description: Informatie over het migreren en publiceren van uw webtoepassing in een Azure-cloud-service met behulp van Visual Studio
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
ms.date: 11/10/2017
ms.author: kraigb
ms.openlocfilehash: 1ced364bed821a9391f8ffd049f61ac236d98309
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>How to: migreren en publiceren van webtoepassingen met een Azure-Cloud-Service vanuit Visual Studio

Als u wilt profiteren van de hosting-services en schalen van de mogelijkheid van Azure, is het raadzaam om te migreren en implementeren van uw webtoepassing in een Azure-cloud-service. Slechts minimale wijzigingen zijn vereist. Dit artikel gaat over het implementeren van cloud-services. Zie voor App Service [een web-app in Azure App Service implementeren](app-service/app-service-deploy-local-git.md).

> [!Important]
> Deze migratie wordt alleen ondersteund voor de specifieke ASP.NET, Silverlight, WCF en WCF-Workflow-projecten. Dit wordt niet ondersteund voor ASP.NET Core projecten. Zie [projectsjablonen ondersteund](#supported-project-templates).

## <a name="migrate-a-project-to-cloud-services"></a>Een project met cloud-services migreren

1. Met de rechtermuisknop op het webtoepassingsproject en selecteer **converteren > converteren naar Microsoft Azure-Cloudserviceproject**. (Houd er rekening mee dat deze opdracht niet weergegeven wordt als er al een rol webproject in de oplossing.)
1. Visual Studio maakt een cloudserviceproject in de oplossing die de vereiste Webrol bevat. De naam van dit project is hetzelfde als uw toepassingsproject met plus het achtervoegsel `.Azure`.
1. Visual Studio worden ook de **lokale kopie** eigenschap in op true voor assembly's die vereist voor de MVC-2, 3 MVC, MVC 4 en Silverlight Business-toepassingen zijn. Deze eigenschap voegt deze assembly's toe aan het servicepakket dat wordt gebruikt voor implementatie.

   > [!Important]
   > Als u andere assembly's of bestanden die vereist voor deze webtoepassing zijn hebt, moet u de eigenschappen voor deze bestanden handmatig instellen. Zie voor meer informatie over het instellen van deze eigenschappen [bestanden opnemen in het servicepakket](#include-files-in-the-service-package).

### <a name="errors-and-warnings"></a>Fouten en waarschuwingen

Wijzen op problemen op te lossen voordat u implementeert naar Azure, zoals een ontbrekende assembly's eventuele waarschuwingen of fouten die optreden.

Als u uw toepassing bouwen, lokaal met behulp van de rekenemulator uitvoeren of deze naar Azure publiceren, ziet u mogelijk de volgende fout: "het opgegeven pad, de bestandsnaam of beide zijn te lang." Deze fout geeft aan dat de lengte van de Azure-project volledig gekwalificeerde naam 146 tekens overschrijdt. U kunt dit probleem, uw oplossing te verplaatsen naar een andere map met een korter pad.

Zie voor meer informatie over het eventuele waarschuwingen als fouten behandelen [configureren van een Azure-Cloudserviceproject met Visual Studio](vs-azure-tools-configuring-an-azure-project.md).

### <a name="test-the-migration-locally"></a>De migratie lokaal te testen

1. In Visual Studio **Solution Explorer**, met de rechtermuisknop op het toegevoegde cloudserviceproject en selecteert u **instellen als opstartproject**.
1. Selecteer **fouten opsporen > Foutopsporing starten** (F5) starten van de Azure foutopsporingsomgeving. Deze omgeving biedt specifiek emulatie van verschillende Azure-services.

### <a name="use-an-azure-sql-database-for-your-application"></a>Een Azure SQL Database voor uw toepassing gebruiken

Als u een verbindingsreeks voor uw webtoepassing die gebruikmaakt van een on-premises SQL Server database hebt, moet u uw database in plaats daarvan migreren naar Azure SQL Database en de verbindingsreeks bijwerken. Voor hulp bij dit proces, raadpleegt u de volgende onderwerpen:

- [Migratie van SQL Server-database met SQL Database in de cloud](sql-database/sql-database-cloud-migrate.md)
- [.NET (C#) gebruiken met Visual Studio verbinding en opvragen en Azure SQL-database](sql-database/sql-database-connect-query-dotnet-visual-studio.md).

## <a name="publish-the-application-to-azure-cloud-service"></a>De toepassing publiceren in Azure Cloud Service

1. Maak de benodigde cloud service- en storage-accounts in uw Azure-abonnement zoals beschreven op [voorbereiden publiceert of implementeert vanuit Visual Studio een Azure-toepassing](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
1. In Visual Studio met de rechtermuisknop op het toepassingsproject en selecteer **publiceren naar Microsoft Azure...**  (die verschilt van de opdracht 'Publiceren...'.).
1. In de **Publish Azure Application** die wordt weergegeven, meld u aan met het account op met uw Azure-abonnement en selecteer **volgende >**.
1. In de **instellingen > algemene instellingen** tabblad, selecteert u de cloudservice doel van de **Cloudservice** vervolgkeuzelijst samen met uw gekozen omgeving en configuraties. 
1. In **instellingen > instellingen voor geavanceerde**, selecteert u het opslagaccount om te gebruiken en selecteer vervolgens **volgende >**.
1. In **Diagnostics**, kies of u informatie naar Application Insights te verzenden.
1. Selecteer **volgende >** om een samenvatting wilt weergeven, selecteert u vervolgens **publiceren** implementatie te starten.
1. Visual Studio wordt geopend een venster activiteitenlogboek van waar u de voortgang kan volgen:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Optioneel) Als u wilt annuleren van het implementatieproces, met de rechtermuisknop op het artikel in het gebeurtenissenlogboek en kies **annuleren en verwijder**. Met deze opdracht het implementatieproces stopt en verwijdert deze de implementatieomgeving van Azure. Opmerking: als u wilt deze implementatieomgeving verwijdert nadat deze is geïmplementeerd, moet u de [Azure-portal](https://portal.azure.com).
1. (Optioneel) Nadat uw rolinstanties hebt gestart, Visual Studio automatisch de implementatieomgeving in toont de **Server Explorer > Cloud Services** knooppunt. U kunt hier de status van de afzonderlijke rolinstanties bekijken.
1. Voor toegang tot uw toepassing na de implementatie, klik op de pijl naast uw implementatie wanneer de status van **voltooid** wordt weergegeven in de **Azure Activity log** samen met de URL. Zie de volgende tabel voor meer informatie over het starten van een specifiek type van de webtoepassing van Azure.

## <a name="using-the-compute-emulator-and-starting-application-in-azure"></a>Met behulp van de rekenemulator en -toepassing in Azure

Alle toepassingstypen kunnen worden gestart in een browser die verbonden zijn met de foutopsporingsfunctie door het selecteren van **fouten opsporen > Foutopsporing starten** (F5). Met een lege ASP.NET-webtoepassing-project, moet u eerst toevoegen een `.aspx` pagina in uw toepassing en stel dit in als de startpagina voor uw webproject.

De volgende tabel bevat informatie over het starten van de toepassing in Azure:

   | Webtoepassingstype | in Azure wordt uitgevoerd |
   | --- | --- | --- |
   | ASP.NET-webtoepassing<br/>(met inbegrip van MVC-2, 3 MVC of MVC-4) | Selecteer de URL in de **implementatie** tabblad voor de **Azure Activity log**. |
   | Lege ASP.NET-webtoepassing | Als u een standaard hebt `.aspx` pagina in uw toepassing, selecteert u de URL in de **implementatie** tabblad voor de **Azure Activity log**. Voer een URL van de volgende vorm in een browser om te navigeren naar een andere pagina:`<deployment_url>/<page_name>.aspx` |
   | Silverlight-toepassing<br/>Silverlight-Business-toepassing<br/>Silverlight-toepassing voor navigatie | Navigeer naar de pagina voor uw toepassing met behulp van de volgende URL-notatie:`<deployment_url>/<page_name>.aspx` |
    WCF-Service-toepassing<br/>WCF-werkstroom-servicetoepassing | Stel de `.svc` bestand als de startpagina voor uw project WCF-Service. Ga naar`<deployment_url>/<service_file>.svc` |
   | ASP.NET dynamische entiteiten<br/>ASP.NET dynamische gegevens Linq to SQL | De verbindingsreeks bijwerken zoals beschreven in de volgende sectie. Navigeer naar `<deployment_url>/<page_name>.aspx`. Voor Linq naar SQL, moet u een Azure SQL database. |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Bijwerken van een verbindingsreeks voor ASP.NET dynamische entiteiten

1. Een Azure SQL-database voor een dynamische entiteiten van ASP.NET-webtoepassing maken zoals eerder in (#use-an-azuresql-database-for-your-application) beschreven.
1. Voeg de tabellen en velden die u nodig hebt voor deze database uit de Azure portal.
1. Geef een verbindingsreeks in de `web.config` -bestand met de volgende indeling en sla het bestand:

    ```xml
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Update de *connectionString* waarde met de ADO.NET-verbindingsreeks voor uw Azure SQL database als volgt:

    ```xml
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

## <a name="supported-project-templates"></a>Ondersteunde projectsjablonen

Toepassingen die kunnen worden gemigreerd en kan worden gepubliceerd met cloud-services moeten een van de sjablonen in de onderstaande tabel gebruiken. ASP.NET Core wordt niet ondersteund.

| Sjabloon-groep | Project-sjabloon |
| --- | --- |
| Web | ASP.NET-webtoepassing (.NET Framework) |
| Web | ASP.NET MVC 2-webtoepassing |
| Web | ASP.NET MVC 3-webtoepassing |
| Web | MVC4 voor ASP.NET-webtoepassing |
| Web | Lege ASP.NET-webtoepassing (of Site) |
| Web | Lege ASP.NET MVC 2-webtoepassing |
| Web | De webtoepassing van ASP.NET dynamische gegevensentiteiten |
| Web | ASP.NET dynamische gegevens Linq to SQL-webtoepassing |
| Silverlight | Silverlight-toepassing |
| Silverlight | Silverlight-Business-toepassing |
| Silverlight | Silverlight-toepassing voor navigatie |
| WCF | WCF-Service-toepassing |
| WCF | WCF-werkstroom-servicetoepassing |
| Werkstroom | WCF-werkstroom-servicetoepassing |

## <a name="next-steps"></a>Volgende stappen

- [Voorbereiden op publiceert of implementeert vanuit Visual Studio een Azure-toepassing](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md)
- [Instellen van referenties voor verificatie met de naam](vs-azure-tools-setting-up-named-authentication-credentials.md).

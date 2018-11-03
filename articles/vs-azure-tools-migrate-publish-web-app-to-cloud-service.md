---
title: Over het migreren en publiceren van een webtoepassing in een Azure Cloudservice vanuit Visual Studio | Microsoft Docs
description: Meer informatie over het migreren en publiceren van uw webtoepassing met een Azure-cloudservice met Visual Studio
services: visual-studio-online
author: ghogen
manager: douge
ms.assetid: 9394adfd-a645-4664-9354-dd5df08e8c91
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: ghogen
ms.openlocfilehash: df4fab213d5bf3f947d696e0a45c27fe1bcf597b
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969571"
---
# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Hoe: migreren en publiceren van een webtoepassing in een Azure Cloudservice vanuit Visual Studio

Als u wilt profiteren van de services hosten en schalen mogelijkheid van Azure, is het raadzaam om te migreren en implementeren van uw webtoepassing met een Azure-cloudservice. Alleen minimale wijzigingen zijn nodig. Dit artikel gaat over het implementeren van cloud Services. Zie voor App Service, [een web-app in Azure App Service implementeren](app-service/app-service-deploy-local-git.md).

> [!Important]
> Deze migratie wordt alleen ondersteund voor de specifieke projecten van ASP.NET, Silverlight, WCF en WCF-werkstroom. Het wordt niet ondersteund voor ASP.NET Core-projecten. Zie [ondersteunde projectsjablonen](#supported-project-templates).

## <a name="migrate-a-project-to-cloud-services"></a>Migreren van een project met cloud-services

1. Met de rechtermuisknop op het web application-project en selecteer **converteren > converteren naar Microsoft Azure-Cloudserviceproject**. (Houd er rekening mee dat deze opdracht niet weergegeven wordt als u al een project om de rol in de oplossing.)
1. Visual Studio maakt een cloud service-project in de oplossing die het bevat de vereiste Webrol. De naam van dit project is hetzelfde als uw project een toepassing met het achtervoegsel `.Azure`.
1. Visual Studio stelt ook het **lokale kopie** eigenschap op ' True ' voor elke assembly's die vereist voor MVC 2, 3 MVC, MVC 4 en Silverlight zakelijke toepassingen zijn. Deze eigenschap wordt deze assembly's toegevoegd aan het servicepakket dat wordt gebruikt voor implementatie.

   > [!Important]
   > Als u andere assembly's of bestanden die vereist voor deze webtoepassing zijn hebt, moet u de eigenschappen voor deze bestanden handmatig instellen. Zie voor meer informatie over het instellen van deze eigenschappen [opnemen van bestanden in het servicepakket](#include-files-in-the-service-package).

### <a name="errors-and-warnings"></a>Fouten en waarschuwingen

Eventuele waarschuwingen of fouten die optreden duiden op problemen op te lossen voordat u implementeert in Azure, zoals ontbrekende assembly's.

Als u uw toepassing bouwen, voeren lokaal met behulp van de rekenemulator of deze naar Azure publiceren, ziet u mogelijk de fout: "het opgegeven pad, de bestandsnaam of beide zijn te lang." Deze fout geeft aan dat de lengte van de volledig gekwalificeerde Azure projectnaam 146 tekens overschrijdt. Het probleem oplossen door uw oplossing te verplaatsen naar een andere map met een korter pad.

Zie voor meer informatie over het behandelen van waarschuwingen als fouten [een Azure-Cloudserviceproject configureren met Visual Studio](vs-azure-tools-configuring-an-azure-project.md).

### <a name="test-the-migration-locally"></a>De migratie lokaal testen

1. In Visual Studio **Solution Explorer**, met de rechtermuisknop op het toegevoegde cloud service-project en selecteer **Set as Startup Project**.
1. Selecteer **fouten opsporen > Foutopsporing starten** (F5) om te starten van de Azure foutopsporingsomgeving. Deze omgeving biedt specifiek emulatie van verschillende Azure-services.

### <a name="use-an-azure-sql-database-for-your-application"></a>Een Azure SQL Database voor uw toepassing gebruiken

Als u een verbindingsreeks voor uw webtoepassing die gebruikmaakt van een on-premises SQL Server-database hebt, moet u in plaats daarvan uw database migreren naar Azure SQL Database en de verbindingsreeks bijwerken. Zie voor hulp bij dit proces, in de volgende onderwerpen:

- [SQL Server-databasemigratie naar SQL Database in de cloud](sql-database/sql-database-cloud-migrate.md)
- [.NET (C#) gebruiken met Visual Studio dat verbinding maakt en query's uitvoeren en Azure SQL-database](sql-database/sql-database-connect-query-dotnet-visual-studio.md).

## <a name="publish-the-application-to-azure-cloud-service"></a>De toepassing publiceren in Azure Cloud Service

1. Maak de benodigde cloud-service en storage-accounts in uw Azure-abonnement zoals beschreven op [voorbereiden om te publiceren of implementeren van een Azure-toepassing vanuit Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).
1. In Visual Studio met de rechtermuisknop op het toepassingsproject en selecteer **publiceren naar Microsoft Azure...**  (die verschilt van de opdracht 'Publiceren...'.).
1. In de **Publish Azure Application** die wordt weergegeven, meld u aan met het account met uw Azure-abonnement en selecteer **volgende >**.
1. In de **instellingen > algemene instellingen die u** tabblad, selecteert u de doelcloudservice van de **Cloudservice** vervolgkeuzelijst, samen met uw gekozen omgeving en configuraties. 
1. In **instellingen > Geavanceerde instellingen**, selecteert u het opslagaccount om te gebruiken, en selecteer vervolgens **volgende >**.
1. In **Diagnostics**, kies of u gegevens te verzenden naar Application Insights.
1. Selecteer **volgende >** als u wilt een samenvatting weergeven, selecteert u vervolgens **publiceren** implementatie te starten.
1. Visual Studio opent u een venster activiteitenlogboek van waar u de voortgang kunt volgen:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Optioneel) Als u wilt annuleren van het implementatieproces, met de rechtermuisknop op het regelitem in het activiteitenlogboek en kies **annuleren en verwijderen**. Met deze opdracht stopt het implementatieproces en verwijdert u de implementatieomgeving van Azure. Opmerking: als u wilt deze implementatieomgeving verwijderen nadat deze is geïmplementeerd, moet u de [Azure-portal](https://portal.azure.com).
1. (Optioneel) Nadat uw rolinstanties hebt gestart, Visual Studio automatisch ziet u de implementatieomgeving in de **Server Explorer > Cloud Services** knooppunt. Hier ziet u de status van de afzonderlijke rolinstanties.
1. Voor toegang tot uw toepassing na de implementatie, kies de pijl naast uw implementatie wanneer de status van **voltooid** wordt weergegeven in de **Azure-activiteitenlogboek** samen met de URL. Zie de volgende tabel voor informatie over het starten van een specifiek type web-App van Azure.

## <a name="using-the-compute-emulator-and-starting-application-in-azure"></a>Met behulp van de rekenemulator en starten van de toepassing in Azure

Alle soorten toepassingen kunnen worden gestart in een browser die is verbonden met de foutopsporingsfunctie hiervoor **fouten opsporen > Foutopsporing starten** (F5). Met een lege ASP.NET-webtoepassing-project, moet u eerst toevoegen een `.aspx` pagina in uw toepassing en deze instellen als de startpagina voor uw webproject.

De volgende tabel bevat details over het starten van de toepassing in Azure:

   | Het toepassingstype | Uitvoeren in Azure |
   | --- | --- | --- |
   | ASP.NET-webtoepassing<br/>(inclusief MVC 2, 3 MVC, MVC 4) | Selecteer de URL in de **implementatie** tabblad voor de **Azure-activiteitenlogboek**. |
   | Lege ASP.NET-webtoepassing | Als u een standaard hebt `.aspx` pagina in uw toepassing, selecteert u de URL in de **implementatie** tabblad voor de **Azure-activiteitenlogboek**. Ga naar een andere pagina, voer een URL van de volgende notatie in een browser: `<deployment_url>/<page_name>.aspx` |
   | Silverlight-toepassing<br/>Silverlight-Business-toepassing<br/>Silverlight-toepassing voor navigatie | Navigeer naar de specifieke pagina voor uw toepassing met behulp van de volgende URL-notatie: `<deployment_url>/<page_name>.aspx` |
    WCF-Service-toepassing<br/>WCF-werkstroom-servicetoepassing | Stel de `.svc` bestand op als de startpagina voor uw project WCF-Service. Ga naar `<deployment_url>/<service_file>.svc` |
   | ASP.NET-Dynamic-entiteiten<br/>ASP.NET-dynamische gegevens Linq to SQL | De verbindingsreeks bijwerken zoals beschreven in de volgende sectie. Navigeer vervolgens naar `<deployment_url>/<page_name>.aspx`. Voor Linq to SQL, moet u een Azure SQL database. |

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Een verbindingsreeks voor ASP.NET-Dynamic-entiteiten bijwerken

1. Maak een SQL Azure database voor een dynamische entiteiten van ASP.NET-webtoepassing zoals eerder in (#use-an-azuresql-database-for-your-application) beschreven.
1. Voeg de tabellen en velden die u nodig hebt voor deze database vanuit de Azure-portal.
1. Geef een verbindingsreeks in de `web.config` -bestand met de volgende indeling en sla het bestand:

    ```xml
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Update de *connectionString* waarde met de ADO.NET-verbindingsreeks voor uw SQL Azure-database als volgt:

    ```xml
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

## <a name="supported-project-templates"></a>Ondersteunde projectsjablonen

Toepassingen die kunnen worden gemigreerd en gepubliceerd met cloud-services moeten een van de sjablonen in de onderstaande tabel gebruiken. ASP.NET Core wordt niet ondersteund.

| Sjabloon-groep | Projectsjabloon |
| --- | --- |
| Web | ASP.NET-webtoepassing (.NET Framework) |
| Web | ASP.NET MVC 2-webtoepassing |
| Web | ASP.NET MVC 3-webtoepassing |
| Web | Web-App van ASP.NET MVC4 |
| Web | Lege ASP.NET-webtoepassing (of Site) |
| Web | Lege ASP.NET MVC 2-webtoepassing |
| Web | Gegevensentiteiten dynamische ASP.NET-webtoepassing |
| Web | ASP.NET-dynamische gegevens Linq to SQL-webtoepassing |
| Silverlight | Silverlight-toepassing |
| Silverlight | Silverlight-Business-toepassing |
| Silverlight | Silverlight-toepassing voor navigatie |
| WCF | WCF-Service-toepassing |
| WCF | WCF-werkstroom-servicetoepassing |
| Werkstroom | WCF-werkstroom-servicetoepassing |

## <a name="next-steps"></a>Volgende stappen

- [Voorbereiden om te publiceren of implementeren van een Azure-toepassing vanuit Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md)
- [Instellen van referenties voor verificatie met de naam](vs-azure-tools-setting-up-named-authentication-credentials.md).

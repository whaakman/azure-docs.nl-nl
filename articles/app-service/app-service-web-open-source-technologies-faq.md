---
title: "Open source-technologieën Veelgestelde vragen over Azure web-apps | Microsoft Docs"
description: "Vind antwoorden op veelgestelde vragen over open source-technologieën in de functie Web Apps van Azure App Service."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 4fb443691e216169dd1322b96d77139ffde752d4
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a>Open source-technologieën Veelgestelde vragen voor Web-Apps in Azure

In dit artikel bevat de antwoorden op veelgestelde vragen (FAQ's) over problemen met technologieën voor open-source de [functie van Azure App Service Web Apps](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a>Mijn ClearDB-database is niet beschikbaar. Hoe kan ik dit oplossen?

Problemen met database contact op met [ClearDB ondersteuning](https://www.cleardb.com/developers/help/support). 

Zie voor antwoorden op veelgestelde vragen over ClearDB [ClearDB Veelgestelde vragen over](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a>Waarom worden mijn ClearDB-database, is niet gemigreerd tijdens de abonnementmigratie van mijn?

Wanneer u Resourcemigratie voor abonnementen uitvoert, zijn enkele beperkingen van toepassing. Een ClearDB MySQL-database is een service van derden en wordt tijdens de migratie van een Azure-abonnement niet gemigreerd.

Als u niet de migratie van uw MySQL-database beheert voordat u uw Azure-resources migreert, is het mogelijk dat uw ClearDB MySQL-database niet beschikbaar. Om dit te voorkomen, eerst handmatig migreren uw ClearDB-database, en vervolgens migreren de Azure-abonnement voor uw web-app.

Zie voor meer informatie [Veelgestelde vragen over ClearDB MySQL-databases met Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).

## <a name="how-do-i-turn-on-php-logging-to-troubleshoot-php-issues"></a>Hoe kan ik PHP logboekregistratie voor het oplossen van problemen voor PHP inschakelen?

PHP-logboekregistratie inschakelen:

1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
2. Selecteer in het bovenste menu **Console voor foutopsporing** > **CMD**.
3. Selecteer de **Site** map.
4. Selecteer de **wwwroot** map.
5. Selecteer de  **+**  pictogram en selecteer vervolgens **nieuw bestand**.
6. De bestandsnaam ingesteld op **. user.ini**.
7. Schakel het potloodpictogram naast **. user.ini**.
8. Voeg deze code in het bestand:`log_errors=on`
9. Selecteer **Opslaan**.
10. Schakel het potloodpictogram naast **wp-config.php**.
11. Wijzig de tekst in de volgende code:
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging to /wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings to screendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors to screenini_set('display_errors', 0);
   ```
12. Start opnieuw op uw web-app in de Azure portal, in het webmenu-app.

Zie voor meer informatie [inschakelen WordPress foutenlogboeken](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a>Hoe meld ik toepassingsfouten in Python in apps die worden gehost in App Service

Voor het vastleggen van toepassingsfouten in Python:

1. Selecteer in de Azure-portal in uw web-app **instellingen**.
2. Op de **instellingen** tabblad **toepassingsinstellingen**.
3. Onder **appinstellingen**, voer de volgende sleutel-waardepaar:
    * Sleutel: WSGI_LOG
    * Waarde: D:\home\site\wwwroot\logs.txt (uw keuze van bestandsnaam invoeren)

U ziet nu de fouten in het bestand logs.txt in de wwwroot-map.

## <a name="how-do-i-change-the-version-of-the-nodejs-application-that-is-hosted-in-app-service"></a>Hoe wijzig ik de versie van de Node.js-toepassing die wordt gehost in App Service?

Als u wilt de versie van de Node.js-toepassing wijzigen, kunt u een van de volgende opties:

*   In de Azure portal gebruiken **appinstellingen**.
    1. Ga naar uw web-app in de Azure portal.
    2. Op de **instellingen** blade Selecteer **toepassingsinstellingen**.
    3. In **appinstellingen**, kunt u WEBSITE_NODE_DEFAULT_VERSION opnemen als de sleutel en de versie van Node.js die u wilt gebruiken als de waarde.
    4. Ga naar uw [Kudu-console](https://*yourwebsitename*.scm.azurewebsites.net).
    5. Als u wilt controleren de Node.js-versie, voer de volgende opdracht:  
   ```
   node -v
   ```
*   Het bestand iisnode.yml wijzigen. Het wijzigen van de Node.js-versie in het bestand iisnode.yml alleen Hiermee stelt u de runtime-omgeving waarnaar iisnode gebruikt. Uw cmd Kudu en andere nog steeds gebruik van de Node.js-versie die is ingesteld in **appinstellingen** in de Azure portal.

    De iisnode.yml als handmatig wilt instellen, door een bestand iisnode.yml in de hoofdmap van uw app te maken. In het bestand zijn onder andere de volgende regel:
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   Het bestand iisnode.yml instellen met behulp van package.json tijdens de implementatie van de bron-besturingselement.
    Het implementatieproces voor beheer van Azure bron omvat de volgende stappen:
    1. Hiermee verplaatst u inhoud naar de Azure-web-app.
    2. Als er niet (deploy.cmd, .deployment-bestanden) in de hoofdmap van de web-app is, maakt een standaard implementatiescript.
    3. Wordt uitgevoerd een implementatiescript waarin deze een iisnode.yml bestand maakt als u de Node.js-versie in het bestand package.json vermeld > engine`"engines": {"node": "5.9.1","npm": "3.7.3"}`
    4. Het bestand iisnode.yml heeft de volgende regel code:
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-the-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a>Verschijnt het bericht 'Fout bij het maken van een databaseverbinding ' in mijn WordPress-app die wordt gehost in App Service. Hoe kan ik dit oplossen?

Als deze fout wordt weergegeven in de Azure WordPress-app, zodat php_errors.log en debug.log, voltooid de stappen uiteengezet in [inschakelen WordPress foutenlogboeken](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

De fout optreedt wanneer de logboeken zijn ingeschakeld, en controleer de logboeken om te zien als u buiten de verbindingen worden uitgevoerd:
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded the ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

Als deze fout wordt weergegeven in uw debug.log of php_errors.log bestanden, wordt uw app overschrijdt het aantal verbindingen. Als u op ClearDB host, controleert u of het aantal verbindingen die beschikbaar zijn in uw [serviceplan](https://www.cleardb.com/pricing.view).

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a>Hoe ik fouten opsporen in een Node.js-app die wordt gehost in App Service?

1.  Ga naar uw [Kudu-console](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).
2.  Ga naar de map van uw toepassing logs (D:\home\LogFiles\Application).
3.  Controleer in het bestand logging_errors.txt voor inhoud.

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a>Hoe kan ik systeemeigen Python-modules in een App Service-web-app of API-app installeren?

Sommige pakketten mogelijk niet installeren met behulp van pip in Azure. Het pakket mogelijk niet beschikbaar op de Python Package Index of het is mogelijk dat een compiler vereist (een compiler is niet beschikbaar is op de computer waarop de web-app in App Service). Zie voor meer informatie over het installeren van systeemeigen modules in App Service-web-apps en API apps [installeren Python-modules in App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).

## <a name="how-do-i-deploy-a-django-app-to-app-service-by-using-git-and-the-new-version-of-python"></a>Hoe kan ik een Django-app in App Service implementeren met behulp van Git en de nieuwe versie van Python?

Zie voor meer informatie over het installeren van Django [implementeert een Django-app in App Service](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).

## <a name="where-are-the-tomcat-log-files-located"></a>Waar bevinden de Tomcat-logboekbestanden zich?

Voor Azure Marketplace en aangepaste implementaties:

* Locatie van de map: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs
* Bestanden die van belang:
    * catalina. *jjjj-mm-dd*.log
    * host-manager. *jjjj-mm-dd*.log
    * localhost. *jjjj-mm-dd*.log
    * Manager. *jjjj-mm-dd*.log
    * site_access_log. *jjjj-mm-dd*.log


Voor portal **appinstellingen** implementaties:

* Locatie van de map: D:\home\LogFiles
* Bestanden die van belang:
    * catalina. *jjjj-mm-dd*.log
    * host-manager. *jjjj-mm-dd*.log
    * localhost. *jjjj-mm-dd*.log
    * Manager. *jjjj-mm-dd*.log
    * site_access_log. *jjjj-mm-dd*.log

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a>Hoe kan ik JDBC-stuurprogramma verbindingsfouten oplossen?

U ziet het volgende bericht in uw Tomcat-Logboeken:

```
The web application[ROOT] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak,the JDBC Driver has been forcibly unregistered
```

De fout oplossen:

1. Verwijder het bestand sqljdbc*.jar van uw app/lib-map.
2. Als u de aangepaste Tomcat of Azure Marketplace Tomcat-webserver gebruikt, moet u dit JAR-bestand kopiëren naar de map Tomcat-lib.
3. Als u Java via de Azure-portal inschakelen wilt (Selecteer **Java 1.8** > **Tomcat-server**), het sqljdbc.* jar-bestand in de map die parallel aan uw app te kopiëren. De volgende klassepad vervolgens toevoegen aan het bestand web.config:

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path to the sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-to-copy-live-log-files"></a>Waarom zie ik fouten wanneer ik probeert te kopiëren live logboekbestanden?

Als u probeert te kopiëren van live logboekbestanden voor een Java-app (bijvoorbeeld Tomcat), ziet u mogelijk deze FTP-fout:

```
Error transferring file [filename] Copying files from remote side failed.
    
The process cannot access the file because it is being used by another process.
```

Het foutbericht kan afwijken, afhankelijk van de FTP-client.

Alle Java-apps hebben dit probleem vergrendelen. Alleen Kudu biedt ondersteuning voor het downloaden van dit bestand terwijl de app wordt uitgevoerd.

De app wordt gestopt, kunt FTP-toegang tot deze bestanden.

Een andere oplossing is het schrijven van een webtaak waarmee volgens een planning wordt uitgevoerd en deze bestanden worden gekopieerd naar een andere map. Zie voor een voorbeeldproject de [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) project.

## <a name="where-do-i-find-the-log-files-for-jetty"></a>Waar vind ik de logboekbestanden voor Jetty

Voor de Marketplace en aangepaste implementaties is het logboekbestand in de map D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs. Houd er rekening mee dat de locatie van de map afhankelijk is van de versie van de Jetty die u gebruikt. Bijvoorbeeld, het pad opgegeven hier is voor Jetty 9.1.2. Zoek naar jetty_*YYYY_MM_DD*. stderrout.log.

Voor implementaties van de portal App-instelling is het logboekbestand in D:\home\LogFiles. Zoek naar jetty_*YYYY_MM_DD*. stderrout.log

## <a name="can-i-send-email-from-my-azure-web-app"></a>Kan ik e-mail verzenden vanuit Mijn Azure-web-app

App Service beschikt niet over een ingebouwde e-functie. Voor sommige goede alternatieven voor het verzenden van e-mail van uw app, raadpleegt u dit [bespreking van de Stack Overflow](http://stackoverflow.com/questions/17666161/sending-email-from-azure).

## <a name="why-does-my-wordpress-site-redirect-to-another-url"></a>Waarom mijn WordPress-site omleiden naar een andere URL?

Als u onlangs naar Azure hebt gemigreerd, mogelijk WordPress omleiden naar de oude domein-URL. Dit wordt veroorzaakt door een instelling in de MySQL-database.

WordPress contactpersoon + is een Azure Site-uitbreiding die u gebruiken kunt om bij te werken van de omleidings-URL rechtstreeks in de database. Zie voor meer informatie over het gebruik van WordPress contactpersoon + [WordPress hulpprogramma's en MySQL migratie met WordPress contactpersoon +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

U kunt ook als u liever handmatig bijwerken van de omleiding URL met behulp van SQL-query's of PHPMyAdmin, Zie [WordPress: omleiden naar foutieve URL](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a>Hoe kan ik mijn WordPress aanmelden wachtwoord wijzigen?

Als u uw WordPress aanmelden wachtwoord vergeten bent, kunt u WordPress contactpersoon + bij te werken. De WordPress contactpersoon + Azure Site-uitbreiding installeren om uw wachtwoord opnieuw instellen, en voltooi vervolgens de stappen in [WordPress hulpprogramma's en MySQL migratie met WordPress contactpersoon +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="i-cant-sign-in-to-wordpress-how-do-i-resolve-this"></a>Ik kan niet aanmelden bij WordPress. Hoe kan ik dit oplossen?

Als u WordPress vergrendeld merkt na de installatie van een invoegtoepassing onlangs, hebt u mogelijk een defecte invoegtoepassing. WordPress contactpersoon + is een Azure-Site-uitbreiding die u kunnen helpen uitschakelen invoegtoepassingen in WordPress. Zie voor meer informatie [WordPress hulpprogramma's en MySQL migratie met WordPress contactpersoon +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="how-do-i-migrate-my-wordpress-database"></a>Hoe Migreer ik mijn WordPress-database

U hebt meerdere mogelijkheden voor het migreren van de MySQL-database die verbonden met uw WordPress-website:

* Ontwikkelaars: Gebruik de [opdrachtprompt of PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)
* Niet-Ontwikkelaars: [WordPress contactpersoon +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)

## <a name="how-do-i-help-make-wordpress-more-secure"></a>Hoe u helpen WordPress veiliger maken?

Zie voor meer informatie over aanbevolen beveiligingsprocedures voor WordPress, [Best practices voor beveiliging van WordPress in Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).

## <a name="i-am-trying-to-use-phpmyadmin-and-i-see-the-message-access-denied-how-do-i-resolve-this"></a>Ik wil gebruiken PHPMyAdmin en verschijnt het bericht 'Toegang geweigerd.' Hoe kan ik dit oplossen?

U kunt dit probleem kan optreden als de MySQL-in-app-functie nog niet wordt uitgevoerd in dit App Service-exemplaar. Probeer het probleem op te lossen voor toegang tot uw website. Hiermee start u de vereiste processen, met inbegrip van het proces van MySQL-app. Zorg ervoor dat mysqld.exe wordt vermeld in de processen om te controleren of MySQL in-app wordt uitgevoerd, klikt u in proces Explorer.

Nadat u ervoor te dat MySQL zorgen in-app wordt uitgevoerd, probeert u PHPMyAdmin gebruiken.

## <a name="i-get-an-http-403-error-when-i-try-to-import-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a>Ik krijg een HTTP 403-foutmelding wanneer ik probeer te importeren en exporteren van mijn MySQL-database in-app met behulp van PHPMyadmin. Hoe kan ik dit oplossen?

Als u een oudere versie van Chrome gebruikt, u mogelijk ondervindt een bekend probleem. Upgraden naar een nieuwere versie van Chrome het probleem op te lossen. Ook kunt u proberen met een andere browser, zoals Internet Explorer of Edge, waar het probleem niet wordt uitgevoerd.

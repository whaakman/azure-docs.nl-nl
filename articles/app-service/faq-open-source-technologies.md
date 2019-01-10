---
title: Open-source technologieën Veelgestelde vragen - Azure App Service | Microsoft Docs
description: Krijg antwoorden op veelgestelde vragen over open-source technologieën in de functie Web Apps van Azure App Service.
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: 07912dab52cb0569428d070282551eebbdb1c7bc
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54191442"
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a>Open-source technologieën Veelgestelde vragen over de Web-Apps in Azure

In dit artikel vindt u antwoorden op veelgestelde vragen over problemen met open-source-technologieën voor de [functie van Azure App Service Web Apps](https://azure.microsoft.com/services/app-service/web/).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-turn-on-php-logging-to-troubleshoot-php-issues"></a>Hoe kan ik logboekregistratie voor het oplossen van problemen met PHP PHP inschakelen?

PHP-logboekregistratie inschakelen:

1. Aanmelden bij uw [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).
2. Selecteer in het bovenste menu **Foutopsporingsconsole** > **CMD**.
3. Selecteer de **Site** map.
4. Selecteer de **wwwroot** map.
5. Selecteer de **+** pictogram en selecteer vervolgens **nieuw bestand**.
6. Naam van het bestand ingesteld op **. user.ini**.
7. Selecteer het potloodpictogram naast **. user.ini**.
8. Voeg deze code toe in het bestand: `log_errors=on`
9. Selecteer **Opslaan**.
10. Selecteer het potloodpictogram naast **wp-config.php**.
11. Wijzig de tekst in de volgende code:
   ```php
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging to /wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Suppress errors and warnings to screendefine('WP_DEBUG_DISPLAY', false);//Suppress PHP errors to screenini_set('display_errors', 0);
   ```
12. Start opnieuw op met uw web-app in Azure portal, in het webmenu-app.

Zie voor meer informatie, [inschakelen WordPress-foutenlogboeken](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a>Hoe meld ik Python-toepassingsfouten in apps die worden gehost in App Service?
[!INCLUDE [web-sites-python-troubleshooting-wsgi-error-log](../../includes/web-sites-python-troubleshooting-wsgi-error-log.md)]

## <a name="how-do-i-change-the-version-of-the-nodejs-application-that-is-hosted-in-app-service"></a>Hoe wijzig ik de versie van de Node.js-toepassing die wordt gehost in App Service?

Als u wilt de versie van de Node.js-toepassing wijzigen, kunt u een van de volgende opties gebruiken:

*   In de Azure-portal, gebruikt u **App-instellingen**.
    1. In de Azure-portal, gaat u naar uw web-app.
    2. Op de **instellingen** Selecteer **toepassingsinstellingen**.
    3. In **App-instellingen**, kunt u WEBSITE_NODE_DEFAULT_VERSION opnemen als de sleutel en de versie van Node.js die u wilt gebruiken als de waarde.
    4. Ga naar uw [Kudu-console](https://*yourwebsitename*.scm.azurewebsites.net).
    5. Om te controleren of de Node.js-versie, voer de volgende opdracht:  
   ```
   node -v
   ```
*   Het bestand iisnode.yml wijzigen. Wijzigen van de Node.js-versie in het bestand iisnode.yml alleen Hiermee stelt u de runtime-omgeving waarnaar iisnode wordt gebruikt. De Kudu-cmd en andere nog steeds gebruikmaken van de Node.js-versie die is ingesteld in **App-instellingen** in Azure portal.

    Als u wilt de iisnode.yml handmatig instellen, maakt u een iisnode.yml-bestand in de hoofdmap van uw app. In het bestand, zijn onder andere de volgende regel:
   ```yml
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   Het bestand iisnode.yml instellen met behulp van package.json tijdens de implementatie van de bron-besturingselement.
    Het implementatieproces van de Azure-bron-besturingselement omvat de volgende stappen uit:
    1. Inhoud verplaatst naar de Azure-web-app.
    2. Hiermee maakt u een implementatiescript standaard als er geen een (deploy.cmd, .deployment-bestanden) in de hoofdmap van de web-app.
    3. Wordt uitgevoerd een implementatiescript waarin deze een iisnode.yml bestand maakt als u de Node.js-versie in het bestand package.json vermeld > engine `"engines": {"node": "5.9.1","npm": "3.7.3"}`
    4. Het bestand iisnode.yml heeft de volgende coderegel:
        ```yml
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-the-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a>Ik zie het bericht 'Fout bij het maken van een databaseverbinding' in mijn WordPress-app die wordt gehost in App Service. Hoe kan ik dit oplossen?

Als u deze fout in uw Azure-WordPress-app ziet, zodat php_errors.log en -foutopsporingslogboek, voltooid de stappen beschreven [inschakelen WordPress-foutenlogboeken](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).

Wanneer de logboeken zijn ingeschakeld, de fout te reproduceren en controleer de logboeken om te zien als u verbindingen, worden uitgevoerd:
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded the ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

Als u deze fout in uw foutopsporingslogboek of php_errors.log bestanden ziet, wordt uw app van meer dan het aantal verbindingen. Als u die als host voor ClearDB fungeert, controleert u of het aantal verbindingen die beschikbaar zijn in uw [service-plan](https://www.cleardb.com/pricing.view).

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a>Hoe ik fouten opsporen in een Node.js-app die wordt gehost in App Service?

1.  Ga naar uw [Kudu-console](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).
2.  Ga naar de toepassingsmap Logboeken (D:\home\LogFiles\Application).
3.  Controleer in het bestand logging_errors.txt voor inhoud.

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a>Hoe kan ik systeemeigen Python-modules installeren in een App Service-web-app of API-app?

Sommige pakketten kunnen niet installeren met behulp van pip in Azure. Het pakket mogelijk niet beschikbaar op de Python Package Index of het is mogelijk dat een compiler vereist (een compiler is niet beschikbaar is op de computer waarop de web-app in App Service). Zie voor meer informatie over het installeren van systeemeigen modules in App Service WebApps en API apps [installeren Python-modules in App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).

## <a name="how-do-i-deploy-a-django-app-to-app-service-by-using-git-and-the-new-version-of-python"></a>Hoe implementeer ik een Django-app in App Service met behulp van Git en de nieuwe versie van Python?

Zie voor meer informatie over het installeren van Django [een Django-app implementeren in App Service](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).

## <a name="where-are-the-tomcat-log-files-located"></a>Waar bevinden de Tomcat-logboekbestanden zich?

Voor Azure Marketplace en aangepaste implementaties:

* Locatie van map: D:\home\site\wwwroot\bin\apache-Tomcat-8.0.33\logs
* Bestanden van belang:
    * catalina.*yyyy-mm-dd*.log
    * host-manager.*yyyy-mm-dd*.log
    * localhost. *jjjj-mm-dd*.log
    * Manager. *jjjj-mm-dd*.log
    * site_access_log.*yyyy-mm-dd*.log


Voor portal **App-instellingen** implementaties:

* Locatie van map: D:\home\LogFiles
* Bestanden van belang:
    * catalina.*yyyy-mm-dd*.log
    * host-manager.*yyyy-mm-dd*.log
    * localhost. *jjjj-mm-dd*.log
    * Manager. *jjjj-mm-dd*.log
    * site_access_log.*yyyy-mm-dd*.log

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a>Hoe kan ik JDBC-stuurprogramma-verbindingsfouten oplossen?

U ziet het volgende bericht weergegeven in de Tomcat-Logboeken:

```
The web application[ROOT] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak,the JDBC Driver has been forcibly unregistered
```

De fout kunt oplossen:

1. Verwijder het bestand sqljdbc*.jar vanuit uw app/lib-map.
2. Als u de aangepaste Tomcat of Azure Marketplace Tomcat-webserver, moet u deze JAR-bestand kopiëren naar de bibliotheekmap van Tomcat.
3. Als u Java vanuit de Azure-portal inschakelen wilt (Selecteer **Java 1.8** > **Tomcat-server**), het sqljdbc.* jar-bestand in de map die parallel aan uw app te kopiëren. Voeg de volgende klassepad naar het web.config-bestand:

    ```xml
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path to the sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-to-copy-live-log-files"></a>Waarom zie ik fouten wanneer ik probeert te kopiëren van live logboekbestanden?

Als u probeert te kopiëren van live logboekbestanden voor een Java-app (bijvoorbeeld Tomcat), ziet u mogelijk deze FTP-fout:

```
Error transferring file [filename] Copying files from remote side failed.
    
The process cannot access the file because it is being used by another process.
```

Het foutbericht kan afwijken, afhankelijk van de FTP-client.

Alle Java-apps hebben deze vergrendelingsprobleem. Alleen Kudu biedt ondersteuning voor dit bestand wordt gedownload terwijl de app wordt uitgevoerd.

De app stoppen, kunt FTP-toegang tot deze bestanden.

Een andere oplossing is het schrijven van een webtaak die volgens een schema wordt uitgevoerd en deze bestanden worden gekopieerd naar een andere map. Zie voor een voorbeeldproject de [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) project.

## <a name="where-do-i-find-the-log-files-for-jetty"></a>Waar vind ik de logboekbestanden voor Jetty?

Voor de Marketplace en aangepaste implementaties is het logboekbestand in de map D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs. Houd er rekening mee dat de maplocatie is afhankelijk van de versie van de Jetty die u gebruikt. Bijvoorbeeld, het pad is opgegeven voor Jetty 9.1.2 als volgt. Zoek naar jetty_*YYYY_MM_DD*. stderrout.log.

Voor implementaties van App-instelling portal is het logboekbestand in D:\home\LogFiles. Zoek naar jetty_*YYYY_MM_DD*. stderrout.log

## <a name="can-i-send-email-from-my-azure-web-app"></a>Kan ik e-mailbericht verzenden vanuit Mijn Azure-web-app?

App Service beschikt niet over een ingebouwde e-functie. Voor enkele goede alternatieven voor het verzenden van e-mail in uw app, ziet deze [Stack Overflow discussie](https://stackoverflow.com/questions/17666161/sending-email-from-azure).

## <a name="why-does-my-wordpress-site-redirect-to-another-url"></a>Waarom mijn WordPress-site naar een andere URL omleiden?

Als u onlangs naar Azure hebt gemigreerd, kan WordPress omleiden naar de oude domein-URL. Dit wordt veroorzaakt door een instelling in de MySQL-database.

WordPress vriend + is een Azure-Site-extensie die u gebruiken kunt om bij te werken van de omleidings-URL rechtstreeks in de database. Zie voor meer informatie over het gebruik van WordPress vriend + [WordPress hulpprogramma's en MySQL-migratie met WordPress vriend +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

U kunt ook als u liever handmatig bijwerken van de omleidings-URL met behulp van SQL-query's of PHPMyAdmin, Zie [WordPress: Omleiden naar de URL van de verkeerde](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a>Hoe kan ik mijn WordPress aanmelden wachtwoord wijzigen?

Als u uw WordPress-aanmelden wachtwoord vergeten bent, kunt u WordPress vriend + bij te werken. Als u wilt uw wachtwoord opnieuw instellen, de WordPress vriend + Azure Site-extensie installeren en voltooi de stappen in [WordPress hulpprogramma's en MySQL-migratie met WordPress vriend +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="i-cant-sign-in-to-wordpress-how-do-i-resolve-this"></a>Ik kan niet aanmelden bij WordPress. Hoe kan ik dit oplossen?

Als u zelf WordPress vergrendeld na de installatie van een invoegtoepassing onlangs hebt gevonden, hebt u mogelijk een defecte invoegtoepassing. WordPress vriend + is een Azure-Site-extensie die u kan helpen uitschakelen invoegtoepassingen in WordPress. Zie voor meer informatie, [WordPress hulpprogramma's en MySQL-migratie met WordPress vriend +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).

## <a name="how-do-i-migrate-my-wordpress-database"></a>Hoe Migreer ik mijn WordPress-database?

U hebt meerdere opties voor het migreren van de MySQL-database die verbonden met uw WordPress-website:

* Ontwikkelaars: Gebruik de [-opdrachtprompt of PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)
* Niet-ontwikkelaars: Gebruik [WordPress vriend +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)

## <a name="how-do-i-help-make-wordpress-more-secure"></a>Hoe u helpen u WordPress beter te beveiligen?

Zie voor meer informatie over aanbevolen beveiligingsprocedures voor WordPress, [aanbevolen procedures voor beveiliging van WordPress in Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).

## <a name="i-am-trying-to-use-phpmyadmin-and-i-see-the-message-access-denied-how-do-i-resolve-this"></a>Ik wil PHPMyAdmin gebruiken en het bericht ziet: "Toegang geweigerd." Hoe kan ik dit oplossen?

U kunt dit probleem kan optreden als de MySQL-functie voor in-app nog niet wordt uitgevoerd in deze App Service-exemplaar. Los het probleem, probeert u toegang tot uw website. Hiermee start u de vereiste processen, met inbegrip van het proces van MySQL in-app. Zorg ervoor dat mysqld.exe wordt vermeld in de processen om te controleren of MySQL in-app wordt uitgevoerd, klikt u in proces Explorer.

Nadat u ervoor dat MySQL zorgen in app wordt uitgevoerd, kunt u proberen te gebruiken PHPMyAdmin.

## <a name="i-get-an-http-403-error-when-i-try-to-import-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a>Ik krijg een HTTP 403-fout op wanneer ik probeer te importeren of exporteren van mijn database voor MySQL in-app via PHPMyadmin. Hoe kan ik dit oplossen?

Als u een oudere versie van Chrome gebruikt, ondervindt u mogelijk een bekend probleem. Upgraden naar een nieuwere versie van Chrome het probleem op te lossen. Probeer ook met een andere browser, zoals Internet Explorer of Microsoft Edge, waar het probleem niet wordt uitgevoerd.

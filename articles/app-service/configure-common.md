---
title: Apps in de portal - Azure App Service configureren
description: Meer informatie over het configureren van algemene instellingen voor een App Service-app in Azure portal.
keywords: Azure appservice, web-app, app-instellingen, omgevingsvariabelen
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: bcc970375120f76e4ec8a90f487d251296f92dba
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957908"
---
# <a name="configure-an-app-service-app-in-the-azure-portal"></a>Een App Service-app in Azure portal configureren

In dit onderwerp wordt uitgelegd hoe u algemene instellingen configureren voor web-apps, mobiele back-end of API-app via de [Azure Portal].

## <a name="configure-app-settings"></a>App-instellingen configureren

In App Service gebruikt u de toepassingsinstellingen in zoals omgevingsvariabelen. In de [Azure Portal], gaat u naar de beheerpagina van uw app. Klik in het linkermenu van de app op **configuratie** > **toepassingsinstellingen**.

![Toepassingsinstellingen](./media/configure-common/open-ui.png)

Voor ontwikkelaars van ASP.NET en ASP.NET Core, zijn instelling app-instellingen in App Service, zoals het instellen van deze `<appSettings>` in *Web.config*, maar de waarden in App Service worden overschreven door de namen in *Web.config*. U kunt instellingen voor het ontwikkelen (bijvoorbeeld lokale MySQL wachtwoord) in houden *Web.config*, maar de productie-geheimen (bijvoorbeeld Azure MySQL-databasewachtwoord) beschermd zijn in App Service. Dezelfde code maakt gebruik van uw instellingen voor het ontwikkelen als u lokaal fouten opsporen, en maakt gebruik van uw productie-geheimen bij de implementatie op Azure.

De app-instellingen als omgevingsvariabelen ophalen andere taal-stacks op dezelfde manier tijdens runtime. Zie voor specifieke stappen taal-stack:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Aangepaste containers](containers/configure-custom-container.md#configure-environment-variables)

App-instellingen worden altijd versleuteld wanneer die zijn opgeslagen (versleuteld in rust).

> [!NOTE]
> App-instellingen kunnen ook worden opgelost vanuit het [Key Vault](/azure/key-vault/) met behulp van [Key Vault verwijst naar](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Verborgen waarde weergeven

Waarden voor app-instellingen zijn standaard verborgen in de portal voor beveiliging. Een verborgen waarde van een app-instelling wilt weergeven, klikt u op de **waarde** veld van de instelling. Voor de waarden van alle app-instellingen, klikt u op de **waarde weergeven** knop.

### <a name="add-or-edit"></a>Toevoegen of bewerken

Als u wilt een nieuw app-instelling toevoegen, klikt u op **nieuwe toepassingsinstelling**. U kunt in het dialoogvenster [gekozen de instelling voor de huidige sleuf](deploy-staging-slots.md#which-settings-are-swapped).

Als u wilt een instelling bewerken, klikt u op de **bewerken** aan de rechterkant.

Wanneer u klaar bent, klikt u op **Update**. Vergeet niet om op **opslaan** terug in de **configuratie** pagina.

> [!NOTE]
> Een standaard Linux-container of een aangepaste Linux-container en alle geneste JSON-sleutelstructuur in de naam van de app-instelling, zoals `ApplicationInsights:InstrumentationKey` moet worden geconfigureerd in App Service als `ApplicationInsights__InstrumentationKey` naam voor de sleutel. Met andere woorden, een `:` moet worden vervangen door `__` (dubbele onderstrepingstekens).
>

### <a name="edit-in-bulk"></a>Bulksgewijs bewerken

Als u wilt toevoegen of bewerken van app-instellingen in bulk, klikt u op de **geavanceerde bewerken** knop. Wanneer u klaar bent, klikt u op **Update**. Vergeet niet om op **opslaan** terug in de **configuratie** pagina.

App-instellingen hebben de volgende JSON-opmaak:

```json
[
  {
    "name": "<key-1>",
    "value": "<value-1>",
    "slotSetting": false
  },
  {
    "name": "<key-2>",
    "value": "<value-2>",
    "slotSetting": false
  },
  ...
]
```

## <a name="configure-connection-strings"></a>Verbindingsreeksen configureren

In de [Azure Portal], gaat u naar de beheerpagina van de app. Klik in het linkermenu van de app op **configuratie** > **toepassingsinstellingen**.

![Toepassingsinstellingen](./media/configure-common/open-ui.png)

Voor ontwikkelaars van ASP.NET en ASP.NET Core, instelling verbindingsreeksen in App Service zijn zoals ze te stellen `<connectionStrings>` in *Web.config*, maar de waarden die u in App Service instelt worden overschreven door de namen in *Web.config*. U kunt instellingen voor het ontwikkelen (bijvoorbeeld een databasebestand) in houden *Web.config* en productie-geheimen (zoals SQL-Database referenties) beschermd zijn in App Service. Dezelfde code maakt gebruik van uw instellingen voor het ontwikkelen als u lokaal fouten opsporen, en maakt gebruik van uw productie-geheimen bij de implementatie op Azure.

Voor andere taal-stacks, is het beter om te gebruiken [app-instellingen](#configure-app-settings) in plaats daarvan omdat verbindingsreeksen vereist speciale opmaak in de variabele sleutels in volgorde van voor toegang tot de waarden. Hier echter is één uitzondering: bepaalde databasetypen Azure-back-ups, samen met de app als u de verbindingsreeksen in uw app configureren. Zie voor meer informatie, [wat wordt een back-up](manage-backup.md#what-gets-backed-up). Als u kunt deze automatische back-up niet nodig hebt, gebruikt u app-instellingen.

Tijdens runtime zijn tekenreeksen voor databaseverbindingen beschikbaar als omgevingsvariabelen, voorafgegaan door de volgende verbindingstypen:

* SQL Server: `SQLCONNSTR_`
* MySQL: `MYSQLCONNSTR_`
* SQL Database: `SQLAZURECONNSTR_`
* Aangepast: `CUSTOMCONNSTR_`

Bijvoorbeeld een MySql-verbindingsreeks met de naam *connectionstring1* kunnen worden geopend als de omgevingsvariabele `MYSQLCONNSTR_connectionString1`. Zie voor specifieke stappen taal-stack:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Aangepaste containers](containers/configure-custom-container.md#configure-environment-variables)

Verbindingsreeksen worden altijd versleuteld wanneer die zijn opgeslagen (versleuteld in rust).

> [!NOTE]
> Tekenreeksen voor databaseverbindingen, kunnen ook worden opgelost van [Key Vault](/azure/key-vault/) met behulp van [Key Vault verwijst naar](app-service-key-vault-references.md).

### <a name="show-hidden-values"></a>Verborgen waarde weergeven

Standaard zijn waarden voor verbindingsreeksen verborgen in de portal voor beveiliging. Een verborgen waarde van een verbindingsreeks wilt weergeven, klikt u op de **waarde** veld van de reeks. Voor de waarden van alle tekenreeksen voor databaseverbindingen, klikt u op de **waarde weergeven** knop.

### <a name="add-or-edit"></a>Toevoegen of bewerken

Als u wilt een nieuwe verbindingsreeks toevoegen, klikt u op **nieuwe verbindingsreeks**. U kunt in het dialoogvenster [gekozen de verbindingsreeks voor de huidige sleuf](deploy-staging-slots.md#which-settings-are-swapped).

Als u wilt een instelling bewerken, klikt u op de **bewerken** aan de rechterkant.

Wanneer u klaar bent, klikt u op **Update**. Vergeet niet om op **opslaan** terug in de **configuratie** pagina.

### <a name="edit-in-bulk"></a>Bulksgewijs bewerken

Als u wilt toevoegen of bewerken van verbindingsreeksen in bulk, klikt u op de **geavanceerde bewerken** knop. Wanneer u klaar bent, klikt u op **Update**. Vergeet niet om op **opslaan** terug in de **configuratie** pagina.

Tekenreeksen voor databaseverbindingen hebben de volgende JSON-opmaak:

```json
[
  {
    "name": "name-1",
    "value": "conn-string-1",
    "type": "SQLServer",
    "slotSetting": false
  },
  {
    "name": "name-2",
    "value": "conn-string-2",
    "type": "PostgreSQL",
    "slotSetting": false
  },
  ...
]
```

<a name="platform"></a>
<a name="alwayson"></a>

## <a name="configure-general-settings"></a>Algemene instellingen configureren

In de [Azure Portal], gaat u naar de beheerpagina van de app. Klik in het linkermenu van de app op **configuratie** > **toepassingsinstellingen**.

![Algemene instellingen](./media/configure-common/open-general.png)

Hier kunt kunt u enkele algemene instellingen voor de app configureren. Sommige instellingen moeten u [omhoog schalen naar een hogere prijscategorie](web-sites-scale.md).

- **Stack-instellingen**: De softwarestack om uit te voeren de app, met inbegrip van de taal en de SDK-versies. Voor Linux-apps en aangepaste container-apps, kunt u ook een optionele opstartopdracht of bestand instellen.
- **Platforminstellingen**: Hiermee kunt u instellingen configureren voor de host-platform, met inbegrip van:
    - **Bitness**: 32-bits of 64-bits.
    - **WebSocket-protocol**: Voor [ASP.NET SignalR] of [socket.io](https://socket.io/), bijvoorbeeld.
    - **Always On**: Houd de app geladen, zelfs wanneer er geen verkeer. U moet deze inschakelen voor doorlopende webtaken of voor webtaken die worden geactiveerd met behulp van een CRON-expressie.
    - **Beheerde pipeline-versie**: De IIS [pipeline-modus]. Stel deze in op **klassieke** als u hebt een verouderde app waarvoor een oudere versie van IIS.
    - **HTTP-versie**: Ingesteld op **2.0** ondersteuning voor inschakelen [HTTPS/2](https://wikipedia.org/wiki/HTTP/2) protocol.
    > [!NOTE]
    > De meeste moderne browsers ondersteunen HTTP/2-protocol via TLS, terwijl niet-versleuteld verkeer gaat door met HTTP/1.1. Om ervoor te zorgen dat de client browsers verbinding maken met uw app met HTTP/2, ofwel [kopen van een App Service Certificate](web-sites-purchase-ssl-web-site.md) voor het aangepaste domein van de app of [binden van een certificaat van derden](app-service-web-tutorial-custom-ssl.md).
    - **ARR-affiniteit**: Zorg ervoor dat de client wordt doorgestuurd naar hetzelfde exemplaar voor de levensduur van de sessie in een implementatie met meerdere instanties. U kunt deze optie instelt op **uit** voor stateless toepassingen.
- **Foutopsporing**: Schakel foutopsporing op afstand voor [ASP.NET](troubleshoot-dotnet-visual-studio.md#remotedebug), [ASP.NET Core](/visualstudio/debugger/remote-debugging-azure), of [Node.js](containers/configure-language-nodejs.md#debug-remotely) apps. Deze optie wordt uitgeschakeld na 48 uur automatisch.
- **Inkomende clientcertificaten**: clientcertificaten in vereisen [wederzijdse verificatie](app-service-web-configure-tls-mutual-auth.md).

## <a name="configure-default-documents"></a>Standaarddocumenten configureren

Deze instelling geldt alleen voor Windows-apps.

In de [Azure Portal], gaat u naar de beheerpagina van de app. Klik in het linkermenu van de app op **configuratie** > **documenten standaard**.

![Algemene instellingen](./media/configure-common/open-documents.png)

Het standaarddocument is de webpagina die wordt weergegeven op de basis-URL voor een website. De eerste overeenkomende bestand in de lijst wordt gebruikt. Als u wilt een nieuw standaarddocument toevoegen, klikt u op **nieuw document**. Vergeet niet om op **opslaan**.

Als de app gebruikmaakt van modules die route op basis van URL in plaats van het leveren van statische inhoud, is er niet nodig voor standaarddocumenten.

## <a name="configure-path-mappings"></a>Pad-toewijzingen configureren

In de [Azure Portal], gaat u naar de beheerpagina van de app. Klik in het linkermenu van de app op **configuratie** > **pad toewijzingen**.

![Algemene instellingen](./media/configure-common/open-path.png)

De **pad toewijzingen** pagina ziet u andere dingen die op basis van het type besturingssysteem.

### <a name="windows-apps-uncontainerized"></a>Windows-apps (uncontainerized)

Voor Windows-apps, kunt u de IIS-Handlertoewijzingen en virtuele toepassingen en mappen kunt aanpassen.

Handlertoewijzingen kunnen u de processors aangepast script voor het afhandelen van aanvragen voor specifieke bestandsextensies toevoegen. Als u wilt een aangepaste handler toevoegen, klikt u op **nieuwe handler**. De handler als volgt configureren:

- **Extensie**. De bestandsextensie die u verwerken wilt, zoals  *\*.php* of *handler.fcgi*.
- **ScriptProcessor**. Het absolute pad van de ScriptProcessor aan u. Aanvragen voor bestanden die overeenkomen met de bestandsextensie worden verwerkt door de ScriptProcessor. Het pad gebruiken `D:\home\site\wwwroot` om te verwijzen naar de hoofdmap van uw app.
- **Arguments**. Optionele opdrachtregelargumenten voor de ScriptProcessor.

Elke app heeft het standaardpad naar de hoofdmap (`/`) toegewezen aan `D:\home\site\wwwroot`, waarop uw code standaard wordt geïmplementeerd. Als de hoofdmap van uw app zich in een andere map, of als uw opslagplaats meer dan één toepassing heeft, kunt u bewerken of toevoegen van virtuele toepassingen en mappen hier. Klik op **nieuwe virtuele toepassing of map**.

Voor het configureren van virtuele toepassingen en mappen, het opgeven van elke virtuele map en de bijbehorende fysieke pad ten opzichte van hoofdmap van de website (`D:\home`). Desgewenst kunt u de **toepassing** selectievakje in om u te markeren van een virtuele map als een toepassing.

### <a name="containerized-apps"></a>Apps in containers

U kunt [aangepaste opslag toevoegen aan uw container geplaatste app](containers/how-to-serve-content-from-azure-storage.md). Apps in containers zijn alle Linux-apps en ook de Windows- en Linux aangepaste containers die worden uitgevoerd in App Service. Klik op **nieuwe Azure-opslag koppelen** en configureer de aangepaste opslag als volgt:

- **Naam**: De weergavenaam.
- **Configuratie-opties**: **Basic** of **geavanceerde**.
- **Storage-accounts**: Het opslagaccount met de container.
- **Opslagtype**: **Azure-Blobs** of **Azure Files**.
  > [!NOTE]
  > Windows-container-apps alleen ondersteuning voor Azure Files.
- **Storage-container**: Voor de basisconfiguratie, de container die u wilt.
- **Sharenaam**: Voor geavanceerde configuratie, is het bestand sharenaam.
- **Toegangssleutel**: Voor geavanceerde configuratie, de toegangssleutel.
- **Koppelpad**: Het absolute pad in de container te koppelen van de aangepaste opslag.

Zie voor meer informatie, [inhoud leveren vanuit Azure Storage in App Service on Linux](containers/how-to-serve-content-from-azure-storage.md).

## <a name="configure-language-stack-settings"></a>Taal stack-instellingen configureren

Zie voor Linux-apps:

- [ASP.NET Core](containers/configure-language-dotnetcore.md)
- [Node.js](containers/configure-language-nodejs.md)
- [PHP](containers/configure-language-php.md)
- [Python](containers/how-to-configure-python.md)
- [Java](containers/configure-language-java.md)
- [Ruby](containers/configure-language-ruby.md)

## <a name="configure-custom-containers"></a>Aangepaste containers configureren

Zie [een aangepaste Linux-container configureren voor Azure App Service](containers/configure-custom-container.md)

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste domeinnaam configureren in Azure App Service]
- [Faseringsomgevingen in Azure App Service instellen]
- [HTTPS inschakelen voor een app in Azure App Service]
- [Logboeken met diagnostische gegevens inschakelen](troubleshoot-diagnostic-logs.md)
- [Een app schalen in Azure App Service]
- [Controle-bewerkingen in Azure App Service]
- [Instellingen voor applicationHost.config met applicationHost.xdt wijzigen](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Een aangepaste domeinnaam configureren in Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[Faseringsomgevingen in Azure App Service instellen]: ./deploy-staging-slots.md
[HTTPS inschakelen voor een app in Azure App Service]: ./app-service-web-tutorial-custom-ssl.md
[How to: Monitor web endpoint status]: https://go.microsoft.com/fwLink/?LinkID=279906
[Controle-bewerkingen in Azure App Service]: ./web-sites-monitor.md
[pipeline-modus]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Een app schalen in Azure App Service]: ./web-sites-scale.md

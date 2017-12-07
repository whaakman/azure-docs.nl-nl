---
title: Logboekregistratie van diagnostische gegevens van web-apps in Azure App Service
description: Meer informatie over het vastleggen van diagnostische gegevens inschakelen en instrumentation toevoegen aan uw toepassing, evenals over toegang tot de informatie die wordt geregistreerd door Azure.
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.openlocfilehash: a5ac6c02e28c19346abae9e5ea3dba9af4022dde
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Logboekregistratie van diagnostische gegevens van web-apps in Azure App Service
## <a name="overview"></a>Overzicht
Azure biedt ingebouwde diagnostische gegevens om te helpen bij foutopsporing een [App Service-web-app](http://go.microsoft.com/fwlink/?LinkId=529714). In dit artikel leert u hoe diagnostische logboekregistratie inschakelen en instrumentation toevoegen aan uw toepassing, evenals over toegang tot de informatie die wordt geregistreerd door Azure.

Dit artikel wordt de [Azure-portal](https://portal.azure.com), Azure PowerShell en de Azure-opdrachtregelinterface (Azure CLI) werken met Logboeken met diagnostische gegevens. Zie voor meer informatie over het werken met Logboeken met diagnostische gegevens met Visual Studio [probleemoplossing voor Azure in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Web server diagnostics en application diagnostics
App Service-web-apps bieden diagnostische functionaliteit voor logboekregistratie informatie van de webserver en de webtoepassing. Deze logisch zijn verdeeld in **web server diagnostische** en **application diagnostics**.

### <a name="web-server-diagnostics"></a>Web server diagnostische gegevens
U kunt in- of uitschakelen van de volgende soorten logboeken:

* **Gedetailleerde fout logboekregistratie** -gedetailleerde informatie over de fout voor HTTP-statuscodes die wijzen op mislukte (statuscode 400 of hoger). Deze kan informatie om te bepalen waarom de server heeft geretourneerd met de foutcode bevatten.
* **Kan geen aanvraag voor het traceren** -gedetailleerde informatie over de mislukte aanvragen, met inbegrip van een tracering van de IIS-onderdelen gebruikt voor het verwerken van de aanvraag en de tijd die in elk onderdeel. Dit is handig als u probeert te verhogen van de prestaties van de site of isoleren wat de oorzaak van een specifieke HTTP-fout wordt geretourneerd.
* **Web Server-logboekregistratie** -informatie over HTTP transacties met behulp van de [uitgebreide W3C-logboekbestandsindeling](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Dit is handig bij het bepalen van de algemene site metrische gegevens zoals het aantal aanvragen dat is verwerkt of hoeveel aanvragen van een specifiek IP-adres.

### <a name="application-diagnostics"></a>Application diagnostics
Application diagnostics kunt u informatie die wordt geproduceerd door een webtoepassing vastleggen. ASP.NET-toepassingen kunnen gebruikmaken van de [System.Diagnostics.Trace](http://msdn.microsoft.com/library/36hhw2t6.aspx) klasse om informatie te registreren in het toepassingslogboek van de diagnostische gegevens. Bijvoorbeeld:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

U kunt deze logboeken om te helpen bij het oplossen van problemen ophalen tijdens runtime. Zie voor meer informatie [probleemoplossing voor Azure-web-apps in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

App Service WebApps zich ook aanmelden voor informatie over de implementatie, wanneer u inhoud naar een web-app publiceert. Dit gebeurt automatisch en er zijn geen configuratieinstellingen voor logboekregistratie in een implementatie. Logboekregistratie van de implementatie kunt u om te bepalen waarom een implementatie is mislukt. Als u een aangepaste implementatiescript gebruikt, kunt u implementatie logboekregistratie gebruiken om te bepalen waarom het script is mislukt.

## <a name="enablediag"></a>Hoe diagnostische gegevens inschakelen
Inschakelen van diagnostische gegevens in de [Azure-portal](https://portal.azure.com), gaat u naar de pagina voor uw web-app en klik op **instellingen > diagnostische logboeken**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Logboeken-onderdeel](./media/web-sites-enable-diagnostic-log/logspart.png)

Wanneer u het inschakelen van **application diagnostics**, u er ook voor kiezen de **niveau**. Deze instelling kunt u de gegevens die zijn vastgelegd op wilt filteren **informatief**, **waarschuwing**, of **fout** informatie. Instellen op **uitgebreide** registreert alle informatie die wordt geproduceerd door de toepassing.

> [!NOTE]
> In tegenstelling tot het bestand web.config te wijzigen, komt Application diagnostics inschakelen of wijzigen van diagnostische logboekniveaus niet gerecycled het app-domein dat de toepassing wordt uitgevoerd in.
>
>

Voor **toepassingslogboeken**, kunt u de optie file system tijdelijk voor foutopsporing inschakelen. Deze optie schakelt automatisch op 12 uur. U kunt ook de optie blob-opslag om te selecteren van een blobcontainer in Logboeken om te schrijven inschakelen.

Voor **Web server-logboekregistratie**, kunt u **opslag** of **bestandssysteem**. Selecteren **opslag** kunt u een opslagaccount en een blob-container die de logboeken worden geschreven om te selecteren. 

Als u Logboeken op het bestandssysteem opslaat, worden de bestanden geopend door FTP of gedownload als een Zip-archief met behulp van de Azure PowerShell of Azure-opdrachtregelinterface (Azure CLI).

Standaard logboeken worden niet automatisch verwijderd (met uitzondering van **toepassingslogboeken (bestandssysteem)**). Voor het automatisch verwijderen van Logboeken, stel de **bewaartermijn (dagen)** veld.

> [!NOTE]
> Als u [toegangssleutels voor uw opslagaccount opnieuw genereren](../storage/common/storage-create-storage-account.md), moet u de configuratie van de respectieve logboekregistratie voor het gebruik van de bijgewerkte sleutels opnieuw instellen. Om dit te doen:
>
> 1. In de **configureren** tabblad, stelt u de functie voor logboekregistratie respectieve op **uit**. Sla de instelling.
> 2. Logboekregistratie op de storage-account blob of de tabel opnieuw. Sla de instelling.
>
>

Een combinatie van het bestandssysteem, table storage of blob-opslag kan worden ingeschakeld op hetzelfde moment en afzonderlijk logboek-niveau configuraties hebben. Mogelijk wilt u melden fouten en waarschuwingen naar de blob storage als lange termijn logboekregistratie oplossing tijdens het inschakelen van logboekregistratie in systeem met een uitgebreide machtigingsniveau.

Terwijl alle drie opslaglocaties de dezelfde basisgegevens voor vastgelegde gebeurtenissen bieden **tabel opslag** en **blobopslag** Meld u aanvullende informatie zoals de exemplaar-ID, thread-ID en een meer gedetailleerd tijdstempel (maatstreepjes-indeling) dan de logboekregistratie naar **bestandssysteem**.

> [!NOTE]
> Gegevens die zijn opgeslagen **tabel opslag** of **blobopslag** alleen toegankelijk is met een opslag-client of een toepassing die rechtstreeks met deze opslagsystemen kunt werken. Bijvoorbeeld, Visual Studio 2013 bevat een Opslagverkenner die kunnen worden gebruikt om te verkennen tabel of blob storage en HDInsight toegang tot de gegevens die zijn opgeslagen in blob storage. U kunt ook schrijven met een toepassing die toegang heeft tot Azure Storage via een van de [Azure SDK's](/downloads/#).
>
> [!NOTE]
> Diagnostische gegevens kunnen ook worden ingeschakeld vanuit Azure PowerShell met behulp van de **Set AzureWebsite** cmdlet. Als u Azure PowerShell nog niet hebt geïnstalleerd, of het gebruik van uw Azure-abonnement niet hebt geconfigureerd, Zie [hoe Azure PowerShell gebruiken](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

## <a name="download"></a>Hoe: Logboeken downloaden
Diagnostische gegevens die zijn opgeslagen in het bestandssysteem van de web-app toegankelijk zijn rechtstreeks met FTP. Het kan ook worden gedownload als een Zip-archief met Azure PowerShell of Azure-opdrachtregelinterface.

De mapstructuur die de logboeken worden opgeslagen in is als volgt:

* **Toepassingslogboeken** -/LogFiles/toepassing /. Deze map bevat een of meer tekstbestanden met informatie die wordt geproduceerd door logboekregistratie van toepassingen.
* **Kan aanvraag traceringen** -logboekbestanden/W3SVC ### /. Deze map bevat een XSL-bestand en een of meer XML-bestanden. Zorg ervoor dat het XSL-bestand te downloaden in dezelfde map als het XML-bestand nodig omdat het XSL-bestand functionaliteit biedt voor het formatteren en het filteren van de inhoud van de XML-bestanden ziet er in Internet Explorer.
* **Gedetailleerde foutenlogboeken** -/LogFiles/DetailedErrors /. Deze map bevat een of meer htm-bestanden met uitgebreide informatie voor een HTTP-fouten dat is opgetreden.
* **Web Server-logboeken** -/LogFiles/http/RawLogs. Deze map bevat een of meer tekstbestanden geformatteerd met het [uitgebreide W3C-logboekbestandsindeling](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Implementatielogboeken** -logboekbestanden/Git. Deze map bevat de logboeken die worden gegenereerd door de van interne implementatieprocessen die worden gebruikt door de Azure-web-apps, evenals de logboeken voor Git-implementaties.

### <a name="ftp"></a>FTP

Zie het openen van een FTP-verbinding met uw app FTP-server [uw app implementeren in Azure App Service met behulp van FTP/S](app-service-deploy-ftp.md).

Zodra de verbinding met uw web-app FTP-/ S-server, opent u de **logboekbestanden** map, waarin de logboekbestanden worden opgeslagen.

### <a name="download-with-azure-powershell"></a>Met Azure PowerShell downloaden
Downloaden van de logboekbestanden, start een nieuw exemplaar van Azure PowerShell en gebruik de volgende opdracht:

    Save-AzureWebSiteLog -Name webappname

Deze opdracht slaat u de logboeken voor de web-app die is opgegeven door de **-naam** parameter naar een bestand met de naam **logs.zip** in de huidige map.

> [!NOTE]
> Als u Azure PowerShell nog niet hebt geïnstalleerd, of het gebruik van uw Azure-abonnement niet hebt geconfigureerd, Zie [hoe Azure PowerShell gebruiken](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="download-with-azure-command-line-interface"></a>Met Azure-opdrachtregelinterface downloaden
De logboekbestanden met de Azure-opdrachtregelinterface downloaden, opent u een nieuwe opdrachtprompt, PowerShell, Bash of terminalsessie en voer de volgende opdracht:

    azure site log download webappname

Deze opdracht slaat u de logboeken voor de web-app met de naam 'webappname' naar een bestand met de naam **diagnostics.zip** in de huidige map.

> [!NOTE]
> Als u de Azure-opdrachtregelinterface (Azure CLI) nog niet hebt geïnstalleerd of niet via uw Azure-abonnement hebt geconfigureerd, Zie [hoe Azure CLI gebruiken](../cli-install-nodejs.md).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Hoe: weergave wordt geregistreerd in Application Insights
Visual Studio Application Insights biedt hulpprogramma's voor het filteren en zoeken van Logboeken en de logboeken correleren met aanvragen en andere gebeurtenissen.

1. De Application Insights-SDK toevoegen aan uw project in Visual Studio.
   * Klik in Solution Explorer met de rechtermuisknop op uw project en kies Application Insights toevoegen. De interface leidt u door de stappen die bestaan uit het maken van een Application Insights-resource. [Meer informatie](../application-insights/app-insights-asp-net.md)
2. Het pakket traceringslistener toevoegen aan uw project.
   * Met de rechtermuisknop op uw project en kies NuGet-pakketten beheren. Selecteer `Microsoft.ApplicationInsights.TraceListener` [meer informatie](../application-insights/app-insights-asp-net-trace-logs.md)
3. Uploaden van uw project en voer dit logboekgegevens moeten worden gegenereerd.
4. In de [Azure-portal](https://portal.azure.com/), blader naar de nieuwe Application Insights-resource en open **Search**. U ziet uw logboekgegevens, samen met de aanvraag, het gebruik en andere telemetrie. Telemetrie kan even duren om te komen: klik op vernieuwen. [Meer informatie](../application-insights/app-insights-diagnostic-search.md)

[Meer informatie over prestaties bijhouden met Application Insights](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a>Hoe: Logboeken Stream
Tijdens het ontwikkelen van een toepassing, is het vaak nuttig om gegevens in bijna realtime logboekregistratie te bekijken. U kunt informatie logboekregistratie op uw ontwikkelomgeving met behulp van Azure PowerShell of Azure-opdrachtregelinterface streamen.

> [!NOTE]
> Bepaalde typen logboekregistratie buffer schrijven naar het logboekbestand, in volgorde van gebeurtenissen in de stroom resulteren kan. Een toepassing logboekvermelding die deze gebeurtenis treedt op wanneer een gebruiker een pagina bezoekt kan bijvoorbeeld worden weergegeven in de stroom voordat de bijbehorende HTTP logboekvermelding voor de pagina-aanvraag.
>
> [!NOTE]
> Streaming-logboek ook-streams gegevens geschreven naar een tekstbestand dat is opgeslagen in de **D:\\thuis\\logboekbestanden\\**  map.
>
>

### <a name="streaming-with-azure-powershell"></a>Streamen met Azure PowerShell
Stream logboekinformatie, start een nieuw exemplaar van Azure PowerShell en gebruik de volgende opdracht:

    Get-AzureWebSiteLog -Name webappname -Tail

Met deze opdracht maakt verbinding met de web-app die is opgegeven door de **-naam** parameter en beginnen met het streaming-informatie naar de PowerShell-venster als logboekgebeurtenissen op de web-app optreden. Alle informatie die is geschreven naar eindigt op .txt, .log of htm-bestanden die zijn opgeslagen in de map /LogFiles (home-d:/logboekbestanden) is gestreamd naar de lokale console.

Om te filteren op specifieke gebeurtenissen, zoals fouten, gebruiken de **-bericht** parameter. Bijvoorbeeld:

    Get-AzureWebSiteLog -Name webappname -Tail -Message Error

Gebruiken om te filteren op specifieke logboek typen, zoals HTTP, de **-pad** parameter. Bijvoorbeeld:

    Get-AzureWebSiteLog -Name webappname -Tail -Path http

Als een lijst met beschikbare paden weergeven, gebruikt u de parameter - ListPath.

> [!NOTE]
> Als u Azure PowerShell nog niet hebt geïnstalleerd, of het gebruik van uw Azure-abonnement niet hebt geconfigureerd, Zie [hoe Azure PowerShell gebruiken](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="streaming-with-azure-command-line-interface"></a>Streamen met Azure-opdrachtregelinterface
Om te streamen logboekinformatie, opent u een nieuwe opdrachtprompt, PowerShell, Bash of terminalsessie en voer de volgende opdracht:

    az webapp log tail --name webappname --resource-group myResourceGroup

Deze opdracht maakt verbinding met de web-app met de naam 'webappname' en beginnen met de informatie in het venster streaming zoals gebeurtenissen op de web-app plaatsvinden. Alle informatie die is geschreven naar eindigt op .txt, .log of htm-bestanden die zijn opgeslagen in de map /LogFiles (home-d:/logboekbestanden) is gestreamd naar de lokale console.

Om te filteren op specifieke gebeurtenissen, zoals fouten, gebruiken de **--Filter** parameter. Bijvoorbeeld:

    az webapp log tail --name webappname --resource-group myResourceGroup --filter Error

Gebruiken om te filteren op specifieke logboek typen, zoals HTTP, de **--pad** parameter. Bijvoorbeeld:

    az webapp log tail --name webappname --resource-group myResourceGroup --path http

> [!NOTE]
> Als u de Azure-opdrachtregelinterface niet hebt geïnstalleerd of niet via uw Azure-abonnement hebt geconfigureerd, Zie [hoe voor gebruik van Azure-opdrachtregelinterface](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a>Hoe: inzicht in Logboeken met diagnostische gegevens
### <a name="application-diagnostics-logs"></a>Diagnostische logboeken
Application diagnostics wordt informatie opgeslagen in een specifieke indeling voor .NET-toepassingen, afhankelijk van of het opslaan van logboeken naar het bestandssysteem, table storage of blob-opslag. De basisset van opgeslagen gegevens is hetzelfde voor alle drie opslagtypen - de datum en tijd die de gebeurtenis heeft plaatsgevonden, de proces-ID die de gebeurtenis, het gebeurtenistype (informatie, waarschuwing, fout) en het gebeurtenisbericht geproduceerd.

**Bestandssysteem**

Elke regel geregistreerd in het bestandssysteem of ontvangen met streaming is in de volgende indeling:

    {Date}  PID[{process ID}] {event type/level} {message}

Zo zou een foutgebeurtenis vergelijkbaar met het volgende voorbeeld weergegeven:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

Logboekregistratie naar het bestandssysteem biedt de meest elementaire informatie van de drie beschikbare methoden bieden alleen de tijd, proces-ID, gebeurtenisniveau en bericht.

**Table Storage**

Bij het aanmelden naar table storage, worden aanvullende eigenschappen gebruikt ter bevordering van de gegevens die zijn opgeslagen in de tabel evenals meer gedetailleerde informatie over de gebeurtenis te zoeken. De volgende eigenschappen (kolommen) worden gebruikt voor elke entiteit (rij) opgeslagen in de tabel.

| Naam van eigenschap | Waarde/indeling |
| --- | --- |
| PartitionKey |Datum/tijd van de gebeurtenis in de indeling yyyyMMddHH |
| RowKey |Een GUID-waarde die is uniek voor deze entiteit |
| Timestamp |De datum en tijd waarop de gebeurtenis heeft plaatsgevonden |
| EventTickCount |De datum en tijd waarop de gebeurtenis heeft plaatsgevonden, in de indeling van de maatstreepjes (nauwkeuriger) |
| ApplicationName |De naam van de web-app |
| Niveau |Gebeurtenisniveau (bijvoorbeeld fout, waarschuwing, informatie) |
| Gebeurtenis-id |De gebeurtenis-ID van deze gebeurtenis<p><p>De standaardwaarde is 0 als er geen opgegeven |
| Instantie-id |Exemplaar van de web-app die de gebeurtenis is opgetreden op |
| PID |Proces-id |
| TID |De thread-ID van de thread die de gebeurtenis geproduceerd |
| Bericht |Detail gebeurtenisbericht |

**Blob Storage**

Tijdens de registratie naar de blob storage, kunnen gegevens worden opgeslagen in de indeling met door komma's gescheiden waarden (CSV). Vergelijkbare naar table storage aanvullende velden om meer gedetailleerde informatie over de gebeurtenis worden vastgelegd. De volgende eigenschappen worden voor elke rij in de CSV gebruikt:

| Naam van eigenschap | Waarde/indeling |
| --- | --- |
| Date |De datum en tijd waarop de gebeurtenis heeft plaatsgevonden |
| Niveau |Gebeurtenisniveau (bijvoorbeeld fout, waarschuwing, informatie) |
| ApplicationName |De naam van de web-app |
| Instantie-id |Exemplaar van de web-app die de gebeurtenis is opgetreden op |
| EventTickCount |De datum en tijd waarop de gebeurtenis heeft plaatsgevonden, in de indeling van de maatstreepjes (nauwkeuriger) |
| Gebeurtenis-id |De gebeurtenis-ID van deze gebeurtenis<p><p>De standaardwaarde is 0 als er geen opgegeven |
| PID |Proces-id |
| TID |De thread-ID van de thread die de gebeurtenis geproduceerd |
| Bericht |Detail gebeurtenisbericht |

De gegevens die zijn opgeslagen in een blob eruit als in het volgende voorbeeld:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> De eerste regel van het logboek bevat de kolomkoppen, zoals wordt weergegeven in dit voorbeeld.
>
>

### <a name="failed-request-traces"></a>Aanvraag traceringen is mislukt
Traceringen van mislukte aanvragen zijn opgeslagen in XML-bestanden met de naam **fr ### .xml**. Als u wilt zodat het eenvoudiger om de geregistreerde gegevens weer te geven, een XSL-opmaakmodel met de naam **freb.xsl** vindt u in dezelfde map als de XML-bestanden. Als u een van de XML-bestanden in Internet Explorer openen, worden in Internet Explorer het XSL-opmaakmodel gebruikt voor het bieden van een opgemaakte weergave van de traceergegevens, vergelijkbaar met het volgende voorbeeld:

![mislukte aanvragen in de browser weergegeven](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Gedetailleerde foutenlogboeken
Gedetailleerde foutenlogboeken zijn HTML-documenten met meer gedetailleerde informatie over HTTP-fouten dat is opgetreden. Aangezien ze gewoon HTML-documenten, kunnen ze worden weergegeven met een webbrowser.

### <a name="web-server-logs"></a>Webserverlogboeken
De web server-logboeken zijn geformatteerd met het [uitgebreide W3C-logboekbestandsindeling](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Deze informatie kan worden gelezen met een teksteditor of met behulp van de hulpprogramma's zoals geparseerd [Log Parser](http://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> De logboeken die wordt geproduceerd door de Azure-web-apps bieden geen ondersteuning voor de **s-computername**, **s-IP-**, of **cs-version** velden.
>
>

## <a name="nextsteps"></a> Volgende stappen
* [Web-Apps bewaken](web-sites-monitor.md)
* [Het Azure-web-apps in Visual Studio oplossen](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analyseren web-Applogboeken in HDInsight](http://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
>
>

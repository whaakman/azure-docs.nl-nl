---
title: Diagnostische logboekregistratie inschakelen voor web-apps in Azure App Service
description: Meer informatie over hoe u Diagnostische logboekregistratie inschakelen en instrumentatie kunt toevoegen aan uw toepassing, evenals hoe u toegang tot de gegevens die zijn vastgelegd door Azure.
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
ms.openlocfilehash: 8a58f8722b41944a7be02254e0f00682575c1bbb
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636955"
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Diagnostische logboekregistratie inschakelen voor web-apps in Azure App Service
## <a name="overview"></a>Overzicht
Azure bevat ingebouwde diagnosefuncties voor de ondersteuning bij foutopsporing in een [App Service-web-app](https://go.microsoft.com/fwlink/?LinkId=529714). In dit artikel leert u hoe u Diagnostische logboekregistratie inschakelen en instrumentatie kunt toevoegen aan uw toepassing, evenals hoe u toegang tot de gegevens die zijn vastgelegd door Azure.

In dit artikel wordt de [Azure-portal](https://portal.azure.com) en Azure CLI om te werken met diagnostische logboeken. Zie voor meer informatie over het werken met diagnostische logboeken met behulp van Visual Studio [probleemoplossing voor Azure in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Web server diagnostics en application diagnostics
App Service WebApps bieden diagnosefunctionaliteit voor waardevolle informatie uit de webserver en de web-App. Deze logisch zijn gescheiden in **web server diagnostische** en **toepassingsdiagnose**.

### <a name="web-server-diagnostics"></a>Web server diagnostische gegevens
U kunt inschakelen of uitschakelen van de volgende soorten logboeken:

* **Gedetailleerde fout logboekregistratie** -gedetailleerde foutinformatie voor HTTP-statuscodes die duiden op een fout (statuscode 400 of hoger). Deze kan informatie om te bepalen waarom de server heeft geretourneerd met de foutcode bevatten.
* **Kan geen aanvraag tracering** -gedetailleerde informatie over mislukte aanvragen, met inbegrip van een tracering van de IIS-componenten gebruikt voor het verwerken van de aanvraag en de tijd die in elk onderdeel. Dit is handig als u probeert te verhogen van de prestaties van de site of isolatie van wat de oorzaak is van een specifieke HTTP-fout worden geretourneerd.
* **Web Server-logboekregistratie** -informatie over HTTP-transacties met behulp van de [uitgebreide W3C-logboekbestandsindeling](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Dit is handig bij het bepalen van de algemene metrische sitegegevens, zoals het aantal aanvragen dat is verwerkt of het aantal aanvragen afkomstig zijn van een specifiek IP-adres.

### <a name="application-diagnostics"></a>Application diagnostics
Application diagnostics kunt u voor het vastleggen van gegevens die worden geproduceerd door een web-App. ASP.NET-toepassingen kunnen gebruikmaken van de [System.Diagnostics.Trace](https://msdn.microsoft.com/library/36hhw2t6.aspx) klasse om informatie te registreren in het toepassingslogboek van diagnostische gegevens. Bijvoorbeeld:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Tijdens runtime, kunt u deze logboeken om te helpen bij het oplossen van problemen ophalen. Zie voor meer informatie, [probleemoplossing voor Azure WebApps in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

App Service WebApps zich ook aanmelden voor informatie over de implementatie, wanneer u inhoud naar een web-app publiceert. Dit gebeurt automatisch en er zijn geen configuratieinstellingen voor de implementatie van logboekregistratie. Logboekregistratie van de implementatie kunt u om te bepalen waarom een implementatie is mislukt. Bijvoorbeeld, als u een aangepaste implementatiescript gebruikt, kunt u implementatie logboekregistratie om te bepalen waarom het script is mislukt.

## <a name="enablediag"></a>Diagnostische gegevens inschakelen
Om in te schakelen van diagnostische gegevens in de [Azure-portal](https://portal.azure.com), gaat u naar de pagina voor uw web-app en klikt u op **instellingen > Logboeken met diagnostische gegevens**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Logboeken-onderdeel](./media/web-sites-enable-diagnostic-log/logspart.png)

Als u activeert **toepassingsdiagnose**, u er ook voor kiezen de **niveau**. Deze instelling kunt u voor het filteren van de gegevens die zijn vastgelegd op **informatief**, **waarschuwing**, of **fout** informatie. Instellen op **uitgebreide** registreert alle informatie die wordt geproduceerd door de toepassing.

> [!NOTE]
> In tegenstelling tot het wijzigen van het bestand web.config, worden Application diagnostics inschakelen of wijzigen van niveaus van diagnostische logboeken niet recyclen het app-domein dat de toepassing wordt uitgevoerd in.
>
>

Voor **toepassingslogboeken**, kunt u de optie voor het systeem van bestand tijdelijk voor foutopsporing inschakelen. Deze optie nu schakelt automatisch in 12 uur. U kunt ook inschakelen op de optie blob storage een blobcontainer om te schrijven Logboeken om te selecteren.

Voor **webserverlogboeken**, kunt u **opslag** of **bestandssysteem**. Selecteren **opslag** kunt u een opslagaccount en een blob-container die de logboeken worden geschreven om te selecteren. 

Als u Logboeken op het bestandssysteem opslaat, kunnen de bestanden worden geopend door de FTP of gedownload als een Zip-archief met behulp van Azure CLI.

Standaard logboeken worden niet automatisch verwijderd (met uitzondering van **toepassingslogboeken (bestandssysteem)**). Als u wilt verwijderen automatisch logboeken, instellen de **bewaarperiode (dagen)** veld.

> [!NOTE]
> Als u [opnieuw genereren van toegangssleutels voor uw opslagaccount](../storage/common/storage-create-storage-account.md), moet u de configuratie van de respectieve logboekregistratie voor het gebruik van de bijgewerkte sleutels opnieuw instellen. Om dit te doen:
>
> 1. In de **configureren** tabblad, stelt u de functie voor de respectieve logboekregistratie op **uit**. Sla de instelling.
> 2. Logboekregistratie naar de blob storage-account opnieuw inschakelen. Sla de instelling.
>
>

Een combinatie van file system- of blob storage kan worden ingeschakeld op hetzelfde moment en configuraties van het afzonderlijke log niveau hebben. U wilt bijvoorbeeld aanmelden fouten en waarschuwingen naar de blobopslag als een oplossing op lange termijn logboekregistratie, tijdens het inschakelen van logboekregistratie in systeem met een niveau van uitgebreide.

Hoewel beide opslaglocaties de dezelfde algemene informatie voor de vastgelegde gebeurtenissen bieden **blobopslag** registreert aanvullende informatie zoals de exemplaar-ID, thread-ID en een meer gedetailleerd tijdstempel (maatstreepjes-indeling) dan logboekregistratie naar **bestandssysteem**.

> [!NOTE]
> Gegevens die zijn opgeslagen **blobopslag** kunnen alleen worden geopend met behulp van een storage-client of een toepassing die u rechtstreeks met deze systemen voor opslag werken kunt. Bijvoorbeeld, Visual Studio 2013 bevat een Opslagverkenner die kan worden gebruikt voor het verkennen van blob-opslag en HDInsight toegang tot de gegevens die zijn opgeslagen in blob-opslag. U kunt ook schrijven met een toepassing die toegang heeft tot Azure Storage met behulp van een van de [Azure-SDK's](https://azure.microsoft.com/downloads/).
>

## <a name="download"></a> Hoe: Logboeken downloaden
Diagnostische gegevens die zijn opgeslagen in het bestandssysteem van de web-app is toegankelijk via FTP. Het kan ook worden gedownload als een Zip-archief met behulp van Azure CLI.

De mapstructuur die de logboeken worden opgeslagen in is als volgt:

* **Toepassingslogboeken** -/LogFiles/toepassing /. Deze map bevat een of meer tekstbestanden met gegevens die worden geproduceerd door logboekregistratie van toepassingen.
* **Kan geen aanvraag traceringen** -/ logboekbestanden/W3SVC ### /. Deze map bevat een XSL-bestand en een of meer XML-bestanden. Zorg ervoor dat u de XSL-bestand in dezelfde map downloaden als het XML-bestand bestand(en) omdat het XSL-bestand functionaliteit biedt voor het formatteren en filteren van de inhoud van de XML-bestanden wanneer deze wordt bekeken in Internet Explorer.
* **Gedetailleerde foutenlogboeken** -/LogFiles/DetailedErrors /. Deze map bevat een of meer htm-bestanden met uitgebreide informatie voor HTTP-fouten die zich hebben voorgedaan.
* **Web Server-logboeken** -/LogFiles/http/RawLogs. Deze map bevat een of meer tekstbestanden geformatteerd met het [uitgebreide W3C-logboekbestandsindeling](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Implementatielogboeken** -/ logboekbestanden/Git. Deze map bevat de logboeken die worden gegenereerd door de interne implementatieprocessen die worden gebruikt door Azure-web-apps, evenals de logboeken voor Git-implementaties. U vindt hier ook Logboeken onder D:\home\site\deployments-implementatie.

### <a name="ftp"></a>FTP

Zie het openen van een FTP-verbinding met de FTP-server van uw app [uw app implementeren in Azure App Service met behulp van FTP/S](app-service-deploy-ftp.md).

Nadat het is verbonden met uw web-app FTP/S-server, opent de **logboekbestanden** map waar de logbestanden worden opgeslagen.

### <a name="download-with-azure-cli"></a>Met Azure CLI downloaden
De logboekbestanden met behulp van de Azure-opdrachtregelinterface downloaden, opent u een nieuwe opdrachtprompt, PowerShell, Bash of Terminal-sessie en voer de volgende opdracht:

    az webapp log download --resource-group resourcegroupname --name webappname

Deze opdracht slaat u de logboeken voor de web-app met de naam 'webappname' naar een bestand met de naam **diagnostics.zip** in de huidige map.

> [!NOTE]
> Als u Azure CLI nog niet hebt geïnstalleerd, of zijn niet geconfigureerd voor het gebruik van uw Azure-abonnement, Zie [over het gebruik van Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Hoe: weergeven in Application Insights-Logboeken
Visual Studio Application Insights biedt hulpprogramma's voor het filteren en zoeken in Logboeken en voor de logboeken correleren met aanvragen en andere gebeurtenissen.

1. De Application Insights SDK toevoegen aan uw project in Visual Studio.
   * Klik in Solution Explorer met de rechtermuisknop op uw project en kies Application Insights toevoegen. De interface leidt u door de stappen, waaronder het maken van een Application Insights-resource. [Meer informatie](../application-insights/app-insights-asp-net.md)
2. De Trace Listener-pakket toevoegen aan uw project.
   * Met de rechtermuisknop op uw project en kies NuGet-pakketten beheren. Selecteer `Microsoft.ApplicationInsights.TraceListener` [meer informatie](../application-insights/app-insights-asp-net-trace-logs.md)
3. Uploaden van uw project en voer dit voor het genereren van logboekgegevens.
4. In de [Azure-portal](https://portal.azure.com/), blader naar de nieuwe Application Insights-resource en open **zoeken**. U ziet uw logboekgegevens, samen met de aanvraag, gebruik en andere telemetrie. Telemetrie ontvangen van een paar minuten kan duren: klik op vernieuwen. [Meer informatie](../application-insights/app-insights-diagnostic-search.md)

[Meer informatie over de prestaties bijhouden met Application Insights](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a> Hoe: Stream Logboeken
Tijdens het ontwikkelen van een toepassing, is het vaak nuttig om te zien van logboekgegevens in near-real-time. U kunt de logboekinformatie voor streamen naar uw ontwikkelomgeving met behulp van Azure CLI.

> [!NOTE]
> Bepaalde typen logboekregistratie buffer schrijven naar het logboekbestand, dit in niet-geordende gebeurtenissen in de stroom resulteren kan. De logboekvermelding van een toepassing die wordt uitgevoerd wanneer een gebruiker een pagina bezoeken kan bijvoorbeeld worden weergegeven in de stroom voordat de bijbehorende HTTP-vermelding voor de pagina-aanvraag.
>
> [!NOTE]
> Logboekstreaming ook streams voor gegevens die worden geschreven naar een tekstbestand die zijn opgeslagen in de **D:\\home\\logboekbestanden\\**  map.
>
>

### <a name="streaming-with-azure-cli"></a>Streaming met Azure CLI
Om te streamen van logboekgegevens, opent u een nieuwe opdrachtprompt, PowerShell, Bash of Terminal-sessie en voer de volgende opdracht:

    az webapp log tail --name webappname --resource-group myResourceGroup

Met deze opdracht maakt verbinding met de web-app met de naam 'webappname' en beginnen met streamen van gegevens naar het venster zoals gebeurtenissen op de web-app plaatsvinden. Alle informatie geschreven naar hebben die eindigt op .txt, .log of htm-bestanden die zijn opgeslagen in de map /LogFiles (d:/home/logboekbestanden) worden gestreamd naar de lokale console.

Om te filteren op specifieke gebeurtenissen, zoals fouten, gebruikt u de **--Filter** parameter. Bijvoorbeeld:

    az webapp log tail --name webappname --resource-group myResourceGroup --filter Error

Om te filteren op specifieke logboek typen, zoals HTTP, gebruikt u de **--pad** parameter. Bijvoorbeeld:

    az webapp log tail --name webappname --resource-group myResourceGroup --path http

> [!NOTE]
> Als u Azure CLI nog niet hebt geïnstalleerd, of zijn niet geconfigureerd voor het gebruik van uw Azure-abonnement, Zie [over het gebruik van Azure CLI](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a> Hoe: inzicht in Logboeken met diagnostische gegevens
### <a name="application-diagnostics-logs"></a>Application diagnostics-Logboeken
Application diagnostics wordt informatie opgeslagen in een specifieke indeling voor .NET-toepassingen, afhankelijk van of het opslaan van logboeken naar de file system- of blob storage. 

De basisset met gegevens die zijn opgeslagen is hetzelfde voor beide opslagtypen: de datum en tijd is die de gebeurtenis heeft plaatsgevonden, wordt de proces-ID die de gebeurtenis, het gebeurtenistype (informatie, waarschuwing, fout) en het bericht van de gebeurtenis heeft geproduceerd. Met behulp van het bestandssysteem voor logboekopslag is handig wanneer u directe toegang tot het oplossen van een probleem omdat de logboekbestanden worden bijgewerkt in de buurt onmiddellijk nodig hebt. BLOB-opslag wordt gebruikt voor archiveringsdoeleinden omdat de bestanden worden opgeslagen in de cache en dat vervolgens in de opslagcontainer volgens een schema wordt leeggemaakt.

**Bestandssysteem**

Elke regel geregistreerd in het bestandssysteem of ontvangen met behulp van streaming is in de volgende indeling:

    {Date}  PID[{process ID}] {event type/level} {message}

Bijvoorbeeld, wordt een foutgebeurtenis weergegeven die vergelijkbaar is met het volgende voorbeeld:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

Logboekregistratie naar het bestandssysteem bevat de meest elementaire informatie van de drie beschikbare methoden, alleen de tijd, proces-ID, gebeurtenisniveau en bericht te leveren.

**Blob Storage**

Tijdens de registratie van BLOB storage, kunnen gegevens worden opgeslagen in de indeling met door komma's gescheiden waarden (CSV). Aanvullende velden worden geregistreerd voor gedetailleerdere informatie over de gebeurtenis. De volgende eigenschappen worden gebruikt voor elke rij in de CSV:

| Naam van eigenschap | Waarde-indeling |
| --- | --- |
| Date |De datum en tijd waarop de gebeurtenis heeft plaatsgevonden |
| Niveau |Gebeurtenisniveau (bijvoorbeeld fout, waarschuwing, informatie) |
| ApplicationName |De naam van de web-app |
| Instantie-id |Exemplaar van de web-app die de gebeurtenis is opgetreden op |
| EventTickCount |De datum en tijd waarop de gebeurtenis heeft plaatsgevonden, in de indeling van de maatstreepjes (grotere precisie) |
| Gebeurtenis-id |De gebeurtenis-ID van deze gebeurtenis<p><p>Standaard ingesteld op 0 als er niets opgegeven |
| PID |Proces-id |
| TID |De thread-ID van de thread die de gebeurtenis heeft geproduceerd |
| Bericht |Details van gebeurtenisbericht |

De gegevens die zijn opgeslagen in een blob wordt het volgende voorbeeld als volgt uitzien:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> Voor ASP.NET Core, logboekregistratie wordt gerealiseerd met behulp van de [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider deze provider deposito's aanvullende in de blob-container logboekbestanden. Zie voor meer informatie, [ASP.NET Core logboekregistratie in Azure](/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#logging-in-azure).
>
>

### <a name="failed-request-traces"></a>Traceringen van de aanvraag is mislukt
Traceringen van mislukte aanvragen worden opgeslagen in de XML-bestanden met de naam **fr ### .xml**. Als u wilt maken het gemakkelijker om de geregistreerde gegevens weer te geven, een XSL-opmaakmodel met de naam **freb.xsl** is opgegeven in dezelfde map als de XML-bestanden. Als u een van de XML-bestanden in Internet Explorer openen, Internet Explorer het XSL-opmaakmodel gebruikt voor een opgemaakte weergave van de trace-informatie, zoals in het volgende voorbeeld:

![mislukte aanvragen die worden weergegeven in de browser](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Logboeken met details
Logboeken met details zijn HTML-documenten die bieden meer gedetailleerde informatie over HTTP-fouten die zijn opgetreden. Omdat ze gewoon HTML-documenten, kunnen ze worden weergegeven via een webbrowser.

### <a name="web-server-logs"></a>Webserverlogboeken
De web server-logboeken zijn geformatteerd met het [uitgebreide W3C-logboekbestandsindeling](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Deze informatie kan worden gelezen met behulp van een teksteditor of met behulp van hulpprogramma's zoals geparseerd [Logboekparser](https://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> De logboeken die worden geproduceerd door de Azure-web-apps bieden geen ondersteuning voor de **s-computername**, **s-IP-**, of **cs-version** velden.
>
>

## <a name="nextsteps"></a> Volgende stappen
* [WebApps controleren](web-sites-monitor.md)
* [Oplossen van problemen met Azure-web-apps in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analyseren van web-Applogboeken in HDInsight](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
>
>

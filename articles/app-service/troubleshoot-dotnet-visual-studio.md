---
title: Problemen met een app met Visual Studio - Azure App Service oplossen
description: Informatie over het oplossen van een App Service-app met behulp van foutopsporing op afstand, tracering en logboekregistratie-hulpprogramma's die zijn ingebouwd in Visual Studio 2013.
services: app-service
documentationcenter: .net
author: cephalin
manager: cfowler
editor: ''
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: 03bafcdbf6890573d1d2855e2b47520d0111fe13
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57996775"
---
# <a name="troubleshoot-an-app-in-azure-app-service-using-visual-studio"></a>Problemen met een app in Azure App Service met behulp van Visual Studio oplossen
## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u Visual Studio-hulpprogramma's gebruiken om u te helpen bij foutopsporing in een app [App Service](https://go.microsoft.com/fwlink/?LinkId=529714), door te voeren in [foutopsporingsmodus](https://docs.microsoft.com/visualstudio/debugger/) op afstand of door toepassingslogboeken en webserverlogboeken te bekijken.

U leert het volgende:

* Welke app-beheerfuncties zijn beschikbaar in Visual Studio.
* Het gebruik van Visual Studio externe weergave snel wijzigingen aanbrengen in een externe app.
* Het uitvoeren van de foutopsporingsmodus op afstand tijdens een project wordt uitgevoerd in Azure, voor een app en een WebJob.
* Logbestanden voor Toepassingstracering maken en ze weergeven terwijl de toepassing is ze worden gemaakt.
* Om weer te geven van webserverlogboeken, met inbegrip van gedetailleerde foutberichten en tracering van mislukte aanvragen.
* Klik hier voor meer informatie over het verzenden van diagnostische logboeken naar een Azure Storage-account en ze er weergeven.

Als u Visual Studio Ultimate hebben, kunt u ook gebruiken [IntelliTrace](https://msdn.microsoft.com/library/vstudio/dd264915.aspx) voor foutopsporing. IntelliTrace wordt niet behandeld in deze zelfstudie.

## <a name="prerequisites"></a>Vereisten
In deze zelfstudie werkt met de ontwikkelomgeving, het webproject en de App Service-app die u hebt ingesteld in [een ASP.NET-app maken in Azure App Service](app-service-web-get-started-dotnet-framework.md). Voor de secties WebJobs, moet u de toepassing die u maakt in [aan de slag met de Azure WebJobs SDK][GetStartedWJ].

De codevoorbeelden die wordt weergegeven in deze zelfstudie zijn voor een C# MVC-webtoepassing, maar de procedures voor probleemoplossing zijn hetzelfde voor Visual Basic- en Web Forms-toepassingen.

De zelfstudie wordt ervan uitgegaan dat u Visual Studio 2017. 

De streaminglogboeken functie werkt alleen voor toepassingen die zijn gericht op .NET Framework 4 of hoger.

## <a name="sitemanagement"></a>Appconfiguratie en beheer
Visual Studio biedt toegang tot een subset van de app-beheerfuncties en -configuratie-instellingen die beschikbaar zijn in de [Azure-portal](https://go.microsoft.com/fwlink/?LinkId=529715). In deze sectie ziet u wat beschikbaar is via **Server Explorer**. Als u wilt zien van de nieuwste functies van de integratie van Azure, uitproberen **Cloud Explorer** ook. U kunt zowel windows vanuit openen de **weergave** menu.

1. Als u nog niet hebt aangemeld bij Azure in Visual Studio, met de rechtermuisknop op **Azure** en selecteer verbinding maken met **Microsoft Azure-abonnement** in **Server Explorer**.

    Een alternatief is het installeren van een beheercertificaat waarmee toegang tot uw account. Als u ervoor kiest om een certificaat te installeren, met de rechtermuisknop op de **Azure** knooppunt in **Server Explorer**, en selecteer vervolgens **beheren en Filter abonnementen** in het contextmenu. In de **Microsoft Azure-abonnementen beheren** in het dialoogvenster, klikt u op de **certificaten** tabblad en klik vervolgens op **importeren**. Volg de aanwijzingen om te downloaden en vervolgens importeert u een abonnementsbestand (ook wel een *.publishsettings* bestand) voor uw Azure-account.

   > [!NOTE]
   > Als u een abonnementsbestand hebt gedownload, opslaan in een map buiten uw adreslijsten source code (bijvoorbeeld in de map Downloads) en vervolgens te verwijderen nadat het importeren is voltooid. Een kwaadwillende gebruiker die toegang tot het abonnementsbestand krijgt kunt bewerken, maken en verwijderen van uw Azure-services.
   >
   >

    Zie voor meer informatie over het verbinden met Azure-resources vanuit Visual Studio [Manage Accounts, abonnementen en beheerdersrollen](https://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. In **Server Explorer**, vouw **Azure** en vouw **App Service**.
3. Vouw de resourcegroep met de app die u hebt gemaakt in [een ASP.NET-app maken in Azure App Service](app-service-web-get-started-dotnet-framework.md), en vervolgens met de rechtermuisknop op het app-knooppunt en klik op **weergave-instellingen**.

    ![Instellingen weergeven in Server Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    De **Azure Web App** tabblad wordt weergegeven en u kunt zien er de app-beheer en de configuratie van taken die beschikbaar in Visual Studio zijn.

    ![Het venster van de Azure-Web-App](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    In deze zelfstudie gebruikt u de logboekregistratie en tracering vervolgkeuzelijsten. U zult ook gebruiken voor foutopsporing op afstand, maar u een andere methode gebruiken om te inschakelen.

    Zie voor meer informatie over de App-instellingen en verbindingsreeksen vakken in dit venster [Azure App Service: Hoe tekenreeksen van toepassingen en verbinding werk tekenreeksen](https://azure.microsoft.com/blog/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).

    Als u uitvoeren van een taak voor het beheer van apps die in dit venster kan niet worden uitgevoerd wilt, klikt u op **Open in de beheerportal** om een browservenster naar de Azure-portal te openen.

## <a name="remoteview"></a>Toegang tot app-bestanden in Server Explorer
Normaal gesproken implementeren van een webproject met de `customErrors` vlag in het bestand Web.config ingesteld op `On` of `RemoteOnly`, wat betekent dat er geen een handige foutbericht als er iets misgaat. Voor veel fouten is al krijgt u een pagina, zoals een van de volgende query:

**Serverfout in '/' Application:**

![Onnuttige foutpagina](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Er is een fout opgetreden:**

![Onnuttige foutpagina](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**De website van de pagina kan niet worden weergegeven**

![Onnuttige foutpagina](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Vaak is de eenvoudigste manier om de oorzaak van de fout om in te schakelen gedetailleerde foutberichten worden weergegeven, die de eerste dag van de vorige schermafbeeldingen wordt uitgelegd hoe u kunt doen. Waarvoor is vereist een wijziging in het geïmplementeerde Web.config-bestand. U kunt bewerken de *Web.config* -bestand in het project en implementeer het project opnieuw of maak een [Web.config-transformatie](https://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) en implementeren van een foutopsporingsbuild werkt, maar er is een snellere manier: in **Solution Explorer** , u kunt rechtstreeks bestanden weergeven en bewerken in de externe app met behulp van de *externe weergave* functie.

1. In **Server Explorer**, vouw **Azure**, vouw **App Service**, vouw de resourcegroep die uw app bevindt zich in uit en vouw vervolgens het knooppunt voor uw app.

    U ziet dat knooppunten waarmee u toegang tot de inhoudsbestanden en de logboekbestanden van de app.
2. Vouw de **bestanden** knooppunt uit en dubbelklik op de *Web.config* bestand.

    ![Open Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio opent het bestand Web.config van de externe app en [extern] bevat naast de bestandsnaam van het in de titelbalk.
3. Voeg de volgende regel aan de `system.web` element:

    `<customErrors mode="Off"></customErrors>`

    ![Edit Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Vernieuw de browser die de onnuttige foutbericht wordt weergegeven en u nu een gedetailleerd foutbericht, zoals in het volgende voorbeeld krijgen:

    ![Gedetailleerd foutbericht](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (De fout die wordt weergegeven is gemaakt door het toevoegen van de regel wordt weergegeven in rood naar *Views\Home\Index.cshtml*.)

Bewerken van het bestand Web.config is slechts één voorbeeld van scenario's waarin u de mogelijkheid om te lezen en bewerken van bestanden op uw App Service-app probleemoplossing eenvoudiger.

## <a name="remotedebug"></a>Externe foutopsporing apps
Als het gedetailleerde foutbericht niet voldoende informatie bieden, en u niet opnieuw met de fout lokaal maken, is een andere manier om op te lossen om uit te voeren in de foutopsporingsmodus op afstand. U kunt onderbrekingspunten instellen, rechtstreeks bewerken van geheugen stap via code en zelfs Wijzig het codepad.

Foutopsporing op afstand werkt niet in de Express-edities van Visual Studio.

Deze sectie wordt beschreven hoe u fouten opsporen op afstand met behulp van het project dat u maakt in [een ASP.NET-app maken in Azure App Service](app-service-web-get-started-dotnet-framework.md).

1. Open het webproject dat u hebt gemaakt in [een ASP.NET-app maken in Azure App Service](app-service-web-get-started-dotnet-framework.md).

2. Open *Controllers\HomeController.cs*.

3. Verwijder de `About()` methode en voeg de volgende in plaats daarvan code.

``` c#
public ActionResult About()
{
    string currentTime = DateTime.Now.ToLongTimeString();
    ViewBag.Message = "The current time is " + currentTime;
    return View();
}
```

1. [Stel een onderbrekingspunt](https://docs.microsoft.com/visualstudio/debugger/) op de `ViewBag.Message` regel.

1. In **Solution Explorer**, met de rechtermuisknop op het project en klik op **publiceren**.

1. In de **profiel** vervolgkeuzelijst in de lijst met hetzelfde profiel dat u gebruikt in [een ASP.NET-app maken in Azure App Service](app-service-web-get-started-dotnet-framework.md). Klik vervolgens op instellingen.

1. In de **publiceren** dialoogvenster, klikt u op de **instellingen** tabblad en wijzig vervolgens de **configuratie** naar **Debug**, en klik vervolgens op  **Sla**.

    ![Publiceren in de foutopsporingsmodus](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)

1. Klik op **Publish**. Na de implementatie is voltooid en de browser wordt geopend in de Azure-URL van uw app, sluit de browser.

1. In **Server Explorer**, met de rechtermuisknop op uw app en klik vervolgens op **foutopsporingsprogramma koppelen**.

    ![Debugger koppelen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    De browser wordt automatisch geopend op uw startpagina uitvoeren in Azure. Mogelijk hebt 20 seconden wachten terwijl Azure de server voor foutopsporing ingesteld. Deze vertraging alleen de eerste keer dat u in de foutopsporingsmodus op een app in een periode van 48 uur uitvoert wordt uitgevoerd. Als u foutopsporing in dezelfde periode opnieuw start, moet u er een vertraging geen.

    > [!NOTE] 
    > Als u eventuele problemen met het starten van het foutopsporingsprogramma hebt, kunt u om dat te doen met behulp van **Cloud Explorer** in plaats van **Server Explorer**.
    >

1. Klik op **over** in het menu.

    Visual Studio op het onderbrekingspunt gestopt en de code wordt uitgevoerd in Azure, niet op uw lokale computer.

1. Beweeg de muisaanwijzer over de `currentTime` variabele om te zien van de tijd-waarde.

    ![De variabele in de foutopsporingsmodus uitvoeren in Azure](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

    De tijd die u ziet is de tijd Azure-server, die mogelijk in een andere tijdzone dan uw lokale computer.

1. Voer een nieuwe waarde voor de `currentTime` variabele, zoals 'Nu uitvoeren in Azure'.

1. Druk op F5 om door te gaan die wordt uitgevoerd.

     De Info-pagina die wordt uitgevoerd in Azure wordt de nieuwe waarde die u hebt ingevoerd in de variabele currentTime weergegeven.

     ![Pagina met de nieuwe waarde](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a> WebJobs voor foutopsporing op afstand
Deze sectie wordt beschreven hoe u fouten opsporen op afstand met behulp van het project en de app die u maakt in [aan de slag met de Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).

Welke functies worden weergegeven in deze sectie zijn alleen beschikbaar in Visual Studio 2013 met Update 4 of hoger.

Foutopsporing op afstand werkt alleen met doorlopende webtaken. Geplande en on-demand WebJobs bieden geen ondersteuning voor foutopsporing.

1. Open het webproject dat u hebt gemaakt in [aan de slag met de Azure WebJobs SDK][GetStartedWJ].

2. Open in het project ContosoAdsWebJob *Functions.cs*.

3. [Stel een onderbrekingspunt](https://docs.microsoft.com/visualstudio/debugger/) op de eerste instructie in de `GnerateThumbnail` methode.

    ![Onderbrekingspunt instellen](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)

4. In **Solution Explorer**, met de rechtermuisknop op het webproject (niet op het project webtaak) en klikt u op **publiceren**.

5. In de **profiel** vervolgkeuzelijst in de lijst met hetzelfde profiel dat u gebruikt in [aan de slag met de Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).

6. Klik op de **instellingen** tabblad en wijzig **configuratie** naar **fouten opsporen in**, en klik vervolgens op **publiceren**.

    Visual Studio implementeert het web en WebJob-projecten en uw browser geopend met de Azure-URL van uw app.

7. In **Server Explorer**, vouw **Azure > App Service > uw resourcegroep > uw app > WebJobs > doorlopend**, en klik vervolgens met de rechtermuisknop op **ContosoAdsWebJob**.

8. Klik op **foutopsporingsprogramma koppelen**.

    ![Debugger koppelen](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    De browser wordt automatisch geopend op uw startpagina uitvoeren in Azure. Mogelijk hebt 20 seconden wachten terwijl Azure de server voor foutopsporing ingesteld. Deze vertraging alleen de eerste keer dat u in de foutopsporingsmodus op een app in een periode van 48 uur uitvoert wordt uitgevoerd. Als u foutopsporing in dezelfde periode opnieuw start, moet u er een vertraging geen.

9. In de webbrowser naar de Contoso Ads-startpagina wordt geopend, maakt u een nieuwe ad.

    Het maken van een ad zorgt ervoor dat een wachtrijbericht te maken dat is opgehaald door de webtaak en wordt verwerkt. De WebJobs SDK roept de functie voor het verwerken van het wachtrijbericht, de treffers code wanneer uw onderbrekingspunt.

10. Wanneer het foutopsporingsprogramma op uw onderbrekingspunt onderbreekt, kunt u onderzoeken en waarden van variabelen niet wijzigen terwijl het programma wordt uitgevoerd de cloud. In de volgende afbeelding ziet u het foutopsporingsprogramma de inhoud van het blobInfo-object dat is doorgegeven aan de `GenerateThumbnail` methode.

     ![in Foutopsporing blobInfo-object](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)

11. Druk op F5 om door te gaan die wordt uitgevoerd.

     De `GenerateThumbnail` methode klaar is met het maken van de miniatuur.

12. In de browser vernieuwen van de Index-pagina en ziet u de miniatuur.

13. Druk op SHIFT + F5 om te stop de foutopsporing in Visual Studio.

14. In **Server Explorer**, met de rechtermuisknop op het knooppunt ContosoAdsWebJob en klikt u op **Dashboard weergeven**.

15. Meld u aan met uw Azure-referenties, en klik vervolgens op de naam van een webtaak te gaan naar de pagina voor de webtaak.

     ![Klik op ContosoAdsWebJob](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Het Dashboard ziet u dat de `GenerateThumbnail` -functie is onlangs uitgevoerd.

     (De volgende keer dat u op **Dashboard weergeven**, u hoeft aan te melden bij, en de browser gaat rechtstreeks naar de pagina voor de webtaak.)

16. Klik op de naam van de functie voor informatie over de functie wordt uitgevoerd.

     ![Functiedetails](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

Als uw functie [Logboeken geschreven](https://github.com/Azure/azure-webjobs-sdk/wiki), kunt u op **ToggleOutput** te zien.

## <a name="notes-about-remote-debugging"></a>Opmerkingen over foutopsporing op afstand

* Uitgevoerd in de foutopsporingsmodus in productie wordt niet aanbevolen. Als uw app in productie is niet geschaald naar meerdere exemplaren van server, foutopsporing wordt voorkomen dat de webserver reageren op andere aanvragen. Als u meerdere exemplaren van web server, hebt wanneer u aan het foutopsporingsprogramma koppelt, krijgt u een willekeurig exemplaar en er geen manier om ervoor te zorgen dat browseraanvragen van de volgende gaat u naar hetzelfde exemplaar. Ook, u niet een foutopsporingsbuild werkt doorgaans implementeren naar productie, en compiler optimalisaties voor release builds kunnen het onmogelijk om weer te geven wat er in uw broncode per regel gebeurt. Voor het oplossen van problemen, is de beste resource toepassing tracerings- en web server-Logboeken.
* Te voorkomen dat lange wordt gestopt bij onderbrekingspunten wanneer externe foutopsporing. Azure behandelt een proces dat langer dan een paar minuten als een niet-reagerende proces wordt gestopt en wordt afgesloten.
* Tijdens de foutopsporing, is de server verzenden van gegevens naar Visual Studio, die invloed kunnen zijn op de bandbreedte kosten in rekening gebracht. Zie voor meer informatie over tarieven voor bandbreedte [Azure-prijzen](https://azure.microsoft.com/pricing/calculator/).
* Zorg ervoor dat de `debug` kenmerk van de `compilation` -element in de *Web.config* bestand is ingesteld op true. Deze is ingesteld op true is standaard wanneer u een buildconfiguratie foutopsporing publiceert.

``` xml
<system.web>
  <compilation debug="true" targetFramework="4.5" />
  <httpRuntime targetFramework="4.5" />
</system.web>
```
* Als u merkt dat het foutopsporingsprogramma niet stap in de code die u fouten wilt opsporen, is het mogelijk om de instelling alleen mijn Code te wijzigen.  Zie voor meer informatie, [opgeven of u alleen gebruikerscode met alleen mijn Code in Visual Studio fouten opsporen in](https://docs.microsoft.com/visualstudio/debugger/just-my-code).
* Een timer wordt gestart op de server wanneer u de functie voor externe foutopsporing inschakelen, en na 48 uur de functie is automatisch uitgeschakeld. Deze limiet 48 uur is uitgevoerd voor beveiliging en prestaties. U kunt eenvoudig de functie weer inschakelen als vaak is als u wilt. Het is raadzaam dat wanneer u niet actief foutopsporing uitgeschakeld.
* U kunt het foutopsporingsprogramma handmatig koppelen aan een proces, niet alleen de app-proces (w3wp.exe). Zie voor meer informatie over het gebruik van foutopsporingsmodus in Visual Studio [foutopsporing in Visual Studio](https://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Overzicht van diagnostische logboeken
Een ASP.NET-toepassing die wordt uitgevoerd in een App Service-app kunt maken van de volgende soorten logboeken:

* **Tracering van toepassingslogboeken**<br/>
  De toepassing maakt deze logboeken door het aanroepen van methoden van de [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx) klasse.
* **Webserverlogboeken**<br/>
  De webserver wordt gemaakt van een logboekvermelding voor elke HTTP-aanvraag naar de app.
* **Logboeken met details bericht**<br/>
  De webserver maakt een HTML-pagina met aanvullende informatie voor mislukte HTTP-aanvragen (aanvragen die leiden tot statuscode 400 of hoger).
* **Logboeken voor mislukte aanvragen traceren**<br/>
  De webserver maakt een XML-bestand met gedetailleerde traceringsgegevens voor mislukte HTTP-aanvragen. De webserver biedt ook een XSL-bestand als u wilt opmaken van het XML-bestand in een browser.

Logboekregistratie is van invloed op app-prestaties, zodat Azure kunt u de in- of uitschakelen van elk type logboek naar behoefte. Voor de toepassing zich aanmeldt, kunt u opgeven dat alleen logboeken boven een bepaalde mate van ernst tekstberichten worden geschreven. Wanneer u een nieuwe app maken, standaard alle logboekregistratie is uitgeschakeld.

Logboeken worden geschreven naar bestanden in een *logboekbestanden* map in het bestandssysteem van uw app en zijn toegankelijk via FTP. Web server-logboeken en toepassingslogboeken kunnen ook worden geschreven naar een Azure Storage-account. U kunt een groter volume van de logboeken in een storage-account dan mogelijk is in het bestandssysteem behouden. U bent beperkt tot een maximum van 100 MB van Logboeken wanneer u het bestandssysteem. (Alleen voor het bewaren van op korte termijn zijn bestand het systeemlogboek in Logboeken. Azure verwijdert oude logbestanden om ruimte te maken voor nieuwe nadat de limiet is bereikt.)  

## <a name="apptracelogs"></a>Maken en logbestanden voor Toepassingstracering weergeven
In deze sectie maakt uitvoeren u de volgende taken:

* Instructies voor het traceren toevoegen aan de webproject dat u hebt gemaakt in [aan de slag met Azure en ASP.NET](app-service-web-get-started-dotnet-framework.md).
* Bekijk de logboeken als u het project lokaal uitvoeren.
* Bekijk de logboeken nadat ze zijn gegenereerd door de toepassing die wordt uitgevoerd in Azure.

Zie voor informatie over het maken van de toepassing zich WebJobs aanmeldt, [over het werken met Azure queue storage met behulp van de WebJobs SDK - het schrijven van logboeken](https://github.com/Azure/azure-webjobs-sdk/wiki). De volgende instructies voor het weergeven van Logboeken en bepalen hoe ze zijn opgeslagen in Azure ook van toepassing op toepassingslogboeken die zijn gemaakt door WebJobs.

### <a name="add-tracing-statements-to-the-application"></a>Instructies voor het traceren toevoegen aan de toepassing
1. Open *Controllers\HomeController.cs*, en vervang de `Index`, `About`, en `Contact` methoden met de volgende code om toe te voegen `Trace` instructies en een `using` -instructie voor `System.Diagnostics`:

```c#
public ActionResult Index()
{
    Trace.WriteLine("Entering Index method");
    ViewBag.Message = "Modify this template to jump-start your ASP.NET MVC application.";
    Trace.TraceInformation("Displaying the Index page at " + DateTime.Now.ToLongTimeString());
    Trace.WriteLine("Leaving Index method");
    return View();
}

public ActionResult About()
{
    Trace.WriteLine("Entering About method");
    ViewBag.Message = "Your app description page.";
    Trace.TraceWarning("Transient error on the About page at " + DateTime.Now.ToShortTimeString());
    Trace.WriteLine("Leaving About method");
    return View();
}

public ActionResult Contact()
{
    Trace.WriteLine("Entering Contact method");
    ViewBag.Message = "Your contact page.";
    Trace.TraceError("Fatal error on the Contact page at " + DateTime.Now.ToLongTimeString());
    Trace.WriteLine("Leaving Contact method");
    return View();
}        
```

1. Voeg een `using System.Diagnostics;` instructie naar de bovenkant van het bestand.

### <a name="view-the-tracing-output-locally"></a>De traceringsuitvoer lokaal weergeven
1. Druk op F5 voor het uitvoeren van de toepassing in de foutopsporingsmodus.

    De standaard traceringslistener schrijft alle traceringsuitvoer naar de **uitvoer** venster, samen met andere foutopsporingsuitvoer. De volgende afbeelding ziet de uitvoer van de traceringsinstructies die u hebt toegevoegd aan de `Index` methode.

    ![Tracering in het venster Foutopsporing](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    De volgende stappen laten zien hoe trace-uitvoer weergeven in webpagina's, zonder het compileren van in de foutopsporingsmodus.
2. Open het bestand Web.config (het account zich in de projectmap) en voeg een `<system.diagnostics>` element aan het einde van het bestand net voordat de afsluiting `</configuration>` element:

``` xml
<system.diagnostics>
<trace>
  <listeners>
    <add name="WebPageTraceListener"
        type="System.Web.WebPageTraceListener,
        System.Web,
        Version=4.0.0.0,
        Culture=neutral,
        PublicKeyToken=b03f5f7f11d50a3a" />
  </listeners>
</trace>
</system.diagnostics>
```

De `WebPageTraceListener` Hiermee kunt u weergeven trace-uitvoer door te bladeren naar `/trace.axd`.
1. Voeg een <a href="https://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">trace-element</a> onder `<system.web>` in het Web.config-bestand, zoals in het volgende voorbeeld:

``` xml
<trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
```       

1. Druk op CTRL + F5 om de toepassing uit te voeren.
1. Voeg in de adresbalk van het browservenster *trace.axd* toe aan de URL en druk op Enter (de URL is vergelijkbaar met `http://localhost:53370/trace.axd`).
1. Op de **Toepassingstracering** pagina, klikt u op **Details weergeven** op de eerste regel (niet de BrowserLink).

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    De **aanvraagdetails** pagina wordt weergegeven, en klik in de **traceringsinformatie** sectie u de uitvoer van de traceringsinstructies die u hebt toegevoegd ziet aan de `Index` methode.

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Standaard `trace.axd` alleen lokaal beschikbaar is. Als u het toegankelijk maken vanuit een externe app wilt, kunt u toevoegen `localOnly="false"` naar de `trace` -element in de *Web.config* bestand, zoals weergegeven in het volgende voorbeeld:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Inschakelen van echter `trace.axd` in een productie app niet wordt aanbevolen vanwege veiligheidsredenen. In de volgende secties ziet u een eenvoudigere manier om te lezen traceringslogboeken in een App Service-app.

### <a name="view-the-tracing-output-in-azure"></a>De traceringsuitvoer bekijken in Azure
1. In **Solution Explorer**, met de rechtermuisknop op het webproject en klik op **publiceren**.
2. In de **webpublicatie** in het dialoogvenster, klikt u op **publiceren**.

    Nadat Visual Studio publiceert de update, wordt een browservenster naar de startpagina wordt geopend (ervan uitgaande dat u niet wissen **doel-URL** op de **verbinding** tabblad).
3. In **Server Explorer**, met de rechtermuisknop op uw app en selecteer **Streaminglogboeken bekijken**.

    ![Streaminglogboeken bekijken in het contextmenu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    De **uitvoer** venster ziet u dat u met de logstreamingservice verbonden bent en wordt een melding regel toegevoegd die elke minuut die gaat door zonder een logboek weer te geven.

    ![Streaminglogboeken bekijken in het contextmenu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. Klik in het browservenster waarin de startpagina van uw toepassing, op **Neem contact op met**.

    De uitvoer van het foutniveau traceren binnen een paar seconden, u hebt toegevoegd aan de `Contact` methode wordt weergegeven in de **uitvoer** venster.

    ![Fouttracering in het uitvoervenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Visual Studio is foutniveau traceringen worden alleen weergegeven, omdat dat is de standaardinstelling bij het inschakelen van het logboek monitoring-service. Wanneer u een nieuwe App Service-app maakt, is alle logboekregistratie standaard uitgeschakeld, zoals u hebt gezien als u de instellingenpagina eerder hebt geopend:

    ![Afmelden van toepassing](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Echter, wanneer u hebt geselecteerd **Streaminglogboeken bekijken**, Visual Studio automatisch gewijzigd **toepassing Logging(File System)** naar **fout**, wat betekent dat foutniveau Logboeken u heeft gerapporteerd. U kunt deze instelling om te wijzigen om te zien welke van uw logboekbestanden voor tracering, **uitgebreid**. Wanneer u een lager is dan fout ernstniveau selecteert, worden ook alle logboeken voor een hogere mate van ernst gerapporteerd. Dus als u uitgebreide, ziet ook u informatie, waarschuwing en foutenlogboeken.  

5. In **Server Explorer**, met de rechtermuisknop op de app en klik vervolgens op **weergave-instellingen** als u eerder hebt uitgevoerd.
6. Wijziging **toepassingslogboeken (bestandssysteem)** naar **uitgebreid**, en klik vervolgens op **opslaan**.

    ![Instellen van het traceringsniveau naar uitgebreid](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
7. In het browservenster dat nu wordt weergegeven de **Neem contact op met** pagina, klikt u op **Start**, klikt u vervolgens op **over**, en klik vervolgens op **Neem contact op met**.

    Binnen enkele seconden, de **uitvoer** venster toont alle van de traceringsuitvoer.

    ![Uitgebreide trace-uitvoer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    In deze sectie maakt u ingeschakeld en logboekregistratie met behulp van de appinstellingen uitgeschakeld. U kunt ook in- en traceer-listeners uitschakelen door het wijzigen van het bestand Web.config. Echter wijzigen van het bestand Web.config zorgt ervoor dat de app-domein te recyclen, terwijl het inschakelen van logboekregistratie via de app-configuratie niet dat doen. Als het probleem te reproduceren, lang duurt of is onderbroken, recycling van het app-domein mogelijk 'oplossen' en u moet worden gewacht tot deze zich weer voordoet verplichten. Inschakelen van diagnostische gegevens in Azure, kunt u begint met het opnemen van informatie over de fout onmiddellijk zonder te recyclen van de app-domein.

### <a name="output-window-features"></a>Functies van het venster uitvoer
De **Microsoft Azure-logboeken** tabblad van de **uitvoer** venster bevat enkele knoppen en een tekstvak in:

![Logboeken tabblad knoppen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Deze voert u de volgende functies:

* Schakel de **uitvoer** venster.
* In- of uitschakelen van tekstterugloop.
* Starten of stoppen van de bewaking van Logboeken.
* Opgeven welke logboeken om te controleren.
* Logboeken downloaden.
* Filteren op basis van een zoekreeks of een reguliere expressie Logboeken.
* Sluit de **uitvoer** venster.

Als u een zoekreeks of een reguliere expressie invoert, filtert Visual Studio waardevolle informatie op de client. Dit betekent dat u kunt de criteria opgeven nadat de logboeken worden weergegeven in de **uitvoer** -venster en u kunt filtercriteria wijzigen zonder opnieuw genereren van de logboeken.

## <a name="webserverlogs"></a>Web server-logboeken weergeven
Webserverlogboeken Noteer alle HTTP-activiteit voor de app. Als u wilt bekijken in de **uitvoer** venster heeft, moet u ze wilt inschakelen voor de app en Visual Studio laat weten dat u wilt ze te controleren.

1. In de **Azure Web App-configuratie** tabblad dat u hebt geopend vanuit **Server Explorer**, Web Server-logboekregistratie te wijzigen **op**, en klik vervolgens op **Opslaan**.

    ![Web server-logboekregistratie inschakelen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. In de **uitvoer** venster, klikt u op de **opgeven waarop Microsoft Azure-logboeken voor het bewaken van** knop.

    ![Geef op welke Azure-Logboeken om te controleren](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. In de **opties voor Microsoft Azure-logboekregistratie** in het dialoogvenster, selecteer **Web server-logboeken**, en klik vervolgens op **OK**.

    ![Webserverlogboeken bewaken](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. In het browservenster waarin de app wordt weergegeven, klikt u op **Start**, klikt u vervolgens op **over**, en klik vervolgens op **Neem contact op met**.

    De toepassingslogboeken in het algemeen weergegeven eerst, gevolgd door de web server-Logboeken. Mogelijk moet u wacht enige tijd voor de logboeken worden weergegeven.

    ![Webserver registreert in het uitvoervenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Standaard, wanneer u eerst webserverlogboeken inschakelen met behulp van Visual Studio schrijft Azure de logboeken naar het bestandssysteem. Als alternatief kunt kunt u de Azure-portal gebruiken om op te geven die webserver logboeken worden geschreven naar een blobcontainer in een storage-account.

Als u de portal gebruiken om in te schakelen webserver registreren van een Azure storage-account en schakel zijn logboekregistratie in Visual Studio, wanneer u opnieuw inschakelen van logboekregistratie in Visual Studio de instellingen van uw storage-account hersteld.

## <a name="detailederrorlogs"></a>Logboeken met details bericht weergeven
Gedetailleerde fout-logboeken bieden aanvullende informatie over HTTP-aanvragen die in de foutcodes van antwoord (400 of hoger resulteren). Als u wilt bekijken in de **uitvoer** venster, die u moet deze inschakelen voor de app en Visual Studio laat weten dat u wilt ze te controleren.

1. In de **Azure Web App-configuratie** tabblad dat u hebt geopend vanuit **Server Explorer**, wijzigen **gedetailleerde foutberichten** naar **op**, en vervolgens Klik op **opslaan**.

    ![Gedetailleerde foutberichten inschakelen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)

2. In de **uitvoer** venster, klikt u op de **opgeven waarop Microsoft Azure-logboeken voor het bewaken van** knop.

3. In de **opties voor Microsoft Azure-logboekregistratie** in het dialoogvenster, klikt u op **alle logboeken**, en klik vervolgens op **OK**.

    ![Alle logboeken bewaken](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)

4. In de adresbalk van het browservenster geopend, kunt u een extra teken toevoegen aan de URL om te leiden tot een 404-fout (bijvoorbeeld `http://localhost:53370/Home/Contactx`), en druk op Enter.

    Na enkele seconden, het logboek gedetailleerde fout wordt weergegeven in de Visual Studio **uitvoer** venster.

    ![Gedetailleerde foutenlogboek - venster uitvoer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Beheren en klik op de koppeling voor de logboekuitvoer opgemaakt in een browser:

    ![Gedetailleerde foutenlogboek - browservenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Bestand systeemlogboeken downloaden
Alle logboeken die u kunt controleren in de **uitvoer** venster kan ook worden gedownload als een *.zip* bestand.

1. In de **uitvoer** venster, klikt u op **Streaminglogboeken downloaden**.

    ![Logboeken tabblad knoppen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Verkenner wordt geopend op uw *Downloads* map met het gedownloade bestand geselecteerd.

    ![Gedownload bestand](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Pak de *.zip* bestand ziet u de volgende mapstructuur:

    ![Gedownload bestand](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Logboeken voor tracering van toepassing zijn *.txt* bestanden in de *LogFiles\Application* map.
   * Webserverlogboeken zijn *.log* bestanden in de *LogFiles\http\RawLogs* map. U kunt een hulpprogramma zoals [Logboekparser](https://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) weergeven en bewerken van deze bestanden.
   * Gedetailleerde fout bericht logboekbestanden *.html* bestanden in de *LogFiles\DetailedErrors* map.

     (De *implementaties* map voor bestanden die zijn gemaakt door bronbeheer publiceren; is geen zaken die betrekking hebben op Visual Studio publiceren. De *Git* map traceringen die betrekking hebben op broncodebeheer wordt publiceren en het logboek voor file-service voor streaming.)  

<!-- ## <a name="storagelogs"></a>View storage logs
Application tracing logs can also be sent to an Azure storage account, and you can view them in Visual Studio. To do that you'll create a storage account, enable storage logs in the Azure portal, and view them in the **Logs** tab of the **Azure Web App** window.

You can send logs to any or all of three destinations:

* The file system.
* Storage account tables.
* Storage account blobs.

You can specify a different severity level for each destination.

Tables make it easy to view details of logs online, and they support streaming; you can query logs in tables and see new logs as they are being created. Blobs make it easy to download logs in files and to analyze them using HDInsight, because HDInsight knows how to work with blob storage. For more information, see **Hadoop and MapReduce** in [Data Storage Options (Building Real-World Cloud Apps with Azure)](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

You currently have file system logs set to verbose level; the following steps walk you through setting up information level logs to go to storage account tables. Information level means all logs created by calling `Trace.TraceInformation`, `Trace.TraceWarning`, and `Trace.TraceError` will be displayed, but not logs created by calling `Trace.WriteLine`.

Storage accounts offer more storage and longer-lasting retention for logs compared to the file system. Another advantage of sending application tracing logs to storage is that you get some additional information with each log that you don't get from file system logs.

1. Right-click **Storage** under the Azure node, and then click **Create Storage Account**.

![Create Storage Account](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. In the **Create Storage Account** dialog, enter a name for the storage account.

    The name must be must be unique (no other Azure storage account can have the same name). If the name you enter is already in use you'll get a chance to change it.

    The URL to access your storage account will be *{name}*.core.windows.net.
2. Set the **Region or Affinity Group** drop-down list to the region closest to you.

    This setting specifies which Azure datacenter will host your storage account. For this tutorial your choice won't make a noticeable difference, but for a production web app you want your web server and your storage account to be in the same region to minimize latency and data egress charges. The web app (which you'll create later) should run in a region as close as possible to the browsers accessing your web app in order to minimize latency.
3. Set the **Replication** drop-down list to **Locally redundant**.
   
    When geo-replication is enabled for a storage account, the stored content is replicated to a secondary datacenter to enable failover to that location in case of a major disaster in the primary location. Geo-replication can incur additional costs. For test and development accounts, you generally don't want to pay for geo-replication. For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).
4. Click **Create**.

    ![New storage account](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. In the Visual Studio **Azure Web App** window, click the **Logs** tab, and then click **Configure Logging in Management Portal**.

     <!-- todo:screenshot of new portal if the VS page link goes to new portal -- >
    ![Configure logging](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    This opens the **Configure** tab in the portal for your web app.
6. In the portal's **Configure** tab, scroll down to the application diagnostics section, and then change **Application Logging (Table Storage)** to **On**.
7. Change **Logging Level** to **Information**.
8. Click **Manage Table Storage**.

    ![Click Manage TableStorage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    In the **Manage table storage for application diagnostics** box, you can choose your storage account if you have more than one. You can create a new table or use an existing one.

    ![Manage table storage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. In the **Manage table storage for application diagnostics** box, click the check mark to close the box.
10. In the portal's **Configure** tab, click **Save**.
11. In the browser window that displays the application web app, click **Home**, then click **About**, and then click **Contact**.

     The logging information produced by browsing these web pages is written to the storage account.
12. In the **Logs** tab of the **Azure Web App** window in Visual Studio, click **Refresh** under **Diagnostic Summary**.

     ![Click Refresh](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     The **Diagnostic Summary** section shows logs for the last 15 minutes by default. You can change the period to see more logs.

     (If you get a "table not found" error, verify that you browsed to the pages that do the tracing after you enabled **Application Logging (Storage)** and after you clicked **Save**.)

     ![Storage logs](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     Notice that in this view you see **Process ID** and **Thread ID** for each log, which you don't get in the file system logs. You can see additional fields by viewing the Azure storage table directly.
13. Click **View all application logs**.

     The trace log table appears in the Azure storage table viewer.

     (If you get a "sequence contains no elements" error, open **Server Explorer**, expand the node for your storage account under the **Azure** node, and then right-click **Tables** and click **Refresh**.)

     ![Storage logs in table view](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     This view shows additional fields you don't see in any other views. This view also enables you to filter logs by using special Query Builder UI for constructing a query. For more information, see Working with Table Resources - Filtering Entities in [Browsing Storage Resources with Server Explorer](https://msdn.microsoft.com/library/ff683677.aspx).
14. To look at the details for a single row, double-click one of the rows.

     ![Trace table in Server Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)
 -->
## <a name="failedrequestlogs"></a>Logboeken voor tracering van mislukte aanvragen weergeven
Logboeken voor tracering van mislukte aanvragen zijn nuttig wanneer u nodig hebt om te begrijpen van de details van de manier waarop IIS een HTTP-aanvraag in scenario's zoals bij problemen de verificatie of voor het herschrijven van URL verwerkt.

App Service-apps gebruiken dezelfde mislukte aanvragen traceren functionaliteit die beschikbaar zijn in IIS 7.0 en hoger is. U hebt geen toegang tot de IIS-instellingen die welke fouten geregistreerd configureren, maar. Wanneer u een tracering van mislukte aanvragen inschakelt, worden alle fouten worden vastgelegd.

U kunt de tracering van mislukte aanvragen inschakelen met behulp van Visual Studio, maar u kunt ze niet weergeven in Visual Studio. Deze logboeken zijn XML-bestanden. De streaming log-service controleert alleen bestanden die als leesbare in tekst zonder opmaak modus worden beschouwd: *.txt*, *.html*, en *.log* bestanden.

U kunt Logboeken voor tracering van mislukte aanvragen kunt weergeven in een browser rechtstreeks via FTP of lokaal na een FTP-hulpprogramma gebruiken om deze te downloaden naar uw lokale computer. In deze sectie bekijkt u deze in een browser rechtstreeks.

1. In de **configuratie** tabblad van de **Azure Web App** venster dat u hebt geopend vanuit **Server Explorer**, wijzigen **tracering van mislukte aanvragen** naar **Op**, en klik vervolgens op **opslaan**.

    ![Tracering van mislukte aanvragen inschakelen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Een extra teken toevoegen aan de URL in de adresbalk van het browservenster waarin de app, en klikt u op Enter om te leiden tot een 404-fout.

    Dit zorgt ervoor dat een logboek voor tracering van mislukte aanvragen worden gemaakt en de volgende stappen laten zien hoe weergeven of downloaden van het logboek.

3. In Visual Studio in de **configuratie** tabblad van de **Azure Web App** venster, klikt u op **geopend in de beheerportal**.

4. In de [Azure-portal](https://portal.azure.com) **instellingen** pagina voor uw app, klikt u op **implementatiereferenties**, en voer vervolgens een nieuwe gebruikersnaam en wachtwoord.

    ![Nieuwe FTP-gebruikersnaam en wachtwoord](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    > [!NOTE]
    > Wanneer u zich aanmeldt, hebt u de volledige gebruikersnaam gebruiken met de naam van de app als voorvoegsel toegevoegd aan het. Bijvoorbeeld, als u "myid" Als de naam van een gebruiker invoeren en de site 'myexample' is, aanmelden u als 'myexample\myid'.
    >

5. Ga in een nieuw browservenster naar de URL die wordt weergegeven onder **FTP-hostnaam** of **FTPS-hostnaam** in de **overzicht** pagina voor uw app.

6. Meld u met behulp van de FTP-referenties die u eerder hebt (met inbegrip van het voorvoegsel van de app voor de gebruikersnaam) hebt gemaakt.

    De browser ziet u de hoofdmap van de app.

7. Open de *logboekbestanden* map.

    ![Open de map LogFiles](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)

8. Open de map met de naam W3SVC plus een numerieke waarde.

    ![Open W3SVC folder](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    De map bevat de XML-bestanden voor eventuele fouten die zijn geregistreerd nadat u de tracering van mislukte aanvragen ingeschakeld en een XSL-bestand dat een browser kunt gebruiken om de opmaak van het XML-bestand.

    ![W3SVC-map](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)

9. Klik op het XML-bestand voor de mislukte aanvragen die u wilt zien van traceringsgegevens voor.

    De volgende afbeelding ziet u deel uit van de traceringsgegevens voor een voorbeeld-fout.

    ![Tracering van mislukte aanvragen in browser](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Volgende stappen
U hebt gezien hoe u Visual Studio gemakkelijk om logboeken die zijn gemaakt door een App Service-app weer te geven. De volgende secties vindt u koppelingen naar meer bronnen op Verwante onderwerpen:

* App Service oplossen
* Foutopsporing in Visual Studio
* Externe foutopsporing in Azure
* Traceren in ASP.NET-toepassingen
* Analyseren van webserverlogboeken
* Analyse van Logboeken voor mislukte aanvragen traceren
* Debugging Cloud Services

### <a name="app-service-troubleshooting"></a>App Service oplossen
Zie de volgende bronnen voor meer informatie over het oplossen van apps in Azure App Service:

* [Apps controleren](web-sites-monitor.md)
* [Onderzoeken geheugenlekken in Azure App Service met Visual Studio 2013](https://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Microsoft ALM-blogbericht over Visual Studio-functies voor het analyseren van beheerd geheugenproblemen.
* [Azure App Service-onlineprogramma's, moet u weten over](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Blogbericht Amit Apple.

Voor hulp bij een specifieke vraag voor het oplossen van problemen, een thread te starten in een van de volgende forums:

* [De Azure-forum op de ASP.NET-website](https://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [De Azure-forum op MSDN](https://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](https://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Foutopsporing in Visual Studio
Zie voor meer informatie over het gebruik van foutopsporingsmodus in Visual Studio [foutopsporing in Visual Studio](https://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) en [Tips voor fouten opsporen met Visual Studio 2010](https://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

### <a name="remote-debugging-in-azure"></a>Externe foutopsporing in Azure
Zie de volgende bronnen voor meer informatie over foutopsporing op afstand voor App Service-apps en WebJobs:

* [Inleiding tot externe foutopsporing, Azure App Service](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Inleiding tot externe foutopsporing in Azure App Service deel 2 - binnen foutopsporing op afstand](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Inleiding tot de externe foutopsporing op Azure App Service-deel 3 - omgeving met meerdere exemplaren en GIT](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [WebJobs foutopsporing (video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Als uw app gebruikmaakt van een Azure-Web-API of Mobile Services back-end en u moet daarop foutopsporing uitvoeren, Zie [.NET back-end in Visual Studio foutopsporing](https://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>Traceren in ASP.NET-toepassingen
Er zijn geen grondige en up-to-date introductie van ASP.NET-tracering beschikbaar op het Internet. Het beste kunt u doen is aan de slag met oude inleidende materialen die zijn geschreven voor Web Forms omdat MVC niet nog bestaat en die met nieuwere blog aanvulling zijn op alle nieuwe die gericht op specifieke problemen berichten. Een goede plaats om te starten, zijn de volgende bronnen:

* [Controle en telemetrie (Building Real-World Cloudapps with Azure)](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  E-book hoofdstuk met aanbevelingen voor tracering in Azure-cloud-toepassingen.
* [ASP.NET-tracering](https://msdn.microsoft.com/library/ms972204.aspx)<br/>
  Oude maar nog steeds een goede bron voor een algemene inleiding tot aan het onderwerp.
* [Traceer-Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  Informatie over traceer-listeners, maar wordt niet vermeld de [WebPageTraceListener](https://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx).
* [Walkthrough: Tracering van ASP.NET integreren met System.Diagnostics tracering](https://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  In dit artikel is ook oude, maar bevat aanvullende informatie die niet wordt behandeld in het inleidende artikel.
* [Traceren in ASP.NET MVC Razor weergaven](https://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Naast de tracering in Razor weergaven, het bericht ook wordt uitgelegd hoe u een fout-filter maken om aan te melden van alle niet-verwerkte uitzonderingen in een MVC-toepassing. Zie voor informatie over hoe u zich aanmeldt alle niet-verwerkte uitzonderingen in een Web Forms-toepassing in het voorbeeld Global.asax in [compleet voorbeeld voor Foutenhandlers](https://msdn.microsoft.com/library/bb397417.aspx) op MSDN. In MVC of Web Forms, als u wilt vastleggen van bepaalde uitzonderingen, maar laat de standaard-framework verwerking wordt pas van kracht voor, kunt u variabel en foutparameters zoals in het volgende voorbeeld:

``` c#
try
{
   // Your code that might cause an exception to be thrown.
}
catch (Exception ex)
{
    Trace.TraceError("Exception: " + ex.ToString());
    throw;
}
```

* [Streaming diagnostische traceerlogboeken van de Azure Command Line (plus glimp!)](https://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Het gebruik van de opdrachtregel doen wat in deze zelfstudie laat zien hoe doen in Visual Studio. [Glimp](https://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) is een hulpprogramma voor foutopsporing van ASP.NET-toepassingen.
* [Met behulp van WebApps-logboekregistratie en diagnose - met David Ebbo](https://azure.microsoft.com/documentation/videos/azure-web-site-logging-and-diagnostics/) en [Streaminglogboeken van Web-Apps - met David Ebbo](https://azure.microsoft.com/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Video's met Scott Hanselman en David Ebbo.

Voor logboekregistratie van fouten, een alternatief voor het schrijven van uw eigen code tracering is een open-source logboekregistratie-framework gebruikt zoals [ELMAH](https://nuget.org/packages/elmah/). Zie voor meer informatie, [blogberichten van Scott Hanselman over ELMAH](https://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Moet u ook geen gebruik van ASP.NET of `System.Diagnostics` voor het ophalen van streaming-logboeken van Azure. De App Service-app streamen log-service een streamt *.txt*, *.html*, of *.log* -bestand dat is gevonden in de *logboekbestanden* map. Daarom kunt u uw eigen systeem logboekregistratie die worden geschreven naar het bestandssysteem van de app en het bestand automatisch wordt gestreamd en gedownload. U moet doen, is toepassingscode schrijven waarmee bestanden in de *d:\home\logfiles* map.

### <a name="analyzing-web-server-logs"></a>Analyseren van webserverlogboeken
Zie de volgende bronnen voor meer informatie over het analyseren van weblogboeken server:

* [LogParser](https://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Een hulpprogramma voor het weergeven van gegevens in webserverlogboeken (*.log* bestanden).
* [Het oplossen van prestatieproblemen voor IIS of met behulp van LogParser toepassingsfouten](https://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Een inleiding tot het hulpprogramma Logboekparser die u gebruiken kunt voor het analyseren van weblogboeken-server.
* [Blog van Robert McMurray over het gebruik van LogParser plaatst.](https://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [De HTTP-statuscode in IIS 7.0, IIS 7.5 en IIS 8.0](https://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Analyse van Logboeken voor mislukte aanvragen traceren
De Microsoft TechNet-website bevat een [met behulp van tracering van mislukte aanvragen](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing) sectie, die mogelijk nuttig zijn voor informatie over het gebruik van deze logboeken. Maar deze documentatie richt zich voornamelijk op de tracering van mislukte aanvragen configureren in IIS, die u niet in Azure App Service doen.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: https://github.com/Azure/azure-webjobs-sdk/wiki

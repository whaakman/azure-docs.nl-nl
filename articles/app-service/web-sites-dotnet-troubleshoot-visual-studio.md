---
title: Een web-app in Azure App Service met behulp van Visual Studio oplossen
description: Informatie over het oplossen van problemen met een Azure-web-app met behulp van foutopsporing op afstand, traceren en logboekregistratie-hulpprogramma's die zijn ingebouwd in Visual Studio 2013.
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: 
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: 1e3aff1898665c834a70e6c49f23e408a508b10a
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="troubleshoot-a-web-app-in-azure-app-service-using-visual-studio"></a>Een web-app in Azure App Service met behulp van Visual Studio oplossen
## <a name="overview"></a>Overzicht
Deze zelfstudie laat zien hoe u Visual Studio-hulpprogramma gebruiken om u te helpen bij foutopsporing van een web-app in [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), door te voeren in [foutopsporingsmodus](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) op afstand of door toepassingslogboeken en webserverlogboeken te bekijken.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

U leert het volgende:

* Welke Azure-web-app-beheerfuncties zijn beschikbaar in Visual Studio.
* Het gebruik van Visual Studio externe weergave snel wijzigingen aanbrengen in een externe web-app.
* Het uitvoeren van de foutopsporingsmodus op afstand tijdens een project wordt uitgevoerd in Azure, voor een web-app en een webtaak.
* Logboeken met traceringen maken en deze te bekijken wanneer de toepassing is ze worden gemaakt.
* Het weergeven van webserverlogboeken, met inbegrip van gedetailleerde foutberichten en tracering van mislukte aanvragen.
* Het verzenden van diagnostische logboeken naar een Azure Storage-account en ze er weergeven.

Als u Visual Studio Ultimate hebt, kunt u ook gebruiken [IntelliTrace](http://msdn.microsoft.com/library/vstudio/dd264915.aspx) voor foutopsporing. IntelliTrace wordt niet behandeld in deze zelfstudie.

## <a name="prerequisites"></a>Vereisten
Deze zelfstudie werkt met de ontwikkelomgeving, het webproject en de Azure-web-app die u hebt ingesteld in [aan de slag met Azure en ASP.NET][GetStarted]. Voor de secties WebJobs, moet u de toepassing die u maakt in [aan de slag met de Azure WebJobs SDK][GetStartedWJ].

De codevoorbeelden in deze zelfstudie wordt weergegeven, zijn voor een C# MVC-webtoepassing, maar de procedures voor probleemoplossing zijn hetzelfde voor Visual Basic en Web Forms-toepassingen.

De zelfstudie wordt ervan uitgegaan dat u Visual Studio 2017. 

De streaminglogboeken functie werkt alleen voor toepassingen die zijn gericht op .NET Framework 4 of hoger.

## <a name="sitemanagement"></a>Web-app-configuratie en beheer
Visual Studio biedt toegang tot een subset van de web-app-beheerfuncties en configuratie-instellingen die beschikbaar zijn in de [Azure-portal](http://go.microsoft.com/fwlink/?LinkId=529715). In deze sectie ziet u wat er beschikbaar is via **Server Explorer**. Overzicht van de nieuwste integratie van Azure-functies uitproberen **Cloud Explorer** ook. U kunt openen beide windows uit de **weergave** menu.

1. Als u nog niet hebt aangemeld bij Azure in Visual Studio, met de rechtermuisknop **Azure** en selecteer Connect to **Microsoft Azure-abonnement** in **Server Explorer**.

    Een alternatief is voor het installeren van een beheercertificaat waarmee u toegang hebt tot je account. Als u ervoor kiest om een certificaat te installeren, met de rechtermuisknop op de **Azure** knooppunt in **Server Explorer**, en selecteer vervolgens **beheren en Filter abonnementen** in het contextmenu. In de **Microsoft Azure-abonnementen beheren** in het dialoogvenster, klikt u op de **certificaten** tabblad en klik vervolgens op **importeren**. Volg de instructies voor het downloaden en vervolgens een abonnementsbestand importeren (ook wel een *.publishsettings* bestand) voor uw Azure-account.

   > [!NOTE]
   > Als u een abonnementsbestand hebt gedownload, opslaan in een map buiten uw adreslijsten source code (bijvoorbeeld in de map Downloads) en verwijderen wanneer het importeren is voltooid. Een kwaadwillende gebruiker die toegang tot het abonnementsbestand krijgt kunt bewerken, maken en verwijderen van uw Azure-services.
   >
   >

    Zie voor meer informatie over verbinding maken met Azure-resources vanuit Visual Studio [Accounts beheren, abonnementen en beheerdersrollen](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. In **Server Explorer**, vouw **Azure** en vouw **App Service**.
3. Vouw de resourcegroep met de web-app die u hebt gemaakt in [een ASP.NET-web-app maken in Azure][app-service-web-get-started-dotnet.md], en vervolgens met de rechtermuisknop op het web-app-knooppunt en klik op **weergave-instellingen**.

    ![Instellingen weergeven in Server Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    De **Azure-Web-App** tabblad wordt weergegeven en kunt u zien er de web-app management- en configuratietaken die beschikbaar in Visual Studio zijn.

    ![Azure-Web-App-venster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    In deze zelfstudie gebruikt u de logboekregistratie en tracering vervolgkeuzelijsten. U zult ook gebruiken voor foutopsporing op afstand, maar u een andere methode in te schakelen.

    Zie voor meer informatie over de App-instellingen en verbindingsreeksen in vakken in dit venster [Azure Web Apps: tekenreeksen van toepassingen en verbindingsreeksen](https://azure.microsoft.com/blog/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).

    Als u een web-app management taak uitvoeren die in dit venster kan niet worden uitgevoerd wilt, klikt u op **openen in de beheerportal** een browservenster om de Azure-portal te openen.

## <a name="remoteview"></a>Toegang tot web-app-bestanden in Server Explorer
U doorgaans een webproject met implementeert de `customErrors` vlag in het bestand Web.config is ingesteld op `On` of `RemoteOnly`, wat betekent dat er geen een handig foutbericht als er iets mis gaat. Voor veel fouten is alle dat u een pagina zoals een van de volgende waarden:

**Serverfout in '/' toepassing:**

![Onnuttige foutpagina](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Er is een fout opgetreden:**

![Onnuttige foutpagina](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**De pagina weergeven niet**

![Onnuttige foutpagina](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Vaak is de eenvoudigste manier om de oorzaak van de fout te achterhalen om in te schakelen gedetailleerde foutberichten worden weergegeven, omdat de eerste dag van de vorige schermafbeeldingen daarin wordt uitgelegd hoe u kunt doen. Waarvoor een wijziging in het geïmplementeerde Web.config-bestand is vereist. U kunt bewerken de *Web.config* bestand in het project en implementeer het project opnieuw of maak een [Web.config-transformatie](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) en implementeren van een foutopsporingsversie, maar er is een snellere manier: in **Solution Explorer** , u rechtstreeks kunt bekijken en bewerken van bestanden in de externe web-app met behulp van de *externe weergave* functie.

1. In **Server Explorer**, vouw **Azure**, vouw **App Service**, vouwt u de resourcegroep die zich in uw web-app uit en vouw vervolgens het knooppunt voor uw web-app.

    Ziet u knooppunten waarmee u toegang tot de inhoudsbestanden en logboekbestanden van de web-app.
2. Vouw de **bestanden** knooppunt uit en dubbelklik op de *Web.config* bestand.

    ![Open Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio, opent het bestand Web.config van de externe web-app en [afstand] bevat naast de bestandsnaam in de titelbalk.
3. Voeg de volgende regel om de `system.web` element:

    `<customErrors mode="Off"></customErrors>`

    ![Web.config bewerken](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Vernieuw de browser die de onnuttige foutbericht wordt weergegeven en u nu een gedetailleerd foutbericht, zoals in het volgende voorbeeld:

    ![Gedetailleerd foutbericht](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (De fout weergegeven is gemaakt door het toevoegen van de regel wordt weergegeven in rood naar *Views\Home\Index.cshtml*.)

Bewerken van het bestand Web.config is slechts één voorbeeld van scenario's waarin de mogelijkheid om te lezen en bewerken van bestanden op uw Azure-web-app maken probleemoplossing eenvoudiger.

## <a name="remotedebug"></a>Externe foutopsporing web-apps
Als het gedetailleerde foutbericht niet voldoende informatie geven, en u kan niet opnieuw met de fout lokaal maken, wordt een andere manier om op te lossen is extern worden uitgevoerd in de foutopsporingsmodus. U kunt onderbrekingspunten instellen, geheugen rechtstreeks te manipuleren, analyseer code en zelfs de codepad wijzigen.

Foutopsporing op afstand werkt niet in de Express-edities van Visual Studio.

Deze sectie wordt beschreven hoe u foutopsporing op afstand met behulp van het project dat u maakt in [maken van een ASP.NET-web-app in Azure][app-service-web-get-started-dotnet.md].

1. Open het webproject dat u hebt gemaakt in [maken van een ASP.NET-web-app in Azure][app-service-web-get-started-dotnet.md].

2. Open *Controllers\HomeController.cs*.

3. Verwijder de `About()` methode en plaats de volgende in plaats daarvan code.

        public ActionResult About()
        {
            string currentTime = DateTime.Now.ToLongTimeString();
            ViewBag.Message = "The current time is " + currentTime;
            return View();
        }
4. [Stel een onderbrekingspunt in](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) op de `ViewBag.Message` regel.

5. In **Solution Explorer**, met de rechtermuisknop op het project en klik op **publiceren**.

6. In de **profiel** vervolgkeuzelijst, Selecteer hetzelfde profiel dat u gebruikt in [maken van een ASP.NET-web-app in Azure][app-service-web-get-started-dotnet.md]. Klik op instellingen.

7. In de **publiceren** dialoogvenster, klikt u op de **instellingen** tabblad en wijzig vervolgens de **configuratie** naar **Debug**, en klik vervolgens op  **Sla**.

    ![In de foutopsporingsmodus publiceren](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)

8. Klik op **Publish**. Na de implementatie is voltooid en de browser wordt geopend met de Azure-URL van uw web-app, sluit de browser.

9. In **Server Explorer**, met de rechtermuisknop op uw web-app en klik vervolgens op **foutopsporingsprogramma koppelen**.

    ![Foutopsporingsprogramma koppelen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    De browser wordt automatisch geopend op uw startpagina in Azure wordt uitgevoerd. U moet wellicht 20 seconden geduld terwijl Azure u de server voor foutopsporing stelt. Deze vertraging alleen de eerste keer dat u in de foutopsporingsmodus op een web-app in een periode van 48 uur uitvoert wordt uitgevoerd. Als u foutopsporing in dezelfde periode opnieuw start, moet u er een vertraging niet.

    > [!NOTE] 
    > Als er problemen het foutopsporingsprogramma wordt gestart, probeert te doen via **Cloud Explorer** in plaats van **Server Explorer**.
    >

10. Klik op **over** in het menu.

     Visual Studio op het onderbrekingspunt gestopt en de code wordt uitgevoerd in Azure, niet op uw lokale computer.

11. Beweeg de muisaanwijzer over de `currentTime` variabele om te zien van de tijdwaarde.

     ![De variabele in de foutopsporingsmodus in Azure wordt uitgevoerd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

     De tijd die u ziet is de tijd Azure-server, die mogelijk in een andere tijdzone dan uw lokale computer.

12. Voer een nieuwe waarde voor de `currentTime` variabele, zoals 'Wordt nu uitgevoerd in Azure'.

13. Druk op F5 om door te gaan die wordt uitgevoerd.

     De Info pagina uitgevoerd in Azure wordt de nieuwe waarde die u hebt ingevoerd in de variabele currentTime.

     ![Over de pagina met de nieuwe waarde](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a>Externe foutopsporing WebJobs
Deze sectie wordt beschreven hoe u foutopsporing op afstand via de project en de web-app die u maakt in [aan de slag met de Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).

De functies in deze sectie wordt weergegeven, zijn alleen beschikbaar in Visual Studio 2013 met Update 4 of hoger.

Foutopsporing op afstand werkt alleen met doorlopende webtaken. Geplande en on-demand WebJobs foutopsporing worden niet ondersteund.

1. Open het webproject dat u hebt gemaakt in [aan de slag met de Azure WebJobs SDK][GetStartedWJ].

2. Open in het project ContosoAdsWebJob *Functions.cs*.

3. [Stel een onderbrekingspunt in](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) op de eerste instructie in de `GnerateThumbnail` methode.

    ![Onderbrekingspunt instellen](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)

4. In **Solution Explorer**, met de rechtermuisknop op het webproject (niet op het project webtaak) en klik op **publiceren**.

5. In de **profiel** vervolgkeuzelijst, Selecteer hetzelfde profiel dat u gebruikt in [aan de slag met de Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).

6. Klik op de **instellingen** tabblad en wijzig **configuratie** naar **Debug**, en klik vervolgens op **publiceren**.

    Visual Studio implementeert het web en webtaak projecten en uw browser geopend met de Azure-URL van uw web-app.

7. In **Server Explorer**, vouw **Azure > App Service > uw resourcegroep > uw web-app > WebJobs > doorlopend**, en klik vervolgens met de rechtermuisknop op **ContosoAdsWebJob**.

8. Klik op **foutopsporingsprogramma koppelen**.

    ![Foutopsporingsprogramma koppelen](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    De browser wordt automatisch geopend op uw startpagina in Azure wordt uitgevoerd. U moet wellicht 20 seconden geduld terwijl Azure u de server voor foutopsporing stelt. Deze vertraging alleen de eerste keer dat u in de foutopsporingsmodus op een web-app in een periode van 48 uur uitvoert wordt uitgevoerd. Als u foutopsporing in dezelfde periode opnieuw start, moet u er een vertraging niet.

9. In de webbrowser die naar de startpagina van Contoso Ads wordt geopend, maakt u een nieuwe ad.

    Maken van een ad zorgt ervoor dat een wachtrijbericht worden gemaakt, die is opgenomen door de webtaak en verwerkt. De WebJobs SDK roept de functie voor het bericht uit de wachtrij, het code-treffers verwerken wanneer uw onderbrekingspunt.

10. Wanneer het foutopsporingsprogramma op uw onderbrekingspunt onderbreekt, kunt u onderzoeken en variabelewaarden wijzigen terwijl het programma voor de cloud wordt uitgevoerd. In de volgende afbeelding ziet u het foutopsporingsprogramma de inhoud van het blobInfo-object dat is doorgegeven aan de `GenerateThumbnail` methode.

     ![blobInfo-object in het foutopsporingsprogramma](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)

11. Druk op F5 om door te gaan die wordt uitgevoerd.

     De `GenerateThumbnail` methode is voltooid met het maken van de miniatuur.

12. Vernieuw de pagina Index in de browser en ziet u de miniatuur.

13. In Visual Studio, drukt u op SHIFT + F5 om te stoppen met het opsporen van fouten.

14. In **Server Explorer**, met de rechtermuisknop op het knooppunt ContosoAdsWebJob en klik op **weergave Dashboard**.

15. Meld u aan met uw Azure-referenties en klik op de naam van een webtaak naar de pagina voor uw webtaak.

     ![Klik op ContosoAdsWebJob](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Het Dashboard ziet u dat de `GenerateThumbnail` functie onlangs is uitgevoerd.

     (De volgende keer dat u op **weergave Dashboard**, u hoeft aan te melden en de browser wordt rechtstreeks naar de pagina voor uw webtaak.)

16. Klik op de naam van de functie voor informatie over de functie wordt uitgevoerd.

     ![Details van functie](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

Als de functie [Logboeken geschreven](https://github.com/Azure/azure-webjobs-sdk/wiki), kunt u op **ToggleOutput** te zien.

## <a name="notes-about-remote-debugging"></a>Opmerkingen over foutopsporing op afstand

* Uitgevoerd in de foutopsporingsmodus in productie wordt niet aanbevolen. Als uw productie-web-app niet naar meerdere exemplaren van server is uitgebreid, foutopsporing voorkomt u dat de webserver op andere aanvragen reageert. Als er meerdere exemplaren van web server, wanneer u aan het foutopsporingsprogramma koppelt, krijgt u een willekeurig exemplaar en u er niet toe om ervoor te zorgen dat toekomstige browseraanvragen gaat u naar hetzelfde exemplaar. Ook u niet een foutopsporingsversie doorgaans implementeren voor productie en optimalisatie van de compiler voor release builds mogelijk maken het onmogelijk om weer te geven wat er per regel gebeurt in de broncode. Voor het oplossen van problemen, is de beste bron toepassing tracering en web server-Logboeken.
* Lange wordt gestopt bij onderbrekingspunten wanneer externe voorkomen foutopsporing. Azure beschouwt een proces dat is langer dan een paar minuten als een niet-reagerende proces gestopt en wordt afgesloten.
* Terwijl u fouten opspoort, is de server verzenden van gegevens naar Visual Studio bandbreedte kosten kan beïnvloeden. Zie voor meer informatie over bandbreedte tarieven [prijzen van Azure](https://azure.microsoft.com/pricing/calculator/).
* Zorg ervoor dat de `debug` kenmerk van de `compilation` -element in de *Web.config* bestand is ingesteld op true. Deze waarde is ingesteld op standaard, wanneer u een configuratie van de build foutopsporing publiceert ingesteld op true.

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>
* Als u merkt dat het foutopsporingsprogramma niet stap in de code die u wilt opsporen, moet u wellicht de instelling voor alleen mijn Code wijzigen.  Zie voor meer informatie [beperken, selecteert u alleen mijn Code](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code).
* Wanneer u de functie voor externe foutopsporing inschakelen en de functie is na 48 uur automatisch uitgeschakeld, wordt er op de server een timer gestart. Deze limiet 48 uur wordt gedaan voor beveiliging en prestaties. U kunt eenvoudig de functie weer inschakelen als vaak is als u wilt. U wordt aangeraden deze uitgeschakeld wanneer u niet actief foutopsporing.
* U kunt het foutopsporingsprogramma handmatig koppelen aan een proces, niet alleen de web-app proces (w3wp.exe). Zie voor meer informatie over het gebruik van de foutopsporingsmodus in Visual Studio, [foutopsporing in Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Overzicht van diagnostische logboeken
Een ASP.NET-toepassing die wordt uitgevoerd in een Azure-web-app kunt maken van de volgende soorten logboeken:

* **Logboeken voor tracering van toepassing**<br/>
  De toepassing maakt deze logboeken door het aanroepen van methoden van de [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) klasse.
* **Web server-Logboeken**<br/>
  De webserver maakt een logboekvermelding voor elke HTTP-aanvraag voor de web-app.
* **Gedetailleerde foutenlogboeken voor bericht**<br/>
  De webserver maakt een HTML-pagina met aanvullende informatie voor mislukte HTTP-aanvragen (aanvragen die in statuscode 400 of hoger resulteren).
* **Logboeken voor tracering van aanvraag is mislukt**<br/>
  De webserver maakt een XML-bestand met gedetailleerde traceringsgegevens voor mislukte HTTP-aanvragen. De webserver biedt ook een XSL-bestand om te formatteren van de XML in een browser.

Logboekregistratie is van invloed op prestaties van web-app, zodat Azure u de mogelijkheid biedt-of uitschakelen van elk type logboek indien nodig. Toepassingslogboeken, kunt u opgeven dat alleen de logboeken boven een bepaalde mate van ernst worden geschreven. Wanneer u maakt een nieuwe web-app standaard alle logboekregistratie is uitgeschakeld.

Logboeken worden geschreven naar bestanden in een *logboekbestanden* map in het bestandssysteem van uw web-app en zijn toegankelijk via FTP. Webserverlogboeken en toepassingslogboeken kunnen ook worden geschreven naar een Azure Storage-account. U kunt een groter volume aan logboeken in een opslagaccount dan mogelijk in het bestandssysteem behouden. U kunt maximaal 100 MB van Logboeken wanneer u het bestandssysteem. (Alleen voor het bewaren van de korte termijn zijn bestand het systeemlogboek in Logboeken. Azure verwijdert oude logbestanden om ruimte te maken voor nieuwe nadat de limiet is bereikt.)  

## <a name="apptracelogs"></a>Maken en toepassing traceringslogboeken weergeven
In deze sectie kunt u de volgende taken uitvoeren:

* Voeg tracering instructies toe aan het webproject die u hebt gemaakt in [aan de slag met Azure en ASP.NET][GetStarted].
* Bekijk de logboeken wanneer u het project lokaal uitvoeren.
* Bekijk de logboeken als ze worden gegenereerd door de toepassing in Azure wordt uitgevoerd.

Zie voor informatie over het maken van de toepassing zich WebJobs aanmeldt, [het werken met Azure queue storage met behulp van de WebJobs SDK - het schrijven van logboeken](https://github.com/Azure/azure-webjobs-sdk/wiki). De volgende instructies voor het weergeven van Logboeken en hoe ze zijn opgeslagen in Azure beheren ook van toepassing op toepassingslogboeken gemaakt door WebJobs.

### <a name="add-tracing-statements-to-the-application"></a>Tracering instructies toevoegen aan de toepassing
1. Open *Controllers\HomeController.cs*, en vervang de `Index`, `About`, en `Contact` methoden met de volgende code om toe te voegen `Trace` instructies en een `using` -instructie voor `System.Diagnostics`:

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
2. Voeg een `using System.Diagnostics;` instructie naar de bovenkant van het bestand.

### <a name="view-the-tracing-output-locally"></a>De traceringsuitvoer lokaal weergeven
1. Druk op F5 om de toepassing uitvoeren in de foutopsporingsmodus.

    De standaard traceringslistener schrijft alle traceeruitvoer naar de **uitvoer** venster, samen met andere foutopsporingsuitvoer. De volgende afbeelding ziet u de uitvoer van de trace-instructies die u hebt toegevoegd aan de `Index` methode.

    ![Tracering in het venster Foutopsporing](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    De volgende stappen laten zien hoe trace-uitvoer in een webpagina te bekijken zonder het compileren van in de foutopsporingsmodus.
2. Open het bestand Web.config (die zich in de projectmap) en voeg toe een `<system.diagnostics>` element aan het einde van het bestand net voordat de afsluiting `</configuration>` element:

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

    De `WebPageTraceListener` Hiermee kunt u weergeven door te bladeren naar uitvoer van traceringen `/trace.axd`.
3. Voeg een <a href="http://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">trace-element</a> onder `<system.web>` in het Web.config-bestand, zoals in het volgende voorbeeld:

        <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
4. Druk op CTRL + F5 om de toepassing uit te voeren.
5. Voeg in de adresbalk van het browservenster *trace.axd* in de URL en druk op Enter (de URL is vergelijkbaar met http://localhost:53370/trace.axd).
6. Op de **toepassing Trace** pagina, klikt u op **Details weergeven** op de eerste regel (niet de BrowserLink).

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    De **aanvraaggegevens** pagina wordt weergegeven, en in de **traceringsinformatie** sectie u de uitvoer van de trace-instructies die u hebt toegevoegd ziet aan de `Index` methode.

    ![trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Standaard `trace.axd` alleen lokaal beschikbaar is. Als u maken van een externe web-app beschikbaar wilt, zou u `localOnly="false"` naar de `trace` -element in de *Web.config* -bestand, zoals wordt weergegeven in het volgende voorbeeld:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Inschakelen van echter `trace.axd` in een productie-web-app niet aanbevolen vanwege veiligheidsredenen. In de volgende secties ziet u een eenvoudigere manier om te lezen van Logboeken voor tracering in een Azure-web-app.

### <a name="view-the-tracing-output-in-azure"></a>De traceringsuitvoer van de weergeven in Azure
1. In **Solution Explorer**, met de rechtermuisknop op het webproject en klik op **publiceren**.
2. In de **webpublicatie** in het dialoogvenster, klikt u op **publiceren**.

    Nadat Visual Studio publiceert de update, wordt een browservenster naar uw startpagina geopend (ervan uitgaande dat u hebt niet wissen **doel-URL** op de **verbinding** tabblad).
3. In **Server Explorer**, met de rechtermuisknop op uw web-app en selecteer **Streaming logboeken bekijken**.

    ![Streaming-logboeken bekijken in het snelmenu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    De **uitvoer** venster bevat zijn verbonden met de streaming-log service en voegt een regel voor melding elke minuut dat gaat door zonder een logboek weer te geven.

    ![Streaming-logboeken bekijken in het snelmenu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. Klik in het browservenster waarin de startpagina van uw toepassing, op **Contact**.

    De uitvoer van het foutniveau traceren binnen een paar seconden u toegevoegd aan de `Contact` wordt weergegeven de **uitvoer** venster.

    ![Fouttracering in uitvoervenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Visual Studio is alleen-foutniveau traceringen weergegeven, omdat dat is de standaardinstelling wanneer u het logboek monitoring-service inschakelt. Wanneer u een nieuwe Azure-web-app maakt, is alle logboekregistratie standaard uitgeschakeld, zoals u hebt gezien wanneer u de instellingenpagina eerder geopend:

    ![Toepassing afmelden](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Echter, wanneer u hebt geselecteerd **Streaming logboeken bekijken**, Visual Studio automatisch gewijzigd **toepassing Logging(File System)** naar **fout**, wat betekent dat foutniveau logboeken ophalen gerapporteerd. Om uw logboeken voor tracering van alle zien, kunt u deze instelling om **uitgebreid**. Wanneer u een urgentieniveau lager is dan fout selecteert, worden ook alle logboeken voor hogere niveaus gemeld. Dus als u uitgebreide selecteert, ziet ook u informatie, waarschuwing en foutenlogboeken.  

1. In **Server Explorer**, met de rechtermuisknop op de web-app en klik vervolgens op **weergave-instellingen** zoals u eerder hebt gedaan.
2. Wijziging **toepassingslogboeken (bestandssysteem)** naar **uitgebreid**, en klik vervolgens op **opslaan**.

    ![Traceringsniveau wordt ingesteld op uitgebreid](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
3. In het browservenster waarin wordt nu weergegeven uw **Contact** pagina, klikt u op **Start**, klikt u vervolgens op **over**, en klik vervolgens op **Contact**.

    Binnen een paar seconden de **uitvoer** venster bevat alle van de traceringsuitvoer.

    ![Uitgebreide trace-uitvoer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    In deze sectie ingeschakeld en logboekregistratie met behulp van Azure-web-app-instellingen uitgeschakeld. U kunt ook inschakelen en uitschakelen van traceer-listeners door het wijzigen van het bestand Web.config. Wijzigen van het Web.config-bestand wordt echter het app-domein te recyclen, terwijl het inschakelen van logboekregistratie via de configuratie van de web-app niet doen. Als het probleem te reproduceren, lang duurt of wordt onderbroken, recycling van het app-domein mogelijk "corrigeren" en u verplichten om wacht totdat deze zich weer voordoet. Inschakelen van diagnostische gegevens in Azure, kunt u begint met het opnemen van informatie over de fout onmiddellijk zonder recycling van het app-domein.

### <a name="output-window-features"></a>Functies van het venster Output
De **Microsoft Azure Logs** tabblad van de **uitvoer** venster bevat verschillende knoppen en een tekstvak:

![Logboeken tabblad knoppen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Deze de volgende functies uitgevoerd:

* Schakel de **uitvoer** venster.
* In- of uitschakelen van automatische terugloop.
* Starten of stoppen van de bewaking van Logboeken.
* Geef op die zich aanmeldt om te controleren.
* Logboeken downloaden.
* Filteren op basis van een zoekopdracht of een reguliere expressie Logboeken.
* Sluit de **uitvoer** venster.

Als u een zoekreeks of standaardexpressie invoert, filtert Visual Studio logboekinformatie op de client. Dit betekent dat u kunt de criteria invoeren nadat de logboeken worden weergegeven in de **uitvoer** venster en u kunt filtercriteria wijzigen zonder de logboeken te genereren.

## <a name="webserverlogs"></a>Web server-logboeken bekijken
Webserverlogboeken Leg alle HTTP-activiteit voor de web-app. Om te zien in de **uitvoer** venster heeft, moet u deze inschakelen voor de web-app en Visual Studio Vertel dat u wilt bewaken.

1. In de **Azure Web App-configuratie** tabblad dat u hebt geopend vanuit **Server Explorer**, Web Server-logboekregistratie te wijzigen **op**, en klik vervolgens op **opslaan**.

    ![Web server-logboekregistratie inschakelen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. In de **uitvoer** venster, klikt u op de **Geef op welke Microsoft Azure voor het bewaken van logboeken** knop.

    ![Geef op welke Azure-logboeken bewaken](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. In de **opties voor Microsoft Azure-logboekregistratie** dialoogvenster, **Web server-logboeken**, en klik vervolgens op **OK**.

    ![Web server-logboeken bewaken](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. Klik in het browservenster waarin de web-app op **Start**, klikt u vervolgens op **over**, en klik vervolgens op **Contact**.

    De toepassingslogboeken van de in het algemeen weergegeven eerst, gevolgd door de web server-Logboeken. U moet wellicht Wacht enige tijd voor de logboeken worden weergegeven.

    ![Logboekbestanden van de webserver in het venster Output](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Standaard, wanneer u eerst webserverlogboeken inschakelen met behulp van Visual Studio schrijft Azure de logboeken naar het bestandssysteem. Als alternatief kunt kunt u de Azure-portal gebruiken om op te geven die webserver logboeken worden geschreven naar een blob-container in een opslagaccount.

Als u de portal gebruiken voor de webserver registratie in Azure storage-account inschakelen en schakel zijn logboekregistratie in Visual Studio wanneer u opnieuw inschakelen van logboekregistratie in Visual Studio de instellingen van uw storage-account hersteld.

## <a name="detailederrorlogs"></a>Gedetailleerde bericht foutenlogboeken weergeven
Gedetailleerde foutenlogboeken aanvullende informatie geven over HTTP-aanvragen die in fout reactiecodes (400 of hoger resulteren). Om te zien in de **uitvoer** venster die u moet deze inschakelen voor de web-app en Visual Studio Vertel dat u wilt bewaken.

1. In de **Azure Web App-configuratie** tabblad dat u hebt geopend vanuit **Server Explorer**, wijzigen **gedetailleerde foutberichten** naar **op**, en klik vervolgens op **opslaan**.

    ![Gedetailleerde foutberichten inschakelen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)

2. In de **uitvoer** venster, klikt u op de **Geef op welke Microsoft Azure voor het bewaken van logboeken** knop.

3. In de **opties voor Microsoft Azure-logboekregistratie** in het dialoogvenster, klikt u op **alle logboeken**, en klik vervolgens op **OK**.

    ![Alle logboeken bewaken](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)

4. In de adresbalk van het browservenster, kunt u een extra teken toevoegen naar de URL om te leiden tot een 404-fout (bijvoorbeeld `http://localhost:53370/Home/Contactx`), en druk op Enter.

    Na een paar seconden de gedetailleerd foutenlogboek wordt weergegeven in de Visual Studio **uitvoer** venster.

    ![Gedetailleerd foutenlogboek - venster uitvoer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Control + klik op de koppeling om de logboekuitvoer-opmaak in een browser te bekijken:

    ![Gedetailleerd foutenlogboek - browservenster](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Systeemlogboeken bestand downloaden
Alle logboeken die u kunt bewaken in de **uitvoer** venster kan ook worden gedownload als een *.zip* bestand.

1. In de **uitvoer** venster, klikt u op **Streaming logboeken downloaden**.

    ![Logboeken tabblad knoppen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Hiermee opent u Windows Verkenner naar uw *downloadt* map met het gedownloade bestand geselecteerd.

    ![Gedownloade bestand](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Pak de *.zip* bestand en u ziet u de volgende mapstructuur:

    ![Gedownloade bestand](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Logboeken voor tracering van toepassing zijn in *.txt* bestanden in de *LogFiles\Application* map.
   * Webserverlogboeken zijn *.log* bestanden in de *LogFiles\http\RawLogs* map. U kunt een hulpprogramma zoals [Log Parser](http://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) weergeven en bewerken van deze bestanden.
   * Bericht van gedetailleerde foutenlogboeken zijn in *.html* bestanden in de *LogFiles\DetailedErrors* map.

    (De *implementaties* map is voor bestanden die zijn gemaakt door het besturingselement voor publiceren; heeft geen iets verband houden met publiceren met Visual Studio. De *Git* map voor traceringen die betrekking hebben op de verbinding met broncodebeheer is publiceren en het logboek file streaming-service.)  

<!-- ## <a name="storagelogs"></a>View storage logs
Application tracing logs can also be sent to an Azure storage account, and you can view them in Visual Studio. To do that you'll create a storage account, enable storage logs in the Azure portal, and view them in the **Logs** tab of the **Azure Web App** window.

You can send logs to any or all of three destinations:

* The file system.
* Storage account tables.
* Storage account blobs.

You can specify a different severity level for each destination.

Tables make it easy to view details of logs online, and they support streaming; you can query logs in tables and see new logs as they are being created. Blobs make it easy to download logs in files and to analyze them using HDInsight, because HDInsight knows how to work with blob storage. For more information, see **Hadoop and MapReduce** in [Data Storage Options (Building Real-World Cloud Apps with Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

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

     This view shows additional fields you don't see in any other views. This view also enables you to filter logs by using special Query Builder UI for constructing a query. For more information, see Working with Table Resources - Filtering Entities in [Browsing Storage Resources with Server Explorer](http://msdn.microsoft.com/library/ff683677.aspx).
14. To look at the details for a single row, double-click one of the rows.

     ![Trace table in Server Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)
 -->
## <a name="failedrequestlogs"></a>Logboeken voor tracering van mislukte aanvragen weergeven
Logboeken voor tracering van mislukte aanvragen zijn nuttig wanneer u moet begrijpen hoe IIS een HTTP-aanvraag in scenario's zoals URL herschrijven of verificatie problemen verwerkt.

Azure-web-apps gebruiken de dezelfde traceringsfunctie voor mislukte aanvragen dat beschikbaar in IIS 7.0 en hoger is. U geen toegang tot de IIS-instellingen die welke fouten worden geregistreerd configureren, maar. Wanneer u tracering van mislukte aanvragen inschakelt, worden alle fouten worden vastgelegd.

U kunt tracering van mislukte aanvragen inschakelen met behulp van Visual Studio, maar u kunt ze niet weergeven in Visual Studio. Deze logboeken vormen een XML-bestanden. De streaming log-service controleert alleen bestanden die worden geacht leesbaar in tekstmodus zonder opmaak: *.txt*, *.html*, en *.log* bestanden.

U kunt de logboeken voor tracering van mislukte aanvragen weergeven in een browser rechtstreeks via de FTP- of lokaal na gebruik van een FTP-hulpprogramma om deze te downloaden naar uw lokale computer. In deze sectie bekijkt u ze in een browser rechtstreeks.

1. In de **configuratie** tabblad van de **Azure-Web-App** venster dat u hebt geopend vanuit **Server Explorer**, wijzigen **tracering van mislukte aanvragen** naar **op**, en klik vervolgens op **opslaan**.

    ![Tracering van mislukte aanvragen inschakelen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Een extra teken toevoegen aan de URL in de adresbalk van het browservenster waarin de web-app en klikt u op Enter als u een 404-fout.

    Dit zorgt ervoor dat een logboek voor tracering van mislukte aanvragen moet worden gemaakt en de volgende stappen laten zien hoe weergeven of downloaden van het logboek.

3. In Visual Studio in de **configuratie** tabblad van de **Azure-Web-App** venster, klikt u op **openen in de beheerportal**.

4. In de [Azure-portal](https://portal.azure.com) **instellingen** pagina voor uw web-app, klikt u op **implementatiereferenties**, en voer vervolgens een nieuwe gebruikersnaam en wachtwoord.

    ![Nieuwe FTP-gebruikersnaam en wachtwoord](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    > [!NOTE]
    > Wanneer u zich aanmeldt, hebt u de volledige gebruikersnaam met de naam van de web-app een voorvoegsel voor het gebruik. Bijvoorbeeld, als u "myid" als een gebruikersnaam invoeren en de site 'myexample' is, aanmelden u als 'myexample\myid'.
    >

5. In een nieuw browservenster en gaat u naar de URL die wordt weergegeven onder **FTP-hostnaam** of **FTPS hostnaam** in de **overzicht** pagina voor uw web-app.

6. Meld u aan met de FTP-referenties die u eerder (inclusief de web-app-voorvoegsel voor de gebruikersnaam) gemaakt.

    De browser ziet u de hoofdmap van de web-app.

7. Open de *logboekbestanden* map.

    ![Logboekbestanden openen](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)

8. Open de map met de naam W3SVC plus een numerieke waarde.

    ![Open de map W3SVC](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    De map bevat een XML-bestanden voor eventuele fouten die zijn geregistreerd nadat u de tracering van mislukte aanvragen ingeschakeld en een XSL-bestand dat een browser kunt gebruiken om het XML-bestand.

    ![W3SVC-map](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)

9. Klik op het XML-bestand voor de mislukte aanvraag die u wilt zien van traceringsgegevens voor.

    De volgende afbeelding ziet deel van de traceringsgegevens voor een voorbeeld-fout.

    ![Tracering van mislukte aanvragen in de browser](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Volgende stappen
U hebt gezien hoe Visual Studio kunt u gemakkelijk logboeken die zijn gemaakt door een Azure-web-app. De volgende secties vindt koppelingen naar meer bronnen op Verwante onderwerpen:

* Azure-web-app oplossen
* Foutopsporing in Visual Studio
* Externe foutopsporing in Azure
* Traceren in ASP.NET-toepassingen
* Analyseren van weblogboeken van server
* Analyseren van Logboeken voor tracering van mislukte aanvragen
* Foutopsporing van Cloud-Services

### <a name="azure-web-app-troubleshooting"></a>Azure-web-app oplossen
Zie de volgende bronnen voor meer informatie over het oplossen van web-apps in Azure App Service:

* [Web-apps bewaken](/manage/services/web-sites/how-to-monitor-websites/)
* [Onderzoeken geheugenlekken in Azure-Web-Apps met Visual Studio 2013](http://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Microsoft ALM blogbericht over de functies van Visual Studio voor het analyseren van problemen met het systeemgeheugen beheerd.
* [Azure-web-apps online hulpprogramma's u moet weten over](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Blogbericht Amit Apple.

Voor meer informatie over een specifieke vraag voor het oplossen van problemen, een thread in een van de volgende forums te starten:

* [Het Azure-forum op de site ASP.NET](http://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [Het Azure-forum op MSDN](http://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](http://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Foutopsporing in Visual Studio
Zie voor meer informatie over het gebruik van de foutopsporingsmodus in Visual Studio, [foutopsporing in Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) en [Tips foutopsporing met Visual Studio 2010](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

### <a name="remote-debugging-in-azure"></a>Externe foutopsporing in Azure
Zie de volgende bronnen voor meer informatie over foutopsporing op afstand voor Azure-web-apps en API's:

* [Inleiding tot Azure App Service WebApps foutopsporing extern](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Inleiding tot externe foutopsporing in Azure App Service Web Apps deel 2 - binnen foutopsporing op afstand](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Inleiding tot foutopsporing op afstand op Azure App Service Web Apps deel 3 - omgeving met meerdere exemplaren en GIT](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [WebJobs foutopsporing (video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Als uw web-app gebruikmaakt van een Azure-Web-API of Mobile Services back-end en u moet foutopsporing uitvoeren, Zie [.NET Backend foutopsporing in Visual Studio](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>Traceren in ASP.NET-toepassingen
Er zijn geen grondige en up-to-date introductie van ASP.NET-tracering beschikbaar op het Internet. Het beste kunt u doen is aan de slag met oude inleidende materialen die zijn geschreven voor webformulieren omdat MVC niet nog bestaat en dat met nieuwere blog vullen die gericht zijn op specifieke problemen boekt. Een goede plaats om te starten, zijn de volgende bronnen:

* [Bewaking en telemetrie (gebouw echte Cloud-Apps met Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  E-book hoofdstuk met aanbevelingen voor tracering in Azure-cloud-toepassingen.
* [ASP.NET-tracering](http://msdn.microsoft.com/library/ms972204.aspx)<br/>
  Oude maar nog steeds een goede resource voor een algemene inleiding tot aan het onderwerp.
* [Traceer-Listeners](http://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  Informatie over traceer-listeners, maar wordt niet vermeld in de [WebPageTraceListener](http://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx).
* [Overzicht: ASP.NET-tracering integreren met System.Diagnostics tracering](http://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  In dit artikel is ook oude, maar bevat aanvullende informatie die de inleidende artikel wordt niet beschreven.
* [Traceren in ASP.NET MVC Razor weergaven](http://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Naast de tracering in Razor weergaven, het bericht ook wordt uitgelegd hoe u een fout-filter maken om u te melden alle niet-verwerkte uitzonderingen in een MVC-toepassing. Voor informatie over het registreren van alle niet-verwerkte uitzonderingen in een toepassing webformulieren, Zie het voorbeeld Global.asax in [compleet voorbeeld voor fout-Handlers](http://msdn.microsoft.com/library/bb397417.aspx) op MSDN. In MVC of webformulieren, als u wilt melden bepaalde uitzonderingen maar laat het standaard-framework afhandeling van kracht, kunt u onderschept en rethrow zoals in het volgende voorbeeld:

        try
        {
           // Your code that might cause an exception to be thrown.
        }
        catch (Exception ex)
        {
            Trace.TraceError("Exception: " + ex.ToString());
            throw;
        }
* [Streaming diagnostische traceerlogboeken van de Azure vanaf de opdrachtregel (plus blik!)](http://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Het gebruik van de opdrachtregel doen wat in deze zelfstudie wordt beschreven hoe u in Visual Studio. [Blik](http://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) is een hulpprogramma voor het opsporen van ASP.NET-toepassingen.
* [Met behulp van WebApps-logboekregistratie en diagnose - met David Ebbo](/documentation/videos/azure-web-site-logging-and-diagnostics/) en [Streaminglogboeken van Web-Apps - met David Ebbo](/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Video van Scott Hanselman en David Ebbo.

Voor foutregistratie, is het een alternatief voor het schrijven van uw eigen code tracering te gebruiken, zoals een framework voor logboekregistratie van open-source [ELMAH](http://nuget.org/packages/elmah/). Zie voor meer informatie [van Scott Hanselman blogberichten over ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Ook, u hoeft niet te gebruiken van ASP.NET of `System.Diagnostics` tracering ophalen streamen registreert van Azure. De Azure-web-app streaming log-service een streams *.txt*, *.html*, of *.log* bestand dat wordt aangetroffen in de *logboekbestanden* map. Daarom kunt u uw eigen systeem logboekregistratie die naar het bestandssysteem van de web-app schrijft maken en het bestand automatisch worden gestreamd en gedownload. U hoeft te doen is toepassingscode schrijven waarmee bestanden in de *d:\home\logfiles* map.

### <a name="analyzing-web-server-logs"></a>Analyseren van weblogboeken van server
Zie de volgende bronnen voor meer informatie over het analyseren van weblogboeken voor server:

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Een hulpprogramma voor het weergeven van gegevens in de web server-Logboeken (*.log* bestanden).
* [Het oplossen van problemen met prestaties van IIS of toepassingsfouten LogParser met](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Een inleiding tot het hulpprogramma Log Parser die u gebruiken kunt voor het analyseren van weblogboeken server.
* [Blogberichten door Robert McMurray over het gebruik van LogParser](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [De HTTP-statuscode in IIS 7.0, IIS 7.5 en IIS 8.0](http://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Analyseren van Logboeken voor tracering van mislukte aanvragen
De Microsoft TechNet-website bevat een [met behulp van tracering van mislukte aanvragen](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing) sectie, die mogelijk nuttig is voor informatie over het gebruik van deze logboeken. Echter, deze documentatie richt zich voornamelijk op de tracering van mislukte aanvragen configureren in IIS, dat niet mogelijk in Azure Web Apps.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: https://github.com/Azure/azure-webjobs-sdk/wiki

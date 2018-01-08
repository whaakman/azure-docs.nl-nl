---
title: Een Azure cloudservice integreren met Azure CDN | Microsoft Docs
description: "Informatie over het implementeren van een cloudservice die inhoud van een geïntegreerde Azure CDN-eindpunt"
services: cdn, cloud-services
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: tysonn
ms.assetid: b3c0108f-9ec5-43a8-8fd0-40eafbd32637
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: f131eb021d85766f12b0fb6cb8b5a07f965f9c97
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="intro"></a>Een cloudservice integreren met Azure CDN
Een cloudservice kan worden geïntegreerd met Azure CDN, voor de inhoud van de locatie van de cloudservice. Deze aanpak kunt u de volgende voordelen:

* Eenvoudig implementeren en bijwerken van installatiekopieën, scripts en stylesheets in mappen van uw cloudservice-project
* De NuGet-pakketten in uw cloudservice, zoals jQuery of Bootstrap versies vervolgens gemakkelijk upgraden
* Beheren van uw webtoepassing en uw CDN-geleverd inhoud alle van de dezelfde Visual Studio-interface
* Uniforme implementatiewerkstroom voor uw webtoepassing en de inhoud van uw CDN-geleverd
* ASP.NET bundeling en minification integreren met Azure CDN

## <a name="what-you-will-learn"></a>Wat u leert
In deze zelfstudie leert u hoe:

* [Een Azure CDN-eindpunt integreren met de cloudservice en bedienen van statische inhoud in uw webpagina's van Azure CDN](#deploy)
* [Cache-instellingen voor statische inhoud configureren in uw cloudservice](#caching)
* [Inhoud verzorgen vanaf een domeincontroller acties via Azure CDN](#controller)
* [Dienst gebundeld en inhoud via Azure CDN minified behoud het script foutopsporing ervaring in Visual Studio](#bundling)
* [Terugval uw scripts en CSS configureren wanneer uw Azure CDN offline is](#fallback)

## <a name="what-you-will-build"></a>Wat u bouwt
U implementeert een cloud service-Webrol met behulp van de ASP.NET MVC-sjabloon, code toevoegen om aan te leveren van inhoud vanaf een geïntegreerde Azure CDN, zoals een installatiekopie van een domeincontroller actie resultaten en de standaard JavaScript en CSS-bestanden, en ook het schrijven van code voor het configureren van het mechanisme voor terugval voor bundels geleverd in het geval dat de CDN offline is.

## <a name="what-you-will-need"></a>Wat u nodig hebt
Deze zelfstudie gelden de volgende vereisten:

* Een actieve [Microsoft Azure-account](/account/)
* Visual Studio 2015 met [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [!NOTE]
> U hebt een Azure-account nodig om deze zelfstudie te voltooien.
> 
> * U kunt [gratis een Azure-account openen](https://azure.microsoft.com/pricing/free-trial/) -u ontvangt tegoed kunt u uitproberen betaalde Azure-services en zelfs nadat ze gebruikt maximaal kun je het account en gebruik gratis Azure-services, zoals Websites.
> * U kunt [voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -uw MSDN-abonnement ontvangt u elke maand tegoeden die u voor betaalde Azure-services kunt gebruiken.
> 
> 

<a name="deploy"></a>

## <a name="deploy-a-cloud-service"></a>Een cloudservice implementeren
In deze sectie maakt u de standaard ASP.NET MVC-toepassingssjabloon in Visual Studio 2015 implementeren in een cloud service-Webrol, en vervolgens te integreren met een nieuw CDN-eindpunt. Volg de onderstaande instructies:

1. Maak in Visual Studio 2015 een nieuwe Azure-cloud-service in de menubalk door te gaan naar **bestand > Nieuw > Project > Cloud > Azure Cloud Service**. Een naam geven en klik op **OK**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)
2. Selecteer **ASP.NET-Webrol** en klik op de  **>**  knop. Klik op OK.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)
3. Selecteer **MVC** en klik op **OK**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)
4. Nu deze rol Web publiceren met een Azure-cloud-service. Met de rechtermuisknop op het cloudserviceproject en selecteer **publiceren**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)
5. Als u nog niet aangemeld bij Microsoft Azure, klikt u op de **account toevoegen...**  vervolgkeuzelijst en klik op de **account toevoegen** menu-item.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)
6. Aanmelden met het Microsoft-account dat u gebruikt voor het activeren van uw Azure-account in de aanmeldingspagina.
7. Nadat u bent aangemeld, klikt u op **volgende**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)
8. Ervan uitgaande dat u een cloud-service of storage-account niet hebt gemaakt, kunt Visual Studio u beide maken. In de **Cloudservice maken en de Account** dialoogvenster, typt u de gewenste servicenaam en selecteer de gewenste regio. Klik vervolgens op **Maken**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)
9. Controleer de configuratie in de instellingenpagina publiceren en op **publiceren**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
   
   > [!NOTE]
   > Het publicatieproces voor cloudservices kan lang duren. Het inschakelen van Web Deploy voor alle functies optie kan zorgen dat uw cloudservice veel sneller foutopsporing door snelle (maar tijdelijke) updates leveren aan uw Web-rollen. Zie voor meer informatie over deze optie [publiceren van een Cloudservice met de Azure-hulpprogramma's](http://msdn.microsoft.com/library/ff683672.aspx).
   > 
   > 
   
    Wanneer de **Microsoft Azure Activity Log** zien is dat publicatiestatus **voltooid**, maakt u een CDN-eindpunt geïntegreerd met deze cloudservice.
   
   > [!WARNING]
   > Als, na de publicatie, wordt de geïmplementeerde cloudservice een scherm weergegeven, is het waarschijnlijk omdat het gebruik van de cloudservice die u hebt geïmplementeerd een [Gast OS die geen .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  U kunt dit probleem door omzeilen [.NET 4.5.2 als een taak starten implementeren](../cloud-services/cloud-services-dotnet-install-dotnet.md).
   > 
   > 

## <a name="create-a-new-cdn-profile"></a>Nieuwe CDN-profielen maken
Een CDN-profiel is een verzameling van CDN-eindpunten.  Elk profiel bevat een of meer CDN-eindpunten.  Mogelijk wilt meerdere profielen gebruiken om de CDN-eindpunten te ordenen op basis van het internetdomein, de webtoepassing of andere criteria.

> [!TIP]
> Als u al een CDN-profiel dat u wilt gebruiken voor deze zelfstudie hebt, gaat u verder met [een nieuw CDN-eindpunt maken](#create-a-new-cdn-endpoint).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Nieuwe CDN-eindpunten maken
**Een nieuw CDN-eindpunt voor uw opslagaccount maken**

1. Navigeer in [Azure Portal](https://portal.azure.com) naar uw CDN-profiel.  Mogelijk hebt u het profiel in de vorige stap vastgemaakt aan het dashboard.  Als dit niet het geval is, kunt u het dashboard zoeken door op **Bladeren** en vervolgens **CDN-profielen** te klikken en op het profiel te klikken die u aan het eindpunt wilt toevoegen.
   
    De blade CDN-profiel wordt weergegeven
   
    ![CDN-profiel][cdn-profile-settings]
2. Klik op de knop **Eindpunt toevoegen**.
   
    ![De knop Eindpunt toevoegen][cdn-new-endpoint-button]
   
    De blade **Een eindpunt toevoegen** wordt weergegeven.
   
    ![De blade Een eindpunt toevoegen][cdn-add-endpoint]
3. Voer een **naam** voor dit CDN-eindpunt.  Deze naam wordt gebruikt voor toegang tot uw resources in de cache in het domein `<EndpointName>.azureedge.net`.
4. In de **oorsprongtype** vervolgkeuzelijst *Cloudservice*.  
5. In de **de hostnaam van oorsprong** vervolgkeuzelijst, selecteer uw cloudservice.
6. Laat de standaardwaarden voor **oorsprongpad**, **host-header van oorsprong**, en **Protocol/oorsprong poort**.  U moet ten minste één protocol (HTTP of HTTPS) opgeven.
7. Klik op de knop **Toevoegen** om het nieuwe eindpunt te maken.
8. Zodra het eindpunt is gemaakt, wordt deze weergegeven in een lijst met eindpunten voor het profiel. In de lijstweergave kunt u zien welke URL u moet gebruiken voor toegang tot de content in cache en het brondomein.
   
    ![CDN-eindpunt][cdn-endpoint-success]
   
   > [!NOTE]
   > Het eindpunt onmiddellijk worden niet beschikbaar voor gebruik.  Het kan tot 90 minuten voor de registratie worden doorgegeven via het netwerk CDN duren voordat. Gebruikers die proberen de naam van het CDN-domein meteen kan gebruiken wordt totdat de inhoud beschikbaar via de CDN is statuscode 404.
   > 
   > 

## <a name="test-the-cdn-endpoint"></a>Het CDN-eindpunt testen
Wanneer de publicatiestatus is **voltooid**, open een browservenster en navigeer naar  **http://<cdnName>*.azureedge.net/Content/bootstrap.css**. In mijn setup is deze URL:

    http://camservice.azureedge.net/Content/bootstrap.css

Dit komt overeen met de volgende bron-URL op het CDN-eindpunt:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Wanneer u naar navigeert  **http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, afhankelijk van uw browser, wordt u gevraagd om te downloaden of open de bootstrap.css die afkomstig zijn van uw gepubliceerde Web-app.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

U hebt ook toegang tot een openbaar toegankelijke URL zijn op  **http://*&lt;serviceName >*.cloudapp.net/** rechtstreeks vanuit uw CDN-eindpunt. Bijvoorbeeld:

* Een JS-bestand van het pad/script
* Alle bestanden uit de/Content pad
* Elke domeincontroller/actie
* Als de queryreeks is ingeschakeld op uw CDN-eindpunt elke URL's met querytekenreeksen

In feite met de bovenstaande configuratie, kunt u de volledige in de cloud-service hosten  **http://*&lt;cdnName >*.azureedge.net/**. Als ik ga naar **http://camservice.azureedge.net/**, verschijnt het resultaat van de actie van de startpagina/Index.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Dit betekent niet, maar het is altijd een goed idee om te dienen als een volledige in de cloud-service via Azure CDN. 

Een CDN met statische leveringsoptimalisatie niet per se versnellen levering van dynamische elementen die niet zijn bedoeld om te worden in de cache, of zeer vaak worden bijgewerkt omdat de CDN van een nieuwe versie van de asset vanaf de oorspronkelijke server heel vaak ophalen moet. Voor dit scenario kunt u inschakelen [dynamische Site-versnelling](cdn-dynamic-site-acceleration.md) optimalisatie (DSA) op uw CDN-eindpunt dat gebruikmaakt van verschillende manieren om levering van niet-caching geschikte dynamische activa te versnellen. 

Als u een site met een combinatie van statische en dynamische inhoud hebt, u kunt kiezen om te dienen als uw statische inhoud uit CDN met een statische optimalisatie type (zoals algemene webtoepassingen levering) en voor het uitvoeren van dynamische inhoud rechtstreeks vanaf de bronserver of via een CDN-eindpunt met DSA optimalisatie ingeschakeld per geval. Daartoe, hebt u al gezien hoe voor toegang tot afzonderlijke inhoudsbestanden van het CDN-eindpunt. Ik wordt beschreven hoe u een specifieke domeincontroller actie via een specifieke CDN-eindpunt in dienst inhoud van de domeincontroller acties via Azure CDN bedienen.

Het alternatief is om te bepalen welke inhoud voor het uitvoeren van Azure CDN op basis van geval in uw cloudservice. Daartoe, hebt u al gezien hoe voor toegang tot afzonderlijke inhoudsbestanden van het CDN-eindpunt. Ik wordt beschreven hoe u een specifieke domeincontroller actie via het CDN-eindpunt in behandeling [inhoud verzorgen vanaf een domeincontroller acties via Azure CDN](#controller).

<a name="caching"></a>

## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Cacheopties voor statische bestanden in uw cloudservice configureren
U kunt opgeven hoe u statische inhoud in de cache worden opgeslagen in het CDN-eindpunt wilt met Azure CDN-integratie in uw cloudservice. Open hiervoor *Web.config* van uw rol Web project (bijvoorbeeld WebRole1) en voeg een `<staticContent>` element op de `<system.webServer>`. De onderstaande XML configureert u de cache om de 3 dagen vervalt.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Als u dit doet, ziet alle statische bestanden in uw cloudservice dezelfde regel in uw CDN-cache. Voor gedetailleerde controle over de clientcache-instellingen, voegt u een *Web.config* bestand naar een map en uw instellingen toe te voegen. Bijvoorbeeld, Voeg een *Web.config* van het bestand in de *\Content* map en vervang de inhoud met de volgende XML-code:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Deze instelling zorgt ervoor dat alle statische bestanden van de *\Content* map mogen worden opgeslagen voor 15 dagen.

Voor meer informatie over het configureren van de `<clientCache>` element, Zie [clientcache &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

In [inhoud verzorgen vanaf een domeincontroller acties via Azure CDN](#controller), ook leest u hoe u de cache-instellingen voor de controller actie resultaten in de cache CDN kunt configureren.

<a name="controller"></a>

## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Inhoud verzorgen vanaf een domeincontroller acties via Azure CDN
Wanneer u een cloud service-Webrol met Azure CDN integreert, is het relatief gemakkelijk op te leveren van inhoud van de domeincontroller acties via Azure CDN. Anders dan voor uw cloud service rechtstreeks via Azure CDN (uitgelegd hierboven) [Maarten Balliauw](https://twitter.com/maartenballiauw) ziet u hoe u dit doen met een leuk MemeGenerator controller in [korte wachttijden op het web met Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). Ik zal gewoon reproduceer het hier.

Stel dat in uw cloud service die u wilt memes genereren op basis van de installatiekopie van een jonge Chuck Norris (foto's door [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) zoals deze:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

U hebt een eenvoudige `Index` de meme actie waarmee de klanten om op te geven van de items in de afbeelding wordt gegenereerd zodra ze boeken aan de actie. Aangezien het Chuck Norris, kunt u deze pagina om te worden globaal sterk populaire zou verwachten. Dit is een goed voorbeeld van voor de semi dynamische inhoud met Azure CDN.

Volg de stappen hierboven om deze actie controller instellen:

1. In de *\Controllers* map, maak een nieuw .cs bestand aangeroepen *MemeGeneratorController.cs* en vervang de inhoud door de volgende code. Zorg ervoor dat het gemarkeerde gedeelte vervangen door uw CDN-naam.  
   
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;
   
        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();
   
                public ActionResult Index()
                {
                    return View();
                }
   
                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }
   
                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }
   
                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }
   
                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }
   
                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);
   
                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }
   
                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }
   
                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);
   
                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }
   
                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }
2. Klik met de rechtermuisknop in de standaard `Index()` actie en selecteer **weergave toevoegen**.
   
    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)
3. Accepteer de onderstaande instellingen en klik op **toevoegen**.
   
   ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)
4. Open de nieuwe *Views\MemeGenerator\Index.cshtml* en vervang de inhoud door de volgende eenvoudige HTML-code voor het indienen van de items:
   
        <h2>Meme Generator</h2>
   
        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>
5. De cloudservice opnieuw publiceren en navigeer naar  **http://*&lt;serviceName >*.cloudapp.net/MemeGenerator/Index** in uw browser.

Wanneer u de formulierwaarden te verzenden `/MemeGenerator/Index`, wordt de `Index_Post` actiemethode retourneert een koppeling naar de `Show` actiemethode met de bijbehorende invoer-ID. Wanneer u de koppeling klikt, kunt u de volgende code bereiken:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Als uw lokale foutopsporing is gekoppeld, krijgt u de gewone foutopsporing ervaring met een lokale omleiding. Als deze wordt uitgevoerd in de cloudservice, wordt naar het omleiden:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Dit komt overeen met de volgende URL van de oorsprong op uw CDN-eindpunt:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


U kunt de `OutputCacheAttribute` -kenmerk uit voor de `Generate` methode om op te geven hoe het resultaat van de actie moet worden in de cache, die Azure CDN wordt geacht. De onderstaande code Geef de vervaldatum van een cache van 1 uur (3600 seconden).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Evenzo kan u fungeren inhoud van elke domeincontroller actie in uw cloudservice via uw Azure CDN, met de gewenste optie voor het opslaan in cache.

In de volgende sectie wordt ik beschreven hoe u de scripts gebundelde en minified en CSS via Azure CDN bedienen.

<a name="bundling"></a>

## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>ASP.NET bundeling en minification integreren met Azure CDN
Scripts en CSS stylesheets niet vaak worden gewijzigd en voornaamste kandidaten zijn voor de Azure CDN-cache. Voor de hele Webrol via uw Azure CDN is de eenvoudigste manier om bundeling en minification integreren met Azure CDN. Echter, als u niet wilt mogelijk u dit doet, ik wordt beschreven hoe u om dat te doen terwijl de ervaring van de gewenste ontwikkelaars van ASP.NET bundeling en minification, zoals behouden:

* Goede foutopsporing modus ervaring
* Gestroomlijnde implementatie
* Onmiddellijke updates voor clients voor script/CSS versie-upgrades
* Terugval mechanisme als uw CDN-eindpunt is mislukt
* Codewijzigingen minimaliseren

In de **WebRole1** project dat u hebt gemaakt in [integreren van een Azure CDN-eindpunt met uw Azure-website en bedienen van statische inhoud in uw webpagina's van Azure CDN](#deploy)Open *App_Start\BundleConfig.cs* en bekijk de `bundles.Add()` methodeaanroepen.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

De eerste `bundles.Add()` instructie voegt u een script bundel op de virtuele map `~/bundles/jquery`. Open vervolgens *Views\Shared\_Layout.cshtml* om te zien hoe de code script bundel wordt weergegeven. U moet de volgende regel code Razor vinden:

    @Scripts.Render("~/bundles/jquery")

Wanneer deze Razor code wordt uitgevoerd in de Azure-Webrol, verschijnt er een `<script>` tag voor de bundel script vergelijkbaar met het volgende:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Echter, als deze wordt uitgevoerd in Visual Studio door te typen `F5`, geeft deze afzonderlijk elke scriptbestand in de bundel weer (in het geval is slechts één scriptbestand is in de bundel):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Hiermee kunt u fouten opsporen in de JavaScript-code in uw ontwikkelingsomgeving terwijl gelijktijdige clientverbindingen (bundeling) verminderen en het verbeteren van bestand prestaties (minification) in de productieomgeving downloaden. Dit is een uitstekende functie handhaven met Azure CDN-integratie. Bovendien, omdat de gerenderde bundel al een automatisch gegenereerde versietekenreeks bevat, u wilt repliceren die functionaliteit zodat de telkens wanneer u uw jQuery-versie via NuGet bijwerkt, deze kan worden bijgewerkt op de client zo snel mogelijk.

Volg de onderstaande stappen integratie ASP.NET bundeling en minification met uw CDN-eindpunt.

1. Terug in de *App_Start\BundleConfig.cs*, wijzig de `bundles.Add()` methoden voor het gebruik van een andere [bundel constructor](http://msdn.microsoft.com/library/jj646464.aspx), die een CDN-adres. Om dit te doen, vervang de `RegisterBundles` methodedefinitie met de volgende code:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    Zorg ervoor dat u `<yourCDNName>` met de naam van uw Azure CDN.
   
    In een gewone woorden die u instelt `bundles.UseCdn = true` en een zorgvuldig ontworpen CDN-URL toegevoegd aan elke bundel. Bijvoorbeeld, de eerste constructor in de code:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
   
    is gelijk aan:
   
        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))
   
    Deze constructor leest ASP.NET bundeling en minification weergeven van afzonderlijke scriptbestanden wanneer foutopsporing lokaal wordt uitgevoerd, maar het opgegeven CDN-adres gebruiken voor toegang tot het script in kwestie. Let echter op twee belangrijke kenmerken met deze zorgvuldig ontworpen CDN-URL:
   
   * De bron voor deze URL CDN is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, namelijk daadwerkelijk op de virtuele map van de bundel script in uw cloudservice.
   * Aangezien u CDN constructor gebruikt, bevat het CDN-scriptcode voor de bundel niet langer de automatisch gegenereerde versietekenreeks in de gerenderde URL. Telkens wanneer de script-bundel is aangepast om af te dwingen een cache ontbreekt bij uw Azure CDN, moet u handmatig een unieke versietekenreeks genereren. Deze unieke versietekenreeks moet blijven constant via de levensduur van de implementatie te maximaliseren treffers in cache op uw Azure CDN na de implementatie van de bundel op hetzelfde moment.
   * De queryreeks v = < W.X.Y.Z > worden van *Properties\AssemblyInfo.cs* in uw webproject rol. U kunt een werkstroom voor de implementatie met de assembly-versie wordt verhoogd telkens wanneer u naar Azure publiceren hebben. Of u kunt alleen wijzigen *Properties\AssemblyInfo.cs* in uw project moet worden automatisch de versietekenreeks wordt verhoogd telkens wanneer u bouwt, met het jokerteken ' *'. Bijvoorbeeld:
     
        [assembly: AssemblyVersion("1.0.0.*")]
     
     Elke andere strategie voor het stroomlijnen van het genereren van een unieke tekenreeks op voor de levensduur van een implementatie wordt hier werken.
2. Publiceren van de cloudservice en toegang tot de startpagina.
3. De HTML-code voor de pagina weergeven. U moet de CDN-URL weergegeven, met een unieke versietekenreeks telkens wanneer u wijzigingen opnieuw naar de cloudservice publiceren te zien. Bijvoorbeeld:  
   
        ...
   
        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
   
        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
   
        ...
   
        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
   
        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
   
        ...
4. In Visual Studio fouten opsporen in de cloudservice in Visual Studio door te typen `F5`.,
5. De HTML-code voor de pagina weergeven. U ziet nog steeds elke scriptbestand afzonderlijk weergegeven zodat u kunt een consistente foutopsporing optreden in Visual Studio hebben.  
   
        ...
   
            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>
   
            <script src="/Scripts/modernizr-2.6.2.js"></script>
   
        ...
   
            <script src="/Scripts/jquery-1.10.2.js"></script>
   
            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>
   
        ...   

<a name="fallback"></a>

## <a name="fallback-mechanism-for-cdn-urls"></a>Terugval mechanisme voor CDN-URL 's
Als uw Azure CDN-eindpunt voor een of andere reden mislukt, wilt u de webpagina worden slim voor toegang tot uw oorsprong webserver als de terugvaloptie voor het laden van JavaScript of Bootstrap. Het is ernstige verliezen van installatiekopieën op uw website vanwege CDN niet beschikbaar zijn, maar veel ernstiger cruciaal pagina functionaliteit van uw scripts en stylesheets verliezen.

De [bundel](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) klasse bevat een eigenschap genaamd [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) waarmee u kunt het terugval mechanisme configureren voor CDN-fout. Volg de onderstaande stappen voor het gebruik van deze eigenschap:

1. Open in uw webrolproject *App_Start\BundleConfig.cs*, waarin u een CDN-URL toegevoegd in elk [bundel constructor](http://msdn.microsoft.com/library/jj646464.aspx), en breng de volgende gemarkeerde wijzigingen terugval mechanisme toevoegen aan de standaard bundels:  
   
        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;
   
            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));
   
            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));
   
            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));
   
            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                  "~/Scripts/bootstrap.js",
                                  "~/Scripts/respond.js"));
   
            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }
   
    Wanneer `CdnFallbackExpression` is niet null script is opgenomen in de HTML-code om te testen of de bundel is geladen en als dat niet het geval is, toegang tot de bundel rechtstreeks vanuit de oorsprong-webserver. Deze eigenschap moet worden ingesteld op een JavaScript-expressie die wordt gecontroleerd of de respectieve CDN-bundel correct wordt geladen. De expressie die nodig is voor het testen van elke bundel is afhankelijk van de inhoud. Voor de standaard bundels bovenstaande:
   
   * `window.jquery`is gedefinieerd in jquery-{version} .js
   * `$.validator`is gedefinieerd in jquery.validate.js
   * `window.Modernizr`is gedefinieerd in modernizer-{version} .js
   * `$.fn.modal`is gedefinieerd in bootstrap.js
     
     U mogelijk opgevallen dat ik niet ingesteld CdnFallbackExpression voor de `~/Cointent/css` bundel. Dit is omdat er momenteel een [fout in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) die injects een `<script>` tag voor de alternatieve CSS in plaats van de verwachte `<link>` label.
     
     Er is echter een goede [stijl bundel terugval](https://github.com/EmberConsultingGroup/StyleBundleFallback) die worden aangeboden door [Ember advies groep](https://github.com/EmberConsultingGroup).
2. Als u wilt de oplossing voor CSS gebruikt, kunt u een nieuw .cs-bestand maken in uw webrolproject *App_Start* map met de naam *StyleBundleExtensions.cs*, en vervang de inhoud ervan met de [code vanuit GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).
3. In *App_Start\StyleFundleExtensions.cs*, geef de naamruimte van de rol van uw Web-naam (bijvoorbeeld **WebRole1**).
4. Ga terug naar `App_Start\BundleConfig.cs` en wijzigen van de laatste `bundles.Add` instructie met de volgende gemarkeerde code:  
   
        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));
   
    Deze nieuwe uitbreidingsmethode maakt gebruik van de dezelfde idee invoeren van een script in de HTML-code om te controleren van de DOM voor de een overeenkomende klassenaam, regelnaam en regel waarde die is gedefinieerd in het CSS-bundel en vallen terug naar de oorsprong webserver als het mislukt om de overeenkomst te vinden.
5. De cloudservice opnieuw publiceren en toegang tot de startpagina.
6. De HTML-code voor de pagina weergeven. Zult u geïnjecteerde scripts vergelijkbaar met het volgende:    
   
        ...
   
        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>
   
        ...
   
            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>
   
            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>
   
        ...

    Houd er rekening mee ingevoegd script voor de CSS-bundel bevat nog steeds de onjuiste restant van de `CdnFallbackExpression` eigenschap in de regel:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Maar omdat het eerste deel van de || expressie wordt altijd waar retourneren, (op de regel die direct boven), de functie document.write() wordt nooit uitgevoerd.

## <a name="more-information"></a>Meer informatie
* [Overzicht van het Azure Content Delivery Network (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
* [Azure CDN gebruiken](cdn-create-new-endpoint.md)
* [ASP.NET bundeling en Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)

[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png

---
title: Hybride on-premises/cloudtoepassing (.NET) met Azure WCF Relay | Microsoft Docs
description: Informatie over het maken van een hybride .NET-on-premises-+/cloudtoepassing met Azure WCF Relay.
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9ed02f7c-ebfb-4f39-9c97-b7dc15bcb4c1
ms.service: service-bus-relay
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 11/02/2017
ms.author: sethm
ms.openlocfilehash: 77bb769a094c2a619c0c75363e23ae3ee561c1e4
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="net-on-premisescloud-hybrid-application-using-azure-wcf-relay"></a>Hybride .NET on-premises/cloudtoepassing met Azure WCF Relay

In dit artikel wordt beschreven hoe u een hybride cloudtoepassing opbouwt met Microsoft Azure en Visual Studio. Bij deze zelfstudie wordt ervan uitgegaan dat u nog geen ervaring hebt met Azure. In minder dan 30 minuten beschikt u over een toepassing die meerdere Azure-resources gebruikt en wordt uitgevoerd in de cloud.

U leert:

* Een bestaande webservice voor verbruik door een weboplossing maken of aanpassen.
* De Azure WCF Relay-service gebruiken voor het delen van gegevens tussen een Azure-toepassing en een webservice die ergens anders wordt gehost.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a>Hoe Azure hulp biedt bij hybride oplossingen

Bedrijfsoplossingen bestaan meestal uit een combinatie van aangepaste code die is geschreven voor nieuwe en unieke zakelijke vereisten en bestaande functionaliteit van oplossingen en systemen die al aanwezig zijn.

Oplossingsarchitecten gaan nu vaak over op de cloud om gemakkelijk aan schaalvereisten te voldoen en operationele kosten te verlagen. Ze ondervinden dan dat bestaande serviceassets die ze willen gebruiken als bouwstenen voor hun oplossingen, zich binnen de firewall van het bedrijf bevinden en niet gemakkelijk bereikbaar zijn voor de cloudoplossing. Veel interne services worden niet zo gemaakt of gehost dat ze gemakkelijk beschikbaar kunnen worden gemaakt aan de rand van het bedrijfsnetwerk.

[Azure Relay](https://azure.microsoft.com/services/service-bus/) is bedoeld voor de gebruikstoepassing waarbij bestaande WCF-webservices (Windows Communication Foundation) veilig toegankelijk worden gemaakt voor oplossingen die zich buiten de bedrijfsperimeter bevinden, zonder dat er hiervoor tussenkomende wijzigingen in de infrastructuur van een bedrijfsnetwerk nodig zijn. Dergelijke relayservices worden nog steeds gehost binnen hun bestaande omgeving, maar ze dragen het luisteren naar binnenkomende sessies en aanvragen over aan de relayservice in de cloud. Ook beveiligt Azure Relay deze services tegen onbevoegde toegang door [SAS (Shared Access Signature)](../service-bus-messaging/service-bus-sas.md)-verificatie te gebruiken.

## <a name="solution-scenario"></a>Oplossingsscenario
In deze zelfstudie maakt u een ASP.NET-website waarmee u een lijst met producten op de pagina met de productinventaris kunt bekijken.

![][0]

In de zelfstudie wordt ervan uitgegaan dat u productgegevens in een bestaand on-premises systeem hebt en wordt Azure Relay gebruikt om dat systeem te bereiken. Dit wordt gesimuleerd door een webservice die in een eenvoudige consoletoepassing wordt uitgevoerd en wordt ondersteund door een set producten in het geheugen. U kunt deze consoletoepassing op uw eigen computer uitvoeren en de webrol in Azure implementeren. Als u dit doet, ziet u hoe de webrol die in het Azure-datacenter wordt uitgevoerd inderdaad uw computer aanroept, ondanks dat de computer bijna zeker achter ten minste één firewall en een NAT-laag (Network Address Translation) blijft.

## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

Voordat u Azure-toepassingen kunt ontwikkelen, dient u de hulpprogramma's te downloaden en uw ontwikkelomgeving in te stellen:

1. Installeer de Azure-SDK voor .NET via de [pagina met downloads](https://azure.microsoft.com/downloads/) voor SDK.
2. Klik in de kolom **.NET** op de versie van [Visual Studio](http://www.visualstudio.com) die u gebruikt. In de stappen in deze zelfstudie wordt Visual Studio 2017 gebruikt.
3. Klik op **Uitvoeren** wanneer u wordt gevraagd of u het installatieprogramma wilt uitvoeren of opslaan.
4. Klik in het **webplatforminstallatieprogramma** op **Installeren** om door te gaan met de installatie.
5. Nadat de installatie is voltooid, hebt u alles wat u nodig hebt om te starten met het ontwikkelen van de app. De SDK bevat hulpprogramma's waarmee u eenvoudig Azure-toepassingen kunt ontwikkelen in Visual Studio.

## <a name="create-a-namespace"></a>Een naamruimte maken

Als u de relayfuncties in Azure wilt gebruiken, moet u eerst een servicenaamruimte maken. Een naamruimte biedt een scoping container voor het verwerken van Azure-resources in uw toepassing. Volg [deze instructies](relay-create-namespace-portal.md) om een Relay-naamruimte te maken.

## <a name="create-an-on-premises-server"></a>Een on-premises server maken

U bouwt eerst een on-premises (model)systeem voor de productcatalogus op. Dit is redelijk eenvoudig; u kunt dit zien als het representeren van een werkelijk on-premises productcatalogussysteem met een volledige serviceoppervlak dat we proberen te integreren.

Dit project is een Visual Studio-consoletoepassing en gebruikt het [Azure Service Bus NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) om de Service Bus-bibliotheken en configuratie-instellingen op te nemen.

### <a name="create-the-project"></a>Het project maken

1. Start Microsoft Visual Studio met administratorbevoegdheden. Om dit te doen, klikt u met de rechtermuisknop op het pictogram van het Visual Studio-programma en vervolgens op **Als administrator uitvoeren**.
2. Klik in het menu **Bestand** van Visual Studio op **Nieuw** en klik vervolgens op **Project**.
3. Klik bij **Geïnstalleerde sjablonen**, onder **Visual C#**, op **Consoletoepassing (.NET Framework)**. Typ in het vak **Naam** de naam **ProductsServer**:

   ![][11]
4. Klik op **OK** om het project **ProductsServer** te maken.
5. Als u NuGet Package Manager voor Visual Studio al hebt geïnstalleerd, slaat u de volgende stap over. Anders gaat u naar [NuGet][NuGet] en klikt u op [NuGet installeren](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Volg de aanwijzingen om NuGet Package Manager te installeren en vervolgens Visual Studio opnieuw te starten.
6. Klik in Solution Explorer met de rechtermuisknop op het project **ProductsServer** en klik vervolgens op **NuGet-pakketten beheren**.
7. Klik op het tabblad **Bladeren** en zoek vervolgens naar **WindowsAzure.ServiceBus**. Selecteer het pakket **WindowsAzure.ServiceBus**.
8. Klik op **Installeren** en accepteer de gebruiksvoorwaarden.

   ![][13]

   Er wordt nu naar de vereiste clientassembly's verwezen.
8. Voeg een nieuwe klasse voor uw productcontract toe. Klik in Solution Explorer met de rechtermuisknop op het project **ProductsServer** en klik achtereenvolgens op **Toevoegen** en **Klasse**.
9. Typ in het vak **Naam** de naam **ProductsContract.cs**. Klik vervolgens op **Toevoegen**.
10. Vervang in **ProductsContract.cs** de naamruimtedefinitie door de volgende code waarmee het contract voor de service wordt gedefinieerd.

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();

        }

        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```
11. Vervang in Program.cs de naamruimtedefinitie door de volgende code waarmee de profielservice en de host voor deze service worden toegevoegd.

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement the IProducts interface.
        class ProductsService : IProducts
        {

            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };

            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```
12. Dubbelklik in Solution Explorer op het bestand **App.config** om dit te openen in de Visual Studio-editor. Voeg onderaan het `<system.ServiceModel>`-element (maar nog wel binnen `<system.ServiceModel>`) de volgende XML-code toe. Zorg ervoor dat u *yourServiceNamespace* vervangt door de naam van uw naamruimte en *yourKey* door de SAS-sleutel die u eerder hebt opgehaald via de portal:

    ```xml
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
    De fout die wordt veroorzaakt door transportClientEndpointBehavior is slechts een waarschuwing en is geen blokkerend probleem voor dit voorbeeld.
    
13. Terwijl App.config actief is, vervangt u in het `<appSettings>`-element de verbindingsreekswaarde door de verbindingsreeks die u eerder hebt verkregen via de portal.

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. Druk op **Ctrl+Shift+B** of klik in het menu **Bouwen** op **Oplossing opbouwen** en controleer de juistheid van uw werk tot nu toe.

## <a name="create-an-aspnet-application"></a>Een ASP.NET-toepassing maken

In deze sectie bouwt u een eenvoudige ASP.NET-toepassing op waarmee gegevens worden weergegeven die u uit uw productservice hebt opgehaald.

### <a name="create-the-project"></a>Het project maken

1. Zorg ervoor dat Visual Studio met administratorbevoegdheden wordt uitgevoerd.
2. Klik in het menu **Bestand** van Visual Studio op **Nieuw** en klik vervolgens op **Project**.
3. Klik bij **Geïnstalleerde sjablonen**, onder **Visual C#**, op **ASP.NET-webtoepassing (.NET Framework)**. Noem het project **ProductsPortal**. Klik vervolgens op **OK**.

   ![][15]

4. Klik in de lijst **ASP.NET-sjablonen** in het dialoogvenster **New ASP.NET-webtoepassing** op **MVC**.

   ![][16]

6. Klik op de knop **Verificatie wijzigen**. Zorg dat in het dialoogvenster **Verificatie wijzigen** de optie **Geen verificatie** is geselecteerd en klik vervolgens op **OK**. In deze zelfstudie implementeert u een app waarvoor geen gebruikersaanmelding nodig is.

    ![][18]

7. Klik terug in het dialoogvenster **Nieuwe ASP.NET-webtoepassing** op **OK** om de MVC-app te maken.
8. U moet nu Azure-resources configureren voor een nieuwe web-app. Volg de stappen in [het gedeelte Publiceren naar Azure van dit artikel](../app-service/app-service-web-get-started-dotnet.md#publish-to-azure). Ga vervolgens terug naar deze zelfstudie en ga door met de volgende stap.
10. Klik in Solution Explorer met de rechtermuisknop op **Modellen** en klik achtereenvolgens op **Toevoegen** en **Klasse**. Typ in het vak **Naam** de naam **Product.cs**. Klik vervolgens op **Toevoegen**.

    ![][17]

### <a name="modify-the-web-application"></a>De webtoepassing wijzigen

1. In het bestand Product.cs in Visual Studio vervangt u de bestaande naamruimtedefinitie door de volgende code.

   ```csharp
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
       public class Product
       {
           public string Id { get; set; }
           public string Name { get; set; }
           public string Quantity { get; set; }
       }
    }
    ```
2. Vouw in Solution Explorer de map **Controllers** uit en dubbelklik vervolgens op het bestand **HomeController.cs** om het te openen in Visual Studio.
3. In **HomeController.cs** vervangt u de bestaande naamruimtedefinitie door de volgende code.

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```
4. Vouw in Solution Explorer de map Views\Shared uit en dubbelklik vervolgens op **_Layout.cshtml** om dit in de Visual Studio-editor te openen.
5. Wijzig alle instanties van **Mijn ASP.NET-toepassing** in **Producten van Northwind Traders**.
6. Verwijder de koppelingen **Start**, **Info** en **Contact**. Verwijder de gemarkeerde code in het volgende voorbeeld.

    ![][41]

7. Vouw in Solution Explorer de map Views\Home uit en dubbelklik vervolgens op **Index.cshtml** om dit in de Visual Studio-editor te openen. Vervang de volledige inhoud van het bestand door de volgende code.

   ```html
   @model IEnumerable<ProductsWeb.Models.Product>

   @{
            ViewBag.Title = "Index";
   }

   <h2>Prod Inventory</h2>

   <table>
             <tr>
                 <th>
                     @Html.DisplayNameFor(model => model.Name)
                 </th>
                 <th></th>
                 <th>
                     @Html.DisplayNameFor(model => model.Quantity)
                 </th>
             </tr>

   @foreach (var item in Model) {
             <tr>
                 <td>
                     @Html.DisplayFor(modelItem => item.Name)
                 </td>
                 <td>
                     @Html.DisplayFor(modelItem => item.Quantity)
                 </td>
             </tr>
   }

   </table>
   ```
8. U kunt de nauwkeurigheid van uw werk tot nu toe controleren door op **Ctrl+Shift+B** te drukken om het project op te bouwen.

### <a name="run-the-app-locally"></a>De app lokaal uitvoeren

Voer de toepassing uit om te controleren of deze werkt.

1. Zorg ervoor dat **ProductsPortal** het actieve project is. Klik met de rechtermuisknop op de projectnaam in Solution Explorer en selecteer **Instellen als opstartproject**.
2. Druk in Visual Studio op **F5**.
3. Uw toepassing moet dan in een browser worden weergegeven.

   ![][21]

## <a name="put-the-pieces-together"></a>De softwareonderdelen samenstellen

In de volgende stap koppelt u de on-premises productenserver aan de ASP.NET-toepassing.

1. Als het project **ProductsPortal** dat u hebt gemaakt in het gedeelte [Een ASP.NET-toepassing maken](#create-an-aspnet-application) nog niet open is, opent u het opnieuw in Visual Studio.
2. Vergelijkbaar met de stap in het gedeelte 'Een on-premises server maken' voegt u het NuGet-pakket aan de projectverwijzingen toe. Klik in Solution Explorer met de rechtermuisknop op het project **ProductsPortal** en klik vervolgens op **NuGet-pakketten beheren**.
3. Zoek naar **WindowsAzure.ServiceBus** en selecteer het item **WindowsAzure.ServiceBus**. Vervolgens voltooit u de installatie en sluit u dit dialoogvenster.
4. Klik in Solution Explorer met de rechtermuisknop op het project **ProductsPortal** en klik achtereenvolgens op **Toevoegen** en **Bestaand item**.
5. Ga naar het bestand **ProductsContract.cs** vanuit het **ProductsServer**-consoleproject. Klik om ProductsContract.cs te markeren. Klik op de pijl-omlaag naast **Toevoegen** en klik vervolgens op **Toevoegen als koppeling**.

   ![][24]

6. Open nu het bestand **HomeController.cs** in de Visual Studio-editor en vervang de naamruimtedefinitie door de volgende code. Zorg ervoor dat u *yourServiceNamespace* vervangt door de naam van uw servicenaamruimte en *yourKey* door de SAS-sleutel. Hierdoor kan de client de on-premises service aanroepen waarbij het resultaat van de aanroep wordt geretourneerd.

   ```csharp
   namespace ProductsWeb.Controllers
   {
       using System.Linq;
       using System.ServiceModel;
       using System.Web.Mvc;
       using Microsoft.ServiceBus;
       using Models;
       using ProductsServer;

       public class HomeController : Controller
       {
           // Declare the channel factory.
           static ChannelFactory<IProductsChannel> channelFactory;

           static HomeController()
           {
               // Create shared access signature token credentials for authentication.
               channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                   "sb://yourServiceNamespace.servicebus.windows.net/products");
               channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                   TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                       "RootManageSharedAccessKey", "yourKey") });
           }

           public ActionResult Index()
           {
               using (IProductsChannel channel = channelFactory.CreateChannel())
               {
                   // Return a view of the products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```
7. In Solution Explorer klikt u met de rechtermuisknop op de **ProductsPortal**-oplossing (zorg ervoor dat u met de rechtermuisknop op de oplossing klikt en niet op het project). Klik op **Toevoegen** en vervolgens op **Bestaand project**.
8. Ga naar het **ProductsServer**-project en dubbelklik op het **ProductsServer.csproj**-oplossingsbestand om het toe te voegen.
9. **ProductsServer** moet worden uitgevoerd om de gegevens in **ProductsPortal** weer te geven. Klik in Solution Explorer met de rechtermuisknop op de oplossing **ProductsPortal** en klik op **Eigenschappen**. Het dialoogvenster **Eigenschappenvensters** wordt weergegeven.
10. Klik aan de linkerkant op **Opstartproject**. Klik aan de rechterkant op **Meerdere opstartprojecten**. Zorg ervoor dat **ProductsServer** en **ProductsPortal** in de juiste volgorde worden weergegeven waarbij voor beide **Starten** als actie is ingesteld.

      ![][25]

11. Klik ook in het dialoogvenster **Eigenschappen** op **Projectafhankelijkheden** aan de linkerkant.
12. Klik in de lijst **Projecten** op **ProductsServer**. Zorg ervoor dat **ProductsPortal** niet is geselecteerd.
13. Klik in de lijst **Projecten** op **ProductsPortal**. Zorg ervoor dat **ProductsServer** is geselecteerd.

    ![][26]

14. Klik op **OK** in het dialoogvenster **Eigenschappenvensters**.

## <a name="run-the-project-locally"></a>Het project lokaal uitvoeren

Test de toepassing lokaal door in Visual Studio op **F5** te drukken. De on-premises server (**ProductsServer**) moet eerst worden gestart en vervolgens moet de **ProductsPortal**-toepassing worden gestart in een browservenster. De productinventaris bevat nu de gegevens die zijn opgehaald uit het on-premises systeem van de productservice.

![][10]

Klik op **Vernieuwen** op de pagina **ProductsPortal**. Elke keer dat u de pagina vernieuwt, wordt met de server-app een bericht weergegeven wanneer `GetProducts()` vanuit **ProductsServer** wordt aangeroepen.

Sluit beide toepassingen voordat u doorgaat met de volgende stap.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Het project ProductsPortal in een Azure-web-app implementeren

De volgende stap is het opnieuw publiceren van de frontend van de Azure-web-app **ProductsPortal**. Ga als volgt te werk:

1. Klik in Solution Explorer met de rechtermuisknop op het project **ProductsPortal** en klik op **Publiceren**. Klik vervolgens op de pagina **Publiceren** op **Publiceren**.

  > [!NOTE]
  > Mogelijk wordt er een foutbericht weergegeven in het browservenster wanneer het **ProductsPortal**-webproject automatisch wordt gestart na de implementatie. Dit is normaal en doet zich voor omdat de **ProductsServer**-toepassing nog niet wordt uitgevoerd.
>
>

2. Kopieer de URL van de geïmplementeerde web-app. Deze hebt u in de volgende stap nodig. Deze URL is ook beschikbaar in het venster Azure App Service-activiteit in Visual Studio:

  ![][9]

3. Sluit het browservenster om het uitvoeren van de toepassing te stoppen.

### <a name="set-productsportal-as-web-app"></a>ProductsPortal instellen als web-app

Voordat u de toepassing in de cloud uitvoert, moet u ervoor zorgen dat **ProductsPortal** vanuit Visual Studio als een web-app wordt uitgevoerd.

1. Klik in Visual Studio met de rechtermuisknop op het project **ProductsPortal** en klik vervolgens op **Eigenschappen**.
2. Klik in de kolom links op **Web**.
3. Klik in de sectie **Actie starten** op de knop **Start-URL** en voer in het tekstvak de URL voor uw eerder geïmplementeerde web-app (bijvoorbeeld `http://productsportal1234567890.azurewebsites.net/`) in.

    ![][27]

4. Klik in het menu **Bestand** in Visual Studio op **Alles opslaan**.
5. Klik in het menu Bouwen in Visual Studio op **Oplossing opnieuw opbouwen**.

## <a name="run-the-application"></a>De toepassing uitvoeren

1. Druk op F5 om de toepassing op te bouwen en uit te voeren. De on-premises server (de **ProductsServer**-consoletoepassing) moet eerst worden gestart en vervolgens moet de **ProductsPortal**-toepassing worden gestart in een browservenster (zie de volgende schermopname). U ziet weer dat de productinventaris gegevens bevat die zijn opgehaald uit het on-premises systeem van de productservice en dat die gegevens in de web-app worden weergegeven. Controleer de URL om ervoor te zorgen dat **ProductsPortal** als een Azure-web-app wordt uitgevoerd in de cloud.

   ![][1]

   > [!IMPORTANT]
   > De **ProductsServer**-consoletoepassing moet worden uitgevoerd en moet de gegevens aan de **ProductsPortal**-toepassing kunnen leveren. Als een fout wordt weergegeven in de browser, wacht u enkele seconden tot **ProductsServer** is geladen en het volgende bericht wordt weergegeven. Klik vervolgens op **Vernieuwen** in de browser.
   >
   >

   ![][37]
2. Als de browser opnieuw wordt weergegeven, klikt u op **Vernieuwen** op de **ProductsPortal**-pagina. Elke keer dat u de pagina vernieuwt, wordt met de server-app een bericht weergegeven wanneer `GetProducts()` vanuit **ProductsServer** wordt aangeroepen.

    ![][38]

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende resources voor meer informatie over Azure Relay:  

* [Wat is Azure Relay?](relay-what-is-it.md)  
* [Azure Relay gebruiken](relay-wcf-dotnet-get-started.md)  

[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[NuGet]: http://nuget.org

[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
[9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png

[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png

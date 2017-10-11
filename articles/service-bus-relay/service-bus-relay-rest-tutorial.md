---
title: Zelfstudie Service Bus REST met Azure Relay | Microsoft Docs
description: Bouw een eenvoudige Azure Service Bus relay-hosttoepassing een REST gebaseerde interface.
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2017
ms.author: sethm
ms.openlocfilehash: 0db9dbd2d2743907e3f0b259228201d4f5d0c3c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-wcf-relay-rest-tutorial"></a>Azure WCF Relay REST-zelfstudie

Deze zelfstudie wordt beschreven hoe u een eenvoudige Azure Relay-hosttoepassing een REST gebaseerde interface. Met REST kan een webclient, zoals een webbrowser, toegang krijgen tot de Service Bus-API's via HTTP-aanvragen.

De Windows Communication Foundation (WCF) REST-programmeermodel gebruikt voor het maken van een REST-service op de Service Bus maakt gebruik van de zelfstudie. Zie [WCF REST Programming Model](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) (WCF REST-programmeermodel) en [Designing and Implementing Services](/dotnet/framework/wcf/designing-and-implementing-services) (Services ontwerpen en implementeren) in de WCF-documentatie voor meer informatie.

## <a name="step-1-create-a-namespace"></a>Stap 1: Een naamruimte maken

Als u de relayfuncties in Azure wilt gebruiken, moet u eerst een servicenaamruimte maken. Een naamruimte biedt een scoping container voor het verwerken van Azure-resources in uw toepassing. Volg [deze instructies](relay-create-namespace-portal.md) om een Relay-naamruimte te maken.

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-azure-relay"></a>Stap 2: Een REST gebaseerd WCF-servicecontract voor gebruik met Azure Relay definiëren

Wanneer u een service WCF REST-stijl maakt, moet u het contract definiëren. Het contract geeft aan welke bewerkingen door de host worden ondersteund. Een servicebewerking kan worden beschouwd als een webservicemethode. Contracten worden gemaakt door een C++-, C#- of Visual Basic-interface te definiëren. Elke methode in de interface komt overeen met een specifieke servicebewerking. Op elke interface moet het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) worden toegepast en op elke bewerking moet het kenmerk [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) worden toegepast. Als een methode in een interface met het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) niet beschikt over het kenmerk [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), wordt die methode niet weergegeven. In het voorbeeld na de procedure wordt de code weergegeven die voor deze taken wordt gebruikt.

Het belangrijkste verschil tussen een WCF-contract en een REST-stijlcontract is de toevoeging van een eigenschap aan de [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Met deze eigenschap kunt u een methode in uw interface toewijzen aan een methode aan de andere kant van de interface. In dit geval wordt [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) gebruikt om een methode te koppelen aan HTTP GET. Hierdoor kan Service Bus opdrachten die naar de interface worden verzonden, correct ophalen en interpreteren.

### <a name="to-create-a-contract-with-an-interface"></a>Een contract met een interface maken

1. Open Visual Studio als beheerder: klik met de rechtermuisknop op het programma in het menu **Start** en klik vervolgens op **Als administrator uitvoeren**.
2. Maak een nieuw consoletoepassingsproject. Klik op het menu **Bestand**, selecteer **Nieuw** en selecteer vervolgens **Project**. Klik in het dialoogvenster **Nieuw project** op **Visual C#**, selecteer de sjabloon **Consoletoepassing** en geef deze de naam **ImageListener**. Gebruik de standaardwaarde voor **Locatie**. Klik op **OK** om het project te maken.
3. Voor een C#-project maakt Visual Studio een `Program.cs`-bestand. Deze klasse bevat een lege `Main()`-methode, vereist voor de juiste opbouw van een consoletoepassingsproject.
4. Voeg verwijzingen naar Service Bus en **System.ServiceModel.dll** aan het project toe door het Service Bus NuGet-pakket te installeren. Met dit pakket worden automatisch verwijzingen naar de Service Bus-bibliotheken en naar het **System.ServiceModel** van WCF toegevoegd. Klik in Solution Explorer met de rechtermuisknop op het project **ImageListener** en klik vervolgens op **NuGet-pakketten beheren**. Klik op het tabblad **Bladeren** en zoek naar `Microsoft Azure Service Bus`. Klik op **Installeren** en accepteer de gebruiksvoorwaarden.
5. U moet expliciet een verwijzing naar **System.ServiceModel.Web.dll** toevoegen aan het project:
   
    a. Klik in Solution Explorer met de rechtermuisknop op de map **Verwijzingen** in de projectmap en klik vervolgens op **Verwijzing toevoegen**.
   
    b. Klik in het dialoogvenster **Verwijzing toevoegen** op het tabblad **Framework** aan de linkerkant en typ **System.ServiceModel.Web** in het vak **Zoeken**. Schakel het selectievakje **System.ServiceModel.Web** in en klik vervolgens op **OK**.
6. Voeg aan het begin van het bestand Program.cs de volgende `using`-instructies toe:
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is de naamruimte die programmatisch toegang biedt tot de basisfuncties van WCF. Relay WCF maakt gebruik van veel van de objecten en kenmerken van WCF om servicecontracten te definiëren. U gebruikt deze naamruimte in de meeste van de relay-toepassingen. Op deze manier [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) helpt bij het definiëren het kanaal, dit is het object waarmee u met Azure Relay en de clientwebbrowser communiceren. Tot slot bevat [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) de typen waarmee u webtoepassingen kunt maken.
7. Wijzig de naam van de naamruimte `ImageListener` in **Microsoft.ServiceBus.Samples**.
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. Definieer meteen na de openingsaccolade van de naamruimtedeclaratie een nieuwe interface met de naam **IImageContract** en pas het kenmerk **ServiceContractAttribute** met de waarde `http://samples.microsoft.com/ServiceModel/Relay/` toe op de interface. De naamruimtewaarde verschilt van de naamruimte die u in uw code gebruikt. De naamruimtewaarde wordt gebruikt als een unieke id voor dit contract en moet versie-informatie bevatten. Zie [Serviceversiebeheer](http://go.microsoft.com/fwlink/?LinkID=180498) voor meer informatie. Door de naamruimte expliciet op te geven, wordt voorkomen dat de standaardnaamruimtewaarde wordt toegevoegd aan de naam van het contract.
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. Declareer in de `IImageContract`-interface een methode voor de enkelvoudige bewerking die door het `IImageContract`-contract wordt weergegeven in de interface en pas het kenmerk `OperationContractAttribute` toe op de methode die u wilt weergeven als onderdeel van het openbare Service Bus-contract.
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. Voeg de waarde **WebGet** toe aan het kenmerk **OperationContract**.
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    In dat geval schakelt de relay-service op route HTTP GET aanvragen om te doen `GetImage`, en de retourwaarden van vertalen `GetImage` in een HTTP GETRESPONSE-antwoord. Verderop in de zelfstudie gebruikt u een webbrowser om toegang te krijgen tot deze methode en de installatiekopie weer te geven in de browser.
11. Declareer direct na de `IImageContract`-definitie een kanaal dat de eigenschappen overneemt van de `IImageContract`- en `IClientChannel`-interface.
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    Een kanaal is het WCF-object waarmee de service en de client informatie aan elkaar doorgeven. U maakt het kanaal later in uw hosttoepassing. Azure Relay gebruikt dit kanaal om door te geven van de HTTP GET-aanvragen van de browser uw **GetImage** implementatie. De relay gebruikt dit kanaal ook te laten de **GetImage** waarde retourneren en zet deze om naar een HTTP GETRESPONSE voor de clientbrowser.
12. Klik in het menu **Bouwen** op **Oplossing opbouwen** om de juistheid van uw werk tot nu toe te controleren.

### <a name="example"></a>Voorbeeld
De volgende code toont een eenvoudige interface die een Relay WCF-contract wordt gedefinieerd.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Stap 3: een op REST gebaseerd WCF-servicecontract voor gebruik met Service Bus implementeren
Maken van een REST-stijl WCF Relay-service, moet u eerst het contract, die is gedefinieerd met behulp van een interface maken. De volgende stap is het implementeren van de interface. Hiervoor moet u een klasse met de naam **ImageService** maken die de door de gebruiker gedefinieerde **IImageContract**-interface implementeert. Nadat het contract is geïmplementeerd, configureert u de interface met een App.config-bestand. Het configuratiebestand bevat de benodigde informatie voor de toepassing, zoals de naam van de service, de naam van het contract en het type protocol dat wordt gebruikt om te communiceren met de relay-service. In het voorbeeld na de procedure wordt de code weergegeven die voor deze taken wordt gebruikt.

Net als bij de vorige stappen, is er weinig verschil tussen het implementeren van een REST-stijlcontract en een Relay WCF-contract.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Een Service Bus-contract in REST-stijl implementeren
1. Maak een nieuwe klasse met de naam **ImageService** direct na de definitie van de **IImageContract**-interface. Met de klasse **ImageService** wordt de **IImageContract**-interface geïmplementeerd.
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    Net als bij andere interface-implementaties kunt u de definitie implementeren in een ander bestand. In deze zelfstudie wordt de implementatie echter weergegeven in hetzelfde bestand als de interfacedefinitie en de `Main()`-methode.
2. Pas het kenmerk [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) op de klasse **IImageService** toe om aan te geven dat de klasse een implementatie is van een WCF-contract.
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    Zoals eerder is vermeld, is deze naamruimte geen traditionele naamruimte, maar maakt deze deel uit van de WCF-architectuur waarmee het contract wordt geïdentificeerd. Zie het onderwerp [Data Contract Names](https://msdn.microsoft.com/library/ms731045.aspx) (Gegevenscontractnamen) in de WCF-documentatie voor meer informatie.
3. Voeg een JPG-afbeelding toe aan uw project.  
   
    Dit is een afbeelding die de service in de ontvangende browser weergeeft. Klik met de rechtermuisknop op het project en klik op **Toevoegen**. Klik vervolgens op **Bestaand item**. Gebruik het dialoogvenster **Bestaand item toevoegen** om te bladeren naar een geschikt JPG-bestand en klik vervolgens op **Toevoegen**.
   
    Als u het bestand toevoegt, moet u ervoor zorgen dat **Alle bestanden** is geselecteerd in de vervolgkeuzelijst naast het veld **Bestandsnaam:**. In de rest van deze zelfstudie wordt ervan uitgegaan dat de naam van de afbeelding 'image.jpg' is. Als u een ander bestand gebruikt, moet u de naam van de afbeelding wijzigen of moet u uw code wijzigen.
4. Klik in **Solution Explorer** met de rechtermuisknop op het afbeeldingsbestand en klik op **Eigenschappen** om te controleren of de service die wordt uitgevoerd het affbeeldingsbestand kan vinden. Stel **Naar uitvoermap kopiëren** in het deelvenster **Eigenschappen** in op **Kopiëren indien nieuwer**.
5. Voeg een verwijzing naar de **System.Drawing.dll**-assembly toe aan het project en voeg tevens de volgende gekoppelde `using`-instructies toe.  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. Voeg in de klasse **ImageService** de volgende constructor toe waarmee de bitmap wordt geladen en voorbereid op verzending naar de clientbrowser.
   
    ```csharp
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";
   
        Image bitmap;
   
        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```
7. Voeg direct na de vorige code de volgende **GetImage**-methode toe aan de klasse **ImageService** om een HTTP-bericht met de afbeelding te retourneren.
   
    ```csharp
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);
   
        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";
   
        return stream;
    }
    ```
   
    Deze implementatie maakt gebruik van **MemoryStream** om de afbeelding op te halen en deze voor te bereiden op streaming naar de browser. De streampositie start bij nul, de stream wordt gedeclareerd als JPEG en de gegevens worden gestreamd.
8. Klik in het menu **Bouwen** op **Oplossing opbouwen**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>De configuratie voor het uitvoeren van de webservice in Service Bus definiëren
1. Dubbelklik in **Solution Explorer** op het bestand **App.config** om dit te openen in de Visual Studio-editor.
   
    De **App.config** bestand bevat de servicenaam, het eindpunt (dat wil zeggen, de locatie die Azure Relay biedt voor clients en hosts om te communiceren met elkaar) en de binding (het type protocol dat wordt gebruikt voor communicatie). Hier het belangrijkste verschil is dat het geconfigureerde service-eindpunt naar verwijst een [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding.
2. Het `<system.serviceModel>` XML-element is een WCF-element waarmee een of meer services worden gedefinieerd. Hier wordt het gebruikt om de servicenaam en het eindpunt te definiëren. Voeg onder aan het `<system.serviceModel>`-element (maar nog wel binnen `<system.serviceModel>`) een `<bindings>`-element toe met de volgende inhoud. Hiermee definieert u de bindingen die in de toepassing worden gebruikt. U kunt meerdere bindingen definiëren, maar in deze zelfstudie definieert u er slechts één.
   
    ```xml
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```
   
    De vorige code definieert een Relay WCF [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) binding met **relayClientAuthenticationType** ingesteld op **geen**. Met deze instelling geeft u aan dat voor een eindpunt dat deze binding gebruikt, geen clientreferentie is vereist.
3. Na het `<bindings>`-element voegt u een `<services>`-element toe. Net zoals bij bindingen kunt u meerdere services definiëren in een enkel configuratiebestand. In deze zelfstudie definieert u slechts een service.
   
    ```xml
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```
   
    In deze stap configureert u een service die de vooraf gedefinieerde **webHttpRelayBinding**-binding gebruikt. Deze service gebruikt ook de standaard **sbTokenProvider**, die in de volgende stap wordt gedefinieerd.
4. Na de `<services>` element, maakt een `<behaviors>` element met de volgende inhoud en vervang 'sas_key ' vervangen door de *Shared Access Signature* sleutel (SAS) u eerder hebt verkregen via de [Azure-portal] [Azure portal].
   
    ```xml
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```
5. Terwijl App.config actief is, vervangt u in het `<appSettings>`-element de hele verbindingsreekswaarde door de verbindingsreeks die u eerder hebt verkregen via de portal. 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. Klik in het menu **Bouwen** op **Oplossing opbouwen** om de volledige oplossing op te bouwen.

### <a name="example"></a>Voorbeeld
De volgende code toont het contract en de service-implementatie voor een op REST gebaseerde service die in Service Bus wordt uitgevoerd met de binding **WebHttpRelayBinding**.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

In het volgende voorbeeld wordt het aan de service gekoppelde bestand App.config weergegeven.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey="YOUR_SAS_KEY"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-azure-relay"></a>Stap 4: De REST gebaseerd WCF-service voor het gebruik van Azure Relay hosten
Deze stap wordt beschreven hoe een webservice met een consoletoepassing met WCF Relay uit te voeren. Een volledig overzicht van de code die in deze stap wordt geschreven, vindt u in het voorbeeld na de procedure.

### <a name="to-create-a-base-address-for-the-service"></a>Een basisadres voor de service maken
1. In de `Main()` declaratie van de functie, maakt u een variabele voor het opslaan van de naamruimte van uw project. Zorg ervoor dat u `yourNamespace` met de naam van de Relay-naamruimte die u eerder hebt gemaakt.
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    Service Bus gebruikt de naam van uw naamruimte om een unieke URI te maken.
2. Maak een `Uri`-exemplaar voor het basisadres van de service die is gebaseerd op de naamruimte.
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>De webservicehost maken en configureren
* Maak de webservicehost met het URI-adres dat u eerder in dit gedeelte hebt gemaakt.
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    De servicehost is het WCF-object waarmee de hosttoepassing wordt gestart. In dit voorbeeld wordt het type host dat u wilt maken (een **ImageService**), doorgegeven en ook het adres waarop u de hosttoepassing wilt weergeven.

### <a name="to-run-the-web-service-host"></a>De webservicehost uitvoeren
1. Open de service.
   
    ```csharp
    host.Open();
    ```
    De service wordt nu uitgevoerd.
2. Geef een bericht weer waarin wordt aangegeven dat de service wordt uitgevoerd en hoe deze kan worden gestopt.
   
    ```csharp
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. Sluit de servicehost wanneer u klaar bent.
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a>Voorbeeld
Het volgende voorbeeld bevat het servicecontract en de implementatie uit de vorige stappen in de zelfstudie. Hierin wordt de service gehost in een consoletoepassing. Compileer de volgende code in een uitvoerbaar bestand met de naam ImageListener.exe.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>De code compileren
Nadat u de oplossing hebt opgebouwd, gaat u als volgt te werk om de toepassing uit te voeren:

1. Druk op **F5** of blader naar de locatie van het uitvoerbare bestand (ImageListener\bin\Debug\ImageListener.exe) om de service uit te voeren. Zorg ervoor dat de app actief blijft. Dit is vereist voor het uitvoeren van de volgende stap.
2. Kopieer en plak het adres van de opdrachtprompt in een browser om de afbeelding te zien.
3. Druk op **Enter** in het opdrachtpromptvenster om de app te sluiten wanneer u klaar bent.

## <a name="next-steps"></a>Volgende stappen
Nu u een toepassing die gebruikmaakt van de Service Bus relay-service hebt gemaakt, gaat u naar de volgende artikelen voor meer informatie over Azure Relay:

* [Overzicht van Azure Service Bus-architectuur](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [Overzicht van Azure Relay](relay-what-is-it.md)
* [De relay WCF-service gebruiken met .NET](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com

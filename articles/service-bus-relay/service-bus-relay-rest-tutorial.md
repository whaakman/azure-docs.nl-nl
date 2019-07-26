---
title: REST zelf studie met Azure Relay | Microsoft Docs
description: Bouw een eenvoudige Azure Service Bus Relay Host-toepassing die een op REST gebaseerde interface beschikbaar stelt.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/06/2018
ms.author: spelluru
ms.openlocfilehash: 4e988724f842ff12cd599eba95c31006fe208fad
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422885"
---
# <a name="azure-wcf-relay-rest-tutorial"></a>REST-zelf studie voor Azure WCF Relay
In deze zelf studie wordt beschreven hoe u een eenvoudige Azure Relay-hosttoepassing bouwt die een op REST gebaseerde interface beschrijft. Met REST kan een webclient, zoals een webbrowser, toegang krijgen tot de Service Bus-API's via HTTP-aanvragen.

De zelf studie maakt gebruik van het Windows Communication Foundation (WCF) REST programmeer model voor het bouwen van een REST-service op Azure Relay. Zie [WCF REST Programming Model](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) (WCF REST-programmeermodel) en [Designing and Implementing Services](/dotnet/framework/wcf/designing-and-implementing-services) (Services ontwerpen en implementeren) in de WCF-documentatie voor meer informatie.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een relay-naam ruimte maken.
> * Een op REST gebaseerd WCF-service contract definiëren
> * Het op REST gebaseerde WCF-contract implementeren
> * Host en voer de op REST gebaseerde WCF-service uit
> * De service uitvoeren en testen

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Een Azure-abonnement. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.
- [Visual Studio 2015 of hoger](https://www.visualstudio.com). In de voorbeelden in deze zelfstudie wordt Visual Studio 2017 gebruikt.
- Azure-SDK voor .NET. Installeer de SDK via de [SDK-downloadpagina](https://azure.microsoft.com/downloads/).

## <a name="create-a-relay-namespace"></a>Een relay-naam ruimte maken

Als u de relayfuncties in Azure wilt gebruiken, moet u eerst een servicenaamruimte maken. Een naamruimte biedt een scoping container voor het verwerken van Azure-resources in uw toepassing. Volg [deze instructies](relay-create-namespace-portal.md) om een Relay-naamruimte te maken.

## <a name="define-a-rest-based-wcf-service-contract-to-use-with-azure-relay"></a>Definieer een op REST gebaseerd WCF-service contract voor gebruik met Azure Relay

Wanneer u een service voor de REST-stijl van WCF maakt, moet u het contract definiëren. Het contract geeft aan welke bewerkingen door de host worden ondersteund. Een servicebewerking kan worden beschouwd als een webservicemethode. Contracten worden gemaakt door een C++-, C#- of Visual Basic-interface te definiëren. Elke methode in de interface komt overeen met een specifieke servicebewerking. Op elke interface moet het kenmerk [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) worden toegepast en op elke bewerking moet het kenmerk [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute) worden toegepast. Als een methode in een interface met het kenmerk [ServiceContractAttribute](/dotnet/api/system.servicemodel.servicecontractattribute) niet beschikt over het kenmerk [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute), wordt die methode niet weergegeven. In het voorbeeld na de procedure wordt de code weergegeven die voor deze taken wordt gebruikt.

Het belangrijkste verschil tussen een WCF-contract en een REST-stijl contract is het toevoegen van een eigenschap aan de [OperationContractAttribute](/dotnet/api/system.servicemodel.operationcontractattribute): [WebGetAttribute](/dotnet/api/system.servicemodel.web.webgetattribute). Met deze eigenschap kunt u een methode in uw interface toewijzen aan een methode aan de andere kant van de interface. In dit voor beeld wordt het attributes [WebGetAttribute](/dotnet/api/system.servicemodel.web.webgetattribute) gebruikt om een methode aan http Get te koppelen. Hierdoor kunnen Service Bus opdrachten die naar de interface worden verzonden, nauw keurig ophalen en interpreteren.

### <a name="to-create-a-contract-with-an-interface"></a>Een contract met een interface maken

1. Open Visual Studio als beheerder: klik met de rechtermuisknop op het programma in het menu **Start** en klik vervolgens op **Als administrator uitvoeren**.
2. Maak een nieuw consoletoepassingsproject aan. Klik op het menu **Bestand**, selecteer **Nieuw** en selecteer vervolgens **Project**. Klik in het dialoogvenster **Nieuw project** op **Visual C#** , selecteer de sjabloon **Consoletoepassing** en geef deze de naam **ImageListener**. Gebruik de standaardwaarde voor **Locatie**. Klik op **OK** om het project te maken.
3. Voor een C#-project maakt Visual Studio een `Program.cs`-bestand. Deze klasse bevat een lege `Main()`-methode, vereist voor de juiste opbouw van een consoletoepassingsproject.
4. Voeg verwijzingen naar Service Bus en **System.ServiceModel.dll** aan het project toe door het Service Bus NuGet-pakket te installeren. Met dit pakket worden automatisch verwijzingen naar de Service Bus-bibliotheken en naar het **System.ServiceModel** van WCF toegevoegd. Klik in Solution Explorer met de rechtermuisknop op het project **ImageListener** en klik vervolgens op **NuGet-pakketten beheren**. Klik op het tabblad **Bladeren** en zoek vervolgens naar `Microsoft Azure Service Bus`. Klik op **Installeren** en accepteer de gebruiksvoorwaarden.
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
   
    [System.ServiceModel](/dotnet/api/system.servicemodel) is de naamruimte die programmatisch toegang biedt tot de basisfuncties van WCF. WCF Relay maakt gebruik van veel van de objecten en kenmerken van WCF om service contracten te definiëren. U gebruikt deze naam ruimte in de meeste van uw relay-toepassingen. Net als [System. service model. channels](/dotnet/api/system.servicemodel.channels) kunt u het kanaal definiëren. Dit is het object waarmee u communiceert met Azure relay en de webbrowser van de client. Tot slot bevat [System.ServiceModel.Web](/dotnet/api/system.servicemodel.web) de typen waarmee u webtoepassingen kunt maken.
7. Wijzig de naam van de naamruimte `ImageListener` in **Microsoft.ServiceBus.Samples**.
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. Definieer meteen na de openingsaccolade van de naamruimtedeclaratie een nieuwe interface met de naam **IImageContract** en pas het kenmerk **ServiceContractAttribute** met de waarde `https://samples.microsoft.com/ServiceModel/Relay/` toe op de interface. De naamruimtewaarde verschilt van de naamruimte die u in uw code gebruikt. De naamruimtewaarde wordt gebruikt als een unieke id voor dit contract en moet versie-informatie bevatten. Zie [Serviceversiebeheer](https://go.microsoft.com/fwlink/?LinkID=180498) voor meer informatie. Door de naamruimte expliciet op te geven, wordt voorkomen dat de standaardnaamruimtewaarde wordt toegevoegd aan de naam van het contract.
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
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
    
    Hierdoor kan de Relay-service HTTP Get-aanvragen routeren `GetImage`naar en de retour waarden van `GetImage` in een http GETRESPONSE-antwoord omzetten. Verderop in de zelfstudie gebruikt u een webbrowser om toegang te krijgen tot deze methode en de installatiekopie weer te geven in de browser.
11. Declareer direct na de `IImageContract`-definitie een kanaal dat de eigenschappen overneemt van de `IImageContract`- en `IClientChannel`-interface.
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    Een kanaal is het WCF-object waarmee de service en de client informatie aan elkaar doorgeven. Later maakt u het kanaal in uw host-toepassing. Azure Relay maakt vervolgens gebruik van dit kanaal om de HTTP GET-aanvragen van de browser door te geven aan uw **GetImage** -implementatie. De relay gebruikt ook het kanaal om de retour waarde **GetImage** te nemen en deze te vertalen in een http-GETRESPONSE voor de client browser.
12. Klik in het menu **Bouwen** op **Oplossing opbouwen** om de juistheid van uw werk tot nu toe te controleren.

### <a name="example"></a>Voorbeeld
De volgende code toont een basis interface die een WCF Relay contract definieert.

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

    [ServiceContract(Name = "IImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
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

## <a name="implement-the-rest-based-wcf-service-contract"></a>Het op REST gebaseerde WCF-service contract implementeren
Als u een REST-Style WCF Relay-service wilt maken, moet u eerst het contract maken. dit wordt gedefinieerd door middel van een interface. De volgende stap is het implementeren van de interface. Hiervoor moet u een klasse met de naam **ImageService** maken die de door de gebruiker gedefinieerde **IImageContract**-interface implementeert. Nadat het contract is geïmplementeerd, configureert u de interface met een App.config-bestand. Het configuratie bestand bevat de benodigde informatie voor de toepassing, zoals de naam van de service, de naam van het contract en het type protocol dat wordt gebruikt om te communiceren met de Relay-service. In het voorbeeld na de procedure wordt de code weergegeven die voor deze taken wordt gebruikt.

Net als bij de vorige stappen is er zeer weinig verschil tussen het implementeren van een REST-stijl contract en een WCF Relay-contract.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Een Service Bus-contract in REST-stijl implementeren
1. Maak een nieuwe klasse met de naam **ImageService** direct na de definitie van de **IImageContract**-interface. Met de klasse **ImageService** wordt de **IImageContract**-interface geïmplementeerd.
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    Net als bij andere interface-implementaties kunt u de definitie implementeren in een ander bestand. In deze zelfstudie wordt de implementatie echter weergegeven in hetzelfde bestand als de interfacedefinitie en de `Main()`-methode.
2. Pas het kenmerk [ServiceBehaviorAttribute](/dotnet/api/system.servicemodel.servicebehaviorattribute) op de klasse **IImageService** toe om aan te geven dat de klasse een implementatie is van een WCF-contract.
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    Zoals eerder is vermeld, is deze naamruimte geen traditionele naamruimte, maar maakt deze deel uit van de WCF-architectuur waarmee het contract wordt geïdentificeerd. Zie het artikel [gegevens contract namen](https://msdn.microsoft.com/library/ms731045.aspx) in de WCF-documentatie voor meer informatie.
3. Voeg een JPG-afbeelding toe aan uw project.  
   
    Dit is een afbeelding die de service in de ontvangende browser weergeeft. Klik met de rechtermuisknop op het project en klik op **Toevoegen**. Klik vervolgens op **Bestaand item**. Gebruik het dialoogvenster **Bestaand item toevoegen** om te bladeren naar een geschikt JPG-bestand en klik vervolgens op **Toevoegen**.
   
    Als u het bestand toevoegt, moet u ervoor zorgen dat **Alle bestanden** is geselecteerd in de vervolgkeuzelijst naast het veld **Bestandsnaam:** . In de rest van deze zelfstudie wordt ervan uitgegaan dat de naam van de afbeelding 'image.jpg' is. Als u een ander bestand hebt, moet u de naam van de installatie kopie wijzigen of uw code wijzigen om te compenseren.
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
   
    Het bestand **app. config** bevat de service naam, het eind punt (dat wil zeggen, de locatie Azure relay geeft aan dat clients en hosts kunnen communiceren met elkaar) en binding (het type protocol dat wordt gebruikt om te communiceren). Het belangrijkste verschil is hier dat het geconfigureerde service-eind punt verwijst naar een [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) -binding.
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
   
    Met de vorige code wordt een WCF Relay [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) -binding gedefinieerd waarvoor **relayClientAuthenticationType** is ingesteld op **geen**. Met deze instelling geeft u aan dat voor een eindpunt dat deze binding gebruikt, geen clientreferentie is vereist.
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
4. Maak na `<services>` het-element een `<behaviors>` element met de volgende inhoud en vervang "SAS_KEY" door de *Shared Access Signature* SAS-sleutel die u eerder van de [Azure Portal][Azure portal]hebt verkregen.
   
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


    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
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
        <!-- Service Bus specific app settings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY>"/>
    </appSettings>
</configuration>
```

## <a name="host-the-rest-based-wcf-service-to-use-azure-relay"></a>Host de op REST gebaseerde WCF-service voor het gebruik van Azure Relay
In deze stap wordt beschreven hoe u een webservice uitvoert met behulp van een console toepassing met WCF Relay. Een volledig overzicht van de code die in deze stap wordt geschreven, vindt u in het voorbeeld na de procedure.

### <a name="to-create-a-base-address-for-the-service"></a>Een basisadres voor de service maken
1. Maak in `Main()` de functie declaratie een variabele om de naam ruimte van het project op te slaan. Vervang `yourNamespace` door de naam van de relay-naam ruimte die u eerder hebt gemaakt.
   
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

### <a name="example"></a>Voorbeeld
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

    [ServiceContract(Name = "ImageContract", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "https://samples.microsoft.com/ServiceModel/Relay/")]
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

## <a name="run-and-test-the-service"></a>De service uitvoeren en testen
Nadat u de oplossing hebt opgebouwd, gaat u als volgt te werk om de toepassing uit te voeren:

1. Druk op **F5** of blader naar de locatie van het uitvoerbare bestand (ImageListener\bin\Debug\ImageListener.exe) om de service uit te voeren. Zorg ervoor dat de app actief blijft. Dit is vereist voor het uitvoeren van de volgende stap.
2. Kopieer en plak het adres van de opdrachtprompt in een browser om de afbeelding te zien.
3. Druk op **Enter** in het opdrachtpromptvenster om de app te sluiten wanneer u klaar bent.

## <a name="next-steps"></a>Volgende stappen
Nu u een toepassing hebt gebouwd die gebruikmaakt van de Azure Relay-service, raadpleegt u de volgende artikelen voor meer informatie:

* [Overzicht van Azure Relay](relay-what-is-it.md)
* [De WCF Relay-service gebruiken met .NET](service-bus-relay-tutorial.md)

[Azure portal]: https://portal.azure.com

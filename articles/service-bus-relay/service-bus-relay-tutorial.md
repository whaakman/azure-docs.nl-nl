---
title: Azure Service Bus WCF Relay-zelfstudie | Microsoft Docs
description: Een client en service-toepassing met WCF Relay bouwt.
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: sethm
ms.openlocfilehash: a0b06c32cf5f154cf5eb01842d9b917dcb35f7b3
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="azure-wcf-relay-tutorial"></a>Azure Relay WCF-zelfstudie

Deze zelfstudie wordt beschreven hoe u een eenvoudige WCF Relay-clienttoepassing en met Azure Relay-service. Voor een vergelijkbare zelfstudie waarin [Service Bus-berichtenservice](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), Zie [aan de slag met Service Bus-wachtrijen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

Het uitvoeren van deze zelfstudie hebt u een goed begrip van de stappen die nodig zijn voor het maken van een Relay WCF-client en service-toepassing. Als de oorspronkelijke WCF is is een service een middel met een of meer eindpunten, die allemaal een of meer servicebewerkingen. Het eindpunt van een service bevat het adres van de service, een binding met de informatie die een client moet doorgeven aan de service en een contract waarin staat welke functionaliteit de service biedt aan de clients. Het belangrijkste verschil tussen de WCF- en WCF Relay is dat het eindpunt wordt weergegeven in de cloud in plaats van lokaal op uw computer.

Als u de onderwerpen in deze zelfstudie op de juiste volgorde doorloopt, hebt u een actieve service en een client waarmee de bewerkingen van de service kunnen worden aangeroepen. In het eerste onderwerp wordt beschreven hoe u een account instelt. In de volgende stappen wordt beschreven hoe u een service definieert die gebruikmaakt van een contract, hoe u de service implementeert en hoe u de service in code configureert. Er wordt ook beschreven hoe u kunt hosten en hoe u de service uitvoert. De service die is gemaakt, host zichzelf. De client en de service worden uitgevoerd op dezelfde computer. U kunt de service configureren met code of met een configuratiebestand.

In de laatste drie stappen wordt beschreven hoe u een clienttoepassing maakt, hoe u de clienttoepassing configureert en hoe u een client maakt en gebruikt die toegang heeft tot de functionaliteit van de host.

## <a name="prerequisites"></a>Vereisten

Om deze handleiding volledig door te kunnen nemen, hebt u het volgende nodig:

* [Microsoft Visual Studio 2015 of hoger](http://visualstudio.com). Deze zelfstudie wordt Visual Studio 2017.
* Een actief Azure-account. Als u geen Azure-account hebt, kunt u binnen een paar minuten een gratis account maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/free/) voor meer informatie.

## <a name="create-a-service-namespace"></a>Een servicenaamruimte maken

De eerste stap het maken van een naamruimte en ophalen is een [Shared Access Signature (SAS)](../service-bus-messaging/service-bus-sas.md) sleutel. Een naamruimte biedt een toepassingsbegrenzing voor elke toepassing die toegankelijk is via de relay-service. Een SAS-sleutel wordt automatisch door het systeem gegenereerd wanneer een servicenaamruimte wordt gemaakt. De combinatie van Servicenaamruimte en SAS-sleutel biedt de referenties voor Azure verifiëren van toegang tot een toepassing. Volg [deze instructies](relay-create-namespace-portal.md) om een Relay-naamruimte te maken.

## <a name="define-a-wcf-service-contract"></a>Een WCF-servicecontract definiëren

Het servicecontract wordt aangegeven welke bewerkingen (de webserviceterminologie voor methoden of functies) de service ondersteunt. Contracten worden gemaakt door een C++-, C#- of Visual Basic-interface te definiëren. Elke methode in de interface komt overeen met een specifieke servicebewerking. Op elke interface moet het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) worden toegepast en op elke bewerking moet het kenmerk [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) worden toegepast. Als een methode in een interface met het kenmerk [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) niet beschikt over het kenmerk [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), wordt die methode niet weergegeven. In het voorbeeld na de procedure wordt de code voor deze taken weergegeven. Zie [Services ontwerpen en implementeren](https://msdn.microsoft.com/library/ms729746.aspx) in de WCF-documentatie voor meer informatie over contracten en services.

### <a name="create-a-relay-contract-with-an-interface"></a>Een relay-contract met een interface maken

1. Open Visual Studio als beheerder door met de rechtermuisknop achtereenvolgens op het programma in het menu **Start** en op **Als administrator uitvoeren** te klikken.
2. Maak een nieuw consoletoepassingsproject. Klik op het menu **Bestand**, selecteer **Nieuw** en klik vervolgens op **Project**. Klik in het dialoogvenster **Nieuw project** op **Visual C#** (als **Visual C#** niet wordt weergegeven, kijkt u bij **Andere talen**). Klik op de **Console-App (.NET Framework)** -sjabloon en noem deze **EchoService**. Klik op **OK** om het project te maken.

    ![][2]

3. Installeer het Service Bus-pakket NuGet. Met dit pakket worden automatisch verwijzingen naar de Service Bus-bibliotheken en naar het **System.ServiceModel** van WCF toegevoegd. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) is de naamruimte die programmatisch toegang biedt tot de basisfuncties van WCF. Service Bus maakt gebruik van veel van de objecten en kenmerken van WCF om servicecontracten te definiëren.

    Klik in Solution Explorer met de rechtermuisknop op het project en klik vervolgens op **NuGet-pakketten beheren...** . Klik op het tabblad **Bladeren** en zoek vervolgens naar **WindowsAzure.ServiceBus**. Zorg ervoor dat de naam van het project is geselecteerd in het vak **Versie(s)**. Klik op **Installeren** en accepteer de gebruiksvoorwaarden.

    ![][3]
4. Dubbelklik in Solution Explorer op het bestand Program.cs om het in de editor te openen, als het bestand nog niet was geopend.
5. Voeg boven aan het bestand de volgende instructies toe:

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. Wijzig de standaardnaam van de naamruimte (**EchoService**) naar **Microsoft.ServiceBus.Samples**.

   > [!IMPORTANT]
   > Deze zelfstudie wordt gebruikgemaakt van de C#-naamruimte **Microsoft.ServiceBus.Samples**, dit is de naamruimte van de contract-gebaseerde beheerde type dat wordt gebruikt in het configuratiebestand in de [de WCF-client configureren](#configure-the-wcf-client) stap. U kunt bij het maken van dit voorbeeld elke gewenste naamruimte opgeven. Voor de zelfstudie moet u echter wel de naamruimten van het contract en de service naar behoren wijzigen in het configuratiebestand van de toepassing. De opgegeven naamruimte in het bestand App.config moet gelijk zijn aan de opgegeven naamruimte in de C#-bestanden.
   >
   >
7. Direct na de `Microsoft.ServiceBus.Samples` naamruimtedeclaratie, maar in de naamruimte, definieert u een nieuwe interface met de naam `IEchoContract` en toe te passen de `ServiceContractAttribute` kenmerk aan de interface met een naamruimtewaarde van `http://samples.microsoft.com/ServiceModel/Relay/`. De naamruimtewaarde verschilt van de naamruimte die u in uw code gebruikt. In plaats daarvan wordt de naamruimtewaarde gebruikt als een unieke id voor dit contract. Door de naamruimte expliciet op te geven, wordt voorkomen dat de standaardnaamruimtewaarde wordt toegevoegd aan de naam van het contract. Plak de volgende code na de naamruimtedeclaratie:

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > De naamruimte van het servicecontract bevat meestal een schematische naam die ook versie-informatie bevat. Als u versie-informatie in de naamruimte van het servicecontract gebruikt, kunnen services grote wijzigingen isoleren door een nieuw servicecontract te definiëren met een nieuwe naamruimte en het beschikbaar te maken op een nieuw eindpunt. Op deze manier blijven clients het oude servicecontract gebruiken zonder dat moet worden bijgewerkt. De versie-informatie kan bestaan uit een datum of een buildnummer. Zie [Serviceversiebeheer](http://go.microsoft.com/fwlink/?LinkID=180498) voor meer informatie. Voor deze zelfstudie bevat de naam van de servicecontractnaamruimte geen versie-informatie.
   >
   >
8. Binnen de `IEchoContract` interface, een methode voor de enkelvoudige bewerking declareren de `IEchoContract` contract wordt weergegeven in de interface en pas de `OperationContractAttribute` kenmerk voor de methode die u wilt weergeven als onderdeel van het openbare Relay WCF-contract als volgt:

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. Direct na het definiëren van de `IEchoContract`-interface declareert u een kanaal dat eigenschappen overneemt van de interfaces `IEchoContract` en `IClientChannel`. Ga als volgt te werk:

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Een kanaal is het WCF-object waarmee de host en de client informatie aan elkaar doorgeven. Later schrijft u code voor het kanaal om informatie als echo te verzenden tussen de twee toepassingen.
10. Klik in het menu **Bouwen** op **Oplossing opbouwen** of druk op **Ctrl + Shift + B** om de juistheid van uw werk tot nu toe te controleren.

### <a name="example"></a>Voorbeeld

De volgende code toont een eenvoudige interface die een Relay WCF-contract wordt gedefinieerd.

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Nu de interface is gemaakt, kunt u de interface implementeren.

## <a name="implement-the-wcf-contract"></a>De WCF-contract implementeren

Maken van een Azure relay, moet u eerst het contract, die is gedefinieerd met behulp van een interface maken. Zie de vorige stap voor meer informatie over het maken van de interface. De volgende stap is het implementeren van de interface. Hiervoor moet u een klasse maken met de naam `EchoService` die de door de gebruiker gedefinieerde `IEchoContract`-interface implementeert. Nadat de interface is geïmplementeerd, configureert u de interface met een App.config-configuratiebestand. Het configuratiebestand bevat de benodigde informatie voor de toepassing, zoals de naam van de service, de naam van het contract en het type protocol dat wordt gebruikt om te communiceren met de relay-service. In het voorbeeld na de procedure wordt de code weergegeven die voor deze taken wordt gebruikt. Zie [Servicecontracten implementeren](https://msdn.microsoft.com/library/ms733764.aspx) in de WCF-documentatie voor algemene informatie over het implementeren van een servicecontract.

1. Maak een nieuwe klasse met de naam `EchoService` direct nadat de `IEchoContract`-interface is gedefinieerd. Met de klasse `EchoService` wordt de `IEchoContract`-interface geïmplementeerd.

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    Net als bij andere interface-implementaties kunt u de definitie implementeren in een ander bestand. In deze zelfstudie staat de implementatie echter in hetzelfde bestand als de interfacedefinitie en de `Main`-methode.
2. Pas het kenmerk [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) toe op de `IEchoContract`-interface. Aan het kenmerk ziet u wat de servicenaam en naamruimte zijn. Wanneer u dit hebt gedaan, wordt de klasse `EchoService` als volgt weergegeven:

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. Implementeer de `Echo`-methode die is gedefinieerd in de interface `IEchoContract` in de klasse `EchoService`.

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. Klik op **Bouwen** en vervolgens op **Oplossing opbouwen** om de juistheid van uw werk te controleren.

### <a name="define-the-configuration-for-the-service-host"></a>De configuratie voor de ServiceHost definiëren

1. Het configuratiebestand lijkt erg op een WCF-configuratiebestand. Het bevat de servicenaam, eindpunt (dat wil zeggen, de locatie die Azure Relay beschikbaar maakt voor clients en hosts om te communiceren met elkaar) en de binding (het type protocol dat wordt gebruikt voor communicatie). Het belangrijkste verschil is dat het geconfigureerde service-eindpunt verwijst naar een [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding)-binding die geen deel uitmaakt van het .NET Framework. [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) is een van de bindingen die is gedefinieerd door de service.
2. Dubbelklik in **Solution Explorer** op het bestand App.config om dit te openen in de Visual Studio-editor.
3. Vervang in het element `<appSettings>` de tijdelijke aanduidingen door de naam van uw servicenaamruimte en de SAS-sleutel die u in een eerdere stap hebt gekopieerd.
4. In de `<system.serviceModel>`-tags voegt u een `<services>`-element toe. U kunt meerdere relay-toepassingen definiëren in een enkel configuratiebestand. In deze zelfstudie wordt er echter maar één gedefinieerd.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. Voeg in het `<services>`-element een `<service>`-element toe om de naam van de service te definiëren.

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. Geef in het `<service>`-element de locatie op van het eindpuntcontract en het type binding voor het eindpunt.

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Het eindpunt definieert waar de client zoekt naar de hosttoepassing. De zelfstudie wordt later in deze stap voor het maken van een URI die volledig beschikbaar wordt gemaakt van de host via Azure Relay. De binding weet u dat we TCP als protocol gebruikt om te communiceren met de relay-service.
7. Klik in het menu **Bouwen** op **Oplossing opbouwen** om de juistheid van uw werk te controleren.

### <a name="example"></a>Voorbeeld

In de volgende code staat de implementatie van het servicecontract.

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

In de volgende code staat de basisindeling van het aan de servicehost gekoppelde bestand App.config.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-the-relay-service"></a>Hosten en uitvoeren van een eenvoudige webservice registreren bij de relay-service

Deze stap wordt beschreven hoe een Azure-Relay-service uit te voeren.

### <a name="create-the-relay-credentials"></a>De relay-referenties maken

1. Maak in `Main()` twee variabelen waarin de naamruimte en de SAS-sleutel uit het consolevenster worden opgeslagen.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    De SAS-sleutel wordt later gebruikt voor toegang tot uw project. De naamruimte wordt als parameter doorgegeven aan `CreateServiceUri` voor het maken van een service-URI.
2. Met een [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior)-object geeft u aan dat u een SAS-sleutel gaat gebruiken als referentietype. Voeg de volgende code toe direct na de code die u in de vorige stap hebt toegevoegd.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-the-service"></a>Een basisadres voor de service maken

Nadat de code die u in de vorige stap hebt toegevoegd, maakt u een `Uri` exemplaar voor het basisadres van de service. Deze URI bevat het Service Bus-schema, de naamruimte en het pad naar de service-interface.

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

'sb' staat voor het Service Bus-schema en geeft aan dat TCP als protocol wordt gebruikt. Dit is ook eerder aangegeven in het configuratiebestand, toen [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) is opgegeven als binding.

De URI voor deze zelfstudie is `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="create-and-configure-the-service-host"></a>Maak en configureer de ServiceHost

1. Stel de connectiviteitsmodus in op `AutoDetect`.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    De connectiviteitsmodus wordt het protocol dat de service gebruikt om te communiceren met de relay-service beschreven via HTTP of TCP. Met de standaardinstelling `AutoDetect`, probeert de service verbinding maken met Azure Relay via TCP als deze beschikbaar is en HTTP als TCP niet beschikbaar is. Dit wijkt af van het protocol dat de service opgeeft voor clientcommunicatie. Dat protocol wordt bepaald op basis van de gebruikte binding. Bijvoorbeeld, een service kunt gebruiken de [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) binding die aangeeft dat het eindpunt met clients via HTTP communiceert. Die dezelfde service kan opgeeft **ConnectivityMode.AutoDetect** zodat de service via TCP met Azure Relay communiceert.
2. Maak de servicehost met de URI die u eerder in dit gedeelte hebt gemaakt.

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    De servicehost is het WCF-object dat de service start. Hier geeft u aan welk type service u wilt maken (een `EchoService`-type) en het adres waarmee u de service beschikbaar wilt maken.
3. Boven aan het bestand Program.cs voegt u verwijzingen toe naar [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) en [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. Configureer vervolgens in `Main()` het eindpunt voor openbare toegang.

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Deze stap informeert de relay-service die uw toepassing openbaar vinden is te door de ATOM-feed voor uw project in. Als u **DiscoveryType** instelt op **private**, hebben clients nog steeds toegang tot de service. De service wordt niet weergegeven bij het zoeken van de Relay-naamruimte. In plaats daarvan moet de client op voorhand al op de hoogte zijn van het eindpuntpad.
5. Pas de servicereferenties toe op de service-eindpunten die zijn opgegeven in het bestand App.config:

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Zoals vermeld in de vorige stap, kan het zijn dat u meerdere services en eindpunten hebt opgegeven in het configuratiebestand. Als dit het geval is, is met de code het hele configuratiebestand doorzocht naar eindpunten waarop uw referenties kunnen worden toegepast. Voor deze zelfstudie heeft het configuratiebestand echter slechts één eindpunt.

### <a name="open-the-service-host"></a>De ServiceHost openen

1. Open de service.

    ```csharp
    host.Open();
    ```
2. Stel de gebruiker ervan op de hoogte dat de service wordt uitgevoerd en leg uit hoe de service kan worden afgesloten.

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```
3. Sluit de servicehost wanneer u klaar bent.

    ```csharp
    host.Close();
    ```
4. Druk op **Ctrl + Shift + B** om het project op te bouwen.

### <a name="example"></a>Voorbeeld

Uw servicecode voltooide ziet er als volgt. De code bevat het servicecontract en de implementatie uit de vorige stappen in de zelfstudie en de service gehost in een consoletoepassing.

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Relay credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Een WCF-client maken voor het servicecontract

De volgende stap is een clienttoepassing maken en definiëren van het servicecontract dat u in latere stappen gaat implementeren. Opmerking dat veel van deze stappen zijn vergelijkbaar met de stappen voor het maken van een service: een contract, bewerkt een App.config-bestand, met behulp van referenties verbinding maken met de relay-service, enzovoort. In het voorbeeld na de procedure wordt de code weergegeven die voor deze taken wordt gebruikt.

1. Maak een nieuw project in de huidige Visual Studio-oplossing voor de client. Dit doet u als volgt:

   1. Klik in Solution Explorer, in de oplossing die de service bevat, met de rechtermuisknop op de huidige oplossing (niet op het project). Klik vervolgens op **Toevoegen**. Klik vervolgens op **Nieuw project**.
   2. In de **Add New Project** in het dialoogvenster klikt u op **Visual C#** (als **Visual C#** niet wordt weergegeven, kijkt u onder **andere talen**), selecteer de **Console-App (.NET Framework)** -sjabloon en noem deze **EchoClient**.
   3. Klik op **OK**.
      <br />
2. Dubbelklik in Solution Explorer op het bestand Program.cs in het project **EchoClient** om het in de editor te openen, als het bestand nog niet was geopend.
3. Verander de standaardnaam `EchoClient` van de naamruimte in `Microsoft.ServiceBus.Samples`.
4. Installeer de [Service Bus NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus): Klik in Solution Explorer met de rechtermuisknop op de **EchoClient** project en klik vervolgens op **NuGet-pakketten beheren**. Klik op het tabblad **Bladeren** en zoek vervolgens naar `Microsoft Azure Service Bus`. Klik op **Installeren** en accepteer de gebruiksvoorwaarden.

    ![][3]
5. Voeg een `using`-instructie toe voor de naamruimte [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) in het bestand Program.cs.

    ```csharp
    using System.ServiceModel;
    ```
6. Voeg de definitie van het servicecontract toe aan de naamruimte, zoals wordt weergegeven in het volgende voorbeeld. Deze definitie is identiek aan de definitie die is gebruikt in het project **Service**. U moet deze code toevoegen aan de bovenkant van de `Microsoft.ServiceBus.Samples`-naamruimte.

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. Druk op **Ctrl + Shift + B** om de client op te bouwen.

### <a name="example"></a>Voorbeeld

De volgende code toont de huidige status van het bestand Program.cs in het **EchoClient** project.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>De WCF-client configureren

In deze stap maakt u een App.config-bestand voor een eenvoudige clienttoepassing die de service gebruikt die u eerder in deze zelfstudie hebt gemaakt. Dit App.config-bestand definieert het contract, de binding en de naam van het eindpunt. In het voorbeeld na de procedure wordt de code weergegeven die voor deze taken wordt gebruikt.

1. Dubbelklik in Solution Explorer in het project **EchoClient** op het bestand **App.config** om dit te openen in de Visual Studio-editor.
2. Vervang in het element `<appSettings>` de tijdelijke aanduidingen door de naam van uw servicenaamruimte en de SAS-sleutel die u in een eerdere stap hebt gekopieerd.
3. Voeg in het element system.serviceModel een `<client>`-element toe.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Tijdens deze stap wordt aangegeven dat u een clienttoepassing van het type WCF maakt.
4. Geef in het `client`-element de naam, het contract en het type binding op voor het eindpunt.

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Deze stap definieert de naam van het eindpunt, het contract dat is gedefinieerd in de service en het feit dat de clienttoepassing gebruikmaakt van TCP om te communiceren met Azure Relay. De naam van het eindpunt wordt in de volgende stap gebruikt om deze eindpuntconfiguratie te koppelen aan de service-URI.
5. Klik op **bestand**, klikt u vervolgens op **Alles opslaan**.

## <a name="example"></a>Voorbeeld

In de volgende code staat het App.config-bestand voor de Echo-client.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client"></a>De WCF-client implementeren
In deze stap implementeert u een eenvoudige clienttoepassing die de service gebruikt die u eerder in deze zelfstudie hebt gemaakt. De client is vergelijkbaar met de service, voeren veel van dezelfde bewerkingen voor toegang tot Azure Relay:

1. De connectiviteitsmodus wordt ingesteld.
2. De URI wordt gemaakt waarmee de hostservice wordt gezocht.
3. De beveiligingsreferenties worden gedefinieerd.
4. De referenties worden toegepast op de verbinding.
5. De verbinding wordt geopend.
6. De toepassingsspecifieke taken worden uitgevoerd.
7. De verbinding wordt gesloten.

Een van de belangrijkste verschillen is echter de clienttoepassing een kanaal gebruikt om verbinding met de relay-service dat de service een aanroep van gebruikt **ServiceHost**. In het voorbeeld na de procedure wordt de code weergegeven die voor deze taken wordt gebruikt.

### <a name="implement-a-client-application"></a>Een clienttoepassing implementeren
1. Stel de connectiviteitsmodus in op **AutoDetect**. Voeg de volgende code toe in de `Main()`-methode van de **EchoClient**-toepassing.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. Geef variabelen op voor de waarden van de naamruimte van de service. Geef ook de SAS-sleutel op die in de console wordt weergegeven.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. De URI die definieert de locatie van de host in uw Relay-project maken.

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. Maak het referentieobject voor het eindpunt van uw servicenaamruimte.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. Maak de kanaalfactory die de configuratie laadt die wordt beschreven in het App.config-bestand.

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Een kanaalfactory is een WCF-object dat een kanaal maakt waardoor de service- en clienttoepassingen communiceren.
6. De referenties zijn van toepassing.

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. Maak en open het kanaal naar de service.

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. Schrijf de algemene gebruikersinterface en de functionaliteit voor de echo.

    ```csharp
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Voor de code wordt het exemplaar van het kanaalobject gebruikt als proxy voor de service.
9. Sluit het kanaal en sluit de factory.

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a>Voorbeeld

De volledige code ziet er als volgt met het maken van een clienttoepassing, het aanroepen van de bewerkingen van de service en het sluiten van de client na de bewerkingsaanroep is voltooid.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-the-applications"></a>De toepassingen uitvoeren

1. Druk op **Ctrl + Shift + B** om de oplossing op te bouwen. Hiermee worden zowel het clientproject als het serviceproject opgebouwd die u in de vorige stappen hebt gemaakt.
2. Voordat u de clienttoepassing uitvoert, moet u ervoor zorgen dat de servicetoepassing wordt uitgevoerd. Klik in Visual Studio Solution Explorer met de rechtermuisknop op de oplossing **EchoService**. Klik vervolgens op **Eigenschappen**.
3. Klik in het dialoogvenster Oplossingseigenschappen op **Opstartproject** en vervolgens op de knop **Meerdere opstartprojecten**. Controleer of **EchoService** bovenaan staat in de lijst.
4. Stel het vak **Actie** van de projecten **EchoService** en **EchoClient** in op **Start**.

    ![][5]
5. Klik op **Projectafhankelijkheden**. Selecteer in het vak **Projecten** de optie **EchoClient**. Zorg dat in het vak **Is afhankelijk van** **EchoService** is geselecteerd.

    ![][6]
6. Klik op **OK** om het dialoogvenster **Eigenschappen** te sluiten.
7. Druk op **F5** om beide projecten uit te voeren.
8. Beide consolevensters worden geopend en u wordt gevraagd de naam van de naamruimte op te geven. De service moet eerst worden uitgevoerd. Voer daarom in het **EchoService**-consolevenster de naam van de naamruimte in en druk vervolgens op **Enter**.
9. U wordt vervolgens nogmaals om uw SAS-sleutel gevraagd. Voer de SAS-sleutel in en druk op ENTER.

    Hier volgt een voorbeeld van de uitvoer van het consolevenster. De waarden die hier worden weergegeven, dienen slechts als voorbeeld.

    `Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`

    De servicetoepassing geeft in het consolevenster het adres weer dat wordt gebruikt, zoals in het volgende voorbeeld te zien is.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] to exit`
10. Voer in het consolevenster **EchoClient** dezelfde gegevens in als de gegevens die u eerder al hebt ingevoerd voor de servicetoepassing. Volg de vorige stappen om dezelfde servicenaamruimte en SAS-sleutel in te voeren voor de clienttoepassing.
11. Wanneer u deze waarden hebt ingevoerd, opent de client een kanaal naar de service en wordt u gevraagd om tekst in te voeren, zoals te zien is in het volgende console-uitvoervoorbeeld.

    `Enter text to echo (or [Enter] to exit):`

    Voer tekst in om naar de servicetoepassing te verzenden en druk op Enter. Deze tekst wordt naar de service verzonden via een Echo-servicebewerking en wordt in het serviceconsolevenster weergegeven, zoals in de volgende voorbeelduitvoer te zien is.

    `Echoing: My sample text`

    De clienttoepassing ontvangt de geretourneerde waarde van de `Echo`-bewerking: de oorspronkelijke tekst. Deze wordt vervolgens weergegeven in het consolevenster. Hier volgt een voorbeeld van de uitvoer van het clientconsolevenster.

    `Server echoed: My sample text`
12. U kunt op deze manier doorgaan met het verzenden van berichten van de client naar de service. Wanneer u klaar bent, drukt u op Enter in de client- en serviceconsolevensters om beide toepassingen te sluiten.

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie hebt u geleerd hoe u een Azure-Relay-clienttoepassing en -service met behulp van de WCF-Relay-mogelijkheden van Service Bus. Voor een vergelijkbare zelfstudie waarin [Service Bus-berichtenservice](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), Zie [aan de slag met Service Bus-wachtrijen](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

Zie de volgende onderwerpen voor meer informatie over Azure Relay.

* [Overzicht van Azure Service Bus-architectuur](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [Overzicht van Azure Relay](relay-what-is-it.md)
* [De relay WCF-service gebruiken met .NET](relay-wcf-dotnet-get-started.md)

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png

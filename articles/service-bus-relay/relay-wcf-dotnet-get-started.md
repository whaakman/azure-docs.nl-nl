---
title: Aan de slag met Azure Relay WCF doorstuurt in .NET | Microsoft Docs
description: Informatie over hoe u met Azure Relay WCF relays verbinding maken met twee toepassingen die worden gehost op verschillende locaties.
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5493281a-c2e5-49f2-87ee-9d3ffb782c75
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/20/2017
ms.author: sethm
ms.openlocfilehash: face684190456fbf4b78a84ac3afe7a4ead8995a
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-use-azure-relay-wcf-relays-with-net"></a>Het gebruik van Azure Relay WCF doorstuurt met .NET
In dit artikel wordt beschreven hoe u de Azure-Relay-service. De voorbeelden zijn geschreven in C# en maken gebruik van de WCF-API (Windows Communication Foundation) met extensies die deel uitmaken van de Service Bus-assembly. Zie voor meer informatie over Azure relay, de [Azure Relay overzicht](relay-what-is-it.md).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a>Wat is de Relay WCF?

De Azure [ *WCF Relay* ](relay-what-is-it.md) service kunt u hybride toepassingen opbouwen die worden uitgevoerd in een Azure-datacenter en uw eigen on-premises bedrijfsomgeving. De relay-service dit is mogelijk doordat u veilig kunt blootstellen Windows Communication Foundation (WCF)-services die zich in een bedrijfsnetwerk naar de openbare cloud bevinden zonder dat een firewallverbinding openen of een vereiste hoog wijzigingen in de infrastructuur van een bedrijfsnetwerk.

![WCF Relay-concepten](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Azure Relay kunt u de WCF-hostservices in uw bestaande bedrijfsomgeving. U kunt overdragen luisteren naar binnenkomende sessies en aanvragen voor deze WCF-services naar de relay-service actief is in Azure. Hierdoor kunt u deze services beschikbaar stellen voor toepassingscode die wordt uitgevoerd in Azure of voor mobiele werknemers of extranetpartneromgevingen. Relay kunt u veilig bepalen wie toegang deze services heel nauwkeurig tot. Service Bus biedt een krachtige en veilige manier om toepassingsfuncties en -gegevens van uw bestaande bedrijfsoplossingen beschikbaar te maken en hiervan te profiteren vanuit de cloud.

In dit artikel wordt beschreven hoe Azure Relay gebruiken voor het maken van een WCF-webservice weergegeven met behulp van een TCP-kanaal binding, waarmee een beveiligde communicatie tussen twee partijen wordt geïmplementeerd.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Het Service Bus NuGet-pakket ophalen
Het [Service Bus NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is de eenvoudigste manier om de Service Bus-API op te halen en om de toepassing te configureren met alle Service Bus-afhankelijkheden. Ga als volgt te werk om het NuGet-pakket in uw project te installeren:

1. Klik in Solution Explorer met de rechtermuisknop op **Verwijzingen** en klik vervolgens op **NuGet-pakketten beheren**.
2. Zoek ‘Service Bus’ en selecteer het item **Microsoft Azure Service Bus**. Klik op **Installeren** om de installatie te voltooien en sluit het volgende dialoogvenster:
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a>Weergeven en gebruiken van een SOAP-webservice met TCP
Als u een bestaande WCF SOAP-webservice voor extern verbruik beschikbaar wilt maken, moet u wijzigingen aanbrengen aan de servicebindingen en -adressen. Hiervoor zijn mogelijk wijzigingen aan uw configuratiebestand of codewijzigingen nodig, afhankelijk van hoe u de WCF-services hebt ingesteld en geconfigureerd. Houd er rekening mee dat WCF u meerdere netwerkeindpunten via dezelfde service hebben kunt, zodat u de bestaande interne eindpunten behouden kunt bij het toevoegen van de relay-eindpunten voor externe toegang op hetzelfde moment.

In deze taak bouwen van een eenvoudige WCF-service en een relay-listener aan toe te voegen. Bij deze oefening wordt ervan uitgegaan dat u enigszins bekend bent met Visual Studio. Daarom wordt het maken van een project niet in detail behandeld. In plaats daarvan ligt de nadruk op de code.

Voordat u met deze stappen begint, voert u de volgende procedure uit om uw omgeving in te stellen:

1. Maak in Visual Studio een consoletoepassing die twee projecten, Client en Service, binnen de oplossing bevat.
2. De Service Bus NuGet-pakket aan beide projecten toevoegen. Met dit pakket voegt u alle benodigde assemblyverwijzingen aan uw projecten toe.

### <a name="how-to-create-the-service"></a>De service maken
Maak eerst de service zelf. Een WCF-service bestaat uit ten minste drie onderdelen:

* Definitie van een contract waarmee wordt beschreven welke berichten worden uitgewisseld en welke bewerkingen moeten worden aangeroepen.
* Implementatie van deze overeenkomst.
* Host voor de WCF-service waarop een aantal eindpunten beschikbaar wordt gemaakt.

De codevoorbeelden in deze sectie hebben betrekking op elk van deze onderdelen.

Met het contract wordt één bewerking, `AddNumbers`, gedefinieerd waarmee twee getallen worden opgeteld en het resultaat wordt geretourneerd. De client kan met de `IProblemSolverChannel`-interface eenvoudig de levensduur van de proxy beheren. Het wordt aanbevolen een dergelijke interface te maken. U kunt deze contractdefinitie het beste in een afzonderlijk bestand plaatsen zodat u naar dat bestand kunt verwijzen vanuit zowel het project Client als het project Service, maar u kunt de code ook naar beide projecten kopiëren.

```csharp
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Met het contract is de implementatie als volgt uit:

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Een servicehost via een programma configureren
Met het geïmplementeerde contract kunt u nu de service hosten. Hosting vindt plaats in een [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx)-object waarmee exemplaren van de service worden beheerd en dat als host fungeert voor de eindpunten die naar berichten luisteren. De volgende code configureert de service met zowel een regulier lokaal eindpunt als een relay-eindpunt ter illustratie van de weergave, naast elkaar van interne en externe eindpunten. Vervang de tekenreeks *namespace* door de naamruimtenaam en *yourKey* door de SAS-sleutel die u in de vorige instellingsstap hebt verkregen.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

In het voorbeeld maakt u twee eindpunten die deel uitmaken van dezelfde contractimplementatie. Een lokale is en een eindpunt is geprojecteerd via Azure Relay. De belangrijkste verschillen tussen deze twee zijn de bindingen; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) voor het lokale eindpunt en [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) voor de relay-eindpunt en de adressen. Het lokale eindpunt heeft een lokaal netwerkadres met een afzonderlijke poort. De relay-eindpunt heeft een eindpuntadres dat bestaat uit de tekenreeks `sb`, uw naamruimtenaam en het pad 'Solver '.' Dit resulteert in de URI `sb://[serviceNamespace].servicebus.windows.net/solver`, het identificeren van het service-eindpunt als een Service Bus (relais) TCP-eindpunt met een volledig gekwalificeerde externe DNS-naam. Als u de code in de `Main`-functie van de **Service**-toepassing plaatst en de tijdelijke aanduidingen vervangt, hebt u een functionele service. Als u wilt dat uw service uitsluitend op de relay luisteren, verwijdert u de declaratie van het lokale eindpunt.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Een servicehost configureren in het bestand App.config
U kunt de host ook configureren met het bestand App.config. De servicehostingcode in dit geval wordt weergegeven in het volgende voorbeeld.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

De eindpuntdefinities worden verplaatst naar het bestand App.config. Het NuGet-pakket heeft al een reeks definities aan het App.config-bestand, dat de vereiste configuratie-extensies voor Azure Relay zijn toegevoegd. Het volgende voorbeeld, dat het exacte equivalent van de vorige code is, moet direct onder het **system.serviceModel**-element worden weergegeven. In dit voorbeeld wordt ervan uitgegaan dat uw project C#-naamruimte de naam **Service** heeft.
Vervang de tijdelijke aanduidingen door de naam van de relay-naamruimte en SAS-sleutel.

```xml
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://<namespaceName>.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Nadat u deze wijzigingen hebt aangebracht, start de service als voorheen maar met twee live eindpunten: een lokaal eindpunt en een eindpunt waarmee in de cloud wordt geluisterd.

### <a name="create-the-client"></a>De client maken
#### <a name="configure-a-client-programmatically"></a>Een client via een programma configureren
U kunt voor het verbruik van de service een WCF-client maken met een [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx)-object. Service Bus maakt gebruik van een beveiligingsmodel op basis van tokens dat wordt geïmplementeerd met SAS. De klasse [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) vertegenwoordigt een beveiligingstokenprovider met ingebouwde factorymethoden waarmee een aantal bekende tokenproviders wordt geretourneerd. In het volgende voorbeeld wordt de methode [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) gebruikt voor het verwerken van het ophalen van het juiste SAS-token. De naam en de sleutel zijn verkregen via de portal, zoals beschreven in de vorige sectie.

Verwijs eerst naar de `IProblemSolver`-contractcode van de service in uw clientproject of kopieer deze.

Vervang vervolgens de code in de `Main` methode van de client, Vervang ook de tijdelijke aanduiding voor tekst met de relay-naamruimte en SAS-sleutel.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "<namespaceName>", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

U kunt de client en de service nu opbouwen en uitvoeren (voer de service eerst uit). De client roept de service aan en drukt **9** af. U kunt de client en server op verschillende computers uitvoeren, zelfs in netwerken. De communicatie zal dan nog steeds goed verlopen. De clientcode kan in de cloud of lokaal worden uitgevoerd.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Een client configureren in het bestand App.config
De volgende code laat zien hoe u de client met het bestand App.config configureert.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

De eindpuntdefinities worden verplaatst naar het bestand App.config. Het volgende voorbeeld hetzelfde als de code eerder is vermeld is, moet worden weergegeven direct onder de `<system.serviceModel>` element. Hier, als voorheen, moet u vervangen de tijdelijke aanduidingen door uw relay-naamruimte en SAS-sleutel.

```xml
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://<namespaceName>.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van Azure Relay hebt geleerd, volgt u deze koppelingen voor meer informatie.

* [Wat is Azure Relay?](relay-what-is-it.md)
* [Overzicht van Azure Service Bus-architectuur](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* Download Service Bus-voorbeelden van [Azure-voorbeelden] [ Azure samples] of Raadpleeg de [overzicht van Service Bus-voorbeelden][overview of Service Bus samples].

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md

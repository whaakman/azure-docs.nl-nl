---
title: Azure Service Fabric programmatische schalen | Microsoft Docs
description: Schalen van een Azure Service Fabric-cluster in- of programmatisch volgens aangepaste triggers
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2017
ms.author: mikerou
ms.openlocfilehash: 1744e3c49ac06abe9e1067d507fd56d694201ffc
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Een Service Fabric-cluster via een programma schalen 

De grondbeginselen van Service Fabric-cluster schalen in Azure worden behandeld in de documentatie op [clusterschaling](./service-fabric-cluster-scale-up-down.md). Dit artikel bevat informatie over hoe Service Fabric-clusters zijn gebaseerd op de virtuele-machineschaalsets en kunnen worden geschaald, hetzij handmatig, hetzij met regels voor automatisch schalen. Dit document wordt bekeken programmatische van coördinerende Azure vergroten/verkleinen bewerkingen meer geavanceerde scenario's. 

## <a name="reasons-for-programmatic-scaling"></a>Redenen voor het schalen van programmatische
In veel scenario's vindt u goed oplossingen handmatig of via regels voor automatisch schalen. In andere scenario's, maar ze mogelijk niet de juiste keuze te maken. Bij deze methoden zijn:

- Handmatig schalen, moet u zich aanmelden en aanvragen vergroten/verkleinen bewerkingen. Als vergroten/verkleinen bewerkingen vaak vereist of onvoorspelbare tijde zijn, is deze benadering mogelijk geen een goede oplossing.
- Wanneer automatisch schalen regels een exemplaar van een virtuele-machineschaalset verwijdert, verwijder ze automatisch niet kennis van dat knooppunt uit de gekoppelde Service Fabric-cluster tenzij het knooppunttype een niveau duurzaamheid van zilver of goud heeft. Omdat automatisch schalen regels werken met de schaal instellen (in plaats van op het niveau van Service Fabric), kunnen regels voor automatisch schalen Service Fabric-knooppunten verwijderen zonder afgesloten ze. Het verwijderen van ruwe knooppunt laat 'ghost' status van Service Fabric-knooppunt achter na scale-in-bewerkingen. Een persoon (of een service) moet periodiek opschonen van de status van het verwijderde knooppunt in het Service Fabric-cluster.
  - Verwijderde knooppunten een knooppunttype met een serviceniveau duurzaamheid goud of zilver automatisch opgeruimd zodat er geen aanvullende opschoning nodig is.
- Er is wel [veel metrische gegevens](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md) wordt ondersteund door regels voor automatisch schalen, is nog steeds een beperkte set. Als uw scenario aanroept voor schalen op basis van bepaalde metriek niet behandeld in deze groep, klikt u vervolgens regels voor automatisch schalen niet mogelijk een goede optie.

Op basis van deze beperkingen, mogelijk wilt u meer aangepaste automatisch vergroten/verkleinen modellen implementeren. 

## <a name="scaling-apis"></a>Schalen van API 's
Azure-API's bestaan waarmee toepassingen programmatisch werken met virtuele machine sets schalen en Service Fabric-clusters. Als de bestaande opties voor automatisch schalen voor uw scenario niet werken, maken deze API's het mogelijk om aangepaste vergroten/verkleinen logica implementeren. 

Een aanpak voor de implementatie van deze functionaliteit automatisch schalen 'home-made' is een nieuwe staatloze service toevoegen aan de Service Fabric-toepassing voor vergroten/verkleinen bewerkingen beheren. In de service `RunAsync` methode, een verzameling van triggers kan bepalen of schalen vereist is (inclusief parameters zoals maximale clustergrootte controleren en cooldowns schalen).   

De API gebruikt voor de virtuele machine scale set interacties (zowel het huidige aantal exemplaren van virtuele machines controleren en wijzigen) is de [beheersen Azure Management Compute-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). De beheersen compute-bibliotheek biedt een API eenvoudig te gebruiken voor interactie met de virtuele-machineschaalsets.

Gebruiken om te kunnen communiceren met het Service Fabric-cluster zelf, [System.Fabric.FabricClient](/dotnet/api/system.fabric.fabricclient).

Natuurlijk hoeft de code vergroten/verkleinen niet uitgevoerd als een service in het cluster worden geschaald. Beide `IAzure` en `FabricClient` verbinding kunnen maken met de bijbehorende Azure-resources op afstand, zodat de service vergroten/verkleinen eenvoudig worden kan een consoletoepassing of de Windows-service uitvoert vanuit buiten de Service Fabric-toepassing. 

## <a name="credential-management"></a>Beheer van referenties
Eén uitdaging van het schrijven van een service voor het afhandelen van schalen, is de service moet toegang kunnen krijgen tot bronnen voor virtuele machines scale set zonder een interactieve aanmelding. Toegang tot de Service Fabric-cluster is eenvoudig als het vergroten/verkleinen service aan in een eigen Service Fabric-toepassing wijzigingen brengt, maar de referenties zijn nodig voor toegang tot de schaalaanpassingsset. Als u wilt aanmelden, kunt u een [service-principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) gemaakt met de [Azure CLI 2.0](https://github.com/azure/azure-cli).

Een service-principal kan worden gemaakt met de volgende stappen:

1. Aanmelden bij de Azure CLI (`az login`) als een gebruiker met toegang tot de schaal van de virtuele machine instellen
2. De service-principal met maken`az ad sp create-for-rbac`
    1. Noteer de appId (client-ID elders genoemd), naam, wachtwoord en tenant voor later gebruik.
    2. U moet ook uw abonnements-ID kan worden bekeken met`az account list`

De bibliotheek beheersen compute kunt aanmelden met deze referenties als volgt (Houd er rekening mee dat core beheersen Azure typen, zoals `IAzure` zijn in de [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) pakket):

```csharp
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed to login to Azure");
}
```

Nadat u bent aangemeld, scale set-exemplaren kan worden opgevraagd `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.

## <a name="scaling-out"></a>Uitschalen
Met behulp van de beheersen Azure compute SDK, exemplaren kunnen worden toegevoegd aan de virtuele-machineschaalset ingesteld met een paar aanroepen-

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

U kunt ook kan de grootte van virtuele machine scale set ook worden beheerd met PowerShell-cmdlets. [`Get-AzureRmVmss`](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss)kan de virtuele machine scale set object ophalen. De huidige capaciteit is beschikbaar via de `.sku.capacity` eigenschap. Nadat u de capaciteit op de gewenste waarde, kan de virtuele-machineschaalset instellen in Azure worden bijgewerkt met de [ `Update-AzureRmVmss` ](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) opdracht.

Als bij het handmatig toevoegen van een knooppunt toevoegen van een schaalset exemplaar zijn bevat alles wat nodig is om te beginnen een nieuw Service Fabric-knooppunt omdat de sjabloon-schaalset extensies automatisch nieuwe exemplaren te koppelen aan de Service Fabric-cluster. 

## <a name="scaling-in"></a>Schalen in

Schalen is vergelijkbaar met het uitbreiden. De werkelijke virtuele-machineschaalset ingesteld wijzigingen zijn vrijwel hetzelfde. Maar zoals eerder is besproken, Service Fabric alleen automatisch opgeruimd verwijderde knooppunten met een duurzaamheid van goud of zilver. Dus in het Brons duurzaamheid schaal in geval is, is het nodig om te communiceren met de Service Fabric-cluster afsluiten van het knooppunt worden verwijderd en vervolgens verwijdert u de status.

Het knooppunt voorbereiden voor het afsluiten omvat het zoeken naar het knooppunt worden verwijderd (het meest recent toegevoegde knooppunt) en het deactiveren. Voor niet-seed-knooppunten nieuwere knooppunten kunnen worden gevonden door te vergelijken `NodeInstanceId`. 

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n => n.NodeInstanceId)
        .FirstOrDefault();
```

Seed-knooppunten zijn verschillend en niet per se volgt u de overeenkomst groter exemplaar-id's op de eerst worden verwijderd.

Als het knooppunt worden verwijderd, wordt gevonden, deze kunnen worden gedeactiveerd en verwijderd met behulp van dezelfde `FabricClient` exemplaar en de `IAzure` exemplaar van eerder.

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Als met het uitbreiden van de PowerShell-cmdlets voor het wijzigen van de virtuele-machineschaalset kan set capaciteit ook hier worden gebruikt als een scripting benadering de voorkeur verdient. Zodra de instantie van de virtuele machine is verwijderd, kan de status van Service Fabric-knooppunt kan worden verwijderd.

```csharp
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="potential-drawbacks"></a>Nadelen

Zoals wordt beschreven in de voorgaande codefragmenten, biedt maken van uw eigen schalen service dat de grootste mate van controle en aanpassingsmogelijkheden via uw toepassing de gedrag schalen. Dit kan handig zijn voor scenario's waarin u nauwkeurige controle over hoe of wanneer een toepassing wordt geschaald in- of nodig zijn. Dit besturingselement is voorzien van een afweging van complexiteit bij de code. Met deze benadering, betekent dat u moet naar eigen vergroten/verkleinen code, die niet eenvoudig is.

Hoe moet u benadert Service Fabric schalen, is afhankelijk van uw scenario. Als schalen ongewoon is, is de mogelijkheid toevoegen aan of het handmatig verwijderen van knooppunten is waarschijnlijk voldoende. Voor complexere scenario's, regels voor automatisch schalen en de mogelijkheid te schalen programmatisch blootstellen SDK's bieden krachtige alternatieven.

## <a name="next-steps"></a>Volgende stappen

Om te beginnen uitvoering van uw eigen logica automatisch schalen, tijd om uzelf bekend met de volgende concepten en nuttig API's:

- [Handmatig of automatisch schalen regels schalen](./service-fabric-cluster-scale-up-down.md)
- [Beheersen Azure Management-bibliotheken voor .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (dit is nuttig voor interactie met een Service Fabric-cluster onderliggende virtuele-machineschaalsets)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (dit is nuttig voor interactie met een Service Fabric-cluster en de knooppunten ervan)

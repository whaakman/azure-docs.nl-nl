---
title: Azure Service Fabric programmatische schalen | Microsoft Docs
description: Schalen van een Azure Service Fabric-cluster in- of programmatisch volgens aangepaste triggers
services: service-fabric
documentationcenter: .net
author: mjrousos
manager: jonjung
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2018
ms.author: mikerou
ms.openlocfilehash: dcf4721012fb8ec39bcd1de02c294747357b3539
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Een Service Fabric-cluster via een programma schalen 

Service Fabric-clusters worden uitgevoerd in Azure zijn gebouwd op de virtuele-machineschaalsets.  [Met clusterschaling](./service-fabric-cluster-scale-up-down.md) beschrijft hoe Service Fabric-clusters kunnen worden geschaald handmatig of met regels voor automatisch schalen. In dit artikel wordt beschreven hoe referenties beheren en schalen van een cluster in of uit met behulp van de beheersen Azure compute-SDK, die een meer geavanceerde scenario. Lees voor een overzicht [programmatische Azure schalen operations coördinatie van](service-fabric-cluster-scaling.md#programmatic-scaling). 

## <a name="manage-credentials"></a>Referenties beheren
Eén uitdaging van het schrijven van een service voor het afhandelen van schalen, is de service moet toegang kunnen krijgen tot bronnen voor virtuele machines scale set zonder een interactieve aanmelding. Toegang tot de Service Fabric-cluster is eenvoudig als het vergroten/verkleinen service aan in een eigen Service Fabric-toepassing wijzigingen brengt, maar de referenties zijn nodig voor toegang tot de schaalaanpassingsset. Als u wilt aanmelden, kunt u een [service-principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) gemaakt met de [Azure CLI 2.0](https://github.com/azure/azure-cli).

Een service-principal kan worden gemaakt met de volgende stappen:

1. Aanmelden bij de Azure CLI (`az login`) als een gebruiker met toegang tot de schaal van de virtuele machine instellen
2. De service-principal met maken `az ad sp create-for-rbac`
    1. Noteer de appId (client-ID elders genoemd), naam, wachtwoord en tenant voor later gebruik.
    2. U moet ook uw abonnements-ID kan worden bekeken met `az account list`

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

U kunt ook kan de grootte van virtuele machine scale set ook worden beheerd met PowerShell-cmdlets. [`Get-AzureRmVmss`](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) kan de virtuele machine scale set object ophalen. De huidige capaciteit is beschikbaar via de `.sku.capacity` eigenschap. Nadat u de capaciteit op de gewenste waarde, kan de virtuele-machineschaalset instellen in Azure worden bijgewerkt met de [ `Update-AzureRmVmss` ](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) opdracht.

Als bij het handmatig toevoegen van een knooppunt toevoegen van een schaalset exemplaar zijn bevat alles wat nodig is om te beginnen een nieuw Service Fabric-knooppunt omdat de sjabloon-schaalset extensies automatisch nieuwe exemplaren te koppelen aan de Service Fabric-cluster. 

## <a name="scaling-in"></a>Schalen in

Schalen is vergelijkbaar met het uitbreiden. De werkelijke virtuele-machineschaalset ingesteld wijzigingen zijn vrijwel hetzelfde. Maar zoals eerder is besproken, Service Fabric alleen automatisch opgeruimd verwijderde knooppunten met een duurzaamheid van goud of zilver. Dus in het Brons duurzaamheid schaal in geval is, is het nodig om te communiceren met de Service Fabric-cluster afsluiten van het knooppunt worden verwijderd en vervolgens verwijdert u de status.

Het knooppunt voorbereiden voor afsluiten omvat het zoeken naar het knooppunt dat moet worden verwijderd (de laatst toegevoegde virtuele machine scale set exemplaar) en het deactiveren. VM-scale set instanties worden genummerd in de volgorde waarin die ze worden toegevoegd, zodat nieuwere knooppunten u vinden kunnen door te vergelijken met het achtervoegsel in namen van de knooppunten (welke overeen met de onderliggende virtuele-machineschaalset ingesteld exemplaarnamen). 

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

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

## <a name="next-steps"></a>Volgende stappen

Om te beginnen uitvoering van uw eigen logica automatisch schalen, tijd om uzelf bekend met de volgende concepten en nuttig API's:

- [Handmatig of automatisch schalen regels schalen](./service-fabric-cluster-scale-up-down.md)
- [Beheersen Azure Management-bibliotheken voor .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (dit is nuttig voor interactie met een Service Fabric-cluster onderliggende virtuele-machineschaalsets)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (dit is nuttig voor interactie met een Service Fabric-cluster en de knooppunten ervan)

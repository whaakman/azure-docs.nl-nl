---
title: Azure Service Fabric programmatische schalen | Microsoft Docs
description: Een Azure Service Fabric-cluster in- of uitschalen via een programma, op basis van aangepaste triggers
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
ms.openlocfilehash: ff02f79321823e42c25897e9de30dfbb6fac46b0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949607"
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Een Service Fabric-cluster programmatisch schalen 

Service Fabric-clusters die worden uitgevoerd in Azure zijn gebouwd boven op de virtuele-machineschaalsets.  [Clusterschaling](./service-fabric-cluster-scale-up-down.md) beschrijft hoe Service Fabric-clusters kunnen worden geschaald handmatig of met regels voor automatisch schalen. In dit artikel wordt beschreven hoe u referenties beheren en schalen van een cluster in of uit met de fluent Azure compute-SDK, die een meer geavanceerde scenario. Lees voor een overzicht [programmatische methoden voor het coördineren van Azure schalen herverdelen](service-fabric-cluster-scaling.md#programmatic-scaling). 

## <a name="manage-credentials"></a>Referenties beheren
Eén uitdaging van het schrijven van een service voor het afhandelen van schalen is dat de service moet toegang hebben tot de virtuele machine schaalsetresources zonder een interactieve aanmelding. Het is eenvoudig om toegang tot de Service Fabric-cluster als de service vergroten/verkleinen is een eigen Service Fabric-toepassing wijzigen, maar de referenties zijn nodig voor toegang tot de schaalset. Om aan te melden, kunt u een [service-principal](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) die zijn gemaakt met de [Azure CLI](https://github.com/azure/azure-cli).

Een service-principal kan worden gemaakt met de volgende stappen uit:

1. Meld u aan bij de Azure CLI (`az login`) ingesteld als een gebruiker met toegang tot de virtuele-machineschaalset
2. De service-principal met maken `az ad sp create-for-rbac`
    1. Noteer de toepassings-id (elders 'client-ID' genoemd), naam, wachtwoord en tenant voor later gebruik.
    2. U moet ook uw abonnements-ID, dat kan worden weergegeven met `az account list`

De fluent compute-bibliotheek kan zich aanmelden met deze referenties als volgt (Houd er rekening mee dat core fluent Azure typen, zoals `IAzure` zijn de [Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) pakket):

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

Wanneer u bent aangemeld, scale set-exemplaren kan worden opgevraagd `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`.

## <a name="scaling-out"></a>Uitschalen
Met de fluent Azure compute-SDK, exemplaren kunnen worden toegevoegd aan de virtuele-machineschaalset met slechts een paar aanroepen:

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

U kunt ook kan de grootte van virtuele machine scale set ook worden beheerd met PowerShell-cmdlets. [`Get-AzureRmVmss`](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) de virtuele machine schaalsetobject kunnen worden opgehaald. De huidige capaciteit is beschikbaar via de `.sku.capacity` eigenschap. Na het wijzigen van de capaciteit op de gewenste waarde, de virtuele-machineschaalset in Azure kan worden bijgewerkt met de [ `Update-AzureRmVmss` ](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) opdracht.

Wanneer een knooppunt handmatig toe te voegen, toe te voegen een schaalsetinstantie moet worden bevat alles wat nodig is om te beginnen een nieuw Service Fabric-knooppunt omdat de sjabloon voor schaalsets uitbreidingen automatisch nieuwe exemplaren toevoegen aan de Service Fabric-cluster. 

## <a name="scaling-in"></a>Inschalen

Inschalen is vergelijkbaar met het uitschalen. De werkelijke virtuele-machineschaalset wijzigingen zijn vrijwel hetzelfde. Maar zoals eerder is besproken, Service Fabric alleen automatisch opschonen van verwijderde knooppunten met de Gold- of Silver duurzaamheid. In het geval Brons duurzaamheid schaal het is dus nodig om te communiceren met de Service Fabric-cluster om het knooppunt moet worden verwijderd af te sluiten en vervolgens te verwijderen van de status.

Het knooppunt voorbereiden voor afsluiten omvat het zoeken naar het knooppunt dat moet worden verwijderd (meest recent toegevoegde virtuele machine exemplaar in de schaalset) en het deactiveren. Virtual machine scale set exemplaren worden genummerd in de volgorde waarin die ze zijn toegevoegd, zodat nieuwere knooppunten vindt u door het vergelijken van het achtervoegsel in namen van de knooppunten (welke overeen met de onderliggende virtuele-machineschaalset instantienamen). 

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

Zodra het knooppunt moet worden verwijderd, wordt gevonden, deze kan worden gedeactiveerd en worden verwijderd met behulp van dezelfde `FabricClient` exemplaar en de `IAzure` -exemplaar van de eerder.

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

Als met wilt uitschalen, PowerShell-cmdlets voor het wijzigen van virtuele-machineschaalset kan capaciteit ook hier worden gebruikt als een scripting aanpak is het beter. Wanneer het exemplaar van de virtuele machine is verwijderd, kan de status van Service Fabric-knooppunt kan worden verwijderd.

```csharp
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="next-steps"></a>Volgende stappen

Als u wilt gaan implementeren van uw eigen logica automatisch schalen, kunt u dat raken met de volgende concepten en handige API's:

- [Schaal handmatig of met regels voor automatisch schalen](./service-fabric-cluster-scale-up-down.md)
- [Fluent Azure-beheerbibliotheken voor .NET](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (dit is handig voor interactie met een Service Fabric-cluster onderliggende virtual machine scale sets)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (dit is handig voor interactie met Service Fabric-cluster en de knooppunten ervan)

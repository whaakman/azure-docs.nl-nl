---
title: Fouten simuleren in azure Service Fabric-apps | Microsoft Docs
description: Hoe u uw services kunt beveiligen tegen probleemloze en niet-uitgestelde fouten.
services: service-fabric
documentationcenter: .net
author: anmolah
manager: chackdan
editor: ''
ms.assetid: 44af01f0-ed73-4c31-8ac0-d9d65b4ad2d6
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: bbb89b66231c949627c7ffbf99ebe9b5dd379ca2
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348719"
---
# <a name="simulate-failures-during-service-workloads"></a>Storingen simuleren tijdens servicewerkbelastingen
De test scenario's in azure Service Fabric ontwikkel aars in staat stellen geen zorgen te maken over afzonderlijke storingen. Er zijn echter scenario's, waarbij een expliciete interleaving van de werk belasting en storingen van de client mogelijk nodig is. De interleaving van de werk belasting en fouten van de client zorgt ervoor dat de service daad werkelijk een actie uitvoert wanneer er een fout optreedt. Op basis van het niveau van de controle die de test baarheid biedt, kunnen deze nauw keurige punten van de werk belasting worden uitgevoerd. Deze inductie van fouten in verschillende statussen in de toepassing kan fouten opsporen en de kwaliteit verbeteren.

## <a name="sample-custom-scenario"></a>Voor beeld van aangepast scenario
In deze test wordt een scenario weer gegeven dat de zakelijke workload interleaveert met gepaste en niet- [respijt fouten](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions). De fouten moeten in het midden van de service bewerkingen worden veroorzaakt of worden berekend voor de beste resultaten.

Laten we een voor beeld bekijken van een service die vier workloads beschikbaar maakt: A, B, C en D. Elk komt overeen met een set werk stromen en kan reken-, opslag-of mix-bewerkingen zijn. In het geval van eenvoud zullen we de workloads in ons voor beeld samen stellen. De verschillende fouten die in dit voor beeld worden uitgevoerd, zijn:

* RestartNode: Er is een fout opgetreden bij het opnieuw opstarten van een computer.
* RestartDeployedCodePackage: Er is een niet-verwerkte fout opgetreden tijdens het simuleren van het service-hostproces
* RemoveReplica: Fout tijdens het simuleren van het verwijderen van de replica.
* MovePrimary Verkeerde fout bij het simuleren van replica verplaatsingen die zijn geactiveerd door de Service Fabric load balancer.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.TestManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.FaultManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.FaultManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.FaultManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.FaultManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```

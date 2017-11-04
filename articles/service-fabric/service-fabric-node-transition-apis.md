---
title: Starten en stoppen van clusterknooppunten voor het testen van Azure microservices | Microsoft Docs
description: Informatie over hoe u met fout met betrekking tot het invoegen van een Service Fabric-toepassing testen door starten en stoppen van de clusterknooppunten.
services: service-fabric
documentationcenter: .net
author: LMWF
manager: rsinha
editor: 
ms.assetid: f4e70f6f-cad9-4a3e-9655-009b4db09c6d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/12/2017
ms.author: lemai
ms.openlocfilehash: 850fbc0c74811ec942292da64064dec867cd1b9e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="replacing-the-start-node-and-stop-node-apis-with-the-node-transition-api"></a>Het starten en stoppen knooppunten API's vervangen door de API van de overgang knooppunt

## <a name="what-do-the-stop-node-and-start-node-apis-do"></a>Wat het knooppunt stoppen en starten knooppunt API's?

De API knooppunt stoppen (beheerd: [StopNodeAsync()][stopnode], PowerShell: [Stop ServiceFabricNode][stopnodeps]) stopt van een Service Fabric-knooppunt.  Een Service Fabric-knooppunt is proces, niet een VM of machine – de virtuele machine of de machine wordt nog steeds worden uitgevoerd.  Voor de rest van het document 'knooppunt' betekent dat deze Service Fabric-knooppunt.  Stoppen van een knooppunt, plaatst het in een *gestopt* staat waarin deze is geen lid van het cluster en kan geen host zijn services, dus simuleren een *omlaag* knooppunt.  Dit is handig voor het injecteren van fouten in het systeem de toepassing te testen.  De API knooppunt starten (beheerd: [StartNodeAsync()][startnode], PowerShell: [Start ServiceFabricNode][startnodeps]]) keert de API knooppunt stoppen  Dit brengt het knooppunt terug naar een normale status.

## <a name="why-are-we-replacing-these"></a>Waarom zijn we deze vervangen?

Zoals eerder beschreven, een *gestopt* Service Fabric-knooppunt is een knooppunt opzettelijk gericht met behulp van de API knooppunt stoppen.  Een *omlaag* knooppunt is een knooppunt dat is niet beschikbaar voor een andere reden (bijvoorbeeld de virtuele machine of de machine is uitgeschakeld).  Met de API knooppunt stopt het systeem niet beschikbaar informatie onderscheid maken tussen *gestopt* knooppunten en *omlaag* knooppunten.

Er zijn fouten geretourneerd door deze API's zijn niet als beschrijvende zoals ze zijn.  Bijvoorbeeld, aanroepen van de API knooppunt stoppen op een al *gestopt* knooppunt wordt geretourneerd met de fout *InvalidAddress*.  Deze ervaring kan worden verbeterd.

Bovendien is de duur van die een knooppunt is gestopt voor 'oneindig' totdat het knooppunt Start API wordt aangeroepen.  We hebben dit kan problemen veroorzaken en gevoelig voor fouten kan worden gevonden.  We zag bijvoorbeeld problemen waarbij een gebruiker de API knooppunt stoppen op een knooppunt aangeroepen en vervolgens vergeten bent.  Later, was het niet duidelijk als het knooppunt is *omlaag* of *gestopt*.


## <a name="introducing-the-node-transition-apis"></a>Inleiding tot het knooppunt overgang API 's

We hebt deze problemen boven in een nieuwe reeks API's opgelost.  Het nieuwe knooppunt overgang API (beheerd: [StartNodeTransitionAsync()][snt]) kan worden gebruikt voor de overgang van een Service Fabric-knooppunt naar een *gestopt* status, of voor de overgang van een *gestopt* status naar een normale werking.  Houd er rekening mee dat de 'Start' in de naam van de API niet verwijst naar een knooppunt wordt gestart.  Het verwijst naar het begin van een asynchrone bewerking die het systeem wordt uitgevoerd om de overgang van het knooppunt op een *gestopt* of de status gestart.

**Gebruik**

De API van de overgang knooppunt heeft een uitzondering opgetreden bij het aanroepen niet genereren, vervolgens het systeem de asynchrone bewerking heeft geaccepteerd als deze wordt uitgevoerd.  Een geslaagde aanroepen betekent niet dat de bewerking nog is voltooid.  Als u informatie over de huidige status van de bewerking, roept u de API van knooppunt overgang voortgang (beheerd: [GetNodeTransitionProgressAsync()][gntp]) met de guid die wordt gebruikt bij het aanroepen van knooppunt overgang API voor deze bewerking.  Het knooppunt overgang voortgang API retourneert een object NodeTransitionProgress.  De eigenschap State van dit object geeft de huidige status van de bewerking.  De bewerking wordt uitgevoerd als de status 'actief'.  Als deze is voltooid, wordt het opnieuw zonder fouten voltooid.  Als deze fout is opgetreden, moet u er een probleem opgetreden bij het uitvoeren van de bewerking is.  De eigenschap Result uitzondering eigenschap wordt aangegeven wat het probleem is.  Zie https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate voor meer informatie over de eigenschap State en de sectie 'Voorbeeldgebruik' hieronder voor codevoorbeelden.


**Verschillen tussen een gestopte knooppunt en een knooppunt omlaag** als een knooppunt *gestopt* met de API van de overgang knooppunt, de uitvoer van een query voor knooppunt (beheerd: [GetNodeListAsync()] [ nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) wordt aangegeven dat dit knooppunt heeft een *IsStopped* de eigenschapwaarde true.  Houd er rekening mee deze verschilt van de waarde van de *NodeStatus* eigenschap dicteert *omlaag*.  Als de *NodeStatus* eigenschap een waarde heeft van *omlaag*, maar *IsStopped* ONWAAR is, en vervolgens het knooppunt is niet gestopt met de API van knooppunt overgang is en *omlaag*  vanwege een andere reden.  Als de *IsStopped* eigenschap is ingesteld op true, en de *NodeStatus* eigenschap is *omlaag*, en vervolgens deze is gestopt met de API van de overgang knooppunt.

Starten van een *gestopt* knooppunt met de API van de overgang knooppunt om te fungeren als een normale lid van het cluster opnieuw wordt geretourneerd.  De uitvoer van de query voor knooppunt API bevat *IsStopped* als onwaar, en *NodeStatus* als iets dat niet is niet beschikbaar (bijvoorbeeld omhoog).


**Beperkte duur** als u de overgang knooppunt API voor het stoppen van een knooppunt, een van de vereiste parameters *stopNodeDurationInSeconds*, de hoeveelheid tijd in seconden dat het knooppunt vertegenwoordigt *gestopt*.  Deze waarde moet in het toegestane bereik, waarvoor minimaal 600 en een maximum van 14400.  Nadat deze tijd is verstreken, het knooppunt wordt automatisch opnieuw opgestart zelf in de status van.  Raadpleeg Sample 1 hieronder voor een voorbeeld van gebruik.

> [!WARNING]
> Vermijd de combinatie van knooppunt overgang API's en de API's van het knooppunt starten en stoppen knooppunt.  De aanbeveling is het gebruik van de alleen-knooppunt overgang-API.  > Als een knooppunt is al gestopt met de API knooppunt stoppen, het moet worden gestart met behulp van het knooppunt Start API eerst voordat met behulp van de > knooppunt overgang API's.

> [!WARNING]
> Meerdere knooppunt overgang API-aanroepen kunnen niet worden doorgevoerd op hetzelfde knooppunt parallel.  In een dergelijke situatie de API van de overgang knooppunt wordt > genereert een FabricException met een eigenschapswaarde ErrorCode van NodeTransitionInProgress.  Zodra de overgang van een knooppunt op een specifiek knooppunt heeft > is gestart, moet u wachten totdat de bewerking een definitieve status (voltooid, Faulted of ForceCancelled) is bereikt voordat u begint een > nieuwe overgang op hetzelfde knooppunt.  Parallelle knooppunt overgang aanroepen op verschillende knooppunten zijn toegestaan.


#### <a name="sample-usage"></a>Voorbeeld van gebruik


**Voorbeeld 1** -het volgende voorbeeld gebruikt de API van de overgang knooppunt stoppen van een knooppunt.

```csharp
        // Helper function to get information about a node
        static Node GetNodeInfo(FabricClient fc, string node)
        {
            NodeList n = null;
            while (n == null)
            {
                n = fc.QueryManager.GetNodeListAsync(node).GetAwaiter().GetResult();
                Task.Delay(TimeSpan.FromSeconds(1)).GetAwaiter();
            };

            return n.FirstOrDefault();
        }

        static async Task WaitForStateAsync(FabricClient fc, Guid operationId, TestCommandProgressState targetState)
        {
            NodeTransitionProgress progress = null;

            do
            {
                bool exceptionObserved = false;
                try
                {
                    progress = await fc.TestManager.GetNodeTransitionProgressAsync(operationId, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                    exceptionObserved = true;
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                    exceptionObserved = true;
                }

                if (!exceptionObserved)
                {
                    Console.WriteLine("Current state of operation '{0}': {1}", operationId, progress.State);

                    if (progress.State == TestCommandProgressState.Faulted)
                    {
                        // Inspect the progress object's Result.Exception.HResult to get the error code.
                        Console.WriteLine("'{0}' failed with: {1}, HResult: {2}", operationId, progress.Result.Exception, progress.Result.Exception.HResult);

                        // ...additional logic as required
                    }

                    if (progress.State == targetState)
                    {
                        Console.WriteLine("Target state '{0}' has been reached", targetState);
                        break;
                    }
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (true);
        }

        static async Task StopNodeAsync(FabricClient fc, string nodeName, int durationInSeconds)
        {
            // Uses the GetNodeListAsync() API to get information about the target node
            Node n = GetNodeInfo(fc, nodeName);

            // Create a Guid
            Guid guid = Guid.NewGuid();

            // Create a NodeStopDescription object, which will be used as a parameter into StartNodeTransition
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, durationInSeconds);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with the NodeStopDescription from above, which will stop the target node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    // This is retryable
                }
                catch (FabricTransientException fte)
                {
                    // This is retryable
                }

                // Backoff
                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Voorbeeld 2** -het volgende voorbeeld start een *gestopt* knooppunt.  Sommige Help-methoden van het eerste voorbeeld wordt gebruikt.

```csharp
        static async Task StartNodeAsync(FabricClient fc, string nodeName)
        {
            // Uses the GetNodeListAsync() API to get information about the target node
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = n.NodeInstanceId;

            // Create a NodeStartDescription object, which will be used as a parameter into StartNodeTransition
            NodeStartDescription description = new NodeStartDescription(guid, n.NodeName, nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until hte desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Voorbeeld 3** -het volgende voorbeeld toont onjuiste informatie over het gebruik.  Dit gebruik is onjuist omdat de *stopDurationInSeconds* biedt groter is dan het toegestane bereik.  De bewerking is niet geaccepteerd omdat StartNodeTransitionAsync() met een onherstelbare fout mislukt, en de voortgang van de API moet niet worden aangeroepen.  Dit voorbeeld maakt gebruik van een aantal methoden van het eerste voorbeeld.

```csharp
        static async Task StopNodeWithOutOfRangeDurationAsync(FabricClient fc, string nodeName)
        {
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();

            // Use an out of range value for stopDurationInSeconds to demonstrate error
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, 99999);

            try
            {
                await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
            }

            catch (FabricException e)
            {
                Console.WriteLine("Caught {0}", e);
                Console.WriteLine("ErrorCode {0}", e.ErrorCode);
                // Output:
                // Caught System.Fabric.FabricException: System.Runtime.InteropServices.COMException (-2147017629)
                // StopDurationInSeconds is out of range ---> System.Runtime.InteropServices.COMException: Exception from HRESULT: 0x80071C63
                // << Parts of exception omitted>>
                //
                // ErrorCode InvalidDuration
            }
        }
```

**Voorbeeld 4** -het volgende voorbeeld toont de informatie over de fout die wordt geretourneerd van de API van knooppunt overgang uitgevoerd wanneer de bewerking gestart door de API van de overgang knooppunt wordt geaccepteerd, maar later is mislukt tijdens het uitvoeren van.  In het geval is mislukt dit omdat de API van de overgang knooppunt wordt gestart van een knooppunt dat niet bestaat.  Dit voorbeeld maakt gebruik van een aantal methoden van het eerste voorbeeld.

```csharp
        static async Task StartNodeWithNonexistentNodeAsync(FabricClient fc)
        {
            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = 12345;

            // Intentionally use a nonexistent node
            NodeStartDescription description = new NodeStartDescription(guid, "NonexistentNode", nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.  In this case, it will end up in the Faulted state since the node does not exist.
            // When StartNodeTransitionProgressAsync()'s returned progress object has a State if Faulted, inspect the progress object's Result.Exception.HResult to get the error code.
            // In this case, it will be NodeNotFound.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Faulted).ConfigureAwait(false);
        }
```

[stopnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StopNodeAsync_System_String_System_Numerics_BigInteger_System_Fabric_CompletionMode_
[stopnodeps]: https://msdn.microsoft.com/library/mt125982.aspx
[startnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN#System_Fabric_FabricClient_FaultManagementClient_StartNodeAsync_System_String_System_Numerics_BigInteger_System_String_System_Int32_System_Fabric_CompletionMode_System_Threading_CancellationToken_
[startnodeps]: https://msdn.microsoft.com/library/mt163520.aspx
[nodequery]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient#System_Fabric_FabricClient_QueryClient_GetNodeListAsync_System_String_
[nodequeryps]: https://docs.microsoft.com/powershell/servicefabric/vlatest/Get-ServiceFabricNode?redirectedfrom=msdn
[snt]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_StartNodeTransitionAsync_System_Fabric_Description_NodeTransitionDescription_System_TimeSpan_System_Threading_CancellationToken_
[gntp]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_GetNodeTransitionProgressAsync_System_Guid_System_TimeSpan_System_Threading_CancellationToken_

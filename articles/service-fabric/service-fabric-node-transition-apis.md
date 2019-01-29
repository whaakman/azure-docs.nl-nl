---
title: Starten en stoppen van clusterknooppunten voor het testen van apps in Azure Service Fabric | Microsoft Docs
description: Informatie over het gebruik foutinjectie testen van een Service Fabric-toepassing door te starten en stoppen van de clusterknooppunten.
services: service-fabric
documentationcenter: .net
author: LMWF
manager: rsinha
editor: ''
ms.assetid: f4e70f6f-cad9-4a3e-9655-009b4db09c6d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/12/2017
ms.author: lemai
ms.openlocfilehash: ff5d4267de172aa83fae6ce70a609ad9897d7374
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55102682"
---
# <a name="replacing-the-start-node-and-stop-node-apis-with-the-node-transition-api"></a>Het starten en stoppen knooppunten API's vervangen door de API van de overgang knooppunt

## <a name="what-do-the-stop-node-and-start-node-apis-do"></a>Wat het knooppunt stoppen en starten knooppunt API's?

De API-knooppunt stoppen (beheerd: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) stopt een Service Fabric-knooppunt.  Een Service Fabric-knooppunt is proces, niet een virtuele machine of computer – de virtuele machine of de machine wordt nog steeds worden uitgevoerd.  Voor de rest van het document betekent 'knooppunt' Service Fabric-knooppunt.  Stoppen van een knooppunt is ondergebracht in een *gestopt* staat waarin deze is geen lid van het cluster en services, dus simuleren kan geen host een *omlaag* knooppunt.  Dit is handig voor het injecteren van fouten in het systeem om uw toepassing te testen.  De API-knooppunt starten (beheerd: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) keert de API-knooppunt stoppen, die het knooppunt wordt teruggebracht naar een normale status.

## <a name="why-are-we-replacing-these"></a>Waarom zijn we deze vervangen?

Zoals eerder beschreven, een *gestopt* Service Fabric-knooppunt is een knooppunt opzettelijk gericht met behulp van het knooppunt stoppen API.  Een *omlaag* knooppunt is een knooppunt dat is niet beschikbaar vanwege een andere reden (bijvoorbeeld de virtuele machine of de machine is uitgeschakeld).  Met de API-knooppunt niet meer het systeem niet beschikbaar informatie onderscheid maken tussen de *gestopt* knooppunten en *omlaag* knooppunten.

Bovendien zijn sommige fouten geretourneerd door deze API's niet als beschrijvende zoals ze zijn.  Bijvoorbeeld, aanroepen van de API-knooppunt stoppen op een al *gestopt* knooppunt geretourneerd met de fout *InvalidAddress*.  Deze ervaring kan worden verbeterd.

De duur van die een knooppunt is gestopt voor is ook 'oneindige' totdat het knooppunt Start API wordt aangeroepen.  We hebben deze problemen kan veroorzaken en gevoelig voor fouten kan worden ontdekt.  We hebben bijvoorbeeld problemen waarbij een gebruiker de API-knooppunt stoppen op een knooppunt aangeroepen en vervolgens de software vergeten gezien.  Later, is het niet duidelijk als het knooppunt is *omlaag* of *gestopt*.


## <a name="introducing-the-node-transition-apis"></a>Maak kennis met het knooppunt overgang API 's

Wij hebben oog voor deze problemen boven in een nieuwe set API's.  Het nieuwe knooppunt overgang API (beheerd: [StartNodeTransitionAsync()][snt]) kan worden gebruikt voor de overgang van een Service Fabric-knooppunt naar een *gestopt* status, of voor de overgang van een *gestopt* status naar een normale werking.  Houd er rekening mee dat de 'Start' naam van de API niet verwijst naar vanaf een knooppunt.  Deze gegevensset verwijst naar het begin van een asynchrone bewerking die het systeem wordt uitgevoerd als u wilt overgaan van het knooppunt aan een *gestopt* of de status gestart.

**Gebruik**

Als de API van de overgang knooppunt een uitzondering wordt aangeroepen niet genereren heeft, klikt u vervolgens het systeem de asynchrone bewerking heeft geaccepteerd, en deze wordt uitgevoerd.  Een geslaagde aanroepen betekent niet dat de bewerking nog is voltooid.  Voor informatie over de huidige status van de bewerking, het knooppunt overgang voortgang-API aanroepen (beheerd: [GetNodeTransitionProgressAsync()][gntp]) met de guid die wordt gebruikt bij het aanroepen van knooppunt overgang API voor deze bewerking.  Het knooppunt overgang voortgang API retourneert een object NodeTransitionProgress.  Eigenschap van de status van dit object bevat de huidige status van de bewerking.  Als de status 'Running' is, wordt klikt u vervolgens de bewerking uitgevoerd.  Als deze is voltooid, wordt de bewerking is voltooid zonder fouten.  Als deze fout is opgetreden, moet u er een probleem opgetreden bij het uitvoeren van de bewerking is.  De eigenschap Result uitzondering eigenschap wordt aangegeven wat het probleem is.  Zie https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate voor meer informatie over de eigenschap State en het gedeelte 'Voorbeeld van gebruik' hieronder voor voorbeelden van code.


**Verschillen tussen een gestopte knooppunt en een knooppunt omlaag** als een knooppunt is *gestopt* met behulp van het knooppunt overgang API, de uitvoer van een knooppunt-query (beheerd: [GetNodeListAsync()][nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) wordt aangegeven dat dit knooppunt heeft een *IsStopped* waarde van de eigenschap ' True ' geretourneerd.  Houd er rekening mee dit wijkt af van de waarde van de *NodeStatus* eigenschap, dit geeft aan *omlaag*.  Als de *NodeStatus* eigenschap heeft een waarde van *omlaag*, maar *IsStopped* is ingesteld op false, en vervolgens het knooppunt is niet gestopt met de API van de overgang knooppunt en *omlaag*  vanwege een andere reden.  Als de *IsStopped* eigenschap is ingesteld op true, en de *NodeStatus* eigenschap *omlaag*, en vervolgens deze wordt stilgelegd met behulp van de API van de overgang knooppunt.

Starten van een *gestopt* knooppunt met de overgang knooppunt API om te fungeren als een normale lid van het cluster opnieuw wordt geretourneerd.  Ziet u de uitvoer van de API-knooppunt query *IsStopped* als onwaar, en *NodeStatus* als iets dat niet niet actief is (bijvoorbeeld omhoog).


**Beperkte duur** wanneer u de API van de overgang knooppunt om te stoppen van een knooppunt, een van de vereiste parameters *stopNodeDurationInSeconds*, vertegenwoordigt de hoeveelheid tijd in seconden dat het knooppunt *gestopt*.  Deze waarde moet liggen in het toegestane bereik, waarvoor minimaal 600, en een maximum van 14400.  Nadat deze tijd is verlopen, het knooppunt wordt automatisch opnieuw opgestart zelf in werking.  Raadpleeg Voorbeeld 1 hieronder voor een voorbeeld van gebruik.

> [!WARNING]
> Vermijd een combinatie van knooppunt overgang API's en het knooppunt stoppen en starten knooppunt API's.  De aanbeveling is het gebruik van de alleen-knooppunt overgang-API.  > Als een knooppunt is al gestopt met behulp van het knooppunt stoppen API, het moet worden gestart met behulp van het knooppunt Start API eerst voordat met behulp van de > knooppunt overgang API's.

> [!WARNING]
> Meerdere knooppunt overgang API's aanroepen kunnen niet worden gemaakt op hetzelfde knooppunt parallel.  In een dergelijke situatie, de API van de overgang knooppunt wordt > throw een FabricException met een eigenschapswaarde ErrorCode van NodeTransitionInProgress.  Zodra de knooppuntovergang van een op een specifiek knooppunt heeft > is gestart, moet u wachten totdat de bewerking wordt een definitieve status (voltooid, Faulted of ForceCancelled) voordat u begint met bereikt een > nieuwe overgang op hetzelfde knooppunt.  Parallelle knooppunt overgang aanroepen op verschillende knooppunten zijn toegestaan.


#### <a name="sample-usage"></a>Voorbeeld van gebruik


**Voorbeeld 1** -het volgende voorbeeld maakt gebruik van de API van de overgang knooppunt om te stoppen van een knooppunt.

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

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Voorbeeld 2** -het volgende voorbeeld wordt gestart een *gestopt* knooppunt.  Sommige methoden van het eerste voorbeeld wordt gebruikt.

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

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Voorbeeld 3** -het volgende voorbeeld toont onjuiste syntaxis.  Dit gebruik is onjuist omdat de *stopDurationInSeconds* biedt groter is dan het toegestane bereik.  De bewerking is niet geaccepteerd omdat StartNodeTransitionAsync() met een onherstelbare fout mislukken, en de API wordt uitgevoerd, mag niet worden aangeroepen.  In dit voorbeeld maakt gebruik van bepaalde Help-methoden van het eerste voorbeeld.

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

**Voorbeeld 4** -het volgende voorbeeld toont de gegevens van de fout die wordt geretourneerd van de API-knooppunt overgang wordt uitgevoerd wanneer de bewerking gestart door de API van de overgang knooppunt wordt geaccepteerd, maar later is mislukt tijdens het uitvoeren van.  In het geval is, mislukt de bewerking omdat de API van de overgang knooppunt probeert te starten van een knooppunt dat niet bestaat.  In dit voorbeeld maakt gebruik van bepaalde Help-methoden van het eerste voorbeeld.

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
[nodequeryps]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode
[snt]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_StartNodeTransitionAsync_System_Fabric_Description_NodeTransitionDescription_System_TimeSpan_System_Threading_CancellationToken_
[gntp]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient#System_Fabric_FabricClient_TestManagementClient_GetNodeTransitionProgressAsync_System_Guid_System_TimeSpan_System_Threading_CancellationToken_

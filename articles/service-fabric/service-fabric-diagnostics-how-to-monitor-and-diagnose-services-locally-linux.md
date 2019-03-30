---
title: Fouten opsporen in Azure Service Fabric-apps in Linux | Microsoft Docs
description: Informatie over het bewaken en diagnosticeren van uw Service Fabric-services op een lokale Linux-ontwikkelmachine.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: 4eebe937-ab42-4429-93db-f35c26424321
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: f0b850038a29dd0949def97b359b2b7a5ce920bc
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58659743"
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Controle en diagnose van services in een lokale machine development setup


> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

Bewaking, detecteren, onderzoeken en oplossen van problemen kunt voor diensten om door te gaan met minimale onderbreking van de gebruikerservaring. Controle en diagnose zijn essentieel belang is in een werkelijke geïmplementeerde productie-omgeving. Overstappen op een vergelijkbare model tijdens de ontwikkeling van services, zorgt u ervoor dat de diagnostische pijplijn werkt wanneer u naar een productieomgeving. Service Fabric kunt eenvoudig service waarmee ontwikkelaars voor het implementeren van diagnostische gegevens die naadloos kan worden gebruikt voor zowel lokale ontwikkeling van één machine-instellingen en instellingen voor real-world productie-cluster.


## <a name="debugging-service-fabric-java-applications"></a>Foutopsporing in Service Fabric Java-toepassingen

Voor Java-toepassingen, [meerdere frameworks voor logboekregistratie](https://en.wikipedia.org/wiki/Java_logging_framework) beschikbaar zijn. Aangezien `java.util.logging` is de standaardoptie met de JRE wordt geselecteerd, wordt ook gebruikt voor de [codevoorbeelden in GitHub](https://github.com/Azure-Samples/service-fabric-java-getting-started). Het volgende onderwerp wordt uitgelegd hoe u het configureren van de `java.util.logging` framework.

U kunt met behulp van java.util.logging uw toepassingslogboeken omleiden naar geheugen, uitvoerstromen, consolebestanden of sockets. Voor elk van deze opties zijn er standaard handlers al opgegeven in het kader. U kunt maken een `app.properties` bestand dat u wilt configureren van de bestands-handler voor uw toepassing zodat alle logboeken worden omgeleid naar een lokaal bestand.

Het volgende codefragment bevat de voorbeeldconfiguratie van een:

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log
```

De map waarnaar wordt verwezen door de `app.properties` bestand moet bestaan. Na de `app.properties` bestand wordt gemaakt, moet u ook wijzigen van het invoerpunt-script, `entrypoint.sh` in de `<applicationfolder>/<servicePkg>/Code/` map om in te stellen de eigenschap `java.util.logging.config.file` naar `app.properties` bestand. De vermelding moet eruitzien als in het volgende codefragment:

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```


Deze configuratie leidt tot de logboeken die worden verzameld in een draaiende manier op `/tmp/servicefabric/logs/`. Het logboekbestand in dit geval de naam mysfapp%u.%g.log waar:
* **%u** is een uniek nummer dat het oplossen van conflicten tussen processen gelijktijdig Java.
* **%g** is het aantal generatie onderscheid maken tussen het draaien van Logboeken.

Standaard als geen handler expliciet is geconfigureerd, is de console-handler geregistreerd. Een Bekijk de logboeken in syslog onder /var/log/syslog.

Zie voor meer informatie de [codevoorbeelden in GitHub](https://github.com/Azure-Samples/service-fabric-java-getting-started).


## <a name="debugging-service-fabric-c-applications"></a>Foutopsporing in Service Fabric C#-toepassingen


Er zijn meerdere frameworks beschikbaar voor het traceren van CoreCLR-toepassingen op Linux. Zie voor meer informatie, [GitHub: logboekregistratie](http:/github.com/aspnet/logging).  Sinds de gebeurtenisbron is bekend bij de C#-ontwikkelaars ' in dit artikel wordt een gebeurtenisbron voor tracering in CoreCLR-voorbeelden op Linux.

De eerste stap is het System.Diagnostics.Tracing bevatten, zodat u kunt uw logboeken naar geheugen, uitvoerstromen of console-bestanden schrijven.  Voor logboekregistratie met behulp van EventSource, moet u het volgende project toevoegen aan uw project.json:

```json
    "System.Diagnostics.StackTrace": "4.0.1"
```

U kunt een aangepaste EventListener gebruiken om te luisteren naar de servicegebeurtenis en klik vervolgens op de juiste wijze omleiden naar de logboekbestanden voor tracering. Het volgende codefragment toont een Voorbeeldimplementatie van logboekregistratie met behulp van de gebeurtenisbron en een aangepaste EventListener:


```csharp

public class ServiceEventSource : EventSource
{
        public static ServiceEventSource Current = new ServiceEventSource();

        [NonEvent]
        public void Message(string message, params object[] args)
        {
            if (this.IsEnabled())
            {
                var finalMessage = string.Format(message, args);
                this.Message(finalMessage);
            }
        }

        // TBD: Need to add method for sample event.

}

```


```csharp
internal class ServiceEventListener : EventListener
{

        protected override void OnEventSourceCreated(EventSource eventSource)
        {
            EnableEvents(eventSource, EventLevel.LogAlways, EventKeywords.All);
        }
        protected override void OnEventWritten(EventWrittenEventArgs eventData)
        {
                using (StreamWriter Out = new StreamWriter( new FileStream("/tmp/MyServiceLog.txt", FileMode.Append)))
                {
                        // report all event information
                        Out.Write(" {0} ", Write(eventData.Task.ToString(), eventData.EventName, eventData.EventId.ToString(), eventData.Level,""));
                        if (eventData.Message != null)
                                Out.WriteLine(eventData.Message, eventData.Payload.ToArray());
                        else
                        {
                                string[] sargs = eventData.Payload != null ? eventData.Payload.Select(o => o.ToString()).ToArray() : null; 
                                Out.WriteLine("({0}).", sargs != null ? string.Join(", ", sargs) : "");
                        }
                }
        }
}
```


Het bovenstaande codefragment levert de logboeken naar een bestand in `/tmp/MyServiceLog.txt`. De naam van dit bestand moet op de juiste wijze worden bijgewerkt. Als u omleiden van de logboeken wilt naar de console, gebruikt u het volgende codefragment in uw aangepaste EventListener-klasse:

```csharp
public static TextWriter Out = Console.Out;
```

De voorbeelden op [C#-voorbeelden](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) gebeurtenisbron en een aangepaste EventListener gebruiken om aan te melden van gebeurtenissen naar een bestand.



## <a name="next-steps"></a>Volgende stappen
Dezelfde tracering code toegevoegd aan uw toepassing werkt ook met de diagnostische gegevens van uw toepassing op een Azure-cluster. Lees deze artikelen die de verschillende opties voor de hulpprogramma's beschreven en wordt beschreven hoe u ze kunt instellen.
* [Over het verzamelen van logboeken met diagnostische gegevens van Azure](service-fabric-diagnostics-how-to-setup-lad.md)

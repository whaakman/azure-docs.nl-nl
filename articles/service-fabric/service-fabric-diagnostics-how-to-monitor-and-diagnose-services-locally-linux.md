---
title: Fouten opsporen in Azure microservices in Linux | Microsoft Docs
description: Informatie over het bewaken en onderzoeken van uw services die zijn geschreven met behulp van Microsoft Azure Service Fabric op een lokale ontwikkelcomputer.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 4eebe937-ab42-4429-93db-f35c26424321
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 4bc73f581f4855ebc724df19dd56fab8bf103854
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Controle en diagnose van services in de instellingen voor een lokale computer-ontwikkeling


> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

Bewaking, detecteren, onderzoeken en het oplossen van problemen kunt services wilt gaan met minimale onderbreking van de gebruikerservaring. Controle en diagnostische gegevens zijn essentieel in een werkelijke geïmplementeerde productie-omgeving. Een vergelijkbaar model overstap tijdens de ontwikkeling van services zorgt ervoor dat de diagnostische pijplijn werkt wanneer u naar een productieomgeving. Service Fabric maakt het eenvoudig voor ontwikkelaars van de service voor het implementeren van diagnostische gegevens die naadloos voor zowel lokale ontwikkeling één machine-instellingen als de echte productie cluster instellingen werkt.


## <a name="debugging-service-fabric-java-applications"></a>Foutopsporing van Service Fabric-Java-toepassingen

Voor Java-toepassingen [meerdere frameworks voor logboekregistratie](http://en.wikipedia.org/wiki/Java_logging_framework) beschikbaar zijn. Aangezien `java.util.logging` is de standaardoptie met de JRE wordt geselecteerd, wordt ook gebruikt voor de [codevoorbeelden in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  De bespreking van de volgende uitgelegd hoe u configureert de `java.util.logging` framework.

U kunt met behulp van java.util.logging uw toepassingslogboeken omleiden naar geheugen, uitvoerstromen, console-bestanden of sockets. Er zijn al opgegeven in het kader van standaard-handlers voor elk van deze opties. Kunt u een `app.properties` bestand voor het configureren van de bestands-handler voor uw toepassing alle logboeken omleiden naar een lokaal bestand.

Het volgende codefragment bevat de voorbeeldconfiguratie van een:

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

De map waarnaar wordt verwezen door de `app.properties` bestand moet bestaan. Na de `app.properties` bestand is gemaakt, moet u ook uw script post-punt wijzigen `entrypoint.sh` in de `<applicationfolder>/<servicePkg>/Code/` map instellen van de eigenschap `java.util.logging.config.file` naar `app.propertes` bestand. De vermelding moet eruitzien als in het volgende fragment:

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```


Deze configuratie resulteert in Logboeken worden verzameld in een draaiende wijze op `/tmp/servicefabric/logs/`. Het logboekbestand in dit geval heet mysfapp%u.%g.log waar:
* **%u** is een uniek nummer voor het oplossen van conflicten tussen processen gelijktijdig Java.
* **%g** is het aantal generatie onderscheid maken tussen het roteren van Logboeken.

Standaard als er geen handler expliciet is geconfigureerd, is de console-handler geregistreerd. Bekijk de logboeken als een Syslog onder /var/log/syslog.

Zie voor meer informatie de [codevoorbeelden in github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  


## <a name="debugging-service-fabric-c-applications"></a>Foutopsporing van Service Fabric C#-toepassingen


Meerdere frameworks er beschikbaar zijn voor het traceren van CoreCLR toepassingen op Linux. Zie voor meer informatie [GitHub: logboekregistratie](http:/github.com/aspnet/logging).  Aangezien EventSource bekend C# ontwikkelaars ' in dit artikel maakt gebruik van EventSource voor tracering in CoreCLR voorbeelden op Linux.

De eerste stap is het System.Diagnostics.Tracing bevatten, zodat u uw logboeken naar het geheugen, uitvoerstromen of console-bestanden schrijven kunt.  Voor logboekregistratie met behulp van EventSource, moet u het volgende project toevoegen aan uw project.json:

```
    "System.Diagnostics.StackTrace": "4.0.1"
```

U kunt een aangepaste EventListener luister naar de gebeurtenis service en klik vervolgens op de juiste wijze hen omleidt naar traceringsbestanden. Het volgende codefragment ziet u een Voorbeeldimplementatie van logboekregistratie met behulp van EventSource en een aangepaste EventListener:


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
         Out.Write(" {0} ",  Write(eventData.Task.ToString(), eventData.EventName, eventData.EventId.ToString(), eventData.Level,""));
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


Het bovenstaande codefragment levert de logboeken naar een bestand in `/tmp/MyServiceLog.txt`. Deze naam moet op de juiste wijze worden bijgewerkt. Als u dat de logboeken wilt naar de console wordt omgeleid, gebruikt u het volgende fragment in uw aangepaste EventListener-klasse:

```csharp
public static TextWriter Out = Console.Out;
```

De voorbeelden op [voorbeelden C#](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) gebruik EventSource en een aangepaste EventListener voor het vastleggen van gebeurtenissen in een bestand.



## <a name="next-steps"></a>Volgende stappen
Dezelfde tracering code toegevoegd aan uw toepassing werkt ook met de diagnostische gegevens van uw toepassing in een Azure-cluster. Bekijk deze artikelen die de verschillende opties voor de hulpprogramma's worden behandeld en wordt beschreven hoe deze moeten worden ingesteld.
* [Het verzamelen van logboeken met diagnostische Azure-gegevens](service-fabric-diagnostics-how-to-setup-lad.md)

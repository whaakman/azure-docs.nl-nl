---
title: 'Voorbeeld: Video-analyse in realtime: Computer Vision'
titlesuffix: Azure Cognitive Services
description: Leer hoe u met de Computer Vision-API vrijwel in realtime een analyse kunt uitvoeren van frames afkomstig uit een live-videostream.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 01/20/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: b27db1edc60a4eede297ac1ecd8afda671912685
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222397"
---
# <a name="how-to-analyze-videos-in-real-time"></a>Handleiding voor realtime video-analyse
In deze handleiding wordt uitgelegd hoe u bijna in realtime een analyse kunt uitvoeren van frames die afkomstig zijn uit een live-videostream. Dit zijn de basisstappen van een dergelijk systeem:

- Frames verkrijgen uit een videobron
- Selecteren welke frames u wilt analyseren
- Deze frames verzenden naar de API
- De analyseresultaten verbruiken die worden geretourneerd door de API-aanroep

Deze voorbeelden zijn geschreven in C# en de code vindt u hier op GitHub: [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/).

## <a name="the-approach"></a>De benadering
Er zijn meerdere manieren om het probleem op te lossen voor het bijna in realtime analyseren van videostreams. We gaan eerst drie benaderingen beschrijven die steeds uitgebreider worden.

### <a name="a-simple-approach"></a>Een eenvoudige benadering
Het eenvoudigste ontwerp voor een systeem voor bijna in realtime analyseren is een oneindige lus, waarbij we bij elke iteratie een frame pakken, dit frame analyseren en vervolgens het resultaat gebruiken:
```CSharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        AnalysisResult r = await Analyze(f);
        ConsumeResult(r);
    }
}
```
Als onze analyse bestaat uit een lichtgewicht algoritme op de client, is dit een geschikte benadering. Wanneer onze analyse echter in de cloud plaatsvindt, zou een API-aanroep door de betrokken latentie mogelijk enkele seconden duren. In die tijd leggen we geen afbeeldingen vast en doet onze thread als het ware niets. Onze maximale framesnelheid wordt beperkt door de latentie van de API-aanroepen.

### <a name="parallelizing-api-calls"></a>API-aanroepen parallel maken
Waar een eenvoudige lus met één thread nog wel kan worden gebruikt voor een lichtgewicht algoritme aan de clientzijde, is deze minder geschikt voor de latentie waarmee u in API-aanroepen via de cloud te maken hebt. Als u dit probleem wilt oplossen, staat u toe dat de langlopende API-aanroepen tegelijkertijd worden uitgevoerd met het ophalen van de frames. In C# kan dit worden bereikt met behulp van op taken gebaseerd parallellisme, bijvoorbeeld:
```CSharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        var t = Task.Run(async () => 
        {
            AnalysisResult r = await Analyze(f);
            ConsumeResult(r);
        }
    }
}
```
Bij deze benadering wordt elke analyse gestart in een afzonderlijke taak die op de achtergrond kan worden uitgevoerd, terwijl u nieuwe frames blijft ophalen. Het voorkomt dat de hoofdthread wordt geblokkeerd tijdens het wachten op de terugkeer van een API-oproep. Enkele van de garanties die de eenvoudige versie bood, zijn echter verdwenen – meerdere API-oproepen kunnen parallel plaatsvinden, en de resultaten kunnen dan in de verkeerde volgorde worden geretourneerd. Deze benadering kan ook tot gevolg hebben dat meerdere threads tegelijkertijd de functie ConsumeResult() betreden, wat gevaarlijk kan zijn als de functie niet thread-veilig is. Ten slotte houdt deze eenvoudige code niet bij welke taken er worden gemaakt, wat betekent dat uitzonderingen geruisloos verdwijnen. Het laatste onderdeel is dan ook het toevoegen van een 'consumer'-thread die de analysetaken volgt, uitzonderingen genereert, langlopende taken beëindigt en ervoor zorgt dat de resultaten in de juiste volgorde en één voor één worden verbruikt.

### <a name="a-producer-consumer-design"></a>Een ontwerp voor producers en consumers
In ons uiteindelijke systeem met producers en consumers hebben we een producer-thread die lijkt op onze vorige oneindige lus. Echter in plaats van analyseresultaten direct te verbruiken zodra deze beschikbaar zijn, plaatst de producer de taken in een wachtrij om ze te volgen.
```CSharp
// Queue that will contain the API call tasks. 
var taskQueue = new BlockingCollection<Task<ResultWrapper>>();
     
// Producer thread. 
while (true)
{
    // Grab a frame. 
    Frame f = GrabFrame();
 
    // Decide whether to analyze the frame. 
    if (ShouldAnalyze(f))
    {
        // Start a task that will run in parallel with this thread. 
        var analysisTask = Task.Run(async () => 
        {
            // Put the frame, and the result/exception into a wrapper object.
            var output = new ResultWrapper(f);
            try
            {
                output.Analysis = await Analyze(f);
            }
            catch (Exception e)
            {
                output.Exception = e;
            }
            return output;
        }
        
        // Push the task onto the queue. 
        taskQueue.Add(analysisTask);
    }
}
```
We hebben ook een consumer-thread, die taken uit de wachtrij haalt, wacht tot ze zijn voltooid, en vervolgens het resultaat of de uitzondering die is gegenereerd, weergeeft. Met behulp van de wachtrij kunnen we garanderen dat resultaten één voor één worden verbruikt, in de juiste volgorde, zonder dat er een limiet wordt afgedwongen voor de maximale framesnelheid van het systeem.
```CSharp
// Consumer thread. 
while (true)
{
    // Get the oldest task. 
    Task<ResultWrapper> analysisTask = taskQueue.Take();
 
    // Await until the task is completed. 
    var output = await analysisTask;
     
    // Consume the exception or result. 
    if (output.Exception != null)
    {
        throw output.Exception;
    }
    else
    {
        ConsumeResult(output.Analysis);
    }
}
```

## <a name="implementing-the-solution"></a>De oplossing implementeren
### <a name="getting-started"></a>Aan de slag
Om uw app zo snel mogelijk draaiend te krijgen, hebben we het hierboven beschreven systeem geïmplementeerd, met de bedoeling dat het flexibel genoeg is voor het implementeren van verschillende scenario's en tegelijkertijd eenvoudig te gebruiken is. Ga voor toegang tot de code naar [https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis).

De bibliotheek bevat de klasse FrameGrabber, waarmee het hierboven beschreven systeem van producers en consumers wordt geïmplementeerd voor het verwerken van videoframes van een webcam. De gebruiker kan de exacte vorm van de API-aanroep opgeven. De klasse maakt tevens gebruik van gebeurtenissen om aan de aanroepende code door te geven wanneer er een nieuw frame is verkregen of een nieuw analyseresultaat beschikbaar is.

Ter illustratie van enkele van de mogelijkheden, zijn er twee voorbeeld-apps die gebruikmaken van de bibliotheek. De eerste is een eenvoudige console-app, waarvan u hieronder een vereenvoudigde versie ziet. De app legt frames van de standaardwebcam vast en verzendt deze naar de Face-API voor gezichtsherkenning.
```CSharp
using System;
using VideoFrameAnalyzer;
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;
     
namespace VideoFrameConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create grabber, with analysis type Face[]. 
            FrameGrabber<Face[]> grabber = new FrameGrabber<Face[]>();
            
            // Create Face API Client. Insert your Face API key here.
            FaceServiceClient faceClient = new FaceServiceClient("<subscription key>");

            // Set up our Face API call.
            grabber.AnalysisFunction = async frame => return await faceClient.DetectAsync(frame.Image.ToMemoryStream(".jpg"));

            // Set up a listener for when we receive a new result from an API call. 
            grabber.NewResultAvailable += (s, e) =>
            {
                if (e.Analysis != null)
                    Console.WriteLine("New result received for frame acquired at {0}. {1} faces detected", e.Frame.Metadata.Timestamp, e.Analysis.Length);
            };
            
            // Tell grabber to call the Face API every 3 seconds.
            grabber.TriggerAnalysisOnInterval(TimeSpan.FromMilliseconds(3000));

            // Start running.
            grabber.StartProcessingCameraAsync().Wait();

            // Wait for keypress to stop
            Console.WriteLine("Press any key to stop...");
            Console.ReadKey();
            
            // Stop, blocking until done.
            grabber.StopProcessingAsync().Wait();
        }
    }
}
```
De tweede voorbeeld-app is iets interessanter, en stelt u in staat om te kiezen welke API u wilt aanroepen voor de videoframes. Aan de linkerkant toont de app een voorbeeld van de live-video en aan de rechterkant ziet u het meest recente API-resultaat als overlay over het bijbehorende frame.

In de meeste modi is er een zichtbare vertraging tussen de livevideo aan de linkerkant en de gevisualiseerde analyse aan de rechterkant. Deze vertraging is de tijd die nodig is om de API-aanroep te maken. De uitzondering hierop is in de modus EmotionsWithClientFaceDetect, waarin gezichtsdetectie lokaal wordt uitgevoerd op de clientcomputer met behulp van OpenCV, voordat er afbeeldingen naar Cognitive Services worden verzonden. Op deze manier wordt het gedetecteerde gezicht direct gevisualiseerd en worden de emoties later bijgewerkt zodra de API-aanroep is uitgevoerd. Dit demonstreert de mogelijkheid van een 'hybride' benadering, waarbij bepaalde eenvoudige verwerkingen kunnen worden uitgevoerd op de client, waarna de API's van Cognitive Services kunnen worden gebruikt om dit waar nodig te verbeteren met een meer geavanceerde analyse.

![Schermafbeelding van de LiveCameraSample-app waarin een afbeelding met tags wordt weergegeven](../../Video/Images/FramebyFrame.jpg)

### <a name="integrating-into-your-codebase"></a>Integreren in de codebase
Als u met dit voorbeeld aan de slag wilt, volgt u deze stappen:

1. Haal API-sleutels voor de Vision-API's op uit [Abonnementen](https://azure.microsoft.com/try/cognitive-services/). Voor analyse van videoframes zijn dit de relevante API's:
    - [Computer Vision-API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
    - [Emotion-API](https://docs.microsoft.com/azure/cognitive-services/emotion/home)
    - [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/overview)
2. Kloon de GitHub-opslagplaats [Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/)

3. Open het voorbeeld in Visual Studio 2015, bouw de voorbeeldtoepassingen en voer ze uit:
    - Voor BasicConsoleSample is de Face-API-sleutel rechtstreeks in  [BasicConsoleSample/Program.cs](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/blob/master/Windows/BasicConsoleSample/Program.cs) in code vastgelegd.
    - Voor LiveCameraSample moeten de sleutels worden ingevoerd in het instellingenvenster van de app. De sleutels worden als gebruikersgegevens gehandhaafd tussen sessies.
        

Wanneer u klaar om te gaan integreren, **verwijst u vanuit uw eigen projecten naar de bibliotheek VideoFrameAnalyzer**. 

De voorzieningen voor afbeeldingen, spraak, video of tekstbegrip van VideoFrameAnalyzer maken gebruik van Azure Cognitive Services. Microsoft ontvangt de afbeeldingen, audio, video en andere gegevens die u uploadt (via deze app) en kan deze gebruiken met als doel de service te verbeteren. We vragen uw hulp bij het beschermen van de personen van wie uw app gegevens naar Azure Cognitive Services verzendt. 


## <a name="summary"></a>Samenvatting
In deze handleiding hebt u geleerd hoe u bijna in realtime analyses van live-videostreams kunt uitvoeren met behulp van de Face-, Computer Vision- en Emotion-API's en hoe u onze voorbeeldcode kunt gebruiken om snel aan de slag te gaan. U kunt uw eigen app gaan bouwen met behulp van API-sleutels die u gratis kunt ophalen op de [registratiepagina voor Microsoft Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). 

U kunt feedback en suggesties achterlaten in de [GitHub-opslagplaats](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/). Voor meer uitgebreide API-feedback kunt u terecht op onze  [UserVoice-site](https://cognitive.uservoice.com/).


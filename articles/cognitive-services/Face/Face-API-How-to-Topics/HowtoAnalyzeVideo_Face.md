---
title: Analyse van realtime video met de Face-API | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Gebruik de Face-API in cognitieve Services near-realtime analyses uitvoeren op frames die afkomstig zijn uit een live videostream.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 8675f992ddffe2eedfeac294a6c57560434802c2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344772"
---
# <a name="how-to-analyze-videos-in-real-time"></a>Video's in realtime analyseren
Deze handleiding wordt gedemonstreerd hoe kunt near-realtime analyses uitvoeren op frames die afkomstig zijn uit een live videostream. De basisonderdelen van een dergelijk systeem zijn:
- Frames van een videobron verkrijgen
- Selecteer welke frames analyseren
- Deze frames aan de API te verzenden
- Verbruiken elk resultaat dat wordt geretourneerd van de API-aanroep

Deze voorbeelden zijn geschreven in C# en de code hier te vinden op GitHub: [ https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis ](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/).

## <a name="the-approach"></a>De methode
Er zijn meerdere manieren voor het oplossen van het probleem van het in de buurt van de realtime-analyses uitvoeren op video stromen. We wordt gestart door een overzicht van de drie methoden in verfijning te verhogen.

### <a name="a-simple-approach"></a>Een eenvoudige benadering
De eenvoudigste ontwerp voor een systeem in de buurt van de realtime-analyse is een oneindige lus waar in elke herhaling we een frame halen, analyseren en vervolgens het resultaat te gebruiken:
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
Als onze analyse bestond uit een lichtgewicht client-side '-algoritme, zou deze benadering geschikt zijn. Echter als onze analyse gebeurt er in de cloud, betekent de betrokken latentie dat een API-aanroep enkele seconden kan duren, tijdens deze periode we niet van installatiekopieën vastleggen en onze thread is in wezen niets te doen. Onze maximale framesnelheid wordt beperkt door de latentie van de API-aanroepen.

### <a name="parallelizing-api-calls"></a>Parallelizing API-aanroepen
Terwijl een eenvoudige single thread-lus zinvol is voor een lichtgewicht client-side '-algoritme, past niet goed met de latentie die zijn betrokken bij cloud API-aanroepen. De oplossing voor dit probleem is dat de langlopende API-aanroepen om uit te voeren in samenspraak met frame grabbing. In C#, kan Wij realiseren dit met behulp van parallelle uitvoering op basis van een taak, bijvoorbeeld:
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
Elke analyse in een afzonderlijke taak op de achtergrond uitvoeren kunt terwijl we blijven grabbing nieuwe frames wordt gestart. Zo voorkomt u blokkeert de hoofdthread terwijl een API-aanroep om te retourneren, maar we verloren zijn gegaan enkele van de garanties die wachten op de eenvoudige versie geboden--meerdere API-aanroepen parallel optreden en de resultaten mogelijk geretourneerd in de verkeerde volgorde. Dit kan ook meerdere threads tegelijk, geeft u de functie ConsumeResult() die kan worden als de functie niet thread-veilige is gevaarlijke veroorzaken. Ten slotte deze eenvoudige code komt niet bijhouden van de taken die worden gemaakt, zodat uitzonderingen achtergrond verdwijnt. Het laatste ingrediënt voor ons om toe te voegen is dus een 'consumer' thread die de analysis-taken te volgen, uitzonderingen veroorzaakt, kill langlopende taken worden uitgevoerd, en zorg ervoor dat de resultaten in de juiste volgorde, één voor één ophalen verbruikt.

### <a name="a-producer-consumer-design"></a>Een producent-Consumer-ontwerp
We hebben een producent thread die erg op onze vorige oneindige lus lijkt in ons systeem laatste "Maker-gebruiker'. Echter in plaats van de resultaten van de analyse worden verbruikt zodra ze beschikbaar zijn, de producent gewoon worden de taken in een wachtrij te volgen.
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
We hebben ook een consumer-thread, is dat taken uit de wachtrij, wacht tot deze zijn voltooid, en om het resultaat weer te geven of het verhogen van de uitzondering die is geretourneerd. Wij kunnen met behulp van de wachtrij garanderen resultaten verbruikte één op een tijdstip in de juiste volgorde ophalen onverminderd de maximale framesnelheid van het systeem.
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
Als u uw app up en wordt zo snel mogelijk worden uitgevoerd, hebben we het hierboven beschreven, systeem geïmplementeerd wilde het zo flexibel genoeg is voor het implementeren van vele scenario's, terwijl eenvoudig te gebruiken. Voor toegang tot de code, gaat u naar [ https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis ](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis).

De bibliotheek bevat de klasse FrameGrabber waarmee het hierboven beschreven voor het verwerken van video frames uit een webcam producent-consumer-systeem wordt geïmplementeerd. De gebruiker kan de exacte vorm van de API-aanroep, en gebeurtenissen te laten de aanroepende code weten wanneer een nieuw frame wordt verkregen, of een resultaat van de nieuwe beschikbaar maakt gebruik van de klasse.

Ter illustratie van enkele van de mogelijkheden, zijn er twee voorbeeld-apps die gebruikmaakt van de bibliotheek. De eerste is een eenvoudige consoletoepassing en een vereenvoudigde versie van dit wordt hieronder. Het pakt frames van de standaard webcam en verstuurd naar de Face-API voor face-detectie.
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
            FaceServiceClient faceClient = new FaceServiceClient("<Subscription Key>");

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
Het tweede voorbeeld-app is iets interessanter en kunt u kiezen welke API aan te roepen voor de video frames. Aan de linkerzijde geeft de app een voorbeeld van de live video aan de rechterkant ziet u het meest recente API-resultaat heen worden weergegeven op het bijbehorende frame.

In de meeste modi, wordt er een zichtbaar vertraging optreden tussen de live video aan de linkerkant en de gevisualiseerde analyse aan de rechterkant. Deze vertraging is de tijd die nodig is voor het maken van de API-aanroep. De uitzondering hierop is in de modus 'EmotionsWithClientFaceDetect', waarmee face detection lokaal wordt uitgevoerd op de clientcomputer OpenCV, vóór het verzenden van alle installatiekopieën cognitieve Services gebruiken. Op deze manier kunnen we de gedetecteerde face onmiddellijk visualiseren en werk vervolgens de emoties later zodra de API-aanroep als resultaat geeft. Dit toont aan de mogelijkheid van een benadering 'hybride', waarbij sommige eenvoudige verwerking kan worden uitgevoerd op de client en vervolgens cognitieve Services-API's kunnen worden gebruikt voor dit met meer geavanceerde analyses indien nodig te verbeteren.

![HowToAnalyzeVideo](../../Video/Images/FramebyFrame.jpg)

### <a name="integrating-into-your-codebase"></a>Integreren in uw codebase
Voer de volgende stappen uit om te beginnen met dit voorbeeld:

1. API-sleutels ophalen voor de Vision-API's van [abonnementen](https://azure.microsoft.com/try/cognitive-services/). Voor een video frame-analyse zijn de toepasselijke API's:
    - [Computer Vision-API](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
    - [Emotion-API](https://docs.microsoft.com/azure/cognitive-services/emotion/home)
    - [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/overview)
2. Kloon de [Cognitive-Samples-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) GitHub-repo-

3. Open het voorbeeld in Visual Studio 2015, bouwen en uitvoeren van de voorbeeldtoepassingen:
    - De Face-API-sleutel is voor BasicConsoleSample, vastgelegde rechtstreeks in [BasicConsoleSample/Program.cs](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/blob/master/Windows/BasicConsoleSample/Program.cs).
    - Voor LiveCameraSample, moeten de sleutels worden ingevoerd in het deelvenster instellingen van de app. Ze zullen worden vastgehouden over de sessies als gebruikersgegevens.
        

Wanneer u klaar bent om te integreren, **gewoon verwijst naar de bibliotheek VideoFrameAnalyzer van uw eigen projecten.** 



## <a name="developer-code-of-conduct"></a>Gedragscode voor ontwikkelaars
Als met de cognitieve Services ontwikkelaars ontwikkelen met onze API's en voorbeelden moeten zich aan de '[Developer gedragscode voor Services van Microsoft Cognitive](https://azure.microsoft.com/support/legal/developer-code-of-conduct/). " 


De installatiekopie, spraak, video of tekst mogelijkheden van VideoFrameAnalyzer maakt gebruik van Microsoft cognitieve Services. Microsoft ontvangt de afbeeldingen, audio, video en andere gegevens uploaden (via deze app) uit te voeren en kan deze gebruiken voor andere doeleinden CEIP service. We vragen voor uw hulp bij het beschermen van de mensen uw app naar Microsoft cognitieve Services verzendt waarvan de gegevens. 


## <a name="summary"></a>Samenvatting
In deze handleiding hebt u geleerd near-realtime analyse uitvoeren voor live video stromen met Face-, Computer Vision- en Emotion-API's en hoe u onze voorbeeldcode aan de slag kunt gebruiken.  U kunt aan de slag uw app ontwikkelen met vrije API-sleutels op de [cognitieve Services van Microsoft-aanmeldingspagina](https://azure.microsoft.com/try/cognitive-services/). 

U gerust feedback en suggesties in de [GitHub-opslagplaats](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/), of voor meer algemene API feedback over onze [UserVoice-site](https://cognitive.uservoice.com/).



## <a name="related"></a> Verwante onderwerpen
- [Het identificeren van bespreekt in afbeelding](HowtoIdentifyFacesinImage.md)
- [Het vaststellen van bespreekt in afbeelding](HowtoDetectFacesinImage.md)

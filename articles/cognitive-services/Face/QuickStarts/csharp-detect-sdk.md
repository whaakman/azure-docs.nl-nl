---
title: 'Snelstart: gezichten in een afbeelding detecteren - SDK, C#'
titleSuffix: Azure Cognitive Services
description: In deze snelstart detecteert u gezichten in een afbeelding met behulp van de Face Windows C# clientbibliotheek in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 09/14/2018
ms.author: nolachar
ms.openlocfilehash: a9a7efd89f8e7462812064615d07acf12acbc3a3
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364095"
---
# <a name="quickstart-detect-faces-in-an-image-using-c35---face"></a>Snelstart: gezichten in een afbeelding detecteren met C&#35; - Gezicht

In deze snelstart detecteert u menselijke gezichten in een afbeelding met behulp van de Face Windows clientbibliotheek.

De broncode voor dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/Face).

## <a name="prerequisites"></a>Vereisten

* U hebt een abonnementssleutel nodig om het voorbeeld uit te voeren. U kunt abonnementssleutels voor een gratis proefversie downloaden op [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=face-api).
* Eender welke versie van [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* Het NuGet-pakket van de [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)-clientbibliotheek. U hoeft het pakket niet te downloaden. Hieronder vindt u de installatie-instructies.

## <a name="detectwithurlasync-method"></a>Methode DetectWithUrlAsync

De methoden `DetectWithUrlAsync` en `DetectWithStreamAsync` verpakken de [Face - Detect API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) voor respectievelijk externe en lokale afbeeldingen. Gebruik deze methoden om gezichten te herkennen in een afbeelding en gezichtskenmerken te retourneren, waaronder:

* Face ID: de unieke ID die wordt gebruikt in verschillende Face-API-scenario's.
* Gezichtsrechthoek: de coördinaten die de locatie van het gezicht in de afbeelding aangeven (links, boven, breedte en hoogte).
* Oriëntatiepunten: een matrix van 27 gezichtsoriëntatiepunten die verwijzen naar de belangrijkste posities van de gezichtsonderdelen.
* Gezichtskenmerken zoals leeftijd, geslacht, glimlachintensiteit, hoofdhouding en gezichtshaar.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Maak in Visual Studio een nieuwe Visual C#-console-app.
1. Installeer het Face clientbibliotheer NuGet-pakket.
    1. Klik in het menu op **Extra**, selecteer **NuGet Package Manager** en vervolgens **NuGet-pakketten voor oplossing beheren**.
    1. Klik op het tabblad **Bladeren** en selecteer vervolgens **Prerelease toevoegen**.
    1. In het **Zoek** -vak type "Microsoft.Azure.CognitiveServices.Vision.Face".
    1. Selecteer **Microsoft.Azure.CognitiveServices.Vision.Face** wanneer dit wordt weergegeven, klik op het selectievakje naast uw projectnaam en klik op **Installeren**.
1. Vervang *Program.cs* door de code hieronder.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig zo nodig `faceEndpoint` in de Azure-regio die is gekoppeld aan uw abonnementssleutels.
1. Vervang `LocalImage>` desgewenst door het pad en de bestandsnaam van een lokale afbeelding (wordt genegeerd indien niet ingesteld).
1. Stel `remoteImageUrl` desgewenst in op een andere afbeelding.
1. Voer het programma uit.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace DetectFace
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // You must use the same region as you used to get your subscription
        // keys. For example, if you got your subscription keys from westus,
        // replace "westcentralus" with "westus".
        //
        // Free trial subscription keys are generated in the westcentralus
        // region. If you use a free trial subscription key, you shouldn't
        // need to change the region.
        // Specify the Azure region
        private const string faceEndpoint =
            "https://westcentralus.api.cognitive.microsoft.com";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg";

        private static readonly FaceAttributeType[] faceAttributes =
            { FaceAttributeType.Age, FaceAttributeType.Gender };

        static void Main(string[] args)
        {
            FaceClient faceClient = new FaceClient(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });
            faceClient.Endpoint = faceEndpoint;

            Console.WriteLine("Faces being detected ...");
            var t1 = DetectRemoteAsync(faceClient, remoteImageUrl);
            var t2 = DetectLocalAsync(faceClient, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press any key to exit");
            Console.ReadLine();
        }

        // Detect faces in a remote image
        private static async Task DetectRemoteAsync(
            FaceClient faceClient, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine("\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            try
            {
                IList<DetectedFace> faceList =
                    await faceClient.Face.DetectWithUrlAsync(
                        imageUrl, true, false, faceAttributes);

                DisplayAttributes(GetFaceAttributes(faceList, imageUrl), imageUrl);
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imageUrl + ": " + e.Message);
            }
        }

        // Detect faces in a local image
        private static async Task DetectLocalAsync(FaceClient faceClient, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            try
            {
                using (Stream imageStream = File.OpenRead(imagePath))
                {
                    IList<DetectedFace> faceList =
                            await faceClient.Face.DetectWithStreamAsync(
                                imageStream, true, false, faceAttributes);
                    DisplayAttributes(
                        GetFaceAttributes(faceList, imagePath), imagePath);
                }
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imagePath + ": " + e.Message);
            }
        }

        private static string GetFaceAttributes(
            IList<DetectedFace> faceList, string imagePath)
        {
            string attributes = string.Empty;

            foreach (DetectedFace face in faceList)
            {
                double? age = face.FaceAttributes.Age;
                string gender = face.FaceAttributes.Gender.ToString();
                attributes += gender + " " + age + "   ";
            }

            return attributes;
        }

        // Display the face attributes
        private static void DisplayAttributes(string attributes, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(attributes + "\n");
        }
    }
}
```

### <a name="detectwithurlasync-response"></a>Antwoord DetectWithUrlAsync

Een geslaagd antwoord retourneert het geslacht en de leeftijd van elk gezicht in de afbeelding.

Zie [API Snelstart : Gezichten herkennen in een afbeelding met gebruik van C#](CSharp.md) voor een voorbeeld van onbewerkte JSON-uitvoer.

```
https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg
Male 37   Female 56
```

## <a name="next-steps"></a>Volgende stappen

Hier leert u hoe u een WPF-Windows-toepassing kunt maken die gebruikmaakt van de Face-API om gezichten in een afbeelding te herkennen. De toepassing tekent een kader rond elk gezicht en geeft een beschrijving van het gezicht weer op de statusbalk.

> [!div class="nextstepaction"]
> [Zelfstudie: een WPF-app maken om gezichten in een afbeelding te herkennen en te omlijsten](../Tutorials/FaceAPIinCSharpTutorial.md)

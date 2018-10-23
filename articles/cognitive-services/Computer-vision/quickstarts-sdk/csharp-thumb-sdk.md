---
title: 'Quickstart: Een miniatuur genereren - SDK, C# - Computer Vision'
titleSuffix: Azure Cognitive Services
description: In deze snelstart maakt u een miniatuur van een afbeelding met behulp van de Computer Vision Windows C#-clientbibliotheek.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 09/14/2018
ms.author: pafarley
ms.openlocfilehash: 758021a9428672789923d1774f51fd319fe9885f
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343109"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-sdk-and-c"></a>Quickstart: Een miniatuur genereren met de Computer Vision SDK en C#

In deze snelstart maakt u een miniatuur van een afbeelding met behulp van de Computer Vision Windows-clientbibliotheek.

## <a name="prerequisites"></a>Vereisten

* Als u Computer Vision wilt gebruiken, hebt u een abonnementssleutel nodig. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md) voor meer informatie.
* Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/).
* Het NuGet-pakket van de [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision)-clientbibliotheek. U hoeft het pakket niet te downloaden. Hieronder vindt u de installatie-instructies.

## <a name="generatethumbnailasync-method"></a>Methode GenerateThumbnailAsync

> [!TIP]
> Haal de nieuwste code als Visual Studio-oplossing op uit [GitHub](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision).

De methoden `GenerateThumbnailAsync` en `GenerateThumbnailInStreamAsync` verpakken de [Get Thumbnail-API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) voor respectievelijk externe en lokale afbeeldingen.  U kunt deze methoden gebruiken om een miniatuur van een afbeelding te genereren. U geeft de hoogte en breedte op. Deze mag afwijken van de hoogte-breedteverhouding van de ingevoerde afbeelding. Computer Vision maakt gebruik van slim bijsnijden om op intelligente wijze het interessegebied te bepalen en coördinaten voor het bijsnijden te genereren op basis van dat gebied.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Maak in Visual Studio een nieuwe Visual C#-console-app.
1. Installeer het NuGet-pakket van de Computer Vision-client-bibliotheek.
    1. Klik in het menu op **Extra**, selecteer **NuGet Package Manager** en vervolgens **NuGet-pakketten voor oplossing beheren**.
    1. Klik op het tabblad **Bladeren** en typ 'Microsoft.Azure.CognitiveServices.Vision.ComputerVision' in het vak **Zoeken**.
    1. Selecteer **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** wanneer dit wordt weergegeven, klik op het selectievakje naast uw projectnaam en klik op **Installeren**.
1. Vervang `Program.cs` door de volgende code.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig zo nodig `computerVision.Endpoint` in de Azure-regio die is gekoppeld aan uw abonnementssleutels.
1. Vervang `<LocalImage>` desgewenst door het pad en de bestandsnaam van een lokale afbeelding (wordt genegeerd indien niet ingesteld).
1. Stel `remoteImageUrl` desgewenst in op een andere afbeelding.
1. Stel `writeThumbnailToDisk` desgewenst in op `true` om de miniatuur op schijf op te slaan.
1. Voer het programma uit.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;

using System;
using System.IO;
using System.Threading.Tasks;

namespace ImageThumbnail
{
    class Program
    {
        private const bool writeThumbnailToDisk = false;

        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg";

        private const int thumbnailWidth = 100;
        private const int thumbnailHeight = 100;

        static void Main(string[] args)
        {
            ComputerVisionClient computerVision = new ComputerVisionClient(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });

            // You must use the same region as you used to get your subscription
            // keys. For example, if you got your subscription keys from westus,
            // replace "westcentralus" with "westus".
            //
            // Free trial subscription keys are generated in the westcentralus
            // region. If you use a free trial subscription key, you shouldn't
            // need to change the region.

            // Specify the Azure region
            computerVision.Endpoint = "https://westcentralus.api.cognitive.microsoft.com";

            Console.WriteLine("Images being analyzed ...\n");
            var t1 = GetRemoteThumbnailAsync(computerVision, remoteImageUrl);
            var t2 = GetLocalThumbnailAsnc(computerVision, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press ENTER to exit");
            Console.ReadLine();
        }

        // Create a thumbnail from a remote image
        private static async Task GetRemoteThumbnailAsync(
            ComputerVisionClient computerVision, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine(
                    "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            Stream thumbnail = await computerVision.GenerateThumbnailAsync(
                thumbnailWidth, thumbnailHeight, imageUrl, true);

            string path = Environment.CurrentDirectory;
            string imageName = imageUrl.Substring(imageUrl.LastIndexOf('/') + 1);
            string thumbnailFilePath =
                path + "\\" + imageName.Insert(imageName.Length - 4, "_thumb");

            // Save the thumbnail to the current working directory,
            // using the original name with the suffix "_thumb".
            SaveThumbnail(thumbnail, thumbnailFilePath);
        }

        // Create a thumbnail from a local image
        private static async Task GetLocalThumbnailAsnc(
            ComputerVisionClient computerVision, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            using (Stream imageStream = File.OpenRead(imagePath))
            {
                Stream thumbnail = await computerVision.GenerateThumbnailInStreamAsync(
                    thumbnailWidth, thumbnailHeight, imageStream, true);

                string thumbnailFilePath =
                    localImagePath.Insert(localImagePath.Length - 4, "_thumb");

                // Save the thumbnail to the same folder as the original image,
                // using the original name with the suffix "_thumb".
                SaveThumbnail(thumbnail, thumbnailFilePath);
            }
        }

        // Save the thumbnail locally.
        // NOTE: This will overwrite an existing file of the same name.
        private static void SaveThumbnail(Stream thumbnail, string thumbnailFilePath)
        {
            if (writeThumbnailToDisk)
            {
                using (Stream file = File.Create(thumbnailFilePath))
                {
                    thumbnail.CopyTo(file);
                }
            }
            Console.WriteLine("Thumbnail {0} written to: {1}\n",
                writeThumbnailToDisk ? "" : "NOT", thumbnailFilePath);
        }
    }
}
```

## <a name="generatethumbnailasync-response"></a>GenerateThumbnailAsync-antwoord

Bij een geslaagde respons wordt de miniatuur van elke afbeelding lokaal opgeslagen en wordt de locatie van de miniatuur weergegeven, bijvoorbeeld:

```cmd
Thumbnail written to: C:\Documents\LocalImage_thumb.jpg

Thumbnail written to: ...\bin\Debug\Bloodhound_Puppy_thumb.jpg
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API's die worden gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen.

> [!div class="nextstepaction"]
> [De Computer Vision-API's bekijken](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

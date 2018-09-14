---
title: Snelstart voor het ophalen van een miniatuur met de Computer Vision-API met cURL | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart maakt u een miniatuur van een afbeelding met behulp van Computer Vision met cURL in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 4056f05f3f4bf97761f683b5f3a9053666d4ea26
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770112"
---
# <a name="quickstart-generate-a-thumbnail---rest-curl"></a>Snelstart: Een miniatuur genereren - REST, cURL

In deze snelstart maakt u een miniatuur van een afbeelding met behulp van Computer Vision.

## <a name="prerequisites"></a>Vereisten

Als u Computer Vision wilt gebruiken, moet u een abonnementssleutel hebben. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="get-thumbnail-request"></a>Get Thumbnail-aanvraag

Met de methode [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) (miniatuur ophalen) kunt u een miniatuur van een afbeelding genereren. U geeft de hoogte en breedte op. Deze kan afwijken van de hoogte-breedteverhouding van de ingevoerde afbeelding. Computer Vision maakt gebruik van slim bijsnijden om op intelligente wijze het belangrijkste gebied te bepalen en coördinaten voor het bijsnijden te genereren op basis van dat gebied.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code naar een editor.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Vervang `<File>` door het pad en de bestandsnaam waar de miniatuur moet worden opgeslagen.
1. Wijzig zo nodig aanvraag-URL (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) in de locatie waar u de abonnementssleutels hebt verkregen.
1. Wijzig eventueel de te analyseren afbeelding (`{\"url\":\"...`).
1. Open een opdrachtvenster op een computer waarop cURL is geïnstalleerd.
1. Plak de code in het venster en voer de opdracht uit.

>[!NOTE]
>U moet in uw REST-aanroep dezelfde locatie gebruiken waar u uw abonnementssleutels hebt verkregen. Als u bijvoorbeeld uw abonnementssleutels van 'westus' hebt verkregen, vervangt u 'westcentralus' in de onderstaande URL door 'westus'.

```json
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" -o <File> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
```

## <a name="get-thumbnail-response"></a>Get Thumbnail-antwoord

Een geslaagd antwoord schrijft de miniatuurafbeelding naar `<File>`. Als de aanvraag mislukt, bevat het antwoord een foutcode en een bericht om u te helpen bepalen wat er mis is gegaan.

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API's die worden gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen. Als u snel wilt experimenteren met de Computer Vision-API's, probeert u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [De Computer Vision-API's bekijken](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)

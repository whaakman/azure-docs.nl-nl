---
title: Hoe Blitline gebruikt voor afbeelding verwerking - Azure handleiding functie
description: Informatie over het gebruik van de service Blitline verwerken van afbeeldingen binnen een Azure-toepassing.
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: support@blitline.com
ms.openlocfilehash: 1d90599e028b3407a513b04b878e3aefc39928a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Blitline gebruiken met Azure en Azure Storage
Deze handleiding wordt uitgelegd hoe voor toegang tot services Blitline en het verzenden van taken naar Blitline.

## <a name="what-is-blitline"></a>Wat is Blitline?
Blitline is een cloud-gebaseerde biedt enterprise niveau beeldverwerking een fractie van de prijs die u zelf maakt de kosten te verwerken.

Het fact is dat beeldverwerking is nog steeds opnieuw, meestal opnieuw opgebouwd voor elke website. We realiseren dit omdat we hebt gebouwd ze een miljoen keer te. Één dag die we besloten mogelijk is het tijd doen we zojuist dit voor iedereen. We weten hoe u doet dit snel en efficiënt te doen en iedereen werk in de tussentijd opslaan.

Zie voor meer informatie [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Wat Blitline is niet...
Om te verduidelijken wat Blitline is nuttig voor, is het vaak gemakkelijker te bepalen wat Blitline geen voordat u verder gaat.

* Blitline heeft geen HTML-widgets om afbeeldingen te uploaden. Openbaar of met beperkte machtigingen die beschikbaar zijn voor Blitline te bereiken, moet u installatiekopieën beschikbaar hebben.
* Blitline geen live installatiekopie verwerken, zoals Aviary.com
* Blitline uploaden van afbeeldingen niet accepteert, kan niet u uw installatiekopieën direct push naar Blitline. U moet toepassen op Azure Storage of andere locaties Blitline ondersteunt en vervolgens instrueren Blitline waar u ze.
* Blitline massively parallel en niet kan synchrone verwerking. Wat betekent dat u moet Geef ons een postback_url en kunnen we u wanneer we klaar bent verwerking.

## <a name="create-a-blitline-account"></a>Een Blitline-account maken
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Het maken van een taak Blitline
JSON Blitline gebruikt voor het definiëren van de acties die u wilt uitvoeren op een installatiekopie. Deze JSON bestaat uit een paar eenvoudige velden.

De eenvoudigste voorbeeld is als volgt:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Hier hebben we JSON die een installatiekopie van een 'src' duurt "... boys.jpeg" en vervolgens het formaat van deze afbeelding 240 x 140.

De toepassings-ID is iets vindt u in uw **VERBINDINGSGEGEVENS** of **beheren** tabblad op Azure. Het is uw geheime id waarmee u taken uitvoeren op Blitline.

De parameter 'opslaan' identificeert informatie over waar u de afbeelding te plaatsen nadat we hebben het verwerkt. In dit geval trivial nog niet hebben we een gedefinieerd. Als er geen vestiging is gedefinieerd Blitline wordt opslaan lokaal (en tijdelijk) op de locatie van een unieke cloud. U kunt zich dat de locatie van de JSON die is geretourneerd door Blitline als u de Blitline ophalen. De id 'installatiekopie' is vereist en wordt geretourneerd naar u, wanneer deze name identificeren installatiekopie opgeslagen.

U vindt meer informatie over de *functies* we hier ondersteunen: <http://www.blitline.com/docs/functions>

U kunt ook de documentatie over de opties voor de taak hier vinden: <http://www.blitline.com/docs/api>

Zodra u hebt uw JSON is hoeft u **POST** naar`http://api.blitline.com/job`

U ontvangt JSON terug die ziet er ongeveer als volgt uit:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Weet u dat Blitline heeft uw aanvraag is ontvangen, deze in een wachtrij voor verwerking geplaatst is en voltooiing van de installatiekopie zijn beschikbaar op: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Het opslaan van een installatiekopie naar uw Azure Storage-account
Als u een Azure Storage-account hebt, kunt u eenvoudig Blitline push verwerkte afbeeldingen in uw Azure-container hebt. Door het toevoegen van een 'azure_destination' definieert u de locatie en de machtigingen voor Blitline om naar te pushen.

Hier volgt een voorbeeld:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Door de CAPITALIZED waarden door uw eigen ingevuld, dient u deze JSON naar http://api.blitline.com/job en de installatiekopie van het 'src' worden verwerkt met een vervagingsfilter en vervolgens naar Azure bestemming gepusht.

### <a name="please-note"></a>Opmerking:
De SA's moet de volledige SAS-url, inclusief de bestandsnaam van het doelbestand bevatten.

Voorbeeld:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


U kunt ook de meest recente versie van Blitline van Azure Storage documenten lezen [hier](http://www.blitline.com/docs/azure_storage).

## <a name="next-steps"></a>Volgende stappen
Ga naar blitline.com voor meer informatie over onze functies:

* Blitline API-eindpunt Docs <http://www.blitline.com/docs/api>
* Blitline API-functies <http://www.blitline.com/docs/functions>
* Voorbeelden van Blitline API <http://www.blitline.com/docs/examples>
* Het derde deel Nuget bibliotheek <http://nuget.org/packages/Blitline.Net>


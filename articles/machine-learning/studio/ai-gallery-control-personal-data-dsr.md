---
title: Weergeven en verwijderen van uw gegevens uit de galerie van Azure AI | Microsoft Docs
description: U kunt exporteren en uw product gebruikersgegevens verwijderen uit de galerie van Azure AI is in de interface AI galerie catalogus API. Dit artikel ziet u hoe.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
manager: cgronlun
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: studio
ms.topic: conceptual
ms.date: 05/25/2018
ms.openlocfilehash: bc6ffa912d7914c8662dbde623e04947540ac149
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654980"
---
# <a name="view-and-delete-in-product-user-data-from-azure-ai-gallery"></a>Weergeven en binnen het product gebruikersgegevens verwijderen uit de galerie van Azure AI

U kunt bekijken en uw product gebruikersgegevens verwijderen uit de galerie van Azure AI is in de interface AI galerie catalogus API. In dit artikel leest u hoe.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="view-your-data-in-ai-gallery-with-the-ui"></a>Bekijk uw gegevens in de galerie AI met de gebruikersinterface

U kunt de items die u hebt gepubliceerd via de gebruikersinterface van de AI-galerie van Azure-website weergeven. Gebruikers kunnen bekijken, openbare en niet-vermelde oplossingen, projecten, experimenten en andere gepubliceerde items:

1.  Aanmelden bij de [galerie van Azure AI](https://gallery.azure.ai/).
2.  Klik op de profielafbeelding in de rechterbovenhoek en klik vervolgens op de accountnaam laden uw profielpagina.
3.  De profielpagina geeft alle items die zijn gepubliceerd naar de galerie, met inbegrip van niet-vermelde vermeldingen.

## <a name="use-the-ai-gallery-catalog-api-to-view-your-data"></a>De AI galerie catalogus API gebruiken om uw gegevens weer te geven

Programmatisch vindt u gegevens die zijn verzameld via de AI galerie catalogus API, die toegankelijk is op https://catalog.cortanaanalytics.com/entities. Als gegevens wilt weergeven, moet u uw auteur-ID. Als u wilt weergeven in niet-vermelde entiteiten via de API-catalogus, moet u een toegangstoken.

Catalogus reacties worden geretourneerd in JSON-indeling.

### <a name="get-an-author-id"></a>Een auteur-ID ophalen
De auteur-ID is gebaseerd op het e-mailadres dat is gebruikt bij het publiceren van de AI-galerie van Azure. Wordt niet gewijzigd:

1.  Aanmelden bij [galerie van Azure AI](https://gallery.azure.ai/).
2.  Klik op de profielafbeelding in de rechterbovenhoek en klik vervolgens op de accountnaam laden uw profielpagina.
3.  De URL in de adresbalk wordt het alfanumerieke ID volgende `authorId=`. Bijvoorbeeld: voor de URL: `https://gallery.cortanaintelligence.com/Home/Author?authorId=99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`
        
    Auteur-ID: `99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA`

### <a name="get-your-access-token"></a>Uw toegangstoken ophalen

U moet een toegangstoken niet-vermelde entiteiten via de API-catalogus weergeven. Gebruikers kunnen nog steeds zonder een Access Token bekijken openbare entiteiten en andere gebruikersgegevens.

Als u een toegangstoken, moet u controleren de `DataLabAccessToken` koptekst van een HTTP-aanvraag die de browser voor de API Catalog terwijl u bent aangemeld maakt:

1.  Aanmelden bij de [galerie van Azure AI](https://gallery.azure.ai/).
2.  Klik op de profielafbeelding in de rechterbovenhoek en klik vervolgens op de accountnaam laden uw profielpagina.
3.  Het deelvenster van de hulpprogramma's voor ontwikkelaars browser openen door op F12 te drukken, selecteer het tabblad netwerk en vernieuw de pagina. 
4. Filteren van aanvragen op de tekenreeks *catalogus* door in het tekstvak Filter te typen.
5.  In hun aanvragen aan de URL `https://catalog.cortanaanalytics.com/entities`, zoeken naar een GET-aanvraag en selecteer de *Headers* tabblad. Schuif omlaag naar de *aanvraagheaders* sectie.
6.  Onder de kop `DataLabAccessToken` is het alfanumerieke token. Niet om u te helpen uw gegevens te beveiligen, delen deze token.

### <a name="view-user-information"></a>Informatie van de gebruiker weergeven
Met de auteur-ID die u hebt verkregen in de vorige stappen, gegevens weergeven in het profiel van een gebruiker door te vervangen `[AuthorId]` in de volgende URL:

    https://catalog.cortanaanalytics.com/users/[AuthorID]

Deze URL-aanvraag:
    
    https://catalog.cortanaanalytics.com/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA

Retourneert een antwoord, zoals:

    {"entities_count":9,"contribution_score":86.351575190956922,"scored_at":"2018-05-07T14:30:25.9305671+00:00","contributed_at":"2018-05-07T14:26:55.0381756+00:00","created_at":"2017-12-15T00:49:15.6733094+00:00","updated_at":"2017-12-15T00:49:15.6733094+00:00","name":"First Last","slugs":["First-Last"],"tenant_id":"14b2744cf8d6418c87ffddc3f3127242","community_id":"9502630827244d60a1214f250e3bbca7","id":"99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA","_links":{"self":"https://catalog.azureml.net/tenants/14b2744cf8d6418c87ffddc3f3127242/communities/9502630827244d60a1214f250e3bbca7/users/99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA"},"etag":"\"2100d185-0000-0000-0000-5af063010000\""}


### <a name="view-public-entities"></a>Openbare entiteiten weergeven

De API van de catalogus bevat informatie over gepubliceerde entiteiten aan de AI-galerie van Azure die u kunt ook weergeven rechtstreeks op de [AI galerie website](https://gallery.azure.ai/). 

Gepubliceerde entiteiten weergeven, gaat u naar de volgende URL vervangen `[AuthorId]` met de auteur-ID die is verkregen in [een auteur-ID ophalen](#get-an-author-ID) hierboven.

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '[AuthorId]'

Bijvoorbeeld:

    https://catalog.cortanaanalytics.com/entities?$filter=author/id eq '99F1F5C6260295F1078187FA179FBE08B618CB62129976F09C6AF0923B02A5BA'

### <a name="view-unlisted-and-public-entities"></a>Niet-vermelde en openbare entiteiten weergeven

Deze query geeft alleen openbare entiteiten. Als u wilt weergeven van alle entiteiten, inclusief niet-vermelde, toegang geven token verkregen uit de vorige sectie.

1.  Met behulp van een hulpprogramma zoals [Postman](https://www.getpostman.com), maak een HTTP GET-aanvraag naar de catalogus-URL, zoals beschreven in [ophalen van uw toegangstoken](#get-your-access-token).
2.  Maken van een HTTP-aanvraag-header aangeroepen `DataLabAccessToken`, met de waarde die is ingesteld op het toegangstoken.
3.  De HTTP-aanvraag indienen.

> [!TIP]
> Als niet-vermelde entiteiten niet in de antwoorden van de API-catalogus weergegeven worden, wordt de gebruiker mogelijk een ongeldige of verlopen toegangstoken. Meld u af bij de AI-galerie van Azure en Herhaal de stappen in [ophalen van uw toegangstoken](#get-your-access-token) om het token te verlengen. 

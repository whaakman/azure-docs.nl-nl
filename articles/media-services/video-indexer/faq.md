---
title: Veelgestelde vragen over Video Indexer - Azure
titlesuffix: Azure Media Services
description: Krijg antwoorden op veelgestelde vragen over Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 12/19/2018
ms.author: juliako
ms.openlocfilehash: 972858b4bae995b6e9073cf7ee451a317e9f3909
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731114"
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen

In dit artikel vindt u antwoorden op veelgestelde vragen over Video Indexer.

## <a name="general-questions"></a>Algemene vragen

### <a name="what-is-video-indexer"></a>Wat is Video Indexer?

Video Indexer is een service voor het extraheren van metagegevens voor audio en video mediabestanden van Azure Media Services. Een grote verscheidenheid aan machine learning-algoritmen gebruikt om te gemakkelijk te classificeren, analyseren en inzicht in de video en audio-inhoud met behulp van [vooraf gedefinieerde modellen](video-indexer-overview.md). U kunt deze inzichten gebruiken op tal van manieren, zoals het verbeteren van de inhoud vindbaarheid en toegankelijkheid, het maken van nieuwe kansen in de verdiensten of gebouw nieuwe ervaringen die gebruikmaken van de inzichten.

Video Indexer biedt een webinterface voor het testen, configuratie en aanpassing en een REST-API voor ontwikkelaars om te integreren in het productiesysteem.

### <a name="what-can-i-do-with-video-indexer"></a>Wat kan ik doen met Video Indexer

Video Indexer kunt de volgende typen bewerkingen op media-bestanden uitvoeren:

* Identificeren en extraheren van spraak en identificeren van luidsprekers.
* Identificeren en op het scherm uit te pakken tekst in een video.
* Identificeren en labelen van objecten in een videobestand.
* Identificeer merken zoals Microsoft inhuren van audionummers en op het scherm tekst in een video.
* Detecteren en te herkennen gezichten in een database van beroemdheden en een gebruiker gedefinieerde database van gezichten wordt uitgevoerd.
* Trefwoorden extraheren uit de audio en video-inhoud op basis van gesproken en visuele tekst.
* Onderwerpen besproken, maar niet per se expliciet wordt vermeld in de audio en video-inhoud op basis van gesproken en visuele tekst extraheren.
* Ondertitels maken vanuit de audiotrack.

Zie [Overzicht](video-indexer-overview.md) voor meer informatie.

### <a name="how-do-i-get-started-with-video-indexer"></a>Hoe ga ik aan de slag met Video Indexer?

Video Indexer is gratis proberen. De gratis proefversie biedt u 600 minuten in de webinterface en 2.400 minuten via de API.

Index video's en audiobestanden op schaal, u kunt Video Indexer verbinding maken met een betaald Microsoft Azure-abonnement. U vindt meer informatie over prijzen op de [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) pagina.

U vindt meer informatie over aan de slag in [aan de slag](video-indexer-get-started.md).

### <a name="do-i-need-coding-skills-to-use-video-indexer"></a>Moet ik programmeren voor het gebruik van Video Indexer?

U kunt de Video Indexer-API's integreren in uw pijplijn, of u kunt de Video Indexer-Portal te gebruiken en geen code te schrijven op alle nodig. 

### <a name="do-i-need-machine-learning-skills-to-use-video-indexer"></a>Moet ik machine learning-vaardigheden aan Video Indexer gebruiken?

Nee, u kunt uitpakken inzichten uit uw video en audio-inhoud volledig zonder een machine learning-vaardigheden en kennis over algoritmen. 

### <a name="what-media-formats-does-video-indexer-support"></a>Welke media-indelingen biedt ondersteuning voor Video Indexer?

Video Indexer biedt ondersteuning voor meest voorkomende media-indelingen. Raadpleeg de lijst met Azure Media Encoder standard indelingen voor meer informatie.

### <a name="how-to-do-i-upload-a-media-into-video-indexer"></a>Hoe wil ik een medium naar Video Indexer uploaden?

In de Video Indexer webportal, kunt u een mediabestand met behulp van het dialoogvenster voor het uploaden van bestand kunt uploaden of door die verwijst naar een URL die rechtstreeks Hiermee geeft u het bestand. Bijvoorbeeld `http://contoso.cloudapp.net/videos/example.mp4`. Een koppeling naar een pagina met een videospeler zoals `http://contoso.cloudapp.net/videos` werkt niet. De Video Indexer-API moet u het invoerbestand via een URL of een matrix van bytes opgeven. Houd er rekening mee dat via een URL-uploads zijn beperkt tot 10 GB, maar nog geen een duur van de termijn. Voor meer informatie raadpleegt u dit [gebruiksaanwijzing](upload-index-videos.md).

### <a name="how-long-does-it-take-visual-indexer-to-extract-insights-from-media"></a>Hoe lang duurt Visual indexeerfunctie voor het extraheren van inzichten uit media?

De hoeveelheid tijd die nodig is om te indexeren van een video of audio-bestand, beide met behulp van de Video Indexer-API en de Video Indexer webinterface, is afhankelijk van meerdere parameters op, zoals de bestandslengte en kwaliteit, het aantal gevonden in het bestand, het aantal reken-en inzichten koppelt zijn beschikbaar, netwerk en of het streaming-eindpunt is ingeschakeld of niet. Voor meest inhoudstypen ervan uitgaande dat 10 S3-ME zijn ingeschakeld, wordt voorspeld dat indexering van 1/3 naar ½ van de lengte van de video gaat.  We raden echter aan dat u een aantal testbestanden met uw eigen inhoud uitvoeren en een gemiddelde om een beter idee te krijgen. 

### <a name="in-which-azure-regions-is-video-indexer-available"></a>In welke Azure-regio's is de Video indexer beschikbaar?

U kunt zien welke Azure-regio's Video Indexer is beschikbaar op de [regio's](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all) pagina.

### <a name="what-is-the-sla-for-video-indexer"></a>Wat is de SLA voor Video Indexer?

De gratis proefversie voor Video Indexer heeft geen SLA. Van Azure mediaservice SLA behandelt Video Indexer.

U kunt de informatie vinden op de [SLA](https://azure.microsoft.com/support/legal/sla/media-services/v1_0/) pagina.

## <a name="billing-questions"></a>Vragen over facturering

### <a name="how-much-does-video-indexer-cost"></a>Wat kost Video Indexer?

Video Indexer maakt gebruik van een eenvoudige betalen voor wat u tariefmodel gebaseerd op de duur van de invoer van inhoud gebruikt. Extra kosten in rekening kunnen passen voor codering, streaming, opslag, netwerkgebruik en media gereserveerde eenheden. Voor de huidige prijzen, raadpleegt u de [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) pagina.

### <a name="does-video-indexer-offer-a-free-trial"></a>Biedt Video Indexer een gratis proefversie?

Video Indexer biedt Ja, een gratis proefversie aan die wordt gehost in de oostelijke VS Azure-regio en biedt volledige service en de API-functionaliteit. Er is een quotum van 10 uur video's voor gebruikers van de website en 40 uur voor API-gebruikers. 

## <a name="security-questions"></a>Beveiligingsvragen

### <a name="are-audio-and-video-files-indexed-by-video-indexer-stored"></a>Audio-of videobestanden door worden geïndexeerd Video Indexer opgeslagen?

Ja, tenzij u het bestand uit de Video Indexer mogelijk maken via de website van de Video Indexer of de API verwijderen, uw video en audio-bestanden worden opgeslagen. Voor bestanden die u hebt de gratis proefversie, video en audio worden index opgeslagen in de Azure-regio VS-Oost. Anders wordt worden de video en audio-bestanden opgeslagen in de storage-account van uw Azure-abonnement.

### <a name="can-i-delete-my-files-that-are-stored-in-video-indexer-portal"></a>Kan ik mijn bestanden die zijn opgeslagen in Video Indexer Portal verwijderen?

Ja, u kunt altijd verwijderen uw video en audio-bestanden, evenals hun inzichten uit Video Indexer. Nadat u de bestanden hebt verwijderd, is dit definitief uit Video Indexer en waar Video Indexer (Azure-regio VS-Oost voor proefaccounts en de storage-account van uw Azure-abonnement anders) van het bestand hebt opgeslagen.

### <a name="who-has-access-to-my-audio-and-video-files-and-that-have-been-indexed-andor-stored-by-video-indexer"></a>Wie toegang heeft tot mijn audio en video-bestanden en die hebben is geïndexeerd en/of opgeslagen door Video Indexer?

Ja, u kunt altijd verwijderen uw video en audio-bestanden, evenals hun inzichten uit Video Indexer. Nadat u de bestanden hebt verwijderd, is dit definitief uit Video Indexer en waar Video Indexer (Azure-regio VS-Oost voor proefaccounts en de storage-account van uw Azure-abonnement anders) van het bestand hebt opgeslagen.

### <a name="can-i-control-user-access-to-my-video-indexer-account"></a>Kan ik gebruikerstoegang beheren op mijn Video Indexer-Account?

Ja, selecteert u die is uitgenodigd voor het account.

### <a name="who-has-access-to-the-insights-that-were-extracted-from-my-audio-and-video-files-by-video-indexer"></a>Wie heeft toegang tot de inzichten die zijn opgehaald uit mijn audio en video-bestanden door Video Indexer?

Uw video's met openbare als de privacyinstelling zijn toegankelijk voor iedereen die de koppeling naar uw video en de inzichten is. Uw video's met particuliere als de privacyinstelling van de kunnen alleen worden geopend door gebruikers die zijn uitgenodigd voor het account van de video.

### <a name="do-i-still-own-my-content-that-have-been-indexed-and-stored-by-video-indexer"></a>Ik mijn inhoud die zijn geïndexeerd en die zijn opgeslagen door Video Indexer nog steeds eigenaar?

Ja, per de [voorwaarden voor onlineservices Azure](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), eigenaar van uw inhoud.

### <a name="is-the-content-indexed-by-video-indexer-kept-within-the-azure-region-where-i-am-using-video-indexer"></a>Is de inhoud geïndexeerd door Video Indexer binnen de Azure-regio waar ik Video Indexer gebruik gehouden?

Ja, de inhoud en de inzichten die zullen worden bewaard in de Azure-regio, tenzij u een handmatige configuratie hebt in uw Azure-abonnement dat gebruikmaakt van meerdere Azure-regio's. 

### <a name="what-is-the-privacy-policy-for-video-indexer"></a>Wat is de privacyverklaring voor Video Indexer?

Video Indexer wordt gedekt door de [privacyverklaring van Microsoft](https://privacy.microsoft.com/privacystatement).

## <a name="api-questions"></a>API-vragen

### <a name="what-are-the-differences-in-capability-on-the-video-indexer-website-versus-the-video-indexer-api"></a>Wat zijn de verschillen in functionaliteit op de website Video Indexer ten opzichte van de Video Indexer-API?

Met de [Video Indexer](https://www.videoindexer.com) website, u kunt zoeken naar uw videobibliotheek, insights analyseren modellen aanpassen met, configureren van uw account en uw video's eenvoudig bewerken. Met de robuuste [REST-API voor Video Indexer](https://api-portal.videoindexer.ai/), u kunt integreren met een infrastructuur of widgets rechtstreeks insluiten in uw eigen website of toepassing.

### <a name="what-apis-does-video-indexer-offer"></a>Welke API's wordt aangeboden door Video Indexer?

U vindt informatie over het gebruik van de Video Indexer-API's op de [Video Indexer-Portal voor ontwikkelaars](https://api-portal.videoindexer.ai/)

### <a name="how-do-i-get-started-with-video-indexers-api"></a>Hoe ga ik aan de slag met de Video Indexer-API?

Ga als volgt [zelfstudie: aan de slag met Video Indexer-API](video-indexer-use-apis.md).

### <a name="what-is-an-access-token"></a>Wat is een toegangstoken?

De Video Indexer-API bevat een autorisatie-API en een API-bewerkingen. De autorisaties-API-aanroepen die je bevat toegangstoken. Elke aanroep naar de Operations-API moet worden gekoppeld aan een toegangstoken dat overeenkomt met het autorisatiebereik van de aanroep.

### <a name="what-is-the-difference-between-account-access-token-user-access-token-and-video-access-token"></a>Wat is het verschil tussen het toegangstoken voor Account, token voor gebruikerstoegang en Video-toegangstoken?

Deze vertegenwoordigen het bereik van de autorisatie van een aanroep.

* Het gebruikersniveau van de - toegangsniveau gebruikerstokens kunnen u bewerkingen uitvoeren op het gebruikersniveau van de. Hiermee kunt u bijvoorbeeld gekoppelde accounts ophalen.
* Accountniveau – account toegangsniveau van de tokens kunnen u bewerkingen uitvoeren op het accountniveau of de video. Hiermee kunt u bijvoorbeeld een video uploaden, een lijst maken met alle video's, inzichten krijgen in video's, enzovoort.
* Videoniveau – toegangsniveau van de video-tokens kunnen u bewerkingen uitvoeren op een specifieke video. Hiermee kunt u bijvoorbeeld inzichten in een video krijgen, ondertiteling downloaden, widgets ophalen, enzovoort.

U kunt bepalen of deze tokens alleen-lezen zijn of ze bewerken door op te geven bewerken toestaan = true/false.

### <a name="why-do-i-need-access-tokens-when-using-the-video-indexer-apis"></a>Waarom moet ik toegangstokens bij het gebruik van de Video Indexer-API's?

Toegangstokens zijn nodig om de Video Indexer-API's gebruiken om veiligheidsredenen. Dit zorgt ervoor dat alle aanroepen zijn afkomstig van u of gebruikers die beschikken over machtigingen voor toegang tot uw account. 

### <a name="how-often-do-i-need-to-get-a-new-access-token-when-do-access-tokens-expire"></a>Hoe vaak moet ik een nieuw toegangstoken ophalen? Wanneer toegangstokens verlopen?

Access tokens verlopen elk uur, dus u moet voor het genereren van een nieuw toegangstoken elk uur. 

## <a name="next-steps"></a>Volgende stappen

[Overzicht](video-indexer-overview.md)

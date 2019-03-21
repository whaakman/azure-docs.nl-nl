---
title: Veelgestelde vragen over Video Indexer - Azure
titlesuffix: Azure Media Services
description: Krijg antwoorden op veelgestelde vragen over Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: e85beea4250b5ac5f4defd0b918786079d3dbe3e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57892656"
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen

In dit artikel vindt u antwoorden op veelgestelde vragen over Video Indexer.

## <a name="general-questions"></a>Algemene vragen

### <a name="what-is-video-indexer"></a>Wat is Video Indexer?

Video Indexer is een kunstmatige intelligentie-service die deel uitmaakt van Microsoft Azure Media Services. Video Indexer biedt een indeling van meerdere machine learning-modellen waarmee u eenvoudig diepgaand inzicht ophalen uit een video. Geavanceerde en nauwkeurige om inzichten te krijgen, Video Indexer maakt gebruik van meerdere kanalen van de video: audio-, spraak- en visueel element. Video Indexer-inzichten kunnen worden gebruikt op tal van manieren, zoals het verbeteren van de inhoud vindbaarheid en toegankelijkheid, het maken van nieuwe verdiensten kansen of gebouw nieuwe ervaringen die gebruikmaken van de inzichten. Video Indexer biedt een webinterface voor het testen, configuratie en aanpassing van modellen in uw account. Ontwikkelaars kunnen een REST-API gebruiken om te integreren met Video Indexer productiesysteem. 

### <a name="what-can-i-do-with-video-indexer"></a>Wat kan ik doen met Video Indexer

Enkele van de bewerkingen die Video Indexer op de media-bestanden uitvoeren kunt zijn:

* Identificeren van en het ophalen van spraak en identificeren van luidsprekers.
* Identificeren en op het scherm extraheren tekst in een video.
* Detecteren van objecten in een videobestand.
* Identificeren van merken (bijvoorbeeld: Microsoft) van audionummers en op het scherm de tekst in een video.
* Detecteren van en het herkennen van gezichten in een database van beroemdheden en een gebruiker gedefinieerde database van gezichten wordt uitgevoerd.
* Uitpakken van onderwerpen besproken, maar niet noodzakelijkerwijs die worden vermeld in de audio en video-inhoud.
* Het maken van ondertitels uit de audiotrack.

Zie voor meer informatie en meer Video Indexer-functies, [overzicht](video-indexer-overview.md).

### <a name="how-do-i-get-started-with-video-indexer"></a>Hoe ga ik aan de slag met Video Indexer?

Video Indexer bevat een gratis proefversie waarmee u met 600 minuten in de webinterface en 2.400 minuten via de API. U kunt [Meld u aan bij de webinterface van Video Indexer](https://www.videoindexer.ai/) en probeer het zelf met behulp van een web-id en zonder het instellen van een Azure-abonnement. 

Index video's en audiobestanden op schaal, u kunt Video Indexer verbinding maken met een betaald Microsoft Azure-abonnement. U vindt meer informatie over prijzen op de [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) pagina.

U vindt meer informatie over aan de slag in [aan de slag](video-indexer-get-started.md).

### <a name="do-i-need-coding-skills-to-use-video-indexer"></a>Moet ik programmeren voor het gebruik van Video Indexer?

U kunt de webinterface van Video Indexer gebruiken om te evalueren, configureren en beheren van uw account met **geen codering vereist**.  Wanneer u klaar om complexere toepassingen te ontwikkelen bent, kunt u de [Video Indexer-API](https://api-portal.videoindexer.ai/) Video Indexer integreren in uw eigen toepassingen, websites, of [aangepaste werkstromen met behulp van serverloze technologieën, zoals Azure Logic Apps](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/) of Azure Functions.

### <a name="do-i-need-machine-learning-skills-to-use-video-indexer"></a>Moet ik machine learning-vaardigheden aan Video Indexer gebruiken?

Video Indexer biedt Nee, de integratie van meerdere machine learning-modellen in één pijplijn. Indexeren van een video of audio-bestand via Video Indexer, haalt een volledige set met inzichten zonder een machine learning-vaardigheden en kennis over de algoritmen die nodig zijn voor een bepaald deel van de klant heeft uitgepakt op één gedeelde tijdlijn.

### <a name="what-media-formats-does-video-indexer-support"></a>Welke media-indelingen biedt ondersteuning voor Video Indexer?

Video Indexer biedt ondersteuning voor meest voorkomende media-indelingen. Raadpleeg de [Azure Media Encoder standard indelingen](https://docs.microsoft.com/azure/media-services/latest/media-encoder-standard-formats) lijst voor meer informatie.

### <a name="how-to-do-i-upload-a-media-into-video-indexer"></a>Hoe wil ik een medium naar Video Indexer uploaden?

In Video Indexer web gebaseerde portal, een mediabestand met behulp van het dialoogvenster voor het uploaden van bestand kunt uploaden of u door die verwijst naar een URL die rechtstreeks als host fungeert voor de bron-bestand (Zie [voorbeeld](https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4)). Een URL op die hosts de media inhoud met een iFrame of invoegcode niet werkt (Zie [voorbeeld](https://www.videoindexer.ai/accounts/7e1282e8-083c-46ab-8c20-84cae3dc289d/videos/5cfa29e152/?t=4.11)). De Video Indexer-API moet u het invoerbestand via een URL of een matrix van bytes opgeven. Via een URL met behulp van de API-uploads zijn beperkt tot 10 GB, maar nog geen een duur van de termijn. Voor meer informatie raadpleegt u dit [gebruiksaanwijzing](https://docs.microsoft.com/azure/media-services/video-indexer/upload-index-videos).

### <a name="how-long-does-it-take-video-indexer-to-extract-insights-from-media"></a>Hoe lang duurt Video Indexer inzichten ophalen uit media?

De hoeveelheid tijd die nodig is om te indexeren van een video of audio-bestand, beide met behulp van de Video Indexer-API en de Video Indexer webinterface, is afhankelijk van meerdere parameters op, zoals de bestandslengte van het en de kwaliteit, het aantal insights gevonden in het bestand, het aantal [gereserveerde eenheden](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview) beschikbaar is, en of de [streaming-eindpunt](https://docs.microsoft.com/azure/media-services/previous/media-services-streaming-endpoints-overview) of niet is ingeschakeld. U wordt aangeraden dat u een aantal testbestanden met uw eigen inhoud uitvoeren en een gemiddelde om een beter idee te krijgen.

### <a name="can-i-create-customized-workflows-to-automate-processes-with-video-indexer"></a>Kan ik aangepaste werkstromen voor het automatiseren van processen met Video Indexer maken?

Ja, u Video Indexer kunt integreren in de serverloze technologieën zoals Logic Apps, Flow en [Azure Functions](https://azure.microsoft.com/services/functions/). U kunt meer informatie vinden op de [logische App](https://azure.microsoft.com/services/logic-apps/) en [Flow](https://flow.microsoft.com/en-us/) connectors voor Video Indexer [hier](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/). 

### <a name="in-which-azure-regions-is-video-indexer-available"></a>In welke Azure-regio's is de Video indexer beschikbaar?

U kunt zien welke Azure-regio's Video Indexer is beschikbaar op de [regio's](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all) pagina.

### <a name="what-is-the-sla-for-video-indexer"></a>Wat is de SLA voor Video Indexer?

SLA voor Azure Media Service van meer informatie over Video Indexer en kunt u vinden op de [SLA](https://azure.microsoft.com/support/legal/sla/media-services/v1_2/) pagina. De SLA wordt alleen toegepast op Video Indexer rekeningen betalen en geldt niet voor de gratis proefversie.

## <a name="privacy-questions"></a>Vragen over privacy

### <a name="are-video-and-audio-files-indexed-by-video-indexer-stored"></a>Worden video-en audiobestanden geïndexeerd door Video Indexer opgeslagen?

Ja, tenzij u het bestand uit de Video Indexer verwijderen, hetzij met behulp van de Video Indexer-website of de API, worden uw video en audio-bestanden opgeslagen. Voor de gratis proefversie, video en audio-bestanden die u moet index worden opgeslagen in de Azure-regio VS-Oost. Anders worden uw video en audio-bestanden opgeslagen in de storage-account van uw Azure-abonnement.

### <a name="can-i-delete-my-files-that-are-stored-in-video-indexer-portal"></a>Kan ik mijn bestanden die zijn opgeslagen in Video Indexer Portal verwijderen?

Ja, kunt u uw video en audio-bestanden, evenals alle metagegevens en inzichten die zijn geëxtraheerd uit door Video Indexer altijd verwijderen. Nadat u een bestand verwijderen uit de Video Indexer, het bestand en bijbehorende metagegevens en inzichten worden permanent verwijderd uit de Video Indexer. Als u uw eigen back-upoplossing hebt geïmplementeerd in Azure storage, moet het bestand blijft echter in uw Azure-opslag.

### <a name="can-i-control-user-access-to-my-video-indexer-account"></a>Kan ik gebruikerstoegang beheren op mijn Video Indexer-account?

Ja, alleen de accountbeheerders kunt uitnodigen en uitnodiging van personen bij hun account, evenals wie heeft bevoegdheden bewerken en wie alleen-lezen toegang heeft toewijzen.

### <a name="who-has-access-to-my-video-and-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>Wie heeft toegang tot Mijn video en audio-bestanden die zijn is geïndexeerd en/of opgeslagen door Video Indexer en de metagegevens en inzichten die zijn opgehaald?

Uw video of audio-inhoud met openbare als de privacyinstelling zijn toegankelijk voor iedereen die de koppeling naar uw video of audio-inhoud en de inzichten is. Uw video of audio-inhoud privé als de privacyinstelling waarvoor alleen toegankelijk door gebruikers die zijn uitgenodigd voor het account van de video of audio-inhoud. De privacyinstelling van uw inhoud geldt ook voor de metagegevens en inzichten aan Video Indexer worden geëxtraheerd. U kunt de privacyinstelling toewijzen tijdens het uploaden van uw video of audio-bestand. U kunt ook de privacyinstelling na indexeren wijzigen.

### <a name="what-access-does-microsoft-have-to-my-video-or-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>Welke toegang beschikt Microsoft over naar Mijn video- of audio-bestanden die zijn is geïndexeerd en/of opgeslagen door Video Indexer en de metagegevens en inzichten die zijn opgehaald

Per de [voorwaarden voor onlineservices Azure](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) (OST), volledig eigenaar van uw inhoud en Microsoft alleen toegang tot uw inhoud en de metagegevens en inzichten die Video Indexer uit uw inhoud op basis van het OST-bestand en de Microsoft ophaalt Privacyverklaring.

### <a name="are-the-custom-models-that-i-build-in-my-video-indexer-account-available-to-other-accounts"></a>Zijn de aangepaste modellen die ik bouw in mijn Video Indexer-account beschikbaar voor andere accounts?

 Nee, de aangepaste modellen die u in uw account maakt zijn niet beschikbaar voor een ander account. Video Indexer op dit moment kunt u aangepaste [merken](customize-brands-model-overview.md), [taal](customize-language-model-overview.md), en [persoon](customize-person-model-overview.md) modellen in uw account. Deze modellen zijn alleen beschikbaar in het account waarin u de modellen hebt gemaakt.
  
### <a name="is-the-content-indexed-by-video-indexer-kept-within-the-azure-region-where-i-am-using-video-indexer"></a>Is de inhoud geïndexeerd door Video Indexer binnen de Azure-regio waar ik Video Indexer gebruik gehouden?

Ja, de inhoud en de inzichten worden bewaard in de Azure-regio, tenzij u een handmatige configuratie hebt in uw Azure-abonnement dat gebruikmaakt van meerdere Azure-regio's. 

### <a name="what-is-the-privacy-policy-for-video-indexer"></a>Wat is de privacyverklaring voor Video Indexer?

Video Indexer wordt gedekt door de [privacyverklaring van Microsoft](https://privacy.microsoft.com/privacystatement). De privacyverklaring wordt uitgelegd dat de persoonlijke gegevens die Microsoft uit hoe Microsoft verwerkt en voor welke doelen verwerkt Microsoft. Voor meer informatie over privacy, gaat u naar de [Microsoft Trust Center](https://www.microsoft.com/trustcenter).

### <a name="what-certifications-does-video-indexer-have"></a>Welke certificeringen heeft Video Indexer?

Video Indexer heeft momenteel de SOC-certificering. Als u wilt controleren van Video Indexer-certificering, Raadpleeg de [Microsoft Trust Center](https://www.microsoft.com/trustcenter/compliance/complianceofferings?product=Azure).

## <a name="api-questions"></a>API-vragen

### <a name="what-apis-does-video-indexer-offer"></a>Welke API's wordt aangeboden door Video Indexer?

Video Indexer-API's kunt indexeren, extraheren van metagegevens, beheer van middelen, vertaling, insluiten, aanpassing van modellen en nog veel meer. Ga voor meer gedetailleerde informatie over het gebruik van de Video Indexer-API, raadpleegt u de [Video Indexer-Ontwikkelaarsportal](https://api-portal.videoindexer.ai/).

### <a name="what-client-sdks-does-video-indexer-offer"></a>Welke client-SDK's wordt aangeboden door Video Indexer?

Er zijn momenteel geen client SDK's die worden aangeboden. De Video Indexer-team werkt aan de SDK's en wil ze snel te leveren.

### <a name="how-do-i-get-started-with-video-indexers-api"></a>Hoe ga ik aan de slag met de Video Indexer-API?

Ga als volgt [zelfstudie: aan de slag met Video Indexer-API](video-indexer-use-apis.md).

### <a name="what-is-the-difference-between-the-video-indexer-api-and-the-azure-media-service-v3-api"></a>Wat is het verschil tussen de Video Indexer-API en de Azure Media Service-API v3?

Er zijn momenteel enkele overlappingen in functies die worden aangeboden door de Video Indexer-API en de Azure Media Service-API v3. U vindt meer informatie over het vergelijken van beide services [hier](compare-video-indexer-with-media-services-presets.md).

### <a name="what-is-an-api-access-token-and-why-do-i-need-it"></a>Wat is een API-toegangstoken en waarom moet ik deze?

De Video Indexer-API bevat een autorisatie-API en een API-bewerkingen. De autorisaties-API-aanroepen die je bevat toegangstoken. Elke aanroep naar de Operations-API moet worden gekoppeld aan een toegangstoken dat overeenkomt met het autorisatiebereik van de aanroep.

Toegangstokens zijn nodig om de Video Indexer-API's gebruiken om veiligheidsredenen. Dit zorgt ervoor dat alle aanroepen zijn afkomstig van u of gebruikers die beschikken over machtigingen voor toegang tot uw account. 

### <a name="what-is-the-difference-between-account-access-token-user-access-token-and-video-access-token"></a>Wat is het verschil tussen het toegangstoken voor Account, token voor gebruikerstoegang en Video-toegangstoken?

* Accountniveau – account toegangsniveau van de tokens kunnen u bewerkingen uitvoeren op het accountniveau of de video. Bijvoorbeeld, een video uploaden, lijst met alle video's, inzichten in video's ophalen.
* Het gebruikersniveau van de - toegangsniveau gebruikerstokens kunnen u bewerkingen uitvoeren op het gebruikersniveau van de. Hiermee kunt u bijvoorbeeld gekoppelde accounts ophalen.
* Videoniveau – toegangsniveau van de video-tokens kunnen u bewerkingen uitvoeren op een specifieke video. Hiermee kunt u bijvoorbeeld inzichten in een video krijgen, ondertiteling downloaden, widgets ophalen, enzovoort.

### <a name="how-often-do-i-need-to-get-a-new-access-token-when-do-access-tokens-expire"></a>Hoe vaak moet ik een nieuw toegangstoken ophalen? Wanneer toegangstokens verlopen?

Access tokens verlopen elk uur, dus u moet voor het genereren van een nieuw toegangstoken elk uur. 

## <a name="billing-questions"></a>Vragen over facturering

### <a name="how-much-does-video-indexer-cost"></a>Wat kost Video Indexer?

Video Indexer maakt gebruik van een eenvoudige betalen per gebruik prijsmodel op basis van de duur van de inhoud invoer die u met de index. Extra kosten in rekening kunnen passen voor codering, streaming, opslag, netwerkgebruik en media gereserveerde eenheden. Zie voor meer informatie de [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) pagina.

### <a name="when-am-i-billed-for-using-video-indexer"></a>Wanneer Word ik gefactureerd voor het gebruik van Video Indexer?

Bij het verzenden van een video worden geïndexeerd, definieert de gebruiker het indexeren om video-analyse, audioanalyse of beide. Hierdoor kunt u bepalen welke SKU's wordt in rekening gebracht. Als er een kritieke fout tijdens de verwerking, wordt een foutcode worden geretourneerd als antwoord. In dat geval geen facturering vindt plaats.  Een kritieke fout kan zijn veroorzaakt door een fout in de code of heeft een kritieke fout in een interne afhankelijkheid van de service. Fouten, zoals onjuiste-id of het inzicht extractie worden niet beschouwd als kritiek en een antwoord wordt geretourneerd. Wanneer een reactie geldig (niet-foutcode) wordt geretourneerd, wordt in elk geval facturering plaatsvindt.
 
### <a name="does-video-indexer-offer-a-free-trial"></a>Biedt Video Indexer een gratis proefversie?

Ja, Video Indexer biedt een gratis proefversie waarmee u de volledige-service en de API-functionaliteit. Er is een quotum van 600 minuten video's voor gebruikers webgebaseerde interface en 2.400 minuten voor API-gebruikers. 

## <a name="next-steps"></a>Volgende stappen

[Overzicht](video-indexer-overview.md)

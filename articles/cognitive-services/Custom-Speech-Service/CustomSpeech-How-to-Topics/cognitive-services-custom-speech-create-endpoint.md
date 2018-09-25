---
title: Maak een aangepaste spraak-eindpunt met Custom Speech Service op Azure | Microsoft Docs
description: Informatie over het maken van een aangepaste spraak-naar-tekst-eindpunt met de Custom Speech Service in Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: ed93afa8e10fdfbb0d45f4500b4a648716e25e00
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46952219"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Een aangepast spraak naar tekst-eindpunt maken
Nadat u hebt aangepaste akoestische modellen of taalmodellen die zijn gemaakt, kunt u ze kunt implementeren in een aangepast eindpunt voor spraak-naar-tekst. 

## <a name="create-an-endpoint"></a>Een eindpunt maken
Voor het maken van een nieuw aangepast eindpunt selecteert **implementaties** op de **aangepaste spraak** menu aan de bovenkant van de pagina. Deze actie gaat u naar de **implementaties** bevat een tabel met de huidige aangepaste eindpunten. Als u geen eindpunten nog niet hebt gemaakt, wordt de tabel leeg is. De huidige landinstelling wordt weergegeven in de titel van de tabel. 

Voor het maken van een implementatie voor een andere taal selecteert **wijziging landinstelling**. Zie voor meer informatie over ondersteunde talen, [landinstellingen worden ondersteund in de Custom Speech Service](cognitive-services-custom-speech-change-locale.md).

Voor het maken van een nieuw eindpunt selecteert **nieuw**. In de **implementatie maken** in het deelvenster gegevens invoeren in de **naam** en **beschrijving** vakken van uw aangepaste implementatie.

In de **abonnement** keuzelijst met invoervak voor vak, selecteer het abonnement dat u wilt gebruiken. Als het een S2-abonnement, kunt u schaaleenheden en registratie van inhoud. Zie voor meer informatie over schaaleenheden en logboekregistratie [Custom Speech Service meters en quota's](../cognitive-services-custom-speech-meters.md).

De volgende tabel ziet u hoe schaaleenheden toewijzen aan beschikbare gelijktijdige aanvragen:

| Schaaleenheid | Aantal gelijktijdige aanvragen |
| ------ | ----- |
| 0 | 1 |
| 1 | 5 |
| 2 | 10 |
| 3 | 15 |
| n | 5 * n |

U kunt ook selecteren of de registratie van inhoud is overgeschakeld in- of uitschakelen. Dat wil zeggen, selecteert u of het verkeer van het eindpunt wordt opgeslagen voor intern gebruik van Microsoft. Als dit niet is geselecteerd, wordt het verkeer opslaan onderdrukt. Het onderdrukken van registratie van inhoud leidt tot extra kosten. Raadpleeg de [informatiepagina met prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) voor meer informatie.

> [!NOTE]
> Registratie van inhoud heet 'Geen tracering' op de pagina met prijzen.
>


Microsoft biedt bovendien een ruwe schatting van de kosten, zodat u zich bewust bent van de impact op de kosten van schaaleenheden en registratie van inhoud. Deze schatting is een ruwe schatting en kan verschillen van de werkelijke kosten.

> [!NOTE]
> Deze instellingen zijn niet beschikbaar voor abonnementen van F0 (gratis laag).
>

In de **akoestisch Model** , selecteert u het akoestisch model dat u wilt, en klik in de **taalmodel** , selecteert u het taalmodel die u wilt. De opties voor akoestische en modellen worden altijd de base modellen van Microsoft bevatten. De selectie van het basismodel beperkt de combinaties. U kunt geen conversatie base modellen combineren met zoeken en dicteren base modellen.

![De pagina implementatie maken](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-create2.png)

> [!NOTE]
> Zorg dat u akkoord met de voorwaarden van het gebruik en informatie over de prijzen door het selectievakje te selecteren.
>

Nadat u uw modellen akoestische en hebt geselecteerd, selecteert u **maken**. Deze actie gaat u terug naar de **implementaties** pagina. De tabel bevat nu een vermelding die overeenkomt met het nieuwe eindpunt. Status van het eindpunt weerspiegelt de huidige status, terwijl deze wordt gemaakt. Het kan instantiëren van een nieuw eindpunt met uw aangepaste modellen tot 30 minuten duren. Wanneer de status van de implementatie is *voltooid*, het eindpunt is gereed voor gebruik.

![De pagina implementaties](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-ready.png)

Wanneer de implementatie gereed is, wordt de implementatienaam van de van een koppeling. Als u de koppeling wordt weergegeven de **informatie over de implementatie** pagina, waarin de URL's van uw aangepast eindpunt voor gebruik met zowel een HTTP-aanvraag of de Microsoft Cognitive Services Speech Client-bibliotheek, die maakt gebruik van websockets.

![De pagina implementatie-informatie](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-info2.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer zelfstudies:
* [Gebruik een aangepaste spraak-naar-tekst-eindpunt](cognitive-services-custom-speech-use-endpoint.md)
* [Een aangepast akoestisch model maken](cognitive-services-custom-speech-create-acoustic-model.md)
* [Een aangepaste taalmodel maken](cognitive-services-custom-speech-create-language-model.md)

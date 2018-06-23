---
title: Maak een aangepaste spraak-eindpunt met aangepaste spraak-Service op Azure | Microsoft Docs
description: Informatie over het maken van een aangepaste spraak naar tekst-eindpunt met de aangepaste spraak-Service in cognitieve Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ms.openlocfilehash: 99bc275db1f0c1b45b3db440d2e03d0db9ab5cf6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344491"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Maak een aangepaste spraak naar tekst-eindpunt
Nadat u aangepaste akoestisch modellen of modellen taal hebt gemaakt, kunt u ze kunt implementeren in een aangepaste spraak naar tekst-eindpunt. 

## <a name="create-an-endpoint"></a>Een eindpunt maken
Voor het maken van een nieuwe aangepaste eindpunt selecteert **implementaties** op de **aangepaste spraak** menu aan de bovenkant van de pagina. Deze actie gaat u naar de **implementaties** bevat een tabel met de huidige aangepaste eindpunten. Als u nog geen eindpunten hebt gemaakt, wordt de tabel leeg is. De huidige landinstelling wordt doorgevoerd in de titel van de tabel. 

U maakt een implementatie voor een andere taal selecteren **wijziging landinstelling**. Zie voor meer informatie over ondersteunde talen [landinstellingen ondersteund in aangepaste spraak Service](cognitive-services-custom-speech-change-locale.md).

Voor het maken van een nieuw eindpunt selecteert **nieuw**. In de **implementatie maken** deelvenster, voert u gegevens in de **naam** en **beschrijving** vakken van uw aangepaste implementatie.

In de **abonnement** keuzelijst met invoervak Selecteer het abonnement dat u wilt gebruiken. Als het een S2-abonnement, kunt u schaaleenheden en inhoud logboekregistratie. Zie voor meer informatie over schaaleenheden en logboekregistratie [aangepaste spraak Service meters en quota's](../cognitive-services-custom-speech-meters.md).

De volgende tabel ziet u hoe schaaleenheden toewijzen aan beschikbare gelijktijdige aanvragen:

| Schaaleenheid | Aantal gelijktijdige aanvragen |
| ------ | ----- |
| 0 | 1 |
| 1 | 5 |
| 2 | 10 |
| 3 | 15 |
| n | 5 * n |

U kunt ook selecteren of inhoud logboekregistratie is ingeschakeld in- of uitschakelen. Dat wil zeggen, selecteert u of de endpoint-verkeer wordt opgeslagen voor intern gebruik van Microsoft. Als deze niet is geselecteerd, wordt het opslaan van het verkeer onderdrukt. Het onderdrukken van inhoud logboekregistratie resulteert in extra kosten. Raadpleeg de [informatiepagina met prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/) voor meer informatie.

> [!NOTE]
> Inhoud logboekregistratie heet 'No-tracering' op de pagina met prijzen.
>


Microsoft biedt bovendien een ruwe schatting van de kosten, zodat u zich bewust bent van de gevolgen voor de kosten van schaaleenheden en inhoud logboekregistratie. Deze schatting is een ruwe schatting en afwijken van de werkelijke kosten.

> [!NOTE]
> Deze instellingen zijn niet beschikbaar voor abonnementen van F0 (gratis laag).
>

In de **akoestisch Model** , selecteert u het akoestisch model dat u wilt, en in de **taalmodel** , selecteert u de gewenste taalmodel. De opties voor acoustic en taal-modellen bevatten altijd de base Microsoft-modellen. De selectie van het basismodel beperkt de combinaties. U kunt geen eigen base modellen combineren met search of dicteren base modellen.

![De pagina implementatie maken](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-create2.png)

> [!NOTE]
> Zorg dat u akkoord met de voorwaarden van het gebruik en de prijsgegevens door het selectievakje in.
>

Nadat u uw modellen acoustic en taal hebt geselecteerd, selecteert u **maken**. Deze actie terugkeert naar de **implementaties** pagina. De tabel bevat nu een vermelding die overeenkomt met het nieuwe eindpunt. De status van het eindpunt weerspiegelt de huidige status, terwijl deze wordt gemaakt. Het kan duren voordat het instantiëren van een nieuw eindpunt met uw aangepaste modellen tot 30 minuten. Wanneer de status van de implementatie is *Complete*, het eindpunt is gereed voor gebruik.

![De pagina implementaties](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-ready.png)

Wanneer de implementatie klaar is, wordt de implementatienaam van de van een koppeling. Als u de koppeling wordt weergegeven de **implementatiegegevens** pagina, waarin de URL's van uw aangepaste eindpunt voor gebruik met ofwel een HTTP-aanvraag of de Microsoft cognitieve Services spraak-clientbibliotheek dat gebruikmaakt van websockets.

![De pagina implementatie-informatie](../../../media/cognitive-services/custom-speech-service/custom-speech-deployment-info2.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer zelfstudies:
* [Gebruik een aangepaste spraak naar tekst-eindpunt](cognitive-services-custom-speech-use-endpoint.md)
* [Een aangepaste akoestisch model maken](cognitive-services-custom-speech-create-acoustic-model.md)
* [Een aangepaste taalmodel maken](cognitive-services-custom-speech-create-language-model.md)

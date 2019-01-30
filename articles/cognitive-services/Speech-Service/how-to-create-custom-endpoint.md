---
title: Maak een aangepaste spraak-eindpunt met de Speech-Service op Azure | Microsoft Docs
description: Informatie over het maken van een aangepaste spraak-naar-tekst-eindpunt met de Speech-Service in Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 03/12/2018
ms.author: panosper
ms.openlocfilehash: 63d2c15c14340b0d7407b0144a8b8c75a2634e6a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220238"
---
# <a name="create-a-custom-speech-to-text-endpoint"></a>Een aangepast spraak naar tekst-eindpunt maken

Nadat u hebt aangepaste akoestische modellen of taalmodellen die zijn gemaakt, kunt u ze kunt implementeren in een aangepast eindpunt voor spraak-naar-tekst. 

## <a name="create-an-endpoint"></a>Een eindpunt maken
Voor het maken van een nieuw aangepast eindpunt selecteert **eindpunten** op de **aangepaste spraak** menu aan de bovenkant van de pagina. Deze actie gaat u naar de **eindpunten** bevat een tabel met de huidige aangepaste eindpunten. Als u geen eindpunten nog niet hebt gemaakt, wordt de tabel leeg is. De huidige landinstelling wordt weergegeven in de titel van de tabel. 

Voor het maken van een implementatie voor een andere taal selecteert **wijziging landinstelling**. Voor meer informatie over ondersteunde talen.

Voor het maken van een nieuw eindpunt selecteert **nieuw**. In de **Create Endpoint** in het deelvenster gegevens invoeren in de **naam** en **beschrijving** vakken van uw aangepaste implementatie.

In de **abonnement** keuzelijst met invoervak voor vak, selecteer het abonnement dat u wilt gebruiken. Als het een S0 (betalende)-abonnement, kunt u gelijktijdige aanvragen kunt selecteren.

U kunt ook selecteren of de registratie van inhoud is overgeschakeld in- of uitschakelen. Dat wil zeggen, selecteert u of het verkeer van het eindpunt wordt opgeslagen. Als dit niet is geselecteerd, wordt het verkeer opslaan onderdrukt. U kunt voor alle geregistreerde inhoud vinden downloadkoppelingen onder Endpoint Details weergeven ->

In de **akoestisch Model** , selecteert u het akoestisch model dat u wilt, en klik in de **taalmodel** , selecteert u het taalmodel die u wilt. De opties voor akoestische en modellen worden altijd de base modellen van Microsoft bevatten. De selectie van het basismodel beperkt de combinaties. U kunt geen conversatie base modellen combineren met zoeken en dicteren base modellen.

> [!NOTE]
> Zorg dat u akkoord met de voorwaarden van het gebruik en informatie over de prijzen door het selectievakje te selecteren.
>

Nadat u uw modellen akoestische en hebt geselecteerd, selecteert u **maken**. Deze actie gaat u terug naar de **implementaties** pagina. De tabel bevat nu een vermelding die overeenkomt met het nieuwe eindpunt. Status van het eindpunt weerspiegelt de huidige status, terwijl deze wordt gemaakt. Het kan instantiëren van een nieuw eindpunt met uw aangepaste modellen tot 30 minuten duren. Wanneer de status van de implementatie is *voltooid*, het eindpunt is gereed voor gebruik.

Wanneer de implementatie gereed is, wordt de naam van het eindpunt een koppeling. Als u de koppeling wordt weergegeven de **eindpuntgegevens** pagina, waarin de URL's van uw aangepast eindpunt voor gebruik met zowel een HTTP-aanvraag of de Microsoft Cognitive Services Speech Client-bibliotheek, die maakt gebruik van websockets.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer zelfstudies:
- [Uw proefabonnement voor Speech Service ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Aangepast akoestisch model maken](how-to-customize-acoustic-models.md)
- [Aangepaste taalmodel maken](how-to-customize-language-model.md)

---
title: Met de spraak-apparaten SDK - Speech Services oplossen
titleSuffix: Azure Cognitive Services
description: Dit artikel bevat informatie om u te helpen bij het oplossen van problemen die mogelijk optreden wanneer u de spraak-apparaten-SDK gebruiken.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: wellsi
ms.openlocfilehash: 87fb35f06dcb1d1e3fb8c3ae3be64c7448162f14
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026154"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Met de spraak-apparaten SDK oplossen

Dit artikel bevat informatie om u te helpen bij het oplossen van problemen die mogelijk optreden wanneer u de spraak-apparaten-SDK gebruiken.

## <a name="certificate-failures"></a>Certificaatfouten

Als er certificaatfouten bij het gebruik van de spraakservices, zorg ervoor dat uw apparaat de juiste datum en tijd heeft:

1. Ga naar **Settings**. Onder **System**, selecteer **datum en tijd**.

    ![Selecteer onder instellingen voor datum en tijd](media/speech-devices-sdk/qsg-12.png)

1. Houd de **automatische datum en tijd** optie is geselecteerd. Onder **Selecteer tijdzone**, selecteert u uw huidige tijdzone.

    ![Selecteer opties voor datum en tijdzone](media/speech-devices-sdk/qsg-13.png)

    Wanneer u ziet dat de ontwikkelingsset tijd overeenkomt met de tijd op uw PC, wordt de dev kit is verbonden met internet.

## <a name="next-steps"></a>Volgende stappen

* [Opmerkingen bij de release bekijken](devices-sdk-release-notes.md)

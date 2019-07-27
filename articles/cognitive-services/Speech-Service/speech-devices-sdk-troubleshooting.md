---
title: Problemen met de speech-apparaten oplossen SDK-spraak service
titleSuffix: Azure Cognitive Services
description: In dit artikel vindt u informatie over het oplossen van problemen die kunnen optreden wanneer u de speech apparaten SDK gebruikt.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wellsi
ms.openlocfilehash: 9a757402360b4b69e760315a809482be9b4e04d4
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68558904"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Probleem oplossen met de Speech Devices-SDK

In dit artikel vindt u informatie over het oplossen van problemen die kunnen optreden wanneer u de speech apparaten SDK gebruikt.

## <a name="certificate-failures"></a>Certificaatfouten

Als er certificaat fouten optreden bij het gebruik van de spraak Services, moet u ervoor zorgen dat uw apparaat de juiste datum en tijd heeft:

1. Ga naar **instellingen**. Onder **System**, selecteer **datum en tijd**.

    ![Selecteer onder instellingen voor datum en tijd](media/speech-devices-sdk/qsg-12.png)

1. Houd de **automatische datum en tijd** optie is geselecteerd. Onder **Selecteer tijdzone**, selecteert u uw huidige tijdzone.

    ![Selecteer opties voor datum en tijdzone](media/speech-devices-sdk/qsg-13.png)

    Wanneer u ziet dat de ontwikkelingsset tijd overeenkomt met de tijd op uw PC, wordt de dev kit is verbonden met internet.

## <a name="next-steps"></a>Volgende stappen

* [Opmerkingen bij de release bekijken](devices-sdk-release-notes.md)

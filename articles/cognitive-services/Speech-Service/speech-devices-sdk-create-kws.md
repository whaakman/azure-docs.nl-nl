---
title: Een aangepaste Wake Word-speech-service maken
titleSuffix: Azure Cognitive Services
description: Uw apparaat is altijd beschikbaar voor een wake (of meer woorden). Wanneer de gebruiker aangeeft het woord wake dat, verzendt het apparaat alle volgende audio naar de cloud, totdat de gebruiker niet meer spreken. Aanpassen van uw word wake is een effectieve manier om onderscheid maken tussen uw apparaat en Versterk uw huisstijl.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 2bc1a6cbbf1e0d790326849a41b0788e332daa31
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553106"
---
# <a name="create-a-custom-wake-word-by-using-the-speech-service"></a>Een aangepaste wake-woord maken met behulp van de Speech-service

Uw apparaat is altijd beschikbaar voor een wake (of meer woorden). Bijvoorbeeld, is 'Hallo Cortana' een woord wake voor de Cortana-assistent. Wanneer de gebruiker aangeeft het woord wake dat, verzendt het apparaat alle volgende audio naar de cloud, totdat de gebruiker niet meer spreken. Aanpassen van uw word wake is een effectieve manier om onderscheid maken tussen uw apparaat en Versterk uw huisstijl.

In dit artikel leert u over het maken van een aangepaste wake woord voor uw apparaat.

## <a name="choose-an-effective-wake-word"></a>Kies een effectieve wake-woord

Houd rekening met de volgende richtlijnen wanneer u een woord wake kiest:

* Uw word wake moet een Engels woord of woordgroep. Het duurt langer dan twee seconden te zeggen hadden.

* Woorden van 4 tot en met 7 lettergrepen werken het beste. Bijvoorbeeld, is "Hallo, Computer" een goede wake-woord. Alleen 'Hallo' is slecht.

* Wake woorden moeten volgen algemene Engelse uitspraak van regels.

* Een unieke of zelfs een fictieve woord dat voldoet aan de algemene regels voor Engels uitspraak van mogelijk fout-positieven verminderen. Bijvoorbeeld, "computerama" mogelijk een goede wake-woord.

* Kies een algemene word niet. Bijvoorbeeld, 'eten' en 'Ga' zijn woorden die mensen vaak in gewone conversatie zeggen. Ze zijn mogelijk ONWAAR triggers voor uw apparaat.

* Vermijd het gebruik van een wake-woord dat mogelijk alternatieve uitspraak. Gebruikers hoeft te weten de uitspraak 'recht' om te zorgen dat het apparaat om te reageren. Bijvoorbeeld, '509' kunnen worden verergerd "vijf nul negen," "vijf oh negen," of "vijf honderd en 9." "R.E.I." kunnen worden verergerd 'r-e-i' of "ray." 'Live' kunnen worden verergerd "/līv/" of '/liv/'.

* Gebruik geen speciale tekens, symbolen en cijfers. Bijvoorbeeld: 'Ga naar #' en '20 + katten' niet zou goed wake woorden. Echter, 'Ga sharp' of ' twintig plus katten ' werken mogelijk. U kunt nog steeds gebruik van de symbolen in uw huisstijl en gebruiken van marketing en documentatie te versterken van de juiste uitspraak.

> [!NOTE]
> Als u een handelsmerk woord als uw word wake kiest, moet u dat u de eigenaar die handelsmerk bent of dat u toestemming van de eigenaar van het merk hebt voor het gebruik van het woord. Microsoft is niet aansprakelijk voor eventuele juridische problemen die ontstaan van uw keuze van wake-woord.

## <a name="create-your-wake-word"></a>Maken van de wake-woord

Voordat u een aangepast Ontwaak woord kunt gebruiken met uw apparaat, moet u een Ontwaak woord maken met de micro soft Custom Wake-service voor het genereren van woorden. Nadat u een Ontwaak woord hebt geleverd, produceert de service een bestand dat u implementeert in uw Development Kit om het Ontwaak woord op uw apparaat in te scha kelen.

1. Ga naar de [Custom Speech Service-Portal](https://aka.ms/sdsdk-speechportal) en **Meld** u aan of als u geen spraak abonnement hebt, kiest u [**een abonnement maken**](https://go.microsoft.com/fwlink/?linkid=2086754)

    ![De Custom Speech Service Portal](media/speech-devices-sdk/wake-word-4.png)

1. Typ op het [aangepaste Ontwaak woord](https://aka.ms/sdsdk-wakewordportal) pagina in het Ontwaak woord van uw keuze en klik op **Wake Word toevoegen**. Er zijn enkele [richt lijnen](#choose-an-effective-wake-word) die u helpen bij het kiezen van een effectief tref woord. Momenteel wordt alleen de taal en-US ondersteund.

    ![Voer uw wake-woord](media/speech-devices-sdk/wake-word-5.png)

1. Er worden drie alternatieve uitspraak van uw Ontwaak woord gemaakt. U kunt alle uitspraaken kiezen die u wilt. Selecteer vervolgens **verzenden** om het Ontwaak woord te genereren. Als u het Ontwaak woord wilt wijzigen, verwijdert u eerst het bestaande item, wanneer u op de uitspraak lijn klikt, wordt het pictogram verwijderen weer gegeven.

    ![Uw Ontwaak woord controleren](media/speech-devices-sdk/wake-word-6.png)

1. Het kan een minuut duren voordat het model is gegenereerd. U wordt gevraagd het bestand te downloaden.

    ![Het Wake-woord downloaden](media/speech-devices-sdk/wake-word-7.png)

1. Sla het ZIP-bestand op uw computer. U hebt dit bestand nodig om uw aangepaste Wake-woord te implementeren in de Development Kit.

## <a name="next-steps"></a>Volgende stappen

Test uw aangepaste Wake-woord met [Speech-apparaten SDK Quick](https://aka.ms/sdsdk-quickstart)start.

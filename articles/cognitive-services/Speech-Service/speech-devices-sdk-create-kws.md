---
title: Maak een woord aangepaste wake - spraakservices
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
ms.openlocfilehash: f43cbedc633b26a3a7fcbfb5f6a75da514bf0c45
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604868"
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

Voordat u een aangepaste wake word met uw apparaat gebruiken kunt, moet u een woord wake maken met de aangepaste Wake Word generatie Microsoft-service. Nadat u een woord wake, de service produceert een bestandsnaam die opgeven implementeert u uw development kit om in te schakelen van het woord wake op uw apparaat.

1. Ga naar de [Custom Speech Service Portal](https://aka.ms/sdsdk-speechportal) en **aanmelden** of als u geen een spraak-abonnement kiezen [ **een abonnement maken**](https://go.microsoft.com/fwlink/?linkid=2086754)

    ![De Custom Speech serviceportal](media/speech-devices-sdk/wake-word-4.png)

1. Op de [aangepaste Wake Word](https://aka.ms/sdsdk-wakewordportal) typt u het woord wake van uw keuze en klik op pagina **toevoegen wake word**. We hebben enkele [richtlijnen](#choose-an-effective-wake-word) om u te helpen bij het kiezen van een doeltreffende trefwoord. We ondersteunen momenteel alleen en-US-taal.

    ![Voer uw wake-woord](media/speech-devices-sdk/wake-word-5.png)

1. Drie alternatieve uitspraak van de wake-woord wordt gemaakt. U kunt de uitspraak die u bevalt. Selecteer vervolgens **indienen** voor het genereren van het woord wake. Als u wijzigen het woord wake Verwijder het bestaande bestand eerst wilt, wanneer u de muisaanwijzer op de regel uitspraak van wordt het verwijderpictogram weergegeven.

    ![Controleer uw wake-woord](media/speech-devices-sdk/wake-word-6.png)

1. Het duurt maximaal één minuut voor het model moet worden gegenereerd. U wordt gevraagd om het bestand te downloaden.

    ![Downloaden van de wake-woord](media/speech-devices-sdk/wake-word-7.png)

1. Sla het ZIP-bestand op uw computer. U moet dit bestand op het implementeren van uw aangepaste wake word development kit.

## <a name="next-steps"></a>Volgende stappen

Testen van uw aangepaste wake word met [Quick Start de SDK van de spraakherkenning apparaten](https://aka.ms/sdsdk-quickstart).

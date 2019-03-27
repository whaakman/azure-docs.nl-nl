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
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: b5ace2e741f900dd4ab7ba6518d0956284af35f6
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498228"
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

Voordat u een aangepaste wake word met uw apparaat gebruiken kunt, moet u het woord wake maken met behulp van de aangepaste Wake Word generatie Microsoft-service. Nadat u een woord wake, de service produceert een bestandsnaam die opgeven implementeert u uw development kit om in te schakelen van het woord wake op uw apparaat.

1. Ga naar de [Custom Speech serviceportal](https://cris.ai/).

    ![De Custom Speech serviceportal](media/speech-devices-sdk/wake-word-4.png)

1. Meld u aan met het e-mailadres dat de uitnodiging voor Azure Active Directory hebt ontvangen.

1. De **aangepaste Wake Word** pagina is niet beschikbaar voor het publiek, dus er is geen directe koppeling waarmee u er gaat. De functie voor aangepaste spraak vereist een Azure-abonnement, maar de functie aangepaste Wake Word niet. Als u de **geen abonnementen gevonden.** foutpagina, alleen vervangen de **"abonnementen? errorMessage = geen % 20Subscriptions % 20found"** met '**customkws**' in de URL en op ENTER. De URL moet een van de volgende: https://westus.cris.ai/customkws, https://eastasia.cris.ai/customkws of https://northeurope.cris.ai/customkws, afhankelijk van waar uw regio is.

1. Typ in het woord wake van uw keuze, en selecteer vervolgens **verzenden van het woord**.

    ![Voer uw wake-woord](media/speech-devices-sdk/wake-word-5.png)

1. Het duurt een paar minuten de bestanden die worden gegenereerd. Hier ziet u een draaiende cirkel in het browservenster. Kort daarna een informatiebalk wordt weergegeven waarin u een zipbestand te downloaden.

1. Sla het ZIP-bestand op uw computer. U moet dit bestand op het woord aangepaste wake development kit implementeren. Volg de instructies in voor het implementeren van het woord aangepaste wake [aan de slag met de SDK van de apparaten spraak](speech-devices-sdk-qsg.md).

1. Selecteer **Meld u af.**

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen, krijgen een [gratis Azure-account](https://azure.microsoft.com/free/) en zich registreren voor de SDK van de apparaten spraak.

> [!div class="nextstepaction"]
> [Aanmelden voor de SDK van de apparaten spraak](get-speech-devices-sdk.md)

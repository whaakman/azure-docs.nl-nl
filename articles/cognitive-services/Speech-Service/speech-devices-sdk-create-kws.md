---
title: Het maken van een aangepaste wake-woord
description: Het maken van een aangepaste wake-woord voor de SDK van de apparaten spraak.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 615a901c70fff92141442699ea6e4b8fce1c9ace
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282570"
---
# <a name="create-a-custom-wake-word-using-speech-service"></a>Maak een aangepaste wake woord met behulp van spraak-service

Uw apparaat is altijd beschikbaar voor een wake (of meer woorden). Bijvoorbeeld, is 'Hallo Cortana' een woord wake voor de Cortana-assistent. Wanneer de gebruiker aangeeft het woord wake dat, start het apparaat alle volgende audio verzenden naar de cloud, totdat de gebruiker niet meer spreken. Aanpassen van uw word wake is een effectieve manier om onderscheid maken tussen uw apparaat en Versterk uw huisstijl.

In dit artikel leert u over het maken van een aangepaste wake woord voor uw apparaat.

## <a name="choosing-an-effective-wake-word"></a>Kiezen van een doeltreffende wake-woord

Let op de volgende richtlijnen bij het kiezen van een wake-woord.

* Uw word wake moet een Engels woord of woordgroep. Het duurt langer dan twee seconden te zeggen hadden.

* Woorden van 4 tot en met 7 lettergrepen werken het beste. Bijvoorbeeld, is "Hallo, Computer" een goede wake-woord, terwijl alleen 'Hallo' een slechte is.

* Wake woorden moeten volgen algemene Engelse uitspraak van regels.

* Een unieke of zelfs fictieve woord dat voldoet aan de algemene regels voor Engels uitspraak van kan fout-positieven worden verminderd. 'Computerama' kan bijvoorbeeld een goede wake-woord.

* Kies een algemene word niet. Bijvoorbeeld, 'eten' en 'Ga' zijn woorden die mensen vaak in gewone conversatie zeggen. De waarde false triggers voor uw apparaat, kan deze worden.

* Vermijd het gebruik van een wake-woord dat een alternatieve uitspraak kan hebben. Gebruikers hoeft te weten de uitspraak 'recht' om te zorgen dat het apparaat om te reageren. Bijvoorbeeld '509' kunnen worden verergerd zoals "vijf nul negen", "vijf oh negen ', of"vijf honderd en 9." "R.E.I." kan worden uitgesproken als ' R E ik ' of "Ray." 'Live' kunnen worden uitgesproken als [līv] of [Live].

* Gebruik geen speciale tekens, symbolen en cijfers. Bijvoorbeeld: 'Ga naar #' en '20 + katten' niet zou goed wake woorden. Echter, 'Ga sharp' of ' twintig plus katten "kunnen werken. U kunt nog steeds gebruik van de symbolen in uw huisstijl en gebruiken van marketing en documentatie te versterken van de juiste uitspraak.

> [!NOTE]
> Als u een handelsmerk woord als uw word wake kiest, moet u dat u eigenaar bent die handelsmerk, anders gemachtigd door de eigenaar van het merk te gebruiken. Microsoft is niet aansprakelijk voor eventuele juridische problemen die ontstaan van uw keuze van wake-woord.

## <a name="creating-your-wake-word"></a>Het maken van de wake-woord

Voordat u een aangepaste wake word met uw apparaat gebruiken kunt, moet u deze met behulp van de aangepaste Wake Word generatie Microsoft-service maken. Nadat u een woord wake, de service produceert een bestand dat opgeven implementeert u vervolgens op uw dev kit om in te schakelen uw word wake op uw apparaat.

1. Ga naar de [Custom Speech Service portal](https://cris.ai/).

2. Een nieuw account maken met het e-mailadres waarop u de uitnodiging voor Azure Active Directory hebt ontvangen. 

    ![Nieuw account maken](media/speech-devices-sdk/wake-word-1.png)
 
3.  Wanneer u bent aangemeld, vul het formulier in en klik vervolgens op **Start de reis.**

    ![aangemeld](media/speech-devices-sdk/wake-word-3.png)
 
4. De **aangepaste Wake Word** pagina is niet beschikbaar voor het publiek, dus er is geen koppeling waarmee u er gaat. Klik op of plak in deze koppeling in plaats daarvan: https://cris.ai/customkws.

    ![verborgen pagina](media/speech-devices-sdk/wake-word-4.png)
 
6. Typ het woord wake van uw keuze, klikt u vervolgens **indienen** deze.

    ![Voer uw wake-woord](media/speech-devices-sdk/wake-word-5.png)
 
7. Het duurt een paar minuten de bestanden die worden gegenereerd. U ziet een draaiende cirkel op tabblad van uw browser. Kort daarna een informatiebalk wordt weergegeven waarin wordt u voor het downloaden van een `.zip` bestand.

    ![ZIP-bestand ontvangen](media/speech-devices-sdk/wake-word-6.png)

8. Sla de `.zip` bestand op uw computer. U moet dit bestand op het woord aangepaste wake implementeren in de development kit, de instructies in [aan de slag met de SDK van de apparaten spraak](speech-devices-sdk-qsg.md).

9. U kunt nu **Meld u af.**

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen, krijgen een [gratis Azure-account](https://azure.microsoft.com/free/) en zich registreren voor de SDK van de apparaten spraak.

> [!div class="nextstepaction"]
> [Aanmelden voor de SDK van de apparaten spraak](get-speech-devices-sdk.md)


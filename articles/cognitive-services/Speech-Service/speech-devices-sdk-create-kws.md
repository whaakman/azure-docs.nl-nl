---
title: Maken van een aangepaste wake woord | Microsoft Docs
description: Maken van een aangepaste wake woord voor spraak apparaten SDK.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 2575ed24bb931ca4da05dd6663b976406af590e6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345836"
---
# <a name="create-a-custom-wake-word-using-speech-service"></a>Maak een aangepaste wake woord met behulp van spraak-service

Uw apparaat is altijd beschikbaar voor een wake (of meer woorden). Bijvoorbeeld, is 'Artikel van Hey Cortana' een word wake voor de assistent Cortana. Wanneer de gebruiker het woord wake staat, wordt het apparaat wordt gestart alle daaropvolgende audio verzenden naar de cloud totdat de gebruiker niet meer spreken. Uw word wake aanpassen is een effectieve manier om te onderscheiden van het apparaat en uw huisstijl versterken.

In dit artikel leert u het maken van een aangepaste wake woord voor uw apparaat.

## <a name="choosing-an-effective-wake-word"></a>Een woord effectieve wake kiezen

Houd rekening met de volgende richtlijnen bij het kiezen van een wake-woord.

* Uw word wake moet een Engels woord of woordgroep. Het duurt langer dan twee seconden in te spreken.

* Woorden van 4 tot en met 7 lettergrepen werken het beste. Bijvoorbeeld, is "Artikel van Hey, Computer" een goede wake woord, terwijl alleen 'artikel van Hey' een slechte is.

* Wake woorden moeten volgen algemene Engelse uitspraak regels.

* Een unieke of zelfs fictieve woord dat voldoet aan de algemene regels voor Engels uitspraak kan valse positieven te reduceren. 'Computerama' kan bijvoorbeeld een goede wake woord.

* Kies een algemene woord niet. Bijvoorbeeld 'eat' en 'Ga' zijn woorden die mensen zeggen vaak in normale conversatie dat. Deze kan worden false triggers voor uw apparaat.

* Vermijd het gebruik van een wake-woord dat alternatieve uitspraak kan hebben. Gebruikers moeten weten de uitspraak 'juiste' om te zorgen dat het apparaat om te reageren. Bijvoorbeeld '509' kunnen worden verergerd zoals 'vijf nul negen', ' vijf geselecteerd negen ', of "vijf honderd en negen." 'R.E.I.' kan worden verergerd als ' R E ik ' of "Ray." 'Live' kunnen worden verergerd als [līv] of [Live].

* Gebruik geen speciale tekens, symbolen en cijfers. Bijvoorbeeld: 'Ga #' en '20 + katten' niet goed wake woorden. Echter, 'Ga kruis' of ' twintig plus katten ' mogelijk werken. U kunt nog steeds gebruik van de symbolen in uw huisstijl en gebruiken van marketing en documentatie te versterken de juiste uitspraak.

> [!NOTE]
> Als u een handelsmerk woord als uw woord wake, moet u ervoor dat u bezit die handelsmerk, anders gemachtigd door de eigenaar van het merk om deze te gebruiken. Microsoft is niet aansprakelijk voor juridische problemen ten gevolge van uw keuze van wake-woord.

## <a name="creating-your-wake-word"></a>Maken van uw word wake

Voordat u een aangepaste wake word met uw apparaat gebruiken kunt, moet u met behulp van de service Microsoft aangepaste Wake Word generatie maken. Geef een wake-woord, de service een bestand dat u vervolgens implementeert op uw Developer kit om in te schakelen uw word wake op uw apparaat wordt gegenereerd nadat u hebt.

1. Ga naar de [aangepaste spraak-serviceportal](https://cris.ai/).

2. Een nieuw account maken met het e-mailadres waarop u de uitnodiging voor Azure Active Directory hebt ontvangen. 

    ![nieuw account maken](media/speech-devices-sdk/wake-word-1.png)
 
3.  Nadat u bent aangemeld, het formulier invullen en klik vervolgens op **het traject starten.**

    ![aangemeld](media/speech-devices-sdk/wake-word-3.png)
 
4. De **aangepaste Wake Word** pagina is niet openbaar, dus er is geen koppeling waarmee u er gaat. Klik op of plak in plaats daarvan in deze koppeling: https://cris.ai/customkws.

    ![verborgen pagina](media/speech-devices-sdk/wake-word-4.png)
 
6. Typ in het woord wake van uw keuze, klikt u vervolgens **indienen** deze.

    ![Voer uw wake woord](media/speech-devices-sdk/wake-word-5.png)
 
7. Het kan enkele minuten duren voordat de bestanden die worden gegenereerd. U ziet een draaiende cirkel op tabblad van de browser. Na een ogenblik een informatiebalk wordt weergegeven waarin wordt u voor het downloaden van een `.zip` bestand.

    ![ZIP-bestand ontvangen](media/speech-devices-sdk/wake-word-6.png)

8. Sla de `.zip` bestand op uw computer. U moet dit bestand voor het implementeren van het aangepaste wake woord development kit, volgens de instructies in [aan de slag met de SDK van de apparaten spraak](speech-devices-sdk-qsg.md).

9. U kunt nu **afmelden.**

## <a name="next-steps"></a>Volgende stappen

Om te beginnen, krijgen een [gratis Azure-account](https://azure.microsoft.com/free/) en meld u aan voor de apparaten spraak SDK.

> [!div class="nextstepaction"]
> [Aanmelden voor de apparaten Speech SDK](get-speech-devices-sdk.md)


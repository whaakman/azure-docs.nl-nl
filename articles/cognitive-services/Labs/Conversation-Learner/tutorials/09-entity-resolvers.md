---
title: Entiteit Resolvers in een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van entiteit Resolvers in Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ebe555bfd7b34efd87d400d786049964665c76e6
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57451056"
---
# <a name="entity-resolvers"></a>Entiteit Resolvers

In deze zelfstudie ziet u hoe u entiteit Resolvers in Conversatiecursist

## <a name="video"></a>Video

[![Entiteit Resolvers zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_EntityResolvers_Preview)](https://aka.ms/cl_Tutorial_v3_EntityResolvers)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

- Conflictoplosser Type is een optionele eigenschap van aangepaste entiteiten.
- Entiteit Resolvers gebruikmaken van de kracht van de kenmerken van de vooraf getrainde entiteit in LUIS voor aanvullende details, heldere berichten van uw aangepaste entiteit.

## <a name="steps"></a>Stappen

### <a name="create-a-new-model"></a>Een nieuw Model maken

1. Klik op de knop "Nieuw Model" in de Web-UI.
2. In het veld "Naam", typt u 'Entiteit Resolvers', drukt u op of klik op de knop 'Maken'.

### <a name="create-a-pair-of-entities"></a>Maak een sleutelpaar met een van de entiteiten

1. In het linkerdeelvenster klikt u op 'Entiteiten', en klik vervolgens op de knop 'Nieuwe entiteit'.
2. Typ in het veld 'Naam van de entiteit', 'vertrek'.
3. Selecteer in de vervolgkeuzelijst 'Type conflictoplosser' naar beneden, 'datetimeV2'.
4. Klik op de knop 'Maken'.
5. Nadat u de informatie-pop-upvenster hebt gelezen, klikt u op de knop 'OK'.
6. Maakt een andere entiteit met de naam 'terug' die ook is van het type 'datetimeV2' conflictoplosser na de dezelfde stappen.

### <a name="create-a-pair-of-actions"></a>Maak een sleutelpaar met een van de acties

1. In het linkerdeelvenster klikt u op 'Acties', en klik vervolgens op de knop 'Nieuwe actie'.
2. In de 'van de Bot-response' veldtype, 'u bent op $departure verlaten en op $return retourneren'.
    - BELANGRIJK - als te typen in "$[entityName]", moet u druk op enter of klik op de entiteit in de vervolgkeuzelijst, anders Conversatiecursist worden beschouwd als deze optie om te worden van tekst in plaats van een entiteit.
    - U ziet dat het veld 'Entiteiten vereist' ook deze entiteiten krijgt en kan niet worden verwijderd. Dit voorkomt dat deze actie niet langer beschikbaar totdat beide entiteiten aanwezig zijn vereist.
3. Klik op de knop 'Maken'.
4. Klik op de knop 'Nieuwe actie' opnieuw uit om te maken van een tweede actie.
5. In de 'van de Bot response' veld type, "Wanneer u van plan bent om te reizen?".
6. In de "Entiteiten diskwalificeren" veldtype, 'vertrek' en ook type, "resultaat".
    - Deze Vertel ons Bot deze actie niet uitvoeren als een van deze entiteiten een waarde bevatten.
7. Klik op de knop 'Maken'.


### <a name="training"></a>Training

1. Bekijk de "Training: [Status] ' op het bovenste deel van de pagina en wacht tot deze is 'voltooid' links.
    - Als het hierdoor te lang duurt, kunt u de koppeling "Vernieuwen" klikken.
    - Training status is 'Voltooid' nodig zodat onze Resolvers entiteit werken wanneer we het Model te trainen.
2. In het linkerdeelvenster klikt u op 'Train-dialoogvensters', en klik vervolgens op de knop 'Nieuwe Train dialoogvenster'.
3. Type in de eerste gebruiker-utterance 'book me een vlucht'. 
4. Klik op de knop 'Acties Score'.
5. Selecteer het antwoord, "Wanneer u van plan bent om te reizen?".
6. Als de gebruiker reageren met, 'morgen verlaten en retourneren zondag volgende week'.
    - U ziet hoe Conversatiecursist schakelt u twee 'Vooraf getrainde datum' entiteiten in dat de gebruiker heeft gedetecteerd.
7. In het deelvenster "Entiteit detectie", selecteert u de tekst 'morgen' en deze te labelen als "vertrek"
8. Label ook de tekst 'Zondag volgende week' als 'return'
9. Klik op de knop 'Acties Score'.
    - U ziet hoe het deelvenster 'Geheugen' bevat de datums vertrek en keer terug.
    - Beweeg de muisaanwijzer over elkaar en bekijk hoe de entiteiten date-objecten die duidelijk vastleggen van de werkelijke datum in plaats van "Zondag" of 'morgen' zijn.
10. Selecteer de "u verlaat op...' Bot-antwoord.
11. Klik op de knop 'Opslaan'.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Alternatieve invoer](./10-alternative-inputs.md)

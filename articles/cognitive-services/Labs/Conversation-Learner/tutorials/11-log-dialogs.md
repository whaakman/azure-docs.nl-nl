---
title: Hoe u zich aanmeldt dialoogvensters in een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Leer hoe u aan te melden dialoogvensters in een model Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 74ee04fb3d4044573a98535a9bdc26d5c593a222
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796760"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>Hoe u zich aanmeldt dialoogvensters in een model Conversatiecursist

Deze zelfstudie wordt gedemonstreerd hoe Log-dialoogvensters werkzaam zijn beter modellen kunt trainen Conversatiecursist van geregistreerde interacties met gebruikers van de echte wereld werkt.

## <a name="video"></a>Video

[![Zelfstudie log dialoogvensters-Preview](https://aka.ms/cl_Tutorial_v3_LogDialogs_Preview)](https://aka.ms/cl_Tutorial_v3_LogDialogs)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

en het weer-model in de vorige zelfstudies hebt gemaakt.

## <a name="details"></a>Details
Logboek-dialoogvensters zijn opgenomen logboeken van van uw bot interactie met eindgebruikers. U kunt entiteit labels en actie selecties voor het verbeteren van de prestaties van het model en de algehele systeemprestaties oplossen door gebruik te maken van deze Log-dialoogvensters.

## <a name="steps"></a>Stappen

In de web-UI, klik op "Zelfstudies importeren" en selecteer het model met de naam '-zelfstudie-11-LogDialogs'.

Dit Model bevat één entiteit met de naam 'plaats' en acties die zijn ontworpen om te reageren op query's over weer in die plaats. Twee trainen dialoogvensters zijn gebruikt voor het Model te trainen, zodat de prestatieverwachtingen enigszins laag zijn. Het Model wilt verbeteren met extra training en blootstelling aan de echte wereld gebruikersinteracties.

### <a name="create-a-new-conversation"></a>Maak een nieuwe discussie

1. Klik op "Log-dialoogvensters" en vervolgens de knop 'Dialoogvenster voor een nieuwe Log' in het linkerdeelvenster.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Austin weather forecast'
3. Klik op de knop 'Gedaan testen'.
4. Klik op het dialoogvenster 'Austin weather forecast' log in de lijst.
5. Klik op de utterance 'Austin weather forecast' in het deelvenster chat.
6. Klik op 'Austin' en klik vervolgens op 'plaats' uit de lijst met entiteiten.
7. Klik op de knop 'Wijzigingen verzenden'.
    - Deze wijziging in de waarde van de entiteit veroorzaken downstream wijzigingen aan het gesprek, aangezien we hebben nieuwe entiteitswaarden in het geheugen. Latere acties zijn waarschijnlijk ongeldig geworden met name die met betrekking tot de entiteit 'plaats'.
8. Klik op de "Welke plaats?" utterance in het deelvenster chat.
9. Selecteer het antwoord, "het weer in Austin is waarschijnlijk zonnige."
10. Klik op de knop 'Opslaan als Train dialoogvenster'.
    - Training wordt onmiddellijk gestart

Een laatste notitie. Afhankelijk van de behoeften van uw bedrijf, kan de conversatie-functie voor logboekregistratie worden uitgeschakeld door te gaan naar de instellingen en -unchecking "Log gesprekken."

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteit detectie terugbellen](./12-entity-detection-callback.md)

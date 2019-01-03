---
title: Het gebruik van de eigenschap 'Verwacht een entiteit' van de conversatie Learner acties - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van de eigenschap 'Verwacht een entiteit' van een Model van de cursist conversatie.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c6696d0f22026ac333c526b505732d15a4b01be7
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796758"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Het gebruik van de eigenschap 'Verwacht een entiteit' van acties

In deze zelfstudie ziet u de eigenschap 'Verwacht een entiteit' van acties.

## <a name="video"></a>Video

[![Verwachte entiteit zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_ExpectedEntity_Preview)](https://aka.ms/cl_Tutorial_v3_ExpectedEntity)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Gebruik de eigenschap 'Verwacht een entiteit' van een actie op te slaan van het antwoord van de gebruiker voor deze actie in een entiteit.

Als u entiteiten toevoegt aan de eigenschap 'Verwacht een entiteit' van een actie, wordt het systeem:

1. Begin door te proberen zodat deze overeenkomen met entiteiten met behulp van de machine learning op basis van de entiteit extractie Model
2. De utterance hele gebruiker toewijzen aan $entity op basis van heuristiek als er geen entiteiten worden gevonden
3. Bel `EntityDetectionCallback`, en gaat u verder met de actie selecteren.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het Model maken

1. In de Web-gebruikersinterface, klikt u op "Nieuwe Model."
2. Typ in het veld 'Name', 'ExpectedEntities' en druk op enter.
3. Klik op de knop 'Maken'.

### <a name="entity-creation"></a>Entiteit maken

1. In het linkerdeelvenster klikt u op "Entiteiten" en vervolgens de knop 'Nieuwe entiteit'.
2. Selecteer 'Aangepaste getraind' voor het "entiteitstype'.
3. Typ 'naam' voor "Naam van de entiteit."
4. Klik op de knop 'Maken'.

> [!NOTE]
> Het aangepaste getrainde entiteitstype betekent dat deze entiteit kan worden getraind, in tegenstelling tot andere typen entiteiten.

![](../media/tutorial4_entities.PNG)

### <a name="create-the-first-action"></a>De eerste actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u 'Wat is de naam van uw?'
3. Typ in het veld "Entiteiten verwacht", "naam".
4. Klik op de knop 'Maken'.

> [!NOTE]
> Entiteiten gevonden en opgehaald uit het antwoord van de gebruiker wordt opgeslagen in de entiteit "name" Als deze actie wordt gekozen. Als er geen entiteiten worden gedetecteerd, wordt het volledige antwoord worden opgeslagen in deze entiteit.

### <a name="create-the-second-action"></a>De tweede actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u 'Hi $name'!
3. Klik op de knop 'Maken'.

> [!NOTE]
> De entiteit 'naam' automatisch is toegevoegd als een 'vereist entiteiten' verwezen in het antwoord.

U hebt nu twee acties.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-model"></a>Het Model te trainen

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Hallo.'
    - Dit simuleert de kant van de gebruiker van de conversatie.
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "Wat is de naam van uw?"
    - De 'Hi $name!" antwoord kan niet worden geselecteerd omdat deze reactie is vereist voor de 'naam' entiteit nu in het geheugen van het Model worden gedefinieerd.
5. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Frank."
    - 'Frank' is gemarkeerd als een entiteit op basis van de heuristiek die wordt ingesteld lager naar het antwoord opslaan als de entiteit.
6. Klik op de knop 'Acties Score'.
    - De entiteit 'naam' nu wordt gedefinieerd als 'Frank' in het geheugen van het Model, zodat de actie 'Hallo $name' worden geselecteerd als een actie.
7. Selecteer het antwoord, "Hoi $name!"
8. Klik op de knop 'Opslaan'.

Alternatieve invoer verdere treinen toevoegen van het Model.

1. Typ in het veld 'Toevoegen alternatieve invoer...', "Ik ben Jose."
    - Het Model wordt niet de naam als een entiteit herkend, zodat deze het gehele blok geselecteerd als de waarde van de entiteit
2. Klik op de zin 'Ik ben Jose' en klik vervolgens op het prullenbakpictogram.
3. Klik op 'Jose' en klik vervolgens op 'name' in de lijst met entiteiten.
4. Klik op Score acties.
5. Selecteer het antwoord "Hallo Frank!"
6. Klik op de knop 'Opslaan'.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Negatable entiteiten](./06-negatable-entities.md)

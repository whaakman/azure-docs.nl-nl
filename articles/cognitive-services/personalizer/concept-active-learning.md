---
title: Actief leren - Personalizer
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: edjez
ms.openlocfilehash: c44afc81a7ec9d05cdc04cc8bc46c77cd51ceaf8
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722525"
---
# <a name="active-learning-and-learning-policies"></a>Actief leren en leren werken met beleidsregels 

Wanneer uw toepassing de positie-API aanroept, ontvangt u een positie van de inhoud. Zakelijke logica kunt deze rangschikking gebruiken om te bepalen of de inhoud weergegeven aan de gebruiker moet worden. Wanneer u inhoud worden weergegeven de gerangschikte, dat wil zeggen een _active_ positie gebeurtenis. Wanneer uw toepassing niet wordt weergegeven die inhoud, dat wil zeggen dat een _inactieve_ positie gebeurtenis. 

Informatie over de actieve positie gebeurtenis wordt geretourneerd naar Personalizer. Deze informatie wordt gebruikt om door te gaan met het model via het huidige beleid van learning te trainen.

## <a name="active-events"></a>Actieve gebeurtenissen

Actieve gebeurtenissen moeten altijd worden weergegeven aan de gebruiker en de aanroep van derden om te sluiten van de lus learning moet worden geretourneerd. 

### <a name="inactive-events"></a>Niet-actieve gebeurtenissen 

Niet-actieve gebeurtenissen mag niet het onderliggende model niet wijzigen omdat de gebruiker een kans om te kiezen uit de gerangschikte inhoud is niet opgegeven.

## <a name="dont-train-with-inactive-rank-events"></a>Geen trainen met inactieve positie gebeurtenissen 

Voor sommige toepassingen moet u mogelijk de positie-API aanroepen zonder te weten nog als uw toepassing wordt de resultaten voor de gebruiker weergegeven. 

Dit gebeurt wanneer:

* U kunt sommige UI die de gebruiker al dan niet om te zien krijgen vooraf worden weergeven. 
* Uw toepassing kan worden voorspeld personalisatie waarin positie aanroepen worden gedaan met minder realtime context en de uitvoer kan of kan niet worden gebruikt door de toepassing doen. 

### <a name="disable-active-learning-for-inactive-rank-events-during-rank-call"></a>Actief leren uitschakelen voor inactieve positie gebeurtenissen bij positie-oproep

Aan het uitschakelen van automatische learning aanroepen positie met `learningEnabled = False`.

Training voor een inactieve gebeurtenis is impliciet geactiveerd als het verzenden van een beloning voor de positie.

## <a name="learning-policies"></a>Leren werken met beleidsregels

Leren werken met beleid bepaalt de specifieke *hyperparameters* van het trainen van het model. Twee modellen van dezelfde gegevens, getraind van beleidsregels voor verschillende learning zich anders gedragen.

### <a name="importing-and-exporting-learning-policies"></a>Importeren en exporteren van Learning beleid

U kunt importeren en exporteren van learning beleidsbestanden vanuit Azure portal. Hiermee kunt u bestaande beleidsregels opslaan, te testen, vervangt u deze en archiveren in uw broncodebeheer als artefacten voor toekomstig gebruik en controle.

### <a name="learning-policy-settings"></a>Learning-beleidsinstellingen

De instellingen in de **Learning beleid** zijn niet bedoeld om te worden gewijzigd. De instellingen alleen wijzigen als u begrijpt hoe ze Personalizer beïnvloeden. Instellingen voor deze medeweten wijzigt, worden neveneffecten, met inbegrip van ongeldig Personalizer modellen.

### <a name="comparing-effectiveness-of-learning-policies"></a>Vergelijking van de effectiviteit van het leren werken met beleidsregels

U kunt vergelijken hoe verschillende beleidsregels voor leren op deze manier zou hebben uitgevoerd op oude gegevens in Logboeken Personalizer [offline evaluaties](concepts-offline-evaluation.md).

[Upload uw eigen beleid Learning](how-to-offline-evaluation.md) moeten worden vergeleken met het huidige learning-beleid.

### <a name="discovery-of-optimized-learning-policies"></a>Detectie van geoptimaliseerde learning beleid

Personalizer een meer geoptimaliseerde learning-beleid kunt maken bij het uitvoeren van een [offline evaluatie](how-to-offline-evaluation.md). Een meer geoptimaliseerde learning-beleid, die wordt weergegeven om betere beloningen in een offline-evaluatie, leidt tot betere resultaten bij het online in Personalizer gebruikt.

Nadat een geoptimaliseerde learning-beleid is gemaakt, kunt u deze rechtstreeks naar Personalizer toepassen zodat het huidige beleid onmiddellijk vervangt, of u kunt opslaan om nader te evalueren en bepalen in de toekomst of u wilt verwijderen, opslaan of later toepassen.
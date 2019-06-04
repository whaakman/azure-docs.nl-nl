---
title: Wanneer u ENUM entiteiten en acties van de entiteit ingesteld met een Model van de cursist conversatie - Azure Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over wanneer het is geschikt voor gebruik in ENUM entiteiten en acties van de entiteit ingesteld met een Model van de cursist conversatie.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1393d0bd1c31a2c9c24652e260ef7f3182d91367
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476492"
---
# <a name="when-to-use-enum-entities-and-set-entity-actions"></a>Wanneer u een ENUM-entiteiten en acties van de entiteit instellen

In deze zelfstudie wordt uitgelegd wanneer u ENUM (opsomming) entiteiten en SET_ENTITY acties moet gebruiken.

## <a name="video"></a>Video

[![Entiteit zelfstudie Preview-versie instellen](https://aka.ms/cl_Tutorial_v3_SetEntity_Preview)](https://aka.ms/cl_Tutorial_v3_SetEntity)



## <a name="what-is-covered"></a>Wat wordt er behandeld

In deze zelfstudie worden twee nieuwe functies geïntroduceerd. Een nieuw type entiteit ENUM (kort voor opsomming) genoemd en een nieuw type actie SET_ENTITY, die kunnen verwijzen naar een van deze enum-waarden, zoals de naam al aangeeft, wordt de entiteit op deze waarde ingesteld. Als u leert onder, deze nieuwe functies samen worden gebruikt en wordt uitgelegd wat ze zijn en hoe u deze hieronder. Voordat we de gegevens krijgen, is het belangrijk om te weten welk probleem deze functies op te lossen.

![enum_entity_type.png](../media/tutorial-enum-set-entity/enum_entity_type.png)
![action_set_entity_type.png](../media/tutorial-enum-set-entity/action_set_entity_type.png)

## <a name="problem"></a>Probleem

Er zijn gevallen in gesprekken waarin de betekenis van woorden is afhankelijk van de context.  Normaal gesproken de gelabelde sleutel woorden worden geleerd en geëxtraheerd met behulp van een language understanding-service, maar in deze gevallen deze systemen kunnen niet mogelijk om te leren met gelabelde voorbeelden.

Stel dat u deel uit van een gesprek tussen mensen in de buurt overhear en hoort u alleen het woord "Ja". U zou niet weet wat de "Ja" is akkoord te gaan of bevestigen omdat u de vraag hebt niet gereageerd gevraagd. De vraag gevraagd voordat de context, waardoor betekenis om het antwoord is. Op dezelfde manier sinds "Ja" is een algemene reactie op veel verschillende vragen het niet kan worden geleerd door Hiermee worden voorbeelden gegeven net zoals u met doen zou [aangepaste getrainde](04-introduction-to-entities.md) entiteiten omdat vervolgens deze zou leert u hoe om te labelen elke 'Ja' als die entiteit.

### <a name="example"></a>Voorbeeld

We gaan verder verduidelijken met het volgende voorbeeld:

Bot: U dat Azure Cognitive Services?
Gebruiker: Ja Bot: U dat ijs?
Gebruiker: Ja

In de vorige zelfstudies is bekeken [aangepaste getrainde](04-introduction-to-entities.md) entiteiten en uw eerste thought mogelijk te maken van een entiteit met de naam 'likesCogServices' en de eerste 'Ja' als deze entiteit te labelen.  Echter, zou de tweede ook 'Ja' label het systeem. Wanneer wordt geprobeerd om op te lossen van het label van de tweede 'Ja' op "likesIceCream", we maakt dan een conflict van twee dezelfde invoer "Ja" betekenis verschillende dingen en zou zijn vastgelopen.

Het is in dergelijke gevallen die u nodig hebt om ENUM entiteiten en SET_ENTITY acties te gebruiken.

## <a name="when-to-use-enums-or-setentity-actions"></a>Wanneer u een Enum-waarden of SET_ENTITY acties

Gebruik deze onderstaande regels om te weten wanneer u een ENUM-entiteiten en SET_ENTITY acties:

- Detectie of de instelling van de entiteit is afhankelijk van context
- Het aantal mogelijke waarden is opgelost (Ja en Nee zou twee waarden)

Met andere woorden, gebruiken voor het sluiten beëindigd prompts zoals bevestiging vragen die altijd in Ja of Nee resulteren.

> [!NOTE]
> Momenteel hebben we de beperking van maximaal 5 per entiteit enum-waarden. Een van de sleuven in de huidige limiet van 64 maakt gebruik van elke waarde. Zie [cl-waarden-en-grenzen](../cl-values-and-boundaries.md)

Voorbeeld: Bot: Uw order juist is?
Gebruiker: Ja

Wanneer de mogelijke waarden van de entiteit Open en niet opgelost zijn, moet u wilt gebruiken, zoals een andere functie [verwacht entiteit](05-expected-entity.md).

Voorbeeld: Bot: Hoe heet u?
Gebruiker: Matt Bot: Wat is uw favoriete kleur?
Gebruiker: Zilver

Deze vraag worden beschouwd als open omdat ze met willekeurige waarden kunnen worden beantwoord.

## <a name="what"></a>Wat

### <a name="enum-entities"></a>ENUM-entiteiten

ENUM entiteiten worden net als bij de andere entiteiten gemaakt. Net als bij "programmatische" entiteiten, kan niet u woorden labelen als deze entiteiten. Ze kunnen in plaats daarvan moeten worden ingesteld via programmacode of SET_ENTITY acties.

![enum_entity_creation.png](../media/tutorial-enum-set-entity/enum_entity_creation.png)

### <a name="set-entity-actions"></a>Acties van de entiteit instellen

Zoals eerder vermeld, stelt 'Entiteit ingesteld' acties een entiteit op een bekende enum-waarde. U kunt dezelfde resultaten kan realiseren door het maken van een actie van de callback API en het gebruik van het geheugenbeheer om in te stellen van de entiteit op een waarde. Bijvoorbeeld `memory.Set(entityName, entityValue)`. Met deze code schrijven en maken van deze acties zou worden omslachtig en moeilijk te beheren - zodat Conversatiecursist speciale handelingen te vergemakkelijken dit werk en deze acties automatisch genereren wanneer deze worden gebruikt. Met deze als onafhankelijke acties, behoudt de mogelijkheid voor het opstellen van deze zonder wordt gecombineerd met andere acties of de code in uw bot.

- Acties voor instellen-entiteit kunnen alleen worden gemaakt met betrekking tot een waarde van een entiteit enum daarom moet u eerst een enum-entiteit maken.
- Set entiteitacties zijn ook 'niet-await' omdat ze geen zichtbare uitvoer en worden gevolgd door een actie moeten 'wacht' de gebruiker kan zien.
- Acties voor instellen-entiteit zijn onveranderbare wat betekent dat u deze niet bewerken nadat is gemaakt.

![action_set_entity_creation.png](../media/tutorial-enum-set-entity/action_set_entity_creation.png)

### <a name="automatic-action-generation"></a>Automatische actie genereren

Als een enum-entiteit in uw model bestaat, wordt Conversatiecursist acties voor elk van de mogelijke waarden voor tijdelijke aanduiding maken en deze beschikbaar om te selecteren tijdens de training. Bij het selecteren, worden de actie wordt automatisch voor u gemaakt.

Bijvoorbeeld, als ik een enum-entiteit maken met de waarden "Ja" en "Nee":

![enum_entity_order_confirmation.png](../media/tutorial-enum-set-entity/enum_entity_order_confirmation.png)

Zelfs zonder expliciet het maken van acties voor deze nieuwe enum ziet u twee nieuwe acties die beschikbaar zijn tijdens de training:

![action_set_entity_sample.png](../media/tutorial-enum-set-entity/action_set_entity_sample.png)


## <a name="create-a-bot-using-these-new-features"></a>Maken van een Bot met behulp van deze nieuwe functies

### <a name="requirements"></a>Vereisten

In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

We gaan een bot om te simuleren fast food bestellen maken. Er discrete waarden voor de grootten van drank en fries (klein/gemiddeld/groot) en bevestiging vragen met Ja / geen antwoorden. Beide van deze entiteiten voldoen aan de regels van de twee hierboven worden contextafhankelijke antwoorden en vaste waarden.

### <a name="create-the-model"></a>Het model maken

1. In de web-UI, klikt u op 'Import'
2. Selecteer de zelfstudie met de naam '-zelfstudie-Enum-Set-entiteit'

Dit, u gaat u naar de pagina met het model management.
U ziet dat het model bevat al een paar enum-entiteiten en entiteitacties ingesteld.

### <a name="create-the-first-dialog"></a>Het eerste dialoogvenster maken

1. In het navigatievenster aan de linkerkant "Train-dialoogvensters", klik op de knop 'Nieuwe Train dialoogvenster'.
2. Als het gebruikerstype in, 'Hallo, ik wil een cokes en de fries.'.
3. Klik op de knop 'Score acties'

   > De gebruiker nog niet is opgegeven grootten voor de drank of fries dus moeten we hierover.

4. Selecteer de actie met de reactie: 'Welke grootte drank wilt u dat?'
5. Als de gebruiker typt, "grote"
6. Klik op de knop 'Score acties'
7. Selecteer de actie SET_ENTITY - ' drinkSize: GROTE"
8. Selecteer de actie met de reactie: "Wat is het formaat fries wilt u?"
9. Als het type gebruikt in 'Um, moet u deze een medium.
10. Klik op de knop 'Score acties'
11. Selecteer de actie SET_ENTITY - ' friesSize: GEMIDDELD'
12. Selecteer de actie met de reactie: "Wilt u alle kruiden?"
13. Als het type gebruiken in "Ja"
14. Klik op de knop 'Score acties'
15. Selecteer de actie SET_ENTITY - ' condimentsConfirmation: JA"
16. Selecteer de actie met de reactie: 'Oké, ik heb een bestelling voor een grote drank en gemiddeld fries. Klopt dat?'
17. Als het type gebruiken in "Ja"
18. Klik op de knop 'Score acties'
19. Selecteer de actie SET_ENTITY - ' orderConfirmation: JA"
20. Selecteer de actie met de reactie: "Het ordernummer van uw is ok, 58. Wacht hier."
21. Klik op 'Opslaan' om het dialoogvenster te sluiten

U hebt zojuist uw eerste dialoogvenster met ENUM-entiteiten en SET_ENTITY acties gemaakt. U kunt veel meer combinaties van de gebruiker gedeeltelijke informatie op te geven of te experimenteren met andere soorten vragen sluiten beëindigd.

> [!NOTE]
> Tijdens de training, ziet u tijdelijke aanduidingen voor de SET_ENTITY acties zoals
>
> ![action_set_entity_training.png](../media/tutorial-enum-set-entity/action_set_entity_training.png)
>
> maar als het maken van dialoogvensters of met behulp van geïmplementeerd bots niet zichtbaar voor gebruikers deze.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Alternatieve invoer](./10-alternative-inputs.md)

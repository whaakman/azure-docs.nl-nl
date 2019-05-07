---
title: Instellingen configureren
titleSuffix: Azure Cognitive Services
description: Configuratie van de service omvat hoe beloningen in de service wordt behandeld, hoe vaak de service worden verkend, hoe vaak het model is retrained en hoeveel gegevens worden opgeslagen.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: bce260d2a6b897d3e45b231df4eecd7ecb31ba31
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143243"
---
# <a name="personalizer-settings"></a>Instellingen voor personalizer

Configuratie van de service omvat hoe beloningen in de service wordt behandeld, hoe vaak de service worden verkend, hoe vaak het model is retrained en hoeveel gegevens worden opgeslagen.

## <a name="create-personalizer-resource"></a>Personalizer resource maken

Maak een resource Personalizer voor elke feedback-lus. 

1. Meld u aan bij de [Azure-portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer). De vorige koppeling gaat u naar de **maken** pagina voor de service Personlizer. 
1. Voer de servicenaam van uw, selecteert u een abonnement, locatie, dat is prijscategorie, en resourcegroep.
1. Selecteer de bevestiging en selecteer **maken**.

## <a name="configure-service-settings-in-the-azure-portal"></a>Service-instellingen configureren in Azure portal

1. Meld u aan bij [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer).
1. Uw Personalizer bron vinden. 
1. In de **resourcebeheer** sectie, selecteer **instellingen**.

    Voordat u de Azure-portal te verlaten, Kopieer een van de resource-sleutels van de **sleutels** pagina. U moet deze optie om te gebruiken de [Personalizer SDK](https://aka.ms/personalizer-dotnet-sdk-docs).

### <a name="configure-reward-settings-for-the-feedback-loop-based-on-use-case"></a>Instellingen voor de feedback-lus op basis van de use-case beloning configureren

Configureer instellingen voor uw feedback-lus gebruik van de voordelen van de service. Wijzigingen in de volgende instellingen worden opnieuw instellen van het huidige Personalizer-model en het opnieuw trainen met gegevens van de afgelopen 2 dagen:

![Configureer de instellingen van de prijs voor de feedback-lus](media/settings/configure-model-reward-settings.png)

|Instelling|Doel|
|--|--|
|Wachttijd van derden|Stelt de hoeveelheid tijd gedurende welke Personalizer beloning waarden voor een aanroep van de positie worden verzameld, starten vanaf het moment dat de positie aanroep gebeurt. Deze waarde is ingesteld door vragen te stellen: "Hoe lang moet Personalizer wachten gedurende beloningen aanroepen?" Een beloning nadat dit venster wordt geregistreerd maar niet voor learning gebruikt binnenkomen.|
|Standaard beloning|Als er zijn geen derden-gesprek wordt ontvangen door Personalizer tijdens de wachttijd van derden die zijn gekoppeld aan een positie aanroepen, Personalizer, de prijs standaard wordt toegewezen. Standaard, en in de meeste scenario's is de prijs standaard nul.|
|Aggregatie van derden|Als er meerdere beloningen worden ontvangen voor dezelfde positie API aanroepen, deze aggregatiemethode wordt gebruikt: **som** of **vroegste**. Zo spoedig hervat de vroegste score ontvangen en de rest wordt genegeerd. Dit is handig als u wilt dat unieke beloning tussen mogelijk dubbele aanroepen. |

Nadat u deze instellingen wijzigt, zorg ervoor dat u selecteert **opslaan**.

### <a name="exploration-setting"></a>De instelling verkennen 

Persoonlijke instellingen kan waarmee nieuwe patronen ontdekken en aangepast aan de gebruiker gedragswijzigingen na verloop van tijd door alternatieven verkennen. De **verkennen** instelling wordt bepaald welk percentage van positie-aanroepen worden beantwoord met verkennen. 

Wijzigingen in deze instelling wordt opnieuw instellen van het huidige Personalizer-model en het opnieuw trainen met gegevens van de afgelopen 2 dagen.

![De instelling van de verkenning bepaalt welk percentage van positie-aanroepen worden beantwoord met verkennen](media/settings/configure-exploration-setting.png)

Nadat u deze instelling wijzigt, zorg ervoor dat u selecteert **opslaan**.

### <a name="model-update-frequency"></a>De updatefrequentie model

**Model van de updatefrequentie** wordt ingesteld hoe vaak een nieuw model voor Personalizer is retrained. 

![Model updatefrequentie wordt ingesteld hoe vaak een nieuw model voor Personalizer is retrained.](media/settings/configure-model-update-frequency-settings.png)

Nadat u deze instelling wijzigt, zorg ervoor dat u selecteert **opslaan**.

### <a name="data-retention"></a>Bewaartijd van gegevens

**Bewaartermijn voor gegevens** Hiermee stelt u het aantal dagen Personalizer houdt gegevenslogboeken. Afgelopen gegevens logboeken zijn vereist om uit te voeren [offline evaluaties](concepts-offline-evaluation.md), die worden gebruikt voor meet de effectiviteit van Personalizer en optimaliseren van Learning beleid.

Nadat u deze instelling wijzigt, zorg ervoor dat u selecteert **opslaan**.

## <a name="export-the-personalizer-model"></a>Het model Personalizer exporteren

In de sectie het resourcebeheer voor **Model en het beleid**, modelen en Laatst bijgewerkt op controleren en exporteren van het huidige model.

![Huidige Personalizer model exporteren](media/settings/export-current-personalizer-model.png)

## <a name="import-and-export-learning-policy"></a>Importeren en exporteren van learning beleid

In de sectie het resourcebeheer voor **Model en het beleid**, een nieuwe learning-beleid importeren of exporteren van het huidige learning-beleid.

## <a name="next-steps"></a>Volgende stappen

[Het gebruik van de container Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409)

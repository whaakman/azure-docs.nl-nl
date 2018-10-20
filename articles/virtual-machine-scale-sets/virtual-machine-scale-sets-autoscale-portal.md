---
title: Automatisch schalen virtuele-machineschaalsets in Azure portal | Microsoft Docs
description: Hiermee stelt u het maken van regels voor automatisch schalen voor virtuele-machineschaalsets in Azure portal
services: virtual-machine-scale-sets
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: zarhoads
ms.openlocfilehash: 6ce737cb221b948a1cc25b8a441d3b623915662b
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466514"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-the-azure-portal"></a>Automatisch schalen van een virtuele-machineschaalset in Azure portal
Wanneer u een schaalset maakt, definieert u het aantal VM-exemplaren dat u wilt uitvoeren. Wanneer de vraag van de toepassing verandert, kunt u het aantal VM-exemplaren automatisch vergroten of verkleinen. De mogelijkheid van automatisch schalen stelt u in staat om altijd te voldoen aan de vraag van klanten houden of om gedurende de levenscyclus van uw app te reageren op wijzigingen in de prestaties van de toepassing.

Dit artikel ziet u hoe u regels voor automatisch schalen maakt in Azure portal waarmee de prestaties van de VM-exemplaren in uw schaalset controleren. Deze regels voor automatisch schalen vergroot of verklein het aantal VM-exemplaren in reactie op deze maatstaven voor prestaties. U kunt ook deze stappen voltooien met [Azure PowerShell](tutorial-autoscale-powershell.md) of de [Azure CLI](tutorial-autoscale-cli.md).


## <a name="prerequisites"></a>Vereisten
Voor het maken van regels voor automatisch schalen, moet u een bestaande virtuele machine schaalset. U kunt een schaalset maken met de [Azure-portal](quick-create-portal.md), [Azure PowerShell](quick-create-powershell.md), of [Azure CLI](quick-create-cli.md).


## <a name="create-a-rule-to-automatically-scale-out"></a>Maak een regel voor automatisch uitschalen
Als de vraag van uw toepassing toeneemt, neemt de belasting van de VM-exemplaren in de schaalset ook toe. Als deze toegenomen belasting consistent is, en geen piekbelasting is, kunt u regels voor automatisch schalen configureren om het aantal VM-exemplaren in de schaalset te verhogen. Wanneer deze VM-exemplaren worden gemaakt en uw toepassingen worden geïmplementeerd, zorgt de schaalset ervoor dat er via de load balancer verkeer wordt gedistribueerd naar de exemplaren. U bepaalt welke meetwaarden gegevens moeten worden bewaakt, zoals CPU of schijf, hoe lang de belasting van de toepassing overeen moet komen met een bepaalde drempelwaarde en hoeveel VM-exemplaren er moeten worden toegevoegd aan de schaalset.

1. Open de Azure portal en selecteer **resourcegroepen** in het menu aan de linkerkant van het dashboard.
2. Selecteer de resourcegroep die uw schaalset bevat, wordt uw schaalset kiezen uit de lijst met resources.
3. Kies **schalen** instellen in het menu aan de linkerkant van de schaal venster. Selecteer op de knop **automatisch schalen inschakelen**:

    ![Automatisch schalen in Azure portal inschakelen](media/virtual-machine-scale-sets-autoscale-portal/enable-autoscale.png)

4. Voer een naam op voor de instellingen, zoals *voor automatisch schalen*, selecteert u de optie om **toevoegen van een regel**.

5. We gaan een regel maken waarmee het aantal VM-exemplaren in een schaalset verhoogt wanneer de gemiddelde CPU-belasting gedurende een periode van 10 minuten groter dan 70% is. Wanneer de regel wordt geactiveerd, wordt het aantal VM-exemplaren met 20% verhoogd. In de schaalsets met een klein aantal VM-exemplaren, kunt u instellen de **bewerking** naar *aantal verhogen met* en geef vervolgens *1* of *2* voor de *aantal instanties*. In schaalsets met een groot aantal VM-instanties, een toename van 10% of % 20 VM-exemplaren mogelijk meer geschikt.

    Geef de volgende instellingen voor de regel:
    
    | Parameter              | Uitleg                                                                                                         | Waarde          |
    |------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
    | *Tijdverzameling*     | Hiermee definieert u hoe de verzamelde meetwaarden moeten worden samengevoegd voor analyse.                                                | Gemiddeld        |
    | *Naam van de meetwaarde*          | De prestatiemeetwaarde die u wilt bewaken en waarvoor u acties wilt toepassen op de schaalset.                                                   | Percentage CPU |
    | *Tijdsintervalstatistieken* | Hiermee definieert u hoe de verzamelde metrische gegevens in elk tijdsinterval moeten worden samengevoegd voor analyse.                             | Gemiddeld        |
    | *Operator*             | De operator die wordt gebruikt voor het vergelijken van de meetwaarden met de drempelwaarde.                                                     | Groter dan   |
    | *Drempelwaarde*            | Het percentage dat ervoor zorgt dat de regel voor automatisch schalen een actie activeert.                                                 | 70             |
    | *Duur*             | De hoeveelheid tijd waarna de meetwaarde en drempelwaarde met elkaar worden vergeleken.                                   | 10 minuten     |
    | *Bewerking*            | Hiermee definieert u of de schaalset moet omhoog of omlaag wanneer de regel wordt toegepast en door welke verhoging                        | Percentage verhogen met |
    | *Aantal exemplaren*       | Het percentage VM-exemplaren dat moet worden gewijzigd wanneer de regel wordt geactiveerd.                                            | 20             |
    | *Afkoelen (minuten)*  | De tijd die moet worden gewacht voordat de regel opnieuw wordt toegepast, zodat de acties voor automatisch schalen voldoende tijd hebben om effectief te zijn. | 5 minuten      |

    De volgende voorbeelden ziet een regel gemaakt in de Azure portal die overeenkomt met deze instellingen:

    ![Maak een regel voor automatisch schalen om het aantal VM-exemplaren te verhogen](media/virtual-machine-scale-sets-autoscale-portal/rule-increase.png)

6. Voor het maken van de regel, selecteer **toevoegen**


## <a name="create-a-rule-to-automatically-scale-in"></a>Maakt u een regel voor automatisch schalen in
In het weekend of 's avonds kan de vraag voor uw toepassing afnemen. Als deze afgenomen belasting consistent is gedurende een bepaalde periode, kunt u regels voor automatisch schalen configureren om het aantal VM-exemplaren in de schaalset te verlagen. Deze inschaalactie reduceert de kosten voor het uitvoeren van uw schaalset, aangezien u alleen het aantal exemplaren uitvoert dat vereist is om te voldoen aan de actuele vraag.

1. Kies aan **toevoegen van een regel** opnieuw.
2. Maak een regel die het aantal VM-exemplaren in een schaalset vermindert wanneer de gemiddelde CPU-belasting lager dan 30% gedurende een periode van 10 minuten. Wanneer de regel wordt geactiveerd, wordt het aantal VM-exemplaren met 20% verlaagd.

    Dezelfde benadering gebruiken net als bij de vorige regel. De volgende instellingen voor de regel aanpassen:
    
    | Parameter              | Uitleg                                                                                                          | Waarde          |
    |------------------------|----------------------------------------------------------------------------------------------------------------------|----------------|
    | *Operator*             | De operator die wordt gebruikt voor het vergelijken van de meetwaarden met de drempelwaarde.                                                      | Kleiner dan   |
    | *Drempelwaarde*            | Het percentage dat ervoor zorgt dat de regel voor automatisch schalen een actie activeert.                                                 | 30             |
    | *Bewerking*            | Hiermee definieert u of de schaalset moet omhoog of omlaag wanneer de regel wordt toegepast en door welke verhoging                         | Percentage verlagen met |
    | *Aantal exemplaren*       | Het percentage VM-exemplaren dat moet worden gewijzigd wanneer de regel wordt geactiveerd.                                             | 20             |

3. Voor het maken van de regel, selecteer **toevoegen**


## <a name="define-autoscale-instance-limits"></a>De limieten voor automatisch schalen definiëren
Uw profiel voor automatisch schalen moet definiëren een minimum, maximum- en standaardaantal VM-exemplaren. Wanneer uw regels voor automatisch schalen worden toegepast, deze limieten exemplaar, zorg dat u kan niet worden uitgebreid dan het maximum aantal exemplaren of schaal in meer dan het minimum van exemplaren.

1. De volgende limieten voor het exemplaar instellen:

    | Minimum | Maximum | Standaard|
    |---------|---------|--------|
    | 2       | 10      | 2      |

2. Selecteren om toe te passen aan uw regels voor automatisch schalen en limieten voor instanties, **opslaan**.


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Aantal exemplaren in een schaalset controleren
Als u wilt zien van het aantal en de status van VM-exemplaren, selecteer **exemplaren** instellen in het menu aan de linkerkant van de schaal venster. De status geeft aan of het VM-exemplaar *maken* als de schaalset automatisch uitgeschaald wordt, of is *verwijderen* tijdens de schaal automatisch schalen.

![Een overzicht van scale set VM-exemplaren](media/virtual-machine-scale-sets-autoscale-portal/view-instances.png)


## <a name="autoscale-based-on-a-schedule"></a>Automatisch schalen op basis van een planning
In de vorige voorbeelden wordt automatisch een schaalset in- of uitschalen met metrische gegevens zoals CPU-gebruik voor basic hosts geschaald. U kunt ook regels voor automatisch schalen op basis van schema's maken. Deze regels op basis van een planning kunnen u automatisch uitschalen als het aantal VM-instanties vooraf een verwachte toename van de vraag van toepassing, zoals core werkuren, en vervolgens automatisch schalen in het aantal exemplaren op een tijdstip dat u minder verwacht vraag, zoals het weekend.

1. Kies **schalen** instellen in het menu aan de linkerkant van de schaal venster. Als u wilt verwijderen van de bestaande regels voor automatisch schalen, in de vorige voorbeelden hebt gemaakt, kiest u het prullenbakpictogram.

    ![De bestaande regels voor automatisch schalen verwijderen](media/virtual-machine-scale-sets-autoscale-portal/delete-rules.png)

2. Kies aan **een schaalvoorwaarde toevoegen**. Selecteer het potloodpictogram naast de regelnaam van de en geef een naam, zoals *tijdens elke werkdag*.

    ![Wijzig de naam van de standaardregel voor automatisch schalen](media/virtual-machine-scale-sets-autoscale-portal/rename-rule.png)

3. Selecteer het keuzerondje **schalen naar een specifiek aantal instanties**.
4. Als u wilt het aantal instanties opschalen, voer *10* als het aantal instanties.
5. Kies **herhalen op specifieke dagen** voor de **planning** type.
6. Selecteer alle de werkdagen, maandag tot en met vrijdag.
7. Kies de juiste tijdzone, geeft u vervolgens een **begintijd** van *09:00*.
8. Kies aan **een schaalvoorwaarde toevoegen** opnieuw. Herhaal het proces voor het maken van een schema met de naam *schalen in tijdens de avonduren is gepland* die kan worden geschaald naar *3* exemplaren, wordt elke dag herhaald en begint bij *18:00*.
9. Als u wilt toepassen op uw regels voor automatisch schalen op basis van een planning, selecteer **opslaan**.

    ![Regels voor automatisch schalen die kunnen worden geschaald op basis van een schema maken](media/virtual-machine-scale-sets-autoscale-portal/schedule-autoscale.PNG)

Als u wilt zien hoe uw regels voor automatisch schalen worden toegepast, selecteer **Uitvoeringsgeschiedenis** aan de bovenkant van de **schalen** venster. De grafiek en gebeurtenissen van lijst met wordt weergegeven wanneer de trigger van de regels voor automatisch schalen en het aantal VM-exemplaren in uw schaal ingesteld toeneemt of afneemt.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u regels voor automatisch schalen voor horizontaal schalen en vergroten of verkleinen van de *getal* van VM-exemplaren in uw schaal ingesteld. U kunt ook verticaal schalen om te vergroten of verkleinen van het VM-exemplaar *grootte*. Zie voor meer informatie, [verticaal automatisch schalen met Virtual Machine Scale sets](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Zie voor meer informatie over het beheren van uw VM-exemplaren [beheren virtuele-machineschaalsets met Azure PowerShell](virtual-machine-scale-sets-windows-manage.md).

Zie voor informatie over waarschuwingen worden gegenereerd wanneer de functie voor automatisch schalen regels trigger gaat [acties voor automatisch schalen gebruiken voor het verzenden van e-mail en webhook-meldingen van waarschuwingen in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). U kunt ook [auditlogboeken gebruiken voor het verzenden van e-mail en webhook-meldingen van waarschuwingen in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).

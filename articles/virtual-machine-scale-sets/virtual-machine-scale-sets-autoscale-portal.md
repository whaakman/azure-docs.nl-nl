---
title: Automatisch schalen virtuele-machineschaalsets in de Azure portal | Microsoft Docs
description: Het maken van regels voor automatisch schalen voor virtuele-machineschaalset ingesteld in de Azure portal
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
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
ms.author: iainfou
ms.openlocfilehash: c9386f7dd0ba390a5f089be058c7f3edd6e33cf9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652369"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-the-azure-portal"></a>Automatisch schalen van een virtuele-machineschaalset ingesteld in de Azure portal
Wanneer u een schaalset maakt, definieert u het aantal VM-exemplaren dat u wilt uitvoeren. Wanneer de vraag van de toepassing verandert, kunt u het aantal VM-exemplaren automatisch vergroten of verkleinen. De mogelijkheid van automatisch schalen stelt u in staat om altijd te voldoen aan de vraag van klanten houden of om gedurende de levenscyclus van uw app te reageren op wijzigingen in de prestaties van de toepassing.

In dit artikel leest u hoe automatisch schalen om regels te maken in de Azure portal die de prestaties van de VM-exemplaren in de schaalset controleren. Deze regels voor automatisch schalen vergroten of verkleinen het aantal VM-exemplaren in reactie op deze maatstaven voor prestaties. U kunt ook deze stappen voltooien met [Azure PowerShell](tutorial-autoscale-powershell.md) of de [Azure CLI 2.0](tutorial-autoscale-cli.md).


## <a name="prerequisites"></a>Vereisten
Voor het maken van regels voor automatisch schalen, moet u een bestaande virtuele machine schaalset. U kunt een instellen met schaal maken de [Azure-portal](quick-create-portal.md), [Azure PowerShell](quick-create-powershell.md), of [Azure CLI 2.0](quick-create-cli.md).


## <a name="create-a-rule-to-automatically-scale-out"></a>Maak een regel automatisch uit te schalen
Als de vraag van uw toepassing toeneemt, neemt de belasting van de VM-exemplaren in de schaalset ook toe. Als deze toegenomen belasting consistent is, en geen piekbelasting is, kunt u regels voor automatisch schalen configureren om het aantal VM-exemplaren in de schaalset te verhogen. Wanneer deze VM-exemplaren worden gemaakt en uw toepassingen worden geïmplementeerd, zorgt de schaalset ervoor dat er via de load balancer verkeer wordt gedistribueerd naar de exemplaren. U bepaalt welke meetwaarden gegevens moeten worden bewaakt, zoals CPU of schijf, hoe lang de belasting van de toepassing overeen moet komen met een bepaalde drempelwaarde en hoeveel VM-exemplaren er moeten worden toegevoegd aan de schaalset.

1. Open de Azure portal en selecteert u een **resourcegroepen** in het menu aan de linkerkant van het dashboard.
2. Selecteer de resourcegroep die uw schaalset bevat, wordt uw schaal instelt kiezen uit de lijst met resources.
3. Kies **schaal** ingesteld in het menu aan de linkerkant van de schaal venster. Klik op de knop om **inschakelen voor automatisch schalen**:

    ![Schakel automatisch schalen in Azure portal](media/virtual-machine-scale-sets-autoscale-portal/enable-autoscale.png)

4. Voer een naam voor uw instellingen, zoals *automatisch schalen*, selecteert u de optie om **een regel toevoegen**.

5. We gaan een regel maken waarmee het aantal VM-exemplaren in een schaal ingesteld verhoogt wanneer de gemiddelde CPU-belasting groter dan 70% gedurende een periode 10 minuten is. Wanneer de regel wordt geactiveerd, wordt het aantal VM-instanties verhoogd met 20%. In met een klein aantal exemplaren van de VM-schaalsets, stelt u kan de **bewerking** naar *verhogen tellen per* en geef vervolgens *1* of *2* voor de *exemplaar aantal*. In-schaalsets met een groot aantal VM-instanties, een toename van 10% of % 20 VM-instanties mogelijk meer geschikt is.

    Geef de volgende instellingen voor de regel:
    
    | Parameter              | Uitleg                                                                                                         | Waarde          |
    |------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
    | *Tijdaggregatie*     | Hiermee definieert u hoe de verzamelde meetwaarden moeten worden samengevoegd voor analyse.                                                | Gemiddeld        |
    | *Metrische naam*          | De prestatiemeetwaarde die u wilt bewaken en waarvoor u acties wilt toepassen op de schaalset.                                                   | Percentage CPU |
    | *Tijd gebruikerssegmentatie statistiek* | Hiermee definieert u hoe de verzamelde metrische gegevens in elke tijdgranulariteit moeten worden samengevoegd voor analyse.                             | Gemiddeld        |
    | *Operator*             | De operator die wordt gebruikt voor het vergelijken van de meetwaarden met de drempelwaarde.                                                     | Groter dan   |
    | *Drempelwaarde*            | Het percentage dat ervoor zorgt de regel voor automatisch schalen dat voor het activeren van een actie.                                                 | 70             |
    | *Duur*             | De hoeveelheid tijd waarna de meetwaarde en drempelwaarde met elkaar worden vergeleken.                                   | 10 minuten     |
    | *Bewerking*            | Hiermee wordt aangegeven of de schaalaanpassingsset moet schaal omhoog of omlaag wanneer de regel geldt, en door welke verhoging                        | Percentage verhogen met |
    | *Aantal exemplaren*       | Het percentage VM-exemplaren dat moet worden gewijzigd wanneer de regel wordt geactiveerd.                                            | 20             |
    | *Cool omlaag (minuten)*  | De tijd die moet worden gewacht voordat de regel opnieuw wordt toegepast, zodat de acties voor automatisch schalen voldoende tijd hebben om effectief te zijn. | 5 minuten      |

    De volgende voorbeelden ziet een regel gemaakt in de Azure portal die overeenkomt met deze instellingen:

    ![Een regel voor automatisch schalen voor een verhoging van het aantal VM-instanties maken](media/virtual-machine-scale-sets-autoscale-portal/rule-increase.png)

6. Voor het maken van de regel, selecteer **toevoegen**


## <a name="create-a-rule-to-automatically-scale-in"></a>Maak een regel automatisch schalen in
In het weekend of 's avonds kan de vraag voor uw toepassing afnemen. Als deze afgenomen belasting consistent is gedurende een bepaalde periode, kunt u regels voor automatisch schalen configureren om het aantal VM-exemplaren in de schaalset te verlagen. Deze inschaalactie reduceert de kosten voor het uitvoeren van uw schaalset, aangezien u alleen het aantal exemplaren uitvoert dat vereist is om te voldoen aan de actuele vraag.

1. Kies voor **een regel toevoegen** opnieuw.
2. Maak een regel die het aantal VM-exemplaren in een schaal ingesteld vermindert wanneer de gemiddelde CPU-belasting vervolgens onder 30% gedurende een periode 10 minuten zakt. Als de regel wordt gegenereerd, wordt het aantal VM-exemplaren van 20% verminderd.

    Gebruik dezelfde manier als met de vorige regel. De volgende instellingen voor de regel aanpassen:
    
    | Parameter              | Uitleg                                                                                                          | Waarde          |
    |------------------------|----------------------------------------------------------------------------------------------------------------------|----------------|
    | *Operator*             | De operator die wordt gebruikt voor het vergelijken van de meetwaarden met de drempelwaarde.                                                      | Kleiner dan   |
    | *Drempelwaarde*            | Het percentage dat ervoor zorgt de regel voor automatisch schalen dat voor het activeren van een actie.                                                 | 30             |
    | *Bewerking*            | Hiermee wordt aangegeven of de schaalaanpassingsset moet schaal omhoog of omlaag wanneer de regel geldt, en door welke verhoging                         | Percentage verlagen met |
    | *Aantal exemplaren*       | Het percentage VM-exemplaren dat moet worden gewijzigd wanneer de regel wordt geactiveerd.                                             | 20             |

3. Voor het maken van de regel, selecteer **toevoegen**


## <a name="define-autoscale-instance-limits"></a>Definieer de limieten voor automatisch schalen
Uw profiel automatisch schalen moet een minimum, maximum en standaardaantal exemplaren van de virtuele machine definiëren. Wanneer uw regels voor automatisch schalen die worden toegepast, deze limieten exemplaar, zorg dat u mag niet worden uitgebreid dan het maximum aantal exemplaren of schaal in dan het minimum van exemplaren.

1. Stel de limieten voor het volgende:

    | Minimum | Maximum | Standaard|
    |---------|---------|--------|
    | 2       | 10      | 2      |

2. Als uw regels voor automatisch schalen en limieten toepassen, selecteert u **opslaan**.


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Aantal exemplaren in een set scale bewaken
Selecteer het aantal en de status van de VM-instanties vindt **exemplaren** ingesteld in het menu aan de linkerkant van de schaal venster. De status wordt aangegeven of de VM-instantie *maken* als de schaal automatisch uitgeschaald of is *verwijderen* tijdens de schaal automatisch schalen.

![Een overzicht van scale set VM-exemplaren](media/virtual-machine-scale-sets-autoscale-portal/view-instances.png)


## <a name="autoscale-based-on-a-schedule"></a>Op basis van een schema voor automatisch schalen
De eerdere voorbeelden automatisch geschaald op een schaal in- of instellen met basic host metrische gegevens zoals CPU-gebruik. U kunt ook regels voor automatisch schalen op basis van schema's maken. Deze regels op basis van een planning kunnen u automatisch schalen het aantal VM-instanties van tevoren een verwachte toename van de vraag van de toepassing, zoals core werkuren, en vervolgens automatisch schalen in het aantal exemplaren op een tijdstip dat u minder verwacht vraag, zoals het weekend.

1. Kies **schaal** ingesteld in het menu aan de linkerkant van de schaal venster. Kies het pictogram voor de Prullenbak voor het verwijderen van de bestaande regels automatisch schalen die zijn gemaakt in de voorgaande voorbeelden.

    ![Verwijder de bestaande regels voor automatisch schalen](media/virtual-machine-scale-sets-autoscale-portal/delete-rules.png)

2. Kies voor **een scale-voorwaarde toevoegen**. Selecteer het potloodpictogram naast de regelnaam van de en geef een naam, zoals *uitschalen tijdens elke werkdag*.

    ![Wijzig de naam van de standaardregel voor automatisch schalen](media/virtual-machine-scale-sets-autoscale-portal/rename-rule.png)

3. Selecteer het keuzerondje **aanpassen aan een specifieke exemplaren**.
4. Als u wilt schalen van het aantal exemplaren, voer *10* als het aantal exemplaren.
5. Kies **specifieke dagen herhalen** voor de **planning** type.
6. Selecteer alle de werkdagen, maandag tot en met vrijdag.
7. Kies de juiste tijdzone en geef een **begintijd** van *09:00*.
8. Kies voor **een scale-voorwaarde toevoegen** opnieuw. Herhaal dit proces voor het maken van een schema met de naam *schalen tijdens de avond* die kan worden geschaald naar *3* exemplaren en begint bij wordt herhaald elke weekdag *18:00*.
9. Als uw regels op basis van het schema voor automatisch schalen toepassen, selecteert u **opslaan**.

    ![Regels voor automatisch schalen die schalen volgens een schema maken](media/virtual-machine-scale-sets-autoscale-portal/schedule-autoscale.PNG)

Selecteer om te zien hoe uw regels voor automatisch schalen die zijn toegepast, **Uitvoeringsgeschiedenis** aan de bovenkant van de **schaal** venster. De grafiek en gebeurtenissen lijst staat als de trigger van de regels voor automatisch schalen en het aantal VM-exemplaren in uw scale ingesteld toename of afname.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u regels voor automatisch schalen gebruiken om horizontaal schalen en vergroten of verkleinen de *getal* van VM-exemplaren in uw scale ingesteld. Kunt u ook de schaal verticaal vergroten of verkleinen van de VM-instantie *grootte*. Zie voor meer informatie [verticale automatisch geschaald met de virtuele Machine-schaalsets](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Zie voor meer informatie over het beheren van uw VM-exemplaren [beheren virtuele-machineschaalsets met Azure PowerShell](virtual-machine-scale-sets-windows-manage.md).

Zie voor meer informatie over hoe waarschuwingen worden gegenereerd wanneer uw automatisch schalen regels trigger, [acties automatisch schalen gebruiken voor het verzenden van e-mail en -webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). U kunt ook [controlelogboeken voor gebruik met het verzenden van e-mail en webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).

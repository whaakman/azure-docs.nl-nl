---
title: Automatisch schalen virtuele-machineschaalsets in de Azure portal | Microsoft Docs
description: Het maken van regels voor automatisch schalen voor virtuele-machineschaalset ingesteld in de Azure portal
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: e43be53817e7fa65c3d7a95cab9821126ed88831
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-the-azure-portal"></a>Automatisch schalen van een virtuele-machineschaalset ingesteld in de Azure portal
Wanneer u een schaalset maakt, definieert u het aantal VM-exemplaren die u wilt uitvoeren. Als uw toepassing vraag verandert, kunt u automatisch vergroten of verkleinen van het aantal VM-exemplaren. De mogelijkheid om te schalen kunt u met de vraag van klanten houden of reageren op wijzigingen in de toepassingsprestaties gedurende de levenscyclus van uw app.

In dit artikel leest u hoe automatisch schalen om regels te maken in de Azure portal die de prestaties van de VM-exemplaren in de schaalset controleren. Deze regels voor automatisch schalen vergroten of verkleinen het aantal VM-exemplaren in reactie op deze maatstaven voor prestaties. U kunt ook deze stappen voltooien met [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md) of de [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md).


## <a name="prerequisites"></a>Vereisten
Voor het maken van regels voor automatisch schalen, moet u een bestaande virtuele machine schaalset. U kunt een instellen met schaal maken de [Azure-portal](virtual-machine-scale-sets-create-portal.md), [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md), of [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md).


## <a name="create-a-rule-to-automatically-scale-out"></a>Maak een regel automatisch uit te schalen
Als uw toepassing vraag toeneemt stelt de belasting van de VM-exemplaren in uw scale toeneemt. Als deze toegenomen belasting consistent is, in plaats van slechts een korte vraag, kunt u regels voor automatisch schalen voor een verhoging van het aantal VM-exemplaren in de schaalset configureren. Wanneer deze VM-exemplaren zijn gemaakt en uw toepassingen worden geïmplementeerd, wordt de schaalaanpassingsset begint met het distribueren van verkeer via de load balancer. U bepaalt welke metrische gegevens moeten worden bewaakt, zoals CPU of schijf, hoe lang het laden van toepassingen moet voldoen aan een bepaalde drempelwaarde en instellen hoeveel exemplaren van virtuele machine toevoegen aan de schaal.

1. Open de Azure portal en selecteert u een **resourcegroepen** in het menu aan de linkerkant van het dashboard.
2. Selecteer de resourcegroep die uw schaalset bevat, wordt uw schaal instelt kiezen uit de lijst met resources.
3. Kies **schaal** ingesteld in het menu aan de linkerkant van de schaal venster. Klik op de knop om **inschakelen voor automatisch schalen**:

    ![Schakel automatisch schalen in Azure portal](media/virtual-machine-scale-sets-autoscale-portal/enable-autoscale.png)

4. Voer een naam voor uw instellingen, zoals *automatisch schalen*, selecteert u de optie om **een regel toevoegen**.

5. We gaan een regel maken waarmee het aantal VM-exemplaren in een schaal ingesteld verhoogt wanneer de gemiddelde CPU-belasting groter dan 70% gedurende een periode 10 minuten is. Wanneer de regel wordt geactiveerd, wordt het aantal VM-instanties verhoogd met 20%. In met een klein aantal exemplaren van de VM-schaalsets, stelt u kan de **bewerking** naar *verhogen tellen per* en geef vervolgens *1* of *2* voor de *exemplaar aantal*. In-schaalsets met een groot aantal VM-instanties, een toename van 10% of % 20 VM-instanties mogelijk meer geschikt is.

    Geef de volgende instellingen voor de regel:
    
    | Parameter              | Uitleg                                                                                                         | Waarde          |
    |------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
    | *Tijdaggregatie*     | Hiermee definieert u hoe de verzamelde metrische gegevens moeten worden samengevoegd voor analyse.                                                | Gemiddeld        |
    | *Metrische naam*          | De metriek prestaties bewaken en schalen van toepassing instellen acties op.                                                   | Percentage CPU |
    | *Tijd gebruikerssegmentatie statistiek* | Hiermee definieert u hoe de verzamelde metrische gegevens in elke tijdgranulariteit moeten worden samengevoegd voor analyse.                             | Gemiddeld        |
    | *Operator*             | De operator is gebruikt voor het vergelijken van de metrische gegevens tegen de drempelwaarde.                                                     | Groter dan   |
    | *Drempelwaarde*            | Het percentage dat ervoor zorgt de regel voor automatisch schalen dat voor het activeren van een actie.                                                 | 70             |
    | *Duur*             | De hoeveelheid tijd gecontroleerd voordat de metrische gegevens en drempelwaarde waarden worden vergeleken.                                   | 10 minuten     |
    | *Bewerking*            | Hiermee wordt aangegeven of de schaalaanpassingsset moet schaal omhoog of omlaag wanneer de regel geldt, en door welke verhoging                        | Percentage verhogen met |
    | *Aantal exemplaren*       | Het percentage van de VM-exemplaren moet worden gewijzigd wanneer de regel wordt geactiveerd.                                            | 20             |
    | *Cool omlaag (minuten)*  | De hoeveelheid tijd moet worden gewacht voordat de regel wordt opnieuw toegepast zodat de acties voor automatisch schalen die tijd hebt om te laten treden. | 5 minuten      |

    De volgende voorbeelden ziet een regel gemaakt in de Azure portal die overeenkomt met deze instellingen:    

    ![Een regel voor automatisch schalen voor een verhoging van het aantal VM-instanties maken](media/virtual-machine-scale-sets-autoscale-portal/rule-increase.png)

6. Voor het maken van de regel, selecteer **toevoegen**


## <a name="create-a-rule-to-automatically-scale-in"></a>Maak een regel automatisch schalen in
Op een 's avonds of het weekend en kan uw aanvraag toepassing afnemen. Als deze minder belasting gedurende een periode consistent is, kunt u regels voor automatisch schalen Verminder het aantal VM-exemplaren in de schaalset configureren. Deze actie schaal in reduceert de kosten voor het uitvoeren van uw scale instellen als u alleen het aantal exemplaren is vereist om te voldoen aan de huidige aanvraag uitvoeren.

1. Kies voor **een regel toevoegen** opnieuw.
2. Maak een regel die het aantal VM-exemplaren in een schaal ingesteld vermindert wanneer de gemiddelde CPU-belasting vervolgens onder 30% gedurende een periode 10 minuten zakt. Als de regel wordt gegenereerd, wordt het aantal VM-exemplaren van 20% verminderd.

    Gebruik dezelfde manier als met de vorige regel. De volgende instellingen voor de regel aanpassen:
    
    | Parameter              | Uitleg                                                                                                          | Waarde          |
    |------------------------|----------------------------------------------------------------------------------------------------------------------|----------------|
    | *Operator*             | De operator is gebruikt voor het vergelijken van de metrische gegevens tegen de drempelwaarde.                                                      | Kleiner dan   |
    | *Drempelwaarde*            | Het percentage dat ervoor zorgt de regel voor automatisch schalen dat voor het activeren van een actie.                                                 | 30             |
    | *Bewerking*            | Hiermee wordt aangegeven of de schaalaanpassingsset moet schaal omhoog of omlaag wanneer de regel geldt, en door welke verhoging                         | Percentage verlagen met |
    | *Aantal exemplaren*       | Het percentage van de VM-exemplaren moet worden gewijzigd wanneer de regel wordt geactiveerd.                                             | 20             |

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

---
title: Automatisch een cloudservice schalen in de portal | Microsoft Docs
description: Informatie over het gebruik van de portal voor het configureren van regels voor automatisch schalen voor een cloud service-Webrol of werkrol in Azure.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeconnoc
ms.openlocfilehash: 29ee71e7946145e50cc875df96b674abec3e12df
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004333"
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-portal"></a>Functie voor automatisch schalen voor een Cloudservice in de portal configureren

Voorwaarden kunnen worden ingesteld voor de werkrol van een cloud-service die een schaalset in- of opnieuw activeren. De voorwaarden voor de rol kunnen worden gebaseerd op de CPU, schijf of netwerkbelasting van de rol. U kunt ook een voorwaarde op basis van een berichtenwachtrij of de metrische gegevens van een andere Azure-resource die is gekoppeld aan uw abonnement instellen.

> [!NOTE]
> In dit artikel richt zich op een Cloud Service-web- en werkrollen rollen. Wanneer u een virtuele machine (klassiek) rechtstreeks maakt, wordt deze wordt gehost in een cloudservice. U kunt een standaard virtuele machine schalen door te koppelen met een [beschikbaarheidsset](../virtual-machines/windows/classic/configure-availability-classic.md) en handmatig inschakelen of uitschakelen.

## <a name="considerations"></a>Overwegingen
U kunt de volgende informatie moet overwegen voordat u configureert voor uw toepassing schalen:

* Schalen wordt beïnvloed door core-gebruik.

    Instanties van de grotere gebruiken meer kernen. U kunt een toepassing alleen binnen de limiet van kernen schalen voor uw abonnement. Stel bijvoorbeeld dat uw abonnement heeft een limiet van 20 kernen. Als u een toepassing met twee middelgrote cloudservices (een totaal van 4 kernen) uitvoert, kunt u alleen schalen van andere implementaties van cloud service in uw abonnement door de resterende 16 kernen. Zie voor meer informatie over de grootte van [Cloud Service Sizes](cloud-services-sizes-specs.md).

* U kunt schalen op basis van een wachtrij bericht drempelwaarde. Zie voor meer informatie over het gebruik van wachtrijen [over het gebruik van de Service voor wachtrijopslag](../storage/queues/storage-dotnet-how-to-use-queues.md).

* U kunt ook andere resources die zijn gekoppeld aan uw abonnement schalen.

* Om in te schakelen hoge beschikbaarheid van uw toepassing, moet u ervoor zorgen dat deze is geïmplementeerd met twee of meer rolinstanties. Zie voor meer informatie, [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

* Automatisch schalen gebeurt alleen als alle rollen **gereed** staat.  


## <a name="where-scale-is-located"></a>Waar bevindt zich schaal
Nadat u uw cloudservice hebt geselecteerd, moet u de cloud service-blade zichtbaar hebben.

1. Op de blade cloud-service op de **rollen en instanties** tegel, selecteert u de naam van de cloudservice.   
   **BELANGRIJKE**: Zorg ervoor dat u de rol van cloud service, niet de rolinstantie die lager is dan de rol.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. Selecteer de **schaal** tegel.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatisch schalen
U kunt instellingen voor een rol configureren met de twee modi **handmatige** of **automatische**. Handleiding is zoals u zou verwachten, u stelt u het absolute aantal exemplaren. Automatische echter kunt u setregels die hoe en hoe u veel u bepalen moet worden geschaald.

Stel de **schalen door** optie naar **regels voor planning en prestaties**.

![Instellingen voor cloud services schalen met het profiel en de regel](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Een bestaand profiel.
2. Een regel voor het profiel van de bovenliggende toevoegen.
3. Voeg een ander profiel.

Selecteer **profiel toevoegen**. Het profiel bepaalt welke modus die u wilt gebruiken voor de schaal: **altijd**, **terugkeerpatroon**, **vaste datum**.

Nadat u het profiel en de regels hebt geconfigureerd, selecteer de **opslaan** pictogram aan de bovenkant.

#### <a name="profile"></a>Profiel
Hiermee stelt u het profiel minimum- en maximumaantal exemplaren voor de schaal aan, en ook wanneer het bereik van deze actief is.

* **Altijd**

    Bewaar dit bereik van de exemplaren beschikbaar.  

    ![Cloudservice die altijd schalen](./media/cloud-services-how-to-scale-portal/select-always.png)
* **Terugkeerpatroon**

    Kies een set van de dagen van de week om te schalen.

    ![Cloud service schalen met een terugkerend schema](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* **Vaste datum**

    Het bereik van een vaste datum voor het schalen van de rol.

    ![CLoud service schalen met een vaste datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Nadat u het profiel hebt geconfigureerd, selecteer de **OK** knop onderaan de profielblade.

#### <a name="rule"></a>Regel
Regels worden toegevoegd aan een profiel en vertegenwoordigen een voorwaarde die de schaal wordt geactiveerd.

De trigger van de regel is gebaseerd op een metrische waarde van de cloudservice (CPU-gebruik, activiteit van de schijf of activiteit op het netwerk) waaraan u een waarde voor de voorwaarde kunt toevoegen. Daarnaast kunt u de trigger op basis van een berichtenwachtrij of de metrische gegevens van een andere Azure-resource die is gekoppeld aan uw abonnement hebt.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Nadat u de regel hebt geconfigureerd, selecteer de **OK** knop onderaan de blade van de regel.

## <a name="back-to-manual-scale"></a>Terug naar handmatig schalen
Navigeer naar de [schalen instellingen](#where-scale-is-located) en stel de **schalen door** optie naar **het aantal instanties dat ik handmatig heb ingesteld**.

![Instellingen voor cloud services schalen met het profiel en de regel](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Deze instelling verwijdert de functie voor automatisch schalen van de rol en klikt u vervolgens kunt u het aantal instanties rechtstreeks instellen.

1. De schaal-optie voor (handmatig of automatisch).
2. Een rol exemplaar schuifregelaar om in te stellen van de exemplaren voor het schalen.
3. Exemplaren van de rol voor het schalen.

Nadat u de instellingen hebt geconfigureerd, selecteer de **opslaan** pictogram aan de bovenkant.

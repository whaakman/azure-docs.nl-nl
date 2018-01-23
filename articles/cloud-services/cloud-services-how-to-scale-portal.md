---
title: Een cloudservice automatisch schalen in de portal | Microsoft Docs
description: Informatie over het gebruik van de portal voor het configureren van regels voor automatisch schalen voor een cloud service-web-rol of functie worker in Azure.
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 0eea38cdb9827ab6e322025ff344ebbab0e83da3
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-portal"></a>Het automatisch schalen voor een Cloudservice in de portal configureren

Voorwaarden kunnen worden ingesteld voor een cloud service-werkrol die een schaal in- of opnieuw activeren. De voorwaarden voor de rol kunnen worden gebaseerd op de CPU, schijf of netwerkbelasting van de rol. U kunt ook een voorwaarde op basis van een berichtenwachtrij of de metric van sommige andere Azure-resource die is gekoppeld aan uw abonnement instellen.

> [!NOTE]
> Dit artikel is gericht op Cloud Service-web- en werkrollen rollen. Wanneer u een virtuele machine (klassiek) rechtstreeks maakt, wordt deze gehost in een cloudservice. U kunt een standaard virtuele machine schalen door te koppelen met een [beschikbaarheidsset](../virtual-machines/windows/classic/configure-availability-classic.md) en handmatig inschakelen of uitschakelen.

## <a name="considerations"></a>Overwegingen
U kunt de volgende informatie voordat u configureert voor uw toepassing schalen:

* Schalen wordt beïnvloed door de belangrijkste informatie over het gebruik.

    Grotere rolinstanties gebruik meer kernen. U kunt een toepassing alleen binnen de limiet van kernen schalen voor uw abonnement. Bijvoorbeeld, Stel dat uw abonnement heeft een limiet van 20 kernen. Als u een toepassing met twee middelgrote cloudservices (in totaal 4 kernen) uitvoert, kunt u alleen opschalen andere cloud service-implementaties in uw abonnement door de resterende 16 kernen. Zie voor meer informatie over grootten [Cloud Service Sizes](cloud-services-sizes-specs.md).

* U kunt schalen op basis van een wachtrij bericht drempelwaarde. Zie voor meer informatie over het gebruik van wachtrijen [het gebruik van de Queue Storage-Service](../storage/queues/storage-dotnet-how-to-use-queues.md).

* U kunt ook andere resources zijn gekoppeld aan uw abonnement schalen.

* Om hoge beschikbaarheid van uw toepassing, moet u ervoor zorgen dat deze wordt geïmplementeerd met twee of meer rolinstanties. Zie voor meer informatie [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

* Automatische schaling gebeurt alleen als alle rollen zijn in **gereed** status.  


## <a name="where-scale-is-located"></a>Waar schaal bevindt
Nadat u uw cloudservice selecteert, moet u de cloud service-blade zichtbaar hebben.

1. Op de blade cloud-service op de **rollen en instanties** tegel, selecteert u de naam van de cloudservice.   
   **BELANGRIJKE**: Zorg ervoor dat u niet de rolinstantie die lager is dan de rol van de cloud service rol.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. Selecteer de **scale** tegel.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatische schaal
U kunt instellingen voor een rol configureren met de twee modi **handmatige** of **automatische**. Handmatige is zoals u zou verwachten, instellen van het absolute aantal exemplaren. Automatische echter kunt u naar de setregels die hoe en door het veel u bepalen moet de schaal.

Stel de **schalen door** optie om te **planning en prestaties regels**.

![Cloud services-schaalinstellingen met profiel en de regel](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Een bestaand profiel.
2. Een regel voor het profiel van de bovenliggende toevoegen.
3. Voeg een ander profiel.

Selecteer **profiel**. Het profiel bepaalt welke modus die u wilt gebruiken voor de schaal: **altijd**, **terugkeerpatroon**, **vaste datum**.

Nadat u het profiel en de regels hebt geconfigureerd, selecteert u de **opslaan** bovenin.

#### <a name="profile"></a>Profiel
Hiermee stelt u het profiel minimum- en maximumaantal exemplaren voor de schaal aan, en ook als het bereik van deze actief is.

* **Altijd**

    Bewaar dit bereik van de exemplaren beschikbaar.  

    ![Cloudservice, die altijd schalen](./media/cloud-services-how-to-scale-portal/select-always.png)
* **Terugkeerpatroon**

    Kies een reeks dagen van de week te schalen.

    ![Cloud-service schalen met een terugkerende planning](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* **Vaste datum**

    Een vaste datumbereik voor het schalen van de rol.

    ![CLoud-service schalen met een vaste datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Nadat u het profiel hebt geconfigureerd, selecteert u de **OK** knop onderaan de blade-profiel.

#### <a name="rule"></a>Regel
Regels worden toegevoegd aan een profiel en vertegenwoordigen een voorwaarde waarmee de schaal wordt geactiveerd.

De trigger regel is gebaseerd op een waarde van de cloudservice (CPU-gebruik, schijfactiviteit of netwerkactiviteit) waaraan u een waarde voor de voorwaarde kunt toevoegen. U kunt ook de trigger op basis van een berichtenwachtrij of de metric van sommige andere Azure-resource die is gekoppeld aan uw abonnement hebben.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Nadat u de regel hebt geconfigureerd, selecteert u de **OK** knop onderaan de blade regel op.

## <a name="back-to-manual-scale"></a>Terug naar handmatig schalen
Navigeer naar de [schalen instellingen](#where-scale-is-located) en stel de **schalen door** optie naar **aantal instanties dat ik handmatig invoeren**.

![Cloud services-schaalinstellingen met profiel en de regel](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Deze instelling verwijdert automatisch schalen uit de rol en vervolgens kunt u het aantal exemplaren rechtstreeks instellen.

1. De scale-optie voor (handmatig of automatisch).
2. Een rol exemplaar schuifregelaar om in te stellen van de exemplaren te schalen.
3. Exemplaren van de rol te schalen.

Nadat u de instellingen hebt geconfigureerd, selecteert u de **opslaan** bovenin.

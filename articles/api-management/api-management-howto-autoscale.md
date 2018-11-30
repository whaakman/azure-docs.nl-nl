---
title: Automatisch schalen van een Azure API Management-exemplaar configureren | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe u het gedrag voor automatisch schalen voor een Azure API Management-exemplaar kunt instellen.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: a01e50debf11daf2f1163a56726f5574f7e3e379
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52444799"
---
# <a name="automatically-scale-an-azure-api-management-instance"></a>Automatisch schalen van een Azure API Management-exemplaar  

Azure API Management service-exemplaar kan automatisch worden geschaald op basis van een set regels. Dit gedrag kan worden ingeschakeld en geconfigureerd via Azure Monitor en wordt alleen ondersteund in **Standard** en **Premium** segment van de Azure API Management-service.

Het artikel wordt uitgelegd hoe van de configuratie voor automatisch schalen en stelt de optimale configuratie van regels voor automatisch schalen.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen de stappen in dit artikel, moet u:

+ Een actief Azure-abonnement hebben.
+ Een Azure API Management-exemplaar hebben. Zie voor meer informatie, [maken van een Azure API Management-exemplaar](get-started-create-service-instance.md).
+ Informatie over het concept van [capaciteit van een Azure API Management-exemplaar](api-management-capacity.md).
+ Inzicht in [handmatig proces van een Azure API Management-exemplaar schalen](upgrade-and-scale.md), met inbegrip van consequenties van kosten.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="azure-api-management-autoscale-limitations"></a>Beperkingen van Azure API Management-functie voor automatisch schalen

Bepaalde beperkingen en de gevolgen van het vergroten/verkleinen beslissingen moeten worden opgelost voordat gedrag voor automatisch schalen configureren.

+ Automatisch schalen kan alleen worden ingeschakeld voor **Standard** en **Premium** lagen van Azure API Management-service.
+ Prijscategorieën worden ook het maximum aantal eenheden voor een service-exemplaar opgeven.
+ Schalen van proces wordt ten minste 20 minuten duren.
+ Als de service is vergrendeld door een andere bewerking, schalen van de aanvraag mislukt en automatisch opnieuw te proberen.
+ In het geval van een service met meerdere regionale implementaties, alleen-eenheden in de **primaire locatie** kunnen worden geschaald. Eenheden in andere locaties kunnen niet worden geschaald.

## <a name="enable-and-configure-autoscale-for-azure-api-management-service"></a>Inschakelen en automatisch schalen configureren voor Azure API Management-service

Volg de stappen hieronder om automatisch schalen configureren voor een Azure API Management-service:

1. Navigeer naar **Monitor** exemplaar in de Azure-portal.

    ![Azure Monitor](media/api-management-howto-autoscale/01.png)

2. Selecteer **voor automatisch schalen** in het menu aan de linkerkant.

    ![Azure Monitor voor automatisch schalen-resource](media/api-management-howto-autoscale/02.png)

3. Zoek uw Azure API Management-service op basis van de filters in de vervolgkeuzemenu's.
4. Selecteer de gewenste Azure API Management service-exemplaar.
5. In de zojuist geopende sectie, klikt u op de **automatisch schalen inschakelen** knop.

    ![Azure Monitor voor automatisch schalen inschakelen](media/api-management-howto-autoscale/03.png)

6. In de **regels** sectie, klikt u op **+ toevoegen van een regel**.

    ![Automatisch schalen van Azure Monitor-regel toevoegen](media/api-management-howto-autoscale/04.png)

7. Een nieuwe scale-out regel definiëren.

   Een regel voor uitschalen kan bijvoorbeeld een toevoeging van een Azure API Management-eenheid, geactiveerd wanneer de gemiddelde capaciteit metrische gegevens in de afgelopen 30 minuten hoger is dan 80%. De onderstaande tabel bevat de configuratie voor een dergelijke regel.

    | Parameter             | Waarde             | Opmerkingen                                                                                                                                                                                                                                                                           |
    |-----------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Bron van metrische gegevens         | Huidige resource  | Definieer de regel op basis van de huidige metrische gegevens voor Azure API Management-resources.                                                                                                                                                                                                     |
    | *Criteria*            |                   |                                                                                                                                                                                                                                                                                 |
    | Tijdverzameling      | Gemiddeld           |                                                                                                                                                                                                                                                                                 |
    | Naam van de meetwaarde           | Capaciteit          | Capaciteit meetwaarde is een Azure API Management-meting zetten op basis van gebruik van bronnen van een Azure API Management-exemplaar.                                                                                                                                                            |
    | Tijdsintervalstatistieken  | Gemiddeld           |                                                                                                                                                                                                                                                                                 |
    | Operator              | Groter dan      |                                                                                                                                                                                                                                                                                 |
    | Drempelwaarde             | 80%               | De drempelwaarde voor de gemiddelde capaciteit metrische gegevens.                                                                                                                                                                                                                                 |
    | Duur (in minuten) | 30                | Het interval aan de capaciteit metrische gegevens gemiddelde is specifiek voor gebruikspatronen. Hoe langer de periode is, worden de soepeler de reactie - onregelmatige pieken hebben minder invloed op de beslissing van de scale-out. Het wordt echter ook de scale-out-trigger vertragen. |
    | *Actie*              |                   |                                                                                                                                                                                                                                                                                 |
    | Bewerking             | Aantal verhogen met |                                                                                                                                                                                                                                                                                 |
    | Aantal instanties        | 1                 | De Azure API Management-exemplaar per 1 eenheid uitschalen.                                                                                                                                                                                                                          |
    | Afkoelen (minuten)   | 60                | Het duurt minimaal 20 minuten voor de Azure API Management-service om uit te schalen. In de meeste gevallen de afkoelperiode bevindt van 60 minuten wordt voorkomen dat veel schaal-outs wordt geactiveerd.                                                                                                  |

8. Klik op **toevoegen** op de regel niet opslaan.

    ![Azure Monitor scale-out regel](media/api-management-howto-autoscale/05.png)

9. Klik nogmaals op **+ toevoegen van een regel**.

    Deze keer moet een schaal in regel worden gedefinieerd. Dit zorgt ervoor dat resources zijn niet worden verspild, wanneer het gebruik van API's wordt verlaagd.

10. Een nieuwe omvang in regel definiëren.

    Een schaalset in regel kan bijvoorbeeld een verwijdering van een Azure API Management-eenheid, geactiveerd wanneer de gemiddelde capaciteit metrische gegevens in de afgelopen 30 minuten minder dan 35 is %. De onderstaande tabel bevat de configuratie voor een dergelijke regel.

    | Parameter             | Waarde             | Opmerkingen                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
    |-----------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Bron van metrische gegevens         | Huidige resource  | Definieer de regel op basis van de huidige metrische gegevens voor Azure API Management-resources.                                                                                                                                                                                                                                                                                                                                                                                                                         |
    | *Criteria*            |                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Tijdverzameling      | Gemiddeld           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Naam van de meetwaarde           | Capaciteit          | Dezelfde metrische gegevens als het account dat wordt gebruikt voor de scale-out regel.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
    | Tijdsintervalstatistieken  | Gemiddeld           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Operator              | Kleiner dan         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Drempelwaarde             | 35%               | Op dezelfde manier aan de scale-out regel afhankelijk deze waarde sterk van de gebruikspatronen van de Azure API Management. |
    | Duur (in minuten) | 30                | Dezelfde waarde als het account dat wordt gebruikt voor de scale-out regel.                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | *Actie*              |                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
    | Bewerking             | Aantal verlagen met | In de wat is gebruikt voor de scale-out regel.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
    | Aantal instanties        | 1                 | Dezelfde waarde als het account dat wordt gebruikt voor de scale-out regel.                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
    | Afkoelen (minuten)   | 90                | Inschalen moet meer dan een scale-out, conservatieve, zodat de afkoelperiode bevindt mag niet langer zijn.                                                                                                                                                                                                                                                                                                                                                                                                    |

11. Klik op **toevoegen** op de regel niet opslaan.

    ![Azure Monitor-schaal in regel](media/api-management-howto-autoscale/06.png)

12. Stel de **maximale** van Azure API Management-eenheden.

    > [!NOTE]
    > Met Azure API Management heeft een limiet van een exemplaar naar uitschalen kunt eenheden. De limiet is afhankelijk van een servicelaag.

    ![Azure Monitor-schaal in regel](media/api-management-howto-autoscale/07.png)

13. Klik op **Opslaan**. De functie voor automatisch schalen is geconfigureerd.

## <a name="next-steps"></a>Volgende stappen

+ [Over het implementeren van een Azure API Management service-exemplaar naar meerdere Azure-regio 's](api-management-howto-deploy-multi-region.md)

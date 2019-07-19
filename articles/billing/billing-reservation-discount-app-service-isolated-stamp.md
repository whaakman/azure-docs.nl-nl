---
title: Hoe reserverings kortingen van toepassing zijn op Azure App Service afzonderlijke stem pels
description: Meer informatie over hoe reserverings kortingen van toepassing zijn op Azure App Service afzonderlijke stem pels.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: banders
ms.openlocfilehash: cb4b371deadb1ae9e6ae048c3157809aff857c9d
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68298254"
---
# <a name="how-reservation-discounts-apply-to-azure-app-service-isolated-stamps"></a>Hoe reserverings kortingen van toepassing zijn op Azure App Service afzonderlijke stem pels

Nadat u App Service Isolated gereserveerde capaciteit voor de stempel taks hebt gekocht, wordt de reserverings korting automatisch toegepast op de stempel kosten in een regio. De reserverings korting is van toepassing op het gebruik dat wordt verzonden door de kosten meter voor de geïsoleerde stempel. Werk nemers, extra front-ends en alle andere resources die zijn gekoppeld aan de stempel blijven gefactureerd op basis van het normale aantal.

## <a name="reservation-discount-application"></a>Toepassing voor reserverings korting

De korting op de App Service Isolated Stamp-vergoeding wordt toegepast op het per uur uitvoeren van geïsoleerde stem pels. Als u gedurende een uur geen Stamp hebt geïmplementeerd, wordt de gereserveerde capaciteit voor dat uur verspild. Het wordt niet overgedragen.

Na de aankoop wordt de door u gekochte reserve ring vergeleken met een geïsoleerde stempel die wordt uitgevoerd in een specifieke regio. Als u deze stempel afsluit, worden de reserverings kortingen automatisch toegepast op andere stem pels die in de regio worden uitgevoerd. Als er geen stem pels bestaan, wordt de reserve ring toegepast op de volgende stempel die in de regio is gemaakt.

Wanneer stem pels niet worden uitgevoerd voor een volledig uur, wordt de reserve ring automatisch toegepast op andere overeenkomende stem pels in dezelfde regio gedurende hetzelfde uur.

## <a name="choose-a-stamp-type---windows-or-linux"></a>Een type stempel kiezen-Windows of Linux

Een lege, geïsoleerde stempel verzendt standaard de Windows Stamp meter. Bijvoorbeeld wanneer er geen werk rollen zijn geïmplementeerd. Het blijft de meter verzenden wanneer Windows-werk rollen worden geïmplementeerd. De meter verandert in de Linux-stempel meter als u een Linux-werk nemer implementeert. De stempel levert de Windows-meter wanneer zowel Linux-als Windows-werk rollen zijn geïmplementeerd.

Als gevolg hiervan kan de stempel meter worden gewijzigd tussen Windows en Linux gedurende de levens duur van de stempel. Ondertussen zijn de reserve ringen specifiek voor het besturings systeem. U moet een reserve ring aanschaffen die ondersteuning biedt voor de werk nemers die u op de stempel wilt implementeren. Alleen Windows-stem pels en gemengde stem pels gebruiken de Windows-reserve ring. Stem pels met alleen Linux-werk nemers gebruiken de Linux-reserve ring.

Het is de enige keer dat u een Linux-reserve ring moet aanschaffen wanneer u van plan bent om _alleen_ Linux-werk nemers in de stempel te hebben.

## <a name="discount-examples"></a>Kortings voorbeelden

In de volgende voor beelden ziet u hoe de korting voor een gereserveerde instantie voor de bijzondere stempel betaling van toepassing is, afhankelijk van de implementaties.

- **Voor beeld 1**: U koopt één exemplaar van een geïsoleerde gereserveerde stempel capaciteit in een regio zonder App Service Isolated stem pels. U implementeert een nieuwe stempel in de regio en betaalt de gereserveerde tarieven voor die stempel.
- **Voor beeld 2**: U koopt één exemplaar van geïsoleerde gereserveerde stempel capaciteit in een regio waarvoor al een App Service Isolated stempel is geïmplementeerd. U ontvangt het gereserveerde bedrag voor de geïmplementeerde stempel.
- **Voor beeld 3**: U koopt één exemplaar van de geïsoleerde gereserveerde stempel capaciteit in een regio met een App Service Isolated stempel al is geïmplementeerd. U ontvangt het gereserveerde bedrag op het geïmplementeerde stempel. Later verwijdert u de stempel en implementeert u een nieuwe. U ontvangt het gereserveerde bedrag voor de nieuwe stempel. De kortingen hebben geen betrekking op de duur zonder geïmplementeerde stem pels.
- **Voor beeld 4**: U koopt één exemplaar van de geïsoleerde gereserveerde stempel capaciteit van Linux in een regio en implementeert vervolgens een nieuwe stempel in de regio. Wanneer de stempel voor het eerst wordt geïmplementeerd zonder werk nemers, wordt de Windows Stamp meter verzonden. Er is geen korting ontvangen. Wanneer de eerste Linux-werk nemer de stempel implementeert, worden de Linux-stempel meter en de reserverings korting van toepassing. Als een Windows-werk nemer later wordt geïmplementeerd op de stempel, wordt de stempel meter teruggezet naar Windows. U ontvangt geen korting meer voor de gereserveerde reserve ring van de geïsoleerde Linux-stempel.

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure Reservations beheren](billing-manage-reserved-vm-instance.md)voor meer informatie over het beheren van een reserve ring.
- Zie voor meer informatie over het vooraf kopen van App Service Isolated stempel gereserveerde capaciteit voor het besparen van geld, de [betalen voor Azure app service geïsoleerde stempel met gereserveerde capaciteit](billing-prepay-app-service-isolated-stamp.md).
- Raadpleeg de volgende artikelen voor meer informatie over Azure Reservations:
  - [Wat zijn Azure Reservations?](billing-save-compute-costs-reservations.md)
  - [Reserve ringen in azure beheren](billing-manage-reserved-vm-instance.md)
  - [Meer informatie over reserverings gebruik voor een abonnement met betalen per gebruik-tarieven](billing-understand-reserved-instance-usage.md)
  - [Het gebruik van de reserve ring begrijpen voor uw Enter prise-inschrijving](billing-understand-reserved-instance-usage-ea.md)

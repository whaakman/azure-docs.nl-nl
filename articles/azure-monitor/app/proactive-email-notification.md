---
title: 'Azure Application Insights Slimme detectie: aanstaande wijziging van de geadresseerden voor meldingen van standaard | Microsoft Docs'
description: Toepassingstraceringen met Azure Application Insights voor ongewone patronen in tracetelemetrie bewaken.
services: application-insights
author: harelbr
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/13/2019
ms.author: harelbr
ms.openlocfilehash: f984a34be1c5d5fdd18a00812107318df8f5d9bf
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57889281"
---
# <a name="smart-detection-e-mail-notification-change"></a>Slimme detectie van e-mailmelding wijzigen

Op basis van feedback van klanten op 1 April 2019 wijzigt we de standaardrollen die e-mailmeldingen van Slimme detectie ontvangen.

## <a name="what-is-changing"></a>Wat is gewijzigd?

Op dit moment e-mailmeldingen voor slimme detectie om te worden verzonden de _Abonnementhouder_, _Inzender_, en _Abonnementslezer_ rollen. Deze rollen zijn vaak toegewezen aan gebruikers die niet actief betrokken zijn bij de bewaking, waardoor veel van deze gebruikers de meldingen onnodig ontvangen. Als u wilt deze ervaring te verbeteren, maken we een wijziging zodat e-mailmeldingen alleen gaat u naar de [Monitoring Reader](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) en [bewaking Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) functies standaard.

## <a name="scope-of-this-change"></a>Deze wijziging van bereik

Deze wijziging is van invloed op alle regels voor slimme detectie, behalve:

* Regels voor slimme detectie die zijn gemarkeerd als preview. Deze regels voor slimme detectie bieden geen ondersteuning voor e-mailmeldingen vandaag nog.

* Regel voor afwijkende fouten. Deze regel wordt gestart die gericht is op de nieuwe standaardrollen nadat deze is gemigreerd vanuit een klassieke waarschuwing naar het geïntegreerde waarschuwingenplatform (meer informatie vindt u [hier](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement).)

## <a name="how-to-prepare-for-this-change"></a>Hoe bereid ik voor deze wijziging?

Om ervoor te zorgen dat e-mailmeldingen van Slimme detectie worden verzonden naar de relevante gebruikers, die gebruikers moeten worden toegewezen aan de [Monitoring Reader](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) of [bewaking Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) rollen van het abonnement.

Voor gebruikers toewijzen aan de rollen Monitoring Reader of Inzender controleren via de Azure-portal, volgt u de stappen in de [een roltoewijzing toevoegen](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) artikel. Zorg ervoor dat u selecteert de _Monitoring Reader_ of _bewaking Inzender_ als de rol waaraan gebruikers zijn toegewezen.

> [!NOTE]
> Specifieke ontvangers van meldingen voor slimme detectie, geconfigureerd met behulp van de _aanvullende e-mailontvangers_ optie in de regelinstellingen van de, wordt niet beïnvloed door deze wijziging. Deze ontvangers blijven de e-mailmeldingen ontvangen.

Als u vragen of opmerkingen over deze wijziging hebt, gerust [contact met ons opnemen](mailto:smart-alert-feedback@microsoft.com).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over slimme detectie:

- [Foutafwijkingen](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Geheugenlekken](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Prestatieafwijkingen](../../azure-monitor/app/proactive-performance-diagnostics.md)
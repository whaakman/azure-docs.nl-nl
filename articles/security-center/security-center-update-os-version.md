---
title: Update OS-versie in Azure Security Center | Microsoft Docs
description: Dit artikel laat zien hoe u de aanbeveling Azure Security Center implementeert ** Update OS-versie **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: aa372492-ecdb-4368-8fdd-d8ed31e216ee
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: terrylan
ms.openlocfilehash: ce0d178914907750e5da59f223a4b1e04b9bb6fb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="update-os-version-in-azure-security-center"></a>Bijwerken van de versie van het besturingssysteem in Azure Security Center
Voor virtuele machines (VM's) in de cloud-services, Azure Security Center beveelt aan dat het besturingssysteem (OS) worden bijgewerkt als er een recentere versie beschikbaar.  Alleen cloud services-web- en werkrollen rollen die worden uitgevoerd in de productieomgeving sleuven worden bewaakt.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
> 
> 

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **aanbevelingen** blade Selecteer **Update besturingssysteemversie**.
   ![Besturingssysteemversie bijwerken][1]
2. Hiermee opent u de blade **Update besturingssysteemversie**. Volg de stappen in deze blade versie van het besturingssysteem bijwerken.

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u implementeert de aanbeveling Security Center 'Update OS-versie. Zie voor meer informatie over cloudservices en bijwerken van de versie van het besturingssysteem voor een cloudservice:

* [Overzicht van Cloud Services](../cloud-services/cloud-services-choose-me.md)
* [Het bijwerken van een service in de cloud](../cloud-services/cloud-services-update-azure-service.md)
* [Cloud Services configureren](../cloud-services/cloud-services-how-to-configure-portal.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees het laatste nieuws van de Azure-beveiliging en de informatie.

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png

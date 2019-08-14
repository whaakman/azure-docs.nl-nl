---
title: Versie van het besturings systeem bijwerken in Azure Security Center | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de Azure Security Center aanbeveling **update versie van het besturings systeem**implementeert.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: aa372492-ecdb-4368-8fdd-d8ed31e216ee
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: a17bc3cf8c8f1ffbb2a91e17944bfc1c203f439d
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "60905840"
---
# <a name="update-os-version-in-azure-security-center"></a>Versie van het besturings systeem bijwerken in Azure Security Center
Voor virtuele machines (Vm's) in Cloud Services wordt Azure Security Center aanbevolen het besturings systeem (OS) bij te werken als er een nieuwere versie beschikbaar is.  Alleen Cloud Services-Web-en-werk rollen die worden uitgevoerd in productie sleuven worden bewaakt.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
> 
> 

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. Selecteer in de Blade **aanbevelingen** de optie **versie van besturings systeem bijwerken**.
   ![Besturingssysteemversie bijwerken][1]
2. Hiermee opent u de **versie van het besturings systeem**Blade bijwerken. Volg de stappen in deze Blade om de versie van het besturings systeem bij te werken.

## <a name="see-also"></a>Zie ook
In dit artikel wordt uitgelegd hoe u de Security Center aanbeveling ' versie van besturings systeem bijwerken ' implementeert. Zie voor meer informatie over Cloud Services en het bijwerken van de versie van het besturings systeem voor een Cloud service:

* [Overzicht van Cloud Services](../cloud-services/cloud-services-choose-me.md)
* [Een Cloud service bijwerken](../cloud-services/cloud-services-update-azure-service.md)
* [Cloud Services configureren](../cloud-services/cloud-services-how-to-configure-portal.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in azure Security Center](security-center-recommendations.md) : Ontdek hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Beveiligings status controleren in azure Security Center](security-center-monitoring.md) --meer informatie over het controleren van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligings blog](https://blogs.msdn.com/b/azuresecurity/) : krijg het meest recente Azure-beveiligings nieuws en-informatie.

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png

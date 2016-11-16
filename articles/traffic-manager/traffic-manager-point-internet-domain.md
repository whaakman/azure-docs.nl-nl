---
title: Het internetdomein van een bedrijf naar een Traffic Manager-domeinnaam laten wijzen | Microsoft Docs
description: Aan de hand van dit artikel kunt u een domeinnaam van uw bedrijf laten wijzen naar een Traffic Manager-domeinnaam.
services: traffic-manager
documentationcenter: 
author: sdwheeler
manager: carmonm
editor: 
ms.assetid: 29822946-2d45-4434-ba47-fc180a445cc3
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 16602ca37e3b62fc628ec3178e6f65e32fc3d345

---

# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>Het internetdomein van een bedrijf naar een Traffic Manager-domein laten wijzen

Wanneer u een Traffic Manager-profiel maakt, wijst Azure automatisch een DNS-naam toe voor dat profiel. Om een naam voor de DNS-zone te gebruiken, maakt u een DNS CNAME-record die wordt toegewezen aan de domeinnaam van uw Traffic Manager-profiel. U vindt de Traffic Manager-domeinnaam in het gedeelte **Algemeen** van de configuratiepagina van het Traffic Manager-profiel.

Als u de naam www.contosocom bijvoorbeeld wilt wijzen naar de DNS-naam van Traffic Manager, bijv. contoso.trafficmanager.net, moet u de DNS-resourcerecord als volgt maken:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Alle aanvragen voor *www.contoso.com* worden doorgestuurd naar *contoso.trafficmanager.net*.

> [!IMPORTANT]
> U kunt niet naar domeinen op het tweede niveau wijzen, zoals *contoso.com* op het Traffic Manager-domein. DNS-protocolstandaarden staan geen CNAME-records toe voor domeinnamen van het tweede niveau.

## <a name="next-steps"></a>Volgende stappen

* [Methoden voor het doorsturen van Traffic Manager](traffic-manager-routing-methods.md)
* [Traffic Manager - Een profiel uitschakelen, inschakelen of verwijderen](disable-enable-or-delete-a-profile.md)
* [Traffic Manager - Een eindpunt in- of uitschakelen](disable-or-enable-an-endpoint.md)



<!--HONumber=Nov16_HO2-->



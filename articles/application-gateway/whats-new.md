---
title: Wat is er nieuw in Azure Application Gateway
description: Ontdek wat er nieuw in Azure Application Gateway, zoals de meest recente release-opmerkingen worden bekende problemen, oplossingen voor problemen, afgeschafte functies en toekomstige wijzigingen.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 4/1/2019
ms.author: victorh
ms.openlocfilehash: f686c8ac53db2d128cf5bb20f252c547348e5ac7
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58863097"
---
# <a name="whats-new-in-azure-application-gateway"></a>Wat is er nieuw in Azure Application Gateway?

Azure Application Gateway wordt regelmatig bijgewerkt. Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

- De meest recente versies
- Bekende problemen
- Opgeloste fouten
- Afgeschafte functies

## <a name="new-features"></a>Nieuwe functies

|Functie  |Beschrijving  |Datum toegevoegd  |
|---------|---------|---------|
|Koptekst CRUD/Regeneraties     |U kunt nu de HTTP-headers herschrijven. Zie [zelfstudie: Een toepassingsgateway maken en te herschrijven van HTTP-headers](tutorial-http-header-rewrite-powershell.md) voor meer informatie.|December 2018|
|Lijst met de configuratie en uitsluiting van WAF     |Er is meer opties voor het configureren van uw WAF en fout-positieven toegevoegd. Zie [Web application firewall-aanvraaglimieten grootte en uitsluitingslijsten](application-gateway-waf-configuration.md) voor meer informatie.|December 2018|
|Automatisch schalen, zoneredundantie, statische VIP ondersteuning voor preview     |Er zijn veel verbeteringen, zoals automatisch schalen, verbeterde prestaties en meer met de v2-SKU. Zie [wat is Azure Application Gateway?](overview.md#autoscaling-public-preview) voor meer informatie.|September 2018|
|Verwerkingsstop voor verbindingen     |Verwerkingsstop voor verbindingen, kunt u probleemloos leden verwijderen uit uw back endpools. Zie voor meer informatie, [verwerkingsstop](overview.md#connection-draining).|September 2018|
|Aangepaste foutpagina's     |Met aangepaste foutpagina's, kunt u een foutpagina weergegeven in de indeling van de rest van uw websites maken. U kunt deze inschakelen, Zie [Application Gateway maakt aangepaste foutpagina's](custom-error.md).|September 2018|
|Verbeteringen van de metrische gegevens     |U kunt een beter beeld van de status van uw Application Gateway met uitgebreide metrische gegevens ophalen. Zie voor het inschakelen van metrische gegevens over uw Application Gateway [Back-endstatus, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).|Juni 2018|

## <a name="known-issues"></a>Bekende problemen

- [Bekende problemen in de v2-SKU](application-gateway-autoscaling-zone-redundant.md#known-issues-and-limitations)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Application Gateway [wat is Azure Application Gateway?](overview.md)
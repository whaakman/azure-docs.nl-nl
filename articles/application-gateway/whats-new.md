---
title: Wat is er nieuw in Azure Application Gateway
description: Ontdek wat er nieuw in Azure Application Gateway, zoals de meest recente release-opmerkingen worden bekende problemen, oplossingen voor problemen, afgeschafte functies en toekomstige wijzigingen.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 4/30/2019
ms.author: victorh
ms.openlocfilehash: 2a494b924107baeabbcf412af7e1bbdb1db0f753
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752020"
---
# <a name="whats-new-in-azure-application-gateway"></a>Wat is er nieuw in Azure Application Gateway?

Azure Application Gateway wordt regelmatig bijgewerkt. Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

- De meest recente versies
- Bekende problemen
- Opgeloste fouten
- Afgeschafte functies

## <a name="new-features"></a>Nieuwe functies

|Functie  |Description  |Datum toegevoegd  |
|---------|---------|---------|
|Aangepaste WAF-regels |Applicaiton Gateway WAF_v2 now supports creating custom rules. Zie [aangepaste regels voor Application Gateway](custom-waf-rules-overview.md). |Juni 2019 |
|Automatisch schalen, zoneredundantie, ondersteuning voor statische VIP algemeen beschikbaar |Algemene beschikbaarheid voor de v2-SKU die ondersteunt automatisch schalen, zoneredundantie, betere prestaties, statische VIP's, Key Vault, Header herschrijven. Zie [Application Gateway-documentatie voor automatisch schalen](application-gateway-autoscaling-zone-redundant.md). |April 2019 |
|Key Vault-integratie |Application Gateway ondersteunt nu de integratie met Key Vault (in openbare preview) voor certificaten die zijn gekoppeld aan de HTTPS-functionaliteit listeners. Zie [SSL-beëindiging met Key Vault-certificaten](key-vault-certs.md). |April 2019 |
|Koptekst CRUD/Regeneraties     |U kunt nu de HTTP-headers herschrijven. Zie [zelfstudie: Een toepassingsgateway maken en te herschrijven van HTTP-headers](tutorial-http-header-rewrite-powershell.md) voor meer informatie.|December 2018|
|Lijst met de configuratie en uitsluiting van WAF     |Er is meer opties voor het configureren van uw WAF en fout-positieven toegevoegd. Zie [Web application firewall-aanvraaglimieten grootte en uitsluitingslijsten](application-gateway-waf-configuration.md) voor meer informatie.|December 2018|
|Automatisch schalen, zoneredundantie, statische VIP-ondersteuning      |Er zijn veel verbeteringen, zoals automatisch schalen, verbeterde prestaties en meer met de v2-SKU. Zie [wat is Azure Application Gateway?](overview.md) voor meer informatie.|September 2018|
|Verwerkingsstop voor verbindingen     |Verwerkingsstop voor verbindingen, kunt u probleemloos leden verwijderen uit uw back endpools. Zie voor meer informatie, [verwerkingsstop](overview.md#connection-draining).|September 2018|
|Aangepaste foutpagina's     |Met aangepaste foutpagina's, kunt u een foutpagina weergegeven in de indeling van de rest van uw websites maken. U kunt deze inschakelen, Zie [Application Gateway maakt aangepaste foutpagina's](custom-error.md).|September 2018|
|Verbeteringen van de metrische gegevens     |U kunt een beter beeld van de status van uw Application Gateway met uitgebreide metrische gegevens ophalen. Zie voor het inschakelen van metrische gegevens over uw Application Gateway [Back-endstatus, diagnostische logboeken en metrische gegevens voor Application Gateway](application-gateway-diagnostics.md).|Juni 2018|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure Application Gateway [wat is Azure Application Gateway?](overview.md)

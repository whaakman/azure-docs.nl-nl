---
title: Het openen van de firewall-poorten die vereist zijn voor een toepassingsproxy-toepassing | Microsoft Docs
description: Ontdek wat poorten openen voor de Azure AD-toepassingsproxy correct te laten werken
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: efc8c0f0581878d41eb62cbfbd88aec730ecf789
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175232"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Het openen van de firewallpoorten die nodig zijn voor een toepassing Application Proxy

Een volledige lijst van de vereiste poorten en de functie van elke poort wilt bekijken, Zie de sectie vereisten van de [Application Proxy documentatie](application-proxy-add-on-premises-application.md). Houd er rekening mee dat Application Proxy alleen uitgaande poorten gebruikt.

U kunt ook controleren of u beschikt over alle vereiste poorten door het openen van openen de [verbindingslijn poorten Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) vanaf uw on-premises netwerk. Meer een groen vinkje betekent meer flexibiliteit. 

## <a name="app-proxy-regions"></a>App-Proxy-regio 's

We werken op een manier om te laten u weten welke van deze regio's moet groen voor u. Op dit moment dient u ervoor dat ze allemaal zijn. VS-midden is ook vereist, ongeacht welke waarin u zich bevindt.

Als u wilt controleren of dat het hulpprogramma biedt u de juiste resultaten, moet u:

-   Open het hulpprogramma op een browser van de server waar u de Connector hebt geïnstalleerd.

-   Zorg ervoor dat alle proxy's of firewalls van toepassing zijn op uw Connector ook worden toegepast op deze pagina. Dit kan in Internet Explorer worden gedaan door te gaan naar **instellingen**  - &gt; **Internetopties**  - &gt; **verbindingen**  - &gt; **Lan-instellingen**. Op deze pagina ziet u het veld 'Met een Proxy-Server voor uw LAN'. Schakel dit selectievakje in en plaats het proxyadres in het veld "Adres".

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over Azure AD Application Proxy connectors](application-proxy-connectors.md)

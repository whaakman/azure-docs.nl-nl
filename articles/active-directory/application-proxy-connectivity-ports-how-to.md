---
title: Het openen van de firewall-poorten die vereist zijn voor een toepassingsproxy-toepassing | Microsoft Docs
description: Ontdek wat poorten openen voor de Azure AD-toepassingsproxy correct te laten werken
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 6c26fb7cf88a44e7392d6be934d9dfae43b31c08
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39362773"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Het openen van de firewallpoorten die nodig zijn voor een toepassing Application Proxy

Een volledige lijst van de vereiste poorten en de functie van elke poort wilt bekijken, Zie de sectie vereisten van de [Application Proxy documentatie](manage-apps/application-proxy-enable.md). Houd er rekening mee dat Application Proxy alleen uitgaande poorten gebruikt.

U kunt ook controleren of u beschikt over alle vereiste poorten door het openen van openen de [verbindingslijn poorten Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) vanaf uw on-premises netwerk. Meer een groen vinkje betekent meer flexibiliteit. 

## <a name="app-proxy-regions"></a>App-Proxy-regio 's

We werken op een manier om te laten u weten welke van deze regio's moet groen voor u. Op dit moment dient u ervoor dat ze allemaal zijn. VS-midden is ook vereist, ongeacht welke waarin u zich bevindt.

Als u wilt controleren of dat het hulpprogramma biedt u de juiste resultaten, moet u:

-   Open het hulpprogramma op een browser van de server waar u de Connector hebt geïnstalleerd.

-   Zorg ervoor dat alle proxy's of firewalls van toepassing zijn op uw Connector ook worden toegepast op deze pagina. Dit kan in Internet Explorer worden gedaan door te gaan naar **instellingen**  - &gt; **Internetopties**  - &gt; **verbindingen**  - &gt; **Lan-instellingen**. Op deze pagina ziet u het veld 'Met een Proxy-Server voor uw LAN'. Schakel dit selectievakje in en plaats het proxyadres in het veld "Adres".

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over Azure AD Application Proxy connectors](manage-apps/application-proxy-connectors.md)

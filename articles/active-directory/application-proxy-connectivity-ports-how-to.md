---
title: Het openen van de firewall-poorten die vereist zijn voor een toepassing toepassingsproxy | Microsoft Docs
description: Uitzoeken wat poorten te openen voor de Azure AD-toepassingsproxy correct te laten werken
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 8ecd6d7e666d362194126a4abba7a65f2c7b8b6b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Het openen van de firewall-poorten die vereist zijn voor een toepassing toepassingsproxy

Een volledige lijst met de vereiste poorten en de functie van elke poort, Zie de sectie vereisten van de [toepassingsproxy documentatie](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable). Houd er rekening mee dat toepassingsproxy alleen uitgaande poorten gebruikt.

U kunt ook controleren of u beschikt over alle vereiste poorten via openen de [Connectorhulpprogramma poorten Test](https://aadap-portcheck.connectorporttest.msappproxy.net/) vanuit uw on-premises netwerk. Meer een groen vinkje betekent groter tolerantie. 

## <a name="app-proxy-regions"></a>Toepassingsproxy van regio 's

We werken op een manier om te laten u weten welke van deze gebieden moet groen voor u. Zorg dat ze allemaal zijn op dit moment. VS-midden is ook vereist, ongeacht welke regio in.

Om te zorgen dat het hulpprogramma kunt u de juiste resultaten, moet u:

-   Open het hulpprogramma op een browser van de server waar u de Connector hebt geïnstalleerd.

-   Zorg ervoor dat alle proxy's of de van toepassing op uw Connector firewalls ook worden toegepast op deze pagina. Dit kan worden gedaan in Internet Explorer door te gaan naar **instellingen**  - &gt; **Internetopties**  - &gt; **verbindingen**  - &gt; **Lan-instellingen**. Op deze pagina ziet u het veld 'Gebruik een Proxy-Server voor uw LAN'. Schakel dit in en adres voor de proxyserver in het veld 'Adres' geplaatst.

## <a name="next-steps"></a>Volgende stappen
[Azure AD-toepassingsproxy connectors begrijpen](application-proxy-understand-connectors.md)

---
title: Beveiligde Web-API - overzicht | Azure
description: Meer informatie over het bouwen van een beveiligde web-API (overzicht).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5bc0075e6604bd1f94531040f2e4a0628e70667
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074888"
---
# <a name="scenario-protected-web-api"></a>Scenario: Beveiligde web-API

In dit scenario leert u hoe u een web-API kunt weergeven en hoe u deze kunt beveiligen zodat alleen geverifieerde gebruikers hebben toegang tot de API. Moet u aan geverifieerde gebruikers met zowel werk en schoolaccounts of persoonlijke persoonlijk Microsoft-account met uw web-API.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Specifieke informatie

Hier volgen enkele specifieke instellingen die u moet weten om te beveiligen, web-API's:

- Uw app-registratie moet ten minste één bereik worden blootgesteld. De token geaccepteerd door uw web-API-versie, is afhankelijk van het teken in de doelgroep.
- Het token dat wordt gebruikt bij het aanroepen van de web-API moet worden gevalideerd door de configuratie van de code voor de web-API.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-protected-web-api-app-registration.md)

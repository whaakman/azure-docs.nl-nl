---
title: Beveiligde web-API-overzicht | Azure
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
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02bd4b84cc7542714f6db45c12c4b5b13a7fb449
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68852584"
---
# <a name="scenario-protected-web-api"></a>Scenario: Beveiligde web-API

In dit scenario laten we u zien hoe u een web-API beschikbaar maakt en hoe u deze kunt beveiligen zodat alleen geverifieerde gebruikers toegang hebben tot de API. U wilt geverifieerde gebruikers met zowel werk-als school accounts of persoonlijke micro soft-accounts inschakelen om uw web-API te gebruiken.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Opsporingsgegevens

Hier volgen enkele specifieke informatie die u moet kennen om Web-Api's te beveiligen:

- De registratie van uw app moet ten minste één bereik weer geven. De token versie die door uw web-API wordt geaccepteerd, is afhankelijk van de doel groep voor aanmelden.
- De configuratie van de code voor de Web-API moet het token valideren dat wordt gebruikt bij het aanroepen van de Web-API.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Appregistratie](scenario-protected-web-api-app-registration.md)

---
title: Aanbevolen beveiligingsprocedures voor digitale dubbels Azure begrijpen | Microsoft Docs
description: Azure digitale dubbels best practices voor beveiliging.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: adgera
ms.openlocfilehash: 16bb148a0b3a424c9ba3aaae422f423ebd40793b
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358810"
---
# <a name="security-best-practices"></a>Aanbevolen procedures voor beveiliging

Beveiliging op Azure digitale dubbels kunt nauwkeurige toegang tot bepaalde resources en acties in uw IoT-grafiek. Dit gebeurt door middel van gedetailleerde rol en beheer van machtigingen met de naam [op rollen gebaseerd toegangsbeheer](./security-role-based-access-control.md).

Azure digitale dubbels maakt ook gebruik van andere beveiligingsfuncties die aanwezig in Azure IoT zijn, met inbegrip van Azure Active Directory (Azure AD). Om die reden, configureren en beveiligen van toepassingen die zijn gebouwd op Azure digitale dubbels omvat het gebruik van veel van dezelfde [procedures voor Azure IoT-beveiliging](../iot-fundamentals/iot-security-best-practices.md) momenteel aanbevolen.

In dit artikel bevat een overzicht van de belangrijkste aanbevolen procedures te volgen.

> [!IMPORTANT]
> Om ervoor te zorgen maximale beveiliging van uw IoT-ruimte, extra beveiliging resources te controleren. Zorg ervoor dat uw Apparaatleveranciers.

## <a name="iot-security-best-practices"></a>Best practices voor IoT-beveiliging

Enkele belangrijke procedures voor het beveiligen van veilig uw IoT-apparaten zijn onder andere:

> [!div class="checklist"]
> * Elk apparaat dat verbonden met uw IoT-ruimte op een manier fraudebestendig beveiligen.
> * De rol van elk apparaat, sensor en persoon binnen uw IoT-ruimte beperkt. Als ermee is geknoeid, wordt het effect geminimaliseerd.
> * Overweeg het mogelijk gebruik van apparaat-IP-adresfilters en poort van beperking.
> * Limiet voor i/o- en apparaat bandbreedte om prestaties te verbeteren. Frequentiebeperkende, kan de beveiliging verbeteren door het voorkomen van denial-of-service-aanvallen.
> * Apparaatfirmware up-to-date te houden.

Enkele belangrijke procedures voor het beveiligen van een IoT-ruimte veilig zijn onder andere:

> [!div class="checklist"]
> * Opgeslagen, opgeslagen of permanente gegevens versleutelen.
> * Vereisen dat wachtwoorden of sleutels regelmatig worden gewijzigd of vernieuwd.
> * Toegang en machtigingen zorgvuldig beperken door de rol. Zie de sectie [best practices voor Role-based access control](#rbac) hieronder.
> * Gebruik krachtige codering. Lange wachtwoorden en gebruiken van beveiligde protocollen en verificatie met twee factoren.

[Monitor](./how-to-configure-monitoring.md) IoT-resources om te bekijken voor uitschieters, bedreigingen of Resourceparameters die het bereik van de normale bewerking buiten. Gebruik Azure Analytics voor het bewaken van beheer.

> [!NOTE]
> Zie voor meer informatie over gebeurtenis verwerken en bewaking [doorsturen van gebeurtenissen en -berichten met Azure digitale dubbels](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Aanbevolen procedures van Azure Active Directory

Azure van digitale dubbels maakt gebruik van Azure Active Directory om te verifiëren van gebruikers en toepassingen te beschermen. Azure Active Directory ondersteunt verificatie voor een verscheidenheid aan moderne architecturen. Ze zijn gebaseerd op standaardprotocollen zoals OAuth 2.0 of OpenID Connect. Enkele belangrijke procedures voor het beveiligen van uw IoT-ruimte voor Azure Active Directory zijn onder andere:

> [!div class="checklist"]
> * Zoals Azure Active Directory-app-geheimen en sleutels op een veilige locatie Store [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Gebruik van een certificaat dat is uitgegeven door een vertrouwde [certificeringsinstantie](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) in plaats van appgeheimen om te verifiëren.
> * OAuth 2.0-bereik van de toegang voor een token beperken.
> * Controleer of de hoeveelheid tijd die een token geldig is en of u een token geldig blijft.
> * Juiste lengte van de tijd die tokens geldig zijn voor ingesteld.
> * Verlopen vernieuwingstokens.

<div id="rbac"></div>

## <a name="role-based-access-control-best-practices"></a>Aanbevolen procedures voor toegang op basis van rollen beheren

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Volgende stappen

* Lees voor meer informatie over best practices voor Azure IoT [best practices voor IoT-beveiliging](../iot-fundamentals/iot-security-best-practices.md).

* Lees voor meer informatie over op rollen gebaseerd toegangsbeheer, [Role-based access control van](./security-role-based-access-control.md).

* Lees voor meer informatie over verificatie, [verifiëren met API's](./security-authenticating-apis.md).

---
title: Informatie over aanbevolen beveiligingsprocedures voor Azure digitale dubbels | Microsoft Docs
description: Azure digitale dubbels best practices voor beveiliging
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: adgera
ms.openlocfilehash: a17fe8ed47384ed248b339643be11269b8b9cdc0
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092182"
---
# <a name="security-best-practices"></a>Aanbevolen procedures voor beveiliging

Beveiliging op Azure digitale dubbels kunt nauwkeurige toegang tot bepaalde resources en acties in uw IoT-grafiek. Dit gebeurt via gedetailleerde rol en de machtiging beheer met de naam Role-Based Access Control.

Azure van digitale dubbels maakt ook gebruik van andere beveiligingsfuncties die aanwezig zijn op Azure IoT, met inbegrip van Azure Active Directory. Om die reden voor het configureren van uw app in Azure, digitale dubbels omvat het gebruik van veel van dezelfde [procedures voor Azure IoT-beveiliging](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/) momenteel aanbevolen.

In dit artikel bevat een overzicht van de belangrijkste aanbevolen procedures te volgen.

> [!IMPORTANT]
> Extra beveiliging-resources (met inbegrip van uw Apparaatleveranciers) controleren om ervoor te zorgen maximale beveiliging van uw IoT-ruimte.

## <a name="iot-security-best-practices"></a>Best practices voor IoT-beveiliging

Enkele belangrijke procedures voor het beveiligen van veilig uw IoT-apparaten zijn onder andere:

> [!div class="checklist"]
> * Elk apparaat dat verbonden met uw IoT-ruimte op een manier fraudebestendig beveiligen.
> * De rol van elk apparaat, sensor en persoon binnen uw IoT-ruimte beperkt. Als ermee is geknoeid, wordt de impact geminimaliseerd.
> * Potentieel gebruik van apparaat-IP-adres filteren en het poortnummer beperking.
> * Limiet voor i/o- en apparaat bandbreedte om prestaties te verbeteren. Frequentiebeperkende, kan de beveiliging verbeteren door het voorkomen van denial-of-service-aanvallen.
> * Apparaatfirmware up-to-date te houden.

Enkele belangrijke procedures voor het beveiligen van een IoT-ruimte veilig zijn onder andere:

> [!div class="checklist"]
> * Opgeslagen, opgeslagen of permanente gegevens versleutelen.
> * Vereisen dat wachtwoorden of sleutels regelmatig worden gewijzigd of vernieuwd.
> * Zorgvuldig beperken van toegang en machtigingen per rol (Zie de aanbevolen procedures voor onderstaande Role-Based Access Control).
> * Gebruik krachtige codering. Dit betekent dat dat lange wachtwoorden, met behulp van veilige protocollen en verificatie met twee factoren.

Bewaking van IoT-resources om te bekijken voor uitschieters, bedreigingen of Resourceparameters die het bereik van de normale werking buiten wordt beheerd via Azure Analytics.

> [!NOTE]
> Voor meer informatie over gebeurtenis verwerken en te bewaken, Zie dan het artikel op [gebeurtenis > routering](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Aanbevolen procedures van Azure Active Directory

Azure van digitale dubbels maakt gebruik van Azure Active Directory om te verifiëren van gebruikers en toepassingen te beschermen. Azure Active Directory ondersteunt verificatie voor een verscheidenheid aan moderne architecturen, allemaal op basis van standaardprotocollen zoals OAuth 2.0 of OpenID Connect. Enkele belangrijke procedures voor het beveiligen van uw IoT-ruimte voor Azure Active Directory zijn onder andere:

> [!div class="checklist"]
> * Zoals Azure Active Directory-app-geheimen en sleutels op een veilige locatie Store [Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Gebruik van een certificaat dat is uitgegeven door een vertrouwde [certificeringsinstantie](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started) in plaats van appgeheimen om te verifiëren.
> * OAuth 2.0-bereik van de toegang voor een token beperken.
> * Controleer of de hoeveelheid tijd die een token geldig is en of u een token geldig blijft.
> * Juiste lengte van de tijd die tokens geldig zijn voor ingesteld.
> * Verlopen vernieuwingstokens.

## <a name="role-based-access-control-best-practices"></a>Aanbevolen procedures voor toegang op basis van rollen beheren

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over best practices voor Azure IoT [best practices voor IoT-beveiliging](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/).

Lees voor meer informatie over Role-Based Access Control, [Role-Based Access Control](./security-role-based-access-control.md).

Lees voor verificatie, [verificatie met API's](./security-authenticating-apis.md).

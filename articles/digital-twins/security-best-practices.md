---
title: Aanbevolen beveiligingsprocedures voor digitale dubbels Azure begrijpen | Microsoft Docs
description: Azure digitale dubbels best practices voor beveiliging
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: adgera
ms.openlocfilehash: 6ca01523744dbce15f8fdb3bbe2d5a9b44510f3f
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959474"
---
# <a name="security-best-practices"></a>Aanbevolen procedures voor beveiliging

Beveiliging op Azure digitale dubbels kunt nauwkeurige toegang tot bepaalde resources en acties in uw IoT-grafiek. Dit gebeurt door middel van gedetailleerde rol en beheer van machtigingen op rollen gebaseerd toegangsbeheer met de naam.

Azure van digitale dubbels maakt ook gebruik van andere beveiligingsfuncties die aanwezig zijn op Azure IoT, waaronder Azure Active Directory (Azure AD). Om die reden voor het configureren van uw app in Azure, digitale dubbels omvat het gebruik van veel van dezelfde [procedures voor Azure IoT-beveiliging](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/) momenteel aanbevolen.

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
> * Toegang en machtigingen zorgvuldig beperken door de rol. Zie de sectie 'Role-based access control best practices' is.
> * Gebruik krachtige codering. Lange wachtwoorden en gebruiken van beveiligde protocollen en verificatie met twee factoren.

IoT-resources om te bekijken voor uitschieters, bedreigingen of Resourceparameters die het bereik van de normale bewerking buiten bewaken. Gebruik Azure Analytics voor het beheren van de bewaking.

> [!NOTE]
> Zie voor meer informatie over gebeurtenis verwerken en bewaking [doorsturen van gebeurtenissen en -berichten met Azure digitale dubbels](./concepts-events-routing.md).

## <a name="azure-active-directory-best-practices"></a>Aanbevolen procedures van Azure Active Directory

Azure van digitale dubbels maakt gebruik van Azure AD bij het verifiëren van gebruikers en toepassingen te beschermen. Azure AD ondersteunt verificatie voor een verscheidenheid aan moderne architecturen. Ze zijn gebaseerd op standaardprotocollen zoals OAuth 2.0 of OpenID Connect. Enkele belangrijke procedures voor het beveiligen van uw IoT-ruimte voor Azure AD zijn onder andere:

> [!div class="checklist"]
> * Zoals Azure AD appgeheimen en sleutels op een veilige locatie Store [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).
> * Gebruik van een certificaat dat is uitgegeven door een vertrouwde [certificeringsinstantie](https://docs.microsoft.com/azure/active-directory/authentication/active-directory-certificate-based-authentication-get-started) in plaats van appgeheimen om te verifiëren.
> * OAuth 2.0-bereik van de toegang voor een token beperken.
> * Controleer of de hoeveelheid tijd die een token geldig is en of u een token geldig blijft.
> * Juiste lengte van de tijd die tokens geldig zijn voor ingesteld.
> * Verlopen vernieuwingstokens.

## <a name="role-based-access-control-best-practices"></a>Aanbevolen procedures voor toegang op basis van rollen beheren

[!INCLUDE [digital-twins-rbac-best-practices](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over best practices voor Azure IoT [best practices voor IoT-beveiliging](https://docs.microsoft.com/azure/iot-fundamentals/iot-security-best-practices?context=azure/iot-hub/).

Lees voor meer informatie over op rollen gebaseerd toegangsbeheer, [Role-based access control van](./security-role-based-access-control.md).

Lees voor meer informatie over verificatie, [verifiëren met API's](./security-authenticating-apis.md).

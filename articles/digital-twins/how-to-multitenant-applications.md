---
title: Multitenant-toepassingen met Azure digitale dubbels inschakelen | Microsoft Docs
description: Klik hier voor meer informatie over het configureren van Azure Active Directory-toepassingen voor meerdere tenants voor digitale dubbels van Azure.
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2019
ms.author: mavoge
ms.openlocfilehash: 443a697a576aa26fe352d8ad47d9e61214c3fcf3
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54024107"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Multitenant-toepassingen met Azure digitale dubbels inschakelen

Oplossingen voor ontwikkelaars die op Azure digitale dubbels bouwen kunnen vinden die ze willen ondersteuning voor meerdere klanten met één service of oplossing. In feite *multitenant* toepassingen zijn een van de meest voorkomende digitale dubbels Azure-configuraties.

Dit document wordt beschreven hoe het configureren van apps digitale dubbels Azure ter ondersteuning van verschillende tenants van Azure Active Directory en klanten.

## <a name="multitenancy"></a>Multitenancy

Een *multitenant* resource is een ingerichte exemplaar die ondersteuning biedt voor meerdere klanten. Elke klant heeft een eigen onafhankelijke gegevens en bevoegdheden. Elke klant ervaring is geïsoleerd van elkaars zodat hun "weergeven" van de toepassing is niet hetzelfde.

Lees voor meer informatie over multitenancy [Multitenant-toepassingen in Azure](https://docs.microsoft.com/azure/dotnet-develop-multitenant-applications).

## <a name="problem-scenario"></a>Probleem-scenario

In dit scenario kunt u een ontwikkelaar van het bouwen van een digitale dubbels Azure-oplossing (**DEVELOPER**) en een klant die gebruikmaakt van die oplossing (**klant**):

- **DEVELOPER** heeft een Azure-abonnement met een Azure Active Directory-tenant.
- **DEVELOPER** implementeert u een exemplaar van Azure digitale Twins in hun Azure-abonnement. Azure Active Directory automatisch een service-principal gemaakt in **DEVELOPER**van Azure Active Directory-tenant.
- Gebruikers binnen **DEVELOPER**de Azure Active Directory-tenant kunnen vervolgens [verkrijgen van tokens van OAuth 2.0](./security-authenticating-apis.md) van de digitale dubbels Azure-service.
- **DEVELOPER** maakt nu een mobiele app, die rechtstreeks worden geïntegreerd met de Azure digitale dubbels Management API's.
- **DEVELOPER** kunt **klant** het gebruik van de mobiele toepassing.
- **KLANT** moet worden gemachtigd om te gebruiken de API in Azure, digitale Management dubbels **DEVELOPER**van toepassing.

Het probleem:

- Wanneer **klant** meldt zich aan bij **DEVELOPER**van toepassing, de app kan niet verkrijgen van tokens voor **klant**van gebruikers te verifiëren met de Azure digitale dubbels Management API's.
- Een uitzondering wordt uitgegeven in Azure Active Directory die aangeeft dat Azure digitale dubbels wordt niet herkend in **klant**van directory.

## <a name="problem-solution"></a>Probleem-oplossing

Als u wilt het vorige scenario voor het probleem is opgelost, de volgende acties zijn nodig voor het maken van een digitale dubbels van Azure service-principal binnen de **klant**van Azure Active Directory-tenant:

- Als **klant** niet al een Azure-abonnement met een Azure Active Directory-tenant:

  - **KLANT**van Azure Active Directory-tenantbeheerder moet verkrijgen een [betalen per gebruik Azure-abonnement](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - **KLANT**de tenantbeheerder van Azure Active Directory en vervolgens moet [hun tenant met het nieuwe abonnement koppelen](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity).

- Op de [Azure-portal](https://portal.azure.com), **klant**van Azure Active Directory-tenant-beheerder heeft de volgende stappen uit:

  1. Open **abonnementen**.
  1. Selecteer het abonnement met de Azure Active Directory-tenant moet worden gebruikt **DEVELOPER**van toepassing.

     ![Azure Active Directory-abonnementen][1]

  1. Selecteer **Resourceproviders**.
  1. Zoeken naar **Microsoft.IoTSpaces**.
  1. Selecteer **Registreren**.

     ![Azure Active Directory-resourceproviders][2]
  
## <a name="next-steps"></a>Volgende stappen

- Lees voor meer informatie over het gebruik van de gebruiker gedefinieerde functies met Azure digitale dubbels [over het maken van de gebruiker gedefinieerde functies van Azure digitale dubbels](./how-to-user-defined-functions.md).

- Lees meer over het gebruik van op rollen gebaseerd toegangsbeheer voor het beveiligen van de toepassing met roltoewijzingen meer [over het maken en beheren van Azure digitale dubbels op rollen gebaseerd toegangsbeheer](./security-create-manage-role-assignments.md).

<!-- Images -->
[1]: media/multitenant/ad-subscriptions.png
[2]: media/multitenant/ad-resource-providers.png

---
title: Het inschakelen van multitenant-toepassingen met Azure digitale dubbels | Microsoft Docs
description: Informatie over hoe u uw klanten Azure Active Directory-tenants registreren bij Azure Digital dubbels
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: mavoge
ms.openlocfilehash: e465ab95b670268f8ef31472259783fce8f24b36
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324161"
---
# <a name="how-to-enable-multitenant-applications-with-azure-digital-twins"></a>Multitenant-toepassingen met Azure digitale dubbels inschakelen

Ontwikkelaars die gebruikmaken van de digitale dubbels Azure-platform waarschijnlijk willen multitenant-toepassingen te bouwen. Een *multitenant-toepassing* is een ingerichte exemplaar gebruikt ter ondersteuning van meerdere klanten met hun eigen onafhankelijke gegevens en bevoegdheden.

Dit document wordt uitgelegd hoe een Azure digitale dubbels multitenant-app ondersteunt verschillende tenants van Azure Active Directory (AD) en klanten te maken.

## <a name="scenario-summary"></a>Scenario-overzicht

In dit scenario, houd rekening met Developer D en klant C:

- Ontwikkelaar D heeft een Azure-abonnement met een Azure AD-tenant.
- Developer D is een exemplaar van de digitale Twins in hun Azure-abonnement worden geïmplementeerd.
- Gebruikers binnen de Azure AD-tenant van de ontwikkelaar D krijgt tokens op basis van de service digitale dubbels omdat Azure AD is een service-principal in Azure AD-tenant van de D-ontwikkelaar gemaakt.
- Developer D maakt nu een mobiele app, die rechtstreeks worden geïntegreerd met digitale dubbels Management-API.
- Klant C kan Developer D vervolgens het gebruik van de mobiele toepassing.
- Klant C moet nu worden geautoriseerd gebruik van de digitale dubbels Management-API in Developer D van toepassing.

  > [!IMPORTANT]
  > - Als klant C meldt zich aan bij de ontwikkelaar D van toepassing, kunnen de app te verkrijgen van tokens voor gebruikers van de klant C om te communiceren met de API Management niet mogelijk.
  > - Azure AD genereert vervolgens een foutbericht dat aangeeft dat digitale dubbels binnen de klant C-map wordt niet herkend.

## <a name="solution"></a>Oplossing

De volgende acties zijn om op te lossen de bovenstaande scenario, nodig om een digitale Twins-service-principal in Azure AD-tenant van de klant C maken:

- Als klant C niet al een Azure-abonnement met Azure AD-tenant:

  - Klant C van Azure AD-Tenant-beheerder aan te schaffen moet een [betalen per gebruik Azure-abonnement](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - Azure AD-Tenant Klantbeheerder van C vervolgens moet [hun tenant met het nieuwe abonnement koppelen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- Uit de [Azure Portal](https://portal.azure.com), klikt u vervolgens moet de klant C van Azure AD-Tenant-beheerder:
  1. Open **abonnementen**.
  1. Selecteer het abonnement met de Azure AD-tenant in Developer D van toepassing moet worden gebruikt.
  1. Selecteer **Resourceproviders**.
  1. Zoeken naar **Microsoft.IoTSpaces**.
  1. Klik op **registreren**.
  
## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over het gebruik van de gebruiker gedefinieerde functies met Azure digitale dubbels [Azure digitale dubbels UDF's](how-to-user-defined-functions.md).

Lees meer over het gebruik van op rollen gebaseerd toegangsbeheer voor het beveiligen van de toepassing met roltoewijzingen meer [digitale dubbels op rollen gebaseerd toegangsbeheer](security-create-manage-role-assignments.md).

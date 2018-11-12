---
title: Multitenant-toepassingen met Azure digitale dubbels inschakelen | Microsoft Docs
description: Informatie over hoe u uw klanten Azure Active Directory-tenants registreren bij Azure Digital dubbels
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: mavoge
ms.openlocfilehash: a2d9ece119003c341f49ee03d735d5636b179a32
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259884"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Multitenant-toepassingen met Azure digitale dubbels inschakelen

Ontwikkelaars die gebruikmaken van Azure digitale dubbels meestal wilt multitenant-toepassingen te bouwen. Een *multitenant-toepassing* is een ingerichte exemplaar die ondersteuning biedt voor meerdere klanten. Elke klant heeft een eigen onafhankelijke gegevens en bevoegdheden.

Dit document wordt uitgelegd hoe een Azure digitale dubbels multitenant-app die ondersteuning biedt voor meerdere tenants van Azure Active Directory (Azure AD) en klanten te maken.

## <a name="scenario-summary"></a>Scenario-overzicht

In dit scenario, houd rekening met Developer D en klant C:

- Ontwikkelaar D heeft een Azure-abonnement met een Azure AD-tenant.
- Developer D implementeert u een exemplaar van Azure digitale Twins in hun Azure-abonnement.
- Gebruikers binnen de Azure AD-tenant van de ontwikkelaar D krijgt tokens op basis van de service Azure digitale dubbels omdat Azure AD een service-principal in Azure AD-tenant van de ontwikkelaar D gemaakt.
- Developer D maakt nu een mobiele app die rechtstreeks is geïntegreerd met de Azure API voor beheer van digitale dubbels.
- Developer D kunt C van de klant het gebruik van de mobiele toepassing.
- Klant C worden gemachtigd om de digitale dubbels Management API in Azure gebruikt in Developer D van toepassing.

  > [!IMPORTANT]
  > - Als klant C meldt zich aan bij de ontwikkelaar D van toepassing, kan de app-tokens voor gebruikers van de klant C om te communiceren met de API Management niet verkrijgen.
  > - Azure AD genereert een fout, waarmee wordt aangegeven dat Azure digitale dubbels binnen de klant C-map wordt niet herkend.

## <a name="solution"></a>Oplossing

De volgende acties zijn om op te lossen het voorgaande scenario, nodig om een digitale dubbels van Azure service-principal in Azure AD-tenant van de klant C maken:

- Als klant C niet al een Azure-abonnement met een Azure AD-tenant:

  - Klant C van Azure AD-tenant-beheerder moet verkrijgen een [betalen per gebruik Azure-abonnement](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - Klant C van Azure AD-tenant-beheerder moet [hun tenant met het nieuwe abonnement koppelen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- Op de [Azure-portal](https://portal.azure.com), klant C van Azure AD-tenant-beheerder heeft de volgende stappen uit:

  1. Open **abonnementen**.
  1. Selecteer het abonnement met de Azure AD-tenant in Developer D van toepassing moet worden gebruikt.
  1. Selecteer **Resourceproviders**.
  1. Zoeken naar **Microsoft.IoTSpaces**.
  1. Selecteer **Registreren**.
  
## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over het gebruik van de gebruiker gedefinieerde functies met Azure digitale dubbels [Azure digitale dubbels UDF's](how-to-user-defined-functions.md).

Lees meer over het gebruik van op rollen gebaseerd toegangsbeheer voor het beveiligen van de toepassing met roltoewijzingen meer [Azure digitale dubbels op rollen gebaseerd toegangsbeheer](security-create-manage-role-assignments.md).

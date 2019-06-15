---
title: Hoe weet ik als een Azure AD-aanmeldingspagina Microsoft-accounts accepteert | Microsoft Docs
description: Hoe op het scherm messaging gebruikersnaam lookup weergegeven tijdens het aanmelden
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 04/10/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d26ff0f9259e3531259673f94fe477444cc786b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60468200"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Aanmeldingsopties voor Microsoft-accounts in Azure Active Directory

De Microsoft 365-aanmeldingspagina voor Azure Active Directory (Azure AD) biedt ondersteuning voor werk of schoolaccounts en Microsoft-accounts, maar afhankelijk van de situatie van de gebruiker, wordt een of andere of beide. Bijvoorbeeld, ondersteunt de aanmeldingspagina van Azure AD:

* Apps die aanmeldingen vanaf beide typen account accepteren
* Organisaties die gasten accepteren

## <a name="identification"></a>Identificatie
U kunt zien als uw organisatie gebruikmaakt van pagina biedt ondersteuning voor Microsoft-accounts door te kijken naar de tekst van de hint in het gebruikersnaamveld. Als de tekst voor hint aangeeft dat 'E-mailadres, telefoonnummer of Skype', de aanmeldingspagina biedt ondersteuning voor Microsoft-accounts.

![Verschil tussen de account aanmelden pagina 's](./media/signin-account-support/ui-prompt.png)

[Aanvullende opties voor aanmelden werkt alleen voor persoonlijke Microsoft-accounts](https://azure.microsoft.com/updates/microsoft-account-signin-options/ ) , maar kan niet worden gebruikt voor het aanmelden met werk of school-accountresources.

## <a name="next-steps"></a>Volgende stappen

[Meld u in uw huisstijl aanpassen](../fundamentals/add-custom-domain.md)
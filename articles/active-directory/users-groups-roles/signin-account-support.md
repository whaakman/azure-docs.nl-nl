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
ms.date: 04/08/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a62c4d56fbfca34ff6291863149b078f7ddc6680
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288589"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Aanmeldingsopties voor Microsoft-accounts in Azure Active Directory

De Microsoft 365-aanmeldingspagina voor Azure Active Directory (Azure AD) biedt ondersteuning voor een of beide van werk of schoolaccounts en Microsoft-accounts, afhankelijk van de situatie, ter ondersteuning van:

* Apps die aanmeldingen vanaf beide typen account accepteren
* Organisaties die gasten accepteren

## <a name="identification"></a>Identificatie
U kunt zien als uw organisatie gebruikmaakt van pagina biedt ondersteuning voor Microsoft-accounts door te kijken naar de tekst van de hint in het gebruikersnaamveld. Als de tekst voor hint aangeeft dat 'E-mailadres, telefoonnummer of Skype', de aanmeldingspagina biedt ondersteuning voor Microsoft-accounts.

![Verschil tussen de account aanmelden pagina 's](./media/signin-account-support/ui-prompt.png)

[Aanvullende opties voor aanmelden werkt alleen voor persoonlijke Microsoft-accounts](index.yml) , maar kan niet worden gebruikt voor het aanmelden met werk of school-accountresources.

## <a name="next-steps"></a>Volgende stappen

[Meld u in uw huisstijl aanpassen](../fundamentals/add-custom-domain.md)
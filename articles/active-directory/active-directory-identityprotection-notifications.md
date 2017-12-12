---
title: 'Azure Active Directory: Identity Protection-meldingen | Microsoft Docs'
description: Meer informatie over hoe de onderzoeksactiviteiten van uw ondersteuning bieden voor meldingen.
services: active-directory
keywords: beveiliging in Azure active directory-identiteit, cloud app discovery, het beheren van toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: bea21439afef4fda453732edffc84c62667dfe38
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory: Identity Protection-meldingen

Azure AD Identity Protection verzendt twee soorten e-mailmeldingen geautomatiseerde voor het risico van de gebruiker en risico's beheren:

- Gebruikers risico e gedetecteerd
- Wekelijks overzicht via e-mail

In dit artikel biedt een overzicht van beide e-mailmeldingen.


## <a name="users-at-risk-detected-email"></a>Gebruikers risico e gedetecteerd

In reactie op een gedetecteerde account risico, Azure AD Identity Protection met een e-mailmelding genereert **gebruikers risico gedetecteerd** als onderwerp. Het e-mailbericht bevat een koppeling naar de  **[gebruikers die zijn gemarkeerd voor risico](active-directory-reporting-security-user-at-risk.md)**  rapport. Als een best practice moet u de gebruikers risico onmiddellijk onderzoeken.

![Gebruikers risico e gedetecteerd](./media/active-directory-identityprotection-notifications/01.png)


### <a name="configuration"></a>Configuratie

Als beheerder, kunt u het volgende instellen:

- **Het risiconiveau die de generatie van dit e-mailbericht activeert** -het risiconiveau is standaard ingesteld op 'Hoog' risico.
- **De ontvangers van dit e-mailadres** -standaard ontvangers alle globale beheerders bevatten. Globale beheerders kunnen ook andere Hoofdbeheerders Beveiligingsbeheerder, beveiliging lezers als ontvangers toevoegen.  


Het gerelateerde dialoogvenster, klikt u op **waarschuwingen** in de **instellingen** sectie van de **Identity Protection** pagina.

![Gebruikers risico e gedetecteerd](./media/active-directory-identityprotection-notifications/05.png)


## <a name="weekly-digest-email"></a>Wekelijks overzicht via e-mail

Het wekelijkse digest-e-mailbericht bevat een overzicht van nieuwe risico's.  
Het bevat:

- Gebruikers die risico lopen

- Verdachte activiteiten

- Gedetecteerde kwetsbaarheden

- Koppelingen naar de gerelateerde rapporten in Identity Protection

    ![Herstel](./media/active-directory-identityprotection-notifications/400.png "herstel")

### <a name="configuration"></a>Configuratie

Als een beheerder, kunt u een bericht wordt verzonden wekelijkse Verificatiesamenvatting uit.

![Gebruiker risico's](./media/active-directory-identityprotection-notifications/62.png "gebruiker risico's")

Het gerelateerde dialoogvenster, klikt u op **wekelijkse Digest** in de **instellingen** sectie van de **Identity Protection** pagina.

![Gebruikers risico e gedetecteerd](./media/active-directory-identityprotection-notifications/04.png)


## <a name="see-also"></a>Zie ook

- [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

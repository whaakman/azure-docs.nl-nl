---
title: Azure Active Directory Identity Protection-meldingen | Microsoft Docs
description: Meer informatie over hoe de onderzoeksactiviteiten van uw ondersteuning bieden voor meldingen.
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0065ec03695ee977133ae2ec43aafba7d5bfff78
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784343"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection notifications

Azure AD Identity Protection verzendt twee typen e-mailmeldingen geautomatiseerde voor het beheren van gebruikersrisico en risicogebeurtenissen:

- Gebruikers die risico lopen gedetecteerd e-mailadres
- E-mail met de wekelijkse samenvatting

In dit artikel biedt een overzicht van beide e-mailmeldingen.


## <a name="users-at-risk-detected-email"></a>Gebruikers die risico lopen gedetecteerd e-mailadres

In reactie op een gedetecteerde account lopen, Azure AD Identity Protection met een e-mailmelding genereert **gebruikers gedetecteerd die risico lopen** als onderwerp. Het e-mailbericht bevat een koppeling naar de **[gebruikers die zijn gemarkeerd voor risico's](../reports-monitoring/concept-user-at-risk.md)** rapport. Als een best practice, moet u de gebruikers die risico lopen onmiddellijk onderzoeken.

De configuratie voor deze waarschuwing kunt u opgeven op welke gebruiker risiconiveau dat u wilt dat de waarschuwing is gegenereerd. Het e-mailbericht worden gegenereerd wanneer het risiconiveau van de gebruiker is bereikt wat u hebt opgegeven. maar ontvangt u geen nieuwe gebruikers lopen risico gedetecteerd e-mailwaarschuwingen voor deze gebruiker nadat ze naar het risiconiveau van deze gebruiker verplaatst. Bijvoorbeeld, als u het beleid om te waarschuwen wanneer gemiddeld gebruikersrisico en de gebruiker die Jan wordt verplaatst naar gemiddeld risico instelt, ontvangt u gebruikers lopen risico gedetecteerd e John. Echter ontvangt niet u een tweede gebruikers lopen risico gedetecteerd waarschuwing als John vervolgens naar een hoog risico verplaatst of extra risicogebeurtenissen heeft.

![Gebruikers die risico lopen gedetecteerd e-mailadres](./media/notifications/01.png)


### <a name="configuration"></a>Configuratie

Als beheerder, kunt u het volgende instellen:

- **Het risiconiveau van de gebruiker die de generatie van dit e-mailbericht activeert** -het risiconiveau is standaard ingesteld op 'Hoog' risico.
- **De ontvangers van dit e-mailbericht** -standaard ontvangers bevatten alle globale beheerders. Globale beheerders kunnen ook andere globale beheerders, Beveiligingsbeheerders, Beveiligingslezers toevoegen als ontvangers.  


De gerelateerde dialoogvenster, klikt u op **waarschuwingen** in de **instellingen** sectie van de **Identity Protection** pagina.

![Gebruikers die risico lopen gedetecteerd e-mailadres](./media/notifications/05.png)


## <a name="weekly-digest-email"></a>E-mail met de wekelijkse samenvatting

De e-mail met de wekelijkse samenvatting bevat een overzicht van nieuwe risicogebeurtenissen.  
Het bevat:

- Gebruikers die risico lopen

- Verdachte activiteiten

- Gedetecteerde kwetsbaarheden

- Koppelingen naar de gerelateerde rapporten in Identity Protection

    ![Herstel](./media/notifications/400.png "herstel")

### <a name="configuration"></a>Configuratie

Als beheerder, kunt u overschakelen een wekelijkse samenvatting e-mailbericht verzenden uitschakelen.

![Gebruiker risico's](./media/notifications/62.png "gebruiker risico's")

De gerelateerde dialoogvenster, klikt u op **wekelijkse samenvatting** in de **instellingen** sectie van de **Identity Protection** pagina.

![Gebruikers die risico lopen gedetecteerd e-mailadres](./media/notifications/04.png)


## <a name="see-also"></a>Zie ook

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

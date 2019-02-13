---
title: E-mailmeldingen in PIM - Azure | Microsoft Docs
description: Beschrijving van e-mailmeldingen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 01/05/2019
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5f4eab44a55574f84f29488e78cd60522d7a52eb
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171193"
---
# <a name="email-notifications-in-pim"></a>E-mailmeldingen in PIM

Azure AD Privileged Identity Management (PIM) laat u weten wanneer belangrijke gebeurtenissen zoals wanneer een rol is toegewezen of geactiveerd. PIM blijft u op de hoogte door te sturen dat u en andere deelnemers aan de e-mailmeldingen. Deze e-mailberichten kunnen ook koppelingen naar relevante taken, zoals activeren of vernieuwen van een rol bevatten. Dit artikel wordt beschreven hoe deze e-mailberichten eruit zien en wie deze ontvangt wanneer ze worden verzonden.

## <a name="sender-email-address-and-subject-line"></a>Afzender e-mailadres en onderwerp regel

E-mailberichten verzonden uit PIM voor zowel Azure AD en Azure-resourcerollen hebben de volgende e-mailadres van afzender:

- E-mailadres:  **azure-noreply@microsoft.com**
- Weergavenaam: Microsoft Azure

Deze e-mailberichten bevatten een **PIM** voorvoegsel in de onderwerpregel. Hier volgt een voorbeeld:

- PIM: Alain Charon is permanent toegewezen aan de rol van lezer van de back-up

## <a name="pim-emails-for-azure-ad-roles"></a>PIM-e-mailberichten voor Azure AD-rollen

PIM verzonden e-mailberichten wanneer de volgende voor Azure AD-rollen gebeurtenissen:

- Wanneer een activering van bevoorrechte rollen is in afwachting van goedkeuring
- Wanneer een aanvraag voor activering van bevoorrechte rol is voltooid
- Wanneer Azure AD PIM is ingeschakeld

Wie deze e-mailberichten voor Azure AD-rollen ontvangt is afhankelijk van uw rol, de gebeurtenis en de meldingen instellen:

| Gebruiker | Rolactivering is in afwachting van goedkeuring | De aanvraag voor rolactivering is voltooid | PIM is ingeschakeld |
| --- | --- | --- | --- |
| Beheerder met bevoorrechte rol</br>(Geactiveerd/geschikte) | Ja</br>(alleen als er geen expliciete goedkeurders zijn opgegeven) | Ja* | Ja |
| Beveiligingsbeheerder</br>(Geactiveerd/geschikte) | Nee | Ja* | Ja |
| Globale beheerder</br>(Geactiveerd/geschikte) | Nee | Ja* | Ja |

\* Als de [ **meldingen** instelling](pim-how-to-change-default-settings.md#notifications) is ingesteld op **inschakelen**.

Hieronder ziet u een voorbeeld van de e-mailbericht wordt verzonden wanneer een gebruiker een Azure AD-rol voor het fictieve bedrijf Contoso-organisatie activeert.

![Nieuwe PIM-e-mailadres voor Azure AD-rollen](./media/pim-email-notifications/email-directory-new.png)

### <a name="weekly-pim-digest-email-for-azure-ad-roles"></a>Wekelijkse e-mail met PIM de samenvatting voor Azure AD-rollen

Een wekelijkse samenvatting PIM-e-mail voor Azure AD-rollen wordt verzonden Privileged Role Administrators, Beveiligingsbeheerders en globale beheerders die PIM ingeschakeld. Dit wekelijkse e-mailbericht bevat een momentopname van de PIM-activiteiten voor de week, evenals de toewijzing van bevoorrechte rollen. Het is alleen beschikbaar voor tenants in de openbare cloud. Hier volgt een voorbeeld van de e-mailbericht:

![Wekelijkse e-mail met PIM de samenvatting voor Azure AD-rollen](./media/pim-email-notifications/email-directory-weekly.png)

De e-mailbericht bevat vier tegels:

| Naast elkaar | Description |
| --- | --- |
| **Gebruikers die zijn geactiveerd** | Aantal keren dat gebruikers hun in aanmerking komende rol binnen de tenant geactiveerd. |
| **Gebruikers permanent gemaakt** | Aantal keren dat gebruikers met een in aanmerking komende toewijzing is permanent gemaakt worden. |
| **Roltoewijzingen in PIM** | Aantal keren dat gebruikers een in aanmerking komende rol binnen PIM zijn toegewezen. |
| **Roltoewijzingen buiten PIM** | Aantal keren dat gebruikers een permanente rol buiten PIM (binnen Azure AD) is toegewezen. |

De **overzicht van de belangrijkste functies** sectie vindt u de top vijf functies in uw tenant op basis van totaal aantal permanente en in aanmerking komende beheerders voor elke rol. De **actie ondernemen** koppeling wordt geopend de [PIM wizard](pim-security-wizard.md) waar u permanente beheerders kunt converteren naar in aanmerking komende beheerders in batches.

## <a name="pim-emails-for-azure-resource-roles"></a>PIM-e-mailberichten voor Azure-resourcerollen

Als de volgende gebeurtenissen voor Azure-resourcerollen plaatsvinden, PIM e-mailberichten verzendt naar eigenaars en beheerders van de gebruiker toegang:

- Wanneer een roltoewijzing is in afwachting van goedkeuring
- Wanneer een rol wordt toegewezen
- Wanneer een rol is binnenkort verlopen
- Wanneer een rol komt in aanmerking om uit te breiden
- Wanneer een rol door een eindgebruiker wordt vernieuwd
- Wanneer een aanvraag voor rolactivering is voltooid

Als de volgende gebeurtenissen voor Azure-resourcerollen plaatsvinden, PIM e-mailberichten verzendt voor eindgebruikers:

- Wanneer een rol wordt toegewezen aan de gebruiker
- Wanneer de rol van een gebruiker is verlopen
- Wanneer de rol van een gebruiker wordt uitgebreid
- Wanneer een gebruiker de aanvraag voor rolactivering is voltooid

Hieronder ziet u een voorbeeld van de e-mailbericht wordt verzonden wanneer een gebruiker de rol van een Azure-resource voor het fictieve bedrijf Contoso-organisatie is toegewezen.

![Nieuwe PIM-e-mailadres voor Azure-resourcerollen](./media/pim-email-notifications/email-resources-new.png)

## <a name="next-steps"></a>Volgende stappen

- [Azure AD directory rolinstellingen in PIM configureren](pim-how-to-change-default-settings.md)
- [Goedkeuren of weigeren van aanvragen voor Azure AD directory-rollen in PIM](azure-ad-pim-approval-workflow.md)

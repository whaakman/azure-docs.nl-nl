---
title: E-mailmeldingen in Azure AD PIM | Microsoft Docs
description: Beschrijving van e-mailmeldingen in Azure AD Privileged Identity Management (PIM)
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 07/14/2018
ms.author: rolyon
ms.reviewer: hanki
ms.custom: pim
ms.openlocfilehash: 6c329554b5854f113fb216f874fa5a918110f9c5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059779"
---
# <a name="email-notifications-in-azure-ad-pim"></a>E-mailmeldingen in Azure AD PIM

Als er belangrijke gebeurtenissen optreedt in Azure AD Privileged Identity Management (PIM), worden e-mailmeldingen verzonden naar de desbetreffende beheerder of gebruiker. PIM verzendt bijvoorbeeld e-mailberichten voor de volgende gebeurtenissen:

- Wanneer een activering van bevoorrechte rollen is in afwachting van goedkeuring
- Wanneer een aanvraag voor activering van bevoorrechte rol is goedgekeurd
- Wanneer een bevoorrechte rol wordt geactiveerd
- Wanneer een bevoorrechte rol wordt toegewezen
- Wanneer Azure AD PIM is ingeschakeld

Beginnen aan het einde van juli 2018, heeft e-mailmeldingen verzonden via PIM een nieuw e-mailadres van afzender en een nieuw visueel ontwerp. Deze update is van invloed op beide PIM voor Azure AD en PIM voor Azure-resources. Alle gebeurtenissen die al waren geactiveerd een e-mailmelding blijft een e-mail verzenden. Deze update is uitsluitend een visual wijzigen, zonder wijzigingen in functionaliteit.

## <a name="sender-email-address"></a>E-mailadres van afzender

Beginnen aan het einde van juli 2018, hebt e-mailmeldingen het volgende adres:

- E-mailadres:  **azure-noreply@microsoft.com**
- Weergavenaam: Microsoft Azure

Voorheen moest e-mailmeldingen het volgende adres:

- E-mailadres:  **azureadnotifications@microsoft.com**
- Weergavenaam: Microsoft Azure AD-Notification-Service

## <a name="pim-emails-for-azure-ad-roles"></a>PIM-e-mailberichten voor Azure AD-rollen

Beginnen aan het einde van juli 2018, hebben de PIM-e-mailmeldingen voor Azure AD-rollen voor het ontwerp van een bijgewerkt. Hieronder ziet u een voorbeeld van de e-mailbericht wordt verzonden wanneer een gebruiker een bevoorrechte rol voor het fictieve bedrijf Contoso-organisatie te activeren.

![Nieuwe PIM-e-mailadres voor Azure AD-rollen](./media/pim-email-notifications/email-directory-new.png)

Wanneer een gebruiker een bevoorrechte rol geactiveerd, moet het e-mailbericht bekeken eerder, als volgt uit.

![Oud PIM e-mailadres voor Azure AD-rollen](./media/pim-email-notifications/email-directory-old.png)

## <a name="pim-emails-for-azure-resource-roles"></a>PIM-e-mailberichten voor Azure-resourcerollen

Beginnen aan het einde van juli 2018, hebben de PIM-e-mailmeldingen voor Azure-resourcerollen het ontwerp van een bijgewerkte. Hieronder ziet u een voorbeeld van de e-mailbericht wordt verzonden wanneer een gebruiker een bevoorrechte rol voor het fictieve bedrijf Contoso-organisatie is toegewezen.

![Nieuwe PIM-e-mailadres voor Azure-resourcerollen](./media/pim-email-notifications/email-resources-new.png)

Wanneer een gebruiker kan een bevoorrechte rol is toegewezen, gezien het e-mailbericht eerder, als volgt uit.

![Oud PIM e-mailadres voor Azure-resourcerollen](./media/pim-email-notifications/email-resources-old.png)

## <a name="next-steps"></a>Volgende stappen

- [Over het beheren van instellingen voor rolactivering in Azure AD PIM](pim-how-to-change-default-settings.md)
- [Goedkeuringen in Azure AD PIM](azure-ad-pim-approval-workflow.md)

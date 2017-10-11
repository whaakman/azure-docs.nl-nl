---
title: 'Azure Active Directory: Identity Protection-meldingen | Microsoft Docs'
description: Meer informatie over hoe de onderzoeksactiviteiten van uw ondersteuning bieden voor meldingen.
services: active-directory
keywords: beveiliging in Azure active directory-identiteit, cloud app discovery, het beheren van toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 079d16bbf75cd2b3b94269d684e1ae1a0e6aa967
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory: Identity Protection-meldingen
Azure AD Identity Protection verzendt twee soorten e-mailmeldingen geautomatiseerde voor het risico van de gebruiker en risico's beheren:

* Gebruiker e-mailwaarschuwingen geknoeid
* Wekelijks overzicht via e-mail

## <a name="user-compromised-alert-email"></a>Gebruiker e-mailwaarschuwingen geknoeid
Een gebruiker verdachte e-mailmelding wordt gegenereerd wanneer een account van Azure AD Identity Protection wordt aangemerkt als geknoeid. Het e-mailbericht bevat een koppeling naar de gebruikers die zijn gemarkeerd voor risico rapport in het Identity Protection-dashboard. Het is raadzaam dat u onmiddellijk meldingen van accounts waarmee is geknoeid onderzoeken.

## <a name="weekly-digest-email"></a>Wekelijks overzicht via e-mail
Het wekelijkse digest-e-mailbericht bevat een overzicht van nieuwe risico's.<br>
Het bevat:

* Gebruikers die risico lopen
* Verdachte activiteiten
* Gedetecteerde kwetsbaarheden
* Koppelingen naar de gerelateerde rapporten in Identity Protection

<br>
![Herstel](./media/active-directory-identityprotection-notifications/400.png "herstel")
<br>

U kunt overschakelen verzenden van wekelijkse Verificatiesamenvatting uit.
<br><br>
![Gebruiker risico's](./media/active-directory-identityprotection-notifications/62.png "gebruiker risico's")
<br>

**Opent het dialoogvenster gerelateerde configuratie**:

1. Op de **Azure AD Identity Protection** blade, klikt u op **instellingen**.
   <br><br>
   ![Gebruikersbeleid risico](./media/active-directory-identityprotection-notifications/401.png "risico gebruikersbeleid")
   <br>
2. In de **algemene** sectie, klikt u op **meldingen**.
   <br><br>
   ![Gebruikersbeleid risico](./media/active-directory-identityprotection-notifications/405.png "risico gebruikersbeleid")
   <br>

## <a name="see-also"></a>Zie ook
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

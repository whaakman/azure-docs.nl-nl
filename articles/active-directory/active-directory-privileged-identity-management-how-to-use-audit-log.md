---
title: Het gebruik van het controlelogboek in Azure AD Privileged Identity Management | Microsoft Docs
description: Informatie over het gebruik van het controlelogboek in de extensie Azure Privileged Identity Management.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 5d13a6dd-1fcb-4e76-82fb-cb2f4f0e4357
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: dd6cecf9211dada02025d93e4d63234ff0a73949
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="using-the-audit-log-in-pim"></a>Met behulp van het controlelogboek in PIM
U kunt het controlelogboek Privileged Identity Management (PIM) gebruiken om te zien van de toewijzingen van gebruikers en activeringen binnen een bepaalde periode. Als u zien van de volledige controlegeschiedenis van activiteit in uw tenant wilt, met inbegrip van de beheerder, eindgebruikers en synchronisatieactiviteiten, kunt u de [Azure Active Directory-toegang en gebruik rapporten.](active-directory-view-access-usage-reports.md)

## <a name="navigate-to-the-audit-log"></a>Navigeer naar het controlelogboek
Van de [Azure-portal](https://portal.azure.com) dashboard, selecteer de **Azure AD Privileged Identity Management** app. Van daaruit toegang krijgen tot het controlelogboek door te klikken op **bevoorrechte rollen beheren** > **controlegeschiedenis** in het PIM-dashboard.

## <a name="the-audit-log-graph"></a>De audit log-grafiek
Het controlelogboek kunt u het totale aantal activeringen, max activeringen per dag en gemiddelde activeringen per dag weergeven in een lijndiagram.  U kunt ook de gegevens filteren per rol als er meer dan één rol in de controlegeschiedenis.

Gebruik de **tijd**, **actie**, en **rol** knoppen om te sorteren van het logboek.

## <a name="the-audit-log-list"></a>De lijst van de controle-logboek
De kolommen in de lijst van audit log zijn:

* **Aanvrager** -de gebruiker die de rol activeren of wijzigen.  Als de waarde is 'Azure systeem', controleert u het controlelogboek voor Azure voor meer informatie.
* **Gebruiker** -de gebruiker die geactiveerd of is toegewezen aan een rol is.
* **Rol** -de rol toegewezen of geactiveerd door de gebruiker.
* **Actie** : de acties die door de aanvrager. Het kan hierbij gaan toewijzing, ongedaan maken, activeren of deactiveren.
* **Tijd** : wanneer de actie is opgetreden.
* **Redeneren** -als de tekst in het veld reden is ingevoerd tijdens de activering, deze hier wordt weergegeven.
* **Vervaldatum** - alleen relevant voor de activering van rollen.

## <a name="filter-the-audit-log"></a>Het controlelogboek filteren
U kunt de informatie filteren die wordt weergegeven in het controlelogboek door te klikken op de **Filter** knop.  De **Update grafiek parameters blade** wordt weergegeven.

Nadat u de filters hebt ingesteld, klikt u op **Update** om de gegevens in het logboek te filteren.  Als de gegevens niet meteen weergegeven, vernieuw de pagina.

### <a name="change-the-date-range"></a>Wijzig het datumbereik
Gebruik de **vandaag**, **afgelopen Week**, **afgelopen maand**, of **aangepaste** knoppen om te wijzigen van het tijdsbereik van het controlelogboek.

Als u ervoor kiest de **aangepaste** knop, krijgt u een **van** datumveld en een **naar** datumveld om op te geven van een datumbereik voor het logboek.  U kunt datums opgeven in de notatie DD-MM-jjjj of klik op de **kalender** pictogram en kies de datum in een kalender.

### <a name="change-the-roles-included-in-the-log"></a>Wijzigen van de rollen die zijn opgenomen in het logboek
Schakel of de **rol** selectievakje naast elke rol wilt opnemen of uitsluiten van het logboek.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


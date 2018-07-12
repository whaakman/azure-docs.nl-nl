---
title: Over het gebruik van het auditlogboek in Azure AD Privileged Identity Management | Microsoft Docs
description: Informatie over het gebruik van het auditlogboek opgenomen in de Azure Privileged Identity Management-extensie.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: ca6bb987c0f977746a8359dfd40cf7a3d643950a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590294"
---
# <a name="using-the-audit-log-in-pim"></a>Met behulp van het auditlogboek in PIM
U kunt het auditlogboek Privileged Identity Management (PIM) gebruiken om te zien van alle toewijzingen van gebruikers en activeringen binnen een bepaalde periode. Als u wilt zien van de volledige controlegeschiedenis van activiteit in uw tenant, met inbegrip van de beheerder, eindgebruikers en synchronisatieactiviteit, kunt u de [Azure Active Directory-toegang en gebruik rapporten.](../active-directory-reporting-azure-portal.md)

## <a name="navigate-to-the-audit-log"></a>Navigeer in het auditlogboek opgenomen
Uit de [Azure-portal](https://portal.azure.com) dashboard, selecteer de **Azure AD Privileged Identity Management** app. Van daaruit toegang krijgen tot het auditlogboek door te klikken op **bevoorrechte rollen beheren** > **controlegeschiedenis** in de PIM-dashboard.

## <a name="the-audit-log-graph"></a>De audit log-grafiek
U kunt het auditlogboek gebruiken om de totale activeringen, max activeringen per dag en gemiddelde activeringen per dag in een lijndiagram weer te geven.  U kunt ook de gegevens filteren op rol, als er meer dan één rol in de controlegeschiedenis.

Gebruik de **tijd**, **actie**, en **rol** knoppen voor het sorteren van het logboek.

## <a name="the-audit-log-list"></a>De lijst met audit log
De kolommen in de lijst met audit log zijn:

* **Aanvrager** -de gebruiker die de rolactivering of wijzigen.  Als de waarde 'Azure systeem', controleert u het Azure-auditlogboek voor meer informatie.
* **Gebruiker** -de gebruiker die geactiveerd of toegewezen aan een rol is.
* **Rol** -de rol toegewezen of geactiveerd door de gebruiker.
* **Actie** - de acties die door de aanvrager. Dit kunnen bijvoorbeeld toewijzing, ongedaan, activeren of deactiveren.
* **Tijd** : wanneer de actie is opgetreden.
* **Redeneren** -als de tekst in het veld reden tijdens de activering is ingevoerd, wordt het hier weergegeven.
* **Vervaldatum** - alleen relevant voor activering van rollen.

## <a name="filter-the-audit-log"></a>Het controlelogboek filteren
U kunt de informatie die wordt weergegeven in het auditlogboek door te klikken op filteren de **Filter** knop.  De **Update grafiek parameters blade** wordt weergegeven.

Nadat u de filters hebt ingesteld, klikt u op **Update** om de gegevens in het logboek te filteren.  Als de gegevens niet meteen wordt weergegeven, vernieuw de pagina.

### <a name="change-the-date-range"></a>Wijzig het datumbereik
Gebruik de **vandaag**, **afgelopen Week**, **afgelopen maand**, of **aangepaste** knoppen om te wijzigen van het tijdsbereik van het auditlogboek.

Als u ervoor kiest de **aangepaste** knop klikt, krijgt u een **van** datumveld en een **naar** datumveld om op te geven van een datumbereik voor het logboek.  U kunt de datums invoeren in MM/DD/JJJJ-notatie of klik op de **agenda** pictogram en kiest u de datum in een agenda.

### <a name="change-the-roles-included-in-the-log"></a>Wijzigen van de rollen die zijn opgenomen in het logboek
Controleer of schakel de **rol** selectievakje in naast elke rol moeten worden opgenomen of uitgesloten van het logboek.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]


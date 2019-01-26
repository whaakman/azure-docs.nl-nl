---
title: Controlegeschiedenis voor Azure AD-directory-rollen in PIM weergeven | Microsoft Docs
description: Informatie over het weergeven van de controlegeschiedenis voor Azure AD directory-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 191d384d51fe3aecbd967586516cfb198256af81
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911724"
---
# <a name="view-audit-history-for-azure-ad-directory-roles-in-pim"></a>Controlegeschiedenis voor Azure AD directory-rollen in PIM weergeven
U kunt de controlegeschiedenis Privileged Identity Management (PIM) gebruiken om te zien van alle toewijzingen van gebruikers en activeringen binnen een bepaalde periode voor alle bevoorrechte rollen. Als u wilt zien van de volledige controlegeschiedenis van activiteit in uw tenant, met inbegrip van de beheerder, eindgebruikers en synchronisatieactiviteit, kunt u de [Azure Active Directory-toegang en gebruik rapporten.](../reports-monitoring/overview-reports.md)

## <a name="navigate-to-audit-history"></a>Navigeer om controlegeschiedenis
Uit de [Azure-portal](https://portal.azure.com) dashboard, selecteer de **Azure AD Privileged Identity Management** app. Van daaruit toegang krijgen tot de controlegeschiedenis door te klikken op **bevoorrechte rollen beheren** > **controlegeschiedenis** in de PIM-dashboard.

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
Sorteer de gegevens door de actie en zoek naar "Activeren goedgekeurd"


## <a name="audit-history-graph"></a>Audit-grafiek
U kunt de controlegeschiedenis gebruiken om de totale activeringen, max activeringen per dag en gemiddelde activeringen per dag in een lijndiagram weer te geven.  U kunt ook de gegevens filteren op rol, als er meer dan één rol in de controlegeschiedenis.

Gebruik de **tijd**, **actie**, en **rol** knoppen voor het sorteren van de geschiedenis.

## <a name="audit-history-list"></a>Lijst van de geschiedenis van controle
De kolommen in de lijst van de geschiedenis audit zijn:

* **Aanvrager** -de gebruiker die de rolactivering of wijzigen.  Als de waarde 'Azure systeem', controleert u de controlegeschiedenis voor Azure voor meer informatie.
* **Gebruiker** -de gebruiker die geactiveerd of toegewezen aan een rol is.
* **Rol** -de rol toegewezen of geactiveerd door de gebruiker.
* **Actie** - de acties die door de aanvrager. Dit kunnen bijvoorbeeld toewijzing, ongedaan, activeren of deactiveren.
* **Tijd** : wanneer de actie is opgetreden.
* **Redeneren** -als de tekst in het veld reden tijdens de activering is ingevoerd, wordt het hier weergegeven.
* **Vervaldatum** - alleen relevant voor activering van rollen.

## <a name="filter-audit-history"></a>Controlegeschiedenis filteren
U kunt de informatie die wordt weergegeven in de controlegeschiedenis door te klikken op filteren de **Filter** knop.  De **Update grafiek parameters blade** wordt weergegeven.

Nadat u de filters hebt ingesteld, klikt u op **Update** om de gegevens in de geschiedenis te filteren.  Als de gegevens niet meteen wordt weergegeven, vernieuw de pagina.

### <a name="change-the-date-range"></a>Wijzig het datumbereik
Gebruik de **vandaag**, **afgelopen Week**, **afgelopen maand**, of **aangepaste** knoppen om te wijzigen van het tijdsbereik van de controlegeschiedenis.

Als u ervoor kiest de **aangepaste** knop klikt, krijgt u een **van** datumveld en een **naar** datumveld om op te geven van een datumbereik voor de geschiedenis.  U kunt de datums invoeren in MM/DD/JJJJ-notatie of klik op de **agenda** pictogram en kiest u de datum in een agenda.

### <a name="change-the-roles-included-in-the-history"></a>Wijzigen van de rollen die zijn opgenomen in de geschiedenis
Controleer of schakel de **rol** selectievakje in naast elke rol moeten worden opgenomen of uitgesloten van de geschiedenis.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

- [Activiteiten bekijken en controlegeschiedenis voor Azure-resource-rollen in PIM](azure-pim-resource-rbac.md)

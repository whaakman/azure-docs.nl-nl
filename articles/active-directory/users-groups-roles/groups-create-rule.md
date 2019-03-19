---
title: Een dynamische groep maken en controleer de status - Azure Active Directory | Microsoft Docs
description: Hoe u een regel voor groepslidmaatschap maken in Azure portal, de status controleren.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/01/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 79b40af914cd55ae37205123df595ab177c87752
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58098199"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Een dynamische groep maken en de status controleren

In Azure Active Directory (Azure AD), kunt u regels gebruiken om te bepalen op basis van gebruiker of het apparaat eigenschappen groepslidmaatschap. In dit artikel wordt uitgelegd hoe het instellen van een regel voor een dynamische groep in Azure portal.
Dynamisch lidmaatschap wordt voor beveiligingsgroepen of Office 365-groepen ondersteund. Wanneer een regel voor groepslidmaatschap wordt toegepast, worden gebruikers- en apparaatkenmerken worden geëvalueerd op overeenkomsten met het lidmaatschapsregel. Wanneer een kenmerk voor een gebruiker of apparaat, worden alle dynamische groepsregels in de organisatie voor wijzigingen in het lidmaatschap verwerkt. Gebruikers en apparaten worden toegevoegd of verwijderd als ze voldoen aan de voorwaarden voor een groep.

Zie voor meer voorbeelden van syntaxis, ondersteunde eigenschappen, operators en waarden voor de regel voor een verzameling [dynamisch-lidmaatschapregels voor groepen in Azure Active Directory](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>Een regel voor groepslidmaatschap maken

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat in de globale beheerder, Intune-beheerder of de beheerdersrol gebruiker in de tenant.
2. Selecteer **groepen**.
3. Selecteer **alle groepen**, en selecteer **nieuwe groep**.

   ![Nieuwe groep toevoegen](./media/groups-create-rule/new-group-creation.png)

4. Op de **groep** pagina, voer een naam en beschrijving voor de nieuwe groep. Selecteer een **lidmaatschapstype** voor gebruikers of apparaten en selecteer vervolgens **dynamische query toevoegen**. U kunt de opbouwfunctie voor regel gebruiken om te maken van een eenvoudige regel of [zelf schrijven van een lidmaatschapsregel](groups-dynamic-membership.md).

   ![Regel voor dynamisch lidmaatschap toevoegen](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Om te zien van de aangepaste extensie-eigenschappen die beschikbaar zijn voor uw lidmaatschapquery
   1. Selecteer **aangepaste extensie-eigenschappen ophalen**
   2. Voer de toepassings-ID en selecteer vervolgens **eigenschappen vernieuwen**. 
6. Na het maken van de regel, selecteer **query toevoegen** aan de onderkant van de blade.
7. Selecteer **maken** op de **groep** blade om de groep te maken.

Als de regel die u hebt ingevoerd is niet geldig, wordt een uitleg waarom de regel kan niet worden verwerkt in de rechterbovenhoek van de portal weergegeven. Lees deze zorgvuldig voor meer informatie over het oplossen van de regel.

## <a name="turn-on-or-off-welcome-email"></a>Schakel in of uit de verwelkomingse-mail

Wanneer een nieuwe Office 365-groep is gemaakt, wordt de gebruikers die zijn toegevoegd aan de groep in een Welkom melding verzonden. Als kenmerken van een gebruiker of apparaat wijzigt, worden alle dynamische groepsregels in de organisatie later verwerkt voor wijzigingen in het lidmaatschap. Gebruikers die zijn toegevoegd vervolgens ook de Welkom melding ontvangen. U kunt dit gedrag in uitschakelen [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps). 

## <a name="check-processing-status-for-a-rule"></a>Controleer de verwerkingsstatus voor een regel

Ziet u het lidmaatschap van de status en het laatst bijgewerkt op verwerking op de **overzicht** pagina voor de groep.
  
  ![dynamische groep status weergeven](./media/groups-create-rule/group-status.png)

De volgende statusberichten kunnen worden weergegeven voor **lidmaatschap verwerking** status:

* **Evaluatie van**:  De groepswijziging is ontvangen en de updates worden geëvalueerd.
* **Verwerking van**: Updates worden verwerkt.
* **Bijwerken voltooid**: Verwerking is voltooid en alle toepasselijke updates zijn aangebracht.
* **Verwerkingsfout**:  Verwerking kan niet worden voltooid vanwege een fout bij het evalueren van de lidmaatschapsregel.
* **Update onderbroken**: Dynamische lidmaatschap regel updates zijn onderbroken door de beheerder. MembershipRuleProcessingState is ingesteld op 'Onderbroken'.

De volgende statusberichten kunnen worden weergegeven voor **lidmaatschap laatst bijgewerkt** status:

* &lt;**Datum en tijd**&gt;: De laatste keer dat het lidmaatschap is bijgewerkt.
* **Bezig**: Updates worden momenteel uitgevoerd.
* **Onbekende**: De laatste updatetijd kan niet worden opgehaald. De groep mogelijk nieuwe.

Als er een fout optreedt tijdens het verwerken van de lidmaatschapsregel voor een specifieke groep, een waarschuwing wordt weergegeven bovenaan het **overzichtspagina** voor de groep. Als niet in afwachting van lidmaatschap van dynamische updates kunnen worden verwerkt voor alle groepen in de tenant voor meer dan 24 uur, een waarschuwing wordt weergegeven boven in het **alle groepen**.

![verwerking van foutbericht](./media/groups-create-rule/processing-error.png)

Deze artikelen bevatten aanvullende informatie over groepen in Azure Active Directory.

* [Bestaande groepen weergeven](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toevoegen](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Lidmaatschappen van een groep beheren](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](groups-dynamic-membership.md)

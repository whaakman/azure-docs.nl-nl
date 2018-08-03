---
title: Een dynamische groep maken en controleer de status in Azure Active Directory | Microsoft Docs
description: Over het maken van een groepslidmaatschap regels in de Azure-portal, de status controleren.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/02/2018
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 9a2eb8ab4e3ee65e97de578c825bf106aee1b829
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480337"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Een dynamische groep maken en de status controleren

In Azure Active Directory (Azure AD), kunt u groepen maken door het toepassen van een regel om te bepalen op basis van gebruiker of het apparaat eigenschappen lidmaatschap. Wanneer de kenmerken van een gebruiker of apparaat wijzigt, Azure AD evalueert alle dynamische groep regels in de Azure AD-tenant en voert een toegevoegd of verwijderd. Als een gebruiker of het apparaat voldoet aan een regel voor een groep, worden deze toegevoegd als een lid en wanneer ze niet langer voldoen aan de regel, worden ze verwijderd.

Dit artikel wordt uitgelegd hoe u een regel in de Azure-portal voor dynamisch lidmaatschap voor beveiligingsgroepen of Office 365-groepen kunt instellen. Zie voor voorbeelden van de regelsyntaxis van de en een volledige lijst van de ondersteunde eigenschappen, operators en waarden voor de regel voor een verzameling [dynamisch-lidmaatschapregels voor groepen in Azure Active Directory](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>Een regel voor groepslidmaatschap maken

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com) met een account dat een globale beheerder of een beheerder van gebruikersaccounts.
2. Selecteer **groepen**.
3. Selecteer **alle groepen**, en selecteer **nieuwe groep**.

   ![Nieuwe groep toevoegen](./media/groups-create-rule/new-group-creation.png)

4. Op de **groep** blade, voer een naam en beschrijving voor de nieuwe groep. Selecteer een **lidmaatschapstype** van een van beide **dynamische gebruiker** of **dynamisch apparaat**, afhankelijk van of u wilt maken van een regel voor gebruikers of apparaten, en selecteer vervolgens **Dynamische query toevoegen**. U kunt de opbouwfunctie voor regel gebruiken om te maken van een eenvoudige regel of een lidmaatschapsregel zelf schrijven. In dit artikel bevat meer informatie over beschikbare gebruikers- en -kenmerken als voorbeelden van regels voor groepslidmaatschap.

   ![Regel voor dynamisch lidmaatschap toevoegen](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Na het maken van de regel, selecteer **query toevoegen** aan de onderkant van de blade.
6. Selecteer **maken** op de **groep** blade om de groep te maken.

> [!TIP]
> Het maken van beveiligingsgroepen mislukt als de regel die u hebt ingevoerd onjuist gevormd of ongeldig is. Een melding wordt weergegeven in de rechterbovenhoek hoek rechtsboven van de portal, met een uitleg waarom de regel kan niet worden verwerkt. Lees zorgvuldig om te begrijpen hoe moet u de regel zodat deze geldige aanpassen.

## <a name="check-processing-status-for-a-membership-rule"></a>Controleer de verwerkingsstatus voor een lidmaatschapsregel

Ziet u het lidmaatschap van de status en het laatst bijgewerkt op verwerking op de **overzicht** pagina voor de groep.
  
  ![dynamische groep status weergeven](./media/groups-create-rule/group-status.png)

De volgende statusberichten kunnen worden weergegeven voor **lidmaatschap verwerking** status:

* **Evaluatie van**: de groepswijziging is ontvangen en de updates worden geëvalueerd.
* **Verwerking van**: Updates worden verwerkt.
* **Bijwerken voltooid**: verwerking is voltooid en alle toepasselijke updates zijn aangebracht.
* **Verwerkingsfout**: Er is een fout opgetreden tijdens het evalueren van de regel voor de verzameling en verwerking kan niet worden voltooid.
* **Update onderbroken**: updates zijn onderbroken door de beheerder van de regel voor dynamisch lidmaatschap. MembershipRuleProcessingState is ingesteld op 'Onderbroken'.

De volgende statusberichten kunnen worden weergegeven voor **lidmaatschap laatst bijgewerkt** status:

* &lt;**Datum en tijd**&gt;: de laatste keer dat het lidmaatschap is bijgewerkt.
* **Bezig**: Updates worden momenteel uitgevoerd.
* **Onbekende**: de laatste updatetijd kan niet worden opgehaald. Het kan zijn vanwege de groep wordt pas gemaakt.

Als er een fout optreedt tijdens het verwerken van de lidmaatschapsregel voor een specifieke groep, een waarschuwing wordt weergegeven bovenaan het **overzichtspagina** voor de groep. Als niet in afwachting van lidmaatschap van dynamische updates kunnen worden verwerkt voor alle groepen in de tenant voor meer dan 24 uur, een waarschuwing wordt weergegeven boven in het **alle groepen**.

![verwerking van foutbericht](./media/groups-create-rule/processing-error.png)

Deze artikelen bevatten aanvullende informatie over groepen in Azure Active Directory.

* [Bestaande groepen weergeven](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een nieuwe groep maken en leden toevoegen](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Instellingen van een groep beheren](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Lidmaatschappen van een groep beheren](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Dynamische regels voor gebruikers in een groep beheren](groups-dynamic-membership.md)

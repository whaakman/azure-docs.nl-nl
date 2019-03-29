---
title: Een toegangscontrole voor Azure AD-rollen in PIM - Azure Active Directory starten | Microsoft Docs
description: Informatie over het starten van een toegangscontrole voor Azure AD-rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5cbf96c165d79c26985663ef5a9d64bbf8f9892
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574991"
---
# <a name="start-an-access-review-for-azure-ad-roles-in-pim"></a>Een toegangscontrole voor Azure AD-rollen in PIM starten
Roltoewijzingen worden 'verouderde' wanneer gebruikers uitgebreide toegang die ze niet meer nodig hebt. Om te reduceren het risico dat samenhangt met deze verouderde roltoewijzingen bevoorrechte rol moeten medewerkers of hoofdbeheerders regelmatig maken toegangsbeoordelingen om te vragen van beheerders voor het controleren van de functies die gebruikers hebben gekregen. In dit document bevat informatie over de stappen voor het starten van een toegangscontrole in Azure Active Directory (Azure AD) Privileged Identity Management (PIM).

## <a name="start-an-access-review"></a>Een toegangsbeoordeling starten
> [!NOTE]
> Als u dit nog niet hebt de PIM-toepassing aan uw dashboard toegevoegd in de Azure-portal, raadpleegt u de stappen in [aan de slag met Azure Privileged Identity Management](pim-getting-started.md)
> 
> 

Op de toepassing hoofdpagina van PIM zijn er drie manieren om een toegangscontrole starten:

* **Toegangsbeoordelingen** > **toevoegen**
* **Rollen** > **revisie** knop
* Selecteer de specifieke rol om te worden verwijderd uit de lijst met gebruikersrollen > **revisie** knop

Als u klikt op de **bekijken** knop, de **een toegangscontrole starten** blade wordt weergegeven. Op deze blade gaat u naar de revisie configureren met een naam en een tijdslimiet, kiest u een rol om te controleren en bepalen wie verantwoordelijk is voor de beoordeling.

![Start een toegangsbeoordeling - schermafbeelding](./media/pim-how-to-start-security-review/PIM_start_review.png)

### <a name="configure-the-review"></a>Configureren van de beoordeling
Als u wilt een toegangsbeoordeling maken, moet u deze de naam en stel een begin- en datum.

![Configureren van controle - schermafbeelding](./media/pim-how-to-start-security-review/PIM_review_configure.png)

Controleer de lengte van de beoordeling lang genoeg is voor gebruikers om deze te voltooien. Als u klaar voor de einddatum bent, kunt u de controle vroeg altijd stoppen.

### <a name="choose-a-role-to-review"></a>Kies een rol om te controleren
Elke beoordeling die is gericht op slechts één rol. Tenzij u de toegangsbeoordeling vanuit de blade voor een specifieke rol gestart, moet u nu een rol kiezen.

1. Navigeer naar **rollidmaatschap beoordelen**
   
    ![Rollidmaatschap controle - schermafbeelding](./media/pim-how-to-start-security-review/PIM_review_role.png)
2. Kies één rol in de lijst.

### <a name="decide-who-will-perform-the-review"></a>Bepalen wie verantwoordelijk is voor de beoordeling
Er zijn drie opties voor het uitvoeren van een beoordeling. U kunt de beoordeling toewijzen aan iemand anders om te voltooien, zelf doen of u kunt elke gebruikersbeoordeling een eigen toegang hebben.

1. Navigeer naar **beoordelaars selecteren**
   
    ![Selecteer revisoren - schermafbeelding](./media/pim-how-to-start-security-review/PIM_review_reviewers.png)
2. Kies een van de opties:
   
   * **Selecteer de revisor**: Gebruik deze optie als u niet weet die toegang nodig heeft. Met deze optie kunt u de controle toewijzen aan een resource-eigenaar of groepmanager om te voltooien.
   * **Mij**: Dit is handig als u wilt zien hoe toegang tot beoordelingen voor werk of u wenst te namens de mensen die niet kan controleren.
   * **Leden lees zelf**: Gebruik deze optie om de gebruikers hun eigen roltoewijzingen te bekijken.

### <a name="start-the-review"></a>De revisie starten
Ten slotte hebt u de mogelijkheid om te vereisen dat gebruikers een reden opgeven als ze hun toegang goedkeuren. Indien gewenst een beschrijving van de beoordeling toevoegen en selecteer **Start**.

Zorg ervoor dat u uw gebruikers weten dat er een toegangsbeoordeling wachten tot ze en ze weergeven brengt [hoe u een toegangscontrole uitvoeren](pim-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Beheren van de toegangsbeoordeling
U kunt de voortgang volgen de revisoren hun beoordelingen in het dashboard Azure AD PIM in de sectie toegang beoordelingen zijn voltooid. Er is geen toegangsrechten worden gewijzigd in de map tot [de controle is voltooid](pim-how-to-complete-review.md).

Totdat de periode van de toegangsbeoordeling voltooid is, kunt u Herinner de gebruikers om uit te voeren van de beoordeling of stoppen van de beoordeling vroeg in het gedeelte van de beoordelingen toegang.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

- [Een toegangscontrole voor Azure AD-rollen in PIM voltooien](pim-how-to-complete-review.md)
- [Een toegangscontrole van mijn Azure AD-rollen in PIM uitvoeren](pim-how-to-perform-security-review.md)
- [Een toegangsbeoordeling starten voor mijn Azure AD-resourcerollen in PIM](pim-resource-roles-start-access-review.md)

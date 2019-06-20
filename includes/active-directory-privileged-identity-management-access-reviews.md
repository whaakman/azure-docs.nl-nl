---
title: bestand opnemen
description: bestand opnemen
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 04/29/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: 364d4a11772e6bb72e2e258503f3cce49dc61453
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176730"
---
## <a name="create-one-or-more-access-reviews"></a>Maken van een of meer toegangsbeoordelingen

1. Klik op **nieuw** naar een nieuwe toegangsbeoordeling maken.

1. De naam van de toegangsbeoordeling. (Optioneel) Geef de beoordeling van een beschrijving. De naam en beschrijving worden weergegeven aan de revisoren.

    ![Een toegangsbeoordeling - naam van de beoordeling en een beschrijving maken](./media/active-directory-privileged-identity-management-access-reviews/name-description.png)

1. Stel de **begindatum**. Standaard een toegangsbeoordeling gebeurt eenmaal, hetzelfde moment die wordt deze gemaakt begint en eindigt in één maand. Kunt u het begin en einddatum hebben een toegang controleren start in de toekomst en laatste hoeveel dagen u dat wilt.

    ![Maken van een toegangscontrole - begin- en einddatums](./media/active-directory-privileged-identity-management-access-reviews/start-end-dates.png)

1. Als u de toegang tot revisie terugkerende, wijzigen de **frequentie** instellen op basis van **één keer** naar **wekelijkse**, **maandelijkse**,  **Elk kwartaal**, **jaarlijks**, of **semi annually**. Gebruik de **duur** schuifregelaar of tekst in om te bepalen hoeveel dagen elk onderzoek van de reeks terugkerende is geopend voor invoer van de revisoren. Bijvoorbeeld, is de maximale duur die u voor een maandelijkse revisie instellen kunt 27 dagen, om te voorkomen dat overlappende beoordelingen.

1. Gebruik de **End** instelling kunt u opgeven hoe de toegang tot de terugkerende beëindigen serie bekijken. De reeks kunt op drie manieren beëindigen: deze continu wordt uitgevoerd voor het starten van beoordelingen voor onbepaalde tijd, tot een bepaalde datum of na een opgegeven aantal exemplaren is voltooid. U, Gebruikerbeheerder van een andere of een ander algemeen beheerder kunt stoppen de reeks na het maken van door het veranderen van de datum in de **instellingen**, zodat deze wordt beëindigd op die datum.

1. In de **gebruikers** sectie, selecteer een of meer rollen die u wilt bekijken van het lidmaatschap van.

    ![Een toegangsbeoordeling - gebruikers maken](./media/active-directory-privileged-identity-management-access-reviews/users.png)

    > [!NOTE]
    > Meer dan één rol selecteren maakt meerdere toegangsbeoordelingen. Bijvoorbeeld, u vijf afzonderlijke toegangsbeoordelingen maken vijf rollen selecteren.

    Als u een toegangsbeoordeling van Azure AD-rollen maakt, wordt het volgende toont een voorbeeld van de beoordeling ledenlijst wordt weergegeven.

    ![Een toegangsbeoordeling - rollidmaatschap beoordeling maken](./media/active-directory-privileged-identity-management-access-reviews/review-membership.png)

    Als u een toegangsbeoordeling van Azure-resourcerollen maakt, wordt het volgende toont een voorbeeld van de beoordeling ledenlijst wordt weergegeven.

    ![Een toegangsbeoordeling - rollidmaatschap beoordeling maken](./media/active-directory-privileged-identity-management-access-reviews/review-membership-azure-resource-roles.png)

1. In de **revisoren** sectie, selecteer een of meer mensen te bekijken van alle gebruikers. Of u kunt selecteren om de leden hun eigen toegang beoordelen.

    ![Een toegangsbeoordeling - revisoren maken](./media/active-directory-privileged-identity-management-access-reviews/reviewers.png)

    - **Geselecteerde gebruikers** -Gebruik deze optie als u niet weet die toegang nodig heeft. Met deze optie kunt u de controle toewijzen aan een resource-eigenaar of groepmanager om te voltooien.
    - **Leden (zelf)** -Gebruik deze optie om de gebruikers hun eigen roltoewijzingen bekijken.

### <a name="upon-completion-settings"></a>Na voltooiing van de instellingen

1. Om op te geven wat gebeurt er wanneer een beoordeling is voltooid, vouw de **na voltooiing van de instellingen** sectie.

    ![Na voltooiing van de instellingen](./media/active-directory-privileged-identity-management-access-reviews/upon-completion-settings.png)

1. Als u automatisch de toegang verwijderen voor gebruikers die zijn geweigerd wilt, stelt u **automatisch resultaten toepassen op resource** naar **inschakelen**. Als u de resultaten handmatig toepassen wilt als de controle is voltooid, stelt u de switch op **uitschakelen**.

1. Gebruik de **moet revisor niet reageert** lijst om op te geven wat er gebeurt met gebruikers die niet zijn beoordeeld door de revisor binnen de controleperiode. Deze instelling heeft geen invloed op gebruikers die handmatig door de revisoren zijn gecontroleerd. Als de laatste revisor besluit weigeren, wordt de toegang van de gebruiker worden verwijderd.

    - **Er is geen wijziging** -gebruikerstoegang, ongewijzigd laten
    - **Toegang verwijderen** -van de gebruiker toegang verwijderen
    - **Toegang goedkeuren** -van de gebruiker toegang goedkeuren
    - **Aanbevelingen ophalen** - nemen van het systeem de aanbeveling weigeren of goedkeuring van de gebruiker de blijvende toegang

### <a name="advanced-settings"></a>Geavanceerde instellingen

1. Als u aanvullende instellingen, vouw de **geavanceerde instellingen** sectie.

    ![Geavanceerde instellingen](./media/active-directory-privileged-identity-management-access-reviews/advanced-settings.png)

1. Stel **aanbevelingen weergeven** naar **inschakelen** om weer te geven de revisoren van het systeem aanbevelingen op basis van de gebruiker toegang tot informatie.

1. Stel **reden vereisen bij goedkeuring** naar **inschakelen** naar de revisor moet een reden voor goedkeuring opgeven.

1. Stel **e-mailmeldingen** naar **inschakelen** dat Azure AD e-mailmeldingen naar revisoren verzenden wanneer een toegangsbeoordeling wordt gestart, en naar beheerders wanneer een beoordeling is voltooid.

1. Stel **herinneringen** naar **inschakelen** om in Azure AD herinneringen stuurt van toegangsbeoordelingen wordt uitgevoerd naar de revisors die de beoordeling nog niet hebben voltooid.

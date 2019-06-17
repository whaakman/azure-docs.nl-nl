---
title: Een toegangsbeoordeling van groepen of toepassingen - Azure Active Directory maken | Microsoft Docs
description: Informatie over het maken van een toegangscontrole van leden van beveiligingsgroep of toegang tot toepassingen in Azure Active Directory-toegangsbeoordelingen.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/21/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ef72f1649c3f3e0af7fba53b2e8dbcee49d4b59
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66734568"
---
# <a name="create-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Maken van een toegangscontrole van groepen of toepassingen in Azure AD-toegangsbeoordelingen

Toegang tot groepen en toepassingen voor werknemers en gasten gewijzigd na verloop van tijd. Beheerders kunnen Azure Active Directory (Azure AD) gebruiken voor het maken van toegangsbeoordelingen voor groepsleden of toegang tot toepassingen, verminderen het risico dat samenhangt met verouderde toegangstoewijzingen. Als u regelmatig toegang beoordelen wilt, kunt u ook terugkerende toegangsbeoordelingen te maken. Zie voor meer informatie over deze scenario's, [gebruikerstoegang beheren](manage-user-access-with-access-reviews.md) en [beheren van toegang voor gasten](manage-guest-access-with-access-reviews.md).

In dit artikel wordt beschreven hoe u een of meer toegangsbeoordelingen voor groepsleden of toegang tot toepassingen maken.

## <a name="prerequisites"></a>Vereisten

- Azure AD Premium P2
- Globale beheerder of Gebruikerbeheerder

Zie voor meer informatie, [welke gebruikers moeten beschikken over licenties?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="create-one-or-more-access-reviews"></a>Maken van een of meer toegangsbeoordelingen

1. Aanmelden bij Azure portal en open de [Identiteitsbestuur pagina](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. Klik in het menu links op **Toegangsbeoordelingen**.

1. Klik op **nieuwe toegangsbeoordeling** naar een nieuwe toegangsbeoordeling maken.

    ![Overzicht van Access - besturingselementen](./media/create-access-review/access-reviews.png)

1. De naam van de toegangsbeoordeling. (Optioneel) Geef de beoordeling van een beschrijving. De naam en beschrijving worden weergegeven aan de revisoren.

    ![Een toegangsbeoordeling - naam van de beoordeling en een beschrijving maken](./media/create-access-review/name-description.png)

1. Stel de **begindatum**. Standaard een toegangsbeoordeling gebeurt eenmaal, hetzelfde moment die wordt deze gemaakt begint en eindigt in één maand. Kunt u het begin en einddatum hebben een toegang controleren start in de toekomst en laatste hoeveel dagen u dat wilt.

    ![Maken van een toegangscontrole - begin- en einddatums](./media/create-access-review/start-end-dates.png)

1. Als u de toegang tot revisie terugkerende, wijzigen de **frequentie** instellen op basis van **één keer** naar **wekelijkse**, **maandelijkse**,  **Elk kwartaal** of **jaarlijks**. Gebruik de **duur** schuifregelaar of tekst in om te bepalen hoeveel dagen elk onderzoek van de reeks terugkerende is geopend voor invoer van de revisoren. Bijvoorbeeld, is de maximale duur die u voor een maandelijkse revisie instellen kunt 27 dagen, om te voorkomen dat overlappende beoordelingen.

1. Gebruik de **End** instelling kunt u opgeven hoe de toegang tot de terugkerende beëindigen serie bekijken. De reeks kunt op drie manieren beëindigen: deze continu wordt uitgevoerd voor het starten van beoordelingen voor onbepaalde tijd, tot een bepaalde datum of na een opgegeven aantal exemplaren is voltooid. U, Gebruikerbeheerder van een andere of een ander algemeen beheerder kunt stoppen de reeks na het maken van door het veranderen van de datum in de **instellingen**, zodat deze wordt beëindigd op die datum.

1. In de **gebruikers** sectie, geeft u de gebruikers die de toegangsbeoordeling is van toepassing op. Met toegangsbeoordelingen kunnen zich voor de leden van een groep of voor gebruikers die zijn toegewezen aan een toepassing. U kunt de toegang verder bereik bekijken om te bekijken alleen de gastgebruikers die lid zijn (of toegewezen aan de toepassing), in plaats van het controleren van alle gebruikers die lid zijn of die toegang tot de toepassing hebben.

    ![Een toegangsbeoordeling - gebruikers maken](./media/create-access-review/users.png)

1. In de **groep** sectie, selecteer een of meer groepen die u wilt controleren van het lidmaatschap van.

    > [!NOTE]
    > Selecteer meer dan één groep maakt meerdere toegangsbeoordelingen. Bijvoorbeeld, u vijf afzonderlijke toegangsbeoordelingen maken vijf groepen te selecteren.
    
    ![Een toegangsbeoordeling - groep maken](./media/create-access-review/select-group.png)

1. In de **toepassingen** sectie (als u hebt geselecteerd **toegewezen aan een toepassing** in stap 8), selecteer de toepassingen die u wilt toegang te beoordelen.

    > [!NOTE]
    > Meer dan één toepassing selecteert, maakt meerdere toegangsbeoordelingen. Bijvoorbeeld, u vijf afzonderlijke toegangsbeoordelingen maken vijf toepassingen selecteren.
    
    ![Maken van een toegangscontrole - toepassing selecteren](./media/create-access-review/select-application.png)

1. In de **revisoren** sectie, selecteer een of meer mensen te bekijken van alle gebruikers in het bereik. Of u kunt selecteren om de leden hun eigen toegang beoordelen. Als de resource een groep is, kunt u vragen de eigenaren van groepen om te controleren. Ook kunt u vereisen dat de revisoren een reden opgeven wanneer ze toegang goedkeuren.

    ![Een toegangsbeoordeling - revisoren maken](./media/create-access-review/reviewers.png)

1. In de **programma's** sectie, selecteer het programma dat u wilt gebruiken. **Standaardprogramma dat** altijd aanwezig is.

    ![Een toegangsbeoordeling - programma's maken](./media/create-access-review/programs.png)

    U kunt bijhouden en verzamelen van toegangsbeoordelingen voor verschillende doeleinden door ze te ordenen in programma's vereenvoudigen. Elke toegangsbeoordeling die kan worden gekoppeld aan een programma. Klik wanneer u rapporten voor één auditor voorbereidt, u zich kunt richten op de toegangsbeoordelingen binnen het bereik van een bepaalde initiatief. Programma's en resultaten van de toegangsbeoordeling zijn zichtbaar voor gebruikers in de globale beheerder, beheerder, beveiligingsbeheerder of rol van beveiligingslezer.

    Een lijst met programma's wilt bekijken, gaat u naar de toegang beoordeelt pagina en selecteer **programma's**. Als u in een globale beheerder of de gebruikersrol beheerder bent, kunt u aanvullende programma's maken. Bijvoorbeeld, u kunt een programma voor elke initiatief naleving of zakelijke doelstelling. Als u een programma niet meer nodig hebt en dit niet alle besturingselementen die zijn gekoppeld hoeft, kunt u deze kunt verwijderen.

### <a name="upon-completion-settings"></a>Na voltooiing van de instellingen

1. Om op te geven wat gebeurt er wanneer een beoordeling is voltooid, vouw de **na voltooiing van de instellingen** sectie.

    ![Na voltooiing van de instellingen](./media/create-access-review/upon-completion-settings.png)

1. Als u automatisch de toegang verwijderen voor gebruikers die zijn geweigerd wilt, stelt u **automatisch resultaten toepassen op resource** naar **inschakelen**. Als u de resultaten handmatig toepassen wilt als de controle is voltooid, stelt u de switch op **uitschakelen**.

1. Gebruik de **moet revisor niet reageert** lijst om op te geven wat er gebeurt met gebruikers die niet zijn beoordeeld door de revisor binnen de controleperiode. Deze instelling heeft geen invloed op gebruikers die handmatig door de revisoren zijn gecontroleerd. Als de laatste revisor besluit weigeren, wordt de toegang van de gebruiker worden verwijderd.

    - **Er is geen wijziging** -gebruikerstoegang, ongewijzigd laten
    - **Toegang verwijderen** -van de gebruiker toegang verwijderen
    - **Toegang goedkeuren** -van de gebruiker toegang goedkeuren
    - **Aanbevelingen ophalen** - nemen van het systeem de aanbeveling weigeren of goedkeuring van de gebruiker de blijvende toegang

### <a name="advanced-settings"></a>Geavanceerde instellingen

1. Als u aanvullende instellingen, vouw de **geavanceerde instellingen** sectie.

    ![Geavanceerde instellingen](./media/create-access-review/advanced-settings.png)

1. Stel **aanbevelingen weergeven** naar **inschakelen** om weer te geven de revisoren van het systeem aanbevelingen op basis van de gebruiker toegang tot informatie.

1. Stel **reden vereisen bij goedkeuring** naar **inschakelen** naar de revisor moet een reden voor goedkeuring opgeven.

1. Stel **e-mailmeldingen** naar **inschakelen** dat Azure AD e-mailmeldingen naar revisoren verzenden wanneer een toegangsbeoordeling wordt gestart, en naar beheerders wanneer een beoordeling is voltooid.

1. Stel **herinneringen** naar **inschakelen** om in Azure AD herinneringen stuurt van toegangsbeoordelingen wordt uitgevoerd naar de revisors die de beoordeling nog niet hebben voltooid.

    Standaard ontvangen de beoordelaars die nog niet hebben gereageerd halverwege de einddatum automatisch een herinnering van Azure AD.

## <a name="start-the-access-review"></a>Start de toegangsbeoordeling

Nadat u de instellingen voor een overzicht van access hebt opgegeven, klikt u op **Start**. De toegangsbeoordeling wordt weergegeven in de lijst met een indicatie van de status ervan.

![Lijst met toegangsbeoordelingen](./media/create-access-review/access-reviews-list.png)

Standaard verzendt Azure AD een e-mailbericht naar de revisors kort nadat de evaluatie wordt gestart. Als u geen Azure AD-tenant het e-mailbericht verzenden, zorg er dan voor dat laten weten de revisoren een toegangsbeoordeling wordt gewacht om uit te voeren. U ze kunt weergeven, kunt u de instructies voor het [toegang met groepen of toepassingen beoordelen](perform-access-review.md). Als uw beoordeling voor gasten op hun eigen toegang beoordelen, geven ze de instructies voor het [toegang beoordelen zelf aan groepen of toepassingen](review-your-access.md).

Als u gasten als beoordelaar hebt toegewezen en ze de uitnodiging niet hebben geaccepteerd, wordt ze een e-mailbericht niet ontvangen van toegangsbeoordelingen omdat ze moeten eerst akkoord gaan met de uitnodiging voordat controleren.

## <a name="create-reviews-via-apis"></a>Beoordelingen via API's maken

U kunt ook toegangsbeoordelingen met behulp van API's maken. Wat u doet voor het beheren van toegang tot beoordelingen van groepen en gebruikers van de toepassing in Azure portal kunnen ook worden uitgevoerd met behulp van Microsoft Graph-API's. Zie voor meer informatie de [API-verwijzing voor Azure AD-toegangsbeoordelingen](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta). Zie voor een codevoorbeeld, [beoordelingen van voorbeeld van het ophalen van Azure AD-toegang via Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Volgende stappen

- [Toegang tot groepen of toepassingen controleren](perform-access-review.md)
- [Toegang beoordelen zelf aan groepen of toepassingen](review-your-access.md)
- [Een toegangscontrole van groepen of toepassingen voltooien](complete-access-review.md)

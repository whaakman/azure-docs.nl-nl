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
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e25af938d09a254abd5d28ca3a5eecca2d3f8f1
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576192"
---
# <a name="create-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Maken van een toegangscontrole van groepen of toepassingen in Azure AD-toegangsbeoordelingen

Toegang tot groepen en toepassingen voor werknemers en gasten gewijzigd na verloop van tijd. Beheerders kunnen Azure Active Directory (Azure AD) gebruiken voor het maken van toegangsbeoordelingen voor groepsleden of toegang tot toepassingen, verminderen het risico dat samenhangt met verouderde toegangstoewijzingen. Als u regelmatig toegang beoordelen wilt, kunt u ook terugkerende toegangsbeoordelingen te maken. Zie voor meer informatie over deze scenario's, [gebruikerstoegang beheren](manage-user-access-with-access-reviews.md) en [beheren van toegang voor gasten](manage-guest-access-with-access-reviews.md).

In dit artikel wordt beschreven hoe u een of meer toegangsbeoordelingen voor groepsleden of toegang tot toepassingen maken.

## <a name="prerequisites"></a>Vereisten

- [Toegangsbeoordelingen ingeschakeld](access-reviews-overview.md)
- Globale beheerder of Gebruikerbeheerder

## <a name="create-one-or-more-access-reviews"></a>Maken van een of meer toegangsbeoordelingen

1. Aanmelden bij Azure portal en open de [pagina toegangsbeoordelingen](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. Klik op **besturingselementen**.

1. Klik op **nieuwe toegangsbeoordeling** naar een nieuwe toegangsbeoordeling maken.

    ![Overzicht van Access - besturingselementen](./media/create-access-review/controls.png)

1. De naam van de toegangsbeoordeling. (Optioneel) Geef de beoordeling van een beschrijving. De naam en beschrijving worden weergegeven aan de revisoren.

    ![Een toegangsbeoordeling - naam van de beoordeling en een beschrijving maken](./media/create-access-review/name-description.png)

1. Stel de **begindatum**. Standaard een toegangsbeoordeling gebeurt eenmaal, hetzelfde moment die wordt deze gemaakt begint en eindigt in één maand. Kunt u het begin en einddatum hebben een toegang controleren start in de toekomst en laatste hoeveel dagen u dat wilt.

    ![Maken van een toegangscontrole - begin- en einddatums](./media/create-access-review/start-end-dates.png)

1. Als u de toegang tot revisie terugkerende, wijzigen de **frequentie** instellen op basis van **één keer** naar **wekelijkse**, **maandelijkse**,  **Elk kwartaal** of **jaarlijks**, en gebruik de **duur** schuifregelaar of tekst in om te bepalen hoeveel dagen elk onderzoek van de reeks terugkerende is geopend voor invoer van de revisoren. Bijvoorbeeld, is de maximale duur die u voor een maandelijkse revisie instellen kunt 27 dagen, om te voorkomen dat overlappende beoordelingen.

1. Gebruik de **End** instelling kunt u opgeven hoe de toegang tot de terugkerende beëindigen serie bekijken. De reeks kunt op drie manieren beëindigen: deze continu wordt uitgevoerd voor het starten van beoordelingen voor onbepaalde tijd, tot een bepaalde datum of na een opgegeven aantal exemplaren is voltooid. U, Gebruikerbeheerder van een andere of een ander algemeen beheerder kunt stoppen de reeks na het maken van door het veranderen van de datum in de **instellingen**, zodat deze wordt beëindigd op die datum.

1. In de **gebruikers** sectie, geeft u de gebruikers die toegang tot revisie van toepassing op. Met toegangsbeoordelingen kunnen zich voor de leden van een groep of voor gebruikers die zijn toegewezen aan een toepassing. U kunt de toegang verder bereik bekijken om te bekijken alleen de gastgebruikers die lid zijn (of toegewezen aan de toepassing), in plaats van het controleren van alle gebruikers die lid zijn of die toegang tot de toepassing hebben.

    ![Een toegangsbeoordeling - gebruikers maken](./media/create-access-review/users.png)

1. In de **groepen** sectie, selecteer een of meer groepen die u wilt controleren van het lidmaatschap van.

    > [!NOTE]
    > Selecteer meer dan één groep maakt meerdere toegangsbeoordelingen. Bijvoorbeeld, u vijf afzonderlijke toegangsbeoordelingen maken vijf groepen te selecteren.
    
    ![Een toegangsbeoordeling - groep maken](./media/create-access-review/select-group.png)

1. In de **toepassingen** sectie (als u hebt geselecteerd **toegewezen aan een toepassing** in stap 8), selecteer de toepassingen die u wilt toegang te beoordelen.

    > [!NOTE]
    > Meer dan één toepassing selecteert, maakt meerdere toegangsbeoordelingen. Bijvoorbeeld, u vijf afzonderlijke toegangsbeoordelingen maken vijf toepassingen selecteren.
    
    ![Maken van een toegangscontrole - toepassing selecteren](./media/create-access-review/select-application.png)

1. In de **revisoren** sectie, selecteer een of meer mensen te bekijken van alle gebruikers in het bereik. Of u kunt selecteren om de leden hun eigen toegang beoordelen. Als de resource een groep is, kunt u vragen de eigenaren van groepen om te controleren. Ook kunt u vereisen dat de revisoren een reden opgeven wanneer ze toegang goedkeuren.

    ![Een toegangsbeoordeling - revisoren maken](./media/create-access-review/reviewers.png)

1. In de **programma's** sectie, selecteer het programma dat u wilt gebruiken. U kunt bijhouden en verzamelen van toegangsbeoordelingen voor verschillende doeleinden door ze te ordenen in programma's vereenvoudigen. **Standaardprogramma dat** is altijd aanwezig is, of u kunt een ander programma maken. Bijvoorbeeld, u kunt een programma voor elke initiatief naleving of zakelijke doelstelling.

    ![Een toegangsbeoordeling - programma's maken](./media/create-access-review/programs.png)

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

## <a name="start-the-access-review"></a>Start de toegangsbeoordeling

Nadat u de instellingen voor een overzicht van access hebt opgegeven, klikt u op **Start**.

Standaard verzendt Azure AD een e-mailbericht naar de revisors kort nadat de evaluatie wordt gestart. Als u geen Azure AD-tenant het e-mailbericht verzenden, zorg er dan voor dat laten weten de revisoren een toegangsbeoordeling wordt gewacht om uit te voeren. U ze kunt weergeven, kunt u de instructies voor het [toegang met groepen of toepassingen beoordelen](perform-access-review.md). Als uw beoordeling voor gasten op hun eigen toegang beoordelen, geven ze de instructies voor het [toegang beoordelen zelf aan groepen of toepassingen](review-your-access.md).

Als sommige van de revisoren gasten, gasten krijgt een melding via e-mail alleen als ze al hun uitnodiging hebt geaccepteerd.

## <a name="manage-the-access-review"></a>Beheren van de toegangsbeoordeling

U kunt de voortgang volgen als de revisoren voltooid hun beoordelingen in het dashboard Azure AD in de **Toegangsbeoordelingen** sectie. Er is geen toegangsrechten worden gewijzigd in de map tot [de beoordeling is voltooid](complete-access-review.md).

Als dit een eenmalige beoordeling, klikt u vervolgens of de beheerder van de toegangsbeoordeling, stopt de periode van de toegangsbeoordeling is via de stappen in [een toegangscontrole van groepen of toepassingen voltooien](complete-access-review.md) om te zien en de resultaten van toepassing.  

Voor het beheren van een reeks toegangsbeoordelingen, gaat u naar de toegangsbeoordeling van **besturingselementen**, en u toekomstige voorvallen niet vinden in de geplande beoordelingen, en de einddatum bewerken of toevoegen/verwijderen revisoren dienovereenkomstig. 

Op basis van uw selecties in na voltooiing van de instellingen, auto-toepassing zal worden uitgevoerd na de einddatum van de beoordeling of wanneer u de beoordeling handmatig stoppen. De status van de beoordeling wordt gewijzigd van voltooide via tussenliggende Staten zoals toepassen en tot slot naar status toegepast. U kunt verwachten om te zien van geweigerde gebruikers, indien van toepassing, wordt verwijderd uit de groepstoewijzing voor lidmaatschap of uw toepassing in een paar minuten.

## <a name="create-reviews-via-apis"></a>Beoordelingen via API's maken

U kunt ook toegangsbeoordelingen met behulp van API's maken. Wat u doet voor het beheren van toegang tot beoordelingen van groepen en gebruikers van de toepassing in Azure portal kunnen ook worden uitgevoerd met behulp van Microsoft Graph-API's. Zie voor meer informatie de [API-verwijzing voor Azure AD-toegangsbeoordelingen](https://docs.microsoft.com/graph/api/resources/accessreviews-root?view=graph-rest-beta). Zie voor een codevoorbeeld, [beoordelingen van voorbeeld van het ophalen van Azure AD-toegang via Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Volgende stappen

- [Toegang tot groepen of toepassingen controleren](perform-access-review.md)
- [Toegang beoordelen zelf aan groepen of toepassingen](review-your-access.md)
- [Een toegangscontrole van groepen of toepassingen voltooien](complete-access-review.md)

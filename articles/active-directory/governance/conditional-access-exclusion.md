---
title: Azure AD-Toegangsbeoordelingen gebruiken voor het beheren van gebruikers die zijn uitgesloten van beleid voor voorwaardelijke toegang | Microsoft Docs
description: Informatie over het gebruik van Toegangsbeoordelingen voor Azure Active Directory (Azure AD) voor het beheren van gebruikers die zijn uitgesloten van beleid voor voorwaardelijke toegang
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 09/25/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: a197a6c27b337d7aa97667dc07b1059e82050549
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57892712"
---
# <a name="use-azure-ad-access-reviews-to-manage-users-excluded-from-conditional-access-policies"></a>Gebruik Azure AD-Toegangsbeoordelingen voor het beheren van gebruikers die zijn uitgesloten van beleid voor voorwaardelijke toegang

In een ideaal volgt alle gebruikers de toegang tot de beleidsregels voor beveiligde toegang tot resources van uw organisatie. Soms zijn er echter bedrijfsscenario's waarvoor u uitzonderingen maken. In dit artikel staan enkele voorbeelden waar uitsluitingen mogelijk vereist zijn en hoe u als IT-beheerder kunt met deze taak beheren, toezicht van beleidsuitzonderingen voorkomen en auditors voorzien van bewijs dat deze uitzonderingen worden gecontroleerd regelmatig met behulp van Azure Active Directory (Azure AD)-Toegangsbeoordelingen.

> [!NOTE]
> Een geldige Azure AD Premium P2, Enterprise Mobility + Security E5 betaalde versie of proefversie licentie is vereist voor het gebruik van Azure AD-toegangsbeoordelingen. Zie [Azure Active Directory-edities](../fundamentals/active-directory-whatis.md) voor meer informatie.

## <a name="why-would-you-exclude-users-from-policies"></a>Waarom zou u gebruikers uitsluiten van beleidsregels?

Als IT-beheerder, kunt u [voorwaardelijke toegang voor Azure AD](../conditional-access/overview.md) om te vereisen dat gebruikers om te verifiëren met multi-factor authentication (MFA) of aanmelden vanaf een vertrouwd netwerk of het apparaat. Tijdens de implementatie plannen weten u dat sommige van deze vereisten niet wordt voldaan aan door alle gebruikers. Bijvoorbeeld, er zijn gebruikers die werken vanaf een externe locatie die geen deel uitmaakt van uw interne netwerk of er is een manager die gebruikmaakt van een oude telefoon die niet wordt ondersteund. Het bedrijf vereist dat deze gebruikers zich aanmelden en hun werkzaamheden mag daarom worden uitgesloten van het beleid voor voorwaardelijke toegang.

Als een ander voorbeeld: u kunt [benoemde locaties](../conditional-access/location-condition.md) in voorwaardelijke toegang tot een set van regio's en regio's waaruit u niet wilt toestaan dat gebruikers toegang krijgen tot hun tenant configureren.

![Benoemde locaties](./media/conditional-access-exclusion/named-locations.png)

Gebruikers kunnen echter in sommige gevallen hebben een legitieme reden aan te melden bij uit deze geblokkeerde landen. Gebruikers kunnen bijvoorbeeld op reis voor werk- of persoonlijke redenen. In dit voorbeeld hebt het beleid voor voorwaardelijke toegang moet worden geblokkeerd dat deze landen kan een toegewijde cloud-beveiligingsgroep voor gebruikers die zijn uitgesloten van het beleid. Gebruikers die toegang nodig hebben wanneer u onderweg bent, kunnen worden toegevoegd aan de groep met [groepsbeheer voor Azure AD-Self-servicegebruikers](../users-groups-roles/groups-self-service-management.md).

Een ander voorbeeld is mogelijk dat u een beleid voor voorwaardelijke toegang hebt die [blokken verouderde verificatie voor de meeste gebruikers](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/07/azure-ad-conditional-access-support-for-blocking-legacy-auth-is-in-public-preview/). Microsoft raadt u aan het gebruik van verouderde protocollen te blokkeren in uw tenant om uw beveiliging te verbeteren. Echter als sommige gebruikers die absoluut verouderde verificatiemethoden gebruiken moeten voor toegang tot uw resources via Office 2010 of SMTP-IMAP-POP op basis van clients, klikt u vervolgens u kunt deze gebruikers uitsluiten van het beleid waarmee verouderde verificatiemethoden worden geblokkeerd.

## <a name="why-are-exclusions-challenging"></a>Waarom zijn uitsluitingen uitdagende?

In Azure AD, kunt u een beleid voor voorwaardelijke toegang om een groep gebruikers te beperken. U kunt ook enkele van deze gebruikers uitsluiten door directory-rollen, afzonderlijke gebruikers of gasten van gebruikers te selecteren. Het is belangrijk te onthouden dat wanneer deze uitzonderingen worden geconfigureerd, kunt u lezen wat het beleid kan niet worden afgedwongen voor gebruikers. Als deze uitsluitingen zijn geconfigureerd als een overzicht van afzonderlijke gebruikers of via een verouderde on-premises-beveiligingsgroep, wordt Hiermee beperkt u de zichtbaarheid van deze lijst met uitsluitingen (gebruikers weet mogelijk niet van het bestaan van) en de IT-beheerder controle over het (gebruikers kunnen deelnemen aan de beveiligingsgroep kunnen overslaan het beleid). Bovendien kunnen gebruikers die in aanmerking voor het Uitsluitingsfilter in één keer niet meer nodig hebt of in aanmerking komen voor het.

Aan het begin van een uitsluiting is er een korte lijst met gebruikers die het beleid negeren. Na verloop van tijd steeds meer gebruikers zijn uitgesloten en de lijst groeit. Op een bepaald moment is er een nodig om te controleren van de lijst en Bevestig dat elk van deze gebruikers moet nog steeds worden uitgesloten. De lijst beheren vanuit technisch oogpunt betrekkelijk eenvoudig kunnen worden, maar uit wie de zakelijke beslissingen te nemen en hoe zorgt u ervoor dat deze alle controleerbare?

Echter, als u de uitsluiting aan het beleid voor voorwaardelijke toegang met behulp van een Azure AD-groep configureert, klikt u vervolgens kunt u toegangsbeoordelingen als een compenserende besturingselement, station zichtbaarheid en verminder het aantal gebruikers die beschikken over een uitzondering.

## <a name="how-to-create-an-exclusion-group-in-a-conditional-access-policy"></a>Over het maken van een uitsluitingsgroep in een beleid voor voorwaardelijke toegang

Volg deze stappen voor het maken van een nieuwe Azure AD-groep en beleid voor voorwaardelijke toegang dat niet voor die groep geldt.

### <a name="create-an-exclusion-group"></a>Een uitgesloten groep maken

1. Meld u aan bij Azure Portal.

1. Klik in het linkernavigatievenster op **Azure Active Directory** en klik vervolgens op **groepen**.

1. Klik op het bovenste menu **nieuwe groep** om het deelvenster van de groep te openen.

1. In de **groepstype** in de lijst met **Security**. Geef een naam en beschrijving op.

1. Zorg ervoor dat u stelt de **lidmaatschap** type **toegewezen**.

1. Selecteer de gebruikers die moeten deel uitmaken van deze uitsluitingsgroep en klik vervolgens op **maken**.

    ![Nieuwe groep deelvenster](./media/conditional-access-exclusion/new-group.png)

### <a name="create-a-conditional-access-policy-that-excludes-the-group"></a>Beleid voor voorwaardelijke toegang die niet van toepassing op de groep maken

U kunt nu een beleid voor voorwaardelijke toegang die gebruikmaakt van deze uitsluitingsgroep maken.

1. Klik in het linkernavigatievenster op **Azure Active Directory** en klik vervolgens op **voorwaardelijke toegang** openen de **beleid** blade.

1. Klik op **nieuw beleid** openen de **nieuw** deelvenster.

1. Geef een naam.

1. Klik onder toewijzingen op **gebruikers en groepen**.

1. Op de **opnemen** tabblad **alle gebruikers**.

1. Op de **uitsluiten** tabblad, voegt u een vinkje te **gebruikers en groepen** en klik vervolgens op **uitgesloten gebruikers selecteren**.

1. Selecteer de uitsluitingsgroep die u hebt gemaakt.

    > [!NOTE]
    > Als een best practice is het aanbevolen ten minste één beheerdersaccount uitsluiten van het beleid bij het testen om ervoor te zorgen dat u geen toegang tot uw tenant zijn geblokkeerd.

1. Ga door met het instellen van het beleid voor voorwaardelijke toegang op basis van de behoeften van uw organisatie.

    ![Uitgesloten gebruikers selecteren](./media/conditional-access-exclusion/select-excluded-users.png)

We hebben betrekking op twee voorbeelden waar u toegangsbeoordelingen gebruiken kunt voor het beheren van uitsluitingen in beleid voor voorwaardelijke toegang.

## <a name="example-1-access-review-for-users-accessing-from-blocked-countries"></a>Voorbeeld 1: Toegangsbeoordeling voor gebruikers die uit geblokkeerde landen

Stel dat u hebt een beleid voor voorwaardelijke toegang die blokkeert de toegang van bepaalde landen. Het bevat een groep die is uitgesloten van het beleid. Hier volgt een aanbevolen toegangsbeoordeling waar leden van de groep worden gecontroleerd.

> [!NOTE]
> Een globale beheerder of de gebruiker administrator-rol is vereist voor het maken van toegangsbeoordelingen.

1. De evaluatie wordt elke week opnieuw.

2. Verloopt nooit om ervoor te zorgen dat u houden deze uitsluitingsgroep de meest recente.

3. Alle leden van deze groep is binnen het bereik van de beoordeling.

4. Elke gebruiker heeft dat ze nog steeds toegang hebben uit deze geblokkeerde landen nodig, dus ze nog steeds moeten lid zijn van de groep zelf te bevestigen.

5. Als de gebruiker niet op de aanvraag beoordelen reageert, ze automatisch wordt verwijderd uit de groep, en daarom kunnen geen toegang meer tot de tenant wanneer u onderweg bent aan deze landen.

6. E-mailmeldingen inschakelen, zodat gebruikers worden gewaarschuwd over het begin en einde van de toegangsbeoordeling.

    ![Een toegangsbeoordeling maken](./media/conditional-access-exclusion/create-access-review-1.png)

## <a name="example-2-access-review-for-users-accessing-with-legacy-authentication"></a>Voorbeeld 2: Toegangsbeoordeling voor gebruikers met toegang tot met verouderde verificatie

Stel dat u hebt een beleid voor voorwaardelijke toegang die blokkeert de toegang voor gebruikers met behulp van verouderde en oudere clientversies. Het bevat een groep die is uitgesloten van het beleid. Hier volgt een aanbevolen toegangsbeoordeling waar leden van de groep worden gecontroleerd.

1. Deze beoordeling zou moeten zijn van een terugkerende beoordeling.

2. Iedereen in de groep moet worden gecontroleerd.

3. Deze kan worden geconfigureerd als de eigenaren van zakelijke eenheid als de geselecteerde revisoren.

4. De resultaten automatisch toepassen en verwijderen van gebruikers die niet zijn goedgekeurd om door te gaan met verouderde verificatiemethoden.

5. Het kan zinvol zijn om in te schakelen van aanbevelingen, zodat revisoren van grote groepen eenvoudig hun beslissingen kunnen nemen.

6. E-mailmeldingen inschakelen, zodat gebruikers worden gewaarschuwd over het begin en einde van de toegangsbeoordeling.

    ![Een toegangsbeoordeling maken](./media/conditional-access-exclusion/create-access-review-2.png)

**Pro-tip**: Als u veel uitsluitingsgroepen hebben en moet daarom meerdere toegangsbeoordelingen maken, hebben we nu een API in de Microsoft Graph bèta-eindpunt waarmee u maken en ze op programmatische wijze beheren. Als u wilt beginnen, Zie de [API-verwijzing voor Azure AD-toegangsbeoordelingen](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/accessreviews_root) en [beoordelingen van voorbeeld van het ophalen van Azure AD-toegang via Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/td-p/236096).

## <a name="access-review-results-and-audit-logs"></a>Resultaten van de toegangsbeoordeling en auditlogboeken

Nu u alles hebt in plaats, groep, beleid voor voorwaardelijke toegang en toegangsbeoordelingen, is het tijd om te controleren en bijhouden van de resultaten van deze beoordelingen.

1. Open in de Azure-portal, de **Toegangsbeoordelingen** blade.

1. Open de controle- en programma dat u hebt gemaakt voor het beheren van de uitsluitingsgroep.

1. Klik op **resultaten** om te zien die is goedgekeurd om te blijven in de lijst en die is verwijderd.

    ![Resultaten voor toegangsbeoordelingen](./media/conditional-access-exclusion/access-reviews-results.png)

1. Klik vervolgens op **auditlogboeken** om te zien van de acties die zijn uitgevoerd tijdens deze beoordeling.

    ![Auditlogboeken voor toegangsbeoordelingen](./media/conditional-access-exclusion/access-reviews-audit-logs.png)

Als IT-beheerder weet u dat beheren uitsluitingsgroepen aan uw beleid soms onvermijdelijk is. Echter controleert deze groepen beheren, controleren op regelmatige basis door eigenaar van het bedrijf of de gebruikers zelf, controle en die deze wijzigingen kunnen eenvoudiger gemaakt met de toegang van Azure AD.

## <a name="next-steps"></a>Volgende stappen

- [Een toegangsbeoordeling van groepen of toepassingen maken](create-access-review.md)
- [Wat is voorwaardelijke toegang in Azure Active Directory?](../conditional-access/overview.md)

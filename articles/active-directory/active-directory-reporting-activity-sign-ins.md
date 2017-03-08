---
title: Aanmeldactiviteitenrapporten in Azure Active Directory Portal - preview | Microsoft Docs
description: Ontdek de aanmeldactiviteitenrapporten in de previewversie van Azure Active Directory Portal
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 78617dc7e3a4b6eb4fc32d32b6850b8c0832d6d8
ms.openlocfilehash: 9819c5f6a3aea53664d86e3a23b25946c0f2b731
ms.lasthandoff: 02/22/2017


---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal---preview"></a>Aanmeldactiviteitenrapporten in Azure Active Directory Portal - preview

Wanneer u rapporten bekijkt in de [previewversie](active-directory-preview-explainer.md) van Azure Active Directory, ontvangt u alle informatie die nodig is om te bepalen hoe het gaat met uw omgeving.

De rapportstructuur in Azure Active Directory bestaat uit de volgende onderdelen:

- **Activiteit** 
    - **Aanmeldactiviteiten**: informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers
    - **Controlelogboeken**: informatie over systeemactiviteit van gebruikers, groepsbeheer, uw beheerde toepassingen en directory-activiteiten.
- **Beveiliging** 
    - **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie Riskante aanmeldingen voor meer informatie.
    - **Gebruikers van wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie Gebruikers van wie wordt aangegeven dat ze risico lopen voor meer informatie.

In dit onderwerp vindt u meer informatie over de aanmeldactiviteiten.

## <a name="signs-in-activities"></a>Aanmeldactiviteiten

In de informatie die wordt aangeboden in het rapport over aanmeldactiviteiten van gebruikers, vindt u antwoord op vragen zoals:

* Wat is het aanmeldingspatroon van een gebruiker?
* Hoeveel keer hebben gebruikers zich aangemeld gedurende een week?
* Wat is de status van deze aanmeldingen?

Uw beginpunt voor deze gegevens is de aanmeldingsgrafiek van gebruikers in het gedeelte **Overzicht** onder **Gebruikers en groepen**.

 ![Rapportage](./media/active-directory-reporting-activity-sign-ins/05.png "Rapportage")

In de aanmeldingsgrafiek van gebruikers ziet u alle aanmeldingen van alle gebruikers gedurende een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Rapportage](./media/active-directory-reporting-activity-sign-ins/02.png "Rapportage")

Als u op een dag in de aanmeldingsgrafiek klikt, ziet u een gedetailleerd overzicht van de aanmeldactiviteiten.

![Rapportage](./media/active-directory-reporting-activity-sign-ins/03.png "Rapportage")

Elke rij in de lijst met aanmeldactiviteiten bevat gedetailleerde informatie over de geselecteerde aanmelding, zoals:

* Wie heeft zich aangemeld?
* Wat was de gerelateerde UPN?
* Welke toepassing was het aanmeldingsdoel?
* Wat is het IP-adres van de persoon die zich heeft aangemeld?
* Wat is de status van de aanmelding?

## <a name="usage-of-managed-applications"></a>Het gebruik van beheerde toepassingen

Met een toepassingsgerichte weergave van uw aanmeldgegevens kunt u antwoord vinden op vragen zoals:

* Wie gebruikt mijn toepassingen?
* Wat zijn de drie meest gebruikte toepassingen in uw organisatie?
* Ik heb onlangs een toepassing geïmplementeerd. Hoe gaat het ermee?

Uw beginpunt voor deze gegevens is het overzicht van de drie populairste toepassingen in uw organisatie volgens het rapport van de laatste 30 dagen. Het overzicht vindt u in het gedeelte **Overzicht** onder **Bedrijfstoepassingen**.

 ![Rapportage](./media/active-directory-reporting-activity-sign-ins/06.png "Rapportage")

In de grafiek over appgebruik staat een wekelijks overzicht van alle aanmeldingen bij de drie populairste toepassingen gedurende een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Rapportage](./media/active-directory-reporting-activity-sign-ins/78.png "Rapportage")

Als u wilt, kunt u de focus instellen op een specifieke toepassing.

![Rapportage](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Rapportage")

Als u op een dag in de appgebruikgrafiek klikt, ziet u een gedetailleerd overzicht van de aanmeldactiviteiten.

![Rapportage](./media/active-directory-reporting-activity-sign-ins/top_app_sign_ins.png "Rapportage")

Met de optie **Aanmeldingen** krijgt u een volledig overzicht van alle aanmeldingsgebeurtenissen voor uw toepassingen.

![Rapportage](./media/active-directory-reporting-activity-sign-ins/85.png "Rapportage")

Met de kolomkiezer kunt u kiezen welke gegevensvelden u wilt weergeven.

![Rapportage](./media/active-directory-reporting-activity-sign-ins/column_chooser.png "Rapportage")

## <a name="filtering-sign-ins"></a>Aanmeldingen filteren
U kunt de aanmeldingen filteren om de hoeveelheid weergegeven gegevens te beperken. Gebruik daarvoor de volgende informatie:

* Datum en tijd 
* UPN van gebruiker
* De naam van de toepassing
* De clientnaam
* Aanmeldingsstatus

![Rapportage](./media/active-directory-reporting-activity-sign-ins/293.png "Rapportage")

Een andere manier waarop u het aantal aanmeldactiviteiten kunt filteren, is door te zoeken naar specifieke vermeldingen.
Met deze zoekmethode kunt u zoeken naar aanmeldingen van specifieke **gebruikers** of **groepen** en bij specifieke **toepassingen**.

![Rapportage](./media/active-directory-reporting-activity-sign-ins/84.png "Rapportage")


## <a name="next-steps"></a>Volgende stappen
Zie de [Azure Active Directory-rapportagegids](active-directory-reporting-guide.md).



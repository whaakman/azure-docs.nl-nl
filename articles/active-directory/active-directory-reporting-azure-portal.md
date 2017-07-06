---

title: Azure Active Directory-rapportage | Microsoft Docs
description: Bevat een lijst met de diverse beschikbare rapporten voor Azure Active Directory
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: c7fe995f097c72ab5275249538fe2bb65efac256
ms.contentlocale: nl-nl
ms.lasthandoff: 05/08/2017


---
# <a name="azure-active-directory-reporting"></a>Azure Active Directory-rapportage


*Deze documentatie maakt deel uit van de [Azure Active Directory-rapportagegids](active-directory-reporting-guide.md).*

Met rapporten in Azure Active Directory ontvangt u alle informatie die nodig is om te bepalen hoe het gaat met uw omgeving.

Er zijn twee hoofdonderwerpen waarover wordt gerapporteerd:

* **Aanmeldactiviteiten**: informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers
* **Controlelogboeken**: informatie over systeemactiviteit van gebruikers, groepsbeheer, uw beheerde toepassingen en directory-activiteiten

Afhankelijk van het bereik van de gegevens die u zoekt, kunt u de rapporten openen door op **Gebruikers en groepen** of **Bedrijfstoepassingen** te klikken in de lijst met services in [Azure Portal](https://portal.azure.com).

## <a name="sign-in-activities"></a>Aanmeldactiviteiten
### <a name="user-sign-in-activities"></a>Aanmeldactiviteiten van gebruikers
In de informatie die wordt aangeboden in het rapport over aanmeldactiviteiten van gebruikers, vindt u antwoord op vragen zoals:

* Wat is het aanmeldingspatroon van een gebruiker?
* Hoeveel keer hebben gebruikers zich aangemeld gedurende een week?
* Wat is de status van deze aanmeldingen?

Uw beginpunt voor deze gegevens is de aanmeldingsgrafiek van gebruikers in het gedeelte **Overzicht** onder **Gebruikers en groepen**.

 ![Rapportage](./media/active-directory-reporting-azure-portal/05.png "Rapportage")

In de aanmeldingsgrafiek van gebruikers ziet u alle aanmeldingen van alle gebruikers gedurende een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Rapportage](./media/active-directory-reporting-azure-portal/02.png "Rapportage")

Als u op een dag in de aanmeldingsgrafiek klikt, ziet u een gedetailleerd overzicht van de aanmeldactiviteiten.

![Rapportage](./media/active-directory-reporting-azure-portal/03.png "Rapportage")

Elke rij in de lijst met aanmeldactiviteiten bevat gedetailleerde informatie over de geselecteerde aanmelding, zoals:

* Wie heeft zich aangemeld?
* Wat was de gerelateerde UPN?
* Welke toepassing was het aanmeldingsdoel?
* Wat is het IP-adres van de persoon die zich heeft aangemeld?
* Wat is de status van de aanmelding?

### <a name="usage-of-managed-applications"></a>Het gebruik van beheerde toepassingen
Met een toepassingsgerichte weergave van uw aanmeldingsgegevens kunt u antwoord vinden op vragen zoals:

* Wie gebruikt mijn toepassingen?
* Wat zijn de drie meest gebruikte toepassingen in uw organisatie?
* Ik heb onlangs een toepassing geïmplementeerd. Hoe gaat het ermee?

Uw beginpunt voor deze gegevens is het overzicht van de drie populairste toepassingen in uw organisatie volgens het rapport van de laatste 30 dagen. Het overzicht vindt u in het gedeelte **Overzicht** onder **Bedrijfstoepassingen**.

 ![Rapportage](./media/active-directory-reporting-azure-portal/06.png "Rapportage")

In de grafiek over appgebruik staat een wekelijks overzicht van alle aanmeldingen bij de drie populairste toepassingen gedurende een bepaalde periode. De standaard ingestelde periode is 30 dagen.

![Rapportage](./media/active-directory-reporting-azure-portal/78.png "Rapportage")

Als u wilt, kunt u de focus instellen op een specifieke toepassing.

![Rapportage](./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "Rapportage")

Als u op een dag in de appgebruikgrafiek klikt, ziet u een gedetailleerd overzicht van de aanmeldactiviteiten.

![Rapportage](./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "Rapportage")

Met de optie **Aanmeldingen** krijgt u een volledig overzicht van alle aanmeldingsgebeurtenissen voor uw toepassingen.

![Rapportage](./media/active-directory-reporting-azure-portal/85.png "Rapportage")

Met de kolomkiezer kunt u kiezen welke gegevensvelden u wilt weergeven.

![Rapportage](./media/active-directory-reporting-azure-portal/column_chooser.png "Rapportage")

### <a name="filtering-sign-ins"></a>Aanmeldingen filteren
U kunt de aanmeldingen filteren om de hoeveelheid weergegeven gegevens te beperken. Gebruik daarvoor de volgende informatie:

* Datum en tijd 
* UPN van gebruiker
* De naam van de toepassing
* De clientnaam
* Aanmeldingsstatus

![Rapportage](./media/active-directory-reporting-azure-portal/293.png "Rapportage")

Een andere manier waarop u het aantal aanmeldactiviteiten kunt filteren, is door te zoeken naar specifieke vermeldingen.
Met deze zoekmethode kunt u zoeken naar aanmeldingen van specifieke **gebruikers** of **groepen** en bij specifieke **toepassingen**.

![Rapportage](./media/active-directory-reporting-azure-portal/84.png "Rapportage")

## <a name="audit-logs"></a>Controlelogboeken
In de controlelogboeken in Azure Active Directory staan records van systeemactiviteiten voor naleving.

Er zijn drie hoofdcategorieën voor controlegerelateerde activiteiten in Azure Portal:

* Gebruikers en groepen   
* Toepassingen
* Directory   

Zie de [lijst met rapporten over controlegebeurtenissen](active-directory-reporting-audit-events.md#list-of-audit-report-events) voor een volledig overzicht van alle rapporten over controleactiviteiten.

Uw beginpunt voor alle controlegegevens is **Controlelogboeken** in het gedeelte **Activiteit** van **Azure Active Directory**.

![Controle](./media/active-directory-reporting-azure-portal/61.png "Controle")

Een controlelogboek heeft een lijstweergave met daarin de actoren (wie), de activiteiten (wat) en de doelen.

![Controle](./media/active-directory-reporting-azure-portal/345.png "Controle")

Wanneer u op een item in de lijstweergave klikt, krijgt u er meer informatie over te zien.

![Controle](./media/active-directory-reporting-azure-portal/873.png "Controle")

### <a name="users-and-groups-audit-logs"></a>Controlelogboeken voor gebruikers en groepen
Met de controlerapporten op basis van gebruikers en groepen krijgt u antwoord op vragen zoals:

* Welke soorten updates zijn toegepast op de gebruikers?
* Hoeveel gebruikers zijn gewijzigd?
* Hoeveel wachtwoorden zijn gewijzigd?
* Wat heeft een beheerder in een directory gedaan?
* Welke groepen zijn toegevoegd?
* Zijn er groepen met wijzigingen in het lidmaatschap?
* Zijn de eigenaren van een groep gewijzigd?
* Welke licenties zijn toegewezen aan een groep of een gebruiker?

Als u alleen de controlegegevens wilt bekijken die zijn gerelateerd aan gebruikers en groepen, kunt u een gefilterde weergave openen via **Controlelogboeken** in het gedeelte **Activiteit** van **Gebruikers en groepen**.

![Controle](./media/active-directory-reporting-azure-portal/93.png "Controle")

### <a name="application-audit-logs"></a>Controlelogboeken van toepassingen
Met de controlerapporten op basis van toepassingen krijgt u antwoord op vragen zoals:

* Welke toepassingen zijn toegevoegd of bijgewerkt?
* Welke toepassingen zijn verwijderd?
* Is de service-principal van een toepassing gewijzigd?
* Zijn de namen van toepassingen gewijzigd?
* Wie heeft toestemming gegeven voor een toepassing?

Als u alleen de controlegegevens wilt bekijken die zijn gerelateerd aan toepassingen, kunt u een gefilterde weergave openen via **Controlelogboeken** in het gedeelte **Activiteit** van **Bedrijfstoepassingen**.

![Controle](./media/active-directory-reporting-azure-portal/134.png "Controle")

### <a name="filtering-audit-logs"></a>Controlelogboeken filteren
U kunt de aanmeldingen filteren om de hoeveelheid weergegeven gegevens te beperken. Gebruik daarvoor de volgende informatie:

* Datum en tijd
* UPN van de actor
* Type activiteit
* Activiteit

![Controle](./media/active-directory-reporting-azure-portal/356.png "Controle")

De inhoud van de lijst **Activiteitstype** is gekoppeld aan uw ingangspunt voor deze blade.  
Als uw ingangspunt Azure Active Directory is, bevat deze lijst alle mogelijke activiteitstypen:

* Toepassing 
* Groep 
* Gebruiker
* Apparaat
* Directory
* Beleid
* Overige

![Controle](./media/active-directory-reporting-azure-portal/825.png "Controle")

De vermelde activiteiten worden ingedeeld per activiteitstype.
Als u bijvoorbeeld **Groep** hebt geselecteerd als **activiteitstype**, bevat de lijst **Activiteit** alleen groepsgerelateerde activiteiten.   

![Controle](./media/active-directory-reporting-azure-portal/654.png "Controle")

Een andere manier waarop u de vermeldingen in een controlelogboek kunt filteren, is door te zoeken naar specifieke vermeldingen.

![Controle](./media/active-directory-reporting-azure-portal/237.png "Controle")

## <a name="next-steps"></a>Volgende stappen
Zie de [Azure Active Directory-rapportagegids](active-directory-reporting-guide.md).



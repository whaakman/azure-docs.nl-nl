---
title: Controleactiviteitenrapporten in Azure Active Directory Portal - preview | Microsoft Docs
description: Ontdek de controleactiviteitenrapporten in de previewversie van Azure Active Directory Portal
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/21/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 74460492c5eb6edfcc67015f8f6894267cb9edc8
ms.openlocfilehash: 5a219219cd5e34713cd6a1c54a98d6bd15310e05
ms.lasthandoff: 02/22/2017


---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal---preview"></a>Controleactiviteitenrapporten in Azure Active Directory Portal - preview

Wanneer u rapporten bekijkt in de [previewversie](active-directory-preview-explainer.md) van Azure Active Directory, ontvangt u alle informatie die nodig is om te bepalen hoe het gaat met uw omgeving.

De rapportstructuur in Azure Active Directory bestaat uit de volgende onderdelen:

- **Activiteit** 
    - **Aanmeldactiviteiten**: informatie over het gebruik van beheerde toepassingen en aanmeldactiviteiten van gebruikers
    - **Controlelogboeken**: informatie over systeemactiviteit van gebruikers, groepsbeheer, uw beheerde toepassingen en directory-activiteiten.
- **Beveiliging** 
    - **Riskante aanmeldingen** - Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk is uitgevoerd door iemand die geen rechtmatige eigenaar van een gebruikersaccount is. Zie Riskante aanmeldingen voor meer informatie.
    - **Gebruikers van wie wordt aangegeven dat ze risico lopen** - Een riskante gebruiker is een indicator van een gebruikersaccount dat mogelijk is aangetast. Zie Gebruikers van wie wordt aangegeven dat ze risico lopen voor meer informatie.

In dit onderwerp vindt u meer informatie over de controleactiviteiten.
 
## <a name="audit-logs"></a>Controlelogboeken

In de controlelogboeken in Azure Active Directory staan records van systeemactiviteiten voor naleving.

Er zijn drie hoofdcategorieën voor controlegerelateerde activiteiten in Azure Portal:

- Gebruikers en groepen   

- Toepassingen

- Directory   

Zie de [lijst met rapporten over controlegebeurtenissen](active-directory-reporting-audit-events.md#list-of-audit-report-events) voor een volledig overzicht van alle rapporten over controleactiviteiten.


Uw beginpunt voor alle controlegegevens is **Controlelogboeken** in het gedeelte **Activiteit** van **Azure Active Directory**.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/61.png "Controlelogboeken")

Een controlelogboek heeft een lijstweergave met daarin de actoren (*wie*), de activiteiten (*wat*) en de doelen.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/345.png "Controlelogboeken")

Wanneer u op een item in de lijstweergave klikt, krijgt u er meer informatie over te zien.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/873.png "Controlelogboeken")


## <a name="users-and-groups-audit-logs"></a>Controlelogboeken voor gebruikers en groepen

Met de controlerapporten op basis van gebruikers en groepen krijgt u antwoord op vragen zoals:

- Welke soorten updates zijn toegepast op de gebruikers?

- Hoeveel gebruikers zijn gewijzigd?

- Hoeveel wachtwoorden zijn gewijzigd?

- Wat heeft een beheerder in een directory gedaan?

- Welke groepen zijn toegevoegd?

- Zijn er groepen met wijzigingen in het lidmaatschap?

- Zijn de eigenaren van een groep gewijzigd?

- Welke licenties zijn toegewezen aan een groep of een gebruiker?

Als u alleen de controlegegevens wilt bekijken die zijn gerelateerd aan gebruikers en groepen, kunt u een gefilterde weergave openen via **Controlelogboeken** in het gedeelte **Activiteit** van **Gebruikers en groepen**.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/93.png "Controlelogboeken")

## <a name="application-audit-logs"></a>Controlelogboeken van toepassingen
Met de controlerapporten op basis van toepassingen krijgt u antwoord op vragen zoals:

* Welke toepassingen zijn toegevoegd of bijgewerkt?
* Welke toepassingen zijn verwijderd?
* Is de service-principal van een toepassing gewijzigd?
* Zijn de namen van toepassingen gewijzigd?
* Wie heeft toestemming gegeven voor een toepassing?

Als u alleen de controlegegevens wilt bekijken die zijn gerelateerd aan toepassingen, kunt u een gefilterde weergave openen via **Controlelogboeken** in het gedeelte **Activiteit** van **Bedrijfstoepassingen**.

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/134.png "Controlelogboeken")

## <a name="filtering-audit-logs"></a>Controlelogboeken filteren
U kunt de aanmeldingen filteren om de hoeveelheid weergegeven gegevens te beperken. Gebruik daarvoor de volgende informatie:

- Datum en tijd

- UPN van de actor

- Category

- Resourcetype van activiteit

- Activiteit

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/625.png "Controlelogboeken")


Met het filter **Categorie** kunt u het bereik van uw controlerapport verkleinen. U kunt daarbij de volgende categorieën gebruiken:

- Hoofddirectory

- Self-service voor wachtwoordbeheer

- Self-service voor groepsbeheer

- Automatische wachtwoordoverschakeling 

![Controlelogboeken](./media/active-directory-reporting-activity-audit-logs/626.png "Controlelogboeken")



De inhoud van de lijst **Resourcetype van activiteit** is gekoppeld aan uw ingangspunt voor deze blade.  
Als uw ingangspunt Azure Active Directory is, bevat deze lijst alle mogelijke activiteitstypen:

- Directory

- Gebruiker

- Groep 

- Toepassing 

- Beleid

- Apparaat


![Controle](./media/active-directory-reporting-activity-audit-logs/627.png "Controle")

De vermelde activiteiten worden ingedeeld per activiteitstype.
Als u bijvoorbeeld **Gebruiker** hebt geselecteerd als **activiteitstype**, bevat de lijst **Activiteit** alleen groepsgerelateerde activiteiten.   

![Controle](./media/active-directory-reporting-activity-audit-logs/628.png "Controle")

Als u **Groep** selecteert als **activiteitstype** krijgt u een extra filteroptie zodat u ook kunt filteren op basis van de volgende **activiteitsbronnen**:

- Azure AD

- O365


![Controle](./media/active-directory-reporting-activity-audit-logs/629.png "Controle")



Een andere manier waarop u de vermeldingen in een controlelogboek kunt filteren, is door te zoeken naar specifieke vermeldingen.

![Controle](./media/active-directory-reporting-activity-audit-logs/237.png "Controle")




## <a name="next-steps"></a>Volgende stappen
Zie de [Azure Active Directory-rapportagegids](active-directory-reporting-guide.md).



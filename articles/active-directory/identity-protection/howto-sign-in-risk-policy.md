---
title: Het beleid voor aanmeldingsrisico configureren in Azure Active Directory Identity Protection | Microsoft Docs
description: Informatie over het configureren van de Azure AD Identity Protection-beleid voor aanmeldingsrisico.
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 7350cbd3e8aed6098f24d0edaa5807d241890287
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45581473"
---
# <a name="how-to-configure-the-sign-in-risk-policy"></a>Procedure: Het beleid voor aanmeldingsrisico configureren

Azure Active Directory detecteert [risicogebeurtenistypen](../reports-monitoring/concept-risk-events.md#risk-event-types) in realtime en offline. Elke risicogebeurtenis die is gedetecteerd voor een aanmelding van een gebruiker draagt bij aan een logisch concept is met de naam riskante aanmelding. Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk niet uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount.


## <a name="sign-in-risk-level"></a>Niveau van aanmeldingsrisico

Een niveau voor aanmeldingsrisico wordt aangegeven (hoog, Gemiddeld of laag) van de kans dat een poging tot aanmelden niet is uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount.

## <a name="mitigating-sign-in-risk-events"></a>Beperkende aanmeldingsrisico gebeurtenissen

Een beperking is een actie om te beperken van de mogelijkheid van een aanvaller om misbruik te maken van een apparaat of aangetaste identiteit zonder dat de identiteit of het apparaat terugzet op een veilige status heeft. Een vorige aanmeldingsrisico gebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat niet wordt opgelost.

Als u wilt riskante aanmeldingen automatisch oplossen, kunt u aanmeldingsrisico beveiligingsbeleid configureren. Met behulp van deze beleidsregels, overwegen u het risiconiveau van de gebruiker of de aanmelding voor het blokkeren van riskante aanmeldingen of vereisen dat de gebruiker naar de multi-factor authentication uitvoeren. Deze acties kunnen voorkomen dat een aanvaller misbruik kan maken van de identiteit van een gestolen om te leiden tot beschadiging en mogelijk hebt u enige tijd voor het beveiligen van de identiteit.

## <a name="sign-in-risk-security-policy"></a>Beveiligingsbeleid voor aanmeldingsrisico
Een beleid voor aanmeldingsrisico is een beleid voor voorwaardelijke toegang die wordt geëvalueerd als de risico's voor een specifieke aanmelding en oplossingen op basis van vooraf gedefinieerde voorwaarden en regels van toepassing is.

![Beleid voor aanmeldingsrisico](./media/howto-sign-in-risk-policy/1014.png "aanmelden beleid voor gebruikersrisico's")

Azure AD Identity Protection helpt die u bij het beheren van het beperken van riskante aanmeldingen doordat u:

* Stel de gebruikers en groepen die het beleid van toepassing op:

    ![Beleid voor aanmeldingsrisico](./media/howto-sign-in-risk-policy/1015.png "aanmelden beleid voor gebruikersrisico's")
* Stel de aanmeldingsrisico niveau drempelwaarde (laag, Gemiddeld of hoog) waarmee het beleid wordt geactiveerd:

    ![Beleid voor aanmeldingsrisico](./media/howto-sign-in-risk-policy/1016.png "aanmelden beleid voor gebruikersrisico's")
* Stel de besturingselementen moeten worden afgedwongen wanneer het beleid wordt geactiveerd:  

    ![Beleid voor aanmeldingsrisico](./media/howto-sign-in-risk-policy/1017.png "aanmelden beleid voor gebruikersrisico's")
* De status van uw beleid switch:

    ![MFA-registratie](./media/howto-sign-in-risk-policy/403.png "MFA-registratie")
* Bekijken en evalueren van de gevolgen van een wijziging voordat u deze activeert:

    ![Beleid voor aanmeldingsrisico](./media/howto-sign-in-risk-policy/1018.png "aanmelden beleid voor gebruikersrisico's")

## <a name="what-you-need-to-know"></a>Wat u moet weten
U kunt een beveiligingsbeleid aanmeldingsrisico om te vereisen dat multi-factor authentication configureren:

![Beleid voor aanmeldingsrisico](./media/howto-sign-in-risk-policy/1017.png "aanmelden beleid voor gebruikersrisico's")

Echter, uit veiligheidsoverwegingen, deze instelling werkt alleen voor gebruikers die al zijn geregistreerd voor meervoudige verificatie. Als de voorwaarde voor meervoudige verificatie vereisen voor een gebruiker die nog niet is geregistreerd voor multi-factor authentication is voldaan, wordt de gebruiker is geblokkeerd.

Als een best practice, als u wilt meervoudige verificatie vereisen voor riskante aanmeldingen, dient u eerst:

1. Schakel de [registratiebeleid voor meervoudige verificatie](#multi-factor-authentication-registration-policy) voor de betreffende gebruikers.
2. Vereisen dat de betrokken gebruikers om aan te melden in een niet-riskante-sessie met het uitvoeren van een MFA-registratie

Deze stappen uit te voeren, zorgt u ervoor dat meervoudige verificatie is vereist voor een riskante aanmelding.

## <a name="best-practices"></a>Aanbevolen procedures
Kiezen van een **hoge** drempelwaarde vermindert het aantal keer dat een beleid wordt geactiveerd en minimaliseert de gevolgen voor gebruikers.  

Echter, worden uitgesloten **laag** en **gemiddeld** aanmeldingen die zijn gemarkeerd voor risico's van het beleid, dat een aanvaller misbruik kan maken van de identiteit van een verdachte kan niet worden geblokkeerd.

Bij het instellen van het beleid

* Voorkomen dat gebruikers die geen / geen meervoudige verificatie
* Gebruikers in de landinstellingen waar als het beleid is het niet praktisch uitsluiten (bijvoorbeeld geen toegang tot de helpdesk)
* Gebruikers die waarschijnlijk voor het genereren van een groot aantal fout-positieven (ontwikkelaars, beveiligingsanalisten) uitsluiten
* Gebruik een **hoge** drempelwaarde tijdens de implementatie van het eerste beleid, of als u uitdagingen gezien door eindgebruikers moet minimaliseren.
* Gebruik een **laag** drempelwaarde als uw organisatie betere beveiliging vereist. Selecteren van een **laag** drempelwaarde introduceert extra gebruiker aanmelden uitdagingen, maar de verbeterde beveiliging.

De aanbevolen standaardwaarde voor de meeste organisaties is het configureren van een regel voor een **gemiddeld** drempelwaarde een evenwicht tussen bruikbaarheid en veiligheid te vinden.

Het beleid voor aanmeldingsrisico is:

* Toegepast op alle browserverkeer en aanmeldingen die moderne authenticatie gebruiken.
* Niet toegepast op toepassingen die gebruikmaken van oudere beveiligingsprotocollen door het WS-Trust-eindpunt op de federatieve id-provider, zoals ADFS uit te schakelen.

De **Risicogebeurtenissen** pagina in de console Identity Protection geeft een lijst van alle gebeurtenissen:

* Dit beleid is toegepast op
* U kunt de activiteiten controleren en bepalen of de actie die geschikt of niet is

Zie voor een overzicht van de gebruikerservaring:

* [Riskante aanmelding herstel](flows.md#risky-sign-in-recovery)
* [Riskante aanmelding geblokkeerd](flows.md#risky-sign-in-blocked)  
* [Aanmelden-ervaringen met Azure AD Identity Protection](flows.md)  

**Opent het dialoogvenster gerelateerde configuratie**:

- Op de **Azure AD Identity Protection** blade in de **configureren** sectie, klikt u op **aanmelden beleid voor gebruikersrisico's**.

    ![Beleid voor gebruikersrisico's](./media/howto-sign-in-risk-policy/1014.png "beleid voor gebruikersrisico's")




## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview.md).

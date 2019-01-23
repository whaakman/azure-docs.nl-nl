---
title: Het beleid voor aanmeldingsrisico configureren in Azure Active Directory Identity Protection | Microsoft Docs
description: Informatie over het configureren van de Azure AD Identity Protection-beleid voor aanmeldingsrisico.
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: daveba
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
ms.openlocfilehash: 1c3b7dc8e027169db51ab9ef50634a69a43cd3e0
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54462059"
---
# <a name="how-to-configure-the-sign-in-risk-policy"></a>Procedures: Het beleid voor aanmeldingsrisico’s configureren

Azure Active Directory detecteert [risicogebeurtenistypen](../reports-monitoring/concept-risk-events.md#risk-event-types) in realtime en offline. Elke risicogebeurtenis die is gedetecteerd voor een aanmelding van een gebruiker draagt bij aan een logisch concept is met de naam riskante aanmelding. Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk niet uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount.


## <a name="what-is-the-sign-in-risk-policy"></a>Wat is het beleid voor aanmeldingsrisico?

Azure AD analyseert elke aanmelding van een gebruiker. Het doel van de analyse is voor het detecteren van verdachte activiteit die afkomstig, samen met de aanmelding zijn. Bijvoorbeeld, is de aanmelding uitgevoerd met behulp van een anoniem IP-adres of is de aanmelding die afkomstig zijn van een onbekende locatie? De verdachte activiteit die kan worden gedetecteerd door het systeem zijn in Azure AD, ook wel bekend als risicogebeurtenissen. Op basis van de risicogebeurtenissen die zijn gedetecteerd tijdens de aanmelding, Azure AD een waarde wordt berekend. De waarde vertegenwoordigt de kans (laag, Gemiddeld, hoog) dat de aanmelding wordt niet uitgevoerd door de bevoegde gebruiker. De kans op heet **niveau van aanmeldingsrisico**.

Het beleid voor aanmeldingsrisico is een geautomatiseerd antwoord dat u voor een specifieke aanmeldingsrisico-niveau configureren kunt. U kunt toegang tot uw resources blokkeren of vereisen voor het doorgeven van een uitdaging multi-factor authentication (MFA) om toegang te krijgen in uw antwoord.

   
## <a name="how-do-i-access-the-sign-in-risk-policy"></a>Hoe krijg ik toegang tot het beleid voor aanmeldingsrisico?
   
Het beleid voor aanmeldingsrisico is in de **configureren** sectie op de [Azure AD Identity Protection-pagina](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Beleid voor aanmeldingsrisico](./media/howto-sign-in-risk-policy/1014.png "aanmelden beleid voor gebruikersrisico's")


## <a name="policy-settings"></a>Beleidsinstellingen

Wanneer u het beleid voor aanmeldingsrisico configureert, moet u instellen:

- De gebruikers en groepen die het beleid van toepassing op:

    ![Gebruikers en groepen](./media/howto-sign-in-risk-policy/11.png)

- Het niveau van aanmeldingsrisico waarmee het beleid wordt geactiveerd:

    ![Niveau van aanmeldingsrisico](./media/howto-sign-in-risk-policy/12.png)

- Het type toegang dat u wilt worden afgedwongen wanneer de risiconiveau van uw aanmelding is voldaan:  

    ![Access](./media/howto-sign-in-risk-policy/13.png)

- De status van uw beleid:

    ![Afdwingen van beleid](./media/howto-sign-in-risk-policy/14.png)


Het dialoogvenster van de configuratie van beleid biedt u een optie voor het schatten van de impact van herconfiguratie.

![Verwachte impact](./media/howto-sign-in-risk-policy/15.png)

## <a name="what-you-should-know"></a>Wat u moet weten

U kunt een beveiligingsbeleid aanmeldingsrisico als u MFA wilt configureren:

![MFA vereisen](./media/howto-sign-in-risk-policy/16.png)

Echter, uit veiligheidsoverwegingen, deze instelling werkt alleen voor gebruikers die al zijn geregistreerd voor MFA. Identiteitsbeveiliging wordt voorkomen dat gebruikers met een MFA-vereiste als ze zijn nog niet geregistreerd voor MFA.

Als u wilt om MFA te vereisen voor riskante aanmeldingen, moet u:

1. Schakel de [registratiebeleid voor meervoudige verificatie](#multi-factor-authentication-registration-policy) voor de betreffende gebruikers.

2. Vereisen dat de betrokken gebruikers aanmelden bij een niet-riskante-sessie uit te voeren een MFA-registratie.

Deze stappen uit te voeren, zorgt u ervoor dat meervoudige verificatie is vereist voor een riskante aanmelding.

Het beleid voor aanmeldingsrisico is:

- Toegepast op alle browserverkeer en aanmeldingen die moderne authenticatie gebruiken.

- Niet toegepast op toepassingen die gebruikmaken van oudere beveiligingsprotocollen door het WS-Trust-eindpunt op de federatieve id-provider, zoals ADFS uit te schakelen.


Zie voor een overzicht van de gebruikerservaring:

* [Riskante aanmelding herstel](flows.md#risky-sign-in-recovery)
* [Riskante aanmelding geblokkeerd](flows.md#risky-sign-in-blocked)  
* [Aanmelden-ervaringen met Azure AD Identity Protection](flows.md)  

## <a name="best-practices"></a>Aanbevolen procedures

Kiezen van een **hoge** drempelwaarde vermindert het aantal keer dat een beleid wordt geactiveerd en minimaliseert de gevolgen voor gebruikers.  

Echter, worden uitgesloten **laag** en **gemiddeld** aanmeldingen die zijn gemarkeerd voor risico's van het beleid, dat een aanvaller misbruik kan maken van de identiteit van een verdachte kan niet worden geblokkeerd.

Bij het instellen van het beleid

- Voorkomen dat gebruikers die geen / geen meervoudige verificatie

- Gebruikers in de landinstellingen waar als het beleid is het niet praktisch uitsluiten (bijvoorbeeld geen toegang tot de helpdesk)

- Gebruikers die waarschijnlijk voor het genereren van veel fout-positieven (ontwikkelaars, beveiligingsanalisten) uitsluiten

- Gebruik een **hoge** drempelwaarde tijdens de eerste beleid uitrollen, of als u uitdagingen gezien door eindgebruikers moet minimaliseren.

- Gebruik een **laag** drempelwaarde als uw organisatie betere beveiliging vereist. Selecteren van een **laag** drempelwaarde introduceert extra gebruiker aanmelden uitdagingen, maar de verbeterde beveiliging.

De aanbevolen standaardwaarde voor de meeste organisaties is het configureren van een regel voor een **gemiddeld** drempelwaarde een evenwicht tussen bruikbaarheid en veiligheid te vinden.






## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview.md).

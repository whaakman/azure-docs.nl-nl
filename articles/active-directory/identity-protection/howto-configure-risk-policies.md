---
title: Risico-beleidsregels configureren in Azure Active Directory identity protection (vernieuwd) | Microsoft Docs
description: Klik hier voor meer informatie over het risico-beleid configureren in Azure Active Directory identity protection (vernieuwd).
services: active-directory
keywords: Azure active directory identity protection cloud app discovery, toepassingen, beveiliging, risico's, risiconiveau, beveiligingsproblemen, beveiligingsbeleid beheren
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: a5e6d63627764ff64ac5e0621e271140ab3ed828
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55099812"
---
# <a name="how-to-configure-risk-policies-in-azure-active-directory-identity-protection-refreshed"></a>Procedures: Risico-beleid configureren in Azure Active Directory identity protection (vernieuwd)


Azure AD detecteert risicogebeurtenissen die indicatoren voor mogelijk verdachte identiteiten. Risico's door beleidsregels te configureren, kunt u automatische antwoorden om de resultaten te definiëren:

- Met het beleid voor aanmeldingsrisico, kunt u een reactie op realtime risicogebeurtenissen die zijn gedetecteerd tijdens de aanmelding van een gebruiker configureren. 
- Met het beleid voor gebruikersrisico's, kunt u een reactie op alle actieve gebruiker risico's die zijn gedetecteerd voor een gebruiker na verloop van tijd configureren.  


## <a name="what-is-the-sign-in-risk-policy"></a>Wat is het beleid voor aanmeldingsrisico?

Azure AD analyseert elke aanmelding van een gebruiker. Het doel van de analyse is voor het detecteren van verdachte activiteit die afkomstig, samen met de aanmelding zijn. Bijvoorbeeld, is de aanmelding uitgevoerd met behulp van een anoniem IP-adres of is de aanmelding die afkomstig zijn van een onbekende locatie? De verdachte activiteit die kan worden gedetecteerd door het systeem zijn in Azure AD, ook wel bekend als risicogebeurtenissen. Op basis van de risicogebeurtenissen die zijn gedetecteerd tijdens de aanmelding, Azure AD een waarde wordt berekend. De waarde vertegenwoordigt de kans (laag, Gemiddeld, hoog) dat de aanmelding wordt niet uitgevoerd door de bevoegde gebruiker. De kans op heet **niveau van aanmeldingsrisico**.

Het beleid voor aanmeldingsrisico is een geautomatiseerd antwoord dat u voor een specifieke aanmeldingsrisico-niveau configureren kunt. U kunt toegang tot uw resources blokkeren of vereisen voor het doorgeven van een uitdaging multi-factor authentication (MFA) om toegang te krijgen in uw antwoord.

   
## <a name="how-do-i-access-the-sign-in-risk-policy"></a>Hoe krijg ik toegang tot het beleid voor aanmeldingsrisico?
   
Het beleid voor aanmeldingsrisico is in de **configureren** sectie op de [Azure AD Identity Protection-pagina](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Beleid voor aanmeldingsrisico](./media/howto-configure-risk-policies/1014.png "aanmelden beleid voor gebruikersrisico's")


## <a name="sign-in-risk-policy-settings"></a>Instellingen voor aanmeldingsrisico

Wanneer u het beleid voor aanmeldingsrisico configureert, moet u instellen:

- De gebruikers en groepen die het beleid van toepassing op:

    ![Gebruikers en groepen](./media/howto-configure-risk-policies/11.png)

- Het niveau van aanmeldingsrisico waarmee het beleid wordt geactiveerd:

    ![Niveau van aanmeldingsrisico](./media/howto-configure-risk-policies/12.png)

- Het type toegang dat u wilt worden afgedwongen wanneer de risiconiveau van uw aanmelding is voldaan:  

    ![Access](./media/howto-configure-risk-policies/13.png)

- De status van uw beleid:

    ![Afdwingen van beleid](./media/howto-configure-risk-policies/14.png)


Het dialoogvenster van de configuratie van beleid biedt u een optie voor het schatten van de impact van herconfiguratie.

![Verwachte impact](./media/howto-configure-risk-policies/15.png)

## <a name="what-you-should-know-about-sign-in-risk-policies"></a>Wat u moet weten over het beleid voor aanmeldingsrisico

U kunt een beveiligingsbeleid aanmeldingsrisico als u MFA wilt configureren:

![MFA vereisen](./media/howto-configure-risk-policies/16.png)

Echter, uit veiligheidsoverwegingen, deze instelling werkt alleen voor gebruikers die al zijn geregistreerd voor MFA. Identiteitsbeveiliging wordt voorkomen dat gebruikers met een MFA-vereiste als ze zijn nog niet geregistreerd voor MFA.

Als u wilt om MFA te vereisen voor riskante aanmeldingen, moet u:

1. Schakel de [registratiebeleid voor meervoudige verificatie](#multi-factor-authentication-registration-policy) voor de betreffende gebruikers.

2. Vereisen dat u de betrokken gebruikers om aan te melden in een niet-riskante-sessie uit te voeren een MFA-registratie.

Deze stappen uit te voeren, zorgt u ervoor dat meervoudige verificatie is vereist voor een riskante aanmelding.

Het beleid voor aanmeldingsrisico is:

- Toegepast op alle browserverkeer en aanmeldingen die moderne authenticatie gebruiken.

- Niet toegepast op toepassingen die gebruikmaken van oudere beveiligingsprotocollen door het WS-Trust-eindpunt op de federatieve id-provider, zoals ADFS uit te schakelen.


Zie voor een overzicht van de gebruikerservaring:

* [Riskante aanmelding herstel](flows.md#risky-sign-in-recovery)
* [Riskante aanmelding geblokkeerd](flows.md#risky-sign-in-blocked)  
* [Aanmelden-ervaringen met Azure AD Identity Protection](flows.md)  









## <a name="what-is-a-user-risk-policy"></a>Wat is er een beleid voor gebruikersrisico's?

Azure AD analyseert elke aanmelding van een gebruiker. Het doel van de analyse is voor het detecteren van verdachte activiteit die afkomstig, samen met de aanmelding zijn. De verdachte activiteit die kan worden gedetecteerd door het systeem zijn in Azure AD, ook wel bekend als risicogebeurtenissen. Risico's en gebeurtenissen in realtime kunnen worden gedetecteerd, er zijn ook risicogebeurtenissen vereisen meer tijd. Voor het detecteren van een onmogelijke reis naar ongewone locaties, bijvoorbeeld, vereist het systeem een eerste leerperiode van 14 dagen voor meer informatie over het normale gedrag van een gebruiker. Er zijn verschillende opties om op te lossen gedetecteerde risico. Bijvoorbeeld, kunt u afzonderlijke risicogebeurtenissen handmatig oplossen, of u deze omgezet met behulp van een aanmeldingsrisico of een beleid voor gebruikersrisico's voor voorwaardelijke toegang kunt krijgen.

Alle risicogebeurtenissen die zijn gedetecteerd voor een gebruiker en niet worden opgelost staat bekend als actieve risico's. De actieve risicogebeurtenissen die gekoppeld aan een gebruiker zijn staat bekend als gebruikersrisico. Op basis van het gebruikersrisico, berekent Azure AD een kans (laag, Gemiddeld, hoog) dat een gebruiker is aangetast. De kans op heet risiconiveau van de gebruiker.

![Gebruiker risico 's](./media/howto-configure-risk-policies/11031.png)

Het beleid voor gebruikersrisico's is een geautomatiseerd antwoord dat u voor een specifieke gebruiker risiconiveau configureren kunt. U kunt met een beleid voor gebruikersrisico's, toegang tot uw resources blokkeren of een wachtwoordwijziging aan een gebruikersaccount toegang krijgen tot een schone toestand vereisen.


## <a name="how-do-i-access-the-user-risk-policy"></a>Hoe krijg ik toegang tot het beleid voor gebruikersrisico's?
   
Het beleid voor gebruikersrisico's zich in de **configureren** sectie op de [Azure AD Identity Protection-pagina](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Beleid voor gebruikersrisico's](./media/howto-configure-risk-policies/11014.png)



## <a name="user-risk-policy-settings"></a>Instellingen voor gebruikersbeleid risico

Wanneer u het beleid voor aanmeldingsrisico configureert, moet u instellen:

- De gebruikers en groepen die het beleid van toepassing op:

    ![Gebruikers en groepen](./media/howto-configure-risk-policies/111.png)

- Het niveau van aanmeldingsrisico waarmee het beleid wordt geactiveerd:

    ![Niveau van gebruikersrisico](./media/howto-configure-risk-policies/112.png)

- Het type toegang dat u wilt worden afgedwongen wanneer de risiconiveau van uw aanmelding is voldaan:  

    ![Access](./media/howto-configure-risk-policies/113.png)

- De status van uw beleid:

    ![Afdwingen van beleid](./media/howto-configure-risk-policies/114.png)

Het dialoogvenster van de configuratie van beleid biedt u een optie voor het schatten van de impact van uw configuratie.

![Verwachte impact](./media/howto-configure-risk-policies/115.png)

## <a name="what-you-should-know-about-user-risk-polices"></a>Wat u moet weten over gebruikersrisico beleidsregels

U kunt een beleid voor gebruikersrisico's security instellen om te voorkomen dat gebruikers bij het aanmelden, afhankelijk van het risiconiveau.

![Blokkeren](./media/howto-configure-risk-policies/116.png)


Blokkering van een aanmeldingen:

* Hiermee voorkomt u dat de generatie van de nieuwe gebruiker-risicogebeurtenissen voor de betrokken gebruiker
* Hiermee kunnen beheerders handmatig de risicogebeurtenissen die betrekking hebben op de identiteit van de gebruiker herstellen en terug te zetten naar een veilige status


























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

 [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)


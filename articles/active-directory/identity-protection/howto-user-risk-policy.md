---
title: Het beleid voor gebruikersrisico's configureren in Azure Active Directory Identity Protection | Microsoft Docs
description: Informatie over het configureren van het beleid voor gebruikersrisico's Azure AD Identity Protection.
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
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 3ba8c93d158a6ff80c3db788924503e9d4e87242
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378116"
---
# <a name="how-to-configure-the-user-risk-policy"></a>Procedure: Het beleid voor gebruikersrisico's configureren

Met het gebruikersrisico vaststelt Azure AD dat de kans dat een gebruikersaccount verdacht is. U kunt als beheerder, gebruiker risico beleid voor voorwaardelijke toegang, automatisch reageren op een specifieke gebruiker risiconiveau configureren.
 
In dit artikel biedt u de informatie die u moet een beleid voor gebruikersrisico's configureren.


## <a name="what-is-a-user-risk-policy"></a>Wat is er een beleid voor gebruikersrisico's?

Azure AD analyseert elke aanmelding van een gebruiker. Het doel van de analyse is voor het detecteren van verdachte activiteit die afkomstig, samen met de aanmelding zijn. De verdachte activiteit die kan worden gedetecteerd door het systeem zijn in Azure AD, ook wel bekend als risicogebeurtenissen. Risico's en gebeurtenissen in realtime kunnen worden gedetecteerd, er zijn ook risicogebeurtenissen vereisen meer tijd. Voor het detecteren van een onmogelijke reis naar ongewone locaties, bijvoorbeeld, vereist het systeem een eerste leerperiode van 14 dagen voor meer informatie over het normale gedrag van een gebruiker. Er zijn verschillende opties om op te lossen gedetecteerde risico. Bijvoorbeeld, kunt u afzonderlijke risicogebeurtenissen handmatig oplossen, of u deze omgezet met behulp van een aanmeldingsrisico of een beleid voor gebruikersrisico's voor voorwaardelijke toegang kunt krijgen.

Alle risicogebeurtenissen die zijn gedetecteerd voor een gebruiker en niet worden opgelost staat bekend als actieve risico's. De actieve risicogebeurtenissen die gekoppeld aan een gebruiker zijn staat bekend als gebruikersrisico. Op basis van het gebruikersrisico, berekent Azure AD een kans (laag, Gemiddeld, hoog) dat een gebruiker is aangetast. De kans op heet risiconiveau van de gebruiker.

![Gebruiker risico 's](./media/howto-user-risk-policy/1031.png)

Het beleid voor gebruikersrisico's is een geautomatiseerd antwoord dat u voor een specifieke gebruiker risiconiveau configureren kunt. U kunt met een beleid voor gebruikersrisico's, toegang tot uw resources blokkeren of een wachtwoordwijziging aan een gebruikersaccount toegang krijgen tot een schone toestand vereisen.


## <a name="how-do-i-access-the-user-risk-policy"></a>Hoe krijg ik toegang tot het beleid voor gebruikersrisico's?
   
Het beleid voor aanmeldingsrisico is in de **configureren** sectie op de [Azure AD Identity Protection-pagina](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Beleid voor gebruikersrisico's](./media/howto-user-risk-policy/1014.png)



## <a name="policy-settings"></a>Beleidsinstellingen

Wanneer u het beleid voor aanmeldingsrisico configureert, moet u instellen:

- De gebruikers en groepen die het beleid van toepassing op:

    ![Gebruikers en groepen](./media/howto-user-risk-policy/11.png)

- Het niveau van aanmeldingsrisico waarmee het beleid wordt geactiveerd:

    ![Niveau van gebruikersrisico](./media/howto-user-risk-policy/12.png)

- Het type toegang dat u wilt worden afgedwongen wanneer de risiconiveau van uw aanmelding is voldaan:  

    ![Access](./media/howto-user-risk-policy/13.png)

- De status van uw beleid:

    ![Afdwingen van beleid](./media/howto-user-risk-policy/14.png)

Het dialoogvenster van de configuratie van beleid biedt u een optie voor het schatten van de impact van uw configuratie.

![Verwachte impact](./media/howto-user-risk-policy/15.png)

## <a name="what-you-should-know"></a>Wat u moet weten

U kunt een beleid voor gebruikersrisico's security instellen om te voorkomen dat gebruikers bij het aanmelden, afhankelijk van het risiconiveau.

![Blokkeren](./media/howto-user-risk-policy/16.png)


Blokkering van een aanmeldingen:

* Hiermee voorkomt u dat de generatie van de nieuwe gebruiker-risicogebeurtenissen voor de betrokken gebruiker
* Hiermee kunnen beheerders handmatig de risicogebeurtenissen die betrekking hebben op de identiteit van de gebruiker herstellen en terug te zetten naar een veilige status

## <a name="best-practices"></a>Aanbevolen procedures

Kiezen van een **hoge** drempelwaarde vermindert het aantal keer dat een beleid wordt geactiveerd en minimaliseert de gevolgen voor gebruikers.
Echter, worden uitgesloten **laag** en **gemiddeld** gebruikers die zijn gemarkeerd voor risico's van het beleid, die niet-id's of apparaten die beveiligen mogelijk eerder zijn verdachte of bekend is.

Bij het instellen van het beleid

* Gebruikers die waarschijnlijk voor het genereren van een groot aantal fout-positieven (ontwikkelaars, beveiligingsanalisten) uitsluiten
* Gebruikers in de landinstellingen waar als het beleid is het niet praktisch uitsluiten (bijvoorbeeld geen toegang tot de helpdesk)
* Gebruik een **hoge** drempelwaarde tijdens de implementatie van het eerste beleid, of als u uitdagingen gezien door eindgebruikers moet minimaliseren.
* Gebruik een **laag** drempelwaarde als uw organisatie betere beveiliging vereist. Selecteren van een **laag** drempelwaarde introduceert extra gebruiker aanmelden uitdagingen, maar de verbeterde beveiliging.

De aanbevolen standaardwaarde voor de meeste organisaties is het configureren van een regel voor een **gemiddeld** drempelwaarde een evenwicht tussen bruikbaarheid en veiligheid te vinden.

Zie voor een overzicht van de gebruikerservaring:

* [Aangetast account recovery stroom](flows.md#compromised-account-recovery).  
* [Aangetast account geblokkeerd stroom](flows.md#compromised-account-blocked).  

**Opent het dialoogvenster gerelateerde configuratie**:

- Op de **Azure AD Identity Protection** blade in de **configureren** sectie, klikt u op **beleid voor gebruikersrisico's**.

    ![Beleid voor gebruikersrisico's](./media/howto-user-risk-policy/1009.png "beleid voor gebruikersrisico's")




## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview.md).

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
ms.openlocfilehash: bcfab9ab95e41b723cb8a8e49d7390a2894d5219
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45581368"
---
# <a name="how-to-configure-the-user-risk-policy"></a>Procedure: Het beleid voor gebruikersrisico's configureren

Alle actieve [risicogebeurtenissen](../reports-monitoring/concept-risk-events.md) die zijn gedetecteerd door Azure Active Directory voor een gebruiker bijdragen aan een logisch concept is gebruikersrisico genoemd. Een gebruiker die is gemarkeerd voor risico's is een indicator van een gebruikersaccount dat mogelijk is aangetast.

![Gebruikers voor wie wordt aangegeven dat ze risico lopen](./media/howto-user-risk-policy/1200.png)


## <a name="user-risk-level"></a>Niveau van gebruikersrisico

Een risiconiveau van de gebruiker wordt aangegeven (hoog, Gemiddeld of laag) van de kans dat de identiteit van de gebruiker is aangetast. Dit wordt berekend op basis van de gebruiker risicogebeurtenissen die gekoppeld aan de identiteit van een gebruiker zijn.

De status van een risicogebeurtenis is **Active** of **gesloten**. Alleen risicogebeurtenissen die zijn **Active** bijdragen aan de berekening van risico-niveau voor de gebruiker.

Het risiconiveau van de gebruiker wordt berekend met behulp van de invoer van de volgende:

* Actieve risicogebeurtenissen die invloed hebben op de gebruiker
* Risiconiveau van deze gebeurtenissen
* Of welke herstelacties zijn getroffen

![Gebruiker risico's](./media/howto-user-risk-policy/1031.png "gebruiker risico's")

U kunt de risiconiveaus van de gebruiker gebruiken voor het maken van beleid voor voorwaardelijke toegang die riskante gebruikers aanmelden blokkeren of zorgen dat ze veilig hun wachtwoord wijzigen.

## <a name="closing-risk-events-manually"></a>Risicogebeurtenissen handmatig sluiten

In de meeste gevallen neemt u herstelacties zoals het opnieuw instellen van een beveiligd wachtwoord om risicogebeurtenissen automatisch te sluiten. Dit is echter niet altijd mogelijk.  
Dit is, bijvoorbeeld het geval wanneer:

* Een gebruiker met actieve risicogebeurtenissen zijn verwijderd
* Uit onderzoek blijkt dat een risicogebeurtenis is uitgevoerd door een legitieme gebruiker

Omdat risicogebeurtenissen die **Actief** zijn, bijdragen aan de berekening van het gebruikersrisico, moet u een risiconiveau mogelijk handmatig verlagen door risicogebeurtenissen handmatig te sluiten.  
Gedurende het onderzoek kunt u een van deze acties kiezen om de status van een risicogebeurtenis te wijzigen:

![Acties](./media/howto-user-risk-policy/34.png "acties")

* **Oplossen** - als u na onderzoek van een risicogebeurtenis een juiste herstelactie buiten Identity Protection hebt ondernomen en van mening bent dat de risicogebeurtenis kan worden gesloten, markeert u de gebeurtenis als Opgelost. Een opgeloste risicogebeurtenis krijgt de status Gesloten. Bovendien draagt de risicogebeurtenis niet meer bij aan het gebruikersrisico.
* **Markeren als fout-positieve** -In sommige gevallen kan u een risicogebeurtenis onderzoeken en ontdek ten onrechte is gemarkeerd als een riskant. U kunt het nummer van deze instanties verminderen door de risicogebeurtenis als fout-positieve markeren. Hiermee kunt u de machine learning-algoritmen voor de classificatie van soortgelijke gebeurtenissen in de toekomst te verbeteren. De status van de fout-positieve gebeurtenissen wordt **gesloten** en ze niet meer dragen bij aan gebruikersrisico.
* **Negeren** :  als u geen herstelactie heb uitgevoerd, maar wilt dat de risicogebeurtenis wordt verwijderd uit de actieve lijst, kunt u een risicogebeurtenis markeren als Negeren. De status van de gebeurtenis wordt dan Gesloten. Genegeerde gebeurtenissen dragen niet bij aan het gebruikersrisico. Deze optie dient alleen in ongebruikelijke omstandigheden te worden gebruikt.
* **Opnieuw activeren** -gebeurtenissen die handmatig zijn gesloten (door bijvoorbeeld te kiezen voor **Oplossen**,  **Vals positief** of **Negeren**), kunnen opnieuw worden geactiveerd. Dit kan door de status van de gebeurtenis terug te zetten naar **Actief**. Opnieuw geactiveerde risicogebeurtenissen dragen bij aan de berekening van het gebruikersrisiconiveau. Risico's die gesloten zijn via herstelacties (zoals het opnieuw instellen van een beveiligd wachtwoord), kunnen niet opnieuw worden geactiveerd.


**Opent het dialoogvenster gerelateerde configuratie**:

1. Op de **Azure AD Identity Protection** blade onder **onderzoeken**, klikt u op **Risicogebeurtenissen**.

    ![Handmatige wachtwoordherstel](./media/howto-user-risk-policy/1002.png "handmatige wachtwoord opnieuw instellen")
2. In de **Risicogebeurtenissen** lijst, klikt u op een risico.

    ![Handmatige wachtwoordherstel](./media/howto-user-risk-policy/1003.png "handmatige wachtwoord opnieuw instellen")
3. Met de rechtermuisknop op een gebruiker op de blade risico.

    ![Handmatige wachtwoordherstel](./media/howto-user-risk-policy/1004.png "handmatige wachtwoord opnieuw instellen")

## <a name="closing-all-risk-events-for-a-user-manually"></a>Alle risicogebeurtenissen voor een gebruiker sluiten handmatig
In plaats van handmatig sluiten afzonderlijk risicogebeurtenissen voor een gebruiker, biedt Azure Active Directory Identity Protection u ook een methode voor het sluiten van alle gebeurtenissen voor een gebruiker met één klik.

![Acties](./media/howto-user-risk-policy/2222.png "acties")

Wanneer u klikt op **alle gebeurtenissen sluiten**, alle gebeurtenissen worden gesloten en de betrokken gebruiker is niet meer risico.

## <a name="remediating-user-risk-events"></a>Bescherm risicogebeurtenissen voor gebruiker

Een herstel is een actie voor het beveiligen van een identiteit of een apparaat dat eerder is verdacht of bekend is. Een herstelactie wordt de identiteit of het apparaat hersteld naar een veilige status en wordt omgezet vorige risicogebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat.

Als u wilt herstellen risicogebeurtenissen gebruiker, kunt u het volgende doen:

* Uitvoeren van een veilig wachtwoord opnieuw instellen op gebruiker risicogebeurtenissen handmatig herstellen
* Configureren van een security beleid voor gebruikersrisico's om te beperken of gebruiker risicogebeurtenissen automatisch herstellen
* De geïnfecteerd apparaat herstellen met installatiekopie  

### <a name="manual-secure-password-reset"></a>Handmatige beveiligd wachtwoord opnieuw instellen
Een veilig wachtwoord opnieuw instellen is van een doeltreffende herstel voor veel risicogebeurtenissen en wanneer uitgevoerd, sluit u deze risicogebeurtenissen automatisch en het risiconiveau van de gebruiker opnieuw worden berekend. U kunt het dashboard Identity Protection gebruiken om te starten van een wachtwoord opnieuw instellen voor een riskante gebruiker.

De gerelateerde dialoogvenster biedt twee verschillende methoden om een wachtwoord opnieuw instellen:

**Wachtwoord opnieuw instellen** : Selecteer **vereisen dat de gebruiker het wachtwoord opnieuw instelt** zodat de gebruiker zelf herstellen als de gebruiker is geregistreerd voor meervoudige verificatie. Tijdens de gebruiker de volgende aanmelding wordt worden de gebruiker voor het oplossen van een uitdaging meervoudige verificatie is vereist en vervolgens gedwongen om het wachtwoord te wijzigen. Deze optie niet beschikbaar als het gebruikersaccount dat nog niet is geregistreerd met meervoudige verificatie.

**Tijdelijk wachtwoord** : Selecteer **een tijdelijk wachtwoord genereren** onmiddellijk het bestaande wachtwoord ongeldig te maken en het maken van een nieuw tijdelijk wachtwoord voor de gebruiker. Het tijdelijke wachtwoord verzenden naar een alternatieve e-mailadres voor de gebruiker of de manager van de gebruiker. Omdat het wachtwoord tijdelijk is, wordt de gebruiker gevraagd het wachtwoord aanmelden bij een te wijzigen.

![Beleid](./media/howto-user-risk-policy/1005.png "beleid")

**Opent het dialoogvenster gerelateerde configuratie**:

1. Op de **Azure AD Identity Protection** blade, klikt u op **gebruikers die zijn gemarkeerd voor risico's**.

    ![Handmatige wachtwoordherstel](./media/howto-user-risk-policy/1006.png "handmatige wachtwoord opnieuw instellen")
2. Selecteer een gebruiker met ten minste één risicogebeurtenissen in de lijst van gebruikers.

    ![Handmatige wachtwoordherstel](./media/howto-user-risk-policy/1007.png "handmatige wachtwoord opnieuw instellen")
3. Klik op de blade van de gebruiker, **wachtwoord opnieuw instellen**.

    ![Handmatige wachtwoordherstel](./media/howto-user-risk-policy/1008.png "handmatige wachtwoord opnieuw instellen")

## <a name="user-risk-security-policy"></a>Beleid voor gebruikersrisico's security
Een beleid voor gebruikersrisico's security is een beleid voor voorwaardelijke toegang die wordt geëvalueerd als het risiconiveau dat aan een specifieke gebruiker en het herstel en risicobeperking op basis van vooraf gedefinieerde voorwaarden en regels maatregelen.

![Beleid voor gebruikersrisico's](./media/howto-user-risk-policy/1009.png "beleid voor gebruikersrisico's")

Azure AD Identity Protection kunt u de risicobeperking en herstel van gebruikers die zijn gemarkeerd voor risico's door te beheren:

* Stel de gebruikers en groepen die het beleid van toepassing op:

    ![Beleid voor gebruikersrisico's](./media/howto-user-risk-policy/1010.png "beleid voor gebruikersrisico's")
* Drempelwaarde in te stellen de gebruiker risico niveau (laag, Gemiddeld of hoog) waarmee het beleid wordt geactiveerd:

    ![Beleid voor gebruikersrisico's](./media/howto-user-risk-policy/1011.png "beleid voor gebruikersrisico's")
* Stel de besturingselementen moeten worden afgedwongen wanneer het beleid wordt geactiveerd:

    ![Beleid voor gebruikersrisico's](./media/howto-user-risk-policy/1012.png "beleid voor gebruikersrisico's")
* De status van uw beleid switch:

    ![Beleid voor gebruikersrisico's](./media/howto-user-risk-policy/403.png "MFA-registratie")
* Bekijken en evalueren van de gevolgen van een wijziging voordat u deze activeert:

    ![Beleid voor gebruikersrisico's](./media/howto-user-risk-policy/1013.png "beleid voor gebruikersrisico's")

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

## <a name="mitigating-user-risk-events"></a>Beperkende risicogebeurtenissen voor gebruiker
Beheerders kunnen een beleid voor gebruikersrisico's security instellen om te voorkomen dat gebruikers bij het aanmelden, afhankelijk van het risiconiveau.

Blokkering van een aanmeldingen:

* Hiermee voorkomt u dat de generatie van de nieuwe gebruiker-risicogebeurtenissen voor de betrokken gebruiker
* Hiermee kunnen beheerders handmatig de risicogebeurtenissen die betrekking hebben op de identiteit van de gebruiker herstellen en terug te zetten naar een veilige status


## <a name="next-steps"></a>Volgende stappen

Als u een overzicht van Azure AD Identity Protection, raadpleegt u de [overzicht van Azure AD Identity Protection](overview.md).

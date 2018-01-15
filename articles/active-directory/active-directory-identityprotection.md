---
title: Azure Active Directory Identity Protection | Microsoft Docs
description: Meer informatie over hoe Azure AD Identity Protection kunt u de mogelijkheid van een aanvaller misbruik maakt van een verdachte identiteit of het apparaat en voor het beveiligen van een identiteit of een apparaat dat eerder is verdacht of bekend is dat inbreuk wordt gepleegd beperken.
services: active-directory
keywords: beveiliging in Azure active directory-identiteit, cloud app discovery, het beheren van toepassingen, beveiliging, risico, risiconiveau, beveiligingsprobleem, beveiligingsbeleid
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: e66d033d95efccf53ea2de889b5811fe2eafb76a
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

Azure Active Directory: Identity Protection is een functie van de Azure AD Premium P2-versie waarmee u kunt:

- Detecteren van mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie

- Automatische antwoorden op gedetecteerde verdachte acties die zijn gekoppeld aan de identiteiten van uw organisatie configureren  

- Verdachte incidenten onderzoeken en onderneem gepaste actie deze op te lossen   


## <a name="getting-started"></a>Aan de slag

Cloud-gebaseerde identiteiten is voor meer dan een tien jaar beveiligd door Microsoft. Met Azure Active Directory: Identity Protection kunt in uw omgeving, u de dezelfde systemen ter bescherming van die Microsoft gebruikt voor het beveiligen van identiteiten.

De meeste beveiligingslekken plaatsvinden als aanvallers toegang krijgen tot een omgeving met de identiteit van een gebruiker te stelen. In de afgelopen jaren, zijn aanvallers steeds effectief gebruik van schendingen van derden en het gebruik van geavanceerde phishingaanvallen geworden. Als een aanvaller toegang tot het zelfs als deze gebruiker met laag bevoegde accounts krijgt, is het relatief gemakkelijk voor hen toegang te krijgen tot bronnen van belangrijke bedrijfsgegevens via laterale verplaatsing.

Als gevolg hiervan moet u:

- Beveiligen van alle identiteiten ongeacht hun machtigingsniveau

- Proactief te voorkomen dat waarmee is geknoeid identiteiten geschonden

Detectie van verdachte identiteiten is geen eenvoudige taak. Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en identiteiten waarmee is geknoeid methodiek voor het detecteren van afwijkingen en verdachte incidenten die mogelijk aangeven. Met deze gegevens Identity Protection genereert rapporten en waarschuwingen waarmee u kunt de gedetecteerde problemen evalueren en juiste risicobeperking of herstelacties duren.

Azure Active Directory: Identity Protection is meer dan een controle en rapportage. Ter bescherming van uw organisatie identiteiten, kunt u risico-beleidsregels die automatisch op gedetecteerde problemen reageren als een opgegeven risiconiveau is bereikt. Deze beleidsregels, naast de andere besturingselementen voorwaardelijke toegang is verstrekt door Azure Active Directory en EMS, kunnen automatisch blokkeren of initiëren adaptieve herstelacties met inbegrip van wachtwoorden opnieuw instellen en afdwingen van multi-factor authentication-server.


#### <a name="identity-protection-capabilities"></a>Mogelijkheden voor preventie van identiteit

**Detecteren van zwakke plekken en riskant accounts:**  

* Aangepaste aanbevelingen voor de algehele beveiligingsstatus markeert beveiligingslekken bieden
* Aanmelden risiconiveaus berekenen
* Gebruiker risiconiveaus berekenen


**Bezig met het onderzoeken van risico's:**

* Verzenden van meldingen voor risico 's
* Het onderzoeken van risico's met behulp van de relevante en contextuele informatie
* Biedt eenvoudige werkstromen voor het bijhouden van onderzoeken
* Eenvoudige toegang tot herstelacties zoals het wachtwoord opnieuw instellen

**Beleid voor voorwaardelijke toegang op basis van risico's:**

* Beleid voor riskante aanmeldingen beperken door aanmeldingen blokkeren of uitdagingen voor meervoudige verificatie vereisen
* Beleid voor het blokkeren of beveiligde riskant gebruikersaccounts
* Beleid voor gebruikers moeten zich registreren voor meervoudige verificatie



## <a name="identity-protection-roles"></a>Identity Protection-functies

Taakverdeling van de beheertaken voor uw implementatie Identity Protection kunt u verschillende rollen toewijzen. Azure AD Identity Protection ondersteunt 3 directory-functies:

| Rol                         | Kan doen                          | Is niet mogelijk
| :--                          | ---                                |  ---   |
| Globale beheerder         | Volledige toegang tot Identity Protection, vrijgeven Identity Protection| |
| Beveiligingsbeheerder       | Volledige toegang tot Identity Protection | Ingebouwde Identity Protection, wachtwoorden opnieuw instellen voor een gebruiker |
| Beveiligingslezer              | Alleen-lezentoegang tot Identity Protection | Ingebouwde Identity Protection, remidiate gebruikers,-beleid configureren, wachtwoorden opnieuw instellen |




Zie voor meer informatie [beheerdersrollen toewijzen in Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)



## <a name="detection"></a>Detectie

### <a name="vulnerabilities"></a>Beveiligingsproblemen

Azure Active Directory: Identity Protection analyses van uw configuratie en beveiligingsproblemen die invloed op uw gebruikers-id's hebben kunnen detecteert. Zie voor meer informatie [beveiligingsproblemen die worden gedetecteerd door Azure Active Directory: Identity Protection](active-directory-identityprotection-vulnerabilities.md).

### <a name="risk-events"></a>Risicogebeurtenissen

Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodiek voor het detecteren van verdachte acties die zijn gerelateerd aan uw gebruikers-id's. Maakt een record voor elke gedetecteerde verdachte actie van het systeem. Deze records worden ook wel bekend als risico's.  
Zie [Risicogebeurtenissen in Azure Active Directory](active-directory-identity-protection-risk-events.md) voor meer informatie.


## <a name="investigation"></a>Onderzoek
Uw reis door Identity Protection wordt doorgaans begint met het Identity Protection-dashboard.

![Herstel](./media/active-directory-identityprotection/1000.png "herstel")

Het dashboard biedt die u toegang tot:

* Rapporten, zoals **gebruikers die zijn gemarkeerd voor risico**, **bestaat de kans dat gebeurtenissen** en **beveiligingsproblemen**
* Instellingen zoals de configuratie van uw **beveiligingsbeleid**, **meldingen** en **multi-factor authentication-registratie**

Is doorgaans het beginpunt voor onderzoek, het proces is van het controleren van de activiteiten, logboeken en andere relevante informatie die betrekking hebben op een risicogebeurtenis om te bepalen of de updateherstel- of risicobeperking stappen nodig zijn, en hoe de identiteit is geknoeid en begrijpen hoe de identiteit van de verdachte is gebruikt.

U kunt verbinden, de onderzoeksactiviteiten van uw naar het [meldingen](active-directory-identityprotection-notifications.md) Azure Active Directory Protection per e-mail verzendt.

De volgende secties bieden u meer informatie en de stappen die zijn gerelateerd aan een onderzoek.  


## <a name="risky-sign-ins"></a>Riskante aanmeldingen

Azure Active Directory detecteert [risico gebeurtenistypen](active-directory-reporting-risk-events.md#risk-event-types) in realtime en offline. Elke risicogebeurtenis dat is aangetroffen voor een aanmelding van een gebruiker bijdraagt aan een logisch concept is aangeroepen riskant aanmelden. Een riskante aanmelden is een indicator voor een aanmeldingspoging die mogelijk niet uitgevoerd door de legitieme eigenaar van een gebruikersaccount.


### <a name="sign-in-risk-level"></a>Aanmelden risiconiveau

Een risiconiveau aanmelden is een indicatie (hoog, Gemiddeld of laag) van de kans op een aanmeldingspoging is niet uitgevoerd door de legitieme eigenaar van een gebruikersaccount.

### <a name="mitigating-sign-in-risk-events"></a>Beperkende aanmelden risico 's

Een beperking is een actie die de mogelijkheid van een aanvaller misbruik maakt van een verdachte identiteit of het apparaat zonder dat de identiteit of het apparaat hersteld naar een veilige status beperken. Een risicobeperking vorige aanmelden risicogebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat niet is opgelost.

U kunt aanmelden risico beveiligingsbeleid configureren riskant aanmeldingen automatisch om's te beperken. Met deze beleidsregels, u rekening houden met het risiconiveau van de gebruiker of de aanmeldingspagina riskant aanmeldingen wordt geblokkeerd of dat de gebruiker naar de multi-factor authentication uitvoeren. Deze acties kunnen voorkomen dat een aanvaller gebruikmaakt van de identiteit van een gestolen om te leiden tot beschadiging en mogelijk hebt u enige tijd voor het beveiligen van de identiteit.

### <a name="sign-in-risk-security-policy"></a>Beveiligingsbeleid Aanmelden risico
Een beleid voor aanmelden risico is een beleid voor voorwaardelijke toegang dat het risico op een specifieke aanmelden wordt geëvalueerd en oplossingen op basis van vooraf gedefinieerde voorwaarden en regels van toepassing is.

![Beleid voor aanmelden risico](./media/active-directory-identityprotection/1014.png "aanmelden risico beleid")

Azure AD Identity Protection helpt die u bij het beheren van het beperken van riskante aanmeldingen doordat u:

* Stel de gebruikers en groepen die het beleid van toepassing:

    ![Beleid voor aanmelden risico](./media/active-directory-identityprotection/1015.png "aanmelden risico beleid")
* Stel de aanmeldingspagina risico niveau drempelwaarde (laag, Gemiddeld of hoog) waarmee het beleid wordt geactiveerd:

    ![Beleid voor aanmelden risico](./media/active-directory-identityprotection/1016.png "aanmelden risico beleid")
* Stel de besturingselementen om te worden afgedwongen wanneer het beleid wordt geactiveerd:  

    ![Beleid voor aanmelden risico](./media/active-directory-identityprotection/1017.png "aanmelden risico beleid")
* De status van uw beleid switch:

    ![Registratieprocedure voor MFA](./media/active-directory-identityprotection/403.png "registratieprocedure voor MFA")
* Bekijken en evalueren van de gevolgen van een wijziging voordat u deze activeert:

    ![Beleid voor aanmelden risico](./media/active-directory-identityprotection/1018.png "aanmelden risico beleid")

#### <a name="what-you-need-to-know"></a>Wat u moet weten
U kunt een beveiligingsbeleid Aanmelden risico meervoudige authenticatie configureren:

![Beleid voor aanmelden risico](./media/active-directory-identityprotection/1017.png "aanmelden risico beleid")

Echter, uit veiligheidsoverwegingen deze instelling werkt alleen voor gebruikers die al zijn geregistreerd voor multi-factor authentication. Als de voorwaarde voor meervoudige authenticatie voor een gebruiker die nog niet is geregistreerd voor multi-factor authentication is voldaan, wordt de gebruiker is geblokkeerd.

Als het wordt aanbevolen als u wilt meervoudige verificatie vereisen voor riskante aanmeldingen, moet u:

1. Schakel de [registratiebeleid voor meervoudige verificatie](#multi-factor-authentication-registration-policy) voor de betrokken gebruikers.
2. Vereisen dat de betrokken gebruikers aanmelding in een niet-riskant sessie een MFA-registratie uitvoeren

U deze stappen uitvoert, zorgt u ervoor dat meervoudige verificatie is vereist voor een riskante aanmelden.

#### <a name="best-practices"></a>Aanbevolen procedures
U kiest een **hoge** drempelwaarde vermindert het aantal keer dat een beleid wordt geactiveerd en minimaliseert de overlast voor gebruikers.  

Echter, worden uitgesloten **laag** en **gemiddeld** aanmeldingen gemarkeerd voor risico's van het beleid, dat een aanvaller gebruikmaakt van de identiteit van een verdachte kan niet worden geblokkeerd.

Bij het instellen van het beleid

* Gebruikers die geen / multi-factor authentication-server kunnen geen uitsluiten
* Gebruikers in landen waar het inschakelen van het beleid is het niet praktisch uitsluiten (bijvoorbeeld geen toegang tot de helpdesk)
* Voorkomen dat gebruikers die waarschijnlijk voor het genereren van een groot aantal ONWAAR-positieven (ontwikkelaars, beveiligingsanalisten)
* Gebruik een **hoge** drempelwaarde tijdens de eerste beleid implementeert, of als u uitdagingen die zichtbaar zijn voor eindgebruikers minimaliseert.
* Gebruik een **laag** drempelwaarde als uw organisatie betere beveiliging vereist. Als u een **laag** drempelwaarde introduceert extra gebruiker aanmelden uitdagingen, maar een hogere beveiliging.

De aanbevolen standaardwaarde voor de meeste organisaties is het configureren van een regel voor een **gemiddeld** drempelwaarde een evenwicht tot stand tussen bruikbaarheid en veiligheid.

Het beleid voor aanmelden risico is:

* Toegepast op alle browserverkeer en aanmeldingen die moderne authenticatie gebruiken.
* Niet toegepast op toepassingen met behulp van beveiligingsprotocollen voor oudere door het WS-Trust-eindpunt op de federatieve IDP, zoals ADFS uit te schakelen.

De **Risicogebeurtenissen** pagina in de console Identity Protection worden alle gebeurtenissen:

* Dit beleid is toegepast
* U kunt bekijken van de activiteit en bepalen of de actie die geschikt of niet is

Zie voor een overzicht van de gerelateerde gebruikerservaring:

* [Herstel voor riskante aanmelden](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [Riskant aanmelden geblokkeerd](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [Aanmelden-ervaring met Azure AD Identity Protection](active-directory-identityprotection-flows.md)  

**Opent het dialoogvenster gerelateerde configuratie**:

- Op de **Azure AD Identity Protection** blade in de **configureren** sectie, klikt u op **aanmelden risico beleid**.

    ![Gebruikersbeleid ridk](./media/active-directory-identityprotection/1014.png "ridk gebruikersbeleid")



## <a name="users-flagged-for-risk"></a>Gebruikers voor wie wordt aangegeven dat ze risico lopen

Alle actieve [bestaat de kans dat gebeurtenissen](active-directory-identity-protection-risk-events.md) die zijn gedetecteerd door Azure Active Directory voor een gebruiker bijdragen aan een logisch concept is aangeroepen gebruiker risico. Een gebruiker die is gemarkeerd voor risico is een indicator voor een gebruikersaccount die mogelijk zijn aangetast.

![Gebruikers voor wie wordt aangegeven dat ze risico lopen](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a>Risiconiveau van gebruiker

Het risiconiveau van een gebruiker is een indicatie van de kans dat de identiteit van de gebruiker is ingebroken (hoog, Gemiddeld of laag). Deze wordt berekend op basis van de gebruiker risicogebeurtenissen die gekoppeld aan de identiteit van een gebruiker zijn.

De status van een risicogebeurtenis is **Active** of **gesloten**. Alleen het risico van gebeurtenissen die zijn **Active** bijdragen aan de gebruiker risico niveau berekening.

Het risiconiveau van de gebruiker wordt berekend met behulp van de volgende invoer:

* Actieve risicogebeurtenissen die invloed hebben op de gebruiker
* Risiconiveau van deze gebeurtenissen
* Hiermee wordt aangegeven of welke herstelacties zijn gemaakt

![Gebruiker risico's](./media/active-directory-identityprotection/1031.png "gebruiker risico's")

U kunt de risiconiveaus gebruiker gebruiken voor het maken van beleid voor voorwaardelijke toegang die riskant gebruikers aanmelden blokkeren of zorgen dat ze veilig hun wachtwoord wijzigen.

### <a name="closing-risk-events-manually"></a>Risico's handmatig sluiten

In de meeste gevallen duurt herstelacties zoals een beveiligd wachtwoord opnieuw instellen op automatisch risico's te sluiten. Echter, dit mogelijk niet altijd mogelijk.  
Dit is, bijvoorbeeld het geval wanneer:

* Een gebruiker met actieve risicogebeurtenissen is verwijderd
* Een onderzoek blijkt dat een risicogebeurtenis gemelde heeft zijn uitvoeren door de gebruiker legitieme

Omdat de risico's die zijn **Active** bijdragen aan de gebruiker risico berekening, moet u mogelijk handmatig een risiconiveau verlagen door risicogebeurtenissen handmatig sluiten.  
U kunt kiezen tijdens het onderzoek moet worden uitgevoerd op een van deze acties om de status van een gebeurtenis risico's te wijzigen:

![Acties](./media/active-directory-identityprotection/34.png "acties")

* **Los** - als na een risicogebeurtenis onderzoeken, u een juiste herstelactie buiten Identity Protection heeft ondernomen, en u van mening bent dat de gebeurtenis risico's nodig hebt gesloten, markeert u de gebeurtenis als opgelost. Gebeurtenissen stelt de status van de risicogebeurtenis op gesloten en de risicogebeurtenis wordt niet langer bijdragen aan de gebruiker risico's opgelost.
* **Markeren als fout-positieve** -In sommige gevallen kan u een risicogebeurtenis onderzoeken en onjuist is gemarkeerd als een riskante te detecteren. Verkleint u het aantal dergelijke instanties door de risicogebeurtenis als fout-positieve markeren. Dit helpt de machine learning-algoritmen ter verbetering van de classificatie van soortgelijke gebeurtenissen in de toekomst. De status van de fout-positieve gebeurtenissen wordt **gesloten** en ze niet langer draagt bij tot het risico van de gebruiker.
* **Negeren** : als u een herstelactie niet genomen, maar u wilt dat de risicogebeurtenis worden verwijderd uit de actieve lijst kunt u een risicogebeurtenis negeren markeren en de gebeurtenisstatus van de wordt gesloten. Genegeerd gebeurtenissen bijdragen niet tot het risico van de gebruiker. Deze optie dient alleen ongebruikelijke omstandigheden worden gebruikt.
* **Opnieuw activeren** -gebeurtenissen die handmatig zijn gesloten risico (door te kiezen **los**, **fout-positief**, of **negeren**) kan opnieuw worden geactiveerd, instellen van de gebeurtenisstatus terug naar **Active**. Opnieuw geactiveerde risicogebeurtenissen bijdragen aan de gebruiker risico niveau berekening. Risico's gesloten via herstel (zoals een beveiligd wachtwoord opnieuw instellen) kunnen niet opnieuw worden geactiveerd.

**Opent het dialoogvenster gerelateerde configuratie**:

1. Op de **Azure AD Identity Protection** blade onder **onderzoeken**, klikt u op **bestaat de kans dat gebeurtenissen**.

    ![Handmatige wachtwoordherstel](./media/active-directory-identityprotection/1002.png "handmatige wachtwoord opnieuw instellen")
2. In de **bestaat de kans dat gebeurtenissen** lijst, klikt u op een risico.

    ![Handmatige wachtwoordherstel](./media/active-directory-identityprotection/1003.png "handmatige wachtwoord opnieuw instellen")
3. Met de rechtermuisknop op een gebruiker op de blade risico.

    ![Handmatige wachtwoordherstel](./media/active-directory-identityprotection/1004.png "handmatige wachtwoord opnieuw instellen")

### <a name="closing-all-risk-events-for-a-user-manually"></a>Alle gebeurtenissen van de risico's voor een gebruiker sluiten handmatig
In plaats van handmatig sluiten afzonderlijk risico's voor een gebruiker, biedt Azure Active Directory: Identity Protection u ook een methode voor het sluiten van alle gebeurtenissen voor een gebruiker met één klik.

![Acties](./media/active-directory-identityprotection/2222.png "acties")

Wanneer u klikt op **sluiten alle gebeurtenissen**, alle gebeurtenissen zijn gesloten en de betrokken gebruiker is niet meer risico.

### <a name="remediating-user-risk-events"></a>Beveiligingsstatus gebruiker risico 's

Een herstel is een actie voor het beveiligen van een identiteit of een apparaat dat eerder verdachte of bekend is mogelijk onveilig. Een herstelactie herstelt u de identiteit of het apparaat naar een veilige status en wordt omgezet vorige risico's die zijn gekoppeld aan de identiteit of het apparaat.

Als u wilt herstellen, gebruiker risicogebeurtenissen, kunt u:

* Uitvoeren van een beveiligd wachtwoord opnieuw instellen op gebruiker risicogebeurtenissen handmatig herstellen
* Een gebruiker risico security-beleid te beperken of gebruiker risicogebeurtenissen automatisch herstellen configureren
* Het geïnfecteerde apparaat herstellen met installatiekopie  

#### <a name="manual-secure-password-reset"></a>Handmatige beveiligd wachtwoord opnieuw instellen
Een beveiligd wachtwoord opnieuw instellen is een effectieve herstel voor veel risicogebeurtenissen en wanneer uitgevoerd, sluit u deze risicogebeurtenissen automatisch en het risiconiveau van de gebruiker opnieuw worden berekend. Het Identity Protection-dashboard kunt u een wachtwoord opnieuw instellen voor een riskante gebruiker te initiëren.

Het gerelateerde dialoogvenster biedt twee verschillende methoden om uw wachtwoord opnieuw instellen:

**Wachtwoord opnieuw instellen** : Selecteer **dat hun wachtwoord opnieuw instellen van de gebruiker** zodat de gebruiker zelf herstellen als de gebruiker is geregistreerd voor multi-factor authentication. Tijdens de gebruiker van de volgende keer aanmelden, wordt de gebruiker worden vereist voor het oplossen van een challenge multi-factor authentication-server is en vervolgens gedwongen het wachtwoord te wijzigen. Deze optie niet beschikbaar als het gebruikersaccount nog niet is geregistreerd multi-factor authentication-server.

**Tijdelijke wachtwoord** : Selecteer **genereren van een tijdelijk wachtwoord** onmiddellijk het bestaande wachtwoord ongeldig te maken en het maken van een nieuw tijdelijk wachtwoord voor de gebruiker. Nieuw tijdelijk wachtwoord verzenden naar een alternatief e-mailadres voor de gebruiker of de manager van de gebruiker. Omdat het wachtwoord tijdelijk is, wordt de gebruiker gevraagd het wachtwoord van de aanmeldingspagina te wijzigen.

![Beleid](./media/active-directory-identityprotection/1005.png "beleid")

**Opent het dialoogvenster gerelateerde configuratie**:

1. Op de **Azure AD Identity Protection** blade, klikt u op **gebruikers die zijn gemarkeerd voor risico**.

    ![Handmatige wachtwoordherstel](./media/active-directory-identityprotection/1006.png "handmatige wachtwoord opnieuw instellen")
2. Selecteer een gebruiker met ten minste één risicogebeurtenissen uit de lijst met gebruikers.

    ![Handmatige wachtwoordherstel](./media/active-directory-identityprotection/1007.png "handmatige wachtwoord opnieuw instellen")
3. Klik op de blade gebruiker **wachtwoord opnieuw instellen**.

    ![Handmatige wachtwoordherstel](./media/active-directory-identityprotection/1008.png "handmatige wachtwoord opnieuw instellen")

### <a name="user-risk-security-policy"></a>Gebruiker risico beveiligingsbeleid
Een beveiligingsbeleid voor gebruiker risico is een beleid voor voorwaardelijke toegang dat het risiconiveau aan een specifieke gebruiker wordt geëvalueerd en herstel en risicobeperking op basis van vooraf gedefinieerde voorwaarden en regels maatregelen.

![Gebruikersbeleid ridk](./media/active-directory-identityprotection/1009.png "ridk gebruikersbeleid")

Azure AD Identity Protection helpt bij het beheren van de risicobeperking en het doorvoeren van gebruikers die zijn gemarkeerd voor risico doordat u:

* Stel de gebruikers en groepen die het beleid van toepassing:

    ![Gebruikersbeleid ridk](./media/active-directory-identityprotection/1010.png "ridk gebruikersbeleid")
* Stel de gebruiker risico niveau drempelwaarde (laag, Gemiddeld of hoog) waarmee het beleid wordt geactiveerd:

    ![Gebruikersbeleid ridk](./media/active-directory-identityprotection/1011.png "ridk gebruikersbeleid")
* Stel de besturingselementen om te worden afgedwongen wanneer het beleid wordt geactiveerd:

    ![Gebruikersbeleid ridk](./media/active-directory-identityprotection/1012.png "ridk gebruikersbeleid")
* De status van uw beleid switch:

    ![Gebruikersbeleid ridk](./media/active-directory-identityprotection/403.png "registratieprocedure voor MFA")
* Bekijken en evalueren van de gevolgen van een wijziging voordat u deze activeert:

    ![Gebruikersbeleid ridk](./media/active-directory-identityprotection/1013.png "ridk gebruikersbeleid")

U kiest een **hoge** drempelwaarde vermindert het aantal keer dat een beleid wordt geactiveerd en minimaliseert de overlast voor gebruikers.
Echter, worden uitgesloten **laag** en **gemiddeld** gebruikers die zijn gemarkeerd voor risico's van het beleid, die mogelijk niet beveiligd identiteiten of apparaten die eerder zijn verdachte of bekend is mogelijk onveilig.

Bij het instellen van het beleid

* Voorkomen dat gebruikers die waarschijnlijk voor het genereren van een groot aantal ONWAAR-positieven (ontwikkelaars, beveiligingsanalisten)
* Gebruikers in landen waar het inschakelen van het beleid is het niet praktisch uitsluiten (bijvoorbeeld geen toegang tot de helpdesk)
* Gebruik een **hoge** drempelwaarde tijdens de eerste beleid implementeert, of als u uitdagingen die zichtbaar zijn voor eindgebruikers minimaliseert.
* Gebruik een **laag** drempelwaarde als uw organisatie betere beveiliging vereist. Als u een **laag** drempelwaarde introduceert extra gebruiker aanmelden uitdagingen, maar een hogere beveiliging.

De aanbevolen standaardwaarde voor de meeste organisaties is het configureren van een regel voor een **gemiddeld** drempelwaarde een evenwicht tot stand tussen bruikbaarheid en veiligheid.

Zie voor een overzicht van de gerelateerde gebruikerservaring:

* [Account recovery stroom geknoeid](active-directory-identityprotection-flows.md#compromised-account-recovery).  
* [Account geblokkeerd stroom geknoeid](active-directory-identityprotection-flows.md#compromised-account-blocked).  

**Opent het dialoogvenster gerelateerde configuratie**:

- Op de **Azure AD Identity Protection** blade in de **configureren** sectie, klikt u op **risico gebruikersbeleid**.

    ![Gebruikersbeleid ridk](./media/active-directory-identityprotection/1009.png "ridk gebruikersbeleid")

### <a name="mitigating-user-risk-events"></a>Beperkende gebruiker risico 's
Beheerders kunnen een beveiligingsbeleid voor gebruiker risico voorkomen dat gebruikers bij het aanmelden, afhankelijk van het risiconiveau instellen.

Een aanmeldingspagina blokkeren:

* Voorkomt dat het genereren van de nieuwe gebruiker risico's voor de betrokken gebruiker
* Hiermee kunnen beheerders handmatig herstellen van de risicogebeurtenissen die invloed hebben op de identiteit van de gebruiker en herstellen naar een beveiligde status



## <a name="multi-factor-authentication-registration-policy"></a>Registratiebeleid voor meervoudige verificatie
Azure multi-factor authentication-server is een methode om te controleren wie u bent die het gebruik van meer dan alleen een gebruikersnaam en wachtwoord vereist. Het biedt een tweede beveiligingslaag op gebruikersaanmeldingen en transacties.  
We raden u aan Azure multi-factor authentication-server voor de gebruikersaanmeldingen omdat deze:

* Biedt geavanceerde verificatie met een bereik van eenvoudige verificatie-opties
* Speelt een belangrijke rol bij het voorbereiden van uw organisatie te beveiligen en herstellen van account compromissen

![Gebruikersbeleid ridk](./media/active-directory-identityprotection/1019.png "ridk gebruikersbeleid")

Zie voor meer informatie [wat is Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

Azure AD Identity Protection helpt u de uitrollen van multi-factor authentication-registratie beheren door een beleid waarmee u kunt configureren:

* Stel de gebruikers en groepen die het beleid van toepassing:

    ![MFA-beleid](./media/active-directory-identityprotection/1020.png "MFA-beleid")
* Stel de instellingen moeten worden afgedwongen wanneer het beleid wordt geactiveerd::  

    ![MFA-beleid](./media/active-directory-identityprotection/1021.png "MFA-beleid")
* De status van uw beleid switch:

    ![MFA-beleid](./media/active-directory-identityprotection/403.png "MFA-beleid")
* De registratiestatus van de huidige weergave:

    ![MFA-beleid](./media/active-directory-identityprotection/1022.png "MFA-beleid")

Zie voor een overzicht van de gerelateerde gebruikerservaring:

* [Registratie voor meervoudige authenticatiestroom](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  
* [Aanmelden-ervaringen met Azure AD Identity Protection](active-directory-identityprotection-flows.md).  

**Opent het dialoogvenster gerelateerde configuratie**:

- Op de **Azure AD Identity Protection** blade in de **configureren** sectie, klikt u op **multi-factor authentication-registratie**.

    ![MFA-beleid](./media/active-directory-identityprotection/1019.png "MFA-beleid")

## <a name="next-steps"></a>Volgende stappen
* [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [Inschakelen van beveiliging voor Azure Active Directory-identiteit](active-directory-identityprotection-enable.md)

* [Beveiligingsproblemen die worden gedetecteerd door Azure Active Directory: Identity Protection](active-directory-identityprotection-vulnerabilities.md)

* [Azure Active Directory-risicogebeurtenissen](active-directory-identity-protection-risk-events.md)

* [Azure Active Directory: Identity Protection-meldingen](active-directory-identityprotection-notifications.md)

* [Playbook voor Azure Active Directory: Identity Protection](active-directory-identityprotection-playbook.md)

* [Azure Active Directory: Identity Protection verklarende woordenlijst](active-directory-identityprotection-glossary.md)

* [Aanmelden-ervaring met Azure AD Identity Protection](active-directory-identityprotection-flows.md)

* [Azure Active Directory: Identity Protection - blokkering opheffen van gebruikers](active-directory-identityprotection-unblock-howto.md)

* [Aan de slag met Azure Active Directory: Identity Protection en Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)

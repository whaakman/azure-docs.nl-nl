---
title: Azure Active Directory Identity Protection | Microsoft Docs
description: Meer informatie over hoe Azure AD Identity Protection kunt u de mogelijkheid van een aanvaller misbruik te maken van een apparaat of aangetaste identiteit en beveiliging van een identiteit of een apparaat dat eerder is verdacht of bekend is dat inbreuk worden gepleegd beperken.
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
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 06e3a596b60bf96319071fff68b0bf1655869559
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003794"
---
# <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection

Azure Active Directory Identity Protection is een functie van de Azure AD Premium P2-editie waarmee u kunt:

- Detecteren van mogelijke beveiligingsproblemen die betrekking hebben op de identiteiten van uw organisatie

- Automatische antwoorden op gedetecteerde verdachte activiteit die betrekking op de identiteiten van uw organisatie hebben configureren  

- Verdachte incidenten onderzoeken en onderneem gepaste actie op te lossen   


## <a name="getting-started"></a>Aan de slag

Cloud-gebaseerde identiteiten is voor meer dan tien jaar beveiligd door Microsoft. Met Azure Active Directory Identity Protection kunt in uw omgeving, u de dezelfde systemen ter bescherming van die Microsoft gebruikt voor het beveiligen van identiteiten.

De meeste van de beveiliging terugdringen plaatsvinden als aanvallers toegang krijgen tot een omgeving door het stelen van de identiteit van een gebruiker. In de afgelopen jaren, zijn aanvallers steeds meer in gebruik te maken van inbreuken op de derde partij en het gebruik van geavanceerde phishing-aanvallen van kracht geworden. Als een aanvaller toegang heeft om met lage bevoegdheden gebruikersaccounts nog, is het relatief gemakkelijk voor hen toegang te krijgen tot belangrijke bedrijfsbronnen via een laterale verplaatsing.

Als dit moet u naar:

- Beveiligen van alle identiteiten, ongeacht hun niveau van bevoegdheden

- Proactief te voorkomen dat verdachte identiteiten worden misbruikt

Verdachte identiteiten detecteren, is geen eenvoudige taak. Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodieken voor het detecteren van afwijkingen en verdachte incidenten die duiden op mogelijk verdachte identiteiten. Met behulp van deze gegevens genereert Identity Protection rapporten en waarschuwingen waarmee u kunt de gedetecteerde problemen evalueren en juiste risicobeperking of herstelacties.

Azure Active Directory Identity Protection is meer dan een controle- en rapportagetool. Ter bescherming van identiteiten in uw organisatie, kunt u risico's gebaseerd beleid die automatisch op gedetecteerde problemen reageren wanneer een opgegeven risiconiveau is bereikt. Deze beleidsregels, naast andere besturingselementen voor voorwaardelijke toegang is geleverd door Azure Active Directory en EMS, kunnen automatisch blokkeren of adaptieve herstelacties met inbegrip van wachtwoorden en meervoudige verificatie afdwingen initiëren.


#### <a name="identity-protection-capabilities"></a>Mogelijkheden voor identiteits-beveiliging

**Detecteren van beveiligingsproblemen en riskante accounts:**  

* Bieden van aangepaste aanbevelingen voor het totale beveiligingspostuur verbeteren door het markeren van beveiligingsproblemen
* Berekenen van de aanmelding risiconiveaus
* Berekenen van de gebruiker risiconiveaus


**Risicogebeurtenissen onderzoeken:**

* Verzenden van meldingen voor risicogebeurtenissen
* Het onderzoeken van risico's met behulp van relevante en contextuele informatie
* Biedt eenvoudige werkstromen voor het volgen van onderzoek
* Eenvoudige toegang tot herstelacties, zoals wachtwoord opnieuw instellen

**Beleid voor voorwaardelijke toegang op basis van risico's:**

* Beleid voor riskante aanmeldingen beperken door het blokkeren van aanmeldingen of het vereisen van multi-factor authentication-uitdagingen
* Beleid op blokkeren of accounts voor beveiligde riskante gebruikers
* Beleid om te vereisen dat gebruikers zich registreren voor meervoudige verificatie



## <a name="identity-protection-roles"></a>Identity Protection-functies

Om taken te verdelen de beheeractiviteiten rond uw Identity Protection-implementatie, kunt u verschillende rollen toewijzen. Azure AD Identity Protection ondersteunt 3 directory-rollen:

| Rol                         | Kan doen                          | Niet mogelijk is
| :--                          | ---                                |  ---   |
| Globale beheerder         | Volledige toegang tot Identity Protection, onboarding Identity Protection| |
| Beveiligingsbeheerder       | Volledige toegang tot Identity Protection | Onboarding Identity Protection, wachtwoorden opnieuw instellen voor een gebruiker |
| Beveiligingslezer              | Alleen-lezen toegang tot Identity Protection | Onboarding Identity Protection, remidiate gebruikers,-beleid configureren, wachtwoorden opnieuw instellen |




Zie voor meer informatie, [beheerdersrollen toewijzen in Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)



## <a name="detection"></a>Detectie

### <a name="vulnerabilities"></a>Beveiligingsproblemen

Azure Active Directory Identity Protection analyseert uw configuratie en beveiligingsproblemen die invloed op van uw gebruikers-id's hebben kunnen detecteert. Zie voor meer informatie, [beveiligingsproblemen die worden gedetecteerd door Azure Active Directory Identity Protection](vulnerabilities.md).

### <a name="risk-events"></a>Risicogebeurtenissen

Azure Active Directory maakt gebruik van geavanceerde machine learning-algoritmen en methodieken voor het detecteren van verdachte activiteit die betrekking op de identiteiten van uw gebruikers hebben. Maakt een record voor elke gedetecteerde verdachte actie van het systeem. Deze records worden ook wel bekend als risicogebeurtenissen.  
Zie [Risicogebeurtenissen in Azure Active Directory](../active-directory-identity-protection-risk-events.md) voor meer informatie.


## <a name="investigation"></a>Onderzoek
Uw reis via Identity Protection wordt doorgaans begint met het dashboard Identity Protection.

![Herstel](./media/overview/1000.png "herstel")

Het dashboard biedt u toegang hebt tot:

* Rapporten, zoals **gebruikers die zijn gemarkeerd voor risico's**, **Risicogebeurtenissen** en **beveiligingsproblemen**
* Instellingen zoals de configuratie van uw **beveiligingsbeleid**, **meldingen** en **registratie voor meervoudige verificatie**

Het is doorgaans het startpunt voor onderzoek, is het proces van het controleren van de activiteiten, logboeken en andere relevante informatie met betrekking tot een risicogebeurtenis om te bepalen of de updateherstel- of risicobeperking stappen nodig zijn, en hoe de identiteit is aangetast en te begrijpen hoe de identiteit van de aangetaste is gebruikt.

U kunt uw onderzoeksactiviteiten om te verbinden, de [meldingen](notifications.md) Azure Active Directory Protection per e-mail verzendt.

De volgende secties vindt u met meer informatie en de stappen die betrekking op een onderzoek hebben.  


## <a name="risky-sign-ins"></a>Riskante aanmeldingen

Azure Active Directory detecteert [risicogebeurtenistypen](../reports-monitoring/concept-risk-events.md#risk-event-types) in realtime en offline. Elke risicogebeurtenis die is gedetecteerd voor een aanmelding van een gebruiker draagt bij aan een logisch concept is met de naam riskante aanmelding. Een riskante aanmelding is een indicator van een aanmeldingspoging die mogelijk niet uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount.


### <a name="sign-in-risk-level"></a>Niveau van aanmeldingsrisico

Een niveau voor aanmeldingsrisico wordt aangegeven (hoog, Gemiddeld of laag) van de kans dat een poging tot aanmelden niet is uitgevoerd door de rechtmatige eigenaar van een gebruikersaccount.

### <a name="mitigating-sign-in-risk-events"></a>Beperkende aanmeldingsrisico gebeurtenissen

Een beperking is een actie om te beperken van de mogelijkheid van een aanvaller om misbruik te maken van een apparaat of aangetaste identiteit zonder dat de identiteit of het apparaat terugzet op een veilige status heeft. Een vorige aanmeldingsrisico gebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat niet wordt opgelost.

Als u wilt riskante aanmeldingen automatisch oplossen, kunt u aanmeldingsrisico beveiligingsbeleid configureren. Met behulp van deze beleidsregels, overwegen u het risiconiveau van de gebruiker of de aanmelding voor het blokkeren van riskante aanmeldingen of vereisen dat de gebruiker naar de multi-factor authentication uitvoeren. Deze acties kunnen voorkomen dat een aanvaller misbruik kan maken van de identiteit van een gestolen om te leiden tot beschadiging en mogelijk hebt u enige tijd voor het beveiligen van de identiteit.

### <a name="sign-in-risk-security-policy"></a>Beveiligingsbeleid voor aanmeldingsrisico
Een beleid voor aanmeldingsrisico is een beleid voor voorwaardelijke toegang die wordt geëvalueerd als de risico's voor een specifieke aanmelding en oplossingen op basis van vooraf gedefinieerde voorwaarden en regels van toepassing is.

![Beleid voor aanmeldingsrisico](./media/overview/1014.png "aanmelden beleid voor gebruikersrisico's")

Azure AD Identity Protection helpt die u bij het beheren van het beperken van riskante aanmeldingen doordat u:

* Stel de gebruikers en groepen die het beleid van toepassing op:

    ![Beleid voor aanmeldingsrisico](./media/overview/1015.png "aanmelden beleid voor gebruikersrisico's")
* Stel de aanmeldingsrisico niveau drempelwaarde (laag, Gemiddeld of hoog) waarmee het beleid wordt geactiveerd:

    ![Beleid voor aanmeldingsrisico](./media/overview/1016.png "aanmelden beleid voor gebruikersrisico's")
* Stel de besturingselementen moeten worden afgedwongen wanneer het beleid wordt geactiveerd:  

    ![Beleid voor aanmeldingsrisico](./media/overview/1017.png "aanmelden beleid voor gebruikersrisico's")
* De status van uw beleid switch:

    ![MFA-registratie](./media/overview/403.png "MFA-registratie")
* Bekijken en evalueren van de gevolgen van een wijziging voordat u deze activeert:

    ![Beleid voor aanmeldingsrisico](./media/overview/1018.png "aanmelden beleid voor gebruikersrisico's")

#### <a name="what-you-need-to-know"></a>Wat u moet weten
U kunt een beveiligingsbeleid aanmeldingsrisico om te vereisen dat multi-factor authentication configureren:

![Beleid voor aanmeldingsrisico](./media/overview/1017.png "aanmelden beleid voor gebruikersrisico's")

Echter, uit veiligheidsoverwegingen, deze instelling werkt alleen voor gebruikers die al zijn geregistreerd voor meervoudige verificatie. Als de voorwaarde voor meervoudige verificatie vereisen voor een gebruiker die nog niet is geregistreerd voor multi-factor authentication is voldaan, wordt de gebruiker is geblokkeerd.

Als een best practice, als u wilt meervoudige verificatie vereisen voor riskante aanmeldingen, dient u eerst:

1. Schakel de [registratiebeleid voor meervoudige verificatie](#multi-factor-authentication-registration-policy) voor de betreffende gebruikers.
2. Vereisen dat de betrokken gebruikers om aan te melden in een niet-riskante-sessie met het uitvoeren van een MFA-registratie

Deze stappen uit te voeren, zorgt u ervoor dat meervoudige verificatie is vereist voor een riskante aanmelding.

#### <a name="best-practices"></a>Aanbevolen procedures
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

    ![Beleid voor gebruikersrisico's](./media/overview/1014.png "beleid voor gebruikersrisico's")



## <a name="users-flagged-for-risk"></a>Gebruikers voor wie wordt aangegeven dat ze risico lopen

Alle actieve [risicogebeurtenissen](../reports-monitoring/concept-risk-events.md) die zijn gedetecteerd door Azure Active Directory voor een gebruiker bijdragen aan een logisch concept is gebruikersrisico genoemd. Een gebruiker die is gemarkeerd voor risico's is een indicator van een gebruikersaccount dat mogelijk is aangetast.

![Gebruikers voor wie wordt aangegeven dat ze risico lopen](./media/overview/1200.png)


### <a name="user-risk-level"></a>Niveau van gebruikersrisico

Een risiconiveau van de gebruiker wordt aangegeven (hoog, Gemiddeld of laag) van de kans dat de identiteit van de gebruiker is aangetast. Dit wordt berekend op basis van de gebruiker risicogebeurtenissen die gekoppeld aan de identiteit van een gebruiker zijn.

De status van een risicogebeurtenis is **Active** of **gesloten**. Alleen risicogebeurtenissen die zijn **Active** bijdragen aan de berekening van risico-niveau voor de gebruiker.

Het risiconiveau van de gebruiker wordt berekend met behulp van de invoer van de volgende:

* Actieve risicogebeurtenissen die invloed hebben op de gebruiker
* Risiconiveau van deze gebeurtenissen
* Of welke herstelacties zijn getroffen

![Gebruiker risico's](./media/overview/1031.png "gebruiker risico's")

U kunt de risiconiveaus van de gebruiker gebruiken voor het maken van beleid voor voorwaardelijke toegang die riskante gebruikers aanmelden blokkeren of zorgen dat ze veilig hun wachtwoord wijzigen.

### <a name="closing-risk-events-manually"></a>Risicogebeurtenissen handmatig sluiten

In de meeste gevallen neemt u herstelacties zoals het opnieuw instellen van een beveiligd wachtwoord om risicogebeurtenissen automatisch te sluiten. Dit is echter niet altijd mogelijk.  
Dit is, bijvoorbeeld het geval wanneer:

* Een gebruiker met actieve risicogebeurtenissen zijn verwijderd
* Uit onderzoek blijkt dat een risicogebeurtenis is uitgevoerd door een legitieme gebruiker

Omdat risicogebeurtenissen die **Actief** zijn, bijdragen aan de berekening van het gebruikersrisico, moet u een risiconiveau mogelijk handmatig verlagen door risicogebeurtenissen handmatig te sluiten.  
Gedurende het onderzoek kunt u een van deze acties kiezen om de status van een risicogebeurtenis te wijzigen:

![Acties](./media/overview/34.png "acties")

* **Oplossen** - als u na onderzoek van een risicogebeurtenis een juiste herstelactie buiten Identity Protection hebt ondernomen en van mening bent dat de risicogebeurtenis kan worden gesloten, markeert u de gebeurtenis als Opgelost. Een opgeloste risicogebeurtenis krijgt de status Gesloten. Bovendien draagt de risicogebeurtenis niet meer bij aan het gebruikersrisico.
* **Markeren als fout-positieve** -In sommige gevallen kan u een risicogebeurtenis onderzoeken en ontdek ten onrechte is gemarkeerd als een riskant. U kunt het nummer van deze instanties verminderen door de risicogebeurtenis als fout-positieve markeren. Hiermee kunt u de machine learning-algoritmen voor de classificatie van soortgelijke gebeurtenissen in de toekomst te verbeteren. De status van de fout-positieve gebeurtenissen wordt **gesloten** en ze niet meer dragen bij aan gebruikersrisico.
* **Negeren** :  als u geen herstelactie heb uitgevoerd, maar wilt dat de risicogebeurtenis wordt verwijderd uit de actieve lijst, kunt u een risicogebeurtenis markeren als Negeren. De status van de gebeurtenis wordt dan Gesloten. Genegeerde gebeurtenissen dragen niet bij aan het gebruikersrisico. Deze optie dient alleen in ongebruikelijke omstandigheden te worden gebruikt.
* **Opnieuw activeren** -gebeurtenissen die handmatig zijn gesloten (door bijvoorbeeld te kiezen voor **Oplossen**,  **Vals positief** of **Negeren**), kunnen opnieuw worden geactiveerd. Dit kan door de status van de gebeurtenis terug te zetten naar **Actief**. Opnieuw geactiveerde risicogebeurtenissen dragen bij aan de berekening van het gebruikersrisiconiveau. Risico's die gesloten zijn via herstelacties (zoals het opnieuw instellen van een beveiligd wachtwoord), kunnen niet opnieuw worden geactiveerd.


**Opent het dialoogvenster gerelateerde configuratie**:

1. Op de **Azure AD Identity Protection** blade onder **onderzoeken**, klikt u op **Risicogebeurtenissen**.

    ![Handmatige wachtwoordherstel](./media/overview/1002.png "handmatige wachtwoord opnieuw instellen")
2. In de **Risicogebeurtenissen** lijst, klikt u op een risico.

    ![Handmatige wachtwoordherstel](./media/overview/1003.png "handmatige wachtwoord opnieuw instellen")
3. Met de rechtermuisknop op een gebruiker op de blade risico.

    ![Handmatige wachtwoordherstel](./media/overview/1004.png "handmatige wachtwoord opnieuw instellen")

### <a name="closing-all-risk-events-for-a-user-manually"></a>Alle risicogebeurtenissen voor een gebruiker sluiten handmatig
In plaats van handmatig sluiten afzonderlijk risicogebeurtenissen voor een gebruiker, biedt Azure Active Directory Identity Protection u ook een methode voor het sluiten van alle gebeurtenissen voor een gebruiker met één klik.

![Acties](./media/overview/2222.png "acties")

Wanneer u klikt op **alle gebeurtenissen sluiten**, alle gebeurtenissen worden gesloten en de betrokken gebruiker is niet meer risico.

### <a name="remediating-user-risk-events"></a>Bescherm risicogebeurtenissen voor gebruiker

Een herstel is een actie voor het beveiligen van een identiteit of een apparaat dat eerder is verdacht of bekend is. Een herstelactie wordt de identiteit of het apparaat hersteld naar een veilige status en wordt omgezet vorige risicogebeurtenissen die zijn gekoppeld aan de identiteit of het apparaat.

Als u wilt herstellen risicogebeurtenissen gebruiker, kunt u het volgende doen:

* Uitvoeren van een veilig wachtwoord opnieuw instellen op gebruiker risicogebeurtenissen handmatig herstellen
* Configureren van een security beleid voor gebruikersrisico's om te beperken of gebruiker risicogebeurtenissen automatisch herstellen
* De geïnfecteerd apparaat herstellen met installatiekopie  

#### <a name="manual-secure-password-reset"></a>Handmatige beveiligd wachtwoord opnieuw instellen
Een veilig wachtwoord opnieuw instellen is van een doeltreffende herstel voor veel risicogebeurtenissen en wanneer uitgevoerd, sluit u deze risicogebeurtenissen automatisch en het risiconiveau van de gebruiker opnieuw worden berekend. U kunt het dashboard Identity Protection gebruiken om te starten van een wachtwoord opnieuw instellen voor een riskante gebruiker.

De gerelateerde dialoogvenster biedt twee verschillende methoden om een wachtwoord opnieuw instellen:

**Wachtwoord opnieuw instellen** : Selecteer **vereisen dat de gebruiker het wachtwoord opnieuw instelt** zodat de gebruiker zelf herstellen als de gebruiker is geregistreerd voor meervoudige verificatie. Tijdens de gebruiker de volgende aanmelding wordt worden de gebruiker voor het oplossen van een uitdaging meervoudige verificatie is vereist en vervolgens gedwongen om het wachtwoord te wijzigen. Deze optie niet beschikbaar als het gebruikersaccount dat nog niet is geregistreerd met meervoudige verificatie.

**Tijdelijk wachtwoord** : Selecteer **een tijdelijk wachtwoord genereren** onmiddellijk het bestaande wachtwoord ongeldig te maken en het maken van een nieuw tijdelijk wachtwoord voor de gebruiker. Het tijdelijke wachtwoord verzenden naar een alternatieve e-mailadres voor de gebruiker of de manager van de gebruiker. Omdat het wachtwoord tijdelijk is, wordt de gebruiker gevraagd het wachtwoord aanmelden bij een te wijzigen.

![Beleid](./media/overview/1005.png "beleid")

**Opent het dialoogvenster gerelateerde configuratie**:

1. Op de **Azure AD Identity Protection** blade, klikt u op **gebruikers die zijn gemarkeerd voor risico's**.

    ![Handmatige wachtwoordherstel](./media/overview/1006.png "handmatige wachtwoord opnieuw instellen")
2. Selecteer een gebruiker met ten minste één risicogebeurtenissen in de lijst van gebruikers.

    ![Handmatige wachtwoordherstel](./media/overview/1007.png "handmatige wachtwoord opnieuw instellen")
3. Klik op de blade van de gebruiker, **wachtwoord opnieuw instellen**.

    ![Handmatige wachtwoordherstel](./media/overview/1008.png "handmatige wachtwoord opnieuw instellen")

### <a name="user-risk-security-policy"></a>Beleid voor gebruikersrisico's security
Een beleid voor gebruikersrisico's security is een beleid voor voorwaardelijke toegang die wordt geëvalueerd als het risiconiveau dat aan een specifieke gebruiker en het herstel en risicobeperking op basis van vooraf gedefinieerde voorwaarden en regels maatregelen.

![Beleid voor gebruikersrisico's](./media/overview/1009.png "beleid voor gebruikersrisico's")

Azure AD Identity Protection kunt u de risicobeperking en herstel van gebruikers die zijn gemarkeerd voor risico's door te beheren:

* Stel de gebruikers en groepen die het beleid van toepassing op:

    ![Beleid voor gebruikersrisico's](./media/overview/1010.png "beleid voor gebruikersrisico's")
* Drempelwaarde in te stellen de gebruiker risico niveau (laag, Gemiddeld of hoog) waarmee het beleid wordt geactiveerd:

    ![Beleid voor gebruikersrisico's](./media/overview/1011.png "beleid voor gebruikersrisico's")
* Stel de besturingselementen moeten worden afgedwongen wanneer het beleid wordt geactiveerd:

    ![Beleid voor gebruikersrisico's](./media/overview/1012.png "beleid voor gebruikersrisico's")
* De status van uw beleid switch:

    ![Beleid voor gebruikersrisico's](./media/overview/403.png "MFA-registratie")
* Bekijken en evalueren van de gevolgen van een wijziging voordat u deze activeert:

    ![Beleid voor gebruikersrisico's](./media/overview/1013.png "beleid voor gebruikersrisico's")

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

    ![Beleid voor gebruikersrisico's](./media/overview/1009.png "beleid voor gebruikersrisico's")

### <a name="mitigating-user-risk-events"></a>Beperkende risicogebeurtenissen voor gebruiker
Beheerders kunnen een beleid voor gebruikersrisico's security instellen om te voorkomen dat gebruikers bij het aanmelden, afhankelijk van het risiconiveau.

Blokkering van een aanmeldingen:

* Hiermee voorkomt u dat de generatie van de nieuwe gebruiker-risicogebeurtenissen voor de betrokken gebruiker
* Hiermee kunnen beheerders handmatig de risicogebeurtenissen die betrekking hebben op de identiteit van de gebruiker herstellen en terug te zetten naar een veilige status



## <a name="multi-factor-authentication-registration-policy"></a>Registratiebeleid voor meervoudige verificatie
Verificatie met meerdere factoren van Azure is een methode om te controleren wie u bent die het gebruik van meer dan alleen een gebruikersnaam en wachtwoord vereist. Het biedt een tweede beveiligingslaag gebruikersaanmeldingen en transacties.  
Het is raadzaam dat u Azure multi-factor authentication voor gebruikersaanmeldingen, vereisen omdat deze:

* Biedt een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties
* Speelt een belangrijke rol bij het voorbereiden van uw organisatie beveiligen en herstellen van account compromissen

![Beleid voor gebruikersrisico's](./media/overview/1019.png "beleid voor gebruikersrisico's")

Zie voor meer informatie, [wat is Azure multi-factor Authentication?](../authentication/multi-factor-authentication.md)

Azure AD Identity Protection kunt u het uitrollen van de registratie voor meervoudige verificatie beheren door een beleid waarmee u kunt configureren:

* Stel de gebruikers en groepen die het beleid van toepassing op:

    ![MFA-beleid](./media/overview/1020.png "MFA-beleid")
* Stel de besturingselementen moeten worden afgedwongen wanneer het beleid wordt geactiveerd:  

    ![MFA-beleid](./media/overview/1021.png "MFA-beleid")
* De status van uw beleid switch:

    ![MFA-beleid](./media/overview/403.png "MFA-beleid")
* De huidige registratiestatus weergeven:

    ![MFA-beleid](./media/overview/1022.png "MFA-beleid")

Zie voor een overzicht van de gebruikerservaring:

* [Multi-factor authentication-registratie stroom](flows.md#multi-factor-authentication-registration).  
* [Aanmelden-ervaringen met Azure AD Identity Protection](flows.md).  

**Opent het dialoogvenster gerelateerde configuratie**:

- Op de **Azure AD Identity Protection** blade in de **configureren** sectie, klikt u op **multi-factor authentication-registratie**.

    ![MFA-beleid](./media/overview/1019.png "MFA-beleid")

## <a name="next-steps"></a>Volgende stappen
* [Channel 9: Azure AD en Identity weergeven: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [Azure Active Directory Identity Protection inschakelen](enable.md)

* [Beveiligingsproblemen die worden gedetecteerd door Azure Active Directory Identity Protection](vulnerabilities.md)

* [Azure Active Directory-risicogebeurtenissen](../active-directory-identity-protection-risk-events.md)

* [Azure Active Directory Identity Protection-meldingen](notifications.md)

* [Playbook voor Azure Active Directory Identity Protection](playbook.md)

* [Azure Active Directory Identity Protection-woordenlijst](glossary.md)

* [Aanmelden-ervaringen met Azure AD Identity Protection](flows.md)

* [Azure Active Directory Identity Protection - blokkering opheffen van gebruikers](howto-unblock-user.md)

* [Aan de slag met Azure Active Directory Identity Protection en Microsoft Graph](graph-get-started.md)

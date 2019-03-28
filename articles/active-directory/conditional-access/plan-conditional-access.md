---
title: Beleid voor voorwaardelijke toegang in Azure Active Directory plannen | Microsoft Docs
description: In dit artikel leert u hoe u het beleid voor voorwaardelijke toegang voor Azure Active Directory plannen.
services: active-directory
author: MicrosoftGuyJFlo
manager: daveba
tags: azuread
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.workload: identity
ms.date: 01/25/2019
ms.author: joflore
ms.reviewer: martincoetzer
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4832055bffa72732991cb766ce88cc48c07530e
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518708"
---
# <a name="how-to-plan-your-conditional-access-deployment-in-azure-active-directory"></a>Procedure: De implementatie van voorwaardelijke toegang in Azure Active Directory plannen

Plannen van uw implementatie van voorwaardelijke toegang is van essentieel belang om ervoor te zorgen dat u de strategie van de vereiste toegang voor apps en resources in uw organisatie bereiken. U moet het merendeel van uw moment tijdens de planningsfase van uw implementatie te ontwerpen van de verschillende beleidsregels die u nodig hebt om te verlenen of Blokkeer de toegang tot uw gebruikers in de voorwaarden die u kiest. Dit document worden de stappen beschreven die u nemen moet voor het implementeren van beleid voor voorwaardelijke toegang voor veilig en effectief. Voordat u begint, zorg ervoor dat u begrijpt hoe [voorwaardelijke toegang](overview.md) werkt en wanneer u deze moet gebruiken.


## <a name="what-you-should-know"></a>Wat u moet weten

Voorwaardelijke toegang beschouwen als een framework waarmee u toegang tot de apps en resources, in plaats van een zelfstandige functie van uw organisatie beheren. Sommige instellingen voor voorwaardelijke toegang vereist als gevolg daarvan kunnen extra functies worden geconfigureerd. Bijvoorbeeld, u kunt een beleid configureren dat op een specifieke reageert [niveau van aanmeldingsrisico](../identity-protection/howto-sign-in-risk-policy.md#what-is-the-sign-in-risk-policy). Een beleid dat is gebaseerd op een niveau voor aanmeldingsrisico vereist echter [Azure Active Directory identity protection](../identity-protection/overview.md) zijn ingeschakeld.

Als aanvullende functies vereist zijn, moet u mogelijk ook verwante licenties ophalen. Voor voorwaardelijke toegang is een functie van Azure AD Premium P1, moet identiteitsbeveiliging bijvoorbeeld een Azure AD Premium P2-licentie.

Er zijn twee soorten beleid voor voorwaardelijke toegang: basislijn- en standard. Een [Basisbeleid](baseline-protection.md) is een vooraf gedefinieerde voorwaardelijk toegangsbeleid. Het doel van dit beleid is om ervoor te zorgen dat u ten minste het niveau van de basislijn van de beveiliging is ingeschakeld hebt. Basislijn-beleid. Basislijn-beleidsregels zijn beschikbaar in alle edities van Azure AD en bieden alleen beperkte aanpassingsopties. Als een scenario meer flexibiliteit vereist, dient u het beleid van de basislijn en implementeren van uw vereisten in een aangepast beleid voor standard.

In een standard-voorwaardelijk toegangsbeleid kunt u alle instellingen voor het aanpassen van het beleid aan uw bedrijfsvereisten aanpassen. Standaardbeleid voor nodig een Azure AD Premium P1-licentie.




## <a name="draft-policies"></a>Concept-beleid

Voorwaardelijke toegang van Azure Active Directory kunt u de beveiliging van uw cloud-apps naar een nieuw niveau te brengen. In dit nieuwe niveau, is hoe u toegang hebt tot een cloud-app gebaseerd op een dynamische beleidsevaluatie in plaats van een statische access-configuratie. Met beleid voor voorwaardelijke toegang, definieert u een antwoord (**hiervoor**) aan een voorwaarde toegang (**wanneer dit gebeurt**).

![Reden en het antwoord](./media/plan-conditional-access/10.png)

Elk beleid voor voorwaardelijke toegang die u wilt implementeren met behulp van deze planning model definiëren. De planning oefening:

- Helpt u bij overzicht van de antwoorden en voorwaarden voor elk beleid.
- De resultaten in een goed gedocumenteerde voorwaardelijk beleid catalogus voor uw organisatie. 

U kunt uw catalogus gebruiken om te beoordelen of de implementatie van uw beleid de bedrijfsbehoeften van uw organisatie weerspiegelt. 

Gebruik de volgende voorbeeldsjabloon om te maken van beleid voor voorwaardelijke toegang voor uw organisatie:

|Wanneer *dit* gebeurt:|Voer *dit*:|
|-|-|
|Een poging tot toegang is gemaakt:<br>-Voor een cloud-app*<br>: als u gebruikers en groepen*<br>Met behulp van:<br>-Voorwaarde 1 (bijvoorbeeld buiten het bedrijfsnetwerk)<br>-Voorwaarde 2 (bijvoorbeeld apparaatplatformen)|Toegang tot de toepassing blokkeren|
|Een poging tot toegang is gemaakt:<br>-Voor een cloud-app*<br>: als u gebruikers en groepen*<br>Met behulp van:<br>-Voorwaarde 1 (bijvoorbeeld buiten het bedrijfsnetwerk)<br>-Voorwaarde 2 (bijvoorbeeld apparaatplatformen)|Verlenen van toegang met (en):<br>-Vereiste 1 (bijvoorbeeld, MFA)<br>-Vereiste 2 (bijvoorbeeld apparaatnaleving)|
|Een poging tot toegang is gemaakt:<br>-Voor een cloud-app*<br>: als u gebruikers en groepen*<br>Met behulp van:<br>-Voorwaarde 1 (bijvoorbeeld buiten het bedrijfsnetwerk)<br>-Voorwaarde 2 (bijvoorbeeld apparaatplatformen)|Verlenen van toegang met (of):<br>-Vereiste 1 (bijvoorbeeld, MFA)<br>-Vereiste 2 (bijvoorbeeld apparaatnaleving)|

Ten minste **wanneer dit gebeurt** definieert de principal (**die**) die probeert te krijgen tot een cloud-app (**wat**). Indien nodig, kunt u ook opnemen **hoe** een poging tot toegang wordt uitgevoerd. Voorwaardelijke toegang, de elementen die definiëren in het wie, wat, hoe voorwaarden worden genoemd. Zie voor meer informatie, [wat zijn de voorwaarden voor de voorwaardelijke toegang van Azure Active Directory?](conditions.md) 

Met **voert u deze**, definieert u het antwoord van uw beleid aan een voorwaarde voor toegang. Uw reactie, kunt u blokkeren of verlenen van toegang met aanvullende vereisten, bijvoorbeeld: multi-factor authentication (MFA). Zie voor een volledig overzicht [wat toegang zijn besturingselementen in Azure Active Directory voor voorwaardelijke toegang?](controls.md)  
 

De combinatie van voorwaarden en uw besturingselementen voor toegang stellen een beleid voor voorwaardelijke toegang voor.

![Reden en het antwoord](./media/plan-conditional-access/51.png)


Zie voor meer informatie, [wat is vereist voor het maken van een beleid werkt](best-practices.md#whats-required-to-make-a-policy-work).

Op dit moment is het een goed moment om te bepalen wat een naamgevingsnorm voor uw beleid. De naamgevingsnorm helpt u bij het zoeken van beleid en hun doel te begrijpen zonder deze te openen in de Azure-beheerportal. U moet een naam uw beleid om weer te geven:

- Een volgnummer
- De cloud-app die is van toepassing op
- Het antwoord
- Die van toepassing op
- Als deze van toepassing is (indien van toepassing)
 
![Naamgevingsnorm](./media/plan-conditional-access/11.png)

Terwijl een beschrijvende naam u helpt bij het bewaren van een overzicht van de implementatie van voorwaardelijke toegang, is het volgnummer is handig als u nodig hebt om te verwijzen naar een beleid in een gesprek. Bijvoorbeeld, als u een andere beheerder op de telefoon praten, kunt u vragen hem open beleid EM063 voor het oplossen van een probleem.



Bijvoorbeeld, statussen de naam van de volgende dat het beleid MFA vereist voor de gebruikers op de externe netwerken met behulp van de app CRP Dynamics marketing:

`CA01 - Dynamics CRP: Require MFA For marketing When on external networks`


Naast uw actieve beleidsregels kunt u wordt aangeraden ook implementeren uitgeschakeld beleid die als secundaire fungeren [besturingselementen in onderbreking/EMS-scenario's voor flexibele toegang](../authentication/concept-resilient-controls.md). Uw naamgevingsnorm voor het beleid voor onvoorziene gebeurtenissen moet een paar meer items bevatten: 

- `ENABLE IN EMERGENCY` aan het begin naar de naam van de opvallen tussen de andere beleidsregels.

- De naam van een onderbreking van die deze moet worden toegepast.

- Een bestellen volgnummer om de beheerder om te weten in welke volgorde beleid moeten worden ingeschakeld. 


De naam van de volgende geeft bijvoorbeeld aan dat dit beleid is het eerste beleid uit vier die moet u in het geval van een onderbreking van de MFA inschakelen:

`EM01 - ENABLE IN EMERGENCY, MFA Disruption[1/4] - Exchange SharePoint: Require hybrid Azure AD join For VIP users`







## <a name="plan-policies"></a>Beleidsregels plant

Bij het plannen van uw oplossing van het beleid voor voorwaardelijke toegang te beoordelen of u maken van beleid wilt voor het bereiken van de volgende resultaten. 


### <a name="block-access"></a>Toegang blokkeren

De optie voor het blokkeren van toegang tot krachtige is omdat deze:

- Alle toewijzingen voor een gebruiker trumps

- De stroom moet worden geblokkeerd dat de hele organisatie van de aan te melden bij uw tenant heeft
 
Als u wilt dat de toegang voor alle gebruikers geblokkeerd, moet u ten minste één gebruiker (meestal Noodtoegang-accounts) uitsluiten van het beleid. Zie voor meer informatie, [gebruikers en groepen selecteren](block-legacy-authentication.md#select-users-and-cloud-apps).  
    

### <a name="require-mfa"></a>MFA vereisen

Ter vereenvoudiging van de aanmeldingservaring van uw gebruikers, is het raadzaam om toe te staan dat ze zich aanmelden bij uw cloud-apps met behulp van een gebruikersnaam en wachtwoord. Normaal gesproken zijn er echter ten minste enkele scenario's waarvoor u wordt aangeraden om te vereisen een sterkere vorm van verificatie-account. Met beleid voor voorwaardelijke toegang, kunt u de vereiste voor MFA om bepaalde scenario's te beperken. 

Algemene scenario's om MFA te vereisen zijn toegang:

- [Door beheerders](baseline-protection.md#require-mfa-for-admins)
- [Tot bepaalde apps](app-based-mfa.md) 
- [Van netwerklocaties, u niet vertrouwt](untrusted-networks.md).


### <a name="respond-to-potentially-compromised-accounts"></a>Reageren op mogelijk verdachte accounts

U kunt automatische antwoorden voor aanmeldingen vanaf mogelijk verdachte identiteiten implementeren met beleid voor voorwaardelijke toegang. De kans dat een account is aangetast, uitgedrukt in de vorm van risiconiveaus. Er zijn twee risiconiveaus berekend door identity protection: aanmeldingsrisico en gebruikersrisico. Voor het implementeren van een reactie op een aanmeldingsrisico, hebt u twee opties:

- [De voorwaarde voor aanmeldingsrisico](conditions.md#sign-in-risk) in beleid voor voorwaardelijke toegang
- [Het beleid voor aanmeldingsrisico](../identity-protection/howto-sign-in-risk-policy.md) in identiteitsbeveiliging 

Het aanmeldingsrisico als voorwaarde adressering verdient de voorkeur omdat dit u meer opties voor het aanpassen biedt.

Het risiconiveau van de gebruiker is alleen beschikbaar als [beleid voor gebruikersrisico's](../identity-protection/howto-user-risk-policy.md) in identity protection. 

Zie voor meer informatie, [wat is Azure Active Directory Identity Protection?](../identity-protection/overview.md) 


### <a name="require-managed-devices"></a>Beheerde apparaten vereisen

De toename van ondersteunde apparaten voor toegang tot uw cloudresources kunt u de productiviteit van uw gebruikers verbeteren. Aan de andere kant wilt u waarschijnlijk geen bepaalde resources in uw omgeving om te worden geopend vanaf apparaten met een onbekende beveiligingsniveau. Voor de betrokken resources, moet u vereisen dat gebruikers hebben alleen toegang met behulp van een beheerd apparaat. Zie voor meer informatie, [hoe u beheerde apparaten vereist voor toegang tot cloud-Apps met voorwaardelijke toegang](require-managed-devices.md). 

### <a name="require-approved-client-apps"></a>Goedgekeurde client-apps vereisen

Een van de eerste beslissingen die u maken wilt voor uw eigen apparaten meebrengen (BYOD)-scenario's, is de noodzaak voor het beheren van het hele apparaat of alleen de gegevens erop. Uw werknemers gebruiken mobiele apparaten voor zowel privé- als werktaken. Terwijl te zorgen dat uw werknemers productief zijn kan, wilt u ook om te voorkomen dat gegevens verloren gaan. Met voorwaardelijke toegang van Azure Active Directory (Azure AD), kunt u de toegang beperken tot uw cloud-apps op goedgekeurde client-apps die uw bedrijfsgegevens kunnen beveiligen. Zie voor meer informatie, [goedgekeurde client-apps vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang](app-based-conditional-access.md).


### <a name="block-legacy-authentication"></a>Verouderde verificatie blokkeren

Azure AD biedt ondersteuning voor diverse van de meest gebruikte verificatie en autorisatie protocollen, waaronder oudere authentication. Hoe kunt u voorkomen dat apps met behulp van verouderde verificatie van toegang tot resources van uw tenant? De aanbeveling is om te blokkeren dat ze alleen met een beleid voor voorwaardelijke toegang. Indien nodig, kunt u alleen bepaalde gebruikers en specifieke netwerklocaties apps die zijn gebaseerd op verouderde verificatie gebruiken. Zie voor meer informatie, [verouderde verificatie met Azure AD met voorwaardelijke toegang blokkeren](block-legacy-authentication.md).


## <a name="test-your-policy"></a>Het beleid testen

Voordat u een beleid implementeren naar productie, moet u testen om te controleren of deze werkt zoals verwacht, is.

1. Testgebruikers maken

2. Een testplan maken

3. Het beleid configureren

4. Een gesimuleerde aanmelding evalueren

5. Het beleid testen

6. Opschonen



### <a name="create-test-users"></a>Testgebruikers maken

Als u wilt testen van een beleid, een set van gebruikers die vergelijkbaar is met de gebruikers in uw omgeving te maken. Testgebruikers maken, kunt u controleren of uw beleidsregels werken zoals verwacht voordat u gevolgen hebben voor echte gebruikers en hun toegang tot apps en resources worden mogelijk onderbroken. 


Sommige organisaties hebben test tenants voor dit doel. Echter, kan het lastig zijn om alle voorwaarden en apps in een testtenant voor het testen van het resultaat van een beleid volledig opnieuw te maken. 

### <a name="create-a-test-plan"></a>Een testplan maken

De test-abonnement is belangrijk dat u hebt een vergelijking tussen de verwachte resultaten en de werkelijke resultaten. U moet altijd een verwachting voordat u iets test hebben. De volgende tabel geeft een overzicht van voorbeeld Testscenario's. Pas de scenario's en de verwachte resultaten op basis van hoe uw CA-beleid zijn geconfigureerd.

|Beleid |Scenario |Verwacht resultaat | Resultaat |
|---|---|---|---|
|[Vereis MFA bij inactiviteit](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)|Gemachtigde gebruiker zich in *App* terwijl op een vertrouwde locatie bevindt / werken|Gebruiker is niet gevraagd om MFA| |
|[Vereis MFA bij inactiviteit](https://docs.microsoft.com/azure/active-directory/conditional-access/untrusted-networks)|Gemachtigde gebruiker zich in *App* terwijl niet op een vertrouwde locatie bevindt / werken|Gebruiker wordt gevraagd naar MFA en kan zich aanmelden met succes| |
|[MFA vereisen (voor de beheerder)](https://docs.microsoft.com/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins)|Globale beheerder zich aanmeldt bij *App*|Beheerder wordt gevraagd naar MFA| |
|[Riskante aanmeldingen](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-sign-in-risk-policy)|Gebruiker zich aanmeldt bij *App* met behulp van een [Tor-browser](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-playbook)|Beheerder wordt gevraagd naar MFA| |
|[Apparaatbeheer](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)|Gemachtigde gebruiker wil zich aanmelden vanaf een geautoriseerd apparaat|Toegang verleend| |
|[Apparaatbeheer](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)|Pogingen van de gebruiker zich aanmeldt vanaf een niet-gemachtigd apparaat geautoriseerd|Toegang geblokkeerd| |
|[Wijzigen van wachtwoorden voor riskante gebruikers](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy)|Gemachtigde gebruiker probeert zich aanmelden met gestolen referenties (hoog risico zich aanmelden)|Gebruiker wordt gevraagd om wachtwoord te wijzigen of de toegang is geblokkeerd op basis van uw beleid| |


### <a name="configure-the-policy"></a>Het beleid configureren

Beheren van beleid voor voorwaardelijke toegang is een handmatige taak. U kunt in één centrale locatie - de pagina met voorwaardelijk beleid voor voorwaardelijke toegang beheren in de Azure-portal. Een beginpunt voor de pagina voor voorwaardelijke toegang is de **Security** sectie de **Active Directory** navigatiedeelvenster. 

![Voorwaardelijke toegang](media/plan-conditional-access/03.png)


Als u wilt meer informatie over het maken van beleid voor voorwaardelijke toegang, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md). In deze snelstartgids helpt u bij:

- Vertrouwd raken met de gebruikersinterface.
- Krijg een eerste indruk van hoe voorwaardelijke toegang werkt. 


### <a name="evaluate-a-simulated-sign-in"></a>Een gesimuleerde aanmelding evalueren

Nu u uw beleid voor voorwaardelijke toegang hebt geconfigureerd, wilt u weten of het werkt zoals u wilt. Als een eerste stap gebruikt u de voorwaardelijke toegang [wat gebeurt er als beleid hulpprogramma](what-if-tool.md) voor het simuleren van een aanmelding van uw testgebruiker. De simulatie schat de impact van deze aanmelding op uw beleid in en genereert een simulatierapport.

>[!NOTE]
> Hoewel een gesimuleerde uitvoeren geeft u de indruk van het effect heeft een beleid voor voorwaardelijke toegang, het is geen vervanging voor een werkelijke testuitvoering.


### <a name="test-your-policy"></a>Het beleid testen

Testcases volgens uw testplan uitvoeren. In deze stap maakt uitvoeren u via een end-to-end-test van elk beleid voor uw testgebruikers om ervoor te zorgen dat elk beleid correct werkt. Gebruik de scenario's voor het uitvoeren van elke test hierboven hebt gemaakt.

Het is belangrijk om ervoor te zorgen dat u de uitsluitingscriteria voor van een beleid voor de test. U kunt bijvoorbeeld een gebruiker of groep uitsluiten van een beleid dat MFA vereist. U moet daarom als de uitgesloten gebruikers wordt gevraagd voor MFA, testen, omdat de combinatie van ander beleid MFA voor gebruikers vereisen kunt.


### <a name="cleanup"></a>Opschonen

De procedure opschonen bestaat uit de volgende stappen uit:

1. Het beleid uitschakelen.

2. Verwijder de toegewezen gebruikers en groepen.

3. Verwijder de testgebruikers.  




## <a name="move-to-production"></a>Verplaatsen naar productie

Wanneer er nieuwe beleidsregels zijn gereed voor uw omgeving, implementeert u deze in fasen:

- Interne wijziging communicatie bieden aan eindgebruikers.

- Beginnen met een klein aantal gebruikers en controleer of dat het beleid werkt zoals verwacht.

- Wanneer u een beleid om op te nemen meer gebruikers wilt uitbreiden, blijven uitsluiten van alle beheerders. Met uitzondering van beheerders, zorgt u ervoor dat iemand nog steeds toegang tot een beleid heeft als een wijziging vereist is.

- Alleen als dit is vereist, moet u een beleid toepassen op alle gebruikers.

Als een best practice, ten minste één gebruikersaccount maken dat is:

- Toegewezen aan het beheer van beleid

- Uitgesloten van alle beleidsregels voor uw

 



## <a name="rollback-steps"></a>Terugdraaien van de stappen

Als u uw zojuist geïmplementeerde beleid terugdraaien wilt, gebruikt u een of meer van de volgende opties om terug te draaien:

1. **Het beleid uitschakelen** -uitschakelen van een beleid zorgt ervoor dat het is niet van toepassing wanneer een gebruiker probeert aan te melden. U kunt altijd terugkomen en het beleid in te schakelen wanneer u wilt gebruiken.

    ![Beleid uitschakelen](media/plan-conditional-access/07.png)

2. **Uitsluiten van een gebruiker / groep van een beleid** -als een gebruiker geen toegang tot de app is, kunt u kiezen om uit te sluiten van de gebruiker van het beleid

    ![Exluce gebruikers](media/plan-conditional-access/08.png)

    >[!NOTE]
    > Deze optie moet worden gebruikt in spaarzaam, alleen in situaties waar de gebruiker vertrouwd wordt. De gebruiker moet zo snel mogelijk terug aan het beleid of de groep worden toegevoegd.

3. **Verwijder het beleid** : als het beleid niet langer vereist is, verwijderen.

## <a name="next-steps"></a>Volgende stappen

Bekijk [documentatie voor Azure AD voor voorwaardelijke toegang](index.yml) voor een overzicht van de beschikbare informatie.

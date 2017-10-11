---
title: "Azure Active Directory hybride identiteit ontwerpoverwegingen - strategie voor de ingebruikname van een hybride identiteit definiëren | Microsoft Docs"
description: "Met voorwaardelijk toegangsbeheer controleert de Azure Active Directory de specifieke voorwaarden die u kiest bij het verifiëren van de gebruiker en alvorens deze toegang tot de toepassing. Als deze voorwaarden is voldaan, wordt de gebruiker geverifieerd en toegang te krijgen tot de toepassing."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: b92fa5a9-c04c-4692-b495-ff64d023792c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 2c57b394beb6382807a4c8c83de975a0ae68d726
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="define-a-hybrid-identity-adoption-strategy"></a>Een strategie voor hybride identiteit acceptatie definiëren
In deze taak definieert u de hybride identiteit acceptatie strategie voor uw oplossing voor hybride identiteit om te voldoen aan de bedrijfsvereisten die zijn beschreven in:

* [Bedrijfsbehoeften vaststellen](active-directory-hybrid-identity-design-considerations-business-needs.md)
* [Vereisten voor directory-synchronisatie bepalen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
* [Vereisten multi-factor authentication bepalen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Zakelijke behoeften-strategie bepalen
De eerste taak adressen bepalen van het bedrijf organisaties moet.  Dit kan zeer brede en bereik kneep kan optreden als u niet zorgvuldig.  Houd het eenvoudig maar vergeet om te plannen voor een ontwerp dat wordt aangepast aan en maak veranderingen in de toekomst mogelijk in het begin.  Ongeacht of het ontwerp van een eenvoudige of een zeer complexe, is Azure Active Directory de Microsoft Identity-platform dat ondersteuning biedt voor Office 365, Microsoft Online Services en cloud-bewuste toepassingen.

## <a name="define-an-integration-strategy"></a>Een Integratiestrategie voor definiëren
Microsoft heeft drie belangrijkste integratiescenario's die cloud-identiteiten, gesynchroniseerde identiteiten en federatieve identiteiten zijn.  Plan voor overstap op een van deze integratiestrategieën.  De strategie die u kiest, kan variëren en de beslissingen die u bij het kiezen van een kan bevatten, wat voor soort gebruikerservaring die u bieden wilt, hebt u enkele van de bestaande infrastructuur al in-place en wat is de meest voordelige.  

![](./media/hybrid-id-design-considerations/integration-scenarios.png)

De gedefinieerd in de afbeelding hierboven scenario's:

* **Cloud-identiteiten**: dit zijn de identiteiten die uitsluitend in de cloud bestaat.  In het geval van Azure AD, zouden ze zich bevinden in het bijzonder in uw Azure AD-directory.
* **Gesynchroniseerd**: deze identiteiten die lokaal aanwezig zijn en in de cloud.  Met Azure AD Connect, worden deze gebruikers gemaakt of gekoppeld met de bestaande Azure AD-accounts.  De wachtwoord-hash van de gebruiker worden gesynchroniseerd vanuit de on-premises omgeving aan de cloud in wat een wachtwoord-hash wordt aangeroepen.  Wanneer met behulp van gesynchroniseerd is de enige belemmering als een gebruiker is uitgeschakeld in de on-premises-omgeving, kan deze die accountstatus worden weergegeven in Azure AD tot 3 uur duren.  Dit komt door het synchronisatie-interval.
* **Federatieve**: deze identiteiten bestaan zowel on-premises en in de cloud.  Met Azure AD Connect, worden deze gebruikers gemaakt of gekoppeld met de bestaande Azure AD-accounts.  

> [!NOTE]
> Lees voor meer informatie over de synchronisatie-opties [uw on-premises identiteiten integreren met Azure Active Directory](connect/active-directory-aadconnect.md).
> 
> 

De volgende tabel helpt bij het bepalen van de voordelen en nadelen van elk van de volgende strategieën:

| Een strategie voor | Voordelen | Nadelen |
| --- | --- | --- |
| **Cloud-identiteiten** |Gemakkelijker te beheren voor kleine organisatie. <br> Er is niets om te installeren op de lokale geen extra hardware nodig<br>Eenvoudig uitgeschakeld als de gebruiker het bedrijf verlaat |Gebruikers moeten aanmelden bij het openen van workloads in de cloud <br> Wachtwoorden al kunnen dan niet hetzelfde zijn voor de cloud en on-premises identiteiten |
| **Gesynchroniseerd** |On-premises wachtwoord wordt zowel on-premises verifiëren en cloudadreslijsten <br>Gemakkelijker te beheren voor kleine, grote of middelgrote organisaties <br>Gebruikers kunnen eenmalige aanmelding (SSO) hebben voor een aantal bronnen <br> Microsoft voorkeurs-methode voor synchronisatie <br> Gemakkelijker te beheren |Sommige gebruikers mogelijk hun mappen synchroniseren met de cloud vanwege specifiek bedrijf politie weerstand |
| **Federatieve** |Gebruikers beschikken over eenmalige aanmelding (SSO) <br>Als een gebruiker wordt beëindigd of verlaat, het account kan, onmiddellijk worden uitgeschakeld en de toegang is ingetrokken,<br> Ondersteunt geavanceerde scenario's die niet kan worden bewerkstelligd met gesynchroniseerd |Meer stappen voor het installeren en configureren <br> Hogere onderhoud <br> Extra hardware nodig voor de STS-infrastructuur <br> Extra hardware voor het installeren van de federation-server nodig. Aanvullende software is vereist als AD FS wordt gebruikt <br> Uitgebreide instellingen vereist voor eenmalige aanmelding <br> Kritieke storingspunt als de federation-server niet actief is, gebruikers niet mogelijk om te verifiëren |

### <a name="client-experience"></a>Clientervaring
Meld u in de gebruikerservaring worden bepaald door de strategie die u gebruikt.  De volgende tabellen bieden u informatie over wat de gebruikers moeten hun aanmeldingservaring worden verwacht.  Houd er rekening mee dat niet alle federatieve identiteiten-providers SSO in alle scenario's ondersteunen.

**Toepassingen die lid zijn van Domain en particuliere netwerken**:

|  | Gesynchroniseerde identiteiten | Federatieve identiteiten |
| --- | --- | --- |
| Webbrowsers |Op formulieren gebaseerde verificatie |eenmalige aanmelding op, soms moeten organisatie-ID opgeven |
| Outlook |Prompt voor referenties |Prompt voor referenties |
| Skype voor bedrijven (Lync) |Prompt voor referenties |eenmalige aanmelding voor Lync, referenties gevraagd voor Exchange |
| SkyDrive Pro |Prompt voor referenties |eenmalige aanmelding |
| Office Professional Plus abonnement |Prompt voor referenties |eenmalige aanmelding |

**Externe of niet-vertrouwde bronnen**:

|  | Gesynchroniseerde identiteiten | Federatieve identiteiten |
| --- | --- | --- |
| Webbrowsers |Op formulieren gebaseerde verificatie |Op formulieren gebaseerde verificatie |
| Outlook, Skype voor bedrijven (Lync) Skydrive Pro, Office-abonnement |Prompt voor referenties |Prompt voor referenties |
| Exchange ActiveSync |Prompt voor referenties |eenmalige aanmelding voor Lync, referenties gevraagd voor Exchange |
| Mobiele apps |Prompt voor referenties |Prompt voor referenties |

Als u van de taak 1 dat u hebt een 3rd partij IdP of zijn momenteel als wilt gebruiken een federatie met Azure AD hebt vastgesteld, moet u rekening houden met de volgende ondersteunde mogelijkheden:

* Elke SAML 2.0-provider die voldoen aan het beleid voor het profiel SP Lite ondersteunt verificatie met Azure AD en toepassingen die zijn gekoppeld
* Biedt ondersteuning voor passieve verificatie, wat auth OWA, gesimuleerde Productieorder, enzovoort vergemakkelijkt.
* Exchange Online-clients kunnen worden ondersteund via het SAML 2.0 verbeterde Client profiel (ECP)

Ook moet u rekening houden met welke mogelijkheden zijn niet meer beschikbaar:

* Zonder ondersteuning voor WS-Trust/Federation worden alle actieve clients verbroken
  * Dit betekent dat er geen Lync-client, de client van OneDrive, Office-abonnement, Office Mobile voorafgaand aan Office 2016
* Overgang van Office naar passieve verificatie kunnen zij pure SAML 2.0 IdPs ondersteunen is, maar ondersteuning nog steeds op basis van de client door de client

> [!NOTE]
> Lees het artikel http://aka.ms/ssoproviders voor de meest recente lijst.
> 
> 

## <a name="define-synchronization-strategy"></a>Synchronisatie-strategie bepalen
In deze taak u de hulpprogramma's die worden gebruikt definieert voor het synchroniseren van de organisatie on-premises gegevens naar de cloud en wat topologie die u moet gebruiken.  Omdat de meeste organisaties gebruikmaken van Active Directory, informatie over het gebruik van Azure AD Connect voor het oplossen van de bovenstaande vragen vindt u in sommige details.  Er is informatie over het gebruik van FIM 2010 R2- of MIM 2016 om u te helpen bij het plannen van deze strategie voor omgevingen waarin er geen Active Directory.  Echter toekomstige versies van Azure AD Connect ondersteunt LDAP-adreslijsten, dus zijn afhankelijk van de tijdlijn, wordt deze informatie is mogelijk helpen.

### <a name="synchronization-tools"></a>Hulpprogramma's voor synchronisatie
In de afgelopen jaren, hebben verschillende hulpprogramma's voor synchronisatie bestond en gebruikt voor verschillende scenario's.  Azure AD Connect is momenteel het Ga naar het hulpprogramma naar keuze voor alle ondersteunde scenario's.  AAD Sync en DirSync zijn ook nog steeds rond en zelfs nu aanwezig is in uw omgeving mag zijn. 

> [!NOTE]
> Lees voor de meest recente informatie over de ondersteunde functies van elk hulpprogramma [vergelijking van hulpprogramma's voor Directory-integratie](active-directory-hybrid-identity-design-considerations-tools-comparison.md) artikel.  
> 
> 

### <a name="supported-topologies"></a>Ondersteunde topologieën
Bij het definiëren van een strategie voor synchronisatie, kan de topologie die wordt gebruikt moet worden bepaald. Afhankelijk van de informatie die is bepaald in stap kunt u 2 bepalen welke topologie is de beste is om te gebruiken. Het één forest, één Azure AD-topologie is de meest voorkomende en bestaat uit één Active Directory-forest en één exemplaar van Azure AD.  Dit gaat worden gebruikt in een meerderheid van de scenario's en de verwachte topologie bij gebruik van Azure AD Connect Express-installatie, zoals wordt weergegeven in de afbeelding hieronder.

![](./media/hybrid-id-design-considerations/single-forest.png)Scenario voor één Forest het is heel gebruikelijk voor kleine en zelfs grote organisaties hebben meerdere forests, zoals wordt weergegeven in afbeelding 5.

> [!NOTE]
> Voor meer informatie over de verschillende on-premises en Azure AD-topologieën met Azure AD Connect sync Lees het artikel [topologieën voor Azure AD Connect](connect/active-directory-aadconnect-topologies.md).
> 
> 

![](./media/hybrid-id-design-considerations/multi-forest.png) 

Met meerdere forests

Als dit het geval wordt de topologie met meerdere-forest-enkel Azure AD moet worden beschouwd als de volgende vereisten voldaan is:

* Gebruikers hebben slechts 1 identiteit in alle forests – de unieke id gebruikers sectie hieronder wordt beschreven dit in meer detail.
* De gebruiker wordt geverifieerd met het forest waarin hun identiteit zich bevindt
* UPN en Bronanker (onveranderbare id), zijn afkomstig van dit forest
* Alle forests toegankelijk zijn voor Azure AD Connect: dit betekent dat deze hoeft niet te worden van domein toegevoegd en kan worden geplaatst in een Perimeternetwerk, als dit dit vergemakkelijkt.
* Gebruikers hebben slechts één postvak
* Het forest dat als host fungeert voor postvak van de gebruiker heeft de beste kwaliteit van de gegevens voor kenmerken zichtbaar in de Exchange globale adreslijst (GAL)
* Als er geen postvak van de gebruiker, kan klikt u vervolgens een forest worden gebruikt om bij te dragen van deze waarden
* Als u een gekoppeld postvak hebt, klikt u vervolgens is er ook een ander account in een ander forest gebruikt aan te melden.

> [!NOTE]
> Objecten die in zowel on-premises en in de cloud voorkomen zijn 'verbonden' via een unieke id. In de context van Directory-synchronisatie, worden deze unieke id wordt de SourceAnchor genoemd. In de context van eenmalige aanmelding is dit de onveranderbare id genoemd genoemd. [Concepten voor Azure AD Connect ontwerpen](connect/active-directory-aadconnect-design-concepts.md#sourceanchor) voor meer overwegingen met betrekking tot het gebruik van SourceAnchor.
> 
> 

Als de bovenstaande niet waar zijn en er meer dan één actieve account of meer dan één postvak, wordt Azure AD Connect Kies een en de andere negeren.  Als u postvakken maar geen andere account hebt gekoppeld, worden deze accounts niet geëxporteerd naar Azure AD en die gebruiker geen lid zijn van groepen worden.  Dit wijkt af van hoe deze in het verleden met DirSync is en is bedoeld voor een beter ondersteuning scenario's met meerdere forests. Een scenario met meerdere forests wordt weergegeven in de afbeelding hieronder.

![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 

**Meerdere forests meerdere Azure AD-scenario**

Het is raadzaam te hebben slechts één map in Azure AD voor een organisatie, maar het wordt ondersteund deze een 1:1-relatie tussen een Azure AD Connect sync-server en een Azure Active directory wordt bewaard.  Voor elk exemplaar van Azure AD moet u een installatie van Azure AD Connect.  Ook Azure AD inherent is geïsoleerd en gebruikers in één exemplaar van Azure AD kan niet worden gebruikers in een ander exemplaar wilt weergeven.

Het is mogelijk en verbinding maken met een lokaal exemplaar van Active Directory meerdere Azure AD-directory's zoals weergegeven in de onderstaande afbeelding wordt ondersteund:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 

**Filteren scenario voor één forest**

Om dit te doen het volgende moeten worden voldaan:

* Azure AD Connect sync-servers moeten worden geconfigureerd voor het filteren zodat ze elk een sluiten elkaar wederzijds uit set van objecten hebben.  Dit doen, bijvoorbeeld door het bereik van elke server aan een bepaald domein of organisatie-eenheid.
* Een DNS-domein kan alleen worden geregistreerd in een enkel Azure AD-directory zodat de UPN's van de gebruikers in de on-premises AD afzonderlijke naamruimten moet gebruiken
* Gebruikers in één exemplaar van Azure AD is alleen mogelijk gebruikers uit hun exemplaar wilt weergeven.  Ze kan niet worden gebruikers in de andere exemplaren wilt weergeven
* Alleen een van de Azure AD-mappen Exchange hybride met de on-premises inschakelen kunt AD
* Wederzijdse exclusiviteit geldt ook voor terugschrijven.  Hierdoor kunt u sommige write-back-functies niet ondersteund met deze topologie omdat deze wordt ervan uitgegaan een configuratie met één on-premises dat.  Dit omvat:
  * Groep terugschrijven met standaardconfiguratie
  * Apparaat terugschrijven

Let erop dat het volgende wordt niet ondersteund en moet niet worden gekozen als een implementatie:

* Meerdere servers met Azure AD Connect sync verbinding te maken met de dezelfde Azure AD-directory, zelfs als ze zijn geconfigureerd om te synchroniseren sluiten elkaar wederzijds uit set van object wordt niet ondersteund
* Dit wordt niet ondersteund voor het synchroniseren van de gebruiker zich meerdere Azure AD-mappen. 
* Dit wordt ook niet ondersteund als u wilt maken van een configuratie wijzigen zodat gebruikers in een Azure AD worden weergegeven als contactpersonen in een andere Azure AD-directory. 
* Het is ook niet-ondersteunde Azure AD Connect-synchronisatie verbinding maken met meerdere Azure AD-mappen wijzigen.
* Azure AD-mappen zijn standaard geïsoleerd. Dit wordt niet ondersteund voor de configuratie van Azure AD Connect-synchronisatie gegevens lezen uit een andere Azure AD-directory in een poging tot het bouwen van een gemeenschappelijke en uniform GAL tussen de mappen wijzigen. Dit wordt ook niet ondersteund gebruikers wilt exporteren als contactpersonen aan een andere on-premises AD dat met behulp van Azure AD Connect-synchronisatie.

> [!NOTE]
> Als uw organisatie voorkomt dat computers in uw netwerk verbinding maken met Internet, in dit artikel worden de eindpunten (FQDN's, IPv4 en IPv6-adresbereiken) die moet worden opgenomen uw uitgaande toestaan lijsten en Internet Explorer vertrouwde websites van client computers om te controleren of uw computers kunnen met succes gebruiken voor Office 365. Lees voor meer informatie [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
> 
> 

## <a name="define-multi-factor-authentication-strategy"></a>Multi-factor authentication-strategie bepalen
In deze taak definieert u de strategie voor multi-factor authentication-server te gebruiken.  Azure multi-factor Authentication wordt geleverd in twee verschillende versie.  Een cloud-gebaseerd is en de andere lokaal via de Azure MFA-Server.  Op basis van de evaluatie hierboven u kunt u bepalen welke oplossing is de juiste is voor uw strategie.  Gebruik de onderstaande tabel om te bepalen welke ontwerpoptie het beste voldoen beveiligingsvereiste van uw bedrijf:

Van meervoudige ontwerpopties:

| Asset te beveiligen | MFA in de cloud | MFA on-premises |
| --- | --- | --- |
| Microsoft-apps |Ja |Ja |
| SaaS-apps in de app-galerie |Ja |Ja |
| IIS-toepassingen die zijn gepubliceerd via toepassingsproxy van Azure AD |Ja |Ja |
| IIS-toepassingen die niet zijn gepubliceerd via de toepassingsproxy van Azure AD |Er is geen |Ja |
| Externe toegang als VPN, RDG |Er is geen |Ja |

Hoewel u mogelijk hebt gekozen voor een oplossing voor uw strategie, moet u nog steeds gebruik van de evaluatie van boven op waar uw gebruikers zich bevinden.  Hierdoor kan de oplossing te wijzigen.  Gebruik de onderstaande tabel om u vaststellen hiervan te helpen:

| Gebruikerslocatie | Ontwerp van voorkeur-optie |
| --- | --- |
| Azure Active Directory |Multi-FactorAuthentication in de cloud |
| Azure AD en on-premises AD dat gebruikmaakt van federatie met AD FS |Beide |
| Azure AD en on-premises AD dat gebruikmaakt van Azure AD Connect-geen Wachtwoordsynchronisatie |Beide |
| Azure AD en on-premises met Azure AD Connect met Wachtwoordsynchronisatie |Beide |
| On-premises AD |Multi-Factor Authentication-server |

> [!NOTE]
> U moet er ook voor zorgen dat de optie voor het ontwerp van multi-factor authentication-server die u hebt geselecteerd de functies die vereist voor uw ontwerp zijn ondersteunt.  Lees voor meer informatie [de multi-factor-beveiligingsoplossing kiezen voor u](../multi-factor-authentication/multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).
> 
> 

## <a name="multi-factor-auth-provider"></a>Multi-factor Authentication-Provider
Multi-factor authentication-server is standaard beschikbaar voor hoofdbeheerders met een Azure Active Directory-tenant. Echter, als u wilt uitbreiden multi-factor authentication-server naar alle gebruikers en/of uw globale beheerders kunnen profiteren van functies zoals de beheerportal, aangepaste begroeting en rapporten uitvoeren wilt, klikt u vervolgens moet u kopen en configureren Multi-factor Authentication-Provider.

> [!NOTE]
> U moet er ook voor zorgen dat de optie voor het ontwerp van multi-factor authentication-server die u hebt geselecteerd de functies die vereist voor uw ontwerp zijn ondersteunt. 
> 
> 

## <a name="next-steps"></a>Volgende stappen
[Bepalen van de beveiligingsvereisten voor gegevens](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)


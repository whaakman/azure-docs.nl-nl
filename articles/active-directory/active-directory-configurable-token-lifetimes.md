---
title: Configureerbare token levensduur in Azure Active Directory | Microsoft Docs
description: Informatie over het instellen van de levensduur voor tokens die zijn uitgegeven door Azure AD.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 06f5b317-053e-44c3-aaaa-cf07d8692735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: billmath
ms.custom: aaddev
ms.reviewer: anchitn
ms.openlocfilehash: 19cd4ae8dc0ca3efa4eca51e5a6ba102338b4ef9
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-public-preview"></a>Configureerbare token levensduur in Azure Active Directory (openbare Preview)
U kunt de levensduur van een token dat is uitgegeven door Azure Active Directory (Azure AD) opgeven. Token levensduur voor alle apps in uw organisatie, voor een multitenant (meerdere organisatie)-toepassing of voor een specifieke service-principal kunt u instellen in uw organisatie.

> [!NOTE]
> Deze mogelijkheid is momenteel in de openbare Preview. Wees voorbereid om te herstellen of verwijderen van eventuele wijzigingen. De functie is beschikbaar in een abonnement voor Azure Active Directory tijdens de openbare Preview. Echter, wanneer de functie algemeen beschikbaar wordt, sommige aspecten van de functie mogelijk een [Azure Active Directory Premium](active-directory-get-started-premium.md) abonnement.
>
>

Een beleidsobject vertegenwoordigt in Azure AD een reeks regels die op afzonderlijke toepassingen of op alle toepassingen in een organisatie worden afgedwongen. Elk beleidstype heeft een unieke structuur, met een set eigenschappen die worden toegepast op objecten waarvoor ze worden toegewezen.

U kunt een beleid aanwijzen als het standaardbeleid voor uw organisatie. Het beleid wordt toegepast op alle toepassingen in de organisatie, zolang deze niet is overschreven door een beleid met een hogere prioriteit. U kunt ook een beleid toewijzen aan specifieke toepassingen. De volgorde van prioriteit, varieert per beleidstype.


## <a name="token-types"></a>Tokentypen

U kunt de levensduur van token-beleid voor het vernieuwen van tokens, toegangstokens, sessie tokens en ID-tokens instellen.

### <a name="access-tokens"></a>Toegangstokens
Clients gebruiken toegangstokens voor toegang tot een beveiligde bron. Een toegangstoken kan alleen worden gebruikt voor een specifieke combinatie van de gebruiker, de client en de resource. Toegangstokens kunnen niet worden ingetrokken, en zijn geldig totdat ze aflopen. Een kwaadwillende actor dat een toegangstoken is verkregen kunt gebruiken voor het gebied van de levensduur. Het aanpassen van de levensduur van een toegangstoken is een compromis tussen de systeemprestaties verbeteren en vergroot de hoeveelheid tijd dat de client toegang behouden nadat het account van de gebruiker is uitgeschakeld. Verbeterde systeemprestaties wordt bereikt door het verminderen van het aantal keren dat een client moet een nieuw toegangstoken verkrijgen.

### <a name="refresh-tokens"></a>Vernieuwen van tokens
Wanneer een client een toegangstoken verkrijgt voor toegang tot een beveiligde bron, wordt de client ontvangt een vernieuwingstoken zowel een toegangstoken. Het vernieuwingstoken dat is gebruikt voor nieuwe toegang/vernieuwen token paren wanneer het huidige toegangstoken is verlopen. Een vernieuwingstoken dat is gebonden aan een combinatie van client en de gebruiker. Een vernieuwingstoken kan worden ingetrokken en de geldigheid van het token is ingeschakeld, telkens wanneer het token wordt gebruikt.

Het is belangrijk om onderscheid maken tussen vertrouwelijk en openbare-clients. Zie voor meer informatie over de verschillende soorten clients [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Levensduur van token met het vernieuwen van tokens vertrouwelijke client
Vertrouwelijke clients zijn toepassingen waarvan het wachtwoord van een client (geheim genoemd) kunnen veilig worden opgeslagen. Zij kunnen aantonen dat verzoeken die worden verzonden vanuit de clienttoepassing en niet vanuit een schadelijke actor. Een web-app is bijvoorbeeld een vertrouwelijk client omdat deze een clientgeheim kunt opslaan op de webserver. Deze wordt niet weergegeven. Omdat deze stromen veiliger zijn, is het de standaardlevensduur van vernieuwen van tokens verleend aan deze stromen `until-revoked`, kan niet worden gewijzigd met behulp van beleid en niet op vrijwillige wachtwoorden opnieuw ingesteld, worden ingetrokken.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Levensduur van token met openbare-client vernieuwen van tokens

Openbare clients kunnen niet veilig opslaan van het wachtwoord van een client (geheim genoemd). Bijvoorbeeld een iOS-/ Android-app kan niet verbergen, weergeven een geheim van de resource-eigenaar, zodat deze wordt beschouwd als een openbare-client. U kunt beleid instellen voor bronnen om te voorkomen dat het verkrijgen van een nieuw token toegang/vernieuwen paar vernieuwen van tokens van openbare clients die ouder zijn dan een opgegeven periode. (Gebruik hiervoor de eigenschap vernieuwen Token maximale niet-actieve tijd.) U kunt beleid gebruiken om in te stellen van een periode waarboven het vernieuwen van tokens niet meer worden geaccepteerd. (Gebruik hiervoor de eigenschap Token Max Age vernieuwen.) De levensduur van een vernieuwingstoken om te bepalen wanneer en hoe vaak de gebruiker is vereist in plaats van de achtergrond worden geverifieerd, bij gebruik van een openbare clienttoepassing de referenties opnieuw in te voeren, kunt u aanpassen.

### <a name="id-tokens"></a>ID-tokens
ID-tokens worden doorgegeven aan websites en systeemeigen clients. ID-tokens profielinformatie bevatten over een gebruiker. Een token ID is gebonden aan een specifieke combinatie van client en de gebruiker. ID-tokens worden beschouwd als geldig totdat ze aflopen. Meestal een gebruiker in een webtoepassing overeenkomt met de levensduur van de sessie in de toepassing op de levensduur van het token ID uitgegeven voor de gebruiker. U kunt de levensduur van een token ID om te bepalen hoe vaak de webtoepassing de toepassingssessie is verlopen, en hoe vaak moet de gebruiker te worden geverifieerd met Azure AD (achtergrond of interactief).

### <a name="single-sign-on-session-tokens"></a>Tokens van de sessie voor eenmalige aanmelding
Wanneer een gebruiker wordt geverifieerd met Azure AD, één aanmelding sessie (SSO) tot stand is gebracht met de browser van de gebruiker en de Azure AD. Het token eenmalige aanmelding in de vorm van een cookie vertegenwoordigt deze sessie. Houd er rekening mee dat het sessietoken eenmalige aanmelding niet is gebonden aan een specifieke bron/client-toepassing. SSO-sessie tokens kunnen worden ingetrokken en de geldigheid gecontroleerd telkens wanneer ze worden gebruikt.

Azure AD maakt gebruik van twee soorten SSO sessie tokens: persistent en niet-persistente. Permanente sessie tokens worden opgeslagen als permanente cookies door de browser. Niet-persistente sessie tokens worden opgeslagen als sessiecookies. (Sessiecookies worden vernietigd bij de browser wordt gesloten.) Normaal gesproken wordt een niet-persistente sessietoken opgeslagen. Maar wanneer de gebruiker selecteert de **aangemeld blijven** selectievakje tijdens verificatie wordt een permanente sessietoken is opgeslagen.

Niet-persistente sessie tokens hebben een levensduur van 24 uur. Permanente tokens hebben een levensduur van 180 dagen. Elk gewenst moment die een sessietoken SSO wordt gebruikt binnen de geldigheidsperiode wordt de geldigheidsperiode uitgebreid andere 24 uur of 180 dagen, afhankelijk van het type token. Als een SSO-sessietoken niet binnen de geldigheidsperiode wordt gebruikt, is er sprake verlopen en wordt niet meer geaccepteerd.

U kunt een beleid gebruiken om in te stellen van de tijd nadat de eerste sessietoken is uitgegeven afgezien van dat het sessietoken niet meer geaccepteerd. (Gebruik hiervoor de eigenschap sessie Token Max Age.) U kunt de levensduur van een sessietoken om te bepalen wanneer en hoe vaak een gebruiker vereist is voor het invoeren van referenties in plaats van de achtergrond wordt geverifieerd, wanneer u een webtoepassing aanpassen.

### <a name="token-lifetime-policy-properties"></a>Eigenschappen van de levensduur van token-beleid
Een beleid van de levensduur van token is een soort beleidsobject dat de levensduur van token regels bevat. Gebruik de eigenschappen van het beleid om opgegeven token levensduur. Als er geen beleid is ingesteld, wordt de standaardwaarde van de levensduur afgedwongen door het systeem.

### <a name="configurable-token-lifetime-properties"></a>Eigenschappen van de configureerbare levensduur van token
| Eigenschap | Tekenreeks van de eigenschap beleid | Is van invloed op | Standaard | Minimum | Maximum |
| --- | --- | --- | --- | --- | --- |
| Levensduur van Token toegang |AccessTokenLifetime |Toegangstokens, ID-tokens, SAML2-tokens |1 uur |10 minuten |1 dag |
| Token maximale niet-actieve tijd van vernieuwen |MaxInactiveTime |Vernieuwen van tokens |het is 14 dagen |10 minuten |90 dagen |
| Token maximumleeftijd één Factor vernieuwen |MaxAgeSingleFactor |Vernieuwen van tokens (voor alle gebruikers) |Pas ingetrokken |10 minuten |Pas ingetrokken<sup>1</sup> |
| Token maximumleeftijd meerledige vernieuwen |MaxAgeMultiFactor |Vernieuwen van tokens (voor alle gebruikers) |Pas ingetrokken |10 minuten |Pas ingetrokken<sup>1</sup> |
| Token maximumleeftijd één Factor-sessie |MaxAgeSessionSingleFactor<sup>2</sup> |Sessie-tokens (persistent en niet-persistente) |Pas ingetrokken |10 minuten |Pas ingetrokken<sup>1</sup> |
| Token maximumleeftijd multi-factor-sessie |MaxAgeSessionMultiFactor<sup>3</sup> |Sessie-tokens (persistent en niet-persistente) |Pas ingetrokken |10 minuten |Pas ingetrokken<sup>1</sup> |

* <sup>1</sup>365 dagen is de maximale expliciete lengte voor deze kenmerken kan worden ingesteld.
* <sup>2</sup>als **MaxAgeSessionSingleFactor** is niet ingesteld, gaat deze waarde voor de **MaxAgeSingleFactor** waarde. Als geen van beide parameter is ingesteld, wordt de eigenschap de standaardwaarde (tot ingetrokken).
* <sup>3</sup>als **MaxAgeSessionMultiFactor** is niet ingesteld, gaat deze waarde voor de **MaxAgeMultiFactor** waarde. Als geen van beide parameter is ingesteld, wordt de eigenschap de standaardwaarde (tot ingetrokken).

### <a name="exceptions"></a>Uitzonderingen
| Eigenschap | Is van invloed op | Standaard |
| --- | --- | --- |
| Token Max Age vernieuwen (uitgegeven voor federatieve gebruikers die beschikken over onvoldoende intrekkingsgegevens<sup>1</sup>) |Vernieuwen van tokens (uitgegeven voor federatieve gebruikers die beschikken over onvoldoende intrekkingsgegevens<sup>1</sup>) |12 uur |
| Vernieuwen van Token maximale niet-actieve tijd (uitgegeven voor vertrouwelijke clients) |Vernieuwen van tokens (uitgegeven voor vertrouwelijke clients) |90 dagen |
| Vernieuwen van Token Max Age (uitgegeven voor vertrouwelijke clients) |Vernieuwen van tokens (uitgegeven voor vertrouwelijke clients) |Pas ingetrokken |

* <sup>1</sup>federatieve gebruikers die beschikken over onvoldoende intrekkingsgegevens opnemen alle gebruikers die geen het kenmerk 'LastPasswordChangeTimestamp' is gesynchroniseerd hebben. Deze gebruikers deze korte Max Age zijn opgegeven omdat AAD kan niet worden geverifieerd wanneer voor het intrekken van tokens die zijn gekoppeld aan een referentie voor de oude (zoals een wachtwoord dat is gewijzigd) en moeten controleren in meer vaak om ervoor te zorgen dat de gebruiker en de bijbehorende tokens nog steeds in goede  permanent. Als u wilt deze ervaring te verbeteren, moet tenantbeheerders ervoor te zorgen dat ze het 'LastPasswordChangeTimestamp'-kenmerk (deze kan worden ingesteld op het gebruikersobject met Powershell of via AADSync) synchroniseert.

### <a name="policy-evaluation-and-prioritization"></a>Evaluatie van het beleid en prioriteitsaanduiding
U kunt maken en vervolgens een levensduur van token beleid toewijzen aan een specifieke toepassing, uw organisatie en service-principals. Meerdere beleidsregels mogelijk van toepassing zijn op een specifieke toepassing. De levensduur van token-beleid dat van kracht volgt deze regels:

* Als een beleid expliciet aan de service-principal is toegewezen, wordt het afgedwongen.
* Als er geen beleid expliciet aan de service-principal is toegewezen, wordt een expliciet is toegewezen aan de bovenliggende organisatie van de service-principal-beleid afgedwongen.
* Als er geen beleid expliciet is toegewezen aan de service-principal of aan de organisatie, wordt het beleid dat is toegewezen aan de toepassing afgedwongen.
* Als er geen beleid is toegewezen aan de service-principal, de organisatie of het toepassingsobject, worden de standaardwaarden wordt afgedwongen. (Zie de tabel in [configureerbare levensduur van token eigenschappen](#configurable-token-lifetime-properties).)

Zie voor meer informatie over de relatie tussen toepassingsobjecten en service-principals [toepassing en service-principal objecten in Azure Active Directory](active-directory-application-objects.md).

De geldigheid van het token wordt geëvalueerd op het moment dat het token wordt gebruikt. Het beleid met de hoogste prioriteit voor de toepassing die wordt benaderd wordt van kracht.

> [!NOTE]
> Hier volgt een voorbeeldscenario.
>
> Een gebruiker toegang wil tot twee webtoepassingen: webtoepassing A en B. voor Web-toepassing
> 
> Factoren:
> * Beide webtoepassingen zijn in dezelfde bovenliggende organisatie.
> * Token levensduur beleid 1 met een sessie Token Max Age van acht uur is ingesteld als de bovenliggende organisatie standaard.
> * Webtoepassing A is een webtoepassing reguliere-gebruik en is niet gekoppeld aan alle beleidsregels.
> * Web Application B wordt gebruikt voor zeer gevoelige processen. De service-principal is gekoppeld aan Token levensduur beleid 2, waarmee een sessie Token Max Age van 30 minuten is.
>
> 12:00 uur, de gebruiker een nieuwe browsersessie begint en probeert te krijgen tot de webtoepassing A. De gebruiker wordt omgeleid naar Azure AD en wordt gevraagd aan te melden. Hiermee maakt u een cookie met een sessietoken in de browser. De gebruiker wordt omgeleid naar de webtoepassing een met een token ID waarmee de gebruiker toegang tot de toepassing.
>
> Op 12:15 uur, wordt de gebruiker probeert toegang tot Web Application B. De browser wordt omgeleid naar Azure AD de sessiecookie gedetecteerd. Web Application B service-principal is gekoppeld aan Token levensduur beleid 2, maar het is ook deel uit van de bovenliggende organisatie, met standaard Token levensduur beleid 1. Token levensduur beleid 2 wordt van kracht omdat beleidsregels die zijn gekoppeld aan de service-principals een hogere prioriteit dan de organisatie standaardbeleid hebben. Het sessietoken is oorspronkelijk heeft uitgegeven binnen de laatste 30 minuten, zodat als geldig wordt beschouwd. De gebruiker wordt omgeleid naar Web Application B met een ID-token die toegang verleent.
>
> Op 1:00 uur, wordt de gebruiker probeert te krijgen tot de webtoepassing A. De gebruiker wordt omgeleid naar Azure AD. Web Application een is niet gekoppeld aan alle beleidsregels, maar omdat deze zich in een organisatie met een standaard Token levensduur beleid 1, wordt dat beleid van kracht. De sessiecookie die oorspronkelijk heeft uitgegeven in de afgelopen acht uur wordt gedetecteerd. De gebruiker achtergrond weer omgeleid naar de webtoepassing een aan het token van een nieuwe ID. De gebruiker is niet vereist om te verifiëren.
>
> Onmiddellijk daarna wordt probeert de gebruiker toegang tot Web Application B. De gebruiker wordt omgeleid naar Azure AD. Als wordt voordat, Token levensduur beleid 2 van kracht. Omdat het token meer dan dertig minuten geleden uitgegeven is, wordt de gebruiker gevraagd hun referenties opnieuw in te voeren. Een gloednieuw sessietoken en token ID worden uitgegeven. De gebruiker vervolgens toegang tot Web Application B.
>
>

## <a name="configurable-policy-property-details"></a>Details van de eigenschap configureerbaar beleid
### <a name="access-token-lifetime"></a>Levensduur van Token toegang
**Tekenreeks:** AccessTokenLifetime

**Is van invloed op:** toegangstokens, ID-tokens

**Overzicht:** dit beleid bepaalt hoe lang toegang en ID-tokens voor deze resource als geldig beschouwd. De levensduur van Token toegang eigenschap verminderen vermindert het risico van een toegangstoken of de ID-token wordt gebruikt door een kwaadwillende actor gedurende langere tijd. (Deze tokens kunnen niet worden ingetrokken.) De verhouding is dat prestaties nadelig worden beïnvloed, omdat de tokens moeten vaker worden vervangen.

### <a name="refresh-token-max-inactive-time"></a>Token maximale niet-actieve tijd van vernieuwen
**Tekenreeks:** MaxInactiveTime

**Is van invloed op:** Vernieuwingstokens

**Overzicht:** dit beleid bepaalt hoe oud een vernieuwingstoken mag zijn voordat een client kan niet meer kan gebruiken voor het ophalen van een nieuw token toegang/vernieuwen paar bij een poging tot toegang tot deze resource. Omdat een nieuwe vernieuwingstoken meestal wordt geretourneerd als een vernieuwingstoken wordt gebruikt, heeft dit beleid voorkomt dat toegang als de client probeert toegang tot alle bronnen met behulp van het huidige vernieuwingstoken tijdens de opgegeven periode.

Dit beleid zorgt ervoor dat gebruikers die niet op de client te verifiëren voor het ophalen van een nieuwe vernieuwingstoken actief zijn geweest.

De eigenschap vernieuwen Token maximale niet-actieve tijd moet worden ingesteld op een lagere waarde dan de één-Factor Token Max Age en de eigenschappen van meerdere factoren vernieuwen Token Max Age.

### <a name="single-factor-refresh-token-max-age"></a>Token maximumleeftijd één Factor vernieuwen
**Tekenreeks:** MaxAgeSingleFactor

**Is van invloed op:** Vernieuwingstokens

**Overzicht:** deze beleid bepaalt hoe lang een gebruiker een vernieuwingstoken een nieuw token toegang/vernieuwen paar ontvangen nadat deze laatste is geverifieerd met behulp van slechts één factor kan gebruiken. Nadat een gebruiker wordt geverifieerd en een nieuwe vernieuwingstoken ontvangt, kan de gebruiker de tokenstroom vernieuwen gebruiken voor de opgegeven periode. (Dit geldt zolang het huidige vernieuwingstoken dat niet is ingetrokken en wordt niet langer dan de inactieve tijd ongebruikte blijven.) Op dat moment geforceerd op de gebruiker te verifiëren voor het ontvangen van een nieuw vernieuwingstoken.

De maximale leeftijd verminderen zorgt ervoor dat gebruikers worden geverifieerd vaker. Omdat authenticatie wordt beschouwd als minder veilig dan multi-factor authentication-server, wordt u aangeraden dat u deze eigenschap instellen op een waarde die gelijk is aan of kleiner zijn dan de eigenschap Multi-factor vernieuwen Token Max Age.

### <a name="multi-factor-refresh-token-max-age"></a>Token maximumleeftijd meerledige vernieuwen
**Tekenreeks:** MaxAgeMultiFactor

**Is van invloed op:** Vernieuwingstokens

**Overzicht:** deze beleid bepaalt hoe lang een gebruiker een vernieuwingstoken kunt gebruiken om een nieuw token toegang/vernieuwen paar nadat deze laatste is geverifieerd met behulp van meerdere factoren. Nadat een gebruiker wordt geverifieerd en een nieuwe vernieuwingstoken ontvangt, kan de gebruiker de tokenstroom vernieuwen gebruiken voor de opgegeven periode. (Dit geldt zolang het huidige vernieuwingstoken dat niet is ingetrokken en niet langer dan de inactieve tijd ongebruikte is.) Op dat moment worden gebruikers gedwongen om andere referenties voor het ontvangen van een nieuw vernieuwingstoken.

De maximale leeftijd verminderen zorgt ervoor dat gebruikers worden geverifieerd vaker. Omdat authenticatie wordt beschouwd als minder veilig dan multi-factor authentication-server, wordt u aangeraden dat u deze eigenschap instellen op een waarde die gelijk is aan of groter zijn dan de eigenschap één Factor vernieuwen Token Max Age.

### <a name="single-factor-session-token-max-age"></a>Token maximumleeftijd één Factor-sessie
**Tekenreeks:** MaxAgeSessionSingleFactor

**Is van invloed op:** sessie tokens (persistent en niet-persistente)

**Overzicht:** deze beleid bepaalt hoe lang een gebruiker een sessietoken kunt gebruiken om een nieuwe ID en het sessietoken nadat deze laatste is geverifieerd met behulp van slechts één beveiligingsfactor. Nadat een gebruiker wordt geverifieerd en een nieuwe sessietoken ontvangt, kan de gebruiker kan de sessie-tokenstroom gebruiken voor de opgegeven periode. (Dit geldt zolang het token van de huidige sessie niet is ingetrokken en niet is verlopen.) Na de opgegeven periode, wordt de gebruiker moet om andere referenties voor het ontvangen van een nieuwe sessie-token.

De maximale leeftijd verminderen zorgt ervoor dat gebruikers worden geverifieerd vaker. Omdat authenticatie wordt beschouwd als minder veilig dan multi-factor authentication-server, wordt u aangeraden dat u deze eigenschap instellen op een waarde die gelijk is aan of kleiner zijn dan de eigenschap Multi-factor sessie Token Max Age.

### <a name="multi-factor-session-token-max-age"></a>Token maximumleeftijd multi-factor-sessie
**Tekenreeks:** MaxAgeSessionMultiFactor

**Is van invloed op:** sessie tokens (persistent en niet-persistente)

**Overzicht:** deze beleid bepaalt hoe lang een gebruiker kan een sessietoken gebruiken een nieuwe ID en een sessie-token ophalen na de laatste keer dat ze de verificatie is voltooid met behulp van meerdere factoren. Nadat een gebruiker wordt geverifieerd en een nieuwe sessietoken ontvangt, kan de gebruiker kan de sessie-tokenstroom gebruiken voor de opgegeven periode. (Dit geldt zolang het token van de huidige sessie niet is ingetrokken en niet is verlopen.) Na de opgegeven periode, wordt de gebruiker moet om andere referenties voor het ontvangen van een nieuwe sessie-token.

De maximale leeftijd verminderen zorgt ervoor dat gebruikers worden geverifieerd vaker. Omdat authenticatie wordt beschouwd als minder veilig dan multi-factor authentication-server, wordt u aangeraden dat u deze eigenschap instellen op een waarde die gelijk is aan of groter zijn dan de eigenschap één Factor sessie Token Max Age.

## <a name="example-token-lifetime-policies"></a>Voorbeeld van de levensduur van token-beleid
Veel scenario's zijn mogelijk in Azure AD wanneer u kunt maken en beheren van token levensduur voor apps, service-principals en uw hele organisatie zijn. In deze sectie doorlopen we enkele algemene scenario's voor beleid waarmee u nieuwe regels voor opleggen kunnen:

* Levensduur van token
* Token maximale niet-actieve tijd
* Token maximumleeftijd

In de voorbeelden, leert u hoe:

* Standaardbeleid voor een organisatie beheren
* Maak een beleid voor aanmelding bij de webpagina
* Maak een beleid voor een native-app die een web-API-aanroepen
* Een geavanceerde beleid beheren

### <a name="prerequisites"></a>Vereisten
In de volgende voorbeelden u maken, bijwerken, koppelen en verwijderen van beleidsregels voor apps, service-principals en uw hele organisatie. Als u niet bekend met Azure AD bent, raden wij aan dat u meer informatie over [een Azure AD-tenant verkrijgen](active-directory-howto-tenant.md) voordat u met deze voorbeelden doorgaat.  

Voer de volgende stappen uit om te beginnen:

1. Download de meest recente [Azure AD PowerShell-Module Public Preview-versie](https://www.powershellgallery.com/packages/AzureADPreview).
2. Voer de `Connect` opdracht aan te melden bij uw Azure AD-beheeraccount. Deze opdracht uitvoeren telkens wanneer starten u een nieuwe sessie.

    ```PowerShell
    Connect-AzureAD -Confirm
    ```

3. Overzicht van alle beleidsregels die zijn gemaakt in uw organisatie, moet u de volgende opdracht uitvoeren. Deze opdracht uitvoeren nadat de meeste bewerkingen in de volgende scenario's. Ook de opdracht uit te voeren krijgt u de ** ** van uw beleid.

    ```PowerShell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a>Voorbeeld: Standaardbeleid voor een organisatie beheren
In dit voorbeeld maakt u een beleid waarmee u uw gebruikers zich aanmelden minder vaak in uw hele organisatie. U doet dit door een levensduur van token-beleid voor één Factor vernieuwen beveiligingstokens die wordt toegepast in uw organisatie te maken. Het beleid wordt toegepast op elke toepassing in uw organisatie, en voor elke service principal die nog niet over een beleid is ingesteld.

1. Maak een levensduur van token-beleid.

    1.  Stel het Vernieuwingstoken dat één Factor "totdat-ingetrokken." Het token verloopt niet totdat de toegang is ingetrokken. De volgende beleidsdefinitie maken:

        ```PowerShell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    2.  Voer de volgende opdracht voor het maken van het beleid:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    3.  Om het nieuwe beleid en het beleid **ObjectId**, voer de volgende opdracht:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Bijwerken van het beleid.

    U kunt besluiten het eerste beleid dat u in dit voorbeeld instelt is niet zo streng als uw service vereist. Om in te stellen uw één Factor vernieuwen Token verloopt in twee dagen, moet u de volgende opdracht uitvoeren:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId FROM GET COMMAND> -DisplayName "OrganizationDefaultPolicyUpdatedScenario" -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a>Voorbeeld: Maak een beleid voor aanmelding bij de webpagina

In dit voorbeeld maakt u een beleid waarbij gebruikers om te verifiëren vaker in uw web-app. Dit beleid wordt de levensduur van de tokens toegangs-ID en de maximale leeftijd van een meervoudige sessietoken ingesteld op de service-principal van uw web-app.

1. Maak een levensduur van token-beleid.

    Dit beleid voor web aanmelden, stelt de toegang/ID levensduur van token en de maximale één factor sessie token leeftijd tot twee uur.

    1.  Voor het maken van het beleid, moet u deze opdracht uitvoeren:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  Om het nieuwe beleid en het beleid **ObjectId**, voer de volgende opdracht:

        ```PowerShell
        Get-AzureADPolicy
        ```

2.  Het beleid aan uw service-principal toewijzen. Ook moet u de **ObjectId** van uw service-principal. 

    1.  Overzicht van de service-principals van uw organisatie, kunt u een query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Of in [Explorer van Azure AD Graph](https://graphexplorer.cloudapp.net/), zich aanmelden bij uw Azure AD-account.

    2.  Wanneer u hebt de **ObjectId** van uw service-principal, voer de volgende opdracht:

        ```PowerShell
        Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Voorbeeld: Maak een beleid voor een native-app die een web-API-aanroepen
In dit voorbeeld maakt u een beleid waarvoor gebruikers minder vaak te verifiëren. Het beleid, wordt de hoeveelheid tijd die een gebruiker inactief zijn mag voordat de gebruiker moet verifiëren ook langer. Het beleid wordt toegepast op de web-API. Wanneer de systeemeigen app de web-API als een bron aanvraagt, wordt dit beleid wordt toegepast.

1. Maak een levensduur van token-beleid.

    1.  Om een streng beleid voor een web-API maakt, moet u de volgende opdracht uitvoeren:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  Om het nieuwe beleid en het beleid **ObjectId**, voer de volgende opdracht:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Het beleid toewijzen aan uw web-API. Ook moet u de **ObjectId** van uw toepassing. De beste manier om uw app te zoeken **ObjectId** is het gebruik van de [Azure-portal](https://portal.azure.com/).

   Wanneer u hebt de **ObjectId** van uw app, voer de volgende opdracht:

        ```PowerShell
        Add-AzureADApplicationPolicy -Id <ObjectId of the Application> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-manage-an-advanced-policy"></a>Voorbeeld: Een geavanceerde beleid beheren
In dit voorbeeld maakt u een paar beleidsregels, voor meer informatie over de werking van het systeem prioriteit. U kunt leert ook hoe voor het beheren van meerdere beleidsregels die worden toegepast op meerdere objecten.

1. Maak een levensduur van token-beleid.

    1.  Voer de volgende opdracht voor het maken van een organisatie-standaardbeleid dat de levensduur van één Factor vernieuwen tokens ingesteld op 30 dagen:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2.  Om het nieuwe beleid en het beleid **ObjectId**, voer de volgende opdracht:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Het beleid toewijzen aan een service-principal.

    U hebt nu een beleid dat van toepassing op de hele organisatie. U kunt dit beleid 30 dagen voor een specifieke service-principal te behouden, maar het standaardbeleid organisatie wijzigen in de bovengrens van "tot ingetrokken."

    1.  Overzicht van de service-principals van uw organisatie, kunt u een query [Microsoft Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Of in [Explorer van Azure AD Graph](https://graphexplorer.cloudapp.net/), meld u aan met uw Azure AD-account.

    2.  Wanneer u hebt de **ObjectId** van uw service-principal, voer de volgende opdracht:

            ```PowerShell
            Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
            ```
        
3. Stel de `IsOrganizationDefault` vlag op false:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. Maak een nieuwe organisatie standaardbeleid:

    ```PowerShell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    U hebt nu het oorspronkelijke beleid dat is gekoppeld aan uw service-principal en het nieuwe beleid is ingesteld als het standaardbeleid voor uw organisatie. Het is belangrijk te weten dat beleid toegepast op service-principals de hebben voorrang boven de standaardbeleidsregels organisatie.

## <a name="cmdlet-reference"></a>Cmdlet-naslaginformatie

### <a name="manage-policies"></a>Beleid beheren

U kunt de volgende cmdlets gebruiken voor het beheren van beleid.

#### <a name="new-azureadpolicy"></a>Nieuwe AzureADPolicy

Maakt een nieuw beleid.

```PowerShell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Definition</code> |Matrix van stringified JSON die de regels van het beleid bevat. | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |Tekenreeks van de naam van het beleid. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |Indien waar, wordt u het beleid ingesteld als standaard-beleid van de organisatie. Als het ONWAAR is, gebeurt er niets. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |Type van het beleid. Token levensduur altijd gebruiken 'TokenLifetimePolicy'. | `-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[Optioneel] |Hiermee stelt u een alternatieve ID voor het beleid. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy
Hiermee haalt u alle beleidsregels voor Azure AD of een opgegeven beleid.

```PowerShell
Get-AzureADPolicy
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code>[Optioneel] |**Object-id (Id)** van het gewenste beleid. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadpolicyappliedobject"></a>Get-AzureADPolicyAppliedObject
Hiermee haalt u alle apps en service-principals die zijn gekoppeld aan een beleid.

```PowerShell
Get-AzureADPolicyAppliedObject -Id <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van het gewenste beleid. |`-Id <ObjectId of Policy>` |

</br></br>

#### <a name="set-azureadpolicy"></a>Set-AzureADPolicy
Een bestaand beleid wordt bijgewerkt.

```PowerShell
Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName <string>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van het gewenste beleid. |`-Id <ObjectId of Policy>` |
| <code>&#8209;DisplayName</code> |Tekenreeks van de naam van het beleid. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;Definition</code>[Optioneel] |Matrix van stringified JSON die de regels van het beleid bevat. |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;IsOrganizationDefault</code>[Optioneel] |Indien waar, wordt u het beleid ingesteld als standaard-beleid van de organisatie. Als het ONWAAR is, gebeurt er niets. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code>[Optioneel] |Type van het beleid. Token levensduur altijd gebruiken 'TokenLifetimePolicy'. |`-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code>[Optioneel] |Hiermee stelt u een alternatieve ID voor het beleid. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a>Verwijder AzureADPolicy
Hiermee verwijdert u het opgegeven beleid.

```PowerShell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van het gewenste beleid. | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a>Toepassingsbeleid
U kunt de volgende cmdlets gebruiken voor beleid voor toepassingen.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Voeg AzureADApplicationPolicy
Het opgegeven beleid is gekoppeld aan een toepassing.

```PowerShell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectId** van het beleid. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadapplicationpolicy"></a>Get-AzureADApplicationPolicy
Hiermee haalt u het beleid dat is toegewezen aan een toepassing.

```PowerShell
Get-AzureADApplicationPolicy -Id <ObjectId of Application>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadapplicationpolicy"></a>Verwijder AzureADApplicationPolicy
Een beleid verwijdert uit een toepassing.

```PowerShell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectId** van het beleid. | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a>Service-principal-beleidsregels
U kunt de volgende cmdlets gebruiken voor service-principal-beleid.

#### <a name="add-azureadserviceprincipalpolicy"></a>Voeg AzureADServicePrincipalPolicy
Het opgegeven beleid is gekoppeld aan een service-principal.

```PowerShell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**ObjectId** van het beleid. | `-RefObjectId <ObjectId of Policy>` |

</br></br>

#### <a name="get-azureadserviceprincipalpolicy"></a>Get-AzureADServicePrincipalPolicy
Hiermee haalt u een beleid dat is gekoppeld aan de opgegeven service-principal.

```PowerShell
Get-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |

</br></br>

#### <a name="remove-azureadserviceprincipalpolicy"></a>Verwijder AzureADServicePrincipalPolicy
Hiermee verwijdert u het beleid van de opgegeven service-principal.

```PowerShell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**ObjectId** van het beleid. | `-PolicyId <ObjectId of Policy>` |

---
title: Configureerbare levensduur van tokens in Azure Active Directory | Microsoft Docs
description: Informatie over het instellen van de levensduur van tokens die zijn uitgegeven door Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 06f5b317-053e-44c3-aaaa-cf07d8692735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin
ms.openlocfilehash: 1fa5a2f9d63dfd9af006285beec256395d7ac668
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069502"
---
# <a name="configurable-token-lifetimes-in-azure-active-directory-preview"></a>Configureerbare levensduur van tokens in Azure Active Directory (Preview)

U kunt de levensduur van een token dat is uitgegeven door Azure Active Directory (Azure AD) opgeven. U kunt de levensduur van tokens voor alle apps in uw organisatie, voor een toepassing met meerdere tenants (voor meerdere organisaties) of voor een specifieke service-principal instellen in uw organisatie.

> [!IMPORTANT]
> Na de behandeling van klanten tijdens de Preview-versie wij van plan bent deze functionaliteit vervangen door een nieuwe functie in Azure Active Directory voor voorwaardelijke toegang.  Zodra de nieuwe functie voltooid is, wordt deze functionaliteit uiteindelijk worden afgeschaft na de meldingsperiode van een.  Als u het beleid configureerbare levensduur van tokens, worden voorbereid om over te schakelen op de nieuwe functie voor voorwaardelijke toegang, zodra deze beschikbaar is. 

Een groepsbeleidsobject vertegenwoordigt in Azure AD een reeks regels die op afzonderlijke toepassingen of op alle toepassingen in een organisatie worden afgedwongen. Elk beleidstype heeft een unieke structuur, met een set eigenschappen die worden toegepast op objecten die ze zijn toegewezen.

U kunt een beleid opgeven als het standaardbeleid voor uw organisatie. Het beleid wordt toegepast op elke toepassing in de organisatie, zolang deze niet is overschreven door een beleid met een hogere prioriteit. U kunt ook een beleid toewijzen aan specifieke toepassingen. De volgorde van prioriteit varieert per beleidstype.

> [!NOTE]
> Configureerbare levensduur van vernieuwingstoken beleid wordt niet ondersteund voor SharePoint Online.  Hoewel u de mogelijkheid om te maken van dit beleid via PowerShell hebt, wordt SharePoint Online niet meer op dit beleid. Raadpleeg de [SharePoint Online-blog](https://techcommunity.microsoft.com/t5/SharePoint-Blog/Introducing-Idle-Session-Timeout-in-SharePoint-and-OneDrive/ba-p/119208) voor meer informatie over het configureren van niet-actieve sessies time-outs.
>* De standaardlevensduur voor de SharePoint Online-toegangstoken is 1 uur. 
>* De standaard maximale niet-actieve tijd van de SharePoint Online vernieuwingstoken is 90 dagen.

## <a name="token-types"></a>Typen van token

U kunt de levensduur van vernieuwingstoken-beleid voor het vernieuwen van tokens, toegangstokens, sessie-tokens en ID-tokens instellen.

### <a name="access-tokens"></a>Toegangstokens

Clients gebruiken de toegangstokens voor toegang tot een beveiligde bron. Een toegangstoken kan alleen worden gebruikt voor een specifieke combinatie van gebruikers-, client- en. Toegangstokens kunnen niet worden ingetrokken en zijn geldig tot de vervaldatum. Een kwaadwillende actor die is verkregen van een toegangstoken kunt gebruiken voor de omvang van hun levensduur. Aanpassen van de levensduur van een toegangstoken is een compromis tussen de systeemprestaties verbeteren en het vergroten van de hoeveelheid tijd dat de client toegang behouden nadat het gebruikersaccount is uitgeschakeld. Verbeterde prestaties wordt bereikt door het beperken van het aantal keren dat die een client moet een nieuwe toegangstoken verkrijgen.  De standaardwaarde is 1 uur - na 1 uur, de client het vernieuwingstoken (meestal op de achtergrond) een nieuwe vernieuwingstoken ophalen en toegangstoken moet gebruiken. 

### <a name="refresh-tokens"></a>Vernieuwen van tokens
Wanneer een client een toegangstoken verkrijgt voor toegang tot een beveiligde bron, ontvangt de client ook een vernieuwingstoken. Het vernieuwingstoken dat wordt gebruikt voor het verkrijgen van toegang/vernieuwen van nieuwe token paren wanneer het huidige toegangstoken is verlopen. Een vernieuwingstoken is gebonden aan een combinatie van gebruiker en de client. Een vernieuwingstoken kan worden [ingetrokken op elk gewenst moment](access-tokens.md#token-revocation), en de geldigheid van het token is ingeschakeld telkens wanneer het token wordt gebruikt.  

Het is belangrijk dat u onderscheid maken tussen vertrouwelijke clients en openbare clients, als dit van invloed op hoe lang vernieuwingstokens kunnen worden gebruikt. Zie voor meer informatie over de verschillende soorten clients [RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.1).

#### <a name="token-lifetimes-with-confidential-client-refresh-tokens"></a>Levensduur van tokens met vertrouwelijke client vernieuwingstokens
Vertrouwelijke clients zijn toepassingen die u kunnen veilig opslaan van een wachtwoord voor de client (geheim genoemd). Ze kunnen aantonen dat verzoeken die worden verzonden vanaf de beveiligde clienttoepassing en niet vanaf een schadelijke actor. Een web-app is bijvoorbeeld een vertrouwelijke client omdat deze een clientgeheim kunt opslaan op de webserver. Het is niet beschikbaar gemaakt. Omdat deze stromen worden beter te beveiligen, is het de standaardlevensduur van vernieuwingstokens verleend aan deze stromen `until-revoked`, met behulp van beleid kan niet worden gewijzigd en zal niet worden ingetrokken op vrijwillige wachtwoord opnieuw instellen van wachtwoorden.

#### <a name="token-lifetimes-with-public-client-refresh-tokens"></a>Levensduur van tokens met openbare client vernieuwingstokens

Openbare clients kunnen niet veilig opslaan van een wachtwoord voor de client (geheim genoemd). Bijvoorbeeld, een iOS/Android-app niet kan, onleesbaar maakt een geheim van de resource-eigenaar, zodat deze wordt beschouwd als een openbare client. U kunt beleid instellen voor resources om te voorkomen dat vernieuwingstokens van openbare clients die ouder zijn dan een opgegeven periode geen nieuw paar van de toegang/vernieuwen-token verkrijgen. (Om dit te doen, gebruikt u de eigenschap Refresh Token maximale niet-actieve tijd (`MaxInactiveTime`).) U kunt ook beleidsregels gebruiken om in te stellen van een bepaalde waarboven het vernieuwen van tokens niet meer worden geaccepteerd. (Gebruik hiervoor de eigenschap Token Max-Age vernieuwen.) U kunt de levensduur van een vernieuwingstoken om te bepalen wanneer en hoe vaak de gebruiker is vereist om te opnieuw invoeren van referenties, in plaats van op de achtergrond worden geverifieerd, bij het gebruik van een openbare client-toepassing kunt aanpassen.

### <a name="id-tokens"></a>Id-tokens
ID-tokens worden doorgegeven aan de websites en systeemeigen clients. ID-tokens profielinformatie bevatten over een gebruiker. Een ID-token dat is gebonden aan een specifieke combinatie van client en de gebruiker. ID-tokens worden beschouwd als geldig tot de vervaldatum. Meestal een gebruiker in een webtoepassing overeenkomt met de levensduur van de sessie in de toepassing op de levensduur van de ID-token dat is uitgegeven voor de gebruiker. U kunt de levensduur van een ID-token om te bepalen hoe vaak de web-App verloopt de toepassingssessie, en hoe vaak moet de gebruiker te worden geverifieerd met Azure AD (op de achtergrond of interactief).

### <a name="single-sign-on-session-tokens"></a>Tokens van de sessie voor eenmalige aanmelding
Wanneer een gebruiker wordt geverifieerd met Azure AD, één aanmelding sessie (SSO) tot stand is gebracht met de browser van de gebruiker en de Azure AD. De SSO-token in de vorm van een cookie vertegenwoordigt deze sessie. Houd er rekening mee dat het sessietoken van eenmalige aanmelding niet is gebonden aan een specifieke resource/client-toepassing. SSO-sessie tokens kunnen worden ingetrokken en wordt de geldigheid is ingeschakeld telkens wanneer ze worden gebruikt.

Azure AD maakt gebruik van twee soorten SSO sessie tokens: permanente en niet-persistente. Permanente sessie tokens worden opgeslagen als permanente cookies door de browser. Niet-persistente sessie tokens worden opgeslagen als sessiecookies. (Sessiecookies worden verwijderd wanneer de browser wordt gesloten.) Normaal gesproken wordt een niet-persistente sessietoken opgeslagen. Maar wanneer de gebruiker selecteert de **aangemeld blijven** selectievakje tijdens verificatie wordt een permanente sessietoken wordt opgeslagen.

Niet-persistente sessie tokens hebben een levensduur van 24 uur. Permanente tokens hebben een levensduur van 180 dagen. Telkens wanneer die een sessie SSO-token wordt gebruikt binnen de geldigheidsperiode is de geldigheidsperiode uitgebreid andere 24 uur of 180 dagen, afhankelijk van het type token. Als een sessie SSO-token niet binnen de geldigheidsperiode wordt gebruikt, is er sprake is verlopen en wordt niet meer geaccepteerd.

U kunt een beleid gebruiken om in te stellen van de tijd na de eerste sessietoken is uitgegeven dan dat het sessietoken niet meer wordt geaccepteerd. (Gebruik hiervoor de eigenschap sessie Token Max-Age.) U kunt de levensduur van een sessietoken om te bepalen wanneer en hoe vaak een gebruiker vereist voor het invoeren van referenties is, in plaats van op de achtergrond wordt geverifieerd, bij het gebruik van een web-App kunt aanpassen.

### <a name="token-lifetime-policy-properties"></a>Eigenschappen van beleid voor de levensduur van token
Een beleid voor de levensduur van vernieuwingstoken is een type beleidsobject dat de levensduur van vernieuwingstoken regels bevat. De eigenschappen van het beleid gebruiken voor het beheren van opgegeven levensduur van tokens. Als er geen beleid is ingesteld, wordt de standaardwaarde voor de levensduur van afgedwongen door het systeem.

### <a name="configurable-token-lifetime-properties"></a>Configureerbare levensduur van vernieuwingstoken eigenschappen
| Eigenschap | Tekenreeks van de eigenschap beleid | Is van invloed op | Standaard | Minimum | Maximum |
| --- | --- | --- | --- | --- | --- |
| Toegang tot de levensduur van Token |AccessTokenLifetime |Toegangstokens, ID-tokens, SAML2-tokens |1 uur |10 minuten |1 dag |
| Token maximale niet-actieve tijd van vernieuwen |MaxInactiveTime |Vernieuwen van tokens |90 dagen |10 minuten |90 dagen |
| Maximumleeftijd voor Token-één-Factor vernieuwen |MaxAgeSingleFactor |Vernieuwen van tokens (voor alle gebruikers) |Until-revoked |10 minuten |Until-revoked<sup>1</sup> |
| Token maximumleeftijd van meerdere factoren vernieuwen |MaxAgeMultiFactor |Vernieuwen van tokens (voor alle gebruikers) |Until-revoked |10 minuten |Until-revoked<sup>1</sup> |
| Maximumleeftijd voor Token-sessie voor één van meerdere factoren |MaxAgeSessionSingleFactor<sup>2</sup> |Sessie-tokens (persistent en niet-persistente) |Until-revoked |10 minuten |Until-revoked<sup>1</sup> |
| Maximumleeftijd voor Token-sessie van meerdere factoren |MaxAgeSessionMultiFactor<sup>3</sup> |Sessie-tokens (persistent en niet-persistente) |Until-revoked |10 minuten |Until-revoked<sup>1</sup> |

* <sup>1</sup>365 dagen is de expliciete maximumlengte die kan worden ingesteld voor deze kenmerken.
* <sup>2</sup>als **MaxAgeSessionSingleFactor** is niet ingesteld, gaat deze waarde voor de **MaxAgeSingleFactor** waarde. Als geen van beide parameter is ingesteld, wordt de eigenschap de standaardwaarde (tot en met ingetrokken).
* <sup>3</sup>als **MaxAgeSessionMultiFactor** is niet ingesteld, gaat deze waarde voor de **MaxAgeMultiFactor** waarde. Als geen van beide parameter is ingesteld, wordt de eigenschap de standaardwaarde (tot en met ingetrokken).

### <a name="exceptions"></a>Uitzonderingen
| Eigenschap | Is van invloed op | Standaard |
| --- | --- | --- |
| Token Max Age vernieuwen (uitgegeven voor federatieve gebruikers die beschikken over onvoldoende intrekkingsgegevens<sup>1</sup>) |Vernieuwen van tokens (uitgegeven voor federatieve gebruikers die beschikken over onvoldoende intrekkingsgegevens<sup>1</sup>) |12 uur |
| Refresh Token maximale niet-actieve tijd (uitgegeven voor vertrouwelijke clients) |Vernieuwen van tokens (uitgegeven voor vertrouwelijke clients) |90 dagen |
| Refresh Token Max Age (uitgegeven voor vertrouwelijke clients) |Vernieuwen van tokens (uitgegeven voor vertrouwelijke clients) |Until-revoked |

* <sup>1</sup>federatieve gebruikers die beschikken over onvoldoende intrekkingsgegevens bevatten alle gebruikers die geen het kenmerk "LastPasswordChangeTimestamp" is gesynchroniseerd hebben. Deze gebruikers krijgen deze korte Max Age omdat AAD kan niet worden geverifieerd bij het intrekken van tokens die zijn gekoppeld aan een oude referentie (zoals een wachtwoord dat is gewijzigd) en moeten controleren in meer regelmatig om ervoor te zorgen dat de gebruiker en de bijbehorende tokens nog steeds in goede  permanent. Als u wilt deze ervaring te verbeteren, moet tenantbeheerders ervoor te zorgen dat ze zijn de synchroniseert 'LastPasswordChangeTimestamp' (dit kan worden ingesteld op het gebruikersobject met behulp van Powershell of via AADSync).

### <a name="policy-evaluation-and-prioritization"></a>Evaluatie van het beleid en prioriteitsaanduiding
U kunt maken en vervolgens een levensduur van vernieuwingstoken beleid toewijzen aan een specifieke toepassing, voor uw organisatie en service-principals. Meerdere beleidsregels mogelijk van toepassing op een specifieke toepassing. De levensduur van token-beleid dat van kracht wordt met deze regels de volgende:

* Als een beleid expliciet is toegewezen aan de service-principal, wordt deze afgedwongen.
* Als er geen beleid expliciet is toegewezen aan de service-principal, wordt een expliciet is toegewezen aan de bovenliggende organisatie van de service-principal-beleid afgedwongen.
* Als er geen beleid expliciet is toegewezen aan de service-principal of aan de organisatie, wordt het beleid dat is toegewezen aan de toepassing wordt afgedwongen.
* Als er geen beleid is toegewezen aan de service-principal, de organisatie of het toepassingsobject, worden de standaardwaarden wordt afgedwongen. (Zie de tabel in [configureerbare levensduur van vernieuwingstoken eigenschappen](#configurable-token-lifetime-properties).)

Zie voor meer informatie over de relatie tussen toepassingsobjecten en service-principalobjecten [toepassing en service-principalobjecten in Azure Active Directory](app-objects-and-service-principals.md).

De geldigheid van een token wordt geëvalueerd op het moment dat het token wordt gebruikt. Het beleid met de hoogste prioriteit voor de toepassing die wordt benaderd wordt van kracht.

Alle timespans hier gebruikt zijn ingedeeld volgens de C# [TimeSpan](https://msdn.microsoft.com/library/system.timespan) object - D.HH:MM:SS.  Dus 80 dagen en 30 minuten is `80.00:30:00`.  De voorloopspaties D kan worden verwijderd als nul is, dus 90 minuten zou worden `00:90:00`.  

> [!NOTE]
> Hier volgt een voorbeeldscenario.
>
> Een gebruiker toegang wil tot twee web-apps: Web-App A en B. voor Web-toepassing
> 
> Factoren:
> * Beide webtoepassingen zijn in dezelfde bovenliggende organisatie.
> * Token 1 van de levensduur van beleid met een sessie Token Max Age van acht uur is van de organisatie van de bovenliggende standaard ingesteld.
> * Web-App A is een webtoepassing met normale gebruik en is niet gekoppeld aan alle beleidsregels.
> * Web Application B wordt gebruikt voor zeer gevoelige processen. De service-principal is gekoppeld aan het Token levensduur beleid 2, met een sessie Token Max Age van 30 minuten.
>
> Om 12:00 uur, de gebruiker start een nieuwe browsersessie en probeert te krijgen tot Web-App A. De gebruiker wordt omgeleid naar Azure AD en wordt gevraagd aan te melden. Hiermee maakt u een cookie die de sessietoken van een in de browser heeft. De gebruiker wordt omgeleid naar de webtoepassing een met een ID-token waarmee de gebruiker toegang tot de toepassing.
>
> Om 12:15 uur wil de gebruiker toegang tot Web Application B. De browser wordt omgeleid naar Azure AD, waarvan de sessiecookie detecteert. Web Application B service-principal is gekoppeld aan Token levensduur beleid 2, maar het is ook deel uit van de bovenliggende organisatie, met standaard Token levensduur beleid 1. Token levensduur beleid 2 wordt van kracht omdat beleidsregels die zijn gekoppeld aan de service-principals een hogere prioriteit dan de standaardbeleidsregels organisatie hebben. De sessietoken is oorspronkelijk heeft uitgegeven in de laatste 30 minuten, zodat als ongeldig wordt beschouwd. De gebruiker wordt omgeleid naar Web Application B met een ID-token die toegang verleent.
>
> 1:00 uur wil de gebruiker toegang tot Web-App A. De gebruiker wordt omgeleid naar Azure AD. Web Application A is niet gekoppeld aan alle beleidsregels, maar omdat het is in een organisatie met standaard Token levensduur beleid 1, wordt dat beleid van kracht. De sessiecookie die oorspronkelijk heeft uitgegeven in de afgelopen acht uur wordt gedetecteerd. De gebruiker wordt op de achtergrond omgeleid naar een-webtoepassing met een nieuwe ID-token. De gebruiker is niet vereist om te verifiëren.
>
> Direct daarna wordt probeert de gebruiker toegang tot Web Application B. De gebruiker wordt omgeleid naar Azure AD. Zoals eerder Token levensduur beleid 2 wordt van kracht. Omdat het token meer dan 30 minuten geleden uitgegeven is, wordt de gebruiker gevraagd hun referenties opnieuw in te voeren. Een gloednieuwe sessietoken en ID-token worden uitgegeven. De gebruiker kan vervolgens toegang tot Web Application B.
>
>

## <a name="configurable-policy-property-details"></a>Details van de eigenschap configureerbaar beleid
### <a name="access-token-lifetime"></a>Toegang tot de levensduur van Token
**Tekenreeks:** AccessTokenLifetime

**Is van invloed op:** toegangstokens, ID-tokens

**Overzicht:** dit beleid wordt bepaald hoe lang toegang en ID-tokens voor deze resource als geldig worden beschouwd. Vermindering van de levensduur van Vernieuwingstoken toegang eigenschap vermindert het risico van een toegangstoken of de ID-token dat wordt gebruikt door een kwaadwillende actor voor een lange periode. (Deze tokens kunnen niet worden ingetrokken.) De verhouding is dat is prestaties nadelig worden beïnvloed, omdat de tokens vaker worden vervangen.

### <a name="refresh-token-max-inactive-time"></a>Token maximale niet-actieve tijd van vernieuwen
**Tekenreeks:** MaxInactiveTime

**Is van invloed op:** Vernieuwingstokens

**Overzicht:** dit beleid wordt bepaald hoe oud een vernieuwingstoken kan zijn voordat een client deze niet meer gebruiken kunt om op te halen van een nieuw token sleutelpaar van toegang/vernieuwen wanneer u probeert toegang tot deze resource. Omdat een nieuwe vernieuwingstoken meestal wordt geretourneerd wanneer een vernieuwingstoken wordt gebruikt, heeft dit beleid voorkomt dat toegang als de client probeert te krijgen tot een resource met behulp van de huidige vernieuwingstoken tijdens de opgegeven periode.

Dit beleid zorgt ervoor dat gebruikers die niet actief op de client te verifiëren als u wilt ophalen van een nieuwe vernieuwingstoken zijn.

De eigenschap Refresh Token maximale niet-actieve tijd moet worden ingesteld op een lagere waarde dan de één-Factor Token Max Age en de basis van meerdere factoren Refresh Token Max Age-eigenschappen.

### <a name="single-factor-refresh-token-max-age"></a>Maximumleeftijd voor Token-één-Factor vernieuwen
**Tekenreeks:** MaxAgeSingleFactor

**Is van invloed op:** Vernieuwingstokens

**Overzicht:** deze beleid bepaalt hoe lang een gebruiker een vernieuwingstoken kunt gebruiken om een nieuw token toegang/vernieuwen paar nadat deze laatste is geverifieerd met behulp van slechts één factor. Nadat een gebruiker wordt geverifieerd en een nieuwe vernieuwingstoken ontvangt, kan de gebruiker kan de stroom vernieuwen tokens gebruiken voor de opgegeven periode. (Dit geldt zolang het huidige vernieuwingstoken dat niet is ingetrokken en is niet langer zijn dan de niet-actieve tijd niet-gebruikte verlaten.) De gebruiker wordt op dat moment gedwongen om te verifiëren voor het ontvangen van een nieuw vernieuwingstoken.

De maximale leeftijd beperken zorgt ervoor dat gebruikers vaker te verifiëren. Omdat de authenticatie is minder veilig dan meervoudige verificatie, wordt u aangeraden dat u deze eigenschap ingesteld op een waarde die gelijk is aan of kleiner is dan de basis van meerdere factoren Refresh Token Max Age-eigenschap.

### <a name="multi-factor-refresh-token-max-age"></a>Token maximumleeftijd van meerdere factoren vernieuwen
**Tekenreeks:** MaxAgeMultiFactor

**Is van invloed op:** Vernieuwingstokens

**Overzicht:** deze beleid bepaalt hoe lang een gebruiker een vernieuwingstoken kunt gebruiken om een nieuw token toegang/vernieuwen paar nadat deze laatste is geverifieerd met behulp van meerdere factoren. Nadat een gebruiker wordt geverifieerd en een nieuwe vernieuwingstoken ontvangt, kan de gebruiker kan de stroom vernieuwen tokens gebruiken voor de opgegeven periode. (Dit geldt zolang het huidige vernieuwingstoken dat niet is ingetrokken en het is niet langer zijn dan de niet-actieve tijd niet-gebruikte.) Op dat moment worden gebruikers gedwongen om te verifiëren voor het ontvangen van een nieuw vernieuwingstoken.

De maximale leeftijd beperken zorgt ervoor dat gebruikers vaker te verifiëren. Omdat de authenticatie is minder veilig dan meervoudige verificatie, wordt u aangeraden dat u deze eigenschap ingesteld op een waarde die gelijk is aan of groter zijn dan de eigenschap één van meerdere factoren Refresh Token Max Age.

### <a name="single-factor-session-token-max-age"></a>Maximumleeftijd voor Token-sessie voor één van meerdere factoren
**Tekenreeks:** MaxAgeSessionSingleFactor

**Is van invloed op:** sessie tokens (persistent en niet-persistente)

**Overzicht:** deze beleid bepaalt hoe lang een gebruiker een sessietoken kunt gebruiken om een nieuwe ID en het sessietoken nadat deze laatste is geverifieerd met behulp van slechts één factor. Nadat een gebruiker wordt geverifieerd en een nieuwe sessietoken ontvangt, kan de gebruiker kan de stroom van de sessie-tokens gebruiken voor de opgegeven periode. (Dit geldt zolang het token van de huidige sessie niet is ingetrokken en niet is verlopen.) Na de opgegeven periode wordt de gebruiker gedwongen om te verifiëren voor het ontvangen van de token van een nieuwe sessie.

De maximale leeftijd beperken zorgt ervoor dat gebruikers vaker te verifiëren. Omdat de authenticatie is minder veilig dan meervoudige verificatie, wordt u aangeraden dat u deze eigenschap ingesteld op een waarde die gelijk is aan of kleiner is dan de eigenschap Multi-factor-sessie Token Max-Age.

### <a name="multi-factor-session-token-max-age"></a>Maximumleeftijd voor Token-sessie van meerdere factoren
**Tekenreeks:** MaxAgeSessionMultiFactor

**Is van invloed op:** sessie tokens (persistent en niet-persistente)

**Overzicht:** deze beleid bepaalt hoe lang een gebruiker een sessietoken ophalen van een nieuwe ID en een sessie token na de laatste keer dat deze is geverifieerd met behulp van meerdere factoren kunt gebruiken. Nadat een gebruiker wordt geverifieerd en een nieuwe sessietoken ontvangt, kan de gebruiker kan de stroom van de sessie-tokens gebruiken voor de opgegeven periode. (Dit geldt zolang het token van de huidige sessie niet is ingetrokken en niet is verlopen.) Na de opgegeven periode wordt de gebruiker gedwongen om te verifiëren voor het ontvangen van de token van een nieuwe sessie.

De maximale leeftijd beperken zorgt ervoor dat gebruikers vaker te verifiëren. Omdat de authenticatie is minder veilig dan meervoudige verificatie, wordt u aangeraden dat u deze eigenschap ingesteld op een waarde die gelijk is aan of groter is dan de eigenschap één van meerdere factoren sessie Token Max-Age.

## <a name="example-token-lifetime-policies"></a>Voorbeeld van de levensduur van vernieuwingstoken beleid
Er zijn veel scenario's mogelijk in Azure AD wanneer u kunt maken en beheren van de levensduur van tokens voor apps, service-principals en uw hele organisatie. In deze sectie doorlopen we enkele algemene beleid voor scenario's waarmee u nieuwe regels voor opleggen kunnen:

* Levensduur van token
* Token maximale niet-actieve tijd
* Token maximumleeftijd

In de voorbeelden, leert u hoe u:

* Beheren van een organisatie-standaardbeleid
* Maak een beleid voor aanmelding bij de website
* Een beleid maken voor een systeemeigen app die een web-API-aanroepen
* Het beleid voor een geavanceerde beheren

### <a name="prerequisites"></a>Vereisten
In de volgende voorbeelden u maken, bijwerken, koppelen en verwijderen van beleid voor apps, service-principals en uw hele organisatie. Als u niet bekend bent met Azure AD, raden wij aan dat u meer informatie over [over het verkrijgen van een Azure AD-tenant](quickstart-create-new-tenant.md) voordat u met deze voorbeelden doorgaat.  

Om te beginnen, voer de volgende stappen uit:

1. Download de meest recente [Azure AD PowerShell-Module openbare Preview-versie](https://www.powershellgallery.com/packages/AzureADPreview).
2. Voer de `Connect` opdracht uit om aan te melden bij uw Azure AD-beheerdersaccount. Deze opdracht uitvoeren telkens wanneer starten u een nieuwe sessie.

    ```PowerShell
    Connect-AzureAD -Confirm
    ```

3. Als u wilt zien van alle beleidsregels die zijn gemaakt in uw organisatie, moet u de volgende opdracht uitvoeren. Voer deze opdracht na de meeste bewerkingen in de volgende scenario's. Met de opdracht ook krijgt u de ** ** van uw beleid.

    ```PowerShell
    Get-AzureADPolicy
    ```

### <a name="example-manage-an-organizations-default-policy"></a>Voorbeeld: Het standaardbeleid van een organisatie beheren
In dit voorbeeld maakt u een beleid waarmee uw gebruikers zich aanmelden minder vaak in uw hele organisatie. U doet dit door een beleid voor de levensduur van tokens voor één van meerdere factoren vernieuwen Tokens, die wordt toegepast in uw organisatie te maken. Het beleid wordt toegepast op elke toepassing in uw organisatie, en aan elke service-principal die niet al een beleid is ingesteld.

1. Maak een beleid voor de levensduur van tokens.

    1.  Stel de één-Factor Vernieuwingstoken "tot-ingetrokken." Het token verloopt niet totdat de toegang is ingetrokken. De volgende beleidsdefinitie maken:

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

    3.  Om te zien van het nieuwe beleid en om op te halen van het beleid **ObjectId**, voer de volgende opdracht uit:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Het beleid niet bijwerken.

    Kunt u besluiten het eerste beleid die u in dit voorbeeld instelt is niet zo streng zijn als uw service nodig heeft. Om in te stellen uw één van meerdere factoren Refresh Token verloopt in twee dagen, moet u de volgende opdracht uitvoeren:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId FROM GET COMMAND> -DisplayName "OrganizationDefaultPolicyUpdatedScenario" -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="example-create-a-policy-for-web-sign-in"></a>Voorbeeld: Maak een beleid voor aanmelding bij de website

In dit voorbeeld maakt u een beleid waarbij gebruikers om te verifiëren regelmatig in uw web-app. Dit beleid wordt de levensduur van de toegangs-ID-tokens en de maximale leeftijd van een van de multi-factor Authentication-sessietoken ingesteld op de service-principal van uw web-app.

1. Maak een beleid voor de levensduur van tokens.

    Dit beleid, voor aanmelding web Hiermee stelt u de toegang/ID levensduur van tokens en de maximale sessie voor één van meerdere factoren token leeftijd tot twee uur.

    1.  Voor het maken van het beleid, moet u deze opdracht uitvoeren:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  Het nieuwe beleid, en het beleid **ObjectId**, voer de volgende opdracht uit:

        ```PowerShell
        Get-AzureADPolicy
        ```

2.  Het beleid toewijzen aan uw service-principal. Ook moet u de **ObjectId** van uw service-principal. 

    1.  Als u wilt zien van uw organisatie service-principals, kunt u een query de [Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/serviceprincipal#properties) of de [Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Bovendien kunt u dit testen in de [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/), en de [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) met behulp van uw Azure AD-account.

    2.  Wanneer u hebt de **ObjectId** van uw service-principal, voer de volgende opdracht uit:

        ```PowerShell
        Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Voorbeeld: Maak een beleid voor een systeemeigen app die een web-API-aanroepen
In dit voorbeeld maakt u een beleid dat vereist dat gebruikers minder vaak worden geverifieerd. De hoeveelheid tijd die een gebruiker niet-actief zijn mag voordat de gebruiker moet verifiëren, het beleid wordt ook langer. Het beleid wordt toegepast op de web-API. Wanneer de systeemeigen app de web-API als een bron aanvraagt, wordt dit beleid wordt toegepast.

1. Maak een beleid voor de levensduur van tokens.

    1.  Voer de volgende opdracht voor het maken van een strikt beleid voor een web-API:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    2.  Het nieuwe beleid, en het beleid **ObjectId**, voer de volgende opdracht uit:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Het beleid toewijzen aan uw web-API. Ook moet u de **ObjectId** van uw toepassing. De beste manier om te vinden van uw app **ObjectId** is het gebruik van de [Azure-portal](https://portal.azure.com/).

   Wanneer u hebt de **ObjectId** van uw app, voer de volgende opdracht uit:

        ```PowerShell
        Add-AzureADApplicationPolicy -Id <ObjectId of the Application> -RefObjectId <ObjectId of the Policy>
        ```


### <a name="example-manage-an-advanced-policy"></a>Voorbeeld: Het beleid voor een geavanceerde beheren
In dit voorbeeld maakt u enkele beleidsregels, als u wilt weten hoe het systeem prioriteit werkt. U kunt leert ook hoe u meerdere beleidsregels die worden toegepast op meerdere objecten te beheren.

1. Maak een beleid voor de levensduur van tokens.

    1.  Voer de volgende opdracht voor het maken van een organisatie-standaardbeleid dat Hiermee stelt u de levensduur van tokens voor vernieuwen van één van meerdere factoren tot 30 dagen:

        ```PowerShell
        New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    2.  Om te zien van het nieuwe beleid en om op te halen van het beleid **ObjectId**, voer de volgende opdracht uit:

        ```PowerShell
        Get-AzureADPolicy
        ```

2. Het beleid toewijzen aan een service-principal.

    U hebt nu een beleid dat van toepassing op de hele organisatie. U kunt dit beleid 30 dagen bewaren voor een specifieke service-principal, maar het standaardbeleid organisatie wijzigen in de bovengrens van "tot ingetrokken."

    1.  Als u wilt zien van uw organisatie service-principals, kunt u een query de [Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/serviceprincipal#properties) of de [Azure AD Graph](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity). Bovendien kunt u dit testen in de [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/), en de [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) met behulp van uw Azure AD-account.

    2.  Wanneer u hebt de **ObjectId** van uw service-principal, voer de volgende opdracht uit:

            ```PowerShell
            Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
            ```
        
3. Stel de `IsOrganizationDefault` vlag op false:

    ```PowerShell
    Set-AzureADPolicy -Id <ObjectId of Policy> -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

4. Maak een nieuwe organisatie-standaardbeleid:

    ```PowerShell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    U hebt nu het oorspronkelijke beleid dat is gekoppeld aan uw service-principal en het nieuwe beleid is ingesteld als standaardbeleid voor uw organisatie. Het is belangrijk te onthouden dat beleid toegepast op service-principals tijdens organisatie standaardbeleidsregels prioriteit hebben.

## <a name="cmdlet-reference"></a>Cmdlet-naslaginformatie

### <a name="manage-policies"></a>Beleid beheren

U kunt de volgende cmdlets gebruiken voor het beheren van beleid.

#### <a name="new-azureadpolicy"></a>New-AzureADPolicy

Hiermee maakt u een nieuw beleid.

```PowerShell
New-AzureADPolicy -Definition <Array of Rules> -DisplayName <Name of Policy> -IsOrganizationDefault <boolean> -Type <Policy Type>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Definition</code> |Matrix van stringified JSON die de regels van het beleid bevat. | `-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;DisplayName</code> |De tekenreeks van de naam van het beleid. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;IsOrganizationDefault</code> |Indien waar, stelt het beleid als het standaardbeleid van de organisatie. Indien onwaar, gebeurt er niets. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> |Het type beleid. Voor de levensduur van tokens, gebruikt u altijd "TokenLifetimePolicy." | `-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code> [Optioneel] |Hiermee stelt u een alternatieve ID voor het beleid. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="get-azureadpolicy"></a>Get-AzureADPolicy
Hiermee haalt u alle beleidsregels voor de Azure AD of een opgegeven beleid.

```PowerShell
Get-AzureADPolicy
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> [Optioneel] |**Object-id (Id)** van het gewenste beleid. |`-Id <ObjectId of Policy>` |

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
| <code>&#8209;DisplayName</code> |De tekenreeks van de naam van het beleid. |`-DisplayName "MyTokenPolicy"` |
| <code>&#8209;Definition</code> [Optioneel] |Matrix van stringified JSON die de regels van het beleid bevat. |`-Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"20:00:00"}}')` |
| <code>&#8209;IsOrganizationDefault</code> [Optioneel] |Indien waar, stelt het beleid als het standaardbeleid van de organisatie. Indien onwaar, gebeurt er niets. |`-IsOrganizationDefault $true` |
| <code>&#8209;Type</code> [Optioneel] |Het type beleid. Voor de levensduur van tokens, gebruikt u altijd "TokenLifetimePolicy." |`-Type "TokenLifetimePolicy"` |
| <code>&#8209;AlternativeIdentifier</code> [Optioneel] |Hiermee stelt u een alternatieve ID voor het beleid. |`-AlternativeIdentifier "myAltId"` |

</br></br>

#### <a name="remove-azureadpolicy"></a>Remove-AzureADPolicy
Hiermee verwijdert u het opgegeven beleid.

```PowerShell
 Remove-AzureADPolicy -Id <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van het gewenste beleid. | `-Id <ObjectId of Policy>` |

</br></br>

### <a name="application-policies"></a>Toepassingsbeleid
U kunt de volgende cmdlets gebruiken voor het toepassingsbeleid.</br></br>

#### <a name="add-azureadapplicationpolicy"></a>Add-AzureADApplicationPolicy
Het opgegeven beleid is gekoppeld aan een toepassing.

```PowerShell
Add-AzureADApplicationPolicy -Id <ObjectId of Application> -RefObjectId <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**Object-id** van het beleid. | `-RefObjectId <ObjectId of Policy>` |

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

#### <a name="remove-azureadapplicationpolicy"></a>Remove-AzureADApplicationPolicy
Hiermee verwijdert u een beleid van een toepassing.

```PowerShell
Remove-AzureADApplicationPolicy -Id <ObjectId of Application> -PolicyId <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**Object-id** van het beleid. | `-PolicyId <ObjectId of Policy>` |

</br></br>

### <a name="service-principal-policies"></a>Service-principal-beleidsregels
U kunt de volgende cmdlets gebruiken voor service-principal-beleidsregels.

#### <a name="add-azureadserviceprincipalpolicy"></a>Add-AzureADServicePrincipalPolicy
Het opgegeven beleid is gekoppeld aan een service-principal.

```PowerShell
Add-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal> -RefObjectId <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |
| <code>&#8209;RefObjectId</code> |**Object-id** van het beleid. | `-RefObjectId <ObjectId of Policy>` |

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

#### <a name="remove-azureadserviceprincipalpolicy"></a>Remove-AzureADServicePrincipalPolicy
Hiermee verwijdert u het beleid uit de opgegeven service-principal.

```PowerShell
Remove-AzureADServicePrincipalPolicy -Id <ObjectId of ServicePrincipal>  -PolicyId <ObjectId of Policy>
```

| Parameters | Beschrijving | Voorbeeld |
| --- | --- | --- |
| <code>&#8209;Id</code> |**Object-id (Id)** van de toepassing. | `-Id <ObjectId of Application>` |
| <code>&#8209;PolicyId</code> |**Object-id** van het beleid. | `-PolicyId <ObjectId of Policy>` |

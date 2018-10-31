---
title: Claims worden weergegeven in tokens voor een specifieke app in een Azure AD-tenant (openbare Preview) aanpassen
description: Deze pagina wordt Azure Active Directory claimtoewijzing beschreven.
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2018
ms.author: celested
ms.reviewer: paulgarn, hirsin, jeedes
ms.openlocfilehash: 18cd96c87f294f1dd8e62f41dd759558c2013aa0
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241665"
---
# <a name="how-to-customize-claims-emitted-in-tokens-for-a-specific-app-in-a-tenant-preview"></a>Hoe: claims worden weergegeven in tokens voor een specifieke app in een tenant (Preview) aanpassen

> [!NOTE]
> Deze functie vervangt en vervangt de [claims aanpassing](active-directory-saml-claims-customization.md) vandaag die wordt aangeboden via de portal. Op dezelfde toepassing, als u met behulp van de portal naast de grafiek/PowerShell-methode die in dit document, claims aanpassen tokens die zijn uitgegeven voor de configuratie in de portal wordt genegeerd door toepassing. Configuraties die zijn gemaakt via de methoden die in dit document worden niet doorgevoerd in de portal.

Deze functie wordt gebruikt door tenantbeheerders voor het aanpassen van de claims die worden weergegeven in tokens voor een bepaalde toepassing in hun tenant. U kunt beleid voor het toewijzen van claims gebruiken:

- Selecteer welke claims zijn opgenomen in de tokens.
- Maak claimtypen die nog niet bestaan.
- Kies of wijzigen van de bron van gegevens in specifieke claims verzonden.

> [!NOTE]
> Deze mogelijkheid is momenteel in openbare preview. Wees voorbereid om terug te keren of eventuele wijzigingen te verwijderen. De functie is beschikbaar in alle Azure Active Directory (Azure AD)-abonnement tijdens de openbare preview. Wanneer de functie algemeen beschikbaar komt, kunnen sommige aspecten van de functie echter een Azure AD premium-abonnement vereist. Deze functie ondersteunt configureren claim toewijzing van beleid voor WS-Federation, SAML, OAuth en OpenID Connect-protocollen.

## <a name="claims-mapping-policy-type"></a>Claimtoewijzing beleidstype

In Azure AD een **beleid** object vertegenwoordigt een reeks regels afgedwongen voor afzonderlijke toepassingen of op alle toepassingen in een organisatie. Elk type beleid heeft een unieke structuur, met een set eigenschappen die vervolgens worden toegepast op objecten die ze zijn toegewezen.

Een claims toewijzen van beleid is een type **beleid** -object dat Hiermee wijzigt u de claims die worden weergegeven in tokens die zijn uitgegeven voor specifieke toepassingen.

## <a name="claim-sets"></a>Aanspraak op sets

Er is een bepaalde set van claims waarmee wordt gedefinieerd hoe en wanneer ze worden gebruikt in tokens.

| Claim instellen | Beschrijving |
|---|---|
| Core claimset | Aanwezig zijn in elke token, ongeacht het beleid. Deze claims worden ook beschouwd als beperkt, en kunnen niet worden gewijzigd. |
| Basic claimset | Bevat de claims die worden gegenereerd door de standaardwaarde voor tokens (naast de core claimset). U kunt weglaten of basic claims wijzigen met behulp van de claims toewijzen van beleid. |
| Beperkte claimset | Kan niet worden gewijzigd met behulp van beleid. De gegevensbron kan niet worden gewijzigd en er is geen transformatie is toegepast bij het genereren van deze claims. |

### <a name="table-1-json-web-token-jwt-restricted-claim-set"></a>Tabel 1: JSON Web Token (JWT) beperkte claimset

|Claimtype (naam)|
| ----- |
|_claim_names|
|_claim_sources|
|access_token|
|account_type|
|acr|
|actor|
|actortoken|
|AIO|
|afkomstig|
|AMR|
|app_chain|
|app_displayname|
|app_res|
|appctx|
|appctxsender|
|toepassings-id|
|appidacr|
|bevestiging|
|at_hash|
|AUD|
|auth_data|
|auth_time|
|authorization_code|
|azp|
|azpacr|
|c_hash|
|ca_enf|
|cc|
|cert_token_use|
|client_id|
|cloud_graph_host_name|
|cloud_instance_name|
|CNF|
|Code|
|besturingselementen|
|credential_keys|
|CSR|
|csr_type|
|apparaat-id|
|dns_names|
|domain_dns_name|
|domain_netbios_name|
|e_exp|
|e-mail|
|endpoint|
|enfpolids|
|EXP|
|expires_on|
|grant_type|
|grafiek|
|group_sids|
|groepen|
|hasgroups|
|hash_alg|
|home_oid|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/expired|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier|
|IAT|
|id-provider|
|id-provider|
|in_corp|
|exemplaar|
|IpAddr|
|isbrowserhostedapp|
|ISS|
|jwk|
|key_id|
|key_type|
|mam_compliance_url|
|mam_enrollment_url|
|mam_terms_of_use_url|
|mdm_compliance_url|
|mdm_enrollment_url|
|mdm_terms_of_use_url|
|NameID|
|nbf|
|netbios_name|
|nonce|
|OID|
|on_prem_id|
|onprem_sam_account_name|
|onprem_sid|
|openid2_id|
|wachtwoord|
|platf|
|polids|
|pop_jwk|
|preferred_username|
|previous_refresh_token|
|primary_sid|
|de PUID|
|pwd_exp|
|pwd_url|
|redirect_uri|
|refresh_token|
|refreshtoken|
|request_nonce|
|Bron|
|rol|
|rolls|
|scope|
|SCP|
|beveiligings-id|
|handtekening|
|signin_state|
|src1|
|src2|
|Sub|
|tbid|
|tenant_display_name|
|tenant_region_scope|
|thumbnail_photo|
|TID|
|tokenAutologonEnabled|
|trustedfordelegation|
|unique_name|
|UPN|
|user_setting_sync_url|
|gebruikersnaam|
|uti|
|ver|
|verified_primary_email|
|verified_secondary_email|
|wids|
|win_ver|

### <a name="table-2-saml-restricted-claim-set"></a>Tabel 2: SAML beperkte claimset

|Claimtype (URI)|
| ----- |
|http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/expired|
|http://schemas.microsoft.com/identity/claims/accesstoken|
|http://schemas.microsoft.com/identity/claims/openid2_id|
|http://schemas.microsoft.com/identity/claims/identityprovider|
|http://schemas.microsoft.com/identity/claims/objectidentifier|
|http://schemas.microsoft.com/identity/claims/puid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier [MR1] |
|http://schemas.microsoft.com/identity/claims/tenantid|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod|
|http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/groups|
|http://schemas.microsoft.com/claims/groups.link|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/role|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/wids|
|http://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant|
|http://schemas.microsoft.com/2014/02/devicecontext/claims/isknown|
|http://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged|
|http://schemas.microsoft.com/2014/03/psso|
|http://schemas.microsoft.com/claims/authnmethodsreferences|
|http://schemas.xmlsoap.org/ws/2009/09/identity/claims/actor|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/samlissuername|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/confirmationkey|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authorizationdecision|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sid|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarysid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/denyonlysid|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlywindowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdeviceclaim|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsfqbnversion|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/windowssubauthority|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsuserclaim|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/x500distinguishedname|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn|
|http://schemas.microsoft.com/ws/2008/06/identity/claims/ispersistent|
|http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier|
|http://schemas.microsoft.com/identity/claims/scope|

## <a name="claims-mapping-policy-properties"></a>Claimtoewijzing beleidseigenschappen

Om te bepalen welke claims worden uitgezonden en waarin de gegevens vandaan komen, moet u de eigenschappen van een beleid voor claimtoewijzing gebruiken. Als een beleid is niet ingesteld, het systeem problemen tokens die de kern van het claim set, de basic claimset en eventuele [optionele claims](active-directory-optional-claims.md) die de toepassing heeft gekozen om te ontvangen.

### <a name="include-basic-claim-set"></a>Basic claimset bevatten

**Tekenreeks:** IncludeBasicClaimSet

**Gegevenstype:** Booleaanse waarde (waar of ONWAAR)

**Overzicht:** deze eigenschap bepaalt of de set basic claim is opgenomen in de tokens die worden beïnvloed door dit beleid. 

- Indien ingesteld op True, alle claims in de set basic claim worden uitgezonden in tokens die door het beleid beïnvloed. 
- Indien ingesteld op False, claims in de set basic claim niet in de tokens, zijn tenzij ze afzonderlijk worden toegevoegd in de claims-schema-eigenschap van hetzelfde beleid.

> [!NOTE] 
> Claims in de core claimset zijn aanwezig in elke token, ongeacht wat deze eigenschap is ingesteld op. 

### <a name="claims-schema"></a>Claims schema

**Tekenreeks:** ClaimsSchema

**Gegevenstype:** JSON-blob met een of meer claim schema vermeldingen

**Overzicht:** deze eigenschap wordt gedefinieerd welke claims aanwezig zijn in de tokens beïnvloed door het beleid ook aan de set basic claim en de belangrijkste invoerclaimset.
Voor de invoer van elke claim schema is gedefinieerd in deze eigenschap, is bepaalde informatie vereist. Opgeven waar de gegevens vandaan (**waarde** of **bron/ID paar**), en dat de gegevens claim wordt verzonden als (**Claim Type**).

### <a name="claim-schema-entry-elements"></a>Schema-elementen voor invoer claim

**Waarde:** het waarde-element definieert een statische waarde als de gegevens die moeten worden verzonden in de claim.

**De combinatie van de bron-ID:** elementen van de bron- en -ID definiëren waar de gegevens in de claim afkomstig is uit. 

Stel het bron-element op een van de volgende waarden: 

- 'gebruiker': de gegevens in de claim is een eigenschap van het gebruikersobject. 
- 'application': de gegevens in de claim is een eigenschap van de toepassing (client) service-principal. 
- 'resource': de gegevens in de claim is een eigenschap van de resource-service-principal.
- 'doelgroep': de gegevens in de claim is een eigenschap van de service-principal is de doelgroep van het token (de client of de resource service-principal).
- 'bedrijf': de gegevens in de claim is een eigenschap van de resource-tenant bedrijf object.
- 'transformatie': de gegevens in de claim afkomstig is van de claimtransformatie (Zie de sectie 'Claims transformatie' verderop in dit artikel).

Als de bron-transformatie, is de **TransformationID** element moet worden opgenomen in de claimdefinitie van deze ook.

De ID-element geeft aan welke eigenschap van de bron geeft de waarde voor de claim. De volgende tabel bevat de waarden van geldig is voor elke waarde van de bron-ID.

#### <a name="table-3-valid-id-values-per-source"></a>Tabel 3: Ongeldig id-waarden per bron

|Bron|Id|Beschrijving|
|-----|-----|-----|
|Gebruiker|Achternaam|Familienaam|
|Gebruiker|givenName|Voornaam|
|Gebruiker|DisplayName|Weergavenaam|
|Gebruiker|object-id|ObjectID|
|Gebruiker|mail|E-mailadres|
|Gebruiker|userPrincipalName|User principal name|
|Gebruiker|Afdeling|Afdeling|
|Gebruiker|onpremisessamaccountname|Op de lokale Sam-accountnaam|
|Gebruiker|NetBIOS-naam|NetBios-naam|
|Gebruiker|DNS-domeinnaam|DNS-domeinnaam|
|Gebruiker|onpremisesecurityidentifier|on-premises beveiligings-id|
|Gebruiker|bedrijfsnaam|Naam van organisatie|
|Gebruiker|streetAddress|Adres|
|Gebruiker|Postcode|Postcode|
|Gebruiker|preferredlanguange|Voorkeurstaal|
|Gebruiker|onpremisesuserprincipalname|on-premises UPN|
|Gebruiker|mailnickname|E-mailbijnaam|
|Gebruiker|extensionattribute1|Kenmerk toestelnummer 1|
|Gebruiker|extensionattribute2|Kenmerk toestelnummer 2|
|Gebruiker|extensionattribute3|Kenmerk toestelnummer 3|
|Gebruiker|extensionattribute4|Kenmerk toestelnummer 4|
|Gebruiker|extensionattribute5|Kenmerk toestelnummer 5|
|Gebruiker|extensionattribute6|Kenmerk toestelnummer 6|
|Gebruiker|extensionattribute7|Kenmerk toestelnummer 7|
|Gebruiker|extensionattribute8|Kenmerk toestelnummer 8|
|Gebruiker|extensionattribute9|Kenmerk toestelnummer 9|
|Gebruiker|extensionattribute10|Kenmerk toestelnummer 10|
|Gebruiker|extensionattribute11|Kenmerk toestelnummer 11|
|Gebruiker|extensionattribute12|Kenmerk toestelnummer 12|
|Gebruiker|extensionattribute13|Kenmerk toestelnummer 13|
|Gebruiker|extensionattribute14|Kenmerk toestelnummer 14|
|Gebruiker|extensionattribute15|Kenmerk toestelnummer 15|
|Gebruiker|othermail|Andere e-Mail|
|Gebruiker|Land/regio|Land|
|Gebruiker|city|Plaats|
|Gebruiker|state|Status|
|Gebruiker|functie|Functie|
|Gebruiker|werknemer-id|Werknemer-id|
|Gebruiker|facsimiletelephonenumber|Fax telefoonnummer|
|toepassing, resource, doelgroep|DisplayName|Weergavenaam|
|toepassing, resource, doelgroep|objecten|ObjectID|
|toepassing, resource, doelgroep|tags|Service-Principal Tag|
|Bedrijf|tenantcountry|Land van de tenant|

**TransformationID:** het element TransformationID alleen als de bron-element is ingesteld op 'transformatie' moet worden opgegeven.

- Dit element moet overeenkomen met de ID-element van de transformatie-vermelding in de **ClaimsTransformation** eigenschap waarmee wordt gedefinieerd hoe de gegevens voor deze claim wordt gegenereerd.

**Claimtype:** de **JwtClaimType** en **SamlClaimType** elementen definiëren die deze claim schema-item verwijst naar claim.

- De JwtClaimType moet de naam van de claim te worden verzonden in JWTs bevatten.
- De SamlClaimType mag de URI van de claim in SAML-tokens worden verzonden.

> [!NOTE]
> Namen en URI's van claims in de beperkte claimset kan niet worden gebruikt voor de claim type-elementen. Zie de sectie 'Uitzonderingen en beperkingen' verderop in dit artikel voor meer informatie.

### <a name="claims-transformation"></a>Claimstransformatie

**Tekenreeks:** ClaimsTransformation

**Gegevenstype:** JSON-blob, met een of meer vermeldingen voor transformatie 

**Overzicht:** deze eigenschap gebruiken om algemene transformaties die van toepassing op de brongegevens voor het genereren van de uitvoergegevens voor de claims die zijn opgegeven in de Claims-Schema.

**-ID:** de ID-element gebruiken om te verwijzen naar deze transformatie-vermelding in de vermelding TransformationID Claims Schema. Deze waarde moet uniek zijn voor elk item transformatie binnen dit beleid.

**TransformationMethod:** het element TransformationMethod identificeert welke bewerking wordt uitgevoerd voor het genereren van de gegevens voor de claim.

Op basis van de gekozen methode, wordt een set van invoer en uitvoer verwacht. De invoer en uitvoer definiëren met behulp van de **InputClaims**, **invoerparameters** en **OutputClaims** elementen.

#### <a name="table-4-transformation-methods-and-expected-inputs-and-outputs"></a>Tabel 4: Transformatie methoden en verwachte invoer en uitvoer

|TransformationMethod|Verwachte invoer|Verwachte uitvoer|Beschrijving|
|-----|-----|-----|-----|
|Koppelen|tekenreeks1, tekenreeks2, scheidingsteken voor duizendtallen|outputClaim|Joins invoer tekenreeksen met behulp van een scheidingsteken tussen. Bijvoorbeeld: tekenreeks1: "foo@bar.com", tekenreeks2: "sandbox", scheidingsteken: '. ' resulteert in outputClaim: "foo@bar.com.sandbox"|
|ExtractMailPrefix|mail|outputClaim|Extraheert het lokale gedeelte van een e-mailadres. Bijvoorbeeld: e-mail: "foo@bar.com" resulteert in outputClaim: "foo". Als er geen \@ aanmelding aanwezig is, wordt de oorspronkelijke invoerreeks worden geretourneerd, zoals is.|

**InputClaims:** met een element InputClaims kunt u de gegevens van een claim schema vermelding doorgeven aan een transformatie. Deze twee kenmerken heeft: **ClaimTypeReferenceId** en **TransformationClaimType**.

- **ClaimTypeReferenceId** wordt samengevoegd met de ID-element van de vermelding van de claim schema vinden van de juiste invoer claim. 
- **TransformationClaimType** bieden een unieke naam uit deze invoer wordt gebruikt. Deze naam moet overeenkomen met een van de verwachte invoer voor de transformatiemethode.

**Invoerparameters:** met een element invoerparameters kunt u een constante waarde doorgeven aan een transformatie. Deze twee kenmerken heeft: **waarde** en **ID**.

- **Waarde** is van de werkelijke constante waarde die moet worden doorgegeven.
- **ID** bieden een unieke naam in de invoer wordt gebruikt. De naam moet overeenkomen met een van de verwachte invoer voor de transformatiemethode.

**OutputClaims:** een element OutputClaims gebruiken voor het opslaan van de gegevens die worden gegenereerd door een transformatie, en deze koppelen aan een claim schema-item. Deze twee kenmerken heeft: **ClaimTypeReferenceId** en **TransformationClaimType**.

- **ClaimTypeReferenceId** wordt samengevoegd met de ID van de claim schema-vermelding moet de juiste uitvoerclaim vinden.
- **TransformationClaimType** bieden een unieke naam in de uitvoer wordt gebruikt. De naam moet overeenkomen met een van de verwachte uitvoer voor de transformatiemethode.

### <a name="exceptions-and-restrictions"></a>Uitzonderingen en beperkingen

**NameID van SAML en UPN:** de kenmerken van waaruit u de waarden NameID en UPN en de claimtransformaties die zijn toegestaan, de gegevensbron zijn beperkt. Zie tabel 5 en 6 tot en met de toegestane waarden voor de tabel.

#### <a name="table-5-attributes-allowed-as-a-data-source-for-saml-nameid"></a>Tabel 5: Kenmerken die zijn toegestaan als een gegevensbron voor de NameID van SAML

|Bron|Id|Beschrijving|
|-----|-----|-----|
|Gebruiker|mail|E-mailadres|
|Gebruiker|userPrincipalName|User principal name|
|Gebruiker|onpremisessamaccountname|Op de lokale Sam-accountnaam|
|Gebruiker|werknemer-id|Werknemer-id|
|Gebruiker|extensionattribute1|Kenmerk toestelnummer 1|
|Gebruiker|extensionattribute2|Kenmerk toestelnummer 2|
|Gebruiker|extensionattribute3|Kenmerk toestelnummer 3|
|Gebruiker|extensionattribute4|Kenmerk toestelnummer 4|
|Gebruiker|extensionattribute5|Kenmerk toestelnummer 5|
|Gebruiker|extensionattribute6|Kenmerk toestelnummer 6|
|Gebruiker|extensionattribute7|Kenmerk toestelnummer 7|
|Gebruiker|extensionattribute8|Kenmerk toestelnummer 8|
|Gebruiker|extensionattribute9|Kenmerk toestelnummer 9|
|Gebruiker|extensionattribute10|Kenmerk toestelnummer 10|
|Gebruiker|extensionattribute11|Kenmerk toestelnummer 11|
|Gebruiker|extensionattribute12|Kenmerk toestelnummer 12|
|Gebruiker|extensionattribute13|Kenmerk toestelnummer 13|
|Gebruiker|extensionattribute14|Kenmerk toestelnummer 14|
|Gebruiker|extensionattribute15|Kenmerk toestelnummer 15|

#### <a name="table-6-transformation-methods-allowed-for-saml-nameid"></a>Tabel 6: Transformatie methoden is toegestaan voor NameID van SAML

|TransformationMethod|Beperkingen|
| ----- | ----- |
|ExtractMailPrefix|Geen|
|Koppelen|Het achtervoegsel wordt toegevoegd, moet een geverifieerd domein van de resource-tenant.|

### <a name="custom-signing-key"></a>Aangepaste ondertekeningssleutel

Een aangepaste ondertekeningssleutel moet worden toegewezen aan het service-principal-object voor een claims toewijzen van beleid voor het van kracht. Alle tokens die zijn uitgegeven die mogelijk invloed hebben is door het beleid zijn ondertekend met de aangepaste ondertekeningssleutel en toepassingen moeten worden geconfigureerd voor het accepteren van tokens ondertekend met de ondertekeningssleutel. Dit zorgt ervoor dat de bevestiging dat de tokens zijn gewijzigd door de maker van de claims toewijzen van beleid en toepassingen beschermt tegen claimtoewijzing beleidsregels zijn gemaakt door kwaadwillende actoren.

### <a name="cross-tenant-scenarios"></a>Scenario's met meerdere tenants

Claimtoewijzing beleid niet van toepassing op gastgebruikers. Als een gastgebruiker probeert te krijgen van een toepassing met een beleid dat is toegewezen aan de service-principal voor claimtoewijzing, het standaardtoken wordt weergegeven (het beleid heeft geen effect).

## <a name="claims-mapping-policy-assignment"></a>Claimtoewijzing beleidstoewijzing

Claimtoewijzing beleid kunnen alleen worden toegewezen aan service-principalobjecten.

### <a name="example-claims-mapping-policies"></a>Voorbeeld van claims toewijzing van beleid

In Azure AD zijn veel scenario's mogelijk wanneer u claims die worden weergegeven in tokens voor specifieke service-principals kunt aanpassen. In deze sectie doorlopen we enkele algemene scenario's waarmee u het gebruik van het beleidstype voor claimtoewijzing rapportelementen kunnen.

#### <a name="prerequisites"></a>Vereisten

In de volgende voorbeelden u maken, bijwerken, koppelen en verwijderen van beleidsregels voor service-principals. Als u niet bekend bent met Azure AD, raden we u [meer informatie over hoe u een Azure AD-tenant verkrijgen](quickstart-create-new-tenant.md) voordat u met deze voorbeelden doorgaat.

Om te beginnen, voer de volgende stappen uit:

1. Download de meest recente [openbare preview-versie van Azure AD PowerShell-Module](https://www.powershellgallery.com/packages/AzureADPreview).
1. Voer de opdracht Connect te melden bij uw Azure AD-beheerdersaccount. Deze opdracht uitvoeren telkens wanneer starten u een nieuwe sessie.
    
     ``` powershell
    Connect-AzureAD -Confirm
    
    ```
1. Als u wilt zien van alle beleidsregels die zijn gemaakt in uw organisatie, moet u de volgende opdracht uitvoeren. Het is raadzaam dat u deze opdracht na de meeste bewerkingen in de volgende scenario's uitvoert, om te controleren dat de beleidsregels zijn gemaakt zoals verwacht.
   
    ``` powershell
        Get-AzureADPolicy
    
    ```
#### <a name="example-create-and-assign-a-policy-to-omit-the-basic-claims-from-tokens-issued-to-a-service-principal"></a>Voorbeeld: Maken en toewijzen van een beleid voor het overslaan van de basic claims van tokens die zijn uitgegeven aan een service-principal.
In dit voorbeeld maakt u een beleid dat Hiermee verwijdert u de eenvoudige claim instellen van tokens die zijn uitgegeven aan de gekoppelde service-principals.

1. Maak een beleid voor claimtoewijzing. Dit beleid, gekoppeld aan specifieke service-principals, Hiermee verwijdert u de eenvoudige claim ingesteld van tokens.
    1. Voor het maken van het beleid, moet u deze opdracht uitvoeren: 
    
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims" -Type "ClaimsMappingPolicy"
    ```
    2. Om te zien van het nieuwe beleid en het beleid voor object-id ophalen, voert u de volgende opdracht uit:
    
     ``` powershell
    Get-AzureADPolicy
    ```
1. Het beleid toewijzen aan uw service-principal. U moet ook de object-id van uw service principal ophalen. 
    1.  Als u wilt zien van de service-principals van uw organisatie, kunt u een query Microsoft Graph. Of in Azure AD Graph Explorer, moet u zich aanmelden bij uw Azure AD-account.
    2.  Wanneer u de object-id van uw service-principal hebt, kunt u de volgende opdracht uitvoeren:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
    ```

#### <a name="example-create-and-assign-a-policy-to-include-the-employeeid-and-tenantcountry-as-claims-in-tokens-issued-to-a-service-principal"></a>Voorbeeld: Maken en toewijzen van een beleid om op te nemen van de werknemer-id en TenantCountry als claims in de tokens die zijn uitgegeven aan een service-principal

In dit voorbeeld maakt u een beleid dat de werknemer-id en TenantCountry toegevoegd aan de tokens die zijn uitgegeven aan de gekoppelde service-principals. Id van de werknemer is verzonden als het claimtype naam in SAML-tokens en JWTs. De TenantCountry wordt verzonden als het type van de claim land/regio in zowel JWTs als SAML-tokens. In dit voorbeeld gaan we bevatten de eenvoudige claims in de tokens instellen.

1. Maak een beleid voor claimtoewijzing. Dit beleid, dat is gekoppeld aan een specifieke service-principals, wordt de werknemer-id en TenantCountry claims naar tokens toegevoegd.
    1. Voor het maken van het beleid, moet u deze opdracht uitvoeren:  
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name","JwtClaimType":"name"},{"Source":"company","ID":"tenantcountry","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample" -Type "ClaimsMappingPolicy"
    ```
    
    2. Om te zien van het nieuwe beleid en het beleid voor object-id ophalen, voert u de volgende opdracht uit:
     
     ``` powershell  
    Get-AzureADPolicy
    ```
1. Het beleid toewijzen aan uw service-principal. U moet ook de object-id van uw service principal ophalen. 
    1.  Als u wilt zien van de service-principals van uw organisatie, kunt u een query Microsoft Graph. Of in Azure AD Graph Explorer, moet u zich aanmelden bij uw Azure AD-account.
    2.  Wanneer u de object-id van uw service-principal hebt, kunt u de volgende opdracht uitvoeren:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
    ```

#### <a name="example-create-and-assign-a-policy-that-uses-a-claims-transformation-in-tokens-issued-to-a-service-principal"></a>Voorbeeld: Maken en toewijzen van een beleid dat gebruikmaakt van een claimtransformatie in de tokens die zijn uitgegeven aan een service-principal

In dit voorbeeld maakt u een beleid dat een aangepaste claim "JoinedData" verleend aan de gekoppelde service-principals JWTs verzendt. Deze claim bevat een waarde die is gemaakt door de gegevens die zijn opgeslagen in het kenmerk extensionattribute1 van het gebruikersobject met '.sandbox'. In dit voorbeeld sluiten we de eenvoudige claims in de tokens instellen.

1. Maak een beleid voor claimtoewijzing. Dit beleid, dat is gekoppeld aan een specifieke service-principals, wordt de werknemer-id en TenantCountry claims naar tokens toegevoegd.
    1. Voor het maken van het beleid, moet u deze opdracht uitvoeren: 
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformations":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"ID":"string2","Value":"sandbox"},{"ID":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample" -Type "ClaimsMappingPolicy" 
    ```
    
    2. Om te zien van het nieuwe beleid en het beleid voor object-id ophalen, voert u de volgende opdracht uit: 
     
     ``` powershell
    Get-AzureADPolicy
    ```
1. Het beleid toewijzen aan uw service-principal. U moet ook de object-id van uw service principal ophalen. 
    1.  Als u wilt zien van de service-principals van uw organisatie, kunt u een query Microsoft Graph. Of in Azure AD Graph Explorer, moet u zich aanmelden bij uw Azure AD-account.
    2.  Wanneer u de object-id van uw service-principal hebt, kunt u de volgende opdracht uitvoeren: 
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
    ```

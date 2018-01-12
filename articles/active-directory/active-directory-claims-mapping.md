---
title: Claims toewijzen in Azure Active Directory (openbare preview) | Microsoft Docs
description: Deze pagina beschrijft Azure Active Directory-claims toewijzen.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: billmath
ms.openlocfilehash: 1bc669dfa5a41e38b35751af62560ff650575a08
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="claims-mapping-in-azure-active-directory-public-preview"></a>Claims toewijzen in Azure Active Directory (openbare preview)

>[!NOTE]
>Deze functie vervangt en vervangt de [claims aanpassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization) die tegenwoordig worden aangeboden via de portal. Als u claims op basis van de portal naast de grafiek/PowerShell-methode uiteengezet in dit document voor dezelfde toepassing aanpast, wordt tokens verleend voor de configuratie in de portal wordt genegeerd door toepassing.
Configuraties die worden aangebracht via de methoden die in dit document wordt beschreven worden, niet weergegeven in de portal.

Deze functie wordt gebruikt door tenantbeheerders voor het aanpassen van de claims die wordt verzonden in tokens voor een bepaalde toepassing in de tenant. U kunt beleid voor het toewijzen van claims gebruiken:

- Selecteer welke claims worden opgenomen in de tokens.
- Claimtypen die al bestaan niet maken.
- Kies of wijzig de bron van gegevens die in bepaalde claims.

>[!NOTE]
>Deze mogelijkheid is momenteel in de openbare preview. Wees voorbereid om te herstellen of verwijderen van eventuele wijzigingen. De functie is beschikbaar in een abonnement voor Azure Active Directory (Azure AD) tijdens de openbare preview. Wanneer de functie algemeen beschikbaar wordt, kunnen sommige aspecten van de functie echter een Azure Active Directory premium-abonnement nodig.

## <a name="claims-mapping-policy-type"></a>Claims beleidstype toewijzing
In Azure AD een **beleid** object vertegenwoordigt een reeks regels afgedwongen op afzonderlijke toepassingen of op alle toepassingen in een organisatie. Elk type beleid heeft een unieke structuur, met een set eigenschappen die vervolgens worden toegepast op objecten waarvoor ze worden toegewezen.

Claims van een toewijzing van beleid is een soort **beleid** object waarmee de claims die wordt verzonden in tokens die zijn uitgegeven voor specifieke toepassingen worden gewijzigd.

## <a name="claim-sets"></a>Claimsets
Er zijn bepaalde sets van claims die definiëren hoe en wanneer ze worden gebruikt in tokens.

### <a name="core-claim-set"></a>Claim kernset
Claims in de claim kernset zijn aanwezig in elke token, ongeacht het beleid. Deze claims worden ook beschouwd als beperkt en kunnen niet worden gewijzigd.

### <a name="basic-claim-set"></a>Basic claimset
De basic claim bestaan de claims die worden gegenereerd door de standaardwaarde voor tokens (in aanvulling op de claim kernset). Deze claims worden weggelaten of gewijzigd met behulp van de claims koppelen van beleidsregels.

### <a name="restricted-claim-set"></a>Beperkte claimset
Beperkte claims worden niet gewijzigd met behulp van beleid. De gegevensbron kan niet worden gewijzigd en er is geen transformatie wordt toegepast als deze claims worden gegenereerd.

#### <a name="table-1-json-web-token-jwt-restricted-claim-set"></a>Tabel 1: JSON Web Token (JWT) beperkt claimset
|Claimtype (naam)|
| ----- |
|_claim_names|
|_claim_sources|
|access_token|
|Account_type|
|acr|
|Actor|
|actortoken|
|AIO|
|altsecid|
|AMR|
|app_chain|
|app_displayname|
|app_res|
|appctx|
|appctxsender|
|AppID|
|appidacr|
|Verklaring|
|at_hash|
|AUD|
|auth_data|
|auth_time|
|authorization_code|
|azp|
|azpacr|
|c_hash|
|ca_enf|
|CC|
|cert_token_use|
|client_id|
|cloud_graph_host_name|
|cloud_instance_name|
|CNF|
|code|
|besturingselementen|
|credential_keys|
|aanvraag voor Certificaatondertekening|
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
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/AuthenticationInstant|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/AuthenticationMethod|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/Expiration|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/EXPIRED|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/EmailAddress|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/name|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/NameIdentifier|
|IAT|
|identityprovider|
|IDP|
|in_corp|
|instantie|
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
|NBF|
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
|PUID|
|pwd_exp|
|pwd_url|
|redirect_uri|
|refresh_token|
|refreshtoken|
|request_nonce|
|resource|
|rol|
|rolls|
|Bereik|
|SCP|
|beveiligings-id|
|Handtekening|
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

#### <a name="table-2-security-assertion-markup-language-saml-restricted-claim-set"></a>Tabel 2: Security Assertion Markup Language (SAML) beperkt claimset
|Claimtype (URI)|
| ----- |
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/Expiration|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/EXPIRED|
|http://schemas.Microsoft.com/Identity/claims/accesstoken|
|http://schemas.Microsoft.com/Identity/claims/openid2_id|
|http://schemas.Microsoft.com/Identity/claims/identityprovider|
|http://schemas.Microsoft.com/Identity/claims/objectidentifier|
|http://schemas.Microsoft.com/Identity/claims/PUID|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/NameIdentifier [MR1] |
|http://schemas.Microsoft.com/Identity/claims/tenantid|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/AuthenticationInstant|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/AuthenticationMethod|
|http://schemas.Microsoft.com/AccessControlService/2010/07/claims/identityprovider|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/Groups|
|http://schemas.Microsoft.com/claims/groups.Link|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/Role|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/wids|
|http://schemas.Microsoft.com/2014/09/devicecontext/claims/iscompliant|
|http://schemas.Microsoft.com/2014/02/devicecontext/claims/isknown|
|http://schemas.Microsoft.com/2012/01/devicecontext/claims/ismanaged|
|http://schemas.Microsoft.com/2014/03/psso|
|http://schemas.Microsoft.com/claims/authnmethodsreferences|
|http://schemas.xmlsoap.org/ws/2009/09/Identity/claims/actor|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/samlissuername|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/confirmationkey|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsaccountname|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/primarygroupsid|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/primarysid|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/authorizationdecision|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/Authentication|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/SID|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/denyonlyprimarygroupsid|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/denyonlyprimarysid|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/denyonlysid|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/denyonlywindowsdevicegroup|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsdeviceclaim|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsdevicegroup|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsfqbnversion|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowssubauthority|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsuserclaim|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/x500distinguishedname|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/UPN|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/GroupSID|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/SPN|
|http://schemas.Microsoft.com/ws/2008/06/Identity/claims/ispersistent|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/privatepersonalidentifier|
|http://schemas.Microsoft.com/Identity/claims/scope|

## <a name="claims-mapping-policy-properties"></a>Claims toewijzing van eigenschappen van beleid
Gebruik de eigenschappen van een toewijzing van beleid om te bepalen welke claims worden verzonden en waar de gegevens afkomstig is uit de claimprovider. Als er geen beleid is ingesteld, geeft het systeem tokens met de claimset core, de basic claimset en optionele claims die de toepassing ervoor gekozen heeft te ontvangen.

### <a name="include-basic-claim-set"></a>Basic claimset opnemen

**Tekenreeks:** IncludeBasicClaimSet

**Gegevenstype:** Booleaans (True of False)

**Overzicht:** deze eigenschap bepaalt of de basic claimset is opgenomen in de tokens die worden beïnvloed door dit beleid. 

- Indien ingesteld op True, alle claims in de basis claimset in het beleid van invloed op tokens worden gegenereerd. 
- Als is ingesteld op False, claims in de claimset basic zich niet in de tokens, tenzij ze afzonderlijk in de claims schema-eigenschap van hetzelfde beleid zijn toegevoegd.

>[!NOTE] 
>Claims in de claim kernset zijn aanwezig in elke token, ongeacht wat deze eigenschap is ingesteld op. 

### <a name="claims-schema"></a>Claims schema

**Tekenreeks:** ClaimsSchema

**Gegevenstype:** JSON-blob met een of meer claim schema vermeldingen

**Overzicht:** deze eigenschap wordt gedefinieerd welke claims aanwezig zijn in de tokens die van invloed op een door het beleid ook aan de basis claimset en de kern claim set.
Bepaalde informatie is vereist voor elke claim schema vermelding gedefinieerd voor deze eigenschap. U moet opgeven waar de gegevens vandaan (**waarde** of **paar van de bron-ID**), en dat de gegevens claim wordt verzonden als (**Type Claim**).

### <a name="claim-schema-entry-elements"></a>Claim-vermelding schema-elementen

**Waarde:** het element waarde definieert een statische waarde als de gegevens die moeten worden verzonden in de claim.

**De combinatie van de bron-ID:** elementen van de bron- en -ID definiëren waar de gegevens in de claim afkomstig is uit. 

Het bronelement moet worden ingesteld op een van de volgende: 


- 'gebruiker': de gegevens in de claim is een eigenschap van het gebruikersobject. 
- 'application': de gegevens in de claim is een eigenschap van de toepassing (client) service-principal. 
- 'resource': de gegevens in de claim is een eigenschap van de resource-service-principal.
- 'doelgroep': de gegevens in de claim is een eigenschap van de service-principal is de doelgroep van het token (de client of de resource service-principal).
- 'bedrijf': de gegevens in de claim is een eigenschap op van de resource-tenant bedrijfs-object.
- 'transformatie': de gegevens in de claim afkomstig is van claimtransformatie (Zie de sectie 'Claims transformatie' verderop in dit artikel). 

Als de bron is een transformatie verleent de **TransformationID** element moet worden opgenomen in deze claimdefinitie.

Het ID-element identificeert welke eigenschap op de bron wordt de waarde voor de claim. De volgende tabel bevat de waarden van geldige voor elke waarde van de bron-ID.

#### <a name="table-3-valid-id-values-per-source"></a>Tabel 3: Geldige id-waarden per bron
|Bron|Id|Beschrijving|
|-----|-----|-----|
|Gebruiker|Achternaam|Familienaam|
|Gebruiker|Voornaam|Voornaam|
|Gebruiker|weergavenaam|Weergavenaam|
|Gebruiker|object-id|ObjectID|
|Gebruiker|E-mail|E-mailadres|
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
|Gebruiker|onpremisesuserprincipalname|lokale UPN|
|Gebruiker|mailnickname|E-mail bijnaam|
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
|Gebruiker|othermail|Andere Mail|
|Gebruiker|Land|Land|
|Gebruiker|city|Plaats|
|Gebruiker|state|Status|
|Gebruiker|functie|Functie|
|Gebruiker|Werknemer-id|Werknemer-id|
|Gebruiker|facsimiletelephonenumber|Faxbericht telefoonnummer|
|toepassing, resource, doelgroep|weergavenaam|Weergavenaam|
|toepassing, resource, doelgroep|objecten|ObjectID|
|toepassing, resource, doelgroep|tags|Service-Principal label|
|Bedrijf|tenantcountry|Land van tenant|

**TransformationID:** TransformationID element alleen als het bronelement is ingesteld op 'transformatie' moet worden opgegeven.

- Dit element moet overeenkomen met de ID-element van de transformatie-vermelding in de **ClaimsTransformation** eigenschap die definieert hoe de gegevens voor deze claim wordt gegenereerd.

**Claimtype:** de **JwtClaimType** en **SamlClaimType** elementen definiëren die claim claim schema post verwijst.

- De JwtClaimType moet de naam van de claim te worden verzonden in JWTs bevatten.
- De SamlClaimType moet de URI van de claim te worden verzonden in SAML-tokens bevatten.

>[!NOTE]
>Namen en URI's van claims in de beperkte claimset kan niet worden gebruikt voor het type claimelementen. Zie de sectie 'Uitzonderingen en beperkingen' verderop in dit artikel voor meer informatie.

### <a name="claims-transformation"></a>Claimtransformatie

**Tekenreeks:** ClaimsTransformation

**Gegevenstype:** JSON-blob met een of meer vermeldingen voor transformatie 

**Overzicht:** gebruik van deze eigenschap algemene transformaties toepassen op de brongegevens voor het genereren van de uitvoergegevens voor claims die zijn opgegeven in het Schema van Claims.

**-ID:** met de ID-element verwijzen naar deze transformatie-vermelding in de vermelding TransformationID Claims Schema. Deze waarde moet uniek zijn voor elk item transformatie binnen dit beleid.

**TransformationMethod:** TransformationMethod element identificeert welke bewerking wordt uitgevoerd voor het genereren van de gegevens voor de claim.

Op basis van de gekozen methode, wordt een reeks invoer en uitvoer verwacht. Deze worden gedefinieerd met behulp van de **InputClaims**, **invoerparameters** en **OutputClaims** elementen.

#### <a name="table-4-transformation-methods-and-expected-inputs-and-outputs"></a>Tabel 4: Transformatie methoden en verwachte invoer en uitvoer
|TransformationMethod|Verwachte invoer|Verwachte uitvoer|Beschrijving|
|-----|-----|-----|-----|
|Koppelen|tekenreeks1, tekenreeks2, scheidingselement|outputClaim|Joins invoer tekenreeksen met behulp van een scheidingsteken ertussen. Bijvoorbeeld: tekenreeks1: 'foo@bar.com', tekenreeks2: sandbox '-', scheidingsteken: '. ' resulteert in outputClaim: 'foo@bar.com.sandbox'|
|ExtractMailPrefix|E-mail|outputClaim|Haalt het lokale gedeelte van een e-mailadres. Bijvoorbeeld: e-mail: 'foo@bar.com' resulteert in outputClaim: "foo". Wanneer dit niet het @ is aanmelding aanwezig is, wordt de bestaande invoerreeks is geretourneerd.|

**InputClaims:** met een element InputClaims kunt u de gegevens van een claim schema vermelding doorgeven aan een transformatie. Deze twee kenmerken heeft: **ClaimTypeReferenceId** en **TransformationClaimType**.

- **ClaimTypeReferenceId** wordt samengevoegd met de ID-element van de claim schema vermelding vinden van de juiste invoerclaim wordt aangeduid. 
- **TransformationClaimType** wordt gebruikt voor deze invoer een unieke naam geven. Deze naam moet overeenkomen met een van de verwachte invoer voor de transformatiemethode.

**Invoerparameters:** gebruik van een element invoerparameters te geven van een constante waarde aan een transformatie. Deze twee kenmerken heeft: **waarde** en **ID**.

- **Waarde** de werkelijke constante waarde moet worden doorgegeven.
- **ID** wordt gebruikt voor deze invoer een unieke naam geven. Deze naam moet overeenkomen met een van de verwachte invoer voor de transformatiemethode.

**OutputClaims:** met een element OutputClaims kunt u de gegevens die worden gegenereerd door een transformatie bevatten en deze koppelen aan een claim schema-item. Deze twee kenmerken heeft: **ClaimTypeReferenceId** en **TransformationClaimType**.

- **ClaimTypeReferenceId** wordt samengevoegd met de ID van de claim schema vermelding vinden van de juiste uitvoer claim.
- **TransformationClaimType** wordt gebruikt om aan te geven van een unieke naam voor deze uitvoer. Deze naam moet overeenkomen met een van de verwachte uitvoer voor de transformatiemethode.

### <a name="exceptions-and-restrictions"></a>Uitzonderingen en beperkingen

**SAML NameID en UPN:** de kenmerken van waaruit u de waarden NameID en UPN en de claimtransformaties die zijn toegestaan, bron zijn beperkt.

#### <a name="table-5-attributes-allowed-as-a-data-source-for-saml-nameid"></a>Tabel 5: Kenmerken die zijn toegestaan als een gegevensbron voor SAML-NameID
|Bron|Id|Beschrijving|
|-----|-----|-----|
|Gebruiker|E-mail|E-mailadres|
|Gebruiker|userPrincipalName|User principal name|
|Gebruiker|onpremisessamaccountname|Op de lokale Sam-accountnaam|
|Gebruiker|Werknemer-id|Werknemer-id|
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

#### <a name="table-6-transformation-methods-allowed-for-saml-nameid"></a>Tabel 6: Transformatie methoden zijn toegestaan voor SAML-NameID
|TransformationMethod|Beperkingen|
| ----- | ----- |
|ExtractMailPrefix|None|
|Koppelen|Het achtervoegsel wordt toegevoegd, moet een geverifieerd domein van de resource-tenant.|

### <a name="custom-signing-key"></a>Aangepaste ondertekeningssleutel
Een aangepaste handtekeningsleutel moet worden toegewezen aan het service-principal-object voor een claimprovider toewijzing van beleid kracht te laten worden. Alle tokens uitgegeven hebben zijn beïnvloed door het beleid zijn ondertekend met deze sleutel. Toepassingen moeten worden geconfigureerd voor tokens accepteert ondertekend met deze sleutel. Dit zorgt ervoor bevestiging dat tokens zijn gewijzigd door de maker van de claims die toewijzing van beleid. Deze beschermt toepassingen tegen toewijzing van beleid dat is gemaakt door schadelijke actoren claims.

### <a name="cross-tenant-scenarios"></a>Cross-tenantscenario 's
Claims koppelen van beleidsregels zijn niet van toepassing op gastgebruikers. Als een gastgebruiker probeert te krijgen tot een toepassing met een beleid dat is toegewezen aan de service-principal toewijzing van de claimprovider, het standaardtoken is uitgegeven (het beleid heeft geen effect).

## <a name="claims-mapping-policy-assignment"></a>Claims beleidstoewijzing toewijzing
Claims toewijzing van beleid kunnen alleen worden toegewezen aan de service-principals.

### <a name="example-claims-mapping-policies"></a>Voorbeeld van claims toewijzing van beleid

In Azure AD zijn veel scenario's mogelijk als u claims die worden verzonden in tokens voor een specifieke service-principals kunt aanpassen. In deze sectie doorlopen we enkele algemene scenario's kunt u het gebruik van de toewijzing van beleidstype claims te vatten.

#### <a name="prerequisites"></a>Vereisten
In de volgende voorbeelden u maken, bijwerken, koppelen en beleidsregels voor service-principals verwijderen. Als u niet bekend met Azure AD bent, raden wij u meer informatie over het ophalen van een Azure AD-tenant voordat u met deze voorbeelden doorgaat. 

Voer de volgende stappen uit om te beginnen:


1. Download de meest recente [public preview-versie van Azure AD PowerShell-Module](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.127).
2.  Voer de opdracht Connect aan te melden bij uw Azure AD-beheeraccount. Deze opdracht uitvoeren telkens wanneer starten u een nieuwe sessie.
    
     ``` powershell
    Connect-AzureAD -Confirm
    
    ```
3.  Overzicht van alle beleidsregels die zijn gemaakt in uw organisatie, moet u de volgende opdracht uitvoeren. Het is raadzaam dat u deze opdracht nadat de meeste bewerkingen in de volgende scenario's om te controleren uitvoeren dat de beleidsregels zijn gemaakt zoals verwacht.
   
    ``` powershell
        Get-AzureADPolicy
    
    ```
#### <a name="example-create-and-assign-a-policy-to-omit-the-basic-claims-from-tokens-issued-to-a-service-principal"></a>Voorbeeld: Maken en toewijzen van een beleid voor de eenvoudige claims van tokens die zijn verleend aan een service-principal weglaten.
In dit voorbeeld maakt u een beleid dat Hiermee verwijdert u de eenvoudige claim ingesteld van tokens die zijn uitgegeven aan de gekoppelde service-principals.


1. Maak een van de claimprovider toewijzing van beleid. Dit beleid, gekoppeld aan specifieke service-principals, verwijdert de eenvoudige claim ingesteld op basis van tokens.
    1. Voor het maken van het beleid, moet u deze opdracht uitvoeren: 
    
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims” -Type "ClaimsMappingPolicy"
    ```
    2. Voer de volgende opdracht om te zien van het nieuwe beleid en het beleid voor object-id ophalen:
    
     ``` powershell
    Get-AzureADPolicy
    ```
2.  Het beleid aan uw service-principal toewijzen. Ook moet u de object-id van uw service principal. 
    1.  Overzicht van de service-principals van uw organisatie, kunt u Microsoft Graph opvragen. Of in Azure AD Graph Explorer, moet u zich aanmelden bij uw Azure AD-account.
    2.  Wanneer u de object-id van uw service-principal hebt, voer de volgende opdracht:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
    ```
#### <a name="example-create-and-assign-a-policy-to-include-the-employeeid-and-tenantcountry-as-claims-in-tokens-issued-to-a-service-principal"></a>Voorbeeld: Maken en toewijzen van een beleid voor het opnemen van de werknemer-id en TenantCountry als claims in tokens die zijn verleend aan een service-principal.
In dit voorbeeld maakt u een beleid dat de werknemer-id en TenantCountry toegevoegd aan de tokens die zijn uitgegeven aan de gekoppelde service-principals. De werknemer-id is als het claimtype naam in SAML-tokens en JWTs verzonden. De TenantCountry wordt verzonden als het landclaimtype in SAML-tokens en JWTs. In dit voorbeeld gaan we bevatten de eenvoudige claims in de tokens instellen.

1. Maak een van de claimprovider toewijzing van beleid. Dit beleid is gekoppeld aan een specifieke service-principals, wordt de werknemer-id en TenantCountry claims voor tokens toegevoegd.
    1. Voor het maken van het beleid, moet u deze opdracht uitvoeren:  
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name","JwtClaimType":"name"},{"Source":"company","ID":" tenantcountry ","SamlClaimType":" http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country ","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample” -Type "ClaimsMappingPolicy"
    ```
    
    2. Voer de volgende opdracht om te zien van het nieuwe beleid en het beleid voor object-id ophalen:
     
     ``` powershell  
    Get-AzureADPolicy
    ```
2.  Het beleid aan uw service-principal toewijzen. Ook moet u de object-id van uw service principal. 
    1.  Overzicht van de service-principals van uw organisatie, kunt u Microsoft Graph opvragen. Of in Azure AD Graph Explorer, moet u zich aanmelden bij uw Azure AD-account.
    2.  Wanneer u de object-id van uw service-principal hebt, voer de volgende opdracht:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
    ```
#### <a name="example-create-and-assign-a-policy-that-uses-a-claims-transformation-in-tokens-issued-to-a-service-principal"></a>Voorbeeld: Maken en toewijzen van een beleid dat gebruikmaakt van een claimtransformatie in tokens die zijn verleend aan een service-principal.
In dit voorbeeld maakt u een beleid dat u een aangepaste claim 'JoinedData' moet worden verleend aan de gekoppelde service-principals JWTs verzendt. Deze claim bevat een waarde die is gemaakt door de gegevens die zijn opgeslagen in het kenmerk extensionattribute1 van het gebruikersobject met '.sandbox'. In dit voorbeeld uitsluiten we de eenvoudige claims in de tokens instellen.


1. Maak een van de claimprovider toewijzing van beleid. Dit beleid is gekoppeld aan een specifieke service-principals, wordt de werknemer-id en TenantCountry claims voor tokens toegevoegd.
    1. Voor het maken van het beleid, moet u deze opdracht uitvoeren: 
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformations":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"ID":"string2","Value":"sandbox"},{"ID":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample" -Type "ClaimsMappingPolicy" 
    ```
    
    2. Voer de volgende opdracht om te zien van het nieuwe beleid en het beleid voor object-id ophalen: 
     
     ``` powershell
    Get-AzureADPolicy
    ```
2.  Het beleid aan uw service-principal toewijzen. Ook moet u de object-id van uw service principal. 
    1.  Overzicht van de service-principals van uw organisatie, kunt u Microsoft Graph opvragen. Of in Azure AD Graph Explorer, moet u zich aanmelden bij uw Azure AD-account.
    2.  Wanneer u de object-id van uw service-principal hebt, voer de volgende opdracht: 
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
    ```

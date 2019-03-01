---
title: Groepclaims van toepassingen met Azure Active Directory configureren | Microsoft Docs
description: Informatie over het configureren van groepclaims voor gebruik met Azure AD.
services: active-directory
documentationcenter: ''
ms.reviewer: paulgarn
manager: daveba
ms.component: hybrid
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 02/27/2019
ms.author: billmath
author: billmath
ms.openlocfilehash: 8e4f9e76baf07e6ea2cb4cccb63ed0a9add5d767
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57012047"
---
# <a name="configure-group-claims-for-applications-with-azure-active-directory-public-preview"></a>Configureren van groepclaims voor toepassingen met Azure Active Directory (openbare Preview)

Azure Active Directory krijgt u een gebruikers informatie over het groepslidmaatschap in tokens voor gebruik in toepassingen.  Twee belangrijkste patronen worden ondersteund:

- Groepen die worden aangeduid met hun Azure Active Directory-object-id (OID) (algemeen beschikbaar)
- Groepen die zijn geïdentificeerd door de SAMAccountName of GroupSID voor Active Directory (AD) gesynchroniseerd groepen en gebruikers (openbare Preview)

>[!Note]
>Ondersteuning voor het gebruik van namen en on-premises beveiligings-id's (SID's) is ontworpen om in te schakelen om bestaande toepassingen verplaatsen van AD FS.    Groepen die worden beheerd in Azure AD niet de kenmerken die nodig zijn voor het verzenden van deze claims bevatten.

## <a name="group-claims-for-applications-migrating-from-ad-fs-and-other-idps"></a>Groepclaims van toepassingen migreren van AD FS en andere id-providers

Veel toepassingen die zijn geconfigureerd voor verificatie met AD FS is afhankelijk van gegevens van het groepslidmaatschap in de vorm van kenmerken voor Windows AD-groep.   Deze kenmerken zijn de SAMAccountName, die mogelijk door-domeinnaam van de groep of de SID van de Windows-groep.  Wanneer de toepassing is gefedereerd met AD FS, AD FS maakt gebruik van de functie TokenGroups om op te halen van het lidmaatschap van groepen voor de gebruiker.

Om het token dat een app van AD FS ontvangen wilt, groep en de rol van claims kunnen worden verzonden met het domein gekwalificeerde SAMAccountName in plaats van Azure Active Directory-id van de groep.

De ondersteunde indelingen voor groepclaims zijn:

- **Azure Active Directory-GroupObjectId** (beschikbaar voor alle groepen)
- **SAMAccountName** (beschikbaar voor groepen die zijn gesynchroniseerd vanuit Active Directory)
- **NetbiosDomain\samAccountName** (beschikbaar voor groepen die zijn gesynchroniseerd vanuit Active Directory)
- **DNSDomainName\samAccountName** (beschikbaar voor groepen die zijn gesynchroniseerd vanuit Active Directory)

>[!NOTE]
>SAMAccountName en OnPremisesGroupSID kenmerken zijn alleen beschikbaar op de groepsobjecten worden gesynchroniseerd vanuit Active Directory.   Ze zijn niet beschikbaar is op groepen die zijn gemaakt in Azure Active Directory of Office 365.   Toepassingen die afhankelijk van on-premises groepskenmerken zijn ophalen ze voor alleen gesynchroniseerde groepen.

## <a name="options-for-applications-to-consume-group-information"></a>Opties voor toepassingen die gegevens gebruiken

Eén manier om toepassingen te verkrijgen van informatie over is voor het aanroepen van de Graph-eindpunt voor groepen om op te halen van het lidmaatschap voor de geverifieerde gebruiker. Deze aanroep zorgt ervoor dat alle groepen die een gebruiker lid van zijn beschikbaar is, zelfs wanneer er een groot aantal groepen die betrokken zijn en nodig is om inventariseren van alle groepen die de gebruiker lid van is de toepassing.  Opsomming van de groep is vervolgens onafhankelijk van de beperkingen van de token bestandsgrootte.

Als al een bestaande toepassing verwacht te gebruiken van gegevens via de claims in het token dat wordt ontvangen, kan er echter Azure Active Directory worden geconfigureerd met een aantal opties voor verschillende claims aan de behoeften van de toepassing.  Houd rekening met de volgende opties:

- Wanneer u het lidmaatschap van een toepassing autorisatie-doel (of groepslidmaatschap is verkregen van het token of grafiek), is het beter gebruik van de ObjectID-groep is onveranderbaar en uniek is in Azure Active Directory en beschikbaar voor alle groepen .
- Als u de groep SAMAccountName voor autorisatie, gekwalificeerde domeinnamen gebruiken;  Er is minder kans op situaties ontstaan zijn namen conflicteert. SAMAccountName op zichzelf kan uniek zijn binnen een Active Directory-domein, maar bestaat de mogelijkheid van meer dan één groep in de dezelfde naam als meer dan één Active Directory-domein is gesynchroniseerd met een Azure Active Directory-tenant.
- Overweeg het gebruik van [toepassingsrollen](../../active-directory/develop/howto-add-app-roles-in-azure-ad-apps.md) voor een laag van op die manier tussen het groepslidmaatschap en de toepassing.   De toepassing wordt vervolgens kunt u interne autorisatie beslissingen te nemen op basis van rol clams in het token.
- Als de toepassing is geconfigureerd voor het ophalen van kenmerken die zijn gesynchroniseerd uit Active Directory en een groep die deze niet in de claims opgenomen kenmerken niet bevatten.
- Groepclaims in tokens opnemen geneste groepen.   Als een gebruiker lid van GroupB is en GroupB lid van GroupA is, klikt u vervolgens bevat de groepclaims voor de gebruiker zowel GroupA als GroupB. Het aantal groepen die worden vermeld in het token kan de grootte van de token groeien voor organisaties met intensief gebruik van geneste groepen en gebruikers met een groot aantal groepslidmaatschappen.   Azure Active Directory beperkt het aantal groepen die deze in een 150 voor SAML-asserties ondertekend en 200 voor JWT-token wordt verzenden.

>Vereisten voor het gebruik van de kenmerken van de groep zijn gesynchroniseerd vanaf Active Directory:   De groepen die moeten worden gesynchroniseerd vanuit Active Directory met Azure AD Connect.

Er zijn twee stappen voor het configureren van Azure Active Directory voor het verzenden van namen voor Active Directory-groepen.

1. **Namen van Active Directory synchroniseren** voordat u Azure Active Directory de groepsnamen kan verzenden of op de lokale groep SID in de groep of rol van claims, de vereiste kenmerken moeten worden gesynchroniseerd vanuit Active Directory.  U moet beschikken over Azure AD Connect versie 1.2.70 of hoger.   Voorafgaand aan versie 1.2.70 Azure AD Connect th groepsobjecten uit Active Directory worden gesynchroniseerd, maar niet de vereiste kenmerken van de naam niet standaard opgenomen.  U dient te upgraden naar de huidige versie.

2. **Configureren van de registratie van de toepassing in Azure Active Directory om op te nemen van groepclaims in tokens** groepclaims kunnen worden geconfigureerd in de sectie zakelijke toepassingen van de portal voor een galerie of niet in de galerij SAML SSO-toepassing, of met behulp van het Manifest van de toepassing in de sectie Toepassingsregistraties.  Groepclaims configureren in de application manifest Zie "configureren met de Azure Active Directory-Toepassingsregistratie voor groepskenmerken ' hieronder.

## <a name="configure-group-claims-for-saml-applications-using-sso-configuration"></a>Configureren van groepclaims voor SAML-toepassingen met behulp van SSO-configuratie

Voor het configureren van groepclaims voor een galerie of niet in de galerij SAML-toepassing, zakelijke toepassingen te openen, klikt u op de toepassing in de lijst en selecteer Single Sign On configuratie.

Selecteer het bewerkingspictogram naast 'Groepen geretourneerd in token'

![claims UI](media/how-to-connect-fed-group-claims/group-claims-ui-1.png)

Gebruik de keuzerondjes te selecteren welke groepen moeten worden opgenomen in het token

![claims UI](media/how-to-connect-fed-group-claims/group-claims-ui-2.png)

| Selectie | Description |
|----------|-------------|
| **Alle groepen** | Beveiligingsgroepen en distributie verzendt bevat.   Het wordt ook Directory-rollen die de gebruiker is toegewezen aan het in een claim 'wids' worden verzonden en worden eventuele toepassingsrollen die de gebruiker is toegewezen aan in de rollenclaim worden verzonden. |
| **Beveiligingsgroepen** | Verzendt de beveiligingsgroepen waarvan die de gebruiker lid van de in de claim van groepen is |
| **Distributielijst** | De gebruiker lid van is distributielijsten verzendt |
| **Directory-rol** | Als de gebruiker is toegewezen maprollen worden ze verzonden als een 'wids' claim (groepen claim wordt niet verzonden) |

Bijvoorbeeld, als u wilt verzenden alle beveiligingsgroepen die de gebruiker een lid van is, beveiligingsgroepen selecteren

![claims UI](media/how-to-connect-fed-group-claims/group-claims-ui-3.png)

### <a name="advanced-options"></a>Geavanceerde opties

De manier waarop groepclaims worden verzonden, kan worden gewijzigd door de instellingen onder Geavanceerde opties

De naam van de groepclaim aanpassen:  Indien geselecteerd, kan een ander claimtype voor groepclaims worden opgegeven.   Voer het claimtype in het veld naam en de optionele naamruimte voor de claim in het veld voor de naamruimte.

![claims UI](media/how-to-connect-fed-group-claims/group-claims-ui-4.png)

Om te verzenden van groepen met behulp van Active Directory kenmerken in plaats van Azure AD-extensies Schakel het selectievakje 'Return groepen aan de namen in plaats van id's ' en selecteer de indeling in de vervolgkeuzelijst.  Dit vervangt de object-ID in de claims door tekenreekswaarden met groepsnamen.   Alleen de groepen die zijn gesynchroniseerd vanuit Active Directory worden opgenomen in de claims.

![claims UI](media/how-to-connect-fed-group-claims/group-claims-ui-5.png)

Sommige toepassingen vereisen de gegevens van het groepslidmaatschap wordt weergegeven in de claim 'rol'. U kunt eventueel de gebruikersgroepen als rollen introduceren door het selectievakje 'Groepen die een rol claims verzenden'.  

![claims UI](media/how-to-connect-fed-group-claims/group-claims-ui-6.png)

>[!NOTE]
>Als de optie voor het verzenden van groepsgegevens als rollen wordt gebruikt, worden alleen de groepen wordt weergegeven in de rol-claim.  Alle toepassingsrollen die de gebruiker is toegewezen weergegeven niet in de rol-claim.

## <a name="configure-the-azure-ad-application-registration-for-group-attributes"></a>De registratie van de Azure AD-toepassing voor groepskenmerken configureren  

Groepclaims kunnen ook worden geconfigureerd de [optionele Claims](../../active-directory/develop/active-directory-optional-claims.md) sectie van de [Manifest van de toepassing](../../active-directory/develop/reference-app-manifest.md).

 1. In de portal -> Azure Active Directory -> Application rapporten -> Selecteer Application Manifest ->

 2. Groepslidmaatschap claims inschakelen door het veranderen van de groupMembershipClaim
 
    De geldige waarden zijn:

    - 'Alle'
    - "SecurityGroup"
    - "DistributionList"
    - "DirectoryRole"

    Bijvoorbeeld:

    ```
    "groupMembershipClaims": "SecurityGroup"
    ```
 
    Standaard die groep objectid's wordt verzonden in de groep claimwaarde.  Voor het wijzigen van de waarde van de claim bevat op de lokale groepskenmerken of te wijzigen van het claimtype rol, moet u OptionalClaims configuratie als volgt gebruiken:

 3. De naam van configuratie optioneel groepclaims instellen.

    Als u groepen in het token bevat de on-premises AD-groepskenmerken in de sectie optionele claims welke tokentype optioneel claim moet worden toegepast opgeven op, de naam van de optionele claim aangevraagd en eventuele aanvullende eigenschappen gewenst.  Meerdere typen tokens kunnen worden weergegeven:

    - idToken voor het token OIDC-ID
    - accessToken voor de OAuth/OIDC-toegangstoken
    - Saml2Token voor SAML-tokens.

    >[!NOTE]
    >Het type Saml2Token is van toepassing op zowel SAML1.1 en SAML2.0 indeling tokens  

    Voor elke relevante tokentype, wijzigt u de claim de groepen die moet worden gebruikt u de sectie OptionalClaims in het manifest. Het schema OptionalClaims is als volgt:

 ```
 {
    "name": "groups",
    "source": null,
    "essential": false,
    "additionalProperties": []
 }
 ```

 | Optionele Claims Schema | Value |
 |----------|-------------|
 | **Naam:** | Moet 'groepen' |
 | **Bron:** | Niet gebruikt. Weglaat of geef null-waarde |
 | **essential:** | Niet gebruikt. Weglaat of ONWAAR opgeven |
 | **additionalProperties:** | Lijst met extra eigenschappen.  Valid options are "sam_account_name", “dns_domain_and_sam_account_name”, “netbios_domain_and_sam_account_name”, "emit_as_roles" |

 In additionalProperties slechts één 'sam_account_name","dns_domain_and_sam_account_name", zijn"netbios_domain_and_sam_account_name"vereist.  Als meer dan één aanwezig is, wordt de eerste wordt gebruikt en alle andere genegeerd.

 Sommige toepassingen vereisen groepsinformatie over de gebruiker in de rol-claim.  Het claimtype om toe te voegen uit een groepclaim op een claim rol, "emit_as_roles" om aanvullende eigenschappen te wijzigen.  De waarden van de groep wordt in de claim rol worden verzonden.

 >[!NOTE]
 >Als 'emit_as_roles' wordt gebruikt een toepassingsrollen geconfigureerd dat de gebruiker wordt toegewezen niet wordt weergegeven in de claim rol

### <a name="examples"></a>Voorbeelden

Groepen aan de groepsnamen van de in OAuth-toegangstokens dnsDomainName\SAMAccountName indeling verzenden

```
"optionalClaims": {
    "accessToken": [{
        "name": "groups",
        "additionalProperties": ["dns_domain_and_sam_account_name"]
    }]
}
 ```

Namen moeten worden geretourneerd in netbiosDomain\samAccountName indeling als de rollen in SAML en OIDC-ID-Tokens claim verzenden:

```
"optionalClaims": {
    "saml2Token": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }],

    "idToken": [{
        "name": "groups",
        "additionalProperties": ["netbios_name_and_sam_account_name", "emit_as_roles"]
    }]
 }
 ```

## <a name="next-steps"></a>Volgende stappen

[Wat is hybride identiteit?](whatis-hybrid-identity.md)
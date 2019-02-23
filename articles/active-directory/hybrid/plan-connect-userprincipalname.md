---
title: Populatie van Azure AD UserPrincipalName
description: Het volgende document wordt beschreven hoe het kenmerk UserPrincipalName wordt ingevuld.
author: billmath
ms.subservice: hybrid
ms.author: billmath
ms.date: 06/26/2018
ms.topic: conceptual
ms.workload: identity
ms.service: active-directory
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: c198b329f07c5c7459f25165b2dc0a3bfa032276
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56672845"
---
# <a name="azure-ad-userprincipalname-population"></a>Populatie van Azure AD UserPrincipalName

Dit artikel wordt beschreven hoe het kenmerk UserPrincipalName wordt ingevuld in Azure Active Directory (Azure AD).
De waarde van het kenmerk UserPrincipalName is de Azure AD-gebruikersnaam voor de gebruikersaccounts.

## <a name="upn-terminology"></a>UPN-terminologie
In dit artikel gebruikt de volgende terminologie:

|Termijn|Description|
|-----|-----|
|Eerste domein|Het standaarddomein (onmicrosoft.com) in de Azure AD-Tenant. Bijvoorbeeld contoso.onmicrosoft.com.|
|Microsoft Online routering E‑mailadres (MOERA)|Berekent de MOERA van het kenmerk MailNickName van Azure AD en de eerste Azure AD-domein als de Azure AD &lt;MailNickName&gt;&#64;&lt;eerste domein&gt;.|
|On-premises mailNickName kenmerk|Een kenmerk in Active Directory, waarvan de waarde vertegenwoordigt de alias van een gebruiker in een Exchange-organisatie.|
|On-premises-e-mailkenmerk|Een kenmerk in Active Directory, de waarde die staat voor het e-mailadres van een gebruiker|
|Primair SMTP-adres|Het primaire e-mailadres van een ontvanger Exchange-object. Bijvoorbeeld, SMTP:user\@contoso.com.|
|Alternatieve aanmeldings-ID|Een on-premises kenmerk dan UserPrincipalName, zoals e-mailkenmerk, die wordt gebruikt voor aanmelding bij.|

## <a name="what-is-userprincipalname"></a>Wat is UserPrincipalName?
UserPrincipalName is een kenmerk dat is een aanmeldingsnaam voor Internet-stijl voor een gebruiker op basis van de standaard Internet [RFC 822](https://www.ietf.org/rfc/rfc0822.txt). 

### <a name="upn-format"></a>UPN-indeling
Een UPN bestaat uit een UPN-voorvoegsel (naam van het gebruikersaccount) en een UPN-achtervoegsel (een DNS-domeinnaam). Het voorvoegsel wordt samengevoegd met het achtervoegsel met behulp van de '\@"symbool. Bijvoorbeeld, "iemand\@voorbeeld.com '. Een UPN moet uniek zijn in alle beveiligings-principals in een directory-forest. 

## <a name="upn-in-azure-ad"></a>UPN in Azure AD 
De UPN wordt gebruikt door Azure AD om toe te staan gebruikers om aan te melden.  De UPN die een gebruiker kunt gebruiken, is afhankelijk van of het domein is geverifieerd.  Als het domein is geverifieerd, klikt u vervolgens een gebruiker met dat achtervoegsel toegestaan om aan te melden bij Azure AD.  

Het kenmerk is gesynchroniseerd met Azure AD Connect.  U kunt tijdens de installatie van de domeinen die zijn geverifieerd en de virtuele machines die niet weergeven.

   ![Niet-geverifieerde domeinen](./media/plan-connect-userprincipalname/unverifieddomain.png) 

## <a name="alternate-login-id"></a>Alternatieve aanmeldings-ID
In sommige omgevingen kan eindgebruikers alleen worden op de hoogte van het e-mailadres en niet hun UPN.  Het gebruik van e-mailadres kan worden veroorzaakt door een zakelijke beleid of een on-premises line-of-business-toepassingsafhankelijkheden.

Alternatieve aanmeldings-ID kunt u een aanmelding configureren waar gebruikers kunnen zich aanmelden met een kenmerk dan hun UPN, zoals e-mail.

Om in te schakelen alternatieve aanmeldings-ID met Azure AD, zijn geen aanvullende configuraties stappen nodig bij het gebruik van Azure AD Connect. Alternatieve ID kan rechtstreeks vanuit de wizard worden geconfigureerd. Zie Azure AD-aanmelden-configuratie voor uw gebruikers in de sectie synchronisatie. Onder de **User Principal Name** vervolgkeuzelijst, selecteert u het kenmerk voor alternatieve aanmeldings-ID.

![Niet-geverifieerde domeinen](./media/plan-connect-userprincipalname/altloginid.png)  

Zie voor meer informatie, [configureren Alternate login ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) en [configuratie van aanmelding bij Azure AD](how-to-connect-install-custom.md#azure-ad-sign-in-configuration)

## <a name="non-verified-upn-suffix"></a>Non-verified UPN Suffix
Als het achtervoegsel van on-premises UserPrincipalName kenmerk/alternatieve aanmeldings-ID is niet geverifieerd met Azure AD-Tenant, wordt de waarde van het Azure AD UserPrincipalName-kenmerk ingesteld op MOERA. Berekent de MOERA uit het kenmerk MailNickName van Azure AD en de eerste Azure AD-domein als de Azure AD &lt;MailNickName&gt;&#64;&lt;eerste domein&gt;.

## <a name="verified-upn-suffix"></a>Geverifieerde UPN-achtervoegsel
Als de on-premises-UserPrincipalName-kenmerk/alternatieve wordt aanmeldings-ID achtervoegsel gecontroleerd met de Azure AD-Tenant, en vervolgens de waarde van het Azure AD UserPrincipalName-kenmerk is het verstandig om hetzelfde als de waarde van on-premises UserPrincipalName kenmerk/alternatieve aanmeldings-ID.

## <a name="azure-ad-mailnickname-attribute-value-calculation"></a>Azure AD MailNickName attribute value calculation
Omdat de waarde van het Azure AD UserPrincipalName-kenmerk kan worden ingesteld op MOERA, is het belangrijk om te begrijpen hoe de kenmerkwaarde MailNickName van Azure AD, die het voorvoegsel MOERA is, wordt berekend.

Wanneer een gebruikersobject is gesynchroniseerd met een Azure AD-Tenant voor de eerste keer, wordt Azure AD controleert of de volgende items in de opgegeven volgorde en de waarde van het kenmerk MailNickName ingesteld met het eerste bestaande:

- On-premises mailNickName kenmerk
- Het voorvoegsel van het primaire SMTP-adres
- Het voorvoegsel van on-premises-e-mailkenmerk
- Het voorvoegsel van on-premises userPrincipalName-kenmerk/alternatieve aanmeldings-ID
- Het voorvoegsel van de secundaire SMTP-adres

Wanneer de updates naar een gebruikersobject worden gesynchroniseerd met de Azure AD-Tenant, Azure AD de waarde van het kenmerk MailNickName werkt alleen als er een update voor de waarde van het on-premises mailNickName-kenmerk.

>[!IMPORTANT]
>Azure AD wordt de waarde van het kenmerk UserPrincipalName opnieuw berekend alleen als een update voor de waarde van on-premises UserPrincipalName kenmerk/alternatieve aanmeldings-ID wordt gesynchroniseerd met de Azure AD-Tenant. 
>
>Wanneer Azure AD het kenmerk UserPrincipalName berekend, wordt de MOERA ook opnieuw berekend. 

## <a name="upn-scenarios"></a>UPN-scenario 's
Hieronder vindt u scenario's met voorbeelden van hoe de UPN wordt berekend op basis van het gegeven scenario.

### <a name="scenario-1-non-verified-upn-suffix--initial-synchronization"></a>Scenario 1: Niet-gecontroleerde UPN-achtervoegsel: initiële synchronisatie

![Scenario1](./media/plan-connect-userprincipalname/example1.png)

On-Premises user-object:
- mailNickName      : &lt;not set&gt;
- proxyAddresses: {SMTP:us1@contoso.com}
- e-mail: us2@contoso.com
- userPrincipalName : us3@contoso.com`

Het gebruikersobject dan met Azure AD-Tenant voor de eerste keer is gesynchroniseerd
- MailNickName van Azure AD-kenmerk ingesteld op primaire SMTP-adresvoorvoegsel.
- MOERA ingesteld op &lt;MailNickName&gt;&#64;&lt;eerste domein&gt;.
- Azure AD UserPrincipalName-kenmerk ingesteld op MOERA.

Azure AD-Tenant gebruikersobject:
- MailNickName      : us1           
- UserPrincipalName : us1@contoso.onmicrosoft.com


### <a name="scenario-2-non-verified-upn-suffix--set-on-premises-mailnickname-attribute"></a>Scenario 2: Niet-gecontroleerde UPN-achtervoegsel: set on-premises kenmerk mailNickName

![Scenario2](./media/plan-connect-userprincipalname/example2.png)

On-Premises user-object:
- mailNickName      : us4
- proxyAddresses: {SMTP:us1@contoso.com}
- e-mail: us2@contoso.com
- userPrincipalName : us3@contoso.com

Synchroniseren van de update op de on-premises mailNickName kenmerk aan Azure AD-Tenant
- Kenmerk MailNickName van Azure AD met on-premises mailNickName kenmerk bijwerken.
- Omdat er geen update voor de on-premises userPrincipalName-kenmerk, is er geen wijziging aan het Azure AD UserPrincipalName-kenmerk.

Azure AD-Tenant gebruikersobject:
- MailNickName      : us4
- UserPrincipalName : us1@contoso.onmicrosoft.com

### <a name="scenario-3-non-verified-upn-suffix--update-on-premises-userprincipalname-attribute"></a>Scenario 3: Niet-gecontroleerde UPN-achtervoegsel: update on-premises kenmerk userPrincipalName

![Scenario3](./media/plan-connect-userprincipalname/example3.png)

On-Premises user-object:
- mailNickName      : us4
- proxyAddresses: {SMTP:us1@contoso.com}
- e-mail: us2@contoso.com
- userPrincipalName : us5@contoso.com

Synchroniseren van de update op de on-premises userPrincipalName-kenmerk aan Azure AD-Tenant
- Update voor on-premises kenmerk userPrincipalName activeert herberekening van MOERA en Azure AD UserPrincipalName-kenmerk.
- MOERA ingesteld op &lt;MailNickName&gt;&#64;&lt;eerste domein&gt;.
- Azure AD UserPrincipalName-kenmerk ingesteld op MOERA.

Azure AD-Tenant gebruikersobject:
- MailNickName      : us4
- UserPrincipalName : us4@contoso.onmicrosoft.com

### <a name="scenario-4-non-verified-upn-suffix--update-primary-smtp-address-and-on-premises-mail-attribute"></a>Scenario 4: Niet-gecontroleerde UPN-achtervoegsel: het primaire SMTP-adres bijwerken en on-premises kenmerk mail

![Scenario4](./media/plan-connect-userprincipalname/example4.png)

On-Premises user-object:
- mailNickName      : us4
- proxyAddresses: {SMTP:us6@contoso.com}
- e-mail: us7@contoso.com
- userPrincipalName : us5@contoso.com

Synchroniseren van de update op de on-premises-e-mailkenmerk en het primaire SMTP-adres aan Azure AD-Tenant
- Nadat de initiële synchronisatie van het gebruikersobject is bijgewerkt naar de on-premises e-mail kenmerk en het primaire SMTP-adres heeft geen invloed op de Azure AD-MailNickName of het kenmerk UserPrincipalName.

Azure AD-Tenant gebruikersobject:
- MailNickName      : us4
- UserPrincipalName : us4@contoso.onmicrosoft.com

### <a name="scenario-5-verified-upn-suffix--update-on-premises-userprincipalname-attribute-suffix"></a>Scenario 5: Geverifieerde UPN-achtervoegsel: update on-premises userPrincipalName-kenmerk achtervoegsel

![Scenario5](./media/plan-connect-userprincipalname/example5.png)

On-Premises user-object:
- mailNickName      : us4
- proxyAddresses: {SMTP:us6@contoso.com}
- e-mail: us7@contoso.com
- userPrincipalName : us5@verified.contoso.com

Synchroniseren van de update op de on-premises userPrincipalName-kenmerk met de Azure AD-Tenant
- Update voor on-premises userPrincipalName-kenmerk triggers herberekening van de Azure AD UserPrincipalName-kenmerk.
- Azure AD UserPrincipalName-kenmerk ingesteld op on-premises kenmerk userPrincipalName, zoals het UPN-achtervoegsel is geverifieerd met de Azure AD-Tenant.

Azure AD-Tenant gebruikersobject:
- MailNickName      : us4     
- UserPrincipalName : us5@verified.contoso.com

## <a name="next-steps"></a>Volgende stappen
- [Uw on-premises directory's integreren met Azure Active Directory](whatis-hybrid-identity.md)
- [Aangepaste installatie van Azure AD Connect](how-to-connect-install-custom.md)

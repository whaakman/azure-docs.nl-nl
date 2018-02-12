---
title: Azure AD UserPrincipalName populatie
description: Het volgende document wordt beschreven hoe het kenmerk UserPrincipalName is gevuld.
author: billmath
ms.author: billmath
ms.date: 02/02/2018
ms.topic: article
ms.workload: identity
ms.service: active-Directory
manager: mtillman
ms.openlocfilehash: 96b12fbddd4293c55e9029b194416541ca44c622
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2018
---
# <a name="azure-ad-userprincipalname-population"></a>Azure AD UserPrincipalName populatie

Dit artikel wordt beschreven hoe het kenmerk UserPrincipalName is ingevuld in Azure Active Directory (Azure AD).
De waarde van het kenmerk UserPrincipalName is de Azure AD-gebruikersnaam voor de gebruikersaccounts.

## <a name="upn-terminology"></a>UPN-terminologie
De volgende terminologie wordt in dit artikel gebruikt:

|Termijn|Beschrijving|
|-----|-----|
|Eerste domein|Het standaarddomein (onmicrosoft.com) in de Azure AD-Tenant. Bijvoorbeeld: contoso.onmicrosoft.com.|
|Microsoft Online E-mail routering adres (MOERA)|Berekent de MOERA van Azure AD MailNickName kenmerk en de eerste Azure AD-domein als de Azure AD &lt;MailNickName&gt;&#64;&lt; eerste domein&gt;.|
|Kenmerk van lokale mailNickName|Een kenmerk in Active Directory, waarvan de waarde vertegenwoordigt de alias van een gebruiker in een Exchange-organisatie.|
|Lokale e-mailkenmerk|Een kenmerk in Active Directory, waarvan de waarde vertegenwoordigt de e-mailadres van een gebruiker|
|Primair SMTP-adres|Het primaire e-mailadres van een ontvanger Exchange-object. Bijvoorbeeld SMTP:user@contoso.com.|
|Alternatieve aanmeldings-ID|Een lokale kenmerk dan UserPrincipalName, zoals e-mailkenmerk gebruikt voor aanmelden.|

## <a name="what-is-userprincipalname"></a>Wat is UserPrincipalName?
UserPrincipalName is een kenmerk dat een Internet-aanmeldingsnaam voor een gebruiker op basis van de standaard Internet [RFC 822](http://www.ietf.org/rfc/rfc0822.txt). 

### <a name="upn-format"></a>UPN-indeling
Een UPN bestaat uit een UPN-voorvoegsel (naam van de gebruikersaccount) en een UPN-achtervoegsel (een DNS-domeinnaam). Het voorvoegsel op dat wordt samengevoegd met het achtervoegsel met behulp van de ' @ ' symbool. Bijvoorbeeld 'someone@example.com'. Een UPN moet uniek zijn in alle beveiligings-principals binnen een directory-forest. 

## <a name="upn-in-azure-ad"></a>UPN in Azure AD 
De UPN wordt gebruikt door Azure AD om te kunnen gebruikers zich aanmelden.  De UPN die een gebruiker kan gebruiken, is afhankelijk van of het domein is geverifieerd.  Als het domein is geverifieerd, klikt u vervolgens een gebruiker met dat achtervoegsel toegestaan om aan te melden bij Azure AD.  

Het kenmerk is gesynchroniseerd met Azure AD Connect.  U kunt tijdens de installatie van de domeinen die zijn geverifieerd en verbindingen die niet weergeven.

   ![Niet-geverifieerde domeinen](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png) 

## <a name="alternate-login-id"></a>Alternatieve aanmeldings-ID
In sommige omgevingen vanwege bedrijfsbeleid of lokale line-of-business-toepassingsafhankelijkheden, mogen eindgebruikers alleen worden op de hoogte van het e-mailadres en niet de UPN.

Alternatieve aanmeldings-ID kunt u een aanmeldingservaring configureren waarbij gebruikers zich kunnen aanmelden met een kenmerk dan de UPN, zoals e-mail.

Om alternatieve aanmeldings-ID met Azure AD, zijn geen aanvullende configuraties stappen nodig bij gebruik van Azure AD Connect. Alternatieve ID kan rechtstreeks vanuit de wizard worden geconfigureerd. Zie configuratie Azure AD-in voor uw gebruikers in de sectie synchronisatie. Onder de **principalnaam van gebruiker** vervolgkeuzelijst, selecteert u het kenmerk voor alternatieve aanmeldings-ID.

![Niet-geverifieerde domeinen](./media/active-directory-aadconnect-userprincipalname/altloginid.png)  

Zie voor meer informatie [configureren Alternate login ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) en [configuratie van aanmelding bij Azure AD](active-directory-aadconnect-get-started-custom.md#azure-ad-sign-in-configuration)

## <a name="non-verified-upn-suffix"></a>Niet-geverifieerd UPN-achtervoegsel
Als het achtervoegsel lokale UserPrincipalName kenmerk/alternatieve aanmeldings-ID is niet geverifieerd met Azure AD-Tenant, wordt de waarde van het Azure AD UserPrincipalName-kenmerk ingesteld op MOERA. Berekent de MOERA van de Azure AD MailNickName kenmerk en de eerste Azure AD-domein als de Azure AD &lt;MailNickName&gt;&#64;&lt; eerste domein&gt;.

## <a name="verified-upn-suffix"></a>Geverifieerde UPN-achtervoegsel
Als de lokale UserPrincipalName kenmerk/alternatieve is aanmeldings-ID-achtervoegsel gecontroleerd met de Azure AD-Tenant de waarde van het kenmerk UserPrincipalName van Azure AD is het verstandig om hetzelfde zijn als de waarde van lokale UserPrincipalName kenmerk/alternatieve aanmeldings-ID.

## <a name="azure-ad-mailnickname-attribute-value-calculation"></a>Azure AD MailNickName kenmerk berekening
Omdat de waarde van het kenmerk UserPrincipalName van Azure AD kan worden ingesteld op MOERA, is het belangrijk om te begrijpen hoe de Azure AD MailNickName waarde van het kenmerk dat het voorvoegsel MOERA is, wordt berekend.

Wanneer een gebruikersobject is gesynchroniseerd met een Azure AD-Tenant voor de eerste keer, wordt Azure AD controleert het volgende in de gegeven volgorde en stelt u de MailNickName kenmerk-waarde naar de eerste bestaande:

- Kenmerk van lokale mailNickName
- Het voorvoegsel van het primaire SMTP-adres
- Het voorvoegsel van de lokale e-mailkenmerk
- Het voorvoegsel van de lokale userPrincipalName kenmerk/alternatieve aanmeldings-ID
- Het voorvoegsel van de secundaire SMTP-adres

Wanneer de updates van een gebruikersobject worden gesynchroniseerd met de Azure AD-Tenant, Azure AD de kenmerkwaarde MailNickName werkt alleen als er een update voor de waarde van het lokale mailNickName-kenmerk.

>[!IMPORTANT]
>Azure AD wordt de waarde van het kenmerk UserPrincipalName opnieuw berekend alleen als een update voor de waarde van lokale UserPrincipalName kenmerk/alternatieve aanmeldings-ID is gesynchroniseerd met de Azure AD-Tenant. 
>
>Wanneer u Azure AD worden opnieuw berekend met het kenmerk UserPrincipalName, herberekenen het ook de MOERA. 

## <a name="upn-scenarios"></a>UPN-scenario 's
Hieronder vindt u een voorbeeldscenario's van hoe de UPN wordt berekend op basis van het gegeven scenario.

### <a name="scenario-1-non-verified-upn-suffix--initial-synchronization"></a>Scenario 1: Niet-geverifieerd UPN-achtervoegsel: initiële synchronisatie

On-Premises user-object:
- mailNickName: &lt;niet ingesteld&gt;
- proxyAddresses: {SMTP:us1@contoso.com}
- e-mail:us2@contoso.com
- userPrincipalName: us3@contoso.com'

Het gebruikersobject in Azure AD-Tenant voor de eerste keer is gesynchroniseerd
- Azure AD MailNickName kenmerk instellen op voorvoegsel van het primaire SMTP-adres.
- MOERA ingesteld op &lt;MailNickName&gt;&#64;&lt; eerste domein&gt;.
- Azure AD UserPrincipalName kenmerk instellen op MOERA.

Azure AD-Tenant gebruikersobject:
- MailNickName: us1           
- userPrincipalName:us1@contoso.onmicrosoft.com


### <a name="scenario-2-non-verified-upn-suffix--set-on-premises-mailnickname-attribute"></a>Scenario 2: Niet-geverifieerd UPN-achtervoegsel – set lokale kenmerk mailNickName

On-Premises user-object:
- mailNickName: us4
- proxyAddresses: {SMTP:us1@contoso.com}
- e-mail:us2@contoso.com
- userPrincipalName:us3@contoso.com

Update op de lokale mailNickName kenmerk met Azure AD-Tenant synchroniseren
- Azure AD MailNickName kenmerk bijwerken met lokale mailNickName kenmerk.
- Omdat er geen update voor het kenmerk lokale userPrincipalName is, is er geen wijziging aan het kenmerk UserPrincipalName van Azure AD.

Azure AD-Tenant gebruikersobject:
- MailNickName: us4
- userPrincipalName:us1@contoso.onmicrosoft.com

### <a name="scenario-3-non-verified-upn-suffix--update-on-premises-userprincipalname-attribute"></a>Scenario 3: Niet-geverifieerd UPN-achtervoegsel: update lokale kenmerk userPrincipalName

On-Premises user-object:
- mailNickName: us4
- proxyAddresses: {SMTP:us1@contoso.com}
- e-mail:us2@contoso.com
- userPrincipalName:us5@contoso.com

Update op de lokale userPrincipalName kenmerk met Azure AD-Tenant synchroniseren
- Update op de lokale userPrincipalName kenmerk activeert herberekening van MOERA en Azure AD UserPrincipalName kenmerk.
- MOERA ingesteld op &lt;MailNickName&gt;&#64;&lt; eerste domein&gt;.
- Azure AD UserPrincipalName kenmerk instellen op MOERA.

Azure AD-Tenant gebruikersobject:
- MailNickName: us4
- userPrincipalName:us4@contoso.onmicrosoft.com

### <a name="scenario-4-non-verified-upn-suffix--update-primary-smtp-address-and-on-premises-mail-attribute"></a>Scenario 4: Niet-geverifieerd UPN-achtervoegsel: het primaire SMTP-adres bijwerken en on-premises e-kenmerk

On-Premises user-object:
- mailNickName: us4
- proxyAddresses: {SMTP:us6@contoso.com}
- e-mail:us7@contoso.com
- userPrincipalName:us5@contoso.com

Update op de lokale e-mailkenmerk en het primaire SMTP-adres met Azure AD-Tenant synchroniseren
- Na de initiële synchronisatie van het gebruikersobject-updates naar de on-premises e-kenmerk en het primaire SMTP-adres heeft geen invloed op de Azure AD MailNickName, noch UserPrincipalName kenmerk.

Azure AD-Tenant gebruikersobject:
- MailNickName: us4
- userPrincipalName:us4@contoso.onmicrosoft.com

### <a name="scenario-5-verified-upn-suffix--update-on-premises-userprincipalname-attribute-suffix"></a>Scenario 5: Geverifieerde UPN-achtervoegsel: update lokale userPrincipalName kenmerk achtervoegsel

On-Premises user-object:
- mailNickName: us4
- proxyAddresses: {SMTP:us6@contoso.com}
- e-mail:us7@contoso.com
- serPrincipalName:us5@verified.contoso.com

Update op de lokale userPrincipalName kenmerk met de Azure AD-Tenant synchroniseren
- Op de lokale userPrincipalName kenmerk triggers herberekening van Azure AD UserPrincipalName kenmerk bijwerken.
- Ingesteld op de lokale userPrincipalName kenmerk kenmerk UserPrincipalName van Azure AD als de UPN-achtervoegsel is geverifieerd met de Azure AD-Tenant.

Azure AD-Tenant gebruikersobject:
- MailNickName: us4     
- userPrincipalName:us5@verified.contoso.com

## <a name="next-steps"></a>Volgende stappen
- [Uw on-premises directory's integreren met Azure Active Directory](active-directory-aadconnect.md)
- [Aangepaste installatie van Azure AD Connect](active-directory-aadconnect-get-started-custom.md)

---
title: 'Azure AD Connect: Gebruiker aanmelden | Microsoft Docs'
description: Azure AD Connect gebruiker aanmelden voor aangepaste instellingen.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 547b118e-7282-4c7f-be87-c035561001df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: 4670ec3cacd8d69a4ed59aa2bbbeb2e5c893f173
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-user-sign-in-options"></a>Azure AD Connect gebruiker aanmeldingsopties
Verbinden met Azure Active Directory (Azure AD) kan uw gebruikers zich aanmelden bij zowel cloud als on-premises resources met dezelfde wachtwoorden. In dit artikel beschrijft de belangrijkste concepten voor elk identiteitsmodel bij het kiezen van de identiteit die u gebruiken wilt voor aanmelding bij Azure AD.

Als u al bekend met het Azure AD identity-model bent en voor meer informatie over een specifieke methode, raadpleegt u de desbetreffende koppeling:

* [Synchronisatie van wachtwoordhash](#password-synchronization) met [naadloze eenmalige aanmelding (SSO)](active-directory-aadconnect-sso.md)
* [Pass through-verificatie](active-directory-aadconnect-pass-through-authentication.md) met [naadloze eenmalige aanmelding (SSO)](active-directory-aadconnect-sso.md)
* [Federatieve eenmalige aanmelding (met Active Directory Federation Services (AD FS))](#federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2)

> [!NOTE] 
> Het is belangrijk te weten dat door de Federatie voor Azure AD configureren, u vertrouwensrelatie tussen uw Azure AD-tenant en uw federatieve domeinen een. Met deze federatieve vertrouwensrelatie-domein hebben gebruikers toegang tot Azure AD cloudbronnen binnen de tenant.  
>

## <a name="choosing-the-user-sign-in-method-for-your-organization"></a>De gebruiker aanmelden methode voor uw organisatie te kiezen
Voor de meeste organisaties die willen inschakelen gebruiker aanmelden bij Office 365, SaaS-toepassingen en andere Azure AD gebaseerde-resources, wordt aangeraden de standaardoptie wachtwoord-hash synchronisatie. Sommige organisaties hebben echter een bepaalde reden is dat ze gebruikmaken van deze optie niet. Ze kunnen beide een federatieve aanmelding opties, zoals AD FS of Pass through-verificatie kiezen. U kunt de volgende tabel kunt u de juiste keuze.

Ik wil | PBS met eenmalige aanmelding| PTA met eenmalige aanmelding| AD FS |
 --- | --- | --- | --- |
Nieuwe gebruikers, contactpersonen en groepsaccounts in Active Directory naar de cloud lokale automatisch gesynchroniseerd.|x|x|x|
Instellen van mijn tenants voor Office 365 hybride scenario's.|x|x|x|
Mijn gebruikers aanmelden en toegang tot cloudservices met hun on-premises wachtwoord inschakelen.|x|x|x|
Eenmalige aanmelding implementeren met behulp van zakelijke referenties.|x|x|x|
Zorg ervoor dat er geen wachtwoorden worden opgeslagen in de cloud.||x *|x|
Oplossingen voor on-premises meervoudige verificatie inschakelen.|||x|

* Via een lichtgewicht agent.

### <a name="password-hash-synchronization"></a>Synchronisatie van wachtwoordhash
Met synchronisatie van wachtwoordhash, zijn-hashes van gebruikerswachtwoorden gesynchroniseerd van lokale Active Directory naar Azure AD. Wanneer wachtwoorden worden gewijzigd of opnieuw instellen van lokale, het nieuwe wachtwoord-hashes zijn gesynchroniseerd naar Azure AD onmiddellijk zodat uw gebruikers altijd hetzelfde wachtwoord voor cloud-bronnen en lokale bronnen gebruiken kunnen. De wachtwoorden worden nooit verzonden naar Azure AD of opgeslagen in Azure AD in ongecodeerde tekst. U kunt synchronisatie van wachtwoordhash samen met terugschrijven van wachtwoord gebruiken om in te schakelen selfservice wachtwoordherstel in Azure AD.

Bovendien kunt u inschakelen [naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md) voor gebruikers op domein-machines op het bedrijfsnetwerk. Met eenmalige aanmelding moeten ingeschakelde gebruikers alleen een gebruikersnaam zodat ze veilig toegang tot cloud-bronnen invoeren.

![Synchronisatie van wachtwoordhash](./media/active-directory-aadconnect-user-signin/passwordhash.png)

Zie voor meer informatie de [synchronisatie van wachtwoordhash](active-directory-aadconnectsync-implement-password-synchronization.md) artikel.

### <a name="pass-through-authentication"></a>Pass Through-verificatie
Met Pass through-verificatie, is het wachtwoord van de gebruiker gevalideerd met de lokale Active Directory-domeincontroller. Het wachtwoord hoeft niet aanwezig zijn in Azure AD in een formulier. Hierdoor lokale beleidsregels, zoals de beperkingen van het uur aanmelden, moet worden geëvalueerd tijdens de verificatie naar de cloud-services.

Pass through-verificatie gebruikt een eenvoudige-agent op een Windows Server 2012 R2 domein machine in de on-premises omgeving. Deze agent luistert naar aanvragen voor wachtwoord-validatie. Poorten voor inkomend verkeer te worden gebruikt met het Internet nodig niet is.

Bovendien kunt u ook eenmalige aanmelding voor gebruikers in domein-machines op het bedrijfsnetwerk. Met eenmalige aanmelding moeten ingeschakelde gebruikers alleen een gebruikersnaam zodat ze veilig toegang tot cloud-bronnen invoeren.
![Pass through-verificatie](./media/active-directory-aadconnect-user-signin/pta.png)

Zie voor meer informatie:
- [Pass through-verificatie](active-directory-aadconnect-pass-through-authentication.md)
- [Eenmalige aanmelding](active-directory-aadconnect-sso.md)

### <a name="federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2"></a>Federatieve die gebruikmaakt van een nieuwe of bestaande farm met AD FS in Windows Server 2012 R2
Met federatieve aanmelden, zich uw gebruikers aanmelden bij Azure AD gebaseerde services met hun on-premises wachtwoorden. Terwijl ze op het bedrijfsnetwerk, zijn ze zelfs om hun wachtwoord invoeren. U kunt een nieuwe of bestaande farm met AD FS in Windows Server 2012 R2 implementeren via de optie Federatie met AD FS. Als u ervoor kiest om op te geven van een bestaande farm, configureert Azure AD Connect de vertrouwensrelatie tussen uw farm en Azure AD, zodat uw gebruikers zich kunnen aanmelden.

<center>![Federatie met AD FS in Windows Server 2012 R2](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="deploy-federation-with-ad-fs-in-windows-server-2012-r2"></a>Federatie met AD FS in Windows Server 2012 R2 implementeren

Als u een nieuwe farm implementeert, moet u het volgende nodig:

* Een Windows Server 2012 R2-server voor de federation-server.
* Een Windows Server 2012 R2-server voor de Webtoepassingsproxy.
* Een pfx-bestand met een SSL-certificaat voor de naam van uw beoogde federation-service. Bijvoorbeeld: fs.contoso.com.

Als u een nieuwe farm implementeert of via een bestaande farm, u moet:

* Lokale beheerdersreferenties op uw federatieservers.
* Lokale beheerdersreferenties op de werkgroepservers van een (geen lid van een domein) die u van plan bent voor de implementatie van de functie Web Application Proxy.
* De computer waarop u de wizard uitvoeren op kunnen verbinding maken met andere computers die u wilt installeren van AD FS of Web Application Proxy op met behulp van Windows Remote Management.

Zie voor meer informatie [eenmalige aanmelding met AD FS configureren](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs).

#### <a name="sign-in-by-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Aanmelden met een eerdere versie van AD FS of een oplossing van derden
Als u al cloud aanmelden met behulp van een eerdere versie van AD FS (zoals AD FS 2.0) of een derde partij federatieprovider hebt geconfigureerd, kunt u overslaan van de gebruiker aanmelden configuratie met Azure AD Connect. Hierdoor kunt u de laatste synchronisatie en andere mogelijkheden van Azure AD Connect downloaden terwijl u ook uw bestaande oplossing voor aanmelden.

Zie voor meer informatie de [Azure AD van derden federatiecompatibiliteitslijst](active-directory-aadconnect-federation-compatibility.md).


## <a name="user-sign-in-and-user-principal-name"></a>Gebruiker aanmelden en UPN
### <a name="understanding-user-principal-name"></a>Understanding UPN-naam
In Active Directory is het achtervoegsel van de standaard user principal name (UPN) de DNS-naam van het domein waar het gebruikersaccount is gemaakt. In de meeste gevallen is dit de domeinnaam die geregistreerd als het domein van de onderneming op het Internet. U kunt echter meer UPN-achtervoegsels toevoegen met behulp van Active Directory: domeinen en vertrouwensrelaties.

De UPN van de gebruiker heeft de indeling username@domain. Bijvoorbeeld, een Active Directory-domein 'contoso.com' met de naam van een gebruiker met de naam John wellicht de UPN 'john@contoso.com'. De UPN van de gebruiker is gebaseerd op RFC 822. Hoewel het UPN- en e-mail dezelfde indeling delen, de waarde van de UPN voor een gebruiker kunnen wel of niet hetzelfde zijn als het e-mailadres van de gebruiker.

### <a name="user-principal-name-in-azure-ad"></a>UPN-naam in Azure AD
De Azure AD Connect-wizard gebruikt het kenmerk userPrincipalName of kunt u opgeven dat het kenmerk (in een aangepaste installatie) on-premises worden gebruikt als de user principal name in Azure AD. Dit is de waarde die wordt gebruikt voor aanmelding bij Azure AD. Als de waarde van het kenmerk userPrincipalName komt niet met een geverifieerde domeinnaam in Azure AD overeen, Azure AD wordt deze met een standaardwaarde. onmicrosoft.com-waarde.

Elke map in Azure Active Directory wordt geleverd met een ingebouwde domeinnaam de indeling contoso.onmicrosoft.com, waarmee u aan de slag met Azure of andere Microsoft-services. U kunt verbeteren en de aanmeldingservaring vereenvoudigen met behulp van aangepaste domeinen. Zie voor informatie over aangepaste domeinnamen in Azure AD en het controleren van een domein, [uw aangepaste domeinnaam toevoegen aan Azure Active Directory](../add-custom-domain.md#add-the-custom-domain-name-to-your-directory).

## <a name="azure-ad-sign-in-configuration"></a>Aanmeldconfiguratie Azure AD
### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD-in-configuratie met Azure AD Connect
De Azure AD-aanmeldingservaring aanpast, is afhankelijk van of Azure AD kan overeenkomen met de UPN-achtervoegsel van een gebruiker die wordt gesynchroniseerd met een van de aangepaste domeinen die zijn geverifieerd in de Azure AD-directory. Azure AD Connect bevat help tijdens het configureren van Azure AD-aanmelden-instellingen, zodat de aanmeldingspagina de gebruikerservaring in de cloud is vergelijkbaar met de ervaring van de lokale.

Azure AD Connect bevat de UPN-achtervoegsels die zijn gedefinieerd voor de domeinen en er wordt geprobeerd deze overeenkomen met een aangepast domein in Azure AD. Vervolgens kunt u met de juiste actie die moet worden ondernomen.
De aanmeldingspagina van Azure AD geeft een lijst van de UPN-achtervoegsels die zijn gedefinieerd voor de lokale Active Directory en de bijbehorende status tegen elk achtervoegsel wordt weergegeven. De statuswaarden zijn:

| Status | Beschrijving | Actie vereist |
|:--- |:--- |:--- |
| Geverifieerd |Azure AD Connect gevonden dat een overeenkomende domein in Azure AD geverifieerd. Alle gebruikers voor dit domein kunnen aanmelden met hun on-premises referenties. |Er is geen actie vereist. |
| Niet geverifieerd |Azure AD Connect een overeenkomende aangepast domein in Azure AD gevonden, maar het is niet geverifieerd. Het UPN-achtervoegsel van de gebruikers van dit domein wordt gewijzigd in de standaard. het achtervoegsel onmicrosoft.com na synchronisatie als het domein is niet geverifieerd. | [Controleer of het aangepaste domein in Azure AD.](../add-custom-domain.md#verify-the-custom-domain-name-in-azure-ad) |
| Niet toegevoegd |Azure AD Connect niet zoeken naar een aangepast domein kwam met het UPN-achtervoegsel overeen. Het UPN-achtervoegsel van de gebruikers van dit domein wordt gewijzigd op de standaardwaarde. achtervoegsel onmicrosoft.com als het domein is niet toegevoegd en geverifieerd in Azure. | [Toevoegen en controleer of u een aangepast domein dat overeenkomt met het UPN-achtervoegsel.](../add-custom-domain.md) |

De aanmeldingspagina van Azure AD geeft een lijst van de UPN-achtervoegsels die zijn gedefinieerd voor de lokale Active Directory en de bijbehorende aangepaste domein in Azure AD met de huidige verificatiestatus. In een aangepaste installatie kunt u het kenmerk voor de user principal name nu selecteren op de **Azure AD-aanmeldingspagina** pagina.

![Azure AD-aanmeldingspagina](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

U kunt klikken op de knop Vernieuwen om de laatste status van de aangepaste domeinen opnieuw ophalen uit Azure AD.

### <a name="selecting-the-attribute-for-the-user-principal-name-in-azure-ad"></a>Het kenmerk voor de user principal name selecteren in Azure AD
Het kenmerk userPrincipalName is het kenmerk dat gebruikers gebruiken wanneer ze zich bij Azure AD aanmelden en Office 365. U moet controleren of de domeinen die worden gebruikt in Azure AD voordat de gebruikers worden gesynchroniseerd (ook wel bekend als UPN-achtervoegsels).

Het is raadzaam dat u het standaard kenmerk userPrincipalName behouden. Als dit kenmerk nonroutable is en kan niet worden geverifieerd, is het mogelijk een ander kenmerk (bijvoorbeeld e-mail) selecteren als het kenmerk met de aanmeldings-ID. Dit staat bekend als de alternatieve-ID. De waarde van de alternatieve ID-kenmerk moet voldoen aan de standaard RFC 822. U kunt een alternatieve ID gebruiken met wachtwoord SSO en federatieve SSO als oplossing voor aanmelden.

> [!NOTE]
> Met behulp van een alternatieve ID is niet compatibel met alle Office 365-werkbelastingen. Zie voor meer informatie [Configuring Alternate Login ID](https://technet.microsoft.com/library/dn659436.aspx).
>
>

#### <a name="different-custom-domain-states-and-their-effect-on-the-azure-sign-in-experience"></a>Ander aangepast domein statussen en hun effect op de Azure-aanmeldingservaring aanpast
Het is belangrijk om te begrijpen van de relatie tussen de statussen van het aangepaste domein in uw Azure AD-directory en de UPN-achtervoegsels die zijn gedefinieerd op de lokale. We gaan via de verschillende mogelijke Azure aanmelden ervaringen wanneer u synchronisatie instellen bent met behulp van Azure AD Connect.

Voor de volgende informatie, gaan we ervan uit dat we betrokken zijn bij het UPN-achtervoegsel contoso.com, die wordt gebruikt in de on-premises directory als onderdeel van de UPN--bijvoorbeeld user@contoso.com.

###### <a name="express-settingspassword-hash-synchronization"></a>Express-instellingen/synchronisatie van wachtwoordhash
| Status | Effect op de gebruikerservaring Azure aanmelden |
|:---:|:--- |
| Niet toegevoegd |In dit geval is geen aangepast domein voor contoso.com toegevoegd aan de Azure AD-directory. Gebruikers die beschikken over UPN on-premises met het achtervoegsel @contoso.com niet mogelijk gebruik van de lokale UPN aan te melden bij Azure. Ze hebt in plaats daarvan gebruikt u een nieuwe UPN die opgegeven voor deze door Azure AD door het achtervoegsel voor de standaard Azure AD-directory toe te voegen. Bijvoorbeeld, als u gebruikers de Azure AD-directory azurecontoso.onmicrosoft.com, kan de lokale gebruiker bent synchroniseren user@contoso.com een UPN van krijgt user@azurecontoso.onmicrosoft.com. |
| Niet geverifieerd |In dit geval hebben we een aangepast domein contoso.com die in de Azure AD-directory wordt toegevoegd. Het echter nog niet geverifieerd. Als u verder gaat met het synchroniseren van gebruikers zonder te controleren van het domein, wordt de gebruikers een nieuwe UPN door Azure AD toegewezen, net als in het scenario 'Niet toegevoegd'. |
| Geverifieerd |In dit geval hebben we een aangepast domein contoso.com die al is toegevoegd en geverifieerd in Azure AD voor het UPN-achtervoegsel. Gebruikers gebruiken hun lokale UPN-naam, bijvoorbeeld kan worden user@contoso.com, aan te melden bij Azure nadat ze zijn gesynchroniseerd naar Azure AD. |

###### <a name="ad-fs-federation"></a>AD FS-federatie
U kunt een federatieve maken met de standaardwaarde. onmicrosoft.com-domein in Azure AD of een niet-geverifieerde aangepaste in Azure AD. Wanneer u de Azure AD Connect-wizard uitvoert als u een niet-geverifieerde domein voor het maken van een federatie met selecteert, klikt u vervolgens vraagt Azure AD Connect u met de benodigde records worden gemaakt waar uw DNS voor het domein wordt gehost. Zie voor meer informatie [Controleer of de Azure AD-domein voor Federatie is geselecteerd](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Als u de gebruikersoptie aanmelden geselecteerd **Federatie met AD FS**, en vervolgens moet u een aangepast domein om door te gaan maken van een federatieve in Azure AD hebben. Voor onze bespreking betekent dit dat er moet een aangepast domein contoso.com toegevoegd in de Azure AD-directory.

| Status | Effect op de gebruiker Azure aanmeldingservaring |
|:---:|:--- |
| Niet toegevoegd |Azure AD Connect gevonden in dit geval een overeenkomende aangepast domein voor het UPN-achtervoegsel contoso.com in de Azure AD-directory. U moet een aangepast domein contoso.com toevoegen als u gebruikers moet zich aanmelden met behulp van AD FS met de UPN van de lokale (zoals user@contoso.com). |
| Niet geverifieerd |In dit geval Azure AD Connect vraagt u om met de juiste informatie over hoe u uw domein in een later stadium kunt controleren. |
| Geverifieerd |In dit geval gaat u door met de configuratie zonder verdere actie. |

## <a name="changing-the-user-sign-in-method"></a>De gebruiker aanmelden methode wijzigen
U kunt de gebruiker aanmelden methode wijzigen van federatieve, synchronisatie van wachtwoordhash of Pass through-verificatie met behulp van de taken die beschikbaar in Azure AD Connect na de initiële configuratie van Azure AD Connect met de wizard zijn. De Azure AD Connect-wizard opnieuw uitvoeren en ziet u een lijst met taken die u kunt uitvoeren. Selecteer **wijzigen gebruikersaanmelding** uit de lijst met taken.

![Gebruikersaanmelding wijzigen](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Op de volgende pagina kunt u gevraagd de referenties opgeven voor Azure AD.

![Verbinding maken met Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Op de **gebruikersaanmelding** pagina, selecteert u de gewenste gebruiker aanmelden.

![Verbinding maken met Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2a.png)

> [!NOTE]
> Als u alleen een tijdelijke switch aan synchronisatie van wachtwoordhash brengt, selecteert u de **gebruikersaccounts niet worden geconverteerd** selectievakje. Elke gebruiker federatieve niet controleren of de optie geconverteerd en het kan enkele uren duren.
>
>

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md).
- Meer informatie over [ontwerpconcepten Azure AD Connect](active-directory-aadconnect-design-concepts.md).

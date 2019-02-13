---
title: 'Azure AD Connect: Aanmelden van gebruikers | Microsoft Docs'
description: Azure AD Connect aanmelden van gebruikers voor aangepaste instellingen.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: 547b118e-7282-4c7f-be87-c035561001df
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/31/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: a3b503c7f0693a90d438fcec3ecae335fd349b3d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56187998"
---
# <a name="azure-ad-connect-user-sign-in-options"></a>Azure AD Connect-aanmelden-gebruikersopties
Azure Active Directory (Azure AD) Connect kan uw gebruikers zich aanmelden bij zowel cloud en on-premises resources met behulp van dezelfde wachtwoorden. Dit artikel beschrijft de belangrijkste concepten voor elk identiteitsmodel aan bij het kiezen van de identiteit die u gebruiken wilt voor aanmelding bij Azure AD.

Als u al bekend met het Azure AD identity-model bent en meer informatie over een specifieke methode wilt, raadpleegt u de desbetreffende koppeling:

* [Wachtwoord-hashsynchronisatie](#password-hash-synchronization) met [naadloze eenmalige aanmelding (SSO)](how-to-connect-sso.md)
* [Pass through-verificatie](how-to-connect-pta.md) met [naadloze eenmalige aanmelding (SSO)](how-to-connect-sso.md)
* [Federatieve eenmalige aanmelding (met Active Directory Federatieservices (AD FS))](#federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2)
* [Federatie met PingFederate](#federation-with-pingfederate)

> [!NOTE] 
> Het is belangrijk te onthouden dat door het configureren van Federatie voor Azure AD u vertrouwensrelatie hebt tussen uw Azure AD-tenant en uw federatieve domeinen. Gebruikers hebben toegang tot Azure AD-cloud-resources binnen de tenant met deze vertrouwensrelatie federatief domein.  
>

## <a name="choosing-the-user-sign-in-method-for-your-organization"></a>De gebruiker aanmelden-methode voor uw organisatie kiezen
De eerste beslissing van de implementatie van Azure AD Connect is te kiezen welke verificatiemethode die uw gebruikers wordt gebruikt om te melden. Het is belangrijk om ervoor te zorgen dat u de juiste methode kiezen die voldoet aan de beveiliging en geavanceerde eisen van uw organisatie. Verificatie is belangrijk, omdat deze gebruikersidentiteiten te krijgen tot apps en gegevens in de cloud wordt gevalideerd. Als u de juiste verificatiemethode, moet u rekening houden met de tijd, de bestaande infrastructuur, de complexiteit en de kosten van de implementatie van uw keuze. Deze factoren zijn voor elke organisatie en kunnen na verloop van tijd worden gewijzigd.

Azure AD ondersteunt de volgende verificatiemethoden: 

* **Verificatie in de cloud** : wanneer u ervoor deze verificatiemethode Azure AD het verificatieproces worden verwerkt kiest voor de gebruiker zich sign-in. Met cloud-verificatie kunt u kiezen uit twee opties: 
   * **Wachtwoord-hashsynchronisatie (WHS)** -synchronisatie van Wachtwoordhashes kunnen gebruikers dezelfde gebruikersnaam en hetzelfde wachtwoord dat ze on-premises gebruiken gebruiken zonder dat u hoeft geen aanvullende infrastructuur naast Azure AD Connect te implementeren. 
   * **Pass through-verificatie (PTA)** -deze optie is vergelijkbaar met de synchronisatie van wachtwoordhashes, maar biedt een eenvoudig wachtwoordvalidatie met behulp van on-premises softwareagents voor organisaties met een sterk beleid voor beveiliging en naleving.
* **Federatieve verificatie** : als u ervoor deze verificatiemethode Azure AD wordt afleveren van het verificatieproces uit naar een afzonderlijke vertrouwde verificatiesysteem, kiest bijvoorbeeld als AD FS of een derde partij federation-systeem, voor het valideren van de gebruiker van aanmelding. 

Voor de meeste organisaties die willen inschakelen gebruiker aanmelden bij Office 365, SaaS-toepassingen en andere Azure AD gebaseerde-resources, raden wij de standaardoptie wachtwoord-hash-synchronisatie.
 
Zie voor gedetailleerde informatie over het kiezen van een verificatiemethode [kiest u de juiste verificatiemethode voor uw Azure Active Directory-oplossing voor hybride identiteit](../../security/azure-ad-choose-authn.md)

### <a name="password-hash-synchronization"></a>Synchronisatie van wachtwoord-hashes
-Hashes van wachtwoorden van gebruikers zijn met wachtwoord-hashsynchronisatie van on-premises Active Directory gesynchroniseerd met Azure AD. Wanneer wachtwoorden worden gewijzigd of opnieuw instellen van on-premises, het nieuwe wachtwoord-hashes zijn gesynchroniseerd met Azure AD onmiddellijk zodat uw gebruikers altijd hetzelfde wachtwoord voor cloudresources en on-premises bronnen gebruiken kunnen. De wachtwoorden worden nooit verzonden naar Azure AD of die zijn opgeslagen in Azure AD in niet-versleutelde tekst. Om in te schakelen van self-service voor wachtwoord opnieuw instellen in Azure AD kunt u samen met het terugschrijven van wachtwoord-hashsynchronisatie.

Bovendien kunt u inschakelen [naadloze eenmalige aanmelding](how-to-connect-sso.md) voor gebruikers op een domein machines die zich op het bedrijfsnetwerk bevinden. Met eenmalige aanmelding hoeft ingeschakelde gebruikers in te voeren van een gebruikersnaam om hen te helpen veilig toegang tot cloud-bronnen.

![Synchronisatie van wachtwoord-hashes](./media/plan-connect-user-signin/passwordhash.png)

Zie voor meer informatie de [wachtwoord-hashsynchronisatie](how-to-connect-password-hash-synchronization.md) artikel.

### <a name="pass-through-authentication"></a>Pass-through-verificatie
Wachtwoord van de gebruiker is gevalideerd met Pass through-verificatie, op basis van de on-premises Active Directory-domeincontroller. Het wachtwoord hoeft niet aanwezig zijn in Azure AD in welke vorm. Hiermee wordt voor on-premises-beleid, zoals aanmelding uur beperkingen, moet worden geëvalueerd tijdens de verificatie naar de cloud services.

Pass through-verificatie maakt gebruik van een eenvoudige agent op een Windows Server 2012 R2 domein machine in de on-premises omgeving. Deze agent luistert naar aanvragen voor wachtwoord-validatie. Er is inkomende poorten open zijn voor het Internet geen vereist.

Bovendien kunt u ook eenmalige aanmelding voor gebruikers op een domein machines die zich op het bedrijfsnetwerk bevinden. Met eenmalige aanmelding hoeft ingeschakelde gebruikers in te voeren van een gebruikersnaam om hen te helpen veilig toegang tot cloud-bronnen.
![Pass-through-verificatie](./media/plan-connect-user-signin/pta.png)

Zie voor meer informatie:
- [Pass-through-verificatie](how-to-connect-pta.md)
- [Eenmalige aanmelding](how-to-connect-sso.md)

### <a name="federation-that-uses-a-new-or-existing-farm-with-ad-fs-in-windows-server-2012-r2"></a>Federation die gebruikmaakt van een nieuwe of bestaande-farm met AD FS in Windows Server 2012 R2
Uw gebruikers kunnen met federatieve aanmelding zich aanmelden met Azure AD gebaseerde services met hun on-premises wachtwoorden. Terwijl ze op het bedrijfsnetwerk bevinden, hoeft deze zelfs te voeren hun wachtwoord. Met de optie Federatie met AD FS, kunt u een nieuwe of bestaande farm met AD FS in Windows Server 2012 R2 implementeren. Als u ervoor kiest om op te geven van een bestaande farm, configureert Azure AD Connect het vertrouwen tussen uw farm en Azure AD, zodat uw gebruikers kunnen zich aanmelden.

<center>![Federatie met AD FS in Windows Server 2012 R2](./media/plan-connect-user-signin/federatedsignin.png)</center>

#### <a name="deploy-federation-with-ad-fs-in-windows-server-2012-r2"></a>Federatie met AD FS in Windows Server 2012 R2 implementeren

Als u een nieuwe farm implementeert, hebt u het volgende nodig:

* Een Windows Server 2012 R2-server voor de federation-server.
* Een Windows Server 2012 R2-server voor het Web Application Proxy.
* Een pfx-bestand met een SSL-certificaat voor de naam van uw beoogde federation-service. Bijvoorbeeld: fs.contoso.com.

Als u een nieuwe farm implementeren of een bestaande farm gebruikt, hebt u het volgende nodig:

* Lokale beheerdersreferenties op uw federatieservers.
* Lokale beheerdersreferenties op de werkgroepservers van een (niet-domein) die u van plan bent te implementeren van de Web Application Proxy-rol op.
* De computer waarop u de wizard uitvoeren op kunnen verbinding maken met een andere virtuele machines die u wilt installeren van AD FS of Web Application Proxy op met behulp van Windows Remote Management.

Zie voor meer informatie, [configureren van eenmalige aanmelding met AD FS](how-to-connect-install-custom.md#configuring-federation-with-ad-fs).

### <a name="federation-with-pingfederate"></a>Federatie met PingFederate
Uw gebruikers kunnen met federatieve aanmelding zich aanmelden met Azure AD gebaseerde services met hun on-premises wachtwoorden. Terwijl ze op het bedrijfsnetwerk bevinden, hoeft deze zelfs te voeren hun wachtwoord.

Zie voor meer informatie over het configureren van PingFederate voor gebruik met Azure Active Directory [PingFederate-integratie met Azure Active Directory en Office 365](https://www.pingidentity.com/AzureADConnect)

Zie voor meer informatie over het instellen van Azure AD Connect met PingFederate [aangepaste installatie van Azure AD Connect](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)

#### <a name="sign-in-by-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Meld u aan met behulp van een eerdere versie van AD FS of een oplossing van derden
Als u al cloudaanmelding met behulp van een eerdere versie van AD FS (zoals AD FS 2.0) of een federatieprovider van derden hebt geconfigureerd, kunt u kiezen om over te slaan van de gebruiker aanmelden configuratie via Azure AD Connect. Hierdoor kunt u de meest recente synchronisatie en andere functies van Azure AD Connect ophalen tijdens het nog steeds gebruik van uw bestaande oplossing voor aanmelden.

Zie voor meer informatie de [compatibiliteitslijst voor Azure AD-Federatie van derden](how-to-connect-fed-compatibility.md).


## <a name="user-sign-in-and-user-principal-name"></a>Aanmelden van gebruikers en UPN-naam
### <a name="understanding-user-principal-name"></a>De naam Understanding user principal name
Het achtervoegsel van de standaard user principal name (UPN) is in Active Directory, de DNS-naam van het domein waar het gebruikersaccount dat is gemaakt. In de meeste gevallen is dit de domeinnaam die geregistreerd als het domein van de onderneming op het Internet. U kunt echter meer UPN-achtervoegsels toevoegen met behulp van Active Directory: domeinen en vertrouwensrelaties.

De UPN van de gebruiker heeft de indeling username@domain. Bijvoorbeeld, een Active Directory-domein met de naam 'contoso.com', een gebruiker met de naam John wellicht de UPN 'john@contoso.com'. De UPN van de gebruiker is gebaseerd op RFC 822. Hoewel de UPN en de e-mailbericht dezelfde indeling delen, wordt de waarde van de UPN voor een gebruiker kan of mogelijk niet hetzelfde als het e-mailadres van de gebruiker.

### <a name="user-principal-name-in-azure-ad"></a>UPN-naam in Azure AD
De Azure AD Connect-wizard gebruikt het kenmerk userPrincipalName of kunt u opgeven dat het kenmerk (in een aangepaste installatie) van on-premises worden gebruikt als de user principal name in Azure AD. Dit is de waarde die wordt gebruikt voor aanmelding bij Azure AD. Als de waarde van het kenmerk userPrincipalName niet overeenkomt met een geverifieerd domein in Azure AD, klikt u vervolgens Azure AD vervangen door een standaard. onmicrosoft.com-waarde.

Alle directory's in Azure Active Directory wordt geleverd met een ingebouwde domeinnaam, met de indeling contoso.onmicrosoft.com, waarmee u aan de slag met Azure of andere Microsoft-services. U kunt verbeteren en vereenvoudigen van de aanmeldingservaring met behulp van aangepaste domeinen. Zie voor meer informatie over aangepaste domeinnamen in Azure AD en het controleren van een domein [uw aangepaste domeinnaam toevoegen aan Azure Active Directory](../fundamentals/add-custom-domain.md).

## <a name="azure-ad-sign-in-configuration"></a>Aanmeldconfiguratie Azure AD
### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD-aanmelden-configuratie met Azure AD Connect
De Azure Active Directory-aanmeldingservaring, is afhankelijk van of in Azure AD kan overeenkomen met de UPN-achtervoegsel van een gebruiker die gesynchroniseerd wordt met een van de aangepaste domeinen die in de Azure AD-directory zijn geverifieerd. Azure AD Connect biedt hulp tijdens het configureren van Azure AD-aanmelding-instellingen, zodat de gebruiker aanmelden ervaring in de cloud vergelijkbaar met de on-premises-ervaring is.

Azure AD Connect bevat de UPN-achtervoegsels die zijn gedefinieerd voor de domeinen en er wordt geprobeerd deze overeenkomen met een aangepast domein in Azure AD. Vervolgens kunt u met de juiste actie die moet worden ondernomen.
De aanmeldingspagina van Azure AD bevat de UPN-achtervoegsels die zijn gedefinieerd voor on-premises Active Directory en de bijbehorende status voor elk achtervoegsel wordt weergegeven. De waarden van de status kunnen zijn dat een van de volgende:

| Status | Description | Actie vereist |
|:--- |:--- |:--- |
| Geverifieerd |Azure AD Connect gevonden dat een overeenkomende geverifieerd domein in Azure AD. Alle gebruikers voor dit domein kunnen zich aanmelden met behulp van hun on-premises referenties. |U hoeft geen actie te ondernemen. |
| Niet gecontroleerd |Azure AD Connect een overeenkomende aangepaste domein in Azure AD worden gevonden, maar deze is niet geverifieerd. Het UPN-achtervoegsel van de gebruikers van dit domein wordt gewijzigd in de standaard. het achtervoegsel onmicrosoft.com na synchronisatie als het domein is niet geverifieerd. | [Controleer of het aangepaste domein in Azure AD.](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) |
| Niet toegevoegd |Azure AD Connect niet een aangepast domein dat kwam met het UPN-achtervoegsel overeen niet vinden. Het UPN-achtervoegsel van de gebruikers van dit domein wordt gewijzigd in de standaardinstelling. achtervoegsel onmicrosoft.com als het domein is niet toegevoegd en gecontroleerd in Azure. | [Toevoegen en controleer of een aangepast domein die overeenkomt met het UPN-achtervoegsel.](../fundamentals/add-custom-domain.md) |

De aanmeldingspagina van Azure AD bevat de UPN-achtervoegsels die zijn gedefinieerd voor on-premises Active Directory en het overeenkomende aangepaste domein in Azure AD met de huidige verificatiestatus. In een aangepaste installatie, kunt u nu het kenmerk voor de user principal name selecteren op de **aanmelding bij Azure AD** pagina.

![Azure AD-aanmeldingspagina](./media/plan-connect-user-signin/custom_azure_sign_in.png)

U kunt de vernieuwknop om de meest recente status van de aangepaste domeinen opnieuw ophalen uit Azure AD.

### <a name="selecting-the-attribute-for-the-user-principal-name-in-azure-ad"></a>Het kenmerk voor de user principal name selecteren in Azure AD
Het kenmerk userPrincipalName is het kenmerk waarmee gebruikers wanneer ze zich aanmelden bij Azure AD en Office 365. U moet controleren of de domeinen (ook wel bekend als een UPN-achtervoegsels) die worden gebruikt in Azure AD voordat de gebruikers worden gesynchroniseerd.

Het is raadzaam het standaardkenmerk userPrincipalName te houden. Als dit kenmerk nonroutable is en kan niet worden geverifieerd, is het mogelijk om te selecteren van een ander kenmerk (bijvoorbeeld e-mail) als het kenmerk met de aanmeldings-ID. Dit staat bekend als de alternatieve-ID. De waarde van het alternatieve ID-kenmerk moet voldoen aan de standaard RFC 822. U kunt een alternatieve ID gebruiken met zowel de SSO-wachtwoord als de federatieve SSO als de oplossing voor aanmelden.

> [!NOTE]
> Met behulp van een alternatieve ID is niet compatibel met alle Office 365-werkbelastingen. Zie voor meer informatie, [Configuring Alternate Login ID](https://technet.microsoft.com/library/dn659436.aspx).
>
>

#### <a name="different-custom-domain-states-and-their-effect-on-the-azure-sign-in-experience"></a>Ander aangepast domein Staten en hun effect op de Azure-aanmeldingservaring
Het is heel belangrijk om te begrijpen van de relatie tussen de statussen van het aangepaste domein in uw Azure AD-directory en de UPN-achtervoegsels die on-premises gedefinieerd. Hieronder volgen de verschillende mogelijke Azure aanmelden ervaringen tijdens het instellen van synchronisatie met behulp van Azure AD Connect.

Voor de volgende informatie, gaan we ervan uit dat we betrokken zijn bij het UPN-achtervoegsel contoso.com, die wordt gebruikt in de on-premises directory als onderdeel van de UPN: bijvoorbeeld user@contoso.com.

###### <a name="express-settingspassword-hash-synchronization"></a>Express-instellingen/wachtwoord-hashsynchronisatie
| Status | Effect op de ervaring van gebruiker aanmelden bij Azure |
|:---:|:--- |
| Niet toegevoegd |In dit geval is geen aangepast domein voor contoso.com toegevoegd in de Azure AD-directory. Gebruikers die beschikken over UPN on-premises met het achtervoegsel @contoso.com is niet mogelijk met hun on-premises UPN kunt aanmelden bij Azure. Ze hebt in plaats daarvan gebruik van een nieuwe UPN die voor hen wordt geleverd door Azure AD door het achtervoegsel voor de standaard Azure AD-map toe te voegen. Bijvoorbeeld, als u gebruikers aan de Azure AD-directory azurecontoso.onmicrosoft.com en vervolgens de on-premises gebruiker synchroniseert user@contoso.com de UPN-krijgt user@azurecontoso.onmicrosoft.com. |
| Niet gecontroleerd |In dit geval hebben we een aangepaste domein contoso.com die wordt toegevoegd in de Azure AD-directory. Het echter nog niet geverifieerd. Wanneer u verder gaat met het synchroniseren van gebruikers zonder te controleren van het domein, wordt de gebruikers een nieuwe UPN die is toegewezen door Azure AD, net als in het scenario 'Niet toegevoegd'. |
| Geverifieerd |In dit geval hebben we een aangepaste domein contoso.com die al is toegevoegd en gecontroleerd in Azure AD voor het UPN-achtervoegsel. Gebruikers kunnen hun on-premises user principal name, bijvoorbeeld gebruiken user@contoso.comom aan te melden bij Azure, nadat ze zijn gesynchroniseerd met Azure AD. |

###### <a name="ad-fs-federation"></a>AD FS-federatie
U kunt een federatieve maken met de standaardwaarde. onmicrosoft.com-domein in Azure AD of een niet-geverifieerde aangepaste domein in Azure AD. Wanneer u de Azure AD Connect-wizard uitvoert als u een niet-geverifieerd domein te maken van een federatie met selecteert, klikt u vervolgens Azure AD Connect wordt u gevraagd met de benodigde records worden gemaakt waar uw DNS voor het domein wordt gehost. Zie voor meer informatie, [controleren of de Azure AD-domein voor Federatie is geselecteerd](how-to-connect-install-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Als u ervoor hebt gekozen gebruiker aanmelden **Federatie met AD FS**, hebt u een aangepast domein om door te gaan met het maken van een federatieve in Azure AD. Voor onze discussie, betekent dit dat die toegevoegd in de Azure AD-directory het aangepaste domein contoso.com zijn.

| Status | Effect op de gebruiker Azure-aanmeldingservaring |
|:---:|:--- |
| Niet toegevoegd |Azure AD Connect vinden niet in dit geval een overeenkomende aangepaste domein voor de UPN-achtervoegsel contoso.com in de Azure AD-directory. U moet een aangepast domein contoso.com toevoegen als u gebruikers zich aanmelden met behulp van AD FS met hun on-premises UPN (zoals user@contoso.com). |
| Niet gecontroleerd |In dit geval Azure AD Connect wordt u gevraagd met relevante informatie over hoe u uw domein op een later tijdstip controleren kunt. |
| Geverifieerd |In dit geval gaat u door met de configuratie zonder verdere actie. |

## <a name="changing-the-user-sign-in-method"></a>De gebruiker aanmelden methode wijzigen
U kunt de gebruiker aanmelden methode kunt wijzigen van de Federatie, wachtwoord-hashsynchronisatie en pass-through-verificatie met behulp van de taken die beschikbaar in Azure AD Connect na de eerste configuratie van Azure AD Connect met de wizard zijn. De Azure AD Connect-wizard opnieuw uitvoeren en u ziet een lijst met taken die u kunt uitvoeren. Selecteer **aanmelden van gebruikers wijzigen** uit de lijst met taken.

![Gebruikersaanmelding wijzigen](./media/plan-connect-user-signin/changeusersignin.png)

Op de volgende pagina wordt u gevraagd de referenties opgeven voor Azure AD.

![Verbinding maken met Azure AD](./media/plan-connect-user-signin/changeusersignin2.png)

Op de **aanmelden van gebruikers** pagina, selecteert u de gewenste gebruiker aanmelden.

![Verbinding maken met Azure AD](./media/plan-connect-user-signin/changeusersignin2a.png)

> [!NOTE]
> Als u alleen een tijdelijke switch naar wachtwoord-hashsynchronisatie maakt, selecteert u de **gebruikersaccounts niet converteren** selectievakje. Niet controleren op de optie voor elke gebruiker federatieve wordt geconverteerd en het kan enkele uren duren.
>
>

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md).
- Meer informatie over [ontwerpconcepten Azure AD Connect](plan-connect-design-concepts.md).

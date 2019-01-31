---
title: On-premises voorwaardelijke toegang in Azure Active Directory instellen | Microsoft Docs
description: Een stapsgewijze handleiding voor het inschakelen van voorwaardelijke toegang tot on-premises toepassingen met behulp van Active Directory Federation Services (AD FS) in Windows Server 2012 R2.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.subservice: devices
ms.assetid: 6ae9df8b-31fe-4d72-9181-cf50cfebbf05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.custom: seohack1
ms.openlocfilehash: 00921fd82463a75e81e1279d436a576461e02f78
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55301642"
---
# <a name="setting-up-on-premises-conditional-access-by-using-azure-active-directory-device-registration"></a>Instellen van on-premises voorwaardelijke toegang met behulp van Azure Active Directory device Registration service

Wanneer u vereisen dat gebruikers met workplace join hun persoonlijke apparaten aan de apparaatregistratieservice van Azure Active Directory (Azure AD), kunnen hun apparaten worden gemarkeerd als bekend is bij uw organisatie. Hieronder volgt een stapsgewijze handleiding voor het inschakelen van voorwaardelijke toegang tot on-premises toepassingen met behulp van Active Directory Federation Services (AD FS) in Windows Server 2012 R2.

> [!NOTE]
> Een Office 365-licentie of Azure AD Premium-licentie is vereist bij het gebruik van apparaten die zijn geregistreerd in het beleid voor voorwaardelijke toegang van Azure Active Directory device registration service. Het gaat hierbij om beleidsregels die worden afgedwongen door AD FS in on-premises bronnen.
> 
> Zie voor meer informatie over de scenario's voor voorwaardelijke toegang voor on-premises bronnen, [toevoegt aan werkplek vanaf elk apparaat voor eenmalige aanmelding en naadloze tweeledige verificatie tussen bedrijfstoepassingen](https://technet.microsoft.com/library/dn280945.aspx).

Deze mogelijkheden zijn beschikbaar voor klanten die een Azure Active Directory Premium-licentie kopen.

## <a name="supported-devices"></a>Ondersteunde apparaten

* Windows 7-domein gekoppelde apparaten
* Windows 8.1 persoonlijke en domein apparaten
* iOS 6 en hoger voor de Safari-browser
* Android 4.0 of hoger, Samsung GS3 of hoger telefoons, Samsung Galaxy Opmerking 2 of hoger tablets

## <a name="scenario-prerequisites"></a>Scenario-vereisten

* Abonnement op Office 365 of Azure Active Directory Premium
* Een Azure Active Directory-tenant
* Windows Server Active Directory (WindowsServer 2008 of hoger)
* Bijgewerkte schema in Windows Server 2012 R2
* Licentie voor Azure Active Directory Premium
* Windows Server 2012 R2 Federation Services, geconfigureerd voor eenmalige aanmelding met Azure AD
* Windows Server 2012 R2 Web Application Proxy 
* Microsoft Azure Active Directory Connect (Azure AD Connect) [(Azure AD Connect downloaden)](https://www.microsoft.com/download/details.aspx?id=47594)
* Geverifieerd domein

## <a name="known-issues-in-this-release"></a>Bekende problemen in deze release

* Beleid voor voorwaardelijke toegang op basis van apparaat vereist Write-back van apparaat-object in Active Directory van Azure Active Directory. Duurt maximaal drie uur apparaatobjecten worden teruggeschreven naar Active Directory.
* de gebruiker een certificaat te selecteren tijdens de verificatie van clientcertificaten wordt altijd gevraagd door iOS 7-apparaten.
* Sommige versies van iOS 8 voordat iOS 8.3 niet werken.

## <a name="scenario-assumptions"></a>Scenario aannames

In dit scenario wordt ervan uitgegaan dat u hebt een hybride omgeving die bestaan uit een Azure AD-tenant en een on-premises Active Directory. Deze tenants moeten zijn verbonden met Azure AD Connect, met een geverifieerd domein en met AD FS voor eenmalige aanmelding. Gebruik de volgende controlelijst om hulp bij het configureren van uw omgeving op basis van de vereisten.

## <a name="checklist-prerequisites-for-conditional-access-scenario"></a>Checklist: Vereisten voor het scenario voor voorwaardelijke toegang

Verbinding maken met uw Azure AD-tenant met uw on-premises Active Directory-exemplaar.

## <a name="configure-azure-active-directory-device-registration-service"></a>Azure Active Directory device registratieservice configureren

Deze handleiding gebruiken om te implementeren en configureren van de Azure Active Directory device registratieservice voor uw organisatie.

Deze handleiding wordt ervan uitgegaan dat u Windows Server Active Directory hebt geconfigureerd en dat u hebt zich geabonneerd op Microsoft Azure Active Directory. Zie de vereisten die eerder zijn beschreven.

Voor het implementeren van de Azure Active Directory device registratieservice aan uw Azure Active Directory-tenant, moet u de taken in de volgende controlelijst in volgorde uitvoeren. Wanneer een koppeling u naar een conceptueel onderwerp gaat, terug naar deze controlelijst daarna, zodat u kunt doorgaan met de resterende taken. Sommige taken omvatten een scenario validatiestap kunt u controleren of de stap is voltooid.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Deel 1: Azure Active Directory-apparaatregistratie inschakelen

Volg de stappen in de controlelijst voor het inschakelen en configureren van de Azure Active Directory device registratieservice.

| Taak | Referentie | 
| --- | --- |
| Apparaatregistratie inschakelen in uw Azure Active Directory-tenant waarmee apparaten worden gekoppeld aan de werkplek. Azure multi-factor Authentication is standaard niet ingeschakeld voor de service. We raden echter aan dat u multi-factor Authentication gebruiken wanneer u een apparaat registreren. Voordat u multi-factor Authentication inschakelt in de registratieservice van Active Directory, zorg ervoor dat AD FS is geconfigureerd voor een multi-factor Authentication-provider. |[Azure Active Directory-apparaatregistratie inschakelen](active-directory-device-registration-get-started.md)| 
|Azure Active Directory device registratieservice detecteren apparaten door te zoeken naar bekende DNS-records. Uw bedrijfs-DNS configureren zodat apparaten kunnen uw Azure Active Directory device registration-service detecteren. |[Azure Active Directory-apparaatdetectie registratie configureren](active-directory-device-registration-get-started.md)| 


## <a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Deel 2: Implementeren en configureren van Windows Server 2012 R2 Active Directory Federation Services en stelt u een federatieve relatie met Azure AD

| Taak | Referentie |
| --- | --- |
| Implementeer Active Directory Domain Services met de schema-uitbreidingen voor Windows Server 2012 R2. U hoeft niet een van uw domeincontrollers upgraden naar Windows Server 2012 R2. De schema-upgrade is de enige vereiste. |[Uw Active Directory Domain Services-schema bijwerken](#upgrade-your-active-directory-domain-services-schema) |
| Azure Active Directory device registratieservice detecteren apparaten door te zoeken naar bekende DNS-records. Uw bedrijfs-DNS configureren zodat apparaten kunnen uw Azure Active Directory device registration-service detecteren. |[Voorbereiden van uw Active Directory ondersteuning voor apparaten](#prepare-your-active-directory-to-support-devices) |

## <a name="part-3-enable-device-writeback-in-azure-ad"></a>Deel 3: Apparaat terugschrijven inschakelen in Azure AD

| Taak | Referentie |
| --- | --- |
| Deel twee van "Enabling device writeback in Azure AD Connect." Nadat u klaar bent, kunt u terugkeren naar deze handleiding. |[Apparaat terugschrijven inschakelen in Azure AD Connect](hybrid/how-to-connect-device-writeback.md) |

## <a name="optional-part-4-enable-multi-factor-authentication"></a>[Optioneel] Deel 4: Multi-factor Authentication inschakelen

Sterk aanbevolen dat u een van de verschillende opties voor meervoudige verificatie configureren. Als u wilt de meervoudige verificatie vereisen, Zie [kiest u de multi-factor Authentication-beveiligingsoplossing voor u](authentication/concept-mfa-whichversion.md). Het bevat een beschrijving van elke oplossing en koppelingen naar hulp bij het configureren van de oplossing van uw keuze.

## <a name="part-5-verification"></a>Deel 5: Verificatie

De implementatie is voltooid en u enkele scenario's kunt uitproberen. Gebruik de volgende koppelingen om te experimenteren met de service en vertrouwd raken met de functies ervan.

| Taak | Referentie |
| --- | --- |
| Sommige apparaten toevoegen aan uw werkplek door met Azure Active Directory device registratieservice. U kunt deelnemen aan iOS-, Windows- en Android-apparaten. |[Apparaten toevoegen aan uw werkplek met Azure Active Directory device registratieservice](#join-devices-to-your-workplace-by-using-azure-active-directory-device-registration-service) |
| Weergeven en in- of uitschakelen van geregistreerde apparaten met behulp van de beheerdersportal. In deze taak kunt u sommige geregistreerde apparaten weergeven met behulp van de beheerdersportal. |[Active Directory device registration service overzicht van Azure](active-directory-device-registration-get-started.md) |
| Controleer of dat apparaatobjecten uit Azure Active Directory naar Windows Server Active Directory teruggeschreven. |[Controleer of de geregistreerde apparaten worden teruggeschreven naar Active Directory](#verify-that-registered-devices-are-written-back-to-active-directory) |
| Nu dat gebruikers hun apparaten registreren kunnen, kunt u de toepassing maken toegangsbeleid in AD FS waarmee alleen geregistreerde apparaten. In deze taak maakt u een toegangsregel van toepassing en een aangepast toegang geweigerd-bericht. |[Maak een beleid voor toepassingstoegang en een aangepast bericht voor geweigerde toegang](#create-an-application-access-policy-and-custom-access-denied-message) |

## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Azure Active Directory integreren met on-premises Active Directory

**Zie:**

- [Uw on-premises directory's integreren met Azure Active Directory](hybrid/whatis-hybrid-identity.md) : als u wilt bekijken conceptuele informatie.

- [Aangepaste installatie van Azure AD Connect](hybrid/how-to-connect-install-custom.md) - voor de installatie-instructies.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Uw Active Directory Domain Services-schema bijwerken

> [!NOTE]
> Na de upgrade van uw Active Directory-schema, het proces kan niet ongedaan worden gemaakt. Het is raadzaam dat u eerst de upgrade in een testomgeving uitvoeren.
> 

1. Meld u aan met de domeincontroller met een account dat zowel de enterprise-beheerder als de schema-administrator-rechten heeft.
1. Kopieer de **[media] \support\adprep** map en submappen op een van uw Active Directory-domeincontrollers (waarbij **[media]** is het pad naar de installatiemedia van Windows Server 2012 R2).
1. Vanaf een opdrachtprompt, gaat u naar de **adprep** Active directory en werken met **adprep.exe/forestprep**. Volg de aanwijzingen om de schema-upgrade te voltooien.

## <a name="prepare-your-active-directory-to-support-devices"></a>Voorbereiden van uw Active Directory om apparaten te ondersteunen

> [!NOTE]
> Dit is een eenmalige bewerking die u uitvoeren moet om voor te bereiden uw Active Directory-forest om apparaten te ondersteunen. Voor deze procedure moet u zijn aangemeld met ondernemingsadministratormachtigingen heeft en uw Active Directory-forest moet het schema voor Windows Server 2012 R2.
> 


### <a name="prepare-your-active-directory-forest"></a>Voorbereiden van uw Active Directory-forest

1. Op de federatieserver, open een Windows PowerShell-opdrachtvenster en typ vervolgens **initialiseren-ADDeviceRegistration**. 
1. Wanneer u wordt gevraagd voor **ServiceAccountName**, voer de naam van het serviceaccount dat u hebt geselecteerd als het serviceaccount voor AD FS. Als een gMSA-account is, voert u het account in de **domain\accountname$** indeling. Voor een domeinaccount, gebruik de indeling **domain\accountname**.

### <a name="enable-device-authentication-in-ad-fs"></a>Apparaatverificatie in AD FS

1. De AD FS-beheerconsole openen op uw federatieserver en Ga naar **AD FS** > **verificatiebeleid**.
1. Op de **acties** venster **globale primaire verificatie bewerken**.
1. Controleer **apparaatverificatie inschakelen**, en selecteer vervolgens **OK**.
1. Standaard worden AD FS regelmatig ongebruikte apparaten verwijderd uit Active Directory. Deze taak uitschakelen wanneer u apparaatregistratieservice van Azure Active Directory zodat apparaten kunnen worden beheerd in Azure.

### <a name="disable-unused-device-cleanup"></a>Niet-gebruikte apparaten opschonen uitschakelen

Op de federatieserver, open een Windows PowerShell-opdrachtvenster en typ vervolgens **Set AdfsDeviceRegistration - MaximumInactiveDays 0**.

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Azure AD Connect voorbereiden voor write-back van apparaat

Volledige deel 1: Voorbereiden van Azure AD Connect.

## <a name="join-devices-to-your-workplace-by-using-azure-active-directory-device-registration-service"></a>Apparaten toevoegen aan uw werkplek door met Azure Active Directory device registratieservice

### <a name="join-an-ios-device-by-using-azure-active-directory-device-registration"></a>Deelnemen aan een iOS-apparaat met behulp van Azure Active Directory device Registration service

Azure Active Directory device Registration service maakt gebruik van het registratieproces van het Over-the-Air-profiel voor iOS-apparaten. Dit proces wordt gestart wanneer de gebruiker verbinding met de profiel-URL voor inschrijving met Safari maakt. De URL-indeling is als volgt:

`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname>`

In dit geval `yourdomainname` de domeinnaam die u hebt geconfigureerd met Azure Active Directory. Bijvoorbeeld, als de naam van het domein contoso.com is, is de URL als volgt:

`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

Er zijn veel verschillende manieren om te communiceren deze URL naar uw gebruikers. Bijvoorbeeld, publiceert een methode die wordt aangeraden deze URL in een aangepaste toepassing toegang geweigerd-bericht in AD FS. Deze informatie wordt beschreven in het volgende gedeelte [maken van een beleid voor toepassingstoegang en een aangepast toegang geweigerd bericht](#create-an-application-access-policy-and-custom-access-denied-message).

### <a name="join-a-windows-81-device-by-using-azure-active-directory-device-registration"></a>Deelnemen aan een Windows 8.1-apparaat met behulp van Azure Active Directory device Registration service

1. Selecteer op het apparaat Windows 8.1 **PC-instellingen** > **netwerk** > **werkplek**.
1. Voer uw gebruikersnaam in UPN-indeling. bijvoorbeeld, **dan@contoso.com**.
1. Selecteer **Join**.
1. Wanneer u hierom wordt gevraagd, meldt u zich aan met uw referenties. Het apparaat is nu lid geworden.

### <a name="join-a-windows-7-device-by-using-azure-active-directory-device-registration"></a>Deelnemen aan een apparaat met Windows 7 met behulp van Azure Active Directory device Registration service

Voor het registreren van apparaten voor het domein van Windows 7, moet u implementeren de [softwarepakket voor apparaatregistratie](https://www.microsoft.com/download/details.aspx?id=53554).

Zie voor instructies over het gebruik van het pakket [Windows Installer-pakketten voor Windows 10-computers](devices/hybrid-azuread-join-control.md#control-windows-down-level-devices).

## <a name="verify-that-registered-devices-are-written-back-to-active-directory"></a>Controleer of dat geregistreerde apparaten worden teruggeschreven naar Active Directory

U kunt bekijken en controleren of dat uw apparaatobjecten hebben is teruggeschreven naar uw Active Directory met behulp van LDP.exe of ADSI bewerken. Beide zijn beschikbaar met de beheerhulpprogramma's voor Active Directory.

Apparaatobjecten die worden teruggeschreven uit Azure Active Directory worden standaard geplaatst in hetzelfde domein bevinden als uw AD FS-farm.

`CN=RegisteredDevices,defaultNamingContext`

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Maak een beleid voor toepassingstoegang en een aangepast bericht voor geweigerde toegang

Houd rekening met het volgende scenario: U een Relying Party Trust in AD FS-toepassing maken en configureren van een Uitgifteautorisatieregel waarmee alleen geregistreerde apparaten. Nu mogen alleen apparaten die zijn geregistreerd voor toegang tot de toepassing. 

Mogelijk te maken voor uw gebruikers toegang te krijgen tot de toepassing, moet u een aangepast toegang geweigerd bericht met instructies voor het toevoegen van hun apparaat configureren. Uw gebruikers hebben nu een naadloze manier voor het registreren van hun apparaten, zodat ze krijgen een toepassing tot toegang.

De volgende stappen laten zien hoe u dit scenario te implementeren.

> [!NOTE]
> In deze sectie wordt ervan uitgegaan dat u al hebt geconfigureerd een Relying Party Trust voor uw toepassing in AD FS.
> 

1. Open het hulpprogramma voor AD FS MMC en selecteer vervolgens **AD FS** > **vertrouwensrelaties** > **Relying Party-vertrouwensrelaties**.
1. Ga naar de toepassing waarop deze nieuwe toegangsregel van toepassing is. Met de rechtermuisknop op de toepassing en selecteer vervolgens **Claimregels bewerken**.
1. Selecteer de **autorisatieregels voor uitgifte** tabblad, en selecteer vervolgens **regel toevoegen**.
1. Uit de **claimregel** sjabloon vervolgkeuzelijst, selecteer **toestaan of weigeren gebruikers op basis van een binnenkomende Claim**. Selecteer vervolgens **Volgende**.
1. In de **naam van Claimregel** veld, typt u **toegang toestaan van geregistreerde apparaten**.
1. Uit de **type binnenkomende claim** vervolgkeuzelijst, selecteer **geregistreerde gebruiker Is**.
1. In de **binnenkomende claim waarde** veld, typt u **waar**.
1. Selecteer de **toegang verlenen aan gebruikers met deze binnenkomende claim** keuzerondje.
1. Selecteer **voltooien**, en selecteer vervolgens **toepassen**.
1. Verwijder alle regels die zijn ruimer dan de regel die u hebt gemaakt. Verwijder bijvoorbeeld de standaardregel **toegang toestaan voor alle gebruikers**.

Uw toepassing is nu geconfigureerd, zodat toegang alleen wanneer de gebruiker is die afkomstig zijn van een apparaat dat ze geregistreerd en toegevoegd aan de werkplek. Zie voor meer geavanceerde toegangsbeleid [beheren Risk with Additional multi-factor Authentication for Sensitive Applications](https://technet.microsoft.com/library/dn280949.aspx).

Vervolgens configureert u een aangepast foutbericht voor uw toepassing. Het foutbericht kan gebruikers weten dat ze hun apparaat aan de werkplek toevoegen moeten voordat ze krijgen de toepassing tot toegang. U kunt een aangepaste toepassing toegang geweigerd-bericht maken met behulp van aangepaste HTML- en PowerShell.

Open een PowerShell-opdrachtvenster en typ de volgende opdracht op uw federatieserver. Vervang gedeelten van de opdracht door de items die specifiek voor uw systeem zijn:

`Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage`

U moet uw apparaat registreren voordat u krijgt deze toepassing tot toegang.

**Als u een iOS-apparaat gebruikt, selecteert u deze koppeling naar uw apparaat**:

`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com`

Dit iOS-apparaat toevoegen aan uw werkplek.

Als u een Windows 8.1-apparaat gebruikt, kunt u uw apparaat toevoegen door te selecteren **PC-instellingen**> **netwerk** > **werkplek**.

In de voorgaande opdrachten **relying party vertrouwensrelatie naam** is de naam van de Relying Party-Trust-object van uw toepassing in AD FS.
En **uwdomein.com** de domeinnaam die u hebt geconfigureerd met Azure Active Directory (bijvoorbeeld contoso.com).
Zorg ervoor dat u eventuele regeleinden (indien aanwezig) verwijderen uit de HTML-inhoud die u doorgeeft aan de **Set AdfsRelyingPartyWebContent** cmdlet.

Wanneer gebruikers toegang uw toepassing van een apparaat dat niet geregistreerd bij de Azure Active Directory device registratieservice tot, zien ze nu een fout.

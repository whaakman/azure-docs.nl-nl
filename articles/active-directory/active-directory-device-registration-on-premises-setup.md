---
title: Voorwaardelijke toegang tot lokale in Azure Active Directory instellen | Microsoft Docs
description: Een stapsgewijze handleiding voor het inschakelen van voorwaardelijke toegang tot on-premises toepassingen met behulp van Active Directory Federation Services (AD FS) in Windows Server 2012 R2.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 6ae9df8b-31fe-4d72-9181-cf50cfebbf05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: markvi
ms.reviewer: jairoc
ms.custom: seohack1
ms.openlocfilehash: 466cd564f08b07d443db7cb1de59c5778682ed73
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="setting-up-on-premises-conditional-access-by-using-azure-active-directory-device-registration"></a>Lokale voorwaardelijke toegang instellen met behulp van Azure Active Directory-apparaatregistratie
Als u werkplek koppelen gebruikers hun persoonlijke apparaten naar de apparaatregistratieservice van Azure Active Directory (Azure AD), kunnen hun apparaten kunnen worden gemarkeerd als bekend is bij uw organisatie. Hier volgt een stapsgewijze handleiding voor het inschakelen van voorwaardelijke toegang tot on-premises toepassingen met behulp van Active Directory Federation Services (AD FS) in Windows Server 2012 R2.

> [!NOTE]
> Een licentie voor Office 365 of Azure AD Premium is vereist wanneer de apparaten die zijn geregistreerd in het beleid voor voorwaardelijke toegang van Azure Active Directory device registration service. Het gaat hierbij om beleidsregels die door AD FS in lokale bronnen worden afgedwongen.
> 
> Zie voor meer informatie over de scenario's voor voorwaardelijke toegang voor lokale bronnen [werkplek koppelen vanaf elk apparaat voor eenmalige aanmelding en naadloze tweeledige verificatie tussen bedrijfstoepassingen](https://technet.microsoft.com/library/dn280945.aspx).

Deze mogelijkheden zijn beschikbaar voor klanten die een Azure Active Directory Premium-licentie kopen.

## <a name="supported-devices"></a>Ondersteunde apparaten
* Domein Windows 7-apparaten
* Windows 8.1 persoonlijke en domein-apparaten
* iOS 6 en hoger voor de Safari-browser
* Android 4.0 of hoger, Samsung GS3 of hoger telefoons, Samsung Galaxy Opmerking 2 of hoger tablets

## <a name="scenario-prerequisites"></a>Scenario-vereisten
* Abonnement op Office 365 of Azure Active Directory Premium
* Een Azure Active Directory-tenant
* Windows Server Active Directory (WindowsServer 2008 of hoger)
* Bijgewerkte schema in Windows Server 2012 R2
* Licentie voor Azure Active Directory Premium
* Windows Server 2012 R2 Federation Services, geconfigureerd voor eenmalige aanmelding bij Azure AD
* Windows Server 2012 R2 Web Application Proxy 
* Microsoft Azure Active Directory Connect (Azure AD Connect) [(Azure AD Connect downloaden)](http://www.microsoft.com/en-us/download/details.aspx?id=47594)
* Geverifieerde domein

## <a name="known-issues-in-this-release"></a>Bekende problemen in deze release
* Beleid voor voorwaardelijke toegang op basis van apparaten vereisen apparaat object terugschrijven naar Active Directory van Azure Active Directory. Het kan apparaatobjecten worden teruggeschreven naar Active Directory tot drie uur duren.
* iOS 7-apparaten altijd de gebruiker gevraagd om een certificaat selecteren tijdens de verificatie van clientcertificaten.
* In sommige versies van iOS 8 voordat iOS 8.3 niet werken.

## <a name="scenario-assumptions"></a>Veronderstellingen scenario
Dit scenario wordt ervan uitgegaan dat u hebt een hybride omgeving die bestaan uit een Azure AD-tenant en een lokale Active Directory. Deze tenants moeten zijn verbonden met Azure AD Connect, met een geverifieerde domeinnaam en met AD FS voor eenmalige aanmelding. Gebruik de volgende controlelijst om hulp bij het configureren van uw omgeving afhankelijk van de vereisten.

## <a name="checklist-prerequisites-for-conditional-access-scenario"></a>Controlelijst: Vereisten voor het scenario voor voorwaardelijke toegang
Verbinding maken met uw Azure AD-tenant met uw lokale Active Directory-exemplaar.

## <a name="configure-azure-active-directory-device-registration-service"></a>Azure Active Directory device registratieservice configureren
Deze handleiding gebruiken om te implementeren en configureren van de Azure Active Directory device registratieservice voor uw organisatie.

Deze handleiding wordt ervan uitgegaan dat u Windows Server Active Directory hebt geconfigureerd en u bent geabonneerd op Microsoft Azure Active Directory. Zie de vereisten die eerder zijn beschreven.

Voer de taken in de volgende controlelijst in volgorde voor het implementeren van de Azure Active Directory device registratieservice aan uw Azure Active Directory-tenant. Wanneer een verwijzende koppeling u naar een conceptueel onderwerp gaat, terug naar deze controlelijst daarna, zodat u kunt doorgaan met de resterende taken. Sommige taken omvatten een scenario validatiestap kunt u controleren of de stap is voltooid.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Deel 1: Enable Azure Active Directory-apparaatregistratie
Volg de stappen in de controlelijst inschakelen en configureren van de Azure Active Directory device registratieservice.

| Taak | Referentie | 
| --- | --- |
| Schakel apparaatregistratie in uw Azure Active Directory-tenant om apparaten die aan de werkplek koppelen. Azure multi-factor Authentication is standaard niet ingeschakeld voor de service. We raden echter aan dat u multi-factor Authentication gebruiken wanneer u een apparaat registreren. Voordat u multi-factor Authentication inschakelt in de registratieservice van Active Directory, zorg ervoor dat de AD FS is geconfigureerd voor een multi-factor Authentication-provider. |[Azure Active Directory-apparaatregistratie inschakelen](active-directory-device-registration-get-started.md)| 
|Uw Azure Active Directory device registratieservice detecteren apparaten door te zoeken naar bekende DNS-records. Uw bedrijfs-DNS configureren zodat apparaten de apparaatregistratieservice van Azure Active Directory detecteren kunnen. |[Azure Active Directory device registration detectie configureren](active-directory-device-registration-get-started.md)| 


## <a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Deel 2: Implementeren en configureren van Windows Server 2012 R2 Active Directory Federation Services en stelt u een federatieve relatie met Azure AD

| Taak | Referentie |
| --- | --- |
| Active Directory Domain Services implementeren met de schema-uitbreidingen voor Windows Server 2012 R2. U hoeft niet een van uw domeincontrollers upgraden naar Windows Server 2012 R2. De schema-upgrade is de enige vereiste. |[Uw Active Directory Domain Services-schema bijwerken](#upgrade-your-active-directory-domain-services-schema) |
| Uw Azure Active Directory device registratieservice detecteren apparaten door te zoeken naar bekende DNS-records. Uw bedrijfs-DNS configureren zodat apparaten de apparaatregistratieservice van Azure Active Directory detecteren kunnen. |[Voorbereiden van uw Active Directory ondersteuning voor apparaten](#prepare-your-active-directory-to-support-devices) |

## <a name="part-3-enable-device-writeback-in-azure-ad"></a>Deel 3: Schakel apparaat terugschrijven in Azure AD
| Taak | Referentie |
| --- | --- |
| Deel 2 van "Enabling device writeback in Azure AD Connect." Nadat u klaar bent, keert u terug naar deze handleiding. |[Apparaat terugschrijven inschakelen in Azure AD Connect](#upgrade-your-active-directory-domain-services-schema) |

## <a name="optional-part-4-enable-multi-factor-authentication"></a>[Optioneel] Deel 4: Schakel multi-factor Authentication
Sterk aanbevolen een van de verschillende opties voor multi-factor Authentication te configureren. Als u meervoudige authenticatie wilt, Zie [de multi-factor Authentication-beveiligingsoplossing kiezen voor u](../multi-factor-authentication/multi-factor-authentication-get-started.md). Het bevat een beschrijving van elke oplossing en koppelingen naar informatie waarmee u de oplossing van uw keuze configureren.

## <a name="part-5-verification"></a>Deel 5: verificatie
De implementatie is voltooid en u een aantal scenario's kunt uitproberen. Gebruik de volgende koppelingen om te experimenteren met de service en vertrouwd raken met de functies.

| Taak | Referentie |
| --- | --- |
| Sommige apparaten koppelt aan uw werkplek met behulp van Azure Active Directory device registration-service. U kunt deelnemen aan iOS-, Windows- en Android-apparaten. |[Apparaten koppelt aan uw werkplek met Azure Active Directory device registratieservice](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Weergeven en in- of uitschakelen van geregistreerde apparaten met behulp van de beheerdersportal. In deze taak kunt u een aantal geregistreerde apparaten weergeven met behulp van de beheerdersportal. |[Azure Active Directory device Registratieoverzicht van service](active-directory-device-registration-get-started.md) |
| Controleer of dat apparaatobjecten van Azure Active Directory naar Windows Server Active Directory teruggeschreven. |[Geregistreerde apparaten worden teruggeschreven naar Active Directory controleren](#verify-registered-devices-are-written-back-to-active-directory) |
| Nu dat gebruikers hun apparaten registreren kunnen, kunt u de toepassing maken toegangsbeleid in AD FS die ervoor zorgen alleen geregistreerde apparaten dat. In deze taak maakt u een toegangsregel van toepassing en een aangepast bericht voor geweigerde toegang. |[Maak een beleid voor toepassingstoegang en een aangepaste bericht bij geweigerde toegang](#create-an-application-access-policy-and-custom-access-denied-message) |

## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Azure Active Directory integreren met on-premises Active Directory

**Zie:**

- [Uw on-premises adreslijsten integreren met Azure Active Directory](./connect/active-directory-aadconnect.md) - om conceptuele informatie te bekijken.

- [Aangepaste installatie van Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md) - voor de installatie-instructies.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Uw Active Directory Domain Services-schema bijwerken
> [!NOTE]
> Na de upgrade van uw Active Directory-schema, het proces kan niet ongedaan worden gemaakt. Het is raadzaam dat u eerst de upgrade in een testomgeving uitvoeren.
> 

1. Aanmelden bij uw domeincontroller met een account dat zowel ondernemingsadministrator en schema-administrator-rechten heeft.
2. Kopieer de **[media] \support\adprep** map en submappen op een van uw Active Directory-domeincontrollers (waarbij **[media]** is het pad naar de installatiemedia van Windows Server 2012 R2).
4. Vanaf een opdrachtprompt, gaat u naar de **adprep** map en voer **adprep.exe/forestprep**. Volg de aanwijzingen om de schema-upgrade te voltooien.

## <a name="prepare-your-active-directory-to-support-devices"></a>Voorbereiden van uw Active Directory om apparaten te ondersteunen
> [!NOTE]
> Dit is een eenmalige bewerking die u uitvoeren moet als u wilt voorbereiden van uw Active Directory-forest om apparaten te ondersteunen. Voor deze procedure als u bent aangemeld met ondernemingsadministratormachtigingen heeft en uw Active Directory-forest moet het schema van Windows Server 2012 R2.
> 


### <a name="prepare-your-active-directory-forest"></a>Uw Active Directory-forest voorbereiden
1. Open een Windows PowerShell-opdrachtvenster en typ op de federatieserver **initialiseren ADDeviceRegistration**. 
2. Als u wordt gevraagd **ServiceAccountName**, voer de naam van het serviceaccount dat u hebt geselecteerd als het serviceaccount voor AD FS. Als een beheerd serviceaccount is, voert u het account in de **domain\accountname$** indeling. Gebruik de notatie voor een domeinaccount **domain\accountname**.

### <a name="enable-device-authentication-in-ad-fs"></a>Verificatie van apparaten in AD FS inschakelen
1. Open de AD FS-beheerconsole op uw federatieserver en gaat u naar **AD FS** > **verificatiebeleid**.
2. Op de **acties** deelvenster **globale primaire authenticatie bewerken**.
3. Controleer **apparaatverificatie inschakelen**, en selecteer vervolgens **OK**.
4. Standaard verwijdert AD FS periodiek ongebruikte apparaten uit Active Directory. Deze taak uitschakelen wanneer u Azure Active Directory device registration-service zodat apparaten kunnen worden beheerd in Azure.

### <a name="disable-unused-device-cleanup"></a>Opruimen van niet-gebruikte apparaten uitschakelen
Open een Windows PowerShell-opdrachtvenster en typ op de federatieserver **Set AdfsDeviceRegistration - MaximumInactiveDays 0**.

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Azure AD Connect voorbereiden voor write-back van apparaat
Deel 1: voorbereiden van Azure AD Connect.

## <a name="join-devices-to-your-workplace-by-using-azure-active-directory-device-registration-service"></a>Apparaten aan uw werkplek te koppelen met behulp van Azure Active Directory device registration-service

### <a name="join-an-ios-device-by-using-azure-active-directory-device-registration"></a>Een iOS-apparaat koppelen met behulp van Azure Active Directory-apparaatregistratie
Azure Active Directory-apparaatregistratie maakt gebruik van het inschrijvingsproces van het Over-the-Air-profiel voor iOS-apparaten. Dit proces wordt gestart wanneer de gebruiker verbinding met de profiel-URL voor inschrijving met Safari maakt. De URL-indeling is als volgt:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

In dit geval `yourdomainname` de domeinnaam die u hebt geconfigureerd met Azure Active Directory. Bijvoorbeeld, als uw domeinnaam contoso.com is, de URL is als volgt:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Er zijn veel verschillende manieren om te communiceren deze URL voor uw gebruikers. Deze URL is bijvoorbeeld publiceren van een aanbevolen methode in een aangepaste toepassingsbericht bij geweigerde toegang in AD FS. Deze gegevens worden behandeld in het volgende gedeelte [maken van een beleid voor toepassingstoegang en aangepaste bericht bij geweigerde toegang](#create-an-application-access-policy-and-custom-access-denied-message).

### <a name="join-a-windows-81-device-by-using-azure-active-directory-device-registration"></a>Lid worden van een Windows 8.1-apparaat met Azure Active Directory-apparaatregistratie
1. Selecteer op het apparaat Windows 8.1 **PC-instellingen** > **netwerk** > **werkplek**.
2. Voer uw gebruikersnaam in UPN-indeling. bijvoorbeeld:  **dan@contoso.com** .
3. Selecteer **Join**.
4. Wanneer u wordt gevraagd, aanmelden met uw referenties. Het apparaat is nu lid geworden.

### <a name="join-a-windows-7-device-by-using-azure-active-directory-device-registration"></a>Lid worden van een apparaat met Windows 7 met Azure Active Directory-apparaatregistratie
Voor het registreren van apparaten voor Windows 7 domein, moet u voor het implementeren van de [softwarepakket voor apparaatregistratie](https://www.microsoft.com/download/details.aspx?id=53554).

Zie voor instructies over het gebruik van het pakket [Windows Installer-pakketten voor Windows 10-computers](device-management-hybrid-azuread-joined-devices-setup.md#windows-installer-packages-for-non-windows-10-computers).

## <a name="verify-that-registered-devices-are-written-back-to-active-directory"></a>Controleer of dat geregistreerde apparaten worden teruggeschreven naar Active Directory
U kunt bekijken en controleren of uw apparaatobjecten hebben is teruggeschreven naar uw Active Directory met LDP.exe of ADSI bewerken. Beide zijn beschikbaar met de beheerder van Active Directory's.

Standaard worden in hetzelfde domein bevinden als uw AD FS-farm apparaatobjecten die van Azure Active Directory worden teruggeschreven geplaatst.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Maak een beleid voor toepassingstoegang en een aangepaste bericht bij geweigerde toegang
Neem het volgende scenario: U een Relying Party Trust in AD FS-toepassing maken en configureren van een Uitgifteautorisatieregel waarmee alleen geregistreerde apparaten. Nu mogen alleen apparaten die zijn geregistreerd voor toegang tot de toepassing. 

Om het eenvoudig voor uw gebruikers toegang te krijgen tot de toepassing maken, moet u een aangepast toegang geweigerd bericht instructies voor het bevat koppelen van hun apparaat configureren. Uw gebruikers hebben nu een naadloze manier om hun apparaten registreren, zodat ze toegang een toepassing tot.

De volgende stappen ziet u hoe u dit scenario implementeert.

> [!NOTE]
> Deze sectie wordt ervan uitgegaan dat u al hebt geconfigureerd een Relying Party Trust voor uw toepassing in AD FS.
> 

1. Open het hulpprogramma voor AD FS MMC en selecteer vervolgens **AD FS** > **vertrouwensrelaties** > **Relying Party-vertrouwensrelaties**.
2. Ga naar de toepassing die deze nieuwe toegangsregel van toepassing is. Met de rechtermuisknop op de toepassing en selecteer vervolgens **Claimregels bewerken**.
3. Selecteer de **autorisatieregels voor uitgifte** tabblad en selecteer vervolgens **regel toevoegen**.
4. Van de **claimregel** sjabloon vervolgkeuzelijst, selecteer **toestaan of weigeren gebruikers op basis van een binnenkomende Claim**. Selecteer vervolgens **volgende**.
5. In de **naam Claimregel** veld **toegang verlenen van geregistreerde apparaten**.
6. Van de **type binnenkomende claim** vervolgkeuzelijst, selecteer **geregistreerde gebruiker Is**.
7. In de **binnenkomende claimwaarde** veld **true**.
8. Selecteer de **toegang verlenen aan gebruikers met deze binnenkomende claim** keuzerondje.
9. Selecteer **voltooien**, en selecteer vervolgens **toepassen**.
10. Verwijder alle regels die zijn dan de regel die u hebt gemaakt. Bijvoorbeeld, verwijderen van de standaardregel **toegang toestaan voor alle gebruikers**.

Uw toepassing is nu geconfigureerd voor toegang alleen wanneer de gebruiker afkomstig is van een apparaat dat ze geregistreerd en aan de werkplek heeft gekoppeld. Zie voor meer geavanceerde toegangsbeleid [risico's beheren met extra meervoudige verificatie voor gevoelige toepassingen](https://technet.microsoft.com/library/dn280949.aspx).

Vervolgens configureert u een aangepast foutbericht voor uw toepassing. Het foutbericht kan gebruikers weten dat ze hun apparaat aan de werkplek toevoegen moeten toegang te krijgen tot de toepassing. U kunt een aangepaste toepassing toegang geweigerd-bericht maken met behulp van aangepaste HTML- en PowerShell.

Open een PowerShell-opdrachtvenster en typ de volgende opdracht op de federatieserver. Vervang gedeelten van de opdracht met items die specifiek voor uw systeem zijn:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Voordat u toegang hebt tot deze toepassing, moet u uw apparaat registreren.

**Als u een iOS-apparaat gebruikt, selecteert u deze koppeling naar uw apparaat**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Deze iOS-apparaat koppelen aan uw werkplek.

Als u een Windows 8.1-apparaat gebruikt, kunt u uw apparaat toevoegen door te selecteren **PC-instellingen**> **netwerk** > **werkplek**.

In de voorgaande opdrachten **naam van de vertrouwensrelatie van relying party** is de naam van uw toepassing een vertrouwensrelatie van Relying Party-object in AD FS.
En **uwdomein.com** de domeinnaam die u hebt geconfigureerd met Azure Active Directory (bijvoorbeeld contoso.com).
Zorg ervoor dat eventuele regeleinden (indien aanwezig) verwijderen uit de HTML-inhoud die u doorgeeft aan de **Set AdfsRelyingPartyWebContent** cmdlet.

Wanneer gebruikers toegang krijgen uw toepassing van een apparaat dat niet geregistreerd bij Azure Active Directory device registratieservice tot, zien ze nu een fout opgetreden.


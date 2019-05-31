---
title: Implementatie van Azure managed werkstations - Azure Active Directory
description: Meer informatie over het implementeren van veilige Azure beheerde werkstations om het risico van inbreuk vanwege onjuiste configuratie of inbreuk op
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 242926c0821e4951d2a2bd2f858f63691baf1017
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66307225"
---
# <a name="deploy-a-secure-workstation"></a>Implementeren van een beveiligd werkstation

Nu dat u begrijpt [waarom werkstation toegang beveiligen is het belangrijk?](concept-azure-managed-workstation.md) is het tijd om te beginnen met het proces van implementatie met behulp van de beschikbare hulpprogramma's. Deze handleiding wordt de gedefinieerde profielen gebruiken om te maken van een werkstation dat is veiliger vanaf het begin.

![Implementatie van een beveiligd werkstation](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

Voordat u de oplossing implementeert, moet u het profiel dat u wilt gebruiken selecteren. Het is belangrijk te weten dat u kunt toepassen op een van de geselecteerde profielen en naar een andere verplaatsen het profiel in Intune op basis van de vereiste toe te wijzen. Meerdere profielen kunnen tegelijkertijd worden gebruikt in een implementatie, en toegewezen met behulp van de tag of groep toewijzingen.

| Profiel | Laag | Verbeterd | Hoog | Gespecialiseerd | Beveiligd | Isolated |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Gebruiker in Azure AD | Ja | Ja | Ja | Ja | Ja | Ja |
| Beheerd met Intune | Ja | Ja | Ja | Ja | Ja | Ja |
| Azure AD geregistreerd apparaat | Ja |  |  |  |  | |   |
| Apparaat Azure AD join |   | Ja | Ja | Ja | Ja | Ja |
| Intune-basisbeveiliging is toegepast |   | Ja <br> (Uitgebreid) | Ja <br> (HighSecurity) | Ja <br> (NCSC) | Ja <br> (Beveiligd) |  N.V.T. |
| Hardware voldoet aan de beveiligde Windows 10-standaarden |   | Ja | Ja | Ja | Ja | Ja |
| Microsoft Defender ATP ingeschakeld |   | Ja  | Ja | Ja | Ja | Ja |
| Het verwijderen van beheerdersrechten |   |   | Ja  | Ja | Ja | Ja |
| Implementatie met behulp van Microsoft Autopilot |   |   | Ja  | Ja | Ja | Ja |
| Apps die zijn geïnstalleerd door Intune |   |   |   | Ja | Ja |Ja |
| Beperkt tot alleen goedgekeurde lijst URL 's |   |   |   | Ja | Ja |Ja |
| Internet (Inkomend/uitgaand geblokkeerd) |   |   |   |  |  |Ja |

## <a name="license-requirements"></a>Licentievereisten

De concepten behandeld in deze handleiding wordt ervan uitgegaan Microsoft 365 Enterprise E5 of een equivalente SKU. Sommige van de aanbevelingen in deze handleiding kan worden geïmplementeerd met een lagere SKU's. Als u meer informatie vindt u op [Microsoft 365 Enterprise-licentieverlening](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

U wilt configureren [Groepslicenties](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) voor uw gebruikers voor het automatiseren van de inrichting van licenties.

## <a name="azure-active-directory-configuration"></a>Azure Active Directory-configuratie

Configureren van uw Azure Active Directory (Azure AD)-directory, die u uw gebruikers beheert, groepen en apparaten voor uw werkstations vereist dat u inschakelt identiteitsservices en -functies met een [administrator-account](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

Wanneer u het beveiligd werkstation administrator-account maakt, wordt u het account naar uw huidige werkstation weergegeven. Het is raadzaam dat u deze initiële configuratie en alle globale configuratie van een bekende, veilige apparaat doen. U kunt overwegen de richtlijnen voor het [malware-infecties te voorkomen dat](https://docs.microsoft.com/windows/security/threat-protection/intelligence/prevent-malware-infection) vermindert het risico van het blootstellen van eerst de ervaring van de eerste keer tegen een aanval.

Organisaties moeten ten minste multi-factor authentication vereisen voor de beheerders. Zie [cloud-gebaseerde MFA implementeren](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted) voor begeleiding bij implementatie.

### <a name="azure-ad-users-and-groups"></a>Azure AD-gebruikers en groepen

Vanuit de Azure-portal, blader naar **Azure Active Directory** > **gebruikers** > **nieuwe gebruiker**. [Maken van uw gebruikers beveiligd werkstation](https://docs.microsoft.com/Intune/quickstart-create-user), die de apparaatbeheerder van uw is.

* **Naam** -werkstation beheerder beveiligen
* **Gebruikersnaam** - secure-ws-admin@identityitpro.com
* **Maprol** - **beperkte beheerder** en selecteert u de volgende beheerdersrol
   * **Intune-beheerder**
* **Maken**

We gaan twee groepen één voor gebruikers van de werkstations en één voor de werkstations zelf maken. Vanuit de Azure-portal, blader naar **Azure Active Directory** > **groepen** > **nieuwe groep**

Eerste groep voor gebruikers van beheerwerkstations. U wilt configureren [Groepslicenties](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) voor de gebruikers in deze groep voor het automatiseren van de inrichting van licenties aan gebruikers.

* **Groepstype** -beveiliging
* **Naam van groep** -gebruikers van beheerwerkstations beveiligen
* **Lidmaatschapstype** - toegewezen
* Uw beveiligde werkstation beheerdersgebruiker toevoegen aan de groep
   * secure-ws-admin@identityitpro.com
* U kunt andere gebruikers die u veilig werkstations aan de groep beheren wilt toevoegen
* **Maken**

Tweede groep voor werkstation-apparaten.

* **Groepstype** -beveiliging
* **Naam van groep** -werkstations beveiligen
* **Lidmaatschapstype** - toegewezen
* **Maken**

### <a name="azure-ad-device-configuration"></a>Azure AD-apparaatconfiguratie

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Geef die apparaten kunt toevoegen aan Azure AD

Configureren van uw apparaten instellen in Active Directory om toe te staan uw administratieve beveiligingsgroep apparaten toevoegen aan uw domein. Voor het configureren van deze instelling vanuit Azure portal, blader naar **Azure Active Directory** > **apparaten** > **apparaatinstellingen**. Kies **geselecteerde** onder **gebruikers kunnen apparaten koppelen aan Azure AD** en selecteer de groep 'Beveiligd werkstation gebruikers'.

#### <a name="removal-of-local-admin-rights"></a>Het verwijderen van lokale beheerdersrechten

Als onderdeel van de rollout van hebben werkstations gebruikers van de VIP, DevOps en veilige niveau werkstations geen beheerdersrechten op hun virtuele machines. Voor het configureren van deze instelling vanuit Azure portal, blader naar **Azure Active Directory** > **apparaten** > **apparaatinstellingen**. Selecteer **geen** onder **extra lokale beheerders op Azure AD gekoppelde apparaten**.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Meervoudige verificatie worden toegevoegd aan apparaten vereisen

Als u wilt meer versterking van het proces van het toevoegen van apparaten aan Azure AD, blader naar **Azure Active Directory** > **apparaten** > **apparaatinstellingen** Kies **Ja** onder **multi-factor Authentication vereisen om apparaten** en kies vervolgens **opslaan**.

#### <a name="configure-mdm"></a>MDM configureren

Vanuit de Azure-portal, blader naar **Azure Active Directory** > **Mobility (MDM en MAM)**  > **Microsoft Intune**. Wijzig de instelling **gebruikersbereik van MDM** naar **alle** en kies **opslaan** als we elk apparaat kan worden beheerd door Intune in dit scenario toestaat. Meer informatie vindt u in het artikel [Intune Quickstart: Instellen van automatische inschrijving voor Windows 10-apparaten](https://docs.microsoft.com/Intune/quickstart-setup-auto-enrollment). In een toekomstige stap maken we Intune-beleid voor configuratie en nalevingsbeleid.

#### <a name="azure-ad-conditional-access"></a>Voorwaardelijke toegang van Azure AD

Voorwaardelijke toegang van Azure AD kunt deze bevoegde administratieve taken bewaren op compatibele apparaten. Gebruikers die we hebben gedefinieerd als leden van de **beveiligd werkstation gebruikers** groep moeten multi-factor authentication uitvoeren tijdens het aanmelden bij toepassingen in de cloud. We de richtlijnen voor best practices te volgen en onze accounts voor toegang in noodgevallen uitsluiten van het beleid. Als u meer informatie vindt u in het artikel [toegang in noodgevallen accounts beheren in Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)

Als u wilt configureren voor voorwaardelijke toegang van Azure portal, blader naar **Azure Active Directory** > **voorwaardelijke toegang** > **nieuw beleid**.

* **Naam** -apparaat beveiligen beleid vereist
* Toewijzingen
   * **Gebruikers en groepen**
      * Opnemen - **gebruikers en groepen** : Selecteer de **beveiligd werkstation gebruikers** groep die u eerder hebt gemaakt
      * Uitsluiten - **gebruikers en groepen** -van uw organisatie voor toegang in noodgevallen accounts selecteren
   * **Cloud-apps**
      * Opnemen - **alle cloud-apps**
* Besturingselementen voor toegang
   * **Verleen** : Selecteer **toegang verlenen** keuzerondje
      * **Meervoudige verificatie vereisen**
      * **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**
      * Voor meerdere besturingselementen - **alle geselecteerde besturingselementen vereisen**
* Beleid - inschakelen **op**

Organisaties kunnen eventueel maken voor beleid voor het blok landen waar gebruikers geen toegang bedrijfsbronnen tot zouden. Meer informatie over het beleid voor voorwaardelijke toegang op basis van locatie van IP-kan worden gevonden in het artikel [wat is er met de locatievoorwaarde in Azure Active Directory voor voorwaardelijke toegang?](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition)

## <a name="intune-configuration"></a>Intune-configuratie

### <a name="configure-enrollment-status"></a>Status van inschrijving configureren

Zoals beschreven in de toeleveringsketenbeheer, ervoor te zorgen dat het beveiligd werkstation is een vertrouwd apparaat schoon het is raadzaam dat bij de aanschaf van nieuwe apparaten, die apparaten ingesteld worden op factory [Windows 10 Pro in de modus S](https://docs.microsoft.com/Windows/deployment/Windows-10-pro-in-s-mode), die de blootstelling van en zwakke plekken tijdens het beheer van de leveringsketen. Nadat een apparaat van de leverancier is ontvangen, wordt het apparaat wordt verwijderd uit de S met behulp van Autopilot. De volgende richtlijnen bevat informatie over het toepassen van de transformatie-proces.

Willen we ervoor zorgen dat apparaten volledig zijn geconfigureerd voor gebruik. Intune biedt een manier om **apparaat gebruik blokkeren totdat alle apps en -profielen zijn geïnstalleerd**. 

1. Uit de **Azure-portal** gaat u naar:
1. **Microsoft Intune** > **apparaatinschrijving** > **Windows-inschrijving** > **pagina van de Status van inschrijving (Preview)**  >  **Standaard** > **instellingen**.
1. Stel **installatievoortgang voor app-profiel weergeven** naar **Ja**.
1. Stel **apparaat gebruik blokkeren totdat alle apps en -profielen zijn geïnstalleerd** naar **Ja**.

### <a name="create-an-autopilot-deployment-profile"></a>Een Windows Autopilot-implementatieprofiel maken

Na het maken van een groep apparaten, moet u een implementatieprofiel maken zodat u de Autopilot-apparaten kunt configureren.

1. Kies in Intune in Azure portal **apparaatinschrijving** > **Windows-inschrijving** > **Implementatieprofielen**  >   **Profiel maken**.
1. Voer voor de naam van **beveiligd werkstation implementatieprofiel**. Voer voor de beschrijving, **implementatie van beveiligde werkstations**.
1. Alle doelapparaten set converteren naar Autopilot op Ja. Deze instelling zorgt ervoor dat alle apparaten in de lijst met de Autopilot deployment-service te registreren.  Het kan 48 uur voor de registratie moeten worden verwerkt.
1. Kies voor de implementatie-modus, **zelf implementeren (Preview)** . Apparaten met dit profiel zijn gekoppeld aan de gebruiker het apparaat wordt ingeschreven. Gebruikersreferenties zijn vereist om het apparaat te registreren.
1. In de Join naar Azure AD als vak **toegevoegd aan Azure AD** moet worden gekozen en grijs.
1. Out-of-box experience (OOBE) selecteert, configureert u de volgende opties en laat anderen ingesteld op de standaardwaarden en selecteer vervolgens **Ok**:
   1. Gebruikersaccounttype: **Standard**
1. Selecteer **Maken** om het profiel te maken. Het Autopilot-implementatieprofiel is nu beschikbaar om toe te wijzen aan apparaten.
1. Kies **toewijzingen** > **toewijzen aan** > **geselecteerde groepen**
   1. **Selecteer groepen om op te nemen** -gebruikers van beheerwerkstations beveiligen

### <a name="configure-windows-update"></a>Windows Update configureren

Een van de meest belangrijke dingen met een organisatie doen kunt is Windows 10 up-to-date te houden. Om te kunnen handhaven Windows 10 in een beveiligde status, implementeren we een update-ring voor het beheren van de snelheid waarmee updates worden toegepast op werkstations. Deze configuratie kunt u vinden in de **Azure-portal** > **Microsoft Intune** > **Software-updates**  >  **Windows 10-Updateringen**.

We zullen **maken** een nieuwe update-ring met de volgende instellingen gewijzigd van de standaardwaarden.

* Naam van de - **Azure beheerd werkstation updates**
* Servicing-kanaal - **Windows Insider - snel**
* Kwaliteit update uitstellen (dagen) - **3**
* Uitstelperiode (dagen) - **3**
* Gedrag van automatische updates - **automatisch installeren en opnieuw opstarten zonder tussenkomst eindgebruiker**
* Blokkeren dat gebruiker onderbreken Windows-updates - **blokkeren**
* Vraag goedkeuring van de gebruiker opnieuw opstarten buiten de werkuren - **vereist**
* Gebruiker toestaan om te starten (betrokken opnieuw opstarten) - **vereist**
   * Overgang van gebruikers betrokken opnieuw opstarten na een automatisch opnieuw opstarten (dagen) - **3**
   * Herinnering voor betrokken opnieuw opstarten uitstellen (dagen) - **3**
   * Deadline voor in behandeling instellen opnieuw wordt opgestart (dagen) - **3**

Klik op **maken** klik vervolgens op de **toewijzingen** tabblad toevoegen de **werkstations beveiligen** als een opgenomen groep een groep.

Meer informatie over Windows Update-beleid kan worden gevonden in [beleid CSP - Update](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-update)

### <a name="windows-defender-atp-intune-integration"></a>Windows Defender ATP-Intune-integratie

Windows Defender ATP- en Microsoft Intune werken samen om beveiligingslekken te voorkomen en beperken de gevolgen van schendingen. De mogelijkheden biedt realtime gedetecteerd. ATP biedt ook onze implementatie uitgebreide controle en logboekregistratie van de eindpunt-apparaten.

Voor het configureren van Windows Defender ATP-Intune-integratie in Azure portal, blader naar **Microsoft Intune** > **apparaatcompatibiliteit** > **Windows Defender ATP** .

1. In stap 1 onder **configureren van Windows Defender ATP**, klikt u op **verbinding maken met Windows Defender ATP aan Microsoft Intune in de Windows Defender-Beveiligingscentrum**.
1. In de Windows Defender-Beveiligingscentrum:
   1. Selecteer **instellingen** > **geavanceerde functies**
   1. Voor **Microsoft Intune verbinding**, kiest u **op**
   1. Selecteer **voorkeuren opslaan**
1. Nadat een verbinding tot stand is gebracht, keert u terug naar Intune en klik **vernieuwen** aan de bovenkant.
1. Stel **verbinding maken met Windows-apparaten versie 10.0.15063 en hoger naar Windows Defender ATP** naar **op**.
1. **Opslaan**

Als u meer informatie vindt u in het artikel [Windows Defender Advanced Threat Protection](https://docs.microsoft.com/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="completing-hardening-of-the-workstation-profile"></a>Beveiliging van het werkstation profiel voltooien

Voor de beveiliging van de oplossing, downloaden en uitvoeren van het script op basis van de gewenste **profiel niveau** uit het volgende diagram.

| Profiel | Downloadlocatie | Bestandsnaam |
| --- | --- | --- |
| Lage beveiliging | N/A |  N/A |
| Verbeterde beveiliging | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809).ps1 |
| Hoge beveiliging  | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Gespecialiseerd | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline.ps1 |
| Gespecialiseerde naleving * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Beveiligd | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

Gespecialiseerde naleving * is een script dat de speciale configuratie in de NCSC Windows10 SecurityBaseline wordt afgedwongen.

Wanneer de geselecteerde script met succes wordt uitgevoerd, kunnen updates van de beleidsregels en profielen worden gemaakt in Microsoft Intune. De scripts voor geavanceerde en veilige profielen beleid maken en -profielen voor u, maar u moeten het beleid toepassen voor uw **werkstations beveiligen** groep.

* Intune-profielen voor apparaatconfiguratie die zijn gemaakt door de scripts kunnen u vinden in de **Azure-portal** > **Microsoft Intune** > **apparaatconfiguratie**  >  **Profielen**.
* Nalevingsbeleid voor Intune-apparaten die zijn gemaakt door de scripts kunnen u vinden in de **Azure-portal** > **Microsoft Intune** > **apparaatcompatibiliteit**  >  **Beleid**.

Als u wilt controleren van de wijzigingen kunt u ook de profielen exporteren en wijzigingen toepassen op het exportbestand, zoals wordt beschreven in de SECCON documentatie op basis van en extra beperking die is vereist.

Script voor het uitvoeren van de Intune-gegevens exporteren `DeviceConfiguration_Export.ps1` uit de [DeviceConfiguration GiuHub opslagplaats](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) krijgt u de huidige uitvoer van alle bestaande Intune-profielen.

## <a name="additional-configurations-and-hardening-to-consider"></a>Aanvullende configuraties en beveiliging te houden

De richtlijnen van een beveiligd werkstation is ingeschakeld, zijn extra besturingselementen moeten ook worden overwogen, zoals andere browsers toegang krijgen tot, uitgaande HTTP toegestane en geblokkeerde websites en de mogelijkheid om aangepaste PowerShell-script uit te voeren.

### <a name="restrictive-inbound-and-outbound-rules-in-firewall-configuration-service-provider-csp"></a>Beperkende inkomende en uitgaande regels in de firewall configuration serviceprovider (CSP)

Extra beheer van zowel binnenkomende als uitgaande regels kan worden bijgewerkt om de toegestane en geblokkeerde eindpunten weer te geven. Als we blijven voor het beperken van het beveiligd werkstation, we de beperking die wordt verplaatst naar een weigeren alle inkomende en uitgaande als standaard en toegestane sites voor de uitgaande in overeenstemming met algemene en vertrouwde websites toevoegen. De aanvullende configuratie-informatie voor de Firewall configuration serviceprovider kan worden gevonden in het artikel [Firewall CSP](https://docs.microsoft.com/Windows/client-management/mdm/firewall-csp).

Standaard beperkt aanbevelingen zijn:

* Alle binnenkomend verkeer weigeren
* Al het uitgaande weigeren

Het Spamhaus Project houdt een goede lijst die organisaties als uitgangspunt gebruiken kunnen voor het blokkeren van sites die bekend als [het domein blok lijst (dubbele)](https://www.spamhaus.org/dbl/).

### <a name="managing-local-applications"></a>Beheer van lokale toepassingen

Verwijderen van lokale toepassingen, wordt met inbegrip van het verwijderen van toepassingen het beveiligd werkstation naar een echt beperkte status verplaatsen. In ons voorbeeld we Chrome toevoegen als de standaardbrowser en de mogelijkheid om te wijzigen van de browser met inbegrip van plug-ins beperken met behulp van Intune.

#### <a name="deploy-applications-using-intune"></a>Implementatie van toepassingen met behulp van Intune

In sommige gevallen, toepassingen, zoals de Google Chrome-browser vereist voor het beveiligd werkstation. Het volgende voorbeeld vindt u instructies voor het installeren van Chrome op apparaten in de beveiligingsgroep **werkstations beveiligen** eerder hebt gemaakt.

1. De offline-installatieprogramma downloaden [Chrome-pakket voor Windows 64‑bit](https://cloud.google.com/chrome-enterprise/browser/download/)
1. Pak de bestanden en noteer de locatie van de `GoogleChromeStandaloneEnterprise64.msi` te installeren met behulp van Intune
1. In de **Azure-portal** bladert u naar **Microsoft Intune** > **Client-apps** > **Apps**  >  **Toevoegen**
1. Onder **App-type**, kiest u **Line-of-business**
1. Onder **App-pakketbestand**, selecteer de `GoogleChromeStandaloneEnterprise64.msi` uit de uitgepakte locatie en op **OK**
1. Onder **App-gegevens**, Geef een beschrijving en de uitgever en kies **OK**
1. Klik op **toevoegen**
1. Op de **toewijzingen** Selecteer **beschikbaar voor geregistreerde apparaten** onder **toewijzingstype**
1. Onder **opgenomen groepen**, voeg de **werkstations beveiligen** groep die u eerder hebt gemaakt
1. Klik op **OK** vervolgens **opslaan**

Meer informatie over het configureren van de Chrome-instellingen kunt u vinden in hun ondersteuningsartikel [Chrome-Browser beheren met Microsoft Intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Configureren van de bedrijfsportal-App voor aangepaste apps

In een beveiligde modus, installeren van toepassingen, worden beperkt tot de Intune-bedrijfsportal-App. Installatie van de portal vereist echter toegang tot Microsoft Store. In onze beveiligde oplossing doen wij de portal beschikbaar voor alle apparaten met behulp van een offline modus van de bedrijfsportal-App.

Installatie van een Intune beheerd exemplaar van de [bedrijfsportal](https://docs.microsoft.com/Intune/store-apps-company-portal-app) de mogelijkheid om extra hulpprogramma's op aanvraag voor gebruikers van de beveiligde werkstations toestaat.

Sommige organisaties kunnen worden vereist voor het installeren van apps voor Windows 32-bits of apps waarvoor een andere voorbereidingen treffen om te implementeren. Voor deze toepassingen de [Microsoft win32 inhoud prep tool](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) biedt een gebruiksklare `.intunewin` format-bestand voor installatie.

### <a name="setting-up-custom-settings-using-powershell"></a>Instellen van aangepaste instellingen met behulp van PowerShell

We gebruiken ook een voorbeeld van bij het beheren van de host van uitbreidbaarheid met behulp van PowerShell. Het script stelt een standaardachtergrond op de host. Deze mogelijkheid is ook beschikbaar in profielen, en wordt alleen gebruikt ter illustratie van de capaciteit.

In de oplossing is het mogelijk dat er een nodig voor het instellen van bepaalde aangepaste besturingselementen en instellingen op de beveiligde werkstations. In ons voorbeeld zullen we de achtergrond van het werkstation met behulp van Powershell om het apparaat gemakkelijk herkennen als een beveiligd werkstation klaar voor gebruik te kunnen wijzigen. In ons voorbeeld wordt PowerShell gebruiken om deze taak te voltooien, kan deze ook worden uitgevoerd in Azure portal.

In ons voorbeeld gebruikt u de volgende [gratis algemene achtergrondafbeelding](https://i.imgur.com/OAJ28zO.png) en de [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) van het Microsoft Scripting Center om toe te staan van Windows te laden van de achtergrond op het startscherm.

1. Download het script naar een lokaal apparaat
1. Werk de customerXXXX en de locatie van de achtergrond die u gebruiken in het script wilt in overeenstemming met de achtergrondbestand en map die u wilt dat de implementatie te gebruiken. In dit voorbeeld vervangen we customerXXXX bij achtergronden.  
1. Blader naar de **Azure-portal** > **Microsoft Intune** > **apparaatconfiguratie** > **PowerShell scripts** > **toevoegen**
1. Geef een **naam** voor het script en geef de **scriptlocatie** waar u deze hebt gedownload
1. Selecteer **configureren**
   1. Stel **Voer dit script met de aangemelde referenties**naar **Ja**
   1. Klik op **OK**
1. Klik op **Maken**
1. Selecteer **toewijzingen** > **groepen selecteren**
   1. Voeg de beveiligingsgroep toe **werkstations beveiligen** eerder hebt gemaakt en klikt u op **opslaan**

### <a name="using-the-preview-mdm-security-baseline-for-october-2018"></a>Met behulp van de Preview-versie: MDM-basisbeveiliging voor oktober 2018

Microsoft Intune heeft basislijn management beveiligingsfunctie biedt beheerders een eenvoudige manier om af te dwingen een algemene basislijn beveiligingspostuur geïntroduceerd. De basislijn biedt een vergelijkbare manier te bereiken een vergrendelde omlaag uitgebreid profiel werkstation.

Voor de beveiligde werkstations conflicteert implementatie die deze basislijn niet als deze gebruikt wordt met de implementatie van veilig.

|   |   |   |
| :---: | :---: | :---: |
| Vergrendeling | De installatie van apparaat | Externe bureaubladservices |
| App-Runtime | Apparaat vergrendelen | Extern beheer |
| Beheer van toepassingen | Event Log-Service | Externe procedureaanroep |
| Automatisch afspelen | Ervaring | Search |
| BitLocker | Exploit Guard | Smart Screen |
| Browser | Verkenner | Systeemvereiste|
| Connectiviteit | Internet Explorer | Wi-Fi |
| Referentieoverdracht | Beveiligingsopties lokaal beleid | Windows-Verbindingsbeheer |
| Referenties UI | MS-beveiligingshandleiding | Windows Defender|
| Gegevensbeveiliging | MSS Legacy | Windows Ink Workspace |
| Device Guard | Energiebeheer | Windows PowerShell |

Meer informatie over deze preview-functie kan worden gevonden in het artikel [Windows beveiligingsinstellingen basislijn voor Intune](https://docs.microsoft.com/Intune/security-baseline-settings-windows).

## <a name="enroll-and-validate-your-first-device"></a>Registreren en uw eerste apparaat valideren

1. Als u wilt uw apparaat registreert, moet u de volgende informatie:
   * **Serienummer** - gevonden op het apparaat-chassis
   * **Windows-Product-ID** - gevonden onder **System** > **over** in het menu instellingen van Windows.
   * Met [Get-WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) biedt een hash-CSV-bestand voor de inschrijving van apparaten met alle benodigde informatie. 
      * Voer `Get-WindowsAutoPilotInfo – outputfile device1.csv` om uit te voeren van de gegevens als een CSV-bestand dat kan worden geïmporteerd Intune.

   > [!NOTE]
   > Het script vereist verhoogde rechten en uitvoeren als externe ondertekend. U kunt de volgende opdracht gebruiken waarmee het script correct uit te voeren. `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

   *  U kunt deze informatie verzamelen door het aanmelden bij een Windows 10-versie 1809-apparaat of hoger om de gegevens te verzamelen, of de verkoper van uw hardware kunt u deze informatie verstrekken tijdens het ordenen van nieuwe apparaten.
1. De vereiste gegevens verzamelen en gaat u terug naar de **Azure-portal**. Navigeer naar **Microsoft Intune** > **apparaatinschrijving** > **Windows-inschrijving** >  **-apparaten: Windows Autopilot-apparaten beheren**, selecteer **importeren**, en kies het CSV-bestand dat u hebt gemaakt of zijn opgegeven.
1. Nu geregistreerde apparaten toevoegen aan de beveiligingsgroep **werkstations beveiligen** eerder hebt gemaakt.
1. Via de Windows 10-apparaat u wilt configureren, blader naar **Windows-instellingen** > **bijwerken en beveiliging** > **Recovery**. Kies **aan de slag** onder **opnieuw instellen van deze PC** en volg de aanwijzingen voor het opnieuw instellen en configureren van het apparaat met behulp van de naleving van profielen en beleidsregels die zijn geconfigureerd.

Nadat het apparaat is geconfigureerd, een beoordeling uitvoeren en controleer de configuratie. Controleer of dat het eerste apparaat juist is geconfigureerd voordat u doorgaat uw implementatie.

## <a name="assignment-and-monitoring"></a>Toewijzing van en controle

Apparaten en gebruikers toewijzen, moet de toewijzing van de [geselecteerd profielen](https://docs.microsoft.com/intune/device-profile-assign) de beveiliging van uw groep en alle nieuwe gebruikers die wordt gegeven van machtigingen voor de service moet worden toegevoegd aan de beveiligingsgroep ook.

Bewaking van de profielen kan worden gedaan met behulp van de bewaking [Microsoft Intune-profielen](https://docs.microsoft.com/intune/device-profile-monitor).

## <a name="next-steps"></a>Volgende stappen

[Microsoft Intune](https://docs.microsoft.com/intune/index) documentatie

[Azure AD](https://docs.microsoft.com/azure/active-directory/index) documentatie
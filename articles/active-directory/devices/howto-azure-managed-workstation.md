---
title: Door Azure beheerde werk stations implementeren-Azure Active Directory
description: Meer informatie over het implementeren van beveiligde, door Azure beheerde werk stations om het risico van schending te verminderen vanwege een onjuiste configuratie of inbreuk.
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
ms.openlocfilehash: be9e6374d92fbb7bb1c4b5a2a9e154119c5baf87
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377486"
---
# <a name="deploy-a-secure-azure-managed-workstation"></a>Een beveiligd, door Azure beheerd werk station implementeren

Nu u bekend bent met [beveiligde werk stations](concept-azure-managed-workstation.md), is het tijd om het implementatie proces te starten. Met deze richt lijnen gebruikt u gedefinieerde profielen om een werk station te maken dat veiliger is dan het begin.

![Implementatie van een beveiligd werk station](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

U moet een profiel selecteren voordat u de oplossing kunt implementeren. U kunt meerdere profielen tegelijk in een implementatie gebruiken en deze toewijzen aan Tags of groepen.
> [!NOTE]
> Pas een van de profielen toe die nodig zijn voor uw vereisten. U kunt overstappen op een ander profiel door het toe te wijzen aan intune.

| Profiel | Laag | Verbeterd | Hoog | Gespecialiseerd | Beveiligd | Isolated |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Gebruiker in azure AD | Ja | Ja | Ja | Ja | Ja | Ja |
| Door intune beheerde | Ja | Ja | Ja | Ja | Ja | Ja |
| Apparaat-Azure AD geregistreerd | Ja |  |  |  |  | |   |
| Apparaat-Azure AD toegevoegd |   | Ja | Ja | Ja | Ja | Ja |
| Basis lijn voor beveiliging van intune toegepast |   | Ja <br> Intensievere | Ja <br> (HighSecurity) | Ja <br> (NCSC) | Ja <br> Gesteld |  N.v.t. |
| Hardware voldoet aan de veilige Windows 10-standaarden |   | Ja | Ja | Ja | Ja | Ja |
| Micro soft Defender ATP is ingeschakeld |   | Ja  | Ja | Ja | Ja | Ja |
| Beheer rechten verwijderen |   |   | Ja  | Ja | Ja | Ja |
| Implementatie met micro soft auto pilot |   |   | Ja  | Ja | Ja | Ja |
| Apps die alleen door intune zijn geïnstalleerd |   |   |   | Ja | Ja |Ja |
| Url's die zijn beperkt tot goedgekeurde lijst |   |   |   | Ja | Ja |Ja |
| Internet geblokkeerd (inkomend/uitgaand) |   |   |   |  |  |Ja |

## <a name="license-requirements"></a>Licentievereisten

Voor de concepten die in deze hand leiding worden behandeld, wordt ervan uitgegaan dat u Microsoft 365 Enterprise E5 of een vergelijk bare SKU hebt. Enkele van de aanbevelingen in deze hand leiding kunnen worden geïmplementeerd met lagere Sku's. Zie [Microsoft 365 Enterprise Licensing](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise)voor meer informatie.

U kunt het inrichten van licenties automatiseren door op [groepen gebaseerde licentie verlening](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) te nemen voor uw gebruikers.

## <a name="azure-active-directory-configuration"></a>Azure Active Directory configuratie

Azure Active Directory (Azure AD) beheert gebruikers, groepen en apparaten voor uw beheerders werkstations. U moet identiteits Services en-functies inschakelen met een [beheerders account](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

Wanneer u het beveiligde werk station Administrator-account maakt, wordt het account beschikbaar gesteld aan uw huidige werk station. Zorg ervoor dat u een bekend veilig apparaat gebruikt om deze eerste configuratie en alle globale configuratie uit te voeren. Als u de bloot stelling van aanvallen voor de eerste keer wilt beperken, kunt u overwegen de [richt lijnen te volgen om malware-infecties te voor komen](https://docs.microsoft.com/windows/security/threat-protection/intelligence/prevent-malware-infection).

U moet ook multi-factor Authentication vereisen, ten minste voor uw beheerders. Zie op [cloud gebaseerde MFA implementeren](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted) voor implementatie richtlijnen.

### <a name="azure-ad-users-and-groups"></a>Azure AD-gebruikers en-groepen

1. Blader in het Azure Portal naar **Azure Active Directory** > **gebruikers** > **nieuwe gebruiker**.
1. Maak de apparaat-beheerder door de stappen in de [zelf studie gebruikers maken](https://docs.microsoft.com/Intune/quickstart-create-user)te volgen.
1. Voer
   * **Naam** : Beveilig de beheerder van het werk station
   * **Gebruikers naam** - `secure-ws-admin@identityitpro.com`
   * Beperkte beheerder en selecteer de rol intune- **beheerder** .  - 
1. Selecteer **Maken**.

Vervolgens maakt u twee groepen: werk Station gebruikers en Workstation apparaten.

Blader in het Azure Portal naar **Azure Active Directory** > **groepen** > **nieuwe groep**.

1. Voor de werk Station-gebruikers groep wilt u mogelijk [licentie verlening op basis](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) van een groep configureren om het inrichten van licenties voor gebruikers te automatiseren.
1. Voer voor de groep werk Station gebruikers het volgende in:
   * **Groeps type** -beveiliging
   * **Groeps naam** -beveiligde werk Station gebruikers
   * **Type lidmaatschap** : toegewezen
1. Uw beveiligde werk station-beheerders gebruiker toevoegen:`secure-ws-admin@identityitpro.com`
1. U kunt andere gebruikers toevoegen die beveiligde werk stations gaan beheren.
1. Selecteer **Maken**.

1. Voer voor de groep werk station apparaten het volgende in:
   * **Groeps type** -beveiliging
   * **Groeps naam** -beveiligde werk stations
   * **Type lidmaatschap** : toegewezen
1. Selecteer **Maken**.

### <a name="azure-ad-device-configuration"></a>Configuratie van Azure AD-apparaten

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Opgeven wie apparaten mag toevoegen aan Azure AD

Configureer de instelling van uw apparaten in Active Directory zodat uw administratieve beveiligings groep apparaten kan toevoegen aan uw domein. Als u deze instelling wilt configureren vanuit de Azure Portal:

1. Ga naar de**Apparaatinstellingen**van **Azure Active Directory** > **apparaten** > .
1. Kies **geselecteerd** onder **gebruikers kunnen apparaten toevoegen aan Azure AD**en selecteer vervolgens de groep beveiligde werk Station-gebruikers.

#### <a name="removal-of-local-admin-rights"></a>Verwijderen van lokale beheerders rechten

Deze methode vereist dat gebruikers van het VIP-, DevOps-en secure-level werk station geen beheerders rechten hebben op hun computers. Als u deze instelling wilt configureren vanuit de Azure Portal:

1. Ga naar de**Apparaatinstellingen**van **Azure Active Directory** > **apparaten** > .
1. Selecteer **geen** onder **aanvullende lokale beheerders op apparaten die zijn toegevoegd aan Azure AD**.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Multi-factor Authentication vereisen voor het toevoegen van apparaten

Om het proces van het toevoegen van apparaten aan Azure AD verder te verbeteren:

1. Ga naar de**Apparaatinstellingen**van **Azure Active Directory** > **apparaten** > .
1. Selecteer **Ja** onder **vereisen multi-factor Authentication om lid te worden van apparaten**.
1. Selecteer **Opslaan**.

#### <a name="configure-mdm"></a>MDM configureren

Van de Azure Portal:

1. Blader naar **Azure Active Directory** > **Mobility (MDM en mam)**  > **Microsoft intune**.
1. Wijzig de instelling voor het **MDM-gebruikers bereik** in **alle**.
1. Selecteer **Opslaan**.

Met deze stappen kunt u apparaten beheren met intune. Zie [voor meer informatie de Snelstartgids voor intune: Automatische inschrijving voor Windows 10-apparaten](https://docs.microsoft.com/Intune/quickstart-setup-auto-enrollment)instellen. In een toekomstige stap maakt u een intune-configuratie en nalevings beleid.

#### <a name="azure-ad-conditional-access"></a>Azure AD Conditional Access

Voorwaardelijke toegang van Azure AD kan helpen om geprivilegieerde beheer taken te beperken tot compatibele apparaten. Vooraf gedefinieerde leden van de groep **beveiligde werk Station-gebruikers** zijn vereist om multi-factor Authentication uit te voeren wanneer ze zich aanmelden bij Cloud toepassingen. Een best practice is het uitsluiten van toegangs accounts voor nood gevallen van het beleid. Zie [accounts voor nood toegang beheren in azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)voor meer informatie.

Voorwaardelijke toegang configureren op basis van de Azure Portal:

1. Ga naar **Azure Active Directory** > **nieuwe beleids regels**voor**voorwaardelijke toegang** > .
1. Voer
   * **Naam** : vereist beleid voor beveiligd apparaat
   * Toewijzingen
     * **Gebruikers en groepen**
       * Include- **users en groups** : Selecteer de groep **beveiligde werk Station-gebruikers** die u eerder hebt gemaakt.
       * Uitsluiten: **gebruikers en groepen** : Selecteer de accounts voor nood toegang van uw organisatie.
     * **Cloud-apps** : **alle Cloud-apps**toevoegen.
    * Besturingselementen voor toegang
      * **Grant** : Selecteer het keuze rondje **toegang verlenen** .
        * **Multi-factor Authentication vereisen**.
        * **Vereisen dat het apparaat wordt gemarkeerd als compatibel**.
        * Voor meerdere besturings elementen: **vereist alle geselecteerde besturings elementen**.
    * Schakel het beleid **in**.

U hebt de mogelijkheid om beleids regels te maken voor het blok keren van landen waar gebruikers geen toegang tot bedrijfs bronnen hebben. Zie [de voor waarde voor de locatie in azure Active Directory voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition)voor meer informatie over op IP-locatie gebaseerd beleid voor voorwaardelijke toegang.

## <a name="intune-configuration"></a>InTune-configuratie

### <a name="configure-enrollment-status"></a>Inschrijvings status configureren

Het is belang rijk om ervoor te zorgen dat uw beveiligde werk station een vertrouwd schoon apparaat is. Wanneer u nieuwe apparaten aanschaft, kunt u instellen dat de fabrieks instellingen [van Windows 10 Pro in de S-modus](https://docs.microsoft.com/Windows/deployment/Windows-10-pro-in-s-mode)worden ingesteld, waardoor de bloot stelling aan beveiligings problemen tijdens het beheer van de toeleverings keten wordt beperkt. Nadat u een apparaat van uw leverancier hebt ontvangen, kunt u auto pilot gebruiken om de modus van S te wijzigen. De volgende richt lijnen bevatten informatie over het Toep assen van het transformatie proces.

Om ervoor te zorgen dat apparaten volledig worden geconfigureerd voordat ze worden gebruikt, biedt intune een manier om het **apparaat te blok keren totdat alle apps en profielen zijn geïnstalleerd**.

Van de **Azure Portal**:
1. Ga naar **Microsoft intune** >  > **apparaatregistratie** > de standaardinstellingenvoordeinschrijvingvandeWindows-inschrijvingsstatuspagina. >  > 
1. Stel de voortgang van de installatie van het **app-profiel weer geven** in op **Ja**.
1. Het **gebruik van het blok apparaat instellen totdat alle apps en profielen** op **Ja**zijn geïnstalleerd.

### <a name="create-an-autopilot-deployment-profile"></a>Een auto pilot-implementatie profiel maken

Nadat u een apparaatgroep hebt gemaakt, moet u een implementatie profiel maken om de auto pilot-apparaten te configureren.

In intune in de Azure Portal:

1. Selecteer **registratie** > van**Windows** > -inschrijvings**profielen** > voor apparaatregistratie**maken**.
1. Voer
   * Naam: **profiel voor beveiligde werk station implementeren**.
   * Beschrijving: **implementatie van beveiligde werk stations**.
   * Stel **alle doel apparaten converteren naar auto pilot** in op **Ja**. Deze instelling zorgt ervoor dat alle apparaten in de lijst zijn geregistreerd bij de auto pilot Deployment-service. 48 uur toestaan dat de registratie wordt verwerkt.
1. Selecteer **Volgende**.
   * Voor **implementatie modus**kiest u **zelf implementeren (preview)** . Apparaten met dit profiel zijn gekoppeld aan de gebruiker die het apparaat inschrijft. Er zijn gebruikers referenties vereist om het apparaat in te schrijven. Het is belang rijk om te weten dat wanneer u een apparaat implementeert in de modus **zelf implementeren** , u laptops in een gedeeld model kunt implementeren. Er vindt geen gebruikers toewijzing plaats totdat het apparaat voor de eerste keer wordt toegewezen aan een gebruiker. Als gevolg hiervan worden alle gebruikers beleidsregels, zoals BitLocker, pas ingeschakeld als een gebruikers toewijzing is voltooid. Zie selected profiles (Engelstalig) voor meer informatie over het [](https://docs.microsoft.com/intune/device-profile-assign)aanmelden bij een beveiligd apparaat.
   * In het vak **toevoegen aan Azure AD** moet het **lid van Azure AD zijn opgenomen** en grijs worden weer gegeven.
   * Selecteer uw Langugage (regio), type **standaard**gebruikers account. 
1. Selecteer **Volgende**.
   * Selecteer een scope-tag als u deze vooraf hebt geconfigureerd.
1. Selecteer **Volgende**.
1. Kies **toewijzingen** > **toewijzen aan** > **geselecteerde groepen**. In **groepen selecteren die moeten worden toegevoegd**, kiest u **beveiligde werk Station-gebruikers**.
1. Selecteer **Volgende**.
1. Selecteer **Maken** om het profiel te maken. Het auto pilot-implementatie profiel is nu beschikbaar om aan apparaten toe te wijzen.

Registratie van apparaten in auto pilot biedt een andere gebruikers ervaring op basis van apparaattype en rol. In het voor beeld van de implementatie illustreren we een model waarin de beveiligde apparaten bulksgewijs worden geïmplementeerd en kunnen worden gedeeld, maar wanneer het apparaat voor de eerste keer wordt gebruikt, wordt het aan een gebruiker toegewezen. Zie intune auto [pilot Device Enrollment](https://docs.microsoft.com/intune/device-enrollment)(Engelstalig) voor meer informatie.

### <a name="configure-windows-update"></a>Windows Update configureren

Het up-to-date houden van Windows 10 is een van de belangrijkste dingen die u kunt doen. Als u Windows met een veilige status wilt behouden, implementeert u een update ring om het tempo te beheren dat updates op werk stations worden toegepast. 

In deze richt lijnen wordt geadviseerd om een nieuwe update-ring te maken en de volgende standaard instellingen te wijzigen:

In Azure Portal:

1. Ga naar **Microsoft intune** > -**software-updates** > **Windows 10-update ringen**.
1. Voer
   * Naam- **Azure Managed Workstation-updates**
   * Onderhouds kanaal- **Windows Insider-snel**
   * Uitstel van kwaliteits updates (dagen)- **3**
   * Uitstel periode voor onderdelen updates (dagen)- **3**
   * Gedrag van automatische updates- **automatisch installeren en opnieuw opstarten zonder besturings element voor eind gebruiker**
   * Blok keren dat de gebruiker Windows-updates onderbreekt- **blok keren**
   * Vereisen dat de gebruiker opnieuw wordt opgestart buiten de werk uren- **vereist**
   * Gebruiker mag opnieuw worden opgestart (opnieuw opgestart)- **vereist**
   * Overgangs gebruikers om opnieuw op te starten na het automatisch opnieuw opstarten (dagen)- **3**
   * Herinnering voor ingeschakelde opnieuw starten (dagen)- **3**
   * Deadline instellen voor opnieuw opstarten in behandeling (dagen)- **3**

1. Selecteer **Maken**.
1. Voeg op het tabblad **toewijzingen** de groep **beveiligde werk stations** toe.

Zie voor meer informatie over Windows Updates beleid [CSP-update](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-update).

### <a name="windows-defender-atp-intune-integration"></a>Integratie van Windows Defender ATP intune

Windows Defender ATP en Microsoft Intune samen werken om inbreuk op de beveiliging te voor komen. Ze kunnen ook de impact van inbreuken beperken. ATP-functies bieden realtime detectie van bedreigingen en het inschakelen van uitgebreide controle en logboek registratie van de eind punt apparaten.

Als u de integratie van Windows Defender ATP en intune wilt configureren, gaat u naar de Azure Portal.

1. Bladernaar **Microsoft intune** > apparaatcompatibiliteit > **Windows Defender ATP**.
1. Selecteer in stap 1 onder **Windows Defender ATP configureren** **de optie Windows Defender atp verbinden met Microsoft intune in Windows Defender Security Center**.
1. In Windows Defender Security Center:
   1. Selecteer **instellingen** > **geavanceerde functies**.
   1. Kies **aan** **Microsoft intune-verbinding**.
   1. Selecteer **voor keuren opslaan**.
1. Nadat een verbinding tot stand is gebracht, keert u terug naar intune en selecteert u bovenaan **vernieuwen** .
1. Stel **Connect Windows-apparaten versie 10.0.15063 en hoger in op aan Windows Defender ATP** **.**
1. Selecteer **Opslaan**.

Zie [Windows Defender Advanced Threat Protection](https://docs.microsoft.com/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection)(Engelstalig) voor meer informatie.

### <a name="finish-workstation-profile-hardening"></a>Beveiliging van het workstation profiel volt ooien

Down load en voer het juiste script uit om de beveiliging van de oplossing te volt ooien. Zoek de Download koppelingen voor het gewenste **profiel niveau**:

| Profiel | Downloadlocatie | Bestandsnaam |
| --- | --- | --- |
| Lage beveiliging | N/A |  N/A |
| Verbeterde beveiliging | https://aka.ms/securedworkstationgit | Uitgebreid-werk station-Windows10-(1809). ps1 |
| Hoge beveiliging  | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Gespecialiseerd | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline.ps1 |
| Gespecialiseerde naleving * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Beveiligd | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

\*Gespecialiseerde naleving is een script dat de gespecialiseerde configuratie afdwingt die is opgenomen in de NCSC Windows10 Security Baseline Baseline.

Nadat het script is uitgevoerd, kunt u in intune updates Toep assen op profielen en beleid. De scripts voor uitgebreide en veilige profielen maken beleids regels en profielen voor u, maar u moet het beleid toewijzen aan uw groep **beveiligde werk stations** .

* Hier vindt u de intune-configuratie profielen voor apparaten die zijn gemaakt door de scripts: **Azure Portal** > **Microsoft intune**configuratieprofielenvoorapparaten > . > 
* Hier vindt u het intune-nalevings beleid voor apparaten dat is gemaakt door de scripts: **Azure Portal** **Microsoft intune**nalevings > beleid voor apparaten. >  > 

Als u de wijzigingen wilt bekijken die door de scripts zijn aangebracht, kunt u de profielen exporteren. Op deze manier kunt u extra beveiliging bepalen die nodig is, zoals beschreven in de SECCON- [documentatie](https://docs.microsoft.com/windows/security/threat-protection/windows-security-configuration-framework/windows-security-configuration-framework).

Voer het intune-export script `DeviceConfiguration_Export.ps1` uit vanuit de [apparaatconfiguratie GiuHub-opslag plaats](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) om alle huidige intune-profielen te exporteren.

## <a name="additional-configurations-and-hardening-to-consider"></a>Aanvullende configuraties en beveiliging om rekening mee te houden

Door de volgende instructies te volgen, hebt u een beveiligd werk station geïmplementeerd. U moet echter ook rekening houden met extra besturings elementen. Bijvoorbeeld:

* toegang tot alternatieve browsers beperken
* uitgaande HTTP toestaan
* selecteren van websites blok keren
* machtigingen instellen voor het uitvoeren van aangepaste Power shell-scripts

### <a name="set-rules-in-the-firewall-configuration-service-provider-csp"></a>Regels instellen in de Firewall Configuration service provider (CSP)

U kunt wijzigingen aanbrengen in het beheer van zowel binnenkomende als uitgaande regels als nodig is voor uw toegestane en geblokkeerde eind punten. Als u doorgaat met de beveiliging van het beveiligde werk station, kunt u de beperking versoepelen waardoor al het binnenkomende en uitgaande verkeer wordt geweigerd. U kunt toegestane uitgaande sites toevoegen aan gemeen schappelijke en vertrouwde websites. Zie de [Firewall Configuration-service](https://docs.microsoft.com/Windows/client-management/mdm/firewall-csp)voor meer informatie.

Standaard beperkte aanbevelingen zijn:

* Alle inkomende verbindingen weigeren
* Alle uitgaande berichten weigeren

Het Spamhaus-project onderhoudt [de lijst met domein blokken (dubbele)](https://www.spamhaus.org/dbl/): een goede resource die moet worden gebruikt als uitgangs punt voor het blok keren van sites.

### <a name="manage-local-applications"></a>Lokale toepassingen beheren

Het beveiligde werk station wordt verplaatst naar een echt beveiligde status wanneer lokale toepassingen worden verwijderd, inclusief productiviteits toepassingen. Hier kunt u Chrome toevoegen als de standaard browser en intune gebruiken om de mogelijkheid van een gebruiker om de browser en de invoeg toepassingen te wijzigen te beperken.

#### <a name="deploy-applications-using-intune"></a>Toepassingen implementeren met intune

In sommige gevallen zijn toepassingen als de Google Chrome-browser vereist op het beveiligde werk station. Het volgende voor beeld bevat instructies voor het installeren van Chrome op apparaten in de beveiligings groep **beveiligde werk stations**.

1. Down load de Offline Installer [Chrome bundel voor Windows 64 bits](https://cloud.google.com/chrome-enterprise/browser/download/).
1. Pak de bestanden uit en noteer de locatie van het `GoogleChromeStandaloneEnterprise64.msi` bestand.
1. Ga in het **Azure Portal** naar **Microsoft intune** > **apps** > voor > client-apps**toevoegen**.
1. Kies onder **app-type**de optie **line-of-Business**.
1. Onder **app-pakket bestand**selecteert u `GoogleChromeStandaloneEnterprise64.msi` het bestand van de uitgepakte locatie en selecteert u **OK**.
1. Geef onder **app-gegevens**een beschrijving en een uitgever op. Selecteer **OK**.
1. Selecteer **Toevoegen**.
1. Selecteer op het tabblad **toewijzingen** de optie **beschikbaar voor Inge schreven apparaten** onder **toewijzings type**.
1. Voeg onder **opgenomen groepen**de groep **beveiligde werk stations** toe.
1. Selecteer **OK**en selecteer vervolgens **Opslaan**.

Zie voor meer informatie over het configureren van Chrome-instellingen [Chrome browser beheren met Microsoft intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>De bedrijfs portal configureren voor aangepaste apps

In een beveiligde modus is de installatie van de toepassing beperkt tot de intune-bedrijfs portal. Voor de installatie van de portal is echter toegang tot Microsoft Store vereist. In uw beveiligde oplossing kunt u de bedrijfs portal beschikbaar maken voor alle apparaten via een offline modus.

Een door intune beheerde kopie van de [bedrijfsportal](https://docs.microsoft.com/Intune/store-apps-company-portal-app) biedt u toegang op aanvraag tot extra hulpprogram ma's die u naar gebruikers van de beveiligde werk stations kunt pushen.

Mogelijk moet u Windows 32-bits Apps of andere apps installeren waarvan de implementatie speciale voor bereidingen vereist. In dergelijke gevallen kunt u `.intunewin` het [micro soft Win32 content prep-hulp programma](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) een kant-en-klaar indelings bestand bieden voor installatie.

### <a name="use-powershell-to-create-custom-settings"></a>Power shell gebruiken om aangepaste instellingen te maken

U kunt ook Power shell gebruiken om de mogelijkheden voor het beheer van hosts uit te breiden. Met dit voorbeeld script wordt een standaard achtergrond ingesteld op de host. Het is een mogelijkheid die ook beschikbaar is via de Azure Portal en profielen. Het voorbeeld script dient alleen ter illustratie van de Power shell-functionaliteit.

Mogelijk moet u bepaalde aangepaste besturings elementen en instellingen op uw beveiligde werk stations instellen. In dit voor beeld wordt de achtergrond van het werk station gewijzigd door gebruik te maken van de mogelijkheid van Power shell om het apparaat eenvoudig te identificeren als een geschikt, beveiligd werk station.

Met het script [SetDesktopBackground. ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) van micro soft scripting Center kan Windows deze [gratis, algemene achtergrond afbeelding](https://i.imgur.com/OAJ28zO.png) laden bij het opstarten.

1. Down load het script naar een lokaal apparaat.
1. Werk de customerXXXX en de download locatie van de achtergrond afbeelding bij. In ons voor beeld vervangen we customerXXXX naar achtergronden.  
1. Blader naar het **Azure Portal** > **Microsoft intune** >  > **Device configuration** > **Power shell-scripts** **toevoegen**.
1. Geef een **naam** op voor het script en geef de locatie van het **script**op.
1. Selecteer **Configureren**.
   1. Stel **Dit script uitvoeren met de aanmeldings referenties** in op **Ja**.
   1. Selecteer **OK**.
1. Selecteer **Maken**.
1. Selecteer **toewijzingen** > **groepen selecteren**.
   1. De beveiligings groep **beveiligen werk stations**toevoegen.
   1. Selecteer **Opslaan**.

## <a name="enroll-and-validate-your-first-device"></a>Uw eerste apparaat inschrijven en valideren

1. Als u uw apparaat wilt inschrijven, hebt u de volgende gegevens nodig:
   * **Serie nummer** : gevonden op het chassis van het apparaat.
   * **Windows-product-id** : gevonden onder **systeem** > **info** in het menu Windows-instellingen.
   * U kunt [Get-WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) uitvoeren om een CSV-hash-bestand op te halen met alle vereiste gegevens voor de registratie van het apparaat.
   
     Voer `Get-WindowsAutoPilotInfo – outputfile device1.csv` uit om de informatie uit te voeren als een CSV-bestand dat u in intune kunt importeren.

     > [!NOTE]
     > Voor het script zijn verhoogde rechten vereist. Deze wordt uitgevoerd als een externe hand tekening. Met `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` deze opdracht kan het script correct worden uitgevoerd.

   * U kunt deze informatie verzamelen door u aan te melden bij een apparaat met Windows 10 versie 1809 of hoger. De wederverkoper van uw hardware kan deze informatie ook verstrekken.
1. Ga in **het Azure Portal**naar **Microsoft intune** > **apparaatregistratie** > **Windows registratie** > **apparaten-Windows auto pilot-apparaten beheren**.
1. Selecteer **importeren** en kies uw CSV-bestand.
1. Voeg het apparaat toe aan de beveiligings groep **beveiligde werk stations** .
1. Op het Windows 10-apparaat dat u wilt configureren, gaat u naar **Windows-instellingen** > **Update & beveiligings** > **herstel**.
   1. Kies **aan de slag** onder **deze PC opnieuw instellen**.
   1. Volg de aanwijzingen om het apparaat opnieuw in te stellen en opnieuw te configureren met het profiel en nalevings beleid dat is geconfigureerd.

Nadat u het apparaat hebt geconfigureerd, voltooit u een controle en controleert u de configuratie. Controleer of het eerste apparaat juist is geconfigureerd voordat u doorgaat met de implementatie.

## <a name="assign-and-monitor"></a>Toewijzen en bewaken

Als u apparaten en gebruikers wilt toewijzen, moet u de [geselecteerde profielen](https://docs.microsoft.com/intune/device-profile-assign) toewijzen aan uw beveiligings groep. Alle nieuwe gebruikers die machtigingen nodig hebben voor de service, moeten ook worden toegevoegd aan de beveiligings groep.

U kunt profielen bewaken met intune- [profiel bewaking](https://docs.microsoft.com/intune/device-profile-monitor).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Microsoft intune](https://docs.microsoft.com/intune/index).
* Meer informatie over [Azure AD](https://docs.microsoft.com/azure/active-directory/index).

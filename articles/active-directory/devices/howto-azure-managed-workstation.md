---
title: Azure managed werkstations - Azure Active Directory implementeren
description: Informatie over het implementeren van veilige, beheerde Azure werkstations om het risico van inbreuk vanwege onjuiste configuratie of inbreuk op.
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
ms.openlocfilehash: 51d8bbc8b8be9679fbf024d7c51de53c430dc493
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67550492"
---
# <a name="deploy-a-secure-azure-managed-workstation"></a>Implementeer een veilige, beheerde Azure-werkstation

Nu dat u [begrijpen beveiligde werkstations](concept-azure-managed-workstation.md), is het tijd om te beginnen met het proces van implementatie. Met deze handleiding kunt u gedefinieerde profielen gebruiken om te maken van een werkstation dat is veiliger vanaf het begin.

![Implementatie van een beveiligd werkstation](./media/howto-azure-managed-workstation/deploying-secure-workstations.png)

U moet een profiel selecteren voordat u de oplossing kunt implementeren. U kunt tegelijkertijd meerdere profielen gebruiken in een implementatie en deze toewijzen aan tags of -groepen.
> [!NOTE]
> Een van de profielen zijn van toepassing als nodig is voor uw vereisten. U kunt verplaatsen naar een ander profiel in Intune toe te wijzen.

| Profiel | Laag | Verbeterd | Hoog | Gespecialiseerd | Beveiligd | Isolated |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Gebruiker in Azure AD | Ja | Ja | Ja | Ja | Ja | Ja |
| Intune worden beheerd | Ja | Ja | Ja | Ja | Ja | Ja |
| Apparaat - geregistreerd bij Azure AD | Ja |  |  |  |  | |   |
| Apparaat - toegevoegd aan Azure AD |   | Ja | Ja | Ja | Ja | Ja |
| Intune-basisbeveiliging is toegepast |   | Ja <br> (Uitgebreid) | Ja <br> (HighSecurity) | Ja <br> (NCSC) | Ja <br> (Beveiligd) |  N.v.t. |
| Hardware voldoet aan de beveiligde Windows 10-standaarden |   | Ja | Ja | Ja | Ja | Ja |
| Microsoft Defender ATP ingeschakeld |   | Ja  | Ja | Ja | Ja | Ja |
| Het verwijderen van beheerdersrechten |   |   | Ja  | Ja | Ja | Ja |
| Implementatie met behulp van Microsoft Autopilot |   |   | Ja  | Ja | Ja | Ja |
| Apps die zijn geïnstalleerd door Intune |   |   |   | Ja | Ja |Ja |
| Beperkt tot de goedgekeurde lijst URL 's |   |   |   | Ja | Ja |Ja |
| Internet geblokkeerd (Inkomend/uitgaand) |   |   |   |  |  |Ja |

## <a name="license-requirements"></a>Licentievereisten

De concepten behandeld in deze handleiding wordt ervan uitgegaan dat u hebt Microsoft 365 Enterprise E5 of een equivalente SKU. Sommige van de aanbevelingen in deze handleiding kan worden geïmplementeerd met een lagere SKU's. Zie voor meer informatie, [Microsoft 365 Enterprise-licentieverlening](https://www.microsoft.com/licensing/product-licensing/microsoft-365-enterprise).

Voor het automatiseren van licentie wordt ingericht, houd rekening met [Groepslicenties](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) voor uw gebruikers.

## <a name="azure-active-directory-configuration"></a>Azure Active Directory-configuratie

Azure Active Directory (Azure AD) beheert gebruikers, groepen en apparaten voor uw werkstations. U moet inschakelen identiteitsservices en -functies met een [administrator-account](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

Wanneer u het beveiligd werkstation administrator-account maakt, moet u het account naar uw huidige werkstation weergeven. Zorg ervoor dat u een bekende, veilige apparaat gebruiken voor deze eerste configuratie en alle globale configuratie. De aanval om blootstelling te beperken voor de eerste keer-ervaring, houd rekening met volgende de [richtlijnen om te voorkomen dat de malware-infecties](https://docs.microsoft.com/windows/security/threat-protection/intelligence/prevent-malware-infection).

U moet meervoudige verificatie, ook ten minste voor uw beheerders vereisen. Zie [cloud-gebaseerde MFA implementeren](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted) voor begeleiding bij implementatie.

### <a name="azure-ad-users-and-groups"></a>Azure AD-gebruikers en groepen

1. Vanuit de Azure-portal, blader naar **Azure Active Directory** > **gebruikers** > **nieuwe gebruiker**.
1. De apparaatbeheerder van uw te maken met de volgende stappen in de [maken van de gebruiker zelfstudie](https://docs.microsoft.com/Intune/quickstart-create-user).
1. Voer in:
   * **Naam** -werkstation beheerder beveiligen
   * **Naam van gebruiker** - `secure-ws-admin@identityitpro.com`
   * **Maprol** - **beperkte beheerder** en selecteer de **Intune-beheerder** rol.
1. Selecteer **Maken**.

Vervolgens maakt u twee groepen: gebruikers van werkstation en werkstation-apparaten.

Vanuit de Azure-portal, blader naar **Azure Active Directory** > **groepen** > **nieuwe groep**.

1. Voor de workstation-gebruikersgroep, kunt u configureren [Groepslicenties](https://docs.microsoft.com/azure/active-directory/users-groups-roles/licensing-groups-assign) voor het automatiseren van de inrichting van licenties aan gebruikers.
1. Voer voor de gebruikersgroep werkstation:
   * **Groepstype** -beveiliging
   * **Naam van groep** -gebruikers van beheerwerkstations beveiligen
   * **Lidmaatschapstype** - toegewezen
1. Uw beveiligde werkstation beheerdersgebruiker toevoegen: `secure-ws-admin@identityitpro.com`
1. U kunt andere gebruikers die u veilig werkstations beheren wilt toevoegen.
1. Selecteer **Maken**.

1. Voer voor de groep van de apparaten werkstation:
   * **Groepstype** -beveiliging
   * **Naam van groep** -werkstations beveiligen
   * **Lidmaatschapstype** - toegewezen
1. Selecteer **Maken**.

### <a name="azure-ad-device-configuration"></a>Azure AD-apparaatconfiguratie

#### <a name="specify-who-can-join-devices-to-azure-ad"></a>Geef die apparaten kunt toevoegen aan Azure AD

Uw apparaten instellen in Active Directory om toe te staan uw administratieve beveiligingsgroep apparaten toevoegen aan uw domein configureren. Deze instelling vanuit Azure portal configureren:

1. Ga naar **Azure Active Directory** > **apparaten** > **apparaatinstellingen**.
1. Kies **geselecteerde** onder **gebruikers kunnen apparaten koppelen aan Azure AD**, en selecteer vervolgens de groep 'Beveiligd werkstation gebruikers'.

#### <a name="removal-of-local-admin-rights"></a>Het verwijderen van lokale beheerdersrechten

Deze methode is vereist dat gebruikers van de VIP, DevOps en werkstations beveiligen op serverniveau geen beheerdersrechten op hun computers hebben. Deze instelling vanuit Azure portal configureren:

1. Ga naar **Azure Active Directory** > **apparaten** > **apparaatinstellingen**.
1. Selecteer **geen** onder **extra lokale beheerders op Azure AD gekoppelde apparaten**.

#### <a name="require-multi-factor-authentication-to-join-devices"></a>Meervoudige verificatie worden toegevoegd aan apparaten vereisen

Voor het verder versterking van het proces van het toevoegen van apparaten aan Azure AD:

1. Ga naar **Azure Active Directory** > **apparaten** > **apparaatinstellingen**.
1. Selecteer **Ja** onder **multi-factor Authentication vereisen om apparaten**.
1. Selecteer **Opslaan**.

#### <a name="configure-mdm"></a>MDM configureren

Vanuit de Azure-portal:

1. Blader naar **Azure Active Directory** > **Mobility (MDM en MAM)**  > **Microsoft Intune**.
1. Wijzig de **gebruikersbereik van MDM** instelt op **alle**.
1. Selecteer **Opslaan**.

Deze stappen kunt u voor het beheren van elk apparaat met Intune. Zie voor meer informatie, [Intune Quickstart: Instellen van automatische inschrijving voor Windows 10-apparaten](https://docs.microsoft.com/Intune/quickstart-setup-auto-enrollment). In een toekomstige stap maakt u beleid voor configuratie en nalevingsbeleid van Intune.

#### <a name="azure-ad-conditional-access"></a>Azure AD Conditional Access

Azure AD voorwaardelijke toegang kunt bevoegde administratieve taken beperken tot compatibele apparaten. Vooraf gedefinieerde leden van de **beveiligd werkstation gebruikers** groep zijn vereist om uit te voeren met meervoudige verificatie tijdens het aanmelden bij toepassingen in de cloud. Er is een best practice-accounts voor toegang in noodgevallen uitsluiten van het beleid. Zie voor meer informatie, [toegang in noodgevallen accounts beheren in Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access).

Voorwaardelijke toegang configureren via Azure portal:

1. Ga naar **Azure Active Directory** > **voorwaardelijke toegang** > **nieuw beleid**.
1. Voer in:
   * **Naam** -apparaat beveiligen beleid vereist
   * Toewijzingen
     * **Gebruikers en groepen**
       * Opnemen - **gebruikers en groepen** : Selecteer de **beveiligd werkstation gebruikers** groep die u eerder hebt gemaakt.
       * Uitsluiten - **gebruikers en groepen** -Selecteer van uw organisatie toegang in noodgevallen geldt.
     * **Cloud-apps** -opnemen **alle cloud-apps**.
    * Besturingselementen voor toegang
      * **Verleen** : Selecteer **toegang verlenen** keuzerondje.
        * **Meervoudige verificatie vereisen**.
        * **Vereisen dat het apparaat moet worden gemarkeerd als compatibel**.
        * Voor meerdere besturingselementen - **alle geselecteerde besturingselementen vereisen**.
    * Beleid - inschakelen **op**.

U hebt de optie voor het maken van beleidsregels die van landen waar gebruikers geen toegang bedrijfsbronnen tot zouden blokkeren. Zie voor meer informatie over IP-locatie op basis van beleid voor voorwaardelijke toegang, [met de locatievoorwaarde in Azure Active Directory voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition).

## <a name="intune-configuration"></a>Intune-configuratie

### <a name="configure-enrollment-status"></a>Status van inschrijving configureren

Het is belangrijk om ervoor te zorgen dat uw beveiligde werkstation een vertrouwd apparaat schoon is. Bij de aanschaf van nieuwe apparaten, kunt u eisen dat ze factory is ingesteld op [Windows 10 Pro in de modus S](https://docs.microsoft.com/Windows/deployment/Windows-10-pro-in-s-mode), die de blootstelling aan beveiligingsproblemen tijdens het beheer van de leveringsketen beperkt. Nadat u een apparaat van uw leverancier ontvangt, kunt u Autopilot kunt gebruiken om dit te wijzigen van de S-modus. De volgende richtlijnen bevat informatie over het toepassen van de transformatie-proces.

Om ervoor te zorgen dat apparaten volledig zijn geconfigureerd voor gebruik, Intune biedt een manier om **apparaat gebruik blokkeren totdat alle apps en -profielen zijn geïnstalleerd**.

Uit de **Azure-portal**:
1. Ga naar **Microsoft Intune** > **apparaatinschrijving** > **Windows-inschrijving** > **Status van inschrijving Pagina** > **standaard** > **instellingen**.
1. Stel **installatievoortgang voor app-profiel weergeven** naar **Ja**.
1. Stel **apparaat gebruik blokkeren totdat alle apps en -profielen zijn geïnstalleerd** naar **Ja**.

### <a name="create-an-autopilot-deployment-profile"></a>Een Windows Autopilot-implementatieprofiel maken

Na het maken van een groep apparaten, moet u een implementatieprofiel voor het configureren van het Autopilot-apparaten maken.

In Intune in Azure portal:

1. Selecteer **apparaatinschrijving** > **Windows-inschrijving** > **Implementatieprofielen** > **profiel maken** .
1. Voer in:
   * Naam van de - **beveiligd werkstation implementatieprofiel**.
   * Beschrijving - **implementatie van beveiligde werkstations**.
   * Stel **alle doelapparaten converteren naar Autopilot** naar **Ja**. Deze instelling zorgt ervoor dat alle apparaten in de lijst met de Autopilot deployment-service te registreren. Het kan 48 uur voor de registratie moeten worden verwerkt.
1. Selecteer **Next**.
   * Voor **implementatiemodus**, kiest u **zelf implementeren (Preview)** . Apparaten met dit profiel zijn gekoppeld aan de gebruiker die het apparaat wordt ingeschreven. Gebruikersreferenties zijn vereist om het apparaat te registreren.
   * De **toevoegen aan Azure AD als** box moet worden weergegeven **toegevoegd aan Azure AD** en worden lichter gekleurd weergegeven.
   * Selecteer uw Langugage (regio) type gebruikersaccount **standard**. 
1. Selecteer **Next**.
   * Selecteer een bereiktag als u een vooraf zijn geconfigureerd.
1. Selecteer **Next**.
1. Kies **toewijzingen** > **toewijzen aan** > **geselecteerde groepen**. In **Selecteer groepen om op te nemen**, kiest u **beveiligd werkstation gebruikers**.
1. Selecteer **Next**.
1. Selecteer **Maken** om het profiel te maken. Het Autopilot-implementatieprofiel is nu beschikbaar om toe te wijzen aan apparaten.

### <a name="configure-windows-update"></a>Windows Update configureren

Windows 10 actueel te houden, is een van de meest belangrijke dingen die u kunt doen. Voor het onderhouden van Windows in een beveiligde status, implementeert u een update-ring voor het beheren van het tempo dat updates worden toegepast op werkstations. 

Deze handleiding raadt aan dat u een nieuwe update-ring maken en wijzigen van de volgende standaardinstellingen:

In Azure Portal:

1. Ga naar **Microsoft Intune** > **Software-updates** > **Windows 10-Updateringen**.
1. Voer in:
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

1. Selecteer **Maken**.
1. Op de **toewijzingen** tabblad, voegt u de **werkstations beveiligen** groep.

Zie voor meer informatie over het beleid voor Windows Update, [beleid CSP - Update](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-update).

### <a name="windows-defender-atp-intune-integration"></a>Windows Defender ATP-Intune-integratie

Windows Defender ATP- en Microsoft Intune werken samen om beveiligingslekken te voorkomen. Ze kunnen ook beperkt de gevolgen van schendingen. ATP-mogelijkheden bieden realtime bedreigingsdetectie, evenals uitgebreide controle en logboekregistratie van de eindpunt-apparaten inschakelen.

Ga naar de Azure-portal voor het configureren van de integratie van Windows Defender ATP en Intune.

1. Blader naar **Microsoft Intune** > **apparaatcompatibiliteit** > **Windows Defender ATP**.
1. In stap 1 onder **configureren van Windows Defender ATP**, selecteer **verbinding maken met Windows Defender ATP aan Microsoft Intune in de Windows Defender-Beveiligingscentrum**.
1. In de Windows Defender-Beveiligingscentrum:
   1. Selecteer **instellingen** > **geavanceerde functies**.
   1. Voor **Microsoft Intune verbinding**, kiest u **op**.
   1. Selecteer **voorkeuren opslaan**.
1. Nadat een verbinding tot stand is gebracht, keert u terug naar Intune en selecteer **vernieuwen** aan de bovenkant.
1. Stel **verbinding maken met Windows-apparaten versie 10.0.15063 en hoger naar Windows Defender ATP** naar **op**.
1. Selecteer **Opslaan**.

Zie voor meer informatie, [Windows Defender Advanced Threat Protection](https://docs.microsoft.com/Windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection).

### <a name="finish-workstation-profile-hardening"></a>Werkstation profiel verharding voltooien

Voor de beveiliging van de oplossing, downloaden en uitvoeren van het juiste script. Zoeken naar de downloadkoppelingen voor uw gewenste **profiel niveau**:

| Profiel | Downloadlocatie | Bestandsnaam |
| --- | --- | --- |
| Lage beveiliging | N/A |  N/A |
| Verbeterde beveiliging | https://aka.ms/securedworkstationgit | Enhanced-Workstation-Windows10-(1809).ps1 |
| Hoge beveiliging  | https://aka.ms/securedworkstationgit | HighSecurityWorkstation-Windows10-(1809).ps1 |
| Gespecialiseerd | https://github.com/pelarsen/IntunePowerShellAutomation | DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline.ps1 |
| Gespecialiseerde naleving * | https://aka.ms/securedworkstationgit | DeviceCompliance_NCSC-Windows10(1803).ps1 |
| Beveiligd | https://aka.ms/securedworkstationgit | Secure-Workstation-Windows10-(1809)-SecurityBaseline.ps1 |

\* Gespecialiseerde naleving is een script dat de speciale configuratie in de NCSC Windows10 SecurityBaseline wordt afgedwongen.

Nadat het script met succes wordt uitgevoerd, kunt u updates voor profielen en -beleid in Intune. De scripts voor profielen uitgebreid en veilig voor u beleidsregels en profielen maken, maar moet u het beleid aan uw **werkstations beveiligen** groep.

* Dit is waar u de Intune-apparaatconfiguratieprofielen die zijn gemaakt door de scripts kunt vinden: **Azure-portal** > **Microsoft Intune** > **apparaatconfiguratie** > **profielen**.
* Hier vindt u de Intune-apparaat nalevingsbeleid die zijn gemaakt door de scripts: **Azure-portal** > **Microsoft Intune** > **apparaatcompatibiliteit** > **beleid**.

Als u wilt controleren op wijzigingen door de scripts, kunt u de profielen exporteren. Op deze manier kunt u aanvullende beveiliging die nodig zijn zoals wordt beschreven in de documentatie van SECCON bepalen.

Voer het script Intune gegevens exporteren uit `DeviceConfiguration_Export.ps1` uit de [DeviceConfiguration GiuHub opslagplaats](https://github.com/microsoftgraph/powershell-intune-samples/tree/master/DeviceConfiguration) voor het exporteren van alle huidige Intune-profielen.

## <a name="additional-configurations-and-hardening-to-consider"></a>Aanvullende configuraties en beveiliging te houden

Door hier de richtlijnen te volgen, hebt u een beveiligd werkstation geïmplementeerd. Echter ook rekening met extra besturingselementen. Bijvoorbeeld:

* Beperk de toegang tot andere browsers
* uitgaande HTTP toestaan
* Selecteer websites blokkeren
* machtigingen instellen voor het uitvoeren van aangepaste PowerShell-scripts

### <a name="set-rules-in-the-firewall-configuration-service-provider-csp"></a>Regels instellen in de firewall configuration serviceprovider (CSP)

Indien nodig voor uw eindpunten voor toegestane en geblokkeerde, kunt u aanvullende wijzigingen aan het beheer van zowel binnenkomende als uitgaande regels. Als u doorgaat met de beveiligde werkstation beperken, kunt u de beperking die alle inkomende en uitgaande verkeer weigert versoepelen. U kunt toegestane uitgaande sites als u wilt opnemen, algemene en vertrouwde websites kunt toevoegen. Zie voor meer informatie, [Firewall configuration-service](https://docs.microsoft.com/Windows/client-management/mdm/firewall-csp).

Standaard beperkt aanbevelingen zijn:

* Alle binnenkomend verkeer weigeren
* Al het uitgaande weigeren

Het Spamhaus Project onderhoudt [het domein blok lijst (dubbele)](https://www.spamhaus.org/dbl/): een goede resource moet worden gebruikt als een beginpunt voor het blokkeren van sites.

### <a name="manage-local-applications"></a>Beheren van lokale toepassingen

Het beveiligde werkstation wordt verplaatst naar een echt beperkte status wanneer lokale toepassingen zijn verwijderd, met inbegrip van productiviteitstoepassingen. Hier kunt u Chrome toevoegen als de standaardbrowser en Intune gebruiken voor het beperken van een gebruiker de mogelijkheid om de browser en de invoegtoepassingen te wijzigen.

#### <a name="deploy-applications-using-intune"></a>Implementatie van toepassingen met behulp van Intune

In sommige gevallen, toepassingen, zoals de Google Chrome-browser vereist voor het beveiligd werkstation. Het volgende voorbeeld vindt u instructies voor het installeren van Chrome op apparaten in de beveiligingsgroep **werkstations beveiligen**.

1. De offline-installatieprogramma downloaden [Chrome-pakket voor Windows 64‑bit](https://cloud.google.com/chrome-enterprise/browser/download/).
1. Pak de bestanden en noteer de locatie van de `GoogleChromeStandaloneEnterprise64.msi` bestand.
1. In de **Azure-portal** bladert u naar **Microsoft Intune** > **Client-apps** > **Apps**  >  **Toevoegen**.
1. Onder **App-type**, kiest u **Line-of-business**.
1. Onder **App-pakketbestand**, selecteer de `GoogleChromeStandaloneEnterprise64.msi` vanaf de locatie van het uitgepakte bestand en selecteer **OK**.
1. Onder **App-gegevens**, Geef een beschrijving en een uitgever. Selecteer **OK**.
1. Selecteer **Toevoegen**.
1. Op de **toewijzingen** tabblad **beschikbaar voor geregistreerde apparaten** onder **toewijzingstype**.
1. Onder **opgenomen groepen**, voeg de **werkstations beveiligen** groep.
1. Selecteer **OK**, en selecteer vervolgens **opslaan**.

Zie voor meer informatie over het configureren van instellingen voor Chrome [Chrome-Browser beheren met Microsoft Intune](https://support.google.com/chrome/a/answer/9102677).

#### <a name="configuring-the-company-portal-for-custom-apps"></a>Configureren van de bedrijfsportal-App voor aangepaste apps

De installatie van toepassing is in een beveiligde modus, beperkt tot de Intune-bedrijfsportal-App. Installatie van de portal vereist echter toegang tot Microsoft Store. In uw beveiligde oplossing, kunt u de bedrijfsportal-App beschikbaar voor alle apparaten via een offlinemodus.

Een Intune-beheerde exemplaar van de [bedrijfsportal](https://docs.microsoft.com/Intune/store-apps-company-portal-app) geeft u op aanvraag toegang tot extra hulpprogramma's die u kunt beneden wordt geduwd naar gebruikers van de beveiligde werkstations.

U moet mogelijk apps voor Windows 32-bits of andere apps waarvoor implementatie vereist speciale voorbereidingen te installeren. In dergelijke gevallen de [Microsoft win32 inhoud prep tool](https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool) krijgt u een kant-en-klare `.intunewin` format-bestand voor installatie.

### <a name="use-powershell-to-create-custom-settings"></a>PowerShell gebruiken voor het maken van aangepaste instellingen

U kunt ook PowerShell gebruiken om uit te breiden host beheermogelijkheden. Dit voorbeeldscript stelt u de achtergrondkleur van een standaard op de host. Het is een functie die is ook beschikbaar via de Azure-portal en -profielen. Het voorbeeldscript dient uitsluitend ter illustratie van de functionaliteit van PowerShell.

U moet mogelijk voor het instellen van bepaalde aangepaste besturingselementen en de instellingen op de beveiligde werkstations. In het volgende voorbeeld wordt de achtergrond van het werkstation met behulp van Powershell-mogelijkheid zodat het apparaat als een kant-en-klare, veilige werkstation eenvoudig kunt identificeren.

De [SetDesktopBackground.ps1](https://gallery.technet.microsoft.com/scriptcenter/Set-Desktop-Image-using-5430c9fb/) script van de Microsoft Scripting Center kan Windows laden dit [gratis, algemene achtergrondafbeelding](https://i.imgur.com/OAJ28zO.png) op starten.

1. Download het script naar een lokaal apparaat.
1. Werk de customerXXXX en de locatie van de achtergrondafbeelding. In dit voorbeeld vervangen we customerXXXX bij achtergronden.  
1. Blader naar de **Azure-portal** > **Microsoft Intune** > **apparaatconfiguratie** > **PowerShell scripts** > **toevoegen**.
1. Geef een **naam** voor het script en geef de **scriptlocatie**.
1. Selecteer **Configureren**.
   1. Stel **Voer dit script met de aangemelde referenties** naar **Ja**.
   1. Selecteer **OK**.
1. Selecteer **Maken**.
1. Selecteer **toewijzingen** > **groepen selecteren**.
   1. Voeg de beveiligingsgroep toe **werkstations beveiligen**.
   1. Selecteer **Opslaan**.

## <a name="enroll-and-validate-your-first-device"></a>Registreren en uw eerste apparaat valideren

1. Als u wilt uw apparaat registreert, moet u de volgende informatie:
   * **Serienummer** - gevonden op het apparaat chassis.
   * **Windows-Product-ID** - gevonden onder **System** > **over** in het menu instellingen van Windows.
   * U kunt uitvoeren [Get-WindowsAutoPilotInfo](https://aka.ms/Autopilotshell) om op te halen van een hash-CSV-bestand met alle benodigde informatie voor apparaatinschrijving.
   
     Voer `Get-WindowsAutoPilotInfo – outputfile device1.csv` om uit te voeren van de gegevens als een CSV-bestand dat u intune importeren kunt.

     > [!NOTE]
     > Het script vereist verhoogde rechten. Deze wordt uitgevoerd als extern ondertekend. De `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned` opdracht kunt u het script correct wordt uitgevoerd.

   * U kunt deze informatie verzamelen met het aanmelden bij een Windows 10-versie 1809-apparaat of hoger. De verkoper van uw hardware kunt ook deze informatie verstrekken.
1. In de **Azure-portal**, gaat u naar **Microsoft Intune** > **apparaatinschrijving** > **Windows-inschrijving**  >  **Apparaten - beheren Windows Autopilot-apparaten**.
1. Selecteer **importeren** en kies uw CSV-bestand.
1. Het apparaat toevoegt aan de **werkstations beveiligen** beveiligingsgroep.
1. Op de Windows 10-apparaat u wilt configureren, gaat u naar **Windows-instellingen** > **bijwerken en beveiliging** > **Recovery**.
   1. Kies **aan de slag** onder **opnieuw instellen van deze PC**.
   1. Volg de aanwijzingen voor het opnieuw instellen en configureren van het apparaat met de naleving van profielen en beleidsregels die zijn geconfigureerd.

Nadat u het apparaat hebt geconfigureerd, een beoordeling uitvoeren en controleer de configuratie. Bevestig dat het eerste apparaat correct is geconfigureerd voordat u doorgaat uw implementatie.

## <a name="assign-and-monitor"></a>Toewijzen en bewaken

Om toe te wijzen aan apparaten en gebruikers, die u nodig hebt om toe te wijzen de [geselecteerd profielen](https://docs.microsoft.com/intune/device-profile-assign) aan de beveiligingsgroep. Alle nieuwe gebruikers die behoefte hebben aan machtigingen voor de service moeten worden toegevoegd aan de beveiligingsgroep ook.

U kunt profielen met bewaken [bewaking van de Intune-profiel](https://docs.microsoft.com/intune/device-profile-monitor).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Microsoft Intune](https://docs.microsoft.com/intune/index).
* Inzicht in [Azure AD](https://docs.microsoft.com/azure/active-directory/index).

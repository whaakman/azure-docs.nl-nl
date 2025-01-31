---
title: De Azure AD Connect Health-agent installeren | Microsoft Docs
description: Dit is de Azure AD Connect Health-pagina waarop wordt beschreven hoe u de agent voor AD FS en Sync installeert.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
editor: curtand
ms.assetid: 1cc8ae90-607d-4925-9c30-6770a4bd1b4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2017
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c3d1a8afdbad1878f4ce134edeeb95dad79e98a1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65784820"
---
# <a name="azure-ad-connect-health-agent-installation"></a>De Azure AD Connect Health-agent installeren

In dit document wordt beschreven hoe u de Azure AD Connect Health-agents installeert en configureert. U kunt de agents [hier](how-to-connect-install-roadmap.md#download-and-install-azure-ad-connect-health-agent) downloaden.

## <a name="requirements"></a>Vereisten

In de volgende tabel ziet u een lijst vereisten voor het gebruik van Azure AD Connect Health.

| Vereiste | Description |
| --- | --- |
| Azure AD Premium |Azure AD Connect Health is een Azure AD Premium-functie waarvoor Azure AD Premium is vereist. <br /><br />Zie voor meer informatie [Aan de slag met Azure AD Premium](../fundamentals/active-directory-get-started-premium.md) <br />Als u gebruik wilt maken van een gratis proefversie van 30 dagen, raadpleegt u [Gratis proberen](https://azure.microsoft.com/trial/get-started-active-directory/). |
| U moet hoofdbeheerder van uw Azure AD zijn om aan de slag te gaan met Azure AD Connect Health |Standaard kunnen alleen hoofdbeheerders de Health-agents installeren en configureren om aan de slag te gaan, de Portal openen en bewerkingen uitvoeren in Azure AD Connect Health. Voor meer informatie raadpleegt u [Uw Azure AD-directory beheren](../fundamentals/active-directory-administer.md). <br /><br /> Met behulp van op rollen gebaseerd toegangsbeheer kunt u toegang tot Azure AD Connect Health verlenen aan andere gebruikers binnen uw organisatie. Zie voor meer informatie [Op rollen gebaseerd toegangsbeheer voor Azure AD Connect Health](how-to-connect-health-operations.md#manage-access-with-role-based-access-control). <br /><br />**Belangrijk:** Het account dat u gebruikt tijdens het installeren van agents, moet een werk- of schoolaccount zijn. Het mag geen Microsoft-account zijn. Voor meer informatie raadpleegt u [Als organisatie registreren voor Azure](../fundamentals/sign-up-organization.md) |
| De Azure AD Connect Health-agent wordt geïnstalleerd op elke doelserver | Voor Azure AD Connect Health moeten de Health-agents worden geïnstalleerd en geconfigureerd op de doelservers om gegevens te kunnen ontvangen en om mogelijkheden voor bewaking en analyse te kunnen bieden. <br /><br />Als u bijvoorbeeld gegevens uit uw AD FS-infrastructuur wilt ophalen, moet de agent worden geïnstalleerd op de AD FS-servers en de webtoepassingsproxyservers. Evenzo moet de agent op de domeincontrollers worden geïnstalleerd om gegevens in uw on-premises AD DS-infrastructuur te krijgen. <br /><br /> |
| Uitgaande verbinding met de Azure-service-eindpunten | Tijdens de installatie en runtime moet de agent verbonden zijn met de Azure AD Connect Health-service-eindpunten. Als de uitgaande connectiviteit is geblokkeerd met firewalls, moet u ervoor zorgen dat de volgende eindpunten wel zijn toegestaan. Meer informatie over [uitgaande connectiviteitseindpunten](how-to-connect-health-agent-install.md#outbound-connectivity-to-the-azure-service-endpoints) |
|Uitgaande verbindingen op basis van IP-adressen | Als u meer wilt weten over het filteren op IP-adressen in firewalls, ziet u [Azure IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653).|
| SSL-controle voor uitgaand verkeer is gefilterd of uitgeschakeld | De agentregistratiestap of het uploaden van gegevens kan mislukken als er in de netwerklaag een SSL-inspectie of blokkering is voor uitgaand verkeer. Meer informatie over het [instellen van SSL-inspectie](https://technet.microsoft.com/library/ee796230.aspx) |
| Firewallpoorten op de server waarop de agent wordt uitgevoerd |De volgende firewallpoorten moeten open zijn, anders kan de agent niet communiceren met de eindpunten van de Azure AD Health-service.<br /><br /><li>TCP-poort 443</li><li>TCP-poort 5671</li> <br />Houd er rekening mee dat poort 5671 niet langer vereist voor de meest recente versie van agent is. Upgrade uitvoeren naar de nieuwste versie, zodat alleen poort 443 vereist is. Lees meer over [firewallpoorten inschakelen](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
| Sta de volgende websites toe als verbeterde beveiliging van Internet Explorer is ingeschakeld |Als verbeterde beveiliging van Internet Explorer is ingeschakeld, moeten de volgende websites worden toegestaan op de server waarop u de agent wilt installeren.<br /><br /><li>https:\//login.microsoftonline.com</li><li>https:\//secure.aadcdn.microsoftonline-p.com</li><li>https:\//login.windows.net</li><li>De federatieve server van uw organisatie moet worden vertrouwd door Azure Active Directory. Bijvoorbeeld: https:\//sts.contoso.com</li> Lees meer over [Internet Explorer configureren](https://support.microsoft.com/help/815141/internet-explorer-enhanced-security-configuration-changes-the-browsing) |
| Zorg ervoor dat PowerShell v4.0 of later is geïnstalleerd | <li>Windows Server 2008 R2 wordt geleverd met PowerShell versie 2.0, wat onvoldoende is voor de agent. Werk PowerShell bij zoals wordt uitgelegd in [Agent-installatie op Windows Server 2008 R2-servers](#agent-installation-on-windows-server-2008-r2-servers).</li><li>Windows Server 2012 wordt geleverd met PowerShell v3.0, wat onvoldoende is voor de agent.  [Update](https://www.microsoft.com/download/details.aspx?id=40855) het Windows Management Framework.</li><li>Windows Server 2012 R2 en later wordt geleverd met een voldoende recente versie van PowerShell.</li>|
|FIPS uitschakelen|FIPS wordt niet ondersteund door Azure AD Connect Health-agents.|

### <a name="outbound-connectivity-to-the-azure-service-endpoints"></a>Uitgaande verbinding met de Azure-service-eindpunten

 Tijdens de installatie en runtime moet de agent verbonden zijn met de Azure AD Connect Health-service-eindpunten. Als u uitgaande connectiviteit is geblokkeerd met Firewalls, zorg er dan voor dat de volgende URL's worden niet standaard geblokkeerd. Beveiligingsbewaking of controle van deze URL's niet uit te schakelen, maar kunnen ze net als andere internetverkeer. Ze staan communicatie met Azure AD Connect Health-service-eindpunten. Meer informatie over het [uitgaande connectiviteit met Test-AzureADConnectHealthConnectivity controleren](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-agent-install#test-connectivity-to-azure-ad-connect-health-service).

| Domeinomgeving | Vereiste Azure-service-eindpunten |
| --- | --- |
| Algemeen publiek | <li>&#42;.blob.core.windows.net </li><li>&#42;.aadconnecthealth.azure.com </li><li>&#42;.servicebus.windows.net - Poort: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https:\//management.azure.com </li><li>https:\//policykeyservice.dc.ad.msft.net/</li><li>https:\//login.windows.net</li><li>https:\//login.microsoftonline.com</li><li>https:\//secure.aadcdn.microsoftonline-p.com </li><li>https:\//www.office.com *Dit eindpunt wordt alleen gebruikt voor detectiedoeleinden tijdens de registratie.</li> |
| Azure Duitsland | <li>&#42;.blob.core.cloudapi.de </li><li>&#42;.servicebus.cloudapi.de </li> <li>&#42;.aadconnecthealth.microsoftazure.de </li><li>https:\//management.microsoftazure.de </li><li>https:\//policykeyservice.aadcdi.microsoftazure.de </li><li>https:\//login.microsoftonline.de </li><li>https:\//secure.aadcdn.microsoftonline-p.de </li><li>https:\//www.office.de *Dit eindpunt wordt alleen gebruikt voor detectiedoeleinden tijdens de registratie.</li> |
| Azure Government | <li>&#42;.blob.core.usgovcloudapi.net </li> <li>&#42;.servicebus.usgovcloudapi.net </li> <li>&#42;.aadconnecthealth.microsoftazure.us </li> <li>https:\//management.usgovcloudapi.net </li><li>https:\//policykeyservice.aadcdi.azure.us </li><li>https:\//login.microsoftonline.us </li><li>https:\//secure.aadcdn.microsoftonline-p.com </li><li>https:\//www.office.com *Dit eindpunt wordt alleen gebruikt voor detectiedoeleinden tijdens de registratie.</li> |


## <a name="download-and-install-the-azure-ad-connect-health-agent"></a>De Azure AD Connect Health-agent downloaden en installeren

* Zorg dat u [voldoet aan de vereisten](how-to-connect-health-agent-install.md#requirements) voor Azure AD Connect Health.
* Aan de slag met Azure AD Connect Health voor AD FS
    * [Download de Azure AD Connect Health-agent voor AD FS.](https://go.microsoft.com/fwlink/?LinkID=518973)
    * [Zie de installatie-instructies](#installing-the-azure-ad-connect-health-agent-for-ad-fs).
* Aan de slag met Azure AD Connect Health for Sync
    * [Download en installeer de nieuwste versie van Azure AD Connect](https://go.microsoft.com/fwlink/?linkid=615771). De Health for Sync-agent wordt geïnstalleerd als onderdeel van de Azure AD Connect-installatie (versie 1.0.9125.0 of hoger).
* Aan de slag met Azure AD Connect Health voor AD DS
    * [Download de Azure AD Connect Health-agent voor AD DS](https://go.microsoft.com/fwlink/?LinkID=820540).
    * [Zie de installatie-instructies](#installing-the-azure-ad-connect-health-agent-for-ad-ds).

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>De Azure AD Connect Health-agent voor AD FS installeren

> [!NOTE]
> De AD FS-server moet verschillen van de synchronisatieserver. Installeer AD FS-agent niet op uw synchronisatieserver.
>

Zorg ervoor dat vóór de installatie de hostnaam van uw AD FS-server uniek is en niet aanwezig is in de AD FS-service.
Dubbelklik op het .exe-bestand dat u hebt gedownload om de installatie van de agent te starten. Klik in het eerste scherm op Installeren.

![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/install1.png)

Wanneer de installatie is voltooid, klikt u op Nu configureren.

![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/install2.png)

Hiermee wordt een PowerShell-venster geopend om het agentregistratieproces te starten. Wanneer het wordt gevraagd, meldt u zich aan met een Azure AD-account dat de mogelijkheid biedt om agents te registreren. Met het globale-beheerdersaccount is dat standaard mogelijk.

![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/install3.png)

Na het aanmelden gaat PowerShell door. Zodra het proces is voltooid, kunt u PowerShell sluiten. De configuratie is dan voltooid.

De agentservices moeten nu automatisch worden gestart, zodat de agent de vereiste gegevens veilig kan uploaden naar de cloudservice.

In het PowerShell-venster worden waarschuwingen weergegeven als u niet aan alle vereisten hebt voldaan die in vorige gedeelten zijn beschreven. Zorg ervoor dat u aan alle [vereisten](how-to-connect-health-agent-install.md#requirements) voldoet voordat u de agent installeert. In de schermafbeelding hieronder ziet u een voorbeeld van deze fouten.

![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/install4.png)

Als u wilt controleren of de agent is geïnstalleerd, zoekt u de volgende services op de server. Als de configuratie is voltooid, moeten deze services worden uitgevoerd. Ze worden pas gestart wanneer de configuratie is voltooid.

* Azure AD Connect Health AD FS Diagnostics-service
* Azure AD Connect Health AD FS Insights-service
* Azure AD Connect Health AD FS Monitoring-service

![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/install5.png)

### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>De agent installeren op Windows Server 2008 R2-servers

Stappen voor Windows Server 2008 R2-servers:

1. Zorg ervoor dat de server met Service Pack 1 of hoger wordt uitgevoerd.
2. Verbeterde beveiliging van Internet Explorer uitschakelen voor de installatie van agents:
3. Installeer Windows PowerShell 4.0 op alle servers voordat u de AD Health-agent installeert. Windows PowerShell 4.0 installeren:
   * Installeer [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) via de volgende koppeling. U kunt hier het offline-installatieprogramma downloaden.
   * PowerShell ISE (van Windows-onderdelen) installeren
   * Installeer [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855).
   * Installeer Internet Explorer versie 10 of hoger op de server. (Dit is vereist voor de Health Service, zodat kan worden geverifieerd of u Azure-beheerdersreferenties gebruikt.)
4. Voor meer informatie over het installeren van Windows PowerShell 4.0 voor Windows Server 2008 R2 raadpleegt u [dit](https://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx) wiki-artikel.

### <a name="enable-auditing-for-ad-fs"></a>Controle inschakelen voor AD FS

> [!NOTE]
> Deze sectie is alleen van toepassing op AD FS-servers. U hoeft niet te deze stappen volgen voor webtoepassingsproxyservers.
>

Als u de functie Gebruiksanalyse wilt gebruiken om gegevens te verzamelen en te analyseren, moet de Azure AD Connect Health-agent beschikken over de informatie in de AD FS-auditlogboeken. Deze logboeken worden niet standaard ingeschakeld. Volg de volgende procedures om AD FS-controles in te schakelen en de AD FS-auditlogboeken op uw AD FS-servers te zoeken.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2008-r2"></a>Controle inschakelen voor AD FS in Windows Server 2008 R2

1. Klik op **Start**, ga naar **Programma's**, ga naar **Systeembeheer** en klik vervolgens op **Lokaal beveiligingsbeleid**.
2. Navigeer naar de map **Beveiligingsinstellingen\Lokaal beleid\Gebruikersrechten toewijzen** en dubbelklik op **Beveiligingscontrole genereren**.
3. Controleer op het tabblad **Lokale beveiligingsinstelling** of het AD FS 2.0-serviceaccount wordt vermeld. Als dit niet zo is, klikt u op **Gebruiker of groep toevoegen**, voegt u het account toe aan de lijst en klikt u op **OK**.
4. Als u de controle wilt inschakelen, opent u een opdrachtprompt met verhoogde bevoegdheden en voert u de volgende opdracht uit: <code>auditpol.exe /set /subcategory:{0CCE9222-69AE-11D9-BED3-505054503030} /failure:enable /success:enable</code>
5. Kies **Lokaal beveiligingsbeleid**.
<br />   -- **De volgende stappen zijn alleen vereist voor primaire AD FS-servers.** -- <br />
6. Open de module **AD FS-beheer**. Klik op **Start**, wijs naar **Programma's**, wijs naar **Systeembeheer** en klik vervolgens op **AD FS 2.0-beheer** om de module AD FS-beheer te openen.
7. Klik in het deelvenster **Acties** op **Federation Service-eigenschappen** bewerken.
8. Klik in het dialoogvenster **Federation Service-eigenschappen** op het tabblad **Gebeurtenissen**.
9. Schakel de selectievakjes bij **Succesvolle controles** en **Mislukte controles** in.
10. Klik op **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Controle inschakelen voor AD FS in Windows Server 2012 R2

1. Open **Lokaal beveiligingsbeleid**. Dit doet u door naar **Serverbeheer** te gaan op het startscherm, of door Serverbeheer te openen via de taakbalk op het bureaublad en door vervolgens op **Extra/Lokaal beveiligingsbeleid** te klikken.
2. Navigeer naar de map **Beveiligingsinstellingen\Lokaal beleid\Gebruikersrechten toewijzen** en dubbelklik op **Beveiligingscontrole genereren**.
3. Controleer op het tabblad **Lokale beveiligingsinstelling** of het AD FS-serviceaccount wordt vermeld. Als dit niet zo is, klikt u op **Gebruiker of groep toevoegen**, voegt u het account toe aan de lijst en klikt u op **OK**.
4. Als u de controle wilt inschakelen, opent u een opdrachtprompt met verhoogde bevoegdheden en voert u de volgende opdracht uit: ```auditpol.exe /set /subcategory:{0CCE9222-69AE-11D9-BED3-505054503030} /failure:enable /success:enable```
5. Kies **Lokaal beveiligingsbeleid**.
<br />   -- **De volgende stappen zijn alleen vereist voor primaire AD FS-servers.** -- <br />
6. Open de module **AD FS-beheer** (klik in Serverbeheer op Extra en selecteer vervolgens AD FS-beheer).
7. Klik in het deelvenster **Acties** op **Federation Service-eigenschappen** bewerken.
8. Klik in het dialoogvenster **Federation Service-eigenschappen** op het tabblad **Gebeurtenissen**.
9. Schakel de selectievakjes bij **Succesvolle controles en Mislukte controles** in en klik op **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2016"></a>Controle inschakelen voor AD FS in Windows Server 2016

1. Open **Lokaal beveiligingsbeleid**. Dit doet u door naar **Serverbeheer** te gaan op het startscherm, of door Serverbeheer te openen via de taakbalk op het bureaublad en door vervolgens op **Extra/Lokaal beveiligingsbeleid** te klikken.
2. Navigeer naar de map **Beveiligingsinstellingen\Lokaal beleid\Gebruikersrechten toewijzen** en dubbelklik op **Beveiligingscontrole genereren**.
3. Controleer op het tabblad **Lokale beveiligingsinstelling** of het AD FS-serviceaccount wordt vermeld. Als dit niet zo is, klikt u op **Gebruiker of groep toevoegen**, voegt u het AD FS-serviceaccount toe aan de lijst en klikt u op **OK**.
4. Als u de controle wilt inschakelen, opent u een opdrachtprompt met verhoogde bevoegdheden en voert u de volgende opdracht uit: <code>auditpol.exe /set /subcategory:{0CCE9222-69AE-11D9-BED3-505054503030} /failure:enable /success:enable</code>
5. Kies **Lokaal beveiligingsbeleid**.
<br />   -- **De volgende stappen zijn alleen vereist voor primaire AD FS-servers.** -- <br />
6. Open de module **AD FS-beheer** (klik in Serverbeheer op Extra en selecteer vervolgens AD FS-beheer).
7. Klik in het deelvenster **Acties** op **Federation Service-eigenschappen** bewerken.
8. Klik in het dialoogvenster **Federation Service-eigenschappen** op het tabblad **Gebeurtenissen**.
9. Schakel de selectievakjes bij **Succesvolle controles en Mislukte controles** in en klik op **OK**. Deze moeten standaard zijn ingeschakeld.
10. Open een PowerShell-venster en voer de volgende opdracht uit: ```Set-AdfsProperties -AuditLevel Verbose```.

Het basiscontroleniveau is standaard ingeschakeld. Meer informatie over de [AD FS-controleverbetering in Windows Server 2016](https://docs.microsoft.com/windows-server/identity/ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server)


#### <a name="to-locate-the-ad-fs-audit-logs"></a>De AD FS-auditlogboeken zoeken

1. Open **Logboeken**.
2. Ga naar de Windows-logboeken en selecteer **Beveiliging**.
3. Klik rechts op **Huidige logboeken filteren**.
4. Selecteer onder Gebeurtenisbron **AD FS-controle**.

    En snelle [opmerking over veelgestelde vragen](reference-connect-health-faq.md#operations-questions) over auditlogboeken.

![AD FS-auditlogboeken](./media/how-to-connect-health-agent-install/adfsaudit.png)

> [!WARNING]
> Met een groepsbeleid kan AD FS-controle worden uitgeschakeld. Als AD FS-controle is uitgeschakeld, zijn er geen gebruiksanalyses beschikbaar over aanmeldactiviteiten. Zorg ervoor dat u geen groepsbeleid hebt waarmee AD FS-controle wordt uitgeschakeld.>
>


## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>De Azure AD Connect Health for Sync-agent installeren

De Azure AD Connect Health for Sync-agent wordt automatisch geïnstalleerd via de laatste build van Azure AD Connect. Als u Azure AD Connect Health for Sync wilt gebruiken, moet u de nieuwste verzie van Azure AD Connect downloaden en installeren. U kunt de nieuwste versie [hier](https://www.microsoft.com/download/details.aspx?id=47594) downloaden.

Als u wilt controleren of de agent is geïnstalleerd, zoekt u de volgende services op de server. Als de configuratie is voltooid, moeten deze services worden uitgevoerd. Ze worden pas gestart wanneer de configuratie is voltooid.

* Azure AD Connect Health Sync Insights-service
* Azure AD Connect Health Sync Monitoring-service

![Azure AD Connect Health for Sync verifiëren](./media/how-to-connect-health-agent-install/services.png)

> [!NOTE]
> Als u Azure AD Connect Health wilt gebruiken, is Azure AD Premium vereist. Als u niet over Azure AD Premium beschikt, kunt u de configuratie in Azure Portal niet voltooien. Voor meer informatie raadpleegt u de [pagina met vereisten](how-to-connect-health-agent-install.md#requirements).
>
>

## <a name="manual-azure-ad-connect-health-for-sync-registration"></a>Handmatige Azure AD Connect Health for Sync-registratie

Als de registratie van de Azure AD Connect Health for Sync-agent mislukt nadat Azure AD Connect is geïnstalleerd, kunt u de volgende PowerShell-opdracht gebruiken om de agent handmatig te registreren.

> [!IMPORTANT]
> U hoeft deze PowerShell-opdracht alleen te gebruiken als het registreren van de agent mislukt nadat Azure AD Connect is geïnstalleerd.
>
>

De volgende PowerShell-opdracht is ALLEEN vereist als het registreren van de Health Agent mislukt nadat Azure AD Connect is geïnstalleerd en geconfigureerd. De Azure AD Connect Health-services worden niet gestart nadat de agent is geregistreerd.

U kunt de Azure AD Connect Health for Sync-agent handmatig registreren aan de hand van de volgende PowerShell-opdracht:

`Register-AzureADConnectHealthSyncAgent -AttributeFiltering $false -StagingMode $false`

Voor de opdracht worden de volgende parameters gebruikt:

* AttributeFiltering : $true (standaard) - als Azure AD Connect de standaardkenmerkset niet synchroniseert en is geconfigureerd voor het gebruik van een gefilterde kenmerkset. $false in andere gevallen.
* StagingMode : $false (standaard) - als de Azure AD Connect-server zich niet in de faseringsmodus bevindt, $true als de server is geconfigureerd voor de faseringsmodus.

Als u wordt gevraagd zich te verifiëren, moet u hetzelfde hoofdbeheerdersaccount gebruiken (zoals admin@domain.onmicrosoft.com) dat is gebruikt voor het configureren van Azure AD Connect.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>De Azure AD Connect Health-agent voor AD DS installeren

Dubbelklik op het .exe-bestand dat u hebt gedownload om de installatie van de agent te starten. Klik in het eerste scherm op Installeren.

![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install1.png)

Wanneer de installatie is voltooid, klikt u op Nu configureren.

![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install2.png)

Er wordt een opdrachtprompt geopend, waarna PowerShell wordt gebruikt om Register-AzureADConnectHealthADDSAgent uit te voeren. Als u wordt gevraagd om u aan te melden bij Azure, gaat u door en meldt u zich aan.

![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install3.png)

Na het aanmelden gaat PowerShell door. Zodra het proces is voltooid, kunt u PowerShell sluiten. De configuratie is dan voltooid.

De services worden automatisch gestart en de agent houdt zich bezig met het bewaken en verzamelen van gegevens. In het PowerShell-venster worden waarschuwingen weergegeven als u niet aan alle vereisten hebt voldaan die in vorige gedeelten zijn beschreven. Zorg ervoor dat u aan alle [vereisten](how-to-connect-health-agent-install.md#requirements) voldoet voordat u de agent installeert. In de schermafbeelding hieronder ziet u een voorbeeld van deze fouten.

![Azure AD Connect Health voor AD DS verifiëren](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install4.png)

Als u wilt controleren of de agent is geïnstalleerd, zoekt u de volgende services op de domeincontroller.

* Azure AD Connect Health AD DS Insights-service
* Azure AD Connect Health AD DS Monitoring-service

Als de configuratie is voltooid, worden deze services als het goed is al uitgevoerd. Ze worden pas gestart wanneer de configuratie is voltooid.

![Azure AD Connect Health verifiëren](./media/how-to-connect-health-agent-install/aadconnect-health-adds-agent-install5.png)

### <a name="quick-agent-installation-in-multiple-servers"></a>Snelle agentinstallatie op meerdere servers

1. Maak een gebruikersaccount in Azure AD met een wachtwoord.
2. Toewijzen de **eigenaar** rol voor deze lokale AAD-account in Azure AD Connect Health via de portal. Volg de stappen [hier](how-to-connect-health-operations.md#manage-access-with-role-based-access-control). Wijs de rol toe op alle service-exemplaren. 
3. Download het .exe MSI-bestand in de lokale domeincontroller voor de installatie.
4. Voer het volgende script om te registreren. Vervang de parameters met het nieuwe gebruikersaccount gemaakt en het bijbehorende wachtwoord. 

```powershell
AdHealthAddsAgentSetup.exe /quiet
Start-Sleep 30
$userName = "NEWUSER@DOMAIN"
$secpasswd = ConvertTo-SecureString "PASSWORD" -AsPlainText -Force
$myCreds = New-Object System.Management.Automation.PSCredential ($userName, $secpasswd)
import-module "C:\Program Files\Azure Ad Connect Health Adds Agent\PowerShell\AdHealthAdds"
 
Register-AzureADConnectHealthADDSAgent -UserPrincipalName $USERNAME -Credential $myCreds

```

1. Wanneer u klaar bent, kunt u toegang voor het lokale account verwijderen door het uitvoeren van een of meer van de volgende: 
    * Verwijderen van de roltoewijzing voor het lokale account voor AAD Connect Health
    * Het wachtwoord voor het lokale account draaien. 
    * Het lokale AAD-account uitschakelen
    * Het lokale AAD-account verwijderen  

## <a name="agent-registration-using-powershell"></a>Agentregistratie met behulp van PowerShell

Wanneer u het juiste setup.exe-bestand hebt geïnstalleerd voor de agent, kunt u de agentregistratiestap uitvoeren met de volgende PowerShell-opdrachten (afhankelijk van de rol). Open een PowerShell-venster en voer de juiste opdracht uit:

```powershell
    Register-AzureADConnectHealthADFSAgent
    Register-AzureADConnectHealthADDSAgent
    Register-AzureADConnectHealthSyncAgent

```

In deze opdrachten worden referenties geaccepteerd als parameter voor het niet-interactief voltooien van de registratie op een Server Core-machine.
* De referenties kunnen worden vastgelegd in een PowerShell-variabele die wordt doorgegeven als parameter.
* U kunt een Azure AD-id opgeven waarmee agents kunnen worden geregistreerd en waarvoor MFA NIET is ingeschakeld.
* Globale beheerders hebben standaard de mogelijkheid om agents te registreren. U kunt ook andere id’s met minder bevoegdheden toestaan om deze stap uit te voeren. Meer informatie over [Toegangsbeheer op basis van rollen](how-to-connect-health-operations.md#manage-access-with-role-based-access-control).

```powershell
    $cred = Get-Credential
    Register-AzureADConnectHealthADFSAgent -Credential $cred

```

## <a name="configure-azure-ad-connect-health-agents-to-use-http-proxy"></a>Azure AD Connect Health-agents configureren voor het gebruik van een HTTP-proxy

U kunt Azure AD Connect Health-agents configureren voor het gebruik van een HTTP-proxy.

> [!NOTE]
> * Het gebruik van Netsh WinHttp set ProxyServerAddress wordt niet ondersteund, omdat de agent gebruikmaakt van System.Net om webaanvragen te doen in plaats van Windows HTTP-services.
> * Het geconfigureerde HTTP-proxyadres wordt gebruikt om versleutelde HTTPS-berichten door te geven.
> * Proxy’s die zijn geverifieerd (met HTTPBasic), worden niet ondersteund.
>
>

### <a name="change-health-agent-proxy-configuration"></a>De Health Agent-proxyconfiguratie wijzigen

U hebt de volgende opties voor het configureren van de Azure AD Connect Health-agent voor het gebruik van een HTTP-proxy.

> [!NOTE]
> U moet alle Azure AD Connect Health-agentservices opnieuw opstarten om de proxyinstellingen bij te werken. Voer de volgende opdracht uit:<br />
> Restart-Service AdHealth*
>
>

#### <a name="import-existing-proxy-settings"></a>Bestaande proxyinstellingen importeren

##### <a name="import-from-internet-explorer"></a>Importeren vanuit Internet Explorer

Internet Explorer HTTP-proxy-instellingen kunnen worden geïmporteerd om te worden gebruikt door de Azure AD Connect Health-Agents. Voer op elke server waarop de Health-agent wordt uitgevoerd de volgende PowerShell-opdracht uit:

    Set-AzureAdConnectHealthProxySettings -ImportFromInternetSettings

##### <a name="import-from-winhttp"></a>Importeren vanaf WinHTTP

WinHTTP-proxy-instellingen kunnen worden geïmporteerd om te worden gebruikt door de Azure AD Connect Health-Agents. Voer op elke server waarop de Health-agent wordt uitgevoerd de volgende PowerShell-opdracht uit:

    Set-AzureAdConnectHealthProxySettings -ImportFromWinHttp

#### <a name="specify-proxy-addresses-manually"></a>Handmatig proxyadressen opgeven

U kunt handmatig een proxyserver opgeven door de volgende PowerShell-opdracht uit te voeren op elke server waarop de Health-agent wordt uitgevoerd:

    Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress address:port

Voorbeeld: *Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress mijnproxyserver: 443*

* Het adres kan een oplosbare DNS-servernaam of een IPv4-adres zijn
* U hoeft geen poort op te geven. Indien u geen poort opgeeft, wordt 443 gekozen als standaardpoort.

#### <a name="clear-existing-proxy-configuration"></a>De bestaande proxyconfiguratie wissen

U kunt de bestaande proxyconfiguratie wissen door de volgende opdracht uit te voeren:

    Set-AzureAdConnectHealthProxySettings -NoProxy


### <a name="read-current-proxy-settings"></a>De huidige proxyinstellingen lezen

U kunt de volgende opdracht gebruiken om de momenteel geconfigureerde proxyinstellingen te lezen:

    Get-AzureAdConnectHealthProxySettings


## <a name="test-connectivity-to-azure-ad-connect-health-service"></a>De verbinding met de Azure AD Connect Health-service testen

Het is mogelijk dat er problemen optreden waardoor de verbinding tussen de Azure AD Connect Health-agent en de Azure AD Connect Health-service wordt verbroken. Hierbij kan het gaan om netwerkproblemen, problemen met de machtigingen of andere problemen.

Als de agent gedurende meer dan twee uur geen gegevens kan verzenden naar de Azure AD Connect Health-service, wordt er een waarschuwing weergegeven waarin staat dat de Health-servicegegevens niet up-to-date zijn. U kunt bevestigen dat de betrokken Azure AD Connect Health-agent gegevens kan uploaden naar de Azure AD Connect Health-service door de volgende PowerShell-opdracht uit te voeren:

    Test-AzureADConnectHealthConnectivity -Role ADFS

De rolparameter heeft momenteel de volgende waarden:

* ADFS
* Sync
* ADDS

> [!NOTE]
> Als u het connectiviteitshulpprogramma wilt gebruiken, moet u de agent eerst registreren. Als u de registratie van de agent niet kunt voltooien, moet u ervoor zorgen dat u aan alle [vereisten](how-to-connect-health-agent-install.md#requirements) voor Azure AD Connect Health voldoet. Deze connectiviteitstest wordt standaard uitgevoerd tijdens de agentregistratie.
>
>

## <a name="related-links"></a>Verwante koppelingen

* [Azure AD Connect Health (Engelstalig)](whatis-hybrid-identity-health.md)
* [Azure AD Connect Health Operations](how-to-connect-health-operations.md) (Azure AD Connect Health-bewerkingen)
* [Azure AD Connect Health gebruiken met AD FS](how-to-connect-health-adfs.md)
* [Azure AD Connect Health for Sync gebruiken](how-to-connect-health-sync.md)
* [Azure AD Connect Health gebruiken met AD DS](how-to-connect-health-adds.md)
* [Veelgestelde vragen over Azure AD Connect Health](reference-connect-health-faq.md)
* [Versiegeschiedenis van Azure AD Connect Health](reference-connect-health-version-history.md)

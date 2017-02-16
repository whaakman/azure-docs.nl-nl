---
title: De Azure AD Connect Health-agent installeren | Microsoft Docs
description: Dit is de Azure AD Connect Health-pagina waarop wordt beschreven hoe u de agent voor AD FS en Sync installeert.
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 1cc8ae90-607d-4925-9c30-6770a4bd1b4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 28b5da6098316f8fbe84966e0dac88f5b7d2cb1d
ms.openlocfilehash: 99cd66d0fc4fc96c0b49e1ce3e3e2095fbe62395


---
# <a name="azure-ad-connect-health-agent-installation"></a>De Azure AD Connect Health-agent installeren
In dit document wordt beschreven hoe u de Azure AD Connect Health-agents installeert en configureert. U kunt de agents [hier](active-directory-aadconnect-health.md#download-and-install-azure-ad-connect-health-agent) downloaden.

## <a name="requirements"></a>Vereisten
In de volgende tabel ziet u een lijst vereisten voor het gebruik van Azure AD Connect Health.

| Vereiste | Beschrijving |
| --- | --- |
| Azure AD Premium |Azure AD Connect Health is een Azure AD Premium-functie waarvoor Azure AD Premium is vereist. </br></br>Zie voor meer informatie [Aan de slag met Azure AD Premium](../active-directory-get-started-premium.md) </br>Als u gebruik wilt maken van een gratis proefversie van 30 dagen, raadpleegt u [Gratis proberen](https://azure.microsoft.com/trial/get-started-active-directory/). |
| U moet hoofdbeheerder van uw Azure AD zijn om aan de slag te gaan met Azure AD Connect Health |Standaard kunnen alleen hoofdbeheerders de Health-agents installeren en configureren om aan de slag te gaan, de Portal openen en bewerkingen uitvoeren in Azure AD Connect Health. Voor meer informatie raadpleegt u [Uw Azure AD-directory beheren](../active-directory-administer.md). <br><br> Met behulp van op rollen gebaseerd toegangsbeheer kunt u toegang tot Azure AD Connect Health verlenen aan andere gebruikers binnen uw organisatie. Zie voor meer informatie [Op rollen gebaseerd toegangsbeheer voor Azure AD Connect Health](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control). </br></br>**Belangrijk:** het account dat u gebruikt tijdens het installeren van agents, moet een werk- of schoolaccount zijn. Het mag geen Microsoft-account zijn. Voor meer informatie raadpleegt u [Als organisatie registreren voor Azure](../sign-up-organization.md) |
| De Azure AD Connect Health-agent wordt geïnstalleerd op elke doelserver |Voor Azure AD Connect Health is het vereist dat er op elke doelserver een agent wordt geïnstalleerd om ervoor te zorgen dat de gegevens die in de portal moeten worden weergegeven, worden aangeleverd. </br></br>Als u bijvoorbeeld gegevens in uw on-premises AD FS-infrastructuur wilt krijgen, moet de agent worden geïnstalleerd op de AD FS-servers, AD FS-proxyservers en webtoepassingsproxyservers. Evenzo moet de agent op de domeincontrollers worden geïnstalleerd om gegevens in uw on-premises AD DS-infrastructuur te krijgen. </br></br>**Belangrijk:** het account dat u gebruikt tijdens het installeren van agents, moet een werk- of schoolaccount zijn. Het mag geen Microsoft-account zijn. Voor meer informatie raadpleegt u [Als organisatie registreren voor Azure](../sign-up-organization.md) |
| Uitgaande verbinding met de Azure-service-eindpunten |Tijdens de installatie en runtime moet de agent verbonden zijn met de Azure AD Connect Health-service-eindpunten. Als de uitgaande connectiviteit is geblokkeerd, moet u ervoor zorgen dat de volgende eindpunten wel zijn toegestaan: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.queue.core.windows.net</li><li>adhsprodwus.servicebus.windows.net - Poort: 5671 </li><li>https://management.azure.com </li><li>https://s1.adhybridhealth.azure.com/</li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
| Firewallpoorten op de server waarop de agent wordt uitgevoerd. |De volgende firewallpoorten moeten open zijn, anders kan de agent niet communiceren met de eindpunten van de Azure AD Health-service.</br></br><li>TCP/UDP-poort 443</li><li>TCP/UDP-poort 5671</li> |
| Sta de volgende websites toe als verbeterde beveiliging van Internet Explorer is ingeschakeld |Als verbeterde beveiliging van Internet Explorer is ingeschakeld, moeten de volgende websites worden toegestaan op de server waarop u de agent wilt installeren.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>De federatieve server van uw organisatie moet worden vertrouwd door Azure Active Directory. Bijvoorbeeld: https://sts.contoso.com</li> |

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-fs"></a>De Azure AD Connect Health-agent voor AD FS installeren
Dubbelklik op het .exe-bestand dat u hebt gedownload om de installatie van de agent te starten. Klik in het eerste scherm op Installeren.

![Azure AD Connect Health verifiëren](./media/active-directory-aadconnect-health-requirements/install1.png)

Wanneer de installatie is voltooid, klikt u op Nu configureren.

![Azure AD Connect Health verifiëren](./media/active-directory-aadconnect-health-requirements/install2.png)

Er wordt een opdrachtprompt geopend, waarna PowerShell wordt gebruikt om Register-AzureADConnectHealthADFSAgent uit te voeren. Als u wordt gevraagd om u aan te melden bij Azure, gaat u door en meldt u zich aan.

![Azure AD Connect Health verifiëren](./media/active-directory-aadconnect-health-requirements/install3.png)

Na het aanmelden gaat PowerShell door. Zodra het proces is voltooid, kunt u PowerShell sluiten. De configuratie is dan voltooid.

De services worden automatisch gestart en de agent houdt zich bezig met het bewaken en verzamelen van gegevens. In het PowerShell-venster worden waarschuwingen weergegeven als u niet aan alle vereisten hebt voldaan die in vorige gedeelten zijn beschreven. Zorg ervoor dat u aan alle [vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voldoet voordat u de agent installeert. In de schermafbeelding hieronder ziet u een voorbeeld van deze fouten.

![Azure AD Connect Health verifiëren](./media/active-directory-aadconnect-health-requirements/install4.png)

Als u wilt controleren of de agent is geïnstalleerd, zoekt u de volgende services op de server. Als de configuratie is voltooid, moeten deze services worden uitgevoerd. Ze worden pas gestart wanneer de configuratie is voltooid.

* Azure AD Connect Health AD FS Diagnostics-service
* Azure AD Connect Health AD FS Insights-service
* Azure AD Connect Health AD FS Monitoring-service

![Azure AD Connect Health verifiëren](./media/active-directory-aadconnect-health-requirements/install5.png)

### <a name="agent-installation-on-windows-server-2008-r2-servers"></a>De agent installeren op Windows Server 2008 R2-servers
Stappen voor Windows Server 2008 R2-servers:

1. Zorg ervoor dat de server met Service Pack 1 of hoger wordt uitgevoerd.
2. Verbeterde beveiliging van Internet Explorer uitschakelen voor de installatie van agents:
3. Installeer Windows PowerShell 4.0 op alle servers voordat u de AD Health-agent installeert. Windows PowerShell 4.0 installeren:
   * Installeer [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=40779) via de volgende koppeling. U kunt hier het offline-installatieprogramma downloaden.
   * PowerShell ISE (van Windows-onderdelen) installeren
   * Installeer [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855).
   * Installeer Internet Explorer versie 10 of hoger op de server. (Dit is vereist voor de Health Service, zodat kan worden geverifieerd of u Azure-beheerdersreferenties gebruikt.)
4. Voor meer informatie over het installeren van Windows PowerShell 4.0 voor Windows Server 2008 R2 raadpleegt u [dit](http://social.technet.microsoft.com/wiki/contents/articles/20623.step-by-step-upgrading-the-powershell-version-4-on-2008-r2.aspx) wiki-artikel.

### <a name="enable-auditing-for-ad-fs"></a>Controle inschakelen voor AD FS
> [!NOTE]
> Dit is alleen van toepassing op federatieve AD FS-servers.
> 
> 

Als u de functie Gebruiksanalyse wilt gebruiken om gegevens te verzamelen en te analyseren, moet de Azure AD Connect Health-agent beschikken over de informatie in de AD FS-auditlogboeken. Deze logboeken worden niet standaard ingeschakeld. Volg de volgende procedures om AD FS-controles in te schakelen en de AD FS-auditlogboeken op uw AD FS-servers te zoeken.

#### <a name="to-enable-auditing-for-ad-fs-20"></a>Controle inschakelen voor AD FS 2.0
1. Klik op **Start**, ga naar **Programma's**, ga naar **Systeembeheer** en klik vervolgens op **Lokaal beveiligingsbeleid**.
2. Navigeer naar de map **Beveiligingsinstellingen\Lokaal beleid\Gebruikersrechtenbeheer** en dubbelklik op Beveiligingscontrole genereren.
3. Controleer op het tabblad **Lokale beveiligingsinstelling** of het AD FS 2.0-serviceaccount wordt vermeld. Als dit niet zo is, klikt u op **Gebruiker of groep toevoegen**, voegt u het account toe aan de lijst en klikt u op **OK**.
4. Als u de controle wilt inschakelen, opent u een opdrachtprompt met verhoogde bevoegdheden en voert u de volgende opdracht uit:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable</code>
5. Sluit Lokaal beveiligingsbeleid en open de module Beheer. Als u de module Beheer wilt openen, klikt u op **Start**, gaat u naar **Programma's**, gaat u naar **Systeembeheer** en klikt u vervolgens op AD FS 2.0-beheer.
6. Klik in het deelvenster Acties op Federation Service-eigenschappen bewerken.
7. Klik in het dialoogvenster **Federation Service-eigenschappen** op het tabblad **Gebeurtenissen**.
8. Schakel de selectievakjes bij **Succesvolle controles** en **Mislukte controles** in.
9. Klik op **OK**.

#### <a name="to-enable-auditing-for-ad-fs-on-windows-server-2012-r2"></a>Controle inschakelen voor AD FS in Windows Server 2012 R2
1. Open **Lokaal beveiligingsbeleid**. Dit doet u door naar **Serverbeheer** te gaan op het startscherm, of door Serverbeheer te openen via de taakbalk op het bureaublad en door vervolgens op **Extra/Lokaal beveiligingsbeleid** te klikken.
2. Navigeer naar de map **Beveiligingsinstellingen\Lokaal beleid\Gebruikersrechten toewijzen** en dubbelklik op **Beveiligingscontrole genereren**.
3. Controleer op het tabblad **Lokale beveiligingsinstelling** of het AD FS-serviceaccount wordt vermeld. Als dit niet zo is, klikt u op **Gebruiker of groep toevoegen**, voegt u het account toe aan de lijst en klikt u op **OK**.
4. Als u de controle wilt inschakelen, opent u een opdrachtprompt met verhoogde bevoegdheden en voert u de volgende opdracht uit:<code>auditpol.exe /set /subcategory:"Application Generated" /failure:enable /success:enable.</code>
5. Sluit **Lokaal beveiligingsbeleid** en open vervolgens de module **AD FS-beheer** (klik in Serverbeheer op Extra en selecteer vervolgens AD FS-beheer).
6. Klik in het deelvenster Acties op **Federation Service-eigenschappen bewerken**.
7. Klik in het dialoogvenster Federation Service-eigenschappen op het tabblad **Gebeurtenissen**.
8. Schakel de selectievakjes bij **Succesvolle controles en Mislukte controles** in en klik op **OK**.

#### <a name="to-locate-the-ad-fs-audit-logs"></a>De AD FS-auditlogboeken zoeken
1. Open **Logboeken**.
2. Ga naar de Windows-logboeken en selecteer **Beveiliging**.
3. Klik rechts op **Huidige logboeken filteren**.
4. Selecteer onder Gebeurtenisbron **AD FS-controle**.

![AD FS-auditlogboeken](./media/active-directory-aadconnect-health-requirements/adfsaudit.png)

> [!WARNING]
> Als u een groepsbeleid hebt waarmee AD FS-controle wordt uitgeschakeld, kan de Azure AD Connect Health-agent geen gegevens verzamelen. Zorg ervoor dat u geen groepsbeleid hebt waarmee controles worden uitgeschakeld.
> 
> 

[//]: # (Start of Agent Proxy Configuration Section)

## <a name="installing-the-azure-ad-connect-health-agent-for-sync"></a>De Azure AD Connect Health for Sync-agent installeren
De Azure AD Connect Health for Sync-agent wordt automatisch geïnstalleerd via de laatste build van Azure AD Connect. Als u Azure AD Connect Health for Sync wilt gebruiken, moet u de nieuwste verzie van Azure AD Connect downloaden en installeren. U kunt de nieuwste versie [hier](http://www.microsoft.com/download/details.aspx?id=47594) downloaden.

Als u wilt controleren of de agent is geïnstalleerd, zoekt u de volgende services op de server. Als de configuratie is voltooid, moeten deze services worden uitgevoerd. Ze worden pas gestart wanneer de configuratie is voltooid.

* Azure AD Connect Health Sync Insights-service
* Azure AD Connect Health Sync Monitoring-service

![Azure AD Connect Health for Sync verifiëren](./media/active-directory-aadconnect-health-sync/services.png)

> [!NOTE]
> Als u Azure AD Connect Health wilt gebruiken, is Azure AD Premium vereist. Als u niet over Azure AD Premium beschikt, kunt u de configuratie in Azure Portal niet voltooien. Voor meer informatie raadpleegt u de [pagina met vereisten](active-directory-aadconnect-health-agent-install.md#requirements).
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

Als u wordt gevraagd zich te verifiëren, moet u hetzelfde hoofdbeheerdersaccount gebruiken (zoals admin@domain.onmicrosoft.com)) dat is gebruikt voor het configureren van Azure AD Connect.

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds"></a>De Azure AD Connect Health-agent voor AD DS installeren
Dubbelklik op het .exe-bestand dat u hebt gedownload om de installatie van de agent te starten. Klik in het eerste scherm op Installeren.

![Azure AD Connect Health verifiëren](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install1.png)

Wanneer de installatie is voltooid, klikt u op Nu configureren.

![Azure AD Connect Health verifiëren](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install2.png)

Er wordt een opdrachtprompt geopend, waarna PowerShell wordt gebruikt om Register-AzureADConnectHealthADDSAgent uit te voeren. Als u wordt gevraagd om u aan te melden bij Azure, gaat u door en meldt u zich aan.

![Azure AD Connect Health verifiëren](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install3.png)

Na het aanmelden gaat PowerShell door. Zodra het proces is voltooid, kunt u PowerShell sluiten. De configuratie is dan voltooid.

De services worden automatisch gestart en de agent houdt zich bezig met het bewaken en verzamelen van gegevens. In het PowerShell-venster worden waarschuwingen weergegeven als u niet aan alle vereisten hebt voldaan die in vorige gedeelten zijn beschreven. Zorg ervoor dat u aan alle [vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voldoet voordat u de agent installeert. In de schermafbeelding hieronder ziet u een voorbeeld van deze fouten.

![Azure AD Connect Health voor AD DS verifiëren](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install4.png)

Als u wilt controleren of de agent is geïnstalleerd, zoekt u de volgende services op de domeincontroller.

* Azure AD Connect Health AD DS Insights-service
* Azure AD Connect Health AD DS Monitoring-service

Als de configuratie is voltooid, worden deze services als het goed is al uitgevoerd. Ze worden pas gestart wanneer de configuratie is voltooid.

![Azure AD Connect Health verifiëren](./media/active-directory-aadconnect-health/aadconnect-health-adds-agent-install5.png)

## <a name="installing-the-azure-ad-connect-health-agent-for-ad-ds-on-server-core"></a>De Azure AD Connect Health-agent voor AD DS installeren in Server Core
Na de installatie van het .exe-bestand kunt u het registratieproces voltooien met behulp van de volgende PowerShell-opdracht:

`Register-AzureADConnectHealthADDSAgent -Credential $cred`

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
> U moet alle Azure AD Connect Health-agentservices opnieuw opstarten om de proxyinstellingen bij te werken. Voer de volgende opdracht uit:<br>
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

Voorbeeld: *Set-AzureAdConnectHealthProxySettings -HttpsProxyAddress myproxyserver:443*

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

U kunt de vlag -ShowResults in de opdracht gebruiken om gedetailleerde logboeken weer te geven. Gebruik het volgende voorbeeld:

    Test-AzureADConnectHealthConnectivity -Role Sync -ShowResult

> [!NOTE]
> Als u het connectiviteitshulpprogramma wilt gebruiken, moet u de agent eerst registreren. Als u de registratie van de agent niet kunt voltooien, moet u ervoor zorgen dat u aan alle [vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voor Azure AD Connect Health voldoet. Deze connectiviteitstest wordt standaard uitgevoerd tijdens de agentregistratie.
> 
> 

## <a name="related-links"></a>Verwante koppelingen
* [Azure AD Connect Health (Engelstalig)](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Operations](active-directory-aadconnect-health-operations.md) (Azure AD Connect Health-bewerkingen)
* [Azure AD Connect Health gebruiken met AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health for Sync gebruiken](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health gebruiken met AD DS](active-directory-aadconnect-health-adds.md)
* [Veelgestelde vragen over Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versiegeschiedenis van Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)




<!--HONumber=Dec16_HO3-->



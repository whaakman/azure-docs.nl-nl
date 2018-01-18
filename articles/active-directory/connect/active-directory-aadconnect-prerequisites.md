---
title: 'Azure AD Connect: Vereisten en hardware | Microsoft Docs'
description: Dit onderwerp beschrijft de vereisten en de hardwarevereisten voor Azure AD Connect
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 9c35e796cb823b2b059b726f099d658ee5e8192b
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="prerequisites-for-azure-ad-connect"></a>Vereisten voor Azure AD Connect
Dit onderwerp beschrijft de vereisten en de hardwarevereisten voor Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Voordat u Azure AD Connect installeren
Voordat u Azure AD Connect installeert, zijn er enkele dingen die u nodig hebt.

### <a name="azure-ad"></a>Azure AD
* Een Azure-abonnement of een [Azure-proefabonnement](https://azure.microsoft.com/pricing/free-trial/). Dit abonnement is alleen vereist voor toegang tot de Azure-portal en niet voor het gebruik van Azure AD Connect. Als u PowerShell of Office 365 gebruikt, hoeft niet u geen Azure-abonnement voor Azure AD Connect gebruiken. Als u een licentie voor Office 365 hebt, kunt u ook de Office 365-portal gebruiken. Met een betaald Office 365-licentie, kunt u ook opvragen bij de Azure portal vanuit de Office 365-portal.
  * U kunt ook de [Azure-portal](https://portal.azure.com). Deze portal is niet vereist voor een Azure AD-licentie.
* [Toevoegen en controleren van het domein](../active-directory-domains-add-azure-portal.md) u van plan bent te gebruiken in Azure AD. Bijvoorbeeld, als u van plan bent te contoso.com gebruiken voor uw gebruikers, moet u ervoor zorgen in dit domein is geverifieerd en u niet alleen het standaarddomein contoso.onmicrosoft.com gebruikt.
* Een Azure AD-tenant kan door 50k-standaardobjecten. Bij het controleren van uw domein, wordt de limiet is verhoogd tot 300 k-objecten. Als u nog meer objecten in Azure AD, moet u een ondersteuningsaanvraag om het maximum verhoogd nog verder te openen. Als u meer dan 500 kB-objecten, moet u een licentie, zoals Office 365, Azure AD Basic, Azure AD Premium of Enterprise Mobility en beveiliging.

### <a name="prepare-your-on-premises-data"></a>Uw on-premises gegevens voorbereiden
* Gebruik [IdFix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) fouten zoals duplicaten en opmaak problemen in uw directory identificeren voordat u met Azure AD synchroniseren en Office 365.
* Bekijk [optionele synchronisatie-functies u in Azure AD inschakelen kunt](active-directory-aadconnectsyncservice-features.md) en evalueren van de functies die u moet inschakelen.

### <a name="on-premises-active-directory"></a>On-premises Active Directory
* Het AD-schema-versie en forest-functionaliteitsniveau moet WindowsServer 2003 of hoger. De domeincontrollers kan een willekeurige versie kunnen uitvoeren, zolang de schema- en forest vereisten wordt voldaan.
* Als u van plan bent om de functie te gebruiken **wachtwoord terugschrijven**, moet de domeincontrollers Windows Server 2008 (met de meest recente SP) of hoger. Als uw DC's op 2008 (pre-R2 worden), dan moet u ook toepassen [hotfix KB2386717](http://support.microsoft.com/kb/2386717).
* De domeincontroller die wordt gebruikt door Azure AD moet schrijfbaar zijn. Het is **niet ondersteund** gebruik van een RODC (alleen-lezen domeincontroller) en Azure AD Connect niet voldoet aan alle omleidingen schrijven.
* Het is **niet ondersteund** lokale forests/domeinen met behulp van niveaudomeinen (één Label domeinen) gebruiken.
* Het is **niet ondersteund** lokale forests/domeinen met behulp van 'scheidingspunten' gebruiken (de naam van een punt bevat '. ') NetBIOS-namen.
* Het is raadzaam te [de Prullenbak van Active Directory inschakelen](active-directory-aadconnectsync-recycle-bin.md).

### <a name="azure-ad-connect-server"></a>Azure AD Connect-server
* Azure AD Connect kan niet worden geïnstalleerd op Small Business Server of Windows Server Essentials. De server moet gebruikmaken van Windows Server standard of hoger.
* De Azure AD Connect-server moet een volledige GUI geïnstalleerd hebben. Het is **niet ondersteund** te installeren op server core.
* Azure AD Connect moet worden geïnstalleerd op Windows Server 2008 of hoger. Deze server mogelijk een domeincontroller of lidserver wanneer u een snelle instellingen. Als u aangepaste instellingen, de server kan ook worden zelfstandige en hoeft niet te worden toegevoegd aan een domein.
* Als u Azure AD Connect op Windows Server 2008 of Windows Server 2008 R2 installeren, Controleer of de meest recente hotfixes toepassen via Windows Update. De installatie kan niet beginnen met een niet-gepatchte-server.
* Als u van plan bent om de functie te gebruiken **Wachtwoordsynchronisatie**, en vervolgens de Azure AD Connect-server Windows Server 2008 R2 SP1 of hoger moet.
* Als u wilt gebruiken een **groep beheerd serviceaccount**, en vervolgens de Azure AD Connect-server op Windows Server 2012 of later moet.
* De Azure AD Connect-server moet hebben [.NET Framework 4.5.1](#component-prerequisites) of hoger en [Microsoft PowerShell 3.0](#component-prerequisites) of hoger is geïnstalleerd.
* De Azure AD Connect-server geen PowerShell schrijffouten-Groepsbeleid is ingeschakeld.
* Als Active Directory Federation Services wordt geïmplementeerd, de servers waar AD FS of Web Application Proxy zijn geïnstalleerd moet Windows Server 2012 R2 of hoger. [Windows remote management](#windows-remote-management) moet zijn ingeschakeld op deze servers voor installatie op afstand.
* Als Active Directory Federation Services wordt geïmplementeerd, moet u [SSL-certificaten](#ssl-certificate-requirements).
* Als Active Directory Federation Services wordt geïmplementeerd, moet u voor het configureren van [naamomzetting](#name-resolution-for-federation-servers).
* Als uw globale beheerders hebben MFA ingeschakeld, klikt u vervolgens de URL **https://secure.aadcdn.microsoftonline-p.com** moet zich in de lijst met vertrouwde sites. U wordt gevraagd deze website aan de lijst met vertrouwde sites toevoegen wanneer u wordt gevraagd om een challenge MFA en deze niet is toegevoegd voordat. Internet Explorer kunt u deze toevoegen aan uw vertrouwde sites.

### <a name="sql-server-used-by-azure-ad-connect"></a>SQL Server die wordt gebruikt door Azure AD Connect
* Azure AD Connect vereist een SQL Server-database voor het opslaan van identiteitsgegevens. Een SQL Server 2012 Express LocalDB (een lichte versie van SQL Server Express) wordt standaard geïnstalleerd. SQL Server Express heeft een limiet van 10GB waarmee u ongeveer 100.000 objecten beheren. Als u beheren een groter volume van directory-objecten wilt, moet u de installatiewizard wijst naar een andere installatie van SQL Server.
* Als u een afzonderlijke SQL Server gebruikt, is deze vereisten van toepassing:
  * Azure AD Connect ondersteunt alle versies van Microsoft SQL Server van SQL Server 2008 (met de meest recente servicepack) naar SQL Server 2016 SP1. Microsoft Azure SQL Database is **niet ondersteund** als een database.
  * U moet een niet-hoofdlettergevoelige SQL-sortering gebruiken. Deze sorteringen worden aangeduid met een \_CI_ in hun naam. Het is **niet ondersteund** voor het gebruik van een hoofdlettergevoelige sortering, geïdentificeerd door \_CS_ in hun naam.
  * U kunt slechts één synchronisatie-engine per SQL-exemplaar hebben. Het is **niet ondersteund** delen van een SQL-exemplaar met FIM/MIM Sync en DirSync en Azure AD Sync.

### <a name="accounts"></a>Accounts
* Een globale beheerder van Azure AD-account voor de Azure AD-tenant die u wilt integreren. Deze account moet een **school of organisatie-account** en kan niet worden een **Microsoft-account**.
* Als u snelle instellingen gebruiken of een van DirSync upgrade, moet u een Enterprise-beheerdersaccount hebben voor uw lokale Active Directory.
* [Accounts in Active Directory](active-directory-aadconnect-accounts-permissions.md) als u het installatiepad voor de aangepaste instellingen.

### <a name="connectivity"></a>Connectiviteit
* De Azure AD Connect-server moet DNS-omzetting voor het intranet en internet. De DNS-server moet kunnen omzetten van namen van zowel uw lokale Active Directory en de Azure AD-eindpunten.
* Als u firewalls op uw Intranet hebt en u moet poorten openen tussen de Azure AD Connect-servers en uw domeincontrollers vervolgens Zie [Azure AD Connect-poorten](active-directory-aadconnect-ports.md) voor meer informatie.
* Als uw proxy of firewall beperken welke URL's kunnen worden geopend, wordt de URL's beschreven in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) moet worden geopend.
  * Als u de Microsoft Cloud in Duitsland of de Microsoft Azure Government-cloud gebruikt, Zie [Azure AD Connect sync-service-exemplaren overwegingen](active-directory-aadconnect-instances.md) voor URL's.
* Azure AD Connect (versie 1.1.614.0 en na) maakt standaard gebruik van TLS 1.2 voor het versleutelen van communicatie tussen de synchronisatie-engine en Azure AD. Als TLS 1.2 is niet beschikbaar op het onderliggende besturingssysteem, valt Azure AD Connect incrementeel terug op oudere protocollen (TLS 1.1 en TLS 1.0). Bijvoorbeeld: Azure AD Connect uitgevoerd op Windows Server 2008 maakt gebruik van TLS 1.0 omdat Windows Server 2008 biedt geen ondersteuning TLS 1.1 en TLS 1.2.
* Voorafgaand aan versie 1.1.614.0, Azure AD Connect standaard TLS 1.0 gebruikt voor het versleutelen van communicatie tussen de synchronisatie-engine en Azure AD. Als u wilt TLS 1.2, volg de stappen in [TLS 1.2 inschakelen voor Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
* Als u een uitgaande proxyconfiguratie voor verbinding met Internet, de volgende instelling in de **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** bestand moet worden toegevoegd voor de installatiewizard en Azure AD Connect-synchronisatie te kunnen verbinding maken met Internet en Azure AD. Deze tekst moet worden opgegeven aan de onderkant van het bestand. In deze code &lt;PROXYADRESS&gt; de werkelijke IP-adres of de hostnaam proxynaam vertegenwoordigt.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Als de proxyserver moet worden geverifieerd, wordt de [-serviceaccount](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) moet zich bevinden in het domein en moet u het installatiepad voor de aangepaste instellingen om op te geven een [aangepaste serviceaccount](active-directory-aadconnect-get-started-custom.md#install-required-components). U moet ook een andere wijziging in machine.config. Met deze wijziging in machine.config reageert de wizard en de synchronisatie-engine van installatie op verificatieaanvragen van de proxyserver. In alle installatie wizardpagina's, met uitzondering van de **configureren** pagina de ondertekende in van de gebruiker referenties worden gebruikt. Op de **configureren** pagina aan het einde van de installatiewizard van de context is overgeschakeld naar de [-serviceaccount](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) die door u is gemaakt. De machine.config ziet er als volgt.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

* Wanneer Azure AD Connect een aanvraag voor het web naar Azure AD als onderdeel van directory-synchronisatie verzendt, kunnen Azure AD tot vijf minuten duren om te reageren. Het is gebruikelijk voor proxyservers verbinding time-out voor inactiviteit-configuratie. Controleer dat de configuratie is ingesteld op ten minste 6 minuten of langer.

Zie MSDN voor meer informatie over de [standaard proxy Element](https://msdn.microsoft.com/library/kd3cf2ex.aspx).  
Zie voor meer informatie wanneer u problemen met de connectiviteit hebt, [connectiviteitsproblemen oplossen](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Overige
* Optioneel: Een test gebruikersaccount om te controleren of de synchronisatie.

## <a name="component-prerequisites"></a>Vereisten voor onderdelen
### <a name="powershell-and-net-framework"></a>PowerShell en .net Framework
Azure AD Connect, hangt af van de Microsoft PowerShell en .NET Framework 4.5.1. U moet deze versie of een latere versie is geïnstalleerd op uw server. Afhankelijk van uw versie van Windows Server, het volgende doen:

* Windows Server 2012R2
  * Microsoft PowerShell wordt standaard geïnstalleerd. Er is geen actie vereist.
  * .NET framework 4.5.1 en latere versies worden aangeboden via Windows Update. Zorg ervoor dat u de meest recente updates hebt geïnstalleerd met Windows Server in het Configuratiescherm.
* Windows Server 2008R2 and Windows Server 2012
  * De nieuwste versie van Microsoft PowerShell is beschikbaar in **Windows Management Framework 4.0**, beschikbaar op [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET framework 4.5.1 en latere versies zijn beschikbaar op [Microsoft Download Center](http://www.microsoft.com/downloads).
* Windows Server 2008
  * De laatste ondersteunde versie van PowerShell is beschikbaar in **Windows Management Framework 3.0**, beschikbaar op [Microsoft Download Center](http://www.microsoft.com/downloads).
  * .NET framework 4.5.1 en latere versies zijn beschikbaar op [Microsoft Download Center](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>TLS 1.2 inschakelen voor Azure AD Connect
Voorafgaand aan versie 1.1.614.0, Azure AD Connect standaard TLS 1.0 gebruikt voor het versleutelen van de communicatie tussen de synchronisatie-engine-server en Azure AD. U kunt dit wijzigen door .net toepassingen configureren voor gebruik van TLS 1.2 standaard op de server. Meer informatie over TLS 1.2 vindt u in [Microsoft Security Advisory 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. TLS 1.2 kan niet worden ingeschakeld op Windows Server 2008. U moet Windows Server 2008 R2 of hoger. Zorg ervoor dat u de hotfix .net 4.5.1 is geïnstalleerd voor uw besturingssysteem hebben, Zie [Microsoft Security Advisory 2960358](https://technet.microsoft.com/security/advisory/2960358). U hebt deze hotfix of een latere versie is al geïnstalleerd op uw server.
2. Als u Windows Server 2008 R2 gebruikt, Controleer of dat TLS 1.2 is ingeschakeld. TLS 1.2 moeten al zijn ingeschakeld op de server met Windows Server 2012 en latere versies.
   ```
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
   ```
3. Voor alle besturingssystemen, stelt u deze registersleutel en de server opnieuw opstarten.
   ```
   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
   "SchUseStrongCrypto"=dword:00000001
   ```
4. Als u ook wilt inschakelen van TLS 1.2 tussen de synchronisatie-engine-server en een externe SQL Server en zorg ervoor dat u beschikt over de vereiste versies geïnstalleerd voor [TLS 1.2-ondersteuning voor Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Vereisten voor de federation-installatie en configuratie
### <a name="windows-remote-management"></a>Windows Remote Management
Als u Azure AD Connect om Active Directory Federation Services of de Webtoepassingsproxy te implementeren, moet u deze vereisten voldoet:

* Als de doelserver verbonden met het domein is, zorgt Windows extern beheerde is ingeschakeld
  * Gebruik in een venster met verhoogde bevoegdheid PSH opdracht de opdracht`Enable-PSRemoting –force`
* Als de doelserver is een niet-domein WAP-machine, wordt er zijn een aantal aanvullende vereisten
  * Op de doelcomputer (WAP-machine):
    * Zorg ervoor dat de winrm (Windows Remote Management / WS-Management)-service wordt uitgevoerd via de module Services
    * Gebruik in een venster met verhoogde bevoegdheid PSH opdracht de opdracht`Enable-PSRemoting –force`
  * Op de computer waarop de wizard wordt uitgevoerd (als de doelcomputer niet van het domein gekoppelde of niet-vertrouwd domein is):
    * Gebruik de opdracht in een venster met verhoogde bevoegdheid PSH opdracht`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * In Server Manager:
      * DMZ WAP host toevoegen aan Machinegroep (Serverbeheer beheren ->-Servers toevoegen... > tabblad DNS gebruiken)
      * Tabblad Serverbeheer alle Servers: klik WAP-server met de rechtermuisknop en kies beheren als..., lokale (niet domein) referenties invoeren voor de machine WAP
      * Valideren van externe PSH connectiviteit, in het tabblad Server Manager alle Servers: klik met de rechtermuisknop op WAP-server en kies Windows PowerShell. Een externe PSH-sessie geopend om te controleren of externe PowerShell-sessies kunnen worden vastgesteld.

### <a name="ssl-certificate-requirements"></a>Vereisten voor SSL-certificaten
* Het is raadzaam het SSL-certificaat gebruiken voor alle knooppunten van uw AD FS-farm en alle proxyservers van de webtoepassing.
* Het certificaat moet een X509 certificaat.
* U kunt een zelfondertekend certificaat gebruiken op federatieservers in een testomgeving. We raden echter voor een productieomgeving aan dat het certificaat te van een openbare Certificeringsinstantie verkrijgen.
  * Als een certificaat dat is niet openbaar worden vertrouwd, controleert u dat het certificaat is geïnstalleerd op elke server met Web Application Proxy vertrouwd op de lokale server en op alle federatieservers wordt
* De identiteit van het certificaat moet overeenkomen met de naam van de federation-service (bijvoorbeeld sts.contoso.com).
  * De identiteit is ofwel een onderwerp alternatieve naam (SAN)-extensie van type dNSName of, als er geen SAN-items zijn, de onderwerpnaam moet worden opgegeven als algemene naam.  
  * Meerdere vermeldingen met SAN kunnen aanwezig zijn in het certificaat opgegeven één van deze overeenkomt met de naam van de federation-service.
  * Als u van plan bent te gebruiken met Workplace Join, een extra SAN is vereist bij de waarde **enterpriseregistratie.** gevolgd door het achtervoegsel van de UPN (User Principal Name) van uw organisatie, bijvoorbeeld **: enterpriseregistration.contoso.com**.
* Certificaten op basis van de volgende generation (CNG)-sleutels CryptoAPI en sleutelarchiefproviders worden niet ondersteund. Dit betekent dat u moet een certificaat dat op basis van een CSP (cryptographic serviceprovider) en niet een KSP (sleutelarchiefprovider) gebruiken.
* Jokertekens certificaten worden ondersteund.

### <a name="name-resolution-for-federation-servers"></a>Naamomzetting voor federatieservers
* DNS-records voor de AD FS federation servicenaam (bijvoorbeeld sts.contoso.com) voor zowel het intranet (uw interne DNS-server) en het extranet (openbare DNS-server via uw domeinregistrar) instellen. Voor de intranet-DNS-record, zorg ervoor dat u A-records en geen CNAME-records. Dit is vereist voor windows-verificatie juist werkt in uw domein gekoppelde computer.
* Als u meer dan één AD FS-server of Web Application Proxy server implementeert, controleert u of u de load balancer hebt geconfigureerd en dat de DNS-records voor de naam van de AD FS federation-service (bijvoorbeeld sts.contoso.com) naar de load balancer verwijzen.
* Zorg ervoor dat de naam van de AD FS federation-service (bijvoorbeeld sts.contoso.com) is toegevoegd aan de intranetzone in Internet Explorer voor windows integrated-verificatie voor browser-toepassingen met behulp van Internet Explorer in uw intranet. Dit kan worden beheerd via Groepsbeleid en geïmplementeerd op alle computers in uw domein zijn toegevoegd.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD Connect ondersteunende onderdelen
Hier volgt een lijst van onderdelen die Azure AD Connect installeert op de server waarop Azure AD Connect is geïnstalleerd. Deze lijst is bedoeld voor een basisinstallatie van Express. Als u gebruiken van een andere SQL Server op de pagina installatie synchronisatie services wilt, vervolgens SQL Express LocalDB niet lokaal geïnstalleerd.

* Azure AD Connect Health (Engelstalig)
* Microsoft Online Services-aanmeldhulp voor IT-Professionals (geïnstalleerd, maar er is geen afhankelijkheid van het)
* Microsoft SQL Server 2012 Command Line Utilities
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 Native Client
* Microsoft Visual C++ 2013-distributiepakket

## <a name="hardware-requirements-for-azure-ad-connect"></a>Hardwarevereisten voor Azure AD Connect
De onderstaande tabel bevat de minimale vereisten voor de Azure AD Connect sync-computer.

| Het aantal objecten in Active Directory | CPU | Geheugen | Grootte van de harde schijf |
| --- | --- | --- | --- |
| Minder dan 10.000 |1,6 GHz |4 GB |70 GB |
| 10,000–50,000 |1,6 GHz |4 GB |70 GB |
| 50,000–100,000 |1,6 GHz |16 GB |100 GB |
| 100.000 of meer objecten die is de volledige versie van SQL Server vereist voor | | | |
| 100,000–300,000 |1,6 GHz |32 GB |300 GB |
| 300,000–600,000 |1,6 GHz |32 GB |450 GB |
| Meer dan 600.000 |1,6 GHz |32 GB |500 GB |

De minimale vereisten voor computers met AD FS of Webtoepassingsservers is het volgende:

* CPU: Dual core 1,6 GHz of hoger
* GEHEUGEN: 2 GB of hoger
* Azure virtuele machine: Configuratie A2 of hoger

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).

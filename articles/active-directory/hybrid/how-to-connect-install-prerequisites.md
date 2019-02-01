---
title: 'Azure AD Connect: Vereisten en hardware | Microsoft Docs'
description: Dit onderwerp beschrijft de vereisten en de hardwarevereisten voor Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 91b88fda-bca6-49a8-898f-8d906a661f07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 85b26190e16c9eb6e810c8463ce87a41fd9ad443
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55496795"
---
# <a name="prerequisites-for-azure-ad-connect"></a>Vereisten voor Azure AD Connect
Dit onderwerp beschrijft de vereisten en de hardwarevereisten voor Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Voordat u Azure AD Connect installeren
Voordat u Azure AD Connect installeert, zijn er enkele dingen die u nodig hebt.

### <a name="azure-ad"></a>Azure AD
* Een Azure AD-tenant. U krijgt een met een [gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/). U kunt een van de volgende portals voor het beheren van Azure AD Connect:
  * De [Azure-portal](https://portal.azure.com).
  * De [Office-portal](https://portal.office.com).  
* [Toevoegen en controleer of het domein](../active-directory-domains-add-azure-portal.md) u van plan bent te gebruiken in Azure AD. Bijvoorbeeld, als u van plan bent te gebruiken van contoso.com voor uw gebruikers en zorg ervoor dat dit domein is geverifieerd en u niet alleen het standaarddomein contoso.onmicrosoft.com gebruikt.
* Een Azure AD-tenant kan door 50k-standaardobjecten. Bij het controleren van uw domein, wordt de limiet wordt verhoogd tot 300 kB-objecten. Als u nog meer objecten in Azure AD, moet u een ondersteuningsaanvraag in als u de limiet voor rekenquota nog verder wilt openen. Als u meer dan 500 k-objecten, moet u een licentie, zoals Office 365, Azure AD Basic, Azure AD Premium of Enterprise Mobility and Security.

### <a name="prepare-your-on-premises-data"></a>Uw on-premises gegevens voorbereiden
* Gebruik [IdFix](https://support.office.com/article/Install-and-run-the-Office-365-IdFix-tool-f4bd2439-3e41-4169-99f6-3fabdfa326ac) voor het identificeren van fouten, zoals duplicaten en opmaak problemen in uw directory voordat u met Azure AD synchroniseert en Office 365.
* Beoordeling [optionele synchronisatie-functies u in Azure AD inschakelen kunt](how-to-connect-syncservice-features.md) en evalueren welke functies u moet inschakelen.

### <a name="on-premises-active-directory"></a>On-premises Active Directory
* Het AD-schema-versie en forest-functionaliteitsniveau moet WindowsServer 2003 of hoger. Domeincontrollers kunnen elke versie wordt uitgevoerd, zolang de schema- en forest niveau vereisten wordt voldaan.
* Als u van plan bent om de functie te gebruiken **wachtwoord terugschrijven**, en vervolgens de domeincontrollers op Windows Server 2008 R2 of hoger zijn moet.
* De domeincontroller die wordt gebruikt door Azure AD moet beschrijfbaar zijn. Het is **niet ondersteund** het gebruik van een RODC (alleen-lezen domeincontroller) en Azure AD Connect niet voldoet aan alle omleidingen schrijven.
* Is het **niet ondersteund** gebruik van on-premises forests/domeinen met behulp van "gestippeld" (de naam van een punt bevat ".") NetBIOS-namen.
* Het verdient [de Active Directory-Prullenbak inschakelen](how-to-connect-sync-recycle-bin.md).

### <a name="azure-ad-connect-server"></a>Azure AD Connect-server
* Azure AD Connect kan niet worden geïnstalleerd op Small Business Server of Windows Server Essentials voordat 2019 (Windows Server Essentials 2019 wordt ondersteund). De server moet gebruikmaken van Windows Server standard of hoger.
* De Azure AD Connect-server moet een volledige GUI geïnstalleerd hebben. Het is **niet ondersteund** te installeren op server core.
* Azure AD Connect moet worden geïnstalleerd op Windows Server 2008 R2 of hoger. Deze server kan een domeincontroller of lidserver zijn als met express-instellingen. Als u aangepaste instellingen, de server kan ook worden zelfstandige en hoeft niet te worden toegevoegd aan een domein.
* Als u Azure AD Connect op Windows Server 2008 R2 installeert, Controleer of de meest recente hotfixes toepassen van Windows Update. De installatie kan niet beginnen met een niet-gepatchte server.
* Als u van plan bent om de functie te gebruiken **Wachtwoordsynchronisatie**, en vervolgens de Azure AD Connect-server moet zich op Windows Server 2008 R2 SP1 of hoger.
* Als u van plan bent te gebruiken een **groep beheerd serviceaccount**, en vervolgens de Azure AD Connect-server moet zich op Windows Server 2012 of hoger.
* De Azure AD Connect-server moet hebben [.NET Framework 4.5.1](#component-prerequisites) of hoger en [Microsoft PowerShell 3.0](#component-prerequisites) of hoger is geïnstalleerd.
* De Azure AD Connect-server mag geen PowerShell transcriptie-Groepsbeleid is ingeschakeld.
* Als Active Directory Federation Services wordt geïmplementeerd, de servers waarop de AD FS of Web Application Proxy zijn geïnstalleerd moet Windows Server 2012 R2 of hoger. [Windows remote management](#windows-remote-management) moet zijn ingeschakeld op deze servers voor installatie op afstand.
* Als Active Directory Federation Services wordt geïmplementeerd, moet u [SSL-certificaten](#ssl-certificate-requirements).
* Als Active Directory Federation Services wordt geïmplementeerd, dan moet u configureren [naamomzetting](#name-resolution-for-federation-servers).
* Als uw globale beheerders hebben MFA ingeschakeld, klikt u vervolgens de URL **https://secure.aadcdn.microsoftonline-p.com** moet zich in de lijst met vertrouwde sites. U wordt gevraagd deze website aan de lijst met vertrouwde sites toevoegen wanneer u wordt gevraagd om een MFA-controle en deze niet is toegevoegd voordat. U kunt Internet Explorer toe te voegen aan uw vertrouwde sites.

### <a name="sql-server-used-by-azure-ad-connect"></a>SQL-Server die wordt gebruikt door Azure AD Connect
* Azure AD Connect vereist een SQL Server-database voor het opslaan van identiteitsgegevens. Een SQL Server 2012 Express LocalDB (een eenvoudige versie van SQL Server Express) wordt standaard geïnstalleerd. SQL Server Express heeft een limiet van 10GB waarmee u voor het beheren van ongeveer 100.000 objecten. Als u nodig hebt voor het beheren van een hoger aantal directory-objecten, moet u de installatiewizard verwijzen naar een andere installatie van SQL Server.
* Als u een afzonderlijke SQL Server gebruikt, klikt u vervolgens deze vereisten zijn van toepassing:
  * Azure AD Connect biedt ondersteuning voor alle versies van Microsoft SQL Server van SQL Server 2008 (met de meest recente servicepack) naar SQL Server 2017. Microsoft Azure SQL Database is **niet ondersteund** als een database.
  * U moet een niet-hoofdlettergevoelige SQL-sortering gebruiken. Deze sorteringen worden aangeduid met een \_CI_ in hun naam. Het is **niet ondersteund** voor het gebruik van een hoofdlettergevoelige sortering, geïdentificeerd door \_CS_ in hun naam.
  * U kunt slechts één synchronisatie-engine per SQL-exemplaar hebben. Het is **niet ondersteund** voor het delen van een SQL-exemplaar met FIM/MIM Sync, DirSync of Azure AD Sync.

### <a name="accounts"></a>Accounts
* Een globale beheerder van Azure AD-account voor de Azure AD-tenant die u wilt integreren in. Dit account moet een **school-of organisatieaccount** en mag niet een **Microsoft-account**.
* Als u expresinstellingen gebruiken of een van DirSync upgrade, moet u een Enterprise-beheerdersaccount hebben voor uw on-premises Active Directory.
* [Accounts in Active Directory](reference-connect-accounts-permissions.md) als u het installatiepad van aangepaste instellingen of een Enterprise-beheerder-account voor uw on-premises Active Directory.

### <a name="connectivity"></a>Connectiviteit
* De Azure AD Connect-server moet DNS-omzetting voor intranet en internet. De DNS-server moet kunnen omzetten van namen van zowel uw on-premises Active Directory en de Azure AD-eindpunten.
* Als u firewalls op het Intranet en u moet poorten openen tussen de Azure AD Connect-servers en domeincontrollers, vervolgens worden [Azure AD Connect poorten](reference-connect-ports.md) voor meer informatie.
* Als uw proxy of firewall beperken welke URL's kunnen worden geopend, wordt de URL's die zijn gedocumenteerd in [Office 365-URL's en IP-adresbereiken](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) moet worden geopend.
  * Als u de Microsoft Cloud in Duitsland of de Microsoft Azure Government-cloud gebruikt, Zie [overwegingen met betrekking tot Azure AD Connect sync-service-exemplaren](reference-connect-instances.md) voor URL's.
* Azure AD Connect (versie 1.1.614.0 en na) maakt standaard gebruik van TLS 1.2 voor het versleutelen van communicatie tussen de synchronisatie-engine en Azure AD. Als TLS 1.2 is niet beschikbaar in het onderliggende besturingssysteem, terugvalt Azure AD Connect incrementeel op het oudere protocollen (TLS 1.1 en TLS 1.0).
* Voorafgaand aan versie 1.1.614.0, Azure AD Connect standaard TLS 1.0 gebruikt voor het versleutelen van communicatie tussen de synchronisatie-engine en Azure AD. Als u wilt wijzigen in TLS 1.2, volg de stappen in [TLS 1.2 inschakelen voor Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
* Als u een uitgaande proxy om verbinding te maken met Internet, de volgende instelling in de **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** bestand moet worden toegevoegd voor de installatiewizard en Azure AD Connect-synchronisatie om te kunnen verbinding maken met Internet en Azure AD. Deze tekst moet worden opgegeven aan de onderkant van het bestand. In deze code &lt;PROXYADDRESS&gt; de werkelijke proxy IP-adres of de hostnaam naam vertegenwoordigt.

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

* Als uw proxyserver is vereist voor verificatie, dan zal de [-serviceaccount](reference-connect-accounts-permissions.md#adsync-service-account) moet zich bevinden in het domein en moet u het installatiepad van aangepaste instellingen om op te geven een [aangepast serviceaccount](how-to-connect-install-custom.md#install-required-components). U moet ook een andere wijziging in machine.config. Met deze wijziging in machine.config reageren de installatie-engine van de wizard en synchroniseren op verificatieaanvragen van de proxyserver. In alle installatie wizardpagina's, met uitzondering van de **configureren** pagina, de ondertekende in van de gebruiker referenties worden gebruikt. Op de **configureren** pagina aan het einde van de installatiewizard van de context is overgeschakeld naar de [-serviceaccount](reference-connect-accounts-permissions.md#adsync-service-account) die door u is gemaakt. De sectie machine.config moet er als volgt.

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

* Wanneer Azure AD Connect een webaanvraag naar Azure AD als onderdeel van directory-synchronisatie verzendt, is Azure AD kunnen tot vijf minuten om te reageren. Het is gebruikelijk voor proxyservers configuratie van de time-out voor inactiviteit verbinding hebben. Controleer of dat de configuratie is ingesteld op ten minste 6 minuten of langer.

Zie MSDN voor meer informatie over de [proxy Element standaard](https://msdn.microsoft.com/library/kd3cf2ex.aspx).  
Zie voor meer informatie als u problemen met de connectiviteit, [connectiviteitsproblemen](tshoot-connect-connectivity.md).

### <a name="other"></a>Overige
* Optioneel: Een gebruikersaccount voor de test om te controleren of de synchronisatie.

## <a name="component-prerequisites"></a>Vereisten voor onderdelen
### <a name="powershell-and-net-framework"></a>PowerShell en .net Framework
Azure AD Connect, is afhankelijk van Microsoft PowerShell en .NET Framework 4.5.1. U moet deze versie of een latere versie is geïnstalleerd op uw server. Afhankelijk van uw versie van Windows Server, het volgende doen:

* Windows Server 2012R2
  * Microsoft PowerShell is standaard geïnstalleerd. Geen actie vereist.
  * .NET framework 4.5.1 en latere releases worden aangeboden via Windows Update. Zorg ervoor dat u de meest recente updates hebt geïnstalleerd met Windows Server in het Configuratiescherm.
* Windows Server 2008 R2 en Windows Server 2012
  * De nieuwste versie van Microsoft PowerShell is beschikbaar in **Windows Management Framework 4.0**, beschikbaar op [Microsoft Download Center](https://www.microsoft.com/downloads).
  * .NET framework 4.5.1 en latere versies zijn beschikbaar op [Microsoft Download Center](https://www.microsoft.com/downloads).


### <a name="enable-tls-12-for-azure-ad-connect"></a>TLS 1.2 inschakelen voor Azure AD Connect
Voorafgaand aan versie 1.1.614.0, Azure AD Connect standaard TLS 1.0 gebruikt voor het versleutelen van de communicatie tussen de synchronisatie-engine-server en Azure AD. U kunt dit wijzigen door het configureren van .net-toepassingen kunnen gebruikmaken van TLS 1.2 standaard op de server. Meer informatie over TLS 1.2 kan worden gevonden [Microsoft Security Advisory 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. TLS 1.2 kan niet worden ingeschakeld voordat u Windows Server 2008 R2 of hoger. Zorg ervoor dat u de .net 4.5.1 hotfix voor uw besturingssysteem geïnstalleerd hebben, Zie [Microsoft Security Advisory 2960358](https://technet.microsoft.com/security/advisory/2960358). Mogelijk hebt u deze hotfix of een latere versie is al geïnstalleerd op uw server.
2. Als u Windows Server 2008 R2 gebruikt, Controleer of dat TLS 1.2 is ingeschakeld. TLS 1.2 moeten al zijn ingeschakeld op de server van Windows Server 2012 en latere versies.
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
4. Als u ook wilt TLS 1.2 inschakelen tussen de synchronisatie-engine-server en een externe SQL Server en zorg ervoor dat u de vereiste versies geïnstalleerd hebt voor [TLS 1.2-ondersteuning voor Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Vereisten voor de federation-installatie en configuratie
### <a name="windows-remote-management"></a>Windows Remote Management
Wanneer u Azure AD Connect gebruikt om Active Directory Federation Services of de Web Application Proxy te implementeren, Controleer de volgende vereisten:

* Als de doel-server toegevoegd aan een domein is, zorg ervoor dat Windows extern beheerde is ingeschakeld
  * Gebruik in een venster met verhoogde bevoegdheid PSH opdracht opdracht `Enable-PSRemoting –force`
* Als de doelserver is een niet-domein WAP-machine, wordt er zijn een aantal aanvullende vereisten
  * Op de doelcomputer (WAP-machine):
    * Zorg ervoor dat de winrm (Windows Remote Management / WS-Management)-service wordt uitgevoerd via de module Services
    * Gebruik in een venster met verhoogde bevoegdheid PSH opdracht opdracht `Enable-PSRemoting –force`
  * Op de computer waarop de wizard wordt uitgevoerd (als de doel-VM niet van het domein is toegevoegd aan of niet-vertrouwd domein is):
    * Gebruik de opdracht in een venster met verhoogde bevoegdheid PSH opdracht `Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
    * In Server Manager:
      * DMZ WAP-host aan de Machinegroep toevoegen (Serverbeheer -> beheren-Servers toevoegen... > tabblad DNS gebruiken)
      * Tabblad alle Servers van Serverbeheer: klik WAP-server met de rechtermuisknop en kies beheren opslaan als..., voer de referenties voor lokale (niet-domein) voor de WAP-machine
      * Voor het valideren van externe PSH verbindingen, op het tabblad alle Servers van Serverbeheer: klik WAP-server met de rechtermuisknop en kies Windows PowerShell. Een externe PSH-sessie moet openen om te controleren of externe PowerShell-sessies kunnen worden gemaakt.

### <a name="ssl-certificate-requirements"></a>Vereisten voor SSL-certificaten
* Het is raadzaam het SSL-certificaat gebruiken voor alle knooppunten van uw AD FS-farm en alle Web Application proxy-servers.
* Het certificaat moet een X509 certificaat.
* U kunt een zelfondertekend certificaat gebruiken op de federation-servers in een testomgeving. Echter, voor een productie-omgeving, wordt aangeraden dat u het certificaat van een openbare CA verkrijgen.
  * Als u een certificaat dat is niet openbaar vertrouwde, zorgt u ervoor dat het certificaat is geïnstalleerd op elke Web Application Proxy-server vertrouwd op de lokale server en op alle federatieservers wordt
* De identiteit van het certificaat moet overeenkomen met de naam van de federation-service (bijvoorbeeld sts.contoso.com).
  * De identiteit is ofwel een onderwerp alternatieve naam (SAN)-extensie van type DNS-naam of, als er geen SAN-vermeldingen zijn, de naam van het onderwerp moet worden opgegeven als algemene naam.  
  * Meerdere vermeldingen met SAN kunnen aanwezig zijn in het certificaat opgegeven een van deze overeenkomt met de naam van de federation-service.
  * Als u van plan bent te gebruiken met Workplace Join, een extra SAN is vereist bij de waarde **enterpriseregistratie.** gevolgd door het achtervoegsel van de UPN (User Principal Name) van uw organisatie, bijvoorbeeld **: enterpriseregistration.contoso.com**.
* Certificaten op basis van de volgende generatie (CNG) sleutels CryptoAPI en sleutelarchiefproviders worden niet ondersteund. Dit betekent dat u een certificaat op basis van een CSP (cryptographic serviceprovider) en niet een KSP (sleutelarchiefprovider) moet gebruiken.
* Jokertekens certificaten worden ondersteund.

### <a name="name-resolution-for-federation-servers"></a>Naamomzetting voor federatieservers
* Instellen van DNS-records voor het AD FS federation-service (bijvoorbeeld sts.contoso.com) voor zowel het intranet (uw interne DNS-server) als de extranet (openbare DNS-via uw domeinregistrar). Zorg ervoor dat u een voor de intranet-DNS-record, records en geen CNAME-records. Dit is vereist voor windows-verificatie correct te laten werken op uw computer is toegevoegd aan het domein.
* Als u meer dan één AD FS-server of Web Application Proxy server implementeert, controleer dan dat u de load balancer hebt geconfigureerd en dat de DNS-records voor de naam van de AD FS federation-service (bijvoorbeeld sts.contoso.com) aan de load balancer verwijzen.
* Voor geïntegreerde windows-verificatie om te werken voor browser-toepassingen met behulp van Internet Explorer in uw intranet, zorg ervoor dat de naam van de AD FS federation-service (bijvoorbeeld sts.contoso.com) is toegevoegd aan de zone Lokaal intranet in Internet Explorer. Dit kan worden beheerd via Groepsbeleid en geïmplementeerd voor alle computers in uw domein.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD Connect ondersteunende onderdelen
Hier volgt een lijst van onderdelen die Azure AD Connect installeert op de server waarop Azure AD Connect is geïnstalleerd. Deze lijst is voor een basisinstallatie van Express. Als u een andere SQL Server op de pagina installatie synchronisatie services gebruiken wilt, is SQL Express LocalDB niet lokaal geïnstalleerd.

* Azure AD Connect Health (Engelstalig)
* Microsoft SQL Server 2012 Command Line Utilities
* Microsoft SQL Server 2012 Express LocalDB
* Microsoft SQL Server 2012 Native Client
* Microsoft Visual C++ 2013-distributiepakket

## <a name="hardware-requirements-for-azure-ad-connect"></a>Hardwarevereisten voor Azure AD Connect
De onderstaande tabel ziet u de minimale vereisten voor de Azure AD Connect sync-computer.

| Aantal objecten in Active Directory | CPU | Geheugen | Grootte van de harde schijf |
| --- | --- | --- | --- |
| Minder dan 10.000 |1, 6 GHz |4 GB |70 GB |
| 10,000–50,000 |1, 6 GHz |4 GB |70 GB |
| 50,000–100,000 |1, 6 GHz |16 GB |100 GB |
| 100.000 of meer objecten die is de volledige versie van SQL Server vereist voor | | | |
| 100,000–300,000 |1, 6 GHz |32 GB |300 GB |
| 300,000–600,000 |1, 6 GHz |32 GB |450 GB |
| Meer dan 600.000 |1, 6 GHz |32 GB |500 GB |

De minimale vereisten voor computers met AD FS of Web Application Servers is het volgende:

* CPU: Dual-core, 1,6 GHz of hoger
* GEHEUGEN: 2 GB of hoger
* Azure-VM: Configuratie van a2 of hoger

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).

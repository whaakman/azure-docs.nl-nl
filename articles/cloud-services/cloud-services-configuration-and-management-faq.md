---
title: Problemen met configuratie en beheer voor veelgestelde vragen over Microsoft Azure Cloud Services | Microsoft Docs
description: In dit artikel geeft een lijst van de veelgestelde vragen over configuratie en beheer voor Microsoft Azure Cloud Services.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: genli
ms.openlocfilehash: fe096efba5419b283981f27e16c3e30a791412da
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322667"
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Configuratie en problemen met voor Azure Cloud Services: Veelgestelde vragen (FAQ's)

Dit artikel bevat veelgestelde vragen over problemen met configuratie en beheer voor [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). U kunt ook raadpleegt u de [VM-grootte voor Cloud Services-pagina](cloud-services-sizes-specs.md) voor informatie over de grootte.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

**Certificaten**

- [Waarom is de certificaatketen van mijn Cloud Service SSL-certificaat niet volledig?](#why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete)
- [Wat is het doel van de 'Windows Azure-hulpprogramma's versleuteling voor certificaatuitbreidingen'?](#what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions)
- [Hoe kan ik een Certificate Signing Request (CSR) zonder "RDP-ing" in genereren met het exemplaar?](#how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance)
- [Mijn Cloud Service Management-certificaat verloopt binnenkort. Hoe kunt u het om het te vernieuwen?](#my-cloud-service-management-certificate-is-expiring-how-to-renew-it)
- [Hoe kunt u de installatie van de belangrijkste SSL certificate(.pfx) en tussenliggende certificate(.p7b) automatiseren?](#how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b)
- [Wat is het doel van het certificaat 'Microsoft Azure Service Management voor MachineKey'?](#what-is-the-purpose-of-the-microsoft-azure-service-management-for-machinekey-certificate)

**Controle en logboekregistratie**

- [Wat zijn de aanstaande Service in de Cloud die in Azure portal kunt beheren en bewaken van toepassingen?](#what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications)
- [Waarom IIS stopt met schrijven naar de logboekmap?](#why-does-iis-stop-writing-to-the-log-directory)
- [Hoe schakel ik WAD logboekregistratie voor Cloud Services?](#how-do-i-enable-wad-logging-for-cloud-services)

**Netwerkconfiguratie**

- [Hoe stel ik de time-out voor inactiviteit voor Azure load balancer?](#how-do-i-set-the-idle-timeout-for-azure-load-balancer)
- [Hoe koppel ik een statisch IP-adres met mijn Cloudservice?](#how-do-i-associate-a-static-ip-address-to-my-cloud-service)
- [Wat zijn de functies en mogelijkheden die Azure basic IP-Adressen /-id's en DDOS biedt?](#what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides)
- [Het inschakelen van HTTP/2 op Cloud Services-virtuele machine?](#how-to-enable-http2-on-cloud-services-vm)

**Machtigingen**

- [Kan Microsoft Extern bureaublad naar de Cloud Service-exemplaren zonder toestemming van interne engineers?](#can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission)
- [Ik kan geen extern bureaublad voor Cloud Service-VM met behulp van het RDP-bestand. Ik u de volgende fout: Er is een verificatiefout opgetreden (Code: 0x80004005)](#i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005)

**Schalen**

- [Ik kan niet worden uitgebreid dan X exemplaren](#i-cannot-scale-beyond-x-instances)
- [Hoe kan ik automatisch schalen op basis van metrische gegevens over geheugen configureren?](#how-can-i-configure-auto-scale-based-on-memory-metrics)

**Algemene**

- [Hoe voeg ik 'nosniff' Mijn website?](#how-do-i-add-nosniff-to-my-website)
- [Hoe pas ik IIS voor een Webrol?](#how-do-i-customize-iis-for-a-web-role)
- [Wat is de quotumlimiet voor mijn Cloudservice?](#what-is-the-quota-limit-for-my-cloud-service)
- [Waarom wordt het station van mijn VM van Cloud Service weinig vrije schijfruimte weergegeven?](#why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space)
- [Hoe kan ik een anti-malware-extensie voor mijn Cloud-Services in een automatische manier toevoegen?](#how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way)
- [Het inschakelen van Server Name Indication (SNI) voor Cloud Services?](#how-to-enable-server-name-indication-sni-for-cloud-services)
- [Hoe kan ik labels toevoegen aan mijn Azure-Cloud-Service?](#how-can-i-add-tags-to-my-azure-cloud-service)
- [De Azure-portal niet wordt weergegeven de SDK-versie van de Cloudservice. Hoe vind ik dat?](#the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that)
- [Ik wil de Cloudservice voor enkele maanden afgesloten. Het verminderen van de kosten voor facturering van Cloud Service zonder verlies van het IP-adres?](#i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address)


## <a name="certificates"></a>Certificaten

### <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Waarom is de certificaatketen van mijn Cloud Service SSL-certificaat niet volledig?
    
U wordt aangeraden dat klanten de volledige certificaatketen (leaf-certificaat, tussenliggende certificaten en basiscertificaat) in plaats van het leaf-certificaat installeren. Wanneer u het leaf-certificaat installeert, wordt u afhankelijk zijn van Windows de certificaatketen kunt maken met de CTL lopen. Als er onregelmatige netwerk of de DNS-problemen in Azure of Windows Update optreden als Windows wordt geprobeerd om het certificaat te valideren, kan het certificaat ongeldig worden beschouwd. Als u de volledige certificaatketen installeert, kunt u dit probleem voorkomen. De blog op [een SSL-ketencertificaat installeren](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) laat zien hoe u dit doet.

### <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>Wat is het doel van de 'Windows Azure-hulpprogramma's versleuteling voor certificaatuitbreidingen'?

Deze certificaten worden automatisch gemaakt wanneer een uitbreiding wordt toegevoegd aan de Cloudservice. Meestal dit is de extensie WAD of de RDP-extensie, maar het anderen, zoals de extensie anti-malware of Logboekverzamelaar kan zijn. Deze certificaten worden alleen gebruikt voor het versleutelen en ontsleutelen van de persoonlijke configuratie voor de extensie. De vervaldatum is nooit ingeschakeld, zodat het maakt niet uit als het certificaat is verlopen. 

U kunt deze certificaten negeren. Als u wilt voor het opschonen van de certificaten, kunt u proberen deze alle worden verwijderd. Azure genereert een fout als u probeert te verwijderen van een certificaat dat wordt gebruikt.

### <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Hoe kan ik een Certificate Signing Request (CSR) zonder "RDP-ing" in genereren met het exemplaar?

Zie de volgende richtlijnen-document:

[Het ophalen van een certificaat voor gebruik met Windows Azure Web Sites (WAWS)](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

De certificaataanvraag is slechts een tekstbestand. Het hoeft niet te worden gemaakt op basis van de machine waar het certificaat uiteindelijk worden gebruikt. Hoewel dit document is geschreven voor een App Service, wordt het maken van de CSR is generiek en geldt ook voor Cloud Services.

### <a name="my-cloud-service-management-certificate-is-expiring-how-to-renew-it"></a>Mijn Cloud Service Management-certificaat verloopt binnenkort. Hoe kunt u het om het te vernieuwen?

De volgende PowerShell-opdrachten kunt u uw Management certificaten vernieuwen:

    Add-AzureAccount
    Select-AzureSubscription -Current -SubscriptionName <your subscription name>
    Get-AzurePublishSettingsFile

De **Get-AzurePublishSettingsFile** maakt u een nieuw beheercertificaat in **abonnement** > **Beheercertificaten** in Azure portal. De naam van het nieuwe certificaat lijkt op 'YourSubscriptionNam]-[CurrentDate] - referenties'.

### <a name="how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b"></a>Hoe kunt u de installatie van de belangrijkste SSL certificate(.pfx) en tussenliggende certificate(.p7b) automatiseren?

U kunt deze taak automatiseren met behulp van een opstartscript (batch/cmd/PowerShell) en die opstartscript registreren in het servicedefinitiebestand. Zowel het opstartscript en het certificaat (.p7b-bestand) in de projectmap van dezelfde map van het opstartscript toevoegen.

### <a name="what-is-the-purpose-of-the-microsoft-azure-service-management-for-machinekey-certificate"></a>Wat is het doel van het certificaat 'Microsoft Azure Service Management voor MachineKey'?

Dit certificaat wordt gebruikt voor het versleutelen van computersleutels op webrollen van Azure. Bekijk voor meer informatie dit advies [https://docs.microsoft.com/security-updates/securityadvisories/2018/4092731].

Raadpleeg voor meer informatie de volgende artikelen:
- [Over het configureren en uitvoeren van opstarttaken voor een Cloudservice](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks)
- [Veelvoorkomende opstarttaken voor Cloud-Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks-common)

## <a name="monitoring-and-logging"></a>Bewaking en registratie

### <a name="what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications"></a>Wat zijn de aanstaande Service in de Cloud die in Azure portal kunt beheren en bewaken van toepassingen?

Mogelijkheid om een nieuw certificaat genereren voor Remote Desktop Protocol (RDP) is binnenkort beschikbaar. U kunt ook dit script uitvoeren:

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 20 48 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```
Mogelijkheid om blob of lokale kiezen voor uw csdef- en cscfg locatie uploaden is binnenkort beschikbaar. Met behulp van [New-Azure-implementatie](/powershell/module/servicemanagement/azure/new-azuredeployment?view=azuresmps-4.0.0), kunt u de locatiewaarde van elke instellen.

De mogelijkheid om te controleren metrische gegevens op het exemplaarniveau van het. Extra bewakingsmogelijkheden zijn beschikbaar in [hoe u Cloud Services controleren](cloud-services-how-to-monitor.md).

### <a name="why-does-iis-stop-writing-to-the-log-directory"></a>Waarom IIS stopt met schrijven naar de logboekmap?
U hebt de lokale opslag van inhoud voor het schrijven naar de logboekmap volledig gebruikt. Om dit te corrigeren, kunt u een van drie dingen doen:
* Diagnostische gegevens inschakelen voor IIS en de diagnostische gegevens periodiek wordt verplaatst naar de blob storage.
* Verwijder handmatig logboekbestanden vanuit de map voor logboekregistratie.
* Verhoog de quotumlimiet voor lokale bronnen.

Zie de volgende documenten voor meer informatie:
* [Diagnostische gegevens opslaan en weergeven in Azure Storage](cloud-services-dotnet-diagnostics-storage.md)
* [IIS-logboeken stoppen met schrijven in de Cloud Service](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

### <a name="how-do-i-enable-wad-logging-for-cloud-services"></a>Hoe schakel ik WAD logboekregistratie voor Cloud Services?
U kunt logboekregistratie in Windows Azure Diagnostics (WAD) via de volgende opties inschakelen:
1. [Inschakelen vanuit Visual Studio](https://docs.microsoft.com/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them)
2. [Schakel via .net-code](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics)
3. [Schakel via Powershell](https://docs.microsoft.com/azure/cloud-services/cloud-services-diagnostics-powershell)

Om de huidige WAD-instellingen van uw Cloudservice, kunt u [Get-AzureServiceDiagnosticsExtensions](https://docs.microsoft.com/azure/cloud-services/cloud-services-diagnostics-powershell#get-current-diagnostics-extension-configuration) ps cmd of u kunt deze weergeven via de portal blade 'Cloudservices--> extensies'.


## <a name="network-configuration"></a>Netwerkconfiguratie

### <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Hoe stel ik de time-out voor inactiviteit voor Azure load balancer?
U kunt de time-out opgeven in uw programma servicedefinitiebestand (csdef) als volgt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="mgVS2015Worker" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10100"   idleTimeoutInMinutes="30" />
    </Endpoints>
  </WorkerRole>
```
Zie [nieuwe: Configureerbare Idle time-out voor Azure Load Balancer](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/) voor meer informatie.

### <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Hoe koppel ik een statisch IP-adres met mijn Cloudservice?
Als u een statisch IP-adres instelt, moet u een gereserveerde IP-adres maken. Dit gereserveerde IP-adres kan worden gekoppeld aan een nieuwe Cloudservice of aan een bestaande implementatie. Zie de volgende documenten voor meer informatie:
* [Over het maken van een gereserveerde IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [De IP-adres van een bestaande Cloudservice reserveren](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Een gereserveerde IP-adres aan een nieuwe Cloud Service koppelen](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Een gereserveerde IP-adres aan een actieve implementatie koppelen](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Een gereserveerde IP-adres aan een Cloud Service koppelen met behulp van een service-configuratiebestand](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

### <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Wat zijn de functies en mogelijkheden die Azure basic IP-Adressen /-id's en DDOS biedt?
Azure heeft IP-Adressen /-id's in de fysieke servers voor het datacenter tot verdediging tegen bedreigingen. Klanten kunnen bovendien van derden beveiligingsoplossingen, zoals firewalls voor webtoepassingen, netwerkfirewalls, anti-malware, inbraakdetectie, preventie-systemen (IDS/IPS) en meer implementeren. Zie voor meer informatie, [Bescherm uw gegevens en assets en voldoen aan de standaarden van de algemene beveiliging](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft controleert voortdurend servers, netwerken en toepassingen voor het detecteren van bedreigingen. Azure multipronged beveiligingsbeheer benadering gebruikt inbraakdetectie, distributed denial-of-service (DDoS) aanvalbeveiliging, indringingstests testen, gedragsalgoritmen analytics, detectie van afwijkingen en machine learning om voortdurend de beveiliging versterken en risico's verminderen. Microsoft Antimalware voor Azure beveiligt Azure Cloud Services en virtual machines. U hebt de optie voor het implementeren van beveiligingsoplossingen van derden bovendien zoals web application fire wanden, netwerkfirewalls, anti-malware, indringing detectie en preventie van systemen (IDS/IPS) en meer.

### <a name="how-to-enable-http2-on-cloud-services-vm"></a>Het inschakelen van HTTP/2 op Cloud Services-virtuele machine?

Windows 10 en Windows Server 2016 worden geleverd met ondersteuning voor HTTP/2 zowel client als server aan. Als uw (browser)-client verbinding maakt met de IIS-server via TLS die onderhandelt over HTTP/2 via TLS-uitbreidingen, wordt u niet wilt een wijziging aanbrengt aan de serverzijde. Dit komt doordat via TLS, de h2-14-header op te geven gebruik van HTTP/2 worden standaard verzonden. Als de client daarentegen een Upgrade-header om te upgraden naar HTTP/2 verzendt wordt, moet u de wijziging onder aan de serverzijde om ervoor te zorgen dat de Upgrade werkt en u uiteindelijk een HTTP/2-verbinding maken. 

1. Regedit.exe worden uitgevoerd.
2. Blader naar de registersleutel: Hkey_local_machine\software\microsoft\cryptography\mscep.
3. Maak een nieuwe DWORD-waarde met de naam **DuoEnabled**.
4. Stel de waarde ervan in op 1.
5. Start de server opnieuw.
6. Ga naar uw **Default Web Site** en klikt u onder **bindingen**, maakt u een nieuwe TLS-binding met de zelf-ondertekend certificaat zojuist hebt gemaakt. 

Zie voor meer informatie:

- [HTTP/2 op IIS](https://blogs.iis.net/davidso/http2)
- [Video: HTTP/2 in Windows 10: Browser, Apps en Web-Server](https://channel9.msdn.com/Events/Build/2015/3-88)
         

Deze stappen kunnen worden geautomatiseerd via een opstarttaak, zodat wanneer een nieuw PaaS-exemplaar wordt gemaakt, wordt de wijzigingen boven in het register kunt doen. Zie voor meer informatie, [over het configureren en uitvoeren van opstarttaken voor een Cloudservice](cloud-services-startup-tasks.md).

 
Zodra dit is gedaan, kunt u controleren of de HTTP/2 is ingeschakeld of niet met behulp van een van de volgende methoden:

- Protocolversie in IIS-logboeken inschakelen en zoek naar de IIS-logboeken. HTTP/2 wordt het weergegeven in de logboeken. 
- Schakel F12 Tool voor ontwikkelaars in Internet Explorer/Edge en Ga naar het tabblad netwerk om te controleren of het protocol. 

Zie voor meer informatie, [HTTP/2 op IIS](https://blogs.iis.net/davidso/http2).

## <a name="permissions"></a>Machtigingen

### <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Hoe implementeer ik rollen gebaseerde toegang voor Cloud Services?
Cloudservices biedt geen ondersteuning voor het model voor het toegangsbeheer (RBAC) op basis van de rol, omdat dit niet een op basis van Azure Resource Manager-service.

Zie [inzicht in de verschillende rollen in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).

## <a name="remote-desktop"></a>Extern bureaublad

### <a name="can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission"></a>Kan Microsoft Extern bureaublad naar de Cloud Service-exemplaren zonder toestemming van interne engineers?
Microsoft volgt een strikte proces dat niet toe interne technici met extern bureaublad in uw Cloud-services zonder schriftelijke toestemming (bijvoorbeeld e-mail of andere geschreven communicatie dat) van de eigenaar of de door hun benoemde persoon.

### <a name="i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005"></a>Ik kan geen extern bureaublad voor Cloud Service-VM met behulp van het RDP-bestand. Ik u de volgende fout: Er is een verificatiefout opgetreden (Code: 0x80004005)

Deze fout kan optreden als u het RDP-bestand van een virtuele machine die is gekoppeld aan Azure Active Directory. U lost dit probleem, de volgende stappen uit:

1. Met de rechtermuisknop op het gedownloade RDP-bestand en selecteer vervolgens **bewerken**.
2. Voeg toe '&#92;' als voorvoegsel voor de gebruikersnaam. Gebruik bijvoorbeeld **. \username** in plaats van **gebruikersnaam**.

## <a name="scaling"></a>Schalen

### <a name="i-cannot-scale-beyond-x-instances"></a>Ik kan niet worden uitgebreid dan X exemplaren
Uw Azure-abonnement geldt een limiet voor het aantal kernen dat u kunt gebruiken. Schalen werkt niet als u alle beschikbare kerngeheugens hebt gebruikt. Bijvoorbeeld, als er een limiet van 100 kernen, betekent dit u 100 A1 grootte voor de exemplaren van virtuele machine kan hebben voor je Cloud Service, of 50 A2 grote instanties van virtuele machines.

### <a name="how-can-i-configure-auto-scale-based-on-memory-metrics"></a>Hoe kan ik automatisch schalen op basis van metrische gegevens over geheugen configureren?

Automatisch schalen op basis van metrische gegevens van geheugen voor een Cloud-Services is momenteel niet ondersteund. 

U kunt dit probleem omzeilen, kunt u Application Insights. Automatisch schalen biedt ondersteuning voor Application Insights als een bron metrische gegevens en het aantal rolinstanties op basis van Gast meetwaarde, zoals 'Geheugen' kan worden geschaald.  U hebt voor het configureren van Application Insights in uw Cloud Service-project-pakketbestand (*.cspkg) en Azure Diagnostics-extensie van de service voor het implementeren van deze functie inschakelen.

Zie voor meer informatie over het gebruik van een aangepaste meetwaarde via Application Insights voor het configureren van automatisch schalen op Cloud Services, [aan de slag met automatisch schalen door aangepaste metrische gegevens in Azure](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md)

Zie voor meer informatie over het integreren van Azure Diagnostics met Application Insights voor Cloud Services [verzenden Cloud Service, virtuele Machine of Service Fabric diagnostische gegevens naar Application Insights](../azure-monitor/platform/diagnostics-extension-to-application-insights.md)

Zie voor meer informatie over het inschakelen van Application Insights voor Cloud Services, [Application Insights voor Azure Cloud Services](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices)

Zie voor meer informatie over het inschakelen van logboekregistratie van Azure Diagnostics voor Cloud Services [diagnose instellen voor Azure Cloud Services en virtuele machines](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them)

## <a name="generic"></a>Algemene

### <a name="how-do-i-add-nosniff-to-my-website"></a>Hoe voeg ik 'nosniff' Mijn website?
Om te voorkomen dat clients de MIME-typen bekijken, voegt u een instelling in uw *web.config* bestand.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

U kunt dit ook toevoegen als een instelling in IIS. Gebruik de volgende opdracht met de [veelvoorkomende opstarttaken](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="how-do-i-customize-iis-for-a-web-role"></a>Hoe pas ik IIS voor een Webrol?
Het script voor het opstarten van IIS uit de [veelvoorkomende opstarttaken](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

### <a name="what-is-the-quota-limit-for-my-cloud-service"></a>Wat is de quotumlimiet voor mijn Cloudservice?
Zie [servicespecifieke beperkt](../azure-subscription-service-limits.md#subscription-limits).

### <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Waarom wordt het station van mijn VM van Cloud Service weinig vrije schijfruimte weergegeven?
Dit is normaal en dit mag niet leiden tot een probleem aan uw toepassing. Logboekregistratie is ingeschakeld voor het station % approot % in Azure PaaS-VM's, waarvoor de dubbele hoeveelheid ruimte die bestanden normaal gesproken duren in wezen worden verbruikt. Maar er zijn verschillende dingen die u moet rekening houden met die in wezen kunt u deze uitschakelen in een niet-probleem.

De grootte van % approot % de schijf wordt dan berekend als < grootte van .cspkg + maximale logboekgrootte > + marge vrije ruimte of 1,5 GB, afhankelijk van welke periode groter is. De grootte van uw virtuele machine heeft geen gevolgen voor deze berekening. (De VM-grootte alleen van invloed op de grootte van het tijdelijke station C:.) 

Het wordt niet ondersteund voor het schrijven naar het station % approot %. Als u voor Azure VM ontwikkelt, moet u dit doen in een tijdelijke LocalStorage-resource (of een andere optie, zoals opslag-Blob, Azure Files, enz.). De hoeveelheid vrije ruimte op de map % approot % is dus niet betekenis. Als u niet zeker weet of uw toepassing wordt geschreven naar het station % approot %, altijd kunt u uw service voor een paar dagen uitvoeren en vergelijk de 'voor' en 'na'-grootten. 

Azure wordt alles wat niet schrijven naar het station % approot %. Nadat de VHD is gemaakt op basis van uw .cspkg en in de Azure-VM is gekoppeld, is het enige wat die naar dit station kan schrijven in uw toepassing. 

De wijzigingslogboek-instellingen zijn niet-configureerbare, zodat u deze niet uitschakelen.

### <a name="how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way"></a>Hoe kan ik een anti-malware-extensie voor mijn Cloud-Services in een automatische manier toevoegen?

U kunt met behulp van PowerShell-script in de Opstarttaak anti-malware-extensie inschakelen. Volg de stappen in deze artikelen om dit te implementeren: 
 
- [Een opstarttaak PowerShell maken](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)
- [Set-AzureServiceAntimalwareExtension](https://docs.microsoft.com/powershell/module/servicemanagement/azure/Set-AzureServiceAntimalwareExtension?view=azuresmps-4.0.0 )

Zie voor meer informatie over implementatiescenario's voor Antimalware en hoe u dit inschakelen via de portal [implementatiescenario's voor anti-malware](../security/azure-security-antimalware.md#antimalware-deployment-scenarios).

### <a name="how-to-enable-server-name-indication-sni-for-cloud-services"></a>Het inschakelen van Server Name Indication (SNI) voor Cloud Services?

U kunt SNI in Cloud Services inschakelen met behulp van een van de volgende methoden:

**Methode 1: PowerShell gebruiken**

De SNI-binding kan worden geconfigureerd met behulp van de PowerShell-cmdlet **New-WebBinding** in een opstarttaak voor een Cloudservice-rolinstantie zoals hieronder:
    
    New-WebBinding -Name $WebsiteName -Protocol "https" -Port 443 -IPAddress $IPAddress -HostHeader $HostHeader -SslFlags $sslFlags 
    
Zoals wordt beschreven [hier](https://technet.microsoft.com/library/ee790567.aspx), de $sslFlags mogelijk een van de waarden als het volgende:

|Waarde|Betekenis|
------|------
|0|Er is geen SNI|
|1|SNI ingeschakeld |
|2 |Niet SNI-binding die gebruikmaakt van centrale certificaat Store|
|3|SNI-binding die gebruikmaakt van centrale certificaat opslaan |
 
**Methode 2: Gebruik code**

De SNI-binding kan ook worden geconfigureerd via de code in het opstarten van de rol, zoals beschreven op deze [blogbericht](https://blogs.msdn.microsoft.com/jianwu/2014/12/17/expose-ssl-service-to-multi-domains-from-the-same-cloud-service/):

    
    //<code snip> 
                    var serverManager = new ServerManager(); 
                    var site = serverManager.Sites[0]; 
                    var binding = site.Bindings.Add(“:443:www.test1.com”, newCert.GetCertHash(), “My”); 
                    binding.SetAttributeValue(“sslFlags”, 1); //enables the SNI 
                    serverManager.CommitChanges(); 
    //</code snip> 
    
Met behulp van een van de methoden hierboven, moeten de respectieve certificaten (*.pfx) voor de specifieke hostnamen eerst worden geïnstalleerd op de rolinstanties met behulp van een opstarttaak of via programmacode in de volgorde voor de SNI-binding om te worden van kracht.

### <a name="how-can-i-add-tags-to-my-azure-cloud-service"></a>Hoe kan ik labels toevoegen aan mijn Azure-Cloud-Service? 

Cloudservice is een klassieke resource. Alleen resources die zijn gemaakt via Azure Resource Manager support-tags. U kunt geen tags toepassen op klassieke resources, zoals Service in de Cloud. 

### <a name="the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that"></a>De Azure-portal niet wordt weergegeven de SDK-versie van de Cloudservice. Hoe vind ik dat?

We werken eraan deze functie beschikbaar in Azure portal. U kunt in de tussentijd zorgen, de volgende PowerShell-opdrachten gebruiken om op te halen van de SDK-versie:

    Get-AzureService -ServiceName "<Cloud Service name>" | Get-AzureDeployment | Where-Object -Property SdkVersion -NE -Value "" | select ServiceName,SdkVersion,OSVersion,Slot

### <a name="i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address"></a>Ik wil de Cloudservice voor enkele maanden afgesloten. Het verminderen van de kosten voor facturering van Cloud Service zonder verlies van het IP-adres?

Een reeds geïmplementeerde Service in de Cloud wordt in rekening gebracht voor de rekenkracht en opslag die wordt gebruikt. Dus zelfs als u de Azure-VM wordt afgesloten, nog steeds gefactureerd voor de opslag. 

Dit is wat u kunt doen om te beperken van uw facturering zonder verlies van het IP-adres voor uw service:

1. [Het IP-adres reserveren](../virtual-network/virtual-networks-reserved-public-ip.md) voordat u de implementaties verwijderen.  U wordt enkel gefactureerd voor dit IP-adres. Zie voor meer informatie over IP-adres facturering [IP-adressen prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/).
2. Verwijder de implementaties. Verwijder niet de xxx.cloudapp.net, zodat u deze voor de toekomst gebruiken kunt.
3. Als u wilt de Service in de Cloud implementeren met behulp van het dezelfde gereserveerde IP-adres dat u gereserveerd in uw abonnement, raadpleegt u [gereserveerde IP-adressen voor Cloudservices en virtuele Machines](https://azure.microsoft.com/blog/reserved-ip-addresses/).


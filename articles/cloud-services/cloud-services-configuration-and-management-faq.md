---
title: Configureren en beheren van problemen voor veelgestelde vragen over Microsoft Azure Cloud Services | Microsoft Docs
description: Dit artikel worden de veelgestelde vragen over het configureren en beheren voor Microsoft Azure Cloud Services.
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: genli
ms.openlocfilehash: 916fbb436806c64ded9ebf9fdd9c57c42d0809f0
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Configureren en beheren van problemen voor Azure Cloud Services: veelgestelde vragen (FAQ's)

Dit artikel bevat veelgestelde vragen over het configureren en beheren van problemen voor [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). U kunt ook raadpleegt u de [VM-grootte voor Cloud Services-pagina](cloud-services-sizes-specs.md) voor informatie over de grootte.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

**Certificaten**

- [Waarom is de certificaatketen van mijn Cloud Service SSL-certificaat onvolledige?](#why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete)
- [Wat is het doel van de 'Windows Azure hulpprogramma's voor versleuteling voor certificaatuitbreidingen'?](#what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions)
- [Hoe kan ik een Certificate Signing Request (CSR) zonder 'RDP-ing' met het exemplaar genereren?](#how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance)
- [Mijn Cloud Service Management-certificaat verloopt. Hoe het te vernieuwen?](#my-cloud-service-management-certificate-is-expiring-how-to-renew-it)
- [Hoe kunt u de installatie van de belangrijkste SSL-certificaat (.pfx) en tussenliggende certificate(.p7b) automatiseren?](#how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b)

**Controle en logboekregistratie**

- [Wat zijn de toekomstige Cloudservice die in de Azure portal waarmee kunt beheren en bewaken van toepassingen?](#what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications)
- [Waarom stopt IIS schrijven naar de logboekmap?](#why-does-iis-stop-writing-to-the-log-directory)

**Netwerkconfiguratie**

- [Hoe stel de time-out voor inactiviteit voor Azure load balancer?](#how-do-i-set-the-idle-timeout-for-azure-load-balancer)
- [Hoe koppel ik een statisch IP-adres naar Mijn Cloudservice](#how-do-i-associate-a-static-ip-address-to-my-cloud-service)
- [Wat zijn de functies en mogelijkheden die Azure basic IP-Adressen/id's en DDOS biedt?](#what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides)
- [Het HTTP/2 in de Cloud Services VM inschakelen?](#how-to-enable-http2-on-cloud-services-vm)

**Machtigingen**

- [Kan Microsoft Extern bureaublad voor Cloud Service-exemplaren zonder toestemming van interne engineers?](#can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission)
- [Ik kan geen extern bureaublad voor Cloud Service-VM met behulp van het RDP-bestand. Ik ophalen volgende fout: Er is een verificatiefout opgetreden (Code: 0x80004005)](#i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005)

**Schalen**

- [Ik kan niet worden uitgebreid dan X exemplaren](#i-cannot-scale-beyond-x-instances)
- [Hoe kan ik automatisch geschaald op basis van geheugen metrische gegevens configureren?](#how-can-i-configure-auto-scale-based-on-memory-metrics)

**Algemene**

- [Hoe kan ik 'nosniff' naar mijn website toevoegen](#how-do-i-add-nosniff-to-my-website)
- [Hoe pas IIS voor een Webrol?](#how-do-i-customize-iis-for-a-web-role)
- [Wat is de quotalimiet voor mijn Cloudservice?](#what-is-the-quota-limit-for-my-cloud-service)
- [Waarom wordt zeer weinig vrije schijfruimte weergegeven in het station op mijn VM Cloud-Service?](#why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space)
- [Hoe kan ik een Antimalware-extensie voor mijn Cloudservices in een automatische manier toevoegen?](#how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way)
- [Het inschakelen van de Server de naam van vermelding (SNI) voor Cloudservices?](#how-to-enable-server-name-indication-sni-for-cloud-services)
- [Hoe kan ik labels toevoegen aan mijn Azure Cloud Service?](#how-can-i-add-tags-to-my-azure-cloud-service)
- [De Azure-portal kan de SDK-versie van de Cloudservice wordt niet weergegeven. Hoe kan ik die krijgen?](#the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that)
- [Ik wil de Cloudservice enkele maanden afgesloten. Hoe kan ik de facturering toegangskosten van Cloudservice zonder verlies van het IP-adres?](#i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address)


## <a name="certificates"></a>Certificaten

### <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Waarom is de certificaatketen van mijn Cloud Service SSL-certificaat onvolledige?
    
Het is raadzaam om de volledige certificaatketen (leaf-certificaat, tussenliggende certificaten en hoofdmap cert) in plaats van het leaf-certificaat te installeren. Wanneer u het leaf-certificaat installeert, wordt u op Windows de certificaatketen bouwen door roulatie van de CTL vertrouwen. Als onregelmatige netwerk- of DNS-problemen in Azure of Windows Update optreden wanneer Windows probeert het certificaat te valideren, kan het certificaat ongeldig worden beschouwd. Als u de volledige certificaatketen installeert, kunt u dit probleem voorkomen. Het blog op [een keten SSL-certificaat installeren](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) ziet u hoe u dit doet.

### <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>Wat is het doel van de 'Windows Azure hulpprogramma's voor versleuteling voor certificaatuitbreidingen'?

Deze certificaten worden automatisch gemaakt wanneer er wordt een extensie toegevoegd aan de Cloudservice. Meestal betreft dit is de extensie af of de RDP-uitbreiding, maar het anderen, zoals de extensie Antimalware of Logboekverzamelaar mogelijk. Deze certificaten worden alleen gebruikt voor het versleutelen en ontsleutelen van de persoonlijke configuratie voor de extensie. De verloopdatum wordt nooit gecontroleerd, het maakt niet uit als het certificaat is verlopen. 

U kunt deze certificaten negeren. Als u wilt om de certificaten op te schonen, kunt u proberen deze alle worden verwijderd. Azure genereert een fout als u probeert te verwijderen van een certificaat dat wordt gebruikt.

### <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Hoe kan ik een Certificate Signing Request (CSR) zonder 'RDP-ing' met het exemplaar genereren?

Zie de volgende richtlijnen-document:

[Verkrijgen van een certificaat voor gebruik met Windows Azure websites (WAWS)](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

De certificaataanvraag is een tekstbestand. Dit hoeft niet te worden gemaakt van de machine waarop het certificaat uiteindelijk zal worden gebruikt. Hoewel dit document is geschreven voor een App Service, wordt het maken van de CSR is algemeen en geldt ook voor Cloud-Services.

### <a name="my-cloud-service-management-certificate-is-expiring-how-to-renew-it"></a>Mijn Cloud Service Management-certificaat verloopt. Hoe het te vernieuwen?

U kunt volgende PowerShell-opdrachten gebruiken voor het vernieuwen van certificaten voor uw:

    Add-AzureAccount
    Select-AzureSubscription -Current -SubscriptionName <your subscription name>
    Get-AzurePublishSettingsFile

De **Get-AzurePublishSettingsFile** maakt een nieuw beheercertificaat in **abonnement** > **Beheercertificaten** in de Azure portal. De naam van het nieuwe certificaat eruit als 'YourSubscriptionNam]-[CurrentDate] - referenties'.

### <a name="how-to-automate-the-installation-of-main-ssl-certificatepfx-and-intermediate-certificatep7b"></a>Hoe kunt u de installatie van de belangrijkste SSL-certificaat (.pfx) en tussenliggende certificate(.p7b) automatiseren?

U kunt deze taak automatiseren met behulp van een opstartscript (cmd-batch/PowerShell) en die opstartscript registreren in het servicedefinitiebestand. Zowel het opstartscript en het certificaat (.p7b-bestand) in de projectmap van dezelfde map van het opstartscript toevoegen.

Raadpleeg voor meer informatie de volgende artikelen:
- [Het configureren en starten van de taken uitvoeren voor een Cloudservice](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks)
- [Algemene taken voor Cloud-Service starten](https://docs.microsoft.com/azure/cloud-services/cloud-services-startup-tasks-common)

## <a name="monitoring-and-logging"></a>Controle en logboekregistratie

### <a name="what-are-the-upcoming-cloud-service-capabilities-in-the-azure-portal-which-can-help-manage-and-monitor-applications"></a>Wat zijn de toekomstige Cloudservice die in de Azure portal waarmee kunt beheren en bewaken van toepassingen?

Een nieuw certificaat genereren voor Remote Desktop Protocol (RDP) is binnenkort beschikbaar. U kunt ook dit script uitvoeren:

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 20 48 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```
Mogelijkheid om te kiezen blob of lokale voor csdef- en cscfg locatie uploaden is binnenkort beschikbaar. Met behulp van [nieuw AzureDeployment](/powershell/module/azure/new-azuredeployment?view=azuresmps-4.0.0), kunt u elke locatie een waarde instellen.

De mogelijkheid voor het bewaken van metrische gegevens op het niveau van het exemplaar. Extra bewakingsmogelijkheden zijn beschikbaar in [hoe Monitor Cloudservices](cloud-services-how-to-monitor.md).

### <a name="why-does-iis-stop-writing-to-the-log-directory"></a>Waarom stopt IIS schrijven naar de logboekmap?
Het quotum voor lokale opslag voor het schrijven naar de logboekmap is verbruikt. Om dit te corrigeren, kunt u een van drie dingen doen:
* Diagnostische gegevens inschakelen voor IIS en de diagnostische gegevens periodiek wordt verplaatst naar de blob storage.
* Logboekbestanden handmatig verwijderen uit de map voor logboekregistratie.
* Verhoog de quotalimiet voor lokale bronnen.

Zie de volgende documenten voor meer informatie:
* [Diagnostische gegevens opslaan en weergeven in Azure Storage](cloud-services-dotnet-diagnostics-storage.md)
* [IIS-logboeken stoppen schrijven in de Cloud-Service](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

## <a name="network-configuration"></a>Netwerkconfiguratie

### <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Hoe stel de time-out voor inactiviteit voor Azure load balancer?
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
Zie [nieuw: configureerbare Idle Timeout voor Azure Load Balancer](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/) voor meer informatie.

### <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Hoe koppel ik een statisch IP-adres naar Mijn Cloudservice
Als u een statisch IP-adres instelt, moet u een gereserveerde IP-adres maken. Dit gereserveerde IP-adres kan worden gekoppeld aan een nieuwe Cloudservice of aan een bestaande implementatie. Zie de volgende documenten voor meer informatie:
* [Het maken van een gereserveerd IP-adres](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Het IP-adres van een bestaande Cloudservice reserveren](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Koppelen van een gereserveerd IP-adres op een nieuwe Cloudservice](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Koppelen van een gereserveerd IP-adres aan een actieve implementatie](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Koppelen van een gereserveerd IP-adres op een Cloudservice met behulp van een service-configuratiebestand](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

### <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Wat zijn de functies en mogelijkheden die Azure basic IP-Adressen/id's en DDOS biedt?
Azure heeft IP-Adressen/id's in het datacenter fysieke servers om te beschermen tegen bedreigingen. Klanten kunnen bovendien beveiligingsoplossingen voor de van derden, zoals web application firewalls, netwerkfirewalls anti-malware, inbraakdetectie, preventie systemen (id's / IP-Adressen) en meer implementeren. Zie voor meer informatie [beveiligen van uw gegevens en activa en voldoen aan de globale beveiligingsstandaarden](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft controleert doorlopend servers, netwerken en toepassingen voor het detecteren van bedreigingen. Azure multipronged threat-management benadering gebruikt inbraakdetectie, gedistribueerde denial-of-service (DDoS) aanvallen voorkomen, binnendringen testen, gebruikersgedrag analytics, afwijkingsdetectie en machine learning-om voortdurend de beveiliging te versterken en risico's te verminderen. Microsoft Antimalware voor Azure beveiligt Azure Cloud Services en virtuele machines. U hebt de optie voor het implementeren van oplossingen van derden bovendien zoals web application fire wanden netwerkfirewalls, anti-malware, inbraakdetectie detectie en preventie van systemen (id's / IP-Adressen) en meer.

### <a name="how-to-enable-http2-on-cloud-services-vm"></a>Het HTTP/2 in de Cloud Services VM inschakelen?

Windows 10 en Windows Server 2016 worden geleverd met ondersteuning voor HTTP/2-zijde zowel client als server. Als uw (browser)-client verbinding maakt met de IIS-server via TLS die wordt onderhandeld over HTTP/2 via TLS-uitbreidingen en u niet wilt dat een wijziging aanbrengt aan de serverzijde. Dit is omdat via TLS, de h2-14-header gebruik van HTTP/2 geven standaard is verzonden. Als de client daarentegen een Upgrade verzendt voor een upgrade naar HTTP/2-header, moet u de wijziging moet hieronder op de server om ervoor te zorgen dat de Upgrade werkt en u uiteindelijk met een HTTP-/ 2-verbinding eindigen. 

1. Regedit.exe worden uitgevoerd.
2. Blader naar de registersleutel: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters.
3. Maak een nieuwe DWORD-waarde met de naam **DuoEnabled**.
4. Stel de waarde in op 1.
5. De server opnieuw.
6. Ga naar uw **standaardwebsite** en klikt u onder **bindingen**, maakt u een nieuwe TLS-binding met het zelfondertekende certificaat zojuist hebt gemaakt. 

Zie voor meer informatie:

- [HTTP/2 op IIS](https://blogs.iis.net/davidso/http2)
- [Video: HTTP/2 in Windows 10: Browser, Apps en webserver](https://channel9.msdn.com/Events/Build/2015/3-88)
         

Deze stappen kunnen worden geautomatiseerd via een taak starten, zodat dit telkens wanneer een nieuw PaaS-exemplaar wordt gemaakt, de wijzigingen boven in het register kan worden gedaan. Zie voor meer informatie [configureren en starten van de taken uitvoeren voor een Cloudservice](cloud-services-startup-tasks.md).

 
Zodra dit is gedaan, kunt u controleren of de HTTP/2 is ingeschakeld of niet via een van de volgende methoden:

- Protocolversie in IIS-logboeken inschakelen en zoek naar de IIS-logboeken. HTTP/2 wordt het weergegeven in de logboeken. 
- Schakel F12 ontwikkelprogramma in Internet Explorer/rand en overschakelen naar het tabblad netwerk om te controleren of het protocol. 

Zie voor meer informatie [HTTP/2 op IIS](https://blogs.iis.net/davidso/http2).

## <a name="permissions"></a>Machtigingen

### <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Hoe implementeer ik toegang op basis van rollen voor Cloudservices
Cloud-Services biedt geen ondersteuning voor het model op rollen gebaseerde toegangsbeheer (RBAC), omdat dit geen een op basis van Azure Resource Manager-service.

Zie [Azure RBAC versus klassieke abonnementbeheerders](../active-directory/role-based-access-control-what-is.md#azure-rbac-vs-classic-subscription-administrators).

## <a name="remote-desktop"></a>Extern bureaublad

### <a name="can-microsoft-internal-engineers-remote-desktop-to-cloud-service-instances-without-permission"></a>Kan Microsoft Extern bureaublad voor Cloud Service-exemplaren zonder toestemming van interne engineers?
Microsoft volgt een strikte proces dat niet toe interne technici met extern bureaublad in uw Cloudservice zonder schriftelijke toestemming (e-mailadres of andere geschreven communicatie dat staat) uit de eigenaar of de door hun benoemde persoon.

### <a name="i-cannot-remote-desktop-to-cloud-service-vm--by-using-the-rdp-file-i-get-following-error-an-authentication-error-has-occurred-code-0x80004005"></a>Ik kan geen extern bureaublad voor Cloud Service-VM met behulp van het RDP-bestand. Ik ophalen volgende fout: Er is een verificatiefout opgetreden (Code: 0x80004005)

Deze fout kan optreden als u het RDP-bestand van een machine die is gekoppeld aan Azure Active Directory. U lost dit probleem door de volgende stappen uit:

1. Met de rechtermuisknop op het RDP-bestand dat u hebt gedownload en selecteer vervolgens **bewerken**.
2. Voeg ' &#92; ' als voorvoegsel voordat de gebruikersnaam. Bijvoorbeeld: **. \username** in plaats van **gebruikersnaam**.

## <a name="scaling"></a>Schalen

### <a name="i-cannot-scale-beyond-x-instances"></a>Ik kan niet worden uitgebreid dan X exemplaren
Uw Azure-abonnement heeft een limiet van het aantal kernen dat u kunt gebruiken. Schalen werkt niet als u de beschikbare kernen hebt gebruikt. Bijvoorbeeld, als er een limiet van 100 kernen, betekent dit dat u exemplaren van de virtuele machine 100 A1 grootte voor de Cloudservice kunnen hebben, of 50 A2 grootte exemplaren van de virtuele machine.

### <a name="how-can-i-configure-auto-scale-based-on-memory-metrics"></a>Hoe kan ik automatisch geschaald op basis van geheugen metrische gegevens configureren?

Automatisch geschaald op basis van geheugen metrische gegevens voor een Cloud-Services is momenteel niet ondersteund. 

U kunt dit probleem omzeilen, kunt u Application Insights. Automatisch schalen ondersteunt Application Insights als bron van de metrische gegevens en het aantal rolinstanties dat op basis van de Gast metriek zoals 'Geheugen' kan worden geschaald.  U moet Application Insights configureren in uw Cloud Service-project-pakketbestand (*.cspkg) en Azure Diagnostics-extensie van de service voor het implementeren van deze functie inschakelen.

Zie voor meer informatie over het gebruik van een aangepaste waarde die via de Application Insights automatisch schalen configureren op Cloudservices [aan de slag met automatisch schalen door aangepaste metrische gegevens in Azure](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md)

Zie voor meer informatie over het integreren van Azure Diagnostics met Application Insights voor Cloudservices [Cloudservice verzenden, virtuele Machine of Service Fabric diagnostische gegevens naar Application Insights](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md)

Zie voor meer informatie over het inschakelen van Application Insights voor Cloud Services, [Application Insights voor Azure Cloud Services](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices)

Zie voor meer informatie over het inschakelen van logboekregistratie van Azure Diagnostics voor Cloudservices [diagnostics instellen voor Azure Cloud Services en virtuele machines](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md#turn-on-diagnostics-in-cloud-service-projects-before-you-deploy-them)

## <a name="generic"></a>Algemene

### <a name="how-do-i-add-nosniff-to-my-website"></a>Hoe kan ik 'nosniff' naar mijn website toevoegen
Voeg een instelling in om te voorkomen dat clients de MIME-typen voor het bewaken van netwerken voor uw *web.config* bestand.

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

U kunt dit ook toevoegen als een instelling in IIS. Gebruik de volgende opdracht met de [algemene beheertaken voor opstarten](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="how-do-i-customize-iis-for-a-web-role"></a>Hoe pas IIS voor een Webrol?
Het script voor het opstarten van IIS uit de [algemene beheertaken voor opstarten](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) artikel.

### <a name="what-is-the-quota-limit-for-my-cloud-service"></a>Wat is de quotalimiet voor mijn Cloudservice?
Zie [servicespecifieke beperkt](../azure-subscription-service-limits.md#subscription-limits).

### <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Waarom wordt zeer weinig vrije schijfruimte weergegeven in het station op mijn VM Cloud-Service?
Dit is verwacht gedrag en mag niet een probleem voor uw toepassing leiden. Logboekregistratie is ingeschakeld voor het station % approot % in Azure PaaS-virtuele machines, die in wezen verbruikt dubbele de hoeveelheid schijfruimte die bestanden wordt normaal gesproken in beslag nemen. Er zijn echter enkele dingen te weten dat in wezen kunt u deze uitschakelen in een niet-probleem.

De schijfgrootte % approot % als < grootte van .cspkg + maximale logboekgrootte > + marge vrije ruimte wordt berekend of 1,5 GB, afhankelijk van wat is groter. De grootte van uw virtuele machine heeft geen gevolgen voor deze berekening. (De VM-grootte geldt alleen voor de grootte van het tijdelijke station C:.) 

Dit wordt niet ondersteund voor het schrijven naar het station % approot %. Als u naar de virtuele machine van Azure schrijft, moet u doen in een tijdelijke LocalStorage resource (of een andere optie, zoals de Blob-opslag, Azure-bestanden, enz.). De hoeveelheid vrije ruimte op de map % approot % is dus niet relevant. Als u niet zeker weet of uw toepassing naar het station van de approot schrijft, kunt u altijd gebruiken om uw service een paar dagen uitvoeren en vergelijk de "voor" en "na' grootten. 

Azure wordt niet schrijven naar het station % approot %. Als de VHD is gemaakt op basis van uw .cspkg en in de Azure VM gekoppeld, is het enige dat dat naar dit station kan schrijven uw toepassing. 

De wijzigingslogboek-instellingen zijn niet-configureerbare, zodat u niet uitschakelen.

### <a name="how-can-i-add-an-antimalware-extension-for-my-cloud-services-in-an-automated-way"></a>Hoe kan ik een Antimalware-extensie voor mijn Cloudservices in een automatische manier toevoegen?

U kunt met behulp van PowerShell-script in de taak starten van de Antimalware-extensie inschakelen. Volg de stappen in deze artikelen om dit te implementeren: 
 
- [Een PowerShell starten van de taak maken](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task)
- [Set-AzureServiceAntimalwareExtension](https://docs.microsoft.com/powershell/module/Azure/Set-AzureServiceAntimalwareExtension?view=azuresmps-4.0.0 )

Zie voor meer informatie over implementatiescenario's voor Antimalware en hoe deze in te schakelen via de portal [Antimalware implementatiescenario's](../security/azure-security-antimalware.md#antimalware-deployment-scenarios).

### <a name="how-to-enable-server-name-indication-sni-for-cloud-services"></a>Het inschakelen van de Server de naam van vermelding (SNI) voor Cloudservices?

U kunt SNI in Cloudservices inschakelen met behulp van een van de volgende methoden:

**Methode 1: PowerShell gebruiken**

De SNI-binding kan worden geconfigureerd met behulp van de PowerShell-cmdlet **nieuw WebBinding** in een taak gestart voor een exemplaar van de rol Cloudservice zoals hieronder:
    
    New-WebBinding -Name $WebsiteName -Protocol "https" -Port 443 -IPAddress $IPAddress -HostHeader $HostHeader -SslFlags $sslFlags 
    
Zoals beschreven [hier](https://technet.microsoft.com/library/ee790567.aspx), de $sslFlags mogelijk een van de waarden als het volgende:

|Waarde|Betekenis|
------|------
|0|Er is geen SNI|
|1|SNI ingeschakeld |
|2 |Niet SNI binding waarin een centraal certificaatarchief|
|3|SNI-binding dat gebruikmaakt van het centrale certificaatarchief opslaan |
 
**Methode 2: Code gebruiken**

De SNI-binding kan ook worden geconfigureerd via de code in het opstarten van de rol, zoals beschreven in deze [blogbericht](https://blogs.msdn.microsoft.com/jianwu/2014/12/17/expose-ssl-service-to-multi-domains-from-the-same-cloud-service/):

    
    //<code snip> 
                    var serverManager = new ServerManager(); 
                    var site = serverManager.Sites[0]; 
                    var binding = site.Bindings.Add(“:443:www.test1.com”, newCert.GetCertHash(), “My”); 
                    binding.SetAttributeValue(“sslFlags”, 1); //enables the SNI 
                    serverManager.CommitChanges(); 
    //</code snip> 
    
Met behulp van een van de methoden hierboven, moeten de respectieve certificaten (*.pfx) voor de specifieke hostnamen eerst worden geïnstalleerd voor de rolinstanties met behulp van een taak starten of via de code in de volgorde voor de SNI-binding te worden uitgevoerd.

### <a name="how-can-i-add-tags-to-my-azure-cloud-service"></a>Hoe kan ik labels toevoegen aan mijn Azure Cloud Service? 

Cloudservice is een klassieke resource. Alleen bronnen via Azure Resource Manager support-tags is gemaakt. U kunt tags toepassen op klassieke resources zoals Service in de Cloud. 

### <a name="the-azure-portal-doesnt-display-the-sdk-version-of-my-cloud-service-how-can-i-get-that"></a>De Azure-portal kan de SDK-versie van de Cloudservice wordt niet weergegeven. Hoe kan ik die krijgen?

We werken aan deze functie te brengen in de Azure portal. U kunt ondertussen volgende PowerShell-opdrachten gebruiken om op te halen van de SDK-versie:

    Get-AzureService -ServiceName "<Cloud Service name>" | Get-AzureDeployment | Where-Object -Property SdkVersion -NE -Value "" | select ServiceName,SdkVersion,OSVersion,Slot

### <a name="i-want-to-shut-down-the-cloud-service-for-several-months-how-to-reduce-the-billing-cost-of-cloud-service-without-losing-the-ip-address"></a>Ik wil de Cloudservice enkele maanden afgesloten. Hoe kan ik de facturering toegangskosten van Cloudservice zonder verlies van het IP-adres?

Een reeds geïmplementeerde Service in de Cloud wordt gefactureerd voor de berekenings- en wordt gebruikt. Dus zelfs als u de Azure VM afsluiten, wordt u nog steeds ophalen gefactureerd voor de opslag. 

Dit is wat u kunt doen om uw facturering reduceren zonder verlies van het IP-adres voor uw service:

1. [Het IP-adres reserveren](../virtual-network/virtual-networks-reserved-public-ip.md) voordat u de implementaties verwijderd.  U wordt alleen gefactureerd voor dit IP-adres. Zie voor meer informatie over IP-adres facturering [IP-adressen prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/).
2. De implementaties verwijderd. Verwijder de xxx.cloudapp.net niet zodat u deze voor toekomstige gebruiken kunt.
3. Als u wilt de Cloudservice implementeren met behulp van dezelfde gereserveerde IP die u in uw abonnement gereserveerd, raadpleegt u [gereserveerde IP-adressen voor Cloudservices en virtuele Machines](https://azure.microsoft.com/blog/reserved-ip-addresses/).


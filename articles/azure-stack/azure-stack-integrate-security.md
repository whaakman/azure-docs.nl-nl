---
title: Azure Stack syslog doorsturen
description: Meer informatie over het integreren van Azure Stack met oplossingen voor de controle met behulp van syslog doorsturen
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 01/28/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 01/28/2019
keywords: ''
ms.openlocfilehash: 7dff82538448b27f14dd81e2862cd63d4dd56a9b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247099"
---
# <a name="azure-stack-datacenter-integration---syslog-forwarding"></a>Integratie van datacenter voor Azure Stack - syslog doorsturen

Dit artikel ziet u hoe u Azure Stack-infrastructuur integreren met externe beveiliging oplossing(en) al geïmplementeerd in uw datacenter met syslog. Bijvoorbeeld, een Security informatie Event Management (SIEM)-systeem. De syslog-kanaal wordt aangegeven dat controles, waarschuwingen en beveiligingslogboeken van alle onderdelen van de Azure Stack-infrastructuur. Syslog doorsturen gebruiken voor integratie met oplossingen voor beveiligingsbewaking en/of Logboeken om op te halen van alle controles, waarschuwingen en beveiliging om op te slaan ze voor het bewaren van.

Beginnen met de update 1809, is Azure Stack een geïntegreerde syslog-client die, wanneer dit is geconfigureerd, verzendt de syslog-berichten met de nettolading in Common Event Format (CEF).

Het volgende overzicht beschrijft de integratie van Azure Stack met een externe SIEM. Er zijn twee integratiepatronen die moeten worden overwogen: het eerst een (de computer in het blauw) is de Azure Stack-infrastructuur die overeenkomt met de infrastructuur-VM's en de Hyper-V-knooppunten. De audits, beveiligingslogboeken en waarschuwingen van deze onderdelen zijn centraal verzameld en beschikbaar gesteld via syslog met CEF-nettolading. Dit patroon integratie wordt beschreven in dit document-pagina.
Het tweede integratie-patroon wordt afgebeeld in oranje en bevat informatie over de baseboard management controllers (bmc's), de hardware-lifecycle-host (HLH), de virtuele machines en/of virtuele apparaten met de hardware-partner bewaking en beheer software en de top van rack (TOR) switches. Aangezien deze onderdelen zijn specifiek, contact op met de hardware-partner van uw hardware partners voor documentatie over hoe u ze integreren met een externe SIEM.

![Syslog doorsturen van diagram](media/azure-stack-integrate-security/syslog-forwarding.png)

## <a name="configuring-syslog-forwarding"></a>Syslog doorsturen configureren

De syslog-client in Azure Stack ondersteunt de volgende configuraties:

1. **Syslog via TCP met wederzijdse verificatie (client en server) en TLS 1.2-versleuteling:** In deze configuratie worden zowel de syslog-server en de syslog-client kunnen controleren of de identiteit van elkaar met behulp van certificaten. De berichten worden verzonden via een versleuteld kanaal TLS 1.2.

2. **Syslog via TCP met server-verificatie en TLS 1.2-versleuteling:** In deze configuratie kan de syslog-client controleren of de identiteit van de syslog-server via een certificaat. De berichten worden verzonden via een versleuteld kanaal TLS 1.2.

3. **Syslog via TCP met geen versleuteling:** In deze configuratie de syslog-client en de identiteiten van syslog-server niet worden geverifieerd. De berichten worden verzonden in ongecodeerde tekst via TCP.

4. **Syslog via UDP, met geen versleuteling:** In deze configuratie de syslog-client en de identiteiten van syslog-server niet worden geverifieerd. De berichten worden verzonden in ongecodeerde tekst via UDP.

> [!IMPORTANT]
> Microsoft adviseert om TCP met behulp van verificatie en versleuteling te gebruiken (configuratie #1 of in de zeer minimale #2) voor productieomgevingen ter bescherming tegen man-in-the-middle-aanvallen en niet kan worden afgeluisterd van berichten.

### <a name="cmdlets-to-configure-syslog-forwarding"></a>Cmdlets voor het doorsturen van syslog configureren
Syslog doorsturen configureren vereist toegang tot het eindpunt van de bevoegde (PEP). Twee PowerShell-cmdlets zijn toegevoegd aan de PEP om de syslog-forwarding te configureren:


```powershell
### cmdlet to pass the syslog server information to the client and to configure the transport protocol, the encryption and the authentication between the client and the server

Set-SyslogServer [-ServerName <String>] [-ServerPort <String>] [-NoEncryption] [-SkipCertificateCheck] [-SkipCNCheck] [-UseUDP] [-Remove]

### cmdlet to configure the certificate for the syslog client to authenticate with the server

Set-SyslogClient [-pfxBinary <Byte[]>] [-CertPassword <SecureString>] [-RemoveCertificate] 
```
#### <a name="cmdlets-parameters"></a>Cmdlets parameters

Parameters voor *Set SyslogServer* cmdlet:

| Parameter | Beschrijving | Type | Vereist |
|---------|---------|---------|---------|
|*ServerName* | FQDN of IP-adres van de syslog-server | Reeks | ja|
|*ServerPort* | Poortnummer dat de syslog-server luistert | Reeks | ja|
|*NoEncryption*| Afdwingen dat de client voor het verzenden van syslog-berichten in niet-versleutelde tekst | Vlag | nee|
|*SkipCertificateCheck*| De validatie van het certificaat dat is geleverd door de syslog-server tijdens de initiële TLS-handshake overslaan | Vlag | nee|
|*SkipCNCheck*| De validatie van de waarde van de algemene naam van het certificaat dat is geleverd door de syslog-server tijdens de initiële TLS-handshake overslaan | Vlag | nee|
|*UseUDP*| Syslog met UDP als protocol-transport gebruiken |Vlag | nee|
|*Remove*| Configuratie van de server van de client verwijderen en syslog doorsturen stoppen| Vlag | nee|

Parameters voor *Set SyslogClient* cmdlet:
| Parameter | Beschrijving | Type |
|---------|---------| ---------|
| *pfxBinary* | PFX-bestand met het certificaat moet worden gebruikt door de client-id voor verificatie op basis van de syslog-server  | Byte[] |
| *CertPassword* |  Wachtwoord voor het importeren van de persoonlijke sleutel die is gekoppeld aan het pfx-bestand | SecureString |
|*RemoveCertificate* | Certificaat verwijderen uit de client | Vlag|

### <a name="configuring-syslog-forwarding-with-tcp-mutual-authentication-and-tls-12-encryption"></a>Syslog doorsturen configureren met TCP, wederzijdse verificatie en TLS 1.2-versleuteling

Bij deze configuratie stuurt de syslog-client in Azure Stack berichten naar de syslog-server via TCP, met TLS 1.2-versleuteling. Tijdens de initiële handshake, controleert de client of dat de server een geldig en vertrouwd certificaat biedt; de client biedt een certificaat met de server op dezelfde manier, ook als bewijs van zijn identiteit. Deze configuratie is het veiligste niveau als het biedt een volledige validatie van de identiteit van zowel de client en de server en het verzenden van berichten via een versleuteld kanaal. 

> [!IMPORTANT]
> Microsoft raadt het gebruik van deze configuratie voor productieomgevingen. 

Wederzijdse verificatie en TLS 1.2-versleuteling, worden deze beide cmdlets wilt syslog doorsturen configureren met TCP, uitgevoerd op een sessie PEP:

```powershell
# Configure the server
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>

# Provide certificate to the client to authenticate against the server
Set-SyslogClient -pfxBinary <Byte[] of pfx file> -CertPassword <SecureString, password for accessing the pfx file>
```

Het clientcertificaat moet dezelfde basis als het account dat is opgegeven tijdens de implementatie van Azure Stack. Het moet ook een persoonlijke sleutel bevatten.

```powershell
##Example on how to set your syslog client with the certificate for mutual authentication.
##This example script must be run from your hardware lifecycle host or privileged access workstation.

$ErcsNodeName = "<yourPEP>"
$password = ConvertTo-SecureString -String "<your cloudAdmin account password" -AsPlainText -Force
 
$cloudAdmin = "<your cloudAdmin account name>"
$CloudAdminCred = New-Object System.Management.Automation.PSCredential ($cloudAdmin, $password)
 
$certPassword = $password
$certContent = Get-Content -Path C:\cert\<yourClientCertificate>.pfx -Encoding Byte
 
$params = @{ 
    ComputerName = $ErcsNodeName 
    Credential = $CloudAdminCred 
    ConfigurationName = "PrivilegedEndpoint" 
}

$session = New-PSSession @params
 
$params = @{ 
    Session = $session 
    ArgumentList = @($certContent, $certPassword) 
}
Write-Verbose "Invoking cmdlet to set syslog client certificate..." -Verbose 
Invoke-Command @params -ScriptBlock { 
    param($CertContent, $CertPassword) 
    Set-SyslogClient -PfxBinary $CertContent -CertPassword $CertPassword }
```

### <a name="configuring-syslog-forwarding-with-tcp-server-authentication-and-tls-12-encryption"></a>Syslog doorsturen configureren met TCP, Server-verificatie en TLS 1.2-versleuteling

Bij deze configuratie stuurt de syslog-client in Azure Stack berichten naar de syslog-server via TCP, met TLS 1.2-versleuteling. Tijdens de initiële handshake verifieert de client ook dat de server een geldig certificaat van vertrouwde biedt. Deze configuratie wordt voorkomen dat de client om berichten te verzenden naar niet-vertrouwde doelen.
TCP-met behulp van verificatie en versleuteling is de standaardconfiguratie en Hiermee geeft u het minimale niveau van beveiliging die door Microsoft wordt aanbevolen voor een productie-omgeving. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
```

Als u testen van de integratie van uw syslog-server met de Azure Stack-client wilt met behulp van een zelf-ondertekend en/of niet-vertrouwd certificaat, kunt u deze vlaggen om over te slaan van de validatie van de server door de client tijdens de initiële handshake uitgevoerd.

```powershell
 #Skip validation of the Common Name value in the server certificate. Use this flag if you provide an IP address for your syslog server
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
 -SkipCNCheck

 #Skip entirely the server certificate validation
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
 -SkipCertificateCheck
```

> [!IMPORTANT]
> Microsoft raadt aan op basis van het gebruik van de vlag - SkipCertificateCheck voor productieomgevingen. 

### <a name="configuring-syslog-forwarding-with-tcp-and-no-encryption"></a>Syslog doorsturen configureren met TCP- en geen versleuteling

Bij deze configuratie stuurt de syslog-client in Azure Stack berichten naar de syslog-server via TCP, met geen versleuteling. De client controleert niet of de identiteit van de server en het biedt een eigen identiteit op de server voor verificatie. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on> -NoEncryption
```

> [!IMPORTANT]
> Microsoft raadt het gebruik van deze configuratie voor productie-omgevingen. 


### <a name="configuring-syslog-forwarding-with-udp-and-no-encryption"></a>Syslog doorsturen configureren met UDP en geen versleuteling

Bij deze configuratie stuurt de syslog-client in Azure Stack berichten naar de syslog-server via UDP, met geen versleuteling. De client controleert niet of de identiteit van de server en het biedt een eigen identiteit op de server voor verificatie. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on> -UseUDP
```

UDP met geen versleuteling is de eenvoudigste manier om te configureren, biedt bescherming tegen man-in-the-middle-aanvallen en niet kan worden afgeluisterd van berichten niet meer. 

> [!IMPORTANT]
> Microsoft raadt het gebruik van deze configuratie voor productie-omgevingen. 


## <a name="removing-syslog-forwarding-configuration"></a>Verwijderen van syslog doorsturen configureren

Verwijderen van de configuratie van de syslog-server kan worden overgeslagen en syslog doorsturen stoppen:

**De configuratie van de syslog-server van de client verwijderen**

```PowerShell  
Set-SyslogServer -Remove
```

**Het certificaat van de client verwijderen**

```PowerShell  
Set-SyslogClient -RemoveCertificate
```

## <a name="verifying-the-syslog-setup"></a>De syslog-instellingen controleren

Als u de syslog-client is verbonden met uw syslog-server, moet u snel starten het ontvangen van gebeurtenissen. Als u een gebeurtenis niet ziet, Controleer of de configuratie van uw syslog-client, door het uitvoeren van de volgende cmdlets:

**Controleer of de configuratie van de server in de syslog-client**

```PowerShell  
Get-SyslogServer
```

**Controleer de instellingen van het certificaat in de syslog-client**

```PowerShell  
Get-SyslogClient
```

## <a name="syslog-message-schema"></a>Schema voor Syslog-bericht

De syslog doorsturen van de infrastructuur van Azure Stack wordt verzonden berichten opgemaakt in Common Event Format (CEF).
Alle syslog-bericht is opgebouwd op basis van dit schema: 

```Syslog
<Time> <Host> <CEF payload>
```

De nettolading van de CEF is gebaseerd op de onderstaande structuur, maar de toewijzing voor elk veld is afhankelijk van het type van bericht (Windows-gebeurtenis, waarschuwing gemaakt, waarschuwing gesloten).

```CEF
# Common Event Format schema
CEF: <Version>|<Device Vendor>|<Device Product>|<Device Version>|<Signature ID>|<Name>|<Severity>|<Extensions>
* Version: 0.0
* Device Vendor: Microsoft
* Device Product: Microsoft Azure Stack
* Device Version: 1.0
```

### <a name="cef-mapping-for-privileged-endpoint-events"></a>CEF-toewijzing voor bevoegde eindpunt gebeurtenissen

```
Prefix fields
* Signature ID: Microsoft-AzureStack-PrivilegedEndpoint: <PEP Event ID>
* Name: <PEP Task Name>
* Severity: mapped from PEP Level (details see the PEP Severity table below)
```

Tabel van gebeurtenissen voor het eindpunt van de bevoegde:

| Gebeurtenis | PEP gebeurtenis-ID | De naam van de taak PEP | Severity |
|-------|--------------| --------------|----------|
|PrivilegedEndpointAccessed|1000|PrivilegedEndpointAccessedEvent|5|
|SupportSessionTokenRequested |1001|SupportSessionTokenRequestedEvent|5|
|SupportSessionDevelopmentTokenRequested |1002|SupportSessionDevelopmentTokenRequestedEvent|5|
|SupportSessionUnlocked |1003|SupportSessionUnlockedEvent|10|
|SupportSessionFailedToUnlock |1004|SupportSessionFailedToUnlockEvent|10|
|PrivilegedEndpointClosed |1005|PrivilegedEndpointClosedEvent|5|
|NewCloudAdminUser |1006|NewCloudAdminUserEvent|10|
|RemoveCloudAdminUser |1007|RemoveCloudAdminUserEvent|10|
|SetCloudAdminUserPassword |1008|SetCloudAdminUserPasswordEvent|5|
|GetCloudAdminPasswordRecoveryToken |1009|GetCloudAdminPasswordRecoveryTokenEvent|10|
|ResetCloudAdminPassword |1010|ResetCloudAdminPasswordEvent|10|

PEP ernst tabel:

| Severity | Niveau | Numerieke waarde |
|----------|-------| ----------------|
|0|Undefined|Waarde: 0. Geeft aan dat de logboeken op alle niveaus|
|10|Kritiek|Waarde: 1. Geeft aan dat de logboeken voor een kritieke waarschuwing|
|8|Fout| Waarde: 2. Geeft aan dat de logboeken voor een fout|
|5|Waarschuwing|Waarde: 3. Geeft aan dat de logboeken voor een waarschuwing|
|2|Informatie|Waarde: 4. Geeft aan dat de logboeken voor een informatief bericht|
|0|Uitgebreid|Waarde: 5. Geeft aan dat de logboeken op alle niveaus|

### <a name="cef-mapping-for-recovery-endpoint-events"></a>CEF-toewijzing voor gebeurtenissen-eindpunt

```
Prefix fields
* Signature ID: Microsoft-AzureStack-PrivilegedEndpoint: <REP Event ID>
* Name: <REP Task Name>
* Severity: mapped from REP Level (details see the REP Severity table below)
```

Tabel van gebeurtenissen voor de recovery-eindpunt:

| Gebeurtenis | REP gebeurtenis-ID | De naam van de taak REP | Severity |
|-------|--------------| --------------|----------|
|RecoveryEndpointAccessed |1011|RecoveryEndpointAccessedEvent|5|
|RecoverySessionTokenRequested |1012|RecoverySessionTokenRequestedEvent |5|
|RecoverySessionDevelopmentTokenRequested |1013|RecoverySessionDevelopmentTokenRequestedEvent|5|
|RecoverySessionUnlocked |1014|RecoverySessionUnlockedEvent |10|
|RecoverySessionFailedToUnlock |1015|RecoverySessionFailedToUnlockEvent|10|
|RecoveryEndpointClosed |1016|RecoveryEndpointClosedEvent|5|

REP Severity tabel:
| Severity | Niveau | Numerieke waarde |
|----------|-------| ----------------|
|0|Undefined|Waarde: 0. Geeft aan dat de logboeken op alle niveaus|
|10|Kritiek|Waarde: 1. Geeft aan dat de logboeken voor een kritieke waarschuwing|
|8|Fout| Waarde: 2. Geeft aan dat de logboeken voor een fout|
|5|Waarschuwing|Waarde: 3. Geeft aan dat de logboeken voor een waarschuwing|
|2|Informatie|Waarde: 4. Geeft aan dat de logboeken voor een informatief bericht|
|0|Uitgebreid|Waarde: 5. Geeft aan dat de logboeken op alle niveaus|

### <a name="cef-mapping-for-windows-events"></a>CEF-toewijzing voor Windows-gebeurtenissen

```
* Signature ID: ProviderName:EventID
* Name: TaskName
* Severity: Level (for details, see the severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```

Ernst van de tabel voor Windows-gebeurtenissen:
| CEF-waarde voor ernst | Niveau van de Windows-gebeurtenissen | Numerieke waarde |
|--------------------|---------------------| ----------------|
|0|Undefined|Waarde: 0. Geeft aan dat de logboeken op alle niveaus|
|10|Kritiek|Waarde: 1. Geeft aan dat de logboeken voor een kritieke waarschuwing|
|8|Fout| Waarde: 2. Geeft aan dat de logboeken voor een fout|
|5|Waarschuwing|Waarde: 3. Geeft aan dat de logboeken voor een waarschuwing|
|2|Informatie|Waarde: 4. Geeft aan dat de logboeken voor een informatief bericht|
|0|Uitgebreid|Waarde: 5. Geeft aan dat de logboeken op alle niveaus|

Aangepaste extensie-tabel voor Windows-gebeurtenissen in Azure Stack:
| Naam van de aangepaste uitbreiding | Voorbeeld van de Windows-gebeurtenis | 
|-----------------------|---------|
|MasChannel | Systeem|
|MasComputer | test.azurestack.contoso.com|
|MasCorrelationActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasCorrelationRelatedActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasEventData| Svchost. 4132, G, 0. EseDiskFlushConsistency. ESENT. 0x800000|
|MasEventDescription| De instellingen voor Groepsbeleid voor de gebruiker zijn verwerkt. Er zijn geen wijzigingen gedetecteerd sinds de laatste geslaagde verwerking van Groepsbeleid.|
|MasEventID|1501|
|MasEventRecordID|26637|
|MasExecutionProcessID | 29380|
|MasExecutionThreadID |25480|
|MasKeywords |0x8000000000000000|
|MasKeywordName |Controle geslaagd|
|MasLevel |4|
|MasOpcode |1|
|MasOpcodeName |informatie|
|MasProviderEventSourceName ||
|MasProviderGuid |AEA1B4FA-97D1-45F2-A64C-4D69FFFD92C9|
|MasProviderName |Microsoft-Windows-GroupPolicy|
|MasSecurityUserId |\<Windows SID\> |
|MasTask |0|
|MasTaskCategory| Maken van het proces|
|MasUserData|KB4093112!!5112!!Installed!!0x0!!WindowsUpdateAgent Xpath: /Event/UserData/*|
|MasVersion|0|

### <a name="cef-mapping-for-alerts-created"></a>CEF-toewijzing voor waarschuwingen die zijn gemaakt

```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Alert Severity (for details, see alerts severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```

Tabel van de ernst van waarschuwingen:
| Severity | Niveau |
|----------|-------|
|0|Undefined|
|10|Kritiek|
|5|Waarschuwing|

Aangepaste extensie-tabel voor waarschuwingen die zijn gemaakt in Azure Stack:
| Naam van de aangepaste uitbreiding | Voorbeeld | 
|-----------------------|---------|
|MasEventDescription|BESCHRIJVING: Een gebruikersaccount \<TestUser\> is gemaakt voor \<TestDomain\>. Het is een mogelijk beveiligingsrisico. --HERSTEL: Neem contact op met ondersteuning. Klantondersteuning is vereist om dit probleem te verhelpen. Probeer niet om op te lossen dit probleem zonder de hulp. Voordat u een ondersteuningsaanvraag opent, start u het bestand logboekverzamelproces volgens de richtlijnen van https://aka.ms/azurestacklogfiles |

### <a name="cef-mapping-for-alerts-closed"></a>CEF-toewijzing voor waarschuwingen gesloten

```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Information
```

Het volgende voorbeeld ziet u een syslog-bericht met CEF nettolading:
```
2018:05:17:-23:59:28 -07:00 TestHost CEF:0.0|Microsoft|Microsoft Azure Stack|1.0|3|TITLE: User Account Created -- DESCRIPTION: A user account \<TestUser\> was created for \<TestDomain\>. It's a potential security risk. -- REMEDIATION: Please contact Support. Customer Assistance is required to resolve this issue. Do not try to resolve this issue without their assistance. Before you open a support request, start the log file collection process using the guidance from https://aka.ms/azurestacklogfiles|10
```

## <a name="next-steps"></a>Volgende stappen

[Servicebeleid](azure-stack-servicing-policy.md)
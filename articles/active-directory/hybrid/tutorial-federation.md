---
title: 'Zelfstudie: Federeren één AD-forest omgeving naar Azure | Microsoft Docs'
description: Demonstreert hoe u het instellen van een hybride identiteit-omgeving gebruikmaakt van Federatie.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 2e0afa4566896af98932ccd7a99ecf109ff2e56e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55150852"
---
# <a name="tutorial-federate-a-single-ad-forest-environment-to-the-cloud"></a>Zelfstudie: Een enkele AD-forestomgeving federeren in de cloud

![Maken](media/tutorial-federation/diagram.png)

De volgende zelfstudie begeleidt u bij het maken van een hybride identiteit-omgeving gebruikmaakt van Federatie.  Deze omgeving kan vervolgens worden gebruikt voor het testen of voor het ophalen van meer wilt weten over de werking van een hybride identiteit.

## <a name="prerequisites"></a>Vereisten
De volgende zijn vereisten voor het voltooien van deze zelfstudie
- Een computer met [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview) geïnstalleerd.  Het wordt aangeraden om dit te doen voor een [Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os) of een [Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) computer.
- Een [Azure-abonnement](https://azure.microsoft.com/free)
- - Een [externe netwerkadapter](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network) waarmee de virtuele machine om te communiceren met internet.
- Een kopie van Windows Server 2016
- Een [aangepast domein](../../active-directory/fundamentals/add-custom-domain.md) die kan worden gecontroleerd

> [!NOTE]
> In deze zelfstudie maakt gebruik van PowerShell-scripts zodat u de zelfstudie omgeving in de snelste tijd maken kunt.  Elk van de scripts maakt gebruik van variabelen die zijn gedeclareerd aan het begin van de scripts.  U kunt en de variabelen aanleiding van uw omgeving te wijzigen.
>
>De scripts die worden gebruikt, maak een algemene Active Directory-omgeving voordat u Azure AD Connect installeert.  Ze zijn relevant voor alle van de zelfstudies.
>
> Kopieën van de PowerShell-scripts die worden gebruikt in deze zelfstudie zijn beschikbaar op GitHub [hier](https://github.com/billmath/tutorial-phs).

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Het eerste wat we moeten doen, om op te halen van onze hybride identiteit omgeving van en het uitvoeren is het maken van een virtuele machine die wordt gebruikt als onze on-premises Active Directory-server.  

>[!NOTE]
>Als u een script nooit hebt uitgevoerd in PowerShell op uw hostcomputer moet u om uit te voeren `Set-ExecutionPolicy remotesigned` en zegt Ja in PowerShell, vóór het uitvoeren van scripts.

Ga als volgt te werk:

1. Open de PowerShell ISE als Administrator.
2. Voer het volgende script.

```powershell
#Declare variables
$VMName = 'DC1'
$Switch = 'External'
$InstallMedia = 'D:\ISO\en_windows_server_2016_updated_feb_2018_x64_dvd_11636692.iso'
$Path = 'D:\VM'
$VHDPath = 'D:\VM\DC1\DC1.vhdx'
$VHDSize = '64424509440'

#Create New Virtual Machine
New-VM -Name $VMName -MemoryStartupBytes 16GB -BootDevice VHD -Path $Path -NewVHDPath $VHDPath -NewVHDSizeBytes $VHDSize  -Generation 2 -Switch $Switch  

#Set the memory to be non-dynamic
Set-VMMemory $VMName -DynamicMemoryEnabled $false

#Add DVD Drive to Virtual Machine
Add-VMDvdDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 1 -Path $InstallMedia

#Mount Installation Media
$DVDDrive = Get-VMDvdDrive -VMName $VMName

#Configure Virtual Machine to Boot from DVD
Set-VMFirmware -VMName $VMName -FirstBootDevice $DVDDrive 
```

## <a name="complete-the-operating-system-deployment"></a>Voltooien van de implementatie van besturingssysteem
Klaar bent met het bouwen van de virtuele machine, moet u om de installatie van het besturingssysteem te voltooien.

1. Hyper-V-beheer, dubbelklik op de virtuele machine
2. Klik op de knop Start.
3.  U wordt gevraagd naar 'Op een willekeurige toets om op te starten vanaf CD of DVD'. Ga vooruit, en doen.
4. Selecteer uw taal op de Windows Server-opstartscherm en klikt u op **volgende**.
5. Klik op **nu installeren**.
6. Voer uw licentiesleutel in en klikt u op **volgende**.
7. Controleer ** ik accepteer de licentievoorwaarden en klik op **volgende**.
8. Selecteer **aangepast:  Alleen Windows installeren (Geavanceerd)**
9. Klik op **Volgende**
10. Als de installatie is voltooid, start de virtuele machine, aanmelden en werken met Windows om te controleren of dat de virtuele machine is de meest recente updates.  Installeer de meest recente updates.

## <a name="install-active-directory-pre-requisites"></a>Vereisten voor Active Directory installeren
Nu dat we een virtuele machine u hebt, moeten we een paar handelingen voor de installatie van Active Directory.  Dat wil zeggen, moeten we Wijzig de naam van de virtuele machine, het instellen van een statisch IP-adres en DNS-gegevens en het installeren van de Remote Server Administration tools.   Ga als volgt te werk:

1. Open de PowerShell ISE als Administrator.
2. Voer `Set-ExecutionPolicy remotesigned` en Ja bijvoorbeeld naar alle [A].  Druk op Enter.
3. Voer het volgende script.

```powershell
#Declare variables
$ipaddress = "10.0.1.117" 
$ipprefix = "24" 
$ipgw = "10.0.1.1" 
$ipdns = "10.0.1.117"
$ipdns2 = "8.8.8.8" 
$ipif = (Get-NetAdapter).ifIndex 
$featureLogPath = "c:\poshlog\featurelog.txt" 
$newname = "DC1"
$addsTools = "RSAT-AD-Tools" 

#Set static IP address
New-NetIPAddress -IPAddress $ipaddress -PrefixLength $ipprefix -InterfaceIndex $ipif -DefaultGateway $ipgw 

# Set the DNS servers
Set-DnsClientServerAddress -InterfaceIndex $ipif -ServerAddresses ($ipdns, $ipdns2)

#Rename the computer 
Rename-Computer -NewName $newname -force 

#Install features 
New-Item $featureLogPath -ItemType file -Force 
Add-WindowsFeature $addsTools 
Get-WindowsFeature | Where installed >>$featureLogPath 

#Restart the computer 
Restart-Computer
```

## <a name="create-a-windows-server-ad-environment"></a>Een Windows Server AD-omgeving maken
Nu dat we de virtuele machine gemaakt hebben en het is gewijzigd en een statisch IP-adres heeft, kunnen we eens en installeren en configureren van Active Directory Domain Services.  Ga als volgt te werk:

1. Open de PowerShell ISE als Administrator.
2. Voer het volgende script.

```powershell 
#Declare variables
$DatabasePath = "c:\windows\NTDS"
$DomainMode = "WinThreshold"
$DomainName = "contoso.com"
$DomaninNetBIOSName = "CONTOSO"
$ForestMode = "WinThreshold"
$LogPath = "c:\windows\NTDS"
$SysVolPath = "c:\windows\SYSVOL"
$featureLogPath = "c:\poshlog\featurelog.txt" 
$Password = "Pass1w0rd"

#Install AD DS, DNS and GPMC 
start-job -Name addFeature -ScriptBlock { 
Add-WindowsFeature -Name "ad-domain-services" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "dns" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "gpmc" -IncludeAllSubFeature -IncludeManagementTools } 
Wait-Job -Name addFeature 
Get-WindowsFeature | Where installed >>$featureLogPath

#Create New AD Forest
Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath $DatabasePath -DomainMode $DomainMode -DomainName $DomainName -SafeModeAdministratorPassword $Password -DomainNetbiosName $DomainNetBIOSName -ForestMode $ForestMode -InstallDns:$true -LogPath $LogPath -NoRebootOnCompletion:$false -SysvolPath $SysVolPath -Force:$true
```

## <a name="create-a-windows-server-ad-user"></a>Een Windows Server AD-gebruiker maken
Nu dat we onze Active Directory-omgeving hebt, moet er een testaccount.  Dit account wordt gemaakt in onze on-premises AD-omgeving en vervolgens worden gesynchroniseerd met Azure AD.  Ga als volgt te werk:

1. Open de PowerShell ISE als Administrator.
2. Voer het volgende script.

```powershell 
#Declare variables
$Givenname = "Allie"
$Surname = "McCray"
$Displayname = "Allie McCray"
$Name = "amccray"
$Password = "Pass1w0rd"
$Identity = "CN=ammccray,CN=Users,DC=contoso,DC=com"
$SecureString = ConvertTo-SecureString $Password -AsPlainText -Force


#Create the user
New-ADUser -Name $Name -GivenName $Givenname -Surname $Surname -DisplayName $Displayname -AccountPassword $SecureString

#Set the password to never expire
Set-ADUser -Identity $Identity -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Enabled $true
```

## <a name="create-a-certificate-for-ad-fs"></a>Maak een certificaat voor AD FS
We gaan nu een SSL-certificaat dat wordt gebruikt door AD FS maken.  Dit is, is een zelfondertekend certificaat en is alleen voor testdoeleinden.  Met behulp van een zelfondertekend certificaat in een productieomgeving wordt niet aanbevolen. Ga als volgt te werk:

1. Open de PowerShell ISE als Administrator.
2. Voer het volgende script.

```powershell 
#Declare variables
$DNSname = "adfs.contoso.com"
$Location = "cert:\LocalMachine\My"

#Create certificate
New-SelfSignedCertificate -DnsName $DNSname -CertStoreLocation $Location
```

## <a name="create-an-azure-ad-tenant"></a>Een Azure AD-tenant maken
Nu moet maken van een Azure AD-tenant zodat we onze gebruikers naar de cloud kunt synchroniseren.  Het maken van een nieuwe Azure AD-tenant, het volgende doen.

1. Blader naar de [Azure-portal](https://portal.azure.com) en meld u aan met een account met een Azure-abonnement.
2. Selecteer de **plus-pictogram (+)** en zoek naar de **Azure Active Directory**.
3. Selecteer **Azure Active Directory** in de lijst met zoekresultaten.
4. Selecteer **Maken**.</br>
![Maken](media/tutorial-password-hash-sync/create1.png)</br>
5. Geef een **naam voor de organisatie** samen met de **initiële domeinnaam**. Selecteer vervolgens **Maken**. Hiermee maakt u uw directory.
6. Nadat deze is voltooid, klikt u op de **hier** koppeling om de directory te beheren.

## <a name="create-a-global-administrator-in-azure-ad"></a>Een globale beheerder maken in Azure AD
Nu dat we een Azure AD-tenant hebt, maakt er een globale beheerdersaccount.  Dit account wordt gebruikt voor het maken van het account van Azure AD-Connector tijdens de installatie van Azure AD Connect.  De Azure AD-Connector-account wordt gebruikt voor het schrijven van gegevens in Azure AD.   Voor het maken van de globale beheerder van account het volgende doen.

1.  Onder **Beheren**, selecteer **Gebruikers**.</br>
![Maken](media/tutorial-password-hash-sync/gadmin1.png)</br>
2.  Selecteer **alle gebruikers** en selecteer vervolgens **+ nieuwe gebruiker**.
3.  Geef een naam en de gebruikersnaam voor deze gebruiker. Dit is de globale beheerder voor de tenant. U wordt ook wilt wijzigen de **maprol** naar **globale beheerder.** U kunt ook het tijdelijke wachtwoord weergeven. Wanneer u klaar bent, selecteert u **maken**.</br>
![Maken](media/tutorial-password-hash-sync/gadmin2.png)</br>
4. Wanneer deze is voltooid, opent u een nieuwe webbrowser en meld u aan met behulp van het nieuwe account voor globale beheerder en het tijdelijke wachtwoord myapps.microsoft.com.
5. Wijzig het wachtwoord voor de globale beheerder iets dat u onthouden.

## <a name="add-the-custom-domain-name-to-your-directory"></a>Voeg de aangepaste domeinnaam toe aan uw directory.
Nu dat we een tenant en een globale beheerder hebben, moeten we onze aangepast domein toevoegen zodat dat Azure het kunt controleren.  Ga als volgt te werk:

1. Klik in de [Azure-portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) sluit de **alle gebruikers** blade.
2. Selecteer aan de linkerkant **Namen van aangepaste domeinen**.
3. Selecteer **Aangepast domein toevoegen**.</br>
![Federation](media/tutorial-federation/custom1.png)</br>
4. Op **aangepaste-domeinnamen**, voer de naam van uw aangepaste domein in het vak en klikt u op **domein toevoegen**.
5. Op het scherm van de naam van aangepast domein u hebt opgegeven met de TXT- of MX-informatie.  Deze informatie moet worden toegevoegd aan de DNS-gegevens van de domeinregistrar onder het domein.  Zodat u nodig hebt om naar uw domeinregistrar te gaan, voert u de TXT- of MX-gegevens in de DNS-instellingen voor uw domein.  Hierdoor kunnen Azure om uw domein te verifiëren.  Dit duurt maximaal 24 uur voor Azure om het te verifiëren.  Zie voor meer informatie de [een aangepast domein toevoegen](../../active-directory/fundamentals/add-custom-domain.md) documentatie.</br>
![Federation](media/tutorial-federation/custom2.png)</br>
6. Om ervoor te zorgen dat deze is geverifieerd, klikt u op verifiëren klikt.</br>
![Federation](media/tutorial-federation/custom3.png)</br>

## <a name="download-and-install-azure-ad-connect"></a>Download en installeer Azure AD Connect
Het is nu tijd om te downloaden en installeren van Azure AD Connect.  Zodra deze is geïnstalleerd wordt uitgevoerd door de snelle installatie.  Ga als volgt te werk:

1. Download [Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594)
2. Ga naar **AzureADConnect.msi** en dubbelklik erop.
3. Selecteer in het welkomstscherm het vakje waarmee u aangeeft akkoord te gaan met de licentievoorwaarden en klik op **Doorgaan**.  
4. Klik op het scherm Expresinstellingen op **aanpassen**.  
5. Op het scherm van de installatie van vereiste onderdelen. Klik op **Install**.  
6. Selecteer op het scherm aanmelden van gebruikers **Federatie met AD FS** en klikt u op **volgende**.
![Federation](media/tutorial-federation/fed1.png)

1. Voer de gebruikersnaam en het wachtwoord van de globale beheerder, die eerder is gemaakt op het scherm verbinding maken met Azure AD, en klik op **volgende**.
2. Klik op het scherm van uw directory's verbinding maken **map toevoegen**.  Selecteer vervolgens **nieuw AD-account maken** contoso\Administrator gebruikersnaam en wachtwoord invoeren en op **OK**.
3. Klik op **volgende**.
4. Selecteer op het scherm van de configuratie van aanmelding bij Azure AD **doorgaan zonder bijbehorende alle UPN-achtervoegsels voor geverifieerde domeinen** en klikt u op **volgende.**
5. Klik op het domein en OE filteren scherm **volgende**.
6. Over het uniek identificeren van het scherm van uw gebruikers, klikt u op **volgende**.
7. Klik op het Filter gebruikers en apparaten scherm op **volgende**.
8. Klik op het scherm optionele functies **volgende**.
9. Voer op de pagina van de referenties domeinadministrator contoso\Administrator gebruikersnaam en wachtwoord in en klikt u op **volgende.**
10. Zorg ervoor dat op het scherm voor AD FS-farm **een nieuwe AD FS-farm configureren** is geselecteerd.
11. Selecteer **gebruik van een certificaat geïnstalleerd op de federatieservers** en klikt u op **Bladeren**.
12. DC1 invoeren in het zoekvak in en selecteert u deze als dat is gevonden.  Klik op **OK**.
13. Uit de **certificaatbestand** vervolgkeuzelijst, selecteer **adfs.contoso.com** het certificaat die eerder is gemaakt.  Klik op **volgende**.
![Federation](media/tutorial-federation/fed2.png)

1. Klik op het scherm van de AD FS-server, **Bladeren** DC1 invoeren in het zoekvak in en selecteert u deze als dat is gevonden.  Klik op **OK**.  Klik op **volgende**.
![Federation](media/tutorial-federation/fed3.png)

1. Klik op het scherm Web application Proxy-servers op **volgende**.
2. Voer op het scherm voor AD FS-service-account contoso\Administrator gebruikersnaam en wachtwoord in en klikt u op **volgende.**
3. Op het scherm Azure AD-domein selecteert u uw gecontroleerd aangepast domein in de vervolgkeuzelijst en klikt u op **volgende**.
4. Klik in het venster Gereed om te configureren op **Installeren**.
5. Wanneer de installatie is voltooid, klikt u op **Afsluiten**.
6. Nadat de installatie is voltooid, meldt u zich af en opnieuw aanmelden voordat u de Synchronization Service Manager of Synchronization Rule Editor gebruiken.


## <a name="verify-users-are-created-and-synchronization-is-occurring"></a>Controleer of gebruikers zijn gemaakt en synchronisatie plaatsvindt
Nu controleren we dat de gebruikers die we in onze on-premises directory hebben zijn gesynchroniseerd en nu aanwezig zijn in uit Azure AD-tenant.  Let erop dat dit enkele uren kan duren.  Om te controleren of gebruikers worden gesynchroniseerd als volgt.


1. Blader naar de [Azure-portal](https://portal.azure.com) en meld u aan met een account met een Azure-abonnement.
2. Selecteer aan de linkerkant **Azure Active Directory**
3. Onder **Beheren**, selecteer **Gebruikers**.
4. Controleer of u de nieuwe gebruikers in de tenant zien ![synchronisatie](media/tutorial-password-hash-sync/synch1.png)

## <a name="test-signing-in-with-one-of-our-users"></a>Aanmelden met een van onze gebruikers testen

1.  Blader naar [https://myapps.microsoft.com](httpss://myapps.microsoft.com)
2. Aanmelden met een gebruikersaccount dat is gemaakt in onze nieuwe tenant.  U moet aanmelding met de volgende notatie: (user@domain.onmicrosoft.com). Gebruik hetzelfde wachtwoord dat de gebruiker gebruikt voor aanmelding bij on-premises.
![Controleer of](media/tutorial-password-hash-sync/verify1.png)

U hebt nu is ingesteld, een hybride identiteit-omgeving die u gebruiken kunt om te testen en te raken met wat Azure te bieden heeft.

## <a name="next-steps"></a>Volgende stappen

- [Hardware en vereisten](how-to-connect-install-prerequisites.md) 
- [Aangepaste instellingen](how-to-connect-install-custom.md)
- [Azure AD Connect en federatie](how-to-connect-fed-whatis.md)


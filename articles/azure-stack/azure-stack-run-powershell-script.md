---
title: De Azure-Stack Development Kit implementeren | Microsoft Docs
description: Informatie over het voorbereiden van de Azure-Stack Development Kit en voer de PowerShell-script voor het implementeren van deze.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2017
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 470a45aea253e1e238983527427b600117e413fe
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/16/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>De Azure-Stack Development Kit implementeren

*Van toepassing op: Azure stapelen Development Kit*

Voor het implementeren van de [Azure Stack Development Kit](azure-stack-poc.md), moet u de volgende stappen:

1. [Het implementatiepakket downloaden](https://azure.microsoft.com/overview/azure-stack/try/?v=try) de Cloudbuilder.vhdx ophalen.
2. Bereid de cloudbuilder.vhdx voor het configureren van de computer (de host development kit) waarop u wilt installeren development kit. Na deze stap wordt de development kit host naar de Cloudbuilder.vhdx opgestart.
3. Implementeer de development kit op de host van development kit.

> [!NOTE]
> Voor de beste resultaten, zelfs als u wilt gebruiken van een omgeving zonder verbinding Azure Stack verdient implementeren tijdens verbinding met internet. De evaluatieversie van Windows Server 2016 opgenomen met de installatie van development kit kan dan worden geactiveerd tijdens de implementatie.

## <a name="download-and-extract-the-development-kit"></a>Downloaden en uitpakken van de development kit
1. Voordat u het downloaden, zorg dat de computer voldoet aan de volgende vereisten:

  - De computer moet ten minste 60 GB aan schijfruimte beschikbaar op vier afzonderlijke, identieke logische vasteschijfstations verder naar de schijf van besturingssysteem hebben.
  - [.NET framework 4.6 (of een latere versie)](https://aka.ms/r6mkiy) moet worden geïnstalleerd.

2. [Ga naar de pagina aan de slag](https://azure.microsoft.com/overview/azure-stack/try/?v=try) waar u kunt download de Azure-Stack Development Kit, vindt u de informatie en klik vervolgens op **indienen**.
3. Downloaden en uitvoeren van de [implementatie Checker voor Azure Stack Development Kit](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) vereistencontrole script. Dit script zelfstandige gaat via de controles van vereisten uitgevoerd door de instellingen voor Azure Stack Development Kit. Het biedt een manier om te bevestigen dat u voldoet aan de hardware- en softwarevereisten voor het downloaden van het grotere pakket voor Azure Stack Development Kit.
4. Onder **Download de software**, klikt u op **Azure Stack Development Kit**.

  > [!NOTE]
  > Het downloaden van de ASDK (AzureStackDevelopmentKit.exe) approximiately 10GB is zelfstandig. Als u kiest ook de Windows Server 2016 evaluatie versie ISO-bestand te downloaden, wordt de downloadgrootte verhoogd naar ongeveer 17GB. U kunt dat ISO-bestand maken en een installatiekopie van Windows Server 2016 virtuele machine toevoegen aan de Stack Azure Marketplace nadat ASDK installatie is voltooid. Houd er rekening mee dat deze installatiekopie van Windows Server 2016 evaluatie kan alleen worden gebruikt met de ASDK en onderworpen aan de licentievoorwaarden ASDK is.

5. Nadat het downloaden is voltooid, klikt u op **uitvoeren** starten van de ASDK zelfstandig uitpakken (AzureStackDevelopmentKit.exe).
6. Lees en accepteer de gebruiksrechtovereenkomst voor weergegeven van de **License Agreement** pagina van de Wizard voor zelfstandig uitpakken en klik vervolgens op **volgende**.
7. Lees de privacy-instructie informatie weergegeven op de **belangrijke mededeling** pagina van de Wizard voor zelfstandig uitpakken en klik vervolgens op **volgende**.
8. Selecteer de locatie voor de setup-bestanden worden uitgepakt naar op Azure-Stack de **doellocatie Selecteer** pagina van de Wizard voor zelfstandig uitpakken en klik vervolgens op **volgende**. De standaardlocatie is *huidige map*\Azure Stack Development Kit. 
9. Controleer de doellocatie samenvatting op de **gereed voor uitpakken** pagina van de Wizard voor zelfstandig uitpakken en klik vervolgens op **extraheren** uitpakken van de CloudBuilder.vhdx (ongeveer 25 GB) en ThirdPartyLicenses.rtf bestanden. Dit kan enige tijd duren.
10. Kopieer of het bestand CloudBuilder.vhdx verplaatsen naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx) op de hostcomputer ASDK.

> [!NOTE]
> Nadat u de bestanden hebt uitgepakt, verwijdert u de. EXE en. Bestanden van de OPSLAGLOCATIE voor het herstellen van harde-schijfruimte. Of u kunt back-up van deze bestanden zodat u hoeft niet te downloaden de bestanden opnieuw als u wilt de ASDK implementeren.

## <a name="deploy-the-asdk-using-a-guided-experience"></a>De ASDK met behulp van een begeleide ervaring implementeren
De ASDK kan worden geïmplementeerd met behulp van een grafische gebruikersinterface (GUI) dat is opgegeven door te downloaden en uitvoeren van het asdk installer.ps1 PowerShell-script.

> [!NOTE]
> De gebruikersinterface installatieprogramma voor de Azure-Stack Development Kit is een open brongegevens script op basis van WCF en PowerShell.

### <a name="prepare-the-development-kit-host-using-a-guided-user-experience"></a>De development kit host met behulp van een begeleide gebruikerservaring voorbereiden
Voordat u de ASDK op de hostcomputer installeren kunt, moet de omgeving ASDK worden voorbereid.
1. Meld u aan als lokale beheerder op de computer van de host ASDK.
2. Zorg ervoor dat het bestand CloudBuilder.vhdx is verplaatst naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx).
3. Voer het volgende script voor het downloaden van het installatiebestand van development kit (asdk installer.ps1) van de [Azure Stack GitHub-opslagplaats voor extra](https://github.com/Azure/AzureStack-Tools) naar de **C:\AzureStack_Installer** map op uw Development kit hostcomputer:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. Op een PowerShell-console met verhoogde bevoegdheid, start de **C:\AzureStack_Installer\asdk-installer.ps1** script en klik vervolgens op **omgeving voorbereiden**.
5. Op de **Selecteer Cloudbuilder vhdx** pagina van het installatieprogramma, blader naar en selecteer de **cloudbuilder.vhdx** -bestand dat u hebt gedownload en uitgepakt in de vorige stappen. Schakel op deze pagina u ook, eventueel kunt de **toevoegen van stuurprogramma's** selectievakje in als u moet extra stuurprogramma's toevoegen aan de hostcomputer development kit.  
6. Op de **optionele instellingen** pagina, geeft u het lokale administrator-account voor de hostcomputer development kit. 

  > [!IMPORTANT]
  > Als u deze referenties niet opgeeft, moet u direct of KVM-toegang tot de host nadat de computer opnieuw als onderdeel van het instellen van de development kit opgestart.

7. U kunt ook deze optionele instellingen opgeven op de **optionele instellingen** pagina:
    - **Computernaam**: deze optie stelt u de naam van de host van development kit. De naam moet voldoen aan de vereisten van de FQDN-naam en moet zijn dan 15 tekens of minder. De standaardwaarde is de naam van een willekeurige computer door Windows wordt gegenereerd.
    - **Tijdzone**: Hiermee stelt u de tijdzone voor de host van development kit. De standaardwaarde is (UTC-8:00) Pacific Time (VS en Canada).
    - **Statische IP-configuratie**: Hiermee stelt u uw implementatie een statische IP-adres gebruiken. Wanneer het installatieprogramma opnieuw wordt opgestart in de cloudbuilder.vhx, worden de netwerkinterfaces anders geconfigureerd met DHCP.
11. Klik op **Volgende**.
12. Als u een statisch IP-configuratie in de vorige stap hebt gekozen, moet u nu het volgende doen:
    - Selecteer een netwerkadapter. Zorg ervoor dat u kunt verbinding maken met de netwerkadapter voordat u op **volgende**.
    - Zorg ervoor dat de **IP-adres**, **Gateway**, en **DNS** waarden juist zijn en klik vervolgens op **volgende**.
13. Klik op **volgende** starten van het voorbereidingsproces.
14. Wanneer de voorbereiding blijkt **voltooid**, klikt u op **volgende**.
15. Klik op **nu opnieuw opstarten** opnieuw opgestart in de cloudbuilder.vhdx en doorgaan met het implementatieproces.


### <a name="deploy-the-development-kit-using-a-guided-experience"></a>De development kit met behulp van een begeleide ervaring implementeren
Na het voorbereiden van de hostcomputer ASDK, kan de ASDK worden geïmplementeerd in de installatiekopie van het CloudBuilder.vhdx met behulp van de volgende stappen uit. 
1. Nadat de computer is naar de installatiekopie van het CloudBuilder.vhdx opgestart, moet u aanmelden met de administrator-referenties opgegeven in de vorige stappen. 
2. Open een PowerShell-console met verhoogde bevoegdheid en voer de **\AzureStack_Installer\asdk-installer.ps1** script (dit is mogelijk nu op een ander station in de afbeelding CloudBuilder.vhdx). Klik op **Install**.
3. In de **Type** vervolgkeuzelijst, selecteer **Azure-Cloud** of **AD FS**.
    - **Azure-Cloud**: Hiermee configureert u Azure Active Directory (Azure AD) als de id-provider. Om deze optie gebruikt, moet u een internetverbinding, de volledige naam van een Azure AD directory-tenant in de vorm van *domainname*. onmicrosoft.com of een Azure AD geverifieerd domein aangepaste naam en een globale beheerdersreferenties voor de opgegeven map. 
    - **AD FS**: de standaardstempel Active Directory wordt gebruikt als de id-provider. Het standaardaccount voor aanmelden is azurestackadmin@azurestack.local, en het wachtwoord te gebruiken die u hebt opgegeven als onderdeel van de installatie is.
4. Onder **lokale administrator-wachtwoord**, in de **wachtwoord** vak, typt u het lokale administrator-wachtwoord (die overeenkomen met de huidige geconfigureerde lokale administrator-wachtwoord) en klik vervolgens op **Volgende**.
5. Selecteer een netwerkadapter wilt gebruiken voor de development kit en klik vervolgens op **volgende**.
6. Selecteer DHCP of statische netwerkconfiguratie voor de BGPNAT01 virtuele machine.
    - **DHCP** (standaard): de virtuele machine krijgt de IP-configuratie van de DHCP-server.
    - **Statische**: Gebruik deze optie alleen als DHCP niet een geldig IP-adres voor Azure-Stack toegang tot Internet toewijzen. Een statisch IP-adres moet worden opgegeven met de lengte van het subnetmasker in CIDR-notatie (bijvoorbeeld 10.0.0.5/24).
7. Stel desgewenst de volgende waarden:
    - **VLAN-ID**: Hiermee stelt u de VLAN-ID. Gebruik deze optie alleen als de host en AzS BGPNAT01 VLAN-ID voor toegang tot het fysieke netwerk (en internet) moet configureren. 
    - **DNS-doorstuurserver**: een DNS-server is gemaakt als onderdeel van de Azure-Stack-implementatie. Geef uw bestaande infrastructuur DNS-server zodat computers binnen de oplossing voor het omzetten van namen buiten de stempel. De in het stempel DNS-server stuurt onbekende aanvragen voor naamomzetting naar deze server.
    - **Tijdserver**: dit veld stelt de time-server moet worden gebruikt door de development kit vereist. Deze parameter moet worden opgegeven als een geldige tijd server IP-adres. Servernamen worden niet ondersteund.
  
  > [!TIP]
  > Ga voor een IP-adres naar [pool.ntp.org](http:\\pool.ntp.org) of time.windows.com pingen. 
  
8. Klik op **Volgende**. 
9. Op de **network interface card eigenschappen controleren** pagina ziet u een voortgangsbalk weergegeven. 
    - Als er op het **een update kan niet worden gedownload**, volg de instructies op de pagina.
    - Wanneer deze zegt **voltooid**, klikt u op **volgende**.
10. Op **samenvatting** pagina, klikt u op **implementeren**. U kunt hier ook de PowerShell-opdrachten voor installatie die wordt gebruikt voor het installeren van de development kit kopiëren.
11. Als u de implementatie van een Azure AD gebruikt, wordt u gevraagd om in te voeren van uw Azure AD-referenties voor het account van globale beheerder een paar minuten nadat setup is gestart.
12. De implementatie kan enkele uren, waarin het systeem automatisch opnieuw eenmaal opgestart duren. Als de implementatie is geslaagd, de PowerShell-console wordt weergegeven: **voltooid: actie 'Implementatie'**. Als de implementatie mislukt, kunt u [implementeren](azure-stack-redeploy.md) maakt of gebruik de volgende PowerShell-opdrachten, vanuit de dezelfde verhoogde PowerShell-venster, opnieuw opstarten van de implementatie van de laatste geslaagde stap:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

   > [!IMPORTANT]
   > Als u de voortgang van de implementatie wilt, meld u als azurestack\AzureStackAdmin wanneer de development kit host opnieuw wordt opgestart tijdens de installatie. Als u zich aanmeldt als lokale beheerder nadat de computer is toegevoegd aan het domein, kunt u de voortgang van de implementatie niet zien. Implementatie opnieuw uitgevoerd, in plaats daarvan Meld u aan als azurestack\AzureStackAdmin om te valideren dat deze wordt uitgevoerd niet.
   

## <a name="deploy-the-asdk-using-powershell"></a>De ASDK met behulp van PowerShell implementeren
De vorige stappen doorlopen u de ASDK met behulp van een begeleide gebruikerservaring implementeren. U kunt ook PowerShell gebruiken voor het implementeren van de ASDK op de host van development kit volgens de stappen in deze sectie.

### <a name="configure-the-asdk-host-computer-to-boot-from-cloudbuildervhdx"></a>De hostcomputer ASDK opstarten vanaf CloudBuilder.vhdx configureren
Deze opdrachten wordt de computer van de host ASDK om te starten vanaf de gedownloade en uitgepakte Azure Stack virtuele vaste schijf (CloudBuilder.vhdx) configureren. Start opnieuw op de hostcomputer ASDK na het voltooien van deze stappen.

1. Start een opdrachtprompt als beheerder.
2. Voer `bcdedit /copy {current} /d "Azure Stack"` uit.
3. Kopiëren (CTRL + C) de CLSID-waarde geretourneerd, inclusief de vereiste {} ' s. Deze waarde wordt aangeduid als {CLSID} en moet worden geplakt in (CTRL + V of klik met de rechtermuisknop) in de resterende stappen.
4. Voer `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` uit. 
5. Voer `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` uit. 
6. Voer `bcdedit /set {CLSID} detecthal on` uit. 
7. Voer `bcdedit /default {CLSID}` uit.
8. Uitvoeren om te controleren of de instellingen voor opstarten, `bcdedit`. 
9. Zorg ervoor dat de CloudBuilder.vhdx bestand is verplaatst naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx) en de development kit hostcomputer opnieuw opstarten. Wanneer de host ASDK opnieuw wordt opgestart moet het nu standaard met het starten van de VM CloudBuilder.vhdx. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>De development kit host met behulp van PowerShell voorbereiden 
Nadat de hostcomputer development kit is naar de installatiekopie van het CloudBuilder.vhdx opgestart, kunt u open een PowerShell-console met verhoogde bevoegdheid en voer de opdrachten in deze sectie voor het implementeren van de ASDK op de host van development kit.

> [!IMPORTANT] 
> ASDK installatie ondersteunt één netwerkinterfacekaart (NIC) voor netwerken. Als er meerdere NIC's, zorg ervoor dat slechts één is ingeschakeld (en alle andere protocollen zijn uitgeschakeld) voordat het script voor implementatie wordt uitgevoerd.

U kunt Azure Stack met Azure AD of AD FS implementeren als de id-provider. Werken op dezelfde manier met zowel Azure Stack, resourceproviders en andere toepassingen. Zie voor meer informatie over wat er wordt ondersteund met AD FS in Azure-Stack, de [belangrijke functies en -concepten](.\azure-stack-key-features.md) artikel.

> [!TIP]
> Als u geen (Zie InstallAzureStackPOC.ps1 optionele parameters en de volgende voorbeelden) setup parameters opgeeft, wordt u gevraagd voor de vereiste parameters.

### <a name="deploy-azure-stack-using-azure-ad"></a>Azure-Stack gebruikmaken van Azure AD implementeren 
Voor het implementeren van Azure-Stack **gebruikmaken van Azure AD als de id-provider**, moet u verbinding met internet hebben, rechtstreeks of via een transparentproxy. Voer de volgende PowerShell-opdrachten voor het implementeren van de development kit gebruikmaken van Azure AD:

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator     
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password
  ```

  Enkele minuten in ASDK installatie wordt u gevraagd om Azure AD-referenties. U moet hoofdbeheerdersreferenties opgeven voor uw Azure AD-tenant. 

### <a name="deploy-azure-stack-using-ad-fs"></a>Azure-Stack met AD FS implementeren 
Voor het implementeren van de development kit **AD FS gebruikt als de id-provider**, voer de volgende PowerShell-opdrachten (alleen moet u de parameter - UseADFS toevoegen): 

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator 
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -UseADFS
  ```

In AD FS-implementaties, de standaardstempel Active Directory wordt gebruikt als de id-provider. Het standaardaccount voor aanmelden is azurestackadmin@azurestack.local, en het wachtwoord wordt zo ingesteld dat u hebt opgegeven als onderdeel van de PowerShell-opdrachten voor installatie.

De implementatie kan enkele uren en tijdens deze periode het systeem automatisch opnieuw eenmaal opgestart duren. Als de implementatie is geslaagd, de PowerShell-console wordt weergegeven: **voltooid: actie 'Implementatie'**. Als de implementatie mislukt, kunt u proberen uitvoeren van het script opnieuw met de - parameter opnieuw uitvoeren. U kunt [ASDK implementeren](.\azure-stack-redeploy.md) vanaf het begin.
> [!IMPORTANT]
> Als u bewaken van de implementatie uitgevoerd wilt nadat de host ASDK opnieuw is opgestart, moet u zich aanmelden als AzureStack\AzureStackAdmin. Als u zich aanmeldt als lokale beheerder nadat de computer wordt opnieuw opgestart (en toegevoegd aan het domein azurestack.local), kunt u de voortgang van de implementatie niet zien. Implementatie opnieuw uitgevoerd, in plaats daarvan Meld u aan als azurestack om te valideren dat deze wordt uitgevoerd niet.
>

#### <a name="azure-ad-deployment-script-examples"></a>Voorbeelden van Azure AD-implementatie scripts
U kunt het gehele script Azure AD-implementatie. Hier volgen enkele opmerkingen voorbeelden die een aantal optionele parameters bevatten.

Als uw Azure AD-identiteit is alleen gekoppeld aan **één** Azure AD-directory:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 52.168.138.145 #Example time server IP address.
```

Als uw Azure AD-identiteit is gekoppeld aan **meer dan één** Azure AD-directory:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" #Example: user@AADDirName.onmicrosoft.com 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<Azure AD directory in the form of domainname.onmicrosoft.com or an Azure AD verified custom domain name>" -TimeServer 52.168.138.145 #Example time server IP address.
```

Als uw omgeving **heeft geen** hebben DHCP ingeschakeld en vervolgens moet u de volgende aanvullende parameters op een van de opties hierboven (voorbeeld gebruik opgegeven) opnemen: 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1 -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>Optionele parameters ASDK InstallAzureStackPOC.ps1
|Parameter|Vereiste/optionele|Beschrijving|
|-----|-----|-----|
|AdminPassword|Vereist|Hiermee stelt u het lokale administrator-account en alle andere gebruikersaccounts op alle virtuele machines die worden gemaakt als onderdeel van de implementatie van development kit. Dit wachtwoord moet overeenkomen met het huidige lokale beheerderswachtwoord op de host.|
|InfraAzureDirectoryTenantName|Vereist|Hiermee stelt u de tenantmap. Gebruik deze parameter om op te geven van een specifieke map waar de AAD-account de machtigingen voor het beheren van meerdere directory's heeft. Volledige naam van een Directory-Tenant van AAD in de notatie. onmicrosoft.com of een Azure AD geverifieerd aangepaste domeinnaam.|
|TimeServer|Vereist|Gebruik deze parameter om op te geven van een specifiek tijdstip-server. Deze parameter moet worden opgegeven als een geldige tijd server IP-adres. Servernamen worden niet ondersteund.|
|InfraAzureDirectoryTenantAdminCredential|Optioneel|Hiermee stelt u de Azure Active Directory-gebruikersnaam en wachtwoord. Deze Azure-referenties moet een organisatie ID.|
|InfraAzureEnvironment|Optioneel|Selecteer de Azure-omgeving die u wilt deze Azure-Stack-implementatie te registreren. Opties voor zijn openbare Azure, Azure - China, Azure - US Government.|
|DNSForwarder|Optioneel|Een DNS-server is gemaakt als onderdeel van de Azure-Stack-implementatie. Geef uw bestaande infrastructuur DNS-server zodat computers binnen de oplossing voor het omzetten van namen buiten de stempel. De in het stempel DNS-server stuurt onbekende aanvragen voor naamomzetting naar deze server.|
|NatIPv4Address|Vereist voor DHCP NAT-ondersteuning|Hiermee stelt een statisch IP-adres voor MAS BGPNAT01. Gebruik deze parameter alleen als de DHCP geen geldig IP-adres voor toegang tot het internet kan toewijzen.|
|NatIPv4Subnet|Vereist voor DHCP NAT-ondersteuning|IP-Subnet voorvoegsel gebruikt voor DHCP via NAT-ondersteuning. Gebruik deze parameter alleen als de DHCP geen geldig IP-adres voor toegang tot het internet kan toewijzen.|
|PublicVlanId|Optioneel|Hiermee stelt u de VLAN-ID. Gebruik deze parameter alleen als de host en MAS BGPNAT01 VLAN-ID voor toegang tot het fysieke netwerk (en Internet) moet configureren. Bijvoorbeeld,.\InstallAzureStackPOC.ps1-Verbose - PublicVLan 305|
|Opnieuw uitvoeren|Optioneel|Gebruik deze vlag opnieuw uit te voeren van de implementatie. Alle vorige invoer wordt gebruikt. Opnieuw invoeren van de gegevens eerder hebt opgegeven, wordt niet ondersteund omdat enkele unieke waarden worden gegenereerd en voor implementatie gebruikt.|

## <a name="activate-the-administrator-and-tenant-portals"></a>Activeren van de portals beheerder- en tenantverkeer
Na de implementaties die gebruikmaken van Azure AD, moet u zowel de Azure-Stack-beheerder- en tenantverkeer de portals activeren. Deze activering instemt geven de Stack van Azure portal en Azure Resource Manager de juiste machtigingen (die wordt weergegeven op de pagina toestemming) voor alle gebruikers van de map.

- Navigeer naar https://adminportal.local.azurestack.external/guest/signup, lees de informatie en klik op voor de beheerdersportal **accepteren**. Nadat u hebt geaccepteerd, kunt u servicebeheerders die niet ook tenantbeheerders directory zijn toevoegen.

- Navigeer naar https://portal.local.azurestack.external/guest/signup, lees de informatie en klik op voor de tenantportal **accepteren**. Nadat u hebt geaccepteerd, kunnen gebruikers in de map kunnen aanmelden bij de tenantportal. 

> [!NOTE] 
> Als de portals zijn niet geactiveerd, kan de beheerder van de directory aanmelden en gebruik van de portals. Als een andere gebruiker zich aanmeldt, zien ze een foutbericht dat aangeeft dat de beheerder geen aan andere gebruikers machtigingen heeft. Als de beheerder systeemeigen niet tot de map behoort die voor Azure-Stack is geregistreerd, moet de Azure-Stack-map worden toegevoegd aan de activerings-URL. Bijvoorbeeld, als Azure-Stack is geregistreerd voor fabrikam.onmicrosoft.com en de gebruiker met beheerdersrechten is admin@contoso.com, gaat u naar https://portal.local.azurestack.external/guest/signup/fabrikam.onmicrosoft.com voor het activeren van de portal. 

## <a name="reset-the-password-expiration-policy"></a>Het vervalbeleid voor wachtwoord opnieuw instellen 
Om ervoor te zorgen dat het wachtwoord voor de development kit host voordat uw evaluatieversie is afgelopen periode niet verloopt, door deze stappen uit te voeren nadat u de ASDK hebt geïmplementeerd.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Het vervalbeleid voor wachtwoord wijzigen vanuit Powershell:
Voer de opdracht vanaf een verhoogde Powershell-console:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Het vervalbeleid voor wachtwoorden handmatig wijzigen:
1. Open op de host van de kit ontwikkeling **Group Policy Management** en navigeer naar **Group Policy Management** – **Forest: azurestack.local** – **domeinen** – **azurestack.local**.
2. Klik met de rechtermuisknop **standaarddomeinbeleid** en klik op **bewerken**.
3. In de Editor voor Groepsbeleidsbeheer, gaat u naar **Computerconfiguratie** – **beleid** – **Windows-instellingen** – **beveiligingsinstellingen**– **Beleidsregels van account** – **wachtwoordbeleid**.
4. Dubbelklik in het rechterdeelvenster op **Maximum wachtwoord leeftijd**.
5. In de **Maximum wachtwoord leeftijd eigenschappen** wijziging van het dialoogvenster de **wachtwoord verloopt over** van waarde naar 180 en klik vervolgens op **OK**.


## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)

[Setup van PowerShell voor Azure-Stack-omgevingen](azure-stack-powershell-configure-quickstart.md)

[Azure-Stack registreren bij uw Azure-abonnement](azure-stack-register.md)




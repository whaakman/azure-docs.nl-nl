---
title: PowerShell voor Azure Stack - implementeren | Microsoft Docs
description: In dit artikel, kunt u de ASDK installeren vanaf de opdrachtregel met behulp van PowerShell.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: ''
ms.date: 02/08/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 02/08/2019
ms.openlocfilehash: e8d3653049b0160fac155ebc49329c17cb27224f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58014386"
---
# <a name="deploy-the-asdk-from-the-command-line"></a>De ASDK vanaf de opdrachtregel implementeren
De ASDK is een omgeving voor testen en ontwikkeling die u implementeren kunt om te evalueren en Azure Stack-functies en -services demonstreren. Als u deze moet actief en werkend is, u de hardware van de omgeving voorbereiden en sommige scripts (dit duurt enkele uren) uitgevoerd. Hierna kunt u zich de portals beheerder en gebruiker aan de slag met Azure Stack.

## <a name="prerequisites"></a>Vereisten 
Bereid de hostcomputer development kit. Plan uw hardware, software en netwerk. De computer die als host fungeert voor de development kit (de host development kit) moet voldoen aan hardware, software en netwerkvereisten. U moet er ook voor kiezen tussen het gebruik van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS). Moet u om te voldoen aan deze vereisten voordat u begint met uw implementatie zodat het installatieproces wordt probleemloos uitgevoerd. 

Voordat u de ASDK implementeert, controleert u voldoen aan de minimale vereisten voor het installeren van de ASDK geplande development kit host hardware van de computer, besturingssysteem, -account en netwerkconfiguraties.

**[Bekijk de planningsoverwegingen ASDK-implementatie](asdk-deploy-considerations.md)**

> [!TIP]
> U kunt de [Azure Stack-implementatievereisten controleren hulpprogramma](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) na de installatie van het besturingssysteem om te bevestigen dat de hardware voldoet aan alle vereisten.

## <a name="download-and-extract-the-deployment-package"></a>Downloaden en uitpakken van het implementatiepakket
Nadat u hebt gecontroleerd of uw development kit host-computer voldoet aan de basisvereisten voor het installeren van de ASDK, wordt de volgende stap is het downloaden en uitpakken van het implementatiepakket ASDK. Het implementatiepakket bevat het bestand Cloudbuilder.vhdx, dit is een virtuele harde schijf met een besturingssysteem en de installatiebestanden van de Azure Stack.

U kunt het implementatiepakket downloaden naar de development kit-host of een andere computer. De van de uitgepakte implementatiebestanden duren 60 GB aan vrije schijfruimte beschikbaar, zodat de hardwarevereisten voor de host van development kit verminderen met behulp van een andere computer kan helpen.

**[Downloaden en uitpakken van de Azure Stack Development Kit (ASDK)](asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Voorbereiden van de hostcomputer development kit
Voordat u de ASDK op de hostcomputer installeren kunt, de omgeving moet worden voorbereid en het systeem geconfigureerd om op te starten vanaf VHD. Na deze stap wordt de host van development kit opgestart naar de Cloudbuilder.vhdx (een virtuele harde schijf met een besturingssysteem en de installatiebestanden van de Azure Stack).

PowerShell gebruiken voor het configureren van de computer opstarten vanuit CloudBuilder.vhdx ASDK. Deze opdrachten configureren uw ASDK hostcomputer opstarten vanuit de gedownloade en uitgepakte Azure Stack virtuele harde schijf (CloudBuilder.vhdx). Start de computer van de host ASDK na het voltooien van deze stappen.

De computer opstarten vanuit CloudBuilder.vhdx ASDK configureren:

  1. Start een opdrachtprompt als beheerder.
  2. Voer `bcdedit /copy {current} /d "Azure Stack"` uit.
  3. Kopiëren (CTRL + C) de CLSID-waarde geretourneerd, met inbegrip van de vereiste {}' s. Deze waarde wordt aangeduid als {CLSID} en moet worden geplakt in (CTRL + V of klik met de rechtermuisknop) in de resterende stappen.
  4. Voer `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` uit. 
  5. Voer `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` uit. 
  6. Voer `bcdedit /set {CLSID} detecthal on` uit. 
  7. Voer `bcdedit /default {CLSID}` uit.
  8. Uitvoeren om te controleren of de instellingen voor opstarten, `bcdedit`. 
  9. Zorg ervoor dat de CloudBuilder.vhdx bestand is verplaatst naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx) en start opnieuw op de hostcomputer development kit. Wanneer de hostcomputer ASDK opnieuw wordt opgestart, moet het opstarten van de virtuele machine CloudBuilder.vhdx vaste schijf om te beginnen met ASDK implementatie. 

> [!IMPORTANT]
> Zorg ervoor dat u direct fysieke of KVM-toegang tot de hostcomputer development kit hebben voordat opnieuw kan worden gestart. Wanneer de virtuele machine eerst wordt gestart, wordt u gevraagd om Windows Server-installatie te voltooien. Geef de dezelfde beheerdersreferenties die u gebruikt voor aanmelding bij de computer development kit. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>Voorbereiden van de development kit-host met behulp van PowerShell 
Na de development kit hostcomputer is opgestart in de afbeelding CloudBuilder.vhdx, meld u aan met de dezelfde lokale beheerdersreferenties u gebruikt voor aanmelding bij de computer development kit (en die u hebt opgegeven als onderdeel van Windows Server wordt voltooid Instellen wanneer de computer opgestart vanaf VHD). 

> [!NOTE]
> (Optioneel) u kunt ook configureren [telemetrie-instellingen voor Azure Stack](asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *voordat* de ASDK installeren.

Open een PowerShell-console met verhoogde bevoegdheid en voer de opdrachten in deze sectie om de ASDK op de host van development kit implementeren.

> [!IMPORTANT] 
> ASDK installatie ondersteunt precies één netwerkinterfacekaart (NIC) voor netwerken. Als u meerdere NIC's hebt, zorg ervoor dat slechts één is ingeschakeld (en alle andere zijn uitgeschakeld) voordat u het implementatiescript uitvoert.

U kunt implementeren Azure Stack met Azure AD- of Windows Server AD FS als id-provider. Azure Stack, resourceproviders en andere toepassingen werken op dezelfde manier met beide.

> [!TIP]
> Als u een setup-parameters (Zie de optionele parameters InstallAzureStackPOC.ps1 en voorbeelden) niet opgeeft, wordt u gevraagd de vereiste parameters op te geven.

### <a name="deploy-azure-stack-using-azure-ad"></a>Azure Stack met behulp van Azure AD implementeren 
Azure Stack implementeren **met behulp van Azure AD als id-provider**, moet u verbinding met internet hebben, rechtstreeks of via een transparante proxy. 

Voer de volgende PowerShell-opdrachten voor het implementeren van de development kit met behulp van Azure AD:

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator     
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password
  ```

Een paar minuten in ASDK-installatie die wordt u gevraagd voor Azure AD-referenties. U moet hoofdbeheerdersreferenties opgeven voor uw Azure AD-tenant. 

Na de implementatie is Azure Active Directory-globale beheerder toestemming niet vereist. Bepaalde bewerkingen is echter mogelijk de referentie van de globale beheerder. Bijvoorbeeld, een resource provider installatieprogramma script of een nieuwe functie die een machtiging worden verleend. U kunt tijdelijk opnieuw installeren van de account hoofdbeheerder machtigingen of gebruik een afzonderlijke globale beheerder-account dat is eigenaar van de *providerabonnement standaard*.

### <a name="deploy-azure-stack-using-ad-fs"></a>Azure Stack met behulp van AD FS implementeren 
De development kit implementeren **met AD FS als id-provider**, voer de volgende PowerShell-opdrachten (u hoeft toe te voegen met de parameter - UseADFS): 

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator 
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -UseADFS
  ```

In AD FS-implementaties, de standaardstempel Active Directory wordt gebruikt als de id-provider. Het standaardaccount voor het aanmelden is azurestackadmin@azurestack.local, en het wachtwoord wordt ingesteld op wat u hebt opgegeven als onderdeel van de PowerShell-opdrachten voor installatie.

De implementatie kan enkele uren duren, gedurende welke het systeem automatisch opnieuw wordt opgestart nadat. Als de implementatie is geslaagd, wordt de PowerShell-console wordt weergegeven: **VOLTOOID: Actie 'Implementatie'**. Als de implementatie mislukt, kunt u proberen uitvoeren van het script opnieuw uit met behulp van de - parameter opnieuw uitvoeren. U kunt [opnieuw implementeren ASDK](asdk-redeploy.md) helemaal.

> [!IMPORTANT]
> Als u de voortgang van de implementatie controleren wilt nadat de host ASDK opnieuw is opgestart, moet u zich aanmelden als AzureStack\AzureStackAdmin. Als u zich aanmeldt als een lokale beheerder nadat de computer opnieuw opgestart (en toegevoegd aan het domein azurestack.local), kunt u de voortgang van de implementatie niet weergegeven. Implementatie opnieuw uitvoeren, meld u in plaats daarvan als azurestack aan om te valideren dat deze wordt uitgevoerd niet.


#### <a name="azure-ad-deployment-script-examples"></a>Azure AD-scriptvoorbeelden voor implementatie
U kunt het volledige script Azure AD-implementatie. Hier volgen enkele opmerkingen voorbeelden die enkele optionele parameters bevatten.

Als uw Azure AD-identiteit is alleen gekoppeld aan **één** Azure AD-directory:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 52.168.138.145 #Example time server IP address.
```

Als uw Azure AD-identiteit die is gekoppeld aan **meer dan één** Azure AD-directory:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" #Example: user@AADDirName.onmicrosoft.com 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<Azure AD directory in the form of domainname.onmicrosoft.com or an Azure AD verified custom domain name>" -TimeServer 52.168.138.145 #Example time server IP address.
```

Als er geen DHCP ingeschakeld voor uw omgeving is, moet u de volgende extra parameters voor een van de opties hierboven (voorbeeld gebruik opgegeven) opnemen: 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>Optionele parameters ASDK InstallAzureStackPOC.ps1

|Parameter|Vereist/optioneel|Description|
|-----|-----|-----|
|AdminPassword|Vereist|Hiermee stelt het lokale administrator-account en alle accounts van andere gebruikers op alle virtuele machines die worden gemaakt als onderdeel van de implementatie van development kit. Dit wachtwoord moet overeenkomen met de huidige lokale beheerderswachtwoord op de host.|
|InfraAzureDirectoryTenantName|Vereist|Hiermee stelt u de tenant-directory. Gebruik deze parameter om op te geven van een specifieke map waar de AAD-account heeft machtigingen voor het beheren van meerdere directory's. Volledige naam van een AAD-Directory-Tenant in de indeling van. onmicrosoft.com of een Azure AD geverifieerd aangepaste domeinnaam.|
|TimeServer|Vereist|Gebruik deze parameter om op te geven van een specifiek tijdstip-server. Deze parameter moet worden opgegeven als het IP-adres van een geldige tijd-server. De namen worden niet ondersteund.|
|InfraAzureDirectoryTenantAdminCredential|Optioneel|Hiermee stelt u de Azure Active Directory-gebruikersnaam en wachtwoord. Deze Azure-referenties moet een organisatie-ID.|
|InfraAzureEnvironment|Optioneel|Selecteer de Azure-omgeving waarmee u wilt registreren van deze Azure Stack-implementatie. Opties zijn onder andere openbare Azure, Azure - China, Azure - voor de Amerikaanse overheid.|
|DNSForwarder|Optioneel|Een DNS-server gemaakt als onderdeel van de Azure Stack-implementatie. Geef uw bestaande infrastructuur DNS-server zodat computers in de oplossing voor het omzetten van namen buiten de stempel. De DNS-server in het stempel onbekende naam resolutie stuurt aanvragen door naar deze server.|
|Opnieuw uitvoeren|Optioneel|Gebruik deze eigenschap implementatie opnieuw uit te voeren. Alle vorige invoer wordt gebruikt. Opnieuw in te voeren gegevens eerder hebt opgegeven wordt niet ondersteund omdat verschillende unieke waarden worden gegenereerd en voor implementatie gebruikt.|


## <a name="perform-post-deployment-configurations"></a>Uitvoeren na de implementatie-configuraties
Na de installatie van de ASDK, zijn er enkele aanbevolen controles voor na de installatie en configuratie wijzigingen beschreven die moeten worden aangebracht. U kunt controleren of de installatie is geïnstalleerd met behulp van de cmdlet test-AzureStack en installeren van hulpprogramma's voor PowerShell voor Azure Stack- en GitHub. 

U moet ook opnieuw instellen voor het vervalbeleid voor wachtwoorden om ervoor te zorgen dat het wachtwoord voor de development kit host voordat u een einde van de evaluatie niet verloopt.

> [!NOTE]
> (Optioneel) u kunt ook configureren [telemetrie-instellingen voor Azure Stack](asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *nadat* de ASDK installeren.

**[Boeken ASDK implementatietaken](asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registreren bij Azure
U moet Azure Stack registreren met Azure zodat u kunt [Azure marketplace-items downloaden](asdk-marketplace-item.md) naar Azure Stack.

**[Azure Stack registreren bij Azure](asdk-register.md)**

## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd! Nadat u deze stappen uitvoert, hebt u een omgeving met zowel kit [beheerder](https://adminportal.local.azurestack.external) en [gebruiker](https://portal.local.azurestack.external) portals. 

[Na de installatie van de ASDK configuratietaken](asdk-post-deploy.md)


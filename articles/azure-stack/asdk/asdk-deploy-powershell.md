---
title: Azure Stack - PowerShell implementeren | Microsoft Docs
description: In deze zelfstudie kunt u de ASDK installeren vanaf de opdrachtregel.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 48ccccaba6b7f5780f1d42dfbe5d9747c5e30292
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-deploy-the-asdk-from-the-command-line"></a>Zelfstudie: de ASDK vanaf de opdrachtregel implementeren
In deze zelfstudie maakt implementeren u de Azure Stack Development Kit (ASDK) vanaf de opdrachtregel in een niet-productieomgeving. 

De ASDK is een test- en omgeving die u implementeren kunt om te evalueren en de Azure-Stack-functies en services demonstreren. Als u deze moet actief is, u de hardware van de omgeving voorbereiden en uitvoeren van sommige scripts (dit duurt enkele uren). Daarna kunt kunt u aanmelden bij de portals beheerder en gebruiker aan de slag met Azure-Stack.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Downloaden en uitpakken van het implementatiepakket
> * Voorbereiden van de hostcomputer development kit 
> * Configuraties na de implementatie uitvoeren
> * Registreren bij Azure

## <a name="prerequisites"></a>Vereisten 
Bereid de hostcomputer development kit. Plan uw hardware, software en netwerk. De computer die als host fungeert voor de development kit (de host development kit) moet voldoen aan de eisen aan hardware, software en netwerk. U moet er ook voor kiezen tussen het gebruik van Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS). Zorg ervoor dat deze vereisten voldoen voordat u uw implementatie start, zodat het installatieproces soepel wordt uitgevoerd. 

Zorg voordat u de ASDK implementeert, voldoen aan de minimumvereisten voor het installeren van de ASDK geplande development kit host hardware van de computer, besturingssysteem, account en netwerkconfiguraties.

**[Bekijk de planningsoverwegingen ASDK-implementatie](asdk-deploy-considerations.md)**

> [!TIP]
> U kunt de [Azure Stack implementatievereisten controleren hulpprogramma](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) na de installatie van het besturingssysteem om te bevestigen dat de hardware voldoet aan alle vereisten.

## <a name="download-and-extract-the-deployment-package"></a>Downloaden en uitpakken van het implementatiepakket
Nadat u hebt gecontroleerd of uw development kit host-computer voldoet aan de basisvereisten voor het installeren van de ASDK, worden de volgende stap is het downloaden en uitpakken van het implementatiepakket ASDK. Het implementatiepakket bevat de Cloudbuilder.vhdx-bestand, dat een virtuele harde schijf met een besturingssysteem en de installatiebestanden van de Azure-Stack.

U kunt het implementatiepakket downloaden naar de development kit host of een andere computer. De implementatie van de uitgepakte bestanden duren 60 GB aan vrije schijfruimte, zodat een andere computer kan helpen verminderen de hardwarevereisten voor de host van development kit.

**[Downloaden en uitpakken van de Azure Stack Development Kit (ASDK)](asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Voorbereiden van de hostcomputer development kit
Voordat u de ASDK op de hostcomputer installeren kunt, de omgeving moet worden voorbereid en het systeem geconfigureerd om op te starten vanaf VHD. Na deze stap wordt de development kit host opgestart naar de Cloudbuilder.vhdx (een virtuele harde schijf met een besturingssysteem en de installatiebestanden van de Azure-Stack).

PowerShell gebruiken voor het configureren van de computer opstarten vanaf CloudBuilder.vhdx ASDK. Deze opdrachten configureren de computer van de host ASDK om te starten vanaf de gedownloade en uitgepakte Azure Stack virtuele vaste schijf (CloudBuilder.vhdx). Start opnieuw op de hostcomputer ASDK na het voltooien van deze stappen.

De computer opstarten vanaf CloudBuilder.vhdx ASDK configureren:

  1. Start een opdrachtprompt als beheerder.
  2. Voer `bcdedit /copy {current} /d "Azure Stack"` uit.
  3. Kopiëren (CTRL + C) de CLSID-waarde geretourneerd, inclusief de vereiste {} ' s. Deze waarde wordt aangeduid als {CLSID} en moet worden geplakt in (CTRL + V of klik met de rechtermuisknop) in de resterende stappen.
  4. Voer `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` uit. 
  5. Voer `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` uit. 
  6. Voer `bcdedit /set {CLSID} detecthal on` uit. 
  7. Voer `bcdedit /default {CLSID}` uit.
  8. Uitvoeren om te controleren of de instellingen voor opstarten, `bcdedit`. 
  9. Zorg ervoor dat de CloudBuilder.vhdx bestand is verplaatst naar de hoofdmap van het station C:\ (C:\CloudBuilder.vhdx) en de development kit hostcomputer opnieuw opstarten. Wanneer de hostcomputer ASDK gestart is, moet het opstarten van de virtuele machine CloudBuilder.vhdx vaste schijf om te beginnen met ASDK-implementatie. 

> [!IMPORTANT]
> Zorg ervoor dat er directe fysieke of KVM-toegang tot de hostcomputer development kit voordat u het opnieuw. Wanneer de virtuele machine eerst wordt gestart, wordt u gevraagd om Windows Server-installatie te voltooien. Geef de dezelfde beheerdersreferenties die u gebruikt voor aanmelding bij de computer development kit. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>De development kit host met behulp van PowerShell voorbereiden 
Na de development kit hostcomputer is opgestart naar de installatiekopie van het CloudBuilder.vhdx, meld u aan met de dezelfde lokale beheerdersreferenties u gebruikt voor aanmelding bij de computer development kit (en die u hebt opgegeven als onderdeel van het afronden van Windows Server Het installatieprogramma wanneer de computer opgestart vanaf VHD). 

> [!NOTE]
> Desgewenst kunt u ook configureren [telemetrie-instellingen van Azure-Stack](asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *voordat* de ASDK installeren.

Open een PowerShell-console met verhoogde bevoegdheid en voer de opdrachten in deze sectie voor het implementeren van de ASDK op de host van development kit.

> [!IMPORTANT] 
> ASDK installatie ondersteunt één netwerkinterfacekaart (NIC) voor netwerken. Als er meerdere NIC's, zorg ervoor dat slechts één is ingeschakeld (en alle andere protocollen zijn uitgeschakeld) voordat het script voor implementatie wordt uitgevoerd.

U kunt Azure-Stack met Azure AD implementeren of Windows Server AD FS als de id-provider. Werken op dezelfde manier met zowel Azure Stack, resourceproviders en andere toepassingen.

> [!TIP]
> Als u geen (Zie InstallAzureStackPOC.ps1 optionele parameters en de volgende voorbeelden) setup parameters opgeeft, wordt u gevraagd de vereiste parameters op te geven.

### <a name="deploy-azure-stack-using-azure-ad"></a>Azure-Stack gebruikmaken van Azure AD implementeren 
Voor het implementeren van Azure-Stack **gebruikmaken van Azure AD als de id-provider**, moet u verbinding met internet hebben, rechtstreeks of via een transparentproxy. 

Voer de volgende PowerShell-opdrachten voor het implementeren van de development kit gebruikmaken van Azure AD:

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

De implementatie kan enkele uren en tijdens deze periode het systeem automatisch opnieuw eenmaal opgestart duren. Als de implementatie is geslaagd, de PowerShell-console wordt weergegeven: **voltooid: actie 'Implementatie'**. Als de implementatie mislukt, kunt u proberen uitvoeren van het script opnieuw met de - parameter opnieuw uitvoeren. U kunt [ASDK implementeren](asdk-redeploy.md) vanaf het begin.

> [!IMPORTANT]
> Als u bewaken van de implementatie uitgevoerd wilt nadat de host ASDK opnieuw is opgestart, moet u zich aanmelden als AzureStack\AzureStackAdmin. Als u zich aanmeldt als lokale beheerder nadat de computer wordt opnieuw opgestart (en toegevoegd aan het domein azurestack.local), kunt u de voortgang van de implementatie niet zien. Implementatie opnieuw uitgevoerd, in plaats daarvan Meld u aan als azurestack om te valideren dat deze wordt uitgevoerd niet.


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

Als uw omgeving geen DHCP ingeschakeld heeft, moet u de volgende aanvullende parameters op een van de opties hierboven (voorbeeld gebruik opgegeven) opnemen: 

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


## <a name="perform-post-deployment-configurations"></a>Configuraties na de implementatie uitvoeren
Na de installatie van de ASDK, zijn er enkele aanbevolen na de installatie controleert en configuratiewijzigingen die moeten worden aangebracht. U kunt controleren of de installatie is geïnstalleerd met behulp van de cmdlet test-AzureStack en hulpprogramma's Azure Stack PowerShell en GitHub installeren. 

U moet ook opnieuw instellen voor het vervalbeleid voor wachtwoorden om ervoor te zorgen dat het wachtwoord voor de development kit host niet voordat uw evaluatieversie is afgelopen periode verloopt.

> [!NOTE]
> Desgewenst kunt u ook configureren [telemetrie-instellingen van Azure-Stack](asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *nadat* de ASDK installeren.

**[Na de implementatietaken ASDK](asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registreren bij Azure
U moet Azure Stack registreren met Azure zodat u kunt [Azure marketplace-items downloaden](asdk-marketplace-item.md) naar Azure-Stack.

**[Azure Stack registreren bij Azure](asdk-register.md)**

## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd! Nadat u deze stappen uitvoert, hebt u een development kit omgeving met zowel [beheerder](https://adminportal.local.azurestack.external) en [gebruiker](https://portal.local.azurestack.external) portals. 

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Downloaden en uitpakken van het implementatiepakket
> * Voorbereiden van de hostcomputer development kit 
> * Configuraties na de implementatie uitvoeren
> * Registreren bij Azure

Ga naar de volgende zelfstudie voor informatie over het toevoegen van een Azure-Stack marketplace-item.

> [!div class="nextstepaction"]
> [Een Azure-Stack marketplace-item toevoegen](asdk-marketplace-item.md)


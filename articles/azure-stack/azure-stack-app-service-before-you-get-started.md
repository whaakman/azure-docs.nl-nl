---
title: Voordat u App Service in Azure Stack implementeren | Microsoft Docs
description: Stappen voor het voltooien voordat u App Service in Azure Stack implementeren
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: anwestg
ms.reviewer: anwestg
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: eb2c01052b8dc5fe346196a64e3fcbf7d1b69c2b
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485747"
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Voordat u aan de slag met App Service in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Voordat u Azure App Service in Azure Stack implementeren, moet u de vereiste stappen in dit artikel te voltooien.

> [!IMPORTANT]
> De update 1901 toepassen op uw geïntegreerde Azure Stack-systeem of implementeren van de meest recente Azure Stack Development Kit (ASDK) voordat u Azure App Service 1.5 implementeert.

## <a name="download-the-installer-and-helper-scripts"></a>Het installatieprogramma en helper scripts downloaden

1. Download de [App Service op Azure Stack-implementatie helperscripts](https://aka.ms/appsvconmashelpers).
2. Download de [App Service op Azure Stack-installatieprogramma](https://aka.ms/appsvconmasinstaller).
3. Pak de bestanden van het ZIP-bestand van de helper-scripts. De volgende bestanden en mappen worden geëxtraheerd:

   - Common.ps1
   - Create-AADIdentityApp.ps1
   - Create-ADFSIdentityApp.ps1
   - Create-AppServiceCerts.ps1
   - Get-AzureStackRootCert.ps1
   - Remove-AppService.ps1
   - Map modules
     - GraphAPI.psm1

## <a name="syndicate-the-custom-script-extension-from-the-marketplace"></a>Verbind de aangepaste Scriptextensie vanuit de Marketplace

Azure App Service in Azure Stack is Custom Script Extension v1.9.1 vereist.  De extensie moet [publiceren vanuit de Marketplace](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item) voordat u begint met de installatie of upgrade van Azure App Service in Azure Stack

## <a name="get-certificates"></a>Certificaten ophalen

### <a name="azure-resource-manager-root-certificate-for-azure-stack"></a>Azure Resource Manager-basiscertificaat voor Azure Stack

Open een verhoogde PowerShell-sessie op een computer die de bevoegde eindpunt op de geïntegreerde Azure Stack-systeem of Azure Stack Development Kit Host kunt bereiken.

Voer de *Get-AzureStackRootCert.ps1* script uit de map waar u de helperscripts hebt uitgepakt. Het script maakt u een basiscertificaat in dezelfde map als het script dat een App Service nodig zijn voor het maken van certificaten.

Wanneer u de volgende PowerShell-opdracht uitvoert, hebt u voor het eindpunt van de bevoegdheden en de referenties voor de AzureStack\CloudAdmin.

```powershell
    Get-AzureStackRootCert.ps1
```

#### <a name="get-azurestackrootcertps1-script-parameters"></a>Get-AzureStackRootCert.ps1 script parameters

| Parameter | Vereist of optioneel | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| PrivilegedEndpoint | Vereist | AzS-ERCS01 | Bevoegde eindpunt |
| CloudAdminCredential | Vereist | AzureStack\CloudAdmin | Referenties van het domein account voor Azure Stack-cloud-beheerders |

### <a name="certificates-required-for-asdk-deployment-of-azure-app-service"></a>Certificaten die zijn vereist voor de implementatie van Azure App Service ASDK

De *maken AppServiceCerts.ps1* script werkt met de Azure Stack-CA te maken van de vier certificaten die App Service nodig heeft.

| Bestandsnaam | Gebruiken |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | Het standaard-SSL-certificaatbestand voor App Service |
| api.appservice.local.azurestack.external.pfx | App Service API-SSL-certificaat |
| ftp.appservice.local.azurestack.external.pfx | SSL-certificaat voor App Service-uitgever |
| sso.appservice.local.azurestack.external.pfx | Toepassingscertificaat voor App Service-identiteit |

Volg deze stappen voor het maken van de certificaten:

1. Aanmelden bij de Azure Stack Development Kit-host met behulp van de AzureStack\AzureStackAdmin-account.
2. Open een PowerShell-sessie met verhoogde bevoegdheden.
3. Voer de *maken AppServiceCerts.ps1* script uit de map waar u de helperscripts hebt uitgepakt. Dit script maakt vier certificaten in dezelfde map als het script dat een App Service nodig zijn voor het maken van certificaten.
4. Een wachtwoord invoeren voor het beveiligen van het pfx-bestanden en noteer deze. Hebt u deze invoeren in de App Service op Azure Stack-installatieprogramma.

#### <a name="create-appservicecertsps1-script-parameters"></a>Create-AppServiceCerts.ps1 script parameters

| Parameter | Vereist of optioneel | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| pfxPassword | Vereist | Null | Wachtwoord dat helpt de persoonlijke sleutel van het certificaat te beschermen |
| Domeinnaam | Vereist | local.azurestack.external | Azure Stack-achtervoegsel voor de regio en het domein |

### <a name="certificates-required-for-azure-stack-production-deployment-of-azure-app-service"></a>Certificaten die vereist zijn voor Azure Stack-productie-implementatie van Azure App Service

Als u wilt de resourceprovider in productie uitvoeren, moet u de volgende certificaten:

- Certificaat van het standaarddomein
- API-certificaat
- Publicatiecertificaat
- Identiteitscertificaat

#### <a name="default-domain-certificate"></a>Certificaat van het standaarddomein

Het certificaat van het standaarddomein wordt geplaatst op de Front-End-rol. Gebruikerstoepassingen voor jokertekens of standaard domein-aanvraag in Azure App Service gebruikt dit certificaat. Het certificaat wordt ook gebruikt voor bewerkingen van bron-beheer (Kudu).

Het certificaat in PFX-indeling moet worden en moet een certificaat met jokertekens drie-onderwerp. Deze vereiste kan een certificaat van toepassing op zowel het standaarddomein en de SCM-eindpunt voor bewerkingen voor beheer van gegevensbron.

| Indeling | Voorbeeld |
| --- | --- |
| \*.appservice.\<region\>.\<DomainName\>.\<extension\> | \*.appservice.redmond.azurestack.external |
| \*.scm.appservice.<region>.<DomainName>.<extension> | \*.scm.appservice.redmond.azurestack.external |
| \*.sso.appservice.<region>.<DomainName>.<extension> | \*.sso.appservice.redmond.azurestack.external |

#### <a name="api-certificate"></a>API-certificaat

De API-certificaat is op de beheerrol geplaatst. De resourceprovider gebruikt om u te helpen bij de beveiligde API-aanroepen. Het certificaat voor publicatie moet een onderwerpnaam die overeenkomt met de API-DNS-vermelding bevatten.

| Indeling | Voorbeeld |
| --- | --- |
| api.appservice.\<region\>.\<DomainName\>.\<extension\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Publicatiecertificaat

Het certificaat voor de rol van de uitgever beveiligt het FTPS-verkeer voor toepassingseigenaren van wanneer ze inhoud uploaden. Het certificaat voor publicatie moet een onderwerpnaam die overeenkomt met de FTPS DNS-vermelding bevatten.

| Indeling | Voorbeeld |
| --- | --- |
| ftp.appservice.\<region\>.\<DomainName\>.\<extension\> | ftp.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Identiteitscertificaat

Het certificaat voor de identiteitstoepassing kunt:

- Integratie tussen de Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) directory, Azure Stack, en App Service die ondersteuning bieden voor integratie met de compute resourceprovider.
- Eenmalige aanmelding scenario's voor geavanceerde ontwikkelhulpprogramma's in Azure App Service in Azure Stack.

Het certificaat voor de id moet een onderwerpnaam die overeenkomt met de volgende indeling bevatten.

| Indeling | Voorbeeld |
| --- | --- |
| sso.appservice.\<region\>.\<DomainName\>.\<extension\> | sso.appservice.redmond.azurestack.external |

### <a name="validate-certificates"></a>Certificaten valideren

Voordat u de app service-resourceprovider implementeert, moet u [valideren van de certificaten te gebruiken Vernieuwingsdatum](azure-stack-validate-pki-certs.md#perform-platform-as-a-service-certificate-validation) met behulp van de Azure Stack gereedheid van de Registercontrole beschikbaar is via de [PowerShell Gallery](https://aka.ms/AzsReadinessChecker). Het hulpprogramma Azure Stack Readiness Checker valideert dat de gegenereerde PKI-certificaten geschikt voor implementatie van app-services zijn.

Als een best practice, als u werkt met een van de benodigde [Azure Stack PKI-certificaten](azure-stack-pki-certs.md), moet u van plan bent om te laten voldoende tijd om te testen en certificaten opnieuw uitgegeven, indien nodig.

## <a name="virtual-network"></a>Virtueel netwerk

> [!NOTE]
> Het vooraf maken van een aangepaste virtueel netwerk is optioneel, zoals de Azure App Service in Azure Stack de vereiste virtueel netwerk kunt maken, maar vervolgens moet communiceren met de SQL- en File Server via het openbare IP-adressen.

Azure App Service in Azure Stack kunt u de resourceprovider implementeren op een bestaand virtueel netwerk of kunt u een virtueel netwerk maken als onderdeel van de implementatie. Met behulp van een bestaand virtueel netwerk, kunt het gebruik van interne IP-adressen verbinding maken met de bestandsserver en de SQL server vereist voor Azure App Service in Azure Stack. Het virtuele netwerk moet worden geconfigureerd met de volgende adresbereik en subnetten voor de installatie van Azure App Service in Azure Stack:

Virtual Network - /16

Subnetten

- ControllersSubnet /24
- ManagementServersSubnet /24
- FrontEndsSubnet /24
- PublishersSubnet /24
- WorkersSubnet /21

## <a name="licensing-concerns-for-required-file-server-and-sql-server"></a>Opmerkingen voor het vereiste bestandsserver en SQL Server-licentieverlening

Azure App Service in Azure Stack is vereist voor een bestandsserver en de SQL Server om te werken.  U staat op bestaande resources die zich buiten uw Azure Stack-implementatie gebruiken of implementeren van resources binnen het Azure Stack standaard Provider-abonnement.

Als u ervoor kiest om de resources binnen uw Azure Stack standaard Provider-abonnement te implementeren, zijn de licenties voor deze resources (Windows Server-licenties en licenties voor SQL Server) opgenomen in de kosten van Azure App Service in Azure Stack, afhankelijk van de volgende beperkingen:

- de infrastructuur wordt geïmplementeerd in de **Providerabonnement standaard**;
- de infrastructuur wordt uitsluitend gebruikt door de Azure App Service op Azure Stack-resourceprovider.  Er zijn geen andere werkbelastingen, met beheerdersrechten (bijvoorbeeld SQL-RP andere resourceproviders) of tenant (bijvoorbeeld tenant toepassingen, waarvoor een database), zijn toegestaan om het gebruik van deze infrastructuur.

## <a name="prepare-the-file-server"></a>De bestandsserver voorbereiden

Azure App Service is vereist voor het gebruik van een bestandsserver. Voor productie-implementaties, moet de server worden geconfigureerd om maximaal beschikbare en kan afhandelen van fouten.

### <a name="quickstart-template-for-file-server-for-deployments-of-azure-app-service-on-asdk"></a>QuickStart-sjabloon voor de bestandsserver voor implementaties van Azure App Service op ASDK.

Voor alleen voor implementaties van Azure Stack Development Kit, kunt u de [voorbeeld van Azure Resource Manager-implementatiesjabloon](https://aka.ms/appsvconmasdkfstemplate) om een geconfigureerde bestandsserver van één knooppunt te implementeren. De bestandsserver van één knooppunt bevindt zich in een werkgroep.

### <a name="quickstart-template-for-highly-available-file-server-and-sql-server"></a>QuickStart-sjabloon voor maximaal beschikbare bestandsserver en SQL Server

Een [reference architecture-snelstartsjabloon](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/appservice-fileserver-sqlserver-ha) is nu beschikbaar, die de File Server, SQL Server wordt geïmplementeerd, ondersteuning van Active Directory-infrastructuur in een Virtueelnetwerk geconfigureerd ter ondersteuning van een maximaal beschikbare implementatie van Azure App Service op Azure Stack.

### <a name="steps-to-deploy-a-custom-file-server"></a>Stappen voor het implementeren van een aangepaste-bestandsserver

>[!IMPORTANT]
> Als u ervoor kiest om App Service in een bestaand Virtueelnetwerk te implementeren, moet de Server worden geïmplementeerd in een apart Subnet vanuit App Service.

#### <a name="provision-groups-and-accounts-in-active-directory"></a>Inrichten van groepen en accounts in Active Directory

1. Maak de volgende globale beveiligingsgroepen van Active Directory:

   - FileShareOwners
   - FileShareUsers

2. Maak de volgende Active Directory-accounts als service-accounts:

   - FileShareOwner
   - FileShareUser

   Het wordt aanbevolen de gebruikers voor deze accounts (en voor alle webrollen) moet als een uniek en sterke gebruikersnamen en wachtwoorden hebben. Stel de wachtwoorden met de volgende voorwaarden:

   - Schakel **wachtwoord verloopt nooit**.
   - Schakel **gebruiker kan wachtwoord niet wijzigen**.
   - Uitschakelen **gebruiker moet wachtwoord bij volgende aanmelding wijzigen**.

3. Voeg als volgt de accounts toe aan het lidmaatschap van groepen:

   - Voeg **FileShareOwner** naar de **FileShareOwners** groep.
   - Voeg **FileShareUser** naar de **FileShareUsers** groep.

#### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Inrichten van groepen en accounts in een werkgroep

>[!NOTE]
> Wanneer u een bestandsserver, voer de volgende opdrachten uit configureert een **Beheerdersopdrachtprompt**. <br>***Gebruik geen PowerShell.***

Wanneer u de Azure Resource Manager-sjabloon gebruikt, worden de gebruikers al gemaakt.

1. Voer de volgende opdrachten om de FileShareOwner en FileShareUser-accounts te maken. Vervang `<password>` door uw eigen waarden.

   ``` DOS
   net user FileShareOwner <password> /add /expires:never /passwordchg:no
   net user FileShareUser <password> /add /expires:never /passwordchg:no
   ```

2. Stel de wachtwoorden voor de accounts op nooit verlopen door het uitvoeren van de volgende WMIC-opdrachten:

   ``` DOS
   WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
   WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
   ```

3. De lokale groepen FileShareUsers en FileShareOwners maken en de accounts in de eerste stap toevoegen aan deze:

   ``` DOS
   net localgroup FileShareUsers /add
   net localgroup FileShareUsers FileShareUser /add
   net localgroup FileShareOwners /add
   net localgroup FileShareOwners FileShareOwner /add
   ```

#### <a name="provision-the-content-share"></a>De inhoudsshare inrichten

Inhoudsshare van de bevat tenant-website-inhoud. De procedure voor het inrichten van de inhoud delen op een server met één bestand is hetzelfde voor Active Directory-en werkgroepomgevingen. Maar het is verschillend voor een failovercluster in Active Directory.

#### <a name="provision-the-content-share-on-a-single-file-server-active-directory-or-workgroup"></a>De inhoud delen op een server met één bestand (Active Directory of werkgroep) inrichten

Voer de volgende opdrachten bij een opdrachtprompt met verhoogde bevoegdheid op een server met één bestand. Vervang de waarde voor `C:\WebSites` met de bijbehorende paden in uw omgeving.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="configure-access-control-to-the-shares"></a>Beheer van toegang tot de shares configureren

Voer de volgende opdrachten bij een opdrachtprompt met verhoogde bevoegdheid op de bestandsserver of op het failoverclusterknooppunt, de huidige eigenaar van de cluster-resource is. Waarden in cursief vervangen door waarden die specifiek voor uw omgeving zijn.

#### <a name="active-directory"></a>Active Directory

```DOS
set DOMAIN=<DOMAIN>
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant %DOMAIN%\FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

#### <a name="workgroup"></a>Werkgroep

```DOS
set WEBSITES_FOLDER=C:\WebSites
icacls %WEBSITES_FOLDER% /reset
icacls %WEBSITES_FOLDER% /grant Administrators:(OI)(CI)(F)
icacls %WEBSITES_FOLDER% /grant FileShareOwners:(OI)(CI)(M)
icacls %WEBSITES_FOLDER% /inheritance:r
icacls %WEBSITES_FOLDER% /grant FileShareUsers:(CI)(S,X,RA)
icacls %WEBSITES_FOLDER% /grant *S-1-1-0:(OI)(CI)(IO)(RA,REA,RD)
```

## <a name="prepare-the-sql-server-instance"></a>De SQL Server-exemplaar voorbereiden

Voor de Azure App Service op Azure Stack die als host fungeert en softwarelicentiecontrole databases, moet u een exemplaar van SQL Server voor het opslaan van de App Service-databases.

Voor implementaties van Azure Stack Development Kit, kunt u SQL Server Express 2014 SP2 of hoger.

Voor productie en hoge beschikbaarheid, u moet een volledige versie van SQL Server 2014 SP2 of hoger, verificatie in gemengde modus inschakelen en implementeren een [maximaal beschikbare configuratie](https://docs.microsoft.com/sql/sql-server/failover-clusters/high-availability-solutions-sql-server).

De SQL Server-exemplaar voor Azure App Service in Azure Stack moet toegankelijk zijn vanuit alle App Service-rollen. U kunt SQL Server binnen het abonnement van de Provider standaard in Azure Stack implementeren. U kunt ook het gebruik van de bestaande infrastructuur binnen uw organisatie (zolang er een verbinding met Azure Stack is). Als u een Azure Marketplace-installatiekopie, moet u de firewall dienovereenkomstig configureren.

> [!NOTE]
> Een aantal SQL IaaS-installatiekopieën voor virtuele machines zijn beschikbaar via de Marketplace-Management-functie. Zorg ervoor dat u altijd de nieuwste versie van de SQL IaaS-extensie downloaden voordat u een virtuele machine met behulp van een Marketplace-item implementeert. De SQL-installatiekopieën zijn hetzelfde als de SQL-VM's die beschikbaar in Azure zijn. Voor SQL-VM's gemaakt op basis van deze installatiekopieën, de IaaS-extensie en de bijbehorende portal verbeteringen bieden functies zoals automatische toepassing van patches en back-mogelijkheden.
> 
> Voor een van de SQL Server-rollen, kunt u een standaardexemplaar of een benoemd exemplaar. Als u een benoemd exemplaar gebruikt, moet u handmatig de SQL Server Browser-service starten en-poort 1434 openen.

Het App Service-installatieprogramma wordt gecontroleerd of dat de SQL Server database containment ingeschakeld heeft. Als u database containment op de SQL-Server die als host de App Service-databases fungeert, voert u deze SQL-opdrachten:

```sql
sp_configure 'contained database authentication', 1;
GO
RECONFIGURE;
GO
```

>[!IMPORTANT]
> Als u ervoor kiest om App Service in een bestaand Virtueelnetwerk, de SQL-Server moet worden geïmplementeerd in een apart Subnet van App Service en de bestandsserver te implementeren.
>

## <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

Configureren van een service-principal voor Azure AD, ter ondersteuning van de volgende bewerkingen:

- Virtuele-machineschaalset integratie op werkrolniveaus.
- Eenmalige aanmelding voor de hulpprogramma's voor Azure Functions-portal en geavanceerde ontwikkelaars.

Deze stappen gelden voor alleen in omgevingen met Azure AD-beveiligde Azure Stack.

Beheerders moeten eenmalige aanmelding te configureren:

- De hulpprogramma's voor ontwikkelaars in App Service (Kudu) inschakelen.
- Het gebruik van de Azure Functions-portal-ervaring inschakelen.

Volg deze stappen:

1. Open een PowerShell-sessie als azurestack\AzureStackAdmin.
2. Ga naar de locatie van de scripts die u hebt gedownload en uitgepakt de [vereiste stap](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started).
3. [Installeren van PowerShell voor Azure Stack](azure-stack-powershell-install.md).
4. Voer de **maken AADIdentityApp.ps1** script. Wanneer u wordt gevraagd, typt u de Azure AD-tenant-ID die u voor uw Azure Stack-implementatie. Voer bijvoorbeeld **myazurestack.onmicrosoft.com**.
5. In de **referentie** venster, Voer uw Azure AD-service admin-account en wachtwoord. Selecteer **OK**.
6. Voer het pad naar het certificaatbestand en het certificaatwachtwoord voor de [certificaat dat u eerder hebt gemaakt](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started). Het certificaat dat is gemaakt voor deze stap standaard is **sso.appservice.local.azurestack.external.pfx**.
7. Het script maakt een nieuwe toepassing in de tenant Azure AD-instantie. Noteer de toepassings-ID die wordt geretourneerd in de PowerShell-uitvoer. U hebt deze informatie tijdens de installatie nodig.
8. Open een nieuw browservenster en meld u aan de [Azure-portal](https://portal.azure.com) als de Azure Active Directory-service-beheerder.
9. Open de Azure AD-resourceprovider.
10. Selecteer **App-registraties**.
11. Zoek de toepassings-ID geretourneerd als onderdeel van stap 7. Een App Service-toepassing wordt vermeld.
12. Selecteer **toepassing** in de lijst.
13. Selecteer **instellingen**.
14. Selecteer **vereiste machtigingen** > **machtigingen verlenen** > **Ja**.

```powershell
    Create-AADIdentityApp.ps1
```

| Parameter | Vereist of optioneel | Standaardwaarde | Description |
| --- | --- | --- | --- |
| DirectoryTenantName | Vereist | Null | Azure AD-tenant-ID. Geef de GUID of een tekenreeks. Een voorbeeld is myazureaaddirectory.onmicrosoft.com. |
| AdminArmEndpoint | Vereist | Null | Eindpunt van Admin Azure Resource Manager. Een voorbeeld is adminmanagement.local.azurestack.external. |
| TenantARMEndpoint | Vereist | Null | Eindpunt voor de tenant Azure Resource Manager. Een voorbeeld is management.local.azurestack.external. |
| AzureStackAdminCredential | Vereist | Null | Azure AD-service-beheerreferenties. |
| CertificateFilePath | Vereist | Null | **Volledig pad** naar het bestand voor het identiteitstoepassingscertificaat eerder gegenereerd. |
| CertificatePassword | Vereist | Null | Het wachtwoord dat helpt de persoonlijke sleutel van het certificaat te beschermen. |
| Omgeving | Optioneel | AzureCloud | De naam van de ondersteunde Cloud-omgeving waarin het doel-Azure Active Directory Graph-Service beschikbaar is.  Toegestane waarden: 'AzureCloud', 'AzureChinaCloud', 'AzureUSGovernment', 'AzureGermanCloud'.|

## <a name="create-an-active-directory-federation-services-application"></a>Een Active Directory Federation Services-toepassing maken

Voor Azure Stack-omgevingen die zijn beveiligd door AD FS, moet u een AD FS-service-principal ter ondersteuning van de volgende bewerkingen configureren:

- Virtuele-machineschaalset integratie op werkrolniveaus.
- Eenmalige aanmelding voor de hulpprogramma's voor Azure Functions-portal en geavanceerde ontwikkelaars.

Beheerders moeten eenmalige aanmelding te configureren:

- Een service-principal voor de virtuele machine scale set integratie op werkrolniveaus configureren.
- De hulpprogramma's voor ontwikkelaars in App Service (Kudu) inschakelen.
- Het gebruik van de Azure Functions-portal-ervaring inschakelen.

Volg deze stappen:

1. Open een PowerShell-sessie als azurestack\AzureStackAdmin.
2. Ga naar de locatie van de scripts die u hebt gedownload en uitgepakt de [vereiste stap](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started).
3. [Installeren van PowerShell voor Azure Stack](azure-stack-powershell-install.md).
4. Voer de **maken ADFSIdentityApp.ps1** script.
5. In de **referentie** venster, Geef uw AD FS cloud-beheerdersaccount en het wachtwoord. Selecteer **OK**.
6. Geef het pad naar het certificaatbestand en het certificaatwachtwoord voor de [certificaat dat u eerder hebt gemaakt](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started). Het certificaat dat is gemaakt voor deze stap standaard is **sso.appservice.local.azurestack.external.pfx**.

```powershell
    Create-ADFSIdentityApp.ps1
```

| Parameter | Vereist of optioneel | Standaardwaarde | Description |
| --- | --- | --- | --- |
| AdminArmEndpoint | Vereist | Null | Eindpunt van Admin Azure Resource Manager. Een voorbeeld is adminmanagement.local.azurestack.external. |
| PrivilegedEndpoint | Vereist | Null | Bevoegde eindpunt. Een voorbeeld is AzS-ERCS01. |
| CloudAdminCredential | Vereist | Null | Referenties van het domein account voor Azure Stack-cloud-beheerders. Een voorbeeld is Azurestack\CloudAdmin. |
| CertificateFilePath | Vereist | Null | **Volledig pad** naar de identiteitstoepassing certificaat-PFX-bestand. |
| CertificatePassword | Vereist | Null | Het wachtwoord dat helpt de persoonlijke sleutel van het certificaat te beschermen. |

## <a name="next-steps"></a>Volgende stappen

[Installeer de App Service-resourceprovider](azure-stack-app-service-deploy.md)

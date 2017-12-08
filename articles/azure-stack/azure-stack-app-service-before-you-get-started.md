---
title: Voordat u op Azure-Stack-App Service implementeert | Microsoft Docs
description: Stappen voor het voltooien voordat u op Azure-Stack-App Service implementeert
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: anwestg
ms.openlocfilehash: 17967131853d4334ae2c0ba3c0aa01089b7f3b61
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Voordat u aan de slag met App-Service op Azure-Stack

Op Azure-Stack-Azure App Service heeft een aantal vereiste stappen die moeten worden uitgevoerd voorafgaand aan de implementatie:

- Download de Azure App Service op Azure Stack Helper Scripts
- Hoge beschikbaarheid
- Vereiste certificaten voor Azure App Service op Azure-Stack
- -Bestandsserver voorbereiden
- SQL Server voorbereiden
- Een Azure Active Directory-toepassing maken
- Een Active Directory Federation Services-toepassing maken

## <a name="download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts"></a>Download de Azure App Service op Azure Stack installatieprogramma en Scripts Helper

1. Download de [App-Service op Azure-Stack helper implementatiescripts](https://aka.ms/appsvconmashelpers).
2. Download de [App-Service op Azure-Stack-installatieprogramma](https://aka.ms/appsvconmasinstaller).
3. Pak het zip-bestand van de helper-scripts uit. De volgende bestanden en de mapstructuur worden weergegeven:
  - Common.ps1
  - Maak AADIdentityApp.ps1
  - Maak ADFSIdentityApp.ps1
  - Maak AppServiceCerts.ps1
  - Get-AzureStackRootCert.ps1
  - Verwijder AppService.ps1
  - Modules
    - GraphAPI.psm1
    
## <a name="high-availability"></a>Hoge beschikbaarheid

Azure App Service op Azure-Stack is niet op dit moment kunnen bieden hoge beschikbaarheid omdat Azure Stack alleen werkbelastingen in één enkel domein met fouten worden geïmplementeerd.

Als u Azure App Service op Azure-Stack voorbereiden voor hoge beschikbaarheid, moet u de vereiste File Server en SQL Server implementeren in een maximaal beschikbare configuratie. Wanneer Azure Stack meerdere domeinen met fouten ondersteunt, bieden we hulp bij het inschakelen van Azure App Service op Azure-Stack in een maximaal beschikbare configuratie.


## <a name="certificates-required-for-azure-app-service-on-azure-stack"></a>Vereiste certificaten voor Azure App Service op Azure-Stack

### <a name="certificates-required-for-the-azure-stack-development-kit"></a>Vereiste certificaten voor de Azure-Stack Development Kit

Deze eerste script werkt met de Azure-Stack-certificeringsinstantie vier certificaten die worden door App Service vereist kunt maken.

| Bestandsnaam | Gebruiken |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | App Service standaard SSL-certificaat |
| API.appservice.local.azurestack.external.pfx | App Service API-SSL-certificaat |
| FTP.appservice.local.azurestack.external.pfx | App Service Publisher SSL-certificaat |
| SSO.appservice.local.azurestack.external.pfx | App Service-identiteitscertificaat toepassing |

Het script uitvoert op de host Azure Stack Development Kit en zorg ervoor dat u PowerShell als azurestack\CloudAdmin uitvoert.

1. In een PowerShell-sessie wordt uitgevoerd als azurestack\AzureStackAdmin voert u het script maken AppServiceCerts.ps1 vanuit de map waar u de scripts helper hebt uitgepakt. Het script maakt vier certificaten in dezelfde map als het script van maken certificaten die App Service nodig heeft.
2. Een wachtwoord invoeren voor het pfx-bestanden beveiligen en noteer deze. U moet deze invoeren in de App-Service op Azure-Stack-installatieprogramma.

#### <a name="create-appservicecertsps1-parameters"></a>Maak AppServiceCerts.ps1 parameters

| Parameter | Vereiste/optionele | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| PfxPassword | Vereist | Null | Wachtwoord voor het beveiligen van de persoonlijke sleutel van het certificaat |
| Domeinnaam | Vereist | Local.azurestack.external | Azure Stack-achtervoegsel voor de regio en het domein |

### <a name="certificates-required-for-a-production-deployment-of-azure-app-service-on-azure-stack"></a>Vereiste certificaten voor een productie-implementatie van Azure App Service op Azure-Stack

Als u wilt werken de resourceprovider in productie, moet u de volgende vier certificaten opgeven:

#### <a name="default-domain-certificate"></a>Certificaat van het standaarddomein

Het certificaat van het standaarddomein wordt geplaatst op de Front-End-rol. Deze wordt gebruikt door toepassingen voor jokerteken of standaard domein aanvragen in Azure App Service. Het certificaat wordt ook gebruikt voor beheerbewerkingen bron (KUDU).

Het certificaat in PFX-indeling moet zijn en moet een jokertekencertificaat met twee-onderwerp. Hierdoor kunnen het standaarddomein en het eindpunt scm voor bewerkingen van de bron-beheer om te worden gedekt door een certificaat.

| Indeling | Voorbeeld |
| --- | --- |
| \*.appservice. \<regio\>.\< DomainName\>.\< de extensie\> | \*. appservice.redmond.azurestack.external |
| \*. scm.appservice. <region>. <DomainName>.<extension> | \*. appservice.scm.redmond.azurestack.external |

#### <a name="api-certificate"></a>API-certificaat

De API-certificaat op de rol wordt geplaatst en wordt gebruikt door de resourceprovider voor het beveiligen van api-aanroepen. Het certificaat voor de publicatie moet een onderwerpnaam die overeenkomt met de API-DNS-vermelding bevatten:

| Indeling | Voorbeeld |
| --- | --- |
| API.appservice. \<regio\>.\< DomainName\>.\< de extensie\> | API.appservice.Redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Publicatiecertificaat

Het certificaat voor de rol Publisher beveiligt het verkeer FTPS voor eigenaren van toepassing wanneer ze inhoud uploaden.  Het certificaat voor publicatie moet een onderwerpnaam die overeenkomt met de FTPS DNS-vermelding bevatten.

| Indeling | Voorbeeld |
| --- | --- |
| FTP.appservice. \<regio\>.\< DomainName\>.\< de extensie\> | API.appservice.Redmond.azurestack.external |

#### <a name="identity-certificate"></a>Identiteitscertificaat

Maakt gebruik van het certificaat voor de identiteit van toepassing:
- integratie tussen de AAD/ADFS-directory, Azure-Stack en ondersteuning voor integratie met de Resourceprovider voor Compute-App Service
- Eenmalige aanmelding scenario's voor geavanceerde ontwikkelhulpprogramma's binnen op Azure Stack-Azure App Service.
Het certificaat voor identiteit moet een onderwerpnaam die overeenkomt met de volgende indeling bevatten:

| Indeling | Voorbeeld |
| --- | --- |
| SSO.appservice. \<regio\>.\< DomainName\>.\< de extensie\> | SSO.appservice.Redmond.azurestack.external |

### <a name="extract-the-azure-stack-azure-resource-manager-root-certificate"></a>Pak het basiscertificaat Azure Stack Azure Resource Manager

In een PowerShell-sessie wordt uitgevoerd als azurestack\CloudAdmin voert u het script Get-AzureStackRootCert.ps1 vanuit de map waar u de scripts helper hebt uitgepakt. Het script maakt vier certificaten in dezelfde map als het script van maken certificaten die App Service nodig heeft.

| Get-AzureStackRootCert.ps1 parameter | Vereiste/optionele | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| PrivelegedEndpoint | Vereist | AzS ERCS01 | Bevoorrechte eindpunt. |
| CloudAdminCredential | Vereist | AzureStack\CloudAdmin | Azure-Stack Cloud Admin-account domeinreferentie |


## <a name="prepare-the-file-server"></a>De bestandsserver voorbereiden

Azure App Service vereist het gebruik van een bestandsserver. Voor productie-implementaties moet de bestandsserver geconfigureerd dat deze maximaal beschikbaar en kan afhandelen van fouten.

Voor gebruik met alleen Azure Stack Development Kit-implementaties, kunt u dit voorbeeld Azure Resource Manager-implementatiesjabloon om een bestandsserver geconfigureerde één knooppunt te implementeren: https://aka.ms/appsvconmasdkfstemplate. De bestandsserver van één knooppunt bevindt zich in een werkgroep.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Inrichten van groepen en accounts in Active Directory


1. Maak de volgende globale beveiligingsgroepen van Active Directory:
    - FileShareOwners
    - FileShareUsers
2. De volgende Active Directory-accounts als serviceaccounts maken:
    - FileShareOwner
    - FileShareUser

    Beveiliging moet aanbevolen procedure is om de gebruikers voor deze accounts (en voor alle webrollen) worden van elkaar verschillen en sterke gebruikersnamen en wachtwoorden hebben.
    Stel de wachtwoorden met de volgende voorwaarden:
     - Schakel **wachtwoord verloopt nooit**.
     - Schakel **gebruiker kan wachtwoord niet wijzigen**.
     - Schakel **gebruiker moet wachtwoord bij volgende aanmelding wijzigen**.
3. De accounts aan de groepslidmaatschappen als volgt toevoegen:
    - Voeg **FileShareOwner** naar de **FileShareOwners** groep.
    - Voeg **FileShareUser** naar de **FileShareUsers** groep.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Inrichten van groepen en accounts in een werkgroep

>[!NOTE]
> De volgende opdrachten uitvoeren bij het configureren van de bestandsserver in een opdrachtprompt als Administrator-sessie.  **Gebruik geen PowerShell.**

Wanneer u de bovenstaande Azure Resource Manager-sjabloon gebruikt, worden de gebruikers zijn al gemaakt.

1. Voer de volgende opdrachten voor het maken van de accounts FileShareOwner en FileShareUser. Vervang <password> met uw eigen waarden.
``` DOS
net user FileShareOwner <password> /add /expires:never /passwordchg:no
net user FileShareUser <password> /add /expires:never /passwordchg:no
```
2. Stel de wachtwoorden voor de accounts nooit verloopt met de volgende WMIC-opdrachten:
``` DOS
WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
```
3. Maken van lokale groepen FileShareUsers en FileShareOwners en de accounts in de eerste stap aan toevoegen.
``` DOS
net localgroup FileShareUsers /add
net localgroup FileShareUsers FileShareUser /add
net localgroup FileShareOwners /add
net localgroup FileShareOwners FileShareOwner /add
```

### <a name="provision-the-content-share"></a>De inhoudsshare inrichten

De inhoudsshare van bevat de inhoud van de tenant-website. De procedure voor het inrichten van de inhoud-share op een server met één bestand is hetzelfde voor Active Directory en de werkgroep omgevingen, maar verschillende voor een failovercluster in Active Directory.

#### <a name="provision-the-content-share-on-a-single-file-server-ad-or-workgroup"></a>Inrichten van de inhoud-share op een server met één bestand (AD of werkgroep)

Voer de volgende opdrachten bij een opdrachtprompt met verhoogde bevoegdheden op een server met één bestand. Vervang de waarde voor 'C:\WebSites' door de bijbehorende paden in uw omgeving.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="to-enable-winrm-add-the-fileshareowners-group-to-the-local-administrators-group"></a>Om in te schakelen WinRM, kunt u de FileShareOwners-groep toevoegen aan de lokale groep Administrators

In de volgorde voor Windows Remote Management goed te laten werken, moet u de groep FileShareOwners toevoegen aan de lokale groep Administrators.

#### <a name="active-directory"></a>Active Directory

Voer de volgende opdrachten bij een opdrachtprompt met verhoogde bevoegdheid op de bestandsserver of op elk knooppunt van het failovercluster van bestandsserver. Vervang de waarde voor <DOMAIN> met de domeinnaam die u wilt gebruiken.

```DOS
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Werkgroep

Voer de volgende opdracht bij een opdrachtprompt met verhoogde bevoegdheid op de bestandsserver.

```DOS
net localgroup Administrators FileShareOwners /add
```

### <a name="configure-access-control-to-the-shares"></a>Beheer van toegang tot de shares configureren

Voer de volgende opdrachten bij een opdrachtprompt met verhoogde bevoegdheid op de bestandsserver of op het failovercluster van bestandsserver-knooppunt, wordt de huidige eigenaar van de cluster-bron. Waarden cursief vervangen door waarden die specifiek zijn voor uw omgeving.

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

## <a name="prepare-the-sql-server"></a>De SQL-Server voorbereiden

Voor de Azure App Service voor het hosten van de Azure-Stack en softwarelicentiecontrole databases, moet u een SQL-Server voor het opslaan van de Azure App Service-Databases voorbereiden.

Voor gebruik met de Azure-Stack Development Kit, kunt u SQL Express 2014 SP2 of hoger.

Voor productie en maximale beschikbaarheid dient u moet een volledige versie van SQL 2014 SP2 of hoger, verificatie in gemengde modus inschakelen en implementeren in een [maximaal beschikbare configuratie](https://docs.microsoft.com/en-us/sql/sql-server/failover-clusters/high-availability-solutions-sql-server).

De Azure App Service op Azure Stack SQL Server moet toegankelijk is vanaf alle functies van de App Service. SQL Server kan worden geïmplementeerd in de Provider standaard abonnement in Azure-Stack. Of u kunt het gebruik van de bestaande infrastructuur binnen uw organisatie (zo lang er is verbinding met Azure-Stack). Als u van een Azure Marketplace-installatiekopie gebruikmaakt, moet de firewall dienovereenkomstig configureren. 

Voor een van de SQL Server-rollen, kunt u een standaardexemplaar of een benoemd exemplaar. Als u een benoemd exemplaar gebruikt, zijn echter ervoor dat u handmatig de SQL Browser-Service starten en -poort 1434 openen.

## <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

Een Azure AD-service-principal ter ondersteuning van de volgende configureren:
- Virtuele-machineschaalset ingesteld integratie op Worker lagen.
- Eenmalige aanmelding voor de Azure Functions-portal en geavanceerde developer-tools.

Deze stappen van toepassing op Azure AD, Azure Stack omgevingen alleen beveiligd.

Beheerders moeten eenmalige aanmelding te configureren:
- Schakel de geavanceerde ontwikkelhulpprogramma's in App Service (Kudu).
- Het gebruik van de Azure Functions-portal-ervaring inschakelen.

Volg deze stappen:

1. Open een PowerShell-exemplaar als azurestack\AzureStackAdmin.
2. Ga naar de locatie van de scripts gedownload en uitgepakt de [vereiste stap](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Stack van Azure PowerShell installeren](azure-stack-powershell-install.md).
4. Voer de **maken AADIdentityApp.ps1** script. Wanneer u wordt gevraagd om uw Azure AD-tenant-ID, voert u de Azure AD tenant-ID die u voor uw Azure-Stack-implementatie, bijvoorbeeld myazurestack.onmicrosoft.com.
5. In de **referentie** venster, Voer uw Azure AD-service admin-account en wachtwoord. Klik op **OK**.
6. Geef het bestandspad van de certificaat en het wachtwoord van het certificaat voor de [certificaat eerder gemaakte](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Het certificaat voor deze stap standaard is gemaakt, **sso.appservice.local.azurestack.external.pfx**.
7. Het script maakt een nieuwe toepassing in de Azure AD-tenant. Noteer de toepassings-ID die wordt geretourneerd in de PowerShell-uitvoer. U moet deze informatie tijdens de installatie.
8. Open een nieuw browservenster en meld u aan bij de Azure portal (portal.azure.com) als de **Azure Active Directory-servicebeheerder**.
9. Open de Azure AD-resourceprovider.
10. Klik op **App registraties**.
11. Zoeken naar de **toepassings-ID** geretourneerd als onderdeel van stap 7. Een App Service-toepassing wordt vermeld.
12. Klik op **toepassing** in de lijst
13. Klik op **vereist machtigingen** > **machtigingen verlenen** > **Ja**.

| Maak AADIdentityApp.ps1 parameter | Vereiste/optionele | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| DirectoryTenantName | Vereist | Null | Azure AD tenant-ID. Geef de myazureaaddirectory.onmicrosoft.com GUID of een tekenreeks, bijvoorbeeld: |
| AdminArmEndpoint | Vereist | Null | Het eindpunt van Admin Azure Resource Manager, bijvoorbeeld adminmanagement.local.azurestack.external |
| TenantARMEndpoint | Vereist | Null | Tenant van Azure Resource Manager-eindpunt, bijvoorbeeld: management.local.azurestack.external |
| AzureStackAdminCredential | Vereist | Null | Azure AD Service Admin Credential |
| CertificateFilePath | Vereist | Null | Pad naar het certificaatbestand van de identiteit toepassing eerder gegenereerd. |
| CertificatePassword | Vereist | Null | Wachtwoord voor het beveiligen van de persoonlijke sleutel van het certificaat. |

## <a name="create-an-active-directory-federation-services-application"></a>Een Active Directory Federation Services-toepassing maken

Azure-Stack-omgevingen is beveiligd met AD FS, moet u een AD FS-service-principal ter ondersteuning van de volgende configureren:
- Virtuele-machineschaalset ingesteld integratie op Worker lagen.
- Eenmalige aanmelding voor de Azure Functions-portal en geavanceerde developer-tools.

Beheerders moeten eenmalige aanmelding te configureren:
- Een service-principal voor de virtuele machine scale set integratie op Worker lagen configureren.
- Schakel de geavanceerde ontwikkelhulpprogramma's in App Service (Kudu).
- Het gebruik van de Azure Functions-portal-ervaring inschakelen.

Volg deze stappen:

1. Open een PowerShell-exemplaar als azurestack\azurestackadmin.
2. Ga naar de locatie van de scripts gedownload en uitgepakt de [vereiste stap](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Stack van Azure PowerShell installeren](azure-stack-powershell-install.md).
4.  Voer de **maken ADFSIdentityApp.ps1** script.
5.  In de **referentie** venster uw AD FS cloud admin-account en wachtwoord invoeren. Klik op **OK**.
6.  Geef de bestandspad certificaat en het wachtwoord van het certificaat voor de [certificaat eerder gemaakte](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Het certificaat dat standaard wordt gemaakt voor deze stap is sso.appservice.local.azurestack.external.pfx.

| Maak ADFSIdentityApp.ps1 parameter | Vereiste/optionele | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| AdminArmEndpoint | Vereist | Null | Het eindpunt van admin Azure Resource Manager. Bijvoorbeeld: adminmanagement.local.azurestack.external. |
| PrivilegedEndpoint | Vereist | Null | Bevoorrechte eindpunt. Bijvoorbeeld: AzS ERCS01. |
| CloudAdminCredential | Vereist | Null | Azure Stack cloudadmin domein accountreferentie. Bijvoorbeeld: Azurestack\CloudAdmin. |
| CertificateFilePath | Vereist | Null | Pad naar de identiteit van de toepassing PFX-certificaatbestand. |
| CertificatePassword | Vereist | Null | Wachtwoord voor het beveiligen van de persoonlijke sleutel van het certificaat. |


## <a name="next-steps"></a>Volgende stappen

[Installeer de App Service-resourceprovider](azure-stack-app-service-deploy.md).

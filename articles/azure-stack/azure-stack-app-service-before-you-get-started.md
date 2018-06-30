---
title: Voordat u op Azure-Stack-App Service implementeert | Microsoft Docs
description: Stappen voor het voltooien voordat u op Azure-Stack-App Service implementeert
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2018
ms.author: anwestg
ms.openlocfilehash: 37d6ee2f047768f08ea7a113b7d97911d58a46e2
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110572"
---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Voordat u aan de slag met App-Service op Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Voordat u Azure App Service op de Stack Azure implementeert, moet u de vereiste stappen in dit artikel.

> [!IMPORTANT]
> De update 1804 toepassen op uw systeem Azure Stack geïntegreerd of implementeren van de meest recente Azure Stack Development Kit (ASDK) voordat u Azure App Service 1.2 implementeren.

## <a name="download-the-installer-and-helper-scripts"></a>Het installatieprogramma en helper-scripts downloaden

1. Download de [App-Service op Azure-Stack helper implementatiescripts](https://aka.ms/appsvconmashelpers).
2. Download de [App-Service op Azure-Stack-installatieprogramma](https://aka.ms/appsvconmasinstaller).
3. Pak de bestanden van het ZIP-bestand van de helper-scripts. De volgende bestanden en mappen zijn uitgepakt:

   - Common.ps1
   - Create-AADIdentityApp.ps1
   - Create-ADFSIdentityApp.ps1
   - Create-AppServiceCerts.ps1
   - Get-AzureStackRootCert.ps1
   - Remove-AppService.ps1
   - Map modules
     - GraphAPI.psm1

## <a name="high-availability"></a>Hoge beschikbaarheid

De update Azure Stack 1802 is ondersteuning toegevoegd voor domeinen met fouten. Nieuwe implementaties van Azure App Service op Azure-Stack worden verdeeld over de domeinen met fouten en fouttolerantie.

Zie voor bestaande implementaties van Azure App Service op Azure-Stack die zijn geïmplementeerd voor de update 1802, de [opnieuw verdelen van een App Service-resourceprovider tussen domeinen met fouten](azure-stack-app-service-fault-domain-update.md) artikel.

Bovendien implementeren de vereiste bestandsserver en de SQL Server-exemplaren in een maximaal beschikbare configuratie.

## <a name="get-certificates"></a>Certificaten ophalen

### <a name="azure-resource-manager-root-certificate-for-azure-stack"></a>Azure Resource Manager-basiscertificaat voor Azure-Stack

Open een PowerShell-sessie met verhoogde bevoegdheden op een computer die toegang heeft tot de bevoegde eindpunt op de Azure-Stack geïntegreerd systeem of Azure Stack Development Kit Host.

Voer de *Get-AzureStackRootCert.ps1* script uit de map waar u de scripts helper hebt uitgepakt. Het script maakt een basiscertificaat in dezelfde map als het script dat een App Service nodig heeft voor het maken van certificaten.

Wanneer u de volgende PowerShell-opdracht uitvoert hebt u het bevoegde eindpunt en de referenties voor de AzureStack\CloudAdmin opgeven.

```PowerShell
    Get-AzureStackRootCert.ps1
```

#### <a name="get-azurestackrootcertps1-script-parameters"></a>Get-AzureStackRootCert.ps1 scriptparameters

| Parameter | Vereist of optioneel | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| PrivilegedEndpoint | Vereist | AzS-ERCS01 | Bevoorrechte eindpunt |
| CloudAdminCredential | Vereist | AzureStack\CloudAdmin | Account domeinreferentie voor beheerders voor Azure-Stack-cloud |

### <a name="certificates-required-for-asdk-deployment-of-azure-app-service"></a>Certificaten zijn vereist voor de implementatie van ASDK van Azure App Service

De *maken AppServiceCerts.ps1* script werkt met de Azure-Stack certificeringsinstantie voor het maken van de vier certificaten die door App Service nodig heeft.

| Bestandsnaam | Gebruiken |
| --- | --- |
| _.appservice.local.azurestack.external.pfx | Het standaard-SSL-certificaatbestand voor App Service |
| api.appservice.local.azurestack.external.pfx | App Service API SSL-certificaat |
| ftp.appservice.local.azurestack.external.pfx | App Service publisher SSL-certificaat |
| sso.appservice.local.azurestack.external.pfx | App Service-identiteitscertificaat toepassing |

Volg deze stappen voor het maken van de certificaten:

1. Aanmelden bij de Azure-Stack Development Kit host met de AzureStack\AzureStackAdmin-account.
2. Open een PowerShell-sessie met verhoogde bevoegdheden.
3. Voer de *maken AppServiceCerts.ps1* script uit de map waar u de scripts helper hebt uitgepakt. Dit script maakt vier certificaten in dezelfde map als het script dat een App Service nodig heeft voor het maken van certificaten.
4. Een wachtwoord invoeren voor het pfx-bestanden beveiligen en noteer deze. U hebt deze invoeren in de App-Service op Azure-Stack-installatieprogramma.

#### <a name="create-appservicecertsps1-script-parameters"></a>Maak AppServiceCerts.ps1 scriptparameters

| Parameter | Vereist of optioneel | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| pfxPassword | Vereist | Null | Wachtwoord beschermt de privésleutel voor certificaten |
| Domeinnaam | Vereist | Local.azurestack.external | Azure Stack-achtervoegsel voor de regio en het domein |

### <a name="certificates-required-for-azure-stack-production-deployment-of-azure-app-service"></a>Vereiste certificaten voor Azure-Stack productie-implementatie van Azure App Service

Als u wilt uitvoeren van de resourceprovider in productie, moet u de volgende certificaten opgeven:

- Certificaat van het standaarddomein
- API-certificaat
- Publicatiecertificaat
- Identiteitscertificaat

#### <a name="default-domain-certificate"></a>Certificaat van het standaarddomein

Het certificaat van het standaarddomein wordt geplaatst op de Front-End-rol. Gebruikerstoepassingen voor jokerteken of standaard domein-aanvraag in Azure App Service gebruikmaken van dit certificaat. Het certificaat wordt ook gebruikt voor beheerbewerkingen bron (Kudu).

Het certificaat in PFX-indeling moet zijn en moet een jokertekencertificaat met drie-onderwerp. Deze vereiste kan een certificaat hebben betrekking op het standaarddomein en het SCM-eindpunt voor de bron-en beheerbewerkingen.

| Indeling | Voorbeeld |
| --- | --- |
| \*.appservice. \<regio\>.\< DomainName\>.\< de extensie\> | \*.appservice.redmond.azurestack.external |
| \*.scm.appservice.<region>.<DomainName>.<extension> | \*.scm.appservice.redmond.azurestack.external |
| \*.sso.appservice.<region>.<DomainName>.<extension> | \*.sso.appservice.redmond.azurestack.external |

#### <a name="api-certificate"></a>API-certificaat

De API-certificaat is op de rol geplaatst. De resourceprovider gebruikt om u te helpen beveiligen API-aanroepen. Het certificaat voor de publicatie moet een onderwerpnaam die overeenkomt met de API-DNS-vermelding bevatten.

| Indeling | Voorbeeld |
| --- | --- |
| API.appservice. \<regio\>.\< DomainName\>.\< de extensie\> | api.appservice.redmond.azurestack.external |

#### <a name="publishing-certificate"></a>Publicatiecertificaat

Het certificaat voor de rol Publisher beveiligt het verkeer FTPS voor eigenaren van toepassing wanneer ze inhoud uploaden. Het certificaat voor de publicatie moet een onderwerpnaam die overeenkomt met de FTPS DNS-vermelding bevatten.

| Indeling | Voorbeeld |
| --- | --- |
| FTP.appservice. \<regio\>.\< DomainName\>.\< de extensie\> | ftp.appservice.redmond.azurestack.external |

#### <a name="identity-certificate"></a>Identiteitscertificaat

Maakt gebruik van het certificaat voor de identiteit van toepassing:

- Integratie tussen de Azure Active Directory (Azure AD) of Active Directory Federation Services (AD FS) directory, Azure-Stack en App Service voor de integratie met de compute-resourceprovider.
- Eenmalige aanmelding scenario's voor geavanceerde ontwikkelhulpprogramma's in Azure App Service op Azure-Stack.

Het certificaat voor identiteit moet een onderwerpnaam die overeenkomt met de volgende indeling bevatten.

| Indeling | Voorbeeld |
| --- | --- |
| SSO.appservice. \<regio\>.\< DomainName\>.\< de extensie\> | sso.appservice.redmond.azurestack.external |

## <a name="virtual-network"></a>Virtueel netwerk

Op Azure-Stack-Azure App Service kunt u de resourceprovider implementeren op een bestaand virtueel netwerk of kunt u een virtueel netwerk maken als onderdeel van de implementatie. Met behulp van een bestaand virtueel netwerk maakt het gebruik van interne IP-adressen verbinding maken met de bestandsserver en de SQL server in Azure App Service op Azure-Stack is vereist. Het virtuele netwerk moet worden geconfigureerd met de volgende adresbereik en subnetten voor de installatie van Azure App Service op Azure-Stack:

Virtueel netwerk - /16

Subnetten

- ControllersSubnet /24
- ManagementServersSubnet /24
- FrontEndsSubnet /24
- PublishersSubnet /24
- WorkersSubnet /21

## <a name="prepare-the-file-server"></a>De bestandsserver voorbereiden

Azure App Service vereist het gebruik van een bestandsserver. De bestandsserver moet worden geconfigureerd om maximaal beschikbaar is en kan afhandelen van fouten voor productie-implementaties.

Voor Azure Stack Development Kit implementaties, kunt u de [voorbeeld Azure Resource Manager-implementatiesjabloon](https://aka.ms/appsvconmasdkfstemplate) voor het implementeren van een geconfigureerde bestandsserver van één knooppunt. De bestandsserver met één knooppunt bevindt zich in een werkgroep.

>[!IMPORTANT]
> Als u implementeren in een bestaand virtueel netwerk dat de bestandsserver moeten worden geïmplementeerd in een apart Subnet van App Service-App Service wilt.

### <a name="provision-groups-and-accounts-in-active-directory"></a>Inrichten van groepen en accounts in Active Directory

1. Maak de volgende globale beveiligingsgroepen van Active Directory:

   - FileShareOwners
   - FileShareUsers

2. De volgende Active Directory-accounts als serviceaccounts maken:

   - FileShareOwner
   - FileShareUser

   Beveiliging moet aanbevolen procedure is om de gebruikers voor deze accounts (en voor alle webrollen) uniek en sterke gebruikersnamen en wachtwoorden hebben. Stel de wachtwoorden met de volgende voorwaarden:

   - Schakel **wachtwoord verloopt nooit**.
   - Schakel **gebruiker kan wachtwoord niet wijzigen**.
   - Schakel **gebruiker moet wachtwoord bij volgende aanmelding wijzigen**.

3. De accounts aan de groepslidmaatschappen als volgt toevoegen:

   - Voeg **FileShareOwner** naar de **FileShareOwners** groep.
   - Voeg **FileShareUser** naar de **FileShareUsers** groep.

### <a name="provision-groups-and-accounts-in-a-workgroup"></a>Inrichten van groepen en accounts in een werkgroep

>[!NOTE]
> Wanneer u een bestandsserver, voer de volgende opdrachten uit configureert een **Beheerdersopdrachtprompt**. <br>***Gebruik geen PowerShell.***

Wanneer u de Azure Resource Manager-sjabloon gebruikt, worden de gebruikers zijn al gemaakt.

1. Voer de volgende opdrachten voor het maken van de accounts FileShareOwner en FileShareUser. Vervang `<password>` met uw eigen waarden.

    ``` DOS
    net user FileShareOwner <password> /add /expires:never /passwordchg:no
    net user FileShareUser <password> /add /expires:never /passwordchg:no
    ```
2. Stel de wachtwoorden voor de accounts nooit verloopt met de volgende WMIC-opdrachten:

    ``` DOS
    WMIC USERACCOUNT WHERE "Name='FileShareOwner'" SET PasswordExpires=FALSE
    WMIC USERACCOUNT WHERE "Name='FileShareUser'" SET PasswordExpires=FALSE
    ```
3. Maken van de lokale groepen FileShareUsers en FileShareOwners en aan de accounts in de eerste stap toevoegen:

    ``` DOS
    net localgroup FileShareUsers /add
    net localgroup FileShareUsers FileShareUser /add
    net localgroup FileShareOwners /add
    net localgroup FileShareOwners FileShareOwner /add
    ```

### <a name="provision-the-content-share"></a>De inhoudsshare inrichten

De inhoudsshare van de bevat tenant-website-inhoud. De procedure voor het inrichten van de inhoud-share op een server met één bestand is hetzelfde voor Active Directory-en werkgroepomgevingen. Maar het is verschillend voor een failovercluster in Active Directory.

#### <a name="provision-the-content-share-on-a-single-file-server-active-directory-or-workgroup"></a>Inrichten van de inhoud-share op een server met één bestand (Active Directory of werkgroep)

Voer de volgende opdrachten bij een opdrachtprompt met verhoogde bevoegdheden op een server met één bestand. Vervang de waarde voor `C:\WebSites` met de bijbehorende paden in uw omgeving.

```DOS
set WEBSITES_SHARE=WebSites
set WEBSITES_FOLDER=C:\WebSites
md %WEBSITES_FOLDER%
net share %WEBSITES_SHARE% /delete
net share %WEBSITES_SHARE%=%WEBSITES_FOLDER% /grant:Everyone,full
```

### <a name="add-the-fileshareowners-group-to-the-local-administrators-group"></a>Voeg de FileShareOwners toe aan de lokale groep Administrators

Voor Windows Remote Management goed te laten werken, moet u de groep FileShareOwners toevoegen aan de lokale groep Administrators.

#### <a name="active-directory"></a>Active Directory

Voer de volgende opdrachten bij een opdrachtprompt met verhoogde bevoegdheid op de bestandsserver of op elke bestandsserver die als een failoverclusterknooppunt fungeert. Vervang de waarde voor `<DOMAIN>` met de domeinnaam die u wilt gebruiken.

```DOS
set DOMAIN=<DOMAIN>
net localgroup Administrators %DOMAIN%\FileShareOwners /add
```

#### <a name="workgroup"></a>Werkgroep

Voer de volgende opdracht bij een opdrachtprompt met verhoogde bevoegdheid op de bestandsserver:

```DOS
net localgroup Administrators FileShareOwners /add
```

### <a name="configure-access-control-to-the-shares"></a>Beheer van toegang tot de shares configureren

Voer de volgende opdrachten bij een opdrachtprompt met verhoogde bevoegdheid op de bestandsserver of op het failoverclusterknooppunt, de huidige eigenaar van de cluster-bron. Waarden cursief vervangen door waarden die specifiek voor uw omgeving zijn.

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

Voor de Azure App Service voor het hosten van de Azure-Stack en softwarelicentiecontrole databases, moet u een SQL Server-exemplaar voor het opslaan van de App Service-databases voorbereiden.

Voor implementaties van Azure Stack Development Kit, kunt u SQL Server Express 2014 SP2 of hoger.

Voor productie en hoge beschikbaarheid doeleinden, u moet een volledige versie van SQL Server 2014 SP2 of hoger, gemengde modus-verificatie inschakelen en implementeren in een [maximaal beschikbare configuratie](https://docs.microsoft.com/sql/sql-server/failover-clusters/high-availability-solutions-sql-server).

De SQL Server-exemplaar voor Azure App Service op Azure-Stack moet toegankelijk is vanaf alle functies van de App Service. U kunt SQL Server in het abonnement van de Provider standaard in Azure-Stack implementeren. Of u kunt het gebruik van de bestaande infrastructuur binnen uw organisatie (zo lang er is verbinding met Azure-Stack). Als u een Azure Marketplace-installatiekopie, moet u de firewall dienovereenkomstig configureren.

>[!NOTE]
> Een aantal SQL IaaS virtuele machine afbeeldingen zijn beschikbaar via de Marketplace-Management-functie. Zorg ervoor dat u altijd de nieuwste versie van de SQL-IaaS-uitbreiding downloaden voordat u een VM die gebruikmaakt van een Marketplace-item implementeert. De SQL-afbeeldingen zijn hetzelfde als de SQL-VM's die beschikbaar in Azure zijn. Geef functies zoals automatisch patchen en Backup-functies voor virtuele SQL-machines gemaakt op basis van deze installatiekopieën, de IaaS-uitbreiding en de bijbehorende portal verbeteringen.
>
Voor een van de SQL Server-rollen, kunt u een standaardexemplaar of een benoemd exemplaar. Als u een benoemd exemplaar gebruikt, moet u handmatig de SQL Server Browser-service starten en open poort 1434.

>[!IMPORTANT]
> Als u wilt implementeren in een bestaand virtueel netwerk-App Service kan de SQL-Server moeten worden geïmplementeerd in een apart Subnet van App Service en de bestandsserver.
>

## <a name="create-an-azure-active-directory-application"></a>Een Azure Active Directory-toepassing maken

Configureer een Azure AD-service-principal ter ondersteuning van de volgende bewerkingen:

- Virtuele-machineschaalset ingesteld integratie op worker lagen.
- Eenmalige aanmelding voor de Azure Functions-portal en geavanceerde developer-tools.

Deze stappen gelden voor alleen in omgevingen met Azure AD-beveiligde Azure-Stack.

Beheerders moeten eenmalige aanmelding te configureren:

- Schakel de geavanceerde ontwikkelhulpprogramma's in App Service (Kudu).
- Het gebruik van de Azure Functions-portal-ervaring inschakelen.

Volg deze stappen:

1. Open een PowerShell-exemplaar als azurestack\AzureStackAdmin.
2. Ga naar de locatie van de scripts die u hebt gedownload en uitgepakt de [vereiste stap](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Installeer PowerShell voor Azure Stack](azure-stack-powershell-install.md).
4. Voer de **maken AADIdentityApp.ps1** script. Wanneer u wordt gevraagd, typt u de Azure AD-tenant-ID die u voor uw Azure-Stack-implementatie. Voer bijvoorbeeld **myazurestack.onmicrosoft.com**.
5. In de **referentie** venster, Voer uw Azure AD-service admin-account en wachtwoord. Selecteer **OK**.
6. Geef het bestandspad van de certificaat en het wachtwoord van het certificaat voor de [certificaat eerder gemaakte](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Het certificaat voor deze stap standaard is gemaakt, **sso.appservice.local.azurestack.external.pfx**.
7. Het script maakt een nieuwe toepassing in de Azure AD-exemplaar van tenant. Noteer de toepassings-ID die wordt geretourneerd in de PowerShell-uitvoer. U moet deze informatie tijdens de installatie.
8. Open een nieuw browservenster en meld u bij de [Azure-portal](https://portal.azure.com) als de service Azure Active Directory-beheerder.
9. Open de Azure AD-resourceprovider.
10. Selecteer **App registraties**.
11. Zoek de toepassings-ID geretourneerd als onderdeel van stap 7. Een App Service-toepassing wordt vermeld.
12. Selecteer **toepassing** in de lijst.
13. Selecteer **instellingen**.
14. Selecteer **vereist machtigingen** > **machtigingen verlenen** > **Ja**.

```PowerShell
    Create-AADIdentityApp.ps1
```

| Parameter | Vereist of optioneel | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| DirectoryTenantName | Vereist | Null | Azure AD tenant-ID. Geef het GUID of een tekenreeks. Een voorbeeld is myazureaaddirectory.onmicrosoft.com. |
| AdminArmEndpoint | Vereist | Null | Beheerder Azure Resource Manager-eindpunt. Een voorbeeld is adminmanagement.local.azurestack.external. |
| TenantARMEndpoint | Vereist | Null | Tenant van Azure Resource Manager-eindpunt. Een voorbeeld is management.local.azurestack.external. |
| AzureStackAdminCredential | Vereist | Null | Azure AD-service admin credential. |
| CertificateFilePath | Vereist | Null | **Volledig pad** naar het certificaatbestand van de identiteit toepassing eerder gegenereerd. |
| CertificatePassword | Vereist | Null | Wachtwoord beschermt de persoonlijke sleutel van het certificaat. |

## <a name="create-an-active-directory-federation-services-application"></a>Een Active Directory Federation Services-toepassing maken

Azure-Stack-omgevingen is beveiligd met AD FS, moet u een AD FS-service-principal ter ondersteuning van de volgende bewerkingen configureren:

- Virtuele-machineschaalset ingesteld integratie op worker lagen.
- Eenmalige aanmelding voor de Azure Functions-portal en geavanceerde developer-tools.

Beheerders moeten eenmalige aanmelding te configureren:

- Een service-principal voor de virtuele machine scale set integratie op worker lagen configureren.
- Schakel de geavanceerde ontwikkelhulpprogramma's in App Service (Kudu).
- Het gebruik van de Azure Functions-portal-ervaring inschakelen.

Volg deze stappen:

1. Open een PowerShell-exemplaar als azurestack\AzureStackAdmin.
2. Ga naar de locatie van de scripts die u hebt gedownload en uitgepakt de [vereiste stap](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#download-the-azure-app-service-on-azure-stack-installer-and-helper-scripts).
3. [Installeer PowerShell voor Azure Stack](azure-stack-powershell-install.md).
4. Voer de **maken ADFSIdentityApp.ps1** script.
5. In de **referentie** venster uw AD FS cloud admin-account en wachtwoord invoeren. Selecteer **OK**.
6. Geef de bestandspad certificaat en het wachtwoord van het certificaat voor de [certificaat eerder gemaakte](https://docs.microsoft.com/en-gb/azure/azure-stack/azure-stack-app-service-before-you-get-started#certificates-required-for-azure-app-service-on-azure-stack). Het certificaat voor deze stap standaard is gemaakt, **sso.appservice.local.azurestack.external.pfx**.

```PowerShell
    Create-ADFSIdentityApp.ps1
```

| Parameter | Vereist of optioneel | Standaardwaarde | Beschrijving |
| --- | --- | --- | --- |
| AdminArmEndpoint | Vereist | Null | Beheerder Azure Resource Manager-eindpunt. Een voorbeeld is adminmanagement.local.azurestack.external. |
| PrivilegedEndpoint | Vereist | Null | Bevoorrechte eindpunt. Een voorbeeld is AzS ERCS01. |
| CloudAdminCredential | Vereist | Null | Referentie voor domein-account voor Azure-Stack cloud beheerders. Een voorbeeld is Azurestack\CloudAdmin. |
| CertificateFilePath | Vereist | Null | **Volledig pad** aan de identiteit van de toepassing PFX-certificaatbestand. |
| CertificatePassword | Vereist | Null | Wachtwoord beschermt de persoonlijke sleutel van het certificaat. |

## <a name="next-steps"></a>Volgende stappen

[Installeer de App Service-resourceprovider](azure-stack-app-service-deploy.md)

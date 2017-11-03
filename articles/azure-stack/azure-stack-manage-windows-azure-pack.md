---
title: Beheren van Windows Azure Pack virtuele machines van Azure-Stack | Microsoft Docs
description: Informatie over het beheren van Windows Azure Pack (WAP) virtuele machines van de gebruikersportal in Azure-Stack.
services: azure-stack
documentationcenter: 
author: walterov
manager: byronr
editor: 
ms.assetid: 213c2792-d404-4b44-8340-235adf3f8f0b
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: walterov
ms.openlocfilehash: b07a18055d149e20cd605a892063eccecf3df8a4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-windows-azure-pack-virtual-machines-from-azure-stack"></a>Windows Azure Pack virtuele machines van Azure-Stack beheren

*Van toepassing op: Azure stapelen Development Kit*

U kunt toegang via de portal van de gebruiker Azure Stack aan virtuele machines op Windows Azure Pack tenants inschakelen in de Azure-Stack Development Kit. Gebruikers kunnen de Stack van Azure portal gebruiken voor het beheren van hun bestaande IaaS virtuele machines en virtuele netwerken. Deze resources worden beschikbaar gesteld op Windows Azure Pack via de onderliggende Service Provider Foundation (SPF) en Virtual Machine Manager (VMM)-onderdelen. Gebruikers kunnen in het bijzonder:

* Resources bladeren
* Configuratiewaarden onderzoeken
* Stoppen of een virtuele machine starten
* Verbinding maken met een virtuele machine via Remote Desktop Protocol (RDP)
* Controlepunten maken en beheren
* Verwijderen van virtuele machines en virtuele netwerken

Deze functionaliteit wordt verstrekt door de Windows Azure Pack-Connector voor Azure-Stack (Preview). In dit artikel laat zien hoe de connector voor een omgeving met één knooppunt Azure Stack configureren.

Voor deze preview-versie van de connector worden rekening met het volgende:
* Gebruik de Windows Azure Pack Connector alleen in een testomgeving (voor Azure-Stack en Windows Azure Pack) en niet in productie-implementaties.
* U moet Windows Azure Pack Update Rollup (UR) 9.1 of hoger en System Center SPF en VMM UR 9 of hoger hebben.
* De VMM- en SPF-onderdelen kunnen worden System Center 2012 R2 of System Center 2016.
* Op beide Azure-Stack en op Windows Azure Pack moet u de configuratiestappen uitvoeren.
* De instructies van toepassing op niet - Cloud Platform System CPS omgevingen.
* De bekende problemen Zie [probleemoplossing voor Microsoft Azure-Stack](azure-stack-troubleshooting.md).


## <a name="architecture"></a>Architectuur
Het volgende diagram toont de belangrijkste onderdelen van de Windows Azure Pack-Connector.

![De onderdelen van Windows Azure Pack-Connector](media/azure-stack-manage-wap/image1.png)

U ziet de volgende details:
* De gebruikersportal van Azure-Stack heeft toegang tot de resource-informatie van beide clouds (Azure-Stack en Windows Azure Pack).
* De gebruiker moet een account dat is geldig in beide omgevingen hebben.
* De gebruikersportal van Azure-Stack moet netwerktoegang hebben tot de onderdelen die worden uitgevoerd op Windows Azure Pack.
* In de **WAP** sectie van het diagram ziet u de nieuwe Windows Azure Pack Connector-modules (WAP-extensie en Connector) en de bestaande Windows Azure Pack Tenant-API met SPF en VMM-onderdelen.


## <a name="identity-management"></a>Identiteitsbeheer
De Azure-Stack beveiligingstokenservice (STS) moeten worden vertrouwd door de Windows Azure Pack Tenant-API.

Wanneer een gebruiker een actie wordt uitgevoerd via de Stack van Azure-portal die Windows Azure Pack resources is gericht, wordt in de portal maakt gebruik van de Windows Azure Pack Tenant-API. Daarom kan de opgegeven gebruiker verificatietoken moet afkomstig zijn van een vertrouwde STS. Zie het volgende diagram:

![Diagram van Windows Azure Pack Connector verificatie](media/azure-stack-manage-wap/image2.png)

In de ontwikkelomgeving kit hebben Windows Azure Pack en de Azure-Stack onafhankelijke identiteitsproviders. Gebruikers die toegang hebben tot beide omgevingen van de Stack van Azure-portal moeten dezelfde gebruikersnaam voor UPN (User Principal Name) in beide identiteitsproviders zijn. Bijvoorbeeld: het account  *azurestackadmin@azurestack.local*  moet tevens aanwezig zijn in de STS voor Windows Azure Pack. Wanneer u AD FS is niet ingesteld voor de ondersteuning van uitgaande vertrouwensrelaties, u de vertrouwensrelatie van de Windows Azure Pack-onderdelen (Tenant-API) met het Azure-Stack-exemplaar van AD FS.

## <a name="prerequisites"></a>Vereisten

### <a name="download-the-windows-azure-pack-connector"></a>De Windows Azure Pack Connector downloaden
Op de [Microsoft Download Center](https://aka.ms/wapconnectorazurestackdlc), moet het .exe-bestand downloaden en pak deze op uw lokale computer. Later, kopieert u de inhoud naar een computer die toegang heeft tot uw Windows Azure Pack-omgeving.

### <a name="deployment-option-requirement"></a>Vereisten voor toepassingsimplementatie-optie
Als u wilt integreren met Windows Azure Pack, kunt u Azure-Stack implementeren met behulp van de AD FS-optie of de Azure Active Directory.

### <a name="connectivity-requirements"></a>Vereisten voor connectiviteit
1. Vanaf de computer waarop u toegang tot de gebruikersportal van Azure-Stack, zorg ervoor dat u toegang tot de Windows Azure Pack tenantportal via de webbrowser.
2. De AzS WASP01 virtuele machine op Azure-Stack moet verbinding maken met de Windows Azure Pack portal tenant-computer. Gebruik Ping.exe netwerkconnectiviteit controleren. 
3.  U moet geldige certificaten voor de nieuwe Connector-services hebben. Deze SSL-certificaten moeten worden uitgegeven door een vertrouwde certificeringsinstantie (CA). U kunt geen zelfondertekende certificaten gebruiken. De SSL-certificaten moeten worden vertrouwd door de Azure-Stack (specifiek AzS WASP01-VM genoemd) en een andere computer die de tenant gebruiken kan voor toegang tot de gebruikersportal van Azure-Stack.
 
    >[!NOTE]
    Omdat Windows Server wordt uitgevoerd met de Server Core-installatieoptie van AzS WASP01, kunt u een opdrachtregelprogramma zoals Certutil.ext certificaat te importeren. Bijvoorbeeld, u kan het cer-bestand kopiëren naar c:\temp op AzS WASP01 en voer vervolgens de opdracht **certutil - addstore 'CA' 'c:\temp\certname.cer'**.

4.  Om vast te stellen RDP-verbinding met virtuele machines van Windows Azure Pack tenants via de Stack van Azure-portal, moet de Windows Azure Pack-omgeving extern bureaublad-verkeer naar de tenant-VM's toestaan.
5.  Voor connectiviteit tussen virtuele machines van Azure-Stack tenants en virtuele machines van Windows Azure Pack tenants moet hun externe IP-adressen routeerbaar tussen de twee omgevingen. Deze connectiviteit kan ook een DNS-server voor het omzetten van namen van de virtuele machine tussen de omgevingen koppelen.

### <a name="iis-requirements"></a>Vereisten voor IIS
De computer die als host fungeert voor de tenantportal van Windows Azure Pack (dit is mogelijk een fysieke host, een virtuele machine of meerdere virtuele machines) moet de URL Herschrijf de IIS-uitbreiding geïnstalleerd hebben. Als deze nog niet is geïnstalleerd, kunt u het downloaden van [hier](https://www.iis.net/downloads/microsoft/url-rewrite). Als meerdere virtuele machines de tenantportal host, kunt u de uitbreiding voor de installeren op elke virtuele machine.

## <a name="configure-azure-stack"></a>Azure Stack configureren
Voordat u de Windows Azure Pack-Connector configureert, moet u meerdere cloud-modus in de gebruikersportal van Azure-Stack inschakelen. Deze modus kunnen gebruikers uit welke cloud-toegang tot bronnen te selecteren.

Om in te schakelen met meerdere cloud-modus, moet u het script toevoegen AzurePackConnector.ps1 uitvoeren na de implementatie van de Azure-Stack. De volgende tabel beschrijft de scriptparameters.


|  Parameter | Beschrijving | Voorbeeld |   
| -------- | ------------- | ------- |  
| AzurePackClouds | URI's van de Windows Azure Pack-Connectors. Deze URI's moet overeenkomen met de Windows Azure Pack tenant portals. | @{CloudName = 'AzurePack1'; CloudEndpoint = "https://waptenantportal1:40005"},@{CloudName = 'AzurePack2'; CloudEndpoint = "https://waptenantportal2:40005"}<br><br>  (Standaard is de poortwaarde 40005.) |  
| AzureStackCloudName | Label voor de lokale Azure-Stack-cloud.| 'AzureStack' |
| DisableMultiCloud | Overschakelen naar de modus met meerdere cloud uitschakelen.| N.v.t. |
| | |

U kunt het script toevoegen AzurePackConnector.ps1 uitvoeren onmiddellijk na de implementatie of hoger. Het script uitgevoerd onmiddellijk na de implementatie, gebruiken dezelfde Windows PowerShell-sessie waarop Azure Stack-implementatie is voltooid. Anders kunt u een nieuwe Windows PowerShell-sessie als administrator (aangemeld als het account azurestackadmin) openen.

1. Voer het script toevoegen AzurePackConnector.ps1 met behulp van de volgende opdrachten (met waarden die specifiek zijn voor uw omgeving). U ziet dat het script toevoegen AzurePackConnector kunt u meer dan één eindpunt van de Windows Azure Pack Connector toevoegen.
 
 ```powershell
    $cred = New-Object System.Management.Automation.PSCredential("cloudadmin@azurestack.local", `
    (ConvertTo-SecureString -String "<password>" -AsPlainText -Force))
    $session = New-PSSession -ComputerName 'azs-ercs01' `
     -Credential $cred `
     -ConfigurationName PrivilegedEndpoint `
     -Authentication Credssp

    # Enable Multicloud
    Invoke-Command -Session $session -ScriptBlock { Add-AzurePackConnector -AzurePackClouds `
    @{CloudName = "AzurePack_1"; CloudEndpoint = "https://waptenantportal1:40005"},`
    @{CloudName = "AzurePack_2"; CloudEndpoint = "https://waptenantportal2:40005" } `
    -AzureStackCloudName "AzureStack" }

 ```
> [!NOTE]
> In de huidige build is een probleem waarbij nadat het script toevoegen AzurePackConnector is beëindigd, deze in een lus polling gedurende langere tijd (enkele minuten) totdat blijft deze wordt beëindigd. Nadat u het bericht ziet **uitgebreid: stap 'Azure Pack Connector configureren' status: 'Geslaagd'**, u kunt het script wordt beëindigd of wacht totdat deze niet meer zelf. Het won't verschil maken omdat de configuratie is al voltooid.

2. Noteer de uitvoerbestanden die worden geproduceerd door dit script, één voor elke Windows Azure Pack-omgeving die u hebt opgegeven. De bestanden bevinden zich op: \\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput. Deze bestanden bevatten informatie die vereist is voor het configureren van de Windows Azure Pack doel omgevingen. U kunt dit bestand als een parameter doorgeven aan een script verderop in deze instructies. Dit bestand bevat de volgende informatie:

    * **AzurePackConnectorEndpoint**: bevat het eindpunt met de service Windows Azure Pack-Connector.
    * **AuthenticationIdentityProviderPartner**: de volgende waarde-paar bevat:
        * Verificatietoken voor het handtekeningcertificaat dat de Windows Azure Pack Tenant-API moet aan de vertrouwde accepteert de aanroepen van de uitbreiding van de Stack van Azure-portal.

        * De 'Realm' die is gekoppeld aan het handtekeningcertificaat. Bijvoorbeeld: https://adfs.local.azurestack.global.external/adfs/c1d72562-534e-4aa5-92aa-d65df289a107/.

3.  Blader naar de map waarin de uitvoerbestanden (\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput), en kopieer de bestanden op uw lokale computer. De bestanden ziet er ongeveer als dit: AzurePack-06-27-15-50.txt.

4.  Test de configuratie.

    a. Open uw browser en meld u aan bij de gebruikersportal van Azure-Stack (https://portal.local.azurestack.external/).
    
    b. Nadat u zich aanmeldt als een tenant en de belasting voor de portal, ziet u fouten over niet meer kunt ophalen van abonnementen of extensies van de cloud Azure Pack. Klik op **OK** deze berichten sluiten. (Deze foutberichten verdwijnt nadat u Windows Azure Pack configureren.)

    c. U ziet de **Cloud** vervolgkeuzelijst in de linkerbovenhoek van de portal.

    ![De selector cloud in de Azure-Stack-gebruikersinterface](media/azure-stack-manage-wap/image3.png)

## <a name="configure-windows-azure-pack"></a>Configureer Windows Azure Pack
Windows Azure Pack vereist voor deze Connector preview-versie, handmatige configuratie.

>[!IMPORTANT]
Gebruik voor deze preview-versie, de Windows Azure Pack-Connector in een testomgeving en niet in productie-implementaties.

1.  Connector-MSI-bestanden op de Windows Azure Pack portal virtuele tenantmachine installeren en installeer certificaten. (Als er meerdere portal virtuele machines van tenants, moet u deze stap voltooien op elke virtuele machine.)

    a. In Windows Verkenner, kopieert u de **WAPConnector** map (wat u eerder hebt gedownload) naar een map met de naam **c:\temp** in de tenant portal virtuele machine.

    b. Open een Console of RDP-verbinding met de tenant portal virtuele machine.

    c. Wijzig de mappen te **c:\temp\wapconnector\setup\scripts**, en voer de **installeren Connector.ps1** script drie MSI-bestanden te installeren. Er zijn geen parameters zijn vereist.

     ```powershell
    cd C:\temp\wapconnector\setup\scripts\

    .\Install-Connector.ps1
    ```
     d. Wijzig de mappen te **c:\inetpub** en controleer of de drie nieuwe sites zijn geïnstalleerd:

       * MgmtSvc-Connector

       * MgmtSvc ConnectorExtension

       * MgmtSvc ConnectorController

    e. Uit dezelfde **c:\temp\wapconnector\setup\scripts** map de **configureren Certificates.ps1** script om certificaten te installeren. Standaard wordt hetzelfde certificaat die beschikbaar is voor de site Tenantportal in Windows Azure Pack gebruikt. Zorg ervoor dat dit een geldig certificaat (vertrouwd door de virtuele machine van Azure Stack AzS-WASP01 en elke clientcomputer die toegang heeft tot de Stack van Azure-portal). Communicatie anders werkt niet. (U kunt ook expliciet doorgeven vingerafdruk van het certificaat als een parameter met behulp van de vingerafdruk van het parameter-.)

     ```powershell
        cd C:\temp\wapconnector\setup\scripts\

        .\Configure-Certificates.ps1
    ```

    f. De configuratie van deze drie services voltooien, uit te voeren de **configureren WapConnector.ps1** script voor het bijwerken van de parameters van Web.config-bestand.

    |  Parameter | Beschrijving | Voorbeeld |   
    | -------- | ------------- | ------- |  
    | TenantPortalFQDN | De Windows Azure Pack tenantportal FQDN-naam. | tenant.contoso.com | 
    | TenantAPIFQDN | De Windows Azure Pack Tenant-API FQDN-naam. | tenantapi.contoso.com  |
    | AzureStackPortalFQDN | De Azure-Stack-gebruikersportal FQDN-naam. | Portal.local.azurestack.external |
    | | |
    
     ```powershell
    .\Configure-WapConnector.ps1 -TenantPortalFQDN "tenant.contoso.com" `
         -TenantAPIFQDN "tenantapi.contoso.com" `
         -AzureStackPortalFQDN "portal.local.azurestack.external"
    ```
    g. Als u meerdere portal virtuele tenantmachines hebt, herhaalt u stap 1 voor elk van deze virtuele machines.

2. Installeer de nieuwe Tenant-API-MSI op elke virtuele machine van Windows Azure Pack Tenant-API.

    a. Als u een load balancer wordt gebruikt, is het raadzaam de virtuele machine als offline markeren.

    b. In Windows Verkenner, kopieert u de **WAPConnector** map naar een map met de naam **c:\temp** op elke machine Tenant-API.

    c. Kopieer het AzurePackConnectorOutput.txt-bestand dat u eerder hebt opgeslagen naar **c:\temp\WAPConnector**.

    d. Open de Console of RDP verbinding met de Tenant-API-VM gekopieerd van de bestanden.
    
    e. Wijzig de mappen te **c:\temp\wapconnector\setup\scripts**, en voer **Update TenantAPI.ps1**. Deze nieuwe versie van de WAP-Tenant-API bevat een wijziging om in te schakelen van een vertrouwensrelatie niet alleen met de huidige STS, maar ook met het exemplaar van AD FS in Azure-Stack.

     ```powershell
    cd C:\temp\wapconnector\setup\packages\

    .\Update-TenantAPI.ps1
    ```

    f.  Herhaal stap 2 op een andere virtuele machine met de Tenant-API.
3. Van **slechts één** van de Tenant-API virtuele machines, voer het script configureren TrustAzureStack.ps1 om toe te voegen van een vertrouwensrelatie tussen de Tenant-API en het exemplaar van AD FS op Azure-Stack. U moet een account met sysadmin toegang tot de database Microsoft.MgmtSvc.Store gebruiken. Dit script heeft de volgende parameters:

    | Parameter | Beschrijving | Voorbeeld |
    | --------- | ------------| ------- |
   | SQL Server | De naam van de SQL-Server die de database Microsoft.MgmtSvc.Store bevat. Deze parameter is vereist. | SQL Server | 
   | DataFile | Het uitvoerbestand dat tijdens de configuratie van de Azure-Stack meerdere cloud-modus eerder werd gegenereerd. Deze parameter is vereist. | AzurePack-06-27-15-50.txt | 
   | PromptForSqlCredential | Hiermee wordt aangegeven dat het script moet worden gevraagd interactief voor de referentie van een SQL-verificatie te gebruiken bij het verbinden met de SQL Server-exemplaar. De opgegeven referentie moet voldoende rechten hebt voor het verwijderen van de databases, schema's en gebruikersaanmeldingen verwijderen. Als niets wordt opgegeven, het script wordt ervan uitgegaan dat huidige context van de gebruiker toegang heeft. | Er is geen waarde nodig. |
   |  |  |

    Als u niet dat de SQL-Server weet wilt gebruiken, kunt u het kunt detecteren. Verbinding maken met de computer van de Tenant-API, gebruik de opdracht Unprotect MgmtSvc voor opheffen van het bestand Web.config voor Tenant-API en zoekt u de naam van de server in de verbindingstekenreeks. Vergeet niet om uit te voeren Protect-MgmtSvc voor het beveiligen van het bestand Web.config voor Tenant-API.

  ```powershell
  cd C:\temp\wapconnector\setup\scripts\

 .\Configure-TrustAzureStack.ps1 -SqlServer "SQLServer" `
       -DataFile "C:\temp\wapconnector\AzurePackConnectorOutput.txt"
  ```

## <a name="example"></a>Voorbeeld
Het volgende voorbeeld ziet een volledige implementatie van Windows Azure Pack-Connector op een Azure-Stack configuratie met één knooppunt en twee Windows Azure Pack Express-installaties. (Elke snelle installatie wordt op één computer met de voorbeeldnamen *wapcomputer1* en*wapcomputer2*.)

```powershell
# Run the following script on the Azure Stack host
$cred = New-Object System.Management.Automation.PSCredential("cloudadmin@azurestack.local",`
     (ConvertTo-SecureString -String "p@ssw0rd" -AsPlainText -Force))
$session = New-PSSession -ComputerName 'azs-ercs01' -Credential $cred `
     -ConfigurationName PrivilegedEndpoint -Authentication Credssp
 
# Enable Multicloud
invoke-command -Session $session -ScriptBlock { Add-AzurePackConnector -AzurePackClouds `
     @{CloudName = "AzurePack_1"; CloudEndpoint = "https://wapcomputer1.contoso.com:40005"},`
     @{CloudName = "AzurePack_2"; CloudEndpoint = "https://wapcomputer2.contoso.com:40005"}`
     -AzureStackCloudName "AzureStack" }  

```
Download het .exe-bestand van de [Microsoft Download Center](https://aka.ms/wapconnectorazurestackdlc), pakt u het en kopieer de map WAPConnector naar een **c:\temp** map op de computer met Windows Azure Pack. Kopieer de bestanden die zijn gegenereerd als uitvoer in het vorige script (te vinden op \\\su1fileserver\SU1_Infrastructure_1\AzurePackConnectorOutput) naar de **c:\temp\WAPConnector** map. (De bestanden wordt ziet er ongeveer als volgt: AzurePack-06-27-15-50.txt.) Voer het volgende script eenmaal per exemplaar van Windows Azure Pack:

 ```powershell
# Install Connector components
cd C:\temp\WAPConnector\Setup\Scripts
.\Install-Connector.ps1

# Configure Certificates for the new Connector services
.\Configure-Certificates.ps1

# Configure the Connector services
.\Configure-WapConnector.ps1 -TenantPortalFQDN "wapcomputer1.contoso.com" `
     -TenantAPIFQDN "wapcomputer1.contoso.com" `
     -AzureStackPortalFQDN "portal.local.azurestack.external"

# Install the updated TenantAPI
.\Update-TenantAPI.ps1

# Establish trust with the Azure Stack AD FS
.\Configure-TrustAzureStack.ps1 -SqlServer "wapcomputer1" `
     -DataFile "C:\temp\wapconnector\AzurePack-06-27-15-50.txt" 

```
## <a name="troubleshooting-tips"></a>Tips voor probleemoplossing
1.  Zorg ervoor dat er netwerkverbinding is tussen Azure-Stack en Windows Azure Pack. Deze connectiviteit moet liggen tussen elke tenantcomputer die toegang heeft tot de Stack van Azure-portal en de Windows Azure Pack portal virtuele tenantmachine waar de nieuwe Connector-services worden uitgevoerd.
2.  Controleer of alle opgegeven dat FQDN-namen juist zijn.
3.  Zorg ervoor dat het SSL-certificaten gebruikt in de nieuwe Connector-services worden vertrouwd door de Azure-Stack (specifiek AzS WASP01-VM genoemd) en andere computer die de tenant gebruiken kan voor toegang tot de gebruikersportal van Azure-Stack.
4. Zie voor bekende problemen [probleemoplossing voor Microsoft Azure-Stack](azure-stack-troubleshooting.md).


## <a name="next-steps"></a>Volgende stappen
[Met behulp van de portals beheerder en gebruiker in Azure-Stack](azure-stack-manage-portals.md)

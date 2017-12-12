---
title: PowerShell gebruiken voor het maken van een virtuele machine met een rapportserver Native modus | Microsoft Docs
description: 'In dit onderwerp wordt beschreven en wordt u begeleid bij de implementatie en configuratie van een SQL Server Reporting Services native modus rapportserver in een virtuele Machine van Azure. '
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 553af55b-d02e-4e32-904c-682bfa20fa0f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: 0b9f12127276f5aa689c4a1d3a5bf9fe645a0fc7
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a>PowerShell gebruiken om een Azure VM te maken met een rapportserver in systeemeigen modus
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

In dit onderwerp wordt beschreven en wordt u begeleid bij de implementatie en configuratie van een SQL Server Reporting Services native modus rapportserver in een virtuele Machine van Azure. De stappen in dit document gebruikt een combinatie van handmatige stappen om de virtuele machine en een Windows PowerShell-script voor het configureren van Reporting Services op de virtuele machine te maken. Het configuratiescript bevat een firewallpoort worden geopend voor HTTP of HTTPs.

> [!NOTE]
> Als u geen vereist **HTTPS** op de rapportserver **slaat u stap 2**.
> 
> Nadat de virtuele machine is gemaakt in stap 1, gaat u naar de sectie gebruik script voor het configureren van de rapportserver en HTTP. Nadat u het script uitvoert, is de rapportserver wilt gebruiken.

## <a name="prerequisites-and-assumptions"></a>Vereisten en veronderstellingen
* **Azure-abonnement**: Controleer of het aantal kernen beschikbaar in uw Azure-abonnement. Als u de aanbevolen VM-grootte van **A3**, moet u **4** beschikbaar kernen. Als u een VM-grootte van **A2**, moet u **2** beschikbaar kernen.
  
  * Om te controleren of de core-limiet van uw abonnement, in de Azure portal, klik op instellingen in het linkerdeelvenster en klikt u op informatie over het gebruik in het menu bovenaan.
  * Voor een verhoging van het quotum voor kernen, neem contact op met [ondersteuning van Azure](https://azure.microsoft.com/support/options/). Zie voor informatie over de grootte van virtuele machine [grootten van virtuele machines voor Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* **Windows PowerShell-scripts**: het onderwerp wordt ervan uitgegaan dat u een werkende basiskennis hebben van Windows PowerShell. Zie de volgende onderwerpen voor meer informatie over het gebruik van Windows PowerShell:
  
  * [Windows PowerShell op WindowsServer wordt gestart](https://technet.microsoft.com/library/hh847814.aspx)
  * [Aan de slag met Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Stap 1: Een Azure-Machine inrichten
1. Blader naar de Azure-portal.
2. Klik op **virtuele Machines** in het linkerdeelvenster.
   
    ![Microsoft azure virtuele machines](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. Klik op **Nieuw**.
   
    ![Knop Nieuw](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. Klik op **vanuit galerie**.
   
    ![nieuwe virtuele machine vanuit galerie](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. Klik op **SQL Server 2014 RTM Standard – Windows Server 2012 R2** en klik vervolgens op de pijl om door te gaan.
   
    ![Volgende](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    Als u de Reporting Services gegevensgestuurde abonnementen functie nodig hebt, kiest u **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. Zie voor meer informatie over SQL Server-edities en functieondersteuning [functies ondersteund door de edities van SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).
6. Op de **Virtuele-machineconfiguratie** pagina, bewerkt u de volgende velden:
   
   * Als er meer dan één **versie RELEASEDATUM**, selecteert u de meest recente versie.
   * **Naam van virtuele Machine**: naam van de machine wordt ook gebruikt bij de volgende pagina van de configuratie als de standaardnaam van de Cloud Service DNS. De DNS-naam moet uniek zijn in de Azure-service. Houd rekening met de virtuele machine configureren met de naam van een computer die wordt beschreven wat de virtuele machine wordt gebruikt. Bijvoorbeeld ssrsnativecloud.
   * **Laag**: standaard
   * **Grootte: A3** is de aanbevolen VM-grootte voor SQL Server-werkbelastingen. Als een virtuele machine alleen als een rapportserver gebruikt wordt, is een VM-grootte van A2 voldoende tenzij de report server optreedt in een grote werkbelasting. Zie voor een VM prijsgegevens, [prijzen van virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/).
   * **Nieuwe gebruikersnaam**: de naam die u opgeeft als beheerder op de virtuele machine wordt gemaakt.
   * **Nieuw wachtwoord** en **bevestigen**. Dit wachtwoord wordt gebruikt voor het nieuwe beheerdersaccount en het wordt aanbevolen dat een sterk wachtwoord te gebruiken.
   * Klik op **Volgende**. ![volgende](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
7. Op de volgende pagina de volgende velden te bewerken:
   
   * **Cloudservice**: Selecteer **Maak een nieuwe Cloudservice**.
   * **Cloud-Service DNS-naam**: dit is de openbare DNS-naam van de Cloudservice die is gekoppeld aan de virtuele machine. De standaardnaam is de naam die u hebt ingevoerd voor de VM-naam. Als in latere stappen van het onderwerp u een vertrouwd certificaat voor SSL maakt en vervolgens de DNS-naam wordt gebruikt voor de waarde van de '**verleend aan**' van het certificaat.
   * **Regio/affiniteit groep/virtueel netwerk**: Kies de regio die het dichtst bij uw eindgebruikers.
   * **Storage-Account**: een automatisch gegenereerde opslagaccount gebruiken.
   * **Beschikbaarheidsset**: geen.
   * **EINDPUNTEN** houden de **extern bureaublad** en **PowerShell** eindpunten en voeg vervolgens een HTTP- of HTTPS-eindpunt, afhankelijk van uw omgeving.
     
     * **HTTP**: de openbare en persoonlijke standaardpoorten zijn **80**. Let op: als u een particuliere poort dan 80, wijzigt u **$HTTPport = 80** in het http-script.
     * **HTTPS**: de openbare en persoonlijke standaardpoorten zijn **443**. Er is een best practice bij beveiliging voor het wijzigen van de particuliere poort en configureer uw firewall en de rapportserver de particuliere poort gebruiken. Zie voor meer informatie over eindpunten [hoe instellen om communicatie met een virtuele Machine](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Let op: als u een andere poort dan 443, wijzigt u de parameter **$HTTPsport = 443** in het HTTPS-script.
   * Klik op volgende. ![Volgende](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. Gebruik de standaardwaarde op de laatste pagina van de wizard **de VM-agent installeren** geselecteerde. De stappen in dit onderwerp maken geen gebruik van de VM-agent, maar als u van plan bent te houden van deze virtuele machine, de VM-agent en -extensies kunt u hij CM verbeteren.  Zie voor meer informatie over de VM-agent [VM-Agent en -extensies – Part 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Een van de standaard-serverextensies geïnstalleerd ad met is de extensie 'BGINFO' die wordt weergegeven op de VM-bureaublad, systeemgegevens zoals intern IP-adres en de vrije schijfruimte.
9. Klik op voltooien. ![ok](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. De **Status** van de virtuele machine wordt weergegeven als **starten (inrichten)** bij het inrichten verwerken en vervolgens wordt weergegeven, **met** wanneer de virtuele machine is ingericht en gereed voor gebruik.

## <a name="step-2-create-a-server-certificate"></a>Stap 2: Een servercertificaat maken
> [!NOTE]
> Als u op de rapportserver niet HTTPS vereist, kunt u **slaat u stap 2** en Ga naar de sectie **script gebruiken voor het configureren van de rapportserver en HTTP-**. Het HTTP-script gebruiken voor het configureren van de rapportserver snel en de rapportserver is klaar voor gebruik.

Als u wilt gebruiken HTTPS op de virtuele machine, moet u een vertrouwd certificaat voor SSL. Afhankelijk van uw scenario kunt u een van de volgende twee methoden:

* Een geldig SSL-certificaat uitgegeven door een certificeringsinstantie (CA) en wordt vertrouwd door Microsoft. De basis-CA-certificaten zijn vereist voor het worden gedistribueerd via het Microsoft Root Certificate Program. Zie voor meer informatie over dit programma [Windows en Windows Phone 8 SSL Root Certificate Program (lid CA's)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) en [Inleiding tot het Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).
* Een zelfondertekend certificaat. Zelfondertekende certificaten worden niet aanbevolen voor productieomgevingen.

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>Een certificaat dat is gemaakt door een vertrouwde certificeringsinstantie (CA) te gebruiken
1. **Vraag een servercertificaat voor de website van een certificeringsinstantie**. 
   
    U kunt de Wizard Webservercertificaat gebruiken voor het genereren van een certificaataanvraagbestand (Certreq.txt) die u naar een certificeringsinstantie (CA verzendt) of voor het genereren van een aanvraag voor een online-certificeringsinstantie. Bijvoorbeeld: Microsoft Certificate Services in Windows Server 2012. Afhankelijk van het niveau van uw identiteit die door een servercertificaat aangeboden, is enkele dagen tot enkele maanden duren voor de certificeringsinstantie uw aanvraag goedkeuren en verzendt u een certificaatbestand. 
   
    Zie de volgende onderwerpen voor meer informatie over het aanvragen van een servercertificaten: 
   
   * Gebruik [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
   * Beveiligingsprogramma's voor het beheer van WindowsServer 2012.
     
     [Beveiligingsprogramma's voor het beheer van WindowsServer 2012](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > De **verleend aan** veld van het vertrouwde SSL-certificaat moet hetzelfde zijn als de **Cloud Service DNS-naam** u gebruikt voor de nieuwe virtuele machine.

2. **Het servercertificaat installeren op de webserver**. De webserver in dit geval is de virtuele machine die als host fungeert voor de rapportserver en de website wordt gemaakt in latere stappen wanneer u Reporting Services configureren. Zie voor meer informatie over het installeren van het servercertificaat op de webserver met behulp van de certificaat-MMC-module [een servercertificaat installeren](https://technet.microsoft.com/library/cc740068).
   
    Als u gebruiken van het script opgenomen in dit onderwerp wilt voor het configureren van de rapportserver, de waarde van de certificaten **vingerafdruk** is vereist als een parameter van het script. Zie de volgende sectie voor meer informatie over het verkrijgen van de vingerafdruk van het certificaat.
3. Het servercertificaat toewijzen aan de rapportserver. De toewijzing is voltooid in de volgende sectie bij het configureren van de rapportserver.

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a>Zelfondertekend certificaat voor het gebruik van de virtuele Machines
Een zelfondertekend certificaat is gemaakt op de virtuele machine wanneer de virtuele machine is ingericht. Het certificaat heeft dezelfde naam als de VM DNS-naam. Om te voorkomen dat certificaatfouten, is het vereist dat het certificaat vertrouwd op de virtuele machine zelf, en ook door alle gebruikers van de site wordt.

1. Als u van het certificaat op de lokale virtuele machine de basis-CA vertrouwt, het certificaat toevoegen aan de **Trusted Root Certification Authorities**. Hier volgt een samenvatting van de stappen die nodig zijn. Zie voor gedetailleerde stappen voor het vertrouwen van de CA [een servercertificaat installeren](https://technet.microsoft.com/library/cc740068).
   
   1. Selecteer de virtuele machine vanuit de Azure-portal en klik op verbinden. Afhankelijk van de browserconfiguratie van uw wordt u mogelijk gevraagd een RDP-bestand voor het verbinden met de virtuele machine op te slaan.
      
       ![verbinding maken met virtuele machine van azure](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Gebruik de gebruikersnaam van de VM, gebruikersnaam en wachtwoord die u hebt geconfigureerd tijdens het maken van de virtuele machine. 
      
       In de volgende afbeelding, naam van de VM is bijvoorbeeld **ssrsnativecloud** en de gebruikersnaam is **testgebruiker**.
      
       ![aanmeldingsnaam opgenomen vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. Mmc.exe worden uitgevoerd. Zie voor meer informatie [hoe: certificaten weergeven met de MMC-module](https://msdn.microsoft.com/library/ms788967.aspx).
   3. In de consoletoepassing **bestand** menu toevoegen de **certificaten** -module, selecteer **computeraccount** wanneer u wordt gevraagd en klik vervolgens op **volgende**.
   4. Selecteer **lokale Computer** te beheren en klik vervolgens op **voltooien**.
   5. Klik op **Ok** en vouw vervolgens de **certificaten - persoonlijke** knooppunten en klik vervolgens op **certificaten**. Het certificaat is dezelfde naam als de DNS-naam van de virtuele machine en eindigt op **cloudapp.net**. Met de rechtermuisknop op de naam van het certificaat en klik op **kopie**.
   6. Vouw de **Trusted Root Certification Authorities** knooppunt en klik met de rechtermuisknop **certificaten** en klik vervolgens op **plakken**.
   7. Als u wilt valideren, dubbelklikt u op de naam van het certificaat onder **Trusted Root Certification Authorities** en controleer of er zijn geen fouten ziet u uw certificaat. Als u het HTTPS-script dat is opgenomen in dit onderwerp gebruiken wilt voor het configureren van de rapportserver, de waarde van de certificaten **vingerafdruk** is vereist als een parameter van het script. **Ophalen van de vingerafdrukwaarde**, het volgende voltooien. Er is ook een PowerShell-voorbeeld voor het ophalen van de vingerafdruk in sectie [script gebruiken voor het configureren van de rapportserver en HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).
      
      1. Dubbelklik op de naam van het certificaat, bijvoorbeeld ssrsnativecloud.cloudapp.net.
      2. Klik op de **Details** tabblad.
      3. Klik op **vingerafdruk**. De waarde van de vingerafdruk wordt weergegeven in het veld met details, bijvoorbeeld a6 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.
      4. Kopieer de vingerafdruk en de waarde opslaan voor later of bewerk het script nu.
      5. (*) Voordat u het script uitvoert, verwijdert u de spaties tussen de paren van waarden. Zo zou a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f nu worden door de vingerafdruk die al eerder is vermeld.
      6. Het servercertificaat toewijzen aan de rapportserver. De toewijzing is voltooid in de volgende sectie bij het configureren van de rapportserver.

Als u een zelfondertekend SSL-certificaat gebruikt, wordt in de naam op het certificaat al overeenkomt met de hostnaam van de virtuele machine. Daarom is de DNS-server van de machine is al geregistreerd globaal en toegankelijk zijn vanaf elke client.

## <a name="step-3-configure-the-report-server"></a>Stap 3: Configureer de rapportserver
Deze sectie helpt u bij het configureren van de virtuele machine als een rapportserver voor native-modus van Reporting Services. U kunt een van de volgende methoden gebruiken voor het configureren van de rapportserver:

* Het script gebruiken voor het configureren van de rapportserver
* Configuration Manager gebruiken voor het configureren van de rapportserver.

Voor meer stappen gedetailleerde, Zie de sectie [verbinding maken met de virtuele Machine en Start u Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Verificatie-Opmerking:** Windows-verificatie is de aanbevolen verificatiemethode en is de standaard Reporting Services-verificatie. Alleen gebruikers die zijn geconfigureerd op de virtuele machine toegang heeft tot Reporting Services en toegewezen aan rollen voor Reporting Services.

### <a name="use-script-to-configure-the-report-server-and-http"></a>Script gebruiken voor het configureren van de rapportserver en HTTP
Voor het gebruik van de Windows PowerShell-script voor het configureren van de rapportserver, moet u de volgende stappen uitvoeren. De configuratie omvat HTTP, niet HTTPS:

1. Selecteer de virtuele machine vanuit de Azure-portal en klik op verbinden. Afhankelijk van de browserconfiguratie van uw wordt u mogelijk gevraagd een RDP-bestand voor het verbinden met de virtuele machine op te slaan.
   
    ![verbinding maken met virtuele machine van azure](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Gebruik de gebruikersnaam van de VM, gebruikersnaam en wachtwoord die u hebt geconfigureerd tijdens het maken van de virtuele machine. 
   
    In de volgende afbeelding, naam van de VM is bijvoorbeeld **ssrsnativecloud** en de gebruikersnaam is **testgebruiker**.
   
    ![aanmeldingsnaam opgenomen vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Open op de virtuele machine **Windows PowerShell ISE** met beheerdersbevoegdheden. De PowerShell ISE is standaard geïnstalleerd op WindowsServer 2012. Het verdient aanbeveling om dat u de ISE gebruiken in plaats van een standaard Windows PowerShell-venster zodat u kunt het script in de ISE plakken, het script wijzigen en vervolgens het script uitvoeren.
3. In Windows PowerShell ISE, klikt u op de **weergave** menu en klik vervolgens op **scriptvenster weergeven**.
4. Kopieer het volgende script en het script in de Windows PowerShell ISE scriptvenster plakken.
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
   
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
   
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## Setting the Report Manager URL ##
   
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
   
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
5. Als u de virtuele machine met een HTTP-poort 80 gemaakt, wijzigt u de parameter $HTTPport = 80.
6. Het script is momenteel geconfigureerd voor Reporting Services. Als u wilt het script uitvoeren voor Reporting Services, wijzigt u het gedeelte van de versie van het pad naar de naamruimte 'v11' op de Get-WmiObject-instructie.
7. Voer het script uit.

**Validatie**: om te controleren of de server-functionaliteit eenvoudig rapport werkt, Zie de [configuratie van de](#verify-the-configuration) verderop in dit onderwerp.

### <a name="use-script-to-configure-the-report-server-and-https"></a>Script voor het configureren van de report server- en HTTPS gebruiken
Voor het gebruik van Windows PowerShell voor het configureren van de rapportserver, moet u de volgende stappen uitvoeren. De configuratie omvat HTTPS, niet HTTP.

1. Selecteer de virtuele machine vanuit de Azure-portal en klik op verbinden. Afhankelijk van de browserconfiguratie van uw wordt u mogelijk gevraagd een RDP-bestand voor het verbinden met de virtuele machine op te slaan.
   
    ![verbinding maken met virtuele machine van azure](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Gebruik de gebruikersnaam van de VM, gebruikersnaam en wachtwoord die u hebt geconfigureerd tijdens het maken van de virtuele machine. 
   
    In de volgende afbeelding, naam van de VM is bijvoorbeeld **ssrsnativecloud** en de gebruikersnaam is **testgebruiker**.
   
    ![aanmeldingsnaam opgenomen vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Open op de virtuele machine **Windows PowerShell ISE** met beheerdersbevoegdheden. De PowerShell ISE is standaard geïnstalleerd op WindowsServer 2012. Het verdient aanbeveling om dat u de ISE gebruiken in plaats van een standaard Windows PowerShell-venster zodat u kunt het script in de ISE plakken, het script wijzigen en vervolgens het script uitvoeren.
3. Als u scripts uitgevoerd, voert u de volgende Windows PowerShell-opdracht:
   
        Set-ExecutionPolicy RemoteSigned
   
    Vervolgens kunt u het volgende om te controleren of het beleid uitvoeren:
   
        Get-ExecutionPolicy
4. In **Windows PowerShell ISE**, klikt u op de **weergave** menu en klik vervolgens op **scriptvenster weergeven**.
5. Kopieer het volgende script en plak deze in het scriptvenster Windows PowerShell ISE.
   
        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
   
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
   
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
   
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## 3. Setting the Report Manager URL ##
   
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
   
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
   
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
6. Wijzig de **$certificatehash** parameter in het script:
   
   * Dit is een **vereist** parameter. Als u de waarde van het certificaat niet uit de vorige stap hebt opgeslagen, een van de volgende methoden gebruiken om het kopiëren van de certificaat-hash-waarde van de vingerafdruk van certificaten.:
     
       Open Windows PowerShell ISE op de virtuele machine, en voer de volgende opdracht:
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       De uitvoer ziet er ongeveer als volgt. Als het script retourneert een lege regel, de virtuele machine heeft geen een certificaat dat zo is geconfigureerd, Zie de sectie [naar de zelfondertekende certificaten voor virtuele Machines gebruiken](#to-use-the-virtual-machines-self-signed-certificate).
     
     OF
   * Op de virtuele machine uitgevoerd mmc.exe en voeg vervolgens de **certificaten** -module.
   * Onder de **vertrouwde basiscertificeringsinstanties** knooppunt dubbelklikt u op de certificaatnaam van uw. Als u van het zelfondertekende certificaat van de virtuele machine gebruikmaakt, het certificaat is dezelfde naam als de DNS-naam van de virtuele machine en eindigt op **cloudapp.net**.
   * Klik op de **Details** tabblad.
   * Klik op **vingerafdruk**. De waarde van de vingerafdruk wordt weergegeven in het veld met details, bijvoorbeeld af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6 ter a9 c3 66 4f 31 90 48
   * **Voordat u het script uitvoeren**, verwijdert u de spaties tussen de paren van waarden. Bijvoorbeeld af1160b64b288d890a8212ff6ba9c3664f319048
7. Wijzig de **$httpsport** parameter: 
   
   * Als u poort 443 voor het HTTPS-eindpunt gebruikt, klikt u vervolgens hoeft niet u deze parameter in het script bijwerken. Gebruik anders de waarde van de poort die u hebt geselecteerd toen u de persoonlijke HTTPS-eindpunt geconfigureerd op de virtuele machine.
8. Wijzig de **$DNSName** parameter: 
   
   * Het script is geconfigureerd voor een certificaat met wild card $DNSName = '+'. Als u dit doet geen wilt configureren voor een jokerteken certificaatbinding uitcommentariëren $DNSName =' + 'en inschakelen van de volgende regel, de volledige $DNSNAme referentie, ## $DNSName="$server.cloudapp.net'.
     
       De waarde $DNSName wijzigen als u niet wilt gebruiken van DNS-naam van de virtuele machine voor Reporting Services. Als u de parameter gebruikt, wordt het certificaat moet ook gebruiken voor deze naam en de naam globaal op een DNS-server te registreren.
9. Het script is momenteel geconfigureerd voor Reporting Services. Als u wilt het script uitvoeren voor Reporting Services, wijzigt u het gedeelte van de versie van het pad naar de naamruimte 'v11' op de Get-WmiObject-instructie.
10. Voer het script uit.

**Validatie**: om te controleren of de server-functionaliteit eenvoudig rapport werkt, Zie de [configuratie van de](#verify-the-connection) verderop in dit onderwerp. Binding open een opdrachtprompt met beheerdersbevoegdheden en voer de volgende opdracht om te controleren of het certificaat:

    netsh http show sslcert

Het resultaat omvat het volgende:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a>Configuration Manager gebruiken voor het configureren van de rapportserver
Als u niet uitvoeren van de PowerShell-script wilt voor het configureren van de rapportserver, de stappen in deze sectie voor het Reporting Services native modus configuration manager gebruiken voor het configureren van de rapportserver.

1. Selecteer de virtuele machine vanuit de Azure-portal en klik op verbinden. Gebruik de gebruikersnaam en wachtwoord die u hebt geconfigureerd tijdens het maken van de virtuele machine.
   
    ![verbinding maken met virtuele machine van azure](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. Voer Windows update en installeer updates met de virtuele machine. Als een herstart van de virtuele machine vereist is, start van de virtuele machine en opnieuw verbinding maken met de virtuele machine van de Azure-portal.
3. Typ in het menu Start op de virtuele machine **Reporting Services** en open **Reporting Services Configuration Manager**.
4. Laat de standaardwaarden voor **servernaam** en **rapportserverexemplaar**. Klik op **Verbinden**.
5. Klik in het linkerdeelvenster op **URL van webservice**.
6. RS is standaard geconfigureerd voor HTTP poort 80 met IP-adres 'Alle toegewezen'. HTTPS toevoegen:
   
   1. In **SSL-certificaat**: Selecteer het certificaat dat u gebruiken wilt, bijvoorbeeld [VM name]. cloudapp.net. Als er geen certificaten worden vermeld, Zie de sectie **stap 2: Maak een servercertificaat** voor informatie over het installeren en het certificaat op de virtuele machine wordt vertrouwd.
   2. Onder **SSL-poort**: 443 kiezen. Als u de persoonlijke HTTPS-eindpunt geconfigureerd in de virtuele machine met een andere particuliere poort, hier die waarde gebruiken.
   3. Klik op **toepassen** en wacht totdat de bewerking is voltooid.
7. Klik in het linkerdeelvenster op **Database**.
   
   1. Klik op **wijzigen Databas**e.
   2. Klik op **maken van een nieuw report server-database** en klik vervolgens op **volgende**.
   3. Laat de standaardwaarde **servernaam**: als de virtuele machine een naam geven en laat de standaardwaarde **verificatietype** als **huidige gebruiker** – **geïntegreerde beveiliging**. Klik op **Volgende**.
   4. Laat de standaardwaarde **databasenaam** als **ReportServer** en klik op **volgende**.
   5. Laat de standaardwaarde **verificatietype** als **Servicereferenties** en klik op **volgende**.
   6. Klik op **volgende** op de **samenvatting** pagina.
   7. Wanneer de configuratie voltooid is, klikt u op **voltooien**.
8. Klik in het linkerdeelvenster op **URL van Report Manager**. Laat de standaardwaarde **virtuele map** als **rapporten** en klik op **toepassen**.
9. Klik op **afsluiten** te sluiten van het Reporting Services Configuration Manager.

## <a name="step-4-open-windows-firewall-port"></a>Stap 4: Open Windows Firewall-poort
> [!NOTE]
> Als u een van de scripts voor het configureren van de report server gebruikt, kunt u deze sectie overslaan. Het script opgenomen een stap om de firewallpoort te openen. De standaardwaarde is poort 80 voor HTTP en poort 443 voor HTTPS.
> 
> 

Om extern verbinding maken met Report Manager of de rapportserver op de virtuele machine, is een TCP-eindpunt vereist op de virtuele machine. Dit is vereist voor het openen van dezelfde poort in de VM-firewall. Het eindpunt is gemaakt toen de virtuele machine is ingericht.

Deze sectie bevat algemene informatie voor het openen van de firewallpoort. Zie voor meer informatie [een Firewall configureren voor toegang tot de Server van rapporten](https://technet.microsoft.com/library/bb934283.aspx)

> [!NOTE]
> Als u het script voor het configureren van de report server gebruikt, kunt u deze sectie overslaan. Het script opgenomen een stap om de firewallpoort te openen.
> 
> 

Als u een particuliere poort geconfigureerd voor HTTPS dan 443, wijzigt u het volgende script op de juiste wijze. Poort moet worden geopend **443** voltooien op de Windows Firewall, het volgende:

1. Open een Windows PowerShell-venster met beheerdersbevoegdheden.
2. Als u een andere poort dan 443 gebruikt wanneer u het HTTPS-eindpunt op de virtuele machine hebt geconfigureerd, de poort in de volgende opdracht bijwerken en vervolgens de opdracht uitvoeren:
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. Wanneer de opdracht is voltooid, **Ok** wordt weergegeven in het opdrachtpromptvenster.

Open een Windows PowerShell-venster om te bevestigen dat de poort is geopend, en voer de volgende opdracht:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a>Controleer de configuratie
Open uw browser met Administrator-bevoegdheden om te controleren of de server-functionaliteit eenvoudig rapport nu werkt, en blader vervolgens naar het volgende rapport ad rapport Serverbeheer URL's:

* Blader naar de URL van de rapportserver op de virtuele machine:
  
        http://localhost/reportserver
* Blader naar de URL van report Manager op de virtuele machine:
  
        http://localhost/Reports
* Vanaf uw lokale computer, blader naar de **externe** report Manager op de virtuele machine. Bijwerken van de DNS-naam in het volgende voorbeeld naar gelang van toepassing. Wanneer u wordt gevraagd om een wachtwoord, gebruikt u de beheerdersreferenties die u hebt gemaakt toen de virtuele machine is ingericht. De gebruikersnaam van de is in de [domein]\[gebruikersnaam] waarin het domein zich in de VM-computernaam, bijvoorbeeld ssrsnativecloud\testuser-indeling. Als u geen van HTTP gebruikmaakt**S**, verwijdert u de **s** in de URL. Zie de volgende sectie voor informatie over het maken van extra gebruikers op virtuele machine.
  
        https://ssrsnativecloud.cloudapp.net/Reports
* Blader naar de externe report server-URL van de lokale computer. Bijwerken van de DNS-naam in het volgende voorbeeld naar gelang van toepassing. Als u HTTPS niet gebruikt, verwijdert u de s in de URL.
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Gebruikers maken en toewijzen van rollen
Na het configureren en controleren van de rapportserver, is een algemene beheertaak voor het maken van een of meer gebruikers en gebruikers toewijzen aan rollen voor Reporting Services. Zie de volgende onderwerpen voor meer informatie:

* [Een lokale gebruikersaccount maken](https://technet.microsoft.com/library/cc770642.aspx)
* [Verleen toegang tot een rapportserver (Rapportbeheer)](https://msdn.microsoft.com/library/ms156034.aspx))
* [Maken en beheren van roltoewijzingen](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Om te maken en publiceren van rapporten voor de virtuele Machine van Azure
De volgende tabel ziet u enkele van de opties die beschikbaar zijn voor het publiceren van bestaande rapporten uit een on-premises computer naar de rapportserver op de Microsoft Azure virtuele Machine wordt gehost:

* **RS.exe script**: gebruik RS.exe script rapportitems uit en bestaande rapportserver kopiëren naar uw Microsoft Azure virtuele Machine. Zie voor meer informatie de sectie 'Native modus naar Native modus – Microsoft Azure virtuele Machine' in [voorbeeld Reporting Services-rs.exe Script voor het migreren van inhoud tussen rapportservers](https://msdn.microsoft.com/library/dn531017.aspx).
* **Report Builder**: de virtuele machine bevat het klikt u op-één keer een versie van Microsoft SQL Server Report Builder. Report builder de eerste keer starten op de virtuele machine:
  
  1. Start uw browser met beheerdersbevoegdheden.
  2. Blader naar Rapportbeheer op de virtuele machine en klik op **Report Builder** in het lint.
     
     Zie voor meer informatie [installeren, verwijderen en ondersteunende Report Builder](https://technet.microsoft.com/library/dd207038.aspx).
* **SQL Server Data Tools: VM**: als u de virtuele machine met SQL Server 2012 gemaakt, wordt SQL Server Data Tools op de virtuele machine is geïnstalleerd en kan worden gebruikt voor het maken van **Report Server-projecten** en rapporten op de virtuele machine. SQL Server Data Tools kunt u de rapporten publiceren naar de rapportserver op de virtuele machine.
  
    Als u de virtuele machine met SQL server 2014 gemaakt, kunt u SQL Server Data Tools - BI voor visual Studio installeren. Zie de volgende onderwerpen voor meer informatie:
  
  * [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)
  * [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)
  * [SQL Server Data Tools en SQL Server Business Intelligence (BI SSDT)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* **SQL Server Data Tools: Extern**: op uw lokale computer, kunt u een Reporting Services-project maken in SQL Server Data Tools met Reporting Services-rapporten. Het project verbinding maken met de URL van de webservice configureren.
  
    ![de Projecteigenschappen SSDT voor SQL Server Reporting Services-project](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* **Script gebruiken**: script gebruiken voor het rapport serverinhoud kopiëren. Zie voor meer informatie [voorbeeld Reporting Services-rs.exe Script voor het migreren van inhoud tussen rapportservers](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a>Als u geen van de virtuele machine gebruikmaakt kosten minimaliseren
> [!NOTE]
> Om te beperken kosten voor uw Azure Virtual Machines wanneer deze niet in gebruik, sluit u de virtuele machine van de Azure-portal. Als u de Windows-opties voor energiebeheer binnen een VM afsluiten van de virtuele machine gebruikt, er zijn nog steeds in rekening gebracht dezelfde mate voor de virtuele machine. Om de kosten, moet u het afsluiten van de virtuele machine in de Azure-portal. Als u de virtuele machine niet meer nodig hebt, moet u de virtuele machine en de bijbehorende VHD-bestanden om te voorkomen dat de opslagkosten te verwijderen. Zie voor meer informatie de sectie Veelgestelde vragen op [prijsinformatie voor virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Meer informatie
### <a name="resources"></a>Resources
* Zie voor soortgelijke inhoud gerelateerd aan een implementatie met één server van SQL Server Business Intelligence en SharePoint 2013, [gebruik Windows PowerShell voor het maken van een Azure-VM met SQL Server BI en SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).
* Zie voor soortgelijke inhoud gerelateerd aan een implementatie met meerdere servers van SQL Server Business Intelligence en SharePoint 2013, [implementeren SQL Server Business Intelligence in Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).
* Raadpleeg voor algemene informatie over implementaties van SQL Server Business Intelligence in Azure Virtual Machines, [SQL Server Business Intelligence in Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).
* Zie voor meer informatie over de kosten van Azure compute-kosten, het tabblad virtuele Machines van [Azure prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Inhoud van de community
* Zie voor stapsgewijze instructies voor het maken van een Reporting Services Native modus rapportserver zonder script te gebruiken, [hosten van SQL Reporting Service op Azure virtuele Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a>Koppelingen naar andere bronnen voor SQL Server in Azure VM 's
[SQL Server op Azure Virtual Machines-overzicht](../sql/virtual-machines-windows-sql-server-iaas-overview.md)


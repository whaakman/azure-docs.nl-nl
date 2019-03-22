---
title: SQL Server Business Intelligence | Microsoft Docs
description: Dit onderwerp maakt gebruik van resources die zijn gemaakt met het klassieke implementatiemodel, en beschrijft de beschikbare functies voor Business Intelligence (BI) voor SQL Server op Azure Virtual Machines (VM's) wordt uitgevoerd.
services: virtual-machines-windows
documentationcenter: na
author: markingmyname
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: c681e7a7-eeda-48aa-bc35-6277f4828244
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/30/2017
ms.author: maghan
ms.openlocfilehash: 29e851772e665b4130ee58b04c264d55bcd54523
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317779"
---
# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server Business Intelligence in Virtual Machines van Azure
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

De virtuele Machine van Microsoft Azure-galerie bevat installatiekopieën met SQL Server-installaties. De SQL Server-edities worden ondersteund in de galerie met installatiekopieën zijn dezelfde van installatiebestanden die u op de on-premises computers en virtuele machines installeren kunt. In dit onderwerp bevat een overzicht van de functies van SQL Server Business Intelligence (BI) geïnstalleerd op de afbeeldingen en van configuratiestappen die nodig zijn nadat een virtuele machine is ingericht. Dit onderwerp beschrijft ook ondersteunde implementatietopologieën voor BI-functies en best practices.

## <a name="license-considerations"></a>Licentie-overwegingen
Er zijn twee manieren om de licentie voor SQL Server in Microsoft Azure Virtual Machines:

1. Voorwaarden van mobiliteit die deel van de Software Assurance uitmaken. Zie voor meer informatie, [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/).
2. Betaal per uurtarief van Azure Virtual Machines met SQL Server is geïnstalleerd. Zie de sectie 'SQL Server' in [prijzen van virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Zie voor meer informatie over licentieverlening en huidige tarieven [virtuele Machines Veelgestelde vragen over licenties](https://azure.microsoft.com/pricing/licensing-faq/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server-installatiekopieën die beschikbaar in Azure Virtual Machine-galerie
De virtuele Machine van Microsoft Azure-galerie bevat diverse installatiekopieën met Microsoft SQL Server. De software is geïnstalleerd op de installatiekopieën van virtuele machines is afhankelijk van de versie van het besturingssysteem en de versie van SQL Server. De lijst met installatiekopieën die beschikbaar zijn in de galerie met virtuele Azure-machine regelmatig wordt gewijzigd.

<!--![SQL image in azure VM gallery](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)-->
![SQL-installatiekopie in Azure VM-galerie](./media/virtual-machines-windows-classic-ps-sql-bi/vm-sql-images.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) De volgende PowerShell-script retourneert de lijst met installatiekopieën van Azure die met 'SQL-Server' in de ImageName:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "Subscriptions" menu in Azure portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Zie de volgende onderwerpen voor meer informatie over de versies en functies die worden ondersteund door SQL Server:

* [SQL Server-edities](https://www.microsoft.com/sql-server/sql-server-2017-editions)
* [Functies die worden ondersteund door de verschillende edities van SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>BI-functies die in de galerie met virtuele Machine-installatiekopieën van SQL Server is geïnstalleerd
De volgende tabel geeft een overzicht van de geïnstalleerd op de algemene Microsoft Azure Virtual Machine-afbeeldingen voor SQL Server Business Intelligence-functies:

* SQL Server 2016 SP1 Enterprise
* SQL Server 2016 SP1 Standard
* SQL Server 2014 SP2 Enterprise
* SQL Server 2014 SP2 Standard
* SQL Server 2012 SP3 Enterprise
* SQL Server 2012 SP3 Standard

| Functie van SQL Server BI | Geïnstalleerd op de image z galerie | Opmerkingen |
| --- | --- | --- |
| **Native modus van Reporting Services** |Ja |Geïnstalleerd, maar moet worden geconfigureerd, met inbegrip van de URL van report manager. Zie de sectie [Reporting Services configureren](#configure-reporting-services). |
| **Reporting Services SharePoint-modus** |Nee |De installatiekopie van de Microsoft Azure virtuele Machine-galerie bevat geen SharePoint- of SharePoint bestanden voor installatie. <sup>1</sup> |
| **Analysis Services Multidimensional en Data mining (OLAP)** |Ja |Geïnstalleerd en geconfigureerd als het standaardexemplaar van de Analysis Services |
| **Tabellaire analyseservices** |Nee |Ondersteund in SQL Server 2012, is 2014 en 2016-installatiekopieën, maar het niet standaard geïnstalleerd. Een ander exemplaar van Analysis Services installeren. Zie de sectie andere SQL Server-Services en functies in dit onderwerp installeren. |
| **Analysis Services Power Pivot voor SharePoint** |Nee |De installatiekopie van de Microsoft Azure virtuele Machine-galerie bevat geen SharePoint- of SharePoint bestanden voor installatie. <sup>1</sup> |

<sup>1</sup> Zie voor meer informatie over SharePoint en Azure virtual machines, [Microsoft Azure-architecturen voor SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) en [SharePoint-implementatie op Microsoft Azure Virtual Machines](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Voer de volgende PowerShell-opdracht voor een lijst van geïnstalleerde services die 'SQL' in de naam van de service bevatten.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Algemene aanbevelingen en Best Practices
* De minimaal aanbevolen grootte voor een virtuele machine is **A3** bij het gebruik van SQL Server Enterprise Edition. De **A4** grootte van virtuele machine wordt aanbevolen voor implementaties van SQL Server BI van Analysis Services en Reporting Services.
  
    Zie voor meer informatie over de huidige VM-grootten [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Er is een best practice voor Schijfbeheer voor het opslaan van gegevens, logboeken en back-upbestanden op stations dan **C**: en **D**:. Maak bijvoorbeeld gegevensschijven **E**: en **F**:.
  
  * Het beleid voor het standaardstation voor caching station **C**: is niet optimaal voor het werken met gegevens.
  * De **D**: station is een tijdelijke station dat hoofdzakelijk voor het wisselbestand gebruikt wordt. De **D**: station is niet permanent opgeslagen en wordt niet opgeslagen in blob-opslag. Het formaat van beheertaken, zoals een wijziging in de virtuele machine opnieuw instellen van de **D**: station. Het verdient **niet** gebruiken de **D**: station voor databasebestanden, met inbegrip van tempdb.
    
    Zie voor meer informatie over het maken en koppelen van schijven [hoe u een gegevensschijf koppelen aan een virtuele Machine](../classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Stoppen of verwijderen van services die u niet van plan bent te gebruiken. Voor bijvoorbeeld als de virtuele machine wordt alleen gebruikt voor Reporting Services stoppen of verwijderen van Analysis Services en SQL Server Integration Services. De volgende afbeelding is een voorbeeld van de services die standaard worden gestart.
  
    ![SQL Server-services](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)
  
  > [!NOTE]
  > De SQL Server database engine is vereist in de ondersteunde scenario's voor BI. De database-engine is in een enkele server VM-topologie te worden uitgevoerd op dezelfde virtuele machine vereist.
  
    Zie de volgende onderwerpen voor meer informatie: [Verwijderen van Reporting Services](https://msdn.microsoft.com/library/hh479745.aspx) en [verwijderen van een exemplaar van analyseservices](https://msdn.microsoft.com/library/ms143687.aspx).
* Controleer **Windows Update** voor nieuwe 'belangrijke updates'. De Microsoft Azure Virtual Machine-installatiekopieën worden regelmatig vernieuwd. echter belangrijke updates beschikbaar zijn vanuit kunnen worden **Windows Update** nadat de VM-installatiekopie voor het laatst is vernieuwd.

## <a name="example-deployment-topologies"></a>Voorbeeld van de implementatietopologieën
Hieronder vindt u Voorbeeldimplementaties die gebruikmaken van Microsoft Azure Virtual Machines. De topologieën in deze diagrammen zijn slechts enkele van de mogelijke topologieën die kunt u met de functies van SQL Server BI en Microsoft Azure Virtual Machines.

### <a name="single-virtual-machine"></a>Eén virtuele machine
Analysis Services, Reporting Services, SQL Server Database Engine en gegevensbronnen op een enkele virtuele machine.

![BI iaas-scenario met 1 virtuele machine](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Twee virtuele Machines
* Analyseservices, Reporting Services en de SQL Server Database Engine op een enkele virtuele machine. Deze implementatie omvat de report server-databases.
* Gegevensbronnen op een tweede virtuele machine. De tweede virtuele machine bevat Database-Engine van SQL Server als een gegevensbron.

![BI iaas-scenario met 2 virtuele machines](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Gemengde Azure-gegevens in Azure SQL database
* Analyseservices, Reporting Services en de SQL Server Database Engine op een enkele virtuele machine. Deze implementatie omvat de report server-databases.
* Gegevensbron is Azure SQL-database.

![virtuele machine BI iaas-scenario's en SQL als gegevensbron](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybride-gegevens on-premises
* In deze voorbeeldimplementatie Analysis Services, Reporting Services en de SQL Server Database Engine worden uitgevoerd op een enkele virtuele machine. De virtuele machine als host fungeert voor de report server-databases. De virtuele machine is gekoppeld aan een on-premises domein via virtuele netwerken van Azure, of enige andere VPN-tunneling oplossing.
* De gegevensbron zich on-premises.

![BI iaas-scenario's voor vm- en on-premises gegevensbronnen](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Configuratie van de Native modus van Reporting Services
De installatiekopie van de galerie met virtuele machine voor SQL Server bevat Native modus Reporting Services is geïnstalleerd, maar de report server is niet geconfigureerd. De stappen in deze sectie configureert de Reporting Services-rapportserver. Zie voor meer informatie over het configureren van de Native modus Reporting Services, [installeren Reporting Services Native Mode Report Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

> [!NOTE]
> Zie voor soortgelijke inhoud die gebruikmaakt van Windows PowerShell-scripts voor het configureren van de rapportserver, [PowerShell gebruiken om u te maken van een Azure VM met een Native Mode Report Server](../classic/ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Verbinding maken met de virtuele Machine en de Reporting Services Configuration Manager starten
Er zijn twee algemene werkstromen om verbinding te maken met een virtuele Machine in Azure:

* Verbinding maken in het, klikt u op de naam van de virtuele machine en klik vervolgens op **Connect**. Een verbinding met extern bureaublad wordt geopend en de naam van de computer wordt automatisch ingevuld.
  
    ![verbinding maken met virtuele azure-machine](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)
* Verbinding maken met de virtuele machine met Windows verbinding met extern bureaublad. In de gebruikersinterface van de extern bureaublad:
  
  1. Type de **cloudservicenaam** als de naam van de computer.
  2. Type dubbele punt (:) en de openbare-poortnummer dat is geconfigureerd voor de TCP-eindpunt voor extern bureaublad.
     
      Myservice.cloudapp.net:63133
     
      Zie voor meer informatie, [wat is een cloudservice?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).


**Reporting Services Configuration Manager starten**

In **Windows Server 2012/2016**:

1. Uit de **Start** scherm, typt u **Reporting Services** voor een overzicht van Apps.
2. Met de rechtermuisknop op **Reporting Services Configuration Manager** en klikt u op **als Administrator uitvoeren**.

In **Windows Server 2008 R2**:

1. Klik op **Start**, en klik vervolgens op **alle programma's**.
2. Klik op **Microsoft SQL Server 2016**.
3. Klik op **configuratiehulpprogramma's**.
4. Met de rechtermuisknop op **Reporting Services Configuration Manager** en klikt u op **als Administrator uitvoeren**.

Of:

1. Klik op **Start**.
2. In de **programma's en bestanden zoeken** type dialoogvenster **reporting services**. Als de virtuele machine wordt uitgevoerd Windows Server 2012, typt u **reporting services** op het scherm Start van Windows Server 2012.
3. Met de rechtermuisknop op **Reporting Services Configuration Manager** en klikt u op **als Administrator uitvoeren**.
   
    ![zoeken naar SQL Server Reporting Services configuration manager](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Reporting Services configureren
**Service-account en webservice-URL:**

1. Controleer of de **servernaam** is de naam van de lokale server en klik op **Connect**.
2. Houd er rekening mee de blanco **Report Server-databasenaam**. De database wordt gemaakt wanneer de configuratie is voltooid.
3. Controleer of de **Rapportserverstatus** is **gestart**. Als u controleren of de service in Windows Server Manager wilt, de service is de **SQL Server Reporting Services** Windows-Service.
4. Klik op **serviceaccount** en indien nodig het account wijzigen. Als de virtuele machine wordt gebruikt in een niet-gekoppelde domeinomgeving, de ingebouwde **ReportServer** account voldoende is. Zie voor meer informatie over het serviceaccount [serviceaccount](https://msdn.microsoft.com/library/ms189964.aspx).
5. Klik op **webservice-URL** in het linkerdeelvenster.
6. Klik op **toepassen** het configureren van de standaardwaarden.
7. Houd er rekening mee de **Server webservice-URL's rapporteren**. Opmerking: de standaard-TCP-poort 80 en maakt deel uit van de URL. In een latere stap maakt u een Microsoft Azure Virtual Machine-eindpunt voor de poort.
8. In de **resultaten** deelvenster, Controleer of de acties met succes is voltooid.

**Database:**

1. Klik op **Database** in het linkerdeelvenster.
2. Klik op **-Database wijzigen**.
3. Controleer of **maken van een nieuwe rapportserverdatabase** is geselecteerd en klik op volgende.
4. Controleer of **servernaam** en klikt u op **testverbinding**.
5. Als het resultaat **de testverbinding is geslaagd**, klikt u op **OK** en klik vervolgens op **volgende**.
6. Houd er rekening mee de databasenaam is **ReportServer** en de **mode Report Server** is **systeemeigen** klikt u vervolgens op **volgende**.
7. Klik op **volgende** op de **referenties** pagina.
8. Klik op **volgende** op de **samenvatting** pagina.
9. Klik op **volgende** op de **voltooien en het** pagina.

**Portal-URL of URL van Report Manager op het web voor 2012 en 2014:**

1. Klik op **webportal-URL**, of **URL Report Manager** voor 2014 en 2012, in het linkerdeelvenster.
2. Klik op **Toepassen**.
3. In de **resultaten** deelvenster, Controleer of de acties met succes is voltooid.
4. Klik op **afsluiten**.

Zie voor meer informatie over de report servermachtigingen [machtigingen verlenen voor een rapportserver in Native modus](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Blader naar de lokale Report Manager
Als u wilt controleren of de configuratie, blader naar Rapportbeheer op de virtuele machine.

1. Start Internet Explorer met administrator-bevoegdheden op de virtuele machine.
2. Blader naar http: \/ /localhost/rapporten op de virtuele machine.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Verbinding maken met externe webportal of Report Manager voor 2014 en 2012
Als u wilt verbinding maken met de webportal of Report Manager voor 2014 en 2012 op de virtuele machine vanaf een externe computer, maakt u een nieuwe virtuele machine TCP-eindpunt. Standaard is de rapportserver luistert naar HTTP-aanvragen op **poort 80**. Als u de rapport-URL's voor het gebruik van een andere poort configureren, moet u dat poortnummer opgeven in de volgende instructies.

1. Een eindpunt maken voor de virtuele Machine van de TCP-poort 80. Zie voor meer informatie, de [eindpunten van virtuele machines en Firewall-poorten](#virtual-machine-endpoints-and-firewall-ports) sectie in dit document.
2. Open poort 80 in de firewall van de virtuele machine.
3. Blader naar de web-portal of met behulp van Azure Virtual Machine manager rapporteren **DNS-naam** als de naam van de server in de URL. Bijvoorbeeld:
   
    **Rapportserver**: http://uebi.cloudapp.net/reportserver  **Webportal**: http://uebi.cloudapp.net/reports
   
    [Een Firewall configureren voor toegang tot Report Server](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Maken en publiceren van rapporten met de Azure-Machine
De volgende tabel geeft een overzicht van enkele van de opties die beschikbaar zijn voor het publiceren van bestaande rapporten van een on-premises computer naar de rapportserver die wordt gehost op Microsoft Azure-Machine:

* **Report Builder**: De virtuele machine bevat de Klik-eenmaal versie van Microsoft SQL Server Report Builder voor SQL 2014 en 2012. Report builder de eerste keer starten op de virtuele machine met SQL 2016:
  
  1. Start uw browser met beheerdersbevoegdheden.
  2. Blader naar de web-portal, op de virtuele machine, en selecteer de **downloaden** pictogram in de rechterbovenhoek.
  3. Selecteer **Report Builder**.
     
     Zie voor meer informatie, [Report Builder starten](https://msdn.microsoft.com/library/ms159221.aspx).
* **SQL Server Data Tools**: VIRTUELE MACHINE:  SQL Server Data Tools is geïnstalleerd op de virtuele machine en kan worden gebruikt voor het maken van **Report Server-projecten** en rapporten op de virtuele machine. SQL Server Data Tools kunt u de rapporten publiceren naar de rapportserver op de virtuele machine.
* **SQL Server Data Tools: Externe**:  Op uw lokale computer door een Reporting Services-project te maken in SQL Server Data Tools met Reporting Services-rapporten. Het project verbinding maken met de URL van de webservice configureren.
  
    ![Eigenschappen van SSDT voor SQL Server Reporting Services-project](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)
* Maak een. Harde schijf VHD die rapporten bevat en vervolgens uploaden en koppelen van het station.
  
  1. Maak een. Harde schijf VHD op uw lokale computer met uw rapporten.
  2. Maken en een beheercertificaat te installeren.
  3. Het VHD-bestand uploaden naar Azure met behulp van de cmdlet Add-AzureVHD [maken en uploaden van een Windows Server-VHD voor Azure](../classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  4. De schijf koppelen aan de virtuele machine.

## <a name="install-other-sql-server-services-and-features"></a>Andere SQL Server-Services en onderdelen installeren
Voer de wizard setup van SQL server voor het installeren van extra SQL Server-services, zoals Analysis Services in de modus tabellair. De setup-bestanden zijn op de lokale schijf van de virtuele machine.

1. Klik op **Start** en klik vervolgens op **alle programma's**.
2. Klik op **Microsoft SQL Server 2016**, **Microsoft SQL Server 2014** of **Microsoft SQL Server 2012** en klik vervolgens op **configuratiehulpprogramma's** .
3. Klik op **Installatiecentrum van SQL Server**.

Of C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe of C:\SQLServer_11.0_full\setup.exe uitvoeren

> [!NOTE]
> De eerste keer dat u de installatie van SQL Server uitvoeren meer setup-bestanden kunnen worden gedownload en vereisen een opnieuw opstarten van de virtuele machine en het opnieuw opstarten van SQL Server setup.
> 
> Als u nodig hebt om aan te passen herhaaldelijk de installatiekopie van de Microsoft Azure virtuele Machine hebt geselecteerd, kunt u overwegen uw eigen SQL Server-installatiekopie. Analysis Services SysPrep-functionaliteit is ingeschakeld met SQL Server 2012 SP1 CU2. Zie voor meer informatie, [overwegingen voor het installeren van SQL Server met behulp van SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) en [Sysprep-ondersteuning voor serverfuncties](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).
> 
> 

### <a name="to-install-analysis-services-tabular-mode"></a>Voor het installeren van de Tabellaire modus van Analysis Services
De stappen in deze sectie **samenvatten** de installatie van de modus tabellair Analysis Services. Zie de volgende onderwerpen voor meer informatie:

* [Analyseservices in de modus Tabellair installeren](https://msdn.microsoft.com/library/hh231722.aspx)
* [Tabellaire modellen (Adventure Works-zelfstudie)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Tabellaire Analysis Services-modus installeren:**

1. Klik in de installatiewizard van SQL Server op **installatie** in het linkerdeelvenster en klik vervolgens op **zelfstandige installatie van nieuwe SQL server of functies toevoegen aan een bestaande installatie**.
   
   * Als u ziet de **bladeren naar map**, bladert u naar c:\SQLServer_13.0_full, c:\SQLServer_12.0_full of c:\SQLServer_11.0_full en klik vervolgens op **Ok**.
2. Klik op **volgende** op de pagina product updates.
3. Op de **installatietype** weergeeft, schakelt **voert u een nieuwe installatie van SQL Server** en klikt u op **volgende**.
4. Op de **Setup-rol** pagina, klikt u op **installatie van SQL Server-functies**.
5. Op de **Functieselectie** pagina, klikt u op **Analysis Services**.
6. Op de **Exemplaarconfiguratie** pagina, typ een beschrijvende naam, zoals **in tabelvorm** in **exemplaar met de naam** en **exemplaar-Id** tekstvakken .
7. Op de **Analysis Services-configuratie** weergeeft, schakelt **de modus Tabellair**. De huidige gebruiker toevoegen aan de lijst met beheerdersmachtigingen.
8. Voltooien en sluit de installatiewizard van SQL Server.

## <a name="analysis-services-configuration"></a>Configuratie van Analysis Services
### <a name="remote-access-to-analysis-services-server"></a>Externe toegang tot Analysis Services-Server
Analysis Services-server ondersteunt alleen windows-verificatie. Voor toegang tot Analysis Services op afstand vanuit clienttoepassingen, zoals SQL Server Management Studio of SQL Server Data Tools, moet de virtuele machine worden toegevoegd aan uw lokale domein, met behulp van virtuele netwerken van Azure. Zie voor meer informatie, [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

Een **standaardexemplaar** van Analysis Services luistert op TCP-poort **2383**. Open de poort in de firewall van de virtuele machines. Een geclusterde benoemd exemplaar van Analysis Services ook luistert op poort **2383**.

Voor een **benoemd exemplaar** van Analysis Services, de SQL Server Browser-service is vereist voor het beheren van toegang tot poort. De standaardconfiguratie van SQL Server Browser wordt poort **2382**.

Open poort in de firewall voor virtuele machines, **2382** en maak een statische benoemd Analysis Services-exemplaar poort.

1. Om te controleren of de poorten die zich al in gebruik is op de virtuele machine en welk proces met behulp van de poorten, voer de volgende opdracht uit met beheerdersbevoegdheden:
   
        netstat /ao
2. Gebruik SQL Server Management Studio te maken van een statische Analysis Services met de naam exemplaar poort door bij te werken 'Poort' waarde op in tabelvorm zoals algemene eigenschappen-exemplaar. Zie voor meer informatie, de 'een vaste poort gebruiken voor een standaard of benoemd exemplaar' [de Windows Firewall configureren om te leiden Analysis Services Access](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).
3. Het tabellaire Analysis Services-service-exemplaar opnieuw opstarten.

Zie voor meer informatie, de **eindpunten van virtuele machines en Firewall-poorten** sectie in dit document.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Eindpunten van virtuele machines en Firewall-poorten
In deze sectie bevat een overzicht van Microsoft Azure-eindpunten voor virtuele Machine te maken en poorten openen in de virtuele machine-firewalls. De volgende tabel geeft een overzicht van de **TCP** eindpunten voor maken en de poorten openen in de firewall van de virtuele machines.

* Als u van een enkele virtuele machine gebruikmaakt en de volgende twee items voldaan wordt, u hoeft niet te maken van VM-eindpunten en u niet wilt dat de poorten openen in de firewall op de virtuele machine.
  
  * U verbinding geen op afstand met de SQL Server-functies op de virtuele machine. Tot stand brengen van een extern bureaublad verbinding met de virtuele machine en toegang tot de functies van SQL Server lokaal op de virtuele machine wordt niet beschouwd als een externe verbinding met de SQL Server-functies.
  * U moet de VM niet toevoegen aan een on-premises domein via virtuele netwerken van Azure of een andere oplossing voor VPN-tunneling.
* Als de virtuele machine is niet toegevoegd aan een domein, maar u zich op afstand wilt verbinden met de functies van SQL Server op virtuele machine:
  
  * De poorten openen in de firewall op de virtuele machine.
  * Eindpunten van virtuele machines voor de vermelde poorten (*) maken.
* Als de virtuele machine is toegevoegd aan een domein met behulp van een VPN-tunnel, zoals virtuele netwerken van Azure, klikt u vervolgens zijn de eindpunten niet vereist. Maar de poorten openen in de firewall op de virtuele machine.
  
  | Poort | Type | Description |
  | --- | --- | --- |
  | **80** |TCP |Externe toegang (*)-rapportserver. |
  | **1433** |TCP |SQL Server Management Studio (*). |
  | **1434** |UDP |SQL Server Browser. Dit is nodig wanneer de virtuele machine in toegevoegd aan een domein. |
  | **2382** |TCP |SQL Server Browser. |
  | **2383** |TCP |Standaardexemplaar van SQL Server Analysis Services en geclusterde benoemde exemplaren. |
  | **door de gebruiker gedefinieerde** |TCP |Maken van een statische benoemd Analysis Services-exemplaar poort voor een poortnummer dat die u kiest en vervolgens het poortnummer in de firewall te deblokkeren. |

Zie de volgende onderwerpen voor meer informatie over het maken van eindpunten:

* Eindpunten maken:[uitleg over de instellingen van eindpunten aan een virtuele Machine](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* SQL Server: Zie de sectie 'Volledige configuratie van de stappen om verbinding maken met de virtuele machine met SQL Server Management Studio' van [inrichten van een SQL Server-Machine in Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Het volgende diagram illustreert de poorten te openen in de firewall van de VM zodat externe toegang tot functies en onderdelen op de virtuele machine.

![poorten openen voor bi-toepassingen in Azure VM 's](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Resources
* Het ondersteuningsbeleid voor Microsoft-serversoftware in de virtuele Machine van Azure-omgeving gebruikt. Het volgende onderwerp bevat een overzicht van ondersteuning voor functies, zoals BitLocker, Failover Clustering en netwerktaakverdeling. [Ondersteuning van Microsoft-serversoftware voor Azure Virtual Machines](https://support.microsoft.com/kb/2721672).
* [SQL Server op virtuele Machines van Azure-overzicht](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Virtuele machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Inrichten van een SQL Server-Machine in Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Een gegevensschijf koppelen aan een virtuele Machine](../classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Een Database migreren naar SQL Server op een Azure-VM](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json)
* [Bepalen van de servermodus van een Analysis Services-exemplaar](https://msdn.microsoft.com/library/gg471594.aspx)
* [Multidimensionale modellen (Adventure Works-zelfstudie)](https://technet.microsoft.com/library/ms170208.aspx)
* [Documentatiecentrum van Azure](https://azure.microsoft.com/documentation/)
* [Met behulp van Power BI in een hybride omgeving](https://msdn.microsoft.com/library/dn798994.aspx)

> [!NOTE]
> [Indienen van feedback en neem contact op met gegevens via Microsoft SQL Server verbinding maken](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Community-inhoud
* [Azure SQL Database-beheer met PowerShell](https://azure.microsoft.com/blog/windows-azure-sql-database-management-with-powershell/)


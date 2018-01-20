---
title: SQL Server Business Intelligence | Microsoft Docs
description: Dit onderwerp maakt gebruik van resources die zijn gemaakt met het klassieke implementatiemodel en beschrijft de beschikbare functies voor Business Intelligence (BI) voor SQL Server op Azure Virtual Machines (VM's) wordt uitgevoerd.
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
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
ms.author: asaxton
ms.openlocfilehash: a010e60df2d86d2b1cc923b427aa7d7452f58089
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server Business Intelligence in Virtual Machines van Azure
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

De virtuele Machine in Microsoft Azure-galerie bevat afbeeldingen met SQL Server-installaties. De SQL Server-edities ondersteund in de galerie met installatiekopieën zijn dezelfde van installatiebestanden die u voor lokale computers en virtuele machines installeren kunt. In dit onderwerp bevat een overzicht van de SQL Server Business Intelligence (BI)-functies die zijn geïnstalleerd op de afbeeldingen en configuratiestappen vereist nadat een virtuele machine is ingericht. Dit onderwerp beschrijft ook ondersteunde implementatietopologieën voor BI-functies en best practices.

## <a name="license-considerations"></a>Licentie-overwegingen
Er zijn twee manieren aan licentie SQL Server in Microsoft Azure Virtual Machines:

1. License mobility voordelen die deel van de Software Assurance uitmaken. Zie voor meer informatie [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/).
2. Betalen verwerkingssnelheid per uur van Azure Virtual Machines waarop SQL Server is geïnstalleerd. Zie de sectie 'SQL Server' in [prijzen van virtuele Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Zie voor meer informatie over licentieverlening en huidige tarieven [virtuele Machines Veelgestelde vragen over licenties](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server-installatiekopieën beschikbaar in Azure virtuele Machine-galerie
De virtuele Machine in Microsoft Azure-galerie bevat diverse installatiekopieën met Microsoft SQL Server. De software is geïnstalleerd op de installatiekopieën van virtuele machines varieert op basis van de versie van het besturingssysteem en de versie van SQL Server. De lijst met afbeeldingen die beschikbaar zijn in de virtuele machine van Azure-galerie vaak wordt gewijzigd.

<!--![SQL image in azure VM gallery](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)-->
![SQL-installatiekopie in de galerie van Azure VM](./media/virtual-machines-windows-classic-ps-sql-bi/vm-sql-images.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) De volgende PowerShell-script retourneert de lijst met Azure afbeeldingen die 'SQL-Server' in de ImageName bevatten:

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

Zie de volgende onderwerpen voor meer informatie over edities en functies die worden ondersteund door SQL Server:

* [SQL Server Editions](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)
* [Functies die worden ondersteund door de verschillende edities van SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>BI functies worden geïnstalleerd op de installatiekopieën galerie met virtuele Machine van SQL Server
De volgende tabel geeft een overzicht van de geïnstalleerd op de algemene Microsoft Azure Virtual Machine-afbeeldingen voor SQL Server Business Intelligence-functies:

* SQL Server 2016 SP1 Enterprise
* SQL Server 2016 SP1 Standard
* SQL Server 2014 SP2 Enterprise
* SQL Server 2014 SP2 Standard
* SQL Server 2012 SP3 Enterprise
* SQL Server 2012 SP3 Standard

| SQL Server BI functie | Geïnstalleerd op de installatiekopie van het galerie | Opmerkingen |
| --- | --- | --- |
| **Native modus van Reporting Services** |Ja |Geïnstalleerd, maar moet worden geconfigureerd, inclusief de URL van report manager. Zie de sectie [Reporting Services configureren](#configure-reporting-services). |
| **Reporting Services-modus SharePoint wordt uitgevoerd** |Nee |De installatiekopie van de Microsoft Azure virtuele Machine-galerie bevat geen SharePoint- of SharePoint bestanden voor installatie. <sup>1</sup> |
| **Analysis Services multidimensionale en Data mining (OLAP)** |Ja |Geïnstalleerd en geconfigureerd als het standaardexemplaar van de Analysis Services |
| **Analysis Services Tabular** |Nee |Ondersteund in SQL Server 2012, is 2014 en 2016 afbeeldingen, maar het niet standaard geïnstalleerd. Een ander exemplaar van Analysis Services installeren. Zie de sectie andere SQL Server-Services en functies in dit onderwerp installeren. |
| **Analysis Services Power Pivot voor SharePoint** |Nee |De installatiekopie van de Microsoft Azure virtuele Machine-galerie bevat geen SharePoint- of SharePoint bestanden voor installatie. <sup>1</sup> |

<sup>1</sup> Zie voor meer informatie over SharePoint en virtuele machines in Azure [Microsoft Azure architecturen voor SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) en [SharePoint-implementatie in Microsoft Azure Virtual Machines](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Voer de volgende PowerShell-opdracht voor een lijst met geïnstalleerde services die 'SQL' in de servicenaam bevatten.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Algemene aanbevelingen en Best Practices
* De minimaal aanbevolen grootte voor een virtuele machine is **A3** bij gebruik van SQL Server Enterprise Edition. De **A4** grootte van virtuele machine wordt aanbevolen voor implementaties van SQL Server BI van Analysis Services en Reporting Services.
  
    Zie voor informatie over de huidige VM-grootten, [grootten van virtuele machines voor Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Er is een best practice voor Schijfbeheer voor het opslaan van gegevens, het logboek en back-upbestanden op stations dan **C**: en **D**:. Maak bijvoorbeeld gegevensschijven **E**: en **F**:.
  
  * Het cachebeleid voor het standaardstation station **C**: is niet optimaal voor het werken met gegevens.
  * De **D**: station is een tijdelijke station dat hoofdzakelijk voor het wisselbestand gebruikt wordt. De **D**: station is niet persistent en wordt niet opgeslagen in blob-opslag. Het formaat van beheertaken, zoals een wijziging in de virtuele machine opnieuw instellen van de **D**: station. Het is raadzaam te **niet** gebruiken de **D**: station voor databasebestanden, met inbegrip van tempdb.
    
    Zie voor meer informatie over het maken en koppelen van schijven [hoe een gegevensschijf koppelen aan een virtuele Machine](../classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Stop of verwijder services die u niet wilt gebruiken. Voor bijvoorbeeld als de virtuele machine alleen wordt gebruikt voor Reporting Services stoppen of verwijderen van Analysis Services en SQL Server Integration Services. De volgende afbeelding is een voorbeeld van de services die standaard worden gestart.
  
    ![SQL Server-services](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)
  
  > [!NOTE]
  > De SQL Server database engine is vereist in de ondersteunde BI-scenario's. De database-engine is in een enkele server VM topologie te worden uitgevoerd op dezelfde virtuele machine vereist.
  
    Zie voor meer informatie het volgende: [Reporting Services verwijderen](https://msdn.microsoft.com/library/hh479745.aspx) en [verwijderen van een exemplaar van Analysis Services](https://msdn.microsoft.com/library/ms143687.aspx).
* Controleer **Windows Update** voor nieuwe 'belangrijke updates'. Installatiekopieën van Microsoft Azure virtuele machines worden regelmatig vernieuwd; echter belangrijke updates, worden mogelijk beschikbaar is via **Windows Update** nadat de VM-installatiekopie voor het laatst is vernieuwd.

## <a name="example-deployment-topologies"></a>Voorbeeld van de implementatietopologieën
Hieronder vindt u Voorbeeldimplementaties die gebruikmaken van Microsoft Azure Virtual Machines. De topologieën in deze diagrammen zijn slechts enkele van de mogelijke topologieën die kunt u met SQL Server BI functies en Microsoft Azure Virtual Machines.

### <a name="single-virtual-machine"></a>Eén virtuele Machine
Analysis Services, Reporting Services, SQL Server Database Engine en gegevensbronnen op een enkele virtuele machine.

![BI Standards-scenario met 1 virtuele machine](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Twee virtuele Machines
* Analyseservices, Reporting Services en de SQL Server Database Engine op een enkele virtuele machine. Deze implementatie bevat de report server-databases.
* Gegevensbronnen op een tweede virtuele machine. De tweede VM bevat Database-Engine van SQL Server als een gegevensbron.

![BI iaas-scenario met 2 virtuele machines](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Gemengde Azure-gegevens op Azure SQL-database
* Analyseservices, Reporting Services en de SQL Server Database Engine op een enkele virtuele machine. Deze implementatie bevat de report server-databases.
* Gegevensbron is Azure SQL-database.

![virtuele machine BI iaas-scenario's en AzureSQL als gegevensbron](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybride: gegevens die lokaal
* In deze voorbeeldimplementatie Analysis Services Reporting Services en de SQL Server Database Engine uitgevoerd op een enkele virtuele machine. De virtuele machine fungeert als host van de report server-databases. De virtuele machine wordt toegevoegd aan een lokaal domein via Azure virtuele netwerken of enige andere VPN-tunneling oplossing.
* Gegevensbron is lokaal.

![BI iaas-scenario's voor vm- en on-premises gegevensbronnen](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Configuratie van de Native modus Reporting Services
De installatiekopie van de virtuele machine galerie voor SQL Server bevat Reporting Services Native modus geïnstalleerd, maar de rapportserver is niet geconfigureerd. De stappen in deze sectie configureert de Reporting Services report server. Zie voor meer informatie over het configureren van Reporting Services Native modus, [installeren Reporting Services Native modus Report Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

> [!NOTE]
> Zie voor soortgelijke inhoud die gebruikmaakt van Windows PowerShell-scripts voor het configureren van de rapportserver, [Gebruik PowerShell voor het maken van een Azure VM met een Native modus Report Server](../classic/ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Verbinding maken met de virtuele Machine en Start u Reporting Services Configuration Manager
Er zijn twee algemene werkstromen om verbinding te maken voor een virtuele Machine van Azure:

* Verbinding maken in het op de naam van de virtuele machine en klik vervolgens op **Connect**. Een verbinding met extern bureaublad wordt geopend en de naam van de computer wordt automatisch ingevuld.
  
    ![verbinding maken met virtuele machine van azure](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)
* Verbinding maken met de virtuele machine met Windows verbinding met extern bureaublad. In de gebruikersinterface van de extern bureaublad:
  
  1. Typ de **cloudservicenaam** als de naam van de computer.
  2. Type dubbele punt (:) en het openbare-poortnummer dat is geconfigureerd voor de TCP-eindpunt voor extern bureaublad.
     
      Myservice.cloudapp.net:63133
     
      Zie voor meer informatie [wat is een cloudservice?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).


**Start u Reporting Services Configuration Manager**

In **Windows Server 2012/2016**:

1. Van de **Start** Typ **Reporting Services** voor een overzicht van Apps.
2. Met de rechtermuisknop op **Reporting Services Configuration Manager** en klik op **als Administrator uitvoeren**.

In **Windows Server 2008 R2**:

1. Klik op **Start**, en klik vervolgens op **alle programma's**.
2. Klik op **Microsoft SQL Server 2016**.
3. Klik op **configuratiehulpprogramma's**.
4. Met de rechtermuisknop op **Reporting Services Configuration Manager** en klik op **als Administrator uitvoeren**.

Of:

1. Klik op **Start**.
2. In de **programma's en bestanden zoeken** type dialoogvenster **reporting services**. Als de virtuele machine wordt uitgevoerd van Windows Server 2012, typt u **reporting services** op het scherm van Windows Server 2012 worden gestart.
3. Met de rechtermuisknop op **Reporting Services Configuration Manager** en klik op **als Administrator uitvoeren**.
   
    ![zoeken naar ssrs configuration manager](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Reporting Services configureren
**Service-account en webservice-URL:**

1. Controleer of de **servernaam** is de naam van de lokale server en klik op **Connect**.
2. Houd er rekening mee het leeg **Report Server-databasenaam**. De database wordt gemaakt wanneer de configuratie is voltooid.
3. Controleer of de **Rapportserverstatus** is **gestart**. Als u wilt controleren of de service in Windows Server Manager, de service is de **SQL Server Reporting Services** Windows-Service.
4. Klik op **serviceaccount** en het account indien nodig wijzigen. Als de virtuele machine wordt gebruikt in een niet-gekoppelde domeinomgeving, de ingebouwde **ReportServer** account voldoende is. Zie voor meer informatie over het serviceaccount [serviceaccount](https://msdn.microsoft.com/library/ms189964.aspx).
5. Klik op **URL van webservice** in het linkerdeelvenster.
6. Klik op **toepassen** voor het configureren van de standaardwaarden.
7. Opmerking de **Server webservice-URL's rapporteren**. Noteer de standaard TCP-poort 80 en maakt deel uit van de URL. In een latere stap maakt u een Microsoft Azure Virtual Machine-eindpunt voor de poort.
8. In de **resultaten** deelvenster, Controleer of de acties met succes is voltooid.

**Database:**

1. Klik op **Database** in het linkerdeelvenster.
2. Klik op **Database wijzigen**.
3. Controleer of **maken van een nieuw report server-database** is geselecteerd en klik op volgende.
4. Controleer of **servernaam** en klik op **testverbinding**.
5. Als het resultaat **verbinding testen is voltooid**, klikt u op **OK** en klik vervolgens op **volgende**.
6. Noteer de databasenaam van de is **ReportServer** en de **Report Server-modus** is **systeemeigen** klikt u vervolgens op **volgende**.
7. Klik op **volgende** op de **referenties** pagina.
8. Klik op **volgende** op de **samenvatting** pagina.
9. Klik op **volgende** op de **navigatie- en eindtijd** pagina.

**Web-Portal-URL of URL van Report Manager voor 2012 en 2014:**

1. Klik op **Portal-URL voor webinhoud**, of **URL van Report Manager** voor 2014 en 2012 in het linkerdeelvenster.
2. Klik op **Toepassen**.
3. In de **resultaten** deelvenster, Controleer of de acties met succes is voltooid.
4. Klik op **afsluiten**.

Zie voor informatie over report servermachtigingen [machtigingen verlenen voor een Native modus rapportserver](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Blader naar de lokale Report Manager
Om te controleren of de configuratie, blader naar Rapportbeheer op de virtuele machine.

1. Start Internet Explorer met administrator-bevoegdheden op de virtuele machine.
2. Blader naar http://localhost/reports op de virtuele machine.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Om te verbinden met externe webportal of van Report Manager for 2014 en 2012
Als u wilt verbinding maken met de web-portal of Rapportbeheer voor 2014 en 2012 op de virtuele machine vanaf een externe computer, maakt u een nieuwe virtuele machine TCP-eindpunt. Standaard is de rapportserver luistert naar HTTP-aanvragen op **poort 80**. Als u de URL's report server voor het gebruik van een andere poort configureren, moet u dat poortnummer opgeven in de volgende instructies.

1. Maak een eindpunt voor de virtuele Machine van de TCP-poort 80. Zie voor meer informatie, de [eindpunten van de virtuele Machine en Firewall-poorten](#virtual-machine-endpoints-and-firewall-ports) sectie in dit document.
2. Open poort 80 in de firewall van de virtuele machine.
3. Blader naar de web-portal of met behulp van Azure Virtual Machine manager rapporteren **DNS-naam** als de naam van de server in de URL. Bijvoorbeeld:
   
    **Rapportserver**: http://uebi.cloudapp.net/reportserver **webportal**: http://uebi.cloudapp.net/reports
   
    [Een Firewall configureren voor toegang tot de Server van rapporten](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Om te maken en publiceren van rapporten voor de virtuele Machine van Azure
De volgende tabel ziet u enkele van de opties die beschikbaar zijn voor het publiceren van bestaande rapporten uit een on-premises computer naar de rapportserver op de Microsoft Azure virtuele Machine wordt gehost:

* **Report Builder**: de virtuele machine bevat het klikt u op-één keer een versie van Microsoft SQL Server Report Builder voor SQL 2014 en 2012. Report builder de eerste keer starten op de virtuele machine met SQL 2016:
  
  1. Start uw browser met beheerdersbevoegdheden.
  2. Blader naar de web-portal op de virtuele machine en selecteer de **downloaden** pictogram in de rechterbovenhoek.
  3. Selecteer **Report Builder**.
     
     Zie voor meer informatie [Start Report Builder](https://msdn.microsoft.com/library/ms159221.aspx).
* **SQL Server Data Tools**: VM: SQL Server Data Tools op de virtuele machine is geïnstalleerd en kan worden gebruikt voor het maken van **Report Server-projecten** en rapporten op de virtuele machine. SQL Server Data Tools kunt u de rapporten publiceren naar de rapportserver op de virtuele machine.
* **SQL Server Data Tools: Extern**: op uw lokale computer, kunt u een Reporting Services-project maken in SQL Server Data Tools met Reporting Services-rapporten. Het project verbinding maken met de URL van de webservice configureren.
  
    ![de Projecteigenschappen SSDT voor SQL Server Reporting Services-project](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)
* Maak een. Harde schijf VHD die rapporten bevat en vervolgens uploaden en het station koppelen.
  
  1. Maak een. VHD harde schijf op de lokale computer die uw rapporten bevat.
  2. Maak en installeer een beheercertificaat.
  3. Het VHD-bestand uploaden naar Azure met behulp van de cmdlet Add-AzureVHD [een Windows Server-VHD naar Azure maken en uploaden](../classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  4. De schijf koppelen aan de virtuele machine.

## <a name="install-other-sql-server-services-and-features"></a>Andere SQL Server-Services en onderdelen installeren
Voer de wizard setup van SQL server voor het installeren van aanvullende SQL Server-services, zoals Analysis Services in de modus tabellair. De setup-bestanden zijn op de lokale schijf van de virtuele machine.

1. Klik op **Start** en klik vervolgens op **alle programma's**.
2. Klik op **Microsoft SQL Server 2016**, **Microsoft SQL Server 2014** of **Microsoft SQL Server 2012** en klik vervolgens op **configuratiehulpprogramma's**.
3. Klik op **Installatiecentrum van SQL Server**.

Or run C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe or C:\SQLServer_11.0_full\setup.exe

> [!NOTE]
> De eerste keer dat u de installatie van SQL Server uitvoeren meer setup-bestanden kunnen worden gedownload en vereist een herstart van de virtuele machine en het opnieuw opstarten van SQL Server setup.
> 
> Als u moet herhaaldelijk aanpassen van de installatiekopie van de Microsoft Azure virtuele Machine hebt geselecteerd, kunt u uw eigen SQL Server-installatiekopie maken. Analysis Services SysPrep-functionaliteit is ingeschakeld met SQL Server 2012 SP1 CU2. Zie voor meer informatie [overwegingen voor het installeren van SQL Server met behulp van SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) en [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).
> 
> 

### <a name="to-install-analysis-services-tabular-mode"></a>Tabellaire modus van Analysis Services installeren
De stappen in deze sectie **samenvatten** de installatie van de modus tabellair Analysis Services. Zie de volgende onderwerpen voor meer informatie:

* [Analyseservices in de modus Tabellair installeren](https://msdn.microsoft.com/library/hh231722.aspx)
* [Tabellaire model (Adventure Works zelfstudie)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Tabellaire modus van Analysis Services installeren:**

1. Klik in de installatiewizard van SQL Server op **installatie** in het linkerdeelvenster en klik op **zelfstandige installatie van nieuwe SQL server of functies toevoegen aan een bestaande installatie**.
   
   * Als u ziet de **map**, blader naar c:\SQLServer_13.0_full, c:\SQLServer_12.0_full of c:\SQLServer_11.0_full en klik vervolgens op **Ok**.
2. Klik op **volgende** op de pagina product updates.
3. Op de **installatietype** pagina **voert u een nieuwe installatie van SQL Server** en klik op **volgende**.
4. Op de **Setup-rol** pagina, klikt u op **installatie van SQL Server-functies**.
5. Op de **Functieselectie** pagina, klikt u op **Analysis Services**.
6. Op de **Exemplaarconfiguratie** pagina, typ een beschrijvende naam, zoals **tabelvorm** in **exemplaar met de naam** en **exemplaar-Id** tekstvakken.
7. Op de **configuratie van Analysis Services** pagina **Tabellair**. De huidige gebruiker toevoegen aan de lijst met beheerdersmachtigingen.
8. Voltooien en sluit de installatiewizard van SQL Server.

## <a name="analysis-services-configuration"></a>Configuratie van Analysis Services
### <a name="remote-access-to-analysis-services-server"></a>Externe toegang tot Analysis Services-Server
Analysis Services-server ondersteunt alleen windows-verificatie. Voor toegang tot Analysis Services op afstand via clienttoepassingen, zoals SQL Server Management Studio of SQL Server Data Tools, wordt de virtuele machine moet worden toegevoegd aan uw lokale domein, met behulp van virtuele netwerken van Azure. Zie voor meer informatie, [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

Een **standaardexemplaar** luistert op TCP-poort van Analysis Services **2383**. Open de poort in de firewall van virtuele machines. Een geclusterde benoemd exemplaar van Analysis Services ook luistert op poort **2383**.

Voor een **benoemd exemplaar** van Analysis Services, de SQL Server Browser-service is vereist voor het beheren van toegang tot poort. De standaardconfiguratie van SQL Server Browser wordt poort **2382**.

Open in de firewall voor virtuele machines, poort **2382** en maak een statische Analysis Services met de naam exemplaar poort.

1. Om te controleren of de poorten die al in gebruik is op de virtuele machine en welk proces gebruikmaakt van de poorten, voer de volgende opdracht met administratorbevoegdheden:
   
        netstat /ao
2. SQL Server Management Studio gebruiken voor het maken van een statische Analysis Services met de naam exemplaar poort door bij te werken, 'Poort' waarde in tabelvorm AS algemene eigenschappen-instantie. Zie voor meer informatie de 'een vaste poort gebruiken voor een standaard of benoemd exemplaar' [configureren Windows Firewall Analysis Services-toegang toestaan](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).
3. Het in tabelvorm exemplaar van de Analysis Services-service opnieuw starten.

Zie voor meer informatie, de **eindpunten van de virtuele Machine en Firewall-poorten** sectie in dit document.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Eindpunten van de virtuele Machine en Firewall-poorten
Deze sectie bevat een overzicht van Microsoft Azure virtuele Machine eindpunten om leeg te maken en poorten openen in de virtuele machine-firewalls. De volgende tabel geeft een overzicht van de **TCP** eindpunten voor maken en de poorten openen in de firewall van de virtuele machines.

* Als u van een enkele virtuele machine gebruikmaakt en de volgende twee items voldaan wordt, u hoeft niet te maken van VM-eindpunten en u niet wilt dat de poorten openen in de firewall op de virtuele machine.
  
  * U verbinding geen op afstand met de SQL Server-functies op de virtuele machine. Tot stand brengen van een verbinding met extern bureaublad met de virtuele machine en toegang tot de onderdelen van SQL Server lokaal op de virtuele machine wordt niet beschouwd als een externe verbinding met de SQL Server-functies.
  * U bent niet de virtuele machine toevoegen aan een domein lokale via virtuele netwerken van Azure of een andere VPN-tunneling oplossing.
* Als de virtuele machine is niet toegevoegd aan een domein, maar u extern wilt verbinden met de SQL Server-functies op virtuele machine:
  
  * De poorten openen in de firewall op de virtuele machine.
  * Eindpunten van de virtuele machine voor de vermelde poorten (*) maken.
* De eindpunten zijn niet vereist als de virtuele machine is toegevoegd aan een domein met een VPN-tunnel zoals Azure virtuele netwerken. Evenwel de poorten openen in de firewall op de virtuele machine.
  
  | Poort | Type | Beschrijving |
  | --- | --- | --- |
  | **80** |TCP |Rapportserver externe toegang (*). |
  | **1433** |TCP |SQL Server Management Studio (*). |
  | **1434** |UDP |SQL Server Browser. Dit is vereist wanneer de virtuele machine in die zijn gekoppeld aan een domein. |
  | **2382** |TCP |SQL Server Browser. |
  | **2383** |TCP |Standaardexemplaar van SQL Server Analysis Services en geclusterde benoemde exemplaren. |
  | **De gebruiker gedefinieerde** |TCP |Maak een statische Analysis Services met de naam exemplaar poort van een poortnummer dat die u kiest en vervolgens het poortnummer in de firewall te deblokkeren. |

Zie de volgende onderwerpen voor meer informatie over het maken van eindpunten:

* Eindpunten maken:[instellen eindpunten aan een virtuele Machine](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* SQL Server: Zie de sectie 'De volledige configuratie stappen verbinding maken met de virtuele machine met SQL Server Management Studio' [inrichten van een virtuele Machine van SQL Server op Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Het volgende diagram illustreert de poorten openen in de VM-firewall voor externe toegang tot de functies en onderdelen op de virtuele machine.

![poorten openen voor bi toepassingen in Azure Virtual machines](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Resources
* Bekijk het ondersteuningsbeleid voor Microsoft-serversoftware worden gebruikt in de virtuele Machine van Azure-omgeving. Het volgende onderwerp bevat een overzicht van ondersteuning voor functies zoals BitLocker Failover Clustering en netwerktaakverdeling. [Microsoft server softwareondersteuning voor Azure Virtual Machines](http://support.microsoft.com/kb/2721672).
* [SQL Server op Azure Virtual Machines-overzicht](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Virtuele machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Inrichting van een virtuele Machine van SQL Server op Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Hoe een gegevensschijf koppelen aan een virtuele Machine](../classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Een Database migreren naar SQL Server op een virtuele machine in Azure](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json)
* [Bepalen van de servermodus met een Analysis Services-exemplaar](https://msdn.microsoft.com/library/gg471594.aspx)
* [Multidimensionale model (Adventure Works zelfstudie)](https://technet.microsoft.com/library/ms170208.aspx)
* [Documentatiecentrum van Azure](https://azure.microsoft.com/documentation/)
* [Gebruik van Power BI in een hybride omgeving](https://msdn.microsoft.com/library/dn798994.aspx)

> [!NOTE]
> [Feedback en contactgegevens via het Microsoft SQL Server-verbinding verzenden](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Inhoud van de community
* [Azure SQL Database-beheer met PowerShell](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)


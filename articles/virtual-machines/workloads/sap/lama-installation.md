---
title: LaMa SAP-connector voor Azure | Microsoft Docs
description: SAP-systemen op Azure met behulp van SAP LaMa beheren
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: MSSedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/17/2018
ms.author: sedusch
ms.openlocfilehash: 432dcf4608d23f269c5005b86d2c58b7f70b3068
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58007577"
---
# <a name="sap-lama-connector-for-azure"></a>LaMa SAP-connector voor Azure

[1877727]:https://launchpad.support.sap.com/#/notes/1877727
[2343511]:https://launchpad.support.sap.com/#/notes/2343511
[2350235]:https://launchpad.support.sap.com/#/notes/2350235
[2562184]:https://launchpad.support.sap.com/#/notes/2562184
[2628497]:https://launchpad.support.sap.com/#/notes/2628497
[2445033]:https://launchpad.support.sap.com/#/notes/2445033
[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png
[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[hana-ops-guide]:hana-vm-operations.md

> [!NOTE]
> Algemene ondersteuningsverklaring: Geef altijd een incident openen met SAP op onderdeel BC-VCM-LVM-Hyper-v als u ondersteuning nodig voor SAP LaMa of de Azure-connector hebt.

SAP LaMa wordt gebruikt door veel klanten om te werken en hun SAP-landschap bewaken. Sinds SAP LaMa 3.0 SP05, wordt deze geleverd met een connector voor Azure standaard. U kunt deze connectors kunt gebruiken om de toewijzing ongedaan maken en virtuele machines starten, kopiëren en verplaatsen van beheerde schijven en beheerde schijven verwijderen. Met deze eenvoudige bewerkingen kunt u aan te verplaatsen, kopiëren, klonen en SAP-systemen met behulp van SAP LaMa vernieuwen.

Deze handleiding wordt beschreven hoe u de Azure connector instellen voor de SAP-LaMa, maakt u virtuele machines die kan worden gebruikt voor het installeren van adaptieve SAP-systemen en hoe u ze configureert.

> [!NOTE]
> De connector is alleen beschikbaar in de SAP LaMa Enterprise-editie

## <a name="resources"></a>Resources

De volgende SAP-opmerkingen zijn gerelateerd aan het onderwerp van de SAP-LaMa op Azure:

| Houd er rekening mee getal | Titel |
| --- | --- |
| [2343511] |Microsoft Azure-connector voor SAP-landschap Management (LaMa) |
| [2350235] |SAP Landscape Management 3.0 - Enterprise edition |

Lees ook de [SAP Help Portal voor de SAP-LaMa](https://help.sap.com/viewer/p/SAP_LANDSCAPE_MANAGEMENT_ENTERPRISE).

## <a name="general-remarks"></a>Algemene opmerkingen

* Zorg ervoor dat u inschakelen *automatisch maken van het koppelpunt* in instellingen -> Instellingen -> Engine  
  Als SAP LaMa volumes de Adaptive SAP-extensies gebruiken op een virtuele machine koppelt, moet het koppelpunt bestaan als deze instelling niet is ingeschakeld.

* Gebruik afzonderlijke subnet en geen dynamische IP-adressen gebruiken om te voorkomen dat IP-adres 'stelen' bij het implementeren van nieuwe virtuele machines en SAP-exemplaren zijn niet voorbereid  
  Als u dynamische toewijzing van IP-adressen in het subnet, dat ook wordt gebruikt door de SAP-LaMa, mislukken een SAP-systeem voorbereiden met de SAP-LaMa. Als een SAP-systeem niet voorbereid is, wordt de IP-adressen zijn niet gereserveerd en kunnen krijgen toegewezen aan andere virtuele machines.

* Als u zich bij beheerde hosts aanmeldt, zorg ervoor dat u niet blokkeren bestandssystemen in die wordt ontkoppeld  
  Als u zich aanmeldt bij een virtuele Linux-machines en wijzig de werkmap naar een map in een koppelpunt, bijvoorbeeld /usr/sap/AH1/ASCS00/exe, het volume kan niet ontkoppeld worden en een bijl of unprepare mislukt.

## <a name="set-up-azure-connector-for-sap-lama"></a>Instellen van Azure-connector voor SAP LaMa

De Azure-connector is verzonden vanaf SAP LaMa 3.0 SP05. Het is raadzaam om altijd de meest recente ondersteuningspakket met en de patch voor SAP LaMa 3.0 installeren. De Azure-connector maakt gebruik van een Service-Principal te autoriseren op basis van Microsoft Azure. Volg deze stappen voor het maken van een Service-Principal voor het beheer van SAP-landschap (LaMa).

1. Ga naar https://portal.azure.com
1. Open de Azure Active Directory-blade
1. Klik op App-registraties
1. Klik op toevoegen
1. Voer een naam in, selecteert u het Type toepassing 'Web-app/API', voer een aanmeldings-URL (bijvoorbeeld `http://localhost`) en klik op maken
1. De aanmeldings-URL wordt niet gebruikt en kan geldige URL zijn
1. Selecteer de nieuwe App en klik op de sleutels in het tabblad instellingen
1. Voer een beschrijving in voor een nieuwe sleutel, selecteer 'Verloopt nooit' en klik op Opslaan
1. Noteer de waarde in. Deze wordt gebruikt als het wachtwoord voor de Service-Principal
1. Noteer de toepassings-ID. Deze wordt gebruikt als de gebruikersnaam van de Service-Principal

De Service-Principal heeft geen machtigingen voor toegang tot uw Azure-resources standaard. U moet de Service-Principal machtigingen voor toegang tot deze.

1. Ga naar https://portal.azure.com
1. Open de resourceblade van de groepen
1. Selecteer de resourcegroep die u wilt gebruiken
1. Klik op de Access control (IAM)
1. Klik op de roltoewijzing toevoegen
1. Selecteer de rol Inzender
1. Voer de naam van de toepassing die u hierboven hebt gemaakt
1. Op Opslaan klikken
1. Herhaal stap 3 tot en met 8 voor alle resourcegroepen die u wilt gebruiken in de SAP-LaMa

Open de website die SAP LaMa en navigeer naar de infrastructuur. Ga naar het tabblad Cloud-Managers en klik op toevoegen. Selecteer de Microsoft Azure Cloud-Adapter en klik op volgende. Voer de volgende informatie in:

* Label: Kies een naam voor de connector-exemplaar
* Gebruikersnaam: Toepassings-id van de service-principal
* Wachtwoord: Service-Principal-sleutel en wachtwoord
* URL: Standaardwaarde https://management.azure.com/
* Controle-Interval (seconden): Moet ten minste 300
* Abonnements-id: Azure-abonnement-ID
* Azure Active Directory-Tenant-ID: ID van de Active Directory-tenant
* Proxyhost: Hostnaam van de proxy als SAP LaMa moet een proxy verbinding maken met internet
* Proxypoort: TCP-poort van de proxy

Klik op Testconfiguratie voor het valideren van uw invoer. U ziet

De verbinding is geslaagd: Verbinding met Microsoft-cloud is geslaagd. 7 resourcegroepen vinden (maximaal 10 groepen aangevraagd)

aan de onderkant van de website.

## <a name="provision-a-new-adaptive-sap-system"></a>Inrichten van een nieuwe adaptieve SAP-systeem

U kunt handmatig een nieuwe virtuele machine implementeert of gebruik een van de Azure-sjablonen in de [opslagplaats quickstart](https://github.com/Azure/azure-quickstart-templates). Deze bevat sjablonen voor [SAP NetWeaver ASCS](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-lama-ascs), [SAP NetWeaver-toepassingsservers](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-lama-apps), en de [database](https://github.com/Azure/azure-quickstart-templates/tree/master/sap-lama-database). U kunt deze sjablonen ook gebruiken voor het inrichten van nieuwe hosts als onderdeel van een systeem kopiëren/kloon enzovoort.

We raden u aan met behulp van een apart subnet voor alle virtuele machines dat u wilt beheren met de SAP-LaMa en gebruik geen dynamische IP-adressen op om te voorkomen dat IP-adres 'stelen' bij het implementeren van nieuwe virtuele machines en SAP-exemplaren niet voorbereid zijn.

> [!NOTE]
> Indien mogelijk moet alle extensies voor virtuele machines worden verwijderd omdat ze leiden lang runtimes tot kunnen voor het loskoppelen van schijven van een virtuele machine.

Zorg ervoor dat de gebruiker \<hanasid > adm, \<sapsid > adm en groep sapsys bestaat op de doelcomputer met dezelfde ID en groeps-id of LDAP gebruiken. Inschakelen en starten van de NFS-server op de virtuele machines die moeten worden gebruikt voor het uitvoeren van de SAP NetWeaver (A) SCS.

### <a name="manual-deployment"></a>Handmatige implementatie

SAP LaMa communiceert met de virtuele machine met behulp van de SAP-Host-Agent. Als u de virtuele machines handmatig implementeren of niet met behulp van de Azure Resource Manager-sjabloon uit de opslagplaats voor quickstart, zorg ervoor dat u de meest recente Agent van de SAP-Host en de adaptieve SAP-extensies installeren. Zie voor meer informatie over de vereiste patches voor Azure, SAP-notitie [2343511].

#### <a name="manual-deployment-of-a-linux-virtual-machine"></a>Handmatige implementatie van een virtuele Linux-Machine

Een nieuwe virtuele machine maken met een van de ondersteunde besturingssystemen die worden vermeld in de SAP-notitie [2343511]. Voeg extra IP-configuraties voor de SAP-exemplaren. Elk exemplaar moet ten minste op IP-adres en moet worden geïnstalleerd met een virtuele hostnaam.

De SAP NetWeaver ASCS-exemplaar moeten schijven voor /sapmnt/\<SAPSID >, / usr/sap/\<SAPSID >, en/usr/sap/trans/usr/sap/\<sapsid > adm. De SAP NetWeaver-toepassingsservers hoeft geen extra schijven. Alles met betrekking tot de SAP-instantie moet worden opgeslagen in de ASCS en via NFS geëxporteerd. Anders, het is momenteel niet mogelijk om toe te voegen extra toepassingsservers met behulp van SAP LaMa.

![SAP NetWeaver ASCS op Linux](media/lama/sap-lama-ascs-app-linux.png)

#### <a name="manual-deployment-for-sap-hana"></a>Handmatige implementatie van SAP HANA

Een nieuwe virtuele machine maken met een van de ondersteunde besturingssystemen voor SAP HANA, zoals vermeld in de SAP-notitie [2343511]. Één aanvullende IP-configuratie voor SAP HANA en één per tenant HANA toevoegen.

SAP HANA nodig schijven zijn voor /hana/shared, /hana/backup /hana/data en /hana/log

![SAP HANA op Linux](media/lama/sap-lama-db-hana.png)

#### <a name="manual-deployment-for-oracle-database-on-linux"></a>Handmatige implementatie voor de Oracle-Database op Linux

Een nieuwe virtuele machine maken met een van de ondersteunde besturingssystemen voor Oracle-databases, zoals vermeld in de SAP-notitie [2343511]. Voeg een extra IP-configuratie voor de Oracle-database.

De Oracle-database moet schijven voor /oracle /home/oraod1 en /home/oracle

![Oracle-database op Linux](media/lama/sap-lama-db-ora-lnx.png)

#### <a name="manual-deployment-for-microsoft-sql-server"></a>Handmatige implementatie voor Microsoft SQL Server

Een nieuwe virtuele machine maken met een van de ondersteunde besturingssystemen voor Microsoft SQL Server, zoals vermeld in de SAP-notitie [2343511]. Voeg een extra IP-configuratie voor de SQL Server-exemplaar.

De SQL Server-database-server moet schijven voor de databasegegevens en de logboekbestanden en de schijven voor c:\usr\sap.

![Oracle-database op Linux](media/lama/sap-lama-db-sql.png)

Zorg ervoor dat u een ondersteunde Microsoft ODBC-stuurprogramma voor SQL Server installeren op een virtuele machine die u wilt gebruiken voor het verplaatsen van een toepassingsserver SAP NetWeaver op of als het doel van een systeem kopiëren/klonen.

SAP LaMa kan geen SQL Server zelf verplaatsen, zodat een virtuele machine die u wilt gebruiken voor het verplaatsen van een database-exemplaar op, of als het doel van een systeem kopiëren/klonen, SQL Server vooraf geïnstalleerd moet.

### <a name="deploy-virtual-machine-using-an-azure-template"></a>Virtuele Machine met behulp van een Azure-sjabloon implementeren

Download de volgende meest recente beschikbare van archieven de [SAP Software Marketplace](https://support.sap.com/swdc) voor het besturingssysteem van de virtuele machines:

1. SAPCAR 7.21
1. SAP-HOSTAGENT 7,21
1. SAP ADAPTIEVE EXTENSIE 1.0 EXT

Ook downloaden van de volgende onderdelen van de [Microsoft Download Center](https://www.microsoft.com/download)

1. Microsoft Visual C++ 2010-herdistributiepakket (x64) (alleen Windows)
1. Microsoft ODBC-stuurprogramma voor SQL Server (alleen SQL Server)

De onderdelen zijn vereist om de sjabloon te implementeren. De eenvoudigste manier om het beschikbaar stellen aan de sjabloon is te uploaden naar Azure storage-account en een Shared Access Signature (SAS) maken.

De sjablonen gelden de volgende parameters:

* sapSystemId: De id van de SAP-systeem. Wordt gebruikt voor het maken van de schijfindeling (bijvoorbeeld/usr/sap/\<sapsid >).

* computerName: De computernaam van de nieuwe virtuele machine. Deze parameter wordt ook gebruikt door de SAP-LaMa. Wanneer u deze sjabloon gebruiken voor het inrichten van een nieuwe virtuele machine als onderdeel van een kopie van het systeem, wordt SAP LaMa wacht totdat de host met de naam van deze computer kan worden bereikt.

* osType: Het type van het besturingssysteem die u wilt implementeren.

* DbType: Het type van de database. Deze parameter wordt gebruikt om te bepalen hoeveel extra IP-configuraties moeten worden toegevoegd en hoe de schijfindeling moet uitzien.

* sapSystemSize: De grootte van de SAP-systeem die u wilt implementeren. Wordt gebruikt om te bepalen van het type virtuele machine-instantie en de grootte.

* adminUsername: Gebruikersnaam voor de virtuele machine.

* adminPassword: Wachtwoord voor de virtuele machine. U kunt ook een openbare sleutel opgeven voor SSH.

* sshKeyData: Openbare SSH-sleutel voor de virtuele machines. Alleen ondersteund voor Linux-besturingssystemen.

* subnetId: De ID van het subnet dat u wilt gebruiken.

* deployEmptyTarget: Als u wilt gebruiken van de virtuele machine als een doel voor een exemplaar bijl of vergelijkbare, kunt u een leeg doel implementeren. In dit geval worden er geen extra schijven of IP-configuraties gekoppeld.

* sapcarLocation: De locatie voor de toepassing sapcar die overeenkomt met het besturingssysteem die u implementeert. sapcar wordt gebruikt om op te halen van het archief dat u in de andere parameters opgeven.

* sapHostAgentArchiveLocation: De locatie van het Host-Agent voor SAP-archief. SAP-Host-Agent wordt geïmplementeerd als onderdeel van de sjabloonimplementatie van deze.

* sapacExtLocation: De locatie van de SAP-adaptieve extensies. SAP-notitie [2343511] geeft een lijst van de minimale-patchniveau op die nodig zijn voor Azure.

* vcRedistLocation: De locatie van de VC-Runtime die is vereist voor het installeren van de SAP-adaptieve uitbreidingen. Deze parameter is alleen vereist voor Windows.

* odbcDriverLocation: De locatie van het ODBC-stuurprogramma dat u wilt installeren. Alleen Microsoft ODBC-stuurprogramma voor SQL Server wordt ondersteund.

* sapadmPassword: Het wachtwoord voor de gebruiker sapadm.

* sapadmId: De Linux-gebruikers-ID van de gebruiker sapadm. Niet vereist voor Windows.

* sapsysGid: De Linux-groep-ID van de groep sapsys. Niet vereist voor Windows.

* _artifactsLocation: De basis-URI, waar de artefacten die zijn vereist voor deze sjabloon zich bevinden. Wanneer de sjabloon wordt geïmplementeerd met behulp van de bijbehorende scripts, een persoonlijke locatie in het abonnement wordt gebruikt en deze waarde automatisch wordt gegenereerd. Alleen nodig als u de sjabloon vanuit GitHub hoeft niet te implementeren.

* _artifactsLocationSasToken: De sasToken vereist voor toegang tot _artifactsLocation. Wanneer de sjabloon wordt geïmplementeerd met behulp van de bijbehorende scripts, worden een sasToken automatisch gegenereerd. Alleen nodig als u de sjabloon vanuit GitHub hoeft niet te implementeren.

### <a name="sap-hana"></a>SAP HANA

In de onderstaande voorbeelden veronderstellen we dat u SAP HANA met systeem-ID HN1 en het SAP NetWeaver-systeem met systeem-ID AH1 installeren. De virtuele hostnamen zijn hn1-db voor de HANA-instantie, ah1-db voor de HANA-tenant die wordt gebruikt door het SAP NetWeaver-systeem, ah1-ascs voor de SAP NetWeaver ASCS en ah1-di-0 voor de eerste SAP NetWeaver-toepassingsserver.

#### <a name="install-sap-netweaver-ascs-for-sap-hana"></a>SAP NetWeaver ASCS voor SAP HANA installeren

Voordat u de SAP Software inrichting Manager (SWPM), moet u het IP-adres van de virtuele hostnaam van de ASCS koppelen. De aanbevolen manier is met sapacext. Als u het IP-adres met behulp van sapacext koppelt, zorg ervoor dat het IP-adres koppelen na een opnieuw opstarten.

![Linux][Logo_Linux] Linux

```bash
# /usr/sap/hostctrl/exe/sapacext -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h ah1-ascs -n 255.255.255.128
```

![Windows][Logo_Windows] Windows

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h ah1-ascs -n 255.255.255.128
```

Voer SWPM en gebruik *ah1 ascs* voor de *ASCS-exemplaarnaam Host*.

![Linux][Logo_Linux] Linux  
De volgende parameter voor het profiel aan het profiel SAP Host-Agent, bevindt zich onder /usr/sap/hostctrl/exe/host_profile toevoegen. Zie voor meer informatie, SAP-notitie [2628497].
```
acosprep/nfs_paths=/home/ah1adm,/usr/sap/trans,/sapmnt/AH1,/usr/sap/AH1
```

#### <a name="install-sap-hana"></a>SAP HANA installeren

Als u SAP HANA met behulp van de opdrachtregel hulpprogramma hdblcm installeert, gebruikt u de parameter--hostnaam voor een virtuele hostnaam. U moet het IP-adres van de virtuele hostnaam van de database toevoegen aan een netwerkinterface. De aanbevolen manier is met sapacext. Als u het IP-adres met behulp van sapacext koppelt, zorg ervoor dat het IP-adres koppelen na een opnieuw opstarten.

Voeg nog een virtuele-hostnaam en IP-adres voor de naam die wordt gebruikt door de toepassingsservers verbinding maken met de HANA-tenant.

```bash
# /usr/sap/hostctrl/exe/sapacext -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h hn1-db -n 255.255.255.128
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h ah1-db -n 255.255.255.128
```

De installatie van de database-exemplaar van SWPM worden uitgevoerd op de virtuele machine van de application server, niet op de virtuele machine van HANA. Gebruik *ah1-db* voor de *Database Host* in dialoogvenster *Database voor SAP-systeem*.

#### <a name="install-sap-netweaver-application-server-for-sap-hana"></a>SAP NetWeaver-toepassingsserver voor SAP HANA installeren

Voordat u de SAP Software inrichting Manager (SWPM), moet u het IP-adres van de virtuele host-naam van de toepassingsserver te koppelen. De aanbevolen manier is met sapacext. Als u het IP-adres met behulp van sapacext koppelt, zorg ervoor dat het IP-adres koppelen na een opnieuw opstarten.

![Linux][Logo_Linux] Linux

```bash
# /usr/sap/hostctrl/exe/sapacext -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
/usr/sap/hostctrl/exe/sapacext -a ifup -i eth0 -h ah1-di-0 -n 255.255.255.128
```

![Windows][Logo_Windows] Windows

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h ah1-di-0 -n 255.255.255.128
```

Het verdient aanbeveling gebruik van SAP NetWeaver-profiel parameter databases/hdb/hdb_use_ident de identiteit die wordt gebruikt voor de sleutel niet vinden in de userstore HDB in te stellen. U kunt deze parameter handmatig toevoegen na de installatie van de database-exemplaar met SWPM of SWPM met uitvoeren

```bash
# from https://blogs.sap.com/2015/04/14/sap-hana-client-software-different-ways-to-set-the-connectivity-data/
/sapdb/DVDs/IM_LINUX_X86_64/sapinst HDB_USE_IDENT=SYSTEM_COO
```

Als u deze handmatig instellen, moet u ook nieuwe HDB userstore-vermeldingen te maken.

```bash
# run as <sapsid>adm
/usr/sap/AH1/hdbclient/hdbuserstore LIST
# reuse the port that was listed from the command above, in this example 35041
/usr/sap/AH1/hdbclient/hdbuserstore SET DEFAULT ah1-db:35041@AH1 SAPABAP1 <password>
```

Gebruik *ah1-di-0* voor de *Pa's Host exemplaarnaam* in dialoogvenster *primaire Application Server-exemplaar*.

#### <a name="post-installation-steps-for-sap-hana"></a>Stappen na de installatie voor SAP HANA

Zorg ervoor dat u de back-up van de SYSTEMDB en alle tenantdatabases voordat u het proberen een kopie van de tenant, tenant verplaatsen of de systeemreplicatie van een maken.

### <a name="microsoft-sql-server"></a>Microsoft SQL Server

In de onderstaande voorbeelden veronderstellen we dat u de SAP NetWeaver-systeem met systeem-ID AS1 installeert. De virtuele hostnamen zijn as1-db voor de SQL Server-exemplaar dat is gebruikt door het SAP NetWeaver-systeem, as1-ascs voor de SAP NetWeaver ASCS en as1-di-0 voor de eerste SAP NetWeaver-toepassingsserver.

#### <a name="install-sap-netweaver-ascs-for-sql-server"></a>SAP NetWeaver ASCS voor SQL Server installeren

Voordat u de SAP Software inrichting Manager (SWPM), moet u het IP-adres van de virtuele hostnaam van de ASCS koppelen. De aanbevolen manier is met sapacext. Als u het IP-adres met behulp van sapacext koppelt, zorg ervoor dat het IP-adres koppelen na een opnieuw opstarten.

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h as1-ascs -n 255.255.255.128
```

Voer SWPM en gebruik *as1 ascs* voor de *ASCS-exemplaarnaam Host*.

#### <a name="install-sql-server"></a>Installatie van SQL Server

U moet het IP-adres van de virtuele hostnaam van de database toevoegen aan een netwerkinterface. De aanbevolen manier is met sapacext. Als u het IP-adres met behulp van sapacext koppelt, zorg ervoor dat het IP-adres koppelen na een opnieuw opstarten.

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h as1-db -n 255.255.255.128
```

De installatie van de database-exemplaar van SWPM uitvoeren op de SQL server-machine. Gebruik SAPINST_USE_HOSTNAME =*as1-db* voor de onderdrukking van de hostnaam die wordt gebruikt voor verbinding met SQL Server. Als u de virtuele machine met behulp van de Azure Resource Manager-sjabloon hebt geïmplementeerd, zorg ervoor dat u het instellen van de map voor de databasebestanden van de gegevens naar *C:\sql\data* en databaselogboekbestand *C:\sql\log*.

Zorg ervoor dat de gebruiker *NT AUTHORITY\SYSTEM* heeft toegang tot de SQL-Server en de serverfunctie *sysadmin*. Zie voor meer informatie, SAP-notitie [1877727] en [2562184].

#### <a name="install-sap-netweaver-application-server"></a>SAP NetWeaver-toepassingsserver installeren

Voordat u de SAP Software inrichting Manager (SWPM), moet u het IP-adres van de virtuele host-naam van de toepassingsserver te koppelen. De aanbevolen manier is met sapacext. Als u het IP-adres met behulp van sapacext koppelt, zorg ervoor dat het IP-adres koppelen na een opnieuw opstarten.

```bash
# C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i <network interface> -h <virtual hostname or IP address> -n <subnet mask>
C:\Program Files\SAP\hostctrl\exe\sapacext.exe -a ifup -i "Ethernet 3" -h as1-di-0 -n 255.255.255.128
```

Gebruik *as1-di-0* voor de *Pa's Host exemplaarnaam* in dialoogvenster *primaire Application Server-exemplaar*.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="errors-and-warnings-during-discover"></a>Fouten en waarschuwingen tijdens detecteren

* De machtiging SELECT is geweigerd
  * [Microsoft] [ODBC SQL Server-stuurprogramma] [SQL Server] De machtiging SELECT is geweigerd voor het object 'log_shipping_primary_databases', database 'msdb' schema 'dbo'. [SOAPFaultException]  
  De machtiging SELECT is geweigerd voor het object 'log_shipping_primary_databases', database 'msdb' schema 'dbo'.
  * Oplossing  
    Zorg ervoor dat *NT AUTHORITY\SYSTEM* hebben toegang tot de SQL-Server. Zie SAP-notitie [2562184]


### <a name="errors-and-warnings-for-instance-validation"></a>Fouten en waarschuwingen voor validatie

* Een uitzondering is opgetreden tijdens de controle van de userstore HDB  
  * Logboeken bekijken  
    com.sap.nw.lm.aci.monitor.api.validation.RuntimeValidationException: Uitzondering in validator met ID 'RuntimeHDBConnectionValidator' (validatie: 'VALIDATION_HDB_USERSTORE'): Kan de hdbuserstore niet ophalen  
    HANA userstore bevindt zich niet in de juiste locatie
  * Oplossing  
    Zorg ervoor dat /usr/sap/AH1/hdbclient/install/installation.ini juist is

### <a name="errors-and-warnings-during-a-system-copy"></a>Fouten en waarschuwingen tijdens het kopiëren van een systeem

* Er is een fout opgetreden bij het valideren van het systeem stap inrichten
  * Veroorzaakt door: com.sap.nw.lm.aci.engine.base.api.util.exception.HAOperationException aanroepen van ' / usr/sap/hostctrl/exe/sapacext - een niveau van ShowHanaBackups -m HN1 -f 50 - h hn1-db - o = 0\;status = 5\;poort 35013 pf = / usr/sap/hostctrl = / exe/host_profile -R -T dev_lvminfo -u SYSTEM -p hook - r | /usr/SAP/hostctrl/exe/sapacext - een niveau van ShowHanaBackups -m HN1 -f 50 - h hn1-db - o = 0\;status = 5\;poort = 35013 pf = / usr/sap/hostctrl/exe/host_profile - R -T dev_lvminfo -u SYSTEM -p hook - r
  * Oplossing  
    Back-up maken van alle databases in de bron HANA-systeem

* System up stap *Start* van database-instantie
  * Host-Agent '000D3A282BC91EE8A1D76CF1F92E2944' is mislukt (OperationException. FaultCode: '127', bericht: De opdracht niet uitvoeren. : [Microsoft] [ODBC SQL Server-stuurprogramma] [SQL Server] gebruiker heeft geen machtiging voor het wijzigen van de database 'AS2', de database bestaat niet of de database is niet in een status die toegangscontroles. ")
  * Oplossing  
    Zorg ervoor dat *NT AUTHORITY\SYSTEM* hebben toegang tot de SQL-Server. Zie SAP-notitie [2562184]

### <a name="errors-and-warnings-during-a-system-clone"></a>Fouten en waarschuwingen tijdens het klonen van een systeem

* Er is een fout opgetreden bij het registreren van exemplaar van agent in stap *geforceerde registreren en Start exemplaar Agent* toepassingsserver of ASCS
  * Er is een fout opgetreden bij het registreren van exemplaar-agent. (RemoteException: 'Failed to laden van gegevens van exemplaren van het profiel'\\as1-ascs\sapmnt\AS1\SYS\profile\AS1_D00_as1-di-0':  Geen toegang tot profiel '\\as1-ascs\sapmnt\AS1\SYS\profile\AS1_D00_as1-di-0': Bestand of map niet bestaat. ")
  * Oplossing  
   Zorg ervoor dat de share sapmnt in de ASCS/SCS volledige toegang voor SAP_AS1_GlobalAdmin heeft

* Fout in de stap *opstarten-beveiliging inschakelen voor klonen*
  * Kan bestand niet openen '\\as1-ascs\sapmnt\AS1\SYS\profile\AS1_D00_as1-di-0' oorzaak: Bestand of map niet bestaat
  * Oplossing  
    Het computeraccount van de toepassingsserver moet schrijftoegang tot het profiel

### <a name="errors-and-warnings-during-create-system-replication"></a>Fouten en waarschuwingen tijdens het maken van de Systeemreplicatie

* Uitzondering tijdens te klikken op systeem-replicatie maken
  * Veroorzaakt door: com.sap.nw.lm.aci.engine.base.api.util.exception.HAOperationException aanroepen van ' / usr/sap/hostctrl/exe/sapacext - een niveau van ShowHanaBackups -m HN1 -f 50 - h hn1-db - o = 0\;status = 5\;poort 35013 pf = / usr/sap/hostctrl = / exe/host_profile -R -T dev_lvminfo -u SYSTEM -p hook - r | /usr/SAP/hostctrl/exe/sapacext - een niveau van ShowHanaBackups -m HN1 -f 50 - h hn1-db - o = 0\;status = 5\;poort = 35013 pf = / usr/sap/hostctrl/exe/host_profile - R -T dev_lvminfo -u SYSTEM -p hook - r
  * Oplossing  
    Test of sapacext kan worden uitgevoerd als `<hanasid`> adm

* Fout bij het volledige kopie is niet ingeschakeld in de stap opslag
  * Er is een fout opgetreden tijdens het rapporteren van een bericht van het kenmerk context voor het pad IStorageCopyData.storageVolumeCopyList:1 en veld targetStorageSystemId
  * Oplossing  
    Negeren van waarschuwingen in de stap en probeer het opnieuw. Dit probleem wordt opgelost in een nieuwe ondersteuning voor pakket/patch LaMa van SAP.

### <a name="errors-and-warnings-during-relocate"></a>Fouten en waarschuwingen tijdens bijl

* Pad '/ usr/sap/AH1' is niet toegestaan voor nfs reexports.
  * Controleer de SAP-notitie [2628497] voor meer informatie.
  * Oplossing  
    Voeg dat ASCS exporteert naar ASCS HostAgent-profiel. Zie SAP-notitie [2628497]

* De functie is niet geïmplementeerd wanneer ASCS verplaatsen
  * Opdrachtuitvoer: exportfs: host: / usr/sap/AX1: De functie is niet geïmplementeerd
  * Oplossing  
    Zorg ervoor dat de NFS-server-service is ingeschakeld op de bijl virtuele doelmachine

### <a name="errors-and-warnings-during-application-server-installation"></a>Fouten en waarschuwingen tijdens de installatie van de Server toepassing

* Fout bij het uitvoeren van SAPinst stap: getProfileDir
  * FOUT: (Laatste fout gerapporteerd door de stap: Gevangen ESAPinstException in aanroep van de module: Validatie van de stap ' | NW_DI | ind | ind | ind | ind | 0 | 0 | NW_GetSidFromProfiles | ind | ind | ind | ind | getSid | 0 | NW_readProfileDir | ind | ind | ind | ind | readProfile | 0 | getProfileDir' heeft een fout gerapporteerd: Knooppunt \\\as1-ascs\sapmnt\AS1\SYS\profile bestaat niet. Start de SAPinst in de interactieve modus dit probleem op te lossen)
  * Oplossing  
    Zorg ervoor dat SWPM wordt uitgevoerd met een gebruiker die toegang tot het profiel heeft. Deze gebruiker kan worden geconfigureerd in de wizard installatie van de toepassing-Server

* Fout bij het uitvoeren van SAPinst stap: askUnicode
  * FOUT: (Laatste fout gerapporteerd door de stap: Gevangen ESAPinstException in aanroep van de module: Validatie van de stap ' | NW_DI | ind | ind | ind | ind | 0 | 0 | NW_GetSidFromProfiles | ind | ind | ind | ind | getSid | 0 | NW_getUnicode | ind | ind | ind | ind | unicode | 0 | askUnicode' heeft een fout gerapporteerd: Start de SAPinst in de interactieve modus dit probleem op te lossen)
  * Oplossing  
    Als u een recente kernel voor SAP, kan niet SWPM bepalen of het systeem een unicode-systeem meer met behulp van de berichtenserver van de ASCS. Zie SAP-notitie [2445033] voor meer informatie.  
    Dit probleem wordt opgelost in een nieuwe ondersteuning voor pakket/patch LaMa van SAP.  
    Stel profiel parameter OS_UNICODE = uc in het standaardprofiel van uw SAP-systeem om dit probleem te omzeilen.

* Fout bij het uitvoeren van SAPinst stap: dCheckGivenServer
  * Fout bij het uitvoeren van SAPinst stap: dCheckGivenServer ' versie = "1.0" Fout: (Laatste fout gerapporteerd door de stap: \<p > de installatie is geannuleerd door gebruiker. \</p>
  * Oplossing  
    Zorg ervoor dat SWPM wordt uitgevoerd met een gebruiker die toegang tot het profiel heeft. Deze gebruiker kan worden geconfigureerd in de wizard installatie van de toepassing-Server

* Fout bij het uitvoeren van SAPinst stap: checkClient
  * Fout bij het uitvoeren van SAPinst stap: checkClient ' versie = "1.0" Fout: (Laatste fout gerapporteerd door de stap: \<p > de installatie is geannuleerd door gebruiker. \</p>)
  * Oplossing  
    Zorg ervoor dat het Microsoft ODBC-stuurprogramma voor SQL Server is geïnstalleerd op de virtuele machine waarop u wilt installeren van de toepassingsserver

* Fout bij het uitvoeren van SAPinst stap: copyScripts
  * Laatste fout die door de stap: Systeemaanroep is mislukt. DETAILS: Fout 13 (0x0000000d) (u hebt geen machtiging) in de uitvoering van systeem aanroepen 'fopenU' met de parameter (\\\as1-ascs/sapmnt/AS1/SYS/exe/uc/NTAMD64/strdbs.cmd, w), regel (494) in bestand (\bas/bas/749_REL/bc_749_REL/src/ins/SAPINST/impl/src/syslib / filesystem/syxxcfstrm2.cpp), stack-trace:  
  CThrThread.cpp: 85: CThrThread::threadFunction()  
  CSiServiceSet.cpp: 63: CSiServiceSet::executeService()  
  CSiStepExecute.cpp: 913: CSiStepExecute::execute()  
  EJSController.cpp: 179: EJSControllerImpl::executeScript()  
  JSExtension.hpp: 1136: CallFunctionBase::call()  
  iaxxcfile.cpp: 183: iastring CIaOsFileConnect::callMemberFunction (iastring const & naam, args_t const & argumenten)  
  iaxxcfile.cpp: 1849: iastring CIaOsFileConnect::newFileStream (const args_t & _args)  
  iaxxbfile.cpp: 773: CIaOsFile::newFileStream_impl(4)  
  syxxcfile.cpp: 233: CSyFileImpl::openStream(ISyFile::eFileOpenMode)  
  syxxcfstrm.cpp: 29: CSyFileStreamImpl::CSyFileStreamImpl(CSyFileStream*,iastring,ISyFile::eFileOpenMode)  
  syxxcfstrm.cpp: 265: CSyFileStreamImpl::open()  
  syxxcfstrm2.cpp: 58: CSyFileStream2Impl::CSyFileStream2Impl (const CSyPath & \\\aw1-ascs/sapmnt/AW1/SYS/exe/uc/NTAMD64/strdbs.cmd, 0x4)  
  syxxcfstrm2.cpp: 456: CSyFileStream2Impl::open()
  * Oplossing  
    Zorg ervoor dat SWPM wordt uitgevoerd met een gebruiker die toegang tot het profiel heeft. Deze gebruiker kan worden geconfigureerd in de wizard installatie van de toepassing-Server

* Fout bij het uitvoeren van SAPinst stap: askPasswords
  * Laatste fout die door de stap: Systeemaanroep is mislukt. DETAILS: Fout 5 (0x00000005) (toegang is geweigerd.) in de uitvoering van systeemaanroep 'NetValidatePasswordPolicy' met de parameter (...), regel (359) in bestand (\bas/bas/749_REL/bc_749_REL/src/ins/SAPINST/impl/src/syslib/account/synxcaccmg.cpp), stack-trace:  
  CThrThread.cpp: 85: CThrThread::threadFunction()  
  CSiServiceSet.cpp: 63: CSiServiceSet::executeService()  
  CSiStepExecute.cpp: 913: CSiStepExecute::execute()  
  EJSController.cpp: 179: EJSControllerImpl::executeScript()  
  JSExtension.hpp: 1136: CallFunctionBase::call()  
  CSiStepExecute.cpp: 764: CSiStepExecute::invokeDialog()  
  DarkModeGuiEngine.cpp: 56: DarkModeGuiEngine::showDialogCalledByJs()  
  DarkModeDialog.cpp: 85: DarkModeDialog::submit()  
  EJSController.cpp: 179: EJSControllerImpl::executeScript()  
  JSExtension.hpp: 1136: CallFunctionBase::call()  
  iaxxcaccount.cpp: 107: iastring CIaOsAccountConnect::callMemberFunction (iastring const & naam, args_t const & argumenten)  
  iaxxcaccount.cpp: 1186: iastring CIaOsAccountConnect::validatePasswordPolicy(args_t const& _args)  
  iaxxbaccount.cpp: 430: CIaOsAccount::validatePasswordPolicy_impl()  
  synxcaccmg.cpp: 297: ISyAccountMgt::PasswordValidationMessage CSyAccountMgtImpl::validatePasswordPolicy(saponazure,***) const)
  * Oplossing  
    Zorg ervoor dat u een hostregel toevoegen in stap *isolatie* voor de communicatie van de virtuele machine met de domeincontroller

## <a name="next-steps"></a>Volgende stappen
* [SAP HANA op Azure-bedieningshandleiding][hana-ops-guide]
* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]

---
title: Problemen met SSIS Integration Runtime Management in Azure Data Factory oplossen Microsoft Docs
description: Dit artikel bevat richt lijnen voor probleem oplossing voor beheer problemen van SSIS Integration Runtime (SSIS IR)
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/08/2019
author: chinadragon0515
ms.author: dashe
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: 8abffdf443e26c03c38c12a3947a47a94157c9da
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68609620"
---
# <a name="troubleshoot-ssis-integration-runtime-management-in-azure-data-factory"></a>Problemen met SSIS Integration Runtime Management in Azure Data Factory oplossen

Dit artikel bevat richt lijnen voor probleem oplossing voor beheer problemen in Azure-SQL Server Integration Services (SSIS) Integration Runtime (IR), ook wel SSIS IR genoemd.

## <a name="overview"></a>Overzicht

Als u een probleem ondervindt bij het inrichten of ongedaan maken van de SSIS-IR, ziet u een fout bericht in de Microsoft Azure Data Factory portal of een fout die is geretourneerd door een Power shell-cmdlet. De fout wordt altijd weer gegeven in de indeling van een fout code met een gedetailleerd fout bericht.

Als de fout code InternalServerError is, heeft de service tijdelijke problemen. Voer de bewerking later opnieuw uit. Als een nieuwe poging niet helpt, neemt u contact op met het ondersteunings team van Azure Data Factory.

Anders kunnen drie belang rijke externe afhankelijkheden fouten veroorzaken: een Azure SQL Database Server of een beheerd exemplaar, een aangepast installatie script en een configuratie van een virtueel netwerk.

## <a name="azure-sql-database-server-or-managed-instance-issues"></a>Problemen met de Azure SQL Database Server of het beheerde exemplaar

Een Azure SQL Database Server of een beheerd exemplaar is vereist als u SSIS IR inricht met een SSIS-catalogus database. De SSIS-IR moet toegang hebben tot de Azure SQL Database Server of het beheerde exemplaar. Daarnaast moet het account van de Azure SQL Database Server of het beheerde exemplaar gemachtigd zijn om een SSIS-catalogus database (SSISDB) te maken. Als er een fout optreedt, wordt een fout code met een gedetailleerd SQL-uitzonderings bericht weer gegeven in de Data Factory Portal. Gebruik de informatie in de volgende lijst om de fout codes op te lossen.

### <a name="azuresqlconnectionfailure"></a>AzureSqlConnectionFailure

U ziet dit probleem mogelijk wanneer u een nieuwe SSIS-IR inricht of wanneer IR wordt uitgevoerd. Als u deze fout tijdens het inrichten van IR ondervindt, kunt u een gedetailleerd SqlException-bericht ontvangen in het fout bericht dat een van de volgende problemen aangeeft:

* Een probleem met de netwerk verbinding. Controleer of de hostnaam van de SQL Server of het beheerde exemplaar toegankelijk is. Controleer ook of er geen firewall of netwerk beveiligings groep (NSG) SSIS IR-toegang tot de server blokkeert.
* De aanmelding is mislukt tijdens SQL-verificatie. Het gegeven account kan zich niet aanmelden bij de SQL Server Data Base. Zorg ervoor dat u het juiste gebruikers account opgeeft.
* De aanmelding is mislukt tijdens de verificatie van Microsoft Azure Active Directory (Azure AD) (beheerde identiteit). Voeg de beheerde identiteit van uw fabriek toe aan een AAD-groep en zorg ervoor dat de beheerde identiteit toegangs machtigingen heeft voor de server van de catalogus database.
* Verbindingstime-out. Deze fout wordt altijd veroorzaakt door een configuratie met betrekking tot beveiliging. U wordt aangeraden:
  1. Maak een nieuwe virtuele machine.
  1. Voeg de virtuele machine toe aan dezelfde Microsoft Azure Virtual Network van IR als IR zich in een virtueel netwerk bevindt.
  1. Installeer SSMS en controleer de status van de Azure SQL Database-Server of het beheerde exemplaar.

Los het probleem op dat wordt weer gegeven in het fout bericht gedetailleerde SQL-uitzonde ring voor andere problemen. Als u nog steeds problemen ondervindt, neemt u contact op met het ondersteunings team van de Azure SQL Database Server of het beheerde exemplaar.

Als u de fout ziet wanneer IR wordt uitgevoerd, worden de netwerk beveiligings groep of de firewall wijzigingen waarschijnlijk verhinderd dat het SSIS IR worker-knoop punt toegang heeft tot de Azure SQL Database Server of het beheerde exemplaar. Deblokkeren van het SSIS IR worker-knoop punt zodat het toegang heeft tot de Azure SQL Database Server of het beheerde exemplaar.

### <a name="catalogcapacitylimiterror"></a>CatalogCapacityLimitError

Dit type fout bericht kan er als volgt uitzien: ' De data base ' SSISDB ' heeft het quotum voor de grootte bereikt. Partitioneer of verwijder gegevens, verwijder indexen of Raadpleeg de documentatie voor mogelijke oplossingen. " 

De mogelijke oplossingen zijn:
* Verg root de quotum grootte van uw SSISDB.
* Wijzig de configuratie van SSISDB om de grootte te verkleinen:
   * De retentie periode en het aantal project versies verminderen.
   * De retentie periode van het logboek te verminderen.
   * Het standaard niveau van het logboek wijzigen.

### <a name="catalogdbbelongstoanotherir"></a>CatalogDbBelongsToAnotherIR

Deze fout betekent dat de Azure SQL Database Server of het beheerde exemplaar al een SSISDB heeft en dat deze wordt gebruikt door een andere IR. U moet een andere Azure SQL Database Server of een beheerd exemplaar opgeven, of de bestaande SSISDB verwijderen en de nieuwe IR opnieuw opstarten.

### <a name="catalogdbcreationfailure"></a>CatalogDbCreationFailure

Deze fout kan een van de volgende oorzaken hebben:

* Het gebruikers account dat is geconfigureerd voor de SSIS IR, beschikt niet over de machtiging om de data base te maken. U kunt de gebruiker machtigen om de data base te maken.
* Er treedt een time-out op tijdens het maken van de data base, zoals een time-out voor uitvoering of een time-out voor de Voer de bewerking later opnieuw uit. Als de nieuwe poging niet werkt, neemt u contact op met het ondersteunings team van de Azure SQL Database Server of het beheerde exemplaar.

Controleer het fout bericht van SQL-uitzonde ring voor andere problemen en los het probleem op dat wordt vermeld in de fout Details. Als u nog steeds problemen ondervindt, neemt u contact op met het ondersteunings team van de Azure SQL Database Server of het beheerde exemplaar.

### <a name="invalidcatalogdb"></a>InvalidCatalogDb

Dit type fout bericht ziet er als volgt uit: ' Ongeldige object naam ' Catalog. catalog_properties '. ' In dit geval hebt u al een Data Base met de naam SSISDB, maar deze is niet gemaakt door SSIS IR, of de Data Base bevindt zich in een ongeldige status die wordt veroorzaakt door fouten in de laatste SSIS-IR-inrichting. U kunt de bestaande data base met de naam SSISDB verwijderen, maar u kunt ook een nieuwe Azure SQL Database Server of een beheerd exemplaar configureren voor de IR.

## <a name="custom-setup-issues"></a>Aangepaste installatie problemen

Aangepaste installatie biedt een interface voor het toevoegen van uw eigen installatie stappen tijdens het inrichten of opnieuw configureren van uw SSIS-IR. Zie [Customize setup for the Azure-SSIS Integration runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)voor meer informatie.

Zorg ervoor dat uw container alleen de benodigde aangepaste Setup-bestanden bevat. alle bestanden in de container worden gedownload naar het SSIS IR worker-knoop punt. Het is raadzaam om het aangepaste installatie script op een lokale machine te testen om eventuele problemen met het uitvoeren van scripts op te lossen voordat u het script uitvoert in SSIS IR.

De aangepaste installatie script container wordt gecontroleerd terwijl IR wordt uitgevoerd, omdat SSIS IR regel matig wordt bijgewerkt. Voor deze update is toegang tot de container vereist om het aangepaste installatie script te downloaden en opnieuw te installeren. Het proces controleert ook of de container toegankelijk is en of het Main. cmd-bestand bestaat.

Voor elke fout die aangepaste installatie omvat, ziet u een CustomSetupScriptFailure-fout code met subcode zoals CustomSetupScriptBlobContainerInaccessible of CustomSetupScriptNotFound.

### <a name="customsetupscriptblobcontainerinaccessible"></a>CustomSetupScriptBlobContainerInaccessible

Deze fout betekent dat SSIS IR geen toegang heeft tot uw Azure Blob-container voor aangepaste installatie. Zorg ervoor dat de SAS-URI van de container bereikbaar is en niet is verlopen.

Stop de IR als deze wordt uitgevoerd, Configureer de IR opnieuw met de nieuwe aangepaste installatie container SAS URI en start de IR opnieuw op.

### <a name="customsetupscriptnotfound"></a>CustomSetupScriptNotFound

Deze fout betekent dat SSIS IR geen aangepast installatie script (Main. cmd) kan vinden in de BLOB-container. Zorg ervoor dat Main. cmd bestaat in de container. Dit is het toegangs punt voor de installatie van aangepaste installatie.

### <a name="customsetupscriptexecutionfailure"></a>CustomSetupScriptExecutionFailure

Deze fout betekent dat de uitvoering van het aangepaste installatie script (Main. cmd) is mislukt. Probeer het script eerst op uw lokale machine of Controleer de aangepaste Setup-uitvoerings Logboeken in uw BLOB-container.

### <a name="customsetupscripttimeout"></a>CustomSetupScriptTimeout

Deze fout wijst op een time-out voor het uitvoeren van een aangepaste installatie script. Zorg ervoor dat uw BLOB-container alleen de benodigde aangepaste installatie bestanden bevat. Controleer ook de aangepaste Setup-uitvoerings Logboeken in uw BLOB-container. De maximale periode voor aangepaste Setup is 45 minuten voordat er een time-out optreedt en de maximum periode omvat de tijd voor het downloaden van alle bestanden uit uw container en het installeren ervan op SSIS IR. Als u een langere periode nodig hebt, verhoogt u een ondersteunings ticket.

### <a name="customsetupscriptloguploadfailure"></a>CustomSetupScriptLogUploadFailure

Deze fout betekent dat de poging om de aangepaste Setup-uitvoerings logboeken naar de BLOB-container te uploaden, is mislukt. Dit probleem treedt op omdat SSIS IR geen schrijf machtigingen heeft voor uw BLOB-container of vanwege opslag-of netwerk problemen. Als aangepaste installatie is geslaagd, is deze fout niet van invloed op een SSIS-functie, maar de logboeken ontbreken. Als aangepaste Setup mislukt met een andere fout en het logboek niet is geüpload, wordt deze fout eerst gerapporteerd zodat het logboek kan worden geüpload voor analyse. Nadat dit probleem is opgelost, worden er ook meer specifieke problemen gerapporteerd. Als dit probleem niet wordt opgelost na een nieuwe poging, neemt u contact op met het Azure Data Factory-ondersteunings team.

## <a name="virtual-network-configuration"></a>Configuratie van virtueel netwerk

Wanneer u SSIS IR koppelt aan Azure Virtual Network, maakt SSIS IR gebruik van het virtuele netwerk onder het gebruikers abonnement. Zie [een Azure-SSIS Integration runtime toevoegen aan een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)voor meer informatie.

Wanneer er een probleem is met Virtual Network, wordt een van de volgende fouten weer geven.

### <a name="invalidvnetconfiguration"></a>InvalidVnetConfiguration

Deze fout kan om verschillende redenen optreden. Zie de secties [verboden](#forbidden), [InvalidPropertyValue](#invalidpropertyvalue)en [MisconfiguredDnsServerOrNsgSettings](#misconfigureddnsserverornsgsettings) om het probleem op te lossen.

### <a name="forbidden"></a>Verboden

Dit soort fout kan er als volgt uitzien: ' SubnetId is niet ingeschakeld voor het huidige account. De resource provider micro soft. batch is niet geregistreerd onder hetzelfde abonnement van VNet. "

Deze details betekenen dat Azure Batch geen toegang hebt tot uw virtuele netwerk. Registreer de resource provider micro soft. batch onder hetzelfde abonnement als Virtual Network.

### <a name="invalidpropertyvalue"></a>InvalidPropertyValue

Dit type fout kan in een van de volgende gevallen worden weer gegeven: 

- "Het opgegeven VNet bestaat niet of de batch-service heeft er geen toegang toe."
- "Het opgegeven subnet xxx bestaat niet."

Deze fouten betekenen dat het virtuele netwerk niet bestaat, dat de Azure Batch-service geen toegang heeft, of dat het gegeven subnet niet bestaat. Zorg ervoor dat het virtuele netwerk en het subnet bestaan en dat Azure Batch toegang hebben tot deze.

### <a name="misconfigureddnsserverornsgsettings"></a>MisconfiguredDnsServerOrNsgSettings

Dit type fout bericht kan er als volgt uitzien: Kan geen Integration Runtime inrichten in VNet. Als de DNS-server-of NSG-instellingen zijn geconfigureerd, controleert u of de DNS-server toegankelijk is en of NSG juist is geconfigureerd. "

In dit geval hebt u waarschijnlijk een aangepaste configuratie van de DNS-server-of NSG-instellingen, waardoor wordt voor komen dat de naam van de Azure-server die wordt vereist door SSIS IR, niet kan worden omgezet of geopend. Zie [SSIS IR Virtual Network-configuratie](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)voor meer informatie. Neem contact op met het ondersteunings team van Azure Data Factory als u nog steeds problemen ondervindt.

### <a name="vnetresourcegrouplockedduringupgrade"></a>VNetResourceGroupLockedDuringUpgrade

SSIS IR wordt regel matig automatisch bijgewerkt. Er wordt een nieuwe Azure Batch pool gemaakt tijdens de upgrade en de oude Azure Batch groep wordt verwijderd. Daarnaast worden Virtual Network-gerelateerde resources voor de oude groep verwijderd en worden de nieuwe Virtual Network-gerelateerde resources gemaakt onder uw abonnement. Deze fout betekent dat het verwijderen van Virtual Network gerelateerde resources voor de oude groep is mislukt vanwege een verwijderings vergrendeling op het niveau van het abonnement of de resource groep. Omdat de klant de verwijderings vergrendeling beheert en instelt, moeten ze de verwijderings vergrendeling in deze situatie verwijderen.

### <a name="vnetresourcegrouplockedduringstart"></a>VNetResourceGroupLockedDuringStart

Als SSIS IR-inrichting mislukt, worden alle resources die zijn gemaakt, verwijderd. Als er echter een resource delete lock op het niveau van het abonnement of de resource groep is, worden Virtual Network resources niet op de verwachte manier verwijderd. U kunt deze fout oplossen door de verwijderings vergrendeling te verwijderen en de IR opnieuw op te starten.

### <a name="vnetresourcegrouplockedduringstop"></a>VNetResourceGroupLockedDuringStop

Wanneer u SSIS-IR stopt, worden alle resources die aan Virtual Network zijn gerelateerd, verwijderd. Het verwijderen kan echter mislukken als de vergren deling van een resource wordt verwijderd op het niveau van het abonnement of de resource groep. Hier wordt ook de klant bestuurd en wordt de verwijderings vergrendeling ingesteld. Daarom moeten ze de verwijderings vergrendeling verwijderen en vervolgens de SSIS-IR opnieuw stoppen.

### <a name="nodeunavailable"></a>NodeUnavailable

Deze fout treedt op wanneer IR wordt uitgevoerd en betekent dat IR een slechte status heeft gekregen. Deze fout wordt altijd veroorzaakt door een wijziging in de configuratie van de DNS-server of de NSG die de SSIS IR blokkeert om verbinding te maken met een benodigde service. Omdat de configuratie van de DNS-server en het NSG wordt beheerd door de klant, moet de klant de belemmeringen voor de blok kering op hun einde oplossen. Zie [SSIS IR Virtual Network-configuratie](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)voor meer informatie. Neem contact op met het ondersteunings team van Azure Data Factory als u nog steeds problemen ondervindt.

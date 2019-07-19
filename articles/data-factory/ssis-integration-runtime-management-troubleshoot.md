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
ms.openlocfilehash: 17fe8d9ed02156b8fe9aafd7adca946531895883
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253018"
---
# <a name="troubleshoot-ssis-integration-runtime-management-in-azure-data-factory"></a>Problemen met SSIS Integration Runtime Management in Azure Data Factory oplossen

Dit document bevat richt lijnen voor het oplossen van problemen met het beheer van SSIS-Integration Runtime (SSIS IR).

## <a name="overview"></a>Overzicht

Er wordt een fout bericht weer gegeven in de ADF-portal of het resultaat van de Power shell-cmdlet als er een probleem is bij het inrichten of het ongedaan maken van de inrichting van SSIS IR. De fout is altijd in de indeling als fout code met een gedetailleerd fout bericht.

Dit betekent dat de service enkele tijdelijke problemen ondervindt als de fout code InternalServerError is. U kunt de bewerking later opnieuw proberen. Neem contact op met het ondersteunings team van Azure Data Factory als nieuwe poging niet helpt.

Er zijn drie belang rijke externe afhankelijkheden die fouten kunnen veroorzaken: Azure SQL Database Server of een beheerd exemplaar, een aangepast installatie script en Virtual Network configuratie als de fout code niet InternalServerError is.

## <a name="azure-sql-database-server-or-managed-instance-issues"></a>Problemen met de Azure SQL Database Server of het beheerde exemplaar

Een Azure SQL Database Server of een beheerd exemplaar is vereist als het inrichten SSIS IR met de SSIS-catalogus database is. De Azure SQL Database Server of het beheerde exemplaar moet toegankelijk zijn voor de SSIS IR. Het account van de Azure SQL Database Server of het beheerde exemplaar moet gemachtigd zijn om een SSIS-catalogus database (SSISDB) te maken. Als er een fout optreedt, wordt een fout code met een gedetailleerd SQL-uitzonderings bericht weer gegeven in de ADF-Portal. Volg de onderstaande stappen om de fout codes op te lossen.

### <a name="azuresqlconnectionfailure"></a>AzureSqlConnectionFailure

Mogelijk ziet u dit probleem wanneer u een nieuwe SSIS-IR of tijdens het uitvoeren van een IR inricht.

Dit kan worden veroorzaakt door de volgende redenen als u de fout melding ziet tijdens het inrichten van IR, en u kunt een gedetailleerd SqlException-bericht ontvangen in het fout bericht.

* Probleem met de netwerk verbinding. Controleer of de hostnaam van de SQL Server of het beheerde exemplaar toegankelijk is en of er geen firewall is of NSG de SSIS-IR voor toegang tot de server.
* De aanmelding is mislukt en SQL-verificatie wordt gebruikt. Dit betekent dat het gegeven account niet kan worden aangemeld bij de SQL Server. Controleer of het juiste gebruikers account is ingesteld.
* De aanmelding is mislukt en de AAD-verificatie (beheerde identiteit) wordt gebruikt. Voeg de beheerde identiteit van uw fabriek toe aan een AAD-groep en zorg ervoor dat de beheerde identiteit toegangs machtigingen heeft voor de server van de catalogus database.
* Verbindingstime-out: dit is altijd het gevolg van een configuratie met betrekking tot beveiliging. Het is raadzaam om een nieuwe virtuele machine te maken, de virtuele machine te koppelen aan hetzelfde VNet van IR als IR zich in een VNet bevindt, vervolgens SSMS installeert en de Azure SQL Database Server of de status van het beheerde exemplaar controleren.

Raadpleeg het fout bericht Details SQL-uitzonde ring voor meer informatie en los het probleem op dat in het fout bericht wordt weer gegeven. Neem contact op met Azure SQL Database Server of het ondersteunings team van het beheerde exemplaar als u nog steeds problemen ondervindt.

Waarschijnlijk zijn er een aantal netwerk beveiligings groepen of Firewall wijzigingen als u de fout ziet tijdens het uitvoeren van IR, waardoor het SSIS IR worker-knoop punt geen toegang meer heeft tot de Azure SQL Database Server of het beheerde exemplaar. Deblokkeren van het SSIS IR worker-knoop punt om toegang te krijgen tot de Azure SQL Database Server of het beheerde exemplaar.

### <a name="catalogcapacitylimiterror"></a>CatalogCapacityLimitError

Het fout bericht is net als ' de data base ' SSISDB ' heeft het quotum voor de grootte bereikt. Partitioneer of verwijder gegevens, verwijder indexen of Raadpleeg de documentatie voor mogelijke oplossingen. " De mogelijke oplossingen zijn:
* Verhoog het quotum voor de grootte van uw SSISDB.
* Wijzig de configuratie van SSISDB om de grootte te verkleinen, bijvoorbeeld:
   * De retentie periode en het aantal project versies verminderen.
   * De retentie periode van het logboek wordt beperkt.
   * Het standaard niveau van het logboek wijzigen, enzovoort.

### <a name="catalogdbbelongstoanotherir"></a>CatalogDbBelongsToAnotherIR

Deze fout geeft aan dat de Azure SQL Database Server of het beheerde exemplaar al een SSISDB heeft gemaakt en gebruikt door een andere IR. U moet een andere Azure SQL Database Server of een beheerd exemplaar opgeven, of bestaande SSISDB verwijderen en de nieuwe IR opnieuw opstarten.

### <a name="catalogdbcreationfailure"></a>CatalogDbCreationFailure

Deze fout kan de volgende oorzaken hebben:

* Het gebruikers account dat is geconfigureerd voor de SSIS IR heeft geen machtiging voor het maken van de data base. U kunt de gebruiker toestemming geven om de data base te maken.
* Maak een time-out voor de data base, zoals time-out voor uitvoering, time-out van DB. U kunt het later opnieuw proberen om te zien of het probleem is opgelost. Neem contact op met het ondersteunings team van Azure SQL Database Server of Managed instance als nieuwe poging niet werkt.

Controleer het fout bericht van SQL-uitzonde ring voor andere problemen en los het probleem op dat in het fout bericht wordt vermeld. Als u nog steeds problemen ondervindt, neemt u contact op met Azure SQL Database Server of het ondersteunings team van het beheerde exemplaar.

### <a name="invalidcatalogdb"></a>InvalidCatalogDb

Het fout bericht lijkt op ' ongeldige object naam ' Catalog. catalog_properties '. '. Dit betekent dat u al een Data Base met de naam SSISDB hebt, maar niet gemaakt door SSIS IR, of dat de Data Base een ongeldige status heeft die wordt veroorzaakt door fouten in de laatste SSIS-IR-inrichting. U kunt bestaande data base met de naam SSISDB verwijderen of een nieuwe Azure SQL Database Server of een beheerd exemplaar configureren voor de IR.

## <a name="custom-setup"></a>Aangepaste installatie

Aangepaste installatie biedt een interface voor het toevoegen van uw eigen installatie stappen tijdens het inrichten of opnieuw configureren van uw SSIS-IR. Zie [Customize Setup voor de Azure-SSIS Integration runtime](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)voor meer informatie.

Zorg ervoor dat uw container alleen de benodigde aangepaste installatie bestanden bevat, omdat alle bestanden in de container worden gedownload naar het SSIS IR worker-knoop punt. Het is raadzaam om het aangepaste installatie script op een lokale computer te testen om eventuele problemen met het uitvoeren van scripts op te lossen voordat u het script uitvoert in SSIS IR.

De aangepaste installatie script container wordt gecontroleerd tijdens het uitvoeren van de IR, wanneer de SSIS IR regel matig wordt bijgewerkt en de container opnieuw moet worden geopend om het aangepaste installatie script te downloaden en opnieuw te installeren. De controle geeft aan of de container toegankelijk is en of het Main. cmd-bestand bestaat.

Elke fout met aangepaste installatie wordt weer gegeven met de code CustomSetupScriptFailure. Controleer het fout bericht met een sub-fout code.  Volg de onderstaande stappen om de subfout codes op te lossen.  

### <a name="customsetupscriptblobcontainerinaccessible"></a>CustomSetupScriptBlobContainerInaccessible

Dit betekent dat SSIS IR geen toegang kan krijgen tot de Azure Blob-container voor aangepaste installatie. Controleer of de SAS-URI van de container bereikbaar is en niet is verlopen.

Stop de IR eerst als de IR actief is, Configureer de IR opnieuw met de nieuwe aangepaste installatie container SAS URI en start vervolgens de IR opnieuw.

### <a name="customsetupscriptnotfound"></a>CustomSetupScriptNotFound

Dit betekent dat SSIS IR geen aangepast installatie script (Main. cmd) kan vinden in de BLOB-container. Zorg ervoor dat Main. cmd bestaat in de container. Dit is het toegangs punt voor de installatie van aangepaste installatie.

### <a name="customsetupscriptexecutionfailure"></a>CustomSetupScriptExecutionFailure

Dit betekent dat het uitvoeren van een aangepast installatie script (Main. cmd) is mislukt. u kunt het script eerst op uw lokale machine proberen of de aangepaste Setup-uitvoerings Logboeken in uw BLOB-container controleren.

### <a name="customsetupscripttimeout"></a>CustomSetupScriptTimeout

Dit betekent dat een time-out voor het uitvoeren van aangepaste installatie scripts is. Zorg ervoor dat uw BLOB-container alleen de benodigde aangepaste installatie bestanden bevat. U kunt ook aangepaste Setup-uitvoerings Logboeken in uw BLOB-container controleren. De maximum periode voor aangepaste Setup is ingesteld op 45 minuten voordat er een time-out optreedt en de maximum periode bevat de tijd voor het downloaden van alle bestanden uit uw container en het installeren ervan op SSIS IR. Als er een langere periode nodig is, verhoogt u een ondersteunings ticket.

### <a name="customsetupscriptloguploadfailure"></a>CustomSetupScriptLogUploadFailure

Dit betekent dat het uploaden van aangepaste Setup-uitvoerings logboeken naar de BLOB-container is mislukt, omdat de SSIS IR geen schrijf machtigingen heeft voor uw BLOB-container of voor bepaalde opslag-of netwerk problemen. Als aangepaste installatie is geslaagd, is deze fout niet van invloed op een SSIS-functie, maar ontbreken er logboeken. Als aangepaste Setup is mislukt met een andere fout en het logboek niet kan worden geüpload, wordt deze fout eerst gerapporteerd, dus het logboek kan worden geüpload voor analyse en nadat dit probleem is opgelost, zullen we meer opgegeven problemen melden. Als dit probleem niet wordt opgelost na een nieuwe poging, neemt u contact op met Azure Data Factory ondersteunings team.

## <a name="virtual-network-configuration"></a>Configuratie van virtueel netwerk

Wanneer u SSIS IR koppelt aan een Virtual Network (VNet), wordt het VNet gebruikt onder gebruikers abonnement. Zie [een Azure-SSIS-integratie-runtime toevoegen aan een virtueel netwerk](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)voor meer informatie.

Wanneer er sprake is van een VNet-probleem, wordt de volgende fout weer gegeven:

### <a name="invalidvnetconfiguration"></a>InvalidVnetConfiguration

Dit kan worden veroorzaakt door variant-redenen. Volg de onderstaande stappen om de subfout codes op te lossen.

### <a name="forbidden"></a>Verboden

Het fout bericht lijkt alsof subnetId niet is ingeschakeld voor het huidige account. De resource provider micro soft. batch is niet geregistreerd onder hetzelfde abonnement van VNet. "

Dit betekent dat Azure Batch geen toegang hebt tot uw VNet. Registreer de resource provider micro soft. batch onder hetzelfde abonnement van VNet.

### <a name="invalidpropertyvalue"></a>InvalidPropertyValue

Het fout bericht heeft de volgende strekking: het opgegeven VNet bestaat niet of de batch-service heeft geen toegang tot het bestand of het opgegeven subnet xxx bestaat niet.

Dit betekent dat het VNet niet bestaat of Azure Batch service geen toegang heeft, of dat het gegeven subnet niet bestaat. Zorg ervoor dat het VNet en het subnet bestaan en dat Azure Batch toegang hebben tot deze gebruikers.

### <a name="misconfigureddnsserverornsgsettings"></a>MisconfiguredDnsServerOrNsgSettings

Het bericht lijkt het inrichten van Integration Runtime in Vnet is mislukt. Als de DNS-server-of NSG-instellingen zijn geconfigureerd, controleert u of de DNS-server toegankelijk is en of NSG juist is geconfigureerd.

Waarschijnlijk hebt u een aangepaste configuratie van de DNS-server-of NSG-instellingen, waardoor de Azure-server naam vereist door SSIS IR niet kan worden omgezet of niet kan worden geopend. Zie voor meer informatie [SSIS IR VNet-configuratie](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) document. Als u nog steeds problemen ondervindt, neemt u contact op met Azure Data Factory ondersteunings team.

### <a name="vnetresourcegrouplockedduringupgrade"></a>VNetResourceGroupLockedDuringUpgrade

SSIS IR wordt regel matig automatisch bijgewerkt en er wordt een nieuwe Azure Batch groep tijdens de upgrade gemaakt en er wordt een oude Azure Batch pool verwijderd, de aan VNet gerelateerde resource voor de oude groep wordt verwijderd en er wordt een nieuwe VNet-gerelateerde resource gemaakt onder uw abonnees. Deze fout betekent dat er geen VNet-gerelateerde resource voor de oude groep kan worden verwijderd vanwege een verwijderings vergrendeling op het niveau van de abonnement of de resource groep. Help bij het verwijderen van de verwijderings vergrendeling.

### <a name="vnetresourcegrouplockedduringstart"></a>VNetResourceGroupLockedDuringStart

SSIS-IR-inrichting kan om verschillende redenen mislukken. als er een fout optreedt, worden alle gemaakte resources verwijderd. VNet-resources kunnen echter niet worden verwijderd vanwege een resource verwijderings vergrendeling op het niveau van de abonnement of de resource groep. Verwijder de verwijderings vergrendeling en start de IR opnieuw.

### <a name="vnetresourcegrouplockedduringstop"></a>VNetResourceGroupLockedDuringStop

Wanneer de SSIS IR wordt gestopt, worden alle resources die aan VNet zijn gerelateerd, verwijderd, maar kan de verwijdering niet ongedaan worden gemaakt, omdat er een resource verwijderings vergrendeling is op het niveau van de abonnee of de resource groep. Help bij het verwijderen van de verwijderings vergrendeling en probeer het opnieuw.

### <a name="nodeunavailable"></a>NodeUnavailable

Deze fout treedt op tijdens het uitvoeren van een IR, het betekent dat IR de status heeft voordat en de status niet in orde heeft, het is altijd door de DNS-server of de NSG-configuratie is gewijzigd en de oorzaak SSIS IR kan geen verbinding maken met de afhankelijke service, de DNS-server of de NSG-configuratie problemen oplossen Zie [SSIS IR VNet-configuratie](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)voor meer informatie. Als u nog steeds problemen ondervindt, neemt u contact op met Azure Data Factory ondersteunings team.

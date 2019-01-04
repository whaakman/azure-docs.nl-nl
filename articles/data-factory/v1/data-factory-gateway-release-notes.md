---
title: Opmerkingen bij de release voor Data Management Gateway | Microsoft Docs
description: Releaseopmerkingen voor Data Management Gateway-loop
services: data-factory
author: nabhishek
manager: craigg
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 865bfdae199bca7ebee888be527db239d34511d1
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017613"
---
# <a name="release-notes-for-data-management-gateway"></a>Releaseopmerkingen voor Data Management Gateway
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [zelf-hostende integratieruntime in V2](../create-self-hosted-integration-runtime.md).

Een van de uitdagingen voor de integratie van moderne gegevens is om gegevens te verplaatsen naar en van on-premises naar de cloud. Data Factory kunt u deze integratie met Data Management Gateway, die een agent die u on-premises naar hybride verplaatsing van gegevens inschakelen kan installeren.

Zie de volgende artikelen voor gedetailleerde informatie over Data Management Gateway en het gebruik ervan:

*  [Gegevensbeheergateway](data-factory-data-management-gateway.md)
*  [Gegevens verplaatsen tussen on-premises en in de cloud met Azure Data Factory](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version"></a>HUIDIGE VERSIE 
We onderhouden de Release-opmerkingen niet meer. Ophalen van de meest recente releaseopmerkingen [hier](https://go.microsoft.com/fwlink/?linkid=853077)




## <a name="earlier-versions"></a>Eerdere versies
## <a name="21063477"></a>2.10.6347.7
### <a name="enhancements-"></a>Verbeteringen:
- U kunt DNS-vermeldingen toevoegen aan whitelist servicebus in plaats van in de whitelist aan alle Azure-IP-adressen van uw firewall (indien nodig). U kunt de respectieve DNS-vermelding vinden in Azure portal (Data Factory -> 'Maken en implementeren' -> 'Gateways' serviceurls ' op' (in JSON) ->
- HDFS-connector ondersteunt nu zelfondertekend openbaar certificaat doordat u de SSL-validatie overslaan.
- Opgelost: Probleem met de gateway offline tijdens een update (als gevolg van tijdsverschil)


## <a name="2963132"></a>2.9.6313.2
### <a name="enhancements-"></a>Verbeteringen:
-   U kunt toevoegen DNS-vermeldingen aan lijst met geaccepteerde Service Bus in plaats van in de whitelist aan alle Azure-IP-adressen van uw firewall (indien nodig). Hier voor meer informatie.
-   U kunt nu gegevens kopiëren naar/van één blok-blob tot 4.75 TB, dit de maximale ondersteunde grootte van blok-blob is. (de eerdere limiet was 195 GB).
-   Opgelost: Onvoldoende geheugen-probleem tijdens het uitpakken van verschillende kleine bestanden tijdens de kopieeractiviteit.
-   Opgelost: Index is buiten het bereik probleem tijdens het kopiëren van Document DB naar een on-premises SQL Server met de functie idempotentie.
-   Opgelost: SQL-script voor opschoning werkt niet met on-premises SQL Server van de Wizard kopiëren.
-   Opgelost: De naam van de kolom met ruimte aan het einde werkt niet in de kopieeractiviteit.

## <a name="28662833"></a>2.8.66283.3
### <a name="enhancements-"></a>Verbeteringen:
- Opgelost: Probleem met de referenties voor de gateway de computer moet opnieuw ontbreekt.
- Opgelost: Probleem met de registratie bij de gateway herstellen met behulp van een back-upbestand.


## <a name="2762401"></a>2.7.6240.1
### <a name="enhancements-"></a>Verbeteringen:
- Opgelost: Onjuiste lezen van decimaal null-waarde van Oracle als bron.

## <a name="2661922"></a>2.6.6192.2
### <a name="whats-new"></a>Nieuwe functies
- Klanten kunnen feedback geven over de gateway registreren ervaring.
- Ondersteuning voor een nieuwe compressie-indeling: ZIP (verkleinen)

### <a name="enhancements-"></a>Verbeteringen:
- Prestatieverbetering van de voor de Oracle-Sink, HDFS-bron.
- Opgelost probleem voor gateway automatisch bijwerken, gateway parallelle verwerking van capaciteit.


## <a name="2561641"></a>2.5.6164.1
### <a name="enhancements"></a>Verbeteringen
- Betere en krachtigere Gateway registratie-ervaring: nu kunt u de voortgang weergegeven tijdens het registratieproces Gateway, waardoor de registratie-ervaring nog sneller bijhouden.
- Verbetering van de Gateway herstellen proces - u kunt nog steeds gateway herstellen, zelfs als u de back-upbestand van de gateway met deze update geen hebt. Dit moet u referenties van de gekoppelde Service in de Portal opnieuw instellen.
- Opgelost probleem.

## <a name="2461511"></a>2.4.6151.1

### <a name="whats-new"></a>Nieuwe functies

- U kunt nu referenties voor gegevensbron lokaal opslaan. De referenties zijn versleuteld. De referenties voor gegevensbron kunnen worden hersteld en hersteld met behulp van de back-upbestand die kan worden geëxporteerd uit de bestaande Gateway, alle on-premises.

### <a name="enhancements-"></a>Verbeteringen:

- Betere en krachtigere Gateway registratie-ervaring.
- Ondersteuning voor automatische detectie van de configuratie van QuoteChar voor tekst zonder opmaak in de wizard kopiëren en verbeteren van de nauwkeurigheid van de algemene indeling van detectie.

## <a name="2361002"></a>2.3.6100.2

- Ondersteuning van firstRowAsHeader en SkipLineCount automatische detectie in de wizard kopiëren voor de tekstbestanden in on-premises bestandssysteem en HDFS.
- Verbeter de stabiliteit van de netwerkverbinding tussen de gateway en Service Bus
- Een aantal problemen opgelost


## <a name="2260721"></a>2.2.6072.1

*  Ondersteunt het HTTP-proxy voor de gateway met behulp van de Gateway Configuration Manager instellen. Als geconfigureerd, zijn Azure Blob-, Azure Table-, Azure Data Lake- en Document DB toegankelijk via HTTP-proxy.
*  Ondersteunt header verwerking voor TextFormat bij het kopiëren van gegevens van/naar Azure Blob, Azure Data Lake Store, on-premises bestandssysteem, en on-premises HDFS.
*  Ondersteunt het kopiëren van gegevens uit een toevoeg-Blob en pagina-blobs, samen met de al ondersteunde blok-Blob.
*  Introduceert een nieuwe gatewaystatus **Online (beperkt)**, wat aangeeft dat de belangrijkste functionaliteit van de gateway met uitzondering van de interactieve bewerking-ondersteuning voor de Wizard kopiëren werkt.
*  Verbetert de stabiliteit van de gateway registreren met behulp van de registratiesleutel.

## <a name="216040"></a>2.1.6040.

*  DB2-stuurprogramma is nu opgenomen in de gateway-installatiepakket. U hoeft dit niet afzonderlijk te installeren.
*  DB2-stuurprogramma ondersteunt nu z/OS en DB2 voor ik (zoals / 400) samen met de platforms al ondersteund (Linux-, Unix- en Windows).
*  Ondersteunt het gebruik van Azure Cosmos DB als een bron of bestemming voor on-premises gegevensopslagexemplaren
*  Ondersteunt het kopiëren van gegevens van/naar koud / ' hot ' blob-opslag samen met de al ondersteund opslagaccount voor algemeen gebruik.
*  Hiermee kunt u verbinding maken met on-premises SQL Server via een gateway met bevoegdheden van de externe aanmelding.  

## <a name="2060131"></a>2.0.6013.1

*  U kunt de taal/cultuur moet worden gebruikt door een gateway tijdens de handmatige installatie.

*  Wanneer de gateway werkt niet zoals verwacht, kunt u de gatewaylogboeken van de afgelopen zeven dagen naar Microsoft te sturen om het oplossen van het probleem mogelijk te maken. Als de gateway is niet verbonden met de cloudservice, kunt u opslaan en gateway-logboeken te archiveren.  

*  Verbeteringen in de gebruikersinterface voor gateway configuration manager:

    *  Gatewaystatus beter zichtbaar maken op het tabblad Start.

    *  Ingedeeld en vereenvoudigd besturingselementen.

    *  U kunt gegevens kopiëren vanuit een opslag met de [hulpprogramma voor kopiëren zonder code](data-factory-copy-data-wizard-tutorial.md). Zie [gefaseerd kopiëren](data-factory-copy-activity-performance.md#staged-copy) voor meer informatie over deze functie in het algemeen.
*  U kunt Data Management Gateway aan inkomende gegevens rechtstreeks vanuit een on-premises SQL Server-database gebruiken in Azure Machine Learning.

*  Verbeterde prestaties

    * Verbeter prestaties Schema/Preview op basis van SQL Server wordt weergegeven in het hulpprogramma voor kopiëren zonder code.

## <a name="11259531"></a>1.12.5953.1

*  Opgeloste fouten

## <a name="11159181"></a>1.11.5918.1

*  Maximale grootte van het gebeurtenislogboek van de gateway is verhoogd van 1 MB tot 40 MB.

*  Een dialoogvenster met waarschuwing wordt weergegeven als een herstart nodig is tijdens de gateway automatisch bijwerken. U kunt direct vervolgens of later opnieuw opstarten.

*  Als automatisch bijwerken is mislukt, probeert installatieprogramma van de gateway opnieuw drie keer op maximaal automatisch bijwerken.

*  Verbeterde prestaties

    * Verbeter prestaties voor grote tabellen laden uit on-premises server in een scenario zonder code kopiëren.

*  Opgeloste fouten

## <a name="11058921"></a>1.10.5892.1

*  Verbeterde prestaties

*  Opgeloste fouten

## <a name="1958652"></a>1.9.5865.2

*  Nul mogelijkheid touch automatisch bijwerken
*  Nieuwe pictogram in systeemvak voor met de gateway de statusindicatoren
*  Mogelijkheid om te 'Update nu' van de client
*  Mogelijkheid om in te stellen van de tijd voor het plannen van update
*  PowerShell-script voor het omzetten van automatische updates in-of uitschakelen
*  Ondersteuning voor JSON-indeling  
*  Verbeterde prestaties
*  Opgeloste fouten

## <a name="1858221"></a>1.8.5822.1

*  Verbeter de ervaring van het oplossen van problemen
*  Verbeterde prestaties
*  Opgeloste fouten

### <a name="1757951"></a>1.7.5795.1

*  Verbeterde prestaties
*  Opgeloste fouten

### <a name="1757641"></a>1.7.5764.1

*  Verbeterde prestaties
*  Opgeloste fouten

### <a name="1657351"></a>1.6.5735.1

*  Ondersteuning voor on-premises HDFS bron/Sink
*  Verbeterde prestaties
*  Opgeloste fouten

### <a name="1656961"></a>1.6.5696.1

*  Verbeterde prestaties
*  Opgeloste fouten

### <a name="1656761"></a>1.6.5676.1

*  Ondersteuning voor diagnostische hulpprogramma's in Configuration Manager
*  Kolommen in de tabel ondersteuning voor gegevensbronnen in tabelvorm voor Azure Data Factory
*  SQL DW-ondersteuning voor Azure Data Factory
*  Reclusive in BlobSource en FileSource ondersteuning voor Azure Data Factory
*  Ondersteuning voor CopyBehavior – MergeFiles PreserveHierarchy en FlattenHierarchy in BlobSink en FileSink met binaire kopie voor Azure Data Factory
*  Ondersteuning voor Kopieeractiviteit voortgangsrapportage voor Azure Data Factory
*  Ondersteuning voor bron connectiviteit gegevensvalidatie voor Azure Data Factory
*  Opgeloste fouten

### <a name="1656721"></a>1.6.5672.1

*  Naam van de tabel ondersteuning voor ODBC-gegevensbron voor Azure Data Factory
*  Verbeterde prestaties
*  Opgeloste fouten

### <a name="1656581"></a>1.6.5658.1

*  Sink-ondersteuningsbestand voor Azure Data Factory
*  Ondersteuning voor het behouden van hiërarchie in binaire kopie voor Azure Data Factory
*  Copy-activiteit Idempotentie ondersteuning voor Azure Data Factory
*  Opgeloste fouten

### <a name="1656401"></a>1.6.5640.1

*  Ondersteuning voor 3 meer gegevensbronnen voor Azure Data Factory (ODBC, OData, HDFS)
*  Aanhalingsteken in csv-parser ondersteuning voor Azure Data Factory
*  Compressieondersteuning (BZip2)
*  Opgeloste fouten

### <a name="1556121"></a>1.5.5612.1

*  Vijf relationele databases ondersteunen voor Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata en Sybase)
*  Compressieondersteuning (Gzip en Deflate)
*  Verbeterde prestaties
*  Opgeloste fouten

### <a name="1455491"></a>1.4.5549.1

*  Oracle data source-ondersteuning voor Azure Data Factory toevoegen
*  Verbeterde prestaties
*  Opgeloste fouten

### <a name="1454921"></a>1.4.5492.1

*  Geïntegreerde binaire die ondersteuning biedt voor zowel Microsoft Azure Data Factory en Power BI voor Office 365-services
*  Het proces van configuratie-UI en registratie verfijnen
*  Azure Data Factory – Azure inkomend en uitgaand verkeer ondersteuning voor SQL Server-gegevensbron

### <a name="1253031"></a>1.2.5303.1

*  Probleem met time-out ter ondersteuning van meer tijdrovende gegevensbronverbindingen.

### <a name="1155268"></a>1.1.5526.8

*  Vereist .NET Framework 4.5.1 als een vereiste tijdens de installatie.

### <a name="1051442"></a>1.0.5144.2

*  Er zijn geen wijzigingen die invloed hebben op scenario's voor Azure Data Factory.

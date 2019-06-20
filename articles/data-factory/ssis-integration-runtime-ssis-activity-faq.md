---
title: Het pakket kan worden uitgevoerd in de SSIS-integratieruntime oplossen | Microsoft Docs
description: In dit artikel bevat de richtlijnen voor probleemoplossing voor de uitvoering van SSIS-pakket in de SSIS-Integratieruntime
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/15/2019
author: wenjiefu
ms.author: wenjiefu
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: 7789970b47f0e55adee5bbe9da9f303aee6cdb25
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190127"
---
# <a name="troubleshooting-package-execution-in-ssis-integration-runtime"></a>Het pakket kan worden uitgevoerd in de SSIS-integratieruntime oplossen

Dit artikel bevat de meest voorkomende fouten die u voor het uitvoeren van SSIS-pakketten in SSIS Integration Runtime, de mogelijke oorzaken en acties bereikt mogelijk voor het oplossen van fouten.

## <a name="where-can-i-find-logs-for-troubleshoot"></a>Waar vind ik logboeken voor problemen oplossen

* De ADF-Portal kan worden gebruikt om te controleren of de uitvoer van de SSIS-pakket tot uitvoering van activiteit, met inbegrip van het resultaat van uitvoering, foutberichten en bewerking-ID. Meer informatie kunnen u vinden op [de pijplijn bewaken](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline)

* De SSIS-catalogus (SSISDB) kan worden gebruikt om te controleren of de logboeken van de details voor de uitvoering. Details kunt u vinden op [Monitor uitgevoerd pakketten en andere bewerkingen](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017)

## <a name="common-errors-causes-and-solution"></a>Veelvoorkomende fouten, oorzaak en oplossing

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Foutbericht: `"Connection Timeout Expired."` of `"The service has encountered an error processing your request. Please try again."`

* Mogelijke oorzaak en de aanbevolen actie:
  * Het bron/bestemming is overbelast. Controleer de belasting op uw gegevens bron/bestemming en kijk of er voldoende capaciteit. Bijvoorbeeld, als Azure SQL wordt gebruikt, het nodig rekening houden met schaal omhoog als de Database waarschijnlijk time-out is.
  * Het netwerk tussen SSIS Integration Runtime en de bron/bestemming is instabiel, met name wanneer de verbinding regio-overschrijdende of tussen on-premises en azure is. Het wordt aangeraden om toe te passen patroon voor opnieuw proberen in SSIS-pakket door de volgende stappen uit:
    * Zorg ervoor dat uw SSIS-pakketten kunt uitvoeren op mislukte zonder neveneffect (bijvoorbeeld. verlies van gegevens, gegevens dup...)
    * Configureer de **opnieuw** en **interval voor opnieuw proberen** van SSIS-pakket-activiteit uitvoeren in het tabblad Algemeen ![eigenschappen worden ingesteld op het tabblad Algemeen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
    * Voor ADO.NET en bron-/ doel OLEDB-component, kunnen ConnectRetryCount en ConnectRetryInterval worden ingesteld in het Verbindingsbeheer in SSIS-pakket of SSIS-activiteit

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-the-following-error-message-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Foutbericht: `"ADO NET Source has failed to acquire the connection '...' with the following error message: "A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible."`

* Mogelijke oorzaak en de aanbevolen actie:
  * Dit probleem meestal betekent het bron/bestemming is niet toegankelijk is vanuit SSIS Integration Runtime, die kan worden veroorzaakt door verschillende oorzaken hebben:
    * Zorg ervoor dat u het Data bron/bestemming naam/IP-adres correct geven
    * Zorg ervoor dat de firewall correct is ingesteld
    * Zorg ervoor dat uw vNet juist is geconfigureerd als bron/bestemming van uw gegevens in on-premises.
      * U kunt controleren of het probleem van vNet-configuratie is door in te richten van een Azure-VM in hetzelfde vNet. En controleer vervolgens of de bron/bestemming kunnen worden benaderd vanaf de Azure-VM
      * U vindt meer informatie over het gebruik van vNet met SSIS Integration Runtime op [een Azure-SSIS integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md)

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-the-following-error-message-could-not-create-a-managed-connection-manager"></a>Foutbericht weergegeven: "`ADO NET Source has failed to acquire the connection '...' with the following error message: "Could not create a managed connection manager.`"

* Mogelijke oorzaak en de aanbevolen actie:
  * De ADO.NET-provider die wordt gebruikt in het pakket is niet geïnstalleerd in de SSIS-Integratieruntime. U kunt de provider installeren met behulp van de aangepaste installatie. Meer informatie over aangepaste installatie kunnen u vinden in [setup voor de Azure-SSIS integratieruntime aanpassen](how-to-configure-azure-ssis-ir-custom-setup.md)

### <a name="error-message-the-connection--is-not-found"></a>Foutbericht weergegeven: "`The connection '...' is not found`"

* Mogelijke oorzaak en de aanbevolen actie:
  * Deze fout kan zijn dat een bekend probleem in de oude versie van SSMS. Als het pakket bevat een aangepast onderdeel (bijvoorbeeld Azure-functiepakket voor SSIS of 3rd party onderdelen) die niet is geïnstalleerd op de computer waarop SSMS wordt gebruikt voor de implementatie, wordt het onderdeel wordt verwijderd door SSMS en de fout veroorzaakt. Upgrade [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) naar de nieuwste versie waarvoor het probleem is opgelost.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Foutbericht: 'Er is niet genoeg ruimte op de schijf'

* Mogelijke oorzaak en de aanbevolen actie:
  * Deze fout betekent dat de lokale schijf worden in het knooppunt voor SSIS Integration Runtime. Controleer of het pakket of een aangepaste installatie veel schijf opslagruimten wilt gebruiken.
    * Als de schijf wordt gebruikt door het pakket, wordt deze worden vrijgemaakt nadat de uitvoering van het pakket is voltooid.
    * Als de schijf wordt gebruikt door uw aangepaste installatie, moet u SSIS-Integratieruntime stoppen, het Script wijzigen en opnieuw starten van de SSIS-Integratieruntime. De hele Azure Blob-Container u hebt opgegeven voor aangepaste installatie worden gekopieerd naar SSIS-IR-knooppunt, dus controleer of er is onnodige inhoud onder die container.

### <a name="error-message-cannot-open-file-"></a>Foutbericht: 'Kan bestand niet openen '...' '

* Mogelijke oorzaak en de aanbevolen actie:
  * Deze fout treedt op wanneer de uitvoering van pakket kan bestand niet vinden in de lokale schijf in de SSIS-Integratieruntime.
    * Het wordt niet aangeraden absoluut pad gebruiken in het pakket wordt uitgevoerd in de SSIS-Integratieruntime. Huidige uitvoering werkmap (.) of de tijdelijke map (% TEMP %) gebruiken in plaats daarvan.
    * Als dat nodig is om vast te leggen van enkele bestanden van SSIS-Integratieruntime-knooppunten, het nodig om voor te bereiden van de bestanden via [Setup aanpassen](how-to-configure-azure-ssis-ir-custom-setup.md). Alle bestanden in de werkmap uitvoering worden opgeschoond nadat de uitvoering is voltooid.
    * Een andere optie is het gebruik van Azure-bestand in plaats van het bestand in SSIS Integration Runtime-knooppunt. Meer informatie kunt u vinden op [ https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares ](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares).

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Foutbericht: "De database 'SSISDB' heeft het groottequotum bereikt"

* Mogelijke oorzaak en de aanbevolen actie:
  * De SSISDB in de Azure SQL of het beheerde exemplaar wordt gemaakt bij het maken van SSIS Integration Runtime heeft het quotum bereikt.
    * Houd rekening met het verhogen van de DTU van uw Database om dit probleem te verhelpen. Meer informatie kunnen u vinden op [https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server)
    * Controleer of het pakket kan worden gebruikt voor het genereren van veel Logboeken. Als dit het geval is, kan de elastische taak worden geconfigureerd als u deze logboeken wilt opschonen. Raadpleeg [SSISDB logboeken met Azure-taken voor Elastic Database opschonen](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md) voor details.

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Foutbericht: "De aanvraaglimiet voor de database is... en is bereikt."

* Mogelijke oorzaak en de aanbevolen actie:
  * Als er veel pakketten in de SSIS-Integratieruntime parallel worden uitgevoerd, wordt deze fout kan optreden omdat de aanvraag-beperking van SSISDB is bereikt. Houd rekening met het verhogen van de DTC van uw SSISDB om dit probleem te verhelpen. Meer informatie kunnen u vinden op [https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server)

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Foutbericht: ' SSIS-bewerking is mislukt met de bewerkingsstatus van onverwachte:... '

* Mogelijke oorzaak en de aanbevolen actie:
  * De fout wordt meestal veroorzaakt doordat een tijdelijke fout, dus u kunt de uitvoering van het pakket opnieuw uit te voeren. Het wordt aangeraden om toe te passen patroon voor opnieuw proberen in SSIS-pakket door de volgende stappen uit:
    * Zorg ervoor dat uw SSIS-pakketten kunt uitvoeren op mislukte zonder neveneffect (bijvoorbeeld verlies van gegevens, gegevens dup...)
    * Configureer de **opnieuw** en **interval voor opnieuw proberen** van SSIS-pakket-activiteit uitvoeren in het tabblad Algemeen ![eigenschappen worden ingesteld op het tabblad Algemeen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
    * Voor ADO.NET en bron-/ doel OLEDB-component, kunnen ConnectRetryCount en ConnectRetryInterval worden ingesteld in het Verbindingsbeheer in SSIS-pakket of SSIS-activiteit

### <a name="error-message-there-is-no-active-worker"></a>Foutbericht: "Er is geen actieve werknemer."

* Mogelijke oorzaak en de aanbevolen actie:
  * Deze fout betekent meestal dat de SSIS Integration Runtime is in een slechte status. Controleer Azure-portal voor de status en details van fouten: [https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Foutbericht: 'Uw integratieruntime kan niet worden bijgewerkt en uiteindelijk niet meer zal werken, omdat de Azure Blob-container die u hebt opgegeven voor aangepaste installatie kan niet worden geopend.'

* Deze fout treedt op wanneer SSIS Integration Runtime heeft geen toegang de opslag die is geconfigureerd voor aangepaste installatie tot. Controleer of de SAS-Uri die u hebt opgegeven geldig is en is nog niet is verlopen.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Foutbericht: "Microsoft OLE DB-Provider voor analyseservices. ' Hresult: 0x80004005 beschrijving: " COM-fout: COM-fout: mscorlib; Het doel van een aanroep heeft een uitzondering'

* Mogelijke oorzaak en de aanbevolen actie:
  * Een mogelijke oorzaak dat gebruikersnaam en wachtwoord met MFA ingeschakeld is is geconfigureerd voor Azure Analysis Services-verificatie wordt nog niet ondersteund in de SSIS-integratieruntime. Probeer met Service-Principal voor Azure Analysis Service-verificatie:
    1. Service-principal voor AAS voorbereiden [https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal)
    2. In Verbindingsbeheer configureren "Voor het gebruik van een specifieke gebruikersnaam en wachtwoord": 'AppID' als gebruikersnaam en het 'clientSecret' als wachtwoord instellen

### <a name="package-takes-unexpected-long-time-to-execute"></a>Pakket duurt onverwachte lang om uit te voeren

* Mogelijke oorzaak en de aanbevolen actie:
  * Te veel pakket uitvoeringen zijn gepland op de SSIS-Integratieruntime. In dit geval worden alle volgende uitvoeringen worden wacht in een wachtrij voor hun inschakelen om uit te voeren.
    * Aantal van de maximum aantal parallelle uitvoeringen per IR = aantal knooppunten * Max parallelle uitvoering per knooppunt
    * Raadpleeg [Azure-SSIS-Integratieruntime maken in Azure Data Factory](create-azure-ssis-integration-runtime.md) voor het instellen van het aantal knooppunten en het maximum aantal parallelle uitvoering per knooppunt.
  * SSIS Integration Runtime is gestopt of in een slechte status. Controleer [Azure-SSIS-integratieruntime](monitor-integration-runtime.md#azure-ssis-integration-runtime) voor het controleren van de status van de SSIS-Integratieruntime en fouten.
  * Het wordt aangeraden om in te stellen de time-out voor als u zeker weet dat de uitvoering van het pakket moet is beëindigd, in een bepaalde periode: ![Eigenschappen worden ingesteld op het tabblad Algemeen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

### <a name="poor-performance-in-package-execution"></a>Slechte prestaties bij uitvoeren van een pakket

* Mogelijke oorzaak en de aanbevolen actie:

  * Controleer of de SSIS-Integratieruntime zich in dezelfde regio als de gegevensbron en bestemming.

  * Niveau van logboekregistratie voor 'Prestaties' inschakelen

      U kunt het niveau van logboekregistratie van het pakket uitvoeren naar 'Prestaties' voor het verzamelen van de duur van meer informatie voor elk onderdeel in de uitvoering van instellen. Meer informatie kunnen u vinden op: [https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging)

  * Controleer de prestaties van de IR-knooppunt in IR monitor pagina in Azure portal.
    * Over het bewaken van SSIS Integration Runtime: [Azure-SSIS-integratieruntime](monitor-integration-runtime.md#azure-ssis-integration-runtime)
    * De geschiedenis van CPU/geheugen gebruik van SSIS Integration Runtime is beschikbaar op de metrische gegevens van de Data Factory in Azure portal ![metrische gegevens van SSIS-Integratieruntime controleren](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)

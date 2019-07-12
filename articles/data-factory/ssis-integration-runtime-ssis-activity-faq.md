---
title: De uitvoering van het pakket in de SSIS-integratieruntime oplossen | Microsoft Docs
description: In dit artikel bevat richtlijnen voor probleemoplossing voor de uitvoering van SSIS-pakket in de SSIS-integratieruntime
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
ms.openlocfilehash: 05723a90725992e6b955524a2d35c82d3378ee3d
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621844"
---
# <a name="troubleshoot-package-execution-in-the-ssis-integration-runtime"></a>De uitvoering van het pakket in de SSIS-integratieruntime oplossen

Dit artikel bevat de meest voorkomende fouten die u kunnen bij het uitvoeren van pakketten van SQL Server Integration Services (SSIS) bent u in de SSIS-integratieruntime. Het beschrijft de mogelijke oorzaken en acties om op te lossen de fouten.

## <a name="where-to-find-logs-for-troubleshooting"></a>Waar vind ik logboeken voor het oplossen van problemen

De Azure Data Factory-portal gebruiken om te controleren of de uitvoer van de SSIS-pakket execution-activiteit. De uitvoer bevat het resultaat van uitvoering, foutberichten en bewerking-ID. Zie voor meer informatie, [bewaken van de pijplijn](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline).

Gebruik de SSIS-catalogus (SSISDB) om te controleren of de logboeken van de details voor de uitvoering. Zie voor meer informatie, [Monitor uitgevoerd pakketten en andere bewerkingen](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017).

## <a name="common-errors-causes-and-solutions"></a>Veelvoorkomende fouten, oorzaken en oplossingen

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Foutbericht: 'De time-out van de verbinding is verlopen' of 'in de service heeft een fout bij het verwerken van uw aanvraag aangetroffen. Probeer het opnieuw."

Hier volgen mogelijke oorzaken en aanbevolen acties:
* De gegevensbron of de bestemming is overbelast. Controleer de belasting op de gegevensbron of het doel en kijk of er voldoende capaciteit. Bijvoorbeeld, als u Azure SQL Database gebruikt, kunt u omhoog schalen als de database waarschijnlijk time-out is.
* Het netwerk tussen de SSIS integratieruntime en de gegevensbron of de bestemming is instabiel, met name wanneer de verbinding regio-overschrijdende of tussen on-premises en Azure is. Het patroon opnieuw proberen in de SSIS-pakket van toepassing door de volgende stappen:
  * Zorg ervoor dat uw SSIS-pakketten kunnen uitvoeren op mislukte zonder neveneffecten (bijvoorbeeld verlies van gegevens of gegevensontdubbeling).
  * Configureren van **opnieuw** en **interval voor opnieuw proberen** van **SSIS-pakket uitvoeren** activiteit op de **algemene** tabblad. ![Eigenschappen worden ingesteld op het tabblad Algemeen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
  * Voor een ADO.NET- en OLE DB bron- of -component, stel **ConnectRetryCount** en **ConnectRetryInterval** in Verbindingsbeheer in het SSIS-pakket of de SSIS-activiteit.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Foutbericht: 'ADO NET bron is te verkrijgen van de verbinding '...' mislukt' met ' een netwerk- of instantiefout is fout opgetreden tijdens het maken van een verbinding met SQL Server. De server is niet gevonden of niet toegankelijk was."

Dit probleem betekent meestal dat de gegevensbron of bestemming is niet toegankelijk is vanaf de SSIS-integratieruntime. De redenen kunnen variëren. Probeer deze acties:
* Zorg ervoor dat u de gegevensbron of de bestemming geven servernaam/IP correct.
* Zorg ervoor dat de firewall correct is ingesteld.
* Zorg ervoor dat uw virtuele netwerk juist is geconfigureerd als de gegevensbron en bestemming zich on-premises:
  * U kunt controleren of het probleem van de configuratie van het virtuele netwerk is door het inrichten van een Azure-VM in hetzelfde virtuele netwerk. Controleer vervolgens of de gegevensbron of de bestemming kan worden benaderd vanaf de Azure-VM.
  * U vindt meer informatie over het gebruik van een virtueel netwerk met een SSIS-integratieruntime in [een Azure-SSIS integratieruntime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md).

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-could-not-create-a-managed-connection-manager"></a>Foutbericht: 'ADO NET bron is te verkrijgen van de verbinding '...' mislukt' met 'Kan niet maken een beheerde connection manager'.

De mogelijke oorzaak is dat de ADO.NET-provider die wordt gebruikt in het pakket is niet geïnstalleerd in de SSIS-integratieruntime. U kunt de provider installeren met behulp van een aangepaste installatie. U vindt meer informatie over aangepaste instellingen in [setup voor de Azure-SSIS integratieruntime aanpassen](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="error-message-the-connection--is-not-found"></a>Foutbericht: "De verbinding '...' is niet gevonden'

Een bekend probleem in oudere versies van SQL Server Management Studio (SSMS) kan leiden tot deze fout. Als het pakket een aangepast onderdeel (bijvoorbeeld Azure-functiepakket voor SSIS of partner onderdelen) die niet is geïnstalleerd op de computer waarop SSMS wordt gebruikt bevat voor de implementatie, wordt SSMS verwijderen van het onderdeel en de fout veroorzaakt. Upgrade [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) naar de nieuwste versie waarvoor het probleem is opgelost.

### <a name="error-messagessis-executor-exit-code--1073741819"></a>Foutmelding: ' SSIS Executor afsluitcode:-1073741819. "

* Mogelijke oorzaak en de aanbevolen actie:
  * Deze fout kan zijn vanwege de beperking voor de Excel-bron en bestemming als meerdere gegevensbronnen van Excel of -doelen, worden uitgevoerd parallel in meerdere thread. U kunt deze beperking door wijzigen de Excel-componenten in volgorde uitvoeren, of dat ze in verschillende pakketten en trigger via 'Uitvoeren taak pakket' ExecuteOutOfProcess-eigenschap is ingesteld als ' True ' tijdelijke oplossing.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Foutbericht: 'Er is niet genoeg ruimte op de schijf'

Deze fout betekent dat de lokale schijf in het knooppunt voor SSIS integration runtime is verbruikt. Controleer of het pakket of een aangepaste installatie veel schijfruimte verbruikt:
* Als de schijf wordt gebruikt door het pakket, wordt deze worden vrijgemaakt nadat de uitvoering van het pakket is voltooid.
* Als de schijf wordt gebruikt door uw aangepaste installatie, moet u stoppen van de SSIS-integratieruntime wijzigen van het script, en de integratieruntime opnieuw te starten. De gehele Azure blob-container die u hebt opgegeven voor aangepaste installatie worden gekopieerd naar de SSIS-knooppunt voor integration runtime, dus controleer of er onnodige inhoud onder die container.

### <a name="error-message-failed-to-retrieve-resource-from-master-microsoftsqlserverintegrationservicesscalescaleoutcontractcommonmasterresponsefailedexception-code300004-descriptionload-file--failed"></a>Foutbericht: 'Kan geen resource in de mastervertakking ophalen. Microsoft.SqlServer.IntegrationServices.Scale.ScaleoutContract.Common.MasterResponseFailedException: Code:300004. Beschrijving: laden bestand ' *** "is mislukt."

* Mogelijke oorzaak en de aanbevolen actie:
  * Als de SSIS-activiteit wordt uitgevoerd van pakket van het bestandssysteem (pakketbestand of project-bestand), treedt deze fout op als het project, pakket of de configuratie-bestand is niet toegankelijk met de referenties van het pakket toegang die u hebt opgegeven in de SSIS-activiteit
    * Als u Azure File:
      * Het pad moet beginnen met \\ \\ \<opslagaccountnaam\>. file.core.windows.net\\\<pad naar bestandsshare\>
      * Het domein moet 'Azure'
      * De gebruikersnaam moet \<storage-accountnaam\>
      * Het wachtwoord moet \<toegangssleutel voor opslag\>
    * Als u met behulp van on-premises bestandsserver, Controleer of als VNet, pakket toegang referenties en machtigingen juist zijn geconfigureerd zodat uw Azure-SSIS integratieruntime toegang heeft tot uw on-premises-bestandsshare

### <a name="error-message-the-file-name--specified-in-the-connection-was-not-valid"></a>Foutbericht: 'Naam van het bestand '...' opgegeven in de verbinding is niet geldig "

* Mogelijke oorzaak en de aanbevolen actie:
  * Er is een ongeldige bestandsnaam opgegeven
  * Zorg ervoor dat u met de FQDN-naam (Fully Qualified Domain Name) in plaats van korte tijd uw Verbindingsbeheer

### <a name="error-message-cannot-open-file-"></a>Foutbericht: 'Kan bestand niet openen '...' '

Deze fout treedt op wanneer pakketuitvoering van een bestand niet in de lokale schijf in de SSIS-integratieruntime vinden. Probeer deze acties:
* Gebruik het absolute pad niet in het pakket dat wordt uitgevoerd in de SSIS-integratieruntime. Gebruik de huidige werkmap van uitvoering (.) of de tijdelijke map (% TEMP %) in plaats daarvan.
* Als u nodig hebt om vast te leggen van sommige bestanden op de knooppunten van SSIS integration runtime, voorbereiden van de bestanden zoals beschreven in [setup aanpassen](how-to-configure-azure-ssis-ir-custom-setup.md). Alle bestanden in de werkmap worden opgeschoond nadat de uitvoering is voltooid.
* Azure Files gebruiken in plaats van het bestand in het knooppunt voor SSIS integration runtime. Zie voor meer informatie, [gebruik Azure-bestandsshares](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares).

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Foutbericht: "De database 'SSISDB' heeft het groottequotum bereikt"

Een mogelijke oorzaak is dat de SSISDB-database in de Azure SQL-database of een beheerd exemplaar gemaakt wanneer u een SSIS integratieruntime maakt het quotum heeft bereikt. Probeer deze acties:
* Houd rekening met het verhogen van de DTU van uw database. U vindt de details in [SQL-Database resourcelimieten voor Azure SQL Database-server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server).
* Controleer of het pakket kan worden gebruikt voor het genereren van veel Logboeken. Als dit het geval is, kunt u een elastische taak voor het opschonen van deze logboeken kunt configureren. Zie voor meer informatie, [opschonen van logboeken met taken voor Elastic Database van Azure SSISDB](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md).

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Foutbericht: "De aanvraaglimiet voor de database is... en is bereikt."

Als er veel pakketten in de SSIS-integratieruntime parallel worden uitgevoerd, wordt deze fout kan optreden omdat SSISDB de aanvraaglimiet is bereikt. Houd rekening met het verhogen van de DTC van SSISDB als dit probleem wilt oplossen. U vindt de details in [SQL-Database resourcelimieten voor Azure SQL Database-server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server).

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Foutbericht: ' SSIS-bewerking is mislukt met de bewerkingsstatus van onverwachte:... '

De fout wordt meestal veroorzaakt door een tijdelijk probleem, dus u kunt de uitvoering van het pakket opnieuw uit te voeren. Het patroon opnieuw proberen in de SSIS-pakket van toepassing door de volgende stappen:

* Zorg ervoor dat uw SSIS-pakketten kunnen uitvoeren op mislukte zonder neveneffecten (bijvoorbeeld verlies van gegevens of gegevensontdubbeling).
* Configureren van **opnieuw** en **interval voor opnieuw proberen** van **SSIS-pakket uitvoeren** activiteit op de **algemene** tabblad. ![Eigenschappen worden ingesteld op het tabblad Algemeen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
* Voor een ADO.NET- en OLE DB bron- of -component, stel **ConnectRetryCount** en **ConnectRetryInterval** in Verbindingsbeheer in het SSIS-pakket of de SSIS-activiteit.

### <a name="error-message-there-is-no-active-worker"></a>Foutbericht: "Er is geen actieve werknemer."

Deze fout betekent meestal dat de SSIS integratieruntime heeft een slechte status. Controleer de Azure-portal voor de status en de gedetailleerde fouten. Zie voor meer informatie, [Azure-SSIS-integratieruntime](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Foutbericht: 'Uw integratieruntime kan niet worden bijgewerkt en uiteindelijk niet meer zal werken, omdat de Azure Blob-container die u hebt opgegeven voor aangepaste installatie kan niet worden geopend.'

Deze fout treedt op wanneer de SSIS integratieruntime heeft geen toegang de opslag die is geconfigureerd voor aangepaste installatie tot. Controleer of de shared access signature (SAS) URI die u hebt opgegeven geldig is en is nog niet is verlopen.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Foutbericht: "Microsoft OLE DB-Provider voor analyseservices. ' Hresult: 0x80004005 beschrijving: " COM-fout: COM-fout: mscorlib; Het doel van een aanroep heeft een uitzondering'

Een mogelijke oorzaak is dat de gebruikersnaam of het wachtwoord met Azure multi-factor Authentication is ingeschakeld voor Azure Analysis Services-verificatie is geconfigureerd. Deze verificatie wordt niet ondersteund in de SSIS-integratieruntime. Probeer met een service-principal voor Azure Analysis Services-verificatie:
1. Bereid een service-principal zoals beschreven in [automatisering met service-principals](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal).
2. In Verbindingsbeheer configureren **gebruik een specifieke gebruikersnaam en wachtwoord**: Stel **AppID** als de gebruikersnaam en **clientSecret** als het wachtwoord.

### <a name="error-message-adonet-source-has-failed-to-acquire-the-connection-guid-with-the-following-error-message-login-failed-for-user-nt-authorityanonymous-logon-when-using-a-managed-identity"></a>Foutbericht: "ADONET bron is mislukt voor het verkrijgen van de verbinding {GUID} met de volgende strekking weergegeven: Aanmelding voor gebruiker 'NT AUTHORITY\ANONIEME LOGON' mislukt ' bij het gebruik van een beheerde identiteit

Zorg ervoor dat u configureert u de verificatiemethode van Connection Manager als geen **Active Directory-wachtwoordverificatie** wanneer de parameter *ConnectUsingManagedIdentity* is **True** . U kunt deze configureren als **SQL-verificatie** in plaats daarvan, die wordt genegeerd als *ConnectUsingManagedIdentity* is ingesteld.

### <a name="package-execution-takes-too-long"></a>Uitvoering van pakket duurt te lang

Hier volgen mogelijke oorzaken en aanbevolen acties:
* Te veel pakket uitvoeringen zijn gepland op de SSIS-integratieruntime. Alle deze uitvoeringen moet wachten tot in een wachtrij hun inschakelen.
  * Het maximum wordt bepaald met behulp van deze formule: 
    
    Aantal van de maximum aantal parallelle uitvoeringen per IR = aantal knooppunten * Max parallelle uitvoering per knooppunt
  * Zie voor meer informatie over het instellen van het aantal knooppunten en het maximum aantal parallelle uitvoering per knooppunt, [maken van een Azure-SSIS integratieruntime in Azure Data Factory](create-azure-ssis-integration-runtime.md).
* De SSIS integratieruntime is gestopt of een slechte status heeft. Zie voor informatie over het controleren van de status van SSIS-integratie-runtime en -fouten, [Azure-SSIS-integratieruntime](monitor-integration-runtime.md#azure-ssis-integration-runtime).

We raden u ook aan dat u een time-out ingesteld op de **algemene** tabblad: ![Eigenschappen worden ingesteld op het tabblad Algemeen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png).

### <a name="poor-performance-in-package-execution"></a>Slechte prestaties bij uitvoeren van een pakket

Probeer deze acties:

* Zorg ervoor dat de SSIS-integratieruntime in dezelfde regio als de gegevensbron en bestemming.

* Stel het niveau van logboekregistratie van de uitvoering van het pakket naar **prestaties** van de duurgegevens voor elk onderdeel in de uitvoering te verzamelen. Zie voor meer informatie, [Integration Services (SSIS) logboekregistratie](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging).

* Controleer de prestaties van de IR-knooppunt in de Azure-portal:
  * Zie voor meer informatie over het bewaken van de SSIS-integratieruntime [Azure-SSIS-integratieruntime](monitor-integration-runtime.md#azure-ssis-integration-runtime).
  * U vindt de geschiedenis van de CPU/geheugen voor de SSIS-integratieruntime door de metrische gegevens van de data factory in Azure portal weer te geven.
    ![Metrische gegevens van de SSIS-integratieruntime controleren](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)

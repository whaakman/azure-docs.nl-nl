---
title: Problemen met de uitvoering van pakketten oplossen in SSIS Integration runtime | Microsoft Docs
description: Dit artikel bevat richt lijnen voor probleem oplossing voor het uitvoeren van SSIS-pakketten in de SSIS Integration runtime
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
ms.openlocfilehash: a7ad0f3be754029c654b04d19750aab7bbcd210d
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933633"
---
# <a name="troubleshoot-package-execution-in-the-ssis-integration-runtime"></a>Problemen met de uitvoering van pakketten oplossen in SSIS Integration runtime

In dit artikel vindt u de meest voorkomende fouten die u kunt tegen komen wanneer u SQL Server Integration Services (SSIS)-pakketten uitvoert in de SSIS Integration runtime. Hierin worden de mogelijke oorzaken en acties beschreven om de fouten op te lossen.

## <a name="where-to-find-logs-for-troubleshooting"></a>Waar vind ik logboeken voor het oplossen van problemen

Gebruik de Azure Data Factory Portal om de uitvoer van de SSIS-uitvoering van de pakketten te controleren. De uitvoer bevat het resultaat van de uitvoering, fout berichten en de bewerkings-ID. Zie [de pijp lijn controleren](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline)voor meer informatie.

Gebruik de SSIS-catalogus (SSISDB) om de detail logboeken voor de uitvoering te controleren. Zie [monitoring Running packages and other Operations](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017)(Engelstalig) voor meer informatie.

## <a name="common-errors-causes-and-solutions"></a>Veelvoorkomende fouten, oorzaken en oplossingen

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Foutbericht: "Time-out van verbinding verlopen" of "de service heeft een fout aangetroffen bij het verwerken van uw aanvraag. Probeer het opnieuw. "

Dit zijn mogelijke oorzaken en aanbevolen acties:
* De gegevens bron of het doel is overbelast. Controleer de belasting van de gegevens bron of het doel en kijk of het voldoende capaciteit heeft. Als u bijvoorbeeld Azure SQL Database hebt gebruikt, kunt u overwegen omhoog te schalen als de data base waarschijnlijk een time-out heeft.
* Het netwerk tussen de SSIS Integration runtime en de gegevens bron of het doel is Insta Biel, vooral wanneer de verbinding over meerdere regio's of tussen on-premises en Azure gaat. Pas het patroon voor opnieuw proberen in het SSIS-pakket toe door de volgende stappen uit te voeren:
  * Zorg ervoor dat uw SSIS-pakketten bij storingen zonder neven effecten (bijvoorbeeld gegevens verlies of gegevens duplicatie) opnieuw kunnen worden uitgevoerd.
  * Configureer het **interval** voor **opnieuw proberen** en de activiteit voor het **uitvoeren van SSIS-pakketten** op het tabblad **Algemeen** . ![Eigenschappen instellen op het tabblad Algemeen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
  * Stel voor een ADO.NET-en OLE DB bron-of doel onderdeel **ConnectRetryCount** en **ConnectRetryInterval** in in VERBINDINGS beheer in de SSIS-pakket-of-SSIS-activiteit.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Foutbericht: ' ADO-netwerk bron kan de verbinding '... ' niet verkrijgen met ' er is een netwerk-of instantie-specifieke fout opgetreden bij het tot stand brengen van een verbinding met SQL Server. De server is niet gevonden of niet toegankelijk. "

Dit probleem betekent meestal dat de gegevensbron of -bestemming niet toegankelijk is vanuit de SSIS-integratieruntime. De redenen kunnen variëren. Voer de volgende acties uit:
* Zorg ervoor dat u de gegevens bron of doel naam/IP juist doorgeeft.
* Controleer of de firewall juist is ingesteld.
* Zorg ervoor dat het virtuele netwerk correct is geconfigureerd als uw gegevens bron of doel locatie on-premises is:
  * U kunt controleren of het probleem afkomstig is van de configuratie van een virtueel netwerk door een Azure-VM in te richten in hetzelfde virtuele netwerk. Controleer vervolgens of de gegevens bron of het doel toegankelijk is vanaf de Azure-VM.
  * Meer informatie over het gebruik van een virtueel netwerk met SSIS Integration runtime vindt u in een [Azure-SSIS Integration runtime toevoegen aan een virtueel netwerk](join-azure-ssis-integration-runtime-virtual-network.md).

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-could-not-create-a-managed-connection-manager"></a>Foutbericht: ' ADO-netwerk bron kan de verbinding '... ' niet verkrijgen met ' kan geen beheerd verbindings beheer maken '.

De mogelijke oorzaak is dat de ADO.NET-Provider die in het pakket wordt gebruikt, niet is geïnstalleerd in de SSIS Integration runtime. U kunt de provider installeren met behulp van een aangepaste installatie. Meer informatie over aangepaste Setup vindt u in [Customize Setup voor de Azure-SSIS Integration runtime](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="error-message-the-connection--is-not-found"></a>Foutbericht: ' De verbinding '... ' is niet gevonden "

Een bekend probleem in oudere versies van SQL Server Management Studio (SSMS) kan deze fout veroorzaken. Als het pakket een aangepast onderdeel bevat (bijvoorbeeld SSIS Azure Feature Pack of partneronderdelen) dat niet is geïnstalleerd op de computer waarop SSMS wordt gebruikt om de implementatie uit te voeren, wordt het onderdeel door SSMS verwijderd met de fout als gevolg. Upgrade [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) naar de nieuwste versie waarop het probleem is opgelost.

### <a name="error-messagessis-executor-exit-code--1073741819"></a>Fout bericht: de afsluit code van de SSIS-uitvoerder:-1073741819.

* Mogelijke oorzaak & aanbevolen actie:
  * Deze fout kan zijn veroorzaakt door de beperking van de Excel-bron en-bestemming wanneer meerdere Excel-bronnen of-bestemmingen parallel in meerdere threads worden uitgevoerd. U kunt deze beperking verhelpen door de Excel-onderdelen te wijzigen zodat deze in de juiste volg orde worden uitgevoerd, of ze in verschillende pakketten te scheiden en de trigger pakket taak uitvoeren te activeren, waarbij de eigenschap ExecuteOutOfProcess is ingesteld op True.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Foutbericht: Er is onvoldoende ruimte op de schijf

Deze fout geeft aan dat de lokale schijf wordt gebruikt in het SSIS Integration runtime-knoop punt. Controleer of uw pakket of aangepaste installatie veel schijf ruimte verbruikt:
* Als de schijf wordt gebruikt door uw pakket, wordt deze vrijgemaakt nadat de uitvoering van het pakket is voltooid.
* Als de schijf wordt gebruikt door uw aangepaste installatie, moet u de SSIS-integratie-runtime stoppen, uw script wijzigen en de Integration runtime opnieuw starten. De volledige Azure Blob-container die u hebt opgegeven voor aangepaste installatie, wordt gekopieerd naar het SSIS Integration runtime-knoop punt. Controleer daarom of er overbodige inhoud onder die container staat.

### <a name="error-message-failed-to-retrieve-resource-from-master-microsoftsqlserverintegrationservicesscalescaleoutcontractcommonmasterresponsefailedexception-code300004-descriptionload-file--failed"></a>Foutbericht: Kan de bron niet ophalen uit de hoofd database. Microsoft.SqlServer.IntegrationServices.Scale.ScaleoutContract.Common.MasterResponseFailedException: Code: 300004. Beschrijving: het laden van het bestand * * * is mislukt.

* Mogelijke oorzaak & aanbevolen actie:
  * Als de SSIS-activiteit een pakket uit het bestands systeem (pakket bestand of project bestand) uitvoert, treedt deze fout op als het project, het pakket of het configuratie bestand niet toegankelijk is met de toegangs referentie voor het pakket dat u in de SSIS-activiteit hebt gegeven
    * Als u Azure-bestand gebruikt:
      * Het bestandspad moet beginnen met de \\naam\>van \\ \<het opslag account\\. file.core.Windows.net\<pad naar bestands share\>
      * Het domein moet ' Azure ' zijn
      * De gebruikers naam moet \<de naam van het opslag account zijn\>
      * Het wacht woord moet \<de toegangs sleutel voor opslag zijn\>
    * Als uw on-premises bestand wordt gebruikt, controleert u of VNet, toegangs referenties voor het pakket en de machtigingen juist zijn geconfigureerd, zodat uw Azure SSIS Integration runtime toegang heeft tot uw on-premises bestands share.

### <a name="error-message-the-file-name--specified-in-the-connection-was-not-valid"></a>Foutbericht: "De bestands naam"... " opgegeven in de verbinding is ongeldig

* Mogelijke oorzaak & aanbevolen actie:
  * Er is een ongeldige bestands naam opgegeven
  * Zorg ervoor dat u FQDN (Fully Qualified Domain Name) gebruikt in plaats van korte tijd in uw verbindings beheer

### <a name="error-message-cannot-open-file-"></a>Foutbericht: Kan bestand niet openen...

Deze fout treedt op wanneer de pakket uitvoering geen bestand kan vinden op de lokale schijf in de SSIS Integration runtime. Voer de volgende acties uit:
* Gebruik niet het absolute pad in het pakket dat wordt uitgevoerd in de SSIS Integration runtime. De huidige uitvoerings werkmap (.) of de map Temp (% TEMP%) gebruiken maar.
* Als u een aantal bestanden op SSIS Integration runtime-knoop punten wilt behouden, moet u de bestanden voorbereiden zoals beschreven in [Customize Setup](how-to-configure-azure-ssis-ir-custom-setup.md). Alle bestanden in de werkmap worden opgeruimd nadat de uitvoering is voltooid.
* Gebruik Azure Files in plaats van het bestand op te slaan in het knoop punt SSIS Integration runtime. Zie [Azure-bestands shares gebruiken](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares)voor meer informatie.

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Foutbericht: ' De data base ' SSISDB ' heeft het quotum voor de grootte bereikt '

Een mogelijke oorzaak is dat de SSISDB-database die is gemaakt in de Azure SQL-database of een beheerd exemplaar tijdens het maken van een SSIS-integratieruntime, zijn quotum heeft bereikt. Voer de volgende acties uit:
* Overweeg de DTU van uw database te vergroten. U kunt meer informatie vinden in [SQL Database-resourcelimieten voor een Azure SQL-databaseserver](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server).
* Controleer of uw pakket een groot aantal logboeken kan genereren. Als dat het geval is, kunt u een elastische taak configureren om deze logboeken op te schonen. Meer informatie kunt u vinden in [SSISDB-logboeken opschonen met taken voor Azure Elastic Database](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md).

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Foutbericht: "De aanvraag limiet voor de data base is... en is bereikt. "

Als er een groot aantal pakketten parallel wordt uitgevoerd in de SSIS Integration runtime, kan deze fout optreden omdat SSISDB de aanvraag limiet heeft bereikt. Overweeg de DTC van SSISDB te verhogen om dit probleem op te lossen. U kunt meer informatie vinden in [SQL Database-resourcelimieten voor een Azure SQL-databaseserver](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server).

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Foutbericht: SSIS-bewerking is mislukt met onverwachte bewerkings status:...

De fout wordt meestal veroorzaakt door een tijdelijk probleem, dus probeer de uitvoering van het pakket opnieuw uit te voeren. Pas het patroon voor opnieuw proberen in het SSIS-pakket toe door de volgende stappen uit te voeren:

* Zorg ervoor dat uw SSIS-pakketten bij storingen zonder neven effecten (bijvoorbeeld gegevens verlies of gegevens duplicatie) opnieuw kunnen worden uitgevoerd.
* Configureer het **interval** voor **opnieuw proberen** en de activiteit voor het **uitvoeren van SSIS-pakketten** op het tabblad **Algemeen** . ![Eigenschappen instellen op het tabblad Algemeen](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
* Stel voor een ADO.NET-en OLE DB bron-of doel onderdeel **ConnectRetryCount** en **ConnectRetryInterval** in in VERBINDINGS beheer in de SSIS-pakket-of-SSIS-activiteit.

### <a name="error-message-there-is-no-active-worker"></a>Foutbericht: Er is geen actieve werk nemer.

Deze fout betekent meestal dat de SSIS Integration runtime een slechte status heeft. Controleer de Azure Portal voor de status en gedetailleerde fouten. Zie [Azure-SSIS Integration runtime](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)voor meer informatie.

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Foutbericht: ' Uw Integration runtime kan niet worden bijgewerkt en zal uiteindelijk niet meer werken omdat we geen toegang hebben tot de Azure Blob-container die u hebt ingevoerd voor aangepaste installatie. '

Deze fout treedt op wanneer de SSIS Integration runtime geen toegang kan krijgen tot de opslag die is geconfigureerd voor aangepaste installatie. Controleer of de SAS-URI (Shared Access Signature) die u hebt ingevoerd geldig is en niet is verlopen.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Foutbericht: "Micro soft OLE DB-provider voor Analysis Services. HRESULT Beschrijving van 0x80004005: COM-fout: COM-fout: mscorlib; Er is een uitzonde ring opgetreden bij het doel van een aanroep.

Een mogelijke oorzaak is dat de gebruikers naam of het wacht woord waarvoor Azure multi-factor Authentication is ingeschakeld, is geconfigureerd voor Azure Analysis Services-verificatie. Deze authenticatie wordt niet ondersteund in de SSIS Integration runtime. Probeer een service-principal te gebruiken voor Azure Analysis Services authenticatie:
1. Bereid een service-principal voor zoals beschreven in [Automation met Service](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal)-principals.
2. Configureer in verbindings beheer **een specifieke gebruikers naam en wacht woord gebruiken**: Stel **AppID** in als de gebruikers naam en **clientSecret** als wacht woord.

### <a name="error-message-adonet-source-has-failed-to-acquire-the-connection-guid-with-the-following-error-message-login-failed-for-user-nt-authorityanonymous-logon-when-using-a-managed-identity"></a>Foutbericht: ' ADONET-bron kan de verbinding {GUID} niet verkrijgen met het volgende fout bericht: De aanmelding is mislukt voor de gebruiker ' NT AUTHORITY\ANONYMOUS-aanmelding ' bij het gebruik van een beheerde identiteit

Zorg ervoor dat u de verificatie methode van verbindings beheer niet configureert als **Active Directory wachtwoord verificatie** wanneer de para meter *ConnectUsingManagedIdentity* is ingesteld op **True**. U kunt het configureren als **SQL-verificatie** in plaats daarvan, wat wordt genegeerd als *ConnectUsingManagedIdentity* is ingesteld.

### <a name="multiple-package-executions-are-triggered-unexpectedly"></a>Meerdere pakket uitvoeringen worden onverwacht geactiveerd

* Mogelijke oorzaak & aanbevolen actie:
  * De opgeslagen procedure-activiteit ADF wordt gebruikt voor het activeren van de uitvoering van SSIS-pakketten. De t-SQL-opdracht kan leiden tot een tijdelijk probleem en het opnieuw uitvoeren van het programma activeren waardoor meerdere pakket uitvoeringen kunnen worden uitgevoerd.
  * Gebruik in plaats daarvan ExecuteSSISPackage-activiteit die ervoor zorgt dat de pakket uitvoering niet opnieuw wordt uitgevoerd tenzij gebruikers het aantal pogingen in de activiteit instellen. Meer informatie vindt u op[https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)

### <a name="package-execution-takes-too-long"></a>Het uitvoeren van het pakket duurt te lang

Dit zijn mogelijke oorzaken en aanbevolen acties:

* Er zijn te veel pakket uitvoeringen gepland voor de SSIS Integration runtime. Al deze uitvoeringen worden in een wachtrij gewacht op hun beurt.
  * Bepaal het maximum door gebruik te maken van de volgende formule:

    Maximum aantal parallelle uitvoeringen per IR = aantal knoop punten * maximum aantal parallelle uitvoeringen per knoop punt
  * Zie [een Azure SSIS Integration runtime maken in azure Data Factory](create-azure-ssis-integration-runtime.md)voor meer informatie over het instellen van het aantal knoop punten en het maximum aantal parallelle uitvoeringen per knoop punt.
* De SSIS Integration runtime is gestopt of heeft een slechte status. Zie [Azure-SSIS Integration runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime)voor meer informatie over het controleren van de runtime status en-fouten van de SSIS-integratie.

We raden u ook aan om een time-out op het tabblad **Algemeen** in te stellen: ![Stel eigenschappen in op het tabblad](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)algemeen.

### <a name="poor-performance-in-package-execution"></a>Slechte prestaties bij het uitvoeren van een pakket

Voer de volgende acties uit:

* Zorg ervoor dat de SSIS Integration runtime zich in dezelfde regio bevindt als de gegevens bron en het doel.

* Stel het logboek registratie niveau van de uitvoering van het pakket in op **prestaties** om de duur informatie voor elk onderdeel in de uitvoering te verzamelen. Zie voor meer informatie [logboek registratie van Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging).

* Controleer de prestaties van het IR-knoop punt in de Azure Portal:
  * Zie [Azure-SSIS Integration runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime)voor meer informatie over het bewaken van de SSIS Integration runtime.
  * U kunt de informatie over de CPU/het geheugen voor de SSIS Integration runtime vinden door de metrische gegevens van de data factory in het Azure Portal weer te geven.
    ![Metrische gegevens van de SSIS Integration runtime bewaken](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)

---
title: Setup aanpassen voor de Azure-SSIS-integratie-runtime | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de aangepaste installatie-interface voor de Azure-SSIS Integration runtime gebruikt voor het installeren van extra onderdelen of het wijzigen van instellingen
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 1/25/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 4962070d69af98d0c7b10dc6f931612766529dce
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515719"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Setup aanpassen voor de Azure-SSIS Integration runtime

De aangepaste configuratie-interface voor de Azure-SSIS Integration Runtime biedt een interface voor het toevoegen van uw eigen installatie stappen tijdens het inrichten of opnieuw configureren van uw Azure-SSIS-IR. Met aangepaste installatie kunt u de standaard configuratie of-omgeving wijzigen (bijvoorbeeld om extra Windows-Services te starten of toegangs referenties voor bestands shares op te slaan) of extra onderdelen (bijvoorbeeld assembly's, stuur Programma's of extensies) installeren op elk knoop punt van uw Azure-SSIS-IR.

U configureert uw aangepaste installatie door een script en de bijbehorende bestanden voor te bereiden en ze te uploaden naar een BLOB-container in uw Azure Storage-account. U geeft een Shared Access Signature (SAS) Uniform Resource Identifier (URI) voor uw container op wanneer u uw Azure-SSIS-IR inricht of configureert. Elk knoop punt van uw Azure-SSIS IR downloadt vervolgens het script en de bijbehorende bestanden uit uw container en voert uw aangepaste installatie uit met verhoogde bevoegdheden. Wanneer de aangepaste installatie is voltooid, uploadt elk knoop punt de standaard uitvoer van de uitvoering en andere logboeken naar uw container.

U kunt zowel gratis als niet-gelicentieerde onderdelen en betaalde of gelicentieerde onderdelen installeren. Als u een ISV bent, raadpleegt u [betaalde of gelicentieerde componenten ontwikkelen voor de Azure-SSIS IR](how-to-develop-azure-ssis-ir-licensed-components.md).

> [!IMPORTANT]
> De v2-serie knooppunten van Azure-SSIS IR zijn niet geschikt voor aangepaste installatie, dus gebruik in plaats daarvan de knoop punten uit de V3-serie.  Als u de knoop punten van de v2-serie al gebruikt, moet u zo snel mogelijk overschakelen op het gebruik van de knoop punten van de V3-serie.

## <a name="current-limitations"></a>Huidige beperkingen

-   Als u wilt gebruiken `gacutil.exe` voor het installeren van assembly's in de GAC (Global assembly cache), moet u een deel van uw aangepaste installatie opgeven `gacutil.exe` of de kopie gebruiken die is opgenomen in de open bare preview-container.

-   Als u wilt verwijzen naar een submap in uw script, `msiexec.exe` wordt de `.\` notatie niet ondersteund voor verwijzing naar de hoofdmap. Gebruik een opdracht als `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` in plaats `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`van.

-   Als u uw Azure-SSIS IR met aangepaste installatie wilt toevoegen aan een virtueel netwerk, wordt alleen Azure Resource Manager virtuele netwerk ondersteund. Klassiek virtueel netwerk wordt niet ondersteund.

-   De beheer share wordt op dit moment niet ondersteund op de Azure-SSIS IR.

-   Het ODBC-stuur programma voor IBM iSeries-toegang wordt niet ondersteund op de Azure-SSIS-IR. Mogelijk wordt er een installatie fout weer geven tijdens de aangepaste installatie. Neem contact op met IBM ondersteuning voor hulp.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u uw Azure-SSIS IR wilt aanpassen, moet u het volgende doen:

-   [Azure-abonnement](https://azure.microsoft.com/)

-   [Een Azure SQL Database of Managed instance server](https://ms.portal.azure.com/#create/Microsoft.SQLServer)

-   [Uw Azure-SSIS-IR inrichten](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

-   [Een Azure Storage-account](https://azure.microsoft.com/services/storage/). Voor aangepaste installatie uploadt en slaat u uw aangepaste installatie script en de bijbehorende bestanden op in een BLOB-container. Het aangepaste installatie proces uploadt ook de uitvoerings logboeken naar dezelfde BLOB-container.

## <a name="instructions"></a>Instructies

1. Down load en Installeer [Azure PowerShell](/powershell/azure/install-az-ps).

1. Bereid uw aangepaste installatie script en de bijbehorende bestanden voor (bijvoorbeeld. bat,. cmd,. exe,. dll,. msi of. ps1-bestanden).

   1.  U moet een script bestand hebben met `main.cmd`de naam, het toegangs punt van uw aangepaste installatie.

   1.  Als u extra logboeken wilt genereren die worden gegenereerd door andere hulpprogram `msiexec.exe`ma's (bijvoorbeeld) om te worden geüpload naar uw container, geeft u `CUSTOM_SETUP_SCRIPT_LOG_DIR` de vooraf gedefinieerde omgevings variabele op als de map logboek `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`in uw scripts (bijvoorbeeld).

1. Down load, installeer en start [Azure Storage Explorer](https://storageexplorer.com/).

   1. Onder **(lokaal en gekoppeld)** klikt u met de rechter muisknop op **opslag accounts** en selecteert **u verbinding maken met Azure Storage**.

      ![Verbinding maken met Azure Storage](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   1. Selecteer **een opslag accountnaam en-sleutel gebruiken** en selecteer **volgende**.

      ![De naam en sleutel van een opslagaccount gebruiken](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   1. Voer de naam en sleutel van uw Azure Storage-account in, selecteer **volgende**en selecteer vervolgens **verbinding maken**.

      ![Account naam en-sleutel voor het archief opgeven](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   1. Klik onder uw verbonden Azure Storage-account met de rechter muisknop op **BLOB-containers**, selecteer **BLOB-container maken**en geef de nieuwe container een naam.

      ![Een blob-container maken](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   1. Selecteer de nieuwe container en upload uw aangepaste installatie script en de bijbehorende bestanden. Zorg ervoor dat u uploadt `main.cmd` op het hoogste niveau van uw container, niet in een map. Zorg er ook voor dat de container alleen de benodigde aangepaste installatie bestanden bevat. Als u deze later downloadt naar uw Azure-SSIS-IR, duurt dit niet lang. De maximale periode voor aangepaste installatie is momenteel ingesteld op 45 minuten voordat er een time-out optreedt. Dit omvat de tijd om alle bestanden uit uw container te downloaden en te installeren op Azure-SSIS IR. Als er een langere periode nodig is, kunt u een ondersteunings ticket genereren.

      ![Bestanden uploaden naar de BLOB-container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   1. Klik met de rechter muisknop op de container en selecteer **Shared Access Signature ophalen**.

      ![De Shared Access Signature voor de container ophalen](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   1. Maak de SAS-URI voor uw container met een voldoende lange verloop tijd en met de machtigingen lezen en schrijven + lijst. U hebt de SAS-URI nodig om uw aangepaste installatie script en de bijbehorende bestanden te downloaden en uit te voeren wanneer een wille keurig knoop punt van uw Azure-SSIS IR-installatie kopie wordt bijgewerkt/opnieuw wordt gestart. U hebt schrijf machtigingen nodig voor het uploaden van de installatie Logboeken.

      > [!IMPORTANT]
      > Zorg ervoor dat de SAS-URI niet verloopt en dat aangepaste installatie bronnen altijd beschikbaar zijn tijdens de hele levens cyclus van uw Azure-SSIS IR, van het maken tot verwijderen, met name als u uw Azure-SSIS-IR tijdens deze periode regel matig stopt en start.

      ![De Shared Access Signature voor de container genereren](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   1. Kopieer de SAS-URI van uw container en sla deze op.

      ![De Shared Access Signature kopiëren en opslaan](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

   1. Wanneer u uw Azure-SSIS IR inricht of opnieuw configureert met Data Factory gebruikers interface, moet u voordat u de Azure-SSIS-IR start, de SAS-URI van uw container invoeren in het juiste veld in het deel venster **Geavanceerde instellingen** :

      ![Voer de Shared Access Signature in](media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

      Wanneer u uw Azure-SSIS IR inricht of opnieuw configureert met Power shell, moet u voordat u uw Azure-SSIS `Set-AzDataFactoryV2IntegrationRuntime` -IR start de cmdlet uitvoeren met de SAS-URI van uw `SetupScriptContainerSasUri` container als waarde voor de nieuwe para meter. Bijvoorbeeld:

      ```powershell
      Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                -Name $MyAzureSsisIrName `
                                                -ResourceGroupName $MyResourceGroupName `
                                                -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

      Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                  -Name $MyAzureSsisIrName `
                                                  -ResourceGroupName $MyResourceGroupName
      ```

   1. Nadat de aangepaste installatie is voltooid en uw Azure-SSIS IR wordt gestart, kunt u de standaard uitvoer van `main.cmd` en andere uitvoerings logboeken vinden `main.cmd.log` in de map van uw opslag container.

1. Als u andere voor beelden van aangepaste installatie wilt zien, maakt u verbinding met de open bare preview-container met Azure Storage Explorer.

   a.  Onder **(lokaal en gekoppeld)** klikt u met de rechter muisknop op **opslag accounts**, selecteert **u verbinding maken met Azure Storage**, selecteert **u een Connection String of een URI voor de hand tekening voor gedeelde toegang gebruiken**en selecteert u vervolgens **volgende**.

      ![Verbinding maken met Azure Storage met de Shared Access Signature](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   b.  Selecteer **een SAS-URI gebruiken** en voer de volgende SAS-URI in voor de open bare preview-container. Selecteer **volgende**en selecteer **verbinding maken**.

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

      ![De Shared Access Signature voor de container opgeven](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   c. Selecteer de gekoppelde open bare preview-container en dubbel klik `CustomSetupScript` op de map. In deze map zijn de volgende items:

      1. Een `Sample` map die een aangepaste installatie bevat voor het installeren van een basis taak op elk knoop punt van uw Azure-SSIS-IR. De taak heeft niets maar een paar seconden in de slaap stand. De map bevat ook een `gacutil` map, de volledige inhoud van die (`gacutil.exe`, `gacutil.exe.config`en `1033\gacutlrc.dll`) naar uw container kan worden gekopieerd. `main.cmd` Daarnaast bevat opmerkingen voor het persistent maken van toegangs referenties voor bestands shares.

      1. Een `UserScenarios` map die verschillende aangepaste instellingen bevat voor echte gebruikers scenario's.

   ![Inhoud van de open bare preview-container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   d. Dubbel klik op de `UserScenarios` map. In deze map zijn de volgende items:

      1. Een `.NET FRAMEWORK 3.5` map die een aangepaste installatie bevat voor het installeren van een eerdere versie van de .NET Framework die mogelijk vereist is voor aangepaste onderdelen op elk knoop punt van uw Azure-SSIS-IR.

      1. Een `BCP` map die een aangepaste installatie bevat voor het installeren van SQL Server opdracht regel programma's (`MsSqlCmdLnUtils.msi`), met inbegrip van het bulk`bcp`copy-programma (), op elk knoop punt van uw Azure-SSIS-IR.

      1. Een `EXCEL` map die een aangepaste installatie bevat voor het installeren van open-source-`DocumentFormat.OpenXml.dll`assembly's `ExcelDataReader.DataSet.dll`(, `ExcelDataReader.dll`en) op elk knoop punt van uw Azure-SSIS-IR.

      1. Een `ORACLE ENTERPRISE` map, die een aangepast installatie script (`main.cmd`) en een installatie configuratie bestand (`client.rsp`) bevat waarmee de Oracle-connectors en het OCI-stuur programma worden geïnstalleerd op elk knoop punt van uw Azure-SSIS IR Enter prise-editie. Met deze installatie kunt u de Oracle-verbindings beheer,-bron en-bestemming gebruiken. Down load eerst micro soft-connectors v 5.0`AttunitySSISOraAdaptersSetup.msi` voor `AttunitySSISOraAdaptersSetup64.msi`Oracle (en) van het [micro soft Download centrum](https://www.microsoft.com/en-us/download/details.aspx?id=55179) en de `winx64_12102_client.zip` nieuwste Oracle-client, bijvoorbeeld-van [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html), en upload deze allemaal samen met `main.cmd` en`client.rsp` in uw container. Als u gebruikmaakt van TNS om verbinding te maken met Oracle, moet u `tnsnames.ora`deze ook downloaden, bewerken en uploaden naar uw container, zodat deze kan worden gekopieerd naar de map Oracle-installatie tijdens de installatie.

      1. Een `ORACLE STANDARD ADO.NET` map, die een aangepaste installatie script (`main.cmd`) bevat om het Oracle ODP.net-stuur programma te installeren op elk knoop punt van uw Azure-SSIS-IR. Met deze installatie kunt u de ADO.NET-verbindings beheer,-bron en-bestemming gebruiken. Down load eerst het meest recente Oracle ODP.net-stuur programma, `ODP.NET_Managed_ODAC122cR1.zip` bijvoorbeeld-van [Oracle](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html), en upload het vervolgens samen `main.cmd` met uw container.
       
      1. Een `ORACLE STANDARD ODBC` map met een aangepaste installatie script (`main.cmd`) voor het installeren van het Oracle ODBC-stuur programma en het configureren van DSN op elk knoop punt van uw Azure-SSIS-IR. Met deze installatie kunt u het type ODBC-verbindings beheer/bron/bestemming of Power Query verbindings beheer/bron met ODBC-gegevens bron gebruiken om verbinding te maken met Oracle server. Down load eerst de nieuwste Oracle Instant-client (Basic package of Basic Lite package) en het ODBC-pakket, bijvoorbeeld de 64-bits pakketten van [hier](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) (Basic- `instantclient-basic-windows.x64-18.3.0.0.0dbru.zip`pakket:, `instantclient-basiclite-windows.x64-18.3.0.0.0dbru.zip`Basic Lite-pakket:, `instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip`ODBC-pakket:) of de 32-bits pakketten van [hier](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (Basic-pakket `instantclient-basic-nt-18.3.0.0.0dbru.zip`:, Basic Lite- `instantclient-basiclite-nt-18.3.0.0.0dbru.zip`pakket:, ODBC `instantclient-odbc-nt-18.3.0.0.0dbru.zip`-pakket:) en upload ze samen `main.cmd` met de container.

      1. Een `SAP BW` map met een aangepaste installatie script (`main.cmd`) voor het installeren van de SAP .net-connector-`librfc32.dll`Assembly () op elk knoop punt van uw Azure-SSIS IR Enter prise Edition. Met deze installatie kunt u de SAP BW verbindings beheer, de bron en het doel. Upload eerst de 64-bits of de 32-bits versie van `librfc32.dll` van de SAP-installatiemap in uw container, samen met. `main.cmd` Het script kopieert vervolgens de SAP-assembly `%windir%\SysWow64` naar `%windir%\System32` de of-map tijdens de installatie.

      1. Een `STORAGE` map die een aangepaste installatie bevat om Azure PowerShell te installeren op elk knoop punt van uw Azure-SSIS-IR. Met deze installatie kunt u SSIS-pakketten implementeren en uitvoeren die [Power shell-scripts uitvoeren om uw Azure Storage-account te bewerken](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). Kopieer `main.cmd`een voor beeld `AzurePowerShell.msi` (of installeer de meest recente versie) en `storage.ps1` naar uw container. Gebruik Power shell. dtsx als een sjabloon voor uw pakketten. De pakket sjabloon bevat een combi natie van een [Azure Blob-Download taak](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), die als een gewijzigd Power shell-script wordt gedownload `storage.ps1` en een taak voor het uitvoeren van een [proces](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) waarmee het script op elk knoop punt wordt uitgevoerd.

      1. Een `TERADATA` map, die een aangepast installatie script (`main.cmd`), het bijbehorende bestand (`install.cmd`) en de installatie pakketten (`.msi`) bevat. Met deze bestanden worden Teradata-connectors, de TPT-API en het ODBC-stuur programma geïnstalleerd op elk knoop punt van uw Azure-SSIS IR Enter prise-editie. Met deze installatie kunt u gebruikmaken van de Teradata-verbindings beheer, de bron en de bestemming. Down load eerst de Teradata-hulpprogram ma's en hulpprogram ma's (TTU) 15. x zip-bestand `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`(bijvoorbeeld) van [Teradata](http://partnerintelligence.teradata.com)en upload het vervolgens `.cmd` samen `.msi` met de bestanden in uw container.

   ![Mappen in de map gebruikers scenario's](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   e. Kopieer en plak de inhoud van de geselecteerde map in uw container om deze aangepaste installatie voorbeelden uit te proberen. Wanneer u uw Azure-SSIS IR inricht of opnieuw configureert met Power shell `Set-AzDataFactoryV2IntegrationRuntime` , voert u de cmdlet uit met de SAS-URI van uw `SetupScriptContainerSasUri` container als waarde voor de nieuwe para meter.

## <a name="next-steps"></a>Volgende stappen

-   [Enter prise Edition van het Azure-SSIS-Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Betaalde of gelicentieerde aangepaste onderdelen ontwikkelen voor Azure-SSIS Integration runtime](how-to-develop-azure-ssis-ir-licensed-components.md)

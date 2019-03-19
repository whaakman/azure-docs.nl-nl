---
title: Setup voor de Azure-SSIS integratieruntime aanpassen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u met behulp van de interface van de aangepaste instellingen voor de Azure-SSIS-integratieruntime kunt extra onderdelen installeren of instellingen wijzigen
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
ms.openlocfilehash: d146027ea3a21ab8df3750014c02893bc2f50dd6
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58097726"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Setup voor de Azure-SSIS integratieruntime aanpassen

De interface voor aangepaste instellingen voor de Azure-SSIS Integration Runtime biedt een interface om toe te voegen van uw eigen installatiestappen uit tijdens de inrichting of herconfiguratie van uw Azure-SSIS-IR. Aangepaste installatie kunt u het standaardbesturingssysteem configuratie of de omgeving (bijvoorbeeld aanvullende Windows-services starten of referenties voor toegang voor bestandsshares behouden) te wijzigen of extra onderdelen (bijvoorbeeld: assembly's, stuurprogramma's of extensies) installeren op elk knooppunt van uw Azure-SSIS-IR.

U configureren uw aangepaste instellingen door een script en de bijbehorende bestanden voorbereiden en ze uploaden naar een blobcontainer in uw Azure Storage-account. U kunt een Shared Access Signature (SAS) Uniform Resource Identifier (URI) voor de container opgeven bij het inrichten of opnieuw configureren van uw Azure-SSIS-IR. Elk knooppunt van uw Azure-SSIS IR vervolgens downloadt het script en de bijbehorende bestanden van de container en uw aangepaste installatie wordt uitgevoerd met verhoogde bevoegdheden. Bij aangepaste installatie is voltooid, wordt elk knooppunt de standaarduitvoer van de uitvoering en andere logboeken geüpload naar de container.

U kunt gratis of niet-gelicentieerde onderdelen en een betaalde versie of gelicentieerde onderdelen installeren. Als u een ISV bent, Zie [over het ontwikkelen van een betaalde versie of een licentie van onderdelen voor de Azure-SSIS-IR](how-to-develop-azure-ssis-ir-licensed-components.md).

> [!IMPORTANT]
> De v2-series-knooppunten van de Azure-SSIS IR zijn niet geschikt voor aangepaste installatie, dus gebruik in plaats daarvan de knooppunten v3-serie.  Als u al de v2-series-knooppunten, ga dan voor het gebruik van de knooppunten v3-serie zo snel mogelijk.

## <a name="current-limitations"></a>Huidige beperkingen

-   Als u wilt gebruiken `gacutil.exe` voor het installeren van assembly's in de Global Assembly Cache (GAC), moet u opgeven `gacutil.exe` als onderdeel van uw aangepaste installatie, of gebruik het exemplaar dat is opgegeven in de openbare Preview-container.

-   Als u wilt verwijzen naar een submap in uw script `msiexec.exe` biedt geen ondersteuning voor de `.\` notatie om te verwijzen naar de hoofdmap. Gebruik een opdracht zoals `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` in plaats van `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`.

-   Als u uw Azure-SSIS IR met aangepaste instellingen toevoegen aan een virtueel netwerk wilt, worden alleen virtuele netwerk van Azure Resource Manager wordt ondersteund. Klassiek virtueel netwerk wordt niet ondersteund.

-   Beheershare wordt momenteel niet ondersteund op de Azure-SSIS-IR.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voor het aanpassen van uw Azure-SSIS-IR, moet u de volgende zaken:

-   [Azure-abonnement](https://azure.microsoft.com/)

-   [Een Azure SQL Database of Managed Instance-server](https://ms.portal.azure.com/#create/Microsoft.SQLServer)

-   [Inrichten van uw Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

-   [Een Azure Storage-account](https://azure.microsoft.com/services/storage/). Voor aangepaste installatie, uploaden en uw aangepaste setup-script en de bijbehorende bestanden opslaan in een blob-container. Het aangepaste setup-proces wordt ook de uitvoeringslogboeken geüpload naar de dezelfde blob-container.

## <a name="instructions"></a>Instructies

1. Download en installeer [Azure PowerShell](/powershell/azure/install-az-ps).

1. Bereid uw aangepaste setup-script en de bijbehorende bestanden (bijvoorbeeld, .bat, .cmd, .exe, .dll, MSI- of .ps1-bestanden).

   1.  Hebt u een scriptbestand met de naam `main.cmd`, dit is het beginpunt voor uw aangepaste instellingen.

   1.  Als u wilt dat andere logboeken die worden gegenereerd door andere hulpprogramma's (bijvoorbeeld `msiexec.exe`) om te worden geüpload naar de container, geef de vooraf gedefinieerde omgevingsvariabele `CUSTOM_SETUP_SCRIPT_LOG_DIR` als de map voor het logboek in uw scripts (bijvoorbeeld `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

1. Downloaden, installeren en starten [Azure Storage Explorer](https://storageexplorer.com/).

   1. Onder **(lokaal en gekoppeld)**, selecteer met de rechtermuisknop **Opslagaccounts** en selecteer **verbinding maken met Azure storage**.

      ![Verbinding maken met Azure Storage](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   1. Selecteer **een opslagaccountnaam en -sleutel gebruiken** en selecteer **volgende**.

      ![De naam en sleutel van een opslagaccount gebruiken](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   1. Voer uw Azure Storage-accountnaam en -sleutel, selecteer **volgende**, en selecteer vervolgens **Connect**.

      ![Store-accountnaam en -sleutel opgeven](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   1. Onder de gekoppelde Azure Storage-account met de rechtermuisknop op **Blobcontainers**, selecteer **Blob-Container maken**, en de naam van de nieuwe container.

      ![Een blob-container maken](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   1. Selecteer de nieuwe container en upload uw aangepaste setup-script en de bijbehorende bestanden. Zorg ervoor dat u uploadt `main.cmd` op het hoogste niveau van de container, niet in een map. Controleer ook of dat uw container bevat alleen de benodigde aangepaste setup-bestanden, zodat deze worden gedownload naar uw Azure-SSIS IR later duurt niet lang.

      ![Bestanden uploaden naar de blob-container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   1. Met de rechtermuisknop op de container en selecteer **Shared Access Signature ophalen**.

      ![De Shared Access Signature voor de container ophalen](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   1. De SAS-URI voor de container maken met een lang genoeg verlooptijd en lezen + schrijven + lijst van machtigingen. U moet de SAS-URI te downloaden en uw aangepaste setup-script en de bijbehorende bestanden uitgevoerd wanneer een knooppunt van uw Azure-SSIS IR teruggezet/opnieuw gestart is. U hebt schrijfmachtigingen nodig voor het uploaden van logboekbestanden voor het uitvoeren van setup.

      > [!IMPORTANT]
      > Zorg ervoor dat de SAS-URI niet verloopt en aangepaste setup-bronnen altijd beschikbaar tijdens de gehele levenscyclus van uw Azure-SSIS-IR, het maken van de verwijdering van zijn met name als u regelmatig stoppen en starten van uw Azure-SSIS IR tijdens deze periode.

      ![Genereren van de Shared Access Signature voor de container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   1. Kopieer en bewaar de SAS-URI van de container.

      ![Kopiëren en opslaan van de handtekening voor gedeelde toegang](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

   1. Wanneer u inrichten of opnieuw configureren van uw Azure-SSIS IR met Data Factory-UI, voordat u begint met uw Azure-SSIS-IR, voert u de SAS-URI van de container in het desbetreffende veld op **geavanceerde instellingen** Configuratiescherm:

      ![Voer de Shared Access Signature](media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

      Wanneer u inrichten of opnieuw configureren van uw Azure-SSIS IR met PowerShell, voordat u begint met uw Azure-SSIS-IR, voert de `Set-AzDataFactoryV2IntegrationRuntime` cmdlet met de SAS-URI van de container als de waarde voor nieuwe `SetupScriptContainerSasUri` parameter. Bijvoorbeeld:

      ```powershell
      Set-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                -Name $MyAzureSsisIrName `
                                                -ResourceGroupName $MyResourceGroupName `
                                                -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

      Start-AzDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                  -Name $MyAzureSsisIrName `
                                                  -ResourceGroupName $MyResourceGroupName
      ```

   1. Nadat uw aangepaste setup is voltooid en uw Azure-SSIS IR is gestart, vindt u de standaarduitvoer van `main.cmd` en andere kan worden uitgevoerd, geregistreerd de `main.cmd.log` map van uw storage-container.

1. Andere aangepaste installatie om voorbeelden te bekijken, verbinding maken met de openbare Preview-container met Azure Storage Explorer.

   a.  Onder **(lokaal en gekoppeld)**, met de rechtermuisknop op **Opslagaccounts**, selecteer **verbinding maken met Azure storage**, selecteer **een verbindingsreeks of een gedeelde toegang gebruiken handtekening URI**, en selecteer vervolgens **volgende**.

      ![Verbinding maken met Azure storage met de handtekening voor gedeelde toegang](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   b.  Selecteer **gebruik van een SAS-URI** en voer de volgende SAS-URI voor de openbare Preview-container. Selecteer **volgende**, en selecteer **Connect**.

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

      ![De handtekening voor gedeelde toegang bieden voor de container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   c. Selecteer de verbonden openbare Preview-container en dubbelklikt u op de `CustomSetupScript` map. In deze map zijn de volgende items:

      1. Een `Sample` map een aangepaste instellingen bevat voor het installeren van een eenvoudige taak op elk knooppunt van uw Azure-SSIS-IR. De taak wordt niets gewijzigd maar slaapstand voor een paar seconden. De map bevat ook een `gacutil` , de volledige inhoud van die map (`gacutil.exe`, `gacutil.exe.config`, en `1033\gacutlrc.dll`) kunnen worden gekopieerd omdat in de container. Bovendien `main.cmd` bevat opmerkingen om referenties voor toegang voor bestandsshares persistent te maken.

      1. Een `UserScenarios` map, waarin verschillende aangepaste instellingen voor real-user-scenario's.

   ![Inhoud van de openbare preview-container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   d. Dubbelklik op de `UserScenarios` map. In deze map zijn de volgende items:

      1. Een `.NET FRAMEWORK 3.5` map een aangepaste instellingen bevat voor het installeren van een eerdere versie van .NET Framework die mogelijk vereist zijn voor aangepaste onderdelen op elk knooppunt van uw Azure-SSIS-IR.

      1. Een `BCP` map een aangepaste instellingen bevat voor het installeren van opdrachtregelprogramma's van SQL Server (`MsSqlCmdLnUtils.msi`), met inbegrip van het programma voor bulksgewijs kopiëren (`bcp`), op elk knooppunt van uw Azure-SSIS-IR.

      1. Een `EXCEL` map een aangepaste instellingen bevat voor het installeren van open-source-assembly's (`DocumentFormat.OpenXml.dll`, `ExcelDataReader.DataSet.dll`, en `ExcelDataReader.dll`) op elk knooppunt van uw Azure-SSIS-IR.

      1. Een `ORACLE ENTERPRISE` map, waarin een aangepaste setup-script (`main.cmd`) en config-bestand voor installatie op de achtergrond (`client.rsp`) voor het installeren van de Oracle-connectors en OCI stuurprogramma op elk knooppunt van uw Azure-SSIS IR Enterprise Edition. Deze instelling kunt u de Oracle Connection Manager, de bron en bestemming gebruiken. Download eerst Microsoft-Connectors 5.0 voor Oracle (`AttunitySSISOraAdaptersSetup.msi` en `AttunitySSISOraAdaptersSetup64.msi`) van [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=55179) en de meest recente Oracle-client - bijvoorbeeld `winx64_12102_client.zip` - van [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html), upload vervolgens ze allemaal samen met `main.cmd` en `client.rsp` in de container. Als u verbinding maken met Oracle TNS, moet u ook downloaden `tnsnames.ora`, bewerken en uploaden naar de container, zodat deze kan worden gekopieerd naar de installatiemap van Oracle tijdens de installatie.

      1. Een `ORACLE STANDARD ADO.NET` map, waarin een aangepaste setup-script (`main.cmd`) voor het installeren van de Oracle-ODP.NET-stuurprogramma op elk knooppunt van uw Azure-SSIS-IR. Deze instelling kunt u de ADO.NET Connection Manager, de bron en bestemming gebruiken. Download de meest recente Oracle ODP.NET-stuurprogramma - eerst bijvoorbeeld `ODP.NET_Managed_ODAC122cR1.zip` - van [Oracle](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html), en upload het samen met `main.cmd` in de container.
       
      1. Een `ORACLE STANDARD ODBC` map, waarin een aangepaste setup-script (`main.cmd`) het Oracle ODBC-stuurprogramma installeren en configureren van DSN op elk knooppunt van uw Azure-SSIS-IR. Deze instelling kunt u de bron-Verbindingsbeheer ODBC-/ doel of de Power Query verbinding Manager/bron gebruiken met soort ODBC-gegevensbron verbinding maken met Oracle-server. Download eerst de meest recente Oracle-Instant Client (Basic of Basic Lite-pakket) en ODBC-pakket - bijvoorbeeld de 64-bits-pakketten uit [hier](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) (Basic-pakket: `instantclient-basic-windows.x64-18.3.0.0.0dbru.zip`, Basic Lite-pakket: `instantclient-basiclite-windows.x64-18.3.0.0.0dbru.zip`, ODBC-pakket : `instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip`) of de 32-bits-pakketten uit [hier](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (Basic-pakket: `instantclient-basic-nt-18.3.0.0.0dbru.zip`, Basic Lite-pakket: `instantclient-basiclite-nt-18.3.0.0.0dbru.zip`, ODBC-pakket: `instantclient-odbc-nt-18.3.0.0.0dbru.zip`), en deze vervolgens uploaden samen met `main.cmd` in de container.

      1. Een `SAP BW` map, waarin een aangepaste setup-script (`main.cmd`) voor het installeren van de SAP .NET connector assembly (`librfc32.dll`) op elk knooppunt van uw Azure-SSIS IR Enterprise Edition. Deze instelling kunt u de SAP BW Connection Manager, de bron en bestemming gebruiken. Eerst uploadt de 64-bits of de 32-bits versie van `librfc32.dll` uit de SAP-installatiemap naar de container, samen met de `main.cmd`. Kopieert de SAP-assembly in het script de `%windir%\SysWow64` of `%windir%\System32` map tijdens de installatie.

      1. Een `STORAGE` map een aangepaste instellingen bevat voor het installeren van Azure PowerShell op elk knooppunt van uw Azure-SSIS-IR. Deze instelling kunt u implementeren en uitvoeren van SSIS-pakketten die worden uitgevoerd [PowerShell-scripts voor het bewerken van uw Azure Storage-account](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). Kopie `main.cmd`, een voorbeeld van een `AzurePowerShell.msi` (of installeer de meest recente versie), en `storage.ps1` naar de container. Gebruik PowerShell.dtsx als een sjabloon voor uw pakketten. De pakketsjabloon combineert een [Azure Blob downloaden taak](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), welke downloads `storage.ps1` als een PowerShell-script kan worden gewijzigd, en een [taak proces uitvoeren](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) die het script wordt uitgevoerd op elk knooppunt.

      1. Een `TERADATA` map, waarin een aangepaste setup-script (`main.cmd`), het bijbehorende bestand (`install.cmd`), en installer-pakketten (`.msi`). Deze bestanden worden Teradata-connectors, de API TPT en het ODBC-stuurprogramma installeren op elk knooppunt van uw Azure-SSIS IR Enterprise Edition. Deze instelling kunt u de Teradata Connection Manager, de bron en bestemming gebruiken. Download eerst de Teradata-hulpprogramma's en hulpprogramma's (TTU) 15.x zip-bestand (bijvoorbeeld `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) van [Teradata](http://partnerintelligence.teradata.com), en upload dit samen met de bovenstaande `.cmd` en `.msi` bestanden in de container.

   ![Mappen in de map van de gebruiker-scenario 's](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   e. Als u wilt proberen deze voorbeelden van aangepaste installatie, kopieer en plak de inhoud van de geselecteerde map in de container. Wanneer u inrichten of opnieuw configureren van uw Azure-SSIS IR met PowerShell, voer de `Set-AzDataFactoryV2IntegrationRuntime` cmdlet met de SAS-URI van de container als de waarde voor nieuwe `SetupScriptContainerSasUri` parameter.

## <a name="next-steps"></a>Volgende stappen

-   [Enterprise-editie van de Azure-SSIS Integratieruntime](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Over het ontwikkelen van een betaalde versie of aangepaste onderdelen voor de Azure-SSIS integratieruntime in licentie gegeven](how-to-develop-azure-ssis-ir-licensed-components.md)

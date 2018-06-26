---
title: Setup voor de integratie van Azure SSIS runtime aanpassen | Microsoft Docs
description: Dit artikel wordt beschreven hoe u met de interface met aangepaste installatie voor de integratie van Azure SSIS runtime extra onderdelen installeren of instellingen wijzigen
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/21/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 76308bbb06d6bf1cdc9147258f7c26babae371a9
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750482"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Setup voor de integratie van Azure SSIS runtime aanpassen

De interface met aangepaste installatie voor de integratie van Azure SSIS Runtime biedt een interface voor het toevoegen van uw eigen instellingsstappen tijdens het inrichten of herconfiguratie van uw Azure-SSIS-IR Aangepaste installatie kunt u het standaardbesturingssysteem configuratie of omgeving (bijvoorbeeld aanvullende Windows-services starten of toegangsreferenties voor bestandsshares behouden) wijzigen of extra onderdelen (bijvoorbeeld assembly's, stuurprogramma's of extensies) installeren op elk knooppunt van uw Azure-SSIS-IR

U configureren uw aangepaste instellingen door het voorbereiden van een script en de bijbehorende bestanden en uploadt naar een blobcontainer in uw Azure Storage-account. U kunt een Shared Access Signature (SAS) Uniform Resource Identifier (URI) voor de container opgeven wanneer u inrichten of opnieuw configureren van uw Azure-SSIS-IR Elk knooppunt van uw Azure-SSIS-IR vervolgens het script en de bijbehorende bestanden downloadt van de container en uw aangepaste installatie wordt uitgevoerd met verhoogde bevoegdheden. Wanneer met aangepaste installatie is voltooid, uploadt elk knooppunt de standaarduitvoer van de uitvoering en worden andere logboeken naar de container.

U kunt gratis of niet-gelicentieerde onderdelen en betaalde of gelicentieerde onderdelen installeren. Als u een ISV bent, Zie [het ontwikkelen van betaald of onderdelen een licentie hebben voor de Azure-SSIS-IR](how-to-develop-azure-ssis-ir-licensed-components.md).


## <a name="current-limitations"></a>Huidige beperkingen

-   Als u wilt gebruiken `gacutil.exe` voor het installeren van assembly's in de Global Assembly Cache (GAC), moet u opgeven `gacutil.exe` als onderdeel van uw aangepaste installatie, of gebruik het exemplaar dat is opgegeven in de openbare Preview-container.

-   Als u wilt verwijzen naar een submap in uw script `msiexec.exe` biedt geen ondersteuning voor de `.\` notatie om te verwijzen naar de hoofdmap. Gebruik een opdracht zoals `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` in plaats van `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`.

-   Als u moet uw Azure-SSIS-IR met aangepaste installatie toevoegen aan een virtueel netwerk, worden alleen virtueel netwerk met Azure Resource Manager wordt ondersteund. Klassiek virtueel netwerk wordt niet ondersteund.

-   Administratorshare is momenteel niet ondersteund in de Azure-SSIS-IR

## <a name="prerequisites"></a>Vereisten

Voor het aanpassen van uw Azure-SSIS-IR, moet u de volgende zaken:

-   [Azure-abonnement](https://azure.microsoft.com/)

-   [Een Azure SQL Database of instantie beheerde server](https://ms.portal.azure.com/#create/Microsoft.SQLServer)

-   [Uw Azure-SSIS-IR inrichten](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

-   [Een Azure Storage-account](https://azure.microsoft.com/services/storage/). Voor aangepaste installatie uploaden en uw script met aangepaste installatie en de bijbehorende bestanden opslaan in een blob-container. Het proces met aangepaste installatie wordt ook de logboekbestanden voor het uitvoeren naar de blobcontainer met dezelfde geüpload.

## <a name="instructions"></a>Instructies

2.  Download en installeer [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) (versie 5,4 of hoger).

3.  Bereid uw script met aangepaste installatie en de bijbehorende bestanden (bijvoorbeeld, .bat, .cmd, .exe, dll-, MSI- of .ps1-bestanden).

    1.  U moet een scriptbestand met de naam hebben `main.cmd`, namelijk het toegangspunt voor uw aangepaste instellingen.

    2.  Als u wilt dat extra logboeken die worden gegenereerd door andere hulpprogramma's (bijvoorbeeld `msiexec.exe`) om te worden geüpload naar de container, geef de vooraf gedefinieerde omgevingsvariabele `CUSTOM_SETUP_SCRIPT_LOG_DIR` als de logboekmap in uw scripts (bijvoorbeeld `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

4.  Downloaden, installeren en starten [Azure Opslagverkenner](http://storageexplorer.com/).

    1.  Onder **(lokaal en Attached)** rechts Selecteer **Opslagaccounts** en selecteer **verbinding maken met Azure storage**.

       ![Verbinding maken met Azure Storage](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

    2.  Selecteer **gebruik van een naam van het opslagaccount en de sleutel** en selecteer **volgende**.

       ![De naam en sleutel van een opslagaccount gebruiken](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

    3.  Voer uw Azure Storage-accountnaam en de sleutel, selecteer **volgende**, en selecteer vervolgens **Connect**.

       ![Geef de naam van de store-account en de sleutel](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

    4.  Onder uw Azure Storage-account van verbonden met de rechtermuisknop op **Blob-Containers**, selecteer **Blob-Container maken**, en de naam van de nieuwe container.

       ![Een blob-container maken](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

    5.  Selecteer de nieuwe container en uw script met aangepaste installatie en de bijbehorende bestanden uploaden. Zorg ervoor dat u uploadt `main.cmd` op het hoogste niveau van de container, niet in een map. 

       ![Bestanden uploaden naar de blob-container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

    6.  Met de rechtermuisknop op de container en selecteer **Shared Access Signature ophalen**.

       ![De Shared Access Signature ophalen voor de container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

    7.  De SAS-URI voor de container maken met een lang genoeg verlooptijd en lezen + schrijven + lijst machtigingen. U moet de SAS-URI te downloaden en uitvoeren van uw script met aangepaste installatie en de bijbehorende bestanden telkens wanneer een knooppunt van uw Azure-SSIS-IR hersteld met een installatiekopie/opnieuw gestart is. U hebt schrijfmachtigingen voor het uploaden van de logboekbestanden voor het uitvoeren van setup.

        > [!IMPORTANT]
        > Zorg ervoor dat de SAS-URI niet verloopt en aangepaste installatie bronnen altijd beschikbaar tijdens de hele levenscyclus van uw Azure-SSIS IR, van het maken van een verwijdering, zijn met name als u regelmatig stoppen en starten van uw Azure-SSIS-IR tijdens deze periode.

       ![De Shared Access Signature voor de container genereren](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

    8.  Kopieer en sla de SAS-URI van de container.

       ![Kopiëren en opslaan van de Shared Access Signature](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

    9.  Wanneer u inrichten of uw Azure-SSIS-IR PowerShell opnieuw configureren voordat u uw Azure-SSIS-IR, voert de `Set-AzureRmDataFactoryV2IntegrationRuntime` cmdlet uit met de SAS-URI van de container als de waarde voor nieuwe `SetupScriptContainerSasUri` parameter. Bijvoorbeeld:

       ```powershell
       Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                  -Name $MyAzureSsisIrName `
                                                  -ResourceGroupName $MyResourceGroupName `
                                                  -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

       Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                    -Name $MyAzureSsisIrName `
                                                    -ResourceGroupName $MyResourceGroupName
       ```

    10.  Nadat u uw aangepaste setup is voltooid en uw Azure-SSIS-IR begint, vindt u de standaarduitvoer van `main.cmd` en andere uitvoering in de `main.cmd.log` map van uw storage-container.

2.  Andere voorbeelden van aangepaste installatie, verbinding maken met de openbare Preview-container met Azure Storage Explorer zien.

    a.  Onder **(lokaal en Attached)**, met de rechtermuisknop op **Opslagaccounts**, selecteer **verbinding maken met Azure storage**, selecteer **gebruik een verbindingsreeks of een gedeelde toegang handtekening URI**, en selecteer vervolgens **volgende**.

       ![Verbinding maken met Azure-opslag met de Shared Access Signature](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

    b.  Selecteer **een SAS-URI gebruikt** en voer de volgende SAS-URI voor de openbare Preview-container. Selecteer **volgende**, en selecteer **Connect**.

       `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

       ![Geef de Shared Access Signature voor de container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

    c. Selecteer de verbonden openbare Preview-container en dubbelklikt u op de `CustomSetupScript` map. In deze map zijn de volgende items:

       1. Een `Sample` map een aangepaste instellingen bevat voor het installeren van een eenvoudige taak op elk knooppunt van uw Azure-SSIS-IR De taak niet maar slaapstand enkele seconden. De map bevat ook een `gacutil` map `gacutil.exe`. Bovendien `main.cmd` bevat opmerkingen om te blijven behouden toegangsreferenties voor bestandsshares.

       2. Een `UserScenarios` map, die verschillende aangepaste instellingen voor scenario's met echte gebruiker bevat.

    ![Inhoud van de openbare preview-container](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

    d. Dubbelklik op de `UserScenarios` map. In deze map zijn de volgende items:

       1. Een `.NET FRAMEWORK 3.5` map een aangepaste instellingen bevat voor het installeren van een eerdere versie van .NET Framework die mogelijk vereist zijn voor aangepaste onderdelen op elk knooppunt van uw Azure-SSIS-IR

       2. Een `BCP` map waarin u een aangepaste installatie voor de installatie van SQL Server-opdrachtregelprogramma (`MsSqlCmdLnUtils.msi`), met inbegrip van het programma voor bulksgewijs kopiëren (`bcp`), op elk knooppunt van uw Azure-SSIS-IR

       3. Een `EXCEL` map een aangepaste instellingen bevat voor het installeren van de open source-assembly's (`DocumentFormat.OpenXml.dll`, `ExcelDataReader.DataSet.dll`, en `ExcelDataReader.dll`) op elk knooppunt van uw Azure-SSIS-IR

       4. Een `MSDTC` map een aangepaste instellingen bevat voor het wijzigen van de netwerk- en configuraties voor de service Microsoft Distributed Transaction Coordinator (MSDTC) op elk knooppunt van uw Azure-SSIS-IR Om ervoor te zorgen dat de MSDTC is gestart, voeg toe taak proces uitvoeren aan het begin van controlestroom in uw pakketten om de volgende opdracht: `%SystemRoot%\system32\cmd.exe /c powershell -Command "Start-Service MSDTC"` 

       5. Een `ORACLE ENTERPRISE` map waarin u een script met aangepaste installatie (`main.cmd`) en een installatie zonder toezicht-configuratiebestand (`client.rsp`) voor het installeren van het OCI van Oracle-stuurprogramma op elk knooppunt van uw Azure-SSIS IR Enterprise Edition. Deze instelling kunt u de Oracle-Verbindingsbeheer, de bron en bestemming gebruiken. Download de nieuwste Oracle-client - eerst bijvoorbeeld `winx64_12102_client.zip` - van [Oracle](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) en uploadt u dit samen met `main.cmd` en `client.rsp` naar de container. Als u verbinding maken met Oracle TNS hebt gebruikt, moet u ook downloaden `tnsnames.ora`, bewerken en uploaden naar de container, zodat deze in de installatiemap van Oracle tijdens de installatie kan worden gekopieerd.

       6. Een `ORACLE STANDARD` map waarin u een script met aangepaste installatie (`main.cmd`) voor het installeren van het stuurprogramma Oracle ODP.NET op elk knooppunt van uw Azure-SSIS-IR Deze instelling kunt u de ADO.NET-Verbindingsbeheer, de bron en bestemming gebruiken. Download de meest recente stuurprogramma ODP.NET Oracle - eerst bijvoorbeeld `ODP.NET_Managed_ODAC122cR1.zip` - van [Oracle](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html), en uploadt u dit samen met `main.cmd` naar de container.

       7. Een `SAP BW` map waarin u een script met aangepaste installatie (`main.cmd`) voor het installeren van de assembly van de connector SAP .NET (`librfc32.dll`) op elk knooppunt van uw Azure-SSIS IR Enterprise Edition. Deze instelling kunt u de SAP BW Connection Manager, de bron en bestemming gebruiken. Eerst de 64-bits of de 32-bits versie van uploaden `librfc32.dll` uit de SAP-installatiemap naar de container, samen met `main.cmd`. Het script kopieert de SAP-assembly in de `%windir%\SysWow64` of `%windir%\System32` map tijdens de installatie.

       8. Een `STORAGE` map waarin u een aangepaste installatie om Azure PowerShell installeren op elk knooppunt van uw Azure-SSIS-IR Deze instelling kunt u implementeren en uitvoeren van SSIS-pakketten die worden uitgevoerd [PowerShell-scripts voor het bewerken van uw Azure Storage-account](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). Kopiëren `main.cmd`, een voorbeeld van een `AzurePowerShell.msi` (of installeer de nieuwste versie), en `storage.ps1` aan de container. PowerShell.dtsx als sjabloon gebruiken voor uw pakketten. De pakketsjabloon combineert een [Azure Blob downloaden taak](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), downloads `storage.ps1` als een bewerkbaar PowerShell-script en een [taak proces uitvoeren](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) die het script wordt uitgevoerd op elk knooppunt.

       9. Een `TERADATA` map waarin u een script met aangepaste installatie (`main.cmd)`, het bijbehorende bestand (`install.cmd`), en installer-pakketten (`.msi`). Deze bestanden installeren Teradata-connectors, de TPT API en het ODBC-stuurprogramma op elk knooppunt van uw Azure-SSIS IR Enterprise Edition. Deze instelling kunt u de Teradata Connection Manager, de bron en bestemming gebruiken. Download eerst het Teradata-hulpprogramma's en hulpprogramma's (TTU) 15.x zip-bestand (bijvoorbeeld `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) van [Teradata](http://partnerintelligence.teradata.com), en upload dit samen met de bovenstaande `.cmd` en `.msi` bestanden naar de container.

    ![Mappen in de map van de gebruiker-scenario 's](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

    e. Als u wilt proberen deze voorbeelden aangepaste installatie, kopieer en plak de inhoud van de geselecteerde map in de container. Wanneer u inrichten of opnieuw configureren van uw Azure-SSIS-IR met PowerShell, voert de `Set-AzureRmDataFactoryV2IntegrationRuntime` cmdlet uit met de SAS-URI van de container als de waarde voor nieuwe `SetupScriptContainerSasUri` parameter.

## <a name="next-steps"></a>Volgende stappen

-   [Enterprise-editie van de Runtime Azure SSIS-integratie](how-to-configure-azure-ssis-ir-enterprise-edition.md)

-   [Het ontwikkelen van betaald of aangepaste onderdelen voor de integratie van Azure SSIS runtime licentie](how-to-develop-azure-ssis-ir-licensed-components.md)

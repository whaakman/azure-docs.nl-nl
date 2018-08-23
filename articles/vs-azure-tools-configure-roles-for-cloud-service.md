---
title: Configureren van de rollen voor een Azure-cloudservice met Visual Studio | Microsoft Docs
description: Informatie over het instellen en configureren van rollen voor Azure cloudservices met behulp van Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/21/2017
ms.author: ghogen
ms.openlocfilehash: 010a345d0bc756855b9a85660afcd647d111db75
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054504"
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a>Azure-cloud service-rollen configureren met Visual Studio
Een Azure-cloud-service kan hebben een of meer werknemer of web-rollen. Voor elke rol moet u definiëren hoe deze rol is ingesteld en ook configureren hoe deze rol wordt uitgevoerd. Zie voor meer informatie over rollen in cloudservices, de video [Inleiding tot Azure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). 

De informatie voor uw cloudservice wordt opgeslagen in de volgende bestanden:

- **ServiceDefinition.csdef** -het servicedefinitiebestand definieert de runtime-instellingen voor uw service in de cloud met inbegrip van welke rollen zijn vereist, eindpunten en de grootte van virtuele machine. Geen van de gegevens die zijn opgeslagen in `ServiceDefinition.csdef` kan worden gewijzigd wanneer uw functie wordt uitgevoerd.
- **ServiceConfiguration.cscfg** : het serviceconfiguratiebestand configureert u het aantal exemplaren van een rol worden uitgevoerd en de waarden van de instellingen gedefinieerd voor een rol. De gegevens die zijn opgeslagen in `ServiceConfiguration.cscfg` kan worden gewijzigd terwijl de functie wordt uitgevoerd.

Voor het opslaan van verschillende waarden voor de instellingen die bepalen hoe een rol wordt uitgevoerd, kunt u meerdere configuraties definiëren. U kunt een andere service-configuratie gebruiken voor elke implementatieomgeving. U kunt bijvoorbeeld instellen dat de opslagverbindingsreeks voor een account op de lokale Azure-opslagemulator gebruiken in de configuratie van een lokale service en maak een andere serviceconfiguratie voor het gebruik van Azure-opslag in de cloud.

Wanneer u een Azure-cloud-service in Visual Studio maakt, worden twee serviceconfiguraties automatisch gemaakt en toegevoegd aan uw Azure-project:

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a>Een Azure-cloudservice configureren
U kunt een Azure-cloud-service vanuit Solution Explorer in Visual Studio configureren zoals wordt weergegeven in de volgende stappen uit:

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **eigenschappen**.
   
    ![Contextmenu van Solution Explorer-project](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. Op de pagina van de eigenschappen van het project, selecteer de **ontwikkeling** tabblad. 

    ![Project - eigenschappenpagina voor ontwikkeling-tabblad](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. In de **serviceconfiguratie** , selecteert u de naam van de configuratie van de service die u wilt bewerken. (Als u wijzigingen aanbrengen in de serviceconfiguraties voor deze rol wilt, selecteert u **alle configuraties**.)
   
    > [!IMPORTANT]
    > Als u de configuratie van een specifieke service kiest, worden sommige eigenschappen zijn uitgeschakeld omdat ze kunnen alleen worden ingesteld voor alle configuraties. Als u wilt deze eigenschappen bewerken, moet u **alle configuraties**.
    > 
    > 
   
    ![Lijst met configuratie van de service voor een Azure-cloudservice](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-the-number-of-role-instances"></a>Het aantal rolinstanties wijzigen
Ter verbetering van de prestaties van uw cloudservice, kunt u het aantal exemplaren van een rol die worden uitgevoerd op basis van het aantal gebruikers of de belasting verwacht voor een bepaalde rol wijzigen. Een aparte virtuele machine wordt gemaakt voor elk exemplaar van een rol wanneer de service in de cloud wordt uitgevoerd in Azure. Dit is van invloed op de facturering voor de implementatie van deze service in de cloud. Zie voor meer informatie over facturering [meer informatie over uw factuur voor Microsoft Azure](billing/billing-understand-your-bill.md).

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouw het projectknooppunt. Onder de **rollen** knooppunt, met de rechtermuisknop op de rol die u wilt bijwerken, en selecteer in het contextmenu **eigenschappen**.

    ![Contextmenu van Solution Explorer Azure rol](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecteer de **configuratie** tabblad.

    ![Tabblad configuratie](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. In de **serviceconfiguratie** , selecteert u de configuratie van de service die u wilt bijwerken.
   
    ![Configuratie van de service-lijst](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. In de **aantal instanties** tekst voert u het aantal exemplaren dat u voor deze rol wilt starten. Elk exemplaar wordt uitgevoerd op een afzonderlijke virtuele machine als u de service in de cloud naar Azure publiceren.

    ![Bijwerken van het aantal instanties](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. In de Visual Studio naar werkbalk, selecteer **opslaan**.

## <a name="manage-connection-strings-for-storage-accounts"></a>Verbindingsreeksen voor storage-accounts beheren
U kunt toevoegen, verwijderen of wijzigen van tekenreeksen voor databaseverbindingen voor uw serviceconfiguraties. Bijvoorbeeld, wilt u mogelijk een lokale verbindingsreeks voor de configuratie van een lokale service die een waarde van heeft `UseDevelopmentStorage=true`. U kunt ook het configureren van een cloud service-configuratie die gebruikmaakt van een storage-account in Azure.

> [!WARNING]
> Wanneer u de Azure storage-account-sleutelgegevens voor de verbindingsreeks van een storage-account opgeeft, wordt deze informatie lokaal opgeslagen in het configuratiebestand van de service. Deze informatie is echter momenteel niet opgeslagen als gecodeerde tekst.
> 
> 

Met behulp van een andere waarde voor de configuratie van elke service, hoeft u niet te gebruiken verschillende verbindingsreeksen in uw cloudservice of uw code te wijzigen wanneer u uw cloudservice naar Azure publiceert. U kunt dezelfde naam voor de verbindingsreeks gebruiken in uw code en de waarde is verschillend zijn, op basis van de configuratie van de service die u selecteert wanneer u uw cloudservice maakt of wanneer u deze publiceert.

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouw het projectknooppunt. Onder de **rollen** knooppunt, met de rechtermuisknop op de rol die u wilt bijwerken, en selecteer in het contextmenu **eigenschappen**.

    ![Contextmenu van Solution Explorer Azure rol](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecteer de **instellingen** tabblad.

    ![Tabblad instellingen](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. In de **serviceconfiguratie** , selecteert u de configuratie van de service die u wilt bijwerken.

    ![Configuratie van de service](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Selecteer om een verbindingsreeks toe **instelling toevoegen**.

    ![Verbindingsreeks toevoegen](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Zodra de nieuwe instelling is toegevoegd aan de lijst, bijwerken van de rij in de lijst met de benodigde informatie.

    ![Nieuwe verbindingsreeks](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Naam** -Geef de naam die u wilt gebruiken voor de verbindingsreeks.
    - **Type** : Selecteer **Connection String** uit de vervolgkeuzelijst.
    - **Waarde** -kunt u opgeven de connection string rechtstreeks in de **waarde** cel of Selecteer het weglatingsteken (...) om te werken in de **Create Storage Connection String** dialoogvenster.  

1. In de **Create Storage Connection String** dialoogvenster, selecteer een optie voor **verbinding maken met behulp van**. Volg de instructies voor de optie die u selecteert:

    - **Microsoft Azure-opslagemulator** -als u deze optie selecteert, de overige instellingen in het dialoogvenster zijn uitgeschakeld omdat ze alleen van toepassing op Azure. Selecteer **OK**.
    - **Uw abonnement** : als u deze optie selecteert, gebruikt u de vervolgkeuzelijst op selecteren en meld u aan bij een Microsoft-account of een Microsoft-account toevoegen. Selecteer een Azure-abonnement en storage-account. Selecteer **OK**.
    - **Referenties handmatig worden ingevoerd** -naam van het opslagaccount en de primaire of de tweede sleutel invoeren. Selecteer een optie voor **verbinding** (HTTPS wordt aanbevolen voor de meeste scenario's.) Selecteer **OK**.

1. Als u wilt verwijderen van een verbindingsreeks, selecteert u de verbindingsreeks en selecteer vervolgens **instelling verwijderen**.

1. In de Visual Studio naar werkbalk, selecteer **opslaan**.

## <a name="programmatically-access-a-connection-string"></a>Programmatisch toegang verkrijgen tot een verbindingsreeks

De volgende stappen laten zien hoe u programmatisch toegang verkrijgen tot een verbindingsreeks met behulp van C#.

1. Voeg de volgende using-instructies in een C#-bestand waarin u gaat de instelling te gebruiken:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. De volgende code toont een voorbeeld van hoe u een verbindingsreeks kunt bekijken. Vervang de &lt;ConnectionStringName > tijdelijke aanduiding door de juiste waarde. 

    ```csharp
    // Setup the connection to Azure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Aangepaste instellingen moet worden gebruikt in uw Azure-cloud-service toevoegen
Aangepaste instellingen in het configuratiebestand van de service kunt u een naam en waarde voor een tekenreeks op voor een specifieke service-configuratie toevoegen. U kunt deze instelling gebruiken om een functie in uw cloudservice configureren met het lezen van de instelling en het gebruik van deze waarde voor het beheren van de logica in uw code. U kunt deze service-configuratiewaarden wijzigen zonder opnieuw opbouwen van uw service-pakket of wanneer uw cloudservice wordt uitgevoerd. Uw code kunt meldingen van controleren wanneer een instelling wordt gewijzigd. Zie [RoleEnvironment.Changing gebeurtenis](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

U kunt toevoegen, verwijderen of wijzigen van de aangepaste instellingen voor de serviceconfiguraties van uw. Wilt u mogelijk verschillende waarden voor deze tekenreeksen voor verschillende configuraties.

Met behulp van een andere waarde voor de configuratie van elke service, hoeft u niet te gebruiken van andere tekenreeksen in uw cloudservice of uw code te wijzigen wanneer u uw cloudservice naar Azure publiceert. U kunt dezelfde naam voor de tekenreeks in uw code en de waarde is verschillend zijn, op basis van de configuratie van de service die u selecteert wanneer u uw cloudservice maakt of wanneer u deze publiceert.

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouw het projectknooppunt. Onder de **rollen** knooppunt, met de rechtermuisknop op de rol die u wilt bijwerken, en selecteer in het contextmenu **eigenschappen**.

    ![Contextmenu van Solution Explorer Azure rol](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecteer de **instellingen** tabblad.

    ![Tabblad instellingen](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. In de **serviceconfiguratie** , selecteert u de configuratie van de service die u wilt bijwerken.

    ![Configuratie van de service-lijst](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Selecteer om een aangepaste instelling toe **instelling toevoegen**.

    ![Aangepaste instelling toevoegen](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Zodra de nieuwe instelling is toegevoegd aan de lijst, bijwerken van de rij in de lijst met de benodigde informatie.

    ![Nieuwe aangepaste instelling](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Naam** -Geef de naam van de instelling.
    - **Type** : Selecteer **tekenreeks** uit de vervolgkeuzelijst.
    - **Waarde** -Geef de waarde van de instelling. U kunt ofwel de waarde rechtstreeks in de **waarde** cel of Selecteer het beletselteken (...) in te voeren van de waarde in de **tekenreeks bewerken** dialoogvenster.  

1. Als u wilt een aangepaste instelling verwijderen, selecteert u de instelling en selecteer vervolgens **instelling verwijderen**.

1. In de Visual Studio naar werkbalk, selecteer **opslaan**.

## <a name="programmatically-access-a-custom-settings-value"></a>Programmatisch toegang verkrijgen tot de waarde van een aangepaste instelling
 
De volgende stappen laten zien hoe u programmatisch toegang verkrijgen tot een aangepaste instelling met behulp van C#.

1. Voeg de volgende using-instructies in een C#-bestand waarin u gaat de instelling te gebruiken:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. De volgende code toont een voorbeeld van hoe u een aangepaste instelling kunt bekijken. Vervang de &lt;Instellingsnaam > tijdelijke aanduiding door de juiste waarde. 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Beheer lokale opslag voor elk rolexemplaar
U kunt de lokale opslag bestandssysteem voor elk exemplaar van een rol toevoegen. De gegevens die zijn opgeslagen in deze opslag is niet toegankelijk door andere exemplaren van de rol voor de gegevens worden opgeslagen, of door andere rollen.  

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouw het projectknooppunt. Onder de **rollen** knooppunt, met de rechtermuisknop op de rol die u wilt bijwerken, en selecteer in het contextmenu **eigenschappen**.

    ![Contextmenu van Solution Explorer Azure rol](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecteer de **lokale opslag** tabblad.

    ![Tabblad lokale opslag](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. In de **serviceconfiguratie** lijst, zorg ervoor dat **alle configuraties** is geselecteerd als de instellingen voor lokale opslag van toepassing op alle serviceconfiguraties. Een andere waarde resulteert in de invoer velden op de pagina wordt uitgeschakeld. 

    ![Configuratie van de service-lijst](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. Als u wilt toevoegen een vermelding voor lokale opslag, selecteer **lokale opslag toevoegen**.

    ![Lokale opslag toevoegen](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. Nadat het nieuwe item in de lokale opslag is toegevoegd aan de lijst, worden de rij in de lijst met de benodigde informatie bijgewerkt.

    ![Nieuwe vermelding met lokale opslag](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - **Naam** -Geef de naam die u wilt gebruiken voor de nieuwe lokale opslag.
    - **Grootte (MB)** -Geef de grootte in MB op die u nodig hebt voor de nieuwe lokale opslag.
    - **Opschonen van de rol te recyclen** : Selecteer deze optie om de gegevens in de nieuwe lokale opslag verwijderen wanneer de virtuele machine voor de rol gerecycled wordt.

1. Als u wilt verwijderen een vermelding voor lokale opslag, selecteer de vermelding en selecteer vervolgens **lokale opslag verwijderen**.

1. In de Visual Studio naar werkbalk, selecteer **opslaan**.

## <a name="programmatically-accessing-local-storage"></a>Programmatisch toegang tot lokale opslag

In deze sectie ziet u hoe u programmatisch toegang verkrijgen tot lokale opslag met behulp van C# door het schrijven van een Testtekstbestand `MyLocalStorageTest.txt`.  

### <a name="write-a-text-file-to-local-storage"></a>Schrijven van een tekstbestand met lokale opslag

De volgende code toont een voorbeeld van het schrijven van een tekstbestand met lokale opslag. Vervang de &lt;LocalStorageName > tijdelijke aanduiding door de juiste waarde. 

    ```csharp
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-to-local-storage"></a>Zoeken naar een bestand geschreven naar de lokale opslag

Als u wilt weergeven van het bestand door de code in de vorige sectie hebt gemaakt, de volgende stappen uit:
    
1.  In het systeemvak Windows met de rechtermuisknop op het pictogram van Azure en, in het contextmenu selecteren **weergeven gebruikersinterface van de Rekenemulator**. 

    ![Azure-rekenemulator weergeven](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. Selecteer de Webrol.

    ![Azure-rekenemulator](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. Op de **Microsoft Azure-Rekenemulator** in het menu **extra** > **Open lokale archief**.

    ![Open lokale archief menu-item](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. Wanneer het Windows Explorer-venster wordt geopend, voert u ' MyLocalStorageTest.txt'' in de **zoeken** tekstvak in en selecteer **Enter** wilt beginnen met zoeken. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Azure-projecten in Visual Studio door te lezen [configureren van een Azure-Project](vs-azure-tools-configuring-an-azure-project.md). Meer informatie over het schema van de cloud-service door te lezen [schemaverwijzing](https://msdn.microsoft.com/library/azure/dd179398).


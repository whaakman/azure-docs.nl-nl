---
title: De rollen configureert voor een Azure-cloudservice met Visual Studio | Microsoft Docs
description: Informatie over het instellen en configureren van rollen voor Azure-cloudservices met Visual Studio.
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 17da71ac0c5ab9330b9244c0354e4d161d98229e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a>Azure cloud service-rollen configureren met Visual Studio
Een Azure-cloud-service kan een of meer werkprocessen of webrollen hebben. Voor elke rol moet u definiëren hoe die rol is ingesteld en te configureren hoe die rol wordt uitgevoerd. Zie voor meer informatie over functies in cloudservices, de video [Inleiding tot Azure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). 

De gegevens voor de cloudservice worden opgeslagen in de volgende bestanden:

- **ServiceDefinition.csdef** -het servicedefinitiebestand definieert de runtime-instellingen voor uw cloudservice, met inbegrip van welke rollen zijn vereist, de eindpunten en de grootte van de virtuele machine. Geen van de gegevens die zijn opgeslagen in `ServiceDefinition.csdef` kunnen worden gewijzigd wanneer de functie wordt uitgevoerd.
- **ServiceConfiguration.cscfg** - configuratiebestand van de service wordt geconfigureerd hoeveel exemplaren van een rol worden uitgevoerd en de waarden van de instellingen voor een rol gedefinieerd. De gegevens die zijn opgeslagen `ServiceConfiguration.cscfg` kan worden gewijzigd terwijl uw rol wordt uitgevoerd.

U kunt meerdere configuraties definiëren voor het opslaan van verschillende waarden voor de instellingen die bepalen hoe een rol wordt uitgevoerd. U kunt een andere service-configuratie gebruiken voor elke implementatieomgeving. U kunt bijvoorbeeld instellen dat de opslagverbindingsreeks voor een account op de lokale Azure-opslagemulator gebruiken in de configuratie van een lokale service en maak een andere serviceconfiguratie voor het gebruik van Azure-opslag in de cloud.

Wanneer u een Azure cloudservice in Visual Studio maakt, worden twee serviceconfiguraties automatisch gemaakt en toegevoegd aan uw Azure-project:

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a>Een Azure cloudservice configureren
U kunt een Azure-cloud-service in Solution Explorer in Visual Studio configureren zoals wordt weergegeven in de volgende stappen uit:

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, met de rechtermuisknop op het project en selecteer in het contextmenu **eigenschappen**.
   
    ![Solution Explorer project contextmenu](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. Selecteer in de pagina eigenschappen van het project de **ontwikkeling** tabblad. 

    ![Pagina in de project-eigenschappen - ontwikkeling tabblad](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. In de **serviceconfiguratie** , selecteert u de naam van de configuratie van de service die u wilt bewerken. (Als u wijzigingen aanbrengen op de serviceconfiguraties voor deze rol wilt, selecteert u **alle configuraties**.)
   
    > [!IMPORTANT]
    > Als u een specifieke service-configuratie kiest, worden sommige eigenschappen zijn uitgeschakeld omdat ze kunnen alleen worden ingesteld voor alle configuraties. Deze eigenschappen bewerken, moet u **alle configuraties**.
    > 
    > 
   
    ![Lijst van de configuratie van de service voor een Azure cloudservice](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-the-number-of-role-instances"></a>Het aantal rolinstanties wijzigen
Om te verbeteren de prestaties van uw cloudservice, kunt u het aantal exemplaren van een rol die worden uitgevoerd op basis van het aantal gebruikers of de belasting voor een bepaalde functie verwacht. Een afzonderlijke virtuele machine wordt gemaakt voor elk exemplaar van een rol wanneer de cloudservice wordt uitgevoerd in Azure. Dit is van invloed op de facturering voor de implementatie van deze cloudservice. Zie voor meer informatie over facturering [inzicht in uw factuur voor Microsoft Azure](billing/billing-understand-your-bill.md).

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouw het projectknooppunt. Onder de **rollen** knooppunt met de rechtermuisknop op de rol die u wilt bijwerken, en selecteer in het contextmenu **eigenschappen**.

    ![Solution Explorer Azure rol contextmenu](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecteer de **configuratie** tabblad.

    ![Tabblad configuratie](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. In de **serviceconfiguratie** , selecteert u de configuratie van de service die u wilt bijwerken.
   
    ![Lijst van de configuratie van de service](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. In de **aantal exemplaar** tekst en voer het nummer van exemplaren die u voor deze rol wilt starten. Elk exemplaar wordt uitgevoerd op een afzonderlijke virtuele machine als u de cloudservice naar Azure publiceren.

    ![Bijwerken van het aantal exemplaren](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. Vanuit de Visual Studio werkbalk, selecteer **opslaan**.

## <a name="manage-connection-strings-for-storage-accounts"></a>Tekenreeksen voor databaseverbindingen voor storage-accounts beheren
U kunt toevoegen, verwijderen of wijzigen van tekenreeksen voor databaseverbindingen voor uw serviceconfiguraties. Bijvoorbeeld, wilt u mogelijk een lokale verbindingsreeks voor de configuratie van een lokale service die een waarde van heeft `UseDevelopmentStorage=true`. U kunt ook de configuratie van een cloud-service die gebruikmaakt van een opslagaccount in Azure te configureren.

> [!WARNING]
> Wanneer u de Azure storage-account-sleutelgegevens voor een verbindingsreeks voor opslag account opgeeft, wordt deze informatie lokaal opgeslagen in het configuratiebestand van de service. Deze informatie wordt echter momenteel niet opgeslagen als gecodeerde tekst.
> 
> 

U hebt via een andere waarde voor elke serviceconfiguratie niet voor het gebruik van verschillende verbindingsreeksen in uw cloudservice of uw code te wijzigen wanneer u uw cloudservice naar Azure publiceert. U kunt dezelfde naam voor de verbindingsreeks gebruiken in uw code en de waarde is verschillend zijn, op basis van de configuratie van de service die u selecteert wanneer u uw cloudservice maakt of wanneer u deze hebt gepubliceerd.

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouw het projectknooppunt. Onder de **rollen** knooppunt met de rechtermuisknop op de rol die u wilt bijwerken, en selecteer in het contextmenu **eigenschappen**.

    ![Solution Explorer Azure rol contextmenu](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecteer de **instellingen** tabblad.

    ![Tabblad instellingen](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. In de **serviceconfiguratie** , selecteert u de configuratie van de service die u wilt bijwerken.

    ![Configuratie van de service](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Selecteer om een verbindingsreeks toe **instelling toevoegen**.

    ![Verbindingsreeks toevoegen](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Nadat de nieuwe instelling is toegevoegd aan de lijst, worden de rij in de lijst met de benodigde informatie bijgewerkt.

    ![Nieuwe verbindingsreeks](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Naam** -Voer de naam die u wilt gebruiken voor de verbindingsreeks.
    - **Type** : Selecteer **verbindingsreeks** uit de vervolgkeuzelijst.
    - **Waarde** -kunt u de verbindingsreeks rechtstreeks in invoeren de **waarde** cellen of Selecteer het weglatingsteken (...) om te werken in de **Create Storage Connection String** dialoogvenster.  

1. In de **Create Storage Connection String** dialoogvenster, selecteert u een optie voor **verbinding maken via**. Volg de instructies voor de geselecteerde optie:

    - **Microsoft Azure-opslagemulator** -als u deze optie selecteert, de overige instellingen in het dialoogvenster zijn uitgeschakeld, omdat ze alleen van toepassing op Azure. Selecteer **OK**.
    - **Uw abonnement** : als u deze optie selecteert, gebruikt u de vervolgkeuzelijst ofwel selecteren en meld u aan bij een Microsoft-account of een Microsoft-account toevoegen. Selecteer een Azure-abonnement en storage-account. Selecteer **OK**.
    - **Referenties handmatig worden ingevoerd** -naam van het opslagaccount en de primaire of tweede sleutel opgeven. Selecteer een optie voor **verbinding** (HTTPS wordt aanbevolen voor de meeste scenario's.) Selecteer **OK**.

1. Als u wilt verwijderen een verbindingsreeks, selecteert u de verbindingsreeks en selecteer vervolgens **instelling verwijderen**.

1. Vanuit de Visual Studio werkbalk, selecteer **opslaan**.

## <a name="programmatically-access-a-connection-string"></a>Programmatisch toegang verkrijgen tot een verbindingsreeks

De volgende stappen laten zien hoe programmatisch toegang verkrijgen tot een verbindingsreeks met C#.

1. Voeg het volgende toe met behulp van instructies naar een C#-bestand dat u gaat waar de instelling te gebruiken:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. De volgende code ziet u een voorbeeld van hoe u toegang tot een verbindingsreeks. Vervang de &lt;ConnectionStringName > aanduiding voor items met de juiste waarde. 

    ```csharp
    // Setup the connection to Azure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Toevoegen van aangepaste instellingen te gebruiken in uw Azure-cloud-service
Aangepaste instellingen in het configuratiebestand van de service kunt u een naam en waarde voor een tekenreeks op voor de configuratie van een specifieke service toevoegen. U kunt deze instelling te gebruiken voor het configureren van een functie in uw cloudservice door te lezen van de waarde van de instelling en deze waarde om te bepalen van de logica in uw code. U kunt de configuratiewaarden van deze service wijzigen zonder opnieuw opbouwen van het servicepakket of wanneer uw cloudservice wordt uitgevoerd. Uw code kunt meldingen van controleren wanneer een instelling wordt gewijzigd. Zie [RoleEnvironment.Changing gebeurtenis](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

U kunt toevoegen, verwijderen of wijzigen van de aangepaste instellingen voor uw serviceconfiguraties. Wilt u mogelijk verschillende waarden voor deze tekenreeksen voor verschillende configuraties.

U hebt via een andere waarde voor elke serviceconfiguratie niet voor het gebruik van verschillende tekenreeksen in uw cloudservice of uw code te wijzigen wanneer u uw cloudservice naar Azure publiceert. U kunt dezelfde naam voor de tekenreeks in uw code en de waarde is verschillend zijn, op basis van de configuratie van de service die u selecteert wanneer u uw cloudservice maakt of wanneer u deze hebt gepubliceerd.

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouw het projectknooppunt. Onder de **rollen** knooppunt met de rechtermuisknop op de rol die u wilt bijwerken, en selecteer in het contextmenu **eigenschappen**.

    ![Solution Explorer Azure rol contextmenu](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecteer de **instellingen** tabblad.

    ![Tabblad instellingen](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. In de **serviceconfiguratie** , selecteert u de configuratie van de service die u wilt bijwerken.

    ![Lijst van de configuratie van de service](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. Selecteer om een aangepaste instelling toe **instelling toevoegen**.

    ![Aangepaste instelling toevoegen](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Nadat de nieuwe instelling is toegevoegd aan de lijst, worden de rij in de lijst met de benodigde informatie bijgewerkt.

    ![Nieuwe aangepaste instelling](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Naam** -Voer de naam van de instelling.
    - **Type** : Selecteer **tekenreeks** uit de vervolgkeuzelijst.
    - **Waarde** -Voer de waarde van de instelling. U kunt de waarde rechtstreeks in invoeren de **waarde** cellen of Selecteer het weglatingsteken (...) de waarde in te voeren in de **tekenreeks bewerken** dialoogvenster.  

1. Selecteer de instelling voor het verwijderen van een aangepaste instelling en selecteer vervolgens **instelling verwijderen**.

1. Vanuit de Visual Studio werkbalk, selecteer **opslaan**.

## <a name="programmatically-access-a-custom-settings-value"></a>Programmatisch toegang biedt tot de waarde van een aangepaste instelling
 
De volgende stappen laten zien hoe programmatisch toegang verkrijgen tot een aangepaste met C#-instelling.

1. Voeg het volgende toe met behulp van instructies naar een C#-bestand dat u gaat waar de instelling te gebruiken:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. De volgende code ziet u een voorbeeld van hoe u een aangepaste instelling. Vervang de &lt;Instellingsnaam > aanduiding voor items met de juiste waarde. 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Lokale opslag voor elk rolexemplaar beheren
U kunt de lokale system-bestandsopslag voor elk exemplaar van een rol toevoegen. De gegevens die zijn opgeslagen in de betreffende opslag is niet toegankelijk door andere exemplaren van de rol voor de gegevens worden opgeslagen of door andere rollen.  

1. Maak of open een Azure-cloud service-project in Visual Studio.

1. In **Solution Explorer**, vouw het projectknooppunt. Onder de **rollen** knooppunt met de rechtermuisknop op de rol die u wilt bijwerken, en selecteer in het contextmenu **eigenschappen**.

    ![Solution Explorer Azure rol contextmenu](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Selecteer de **lokale opslag** tabblad.

    ![Tabblad lokale opslag](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. In de **serviceconfiguratie** lijst **alle configuraties** is geselecteerd als de instellingen van lokale opslag van toepassing op alle serviceconfiguraties. Een andere waarde resulteert in de invoer velden op de pagina wordt uitgeschakeld. 

    ![Lijst van de configuratie van de service](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. Selecteer om de vermelding voor een lokale opslag toe **lokale opslag toevoegen**.

    ![Lokale opslag toevoegen](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. Nadat de nieuwe invoer van de lokale opslag is toegevoegd aan de lijst, worden de rij in de lijst met de benodigde informatie bijgewerkt.

    ![Nieuw item voor lokale opslag](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - **Naam** -Voer de naam die u wilt gebruiken voor het nieuwe lokale opslag.
    - **Grootte (MB)** -Geef de grootte in MB, die u nodig hebt voor het nieuwe lokale opslag.
    - **Schoon op functie Prullenbak** -Selecteer deze optie om de gegevens in de nieuwe lokale opslag verwijderen wanneer de virtuele machine voor de rol gerecycled wordt.

1. Selecteer de vermelding voor het verwijderen van een vermelding voor lokale opslag, en selecteer vervolgens **lokale opslag verwijderen**.

1. Vanuit de Visual Studio werkbalk, selecteer **opslaan**.

## <a name="programmatically-accessing-local-storage"></a>Programmatisch toegang tot lokale opslag

Deze sectie ziet u hoe u programmatisch toegang verkrijgen tot lokale opslag met C# met het schrijven van een Testtekstbestand `MyLocalStorageTest.txt`.  

### <a name="write-a-text-file-to-local-storage"></a>Een tekstbestand schrijven naar de lokale opslag

De volgende code toont een voorbeeld van hoe een tekstbestand schrijven naar de lokale opslag. Vervang de &lt;LocalStorageName > aanduiding voor items met de juiste waarde. 

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

### <a name="find-a-file-written-to-local-storage"></a>Zoeken naar een bestand dat is geschreven naar de lokale opslag

Als u wilt weergeven van het bestand gemaakt door de code in de vorige sectie, de volgende stappen uit:
    
1.  In de Windows-systeemvak met de rechtermuisknop op het pictogram voor Azure en, in het contextmenu Selecteer **weergeven Compute-Emulator UI**. 

    ![Azure-rekenemulator weergeven](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. Selecteer de Webrol.

    ![Azure-rekenemulator](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. Op de **Microsoft Azure Compute-Emulator** selecteert u **extra** > **Open lokale archief**.

    ![Open lokale archief menu-item](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. Wanneer de Windows Verkenner-venster wordt geopend, typt u ' MyLocalStorageTest.txt'' in de **Search** tekstvak in en selecteer **Enter** wilt beginnen met zoeken. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over Azure projecten in Visual Studio door te lezen [configureren van een Azure-Project](vs-azure-tools-configuring-an-azure-project.md). Meer informatie over het schema van cloud-service door te lezen [schemaverwijzing](https://msdn.microsoft.com/library/azure/dd179398).


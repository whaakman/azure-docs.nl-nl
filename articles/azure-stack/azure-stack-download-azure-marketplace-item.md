---
title: Downloaden van marketplace-items van Azure | Microsoft Docs
description: De operator cloud kunt marketplace-items downloaden van Azure naar Mijn Azure-Stack-implementatie.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 5d403f7c1d0fff466f6c0fb9942ec777ab820eab
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604529"
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Marketplace-items van Azure naar Azure Stack downloaden

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als een cloud-operator, downloaden van items vanuit Azure Marketplace en zodat ze beschikbaar zijn in Azure-Stack. De items die u kunt kiezen zijn van een samengestelde lijst van Azure Marketplace-items die vooraf zijn getest en ondersteund om te werken met Azure-Stack. Extra artikelen worden regelmatig toegevoegd aan deze lijst, dus blijven om te controleren op nieuwe inhoud weer. 

Er zijn twee scenario's voor het verbinden met Azure Marketplace: 

- **Een gekoppelde scenario** -die uw Azure-Stack-omgeving zijn verbonden met internet vereist. U kunt de Stack van Azure-portal gebruiken om te zoeken en downloaden van items. 
- **Een verbinding is verbroken of gedeeltelijk verbonden scenario** -waarvoor u toegang tot internet met behulp van de marketplace syndication-hulpprogramma voor het downloaden van de marketplace-items. Vervolgens brengt u uw downloads naar uw niet-verbonden Azure Stack-installatie. Dit scenario maakt gebruik van PowerShell.

Zie [Azure Marketplace-items voor Azure-Stack](azure-stack-marketplace-azure-items.md) voor een lijst van de marketplace-items die u kunt downloaden.


## <a name="connected-scenario"></a>Verbonden scenario
Als Azure-Stack verbinding met internet maakt, kunt u het beheerportal voor het downloaden van de marketplace-items.

### <a name="prerequisites"></a>Vereisten
Uw Azure-Stack-implementatie moet een internetverbinding heeft en worden [geregistreerd bij Azure](azure-stack-register.md).

### <a name="use-the-portal-to-download-marketplace-items"></a>De portal gebruiken voor het downloaden van de marketplace-items  
1. Aanmelden bij de Azure-Stack-beheerdersportal.

2.  Bekijk de beschikbare opslagruimte voor het downloaden van de marketplace-items. Wanneer u items downloaden selecteert, kunt u later de downloadgrootte vergelijken met de beschikbare opslagcapaciteit. Als een beperkte capaciteit heeft, kunt u de opties voor [het beheer van beschikbare ruimte](azure-stack-manage-storage-shares.md#manage-available-space). 

    Om te controleren van de beschikbare ruimte in **regio management** Selecteer de regio die u wilt verkennen en gaat u naar **Resourceproviders** > **opslag**.

    ![De opslagruimte controleren](media/azure-stack-download-azure-marketplace-item/storage.png)  

    
3. Open de Stack Azure Marketplace en verbinding maken met Azure. Om dit te doen, selecteert u **Marketplace management**, en selecteer vervolgens **toevoegen uit Azure**.

    ![Toevoegen van Azure](media/azure-stack-download-azure-marketplace-item/marketplace.png)

    De portal wordt weergegeven de lijst met items die worden gedownload vanuit Azure Marketplace. U kunt klikken op elk item om de beschrijving en aanvullende informatie over, onder andere de downloadgrootte weer te geven. 

    ![Lijst van de Marketplace](media/azure-stack-download-azure-marketplace-item/image03.png)

4. Selecteer het item dat u wilt en selecteer vervolgens **downloaden**. Downloadtijden variëren.

    ![Bericht downloaden](media/azure-stack-download-azure-marketplace-item/image04.png)

    Nadat het downloaden is voltooid, kunt u het nieuwe marketplace-item als een Azure-Stack operator of een gebruiker kunt implementeren.

5. Selecteer voor het implementeren van het gedownloade item **+ nieuw**, en zoek vervolgens tussen de categorieën voor het nieuwe marketplace-item. Selecteer naast het item om te beginnen met het implementatieproces. Het proces varieert voor verschillende marketplace-items. 

## <a name="disconnected-or-a-partially-connected-scenario"></a>De verbinding verbroken of een gedeeltelijk verbonden scenario

Als Azure-Stack in een modus voor niet-verbonden en zonder verbinding met internet is, gebruikt u PowerShell en de *marketplace syndication hulpprogramma* voor het downloaden van de marketplace-items aan een machine met een internetverbinding. U kunt vervolgens de items overbrengen aan uw Azure-Stack-omgeving. In een omgeving zonder verbinding, kunt u marketplace-items niet downloaden met behulp van de Stack van Azure-portal. 

Het hulpprogramma voor marketplace-syndicatie kan ook worden gebruikt in een scenario met verbonden. 

Er zijn twee delen in dit scenario:
- **Deel 1:** downloaden vanuit Azure Marketplace. Op de computer met internettoegang PowerShell configureren, downloadt u het hulpprogramma syndication en downloadt items formulier Azure Marketplace.  
- **Deel 2:** uploaden en publiceren naar de Stack Azure Marketplace. Verplaats de bestanden die u hebt gedownload met uw Azure-Stack-omgeving u importeren in Azure-Stack en ze te publiceren op de Stack Azure Marketplace.  


### <a name="prerequisites"></a>Vereisten
- De implementatie van uw Azure-Stack moet [geregistreerd bij Azure](azure-stack-register.md).  

- De computer die verbinding heeft met internet moet hebben **PowerShell-Module voor Azure Stack versie 1.2.11** of hoger. Als deze niet al aanwezig, [Azure Stack specifieke PowerShell-modules installeren](azure-stack-powershell-install.md).  

- Zodat het importeren van een gedownloade marketplace-item, de [PowerShell-omgeving voor de Azure-Stack-operator](azure-stack-powershell-configure-admin.md) moet worden geconfigureerd.  

- U moet hebben een [opslagaccount](azure-stack-manage-storage-accounts.md) in Azure-Stack met een openbaar toegankelijke container (dit is een blob storage). U kunt de container gebruiken als tijdelijke opslag voor de marketplace-items galerie-bestanden. Als u niet bekend met storage-accounts en containers bent, Zie [werken met blobs - Azure-portal](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) in de documentatie van Azure.

- De marketplace-syndicatie tool wordt gedownload tijdens de eerste procedure. 

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>De marketplace syndication-hulpprogramma gebruiken voor het downloaden van de marketplace-items

1. Open een PowerShell-console als beheerder op een computer met een internetverbinding.

2. De Azure-account dat u hebt gebruikt voor het registreren van de Azure-Stack toevoegen. Het account toevoegen in PowerShell uitvoeren `Add-AzureRmAccount` zonder parameters. U wordt gevraagd de referenties van uw Azure-account in te voeren en u wellicht 2-factor-verificatie op basis van de configuratie van uw account te gebruiken.

3. Als u meerdere abonnementen hebt, voer de volgende opdracht om te selecteren die u hebt gebruikt voor registratie:  

   ```PowerShell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Download de nieuwste versie van de marketplace syndication-hulpprogramma met behulp van het volgende script:  

   ```PowerShell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master

   ```

5. Importeer de module syndication en start vervolgens het hulpprogramma voor het volgende script uit te voeren. Vervang de *pad naar de doelmap* met een locatie voor het opslaan van de bestanden die u vanuit Azure Marketplace downloadt.   

   ```PowerShell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination "Destination folder path" `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Wanneer het hulpprogramma wordt uitgevoerd, wordt u gevraagd de referenties van uw Azure-account in te voeren. Aanmelden bij de Azure-account dat u hebt gebruikt voor het registreren van de Azure-Stack. Nadat de aanmelding is geslaagd, ziet u een scherm, zoals de volgende afbeelding met de lijst met beschikbare marketplace-items.  

   ![Azure Marketplace-items pop](media/azure-stack-download-azure-marketplace-item/image05.png)

7. Selecteer het item dat u wilt downloaden en noteer de *versie*. (U kunt meerdere installatiekopieën selecteren door de *Ctrl* sleutel.) U wordt verwezen naar de *versie* wanneer het importeren van het item in de volgende procedure. 
   
   U kunt ook de lijst met afbeeldingen filteren met behulp van de **criteria toevoegen** optie.

8. Selecteer **OK**, en vervolgens bekijken en accepteer de juridische bepalingen. 

9. De tijd die het downloaden duurt is afhankelijk van de grootte van het item. Nadat het downloaden is voltooid, is het item is beschikbaar in de map die u hebt opgegeven in het script. De download bevat een VHD-bestand (voor virtuele machines) of een. ZIP-bestand (voor uitbreidingen van de virtuele machine). Dit omvat ook een gallery-pakket in de *.azpkg* indeling. (A *.azpkg* pakket is een *.zip* bestand.)
 

### <a name="import-the-download-and-publish-to-azure-stack-marketplace"></a>Importeren van het downloaden en publiceren naar Azure Stack Marketplace
1. De bestanden voor installatiekopieën van virtuele machines of oplossingssjablonen die u hebt [eerder gedownloade](#use-the-marketplace-syndication-tool-to-download-marketplace-items) lokaal beschikbaar zijn voor uw Azure-Stack-omgeving moeten worden gemaakt.  

2. Importeren. VHD-bestanden naar Azure-Stack. Als u wilt importeren de installatiekopie van een virtuele machine (VM), hebt u de volgende informatie over de virtuele machine:
   - De *versie*, zoals beschreven in stap 7 van de voorgaande procedure.
   - De waarden voor de virtuele machines *publisher*, *bieden*, en *sku*. Als u deze waarden, wijzig de naam een kopie van de **.azpkg** bestand te wijzigen van de bestandsextensie voor **.zip**. U kunt vervolgens een teksteditor openen **DeploymentTemplates\CreateUiDefinition.json**. Zoek in het bestand .json de *imageReference* sectie waarin deze waarden voor de marketplace-item. Het volgende voorbeeld laat zien hoe deze informatie wordt weergegeven:

     ```json  
     "imageReference": {  
        "publisher": "MicrosoftWindowsServer",  
        "offer": "WindowsServer",  
        "sku": "2016-Datacenter-Server-Core"  
      }
     ```  

   De installatiekopie importeren naar Azure Stack met behulp van de **toevoegen AzsPlatformimage** cmdlet. Wanneer u deze cmdlet gebruikt, zorg ervoor dat u de *publisher*, *bieden*, en andere parameterwaarden met de waarden van de installatiekopie die u wilt importeren. U krijgt de *publisher*, *bieden*, en *sku* waarden van een installatiekopie van het tekstbestand dat samen met het bestand AZPKG gedownload en opgeslagen in de doellocatie . 

   In het volgende voorbeeldscript worden waarden voor de Windows Server 2016 Datacenter - Server Core-virtuele machine gebruikt. 

   ```PowerShell  
   Add-AzsPlatformimage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Windows-Server-2016-DatacenterCore-20171215-en.us-127GB.vhd" `
   ```
   **Over oplossingssjablonen:** sommige sjablonen kunnen een kleine 3 MB bevatten. VHD-bestand met de naam **fixed3.vhd**. U hoeft niet te importeren dat bestand in Azure-Stack. Fixed3.VHD.  Dit bestand is opgenomen in sommige oplossingssjablonen om te voldoen aan de publicatie van de vereisten voor Azure Marketplace.

   Bekijk de beschrijving van de sjablonen en downloaden en vervolgens importeren aanvullende vereisten zoals VHD's die nodig zijn voor het werken met de oplossingssjabloon.

3. De-beheerportal gebruiken voor het pakket voor de marketplace-item (de .azpkg-bestand) uploaden naar Azure Stack Blob-opslag. Uploaden van het pakket maakt deze beschikbaar voor Azure-Stack zodat u later kunt u het item publiceren de Stack Azure Marketplace.

   Upload moet u een opslagaccount met een openbaar toegankelijke container hebben (Zie de vereisten voor dit scenario)   
   1. Ga in de Stack van Azure-beheerportal naar **meer services** > **opslagaccounts**.  
   
   2. Selecteer een opslagaccount van uw abonnement en klik vervolgens onder **BLOB-SERVICE**, selecteer **Containers**.  
      ![BLOB-service](media/azure-stack-download-azure-marketplace-item/blob-service.png)  
   
   3. Selecteer de container die u wilt gebruiken en selecteer vervolgens **uploaden** openen de **blob uploaden** deelvenster.  
      ![Container](media/azure-stack-download-azure-marketplace-item/container.png)  
   
   4. Blader in het deelvenster van de blob uploaden naar de bestanden die u wilt laden in de opslag en selecteer vervolgens **uploaden**.  
      ![upload](media/azure-stack-download-azure-marketplace-item/upload.png)  

   5. Bestanden die u uploadt worden weergegeven in het deelvenster van de container. Selecteer een bestand en kopieer vervolgens de URL van de **Blob-eigenschappen** deelvenster. U gebruikt deze URL in de volgende stap bij het importeren van de marketplace-item naar Azure-Stack.  In de volgende afbeelding de container is *test-blob-opslag* en het bestand is *Microsoft.WindowsServer2016DatacenterServerCore ARM.1.0.801.azpkg*.  Het bestand dat de URL is *https://testblobstorage1.blob.local.azurestack.external/blob-test-storage/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  
      ![BLOB-eigenschappen](media/azure-stack-download-azure-marketplace-item/blob-storage.png)  

4.  PowerShell gebruiken voor het publiceren van de marketplace-item naar de Stack van Azure met behulp van de **toevoegen AzsGalleryItem** cmdlet. Bijvoorbeeld:  
    ```PowerShell  
    Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg" `
     –Verbose
    ```
5. Nadat u een galerij-item publiceert, kunt u het weergeven van **meer services** > **Marketplace**.  Als uw download een oplossingssjabloon is, zorg er dan voor dat u eventuele afhankelijke VHD-installatiekopie voor deze oplossingssjabloon toevoegen.  
  ![Weergave marketplace](media/azure-stack-download-azure-marketplace-item/view-marketplace.png)  

> [!NOTE]
> U kunt nu uitbreidingen van de virtuele Machine met de versie van Azure Stack PowerShell 1.3.0 toevoegen.

Bijvoorbeeld:

````PowerShell
Add-AzsVMExtension -Publisher "Microsoft" -Type "MicroExtension" -Version "0.1.0" -ComputeRole "IaaS" -SourceBlob "https://github.com/Microsoft/PowerShell-DSC-for-Linux/archive/v1.1.1-294.zip" -SupportMultipleExtensions -VmOsType "Linux"
````

## <a name="next-steps"></a>Volgende stappen
[Maken en publiceren van een Marketplace-item](azure-stack-create-and-publish-marketplace-item.md)
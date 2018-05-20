---
title: Downloaden van marketplace-items van Azure | Microsoft Docs
description: Ik kan de marketplace-items van Azure downloaden aan mijn Azure-Stack-implementatie.
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
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 2e92dc96a69400f689e49b70d1b855c955084362
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Marketplace-items van Azure naar Azure Stack downloaden

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*


Als u welke inhoud besluit wilt opnemen in uw Azure-Stack marketplace, moet u rekening houden met de inhoud die beschikbaar zijn vanuit Azure marketplace. U kunt downloaden van een geselecteerde lijst met Azure marketplace-items die vooraf geteste zijn worden uitgevoerd op Azure-Stack. Nieuwe items worden regelmatig toegevoegd aan deze lijst, zorg er dus om te controleren op nieuwe inhoud weer.

## <a name="download-marketplace-items-in-a-connected-scenario-with-internet-connectivity"></a>Marketplace-items in een scenario met verbonden (met een internetverbinding) downloaden

1. Als u wilt downloaden marketplace-items, moet u eerst [Azure Stack registreren bij Azure](azure-stack-register.md).
2. Aanmelden bij de Azure-Stack-beheerdersportal (voor ASDK, gebruik https://portal.local.azurestack.external).
3. Sommige marketplace-items kunnen oplopen. Controleer of er voldoende schijfruimte op het systeem door te klikken op **Resourceproviders** > **opslag**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

4. Klik op **meer Services** > **Marketplace Management**.

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Klik op **toevoegen uit Azure** voor een overzicht van de items beschikbaar voor downloaden. U kunt op elk item in de lijst weergeven van de beschrijving en downloadgrootte klikken.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Selecteer het item dat u wilt dat in de lijst en klik vervolgens op **downloaden**. De VM-afbeelding voor het item dat u hebt geselecteerd dat begint met het downloaden. Downloadtijden variëren.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. Nadat het downloaden is voltooid, kunt u uw nieuwe marketplace-item als een Azure-Stack operator of een gebruiker kunt implementeren. Klik op **+ nieuw**zoeken in de categorieën voor het nieuwe marketplace-item en selecteer vervolgens het item.
7. Klik op **maken** om de ervaring maken voor de zojuist gedownloade item te openen. Volg de stapsgewijze instructies voor het implementeren van uw item.

## <a name="download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Marketplace-items in een niet-verbonden of een gedeeltelijk verbonden scenario downloaden (met beperkte verbinding met internet)

Wanneer u Azure-Stack implementeert in een niet-verbonden modus (zonder een verbinding met internet), kunt u items voor de marketplace niet downloaden met behulp van de Stack van Azure-portal. U kunt echter de marketplace syndication-hulpprogramma gebruiken voor het downloaden van de marketplace-items voor een machine die verbinding heeft met internet en deze vervolgens overbrengen naar uw Azure-Stack-omgeving.

### <a name="prerequisites"></a>Vereisten
Voordat u de marketplace syndication-hulpprogramma gebruiken kunt, zorg ervoor dat u hebt [Azure Stack geregistreerd bij uw Azure-abonnement](azure-stack-register.md).  

Gebruik de volgende stappen voor het downloaden van de vereiste marketplace-items van de computer die verbinding heeft met internet:

1. Open een PowerShell-console als beheerder en [Azure Stack specifieke PowerShell-modules installeren](azure-stack-powershell-install.md). Zorg ervoor dat u installeert **PowerShell-Module voor Azure Stack versie 1.2.11 of hoger**.  

2. De Azure-account dat u hebt gebruikt voor het registreren van de Azure-Stack toevoegen. Uitvoeren als u wilt toevoegen op het account, de **Add-AzureRmAccount** cmdlet zonder parameters. U wordt gevraagd de referenties van uw Azure-account in te voeren en wellicht 2-factor-verificatie op basis van de configuratie van uw account te gebruiken.  

3. Als u meerdere abonnementen hebt, voer de volgende opdracht om te selecteren die u hebt gebruikt voor registratie:  

   ```powershell
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Download de nieuwste versie van marketplace syndication-hulpprogramma met behulp van het volgende script:  

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
   cd \AzureStack-Tools-master

   ```

5. De syndicatie-module importeren en het hulpprogramma starten met de volgende opdrachten:  

   ```powershell
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination "<Destination folder path>" `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Wanneer het hulpprogramma wordt uitgevoerd, wordt u gevraagd de referenties van uw Azure-account in te voeren. Aanmelden bij de Azure-account dat u hebt gebruikt voor het registreren van de Azure-Stack. Nadat de aanmelding is geslaagd, ziet u het volgende scherm met de lijst met beschikbare marketplace-items.  

   ![Azure Marketplace-items pop](./media/azure-stack-download-azure-marketplace-item/image05.png)

7. Selecteer de installatiekopie die u wilt downloaden en noteer de versie van de installatiekopie. U kunt meerdere installatiekopieën selecteren door de Ctrl-toets ingedrukt te houden. U gebruikt de afbeeldingsversie voor het importeren van de afbeelding in de volgende sectie.  Klik vervolgens op **Ok**, en accepteer de juridische voorwaarden door te klikken op **Ja**. U kunt ook de lijst met afbeeldingen filteren met behulp van de **criteria toevoegen** optie. 

   Het downloaden van het duurt even afhankelijk van de grootte van de afbeelding. Één keer downloaden van de afbeeldingen is beschikbaar in het doelpad die u eerder hebt opgegeven. De download bevat een VHD-bestand (voor virtuele machines) of een. ZIP-bestand (voor uitbreidingen van de virtuele machine) en een galerij-item in de notatie Azpkg.

### <a name="import-the-image-and-publish-it-to-azure-stack-marketplace"></a>De installatiekopie van het importeren en deze publiceren naar de Stack Azure marketplace
Er zijn drie verschillende typen van items in de marketplace: virtuele Machines, virtuele Machine uitbreidingen en Oplossingssjablonen. Oplossingssjablonen worden hierna besproken.
> [!NOTE]
> Uitbreidingen van de virtuele Machine kan niet worden toegevoegd aan Azure-Stack op dit moment.

1. Nadat u de installatiekopie en galerij downloaden, ze en de inhoud opslaan in de map AzureStack-hulpprogramma's-master om een verwisselbaar station en kopieer het naar de Stack van Azure-omgeving (u kunt deze kopiëren lokaal naar elke locatie, zoals: 'C:\MarketplaceImages').     

2. Voordat u de installatiekopie importeert, moet u verbinden met de Azure-Stack-operator omgeving met behulp van de stappen in [Azure Stack-operator PowerShell-omgeving configureren](azure-stack-powershell-configure-admin.md).  

3. Als uw download een klein 3MB VHD-bestand met de naam fixed3.vhd opgenomen, is het een oplossingssjabloon. Dit bestand is niet nodig; verder met stap 5. Zorg ervoor dat u alle afhankelijke items downloaden, zoals aangegeven in de beschrijving voor het downloaden.

4. De installatiekopie importeren naar Azure Stack met de cmdlet Add-AzsVMImage. Wanneer u deze cmdlet gebruikt, zorg ervoor dat u de *publisher*, *bieden*, en andere parameterwaarden met de waarden van de installatiekopie die u wilt importeren. U krijgt de *publisher*, *bieden*, en *sku* waarden van de installatiekopie van het object imageReference van het Azpkg-bestand dat u eerder hebt gedownload en de  *versie* waarde uit stap 6 in de vorige sectie.

Als u de imageReference zoekt, moet u de naam van de AZPKG-bestand met de. ZIP-extensie, pakt u het naar een tijdelijke locatie en open het bestand DeploymentTemplates\CreateUiDefinition.json met een teksteditor. Deze sectie vinden:

   ```json
   "imageReference": {
      "publisher": "MicrosoftWindowsServer",
      "offer": "WindowsServer",
      "sku": "2016-Datacenter-Server-Core"
    }
   ```

   Vervang de parameterwaarden en voer de volgende opdracht:

   ```powershell
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   Add-AzsVMImage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Windows-Server-2016-DatacenterCore-20171215-en.us-127GB.vhd" `
    -CreateGalleryItem $False `
    -Location Local
   ```

5. De portal gebruiken voor het uploaden van uw Marketplace-item (. Azpkg) naar Azure Stack Blob-opslag. U kunt uploaden naar de lokale opslag van de Azure-Stack of uploaden naar Azure Storage. (Dit is een tijdelijke locatie voor het pakket.) Zorg ervoor dat de blob openbaar toegankelijk is en noteer de URI.  

6. Het marketplace-item publiceren naar Azure Stack met behulp van de **toevoegen AzsGalleryItem**. Bijvoorbeeld:

   ```powershell
   Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.2.azpkg" `
     –Verbose
   ```

7. Nadat het galerie-item wordt gepubliceerd, kunt u het weergeven van de **nieuw** > **Marketplace** deelvenster. Als uw download een oplossingssjabloon is, zorg er dan voor dat u de afhankelijke VHD-installatiekopie ook gedownload.

   ![Marketplace](./media/azure-stack-download-azure-marketplace-item/image06.png)

## <a name="next-steps"></a>Volgende stappen

[Maken en publiceren van een Marketplace-item](azure-stack-create-and-publish-marketplace-item.md)

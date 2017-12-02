---
title: Downloaden van marketplace-items van Azure | Microsoft Docs
description: Ik kan de marketplace-items van Azure downloaden aan mijn Azure-Stack-implementatie.
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/30/2017
ms.author: erikje
ms.openlocfilehash: 33b7be4a85723ab03e4c656a8dd28632ad854e29
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Marketplace-items van Azure naar Azure Stack downloaden

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als u welke inhoud besluit wilt opnemen in uw Azure-Stack marketplace, moet u rekening houden met de inhoud die beschikbaar zijn vanuit Azure marketplace. U kunt downloaden van een geselecteerde lijst met Azure marketplace-items die vooraf geteste zijn worden uitgevoerd op Azure-Stack. Nieuwe items worden regelmatig toegevoegd aan deze lijst, zorg er dus om te controleren op nieuwe inhoud weer.

## <a name="download-marketplace-items-in-a-connected-scenario-with-internet-connectivity"></a>Marketplace-items in een scenario met verbonden (met een internetverbinding) downloaden

1. Als u wilt downloaden marketplace-items, moet u eerst [Azure Stack registreren bij Azure](azure-stack-register.md). 
2. Aanmelden bij de Azure-Stack-beheerdersportal (https://portal.local.azurestack.external).
3. Sommige marketplace-items kunnen oplopen. Controleer of er voldoende schijfruimte op het systeem door te klikken op **Resourceproviders** > **opslag**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

4. Klik op **meer Services** > **Marketplace Management**.

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Klik op **toevoegen uit Azure** voor een overzicht van de items beschikbaar voor downloaden. U kunt op elk item in de lijst weergeven van de beschrijving en downloadgrootte klikken.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Selecteer het item dat u wilt dat in de lijst en klik vervolgens op **downloaden**. Hiermee start u downloaden van de VM-installatiekopie voor het geselecteerde item. Downloadtijden variëren.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. Nadat het downloaden is voltooid, kunt u uw nieuwe marketplace-item als een Azure-Stack operator of een gebruiker kunt implementeren. Klik op **+ nieuw**zoeken in de categorieën voor het nieuwe marketplace-item en selecteer vervolgens het item.
7. Klik op **maken** om de ervaring maken voor de zojuist gedownloade item te openen. Volg de stapsgewijze instructies voor het implementeren van uw item.

## <a name="download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Marketplace-items in een niet-verbonden of een gedeeltelijk verbonden scenario downloaden (met beperkte verbinding met internet)

Wanneer u Azure-Stack implementeert in een niet-verbonden modus (zonder een verbinding met internet), kunt u items voor de marketplace niet downloaden met behulp van de Stack van Azure-portal. U kunt echter de marketplace syndication-hulpprogramma gebruiken voor het downloaden van de marketplace-items voor een machine die verbinding heeft met internet en deze vervolgens overbrengen naar uw Azure-Stack-omgeving. 

### <a name="prerequisites"></a>Vereisten
Voordat u de marketplace syndication-hulpprogramma gebruiken kunt, zorg ervoor dat u hebt [Azure Stack geregistreerd bij uw Azure-abonnement](azure-stack-register.md).  

Gebruik de volgende stappen voor het downloaden van de vereiste marketplace-items van de computer die verbinding heeft met internet:

1. Open een PowerShell-console als beheerder en [Azure Stack specifieke PowerShell-modules installeren](azure-stack-powershell-install.md). Zorg ervoor dat u installeert **PowerShell versie 1.2.11 of hoger**.  

2. De Azure-account dat u hebt gebruikt voor het registreren van de Azure-Stack toevoegen. U kunt dit doen de **Add-AzureRmAccount** cmdlet zonder parameters. U wordt gevraagd de referenties van uw Azure-account in te voeren en wellicht 2-factor-verificatie op basis van de configuratie van uw account te gebruiken.  

3. Als u meerdere abonnementen hebt, voer de volgende opdracht om te selecteren die u hebt gebruikt voor registratie:  

   ```powershell
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Download de nieuwste versie van marketplace syndication-hulpprogramma met behulp van het volgende script:  

   ```PowerShell
   # Download the tools archive.
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/vnext.zip `
     -OutFile vnext.zip

   # Expand the downloaded files.
   expand-archive vnext.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd \AzureStack-Tools-vnext

   ```

5. De syndicatie-module importeren en het hulpprogramma starten met de volgende opdrachten:  

   ```powershell
   Import-Module .\ Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination “<Destination folder path>” `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Wanneer het hulpprogramma wordt uitgevoerd, wordt u gevraagd de referenties van uw Azure-account in te voeren. Aanmelden bij de Azure-account dat u hebt gebruikt voor het registreren van de Azure-Stack. Na de aanmelding is geslaagd, ziet u het volgende scherm met de lijst met beschikbare marketplace-items.  

   ![Azure Marketplace-items pop](./media/azure-stack-download-azure-marketplace-item/image05.png)

7. Selecteer de installatiekopie die u wilt downloaden (u kunt meerdere installatiekopieën selecteren door de Ctrl-toets ingedrukt te houden) en noteer de versie van de installatiekopie, gebruikt u deze versie voor het importeren van de afbeelding in de volgende sectie > klikt u op **Ok** > accepteer de juridische bepalingen door te klikken op **Ja**. U kunt ook de lijst met afbeeldingen filteren met behulp van de **criteria toevoegen** optie. Het downloaden van het duurt even afhankelijk van de grootte van de afbeelding. Één keer downloaden van de afbeeldingen is beschikbaar in het doelpad die u eerder hebt opgegeven. De download bevat de VHD-bestand en galerie-items in de notatie Azpkg.  

### <a name="import-the-image-and-publish-it-to-azure-stack-marketplace"></a>De installatiekopie van het importeren en deze publiceren naar de Stack Azure marketplace

1. Nadat u de installatiekopie & galerij downloaden, ze en de inhoud opslaan in de map AzureStack-hulpprogramma's-vnext naar een verwisselbaar station en kopieer het naar de Stack van Azure-omgeving (u kunt kopiëren naar lokaal op een willekeurige locatie, zoals: 'C:\MarketplaceImages'.)   

2. Voordat u de installatiekopie importeert, moet u verbinden met de Azure-Stack-operator omgeving met behulp van de stappen in [Azure Stack-operator PowerShell-omgeving configureren](azure-stack-powershell-configure-admin.md).  

3. De installatiekopie importeren naar Azure Stack met de cmdlet Add-AzsVMImage. Wanneer u deze cmdlet gebruikt, zorg er dan voor dat de uitgever, aanbieding en de parameterwaarden van andere vervangen door de waarden van de installatiekopie die u wilt importeren. U kunt de 'publisher', 'aanbieding' en 'sku' waarden van een installatiekopie van het object imageReference van het Azpkg-bestand dat u eerder hebt gedownload en de waarde '-versie uit stap 6 ophalen in de vorige sectie.

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
    -Version "2017.09.25" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Microsoft.WindowsServer2016DatacenterServerCore-ARM-Eval.2017.09.25.vhd" `
    -CreateGalleryItem $False `
    -Location Local 
   ```

4. De portal gebruiken voor het uploaden van uw Marketplace-item (. Azpkg) naar Azure Stack Blob-opslag. U kunt uploaden naar de lokale opslag van de Azure-Stack of uploaden naar Azure Storage. (Dit is een tijdelijke locatie voor het pakket.) Zorg ervoor dat de blob openbaar toegankelijk is en noteer de URI.  

5. Het marketplace-item publiceren naar Azure Stack met behulp van de **toevoegen AzureRMGalleryItem**. Bijvoorbeeld:

   ```powershell
   Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.2.azpkg" `
     –Verbose
   ```

6. Nadat het galerie-item wordt gepubliceerd, kunt u het weergeven van de **nieuw** > **Marketplace** deelvenster.  

   ![Marketplace](./media/azure-stack-download-azure-marketplace-item/image06.png)

## <a name="next-steps"></a>Volgende stappen

[Maken en publiceren van een Marketplace-item](azure-stack-create-and-publish-marketplace-item.md)

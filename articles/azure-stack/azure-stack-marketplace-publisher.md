---
title: Gebruik de Marketplace-toolkit maken en publiceren van marketplace-items | Microsoft Docs
description: Meer informatie over het snel marketplace-items maken met de publicatie Toolkit
services: azure-stack
documentationcenter: 
author: HeathL17
manager: ByronR
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/14/2017
ms.author: helaw
ms.openlocfilehash: 5b2c04d2cbc06e1572dc2e40712f6cf9d886aa1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
#  <a name="add-marketplace-items-using-publishing-tool"></a>Het hulpprogramma voor publishing marketplace-items toevoegen
Toevoegen van de inhoud op de [Azure Stack Marketplace](azure-stack-marketplace.md) uw oplossingen beschikbaar stelt aan u en uw tenants voor implementatie.  De Toolkit Marketplace maakt Azure Marketplace-pakketten (.azpkg)-bestanden op basis van uw IaaS Azure Resource Manager-sjablonen of VM-extensies.  U kunt ook de Marketplace-werkset gebruiken voor het publiceren van .azpkg bestanden, gemaakt met het hulpprogramma of met behulp van [handmatige](azure-stack-create-and-publish-marketplace-item.md) stappen.  In dit onderwerp leidt u door het programma is gedownload, het maken van een marketplace-item op basis van een VM-sjabloon en dat het item te publiceren naar de Stack Azure Marketplace.     


## <a name="prerequisites"></a>Vereisten
 - U moet de toolkit uitvoeren op de Stack van Azure-host of hebt [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) connectiviteit van de computer waarop u het hulpprogramma uitvoert.

 - Download de [Azure Stack-snelstartsjablonen](https://github.com/Azure/AzureStack-QuickStart-Templates/archive/master.zip) en extraheren.

 - Download de [Azure-galerie verpakking hulpprogramma](http://aka.ms/azurestackmarketplaceitem) (AzureGalleryPackage.exe). 

 - Publiceren naar de marketplace vereist pictogrammen en een miniatuur-bestand.  U kunt uw eigen of sla de [voorbeeld](azure-stack-marketplace-publisher.md#support-files) bestanden lokaal voor dit voorbeeld.

## <a name="download-the-tool"></a>Het hulpprogramma voor downloaden
De Toolkit Marketplace kunnen worden [gedownload uit de Stack-hulpprogramma's van Azure-opslagplaats](azure-stack-powershell-download.md).


##  <a name="create-marketplace-items"></a>Marketplace-items maken
In deze sectie kunt u de Toolkit Marketplace gebruiken voor het maken van een pakket voor marketplace-item in .azpkg-indeling.  

### <a name="provide-marketplace-information-with-wizard"></a>Geef informatie op met de wizard marketplace
1. De Toolkit Marketplace uitvoeren vanaf een PowerShell-sessie:
```PowerShell
    .\MarketplaceToolkit.ps1
```

2. Klik op de **oplossing** tabblad.  Dit scherm accepteert informatie over uw marketplace-item. Geef informatie op over uw artikel als u wilt weergeven in de marketplace.  U kunt ook opgeven een [parameterbestand](azure-stack-marketplace-publisher.md#use-a-parameters-file) om in te vullen van het formulier.  
    
    ![schermopname van het eerste scherm Marketplace Toolkit](./media/azure-stack-marketplace-publisher/image7.png)
3. Klik op **Bladeren** en selecteer een installatiekopiebestand voor elk veld pictogram en schermafbeelding.  U kunt uw eigen pictogrammen of de pictogrammen voorbeeld in de [ondersteuningsbestanden](azure-stack-marketplace-publisher.md#support-files) sectie.
4. Eenmaal alle velden zijn ingevuld, selecteert u 'Preview oplossing' voor een voorbeeld van de oplossing in de Marketplace.  U kunt wijzigen en de tekst, afbeeldingen en schermafbeelding bewerken voordat u op **volgende**.  

### <a name="import-template-and-create-package"></a>Sjabloon importeert en pakket maken
In deze sectie, importeer de sjabloon en werken met invoer voor uw oplossing.

1.  Klik op **Bladeren** en selecteer de *azuredeploy.json* vanuit de map 101-eenvoudige-Windows-VM in de gedownloade sjablonen.

    ![Schermafbeelding van de tweede scherm Marketplace Toolkit](./media/azure-stack-marketplace-publisher/image8.png)
2.  De implementatiewizard is gevuld met een *Basic* stap en invoer items voor elke parameter die is opgegeven in de sjabloon.  U kunt aanvullende stappen toevoegen en invoer verplaatsen tussen stappen.  Een voorbeeld: u kunt 'front-configuratie"en" Back-End ' configuratiestappen voor uw oplossing.
3.  Geef het pad naar AzureGalleryPackager.exe.  
4.  Klik op **maken** de Marketplace-Toolkit uw oplossing worden verpakt in een bestand .azpkg.  Als u klaar is, wordt de wizard geeft het pad naar het oplossingsbestand van uw weer en krijgt u de mogelijkheid om door te gaan met het pakket publiceren naar Azure-Stack.


## <a name="publish-marketplace-items"></a>Publiceren van marketplace-items
In deze sectie kunt u het marketplace-item publiceren naar de Stack Azure Marketplace.

![schermopname van het eerste scherm Marketplace Toolkit](./media/azure-stack-marketplace-publisher/image9.png)

1.  De wizard vereist informatie voor het publiceren van uw oplossing:
    
    |Veld|Beschrijving|
    |-----|-----|
    | Naam van de Service-beheer | Service Administrator-account.  Voorbeeld:ServiceAdmin@mydomain.onmicrosoft.com |
    | Wachtwoord | Wachtwoord voor de Service Administrator-account. |
    | API-eindpunt | Azure Stack Azure Resource Manager-eindpunt.  Voorbeeld: management.local.azurestack.external |
2.  Klik op **publiceren** en het publishing logboek wordt weergegeven.
3.  U bent nu uw gepubliceerde artikel via de Stack van Azure-portal te implementeren.


## <a name="use-a-parameters-file"></a>Een parameterbestand gebruiken
U kunt ook een parameterbestand gebruiken voor het voltooien van de marketplace-item-informatie.  

De Marketplace-Toolkit bevat een *solution.parameters.ps1* kunt u uw eigen parameterbestand te maken.


## <a name="support-files"></a>Ondersteuning voor bestanden
| Beschrijving | Voorbeeld |
| ----- | ----- |
| 40 x 40 PNG-pictogram | ![](./media/azure-stack-marketplace-publisher/image1.png) |
| 90 x 90 PNG-pictogram | ![](./media/azure-stack-marketplace-publisher/image2.png) |
| 115 x 115 PNG-pictogram | ![](./media/azure-stack-marketplace-publisher/image3.png) |
| 255 x 115 PNG-pictogram | ![](./media/azure-stack-marketplace-publisher/image4.png) |
| 533 x 324 PNG miniatuur | ![](./media/azure-stack-marketplace-publisher/image5.png) |



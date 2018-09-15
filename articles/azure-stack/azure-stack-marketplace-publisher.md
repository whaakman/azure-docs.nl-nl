---
title: Gebruik de Marketplace-toolkit om te maken en publiceren van marketplace-items | Microsoft Docs
description: Meer informatie over het snel marketplace-items maken met de publicatie Toolkit
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/14/2017
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 0ade78dd992e8d1d2eda2cf27d44e52c4030563f
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630918"
---
#  <a name="add-marketplace-items-using-publishing-tool"></a>Met behulp van het hulpprogramma voor publiceren marketplace-items toevoegen
Toevoegen van uw inhoud naar de [Azure Stack Marketplace](azure-stack-marketplace.md) maakt uw oplossingen beschikbaar voor u en uw tenants voor implementatie.  De Marketplace-Toolkit maakt Azure Marketplace-pakketten (.azpkg)-bestanden op basis van uw IaaS Azure Resource Manager-sjablonen of VM-extensies.  U kunt ook de Marketplace-Toolkit gebruiken voor het publiceren van .azpkg bestanden, gemaakt met het hulpprogramma of met behulp van [handmatige](azure-stack-create-and-publish-marketplace-item.md) stappen.  In dit onderwerp leidt u door het hulpprogramma te downloaden, het maken van een marketplace-item op basis van een VM-sjabloon en vervolgens dat item publiceren naar de Azure Stack Marketplace.     


## <a name="prerequisites"></a>Vereisten
 - U moet de toolkit worden uitgevoerd op de host van de Azure Stack of [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) connectiviteit van de computer waarop u het hulpprogramma uitvoert.

 - Download de [snelstartsjablonen van Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/archive/master.zip) en uit te pakken.

 - Download de [verpakking voor Azure-galerie hulpprogramma](http://aka.ms/azurestackmarketplaceitem) (AzureGalleryPackage.exe). 

 - Publiceren naar de marketplace zijn pictogrammen en een Miniatuurbestand vereist.  U kunt uw eigen of sla de [voorbeeld](azure-stack-marketplace-publisher.md#support-files) bestanden lokaal voor dit voorbeeld.

## <a name="download-the-tool"></a>Het hulpprogramma downloaden
De Toolkit Marketplace kunnen worden [gedownload vanuit de opslagplaats voor Azure Stack-hulpprogramma's](azure-stack-powershell-download.md).


##  <a name="create-marketplace-items"></a>Marketplace-items maken
In deze sectie gebruikt u de Marketplace-Toolkit een marketplace-item-pakket maken in de indeling .azpkg.  

### <a name="provide-marketplace-information-with-wizard"></a>Geef de marketplace-gegevens met de wizard
1. De Marketplace-Toolkit uitvoeren vanuit een PowerShell-sessie:
```PowerShell
    .\MarketplaceToolkit.ps1
```

2. Klik op de **oplossing** tabblad.  Dit scherm accepteert informatie over uw marketplace-item. Informatie over uw item invoeren als u wilt dat deze wordt weergegeven in de marketplace.  U kunt ook opgeven een [parameterbestand](azure-stack-marketplace-publisher.md#use-a-parameters-file) om in te vullen van het formulier.  
    
    ![schermopname van het eerste scherm van Marketplace-Toolkit](./media/azure-stack-marketplace-publisher/image7.png)
3. Klik op **Bladeren** en selecteert u een afbeeldingsbestand voor elk veld pictogram en de schermafbeelding.  U kunt uw eigen pictogrammen of de voorbeeld-pictogrammen in de [ondersteuningsbestanden](azure-stack-marketplace-publisher.md#support-files) sectie.
4. Zodra alle velden zijn ingevuld, selecteert u 'Preview oplossing' voor een Preview-versie van de oplossing in de Marketplace.  U kunt wijzigen en de tekst, afbeeldingen en schermafbeelding bewerken voordat u op **volgende**.  

### <a name="import-template-and-create-package"></a>Sjabloon importeren en -pakket maken
In deze sectie maakt u de sjabloon importeren en werkt met invoer voor uw oplossing.

1.  Klik op **Bladeren** en selecteer de *azuredeploy.json* vanuit de map 101-eenvoudig-Windows-VM in de gedownloade sjablonen.

    ![schermopname van het tweede scherm Marketplace Toolkit](./media/azure-stack-marketplace-publisher/image8.png)
2.  De implementatiewizard is gevuld met een *Basic* stap en invoer-items voor elke parameter die is opgegeven in de sjabloon.  U kunt aanvullende stappen toevoegen en invoer verplaatsen tussen fasen.  Als u bijvoorbeeld kunt u 'Front-End-configuratie' en 'Back-End-configuratie' stappen voor uw oplossing.
3.  Geef het pad naar AzureGalleryPackager.exe.  
4.  Klik op **maken** en de Toolkit Marketplace uw oplossing in een bestand .azpkg pakketten.  Als u klaar bent, wordt de wizard geeft het pad naar het oplossingsbestand weer en geeft u de optie om door te gaan met het publiceren van het pakket naar Azure Stack.


## <a name="publish-marketplace-items"></a>Marketplace-items publiceren
In deze sectie maakt publiceren u de marketplace-item naar uw Azure Stack Marketplace.

![schermopname van het eerste scherm van Marketplace-Toolkit](./media/azure-stack-marketplace-publisher/image9.png)

1.  De wizard vereist informatie voor het publiceren van uw oplossing:
    
    |Veld|Beschrijving|
    |-----|-----|
    | Naam van de Service-beheerder | Service Administrator-account.  Voorbeeld:  ServiceAdmin@mydomain.onmicrosoft.com |
    | Wachtwoord | Wachtwoord voor de Service Administrator-account. |
    | API-eindpunt | Azure Stack Azure Resource Manager-eindpunt.  Voorbeeld: management.local.azurestack.external |
2.  Klik op **publiceren** en het publiceren logboek wordt weergegeven.
3.  U bent nu uw gepubliceerde artikel via de Azure Stack-portal implementeren.


## <a name="use-a-parameters-file"></a>Gebruik een parameterbestand
U kunt ook een parameterbestand gebruiken om uit te voeren van de gegevens van marketplace-item.  

De Marketplace-Toolkit bevat een *solution.parameters.ps1* u kunt gebruiken om uw eigen parameterbestand te maken.


## <a name="support-files"></a>Ondersteuningsbestanden
| Beschrijving | Voorbeeld |
| ----- | ----- |
| 40, 40 PNG-pictogram | ![](./media/azure-stack-marketplace-publisher/image1.png) |
| 90 x 90 PNG-pictogram | ![](./media/azure-stack-marketplace-publisher/image2.png) |
| 115 x 115 PNG-pictogram | ![](./media/azure-stack-marketplace-publisher/image3.png) |
| 255 x 115 PNG-pictogram | ![](./media/azure-stack-marketplace-publisher/image4.png) |
| 533 x 324 .png miniatuur | ![](./media/azure-stack-marketplace-publisher/image5.png) |



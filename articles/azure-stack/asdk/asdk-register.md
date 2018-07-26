---
title: De ASDK registreren bij Azure | Microsoft Docs
description: Beschrijft hoe u Azure Stack registreren bij Azure, marketplace-syndicatie en rapportage over het gebruik.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 19fb5d3cb793b6e1e8e715c41edf8cde5746278b
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257919"
---
# <a name="azure-stack-registration"></a>Azure Stack-registratie
U kunt de installatie van de Azure Stack Development Kit (ASDK) registreren met Azure marketplace-items van Azure downloaden en het instellen van rapportage terug naar Microsoft commercegegevens. Registratie is vereist voor ondersteuning van de volledige functionaliteit van Azure Stack, met inbegrip van marketplace-syndicatie. Registratie wordt aanbevolen omdat hiermee u voor het testen van belangrijke Azure Stack-functionaliteit, zoals marketplace-syndicatie en rapportage over het gebruik kunt. Nadat u Azure Stack hebt geregistreerd, wordt gebruik gerapporteerd aan Azure commerce. U kunt het zien onder het abonnement dat u voor de registratie gebruikt. Echter ASDK gebruikers niet in rekening gebracht voor het gebruik die ze rapporteren.

Als u uw ASDK niet registreert, ziet u mogelijk een **activering vereist** waarschuwing die aangeeft voor het registreren van uw Azure Stack Development Kit. Dit gedrag is verwacht.

## <a name="prerequisites"></a>Vereisten
Voordat u deze instructies voor het registreren van de ASDK met Azure, zorg ervoor dat u hebt de PowerShell voor Azure Stack geïnstalleerd en de hulpprogramma's voor Azure Stack downloaden, zoals beschreven in de [post-implementatieconfiguratie](asdk-post-deploy.md) artikel.

Bovendien de taalmodus PowerShell moet zijn ingesteld op **FullLanguageMode** op de computer gebruikt voor het registreren van de ASDK met Azure. Om te controleren dat de huidige taalmodus is ingesteld op full, open een verhoogde PowerShell-venster en voer de volgende PowerShell-opdrachten:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Zorg ervoor dat de uitvoer geeft als resultaat **FullLanguageMode**. Als een andere taalmodus wordt geretourneerd, registratie moet worden uitgevoerd op een andere computer of de taalmodus worden ingesteld moet op **FullLanguageMode** voordat u doorgaat.

## <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure
Volg deze stappen voor het registreren van de ASDK met Azure.

> [!NOTE]
> Al deze stappen moeten worden uitgevoerd vanaf een computer die toegang tot het eindpunt van de bevoegdheden heeft. Voor de ASDK is dat de hostcomputer development kit.

1. Open een PowerShell-console als beheerder.  

2. Voer de volgende PowerShell-opdrachten voor het registreren van uw installatie ASDK met Azure. U moet zich aanmelden bij uw Azure-abonnement zowel de lokale ASDK-installatie. Als u een Azure-abonnement hebt hebt, u kunt [maken van een gratis Azure-account hier](https://azure.microsoft.com/free/?b=17.06). Registreren van Azure Stack maakt geen kosten op uw Azure-abonnement.

    Als u het script voor de registratie op meer dan één exemplaar van Azure Stack met behulp van de dezelfde Azure-abonnements-ID, een unieke naam voor de registratie instellen tijdens het uitvoeren van de **Set AzsRegistration** cmdlet. De **RegistrationName** parameter heeft een standaardwaarde van **AzureStackRegistration**. Echter, als u dezelfde naam op meer dan één exemplaar van Azure Stack gebruikt, het script mislukken.

  ```PowerShell  
  # Add the Azure cloud subscription environment name. Supported environment names are AzureCloud or, if using a China Azure Subscription, AzureChinaCloud.
  Add-AzureRmAccount -EnvironmentName "AzureCloud"

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  #Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  #Register Azure Stack
  $AzureContext = Get-AzureRmContext
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  $RegistrationName = "<unique-registration-name>"
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint AzS-ERCS01 `
      -BillingModel Development
      -RegistrationName $RegistrationName
  ```
3. Wanneer het script is voltooid, ziet u dit bericht: **uw omgeving is nu geregistreerd en worden geactiveerd met behulp van de opgegeven parameters.**

    ![Uw omgeving is nu geregistreerd](media/asdk-register/1.PNG)

## <a name="verify-the-registration-was-successful"></a>Controleer of dat de registratie is geslaagd
Volg deze stappen om te controleren of de ASDK registratie bij Azure geslaagd is.

1. Aanmelden bij de [Azure Stack-beheerportal](https://adminportal.local.azurestack.external).

2. Klik op **Marketplace Management** > **toevoegen vanuit Azure**.

    ![](media/asdk-register/2.PNG)

3. Als u een lijst met items die beschikbaar zijn in Azure ziet, wordt de activering is voltooid.

    ![](media/asdk-register/3.PNG)

## <a name="next-steps"></a>Volgende stappen
[Een Azure Stack marketplace-item toevoegen](.\.\azure-stack-marketplace.md)

---
title: De ASDK registreren bij Azure | Microsoft Docs
description: Beschrijft hoe Azure-Stack registreren bij Azure marketplace syndication- en gebruiksrapportage inschakelen.
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
ms.date: 06/04/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: bc17045106b2656cdeb5f51037b3138aeac9ee0f
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757464"
---
# <a name="azure-stack-registration"></a>Azure Stack-registratie
U kunt de installatie van Azure Stack Development Kit (ASDK) registreren met Azure marketplace-items van Azure downloaden en voor het instellen van rapportage terug naar Microsoft commerce-gegevens. Registratie is vereist voor volledige functionaliteit van de Azure-Stack, met inbegrip van marketplace syndication ondersteuning. De registratie wordt aanbevolen omdat Hiermee kunt u belangrijke Azure Stack-functionaliteit, zoals marketplace syndication- en gebruiksrapportage testen. Nadat u Azure-Stack geregistreerd, wordt informatie over het gebruik gerapporteerd aan Azure commerce. U kunt dit zien onder het abonnement dat u voor registratie gebruikt. Echter, ASDK gebruikers worden niet in rekening gebracht voor enig gebruik die ze rapporteren.

Als u uw ASDK niet registreert, ziet u mogelijk een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.

## <a name="prerequisites"></a>Vereisten
Voordat u deze instructies de ASDK registreren bij Azure gebruikt, zorg ervoor dat u hebt geïnstalleerd, de Stack Azure PowerShell en gedownload van de Azure-Stack-hulpprogramma's zoals beschreven in de [na de implementatie configuratie](asdk-post-deploy.md) artikel.

Bovendien de PowerShell-taalmodus moet worden ingesteld op **FullLanguageMode** op de computer gebruikt voor het registreren van de ASDK met Azure. Controleren of de huidige taalmodus is ingesteld op full, opent u een PowerShell-venster met verhoogde bevoegdheid en voer de volgende PowerShell-opdrachten:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Zorg ervoor dat de uitvoer retourneert **FullLanguageMode**. Als een andere taalmodus wordt geretourneerd, registratie moet worden uitgevoerd op een andere computer of de taalmodus worden ingesteld moet op **FullLanguageMode** voordat u doorgaat.

## <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure
Volg deze stappen voor het registreren van de ASDK met Azure.

> [!NOTE]
> Alle deze stappen moeten worden uitgevoerd vanaf een computer die toegang tot de bevoegde eindpunt heeft. Voor de ASDK is dat de hostcomputer development kit.

1. Open een PowerShell-console als beheerder.  

2. Voer de volgende PowerShell-opdrachten uw installatie ASDK registreren bij Azure. U moet zich aanmelden bij uw Azure-abonnement zowel de lokale ASDK-installatie. Als u een Azure-abonnement hebt, maar u kunt [maken van een gratis Azure-account hier](https://azure.microsoft.com/free/?b=17.06). Azure-Stack registreren maakt geen kosten op uw Azure-abonnement.

  ```powershell
  # Add the Azure cloud subscription environment name. Supported environment names are AzureCloud or, if using a China Azure Subscription, AzureChinaCloud.
  Add-AzureRmAccount -EnvironmentName "AzureCloud"

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  #Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  #Register Azure Stack
  $AzureContext = Get-AzureRmContext
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint AzS-ERCS01 `
      -BillingModel Development
  ```
3. Wanneer het script is voltooid, ziet u dit bericht: **uw omgeving is nu geregistreerd en geactiveerd met behulp van de opgegeven parameters.**

    ![](media/asdk-register/1.PNG)

## <a name="verify-the-registration-was-successful"></a>Controleer of dat de registratie is geslaagd
Volg deze stappen om te controleren of de registratie ASDK met Azure geslaagd is.

1. Aanmelden bij de [Stack Azure-beheerportal](https://adminportal.local.azurestack.external).

2. Klik op **Marketplace Management** > **toevoegen vanuit Azure**.

    ![](media/asdk-register/2.PNG)

3. Als u een lijst met items die beschikbaar is via Azure ziet, is de activering is geslaagd.

    ![](media/asdk-register/3.PNG)

## <a name="next-steps"></a>Volgende stappen
[Een Azure-Stack marketplace-item toevoegen](asdk-marketplace-item.md)

---
title: De ASDK registreren bij Azure | Microsoft Docs
description: Beschrijft hoe Azure-Stack registreren bij Azure marketplace syndication- en gebruiksrapportage inschakelen.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 1d90be043affcab422f8e59e6bac1c76ec923d48
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure
U kunt de installatie van Azure Stack Development Kit (ASDK) registreren met Azure marketplace-items van Azure downloaden en voor het instellen van rapportage terug naar Microsoft commerce-gegevens. De registratie wordt aanbevolen omdat Hiermee kunt u belangrijke Azure Stack-functionaliteit, zoals marketplace syndication- en gebruiksrapportage testen. Nadat u Azure-Stack geregistreerd, wordt informatie over het gebruik gerapporteerd aan Azure commerce. U kunt dit zien onder het abonnement dat u voor registratie gebruikt. Echter, ASDK gebruikers worden niet in rekening gebracht voor enig gebruik die ze rapporteren.

Als u uw ASDK niet registreert, ziet u mogelijk een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.

## <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure 
Volg deze stappen voor het registreren van de ASDK met Azure.

> [!NOTE]
> Alle deze stappen moeten worden uitgevoerd vanaf een computer die toegang tot de bevoegde eindpunt heeft. Voor de ASDK is dat de hostcomputer development kit. 

Voordat u deze stappen de ASDK registreren bij Azure gebruikt, zorg ervoor dat u hebt geïnstalleerd, de Stack Azure PowerShell en gedownload van de Azure-Stack-hulpprogramma's zoals beschreven in de [na de implementatie configuratie](asdk-post-deploy.md) artikel. 

1. Open een PowerShell-console als beheerder.  

2. Voer de volgende PowerShell-opdrachten uw installatie ASDK registreren bij Azure (u moet zich aanmelden bij uw Azure-abonnement zowel de lokale installatie van de ASDK):

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
    Set-AzsRegistration `
        -PrivilegedEndpointCredential $CloudAdminCred `
        -PrivilegedEndpoint AzS-ERCS01 `
        -BillingModel Development

3. When the script completes, you should see this message: **Your environment is now registered and activated using the provided parameters.**

    ![](media/asdk-register/1.PNG) 

## Verify the registration was successful
Follow these steps to verify that the ASDK registration with Azure was successful.

1. Sign in to the [Azure Stack administration portal](https://adminportal.local.azurestack.external).

2. Click **Marketplace Management** > **Add from Azure**.

    ![](media/asdk-register/2.PNG) 

3. If you see a list of items available from Azure, your activation was successful.

    ![](media/asdk-register/3.PNG) 

## Next steps
[Add an Azure Stack marketplace item](asdk-marketplace-item.md)

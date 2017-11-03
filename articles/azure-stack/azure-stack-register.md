---
title: Azure Stack registreren | Microsoft Docs
description: Azure-Stack registreren bij uw Azure-abonnement.
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
ms.topic: article
ms.date: 10/27/2017
ms.author: erikje
ms.openlocfilehash: 24cde66a132ae2e1ba0eb9b1564915746e5ca448
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Azure Stack registreren bij uw Azure-abonnement

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt registreren [Azure Stack](azure-stack-poc.md) met Azure marketplace-items van Azure downloaden en voor het instellen van rapportage terug naar Microsoft commerce-gegevens. 

> [!NOTE]
>De registratie wordt aanbevolen omdat Hiermee kunt u belangrijke Azure Stack-functionaliteit, zoals marketplace syndication- en gebruiksrapportage testen. Nadat u Azure-Stack geregistreerd, wordt informatie over het gebruik gerapporteerd aan Azure commerce. U kunt dit zien onder het abonnement dat u voor registratie gebruikt. Azure-Stack Development Kit gebruikers worden niet in rekening gebracht voor enig gebruik die ze rapporteren.
>


## <a name="get-azure-subscription"></a>Azure-abonnement ophalen

Voordat u registreert Stack van Azure met Azure, moet u het volgende hebben:

- De abonnement-ID voor een Azure-abonnement. Als u de ID, zich aanmelden bij Azure, klikt u op **meer services** > **abonnementen**, klik op het abonnement dat u gebruiken wilt, en klikt u onder **Essentials** vindt u de **Abonnements-ID**. China, Duitsland en US government-cloudabonnementen zijn momenteel niet ondersteund.
- De gebruikersnaam en het wachtwoord voor een account dat is eigenaar van het abonnement (MSA/2FA accounts worden ondersteund).
- De Azure Active Directory voor het Azure-abonnement. U kunt deze map vinden in Azure door de muiswijzer op uw avatar in de rechterbovenhoek van de Azure portal. 
- [De Azure-Stack-resourceprovider geregistreerd](#register-azure-stack-resource-provider-in-azure).

Als u geen een Azure-abonnement dat aan deze vereisten voldoet, kunt u [maken van een gratis Azure-account hier](https://azure.microsoft.com/en-us/free/?b=17.06). Azure-Stack registreren maakt geen kosten op uw Azure-abonnement.



## <a name="register-azure-stack-resource-provider-in-azure"></a>Azure-Stack resourceprovider in Azure registreren
> [!NOTE] 
> Deze stap hoeft slechts eenmaal worden uitgevoerd in een Azure-Stack-omgeving.
>

1. Start Powershell ISE als beheerder.
2. Aanmelden bij de Azure-account is eigenaar van het Azure-abonnement met EnvironmentName - parameter is ingesteld op 'AzureCloud'.
3. Registreer de Azure-resourceprovider 'Microsoft.AzureStack'.

Voorbeeld: 
```Powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
```


## <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure

> [!NOTE]
>Alle deze stappen moeten worden uitgevoerd op de hostcomputer.
>

1. [Installeer PowerShell voor Azure Stack](azure-stack-powershell-install.md). 
2. Kopieer de [RegisterWithAzure.psm1 script](https://go.microsoft.com/fwlink/?linkid=842959) naar een map (zoals C:\Temp).
3. Start PowerShell ISE als beheerder en importeer de module RegisterWithAzure.    
4. Voer de module toevoegen AzsRegistration uit het script RegisterWithAzure.psm1. De volgende tijdelijke aanduidingen vervangt: 
    - *YourCloudAdminCredential* is een PowerShell-object dat de referenties van het lokale domein voor de domain\cloudadmin bevat (dit is voor de development kit azurestack\cloudadmin).
    - *YourAzureSubscriptionID* is de ID van de Azure-abonnement dat u gebruiken wilt voor het registreren van de Azure-Stack.
    - *YourAzureDirectoryTenantName* is de naam van de Azure-tenant-map die is gekoppeld aan uw Azure-abonnement. De registratie-resource wordt in deze directory-tenant gemaakt. 
    - *YourPrivilegedEndpoint* is de naam van de [bevoegde eindpunt](azure-stack-privileged-endpoint.md).

    ```powershell
    Add-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -AzureDirectoryTenantName $YourAzureDirectoryTenantName  -AzureSubscriptionId $YourAzureSubscriptionId -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Development 
    ```
5. Geef de referenties van uw Azure-abonnement in het aanmeldingsvenster pop.

## <a name="verify-the-registration"></a>Controleer of de registratie

1. Aanmelden bij de beheerdersportal (https://adminportal.local.azurestack.external).
2. Klik op **meer Services** > **Marketplace Management** > **toevoegen vanuit Azure**.
3. Als u ziet een lijst met items die beschikbaar is via Azure (zoals WordPress), wordt de activering is geslaagd.

> [!NOTE]
> Nadat de registratie voltooid is, wordt de actieve waarschuwing voor het registreren van niet langer weergegeven.

## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)


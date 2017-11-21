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
ms.date: 11/15/2017
ms.author: erikje
ms.openlocfilehash: 977630741b8424c4c6bd5f5d492e33b9981b9cb5
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
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

Als u geen een Azure-abonnement dat aan deze vereisten voldoet, kunt u [maken van een gratis Azure-account hier](https://azure.microsoft.com/en-us/free/?b=17.06). Azure-Stack registreren maakt geen kosten op uw Azure-abonnement.



## <a name="register-azure-stack-resource-provider-in-azure"></a>Azure-Stack resourceprovider in Azure registreren
> [!NOTE] 
> Deze stap moet slechts één keer worden voltooid in een Azure-Stack-omgeving.
>

1. Een Powershell-sessie start als beheerder.
2. Aanmelden bij de Azure-account is eigenaar van het Azure-abonnement (u kunt de cmdlet Login-AzureRmAccount aanmelden en wanneer u zich aanmeldt, zorg ervoor dat de parameter - EnvironmentName 'AzureCloud' instellen).
3. Registreer de Azure-resourceprovider 'Microsoft.AzureStack'.

**Voorbeeld:** 
```Powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
```

## <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure

> [!NOTE]
> Alle deze stappen moeten worden uitgevoerd vanaf een machine die toegang tot de bevoegde eindpunt heeft. Voor Azure Stack Development Kit zou worden de hostcomputer. Als u een geïntegreerd systeem, moet u contact op met uw Azure-Stack-operator.
>

1. Open een PowerShell-console als beheerder en [Installeer PowerShell voor Azure-Stack](azure-stack-powershell-install.md).  

2. De Azure-account die u gebruiken wilt voor het registreren van de Azure-Stack toevoegen. U kunt dit doen de `Add-AzureRmAccount` cmdlet zonder parameters. U wordt gevraagd de referenties van uw Azure-account in te voeren en wellicht 2-factor-verificatie op basis van de configuratie van uw account te gebruiken.  

3. Als u meerdere abonnementen hebt, voert u de volgende opdracht om te selecteren die dat u wilt gebruiken:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Verwijder alle bestaande versies van de Powershell-modules die met de registratie overeenkomen en [download de nieuwste versie van deze vanuit GitHub](azure-stack-powershell-download.md).  

5. In de map 'AzureStack-hulpprogramma's-master' dat is gemaakt in de vorige stap, navigeer naar de map "Registratie" en importeer de module '.\RegisterWithAzure.psm1':  

   ```powershell 
   Import-Module .\RegisterWithAzure.psm1 
   ```

6. Voer het volgende script in dezelfde PowerShell-sessie. Wanneer u wordt gevraagd om referenties, opgeven `azurestack\cloudadmin` als de gebruiker en het wachtwoord hetzelfde is als wat u hebt gebruikt voor de lokale beheerder tijdens de implementatie.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Add-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -AzureSubscriptionId $AzureContext.Subscription.Id `
       -AzureDirectoryTenantName $AzureContext.Tenant.TenantId `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development 
   ```

   | Parameter | Beschrijving |
   | -------- | ------------- |
   | CloudAdminCredential | De cloud-domein-referenties die worden gebruikt om [toegang tot de bevoegde eindpunt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). De gebruikersnaam is in de notatie '\<Azure Stack domein\>\cloudadmin '. Voor development kit, is de gebruikersnaam ingesteld op 'azurestack\cloudadmin'. Als u een geïntegreerd systeem, moet u contact op met uw Azure-Stack-operator als deze waarde wilt ophalen.|
   | AzureSubscriptionId | De Azure-abonnement dat u met Azure-Stack registreren.|
   | AzureDirectoryTenantName | De naam van de Azure-tenant-map die is gekoppeld aan uw Azure-abonnement. De registratie-resource wordt in deze directory-tenant gemaakt. |
   | PrivilegedEndpoint | Een vooraf geconfigureerde externe PowerShell-console waarmee u mogelijkheden, zoals logboekgegevens verzameld en andere post implementatietaken. Voor development kit wordt het bevoegde eindpunt gehost op de 'AzS ERCS01' virtuele machine. Als u een geïntegreerd systeem, moet u contact op met uw Azure-Stack-operator als deze waarde wilt ophalen. Raadpleeg voor meer informatie de [met behulp van het eindpunt van de bevoegde](azure-stack-privileged-endpoint.md) onderwerp.|
   | BillingModel | Het facturering model die gebruikmaakt van uw abonnement. Toegestane waarden voor deze parameter zijn 'Capaciteit', 'PayAsYouUse' en 'Ontwikkeling'. Voor development kit, wordt deze waarde ingesteld op 'Ontwikkeling'. Als u een geïntegreerd systeem, moet u contact op met uw Azure-Stack-operator als deze waarde wilt ophalen. |

7. Als het script is voltooid, ziet u een bericht ' activeren Azure Stack (deze stap kan maximaal 10 minuten duren). ' 

## <a name="verify-the-registration"></a>Controleer of de registratie

1. Aanmelden bij de beheerdersportal (https://adminportal.local.azurestack.external).
2. Klik op **meer Services** > **Marketplace Management** > **toevoegen vanuit Azure**.
3. Als u ziet een lijst met items die beschikbaar is via Azure (zoals WordPress), wordt de activering is geslaagd.

> [!NOTE]
> Nadat de registratie voltooid is, wordt de actieve waarschuwing voor het registreren van niet langer weergegeven.

## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)


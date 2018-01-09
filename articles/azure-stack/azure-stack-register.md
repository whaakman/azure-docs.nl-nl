---
title: Azure Stack registreren | Microsoft Docs
description: Azure-Stack registreren bij uw Azure-abonnement.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/8/2018
ms.author: jeffgilb
ms.openlocfilehash: b58b3fc538d2237c12a860d268d550c4223155ba
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Azure Stack registreren bij uw Azure-abonnement

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt registreren [Azure Stack](azure-stack-poc.md) met Azure marketplace-items van Azure downloaden en voor het instellen van rapportage terug naar Microsoft commerce-gegevens.

> [!NOTE]
>De registratie wordt aanbevolen omdat Hiermee kunt u belangrijke Azure Stack-functionaliteit, zoals marketplace syndication- en gebruiksrapportage testen. Nadat u Azure-Stack geregistreerd, wordt informatie over het gebruik gerapporteerd aan Azure commerce. U kunt dit zien onder het abonnement dat u voor registratie gebruikt. Azure-Stack Development Kit gebruikers worden niet in rekening gebracht voor enig gebruik die ze rapporteren.


## <a name="get-azure-subscription"></a>Azure-abonnement ophalen

Voordat u registreert Stack van Azure met Azure, moet u het volgende hebben:

- De abonnement-ID voor een Azure-abonnement. Als u de ID, zich aanmelden bij Azure, klikt u op **meer services** > **abonnementen**, klik op het abonnement dat u gebruiken wilt, en klikt u onder **Essentials** vindt u de **Abonnements-ID**. China, Duitsland en US government-cloudabonnementen zijn momenteel niet ondersteund.
- De gebruikersnaam en het wachtwoord voor een account dat is eigenaar van het abonnement (MSA/2FA accounts worden ondersteund).
- *Niet vereist vanaf Azure Stack 1712 updateversie (180106.1):* de Azure Active Directory voor het Azure-abonnement. U kunt deze map vinden in Azure door de muiswijzer op uw avatar in de rechterbovenhoek van de Azure portal.

Als u geen een Azure-abonnement dat aan deze vereisten voldoet, kunt u [maken van een gratis Azure-account hier](https://azure.microsoft.com/en-us/free/?b=17.06). Azure-Stack registreren maakt geen kosten op uw Azure-abonnement.

## <a name="register-azure-stack-with-azure"></a>Azure Stack registreren bij Azure  
> [!NOTE]
> Alle deze stappen moeten worden uitgevoerd vanaf een machine die toegang tot de bevoegde eindpunt heeft. Voor Azure Stack Development Kit zou worden de hostcomputer. Als u een geïntegreerd systeem, moet u contact op met uw Azure-Stack-operator.
>

1. Open een PowerShell-console als beheerder en [Installeer PowerShell voor Azure-Stack](azure-stack-powershell-install.md).  

2. De Azure-account waarmee u Azure-Stack registreert toevoegen. Uitvoeren als u wilt toevoegen op het account, de `Add-AzureRmAccount` cmdlet uit met de parameter EnvironmentName is ingesteld op **AzureCloud**. U wordt gevraagd de referenties van uw Azure-account in te voeren en wellicht 2-factor-verificatie op basis van de configuratie van uw account te gebruiken.

   ```Powershell
   Add-AzureRmAccount -EnvironmentName "AzureCloud"
   ```

3. Als u meerdere abonnementen hebt, voert u de volgende opdracht om te selecteren die dat u wilt gebruiken:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Voer de volgende opdracht om de Azure-Stack-resourceprovider registreren in uw Azure-abonnement:

   ```Powershell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Verwijder alle bestaande versies van de PowerShell-modules die met de registratie overeenkomen en [download de nieuwste versie van deze vanuit GitHub](azure-stack-powershell-download.md).  

6. In de map 'AzureStack-hulpprogramma's-master' dat is gemaakt in de vorige stap, navigeer naar de map "Registratie" en importeer de module '.\RegisterWithAzure.psm1':  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

7. Voer in dezelfde PowerShell-sessie het volgende script: wanneer u wordt gevraagd om referenties, opgeven `azurestack\cloudadmin` als de gebruiker en het wachtwoord hetzelfde is als wat u hebt gebruikt voor de lokale beheerder tijdens de implementatie.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Set-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development
   ```

   | Parameter | Beschrijving |  
   |--------|-------------|
   | CloudAdminCredential | De cloud-domein-referenties die worden gebruikt om [toegang tot de bevoegde eindpunt](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). De gebruikersnaam is in de notatie  **\<Azure Stack domein\>\cloudadmin**. Voor development kit, de gebruikersnaam is ingesteld op **azurestack\cloudadmin**. Als u een geïntegreerd systeem, moet u contact op met uw Azure-Stack-operator als deze waarde wilt ophalen.|  
   | PrivilegedEndpoint | Een vooraf geconfigureerde externe PowerShell-console waarmee u mogelijkheden, zoals logboekgegevens verzameld en andere post implementatietaken. Voor development kit wordt het bevoegde eindpunt gehost op de 'AzS ERCS01' virtuele machine. Als u een geïntegreerd systeem, moet u contact op met uw Azure-Stack-operator als deze waarde wilt ophalen. Raadpleeg voor meer informatie de [met behulp van het eindpunt van de bevoegde](azure-stack-privileged-endpoint.md) artikel.|  
   | BillingModel | Het facturering model die gebruikmaakt van uw abonnement. Toegestane waarden voor deze parameter zijn - **capaciteit**, **PayAsYouUse**, en **ontwikkeling**. Voor development kit deze waarde is ingesteld op **ontwikkeling**. Als u een geïntegreerd systeem, moet u contact op met uw Azure-Stack-operator als deze waarde wilt ophalen. |

8. Als het script is voltooid, ziet u een bericht ' activeren Azure Stack (deze stap kan maximaal 10 minuten duren). '




## <a name="verify-the-registration"></a>Controleer of de registratie  

1. Aanmelden bij de beheerdersportal (https://adminportal.local.azurestack.external).

2. Klik op **meer Services** > **Marketplace Management** > **toevoegen vanuit Azure**.

3. Als u ziet een lijst met items die beschikbaar is via Azure (zoals WordPress), wordt de activering is geslaagd.

> [!NOTE]
> Nadat de registratie voltooid is, wordt de actieve waarschuwing voor het registreren van niet langer weergegeven.


## <a name="modify-the-registration"></a>Wijzigen van de registratie

### <a name="change-the-subscription-you-use"></a>Wijzigen van het abonnement dat u gebruikt
Als u wijzigen van het abonnement dat u gebruikt wilt, moet u eerst Remove-AzsRegistration uitvoert, zorg ervoor dat u bent aangemeld bij de juiste context van de Azure PowerShell en de voert u Set-AzsRegistration met gewijzigde parameters.

  ```Powershell   
  Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```
### <a name="change-the-billing-model-or-syndication-features"></a>Wijzig de facturering model of syndication-functies
Als u wilt de facturering model of de functies voor uw installatie syndicatie wilt wijzigen, kunt u de registratiefunctie voor het instellen van de nieuwe waarden kunt aanroepen. U hoeft niet te verwijdert u eerst de huidige registratie.  

  ```Powershell
     Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```


## <a name="disconnected-registration"></a>Niet-verbonden registratie
*De informatie in deze sectie geldt die begint met de updateversie Azure Stack 1712 (180106.1) en wordt niet ondersteund in eerdere versies.*

Als u Azure-Stack in een omgeving zonder verbinding registreert, moet u een registratie-token ophalen uit de Stack van Azure-omgeving en dat token vervolgens gebruiken op een computer die verbinding met Azure voor registratie maken kan.  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Een registratie-token ophalen uit de Stack van Azure-omgeving
  1. Als u het registratietoken, voert u de volgende:  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > De registratietoken is opgeslagen in het bestand dat is opgegeven voor *$env:SystemDrive\RegistrationToken.txt*.

  2. Sla dit registratietoken voor gebruik op de Azure verbonden computer.


### <a name="connect-to-azure-and-register"></a>Verbinding maken met Azure en registreren
De stappen voor deze procedure uitvoeren op een computer die verbinding met Azure maken kan.

  1. [Download de meest recente PowerShell-modules die overeenkomen met de registratie vanuit GitHub](azure-stack-powershell-download.md).  

  2. In de map 'AzureStack-hulpprogramma's-master' dat is gemaakt in de vorige stap, navigeer naar de map "Registratie" en importeer de module '.\RegisterWithAzure.psm1':  

     ```powershell
     Import-Module .\RegisterWithAzure.psm1
     ```

  3. Kopiëren [RegisterWithAzure.psm1](https://go.microsoft.com/fwlink/?linkid=842959) naar een map, zoals *C:\Temp*.

  4. Start PowerShell ISE als beheerder en importeer de module RegisterWithAzure.  

  5. Zorg ervoor dat u bent aangemeld bij de juiste context van Azure PowerShell die u gebruiken wilt voor het registreren van uw Azure-Stack-omgeving:  

     ```Powershell
        Set-AzureRmContext -SubscriptionId $YourAzureSubscriptionId   
     ```
  6. Geef de registratietoken registreren bij Azure:

     ```Powershell  
       $registrationToken = "*Your Registration Token*"
       Register-AzsEnvironment -RegistrationToken $registrationToken  
     ```
    Desgewenst kunt u de cmdlet Get-inhoud om te verwijzen naar een bestand met uw registratietoken:

    ```Powershell  
       $registrationToken = Get-Content -Path 'C:\Temp\<Registration Token File>'
       Register-AzsEnvironment -RegistrationToken $registrationToken  
    ```
> [!NOTE]  
> Sla de resourcenaam registratie of het registratietoken voor toekomstig gebruik.

### <a name="remove-a-registered-resource"></a>Verwijderen van een geregistreerde bron
Als u wilt de registratie-resource te verwijderen, moet u UnRegister-AzsEnvironment gebruiken en in de naam van de registratie-resource of de registratietoken die u voor Register AzsEnvironment gebruikt doorgeven.
- **Naam van de bron registratie:**

  ```Powershell    
     UnRegister-AzsEnvironment -RegistrationName "*Name of the registration resource*"
  ```
- **Registratietoken:**    

  ```Powershell
     $registrationToken = "*Your copied registration token*"
     UnRegister-AzsEnvironment -RegistrationToken $registrationToken
   ```


## <a name="next-steps"></a>Volgende stappen
[Verbinding maken met Azure Stack](azure-stack-connect-azure-stack.md)

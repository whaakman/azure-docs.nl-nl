---
title: Azure-account om u als partner-ID koppelen | Microsoft Docs
description: Bewaken met Azure-klanten door partner-ID koppelen aan het gebruikersaccount dat u gebruikt voor het beheren van resources van de klant bijhouden.
services: billing
author: dhirajgandhi
ms.author: cwatson
ms.date: 03/12/2018
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 8226ff956db1b21c308d184e8d5876a59d1b646d
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391775"
---
# <a name="link-partner-id-to-your-azure-accounts"></a>De partner-ID koppelen aan uw Azure-accounts

Als partner kunt u uw invloed bijhouden in uw klant-engagements door uw partner-ID koppelen aan de accounts die worden gebruikt voor het beheren van resources van de klant.

Deze functie is beschikbaar in een openbare preview.

## <a name="get-access-from-your-customer"></a>Toegang van uw klant ontvangt

Voordat u uw partner-ID koppelen, moet uw klant u toegang geven tot hun Azure-resources met behulp van een van de volgende opties:

- **Gastgebruiker:** uw klant kunt u als een gastgebruiker toevoegen en toewijzen van alle RBAC-rollen. Zie voor meer informatie, [gastgebruikers toevoegen van een andere directory](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Directory-account:** uw klant kunt maken van een nieuwe gebruiker van uw organisatie in de map en alle RBAC-rol toe te wijzen.

- **Service-principal:** uw klant een app of script toevoegen van uw organisatie in de map en alle RBAC-rol toe te wijzen. De identiteit van de app of script staat bekend als service-principal.

## <a name="link-partner-id"></a>Partner-id koppelen

Wanneer u toegang tot resources van de klant hebt, gebruikt u Azure portal, PowerShell of CLI te koppelen van het Microsoft Partner Network-ID (MPN-ID) aan uw gebruikers-ID of service-principal. U moet de partner-ID in elke klanttenant koppelen.

### <a name="use-azure-portal-to-link-new-partner-id"></a>Azure portal gebruiken voor het koppelen van nieuwe partner-ID

1. Ga naar [koppeling naar een partner-ID](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) in Azure portal.

2. Meld u aan bij Azure Portal.

3. De Microsoft-partner-id invoeren. De partner-ID is de [Microsoft-Partner Network(MPN)](https://partner.microsoft.com/) -ID van uw organisatie.

  ![Schermafbeelding van partner-ID koppelen](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Gebruik de directorywisselaar om een koppeling partner-ID voor een andere klant. Kies onder schakelen tussen mappen, uw directory.

  ![Schermafbeelding van partner-ID koppelen](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-new-partner-id"></a>PowerShell gebruiken voor het koppelen van nieuwe partner-ID

1. Installeer de [AzureRM.ManagementPartner](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner) PowerShell-Module.

2. Meld u aan bij de tenant van de klant aan het gebruikersaccount of de service-principal, voor meer informatie, Zie [Meld u aan met Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-5.2.0).
 
   ```azurepowershell-interactive
    C:\> Connect-AzureRmAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
   ```


3. Koppeling van de nieuwe partner-ID. De partner-ID is de [Microsoft-Partner Network(MPN)](https://partner.microsoft.com/) -ID van uw organisatie.

    ```azurepowershell-interactive
    C:\> new-AzureRmManagementPartner -PartnerId 12345 
    ```

#### <a name="get-the-linked-partner-id"></a>De gekoppelde partner-ID ophalen
```azurepowershell-interactive
C:\> get-AzureRmManagementPartner 
```

#### <a name="update-the-linked-partner-id"></a>Bijwerken van de gekoppelde partner-ID
```azurepowershell-interactive
C:\> Update-AzureRmManagementPartner -PartnerId 12345 
```
#### <a name="delete-the-linked-partner-id"></a>Verwijder de gekoppelde partner-ID
```azurepowershell-interactive
C:\> remove-AzureRmManagementPartner -PartnerId 12345 
```

### <a name="use-cli-to-link-new-partner-id"></a>CLI gebruiken voor het koppelen van nieuwe partner-ID
1.  Installeer de CLI-extensie.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ``` 

2.  Aanmelden bij de klanttenant met het gebruikersaccount of de service-principal. Zie voor meer informatie, [Meld u aan met Azure CLI](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ``` 

3.  Koppeling van de nieuwe partner-ID. De partner-ID is de [Microsoft-Partner Network(MPN)](https://partner.microsoft.com/) -ID van uw organisatie.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>De gekoppelde partner-ID ophalen
```azurecli-interactive
C:\ az managementpartner show
``` 

#### <a name="update-the-linked-partner-id"></a>Bijwerken van de gekoppelde partner-ID
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
``` 

#### <a name="delete-the-linked-partner-id"></a>Verwijder de gekoppelde partner-ID
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
``` 

## <a name="next-steps"></a>Volgende stappen

Deelnemen aan de discussie de [Microsoft-Partner-Community](https://aka.ms/PALdiscussion) ontvangen van updates of feedback verzenden.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Wie kan de partner-ID koppelen?**

Gebruikers van de partnerorganisatie die wordt beheerd door de klant-resource kan de partner-ID koppelen aan het account.

**Nadat een partner-ID is gekoppeld kan het worden gewijzigd?**

Ja, gekoppelde partner-ID kan worden gewijzigd, toegevoegd of verwijderd.

**Wat gebeurt er als een gebruiker een account heeft in meerdere tenants van de klant?**

De koppeling tussen de partner-ID en het account wordt gedaan voor elke klanttenant.  U moet de partner-ID in elke klanttenant koppelen.

**Kan andere partner of klant bewerken of verwijderen van de koppeling naar de partner-ID?**

De koppeling is gekoppeld op het accountniveau. U kunt alleen bewerken of verwijderen van de koppeling naar de partner-ID. De klant en andere partner kunnen de koppeling wijzigen in de partner-ID. 

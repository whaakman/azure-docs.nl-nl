---
title: Azure-account naar ID partner koppelen | Microsoft Docs
description: Bijhouden betrokkenheid met Azure-klanten door de partner-ID te koppelen aan de gebruikersaccount die u gebruikt om de resources van de klant te beheren.
services: billing
author: dhirajgandhi
ms.author: dhgandhi
ms.date: 03/12/2018
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: a5b6bb683538bd5359ebcbbe2640216e574b3e00
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="link-partner-id-to-your-azure-accounts"></a>Partner-ID voor koppeling naar uw Azure-accounts

Als partner kunt u het effect van uw bijhouden via de betrokkenheid van uw klant uw partner-ID koppelt aan de accounts die worden gebruikt voor het beheren van resources van de klant.

Deze functie is beschikbaar in een openbare preview.

## <a name="get-access-from-your-customer"></a>Toegang krijgen van uw klant

Voordat u uw partner-ID koppelt, moet de klant krijgt u toegang tot Azure-resources met behulp van een van de volgende opties:

- **Gastgebruiker:** uw klant kunt u toevoegen als gastgebruiker en geen RBAC-rollen toewijzen. Zie voor meer informatie [gastgebruikers toevoegen van een andere map](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

- **Directory-account:** uw klant kunt maken van een nieuwe gebruiker van uw organisatie in de map en eventuele RBAC-rol toe te wijzen.

- **Service-principal:** uw klant kunt toevoegen van een app of script uit uw organisatie in de directory en eventuele RBAC-rol toe te wijzen. De identiteit van de app of het script staat bekend als service-principal.

## <a name="link-partner-id"></a>Koppeling partner-ID

Wanneer u toegang tot bronnen van de klant hebt, gebruik Azure-portal, PowerShell of CLI om te koppelen van uw Microsoft Partner Network-ID (ID MPN) aan uw gebruikers-ID of service-principal. U moet de partner-ID in elke tenant klant koppelen.

### <a name="use-azure-portal-to-link-new-partner-id"></a>Azure portal gebruiken voor het koppelen van nieuwe partner-ID

1. Ga naar [koppeling naar een partner-ID](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) in de Azure portal.

2. Meld u aan bij Azure Portal.

3. Voer de Microsoft-partner-ID. De partner-ID is de [Microsoft Partner Network(MPN)](https://partner.microsoft.com/) -ID van uw organisatie.

  ![Schermafbeelding van koppeling partner-ID](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Als koppeling partner-ID voor een andere klant, gebruikt u de directory schakelbaar. Kies uw directory onder Switch-directory.

  ![Schermafbeelding van koppeling partner-ID](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-new-partner-id"></a>PowerShell gebruiken voor het koppelen van nieuwe partner-ID

1. Installeer de [AzurePartnerRP](https://www.powershellgallery.com/packages/AzureRM.ManagementPartner/0.1.0-preview) PowerShell-Module.

2. Aanmelden bij de klant tenant met het gebruikersaccount of de service-principal, voor meer informatie, Zie [aanmelding met Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-5.2.0).
 
   ```azurepowershell-interactive
    C:\> Connect-AzureRmAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
   ```


3. Koppelen van de nieuwe partner-ID. De partner-ID is de [Microsoft Partner Network(MPN)](https://partner.microsoft.com/) -ID van uw organisatie.

    ```azurepowershell-interactive
    C:\> new-AzureRmManagementPartner -PartnerId 12345 
    ```

#### <a name="get-the-linked-partner-id"></a>De gekoppelde partner-ID niet ophalen
```azurepowershell-interactive
C:\> get-AzureRmManagementPartner 
```

#### <a name="update-the-linked-partner-id"></a>Bijwerken van de gekoppelde partner-ID
```azurepowershell-interactive
C:\> Update-AzureRmManagementPartner -PartnerId 12345 
```
#### <a name="delete-the-linked-partner-id"></a>Verwijderen van de gekoppelde partner-ID
```azurepowershell-interactive
C:\> remove-AzureRmManagementPartner -PartnerId 12345 
```

### <a name="use-cli-to-link-new-partner-id"></a>Gebruik CLI voor het koppelen van nieuwe partner-ID
1.  De CLI-uitbreiding te installeren.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ``` 

2.  Aanmelden bij de klant-tenant met het gebruikersaccount of de service-principal. Zie voor meer informatie [aanmelden met Azure CLI 2.0](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ``` 

3.  Koppelen van de nieuwe partner-ID. De partner-ID is de [Microsoft Partner Network(MPN)](https://partner.microsoft.com/) -ID van uw organisatie.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>De gekoppelde partner-ID niet ophalen
```azurecli-interactive
C:\ az managementpartner show
``` 

#### <a name="update-the-linked-partner-id"></a>Bijwerken van de gekoppelde partner-ID
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
``` 

#### <a name="delete-the-linked-partner-id"></a>Verwijderen van de gekoppelde partner-ID
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
``` 

## <a name="next-steps"></a>Volgende stappen

Deelnemen aan de bespreking van de [Microsoft-Partner-Community](https://aka.ms/PALdiscussion) updates ontvangen of verzenden van feedback.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Wie de partner-ID kunt koppelen?**

Alle gebruikers van de partnerorganisatie die wordt beheerd door de bron van de klant kunt u de partner-ID koppelen aan het account.

**Wanneer een partner-ID is gekoppeld kan deze worden gewijzigd?**

Ja, gekoppelde partner-ID kan worden gewijzigd, toegevoegd of verwijderd.

**Wat gebeurt er als een gebruiker een account heeft in meerdere klant tenants?**

De koppeling tussen de partner-ID en het account wordt gedaan voor elke klant-tenant.  U moet de partner-ID in elke tenant klant koppelen.

**Kan andere partner of klant Bewerk of verwijder de koppeling naar de partner-ID?**

De koppeling is gekoppeld op het niveau van het account. U kunt alleen bewerken of verwijder de koppeling naar de partner-ID. De klant en de andere partner wijzigen de koppeling niet in de partner-ID. 

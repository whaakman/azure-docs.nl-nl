---
title: Een Azure-account koppelen aan een partner-ID | Microsoft Docs
description: Volg afspraken met Azure-klanten door een partner-ID te koppelen aan het gebruikers account dat u gebruikt voor het beheren van de resources van de klant.
services: billing
author: dhirajgandhi
manager: dhgandhi
ms.author: banders
ms.date: 03/12/2019
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 0448ffbccddc913bd6359f5f6bbf42988239afb4
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706407"
---
# <a name="link-a-partner-id-to-your-azure-accounts"></a>Een partner-ID koppelen aan uw Azure-accounts

Micro soft-partners bieden services waarmee klanten zakelijke en doel stellingen kunnen bereiken met behulp van micro soft-producten. Bij het handelen namens de klant die Azure-Services beheert, configureert en ondersteunt, hebben de partner gebruikers toegang nodig tot de omgeving van de klant. Met de koppeling partner beheerder kunnen partners de netwerk-ID van de partner koppelen aan de referenties die worden gebruikt voor de levering van services.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-access-from-your-customer"></a>Toegang krijgen tot uw klant

Voordat u uw partner-ID koppelt, moet uw klant u toegang geven tot hun Azure-resources met behulp van een van de volgende opties:

- **Gast gebruiker**: Uw klant kan u als gast gebruiker toevoegen en op rollen gebaseerd toegangs beheer (RBAC) rollen toewijzen. Zie [gast gebruikers toevoegen vanuit een andere Directory](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)voor meer informatie.

- **Directory-account**: Uw klant kan een gebruikers account voor u maken in een eigen map en een RBAC-rol toewijzen.

- **Service-Principal**: Uw klant kan een app of script toevoegen uit uw organisatie in hun Directory en een RBAC-rol toewijzen. De identiteit van de app of het script wordt een Service-Principal genoemd.

## <a name="link-to-a-partner-id"></a>Koppelen aan een partner-id

Wanneer u toegang hebt tot de resources van de klant, gebruikt u de Azure Portal, Power shell of de Azure CLI om uw Microsoft Partner Network-ID (MPN-ID) te koppelen aan uw gebruikers-ID of Service-Principal. Koppel de partner-ID aan elke Tenant van de klant.

### <a name="use-the-azure-portal-to-link-to-a-new-partner-id"></a>De Azure Portal gebruiken om een koppeling te maken naar een nieuwe partner-ID

1. Ga naar [een koppeling naar een partner-id](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) in de Azure Portal.

2. Meld u aan bij Azure Portal.

3. Voer de micro soft-partner-ID in. De partner-ID is de [Microsoft Partner Network](https://partner.microsoft.com/) -id voor uw organisatie.

   ![Scherm afbeelding met een koppeling naar een partner-ID](./media/billing-link-partner-id/link-partner-ID.PNG)

4. Als u een partner-ID voor een andere klant wilt koppelen, schakelt u over naar de map. Selecteer in de **map switch**uw Directory.

   ![Scherm afbeelding met de map voor de switch](./media/billing-link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-to-a-new-partner-id"></a>Power shell gebruiken om een koppeling te maken naar een nieuwe partner-ID

1. Installeer de Power shell-module [AZ. ManagementPartner](https://www.powershellgallery.com/packages/Az.ManagementPartner/) .

2. Meld u aan bij de Tenant van de klant met het gebruikers account of de Service-Principal. Zie [Aanmelden met Power shell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)voor meer informatie.

   ```azurepowershell-interactive
    C:\> Connect-AzAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
   ```

3. Koppeling naar de nieuwe partner-ID. De partner-ID is de [Microsoft Partner Network](https://partner.microsoft.com/) -id voor uw organisatie.

    ```azurepowershell-interactive
    C:\> new-AzManagementPartner -PartnerId 12345
    ```

#### <a name="get-the-linked-partner-id"></a>De gekoppelde partner-ID ophalen
```azurepowershell-interactive
C:\> get-AzManagementPartner
```

#### <a name="update-the-linked-partner-id"></a>De gekoppelde partner-ID bijwerken
```azurepowershell-interactive
C:\> Update-AzManagementPartner -PartnerId 12345
```
#### <a name="delete-the-linked-partner-id"></a>De gekoppelde partner-ID verwijderen
```azurepowershell-interactive
C:\> remove-AzManagementPartner -PartnerId 12345
```

### <a name="use-the-azure-cli-to-link-to-a-new-partner-id"></a>De Azure CLI gebruiken om een koppeling te maken met een nieuwe partner-ID
1. Installeer de Azure CLI-extensie.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ```

2. Meld u aan bij de Tenant van de klant met het gebruikers account of de Service-Principal. Zie [Aanmelden met de Azure cli](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest)voor meer informatie.

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ```

3. Koppeling naar de nieuwe partner-ID. De partner-ID is de [Microsoft Partner Network](https://partner.microsoft.com/) -id voor uw organisatie.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>De gekoppelde partner-ID ophalen
```azurecli-interactive
C:\ az managementpartner show
```

#### <a name="update-the-linked-partner-id"></a>De gekoppelde partner-ID bijwerken
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
```

#### <a name="delete-the-linked-partner-id"></a>De gekoppelde partner-ID verwijderen
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
```

## <a name="next-steps"></a>Volgende stappen

Neem lid van de discussie in de [micro soft partner-community](https://aka.ms/PALdiscussion) om updates te ontvangen of feedback te verzenden.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Wie kan de partner-ID koppelen?**

Elke gebruiker van de partner organisatie die de Azure-resources van een klant beheert, kan de partner-ID aan het account koppelen.

**Kan een partner-ID worden gewijzigd nadat deze is gekoppeld?**

Ja. Een gekoppelde partner-ID kan worden gewijzigd, toegevoegd of verwijderd.

**Wat gebeurt er als een gebruiker in meer dan één klant Tenant een account heeft?**

De koppeling tussen de partner-ID en het account wordt uitgevoerd voor elke Tenant van de klant. Koppel de partner-ID aan elke Tenant van de klant.

**Kunnen andere partners of klanten de koppeling naar de partner-ID bewerken of verwijderen?**

De koppeling is gekoppeld op het niveau van het gebruikers account. Alleen u kunt de koppeling naar de partner-ID bewerken of verwijderen. De klant en andere partners kunnen de koppeling naar de partner-ID niet wijzigen.


**Welke MPN-ID moet ik gebruiken als mijn bedrijf meerdere heeft?**

Partner locatie accounts en gekoppelde MPN-Id's moeten worden gebruikt voor het koppelen van de partner-ID.  Meer informatie over [partner accounts](https://docs.microsoft.com/partner-center/account-structure)

**Waar vind ik beïnvloede omzet rapportage voor gekoppelde partner-ID?**

Rapportage over de prestaties van Cloud producten is beschikbaar voor partners in het partner centrum van [mijn Insights-dash board](https://partner.microsoft.com/membership/reports/myinsights). U moet de koppeling partner beheerder selecteren als het partner koppelings type.

**Waarom kan ik mijn klant niet zien in de rapporten?**

U kunt de klant niet zien in de rapporten om de volgende redenen:

1. Het gekoppelde gebruikers account heeft geen op [rollen gebaseerde toegang tot](https://docs.microsoft.com/azure/role-based-access-control/overview) een Azure-abonnement of-resource van een klant.

2. Het Azure-abonnement waar de gebruiker toegang tot [op rollen gebaseerde toegangs](https://docs.microsoft.com/azure/role-based-access-control/overview) rechten heeft, heeft geen gebruik.

**Werkt de koppelings partner-ID met Azure Stack?**

Ja, u kunt uw partner-ID koppelen voor Azure Stack.

---
title: MSI configureren op een virtuele machine van Azure schaal ingesteld met de Azure-portal
description: Stap door stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op Azure VMSS, met de Azure portal.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: 4d308d9cdc0405ee0041c877f5678647343631ab
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="configure-an-azure-virtual-machine-scale-set-managed-service-identity-msi-using-the-azure-portal"></a>Een Azure virtuele Machine Scale ingesteld beheerde Service identiteit (MSI) met de Azure portal configureren

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u informatie over het inschakelen en MSI voor een virtuele machine van Azure-schaalset verwijderen met behulp van de Azure-portal.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="enable-msi-during-creation-of-azure-virtual-machine-scale-set"></a>MSI inschakelen bij het maken van Azure virtuele-machineschaalset

Vanaf het moment van schrijven van dit wordt MSI inschakelen tijdens het maken van een virtuele-machineschaalset ingesteld in de Azure-portal niet ondersteund. In plaats daarvan, Raadpleeg de volgende virtuele machine van Azure scale set maken Quick Start artikel eerst een Azure virtuele-machineschaalset maken:

- [Een virtuele-Machineschaalset maken in de Azure portal](../../virtual-machine-scale-sets/virtual-machine-scale-sets-create-portal.md)  

Gaat u verder met de volgende sectie voor meer informatie over het inschakelen van MSI op de virtuele-machineschaalset.

## <a name="enable-msi-on-an-existing-azure-vmms"></a>MSI op een bestaande Azure-VMMS inschakelen

Als u een virtuele-machineschaalset die oorspronkelijk was ingericht zonder een MSI-bestand hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Klik op de **configuratie** pagina, MSI inschakelen op de virtuele-machineschaalset instellen door te selecteren **Ja** onder 'Beheerde identiteit service', klik vervolgens op **opslaan**. Deze bewerking duurt 60 seconden of langer om te voltooien:

   ![Schermopname van configuratie-pagina](../media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-virtual-machine-scale-set"></a>MSI uit een Azure virtuele-machineschaalset verwijderen

Als u een virtuele-machineschaalset die een MSI-bestand niet meer nodig hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen op de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Klik op de **configuratie** pagina, MSI verwijdert uit de virtuele-machineschaalset instellen door te selecteren **Nee** onder **beheerde service-identiteit**, klikt u vervolgens op **opslaan** . Deze bewerking duurt 60 seconden of langer om te voltooien:

   ![Schermopname van configuratie-pagina](../media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](overview.md).
- Azure Portal gebruikt, Geef een virtuele machine van Azure schaalset MSI [toegang tot een andere Azure-resource](howto-assign-access-portal.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

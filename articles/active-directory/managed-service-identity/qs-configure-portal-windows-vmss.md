---
title: MSI configureren op een virtuele machine van Azure schaal ingesteld met de Azure-portal
description: Stap door stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op Azure VMSS, met de Azure portal.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: af17bf716ce22bc7c02d40def36248facb6fbcc4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="configure-a-vmss-managed-service-identity-msi-using-the-azure-portal"></a>Een VMSS beheerde Service identiteit (MSI) met de Azure portal configureren

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u hoe inschakelen en uitschakelen van het systeem toegewezen identiteit voor een VMSS, met de Azure portal. Toe te wijzen en verwijderen van de gebruiker toegewezen identiteiten uit een Azure-VMSS is momenteel niet ondersteund via Azure Portal.

> [!NOTE]
> Gebruiker toegewezen identiteit bewerkingen worden op dit moment niet ondersteund via Azure Portal. Controleer regelmatig op updates.

## <a name="prerequisites"></a>Vereisten


- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md).
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.

## <a name="managed-service-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Beheerde Service-identiteit bij het maken van een Azure virtuele-machineschaalset

Maken van VM's via Azure portal ondersteunt op dit moment niet beheerd Service-identiteit bewerkingen. In plaats daarvan, Raadpleeg de volgende virtuele machine van Azure scale set maken Quick Start artikel eerst een Azure virtuele-machineschaalset maken:

- [Een virtuele-Machineschaalset maken in de Azure portal](../../virtual-machine-scale-sets/quick-create-portal.md)  

Gaat u verder met de volgende sectie voor meer informatie over het inschakelen van MSI op de virtuele-machineschaalset.

## <a name="enable-managed-service-identity-on-an-existing-azure-vmms"></a>Service-identiteit beheerd op een bestaande Azure-VMMS inschakelen

Het systeem toegewezen identiteit op een virtuele machine die oorspronkelijk was ingericht zonder deze inschakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Inschakelen van de identiteit van de automatisch toegewezen op de virtuele machine door het selecteren van 'Ja' onder 'Identiteit beheerde service' en klik vervolgens op **opslaan**. Deze bewerking duurt 60 seconden of langer om te voltooien:

   [![Schermopname van configuratie-pagina](../media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

## <a name="remove-managed-service-identity-from-an-azure-virtual-machine-scale-set"></a>Managed Service-identiteit uit een Azure virtuele-machineschaalset verwijderen

Als u een virtuele-machineschaalset die een MSI-bestand niet meer nodig hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen op de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Uitschakelen van het systeem toegewezen identiteit op de virtuele machine door als u op 'Nee' onder 'Identiteit beheerde service' en klik op opslaan. Deze bewerking duurt 60 seconden of langer om te voltooien:

   ![Schermopname van configuratie-pagina](../media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van de Service-identiteit beheerd [overzicht](overview.md).

## <a name="next-steps"></a>Volgende stappen

- Azure Portal gebruikt, Geef een virtuele machine van Azure schaalset MSI [toegang tot een andere Azure-resource](howto-assign-access-portal.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

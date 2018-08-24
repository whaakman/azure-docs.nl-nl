---
title: Beheerde Service-identiteit op een schaalset van de virtuele machine van Azure met behulp van de Azure-portal configureren
description: Voor stap door stap instructies voor het configureren van een beheerde Service-identiteit voor Azure VMSS, met behulp van de Azure portal.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: bfd63262a1d5568223b4e4077e2f8c987b7ec0d4
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746733"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-using-the-azure-portal"></a>Een virtuele machine configureren Virtual Machine scale sets beheerde Service-identiteit met Azure portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u inschakelen en uitschakelen van systeem en door gebruiker toegewezen identiteit voor een VM-schaalset met behulp van de Azure-portal.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md).
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de roltoewijzing van de volgende:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) inschakelen en het systeem toegewezen beheerde identiteit van een virtuele-machineschaalset verwijderen.

## <a name="system-assigned-identity"></a>Systeem toegewezen identiteit 

In deze sectie leert u hoe u kunt in- en uitschakelen van het systeem toegewezen identiteit op een VM-schaalset met behulp van de Azure-portal.

### <a name="enable-system-assigned-identity-during-creation-of-a-virtual-machine-scale-set"></a>Systeem toegewezen identiteit inschakelen tijdens het maken van een virtuele-machineschaalset

De Azure-portal biedt momenteel geen ondersteuning voor inschakelen systeem toegewezen identiteit tijdens het maken van een virtuele-machineschaalset. In plaats daarvan kunt u verwijzen naar de volgende virtuele machine scale set maken van het artikel maakt eerst een virtuele-machineschaalset en gaat u verder met de volgende sectie voor meer informatie over het inschakelen van systeem toegewezen identiteit op een virtuele-machineschaalset:

- [Een virtuele-Machineschaalset maken in Azure portal](../../virtual-machine-scale-sets/quick-create-portal.md)  

### <a name="enable-system-assigned-identity-on-an-existing-virtual-machine-scale-set"></a>Systeem toegewezen identiteit op een bestaande virtuele-machineschaalset inschakelen

Het systeem toegewezen identiteit op een virtuele-machineschaalset die oorspronkelijk is ingericht zonder inschakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Schakelt het systeem toegewezen identiteit op de virtuele machine door 'Ja' onder 'Beheerde service-identiteit' en klik vervolgens op **opslaan**. Met deze bewerking duurt 60 seconden of langer om uit te voeren:

   [![Schermafbeelding van de pagina configuratie](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

### <a name="remove-system-assigned-identity-from-a-virtual-machine-scale-set"></a>Verwijder de systeem toegewezen identiteit van een virtuele-machineschaalset

Als u een virtuele-machineschaalset die niet langer moet een systeem toegewezen identiteit:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Het systeem toegewezen identiteit op de virtuele machine selecteert u 'Nee' onder "Beheerde service-identiteit" uitschakelen en klik op opslaan. Met deze bewerking duurt 60 seconden of langer om uit te voeren:

   ![Schermafbeelding van de pagina configuratie](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen identiteit van een VM-schaalset met behulp van de Azure-portal.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Een gebruiker toegewezen identiteit toewijzen tijdens het maken van een virtuele-machineschaalset

Azure portal biedt momenteel geen ondersteuning voor het toewijzen van een gebruiker toegewezen identiteit tijdens het maken van een virtuele-machineschaalset. In plaats daarvan kunt u verwijzen naar de volgende virtuele machine scale set maken van het artikel maakt eerst een virtuele-machineschaalset en gaat u verder met de volgende sectie voor meer informatie over het toewijzen van een gebruiker toegewezen identiteit:

- [Een virtuele-Machineschaalset maken in Azure portal](../../virtual-machine-scale-sets/quick-create-portal.md)

### <a name="assign-a-user-assigned-identity-to-an-existing-virtual-machine-scale-set"></a>Een gebruiker toegewezen identiteit toewijzen aan een bestaande virtuele-machineschaalset

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.
2. Navigeer naar de gewenste virtuele-machineschaalset en klik op **identiteit**, **gebruiker toegewezen** en vervolgens  **\+toevoegen**.

   ![Door gebruiker toegewezen identiteit toevoegen aan VMSS](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vmss-screenshot1.png)

3. Klik op de gebruiker toegewezen identiteit die u wilt toevoegen aan de virtuele-machineschaalset en klik vervolgens op **toevoegen**.
   
   ![Door gebruiker toegewezen identiteit toevoegen aan VMSS](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-virtual-machine-scale-set"></a>Een gebruiker toegewezen identiteit van een virtuele-machineschaalset verwijderen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.
2. Navigeer naar de gewenste virtuele-machineschaalset en klik op **identiteit**, **gebruiker toegewezen**, de naam van de identiteit van de gebruiker toegewezen die u wilt verwijderen en klik vervolgens op **verwijderen** () Klik op **Ja** in het deelvenster bevestiging).

   ![Door gebruiker toegewezen identiteit van een VMSS verwijderen](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vmss-screenshot.png)


## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van de beheerde Service-identiteit, [overzicht](overview.md).

## <a name="next-steps"></a>Volgende stappen

- Met behulp van Azure portal, krijgt een Azure virtuele-machineschaalset beheerde Service-identiteit [toegang tot een andere Azure-resource](howto-assign-access-portal.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.

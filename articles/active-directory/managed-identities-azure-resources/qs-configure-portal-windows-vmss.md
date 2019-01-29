---
title: Het configureren van beheerde identiteiten voor Azure-resources op een virtuele-machineschaalset
description: Stap voor stap beheerde instructies voor het configureren van identiteiten voor een Azure-resources op een VM-schaalset met behulp van de Azure-portal.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: priyamo
ms.openlocfilehash: 18344ff6c2145f4be5fa79fa16f4e61a2a2386b1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55193366"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-virtual-machine-scale-set-using-the-azure-portal"></a>Configureren van beheerde identiteiten voor Azure-resources op een VM-schaalset met behulp van de Azure-portal

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel, leert met behulp van PowerShell, u hoe u de volgende beheerde identiteiten voor bewerkingen van de Azure-resources op een virtuele-machineschaalset uitvoeren:

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md).
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de volgende Azure op basis van rollen access control-toewijzingen:

    > [!NOTE]
    > Er zijn geen extra Azure AD directory-roltoewijzingen is vereist.

    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) inschakelen en verwijder de door het systeem toegewezen beheerde identiteit van een virtuele-machineschaalset.

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u kunt in- en uitschakelen van de door het systeem toegewezen beheerde identiteit met Azure portal.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-virtual-machine-scale-set"></a>Beheerde identiteit systeem toegewezen tijdens het maken van een virtuele-machineschaalset inschakelen

De Azure-portal ondersteunt momenteel geen beheerde identiteit systeem toegewezen tijdens het maken van een virtuele-machineschaalset inschakelen. In plaats daarvan raadpleegt u de volgende maken van de virtuele machine scale set snelstartartikel voor het eerst maken een virtuele-machineschaalset en gaat u verder met de volgende sectie voor meer informatie over het inschakelen van beheerde identiteit op een virtuele-machineschaalset door het systeem toegewezen:

- [Een virtuele-Machineschaalset maken in Azure portal](../../virtual-machine-scale-sets/quick-create-portal.md)  

### <a name="enable-system-assigned-managed-identity-on-an-existing-virtual-machine-scale-set"></a>De beheerde inschakelen door het systeem toegewezen identiteit op een bestaande virtuele-machineschaalset

De door het systeem toegewezen beheerde identiteit op een virtuele-machineschaalset die oorspronkelijk is ingericht zonder inschakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Onder **systeem toegewezen**, **Status**, selecteer **op** en klik vervolgens op **opslaan**:

   ![Schermafbeelding van de pagina configuratie](./media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](./media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

### <a name="remove-system-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Verwijder de door het systeem toegewezen beheerde identiteit van een virtuele-machineschaalset

Als u een virtuele-machineschaalset die een door het systeem toegewezen beheerde identiteit niet meer nodig hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Onder **systeem toegewezen**, **Status**, selecteer **uit** en klik vervolgens op **opslaan**:

   ![Schermafbeelding van de pagina configuratie](./media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit van een VM-schaalset met behulp van de Azure-portal.

### <a name="assign-a-user-assigned-managed-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen tijdens het maken van een virtuele-machineschaalset

De Azure-portal biedt momenteel geen ondersteuning voor het toewijzen van een beheerde identiteit voor de gebruiker toegewezen tijdens het maken van een virtuele-machineschaalset. In plaats daarvan raadpleegt u het volgende virtual machine scale set maken voor het eerst maken een virtuele-machineschaalset en gaat u verder met de volgende sectie voor meer informatie over het toewijzen van een beheerde identiteit voor de gebruiker toegewezen aan het artikel:

- [Een virtuele-Machineschaalset maken in Azure portal](../../virtual-machine-scale-sets/quick-create-portal.md)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande virtuele-machineschaalset

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.
2. Navigeer naar de gewenste virtuele-machineschaalset en klik op **identiteit**, **gebruiker toegewezen** en vervolgens  **\+toevoegen**.

   ![De gebruiker toegewezen identiteit toevoegen aan VMSS](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vmss-screenshot1.png)

3. Klik op de gebruiker toegewezen identiteit u wilt toevoegen aan de virtuele-machineschaalset en klik vervolgens op **toevoegen**.
   
   ![De gebruiker toegewezen identiteit toevoegen aan VMSS](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-virtual-machine-scale-set"></a>Een gebruiker toegewezen beheerde identiteit van een virtuele-machineschaalset verwijderen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.
2. Navigeer naar de gewenste virtuele-machineschaalset en klik op **identiteit**, **gebruiker toegewezen**, de naam van de gebruiker toegewezen identiteit die u wilt verwijderen en klik vervolgens op beheerde **verwijderen** (Klik op **Ja** in het deelvenster bevestiging).

   ![Verwijder de gebruiker toegewezen identiteit van een VMSS](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vmss-screenshot.png)


## <a name="next-steps"></a>Volgende stappen

- Met behulp van Azure portal, krijgt een Azure virtuele-machineschaalset beheerde identiteit [toegang tot een andere Azure-resource](howto-assign-access-portal.md).



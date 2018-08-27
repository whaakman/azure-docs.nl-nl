---
title: Beheerde Service-identiteit op een virtuele Azure-machine met behulp van de Azure-portal configureren
description: Voor stap door stap instructies voor het configureren van een beheerde Service-identiteit op een Azure-VM met behulp van de Azure portal.
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
ms.date: 09/19/2017
ms.author: daveba
ms.openlocfilehash: 552fce2ffd8b6bd786010da82e702ee98c3f8647
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888553"
---
# <a name="configure-a-vm-managed-service-identity-using-the-azure-portal"></a>Een virtuele machine beheerde Service-identiteit met Azure portal configureren

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u inschakelen en uitschakelen van systeem en door gebruiker toegewezen identiteit voor een Azure Virtual Machine (VM), met behulp van de Azure portal. 

## <a name="prerequisites"></a>Vereiste onderdelen

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md).
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de roltoewijzing van de volgende:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) inschakelen en verwijder de systeem toegewezen identiteit van een Azure-VM.

## <a name="system-assigned-identity"></a>Systeem toegewezen identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van het systeem toegewezen identiteit voor de virtuele machine met behulp van de Azure portal.

### <a name="enable-system-assigned-identity-during-creation-of-a-vm"></a>Systeem toegewezen identiteit tijdens het maken van een virtuele machine inschakelen

De Azure-portal biedt momenteel geen ondersteuning voor inschakelen systeem toegewezen identiteit tijdens het maken van een virtuele machine. In plaats daarvan verwijzen naar een van de volgende artikelen maken Quick Start VM eerst een virtuele machine maken en ga daarna verder met de volgende sectie voor meer informatie over het inschakelen van systeem toegewezen identiteit op de virtuele machine:

- [Een Windows-machine maken met de Azure-portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele Linux-machine maken met de Azure portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

### <a name="enable-system-assigned-identity-on-an-existing-vm"></a>Systeem toegewezen identiteit op een bestaande virtuele machine inschakelen

Het systeem toegewezen identiteit op een virtuele machine die oorspronkelijk is ingericht zonder inschakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

2. Navigeer naar de gewenste virtuele Machine en selecteer **identiteit**.

3. Onder **systeem toegewezen**, **Status**, selecteer **op** en klik vervolgens op **opslaan**:

   ![Schermafbeelding van de pagina configuratie](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-identity-from-a-vm"></a>Systeem toegewezen identiteit van een virtuele machine verwijderen

Als u een virtuele Machine die het systeem toegewezen identiteit niet meer nodig hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. 

2. Navigeer naar de gewenste virtuele Machine en selecteer **identiteit**.

3. Onder **systeem toegewezen**, **Status**, selecteer **uit** en klik vervolgens op **opslaan**:

   ![Schermafbeelding van de pagina configuratie](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-identity"></a>Door gebruiker toegewezen identiteit

 In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen identiteit van een virtuele machine met behulp van de Azure portal.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Een gebruiker toegewezen identiteit tijdens het maken van een virtuele machine toewijzen

De Azure-portal biedt op dit moment geen ondersteuning voor het toewijzen van een gebruiker toegewezen identiteit tijdens het maken van een virtuele machine. In plaats daarvan verwijzen naar een van de volgende artikelen maken Quick Start VM eerst een virtuele machine maken en ga daarna verder met de volgende sectie voor meer informatie over het toewijzen van een gebruiker toegewezen identiteit voor de virtuele machine:

- [Een Windows-machine maken met de Azure-portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele Linux-machine maken met de Azure portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-identity-to-an-existing-vm"></a>Een gebruiker toegewezen identiteit aan een bestaande virtuele machine toewijzen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.
2. Navigeer naar de gewenste virtuele machine en klik op **identiteit**, **gebruiker toegewezen** en vervolgens  **\+toevoegen**.

   ![Door gebruiker toegewezen identiteit toevoegen aan virtuele machine](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. Klik op de gebruiker toegewezen identiteit die u wilt toevoegen aan de virtuele machine en klik vervolgens op **toevoegen**.

    ![Door gebruiker toegewezen identiteit toevoegen aan virtuele machine](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-vm"></a>Een gebruiker toegewezen identiteit van een virtuele machine verwijderen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.
2. Navigeer naar de gewenste virtuele machine en klik op **identiteit**, **gebruiker toegewezen**, de naam van de identiteit van de gebruiker toegewezen die u wilt verwijderen en klik vervolgens op **verwijderen** (Klik **Ja** in het deelvenster bevestiging).

   ![Door gebruiker toegewezen identiteit van een virtuele machine verwijderen](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van de beheerde Service-identiteit, [overzicht](overview.md).

## <a name="next-steps"></a>Volgende stappen

- Met behulp van de Azure portal een Azure-VM beheerde Service-identiteit geven [toegang tot een andere Azure-resource](howto-assign-access-portal.md).


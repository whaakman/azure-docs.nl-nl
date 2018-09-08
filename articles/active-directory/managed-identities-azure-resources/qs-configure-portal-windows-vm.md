---
title: Het configureren van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van de Azure portal
description: Stap voor stap beheerde instructies voor het configureren van identiteiten voor een Azure-resources op een Azure-VM met behulp van de Azure portal.
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
ms.openlocfilehash: d1294f0e500bd3403e02fbfd6845629ff0929ee0
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158855"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-vm-using-the-azure-portal"></a>Configureren van beheerde identiteiten voor Azure-resources op een virtuele machine met behulp van de Azure portal

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u inschakelen en uitschakelen van systeem en de gebruiker toegewezen beheerde identiteiten voor een Azure Virtual Machine (VM), met behulp van de Azure portal. 

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md).
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de roltoewijzing van de volgende:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) inschakelen en verwijder de door het systeem toegewezen beheerde identiteit van een Azure-VM.

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van de door het systeem toegewezen beheerde identiteit voor de virtuele machine met behulp van de Azure portal.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-vm"></a>Beheerde identiteit systeem toegewezen tijdens het maken van een virtuele machine inschakelen

De Azure-portal biedt momenteel geen ondersteuning door het systeem toegewezen identiteit inschakelen tijdens het maken van een virtuele machine. In plaats daarvan verwijzen naar een van de volgende artikelen maken Quick Start VM eerst een virtuele machine maken en ga daarna verder met de volgende sectie voor meer informatie over het inschakelen van het systeem toegewezen identiteit op de virtuele machine:

- [Een Windows-machine maken met de Azure-portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele Linux-machine maken met de Azure portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

### <a name="enable-system-assigned-managed-identity-on-an-existing-vm"></a>Beheerde identiteit systeem toegewezen op een bestaande virtuele machine inschakelen

De door het systeem toegewezen beheerde identiteit op een virtuele machine die oorspronkelijk is ingericht zonder inschakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

2. Navigeer naar de gewenste virtuele Machine en selecteer **identiteit**.

3. Onder **systeem toegewezen**, **Status**, selecteer **op** en klik vervolgens op **opslaan**:

   ![Schermafbeelding van de pagina configuratie](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-managed-identity-from-a-vm"></a>Verwijder de door het systeem toegewezen beheerde identiteit van een virtuele machine

Als u een virtuele Machine die beheerde identiteit systeem toegewezen niet meer nodig hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. 

2. Navigeer naar de gewenste virtuele Machine en selecteer **identiteit**.

3. Onder **systeem toegewezen**, **Status**, selecteer **uit** en klik vervolgens op **opslaan**:

   ![Schermafbeelding van de pagina configuratie](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

 In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit van een virtuele machine met behulp van de Azure portal.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Een gebruiker toegewezen identiteit toewijzen tijdens het maken van een virtuele machine

De Azure-portal biedt momenteel geen ondersteuning voor het toewijzen van een beheerde identiteit voor de gebruiker toegewezen tijdens het maken van een virtuele machine. In plaats daarvan verwijzen naar een van de volgende artikelen maken Quick Start VM eerst een virtuele machine maken en ga daarna verder met de volgende sectie voor meer informatie over het toewijzen van een beheerde identiteit voor de gebruiker toegewezen aan de virtuele machine:

- [Een Windows-machine maken met de Azure-portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele Linux-machine maken met de Azure portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande virtuele machine

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.
2. Navigeer naar de gewenste virtuele machine en klik op **identiteit**, **gebruiker toegewezen** en vervolgens  **\+toevoegen**.

   ![Beheerde identiteit aan een gebruiker zijn toegewezen aan virtuele machine toevoegen](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. Klik op de gebruiker toegewezen identiteit u wilt toevoegen aan de virtuele machine en klik vervolgens op **toevoegen**.

    ![Beheerde identiteit aan een gebruiker zijn toegewezen aan virtuele machine toevoegen](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-vm"></a>Een gebruiker toegewezen beheerde identiteit van een virtuele machine verwijderen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.
2. Navigeer naar de gewenste virtuele machine en klik op **identiteit**, **gebruiker toegewezen**, de naam van de gebruiker toegewezen identiteit die u wilt verwijderen en klik vervolgens op beheerde **verwijderen** (Klik op  **Ja** in het deelvenster bevestiging).

   ![Verwijder de gebruiker toegewezen beheerde identiteit van een virtuele machine](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="next-steps"></a>Volgende stappen

- Beheerde identiteit van een Azure-VM met behulp van de Azure-portal, geven [toegang tot een andere Azure-resource](howto-assign-access-portal.md).


---
title: Het configureren van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van de Azure portal
description: Stap voor stap beheerde instructies voor het configureren van identiteiten voor een Azure-resources op een Azure-VM met behulp van de Azure portal.
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
ms.date: 11/10/2018
ms.author: priyamo
ms.collection: M365-identity-device-management
ms.openlocfilehash: be360225de7fd4484b3d6b41480b73d93f12c4a6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56209639"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-vm-using-the-azure-portal"></a>Configureren van beheerde identiteiten voor Azure-resources op een virtuele machine met behulp van de Azure portal

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u inschakelen en uitschakelen van systeem en de gebruiker toegewezen beheerde identiteiten voor een Azure Virtual Machine (VM), met behulp van de Azure portal. 

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md).
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.

## <a name="system-assigned-managed-identity"></a>Het systeem toegewezen beheerde identiteit

In deze sectie leert u hoe u inschakelen en uitschakelen van de door het systeem toegewezen beheerde identiteit voor de virtuele machine met behulp van de Azure portal.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-vm"></a>Beheerde identiteit systeem toegewezen tijdens het maken van een virtuele machine inschakelen

Om in te schakelen door het systeem toegewezen beheerde identiteit op een virtuele machine tijdens het maken, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing.  Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

- Onder de **Management** tabblad de **identiteit** sectie, schakelt u **beheerde service-identiteit** naar **op**.  

![Het systeem toegewezen identiteit inschakelen tijdens het maken van virtuele machine](./media/msi-qs-configure-portal-windows-vm/enable-system-assigned-identity-vm-creation.png)

Raadpleeg de volgende Quick starts om een virtuele machine maken: 

- [Een Windows-machine maken met de Azure-portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine) 
- [Een virtuele Linux-machine maken met de Azure portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)


### <a name="enable-system-assigned-managed-identity-on-an-existing-vm"></a>Beheerde identiteit systeem toegewezen op een bestaande virtuele machine inschakelen

Om in te schakelen door het systeem toegewezen beheerde identiteit op een virtuele machine die oorspronkelijk is ingericht zonder, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing.  Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.

2. Navigeer naar de gewenste virtuele Machine en selecteer **identiteit**.

3. Onder **systeem toegewezen**, **Status**, selecteer **op** en klik vervolgens op **opslaan**:

   ![Schermafbeelding van de pagina configuratie](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-managed-identity-from-a-vm"></a>Verwijder de door het systeem toegewezen beheerde identiteit van een virtuele machine

Als u wilt verwijderen door het systeem toegewezen beheerde identiteit van een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing.  Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

Als u een virtuele Machine die beheerde identiteit systeem toegewezen niet meer nodig hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. 

2. Navigeer naar de gewenste virtuele Machine en selecteer **identiteit**.

3. Onder **systeem toegewezen**, **Status**, selecteer **uit** en klik vervolgens op **opslaan**:

   ![Schermafbeelding van de pagina configuratie](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-managed-identity"></a>de gebruiker toegewezen beheerde identiteit

 In deze sectie leert u hoe u toevoegen en verwijderen van een gebruiker toegewezen beheerde identiteit van een virtuele machine met behulp van de Azure portal.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Een gebruiker toegewezen identiteit toewijzen tijdens het maken van een virtuele machine

Als u wilt een gebruiker toegewezen identiteit toewijzen aan een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) en [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) roltoewijzingen. Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

De Azure-portal biedt momenteel geen ondersteuning voor het toewijzen van een beheerde identiteit voor de gebruiker toegewezen tijdens het maken van een virtuele machine. In plaats daarvan verwijzen naar een van de volgende artikelen maken Quick Start VM eerst een virtuele machine maken en ga daarna verder met de volgende sectie voor meer informatie over het toewijzen van een beheerde identiteit voor de gebruiker toegewezen aan de virtuele machine:

- [Een Windows-machine maken met de Azure-portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele Linux-machine maken met de Azure portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-vm"></a>Een gebruiker toegewezen beheerde identiteit toewijzen aan een bestaande virtuele machine

Als u wilt een gebruiker toegewezen identiteit toewijzen aan een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) en [beheerde identiteit Operator](/azure/role-based-access-control/built-in-roles#managed-identity-operator) roltoewijzingen. Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.
2. Navigeer naar de gewenste virtuele machine en klik op **identiteit**, **gebruiker toegewezen** en vervolgens  **\+toevoegen**.

   ![Beheerde identiteit aan een gebruiker zijn toegewezen aan virtuele machine toevoegen](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. Klik op de gebruiker toegewezen identiteit u wilt toevoegen aan de virtuele machine en klik vervolgens op **toevoegen**.

    ![Beheerde identiteit aan een gebruiker zijn toegewezen aan virtuele machine toevoegen](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-vm"></a>Een gebruiker toegewezen beheerde identiteit van een virtuele machine verwijderen

Als u wilt verwijderen van een gebruiker toegewezen identiteit van een virtuele machine, uw account moet de [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) roltoewijzing. Er zijn geen extra Azure AD directory-roltoewijzingen zijn vereist.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine.
2. Navigeer naar de gewenste virtuele machine en klik op **identiteit**, **gebruiker toegewezen**, de naam van de gebruiker toegewezen identiteit die u wilt verwijderen en klik vervolgens op beheerde **verwijderen** (Klik op  **Ja** in het deelvenster bevestiging).

   ![Verwijder de gebruiker toegewezen beheerde identiteit van een virtuele machine](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="next-steps"></a>Volgende stappen

- Beheerde identiteit van een Azure-VM met behulp van de Azure-portal, geven [toegang tot een andere Azure-resource](howto-assign-access-portal.md).


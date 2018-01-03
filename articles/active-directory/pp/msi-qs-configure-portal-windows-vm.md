---
title: Het configureren van MSI op een virtuele machine in Azure met Azure portal
description: Stap door stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een virtuele machine van Azure, met de Azure portal.
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d112e75576d76523867f1ec48c1da63227c7fa85
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Een VM beheerde Service identiteit (MSI) met de Azure portal configureren

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u het inschakelen en het verwijderen van MSI voor een virtuele machine van Azure, met de Azure portal.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>MSI bij het maken van een virtuele machine van Azure inschakelen

Vanaf het moment van schrijven van dit wordt MSI inschakelen tijdens het maken van een virtuele machine in de Azure-portal niet ondersteund. In plaats daarvan moet verwijzen naar een van de volgende artikelen maken Quick Start VM naar een virtuele machine eerst te maken:

- [Een virtuele Windows-machine maken met de Azure portal](~/articles/virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele Linux-machine maken met de Azure portal](~/articles/virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Gaat u verder met de volgende sectie voor meer informatie over het inschakelen van MSI op de virtuele machine.

## <a name="enable-msi-on-an-existing-azure-vm"></a>MSI op een bestaande virtuele machine van Azure inschakelen

Als u een virtuele machine die oorspronkelijk is geleverd zonder een MSI-bestand hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor'.

2. Navigeer naar de gewenste virtuele Machine.

2. Klik op de pagina "Configuratie", MSI op de virtuele machine inschakelen als u 'Ja' selecteert onder 'Identiteit beheerde service' en klik op **opslaan**. Deze bewerking duurt 60 seconden of langer om te voltooien:

   ![Schermopname van configuratie-pagina](~/articles/active-directory/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-vm"></a>Verwijder de MSI van een Azure VM

Als u een virtuele Machine die niet langer een MSI-bestand hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor'.

2. Navigeer naar de gewenste virtuele Machine.

3. Klik op de pagina "Configuratie", verwijder de MSI van de VM door als u op 'Nee' onder 'Identiteit beheerde service' en klik op **opslaan**. Deze bewerking duurt 60 seconden of langer om te voltooien:

   ![Schermopname van configuratie-pagina](~/articles/active-directory/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).

## <a name="next-steps"></a>Volgende stappen

- Met de Azure portal een Azure VM MSI geven [toegang tot een andere Azure-resource](msi-howto-assign-access-portal.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

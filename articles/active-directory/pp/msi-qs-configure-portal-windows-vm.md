---
title: MSI-bestand op een virtuele Azure-machine met behulp van de Azure-portal configureren
description: Voor stap door stap instructies voor het configureren van een Managed Service Identity (MSI) op een Azure-VM, met behulp van de Azure portal.
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
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 37710015904c8112e5d2de504ed5b42895ffb809
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610311"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal configureren

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u kunt inschakelen en verwijderen van MSI-bestand voor een Azure-VM met behulp van de Azure portal.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Inschakelen van MSI tijdens het maken van een Azure VM

Vanaf het moment van schrijven, wordt het inschakelen van MSI tijdens het maken van een virtuele machine in Azure portal niet ondersteund. In plaats daarvan ziet u een van de volgende artikelen maken Quick Start VM eerst een virtuele machine te maken:

- [Een Windows-machine maken met de Azure-portal](~/articles/virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele Linux-machine maken met de Azure portal](~/articles/virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Vervolgens gaat u verder met de volgende sectie voor meer informatie over het inschakelen van MSI op de virtuele machine.

## <a name="enable-msi-on-an-existing-azure-vm"></a>MSI-bestand op een bestaande VM in Azure inschakelen

Als u een virtuele machine die oorspronkelijk is ingericht zonder een MSI-bestand hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines'.

2. Navigeer naar de gewenste virtuele Machine.

2. Klik op de pagina "Configuratie", schakelt de MSI-bestand op de virtuele machine door 'Ja' onder 'Beheerde service-identiteit' en klik op **opslaan**. Met deze bewerking duurt 60 seconden of langer om uit te voeren:

   ![Schermafbeelding van de pagina configuratie](~/articles/active-directory/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-msi-from-an-azure-vm"></a>MSI-bestand verwijderen uit een Azure-VM

Als u een virtuele Machine die een MSI-bestand niet meer nodig hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines'.

2. Navigeer naar de gewenste virtuele Machine.

3. Klik op de pagina "Configuratie", MSI-bestand van de virtuele machine verwijderen door te selecteren 'Nee' onder 'Beheerde service-identiteit' en klik op **opslaan**. Met deze bewerking duurt 60 seconden of langer om uit te voeren:

   ![Schermafbeelding van de pagina configuratie](~/articles/active-directory/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](msi-overview.md).

## <a name="next-steps"></a>Volgende stappen

- Met behulp van de Azure portal een Azure-VM-MSI geven [toegang tot een andere Azure-resource](msi-howto-assign-access-portal.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.

---
title: MSI configureren op een schaalset van de virtuele machine van Azure met behulp van de Azure-portal
description: Voor stap door stap instructies voor het configureren van een Managed Service Identity (MSI) voor Azure VMSS, met behulp van de Azure portal.
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
ms.openlocfilehash: 3421d0ab9007bdfe9fab093871a8517531059008
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213170"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-the-azure-portal"></a>Een virtuele machine configureren Virtual Machine scale sets Managed Service Identity (MSI) met behulp van de Azure portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u kunt in- en uitschakelen van het systeem toegewezen identiteit voor een virtuele-machineschaalset met behulp van de Azure portal. Toe te wijzen en een gebruiker toegewezen identiteiten verwijderen uit een schaalset voor virtuele machine van Azure is momenteel niet ondersteund via Azure portal.

> [!NOTE]
> Door gebruiker toegewezen identiteit bewerkingen worden momenteel niet ondersteund via de Azure Portal. Controleer later op updates.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md).
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.
- Als u wilt de beheerbewerkingen in dit artikel uitvoert, moet uw account de roltoewijzing van de volgende:
    - [Inzender voor virtuele machines](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) inschakelen en het systeem toegewezen beheerde identiteit van een virtuele-machineschaalset verwijderen.

## <a name="managed-service-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Beheerde Service-identiteit tijdens het maken van een schaalset voor virtuele Azure-machine

VM maken via Azure portal biedt momenteel geen ondersteuning voor bewerkingen van de beheerde Service-identiteit. In plaats daarvan Raadpleeg de volgende virtuele machine van Azure scale set maken snelstartgids artikel eerst een schaalset voor virtuele Azure-machine maken:

- [Een virtuele-Machineschaalset maken in Azure portal](../../virtual-machine-scale-sets/quick-create-portal.md)  

Vervolgens gaat u verder met de volgende sectie voor meer informatie over het inschakelen van MSI op de virtuele-machineschaalset.

## <a name="enable-managed-service-identity-on-an-existing-azure-vmms"></a>Beheerde Service-identiteit op een bestaande Azure-VMMS inschakelen

Het systeem toegewezen identiteit op een virtuele machine die oorspronkelijk is ingericht zonder inschakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Schakelt het systeem toegewezen identiteit op de virtuele machine door 'Ja' onder 'Beheerde service-identiteit' en klik vervolgens op **opslaan**. Met deze bewerking duurt 60 seconden of langer om uit te voeren:

   [![Schermafbeelding van de pagina configuratie](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

## <a name="remove-managed-service-identity-from-an-azure-virtual-machine-scale-set"></a>Beheerde Service-identiteit verwijderen uit een schaalset voor virtuele Azure-machine

Als u een virtuele-machineschaalset die een MSI-bestand niet meer nodig hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele-machineschaalset. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele-machineschaalset.

2. Navigeer naar de gewenste virtuele-machineschaalset.

3. Het systeem toegewezen identiteit op de virtuele machine selecteert u 'Nee' onder "Beheerde service-identiteit" uitschakelen en klik op opslaan. Met deze bewerking duurt 60 seconden of langer om uit te voeren:

   ![Schermafbeelding van de pagina configuratie](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van de beheerde Service-identiteit, [overzicht](overview.md).

## <a name="next-steps"></a>Volgende stappen

- Met behulp van Azure portal, Geef een virtuele machine van Azure Virtual Machine scale sets MSI [toegang tot een andere Azure-resource](howto-assign-access-portal.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.

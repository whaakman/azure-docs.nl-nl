---
title: MSI-bestand op een virtuele Azure-machine met behulp van de Azure-portal configureren
description: Voor stap door stap instructies voor het configureren van een Managed Service Identity (MSI) op een Azure-VM, met behulp van de Azure portal.
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
ms.openlocfilehash: 27ecb00bddb41ae45e790a54702c058ff3f1d24b
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035938"
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Een virtuele machine Managed Service Identity (MSI) met behulp van de Azure-portal configureren

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u kunt in- en uitschakelen van het systeem toegewezen identiteit voor een Azure-VM met behulp van de Azure portal. Toe te wijzen en het verwijderen van gebruiker toegewezen identiteiten van Azure VM's is momenteel niet ondersteund via de Azure Portal.

> [!NOTE]
> Door gebruiker toegewezen identiteit bewerkingen worden momenteel niet ondersteund via de Azure Portal. Kijk binnenkort voor updates. 

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde Service-identiteit bent, bekijk dan de [overzichtssectie](overview.md).
- Als u nog een Azure-account hebt [zich registreren voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.

## <a name="managed-service-identity-during-creation-of-an-azure-vm"></a>Beheerde Service-identiteit tijdens het maken van een Azure VM

VM maken via Azure portal biedt momenteel geen ondersteuning voor bewerkingen van de beheerde Service-identiteit. In plaats daarvan ziet u een van de volgende artikelen maken Quick Start VM eerst een virtuele machine te maken:

- [Een Windows-machine maken met de Azure-portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele Linux-machine maken met de Azure portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Vervolgens gaat u verder met de volgende sectie voor meer informatie over het inschakelen van de beheerde Service-identiteit op de virtuele machine.

## <a name="enable-managed-service-identity-on-an-existing-azure-vm"></a>Beheerde Service-identiteit van een bestaande VM in Azure inschakelen

Het systeem toegewezen identiteit op een virtuele machine die oorspronkelijk is ingericht zonder inschakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines'.

2. Navigeer naar de gewenste virtuele Machine en selecteert u de pagina "Configuratie".

3. Schakelt het systeem toegewezen identiteit op de virtuele machine door 'Ja' onder 'Beheerde service-identiteit' en klik vervolgens op **opslaan**. Met deze bewerking duurt 60 seconden of langer om uit te voeren:

    > [!NOTE]
    > Een gebruiker toegewezen identiteit toe te voegen aan een virtuele machine wordt momenteel niet ondersteund via de Azure Portal.

   ![Schermafbeelding van de pagina configuratie](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-managed-service-identity-from-an-azure-vm"></a>Verwijder de identiteit van de beheerde Service van een Azure-VM

Als u een virtuele Machine die het systeem toegewezen identiteit niet meer nodig hebt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement met de virtuele machine. Ook voor zorgen dat uw account deel uitmaakt van een rol waarmee u beschikt over machtigingen voor schrijven op de virtuele machine, zoals 'Inzender voor virtuele machines'.

2. Navigeer naar de gewenste virtuele Machine en selecteert u de pagina "Configuratie".

3. Het systeem toegewezen identiteit op de virtuele machine selecteert u 'Nee' onder "Beheerde service-identiteit" uitschakelen en klik op opslaan. Met deze bewerking duurt 60 seconden of langer om uit te voeren:

    > [!NOTE]
    > Een gebruiker toegewezen identiteit toe te voegen aan een virtuele machine wordt momenteel niet ondersteund via de Azure Portal.

   ![Schermafbeelding van de pagina configuratie](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van de beheerde Service-identiteit, [overzicht](overview.md).

## <a name="next-steps"></a>Volgende stappen

- Met behulp van de Azure portal een Azure-VM-MSI geven [toegang tot een andere Azure-resource](howto-assign-access-portal.md).


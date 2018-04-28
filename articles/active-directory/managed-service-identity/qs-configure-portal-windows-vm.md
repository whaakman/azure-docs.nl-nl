---
title: Het configureren van MSI op een virtuele machine in Azure met Azure portal
description: Stap door stapsgewijze instructies voor het configureren van een beheerde Service identiteit (MSI) op een virtuele machine van Azure, met de Azure portal.
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
ms.date: 09/19/2017
ms.author: daveba
ms.openlocfilehash: 91b4d50bb72e438c677b8b83e292998cce26415c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="configure-a-vm-managed-service-identity-msi-using-the-azure-portal"></a>Een VM beheerde Service identiteit (MSI) met de Azure portal configureren

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde Service-identiteit biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om alle services die Azure AD-verificatie ondersteunt, zonder referenties in uw code te verifiëren. 

In dit artikel leert u hoe inschakelen en uitschakelen van het systeem toegewezen identiteit voor een virtuele machine van Azure, met de Azure portal. Toe te wijzen en verwijderen van de gebruiker toegewezen identiteiten van Azure VM's is momenteel niet ondersteund via Azure Portal.

> [!NOTE]
> Gebruiker toegewezen identiteit bewerkingen worden op dit moment niet ondersteund via Azure Portal. Controleer regelmatig op updates. 

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met de Service-identiteit beheerd bent, bekijk de [overzichtssectie van](overview.md).
- Als u al een Azure-account niet hebt [aanmelden voor een gratis account](https://azure.microsoft.com/free/) voordat u doorgaat.

## <a name="managed-service-identity-during-creation-of-an-azure-vm"></a>Beheerde Service-identiteit bij het maken van een Azure VM

Maken van VM's via Azure portal ondersteunt op dit moment niet beheerd Service-identiteit bewerkingen. In plaats daarvan moet verwijzen naar een van de volgende artikelen maken Quick Start VM naar een virtuele machine eerst te maken:

- [Een virtuele Windows-machine maken met de Azure portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele Linux-machine maken met de Azure portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

Gaat u verder met de volgende sectie voor meer informatie over het inschakelen van de Service-identiteit beheerd op de virtuele machine.

## <a name="enable-managed-service-identity-on-an-existing-azure-vm"></a>Service-identiteit beheerd op een bestaande virtuele machine van Azure inschakelen

Het systeem toegewezen identiteit op een virtuele machine die oorspronkelijk was ingericht zonder deze inschakelen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor'.

2. Navigeer naar de gewenste virtuele Machine en selecteer de pagina "Configuratie".

3. Inschakelen van de identiteit van de automatisch toegewezen op de virtuele machine door het selecteren van 'Ja' onder 'Identiteit beheerde service' en klik vervolgens op **opslaan**. Deze bewerking duurt 60 seconden of langer om te voltooien:

    > [!NOTE]
    > Een toegewezen gebruikers-id toe te voegen aan een virtuele machine is momenteel niet ondersteund via Azure Portal.

   ![Schermopname van configuratie-pagina](../media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

## <a name="remove-managed-service-identity-from-an-azure-vm"></a>Verwijder de beheerde Service-identiteit van een Azure VM

Als u een virtuele Machine die niet langer nodig de systeemidentiteit toegewezen hebt heeft:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement met de virtuele machine. Controleer ook of uw account behoort tot een functie waarmee u schrijfmachtigingen heeft op de virtuele machine, zoals 'Virtual Machine Contributor'.

2. Navigeer naar de gewenste virtuele Machine en selecteer de pagina "Configuratie".

3. Uitschakelen van het systeem toegewezen identiteit op de virtuele machine door als u op 'Nee' onder 'Identiteit beheerde service' en klik op opslaan. Deze bewerking duurt 60 seconden of langer om te voltooien:

    > [!NOTE]
    > Een toegewezen gebruikers-id toe te voegen aan een virtuele machine is momenteel niet ondersteund via Azure Portal.

   ![Schermopname van configuratie-pagina](../media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)  

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van de Service-identiteit beheerd [overzicht](overview.md).

## <a name="next-steps"></a>Volgende stappen

- Met de Azure portal een Azure VM MSI geven [toegang tot een andere Azure-resource](howto-assign-access-portal.md).


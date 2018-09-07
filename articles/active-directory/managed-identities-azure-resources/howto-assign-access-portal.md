---
title: Een MSI toegang toewijzen aan een Azure-resource, met behulp van de Azure portal
description: Stapsgewijze instructies voor het toewijzen van een MSI-bestand op een resource toegang naar een andere resource met behulp van de Azure-portal.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: b6423a80b0405bcaabd4f1fc8e26dc3be0adadb0
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028159"
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Een beheerde Service-identiteit toegang tot een resource toewijzen met behulp van Azure portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Nadat u hebt een Azure-resource geconfigureerd met een Managed Service Identity (MSI), kunt u de MSI toegang verlenen naar een andere resource, net als bij elke beveiligings-principal. Dit artikel ziet u hoe u een virtuele Azure-machine of VM-schaalset van MSI toegang geven tot een Azure storage-account met behulp van de Azure-portal.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>RBAC gebruiken voor het toewijzen van de toegang tot de MSI-bestand naar een andere resource

Nadat u hebt MSI ingeschakeld op een Azure-resource, zoals een [virtuele Azure-machine](qs-configure-portal-windows-vm.md) of [Azure VMSS](qs-configure-portal-windows-vmss.md):

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement waarin u het MSI-bestand hebt geconfigureerd.

2. Navigeer naar de gewenste resource waarop u wilt wijzigen van toegangsbeheer. In dit voorbeeld zijn we een virtuele machine van Azure waardoor en Azure virtuele-machineschaalset toegang tot een opslagaccount, zodat we gaat u naar het opslagaccount.

3. Selecteer voor een Azure-machine, de **toegangsbeheer (IAM)** pagina van de resource en selecteer **+ toevoegen**. Geef vervolgens de **rol**, **toegang toewijzen aan virtuele Machine**, en geeft u de bijbehorende **abonnement** en **resourcegroep** waarin de resource zich bevindt. Onder het zoekgebied criteria ziet u de resource. Selecteer de resource en selecteer **opslaan**. 

   ![Schermafbeelding van de Access control (IAM)](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
   Selecteer voor een schaalset voor virtuele Azure-machine, de **toegangsbeheer (IAM)** pagina van de resource en selecteer **+ toevoegen**. Geef vervolgens de **rol**, **toegang toewijzen aan**. Zoeken in het gebied van de criteria zoeken naar uw virtuele-machineschaalset. Selecteer de resource en selecteer **opslaan**.
   
   ![Schermafbeelding van de Access control (IAM)](./media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-before.png)  

4. U keert terug naar de hoofdpagina **toegangsbeheer (IAM)** pagina, waar u een nieuwe vermelding voor de resource-MSI zien.

    Virtuele machine van Azure:

   ![Schermafbeelding van de Access control (IAM)](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

    Azure VM-schaalset:

    ![Schermafbeelding van de Access control (IAM)](./media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-after.png)

## <a name="troubleshooting"></a>Problemen oplossen

Als het MSI-bestand voor de resource niet in de lijst met beschikbare identiteiten weergegeven wordt, controleert u of het MSI-bestand correct is ingeschakeld. In ons geval kunnen we gaat u terug naar de virtuele machine van Azure en controleert u het volgende:

- Bekijk de **configuratie** pagina en zorg ervoor dat de waarde voor **ingeschakelde MSI** is **Ja**.
- Bekijk de **extensies** pagina en zorg ervoor dat de MSI-extensie geïmplementeerd (**extensies** pagina is niet beschikbaar voor een schaalset voor virtuele machine van Azure).

Als een onjuist is, moet u mogelijk het MSI-bestand voor uw resource opnieuw implementeren of problemen met de implementatie mislukt.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](overview.md).
- Zie voor meer informatie over het inschakelen van MSI-bestand op een Azure-machine [configureren van een Azure VM Managed Service Identity (MSI) met behulp van de Azure-portal](qs-configure-portal-windows-vm.md).
- Zie voor meer informatie over het inschakelen van MSI in een schaalset voor virtuele Azure-machine [configureren van een Azure Virtual Machine Scale ingesteld Managed Service Identity (MSI) met behulp van de Azure portal](qs-configure-portal-windows-vmss.md)



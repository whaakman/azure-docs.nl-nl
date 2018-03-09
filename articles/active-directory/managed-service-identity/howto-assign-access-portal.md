---
title: Een MSI-toegang toewijzen aan een Azure-resource met de Azure portal
description: Stapsgewijze instructies voor het toewijzen van een MSI-bestand op een toegang tot bronnen naar een andere bron met behulp van de Azure-portal.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 9120104955aac8ca8a0568e4519c99e1bd786541
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Een beheerde Service-identiteit toegang toewijzen aan een resource met behulp van de Azure-portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Nadat u hebt een Azure-resource met een beheerde Service identiteit (MSI) geconfigureerd, kunt u de MSI-toegang geven tot een andere resource, net als elke beveiligings-principal. In dit artikel laat zien hoe Azure een virtuele machine of virtuele-machineschaalset van MSI toegang geven tot een Azure storage-account met behulp van de Azure-portal.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Gebruikmaken van RBAC de MSI-toegang toewijzen aan een andere bron

Nadat u hebt MSI ingeschakeld op een Azure-resource, zoals een [Azure VM](qs-configure-portal-windows-vm.md) of [Azure VMSS](qs-configure-portal-windows-vmss.md):

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement waaronder u het MSI-bestand hebt geconfigureerd.

2. Navigeer naar de gewenste resource waarop u wilt wijzigen van toegangsbeheer. In dit voorbeeld geeft we Azure een virtuele machine en schaal van de virtuele machine van Azure toegang tot een opslagaccount instellen zodat we gaat u naar het opslagaccount.

3. Voor een virtuele machine van Azure, selecteert u de **toegangsbeheer (IAM)** pagina van de resource en selecteer **+ toevoegen**. Geef vervolgens de **rol**, **toegang toewijzen aan de virtuele Machine**, en geeft u de bijbehorende **abonnement** en **resourcegroep** Wanneer de bron zich bevindt. U ziet onder het gebied van de criteria zoeken de resource. Selecteer de bron en selecteer **opslaan**. 

   ![Access control (IAM)-schermafbeelding](../media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
   Voor een virtuele machine van Azure scale ingesteld, selecteert u de **toegangsbeheer (IAM)** pagina van de resource en selecteer **+ toevoegen**. Geef vervolgens de **rol**, **toewijzen van toegang tot**. Zoek uw virtuele-machineschaalset onder het gebied van de criteria zoeken. Selecteer de bron en selecteer **opslaan**.
   
   ![Access control (IAM)-schermafbeelding](../media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-before.png)  

4. U keert terug naar de belangrijkste **toegangsbeheer (IAM)** pagina waar u een nieuwe vermelding voor de resource MSI zien.

    Virtuele machine van Azure:

   ![Access control (IAM)-schermafbeelding](../media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

    Azure virtuele-machineschaalset:

    ![Access control (IAM)-schermafbeelding](../media/msi-howto-assign-access-vmss-portal/assign-access-control-vmss-iam-blade-after.png)

## <a name="troubleshooting"></a>Problemen oplossen

Als het MSI-bestand voor de resource niet in de lijst met beschikbare identiteiten weergegeven wordt, moet u controleren of het MSI-bestand juist is ingeschakeld. In ons geval kunnen we terug te keren naar de virtuele machine van Azure en controleert u het volgende:

- Bekijk de **configuratie** pagina en zorg ervoor dat de waarde voor **MSI ingeschakeld** is **Ja**.
- Bekijk de **extensies** pagina en zorg ervoor dat de MSI-extensie geïmplementeerd (**extensies** pagina is niet beschikbaar voor een Azure virtuele-machineschaalset).

Als een onjuist is, moet u mogelijk de MSI van uw resources opnieuw te implementeren of problemen met de implementatie is mislukt.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](overview.md).
- Zie voor meer informatie over het inschakelen van MSI op een virtuele machine van Azure [configureren van een Azure VM beheerde Service identiteit (MSI) met de Azure portal](qs-configure-portal-windows-vm.md).
- Zie voor meer informatie over het inschakelen van MSI op een virtuele machine van Azure scale set [een Azure virtuele Machine Scale ingesteld beheerde Service identiteit (MSI) met de Azure portal configureren](qs-configure-portal-windows-vmss.md)



---
title: Een MSI-toegang toewijzen aan een Azure-resource met de Azure portal
description: Stapsgewijze instructies voor het toewijzen van een MSI-bestand op een toegang tot bronnen naar een andere bron met behulp van de Azure-portal.
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
ms.openlocfilehash: 7d0a50db28ba3d9926f7a83fe224b7a0dbe6ed20
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Een beheerde Service-identiteit toegang toewijzen aan een resource met behulp van de Azure-portal

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Nadat u hebt een Azure-resource met een beheerde Service identiteit (MSI) geconfigureerd, kunt u de MSI-toegang geven tot een andere resource, net als elke beveiligings-principal. In dit artikel laat zien hoe Azure een virtuele machine van MSI toegang geven tot een Azure storage-account met behulp van de Azure-portal.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Gebruikmaken van RBAC de MSI-toegang toewijzen aan een andere bron

Nadat u MSI hebt ingeschakeld op een Azure-resource [zoals een Azure VM](msi-qs-configure-portal-windows-vm.md):

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account die is gekoppeld aan het Azure-abonnement waaronder u het MSI-bestand hebt geconfigureerd.

2. Navigeer naar de gewenste resource waarop u wilt wijzigen van toegangsbeheer. In dit voorbeeld we zijn een virtuele machine van Azure die toegang geeft tot een opslagaccount zodat we gaat u naar het opslagaccount.

3. Selecteer de **toegangsbeheer (IAM)** pagina van de resource en selecteer **+ toevoegen**. Geef vervolgens de **rol**, **toegang toewijzen aan de virtuele Machine**, en geeft u de bijbehorende **abonnement** en **resourcegroep** Wanneer de bron zich bevindt. U ziet onder het gebied van de criteria zoeken de resource. Selecteer de bron en selecteer **opslaan**. 

   ![Access control (IAM)-schermafbeelding](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. U keert terug naar de belangrijkste **toegangsbeheer (IAM)** pagina waar u een nieuwe vermelding voor de resource MSI zien. In dit voorbeeld wordt de 'SimpleWinVM' VM van de resourcegroep Demo heeft **Inzender** toegang tot het opslagaccount.

   ![Access control (IAM)-schermafbeelding](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Problemen oplossen

Als het MSI-bestand voor de resource niet in de lijst met beschikbare identiteiten weergegeven wordt, moet u controleren of het MSI-bestand juist is ingeschakeld. In ons geval kunnen we terug te keren naar de Azure VM en controleert u het volgende:

- Bekijk de **configuratie** pagina en zorg ervoor dat de waarde voor **MSI ingeschakeld** is **Ja**.
- Bekijk de **extensies** pagina en zorg ervoor dat de MSI-extensie die is geïmplementeerd.

Als een onjuist is, moet u mogelijk de MSI van uw resources opnieuw te implementeren of problemen met de implementatie is mislukt.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Zie voor het inschakelen van MSI op een Azure VM [configureren van een Azure VM beheerde Service identiteit (MSI) met de Azure portal](msi-qs-configure-portal-windows-vm.md).



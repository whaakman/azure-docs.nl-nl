---
title: Een MSI toegang toewijzen aan een Azure-resource, met behulp van de Azure portal
description: Stapsgewijze instructies voor het toewijzen van een MSI-bestand op een resource toegang naar een andere resource met behulp van de Azure-portal.
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
ms.openlocfilehash: 83a56793d08632918a75f6580360a9dd148d7316
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611065"
---
# <a name="assign-a-managed-service-identity-access-to-a-resource-by-using-the-azure-portal"></a>Een beheerde Service-identiteit toegang tot een resource toewijzen met behulp van Azure portal

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Nadat u hebt een Azure-resource geconfigureerd met een Managed Service Identity (MSI), kunt u de MSI toegang verlenen naar een andere resource, net als bij elke beveiligings-principal. In dit artikel wordt beschreven hoe u een virtuele machine van Azure van MSI toegang geven tot een Azure storage-account met behulp van de Azure-portal.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>RBAC gebruiken voor het toewijzen van de toegang tot de MSI-bestand naar een andere resource

Nadat u MSI-bestand hebt ingeschakeld op een Azure-resource, [, zoals een Azure-VM](msi-qs-configure-portal-windows-vm.md):

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat is gekoppeld aan het Azure-abonnement waarin u het MSI-bestand hebt geconfigureerd.

2. Navigeer naar de gewenste resource waarop u wilt wijzigen van toegangsbeheer. In dit voorbeeld zijn we een Azure VM-toegang geeft om een storage-account, zodat we gaat u naar het opslagaccount.

3. Selecteer de **toegangsbeheer (IAM)** pagina van de resource en selecteer **+ toevoegen**. Geef vervolgens de **rol**, **toegang toewijzen aan virtuele Machine**, en geeft u de bijbehorende **abonnement** en **resourcegroep** waarin de resource zich bevindt. Onder het zoekgebied criteria ziet u de resource. Selecteer de resource en selecteer **opslaan**. 

   ![Schermafbeelding van de Access control (IAM)](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  

4. U keert terug naar de hoofdpagina **toegangsbeheer (IAM)** pagina, waar u een nieuwe vermelding voor de resource-MSI zien. In dit voorbeeld wordt de virtuele machine uit de resourcegroep van de Demo "SimpleWinVM" heeft **Inzender** toegang tot het opslagaccount.

   ![Schermafbeelding van de Access control (IAM)](~/articles/active-directory/media/msi-howto-assign-access-portal/assign-access-control-iam-blade-after.png)

## <a name="troubleshooting"></a>Problemen oplossen

Als het MSI-bestand voor de resource niet in de lijst met beschikbare identiteiten weergegeven wordt, controleert u of het MSI-bestand correct is ingeschakeld. In ons geval kunnen we gaat u terug naar de Azure-VM en controleert u het volgende:

- Bekijk de **configuratie** pagina en zorg ervoor dat de waarde voor **ingeschakelde MSI** is **Ja**.
- Bekijk de **extensies** pagina en zorg ervoor dat de MSI-extensie is geïmplementeerd.

Als een onjuist is, moet u mogelijk het MSI-bestand voor uw resource opnieuw implementeren of problemen met de implementatie mislukt.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](msi-overview.md).
- Zie voor het inschakelen van MSI-bestand op een Azure VM [configureren van een Azure VM Managed Service Identity (MSI) met behulp van de Azure-portal](msi-qs-configure-portal-windows-vm.md).



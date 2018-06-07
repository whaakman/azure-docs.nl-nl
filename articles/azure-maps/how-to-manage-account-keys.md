---
title: Het beheren van uw Azure-Maps-account en de sleutels | Microsoft Docs
description: De Azure-portal kunt u uw Azure-Maps-account beheren en uw toegangssleutels beheren.
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 153fb87b0e2b576fd67a24bb833165f2a1c93c56
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599657"
---
# <a name="how-to-manage-your-azure-maps-account-and-keys"></a>Uw Azure-Maps-account en de sleutels beheren

U kunt uw Azure-Maps-account en de sleutels via de Azure portal beheren. Als u een account en een sleutel hebt, kunt u de API's implementeren in uw website of mobiele toepassing.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-new-account"></a>Een nieuw account maken

1. Meld u aan bij [Azure Portal](http://portal.azure.com).

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.

2. Zoek en selecteer **Maps**, klikt u vervolgens op **maken**.

3. Geef de informatie voor uw nieuwe account. 

![Geef de gegevens in de portal](./media/how-to-manage-account-keys/new-account-portal.png)

## <a name="manage-keys-on-the-account-page"></a>Sleutels op de accountpagina beheren

Als u een account hebt gemaakt, krijgt u twee willekeurig gegenereerde sleutels. U de sleutels worden geverifieerd bij de kaarten-API's, als u wilt ophalen kaartgegevens of maak een nieuw exemplaar van de JavaScript-kaart. 

U vindt uw sleutels in de Azure portal. Navigeer naar uw account en selecteer vervolgens **sleutels** in het menu.

![Account sleutels in de portal beheren](./media/how-to-manage-account-keys/account-keys-portal.png)

U kunt via deze pagina Kopieer uw sleutels of nieuwe te genereren. 

## <a name="delete-an-account"></a>Een account verwijderen

U kunt een account verwijderen uit de Azure-portal. Navigeer naar het Accountoverzicht en selecteer **verwijderen**.

![Uw account in de portal verwijderen](./media/how-to-manage-account-keys/account-delete-portal.png)

## <a name="next-steps"></a>Volgende stappen

Informatie over het gebruik de [toegewezen management API](https://docs.microsoft.com/rest/api/maps-management/accounts) wilt maken, bijwerken en verwijderen van Maps-accounts. 
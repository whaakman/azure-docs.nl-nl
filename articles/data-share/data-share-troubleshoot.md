---
title: Voorbeeld van de Share Azure gegevens oplossen
description: Meer informatie over het oplossen van problemen met Azure gegevens delen Preview
services: data-share
author: joannapea
ms.service: data-share
ms.topic: overview
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: c02f72d6a327c4dcb94ac8844005613cfe316986
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838386"
---
# <a name="troubleshoot-common-issues-in-azure-data-share-preview"></a>Oplossen van algemene problemen in Azure Data Share-Preview

In dit artikel laat zien hoe algemene problemen oplossen voor Azure-Gegevensvoorbeeld delen. 

## <a name="azure-data-share-invitations"></a>Azure uitnodigingen voor het delen van gegevens 

In sommige gevallen, wanneer een nieuwe gebruiker op **uitnodiging accepteren** van het e-mailuitnodiging die is verzonden, kunnen ze worden weergegeven met een lege lijst met uitnodigingen. 

![Er zijn geen uitnodigingen](media/no-invites.png)

De bovenstaande fout is een bekend probleem met de service en is momenteel gericht. Als tijdelijke oplossing, volg de onderstaande stappen. 

1. In de Azure-portal, gaat u naar **abonnementen**
1. Selecteer het abonnement dat u voor Azure het delen van gegevens
1. Klik op **Resourceproviders**
1. Zoeken naar Microsoft.DataShare
1. Klik op **registreren**

U moet beschikken over de [Azure Inzender RBAC-rol](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) om deze stappen te voltooien. 

Als u zich nog steeds niet om een uitnodiging voor delen gegevens te bekijken, neem contact op met uw gegevens en ervoor te zorgen dat zij de uitnodiging voor uw Azure-aanmelding e-mailadres hebt verzonden en *niet* uw e-mailalias. 

> [!IMPORTANT]
> Als u hebt al een Azure-Gegevensshare uitnodiging geaccepteerd en de service voor het configureren van opslag is afgesloten, volgt u de instructies die zijn uiteengezet in de [configureren van de toewijzing van een gegevensset](how-to-configure-mapping.md) handleiding voor meer informatie over hoe u klaar bent met het configureren van uw ontvangen gegevens delen en ontvangen van gegevens. 

## <a name="error-when-creating-or-receiving-a-new-data-share"></a>Fout bij het maken van of het delen van een nieuwe gegevens ontvangen

' Fout: Bewerking heeft een ongeldige statuscode 'BadRequest' geretourneerd als"

' Fout: AuthorizationFailed"

"Fout: roltoewijzing met storage-account"

![Fout: onvoldoende toegangsrechten](media/error-write-privilege.png)

Als u ontvangt een van de bovenstaande fouten bij het maken van een nieuwe gegevens delen of het delen van een nieuwe gegevens ontvangen, is omdat er onvoldoende machtigingen voor het opslagaccount. De vereiste machtiging is *Microsoft.Authorization/role toewijzingen schrijftijd*, die bestaat in de rol van de eigenaar opslag of kunnen worden toegewezen aan een aangepaste rol. Zelfs als u de Storage-account hebt gemaakt, maakt deze niet automatisch u de eigenaar van het storage-account. Volg deze stappen voor het verlenen uzelf eigenaar van het storage-account. U kunt ook kan een aangepaste rol worden gemaakt met deze machtiging die u uzelf in om u te kan toevoegen.  

1. Ga naar opslagaccount in Azure portal
1. Selecteer **toegangsbeheer (IAM)**
1. Klik op **toevoegen**
1. Uzelf in toevoegen als eigenaar.

## <a name="next-steps"></a>Volgende stappen

Als u wilt meer informatie over het delen van gegevens, blijven de [delen van uw gegevens](share-your-data.md) zelfstudie.


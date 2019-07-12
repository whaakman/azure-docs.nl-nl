---
title: Functies en vereisten voor voorbeeld van Azure gegevens delen
description: Functies en vereisten voor voorbeeld van Azure gegevens delen
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: a5d70b9aa611b4f939cb46b5d25655edd818cb35
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807524"
---
# <a name="roles-and-requirements-for-azure-data-share-preview"></a>Functies en vereisten voor voorbeeld van Azure gegevens delen

Dit artikel beschrijft de rollen die zijn vereist voor het delen van gegevens met behulp van Azure gegevens delen Preview, evenals te accepteren en gegevens met behulp van Azure-voorbeeld voor het delen van gegevens's ontvangen. 

## <a name="roles-and-requirements"></a>Functies en vereisten

Om te delen of ontvangen van gegevens met behulp van Azure-gegevens delen, moet het gebruikersaccount dat u gebruiken om aan te melden bij Azure kunnen machtigingen met delen van gegevens naar het opslagaccount dat u van gegevens delen of gegevens in om te ontvangen. Dit is doorgaans een machtiging die deel uitmaakt van de **eigenaar** functie of een aangepaste rol met Microsoft.Authorization/role toewijzingen-/ schrijftoegang tot toegewezen. 

Als u wilt delen, of gegevens ontvangt van of naar een Azure Storage-account, moet u een eigenaar van het opslagaccount. Zelfs als u de Storage-account hebt gemaakt, verleent deze automatisch u geen eigendom van het opslagaccount. Als u wilt uzelf in toevoegen aan de rol van eigenaar van uw Azure Storage-account, de volgende stappen uit.

1. Ga naar opslagaccount in Azure portal
1. Selecteer **toegangsbeheer (IAM)**
1. Klik op **toevoegen**
1. Uzelf in als eigenaar toevoegen

Als u de machtigingen die u in het abonnement hebt, wilt bekijken, gaat u naar Azure Portal, selecteert u rechtsboven uw gebruikersnaam en selecteert u vervolgens **Machtigingen**. Als u toegang tot meerdere abonnementen hebt, moet u het juiste abonnement selecteren. 

## <a name="resource-provider-registration"></a>Registratie van de Resourceprovider 

Wanneer een uitnodiging voor delen van gegevens van Azure accepteert, moet u handmatig registreren van de resourceprovider Microsoft.DataShare in uw abonnement. Volg deze stappen voor het registreren van de resourceprovider Microsoft.DataShare in uw Azure-abonnement. 

1. In de Azure-portal, gaat u naar **abonnementen**
1. Selecteer het abonnement dat u voor Azure het delen van gegevens
1. Klik op **Resourceproviders**
1. Zoeken naar Microsoft.DataShare
1. Klik op **registreren**

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over functies in Azure - [roldefinities begrijpen](../role-based-access-control/role-definitions.md)


---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/06/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 3a3ee0cda3643ac73581f868e47905841d9f1563
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095588"
---
> [!IMPORTANT]
> - De Preview-versie van Azure AD-verificatie voor blobs en wachtrijen is bedoeld voor gebruik in alleen niet-productieomgevingen. Productie service level agreements (Sla's) zijn momenteel niet beschikbaar. Als Azure AD-verificatie wordt nog niet ondersteund voor uw scenario, echter ook doorgaan met de gedeelde sleutel autorisatie of SAS-tokens in uw toepassingen.
>
> - Tijdens de Preview-versie duurt RBAC-roltoewijzingen tot vijf minuten worden doorgegeven.
>
> - U moet HTTPS gebruiken om te verifiëren met Azure AD bij het aanroepen van blob- en wachtrijservices bewerkingen.
>
> - In de preview-versie, de Azure-portal Azure AD-referenties niet gebruikt voor het lezen en schrijven van gegevens voor blob- en wachtrijservices. In plaats daarvan blijft de portal zijn afhankelijk van de toegangssleutel. Als u wilt weergeven of bijwerken van de blob of een wachtrij gegevens in de portal, de gebruiker moet worden toegewezen een RBAC-rol die omvat [Microsoft.Storage/storageAccounts/listkeys/action](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role), die verleent machtigingen aan te roepen [Storage-Accounts: Sleutels weergeven](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/listkeys). De rollen Inzender en lezer voor blobs en wachtrijen die momenteel omvatten niet de **listkeys** actie als onderdeel van de preview release en dus biedt geen toegang tot gegevens via de Azure-portal. Als u wilt verkennen op basis van de identiteit in blob- en wachtrijservices gegevens tijdens de preview-versie, PowerShell of Azure CLI te gebruiken.

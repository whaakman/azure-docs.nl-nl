---
title: De Azure-portal gebruiken voor het beheren van Azure AD-toegangsrechten tot containers en wachtrijen met RBAC (preview) - Azure Storage | Microsoft Docs
description: Op rollen gebaseerd toegangsbeheer (RBAC) van de Azure-portal toegang toewijzen tot containers en wachtrijen om beveiligings-principals te gebruiken. Azure Storage biedt ondersteuning voor ingebouwde en aangepaste RBAC-rollen voor verificatie via Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/25/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 68511d62887cb0463fd8db01cb5c90cbc40ac4cd
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56818983"
---
# <a name="grant-access-to-azure-containers-and-queues-with-rbac-in-the-azure-portal-preview"></a>Toegang verlenen tot Azure-containers en wachtrijen met RBAC in Azure portal (preview)

Azure Active Directory (Azure AD) machtigt toegangsrechten tot beveiligde bronnen via [op rollen gebaseerd toegangsbeheer (RBAC)](../../role-based-access-control/overview.md). Azure Storage definieert een aantal ingebouwde RBAC-rollen die algemene sets machtigingen die wordt gebruikt voor toegang tot containers of wachtrijen omvatten. 

Wanneer een RBAC-rol is toegewezen aan een beveiligings-principal voor Azure AD, wordt de status van Azure verleent toegang tot deze resources voor deze beveiligings-principal. Toegang kan worden gericht op het niveau van het abonnement, de resourcegroep, de storage-account of een afzonderlijke container of de wachtrij. Een beveiligings-principal voor Azure AD kan een gebruiker, een groep, een service-principal van toepassing zijn of een [beheerde identiteit voor de Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md).

In dit artikel wordt beschreven hoe u de Azure portal gebruiken voor het toewijzen van RBAC-rollen. De Azure portal biedt een eenvoudige interface voor RBAC-rollen toewijzen en beheren van toegang tot uw opslagresources. U kunt ook RBAC-rollen voor blob- en wachtrijservices resources met behulp van Azure-opdrachtregelprogramma's of via de Azure Storage management API's toewijzen. Zie voor meer informatie over RBAC-rollen voor opslagresources [verifiëren van toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory (Preview)](storage-auth-aad.md). 

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-rollen voor blobs en wachtrijen

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Resource-bereik bepalen 

Voordat u een RBAC-rol aan een beveiligings-principal toewijzen, bepalen het bereik van toegang dat deze beveiligings-principal moet hebben. Aanbevolen procedures leggen dat is het altijd verstandig om alleen de smalst mogelijke bereik verlenen.

De volgende lijst beschrijft de niveaus waarop u kunt het bereik van toegang tot Azure blob- en wachtrijservices-resources, beginnen met het kleinst mogelijke bereik:

- **Als een afzonderlijke container.** Op dit bereik heeft een beveiligings-principal toegang tot alle blobs in de container, evenals de eigenschappen van de container en de metagegevens.
- **Een afzonderlijke wachtrij.** Op dit bereik heeft een beveiligings-principal toegang tot berichten in de wachtrij, evenals wachtrij-eigenschappen en metagegevens.
- **Het opslagaccount.** Op dit bereik heeft een beveiligings-principal toegang tot alle containers en hun blobs of naar alle wachtrijen en hun berichten.
- **De resourcegroep.** Op dit bereik heeft een beveiligings-principal toegang tot alle containers of wachtrijen in alle van de storage-accounts in de resourcegroep.
- **Het abonnement.** Op dit bereik heeft een beveiligings-principal toegang tot alle containers of wachtrijen in alle van de storage-accounts in alle resourcegroepen in het abonnement.

Wanneer u het gewenste bereik voor een roltoewijzing hebt bepaald, gaat u naar de juiste resource in Azure portal. Weergave de **Access Control (IAM)** instellingen voor de resource en volg de instructies in de volgende secties voor het beheren van roltoewijzingen.

## <a name="assign-rbac-roles-using-the-azure-portal"></a>Toewijzen van RBAC-rollen met behulp van de Azure portal

Verlenen van toegang tot blob- en wachtrijservices resources met Azure AD-referenties omvat de volgende stappen: 

1. De juiste Azure Storage RBAC-rol toegang verlenen tot containers of wachtrijen toewijzen. Voor toegang tot het lezen, wijzen de **gegevenslezer voor Opslagblob (Preview)** of **gegevenslezer voor Opslagwachtrij (Preview)** rol. Voor lezen, schrijven en verwijderen, toewijzen de **Inzender van Blob-gegevens (Preview)** of **wachtrij Inzender (Preview)** rol. U kunt ook een aangepaste rol toewijzen.

1. De Azure Resource Manager toewijzen [lezer](../../role-based-access-control/built-in-roles.md#reader) rol voor gebruikers die toegang moeten krijgen tot containers of wachtrijen via Azure portal met hun Azure AD-referenties. 

De volgende secties beschrijven elk van deze stappen nader uiteengezet.

### <a name="assign-a-built-in-rbac-role"></a>Een ingebouwde RBAC-rol toewijzen

Voordat u een rol aan een beveiligings-principal toewijzen, moet u rekening houden met het bereik van de machtigingen die u verleent. Controleer de [resource bereik bepalen](#determine-resource-scope) sectie om te bepalen het juiste bereik.

De procedure hieronder wijst een rol binnen het bereik van een container, maar u kunt dezelfde stappen als u wilt toewijzen van een rol binnen het bereik van een wachtrij: 

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account en weergeven van de **overzicht** voor het account.
1. Selecteer onder Services **Blobs**. 
1. Zoek naar de container die u wilt een rol toewijzen en weergeven van de instellingen van de container. 
1. Selecteer **toegangsbeheer (IAM)** om instellingen voor toegangsbeheer voor de container weer te geven. Selecteer de **roltoewijzingen** tabblad om te bekijken van de lijst van roltoewijzingen.

    ![Instellingen voor toegangsbeheer container schermopname van het scherm](media/storage-auth-aad-rbac/portal-access-control-container.png)

1. Klik op de **roltoewijzing toevoegen** knop een nieuwe rol toe te voegen.
1. In de **roltoewijzing toevoegen** venster, selecteer de Azure Storage-rol die u wilt toewijzen. Vervolgens zoekt u naar de beveiligings-principal die u wilt toewijzen die rol.

    ![Schermopname waarin een RBAC-rol toewijzen](media/storage-auth-aad-rbac/add-rbac-role.png)

1. Klik op **Opslaan**. De identiteit aan wie u de rol is toegewezen, wordt weergegeven onder die rol. Bijvoorbeeld, de volgende afbeelding ziet u dat de gebruiker is toegevoegd nu heeft tot gegevens in de container met de naam leesmachtigingen *voorbeeldcontainer*.

    ![Schermopname met een lijst van gebruikers die zijn toegewezen aan een rol](media/storage-auth-aad-rbac/container-scoped-role.png)

U kunt vergelijkbare stappen volgen om een rol binnen het bereik van de storage-account, resourcegroep of abonnement toewijzen.

> [!NOTE]
> Als een eigenaar van uw Azure Storage-account, zijn u machtigingen voor toegang tot gegevens niet automatisch toegewezen. U moet zelf expliciet een RBAC-rol toewijzen voor Azure Storage. U kunt deze op het niveau van uw abonnement, resourcegroep, opslagaccount, of een container of een wachtrij toewijzen.
> 
> U kunt een rol binnen het bereik van een container of de wachtrij als uw storage-account een hiërarchische naamruimte ingeschakeld heeft niet toewijzen.

### <a name="assign-the-azure-resource-manager-reader-role"></a>De rol van lezer voor Azure Resource Manager toewijzen

Wanneer u een ingebouwde of aangepaste rol voor Azure Storage op een beveiligings-principal toewijzen, verleent u machtigingen voor die beveiligings-principal voor het uitvoeren van bewerkingen op gegevens in uw storage-account. De ingebouwde **gegevenslezer** rollen bieden leesmachtigingen voor de gegevens in een container of een wachtrij, terwijl de ingebouwde **Inzender Data** rollen bieden lezen, schrijven en verwijderen van machtigingen voor een container of wachtrij. Machtigingen zijn gericht op de opgegeven resource.  

Als u bijvoorbeeld de **Gegevensbijdrager voor Blob (Preview)** rol aan Mary op het niveau van een container met de naam van gebruiker **voorbeeld-container**, Mary is verleend lezen, schrijven en toegang tot alle verwijderen de blobs in die container.

Echter, als Mary wil weergeven van een blob in Azure portal, dan zal de **Gegevensbijdrager voor Blob (Preview)** rol zelf bieden niet voldoende machtigingen om te navigeren via de portal naar de blob om te bekijken. Extra Azure AD-machtigingen zijn vereist om te navigeren via de portal en weergeven van de andere resources die er zichtbaar zijn.

Als uw gebruikers nodig hebben om toegang tot blobs in Azure portal en vervolgens een extra RBAC-rol toewijzen te kunnen de [lezer](../../role-based-access-control/built-in-roles.md#reader) rol aan deze gebruikers. De **lezer** rol is een Azure Resource Manager-rol die gebruikers kunnen resources voor storage-account weergeven, maar ze niet worden gewijzigd. Biedt geen leesmachtigingen voor gegevens in Azure Storage, maar alleen voor account management-resources.

Volg deze stappen om toe te wijzen de **lezer** rol. In dit geval wordt is de toewijzing afgestemd op de container:

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account en weergeven van de **overzicht** voor het account.
1. Selecteer onder Services **Blobs**. 
1. Zoek naar de container die u wilt een rol toewijzen en weergeven van de instellingen van de container. 
1. Selecteer **toegangsbeheer (IAM)** om instellingen voor toegangsbeheer voor de container weer te geven. Selecteer de **roltoewijzingen** tabblad om te bekijken van de lijst van roltoewijzingen.
1. In de **roltoewijzing toevoegen** venster de **lezer** rol. 
1. Uit de **toegang toewijzen aan** vervolgkeuzelijst, selecteer **Azure AD-gebruiker, groep of service-principal**.
1. Zoekt u naar de beveiligings-principal die u wilt de rol toe te wijzen.
1. De roltoewijzing opslaan.

## <a name="use-azure-ad-credentials-with-the-portal"></a>Azure AD-referenties gebruiken met de portal

Voor toegang tot blobs of wachtrijen in Azure portal met uw Azure AD-referenties, gebruikt u de preview-koppelingen weergegeven in de volgende afbeelding:

![Toegang tot blobs of wachtrijen met Azure AD-referenties in portal](media/storage-auth-aad-rbac/access-data-azure-ad.png)

Als u toegang blob of een wachtrij gegevens met behulp van de productie-koppelingen in plaats van de preview-koppelingen tot, de Azure portal maakt gebruik van uw accountsleutel te autoriseren voor toegang, in plaats van met behulp van Azure AD.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over RBAC [wat is op rollen gebaseerd toegangsbeheer (RBAC)?](../../role-based-access-control/overview.md).
- Voor informatie over het toewijzen en beheren van RBAC-roltoewijzingen met Azure PowerShell, Azure CLI of de REST-API, Zie de volgende artikelen:
    - [Op rollen gebaseerd toegangsbeheer (RBAC met Azure PowerShell) beheren](../../role-based-access-control/role-assignments-powershell.md)
    - [Op rollen gebaseerd toegangsbeheer (RBAC met Azure CLI) beheren](../../role-based-access-control/role-assignments-cli.md)
    - [Beheren van op rollen gebaseerd toegangsbeheer (RBAC) met de REST-API](../../role-based-access-control/role-assignments-rest.md)
- Zie voor informatie over het toestaan van toegang tot containers en wachtrijen van binnen uw storage-toepassingen, [gebruik Azure AD met Azure Storage-toepassingen](storage-auth-aad-app.md).
- Zie voor meer informatie over Azure AD-integratie voor Azure-containers en wachtrijen, het blog van het Azure Storage-team plaatst, [aankondiging van de Preview-versie van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).

---
title: Beheren van instellingen van opslagaccount in Azure portal - Azure Storage | Microsoft Docs
description: Informatie over het beheren van instellingen van opslagaccount in Azure portal, inclusief het configureren van instellingen voor toegangsbeheer, opnieuw genereren van toegangssleutels van account, als u de toegangslaag wijzigt of het wijzigen van het type replicatie gebruikt door het account. U leert ook hoe u een opslagaccount in de portal verwijderen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 66bdc4bd1e17347419a6eccd7c9532db17b33001
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303483"
---
# <a name="manage-storage-account-settings-in-the-azure-portal"></a>Instellingen van opslagaccount in Azure portal beheren

Een aantal instellingen voor uw storage-account zijn beschikbaar in de [Azure-portal](https://portal.azure.com). In dit artikel worden enkele van deze instellingen en het gebruik ervan beschreven.

## <a name="access-control"></a>Toegangsbeheer

Azure Storage biedt ondersteuning voor verificatie met Azure Active Directory voor Blob storage en Queue storage via op rollen gebaseerd toegangsbeheer (RBAC). Zie voor meer informatie over verificatie met Azure AD [autoriseren de toegang tot Azure-blobs en wachtrijen met behulp van Azure Active Directory](storage-auth-aad.md).

De **toegangsbeheer** instellingen in de Azure portal biedt een eenvoudige manier om RBAC-rollen toewijzen aan gebruikers, groepen, service-principals en beheerde identiteiten. Zie voor meer informatie over het toewijzen van RBAC-rollen [beheren toegangsrechten tot blob- en wachtrijservices gegevens met RBAC](storage-auth-aad-rbac.md).

## <a name="tags"></a>Tags

Azure Storage biedt ondersteuning voor Azure Resource Manager-tags voor het ordenen van uw Azure-resources met een aangepaste taxonomie. U kunt tags toepassen op uw storage-accounts, zodat u kunt ze binnen uw abonnement op een logische manier groeperen.

Voor storage-accounts een tagnaam is beperkt tot 128 tekens en een tagwaarde is beperkt tot 256 tekens.

Zie voor meer informatie, [tags gebruiken om uw Azure-resources te organiseren](../../azure-resource-manager/resource-group-using-tags.md).

## <a name="access-keys"></a>Toegangssleutels

Wanneer u een opslagaccount maakt, genereert Azure twee 512-bits opslagaccountsleutels. Deze sleutels kunnen worden gebruikt om toegang tot uw opslagaccount via gedeelde sleutel te verlenen. U kunt draaien en de sleutels zonder onderbreking opnieuw genereren voor uw toepassingen en Microsoft raadt aan dat u dit regelmatig doen.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

[!INCLUDE [storage-recommend-azure-ad-include](../../../includes/storage-recommend-azure-ad-include.md)]

### <a name="view-account-keys-and-connection-string"></a>Accountsleutels en verbindingsreeks weergeven

[!INCLUDE [storage-view-keys-include](../../../includes/storage-view-keys-include.md)]

### <a name="regenerate-access-keys"></a>Toegangssleutels regenereren

Microsoft raadt aan dat u de toegangssleutels regelmatig om uw storage-account te beveiligen. Twee toegangssleutels toegewezen zodat u kunt uw sleutels draaien. Als u uw sleutels draait, moet u ervoor zorgen dat uw toepassing toegang tot Azure Storage gedurende het gehele proces behoudt. 

> [!WARNING]
> Opnieuw genereren van uw toegangssleutels kan invloed op toepassingen of Azure-services die afhankelijk van de opslagaccountsleutel zijn. Alle clients die gebruikmaken van de accountsleutel voor toegang tot het opslagaccount dat moeten worden bijgewerkt voor het gebruik van de nieuwe sleutel, met inbegrip van mediaservices, cloud, desktop en mobiele toepassingen en toepassingen van de grafische interface voor Azure Storage, zoals [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).

Volg deze procedure voor uw opslagaccountsleutels draaien:

1. Werk de verbindingsreeksen in uw toepassingscode gebruiken van de secundaire sleutel.
2. Genereer de primaire toegangssleutel voor uw opslagaccount opnieuw. Op de **toegangssleutels** blade in Azure portal, klik op **Key1 opnieuw genereren**, en klik vervolgens op **Ja** te bevestigen dat u wilt om een nieuwe sleutel te genereren.
3. Werk de verbindingsreeksen in uw code bij, zodat deze verwijzen naar de nieuwe primaire toegangssleutel.
4. Genereer de secundaire toegangssleutel op dezelfde manier opnieuw.

## <a name="account-configuration"></a>De configuratie van account

Nadat u een opslagaccount hebt gemaakt, kunt u de configuratie ervan kunt wijzigen. U kunt bijvoorbeeld wijzigen hoe uw gegevens worden gerepliceerd of wijzigen van de toegangslaag van het account van warm naar koud. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account, zoeken en klikt u op **configuratie** onder **instellingen** te bekijken en/of de configuratie van het account wijzigen.

Als u de configuratie van het storage-account wijzigt, kan dit leiden tot extra kosten. Zie voor meer informatie de [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) pagina.

## <a name="delete-a-storage-account"></a>Een opslagaccount verwijderen

Als u een opslagaccount wilt verwijderen dat u niet langer gebruikt, gaat u in [Azure Portal](https://portal.azure.com) naar het opslagaccount en klikt u op **Verwijderen**. Als u een opslagaccount verwijdert, wordt het hele account verwijderd, inclusief alle gegevens in het account.

> [!WARNING]
> Het is niet mogelijk om een verwijderd opslagaccount te herstellen of om inhoud terug te halen die het account vóór verwijdering bevatte. Zorg ervoor dat u een back-up maakt van alles dat u wilt opslaan, voordat u het account verwijdert. Dit geldt ook voor alle resources in het account: als u blobs, tabellen, wachtrijen of bestanden verwijdert, worden deze permanent verwijderd.
> 

Als u probeert een opslagaccount te verwijderen dat is gekoppeld aan een virtuele machine van Azure, ziet u mogelijk een foutbericht dat het account nog in gebruik is. Zie [Troubleshoot errors when you delete Azure storage accounts, containers, or VHDs](../common/storage-resource-manager-cannot-delete-storage-account-container-vhd.md) (Fouten oplossen met het verwijderen van Azure-opslagaccounts, containers of VHD's) voor informatie over het oplossen van deze fout.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van Azure storage-account](storage-account-overview.md)
- [Een opslagaccount maken](storage-quickstart-create-account.md)

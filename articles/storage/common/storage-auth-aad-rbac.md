---
title: RBAC gebruiken voor het beheren van toegangsrechten tot containers en wachtrijen (preview) - Azure Storage | Microsoft Docs
description: Op rollen gebaseerd toegangsbeheer (RBAC) het toewijzen van rollen voor toegang tot blob- en wachtrijservices gegevens aan gebruikers, groepen, service-principals van toepassingen of beheerde service-identiteiten gebruiken Azure Storage biedt ondersteuning voor ingebouwde en aangepaste rollen voor toegangsrechten tot containers en wachtrijen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/15/2018
ms.author: tamram
ms.component: common
ms.openlocfilehash: 5f558ea851d63b08885293efcff3fef600f2cc17
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726386"
---
# <a name="manage-access-rights-to-azure-blob-and-queue-data-with-rbac-preview"></a>Beheer van rechten voor het Azure Blob- en wachtrijgegevens met RBAC (Preview)

Azure Active Directory (Azure AD) machtigt toegangsrechten tot beveiligde bronnen via [op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). Azure Storage definieert een aantal ingebouwde RBAC-rollen die algemene sets machtigingen die wordt gebruikt voor toegang tot containers of wachtrijen omvatten. Wanneer een RBAC-rol is toegewezen aan een Azure AD-identiteit die identiteit krijgt toegang tot deze resources, op basis van het opgegeven bereik. Toegang kan worden gericht op het niveau van het abonnement, de resourcegroep, de storage-account of een afzonderlijke container of de wachtrij. Hier kunt u toegangsrechten voor Azure Storage-resources met behulp van de Azure portal, opdrachtregelprogramma's van Azure en Azure Management-API's. 

Een Azure AD-identiteit is mogelijk een gebruiker, groep of toepassingsservice-principal of een beheerde identiteit voor Azure-resources kan zijn. Een beveiligings-principal kan een gebruiker, groep of service-principal van toepassing zijn. Een [beheerde identiteit voor de Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md) is automatisch beheerde identiteit gebruikt voor het verifiëren van toepassingen die worden uitgevoerd in virtuele machines van Azure, functie-apps, virtuele-machineschaalsets en anderen. Zie voor een overzicht van identiteit in Azure AD, [over Azure-identiteitsoplossingen](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions).

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>RBAC-rollen voor blobs en wachtrijen

Azure Storage ondersteunt zowel ingebouwde als aangepaste RBAC-rollen. Azure Storage biedt deze ingebouwde RBAC-rollen voor gebruik met Azure AD:

- [Gegevensbijdrager voor Blob (Preview)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview)
- [Gegevenslezer voor Opslagblob (Preview)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)
- [Gegevensbijdrager voor wachtrij (Preview)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)
- [Gegevenslezer voor Opslagwachtrij (Preview)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-reader-preview)

Voor meer informatie over hoe u de ingebouwde rollen voor Azure Storage zijn gedefinieerd, Zie [begrijpen roldefinities](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#management-and-data-operations-preview).

U kunt ook aangepaste rollen voor gebruik met containers en wachtrijen definiëren. Zie voor meer informatie, [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). 

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="assign-a-role-to-a-security-principal"></a>Een rol toewijzen aan een beveiligings-principal

Een RBAC-rol toewijzen aan een Azure-identiteit om machtigingen aan containers of wachtrijen in uw opslagaccount te verlenen. U kunt het bereik van de toewijzing van rol met de storage-account, of aan een bepaalde container of de wachtrij. De volgende tabel geeft een overzicht van de rechten verleend door de ingebouwde rollen, afhankelijk van bereik: 

|                                 |     Inzender voor BLOB-gegevens                                                 |     Gegevenslezer voor opslagblob                                                |     Inzender voor wachtrij Data                                  |     Gegevenslezer voor opslagwachtrij                                 |
|---------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------|
|    Binnen het bereik van abonnement       |    Lees-/ schrijftoegang tot alle containers en blobs in het abonnement       |    Leestoegang tot alle containers en blobs in het abonnement       |    Lees-/ schrijftoegang tot alle wachtrijen in het abonnement       |    Leestoegang tot alle wachtrijen in het abonnement         |
|    Binnen het bereik van resourcegroep     |    Lees-/ schrijftoegang tot alle containers en blobs in de resourcegroep     |    Leestoegang tot alle containers en blobs in de resourcegroep     |    Lees-/ schrijftoegang tot alle wachtrijen in de resourcegroep     |    Leestoegang tot alle wachtrijen in de resourcegroep     |
|    Binnen het bereik van storage-account    |    Lees-/ schrijftoegang tot alle containers en blobs in de storage-account    |    Leestoegang tot alle containers en blobs in de storage-account    |    Lees-/ schrijftoegang tot alle wachtrijen in de storage-account    |    Leestoegang tot alle wachtrijen in de storage-account    |
|    Binnen het bereik van container/wachtrij    |    Toegang tot de opgegeven container en de blobs voor lezen/schrijven              |    Leestoegang tot de opgegeven container en de blobs              |    Lees-/ schrijftoegang tot de opgegeven wachtrij                  |    Leestoegang tot de opgegeven wachtrij                    |

> [!NOTE]
> Als een eigenaar van uw Azure Storage-account, zijn u machtigingen voor toegang tot gegevens niet automatisch toegewezen. U moet zelf expliciet een RBAC-rol toewijzen voor Azure Storage. U kunt deze op het niveau van uw abonnement, resourcegroep, opslagaccount, of een container of een wachtrij toewijzen.

Zie voor meer informatie over de vereiste machtigingen voor het aanroepen van Azure Storage-bewerkingen, [machtigingen voor het aanroepen van REST-bewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

De volgende secties laten zien hoe een rol binnen het bereik van de storage-account of binnen het bereik van een afzonderlijke container toewijzen.

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>Een rol binnen het bereik van het opslagaccount in Azure portal toewijzen

Een ingebouwde rol verlenen van toegang tot alle containers of wachtrijen in het opslagaccount in Azure portal toewijzen:

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account.
1. Selecteer uw storage-account en selecteer vervolgens **toegangsbeheer (IAM)** om instellingen voor toegangsbeheer voor het account weer te geven. Selecteer de **roltoewijzingen** tabblad om te bekijken van de lijst van roltoewijzingen.

    ![Schermopname van de instellingen voor toegangsbeheer opslag](media/storage-auth-aad-rbac/portal-access-control.png)

1. Klik op de **roltoewijzing toevoegen** knop een nieuwe rol toe te voegen.
1. In de **roltoewijzing toevoegen** venster, selecteer de rol wilt toewijzen aan een Azure AD-identiteit. Vervolgens kunt u zoeken om te zoeken van de identiteit aan wie u wilt toewijzen die rol. Bijvoorbeeld, de volgende afbeelding toont de **gegevenslezer voor Opslagblob (Preview)** rol die is toegewezen aan een gebruiker.

    ![Schermopname waarin een RBAC-rol toewijzen](media/storage-auth-aad-rbac/add-rbac-role.png)

1. Klik op **Opslaan**. De identiteit aan wie u de rol is toegewezen, wordt weergegeven onder die rol. De volgende afbeelding ziet u bijvoorbeeld dat de gebruiker is toegevoegd nu heeft voor alle blob-gegevens in de storage-account leesmachtigingen.

    ![Schermopname met een lijst van gebruikers die zijn toegewezen aan een rol](media/storage-auth-aad-rbac/account-scoped-role.png)

### <a name="assign-a-role-scoped-to-a-container-or-queue-in-the-azure-portal"></a>Een rol binnen het bereik van een container of een wachtrij in Azure portal toewijzen

De stappen voor het toewijzen van een ingebouwde rol binnen het bereik van een container of een wachtrij zijn vergelijkbaar. De procedure hieronder wijst een rol binnen het bereik van een container, maar u kunt dezelfde stappen als u wilt toewijzen van een rol binnen het bereik van een wachtrij: 

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account en weergeven van de **overzicht** voor het account.
1. Selecteer onder Services **Blobs**. 
1. Zoek naar de container die u wilt een rol toewijzen en weergeven van de instellingen van de container. 
1. Selecteer **toegangsbeheer (IAM)** om instellingen voor toegangsbeheer voor de container weer te geven. Selecteer de **roltoewijzingen** tabblad om te bekijken van de lijst van roltoewijzingen.

    ![Instellingen voor toegangsbeheer container schermopname van het scherm](media/storage-auth-aad-rbac/portal-access-control-container.png)
1. Klik op de **roltoewijzing toevoegen** knop een nieuwe rol toe te voegen.
1. In de **roltoewijzing toevoegen** venster, selecteer de rol die u wilt toewijzen aan een Azure AD-identiteit. Vervolgens zoekt u naar de identiteit die u wilt toewijzen die rol.
1. Klik op **Opslaan**. De identiteit aan wie u de rol is toegewezen, wordt weergegeven onder die rol. Bijvoorbeeld, de volgende afbeelding ziet u dat de gebruiker is toegevoegd nu heeft tot gegevens in de container met de naam leesmachtigingen *voorbeeldcontainer*.

    ![Schermopname met een lijst van gebruikers die zijn toegewezen aan een rol](media/storage-auth-aad-rbac/container-scoped-role.png)

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over RBAC [wat is op rollen gebaseerd toegangsbeheer (RBAC)?](../../role-based-access-control/overview.md).
- Voor informatie over het toewijzen en beheren van RBAC-roltoewijzingen met Azure PowerShell, Azure CLI of de REST-API, Zie de volgende artikelen:
    - [Op rollen gebaseerd toegangsbeheer (RBAC met Azure PowerShell) beheren](../../role-based-access-control/role-assignments-powershell.md)
    - [Op rollen gebaseerd toegangsbeheer (RBAC met Azure CLI) beheren](../../role-based-access-control/role-assignments-cli.md)
    - [Beheren van op rollen gebaseerd toegangsbeheer (RBAC) met de REST-API](../../role-based-access-control/role-assignments-rest.md)
- Zie voor informatie over het toestaan van toegang tot containers en wachtrijen van binnen uw storage-toepassingen, [gebruik Azure AD met Azure Storage-toepassingen](storage-auth-aad-app.md).
- Zie voor meer informatie over Azure AD-integratie voor Azure-containers en wachtrijen, het blog van het Azure Storage-team plaatst, [aankondiging van de Preview-versie van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
- 

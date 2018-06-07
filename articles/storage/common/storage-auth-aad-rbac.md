---
title: Gebruikmaken van RBAC voor het beheren van rechten voor het Azure Storage-containers en -wachtrijen (Preview) | Microsoft Docs
description: Gebruik op rollen gebaseerde toegangsbeheer (RBA) toewijzen van rollen voor toegang tot Azure Storage-gegevens aan gebruikers, groepen, service-principals toepassingen of beheerde service-identiteiten. Azure Storage ondersteunt ingebouwde en aangepaste rollen voor toegangsrechten tot containers en wachtrijen.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/29/2018
ms.author: tamram
ms.openlocfilehash: cb77bd4418e105c877202f0f1725350380ea2308
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660806"
---
# <a name="manage-access-rights-to-azure-storage-data-with-rbac-preview"></a>Rechten voor het Azure Storage-gegevens met RBAC (Preview) beheren

Azure Active Directory (Azure AD) machtigt toegangsrechten tot beveiligde bronnen via [op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). Azure Storage definieert een aantal ingebouwde RBAC-rollen die algemene sets met machtigingen die worden gebruikt voor toegang tot containers of wachtrijen omvatten. Wanneer een RBAC-rol is toegewezen aan een Azure AD-identiteit identiteit is toegang tot deze bronnen, volgens het opgegeven bereik. Toegang kan worden afgestemd op het niveau van het abonnement, de resourcegroep, het storage-account of een afzonderlijke container of de wachtrij. U kunt de toegangsrechten voor Azure Storage-resources met behulp van de Azure-portal, Azure-opdrachtregelprogramma's en Azure Management-API's toewijzen. 

Een Azure AD-identiteit is mogelijk een gebruiker, groep of toepassing service-principal of deze is mogelijk een *beheerde service-identiteit*. Een beveiligings-principal kan een gebruiker, groep of service-principal van toepassing zijn. Een [beheerde service-identiteit](../../active-directory/managed-service-identity/overview.md) is gebruikt voor het verifiëren van toepassingen in een andere virtuele machines in Azure, functie apps en virtuele-machineschaalsets automatisch beheerde identiteit. Zie voor een overzicht van identiteit in Azure AD [begrijpen Azure-identiteitsoplossingen](https://docs.microsoft.com/en-us/azure/active-directory/understand-azure-identity-solutions).

## <a name="rbac-roles-for-azure-storage"></a>RBAC-rollen voor Azure Storage

Azure Storage ondersteunt ingebouwde en aangepaste RBAC-rollen. Azure Storage biedt deze ingebouwde RBAC-rollen voor gebruik met Azure AD:

- [Opslag-Blob Inzender (Preview)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview)
- [Opslag-Blob gegevenslezer (Preview)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview)
- [Opslag wachtrij Inzender (Preview)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview)
- [Opslag wachtrij gegevenslezer (Preview)](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-roles#storage-queue-data-reader-preview)

Voor meer informatie over hoe u ingebouwde rollen zijn gedefinieerd voor Azure Storage, Zie [begrijpen roldefinities](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#management-and-data-operations-preview).

U kunt ook aangepaste rollen voor gebruik met containers en -wachtrijen definiëren. Zie voor meer informatie [aangepaste rollen maken voor op rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/custom-roles.md). 

> [!IMPORTANT]
> Deze preview is bedoeld voor gebruik van niet-productieve alleen. Productie service level agreements (Sla's) worden pas beschikbaar Azure AD-integratie voor Azure Storage is gedeclareerd in het algemeen beschikbaar. Als u Azure AD-integratie is nog niet ondersteund voor uw scenario, blijven de gedeelde sleutel autorisatie of SAS-tokens gebruiken in uw toepassingen. Zie voor meer informatie over de evaluatieversie [verifiëren van toegang tot Azure Storage met Azure Active Directory (Preview)](storage-auth-aad.md).
>
> Tijdens de preview kunnen de RBAC-roltoewijzingen doorgeven maximaal vijf minuten duren.

## <a name="assign-a-role-to-a-security-principal"></a>Een rol toewijzen aan een beveiligings-principal

RBAC-rol toewijzen aan een Azure identiteit machtigen om containers of wachtrijen in uw opslagaccount. U kunt het bereik van de roltoewijzing voor het opslagaccount of aan een specifieke container of de wachtrij. De volgende tabel geeft een overzicht van de toegangsrechten die zijn verleend door de ingebouwde rollen, afhankelijk van bereik: 

|                                 |     Inzender van BLOB-gegevens                                                 |     De gegevenslezer BLOB                                                |     Inzender wachtrij                                  |     De gegevenslezer wachtrij                                 |
|---------------------------------|------------------------------------------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------|
|    Binnen het bereik van abonnement       |    Lees-/ schrijftoegang tot alle containers en blobs in het abonnement       |    Leestoegang tot alle containers en blobs in het abonnement       |    Lees-/ schrijftoegang tot alle wachtrijen in het abonnement       |    Leestoegang tot alle wachtrijen in het abonnement         |
|    Binnen het bereik van resourcegroep     |    Lees-/ schrijftoegang tot alle containers en blobs in de resourcegroep     |    Leestoegang tot alle containers en blobs in de resourcegroep     |    Lees-/ schrijftoegang tot alle wachtrijen in de resourcegroep     |    Leestoegang tot alle wachtrijen in de resourcegroep     |
|    Binnen het bereik van storage-account    |    Lees-/ schrijftoegang tot alle containers en blobs in de storage-account    |    Leestoegang tot alle containers en blobs in de storage-account    |    Lees-/ schrijftoegang tot alle wachtrijen in de storage-account    |    Leestoegang tot alle wachtrijen in de storage-account    |
|    Binnen het bereik van container/wachtrij    |    Lees-/ schrijftoegang tot de opgegeven container en de blobs              |    Leestoegang tot de opgegeven container en de bijbehorende blobs              |    Lees-/ schrijftoegang tot de opgegeven wachtrij                  |    Leestoegang tot de opgegeven wachtrij                    |

> [!NOTE]
> Als een eigenaar van uw Azure Storage-account zijn u machtigingen voor toegang tot gegevens niet automatisch toegewezen. U moet zelf expliciet een RBAC-rol toewijzen voor Azure Storage. Op het niveau van uw abonnement, resourcegroep, storage-account of een container of een wachtrij kunt u deze toewijzen.

Zie voor meer informatie over de vereiste machtigingen voor het aanroepen van Azure Storage-bewerkingen, [machtigingen voor het aanroepen van REST-bewerkingen](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations).

De volgende secties laten zien hoe een rol binnen het bereik van het opslagaccount of binnen het bereik van een afzonderlijke container toe te wijzen.

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>Een rol binnen het bereik van het opslagaccount in de Azure portal toewijzen

Toewijzen van een ingebouwde rol die geen toegang verlenen tot alle containers of wachtrijen in het opslagaccount in de Azure-portal:

1. In de [Azure-portal](https://azure.portal.com/), gaat u naar uw storage-account.
2. Selecteer uw storage-account en vervolgens **Access Control (IAM)** om instellingen voor toegangsbeheer voor het account weer te geven. Klik op de **toevoegen** knop een nieuwe rol toe te voegen.

    ![Schermopname van instellingen voor toegangsbeheer opslag](media/storage-auth-aad-rbac/portal-access-control.png)

3. In de **machtigingen toevoegen** venster, selecteer de rol toewijzen aan een Azure AD-identiteit. Vervolgens zoeken om te zoeken van de identiteit aan wie u wilt toewijzen die rol. Bijvoorbeeld de volgende afbeelding toont de **gegevenslezer Storage-Blob (Preview)** rol die is toegewezen aan een gebruiker.

    ![Schermopname waarin wordt getoond hoe een RBAC-rol toewijzen](media/storage-auth-aad-rbac/add-rbac-role.png)

4. Klik op **Opslaan**. De identiteit aan wie de rol is toegewezen, wordt weergegeven onder die rol. De volgende afbeelding ziet u bijvoorbeeld dat de gebruikers die zijn toegevoegd nu voor alle blob-gegevens in het opslagaccount leesmachtigingen hebben.

    ![Schermopname met een lijst van gebruikers die zijn toegewezen aan een rol](media/storage-auth-aad-rbac/account-scoped-role.png)

### <a name="assign-a-role-scoped-to-a-container-or-queue-in-the-azure-portal"></a>Een rol binnen het bereik van een container of een wachtrij in de Azure portal toewijzen

De stappen voor het toewijzen van een ingebouwde rol binnen het bereik van een container of een wachtrij zijn identiek. De procedure die hieronder worden weergegeven wijst een rol binnen het bereik van een container, maar u kunt de dezelfde stappen volgen om een rol binnen het bereik van een wachtrij toewijzen: 

1. In de [Azure-portal](https://azure.portal.com/), gaat u naar uw opslagaccount en weergeven van de **overzicht** voor het account.
2. Selecteer onder de Blob-Service **bladeren Blobs**. 
3. Zoek naar de container die u wilt een rol toewijzen en de instellingen voor de container weergeven. 
4. Selecteer **Access Control (IAM)** om instellingen voor toegangsbeheer voor de container weer te geven.
5. In de **machtigingen toevoegen** venster, selecteer de rol die u wilt toewijzen aan een Azure AD-identiteit. Vervolgens zoekt u naar de identiteit waarnaar u wilt toewijzen die rol.
6. Klik op **Opslaan**. De identiteit aan wie de rol is toegewezen, wordt weergegeven onder die rol. De volgende afbeelding ziet u bijvoorbeeld dat de gebruiker is toegevoegd nu tot gegevens in de container met de naam leesmachtigingen heeft *voorbeeld-container*.

    ![Schermopname met een lijst van gebruikers die zijn toegewezen aan een rol](media/storage-auth-aad-rbac/container-scoped-role.png)

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over RBAC, [aan de slag met toegangsbeheer op basis van rollen](../../role-based-access-control/overview.md).
- Zie voor informatie over het toewijzen en beheren van RBAC-roltoewijzingen met Azure PowerShell, Azure CLI of de REST-API, deze artikelen:
    - [Op rollen gebaseerde toegangsbeheer (RBAC) met Azure PowerShell beheren](../../role-based-access-control/role-assignments-powershell.md)
    - [Beheren van op rollen gebaseerde toegangsbeheer (RBAC met Azure CLI)](../../role-based-access-control/role-assignments-cli.md)
    - [Beheren van op rollen gebaseerde toegangsbeheer (RBAC) met de REST-API](../../role-based-access-control/role-assignments-rest.md)
- Zie voor meer informatie over hoe u toegang verlenen aan containers en wachtrijen van binnen uw toepassingen moeten worden opgeslagen, [gebruik Azure AD met Azure Storage-toepassingen](storage-auth-aad-app.md).
- Zie voor meer informatie over Azure AD-integratie voor Azure-containers en -wachtrijen, het Azure Storage-team blogbericht, [aankondigen van de Preview van Azure AD-verificatie voor Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
- 
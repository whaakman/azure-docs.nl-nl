---
title: Op rollen gebaseerd toegangsbeheer in Azure Cosmos DB met Azure Active Directory-integratie
description: Meer informatie over hoe Azure Cosmos DB biedt voor databasebeveiliging met Active directory-integratie (RBAC).
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: e43341e59e807fdc20033d909beadb9d582149b5
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078938"
---
# <a name="role-based-access-control-in-azure-cosmos-db"></a>Op rollen gebaseerd toegangsbeheer in Azure Cosmos DB

Azure Cosmos DB biedt ingebouwde rollen gebaseerd toegangsbeheer (RBAC) voor algemene scenario's voor beheer in Azure Cosmos DB. Een persoon die is gekoppeld aan een profiel in Azure Active Directory kunt deze RBAC-rollen toewijzen aan gebruikers, groepen, service-principals, of beheerde identiteiten te verlenen of weigeren van toegang tot resources en bewerkingen op Azure Cosmos DB-resources. Roltoewijzingen zijn gericht op het vlak van het besturingselement alleen toegang, die toegang biedt tot Azure Cosmos-accounts, -databases, containers, en biedt (doorvoer).

## <a name="built-in-roles"></a>Ingebouwde rollen

Hier volgen de ingebouwde rollen die worden ondersteund door Azure Cosmos DB:

|**Ingebouwde rol**  |**Beschrijving**  |
|---------|---------|
|[DocumentDB-Accounts Inzender](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)   | Kan Azure Cosmos DB-accounts beheren.  |
|[Lezer voor cosmos DB-Account](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)  | Kan Azure Cosmos DB-accountgegevens lezen.        |
|[Cosmos-back-upoperator](../role-based-access-control/built-in-roles.md#cosmosbackupoperator)     |  Kan herstel aanvraag indienen voor een Azure Cosmos-database of een container.       |
|[Cosmos DB-Operator](../role-based-access-control/built-in-roles.md#cosmos-db-operator)  | Azure Cosmos-accounts, -databases en -containers kunt inrichten, maar geen toegang tot de sleutels die zijn vereist voor toegang tot de gegevens.         |

> [!IMPORTANT]
> RBAC-ondersteuning in Azure Cosmos DB is van toepassing op de bewerkingen voor de controlelaag alleen. Bewerkingen voor het vlak van gegevens zijn beveiligd met hoofdsleutels of resource-tokens. Zie voor meer informatie, [beveiligde toegang tot gegevens in Azure Cosmos DB](secure-access-to-data.md)

## <a name="identity-and-access-management-iam"></a>Identiteits- en toegangsbeheer (IAM)

De **toegangsbeheer (IAM)** deelvenster in de Azure-portal wordt gebruikt voor het configureren van op rollen gebaseerd toegangsbeheer in Azure Cosmos-resources. De rollen die worden toegepast op gebruikers, groepen, service-principals en beheerde identiteiten in Active Directory. U kunt ingebouwde rollen of aangepaste rollen gebruiken voor individuen en groepen. De volgende schermafbeelding ziet u Active Directory-integratie (RBAC) met behulp van toegangsbeheer (IAM) in Azure portal:

![Toegangsbeheer (IAM) in Azure portal - beveiliging van de database aan te tonen](./media/role-based-access-control/database-security-identity-access-management-rbac.png)

## <a name="custom-roles"></a>Aangepaste rollen

Naast de ingebouwde rollen, kunnen gebruikers ook maken [aangepaste rollen](../role-based-access-control/custom-roles.md) in Azure en deze rollen toepassen op service-principals in alle abonnementen binnen hun Active Directory-tenant. Aangepaste rollen bieden gebruikers een manier om RBAC roldefinities maken met een aangepaste set resource provider-bewerkingen. Voor meer informatie over welke bewerkingen zijn beschikbaar voor het bouwen van aangepaste rollen voor Azure Cosmos DB Zie [resourceproviderbewerkingen Azure Cosmos DB](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

## <a name="next-steps"></a>Volgende stappen

- [Wat is op rollen gebaseerd toegangsbeheer (RBAC) voor Azure-resources](../role-based-access-control/overview.md)
- [Aangepaste rollen voor Azure-resources](../role-based-access-control/custom-roles.md)
- [Azure Cosmos DB-resourceproviderbewerkingen](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

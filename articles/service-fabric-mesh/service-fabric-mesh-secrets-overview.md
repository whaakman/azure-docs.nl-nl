---
title: Azure Service Fabric Mesh-toepassingsgeheimen opslaan en gebruiken | Microsoft Docs
description: Service Fabric Mesh-geheimen opslaan en gebruiken.
services: service-fabric-mesh
keywords: geheimen
author: v-steg
ms.author: v-steg
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: jeanpaul.connock
ms.openlocfilehash: b6db9b6cb2ccbf1c8d29edb817d35677109ae755
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57768618"
---
# <a name="service-fabric-mesh-application-secrets"></a>Service Fabric Mesh-toepassingsgeheimen
Service Fabric Mesh biedt ondersteuning voor geheimen als Azure-resources. Een Service Fabric Mesh-geheim kan bestaan uit gevoelige informatie in tekstvorm, zoals opslagverbindingsreeksen, wachtwoorden of andere waarden die veilig moeten worden opgeslagen en verzonden.

![Overzicht van Mesh-geheimen][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Mesh-geheimenresources
Een Mesh-toepassingsgeheim bestaat uit:
* Een **Geheimen**-resource. Dit is een container die geheimen in tekstvorm bevat. Geheimen die zijn opgenomen in de **Geheimen**-resource, worden veilig opgeslagen en verzonden.
* Een of meer **Geheimen/waarden**-resources die zijn opgeslagen in de **Geheimen**-resourcecontainer. Elke **Geheimen/waarden**-resource wordt gekenmerkt door een uniek versienummer.

## <a name="inline-or-stored-in-azure-key-vault"></a>Inline- of opgeslagen in Azure Key Vault
- Mesh-toepassingen en -serviceresources worden onder andere gevormd door Managed Service Identity (MSI) met Azure Active Directory (AAD) om toegang tot geheimen in Azure Key Vault te krijgen.
- Geheimen en certificaten kunnen worden automatisch worden vervangen door beleid.

## <a name="next-steps"></a>Volgende stappen 
Voor meer informatie over Service Fabric Mesh-geheimen zie:
- [Service Fabric Mesh-toepassingsgeheimen beheren](service-fabric-mesh-howto-manage-secrets.md)
- [Inleiding tot het Service Fabric-resourcemodel](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png

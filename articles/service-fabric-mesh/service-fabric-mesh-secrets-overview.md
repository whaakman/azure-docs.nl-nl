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
ms.openlocfilehash: 4268356db5f46e92862e19d6391cfe5a94511270
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58007480"
---
# <a name="service-fabric-mesh-application-secrets"></a>Service Fabric Mesh-toepassingsgeheimen
Service Fabric Mesh biedt ondersteuning voor geheimen als Azure-resources. Een Service Fabric Mesh-geheim kan bestaan uit gevoelige informatie in tekstvorm, zoals opslagverbindingsreeksen, wachtwoorden of andere waarden die veilig moeten worden opgeslagen en verzonden.

![Overzicht van Mesh-geheimen][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Mesh-geheimenresources
Een Mesh-toepassingsgeheim bestaat uit:
* Een **Geheimen**-resource. Dit is een container die geheimen in tekstvorm bevat. Geheimen die zijn opgenomen in de **Geheimen**-resource, worden veilig opgeslagen en verzonden.
* Een of meer **Geheimen/waarden**-resources die zijn opgeslagen in de **Geheimen**-resourcecontainer. Elke **Geheimen/waarden**-resource wordt gekenmerkt door een uniek versienummer.

## <a name="next-steps"></a>Volgende stappen 
Voor meer informatie over Service Fabric Mesh-geheimen zie:
- [Service Fabric Mesh-toepassingsgeheimen beheren](service-fabric-mesh-howto-manage-secrets.md)
- [Inleiding tot het Service Fabric-resourcemodel](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png

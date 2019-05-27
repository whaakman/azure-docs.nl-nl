---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/19/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ed7bfca6095dbb03042efd14456f34556f74a843
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66115655"
---
U wordt gefactureerd voor Azure Storage op basis van uw gebruik van storage-account. Alle objecten in een opslagaccount worden samen gefactureerd als een groep. 

Opslagkosten worden berekend op basis van de volgende factoren: regio/locatie, accounttype, toegangslaag, opslagcapaciteit, replicatieschema, opslagtransacties en uitgaande gegevens.

* **Regio** verwijst naar de geografische regio waarin uw account is gebaseerd.
* **Accounttype** verwijst naar het type opslagaccount dat u gebruikt. 
* **Toegangslaag** verwijst naar het patroon voor het gebruik van gegevens u hebt opgegeven voor uw voor algemeen gebruik v2- of Blob storage-account.
* Opslag **capaciteit** verwijst naar welk deel van uw opslag aandeel u bent gebruiken voor het opslaan van gegevens.
* **Replicatie** bepaalt hoeveel kopieën van uw gegevens worden bewaard in één keer, en op welke locaties.
* **Transacties** verwijzen naar alle lees- en schrijfbewerkingen voor opslag van Azure.
* **Uitgaande gegevens** verwijst naar alle gegevens die overgedragen vanuit een Azure-regio. Wanneer de gegevens in uw opslagaccount worden geopend door een toepassing die niet wordt uitgevoerd in dezelfde regio, wordt er een bedrag in rekening gebracht voor uitgaande gegevens. Zie voor meer informatie over het gebruik van resourcegroepen voor het groeperen van uw gegevens en services in dezelfde regio te beperken van de kosten voor uitgaand verkeer [wat is er een Azure-resourcegroep?](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group). 

Op de pagina [Prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/) vindt u gedetailleerde prijsinformatie op basis van accounttype, opslagcapaciteit, replicatie en transacties. In [Prijsinformatie over gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) vindt u gedetailleerde informatie over de prijzen voor uitgaande gegevens. Met de [Prijscalculator van Azure Storage](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) kunt u een schatting maken van uw kosten.


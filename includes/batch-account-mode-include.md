---
title: bestand opnemen
description: bestand opnemen
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: b5bb5da2b68bae36353b81a04993814b395ce67b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176389"
---
> [!NOTE]
> Wanneer u een Batch-account maakt, kunt u kiezen tussen twee modi voor *groepstoewijzing*: **gebruikersabonnement** en **Batch-service**. In de meeste gevallen zult u de standaardmodus Batch-service gebruiken, waarbij pools achter de schermen worden toegewezen in door Azure beheerde abonnementen. In de alternatieve modus Gebruikersabonnement worden Batch-VM's en andere resources rechtstreeks in uw abonnement gemaakt wanneer er een groep wordt gemaakt. De modus Gebruikersabonnement is vereist als u Batch-wilt maken met behulp van pools [Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/). Voor het maken van een Batch-account in de gebruikersabonnementmodus moet u het account ook bij Azure Batch registreren en aan een Azure Key Vault koppelen.
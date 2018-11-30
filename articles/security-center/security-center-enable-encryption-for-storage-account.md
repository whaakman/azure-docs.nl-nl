---
title: Versleuteling inschakelen voor storage-account in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe u de aanbevelingen voor Azure Security Center implementeren **versleuteling inschakelen voor Azure Storage-Account**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 7bd6b6fa82d2c7c8129f7e88df6803b697bfc6d6
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52307410"
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Versleuteling inschakelen voor Azure storage-account in Azure Security Center
Azure Security Center kan aanraden om Azure Storage-Serviceversleuteling voor data-at-rest in te schakelen.

Storage Service Encryption (SSE) werkt door de gegevens te coderen wanneer ze worden geschreven naar Azure storage en decoderen van de gegevens voor het ophalen van.  SSE is momenteel alleen beschikbaar voor de Azure Blob-service en kan worden gebruikt voor blok-blobs, pagina-blobs en toevoeg-blobs.  Zie voor meer informatie, [Storage Service Encryption voor data-at-rest](../storage/common/storage-service-encryption.md).


> [!Note]
> Nadat versleuteling is ingeschakeld, worden alleen nieuwe gegevens versleuteld. Alle bestaande blobs in uw storage-account blijven onversleuteld. Voor het versleutelen van bestaande blobs, Zie de [Veelgestelde vragen over Storage Service Encryption](../storage/common/storage-service-encryption.md#faq-for-storage-service-encryption).
>
>

Storage Service-versleuteling wordt alleen ondersteund op Resource Manager-opslagaccounts. Klassieke opslagaccounts worden momenteel niet ondersteund. Zie voor meer informatie over de klassieke en Resource Manager-implementatiemodel, [Azure-implementatiemodellen](../azure-classic-rm.md).

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. In de **aanbevelingen** Selecteer **versleuteling inschakelen voor Azure Storage-Account**.
   ![Versleuteling inschakelen voor opslagaccount][1]
2. De **storage-versleuteling inschakelt** blade wordt geopend. Deze blade bevat de Azure storage-accounts waarbij versleuteling van opslag is uitgeschakeld. In dit voorbeeld gaan we selecteren **storageacct1**.
   ![Opslagversleuteling inschakelen][2]
3. De **versleuteling** blade voor **storageacct1** wordt geopend. Selecteer **ingeschakeld**.
   ![Versleuteling-blade][3]
4. Selecteer **Opslaan**.

U hebt nu versleuteling van opslag voor ingeschakeld **storageacct1**.


## <a name="see-also"></a>Zie ook
Dit document hebt u geleerd hoe u de aanbeveling van Security Center implementeren "Versleuteling inschakelen voor Azure Storage-Account." Voor meer informatie over Azure Storage-Serviceversleuteling, Zie de volgende:

* [Azure Storage-Serviceversleuteling voor Data-at-Rest](../storage/common/storage-service-encryption.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-azure-policy.md) -informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) -informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) -informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) -informatie over hoe aanbevelingen u helpen bij uw Azure-resources te beveiligen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md) -Raadpleeg Veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) : Lees blogberichten over de Azure-beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png

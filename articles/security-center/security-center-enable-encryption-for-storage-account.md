---
title: Schakel versleuteling voor storage-account in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe de Azure Security Center aanbevelingen implementeren ** versleuteling voor Azure Storage Account ** inschakelen.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: terrylan
ms.openlocfilehash: b7b2e8a12cbab68da9c8fcc348e8e3c543607007
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="enable-encryption-for-azure-storage-account-in-azure-security-center"></a>Schakel versleuteling voor Azure storage-account in Azure Security Center
Azure Security Center kunt u het beste inschakelen Azure Storage-Service: versleuteling voor gegevens in rust.

Versleuteling voor opslag-Service (SSE) werkt door de gegevens te coderen wanneer deze is geschreven naar Azure storage en ontsleutelen van de gegevens voordat ophalen.  SSE is momenteel alleen beschikbaar voor de Azure Blob-service en kan worden gebruikt voor blok-blobs, pagina-blobs en toevoeg-blobs.  Zie voor meer informatie, [Service versleuteling van opslag voor gegevens in rust](../storage/common/storage-service-encryption.md).


> [!Note]
> Nadat de codering is ingeschakeld, worden alleen nieuwe gegevens worden versleuteld. Alle bestaande blobs in uw opslagaccount blijven onversleuteld. Voor het versleutelen van bestaande blobs, Zie de [opslag Service versleuteling Veelgestelde vragen over](../storage/common/storage-service-encryption.md#frequently-asked-questions-about-storage-service-encryption-for-data-at-rest).
>
>

Versleuteling van de opslagruimte wordt alleen ondersteund op Resource Manager storage-accounts. Klassieke opslagaccounts worden momenteel niet ondersteund. Zie inzicht in het klassieke en het Resource Manager-implementatiemodel [Azure-implementatiemodellen](../azure-classic-rm.md).

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit document is niet een stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **aanbevelingen** blade Selecteer **Schakel versleuteling voor Azure Storage-Account**.
   ![Versleuteling inschakelen voor opslagaccount][1]
2. De **inschakelen van versleuteling van opslag** blade wordt geopend. Deze blade bevat de Azure storage-accounts waar versleuteling van opslag is uitgeschakeld. In dit voorbeeld gaan we selecteren **storageacct1**.
   ![Versleuteling van opslag inschakelen][2]
3. De **versleuteling** blade voor **storageacct1** wordt geopend. Selecteer **ingeschakeld**.
   ![Codering-blade][3]
4. Selecteer **Opslaan**.

U hebt nu de versleuteling van opslag voor ingeschakeld **storageacct1**.


## <a name="see-also"></a>Zie ook
Dit document hebt u geleerd hoe u de aanbeveling Security Center implementeert "Schakel versleuteling voor Azure Storage-Account." Voor meer informatie over Azure Storage-Service: versleuteling, Zie de volgende:

* [Azure Storage-Service: versleuteling voor gegevens in rust](../storage/common/storage-service-encryption.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) -informatie over het beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen configureren.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) -informatie over het bewaken van de status van uw Azure-resources.
* [Het beheren van en reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) -informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) -Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md) -Raadpleeg Veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) -Lees blogberichten over Azure-beveiliging en naleving.

<!--Image references-->
[1]: ./media/security-center-enable-encryption-for-storage-account/enable-encryption-for-storage-account.png
[2]: ./media/security-center-enable-encryption-for-storage-account/enable-storage-encryption.png
[3]: ./media/security-center-enable-encryption-for-storage-account/encryption-blade.png

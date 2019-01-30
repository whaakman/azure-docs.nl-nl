---
title: Storage-accounts in Azure Stack | Microsoft Docs
description: Informatie over het maken van een Azure Stack-opslagaccount.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 1/18/2019
ms.author: mabrigg
ms.lastreviewed: 1/18/2019
ms.openlocfilehash: 4123d4cec25bab116c642f1b89cd8eff4779af42
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55252146"
---
# <a name="storage-accounts-in-azure-stack"></a>Opslagaccounts in Azure Stack
Opslagaccounts bevatten Blob- en Tabelservices en de unieke naamruimte voor de objecten van uw opslaggegevens. De gegevens in uw account zijn standaard alleen beschikbaar voor u, de eigenaar van het opslagaccount.

1. Op de Azure Stack POC-computer, moet u zich aanmelden bij `https://adminportal.local.azurestack.external` als [beheerder](azure-stack-connect-azure-stack.md), en klik vervolgens op **+ een resource maken** > **gegevens en opslag**  >  **Opslagaccount**.

   ![](media/azure-stack-provision-storage-account/image01.png)
2. In de **storage-account maken** blade, typ een naam voor uw storage-account. Maak een nieuwe **resourcegroep**, of Selecteer een bestaande resourcegroep en klik vervolgens op **maken** om het opslagaccount te maken.

   ![](media/azure-stack-provision-storage-account/image02.png)
3. Voor uw nieuwe storage-account, klikt u op **alle resources**, zoek vervolgens naar het storage-account en klik op de naam.

    ![](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Volgende stappen
[Azure Resource Manager-sjablonen gebruiken](user/azure-stack-arm-templates.md)

[Meer informatie over Azure storage-accounts](../storage/common/storage-create-storage-account.md)

[Download de handleiding Azure Stack Azure consistente validatie](https://aka.ms/azurestacktp1doc)

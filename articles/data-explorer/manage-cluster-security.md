---
title: Uw cluster beveiligen in azure Data Explorer
description: In dit artikel wordt beschreven hoe u uw cluster in azure Data Explorer kunt beveiligen binnen de Azure Portal.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 07/22/2019
ms.openlocfilehash: fbc5b18de093b2c91b17fa310c08a5b02b113a22
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68406520"
---
# <a name="secure-your-cluster-in-azure-data-explorer"></a>Uw cluster beveiligen in azure Data Explorer

[Azure Disk Encryption](/azure/security/azure-security-disk-encryption-overview) helpt uw gegevens te beschermen en beschermen om te voldoen aan de beveiligings-en nalevings verplichtingen van uw organisatie. Het biedt volume versleuteling voor het besturings systeem en de gegevens schijven van de virtuele cluster machines. Het kan ook worden geïntegreerd met [Azure Key Vault](/azure/key-vault/) om u te helpen de versleutelings sleutels en geheimen van de schijf te controleren en te beheren, en om ervoor te zorgen dat alle gegevens op de VM-schijven in rust worden versleuteld tijdens Azure Storage. Met de beveiligings instellingen van uw cluster kunt u schijf versleuteling inschakelen voor uw cluster.
  
## <a name="enable-encryption-at-rest"></a>Versleuteling inschakelen op rest
  
Het inschakelen van [versleuteling in rust](/azure/security/azure-security-encryption-atrest) op uw cluster biedt gegevens beveiliging voor opgeslagen gegevens (op rest). 

1. Ga in het Azure Portal naar de cluster bron van Azure Data Explorer. Selecteer in de kop **instellingen** de optie **beveiliging**. 

    ![Versleuteling inschakelen bij rest](media/manage-cluster-security/security-encryption-at-rest.png)

1. Selecteer in het venster **beveiliging** de optie **aan** voor de beveiligings instelling **schijf versleuteling** . 

1. Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

[De cluster status controleren](/azure/data-explorer/check-cluster-health)

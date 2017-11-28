---
title: Groep machines-evaluatie met Azure migreren | Microsoft Docs
description: Beschrijft hoe u een groep machines voordat u een evaluatie met de service Azure migreren.
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5c279804-aa30-4946-a222-6b77c7aac508
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: 966e0aebf56ab049e898d1787bfdfbb2ef23635e
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2017
---
# <a name="group-machines-for-assessment"></a>Groep machines-evaluatie

In dit artikel wordt beschreven hoe u een groep machines voor beoordeling door [Azure migreren](migrate-overview.md). Azure migreren beoordeelt machines in de groep om te controleren of ze geschikt is voor migratie naar Azure, en biedt sizing en kosten schattingen voor het uitvoeren van de machine in Azure.


## <a name="create-a-group"></a>Een groep maken

1. In de **Dashboard** van het project Azure migreren, klikt u op **groepen** > **+ groep**, en geef een groepsnaam.
2. Een of meer machines toevoegen aan de groep en klik op **maken**. 
3. Desgewenst kunt u instellen dat het uitvoeren van een nieuwe beoordeling van de groep. 

    ![Een groep maken](./media/how-to-create-a-group/create-group.png)

Nadat de groep is gemaakt, kunt u het wijzigen door de groep op de **groepen** pagina, en bij het toevoegen of verwijderen van computers.

## <a name="next-steps"></a>Volgende stappen

- Informatie over het gebruik [machine afhankelijkheid toewijzing](how-to-create-group-machine-dependencies.md) om meer gedetailleerde groepen te maken.
- [Meer informatie](concepts-assessment-calculation.md) over hoe beoordelingen zijn berekend.

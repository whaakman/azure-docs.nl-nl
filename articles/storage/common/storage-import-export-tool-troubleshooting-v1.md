---
title: De Azure Import/Export-hulpprogramma voor probleemoplossing | Microsoft Docs
description: Meer informatie over enkele van de algemene problemen zichtbaar wanneer u de Azure-hulpprogramma voor importeren/exporteren en hoe deze te verwerken.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: b91ca5eb-c557-460a-9afc-0590b38471f9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 7bfda602dbc0ea47828a7c9243b8b9b09ec78432
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-the-azure-importexport-tool"></a>Problemen met het hulpprogramma Azure Import/Export oplossen
Het hulpprogramma Microsoft Azure Import/Export retourneert foutberichten worden weergegeven als deze wordt uitgevoerd in de problemen. Dit onderwerp worden enkele veelvoorkomende problemen die gebruikers kunnen uitvoeren in.  
  
## <a name="a-copy-session-fails-what-i-should-do"></a>Een kopieersessie is mislukt, wat ik moet nemen?  
 Wanneer een sessie kopiëren is mislukt, zijn er twee opties:  
  
 Als de fout herstelbare, bijvoorbeeld als de netwerkshare gedurende een korte periode offline was en nu weer online is, kunt u de kopieersessie hervatten. Als de fout niet herstelbare, bijvoorbeeld als u de map van het verkeerde bron in de opdrachtregelparameters opgegeven is, moet u afbreken van de kopieersessie. Zie [harde schijven voorbereiden voor een Import-taak](../storage-import-export-tool-preparing-hard-drives-import-v1.md) voor meer informatie over wordt hervat en wordt afgebroken sessies kopiëren.  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a>Ik kan hervatten of afbreken van een kopieersessie.  
 Als de sessie kopiëren de eerste kopieersessie voor een station, wordt het foutbericht moet zijn: "de eerste kopieersessie kan niet worden hervat of afgebroken." In dit geval kunt u het oude journaalbestand verwijderen en voer de opdracht opnieuw uit.  
  
 Als een kopieersessie niet het eerste item voor een station, kan altijd hervat of afgebroken.  
  
## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a>Het bestand van het wijzigingslogboek is verbroken, kan ik nog steeds maken de taak?  
 Het logboek-bestand voor een station bevat de volledige gegevens van het kopiëren van gegevens naar dit station en is nodig is voor de bestanden meer toevoegen aan het station en wordt gebruikt voor het maken van een import-taak. Als het journaalbestand verbroken wordt, moet u de kopie-sessies voor het station opnieuw.  
  
## <a name="next-steps"></a>Volgende stappen
 
* [Instellen van het hulpprogramma azure import/export](../storage-import-export-tool-setup-v1.md)   
* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [De taakstatus controleren met kopielogboekbestanden](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [Een importtaak herstellen](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Een exporttaak herstellen](../storage-import-export-tool-repairing-an-export-job-v1.md)

---
title: De Azure Import/Export-hulpprogramma voor probleemoplossing | Microsoft Docs
description: Meer informatie over een aantal van de problemen die regelmatig voorkomen bij gebruik van de Azure Import/Export-hulpprogramma en hoe u deze te verwerken.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 9a4e47143515c7f9c21d701809c35d61853d91ec
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55471909"
---
# <a name="troubleshooting-the-azure-importexport-tool"></a>Problemen met het hulpprogramma Azure Import/Export oplossen
Het Microsoft Azure Import/Export-hulpprogramma geeft als resultaat foutberichten worden weergegeven als deze problemen wordt uitgevoerd. In dit onderwerp worden enkele veelvoorkomende problemen die gebruikers kunnen uitvoeren in.  
  
## <a name="a-copy-session-fails-what-i-should-do"></a>Een sessie kopiëren is mislukt, wat kan ik moet doen?  
 Wanneer een sessie kopiëren is mislukt, zijn er twee opties:  
  
 Als de fout herstelbare, bijvoorbeeld als de netwerkshare voor een korte periode offline is en is nu weer online is, kunt u de kopieersessie hervatten. Als de fout niet herstelbare, bijvoorbeeld als u de map van het verkeerde bron in de opdrachtregelparameters opgegeven is, moet u de sessie kopiëren wordt afgebroken. Zie [harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md) voor meer informatie over het hervatten van de en wordt afgebroken sessies kopiëren.  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a>Ik kan geen hervatten of een kopieersessie afgebroken.  
 Als de sessie kopiëren de eerste kopieersessie voor een station is, moet het foutbericht status: "De eerste kopieersessie kan niet worden hervat of afgebroken." In dit geval kunt u de oude logboekbestand verwijderen en voer de opdracht opnieuw uit.  
  
 Als de sessie van een exemplaar niet het eerste item voor een station is, kan die altijd worden hervat of afgebroken.  
  
## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a>Het logboekbestand is verbroken, kan ik nog steeds maken de taak?  
 Het logboekbestand voor een station bevat de volledige gegevens van het kopiëren van gegevens naar dit station en nodig meer bestanden toevoegen aan het station en wordt gebruikt voor een importtaak maken. Als het logboekbestand gegaan is, moet u de kopie-sessies voor de schijf opnieuw.  
  
## <a name="next-steps"></a>Volgende stappen
 
* [Instellen van het hulpprogramma azure import/export](../storage-import-export-tool-setup-v1.md)   
* [Harde schijven voorbereiden voor een importtaak](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [De taakstatus controleren met kopielogboekbestanden](../storage-import-export-tool-reviewing-job-status-v1.md)   
* [Een importtaak herstellen](../storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Een exporttaak herstellen](../storage-import-export-tool-repairing-an-export-job-v1.md)

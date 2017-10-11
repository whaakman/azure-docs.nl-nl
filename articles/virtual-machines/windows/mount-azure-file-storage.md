---
title: Bestandsopslag op Azure koppelen vanuit een Windows Azure VM | Microsoft Docs
description: Bestand opslaan in de cloud met Azure file storage en uw cloud-bestandsshare koppelen vanuit Azure een virtuele machine (VM).
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 6ffb2d2da1e2439df6f5da543411e3c2c68d3435
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Azure-bestandsshares gebruiken met Windows VM 's 

U kunt Azure-bestandsshares gebruiken als een manier om bestanden opslaan en openen van de virtuele machine. U kunt bijvoorbeeld een script of een toepassingsconfiguratiebestand die u wilt dat alle virtuele machines te delen opslaan. In dit onderwerp zien we u hoe maken en koppelen van een Azure-bestandsshare en hoe bestanden uploaden en downloaden.

## <a name="connect-to-a-file-share-from-a-vm"></a>Verbinding maken met een bestandsshare vanaf een virtuele machine

Deze sectie wordt ervan uitgegaan dat u hebt al een bestandsshare die u verbinding wilt maken. Als u maken wilt, Zie [een bestandsshare maken](#create-a-file-share) verderop in dit onderwerp.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Selecteer een bestandsshare.
6. Klik op **Connect** opent een pagina met de syntaxis van de opdrachtregel voor het koppelen van de bestandsshare vanuit Windows of Linux.
7. Markeer de syntaxis van de opdracht en plak deze in Kladblok of ergens anders waar u eenvoudig toegang toe. 
8. De syntaxis voor het verwijderen van de voorloopspaties bewerken ** > ** en vervang *[stationsaanduiding]* met de stationsletter (bijvoorbeeld **Y:**) waarin u wilt de bestandsshare koppelen.
8. Verbinding maken met uw virtuele machine en open een opdrachtprompt.
9. Plak in de bewerkte verbinding-syntaxis en druk op **Enter**.
10. Als de verbinding is gemaakt, krijgt u het bericht **de opdracht is voltooid.**
11. Controleer de verbinding door te typen in de stationsletter overschakelen naar dat station en typ vervolgens **dir** om te zien van de inhoud van de bestandsshare.



## <a name="create-a-file-share"></a>Een bestandsshare maken 
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Klik op de pagina File-Service op **+ bestandsshare** om uw eerste bestandsshare te maken. \
6. Vul in de naam van de bestandsshare. De namen van bestandsshares kunnen gebruiken, kleine letters, cijfers en afbreekstreepjes één. De naam mag niet beginnen met een afbreekstreepje en u kunt niet meerdere, gebruiken afbreekstreepjes achter elkaar voorkomen. 
7. Vul in een limiet op hoe lang het bestand kunnen zijn, maximaal 5120 GB.
8. Klik op **OK** voor het implementeren van de bestandsshare.
   
## <a name="upload-files"></a>Bestanden uploaden
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Selecteer een bestandsshare.
6. Klik op **uploaden** openen de **bestanden uploaden** pagina.
7. Klik op het pictogram van de map te bladeren naar een bestand voor het uploaden van het lokale bestandssysteem.   
8. Klik op **uploaden** het bestand te uploaden naar de bestandsshare.

## <a name="download-files"></a>Bestanden downloaden
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Selecteer een bestandsshare.
6. Met de rechtermuisknop op het bestand en kies **downloaden** om deze te downloaden naar uw lokale computer.
   

## <a name="next-steps"></a>Volgende stappen

U kunt ook maken en beheren van bestandsshares met behulp van PowerShell. Zie voor meer informatie [aan de slag met Azure File storage in Windows](../../storage/files/storage-dotnet-how-to-use-files.md).

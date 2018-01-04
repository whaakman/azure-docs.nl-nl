---
title: Bestandsopslag op Azure koppelen vanuit een Windows Azure VM | Microsoft Docs
description: Bestand opslaan in de cloud met Azure file storage en uw cloud-bestandsshare koppelen vanuit Azure een virtuele machine (VM).
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.openlocfilehash: 8d537bdc882487784baef9f693e4677c76d3bd8d
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Azure-bestandsshares gebruiken met Windows VM 's 

U kunt Azure-bestandsshares gebruiken als een manier om bestanden opslaan en openen van de virtuele machine. U kunt bijvoorbeeld een script of een toepassingsconfiguratiebestand die u wilt dat alle virtuele machines te delen opslaan. In dit artikel zien we u hoe maken en koppelen van een Azure-bestandsshare en hoe bestanden uploaden en downloaden.

## <a name="connect-to-a-file-share-from-a-vm"></a>Verbinding maken met een bestandsshare vanaf een virtuele machine

Deze sectie wordt ervan uitgegaan dat u hebt al een bestandsshare die u verbinding wilt maken. Als u maken wilt, Zie [een bestandsshare maken](#create-a-file-share) verderop in dit artikel.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Selecteer een bestandsshare of klik op **+ bestandsshare** voor het maken van een nieuwe bestandsshare te gebruiken.
6. Klik op **Connect** opent een pagina met de syntaxis van de opdrachtregel voor het koppelen van de bestandsshare vanuit Windows of Linux.
7. In **stationsletter**, selecteer de letter die u gebruiken wilt voor het identificeren van het station.
8. Kies welke syntaxis om te gebruiken en klik op de knop kopiëren aan de rechterkant om deze te kopiëren naar het Klembord. Plak het enige plaats waar u eenvoudig toegang toe. 
8. Verbinding maken met uw virtuele machine en open een opdrachtprompt.
9. Plak in de bewerkte verbinding-syntaxis en druk op **Enter**.
10. Als de verbinding is gemaakt, krijgt u het bericht **de opdracht is voltooid.**
11. Controleer de verbinding door te typen in de stationsletter overschakelen naar dat station en typ vervolgens **dir** om te zien van de inhoud van de bestandsshare.



## <a name="create-a-file-share"></a>Een bestandsshare maken 
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Klik op de pagina File-Service op **+ bestandsshare**.
6. Vul in de naam van de bestandsshare. De namen van bestandsshares kunnen gebruiken, kleine letters, cijfers en afbreekstreepjes één. De naam mag niet beginnen met een afbreekstreepje en u kunt niet meerdere, gebruiken afbreekstreepjes achter elkaar voorkomen. 
7. Vul in een limiet op hoe lang het bestand kunnen zijn, maximaal 5120 GB.
8. Klik op **OK** om de bestandsshare te maken.
   
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

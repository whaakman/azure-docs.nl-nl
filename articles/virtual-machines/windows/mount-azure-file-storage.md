---
title: Azure file storage koppelen vanuit een Windows VM in Azure | Microsoft Docs
description: Bestand Store in de cloud met Azure file storage en uw cloud-bestandsshare koppelen vanuit een Azure virtuele machine (VM).
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.subservice: files
ms.openlocfilehash: d79a2d33157d9e2a6f56eb18d061f71a7dcd635c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462236"
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Azure-bestandsshares gebruiken met Windows-VM 's 

U kunt Azure-bestandsshares gebruiken als een manier voor het opslaan en toegang krijgen tot bestanden van uw virtuele machine. U kunt bijvoorbeeld een script of een toepassing-configuratiebestand dat u wilt dat alle virtuele machines voor het delen van opslaan. In dit artikel laten we zien u hoe u maakt en een Azure-bestandsshare koppelen en hoe u bestanden uploaden en downloaden.

## <a name="connect-to-a-file-share-from-a-vm"></a>Verbinding maken met een bestandsshare vanaf een virtuele machine

In deze sectie wordt ervan uitgegaan dat u hebt al een bestandsshare die u wilt verbinden. Als u maken wilt, Zie [een bestandsshare maken](#create-a-file-share) verderop in dit artikel.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Selecteer een bestandsshare of klik op **+ bestandsshare** te maken van een nieuwe bestandsshare te gebruiken.
6. Klik op **Connect** om een pagina met de syntaxis van de opdrachtregel voor het koppelen van de bestandsshare vanuit Windows of Linux te openen.
7. In **stationsletter**, selecteert u de stationsletter die u wilt gebruiken voor het identificeren van het station.
8. Kies welke syntaxis voor het gebruik en selecteer de knop kopiëren aan de rechterkant naar het Klembord kopiëren. Plak deze als een plaats waar u deze eenvoudig kunt gebruiken. 
8. Verbinding maken met uw virtuele machine en open een opdrachtprompt.
9. Plak in de syntaxis van de bewerkte verbinding en druk op **Enter**.
10. Wanneer de verbinding is gemaakt, krijgt u het bericht **de opdracht is voltooid.**
11. Controleer de verbinding door te typen in de stationsletter overschakelen naar dat station en typ vervolgens **dir** om te zien van de inhoud van de bestandsshare.



## <a name="create-a-file-share"></a>Een bestandsshare maken 
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Klik op de pagina File-Service op **+ bestandsshare**.
6. Vul de naam van bestandsshare. Namen van bestandsshares kunnen gebruiken, kleine letters, cijfers en enkele afbreekstreepjes bevatten. De naam mag niet beginnen met een afbreekstreepje en u kunt geen meerdere opeenvolgende afbreekstreepjes bevatten. 
7. Vul een limiet op hoe groot het bestand kan worden, maximaal 5120 GB.
8. Klik op **OK** om de bestandsshare te maken.
   
## <a name="upload-files"></a>Bestanden uploaden
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Selecteer een bestandsshare.
6. Klik op **uploaden** openen de **bestanden uploaden** pagina.
7. Klik op het mappictogram om te bladeren in uw lokale bestandssysteem voor een bestand te uploaden.   
8. Klik op **uploaden** het bestand te uploaden naar de bestandsshare.

## <a name="download-files"></a>Bestanden downloaden
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **opslagaccounts**.
3. Kies uw opslagaccount.
4. In de **overzicht** pagina onder **Services**, selecteer **bestanden**.
5. Selecteer een bestandsshare.
6. Met de rechtermuisknop op het bestand en kies **downloaden** om deze te downloaden naar uw lokale computer.
   

## <a name="next-steps"></a>Volgende stappen

U kunt ook maken en beheren van bestandsshares met behulp van PowerShell. Zie voor meer informatie, [aan de slag met Azure File storage in Windows](../../storage/files/storage-dotnet-how-to-use-files.md).

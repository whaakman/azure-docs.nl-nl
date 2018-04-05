---
title: storage-files-create-storage-account-portal
description: Leer hoe u een opslagaccount maakt voor Azure Files.
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 03/28/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 82e141eb4941dbd58be95e19dae186acd18caf94
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
Een opslagaccount is een gedeelde opslaggroep waarin u Azure-bestandsshares of andere opslagresources, zoals blobs of wachtrijen, kunt implementeren. Een opslagaccount kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden bevatten, totdat de capaciteit van het opslagaccount is bereikt.

Een opslagaccount maken:

1. Klik in het menu links op **+** om een resource te maken.
2. Typ **opslagaccount** in het zoekvak (1) en selecteer **Opslagaccount - blob, bestand, tabel, wachtrij** (2) en klik vervolgens op **Maken**.
    ![Een schermopname van hoe de invoer voor het opslagaccount eruit moet zien in het dialoogvenster voor het zoeken van resources](../articles/storage/files/media/storage-how-to-use-files-portal/create-storage-account-1.png)

3. Typ *mystorageacct* in het vak **Naam**, gevolgd door een paar willekeurige cijfers totdat u een groen vinkje ziet en u weet dat dit een unieke naam is. De naam van een opslagaccount mag alleen kleine letters bevatten en moet globaal uniek zijn. Noteer de naam van het opslagaccount want deze hebt u later nodig. 
4. Laat bij **Implementatiemodel** de standaardwaarde **Resource Manager** geselecteerd. Meer informatie over de verschillen tussen Azure Resource Manager en de klassieke implementatie vindt u in [Implementatiemodellen en de status van uw resources begrijpen](../articles/azure-resource-manager/resource-manager-deployment-model.md).
5. Selecteer **StorageV2** bij **Soort account**. Zie [Opties voor Azure Storage-account](../articles/storage/common/storage-account-options.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor meer informatie over de verschillende soorten opslagaccounts.
6. Laat bij **Prestaties** de standaardwaarde **Standard** geselecteerd. Azure Files ondersteunt momenteel alleen standaardopslag. Zelfs als u premium-opslag selecteert, wordt de bestandsshare opgeslagen in standaardopslag.
7. Selecteer **Lokaal redundante opslag (LRS)** bij *Replicatie*. 
8. Wij adviseren om bij **Veilige overdracht vereist** altijd **Ingeschakeld** te selecteren. Zie [Veilige overdracht in Azure Storage vereisen](../articles/storage/common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor meer informatie over deze optie.
9. Selecteer bij **Abonnement** het abonnement waarin u het opslagaccount wilt maken. Als u slechts één abonnement hebt, wordt dit abonnement standaard weergegeven.
10. Selecteer bij **Resourcegroep** het keuzerondje **Nieuw** en voer *myResourceGroup* in als de naam.
11. Selecteer bij **Locatie** **Oostelijke VS**.
12. Laat bij **Virtuele netwerken** de standaardoptie *Uitgeschakeld* staan. 
13. Selecteer **Vastmaken aan dashboard** om het opslagaccount gemakkelijker te vinden.
14. Als u klaar bent, klikt u op **Maken** om de implementatie te starten.
---
title: Machtigingen instellen voor Data Lake Storage Gen2 met Azure Storage Explorer
description: In deze instructie leert u hoe u machtigingen instelt met Azure Storage Explorer voor bestanden en mappen in uw voor Azure Data Lake Storage Gen2 geschikte opslagaccount.
services: storage
author: roygara
ms.custom: mvc
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: rogarana
ms.openlocfilehash: f2569b29ab6124f1cfa22fa745d45082c213a6be
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60627886"
---
# <a name="set-file-and-directory-level-permissions-using-azure-storage-explorer-with-azure-data-lake-storage-gen2"></a>Machtigingen op bestands- en mapniveau instellen voor Azure Data Lake Storage Gen2 met behulp van Azure Storage Explorer

Bestanden die zijn opgeslagen in Azure Data Lake Storage Gen2 bieden ondersteuning voor gedetailleerde machtigingen en ACL-beheer (ACL: Access Control List, toegangsbeheerlijst). Dankzij de combinatie van gedetailleerde machtigingen en ACL-beheer kunt u de toegang tot uw gegevens op een zeer gedetailleerd niveau beheren.

In dit artikel leert u hoe u Azure Storage Explorer kunt gebruiken om:

> [!div class="checklist"]
> * machtigingen op bestandsniveau in te stellen
> * machtigingen op mapniveau in te stellen
> * gebruikers of groepen toe te voegen aan een toegangsbeheerlijst

## <a name="prerequisites"></a>Vereisten

Voor de beste weergave van het proces vragen wij u om onze [snelstart voor Azure Storage Explorer](data-lake-storage-Explorer.md) te voltooien. Dit zorgt ervoor dat uw storage-account wordt in de meest geschikte status (bestandssysteem gemaakt en geüpload naar deze gegevens).

## <a name="managing-access"></a>Toegang beheren

U kunt machtigingen instellen in de hoofdmap van uw bestandssysteem. Om dit te doen, moet u in Azure Storage Explorer zijn aangemeld met uw afzonderlijke account met machtigingen om dit te doen (in plaats van met een verbindingsreeks). Met de rechtermuisknop op uw bestandssysteem en selecteer **machtigingen beheren**, brengen van de **beheren machtiging** in het dialoogvenster.

![Microsoft Azure Storage Explorer - Toegang tot mappen beheren](media/storage-quickstart-blobs-storage-Explorer/manageperms.png)

Via het dialoogvenster **Machtiging beheren** kunt u machtigingen beheren voor de eigenaar en de groep eigenaren. Ook kunt u nieuwe gebruikers en groepen toevoegen aan de toegangsbeheerlijst voor wie u vervolgens machtigingen kunt beheren.

Selecteer het veld **Gebruiker of groep toevoegen** om een nieuwe gebruiker of groep toe te voegen aan de toegangsbeheerlijst.

Voer de bijbehorende AAD-vermelding (Azure Active Directory) in die u aan de lijst wilt toevoegen en selecteer vervolgens **Toevoegen**.

De gebruiker of groep wordt nu weergegeven in het veld **Gebruikers en groepen:**, zodat u kunt beginnen met het beheren van hun machtigingen.

> [!NOTE]
> Het is een best practice en het wordt aanbevolen om een beveiligingsgroep in AAD te maken en machtigingen te onderhouden voor de groep in plaats van afzonderlijke gebruikers. Zie [Best practices voor Data Lake Storage Gen2](data-lake-storage-best-practices.md) voor meer informatie over deze aanbeveling en tevens andere best practices.

Er zijn twee machtigingscategorieën die u kunt toewijzen: Toegangs-ACL’s en Standaard-ACL’s.

* **Toegang**: Met toegangs-ACL's beheert u de toegang tot een object. Bestanden en mappen hebben beide Toegangs-ACL's.

* **Standaard**: Een sjabloon van ACL's die zijn gekoppeld aan een map waarmee de Toegangs-ACL's worden bepaald voor alle onderliggende items die zijn gemaakt onder die map. Bestanden hebben geen Standaard-ACL's.

In beide van deze categorieën zijn er drie machtigingen die u vervolgens aan bestanden of mappen kunt toewijzen: **Lezen**, **Schrijven** en **Uitvoeren**.

>[!NOTE]
> Wanneer u hier selecties maakt, worden machtigingen voor een reeds bestaand item in de map niet ingesteld. U moet naar elk afzonderlijk item gaan en de machtigingen handmatig instellen als het bestand al bestaat.

U kunt machtigingen voor afzonderlijke mappen beheren, evenals de afzonderlijke bestanden waarmee voor u een gedetailleerd toegangsbeheer mogelijk wordt. Het proces voor het beheren van machtigingen voor zowel mappen als bestanden is hetzelfde als hierboven beschreven. Klik met de rechtermuisknop op het bestand of map waarvoor u machtigingen wilt beheren en volg hetzelfde proces.

## <a name="next-steps"></a>Volgende stappen

In deze instructies hebt u geleerd hoe u machtigingen kunt instellen voor bestanden en mappen met behulp van **Azure Storage Explorer**. Voor meer informatie over ACL's, met inbegrip van Standaard-ACL's, Toegangs-ACL's, hun gedrag en de bijbehorende machtigingen gaat u verder met ons conceptuele artikel over het onderwerp.

> [!div class="nextstepaction"]
> [Toegangsbeheer in Data Lake Storage Gen2](data-lake-storage-access-control.md)

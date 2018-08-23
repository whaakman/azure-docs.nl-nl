---
title: Toevoegen van Azure Storage met behulp van Connected Services in Visual Studio | Microsoft Docs
description: Azure-opslag toevoegen aan uw app met behulp van het dialoogvenster Visual Studio verbonden Services toevoegen
services: visual-studio-online
author: ghogen
manager: douge
assetId: 521ec044-ad4b-4828-8864-01decde2e758
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/26/2017
ms.author: ghogen
ms.openlocfilehash: 73b9a1725ada07835db518b57140a2e4d43e143a
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059492"
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Azure storage toevoegen met behulp van Visual Studio Connected Services
Met Visual Studio, kunt u een van de volgende Azure Storage met behulp van de **Connected Services toevoegen** dialoogvenster:

- C#-cloudservice
- .NET-back-end mobiele service
- ASP.NET-website of service
- ASP.NET Core-service
- Azure WebJob-service 

De functionaliteit van de gekoppelde service alle de benodigde referenties en de code van de verbinding aan uw project toegevoegd en wijzigt de configuratiebestanden op de juiste wijze. 

Na voltooiing wordt de **Connected Services toevoegen** dialoogvenster automatisch wordt weergegeven over de benodigde de stappen die nodig zijn aan de slag met blob-opslag, wachtrijen en tabellen.

## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Verbinding maken met Azure Storage met behulp van het dialoogvenster Connected Services
1. Open uw project in Visual Studio

1. In **Solution Explorer**, met de rechtermuisknop op de **Connected Services** knooppunt, en klik in het contextmenu en selecteer **Add Connected Service**.
   
    ![Azure toevoegen verbonden service](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. In de **Connected Services** weergeeft, schakelt **opslag in de Cloud met Azure Storage**.
   
    ![Azure Storage toevoegen](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. In de **Azure Storage** dialoogvenster, selecteer een bestaand opslagaccount en selecteert u **toevoegen**.
   
    Als u maken van een storage-account wilt, gaat u naar de volgende stap. Anders gaat u naar stap 6.
    
    ![Bestaande storage-account toevoegen aan project](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. Een opslagaccount maken: 
   
   1. Selecteer **maken van een nieuw Opslagaccount** aan de onderkant van het dialoogvenster.

   1. Vul de **Storage-Account maken** dialoogvenster en selecteer **maken**.
      
       ![Nieuw Azure storage-account](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. Wanneer de **Azure Storage** dialoogvenster wordt weergegeven, wordt het nieuwe opslagaccount wordt weergegeven in de lijst. Selecteer het nieuwe opslagaccount in de lijst en selecteer **toevoegen**.

1. De opslag gekoppelde service weergegeven onder de **Serviceverwijzingen** knooppunt van uw project.
   
## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd
Wanneer u klaar bent met het dialoogvenster, wordt Visual Studio Hiermee voegt u verwijzingen toe en bepaalde configuratiebestanden wijzigt. De specifieke wijzigingen, is afhankelijk van het type: 

- ASP.NET-project - [wat is er gebeurd: ASP.NET-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513126)
- ASP.NET Core-project - [wat is er gebeurd: ASP.NET 5-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513124) 
- Cloud service-project (webrollen en werkrollen) - [wat is er gebeurd – Cloud Service-projecten](http://go.microsoft.com/fwlink/p/?LinkId=516965)
- Webtaak project - [wat is er gebeurd - webtaak projecten](visual-studio/vs-storage-webjobs-what-happened.md)

## <a name="next-steps"></a>Volgende stappen
- [MSDN-Forum: Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [Blog van Microsoft Azure Storage-Team](http://blogs.msdn.com/b/windowsazurestorage/)
- [Documentatie bij Azure Storage](https://docs.microsoft.com/azure/storage/)

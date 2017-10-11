---
title: Azure Storage toevoegen met behulp van verbonden Services in Visual Studio | Microsoft Docs
description: Azure Storage toevoegen aan uw app met behulp van de Visual Studio verbonden dialoogvenster Services toevoegen
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 521ec044-ad4b-4828-8864-01decde2e758
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2017
ms.author: kraigb
ms.openlocfilehash: 35638083cd75e1b751d00a9c8163a3bc7480f0cd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Azure-opslag toe te voegen met behulp van Visual Studio verbonden Services
Met Visual Studio, kunt u een van de volgende naar Azure Storage met behulp van de **verbonden Services toevoegen** dialoogvenster:

- C#-cloudservice
- .NET-back-end voor mobiele service
- ASP.NET-website of service
- ASP.NET Core-service
- Azure webtaak-service 

De functionaliteit van de gekoppelde service worden alle benodigde verwijzingen en verbinding code toegevoegd aan uw project en de configuratiebestanden op de juiste wijze wordt gewijzigd. 

Na voltooiing wordt de **verbonden Services toevoegen** dialoogvenster automatisch wordt weergegeven met gedetailleerde informatie over de stappen die nodig zijn om te gaan werken met blob storage, wachtrijen, documentatie en tabellen.

## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Verbinding maken met Azure Storage met het dialoogvenster Services verbonden
1. Open het project in Visual Studio

1. In **Solution Explorer**, met de rechtermuisknop op de **verbonden Services** knooppunt en klik in het contextmenu en selecteer **verbonden Service toevoegen**.
   
    ![Toevoegen van Azure service verbonden](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. In de **verbonden Services** pagina **Cloud-opslag met Azure Storage**.
   
    ![Azure-opslag toevoegen](./media/vs-azure-tools-connected-services-storage/add-azure-storage.png)

1. In de **Azure Storage** dialoogvenster, selecteer een bestaand opslagaccount en selecteert u **toevoegen**.
   
    Als u een opslagaccount maken wilt, gaat u naar de volgende stap. Ga anders verder met stap 6.
    
    ![Bestaande opslagaccount toevoegen aan project](./media/vs-azure-tools-connected-services-storage/select-azure-storage-account.png)

1. Een opslagaccount maken: 
   
   1. Selecteer **een nieuw Opslagaccount maken** aan de onderkant van het dialoogvenster.

   1. Vul de **Storage-Account maken** dialoogvenster en selecteer **maken**.
      
       ![Nieuwe Azure storage-account](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
   1. Wanneer de **Azure Storage** dialoogvenster wordt weergegeven, wordt het nieuwe opslagaccount wordt weergegeven in de lijst. Selecteer het nieuwe opslagaccount in de lijst en selecteer **toevoegen**.

1. De opslag gekoppelde service wordt weergegeven onder de **Serviceverwijzingen** knooppunt van uw project.
   
## <a name="how-your-project-is-modified"></a>Hoe uw project is gewijzigd
Wanneer u klaar bent met het dialoogvenster, wordt in Visual Studio verwijzingen worden toegevoegd en bepaalde configuratiebestanden wijzigt. De specifieke wijzigingen, is afhankelijk van het type: 

- ASP.NET-project - [wat is er gebeurd – ASP.NET-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513126)
- ASP.NET Core project - [wat is er gebeurd – ASP.NET 5-projecten](http://go.microsoft.com/fwlink/p/?LinkId=513124) 
- Cloudserviceproject (webrollen en werkrollen) - [wat is er gebeurd – Cloud Service-projecten](http://go.microsoft.com/fwlink/p/?LinkId=516965)
- Webtaak project - [wat is er gebeurd - webtaak projecten](visual-studio/vs-storage-webjobs-what-happened.md)

## <a name="next-steps"></a>Volgende stappen
- [MSDN-Forum: Azure-opslag](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
- [Blog van Microsoft Azure Storage-Team](http://blogs.msdn.com/b/windowsazurestorage/)
- [Documentatie bij Azure Storage](https://docs.microsoft.com/azure/storage/)

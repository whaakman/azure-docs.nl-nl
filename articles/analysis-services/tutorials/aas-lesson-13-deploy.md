---
title: 'Azure Analysis Services-zelfstudie - Les 13: Implementeren | Microsoft Docs'
description: In deze les wordt beschreven hoe u het zelfstudieproject implementeert in Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 09/20/2017
ms.author: owend
ms.openlocfilehash: 9594582419c716a37a1010e2410e7b2b199df04c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="lesson-13-deploy"></a>Les 13: Implementeren

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In deze les gaat u eigenschappen voor de implementatie configureren. U gaat een Azure Analysis Services-server voor implementatie en een naam voor het model opgeven. Vervolgens implementeert u het model naar dat exemplaar. Na de implementatie van het model kunnen gebruikers er via een rapportageclienttoepassing verbinding mee maken. Zie [Implementeren naar Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy) voor meer informatie.  
  
Geschatte tijd voor het voltooien van deze les: **5 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 12: Analyseren in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).  

> [!IMPORTANT]  
> U hebt [beheerdersmachtigingen](../analysis-services-server-admins.md) nodig voor de externe Analysis Services-server om hiernaar te kunnen implementeren.  

> [!IMPORTANT]  
> Als u de voorbeelddatabase AdventureWorksDW2014 hebt geïnstalleerd op een on-premises SQL Server en u uw model implementeert naar een Azure Analysis Services-server, is een [on-premises gegevensgateway](../analysis-services-gateway.md) vereist.
  
## <a name="deploy-the-model"></a>Het model implementeren  
  
#### <a name="to-configure-deployment-properties"></a>Implementatie-eigenschappen configureren:  

  
1.  Klik in **Solution Explorer** met de rechtermuisknop op het project **AW Internet Sales** en klik vervolgens op **Properties**.  
  
2.  Voer bij de eigenschap **Server** onder **Deployment Server** in het dialoogvenster **AW Internet Sales Property Pages** de volledige server in.  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  Typ **Adventure Works Internet Sales** voor de eigenschap **Database**.  
  
4.  Typ **Adventure Works Internet Sales Model** voor de eigenschap **Model Name**.  
  
5.  Controleer de invoer en klik vervolgens op **OK**.  
  
#### <a name="to-deploy-the-adventure-works-internet-sales"></a>Het model Adventure Works Internet Sales implementeren:
  
1.  Klik in **Solution Explorer** met de rechtermuisknop op het project **AW Internet Sales** > **Build**.  

2.  Klik met de rechtermuisknop op het project **AW Internet Sales** > **Deploy**.

    Als u implementeert naar Azure Analysis Services, wordt u mogelijk gevraagd om uw account. Voer in dat geval uw organisatieaccount en het bijbehorende wachtwoord in, zoals nancy@adventureworks.com. Dit account moet een beheerdersaccount zijn op de server.
  
    Het dialoogvenster Deploy wordt weergegeven en toont de implementatiestatus van de metagegevens en tabellen die zijn opgenomen in het model.  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. Als de implementatie is voltooid, kunt u op **Close** klikken.  
  
## <a name="conclusion"></a>Conclusie  
Gefeliciteerd. U bent klaar met het ontwerpen en implementeren van uw eerste tabellaire model van Analysis Services. Deze zelfstudie heeft u stapsgewijs begeleid bij het uitvoeren van de algemene taken voor het maken van een tabellair model. Het model Adventure Works Internet Sales is nu geïmplementeerd. U kunt nu SQL Server Management Studio gebruiken om het model te beheren, en processcripts en een back-upplan te maken. Gebruikers kunnen nu ook verbinding maken met het model met behulp van een rapportageclienttoepassing zoals Microsoft Excel of Power BI.  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a>Volgende stappen
[Verbinden met Power BI Desktop](../analysis-services-connect-pbi.md)   
[Aanvullende les: Dynamische beveiliging](../tutorials/aas-supplemental-lesson-dynamic-security.md)   
[Aanvullende les: Detailrijen](../tutorials/aas-supplemental-lesson-detail-rows.md)   
[Aanvullende les: Onregelmatige hiërarchieën](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   

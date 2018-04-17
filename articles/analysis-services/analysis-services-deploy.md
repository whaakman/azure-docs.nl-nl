---
title: Implementeren naar Azure Analysis Services met behulp van SSDT | Microsoft Docs
description: Leer hoe u een tabellair model met behulp van SSDT implementeert op een Azure Analysis Services-server.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a25066ef8446449148bc0ca95989dc6ca3ca6839
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="deploy-a-model-from-ssdt"></a>Een model implementeren vanuit SSDT
Wanneer u een server in uw Azure-abonnement hebt gemaakt, kunt u een tabellaire modeldatabase op de server implementeren. U kunt SQL Server Data Tools (SSDT) gebruiken om een project voor een tabellair model te bouwen en implementeren waaraan u werkt. 

## <a name="prerequisites"></a>Vereisten
Om aan de slag te gaan, hebt u het volgende nodig:

* **Analysis Services-server** in Azure. Zie [Create an Azure Analysis Services server in Azure portal](analysis-services-create-server.md) (Een Azure Analysis Services-server maken in Azure Portal) voor meer informatie.
* **Project voor tabellair model** in SSDT of een bestaand tabellair model op het compatibiliteitsniveau 1200 of hoger. Nog nooit zo'n project gemaakt? Raadpleeg de [Adventure Works Internet sales tabular modeling tutorial](https://msdn.microsoft.com/library/hh231691.aspx) (Zelfstudie voor tabellaire modellen bij Adventure Works-internetverkoop).
* **On-premises gateway**: als een of meer gegevensbronnen on-premises zijn opgeslagen in het netwerk van uw organisatie, moet u een [on-premises gegevensgateway](analysis-services-gateway.md) installeren. De gateway is nodig om uw server in de cloud verbinding te laten maken met uw on-premises gegevensbronnen om gegevens in het model te verwerken en te vernieuwen.

> [!TIP]
> Voer de implementatie pas uit als u weet dat u de gegevens in de tabellen kunt verwerken. Klik in SSDT op **Model** > **Process** > **Process All**. Als de verwerking mislukt, kunt u niet implementeren.
> 
> 

## <a name="to-deploy-a-tabular-model-from-ssdt"></a>Een tabellair model implementeren vanuit SSDT

1. Voordat u kunt gaan implementeren, moet u de naam van de server weten. In **Azure Portal** > server > **Overview** > **Servernaam**,kopieer de servernaam.
   
    ![Servernaam bepalen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. In SSDT > **Solution Explorer**, klik met de rechtermuisknop op het project > **Properties**. Plak vervolgens in **Deployment** > **Server** de naam van de server.   
   
    ![Servernaam plakken in de eigenschap Deployment Server](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. Klik in **Solution Explorer** met de rechtermuisknop op **Properties** en klik vervolgens op **Deploy**. U wordt mogelijk gevraagd u aan te melden bij Azure.
   
    ![Implementeren op server](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    De status van de implementatie wordt weergegeven in het uitvoervenster en in het venster Deploy.
   
    ![Implementatiestatus](./media/analysis-services-deploy/aas-deploy-status.png)

Dat is alles!


## <a name="troubleshooting"></a>Problemen oplossen
Als de implementatie mislukt tijdens het implementeren van metagegevens, is de kans groot dat SSDT geen verbinding kan maken met de server. Zorg ervoor dat u verbinding kunt maken met de server met behulp van SSMS. Controleer vervolgens of de eigenschap Deployment Server voor het project juist is ingesteld.

Als de implementatie mislukt bij een tabel, komt dat waarschijnlijk omdat de server geen verbinding kan maken met een gegevensbron. Als uw gegevensbron on-premises in het netwerk van uw organisatie is opgeslagen, moet u een [on-premises gegevensgateway](analysis-services-gateway.md) installeren.

## <a name="next-steps"></a>Volgende stappen
Uw tabellaire model is nu geïmplementeerd op de server en dus kunt u verbinding gaan maken met het model. Maak [verbinding via SSMS](analysis-services-manage.md) om het model te beheren. [Maak verbinding met een clienthulpprogramma](analysis-services-connect.md) zoals Power BI, Power BI Desktop of Excel om rapporten te gaan maken.


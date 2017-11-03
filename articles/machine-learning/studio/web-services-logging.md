---
title: Logboekregistratie voor Machine Learning-webservices | Microsoft Docs
description: Informatie over het inschakelen van logboekregistratie voor Machine Learning-webservices. Logboekregistratie biedt aanvullende informatie voor het oplossen van de API's.
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: raymondl;garye
ms.openlocfilehash: 31497bcae5889590a2261b716574ec4eae21852d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a>Logboekregistratie inschakelen voor Machine Learning-webservices
Dit document bevat informatie over de mogelijkheid tot het vastleggen van Machine Learning-webservices. Logboekregistratie biedt aanvullende informatie, dan slechts een foutnummer en een bericht die kan helpen bij het oplossen van de aanroepen van de Machine Learning API's.  

## <a name="how-to-enable-logging-for-a-web-service"></a>Het inschakelen van logboekregistratie voor een webservice

Inschakelen van logboekregistratie van het [Azure Machine Learning-webservices](https://services.azureml.net) portal. 

1. Aanmelden bij de portal voor Azure Machine Learning-webservices op [https://services.azureml.net](https://services.azureml.net). Voor een webservice klassiek, u kunt ook opvragen bij de portal door te klikken op **nieuwe Services webervaring** op de pagina Machine Learning-webservices in Machine Learning Studio.

   ![Nieuwe koppeling van de ervaring van de Web-Services](./media/web-services-logging/new-web-services-experience-link.png)

2. In het bovenste menu, klikt u op **webservices** voor een nieuwe webservice of klik op **klassieke webservices** voor een klassiek webservice.

   ![Selecteer Nieuw of klassiek webservices](./media/web-services-logging/select-web-service.png)

3. Een nieuwe webservice, klikt u op de naam van de webservice. Voor een klassieke-webservice, klikt u op de naam van de webservice en klik vervolgens op de volgende pagina op het juiste eindpunt.

4. In het bovenste menu, klikt u op **configureren**.

5. Stel de **logboekregistratie inschakelen** optie naar *fout* (alleen om fouten te registreren) of *alle* (voor volledige logboekregistratie).

   ![Niveau van logboekregistratie selecteren](./media/web-services-logging/enable-logging.png)

6. Klik op **Opslaan**.

7. Voor klassieke webservices, maken de **ml diagnostics** container.

   Alle web service-logboeken worden opgeslagen in een blob-container met de naam **ml diagnostics** in het opslagaccount dat is gekoppeld met de webservice. Deze container gemaakt voor nieuwe web-services de eerste keer dat u toegang de webservice tot. Voor klassieke webservices moet u de container maken als deze niet al bestaat. 

   1. In de [Azure-portal](https://portal.azure.com), gaat u naar het opslagaccount dat is gekoppeld met de webservice.

   2. Onder **Blob-Service**, klikt u op **Containers**.

   3. Als de container **ml diagnostics** niet bestaat, klikt u op **+ Container**, geven de container de naam 'ml-diagnostische gegevens' en selecteer de **toegangstype** als 'Blob'. Klik op **OK**.

      ![Niveau van logboekregistratie selecteren](./media/web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> Voor een webservice klassieke heeft het Dashboard van de Web-Services in Machine Learning Studio ook een switch logboekregistratie in te schakelen. Omdat logboekregistratie wordt nu beheerd via het Web Services-portal, moet u echter inschakelen van logboekregistratie in via de portal, zoals beschreven in dit artikel. Als u al ingeschakeld logboekregistratie in Studio, in de Web Services-Portal-logboekregistratie uitschakelen en opnieuw inschakelen.


## <a name="the-effects-of-enabling-logging"></a>De gevolgen van het inschakelen van logboekregistratie
Als logboekregistratie is ingeschakeld, de diagnostische gegevens en fouten van de webservice-eindpunt worden vastgelegd in de **ml diagnostics** blob-container in Azure Storage-Account gekoppeld aan de gebruiker werkruimte. Deze container bevat de diagnostische gegevens voor alle de webservice-eindpunten voor de werkruimten die zijn gekoppeld aan dit opslagaccount.

De logboeken kunnen worden weergegeven met een van de verschillende beschikbare hulpprogramma's om te verkennen van een Azure Storage-Account. De eenvoudigste manier is mogelijk gaat u naar het opslagaccount in de Azure portal, klikt u op **Containers**, en klik vervolgens op de container **ml diagnostics**.  

## <a name="log-blob-detail-information"></a>Blob-logboekgegevens
Elke blob in de container bevat de diagnostische gegevens voor exact één van de volgende acties:

* Uitvoering van een van de Batch-Execution-methode  
* Uitvoering van een van de methode aanvragen en antwoorden  
* Initialisatie van een container van aanvragen en antwoorden

De naam van elke blob heeft een voorvoegsel van de volgende notatie: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


Waar _type logboek registreren_ is een van de volgende waarden:  

* Batch  
* score/aanvragen  
* score/init  


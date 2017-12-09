---
title: Een webservice implementeren op meerdere gebieden | Microsoft Docs
description: "Stappen voor het implementeren van (kopiëren) een nieuwe Web-Service naar andere regio's."
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: cgronlun
ms.assetid: 36c60411-f2db-4ee2-9b66-b1f1d77a8f44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: abd6f05e9b9ce711ce55e88f07aa13287c76ebc2
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Een webservice implementeren in meerdere regio’s
De nieuwe Azure-Web-Services kunt u eenvoudig een webservice implementeren in meerdere regio's zonder meerdere abonnementen of werkruimten. 

Prijzen is regio specifiek, dat daarom moet u een abonnement voor elke regio waarin u de webservice implementeren.

## <a name="to-create-a-plan-in-another-region"></a>Een planning maken in een andere regio
1. Meld u aan bij [Microsoft Azure Machine Learning-webservices](https://services.azureml.net/).
2. Klik op de **plannen** menuoptie.
3. Klik op de plannen via de weergavepagina **nieuw**.
4. Van de **abonnement** vervolgkeuzelijst, selecteer het abonnement waarin het nieuwe plan wilt maken.
5. Van de **regio** vervolgkeuzelijst, selecteer een regio voor het nieuwe plan. De opties plannen voor de geselecteerde regio wordt weergegeven in de **opties plannen** sectie van de pagina.
6. Van de **resourcegroep** vervolgkeuzelijst, selecteer een resourcegroep voor het plan. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
7. In **naam** typt u de naam van het plan.
8. Onder **Plan opties**, klikt u op het niveau van facturering voor het nieuwe plan.
9. Klik op **Create**.

## <a name="deploying-the-web-service-to-another-region"></a>De webservice implementeren op een andere regio
1. Klik op de **webservices** menuoptie.
2. Selecteer de webservice die u op een nieuw gebied implementeert.
3. Klik op **kopie**.
4. In **Webservicenaam**, typt u een nieuwe naam voor de webservice.
5. In **Web servicebeschrijving**, typt u een beschrijving voor de webservice.
6. Van de **abonnement** vervolgkeuzelijst, selecteer het abonnement waarin de nieuwe webservice wilt maken.
7. Van de **resourcegroep** vervolgkeuzelijst, selecteer een resourcegroep voor de webservice. Zie voor meer informatie over resourcegroepen [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).
8. Van de **regio** vervolgkeuzelijst, selecteer de regio waarin u de webservice implementeren.
9. Van de **opslagaccount** dropdown, selecteert u een opslag-account waarin u voor het opslaan van de webservice.
10. Van de **prijs Plan** dropdown, selecteert u een plan in de regio die u hebt geselecteerd in stap 8.
11. Klik op **kopie**.


---
title: Maken van de webservice-eindpunten in Machine Learning | Microsoft Docs
description: Webservice-eindpunten maken in Azure Machine Learning
services: machine-learning
documentationcenter: 
author: hiteshmadan
manager: padou
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.author: himad
ms.openlocfilehash: 821ad87fc10b2380e5ed89c037c335bc7747009e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="creating-endpoints"></a>Eindpunten maken
> [!NOTE]
>  Dit onderwerp wordt beschreven technieken die van toepassing op een **klassieke** Machine Learning-webservice.
> 
> 

Wanneer u de webservices die u doorsturen naar uw klanten verkoopt hebt gemaakt, moet u getraind modellen bieden voor elke klant die nog steeds zijn gekoppeld aan het experiment waaruit de webservice is gemaakt. Bovendien moeten updates op het experiment worden toegepast selectief een eindpunt zonder aanpassingen wordt overschreven.

Om dit te realiseren, kunt Azure Machine Learning u meerdere eindpunten voor een geïmplementeerde webservice maken. Elk eindpunt in de webservice is onafhankelijk geadresseerd, beperkt en beheerd. Elk eindpunt is een unieke URL en autorisatiesleutel die u naar uw klanten distribueren kunt.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Eindpunten toevoegen aan een webservice
Er zijn drie manieren een eindpunt toevoegen aan een webservice.

* Programmatisch
* Via de portal voor Azure Machine Learning-webservices
* Hoewel de klassieke Azure portal

Zodra het eindpunt is gemaakt, kunt u deze via synchrone API's, batch-API's, wordt gebruikt en excel-werkbladen. U kunt ook het eindpunt Management-API's via programmacode toevoegen van eindpunten naast het toevoegen van eindpunten via deze gebruikersinterface gebruiken.

> [!NOTE]
> Als u extra eindpunten hebt toegevoegd aan de webservice, kunt u het standaardeindpunt niet verwijderen.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>Een eindpunt toevoegen programmatisch
U kunt een eindpunt toevoegen aan uw webservice programmatisch met behulp van de [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) voorbeeldcode.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Toevoegen van een eindpunt met de portal voor Azure Machine Learning-webservices
1. Klik op webservices op de linkernavigatiebalk-kolom in Machine Learning Studio.
2. Klik aan de onderkant van het dashboard van Web service **eindpunten beheren**. De Azure Machine Learning-webservices wordt geopend op de pagina eindpunten voor de webservice.
3. Klik op **Nieuw**.
4. Typ een naam en beschrijving voor het nieuwe eindpunt. Namen van eindpunten moeten 24 tekens of minder lang zijn en moeten bestaan uit kleine letters of cijfers. Selecteer het niveau van logboekregistratie en of de voorbeeldgegevens is ingeschakeld. Zie voor meer informatie over logboekregistratie [inschakelen van logboekregistratie voor Machine Learning-webservices](web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Toevoegen van een eindpunt met de klassieke Azure portal
1. Aanmelden bij de [klassieke Azure-portal](http://manage.windowsazure.com), klikt u op **Machine Learning** in de linkerkolom staat. Klik op de werkruimte waarin de webservice waarin u geïnteresseerd bent.
   
    ![Navigeer naar de werkruimte](./media/create-endpoint/figure-1.png)
2. Klik op **webservices**.
   
    ![Navigeer naar de Web-services](./media/create-endpoint/figure-2.png)
3. Klik op de webservice die u geïnteresseerd bent in de lijst van beschikbare eindpunten wilt zien.
   
    ![Navigeer naar het eindpunt](./media/create-endpoint/figure-3.png)
4. Klik onder aan de pagina op **eindpunt toevoegen**. Typ een naam en beschrijving, zorg ervoor dat er zijn geen andere eindpunten met dezelfde naam in deze webservice. Laat het niveau van de throttle met de standaardwaarde, tenzij u speciale vereisten hebt. Zie voor meer informatie over het beperken van [API-eindpunten schalen](scaling-webservice.md).
   
    ![Eindpunt maken](./media/create-endpoint/figure-4.png)

## <a name="next-steps"></a>Volgende stappen
[Een Azure Machine Learning-webservice gebruiken](consume-web-services.md).


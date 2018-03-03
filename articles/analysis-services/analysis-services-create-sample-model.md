---
title: Toevoegen van een model in tabelvorm voorbeeld voor uw Azure Analysis Services-server | Microsoft Docs
description: Informatie over het toevoegen van een voorbeeld-model in Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/28/2018
ms.author: owend
ms.openlocfilehash: df83f5dd86d1edf857378ae69b16a86b57f9a2fe
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="tutorial-add-a-sample-model"></a>Zelfstudie: Een voorbeeld-model toevoegen

In deze zelfstudie maakt toevoegen u een voorbeeld Adventure Works model aan uw server. Het model voorbeeld is een voltooide versie van de verkoop van Adventure Works Internet (1200) gegevens modelleren zelfstudie. Een voorbeeld-model is nuttig voor het model management testen, verbinding met hulpprogramma's en clienttoepassingen en het modelgegevens opvragen.

## <a name="before-you-begin"></a>Voordat u begint

Voor deze zelfstudie hebt u het volgende nodig:

- Een Azure Analysis Services-server
- Administrator-machtigingen voor server

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-sample-model"></a>Een voorbeeld-model maken

1. Op de server **overzicht**, klikt u op **nieuw model**.

    ![Een voorbeeld-model maken](./media/analysis-services-create-sample-model/aas-create-sample-new-model.png)

2. In **nieuw model** > **een gegevensbron kiezen**, Controleer of **voorbeeldgegevens** is geselecteerd en klik vervolgens op **toevoegen**.

    ![Selecteer voorbeeldgegevens](./media/analysis-services-create-sample-model/aas-create-sample-data.png)

3. In **overzicht**, Controleer of de `adventureworks` voorbeeld is gemaakt.

    ![Selecteer voorbeeldgegevens](./media/analysis-services-create-sample-model/aas-create-sample-verify.png)

## <a name="clean-up-resources"></a>Resources opschonen

Het model van uw voorbeeld maakt gebruik van resources in het cachegeheugen. Als u uw voorbeeld-model niet gebruikt voor het testen, moet u het verwijderen van uw server.

> [!NOTE]
> Deze stappen wordt beschreven hoe u een model verwijdert uit een server met behulp van SSMS. U kunt ook een model verwijderen met behulp van de ontwerpfunctie webfunctie-preview.

1. In SSMS > **Objectverkenner**, klikt u op **Connect** > **Analysis Services**.

2. In **verbinding maken met Server**, in de naam van de server vervolgens plakken **verificatie**, kies **Active Directory - Universal met ondersteuning voor MFA**, Voer uw gebruikersnaam en klik vervolgens op **Verbinding**.

    ![Aanmelden](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-signin.png)

3. In **Objectverkenner**, met de rechtermuisknop op de `adventureworks` de voorbeelddatabase en klik vervolgens op **verwijderen**.

    ![Voorbeelddatabase verwijderen](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-delete.png)

## <a name="next-steps"></a>Volgende stappen 

[Verbinding maken in Power BI Desktop](analysis-services-connect-pbi.md)   
[Databaserollen en gebruikers beheren](analysis-services-database-users.md)



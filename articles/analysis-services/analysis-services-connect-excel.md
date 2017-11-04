---
title: Verbinding maken met Azure analyseservices met Excel | Microsoft Docs
description: Informatie over het verbinding maken met een Azure Analysis Services-server met behulp van Excel.
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
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 7597792a525583497ec6a400e668eda2adc9a93e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="connect-with-excel"></a>Verbinden met Excel

Nadat u hebt een server in Azure gemaakt en geïmplementeerd een model in tabelvorm, kunt u verbinding maken en gebruiken van gegevens.


## <a name="connect-in-excel"></a>Verbinding maken in Excel

Verbinding maken met een server in Excel wordt ondersteund door ophalen van gegevens in Excel 2016. Verbinding maken met behulp van de Wizard importeren in Power Pivot wordt niet ondersteund. 

**Verbinding maken in Excel 2016**

1. In Excel 2016, op de **gegevens** lint, klikt u op **externe gegevens ophalen** > **van andere bronnen** > **van Analysis Services**.

2. In de Wizard Gegevensverbinding in **servernaam**, voer de naam van de server waaronder het protocol en de URI. Klik op **aanmeldingsreferenties**, selecteer **gebruik de volgende gebruikersnaam en wachtwoord**, en typ vervolgens de organisatie-gebruikersnaam, bijvoorbeeld nancy@adventureworks.com, en het wachtwoord.

    ![Verbinding maken van Excel-aanmelding](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. In **Database en tabel selecteren**, de database en het model of de perspectief selecteren en klik vervolgens op **voltooien**.
   
    ![Verbinding maken vanaf Selecteer Excel-gegevensmodel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Zie ook
[Clientbibliotheken](analysis-services-data-providers.md)   
[Beheren van uw server](analysis-services-manage.md)     



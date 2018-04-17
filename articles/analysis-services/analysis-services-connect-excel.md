---
title: Verbinding maken met Azure analyseservices met Excel | Microsoft Docs
description: Informatie over het verbinding maken met een Azure Analysis Services-server met behulp van Excel.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 8414597c2c394e0b642ff47cba79c87488f56b24
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="connect-with-excel"></a>Verbinden met Excel

Nadat u hebt een server in Azure gemaakt en geïmplementeerd een model in tabelvorm, kunt u verbinding maken en gebruiken van gegevens.


## <a name="connect-in-excel"></a>Verbinding maken in Excel

Verbinding maken met een server in Excel wordt ondersteund door ophalen van gegevens in Excel 2016. Verbinding maken met behulp van de Wizard importeren in Power Pivot wordt niet ondersteund. 

**Verbinding maken in Excel 2016**

1. In Excel 2016, op de **gegevens** lint, klikt u op **externe gegevens ophalen** > **van andere bronnen** > **van Analysis Services**.

2. In de Wizard Gegevensverbinding in **servernaam**, voer de naam van de server waaronder het protocol en de URI. Klik op **aanmeldingsreferenties**, selecteer **gebruik de volgende gebruikersnaam en wachtwoord**, en typ vervolgens de organisatie-gebruikersnaam, bijvoorbeeld nancy@adventureworks.com, en het wachtwoord.

    > [!NOTE]
    > Als u zich met een Microsoft-Account, Live ID, Yahoo, Gmail, enz aanmeldt., of u zich moet aanmelden met multi-factor authentication, laat u het wachtwoordveld leeg. U wordt gevraagd om een wachtwoord wanneer u op volgende.

    ![Verbinding maken van Excel-aanmelding](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. In **Database en tabel selecteren**, de database en het model of de perspectief selecteren en klik vervolgens op **voltooien**.
   
    ![Verbinding maken vanaf Selecteer Excel-gegevensmodel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Zie ook
[Clientbibliotheken](analysis-services-data-providers.md)   
[Beheren van uw server](analysis-services-manage.md)     



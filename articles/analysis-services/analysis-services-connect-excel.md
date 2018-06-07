---
title: Verbinding maken met Azure analyseservices met Excel | Microsoft Docs
description: Informatie over het verbinding maken met een Azure Analysis Services-server met behulp van Excel.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 6585648cbb461f52a22bc6ea239c0899caa2c2e7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34597260"
---
# <a name="connect-with-excel"></a>Verbinden met Excel

Nadat u hebt gemaakt van een server en een model in tabelvorm geïmplementeerd, kunnen clients verbinding maken en beginnen met het verkennen van gegevens. 

## <a name="before-you-begin"></a>Voordat u begint
Het account dat u zich met aanmeldt moet behoren tot een databaserol model met ten minste de machtiging lezen. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Verbinding maken in Excel

Verbinding maken met een server in Excel wordt ondersteund door ophalen van gegevens in Excel 2016. Verbinding maken met behulp van de Wizard importeren in Power Pivot wordt niet ondersteund. 

**Verbinding maken in Excel 2016**

1. In Excel 2016, op de **gegevens** lint, klikt u op **externe gegevens ophalen** > **van andere bronnen** > **van Analysis Services**.

2. In de Wizard Gegevensverbinding in **servernaam**, voer de naam van de server waaronder het protocol en de URI. Bijvoorbeeld: asazure://westcentralus.asazure.windows.net/advworks. Klik op **aanmeldingsreferenties**, selecteer **gebruik de volgende gebruikersnaam en wachtwoord**, en typ vervolgens de organisatie-gebruikersnaam, bijvoorbeeld nancy@adventureworks.com, en het wachtwoord.

    > [!IMPORTANT]
    > Als u met een Microsoft-Account, Live ID, Yahoo, Gmail, enz aanmelden., of u zich moet aanmelden met multi-factor authentication, laat u het wachtwoordveld leeg. U wordt gevraagd om een wachtwoord wanneer u op volgende. 

    ![Verbinding maken van Excel-aanmelding](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. In **Database en tabel selecteren**, de database en het model of de perspectief selecteren en klik vervolgens op **voltooien**.
   
    ![Verbinding maken vanaf Selecteer Excel-gegevensmodel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Zie ook
[Clientbibliotheken](analysis-services-data-providers.md)   
[Beheren van uw server](analysis-services-manage.md)     



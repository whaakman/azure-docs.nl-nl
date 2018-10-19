---
title: Verbinding maken met Azure analyseservices met Excel | Microsoft Docs
description: Leer hoe u verbinding maken met een Azure Analysis Services-server met behulp van Excel.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 2a9646ad2daed5ebc224d6d0b97f1f42c7da101e
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49425849"
---
# <a name="connect-with-excel"></a>Verbinden met Excel

Nadat u hebt gemaakt van een server en een tabellair model geïmplementeerd, kunnen clients verbinding maken met en gegevens verkennen. 

## <a name="before-you-begin"></a>Voordat u begint
Het account dat u zich aan met moet behoren tot een databaserol model met ten minste de machtiging lezen. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Verbinding maken in Excel

Verbinding maken met een server in Excel wordt ondersteund met behulp van gegevens ophalen in Excel 2016. Verbinding maken met behulp van de Wizard importeren in Power Pivot wordt niet ondersteund. 

**Verbinding maken in Excel 2016**

1. In Excel 2016, op de **gegevens** lint, klikt u op **externe gegevens ophalen** > **van andere bronnen** > **van Analysis Services** .

2. In de Wizard Gegevensverbinding in **servernaam**, voer de naam van de server met inbegrip van protocol en de URI. Bijvoorbeeld, asazure://westcentralus.asazure.windows.net/advworks. Klik op **aanmeldingsreferenties**, selecteer **gebruik de volgende gebruikersnaam en wachtwoord**, en typ vervolgens de naam van de organisatie-gebruiker, bijvoorbeeld nancy@adventureworks.com, en het wachtwoord.

    > [!IMPORTANT]
    > Als u zich aanmelden met een Microsoft-Account, Live ID, Yahoo, Gmail, enz., of u moet zich aanmelden met multi-factor authentication, laat u het wachtwoordveld leeg. U wordt gevraagd om een wachtwoord wanneer u op volgende. 

    ![Verbinding maken vanuit Excel-aanmelding](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. In **Database en tabel selecteren**, selecteert u de database en model of perspectief, en klik vervolgens op **voltooien**.
   
    ![Verbinding maken via de optie Excel-gegevensmodel](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Zie ook
[Clientbibliotheken](analysis-services-data-providers.md)   
[Uw server beheren](analysis-services-manage.md)     



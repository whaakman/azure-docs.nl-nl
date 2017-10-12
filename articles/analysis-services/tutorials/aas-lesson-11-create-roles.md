---
title: 'Azure Analysis Services-zelfstudie - Les 11: Rollen maken | Microsoft Docs'
description: In deze les wordt beschreven hoe u rollen maakt in de zelfstudie over Azure Analysis Services.
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
ms.openlocfilehash: 0dfcb9d9fc8cd32f95c5097ec653864364b27bcc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="lesson-11-create-roles"></a>Les 11: Rollen maken

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In deze les gaat u rollen maken. Rollen bieden beveiliging van modeldatabase-objecten en gegevens door de toegang te beperken tot alleen gebruikers waaraan de rol is toegewezen. Elke rol is gekoppeld aan één machtiging: None, Read, Read and Process, Process of Administrator. Rollen kunnen tijdens de ontwerpfase van het model worden gedefinieerd met behulp van Role Manager. Nadat een model is geïmplementeerd, kunt u rollen beheren met behulp van SQL Server Management Studio (SSMS). Zie [Rollen](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular) voor meer informatie.
  
> [!NOTE]  
> Het maken van rollen is geen vereiste om deze zelfstudie te voltooien. De standaardinstelling is dat het account waarmee u bent aangemeld, beheerdersbevoegdheden heeft voor het model. Als andere gebruikers in uw organisatie echter de mogelijkheid moeten hebben om het model te raadplegen met behulp van een rapportageclient, moet u ten minste één rol maken met de machtiging Read en die rol toewijzen aan de gebruikers.  
  
U gaat drie rollen maken:  
  
-   **Sales Manager**: deze rol kan worden toegewezen aan gebruikers in uw organisatie die u leesmachtiging (Read) wilt geven voor alle modelobjecten en -gegevens.  
  
-   **Sales Analyst US**: deze rol kan worden toegewezen aan gebruikers in uw organisatie die gegevens mogen raadplegen over verkopen in de Verenigde Staten. Voor deze rol gaat u een DAX-formule gebruiken om een *rijfilter* te definiëren, waardoor leden van deze rol alleen gegevens voor de Verenigde Staten kunnen inzien.  
  
-   **Administrator**: deze rol kan worden toegewezen aan gebruikers die u de machtiging Administrator wilt geven, waarmee ze onbeperkte toegang en machtigingen krijgen voor het uitvoeren van beheertaken voor de modeldatabase.  
  
Omdat gebruikers- en groepsaccounts van Windows uniek zijn binnen uw organisatie, kunt u accounts van uw organisatie toevoegen aan leden. Voor deze zelfstudie kunt u de leden echter ook leeg laten. U gaat het effect van elke rol later testen, in Les 12: Analyseren in Excel.  
  
Geschatte tijd voor het voltooien van deze les: **15 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 10: Partities maken](../tutorials/aas-lesson-10-create-partitions.md).  
  
## <a name="create-roles"></a>Rollen maken  
  
#### <a name="to-create-a-sales-manager-user-role"></a>Een gebruikersrol Sales Manager maken:  
  
1.  Klik in Tabular Model Explorer met de rechtermuisknop op **Roles** > **Roles**.  
  
2.  Klik in Role Manager op **New**.  
  
3.  Klik op de nieuwe rol en wijzig de naam van de rol vervolgens in de kolom **Name** in **Sales Manager**.  
  
4.  Klik op de vervolgkeuzelijst in de kolom **Permissions** en selecteer vervolgens de machtiging **Read**. 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  Optioneel: Klik op het tabblad **Members** en klik vervolgens op **Add**. Voer in het dialoogvenster **Select Users or Groups** de Windows-gebruikers of -groepen van uw organisatie in die u wilt toewijzen aan de rol.  
  
#### <a name="to-create-a-sales-analyst-us-user-role"></a>Een gebruikersrol Sales Analyst US maken:  
  
1.  Klik in Role Manager op **New**.    
  
2.  Wijzig de naam van de rol in **Sales Analyst US**.  
  
3.  Geef deze rol de machtiging **Read**.  
  
4.  Klik op het tabblad Row Filters en typ vervolgens alleen voor de kolom **DimGeography** de volgende formule in de kolom DAX Filter:  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    Een Row Filter-formule moet worden omgezet in een Boole-waarde (TRUE/FALSE). Met deze formule geeft u aan dat alleen rijen met de landcode 'VS' zichtbaar moeten zijn voor de gebruiker.  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  Optioneel: Klik op het tabblad **Members** en klik vervolgens op **Add**. Voer in het dialoogvenster **Select Users or Groups** de Windows-gebruikers of -groepen van uw organisatie in die u wilt toewijzen aan de rol.  
  
#### <a name="to-create-an-administrator-user-role"></a>Een gebruikersrol Administrator maken:  
  
1.  Klik op **Nieuw**.  
  
2.  Wijzig de naam van de rol in **Administrator**.  
  
3.  Geef deze rol de machtiging **Administrator**.  
  
4.  Optioneel: Klik op het tabblad **Members** en klik vervolgens op **Add**. Voer in het dialoogvenster **Select Users or Groups** de Windows-gebruikers of -groepen van uw organisatie in die u wilt toewijzen aan de rol. 
  
  
## <a name="whats-next"></a>Volgende stappen
[Les 12: Analyseren in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).

  
  

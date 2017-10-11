---
title: Azure analyseservices beheren | Microsoft Docs
description: Informatie over het beheren van een Analysis Services-server in Azure.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 79491d0b-b00d-4e02-9ca7-adc99bc02fdb
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: b897e81351ebee11c292e67ac76ba8202a6f0108
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="manage-analysis-services"></a>Analyseservices beheren
Als u een Analysis Services-server hebt gemaakt in Azure, worden sommige administratie en beheer taken moet u meteen of later opnieuw uitvoeren op de weg. Voer bijvoorbeeld verwerking naar de gegevens vernieuwen, bepalen wie toegang de modellen op uw server of controleren van de status van uw server. Bepaalde beheertaken kunnen alleen worden uitgevoerd in Azure-portal anderen in SQL Server Management Studio (SSMS), en sommige taken kunnen worden uitgevoerd op een.

## <a name="azure-portal"></a>Azure Portal
[Azure-portal](http://portal.azure.com/) is waar u kunt maken en verwijderen van servers, serverbronnen bewaken, grootte wijzigen en beheren wie toegang heeft tot uw servers.  Als u problemen ondervindt, kunt u ook een verzoek om ondersteuning te verzenden.

![Servernaam bepalen in Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Verbinding maken met uw server in Azure is net als de verbinding te maken met een exemplaar van de server in uw eigen organisatie. Van SSMS, kunt u veel van dezelfde taken zoals gegevens over het installatieproces uitvoeren of maken van een verwerkingsscript, rollen beheren en PowerShell gebruiken.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>Download en installeer SSMS
Als u de nieuwste functies en de meest vloeiende ervaring bij het verbinden met uw Azure Analysis Services-server, moet u de nieuwste versie van SSMS. 

[SQL Server Management Studio downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="to-connect-with-ssms"></a>Verbinding maken met SSMS
 Wanneer u SSMS, voordat u verbinding maakt met de server het eerst gebruikt, zorg er dan voor dat uw gebruikersnaam is opgenomen in de Analysis Services-beheerdersgroep. Zie voor meer informatie, [serverbeheerders](#server-administrators) verderop in dit artikel.

1. Voordat u verbinding maakt, moet u de naam van de server ophalen. In **Azure Portal** > server > **Overview** > **Servernaam**,kopieer de servernaam.
   
    ![Servernaam bepalen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. In SSMS > **Objectverkenner**, klikt u op **Connect** > **Analysis Services**.
3. In de **verbinding maken met Server** in het dialoogvenster Plakken in de naam van de server vervolgens in **verificatie**, kies een van de volgende verificatietypen:
   
    **Windows-verificatie** om uw Windows-referenties voor domein\gebruikersnaam en het wachtwoord te gebruiken.

    **Active Directory-wachtwoordverificatie** om een organisatie-account te gebruiken. Bijvoorbeeld, wanneer verbinding maakt vanaf een niet-domein lid zijn van computer.

    **Verificatie van Active Directory-universele** gebruiken [niet-interactieve of multi-factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md). 
   
    ![Verbinding maken in SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Serverbeheerders en databasegebruikers
Er zijn twee typen van gebruikers, beheerders en databasegebruikers in Azure Analysis Services. Beide typen gebruikers, moeten in uw Azure Active Directory en moeten worden opgegeven met de organisatie-e-mailadres of de UPN. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Het oplossen van problemen met de verbinding
Wanneer u verbinding maakt met behulp van SSMS, als u op problemen stuit, moet u wellicht de aanmeldings-cache wissen. Niets in cache wordt opgeslagen op schijf. Schakel de cache, sluiten en opnieuw starten van het proces connect. 

## <a name="next-steps"></a>Volgende stappen
Als u dit nog niet hebt al een model in tabelvorm geïmplementeerd naar de nieuwe server, is een goed moment. Zie [Implementeren naar Azure Analysis Services](analysis-services-deploy.md) voor meer informatie.

Als u een model hebt geïmplementeerd met de server, bent u klaar om te verbinden met met behulp van een client of de browser. Zie voor meer informatie, [gegevens ophalen uit Azure Analysis Services-server](analysis-services-connect.md).


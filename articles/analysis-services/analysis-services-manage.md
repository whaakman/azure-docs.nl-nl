---
title: Azure analyseservices beheren | Microsoft Docs
description: Informatie over het beheren van een Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 0bae06d46c2c96ba9dd058e9c2d380379523811c
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52993925"
---
# <a name="manage-analysis-services"></a>Analyseservices beheren
Wanneer u een Analysis Services-server in Azure hebt gemaakt, worden sommige administratie en beheer taken die u wilt uitvoeren van direct of enige tijd in de weg. Bijvoorbeeld uitvoeren om de gegevens vernieuwen, bepalen wie toegang tot de modellen op uw server, of bewaak de status van uw server. Bepaalde beheertaken kunnen alleen worden uitgevoerd in Azure portal voor anderen in SQL Server Management Studio (SSMS), en sommige taken kunnen worden uitgevoerd in een.

## <a name="azure-portal"></a>Azure Portal
[Azure-portal](https://portal.azure.com/) is waar u kunt maken en verwijderen van servers, bewaken van serverbronnen grootte wijzigen en beheren wie toegang heeft tot uw servers.  Als u problemen ondervindt, kunt u ook een ondersteuningsaanvraag indienen.

![Servernaam bepalen in Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Verbinding maken met uw server in Azure is net als bij verbinding maken met een server-exemplaar in uw eigen organisatie. Van SSMS, kunt u veel van dezelfde taken, zoals gegevens verwerken uitvoeren of maken van een verwerkingsscript, rollen beheren en PowerShell gebruiken.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>Download en installeer SSMS
Als u de nieuwste functies, samen met de meest vloeiende ervaring bij het verbinden met uw Azure Analysis Services-server, moet u de meest recente versie van SSMS. 

[SQL Server Management Studio downloaden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="to-connect-with-ssms"></a>Verbinding maken met SSMS
 Wanneer met behulp van SSMS, voordat u verbinding maakt met uw server de eerste keer, zorg er dan voor dat uw gebruikersnaam is opgenomen in de Analysis Services-Admins-groep. Zie voor meer informatie, [Server-beheerders en databasegebruikers](#server-administrators-and-database-users) verderop in dit artikel.

1. Voordat u verbinding maakt, moet u de naam van de server. In **Azure Portal** > server > **Overview** > **Servernaam**,kopieer de servernaam.
   
    ![Servernaam bepalen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. Klik in SSMS > **Objectverkenner** op **Verbinding maken** > **Analysis Services**.
3. In de **verbinding maken met Server** in het dialoogvenster Plakken in de naam van de server vervolgens in **verificatie**, kies een van de volgende verificatietypen:   
    > [!NOTE]
    > Verificatietype, **Active Directory - Universal met ondersteuning voor MFA**, wordt aanbevolen.

    > [!NOTE]
    > Als u zich aanmeldt met een Microsoft-Account, Live ID, Yahoo, Gmail, enz., laat u het wachtwoordveld leeg. U wordt gevraagd om een wachtwoord na op verbinden te klikken.

    **Windows-verificatie** om uw Windows-referenties voor domein\gebruikersnaam en het wachtwoord te gebruiken.

    **Active Directory-wachtwoordverificatie** gebruik van een organisatie-account. Bijvoorbeeld, wanneer verbinding maakt vanaf een niet-domein toegevoegde computer.

    **Active Directory - Universal met ondersteuning voor MFA** gebruiken [niet-interactieve of multi-factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md). 
   
    ![Verbinding maken in SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Server-beheerders en databasegebruikers
Er zijn twee typen van gebruikers, beheerders en databasegebruikers in Azure Analysis Services. Beide typen gebruikers moeten zich in uw Azure Active Directory en moeten worden opgegeven met organisatie-e-mailadres of UPN. Raadpleeg voor meer informatie [Verificatie en gebruikersmachtigingen](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Het oplossen van verbindingsproblemen
Wanneer u verbinding maakt met behulp van SSMS, als u problemen ondervindt, moet u mogelijk de aanmeldings-cache wissen. Niets in cache is opgeslagen op schijf. Als u wilt wissen in de cache, sluiten en de verbinding maken met het proces opnieuw starten. 

## <a name="next-steps"></a>Volgende stappen
Als u hebt al een tabellair model geïmplementeerd naar uw nieuwe server, is nu een goed moment. Zie [Implementeren naar Azure Analysis Services](analysis-services-deploy.md) voor meer informatie.

Als u een model hebt geïmplementeerd op uw server, bent u klaar om te verbinden met behulp van een client of de browser. Zie voor meer informatie, [gegevens ophalen uit Azure Analysis Services-server](analysis-services-connect.md).


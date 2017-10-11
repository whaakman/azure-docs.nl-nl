---
title: Verbinding maken met Azure analyseservices | Microsoft Docs
description: Informatie over het verbinding maken met en gegevens ophalen uit een Analysis Services-server in Azure.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: b37f70a0-9166-4173-932d-935d769539d1
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: deb3ef28d20decef01826450bd6091f87dd069de
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="connect-to-an-azure-analysis-services-server"></a>Verbinding maken met een Azure Analysis Services-server

Dit artikel wordt beschreven verbinding maken met een server met behulp van gegevens modelleren en toepassingen zoals SQL Server Management Studio (SSMS) of SQL Server Data Tools (SSDT). Of met client reporting toepassingen zoals Microsoft Excel, Power BI Desktop of aangepaste toepassingen. HTTPS gebruikt voor verbindingen met Azure Analysis Services.

## <a name="client-libraries"></a>Clientbibliotheken
[De meest recente clientbibliotheken ophalen](analysis-services-data-providers.md)

Alle verbindingen met een server, ongeacht het type, bijgewerkte AMO ADOMD.NET en OLEDB-clientbibliotheken verbinding maken met en contact kunnen maken met een Analysis Services-server is vereist. Voor SSMS, SSDT, Excel 2016 en Power BI, worden de meest recente clientbibliotheken geïnstalleerd of bijgewerkt met maandelijkse releases. In sommige gevallen is het echter mogelijk dat een toepassing heeft mogelijk niet de nieuwste versie. Bijvoorbeeld, zijn als beleid vertraging worden bijgewerkt of Office 365-updates op het kanaal uitgesteld.

## <a name="server-name"></a>Servernaam

Wanneer u een Analysis Services-server in Azure maakt, geeft u een unieke naam en de regio waar de server wordt gemaakt. Wanneer u de servernaam opgeeft in een verbinding, is de schematische naam server:

```
<protocol>://<region>/<servername>
```
 Protocol is waar tekenreeks **asazure**, regio is de Uri waar de server is gemaakt (bijvoorbeeld westus.asazure.windows.net) en servername is de naam van uw unieke server binnen de regio.

### <a name="get-the-server-name"></a>Naam van de server ophalen
In **Azure-portal** > server > **overzicht** > **servernaam**, de naam van de hele server kopiëren. Als andere gebruikers in uw organisatie verbinding met deze server te maakt, kunt u de naam van deze server met hen kunt delen. Wanneer u een servernaam opgeeft, moet het volledige pad worden gebruikt.

![Servernaam bepalen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a>Verbindingsreeks

Bij het verbinden met Azure Analysis Services met behulp van het Model in tabelvorm Object, gebruikt u de volgende indelingen voor verbinding tekenreeks:

###### <a name="integrated-azure-active-directory-authentication"></a>Geïntegreerde Azure Active Directory-verificatie
Geïntegreerde verificatie neemt over de Azure Active Directory-referentiecache indien beschikbaar. Als dat niet het geval is, moet u het venster Azure-aanmelding wordt weergegeven.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory-verificatie met gebruikersnaam en wachtwoord

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Windows-verificatie (geïntegreerde beveiliging)
Gebruik het Windows-account met het huidige proces.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```



## <a name="connect-using-an-odc-file"></a>Verbinding maken met behulp van een ODC-bestand
Gebruikers kunnen met oudere versies van Excel verbinding met een Azure Analysis Services-server met behulp van een bestand Office Data Connection (.odc). Zie voor meer informatie, [maakt u een bestand Office Data Connection (.odc)](analysis-services-odc.md).


## <a name="next-steps"></a>Volgende stappen
[Verbinding maken met Excel](analysis-services-connect-excel.md)    
[Verbinding maken met Power BI](analysis-services-connect-pbi.md)   
[Beheren van uw server](analysis-services-manage.md)   


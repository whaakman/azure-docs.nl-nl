---
title: Verbinding maken met Azure Analysis Services-servers | Microsoft Docs
description: Leer hoe u verbinding maken met en gegevens ophalen uit een Analysis Services-server in Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: e2c60d48595f2c3687bd178e0f4eed010f14b568
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54191527"
---
# <a name="connecting-to-servers"></a>Verbinding maken met servers

Dit artikel wordt beschreven verbinding maken met een server met behulp van gegevensmodellering en beheertoepassingen, zoals SQL Server Management Studio (SSMS) of SQL Server Data Tools (SSDT). Of met de client melden van toepassingen zoals Microsoft Excel, Power BI Desktop of aangepaste toepassingen. Verbindingen met Azure Analysis Services gebruik van HTTPS.

## <a name="client-libraries"></a>Clientbibliotheken

[De meest recente clientbibliotheken ophalen](analysis-services-data-providers.md)

Alle verbindingen met een server, ongeacht het type, moeten bijgewerkte AMO ADOMD.NET en OLEDB-clientbibliotheken verbinding maken met en een interface met een Analysis Services-server. Voor SSMS, SSDT, Excel 2016 en later en Power BI, worden de meest recente clientbibliotheken geïnstalleerd of bijgewerkt met de maandelijkse versies. In sommige gevallen is het echter mogelijk dat een toepassing bevat mogelijk niet de nieuwste versie. Bijvoorbeeld, zijn als beleid vertraging worden bijgewerkt of Office 365-updates op het uitgestelde kanaal.

## <a name="server-name"></a>Servernaam

Wanneer u een Analysis Services-server in Azure maakt, geeft u een unieke naam en de regio waar de server zich moet worden gemaakt. Wanneer u de servernaam opgeeft in een verbinding, is het naamgevingsschema server:

```
<protocol>://<region>/<servername>
```
 Waar is protocol tekenreeks **asazure**, regio is de Uri waar de server is gemaakt (bijvoorbeeld westus.asazure.windows.net) en servername is de naam van uw unieke-server in de regio.

### <a name="get-the-server-name"></a>De naam van de server

In **Azure-portal** > server > **overzicht** > **servernaam**, Kopieer de volledige servernaam. Als andere gebruikers in uw organisatie verbinding met deze server te, kunt u de naam van deze server met hen kunt delen. Wanneer u een servernaam opgeeft, kan het volledige pad moet worden gebruikt.

![Servernaam bepalen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connection-string"></a>Verbindingsreeks

Bij het verbinden met Azure Analysis Services met behulp van het objectmodel in tabelvorm, gebruikt u de volgende indelingen van de verbinding-tekenreeks:

###### <a name="integrated-azure-active-directory-authentication"></a>Geïntegreerde Azure Active Directory-verificatie

Geïntegreerde verificatie neemt de referentie-cache van Azure Active Directory, indien beschikbaar. Als dat niet het geval is, moet u de Azure-aanmelding-venster wordt weergegeven.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```


###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory-verificatie met gebruikersnaam en wachtwoord

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

###### <a name="windows-authentication-integrated-security"></a>Windows-verificatie (geïntegreerde beveiliging)

Gebruik het Windows-account waarop het huidige proces wordt uitgevoerd.

```
"Provider=MSOLAP;Data Source=<Azure AS instance name>; Integrated Security=SSPI;Persist Security Info=True;"
```

## <a name="connect-using-an-odc-file"></a>Verbinding maken met behulp van een ODC-bestand

Met oudere versies van Excel kunnen gebruikers verbinding kunnen maken met een Azure Analysis Services-server met behulp van een bestand Office Data Connection (.odc). Zie voor meer informatie, [maakt u een bestand Office Data Connection (.odc)](analysis-services-odc.md).


## <a name="next-steps"></a>Volgende stappen

[Verbinding maken met Excel](analysis-services-connect-excel.md)    
[Verbinding maken met Power BI](analysis-services-connect-pbi.md)   
[Uw server beheren](analysis-services-manage.md)   


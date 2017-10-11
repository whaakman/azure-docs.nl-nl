---
title: Azure Data Catalog release-opmerkingen | Microsoft Docs
description: Releaseopmerkingen voor Azure Data Catalog.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d3db9bee0558cac5fb4f5b8fb4ab67a35ce0f141
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-release-notes"></a>Azure Data Catalog release-opmerkingen
## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Informatie voor 20 November 2015-release van Azure Data Catalog
### <a name="opening-data-sources-in-power-bi-desktop"></a>De gegevensbronnen openen in Power BI Desktop
Wanneer u de optie 'Openen in Power BI Desktop' uit de **Azure Data Catalog** portal gebruikers kunnen ondervinden een van twee problemen in de Power BI Desktop-toepassing:

* Een dialoogvenster met de titel 'Kan niet aan Open Document' wordt weergegeven
* De Power BI Desktop-toepassing wordt geopend, maar het bestand is niet leeg zijn

Voor elke situatie kan het probleem worden opgelost door downloaden en installeren van de nieuwste versie van Power BI Desktop van [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Informatie voor 13 November 2015-release van Azure Data Catalog
### <a name="registering-and-connecting-to-teradata"></a>Registreren en te verbinden met de Teradata
Bij het verbinden met de Teradata-gegevensbronnen gebruikers het juiste Teradata ODBC-stuurprogramma moeten hebben geïnstalleerd die overeenkomen met de bitness van de software die wordt gebruikt (32-bits of 64-bits).

Vanaf deze datum ADC release is de meest recente [Teradata ODBC-stuurprogramma voor windows (versie 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) is compatibel met Office 2013, maar niet met Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Informatie voor 13 juli 2015-release van Azure Data Catalog
### <a name="registering-and-connecting-to-oracle-database"></a>Registreren en te verbinden met de Oracle-Database
Bij het verbinden met de Oracle-Database-gegevensbronnen gebruikers moeten de juiste Oracle-stuurprogramma's die overeenkomen met de bitness van de software die wordt gebruikt (32-bits of 64-bits) hebt geïnstalleerd.

* De 32-bits Oracle-stuurprogramma's wordt gebruikt bij het registreren van Oracle-gegevensbronnen op een computer met Windows 32-bits
* De 64-bits Oracle-stuurprogramma's wordt gebruikt bij het registreren van Oracle-gegevensbronnen op een computer met 64-bits Windows
* Bij het verbinden met de Oracle-gegevensbronnen met Excel op een computer met de 32-bits versie van Microsoft Office, wordt inclusief op 64-bits Windows de 32-bits Oracle-stuurprogramma's gebruikt
* Wanneer u verbinding maakt met de Oracle-gegevensbronnen met Excel op een computer met de 64-bits versie van Microsoft Office, worden de 64-bits Oracle-stuurprogramma's gebruikt

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registreren en te verbinden met SQL Server Reporting Services
Ondersteuning voor gegevensbronnen van SQL Server Reporting Services (SSRS) is momenteel beperkt tot alleen servers Native-modus. Ondersteuning voor SQL Server Reporting Services in de modus SharePoint wordt uitgevoerd wordt in een latere versie toegevoegd.

### <a name="opening-data-assets-in-excel"></a>Gegevensassets openen in Excel
Bij het openen van gegevensassets in Microsoft Excel uit de **Azure Data Catalog** portal gebruikers mogelijk gevraagd met een **beveiligingsbericht Microsoft Excel** in het dialoogvenster. Dit is standaard, verwacht gedrag en gebruikers kunnen selecteren **inschakelen** om door te gaan.

Zie voor meer informatie [in- of uitschakelen van beveiligingswaarschuwingen over koppelingen en bestanden van verdachte websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy- en beleid voor configuratie en gegevens registratie van gegevensbron
Gebruikers kunnen ondervinden een situatie waarin ze zich aanmelden kunnen bij de Azure Data Catalog-portal, maar als ze proberen aan te melden op het hulpprogramma voor registratie dat een foutbericht weergegeven dat voorkomt ze dat zich aanmelden optreden.

Er zijn twee mogelijke oorzaken voor dit probleem gedrag:

**1 oorzaak: Active Directory Federation Services configuration** formulierverificatie valideren gebruikersaanmeldingen op basis van Active Directory maakt gebruik van het registratiehulpprogramma van gegevensbronnen. Voor geslaagde aanmelding moet formulierverificatie zijn ingeschakeld in het globale verificatiebeleid door een Active Directory-beheerder.

In sommige gevallen is kan deze fout gebeuren wanneer de gebruiker op het bedrijfsnetwerk, of alleen wanneer de gebruiker verbinding maakt vanaf buiten het bedrijfsnetwerk. Het globale verificatiebeleid kunt verificatiemethoden afzonderlijk worden ingeschakeld voor het intranet en extranet-verbindingen. Aanmeldingsfouten kunnen optreden als formulierverificatie is niet ingeschakeld voor het netwerk van waaruit de gebruiker verbinding maakt.

Zie voor meer informatie [Authenticatiebeleid configureren](https://technet.microsoft.com/library/dn486781.aspx).

**Oorzaak 2: Proxy netwerkconfiguratie** als het bedrijfsnetwerk een proxyserver gebruikt, het registratiehulpprogramma wellicht geen verbinding maken met Azure Active Directory via de proxy. Gebruikers kunnen ervoor zorgen dat het registratiehulpprogramma door het hulpprogramma configuratiebestand, bewerken in deze sectie toe te voegen aan het bestand:

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Zoek het bestand RegistrationTool.exe.config, start het registratiehulpprogramma en open vervolgens het hulpprogramma Windows Taakbeheer. Op het tabblad Details in Taakbeheer met de rechtermuisknop op RegistrationTool.exe en kies bestandslocatie openen in het pop-upmenu.

---
title: Releaseopmerkingen voor Azure Data Catalog
description: Opmerkingen bij de release voor Azure Data Catalog.
services: data-catalog
author: steelanddata
ms.author: maroche
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 539ef4f591a1ef0c6ab344e1d93a750133d9e089
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053579"
---
# <a name="azure-data-catalog-release-notes"></a>Releaseopmerkingen voor Azure Data Catalog
## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Informatie voor 20 November 2015-release van Azure Data Catalog
### <a name="opening-data-sources-in-power-bi-desktop"></a>De gegevensbronnen openen in Power BI Desktop
Wanneer u de optie 'Open in Power BI Desktop' van de **Azure Data Catalog** portal, kunt u de volgende een van twee problemen in de Power BI Desktop-toepassing:

* Een dialoogvenster met de titel 'Kan niet naar Open Document' wordt weergegeven
* De Power BI Desktop-toepassing wordt geopend, maar het bestand is niet leeg zijn

Voor elke situatie, het probleem kan worden omgezet door te downloaden en installeren van de meest recente versie van Power BI Desktop uit [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Informatie voor 13 November 2015-release van Azure Data Catalog
### <a name="registering-and-connecting-to-teradata"></a>Registreren en verbinding maken met Teradata
Wanneer u verbinding maakt met Teradata gegevensbronnen gebruikers moeten de juiste Teradata ODBC-stuurprogramma hebt geïnstalleerd die overeenkomen met de bitness (32-bits of 64-bits) van de software die wordt gebruikt.

Vanaf deze releasedatum ADC, dat u de meest recente [Teradata ODBC-stuurprogramma voor windows (versie 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) is compatibel met Office 2013, maar niet met Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Informatie voor 13 juli 2015-release van Azure Data Catalog
### <a name="registering-and-connecting-to-oracle-database"></a>Registreren en verbinding maken met Oracle-Database
Wanneer u verbinding maakt met Oracle-Database-gegevensbronnen die gebruikers moeten de juiste Oracle-stuurprogramma's die overeenkomen met de bitness (32-bits of 64-bits) van de gebruikte software hebben geïnstalleerd.

* Bij het registreren van Oracle-gegevensbronnen op een 32-bits Windows-computers, worden de 32-bits Oracle-stuurprogramma's gebruikt
* Bij het registreren van Oracle-gegevensbronnen op een 64-bits Windows-computers, worden de 64-bits Oracle-stuurprogramma's gebruikt
* Wanneer u verbinding maakt met Oracle-gegevensbronnen met behulp van Excel op een computer met de 32-bits versie van Microsoft Office, worden met inbegrip van 64-bits Windows, de 32-bits Oracle-stuurprogramma's gebruikt
* Wanneer u verbinding maakt met Oracle-gegevensbronnen met behulp van Excel op een computer met de 64-bits versie van Microsoft Office, worden de 64-bits Oracle-stuurprogramma's gebruikt

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registreren en verbinding maken met SQL Server Reporting Services
Ondersteuning voor gegevensbronnen van SQL Server Reporting Services (SSRS) is momenteel beperkt tot alleen de servers voor de Native-modus. Ondersteuning voor SQL Server Reporting Services in SharePoint-modus wordt toegevoegd in een latere versie.

### <a name="opening-data-assets-in-excel"></a>Gegevensassets openen in Excel
Bij het openen van gegevensassets in Microsoft Excel uit de **Azure Data Catalog** portal gebruikers mogelijk gevraagd met een **beveiligingsbericht Microsoft Excel** in het dialoogvenster. Dit is standaard, normaal, en gebruikers kunnen selecteren **inschakelen** om door te gaan.

Zie voor meer informatie, [in- of uitschakelen van beveiligingswaarschuwingen over koppelingen en bestanden van verdachte websites](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy- en beleid voor configuratie en gegevens registratie van gegevensbron
Gebruikers kunnen ondervinden wanneer ze zich aanmelden kunnen op de Azure Data Catalog-portal, maar als ze proberen aan te melden op het registratiehulpprogramma voor gegevensbronnen een foutbericht dat wordt voorkomen ze dat zich aanmelden optreden op de situatie.

Er zijn twee mogelijke oorzaken voor het gedrag van dit probleem:

**1 oorzaak: Active Directory Federation Services-configuratie** formulierverificatie valideren aanmeldingen van gebruikers op basis van Active Directory maakt gebruik van het hulpprogramma voor registratie. Voor geslaagde aanmelding, moet u formulierverificatie ingeschakeld in het globale verificatiebeleid door een Active Directory-beheerder.

In sommige gevallen kan deze fout gebeuren wanneer de gebruiker op het bedrijfsnetwerk, of alleen wanneer de gebruiker verbinding maakt vanaf buiten het bedrijfsnetwerk bevindt. Het globale verificatiebeleid kunt verificatiemethoden afzonderlijk worden ingeschakeld voor intranet en extranet-verbindingen. Aanmeldingsfouten kunnen optreden als formulierverificatie niet is ingeschakeld voor het netwerk van waaruit de gebruiker verbinding maakt.

Zie voor meer informatie, [verificatiebeleid configureren](https://technet.microsoft.com/library/dn486781.aspx).

**Oorzaak 2: Proxy netwerkconfiguratie** als het bedrijfsnetwerk een proxyserver gebruikt, het hulpprogramma voor registratie wellicht geen verbinding maken met Azure Active Directory via de proxy. Gebruikers kunnen ervoor zorgen dat het hulpprogramma voor registratie door het configuratiebestand van het hulpprogramma, te bewerken in deze sectie toe te voegen aan het bestand:

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Ga naar het bestand RegistrationTool.exe.config, start het hulpprogramma voor registratie en open vervolgens het hulpprogramma Windows Taakbeheer. Met de rechtermuisknop op RegistrationTool.exe en kies bestandslocatie openen in het pop-upmenu op het tabblad Details in Taakbeheer.

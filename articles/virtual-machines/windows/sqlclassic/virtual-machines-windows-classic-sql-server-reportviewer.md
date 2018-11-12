---
title: ReportViewer gebruiken op een website | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u een Microsoft Azure-website met de Visual Studio ReportViewer-besturingselement dat wordt opgeslagen op een Microsoft Azure-Machine maken.
services: virtual-machines-windows
documentationcenter: na
author: markingmyname
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 78b76318-d9bf-48ef-9d9e-d1b7d8cf3042
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: maghan
ms.openlocfilehash: 514e85fc61240834d8db152ece65a4f9cce9023e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250404"
---
# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>ReportViewer gebruiken op een in Azure gehoste website
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

U kunt een Microsoft Azure-website met de Visual Studio ReportViewer-besturingselement dat wordt opgeslagen op een Microsoft Azure-Machine maken. Het ReportViewer-besturingselement is in een webtoepassing die u bouwt met behulp van de sjabloon voor ASP.NET-Web-toepassingen.

> [!IMPORTANT]
> De ASP.NET MVC-webtoepassing-sjablonen bieden geen ondersteuning voor het ReportViewer-besturingselement.

Als u wilt opnemen ReportViewer in uw Microsoft Azure-website, moet u de volgende taken uit te voeren.

* **Voeg** assembly's aan het implementatiepakket
* **Configureer** verificatie en autorisatie
* **Publiceren** de ASP.NET-Web-App naar Azure

## <a name="prerequisites"></a>Vereisten
Raadpleeg de sectie 'algemene aanbevelingen en best practices' in [SQL Server Business Intelligence in Azure Virtual Machines](../classic/ps-sql-bi.md).

> [!NOTE]
> Besturingselementen van rapportviewer worden geleverd met Visual Studio, Standard Edition of hoger. Als u de Web Developer Express Edition gebruikt, moet u de [MICROSOFT REPORT VIEWER 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) de ReportViewer runtime-functies te gebruiken.
> 
> ReportViewer geconfigureerd in de modus voor lokale verwerking wordt niet ondersteund in Microsoft Azure.

## <a name="adding-assemblies-to-the-deployment-package"></a>Assembly's toe te voegen aan het implementatiepakket
Wanneer u uw ASP.NET-toepassing on-premises host, het ReportViewer-assembly's worden meestal geïnstalleerd rechtstreeks in de global assembly-cache (GAC) van de IIS-server tijdens de installatie van Visual Studio en rechtstreeks door de toepassing kunnen worden geopend. Echter, wanneer u uw ASP.NET-toepassing in de cloud hosten, Microsoft Azure is niet toegestaan niets om te worden geïnstalleerd in de GAC, dus moet u ervoor zorgen dat het ReportViewer-assembly's zijn beschikbaar voor uw toepassing lokaal. U kunt dit doen door toe te voegen verwijzingen naar deze in uw project en configureer ze zodanig worden gekopieerd.

Maakt gebruik de volgende assembly's in de modus externe verwerking van het ReportViewer-besturingselement:

* **Microsoft.ReportViewer.WebForms.dll**: bevat het ReportViewer-code, die u wilt ReportViewer gebruiken op de pagina. Een verwijzing voor deze assembly wordt toegevoegd aan uw project wanneer u een ReportViewer-besturingselement naar een ASP.NET-pagina in uw project verwijderen.
* **Microsoft.ReportViewer.Common.dll**: bevat klassen die door het ReportViewer-besturingselement wordt gebruikt tijdens de uitvoering. Het is niet automatisch toegevoegd aan uw project.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Een verwijzing naar Microsoft.ReportViewer.Common toevoegen
* Met de rechtermuisknop op het project **verwijzingen** knooppunt en selecteert u **verwijzing toevoegen**, selecteert u de assembly in het .NET-tabblad en klik op **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>De assembly's lokaal toegankelijk maken door uw ASP.NET-toepassing
1. In de **verwijzingen** map, klikt u op de assembly Microsoft.ReportViewer.Common zodat de eigenschappen worden weergegeven in het deelvenster met eigenschappen.
2. Stel in het deelvenster met eigenschappen **lokale kopie** op ' True '.
3. Herhaal stappen 1 en 2 voor Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Om op te halen ReportViewer Language Pack
1. Installeer de juiste Microsoft Report Viewer 2012 Runtime herdistribueerbare pakket van [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=317386).
2. Selecteer de taal in de vervolgkeuzelijst en de pagina wordt omgeleid naar de bijbehorende download center-pagina.
3. Klik op **downloaden** om het downloaden van ReportViewerLP.exe te starten.
4. Nadat u ReportViewerLP.exe downloaden, klikt u op **uitvoeren** onmiddellijk installeren of klik op **opslaan** wilt opslaan op uw computer. Als u klikt op **opslaan**, vergeet niet de naam van de map waar u het bestand opslaat.
5. Ga naar de map waar u het bestand hebt opgeslagen. Met de rechtermuisknop op ReportViewerLP.exe, klikt u op **als administrator uitvoeren**, en klik vervolgens op **Ja**.
6. Na het uitvoeren van ReportViewerLP.exe, ziet u de c:\windows\assembly heeft de bronbestanden **Microsoft.ReportViewer.Webforms.Resources** en **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Als u wilt configureren voor gelokaliseerde ReportViewer-besturingselement
1. Download en installeer het herdistribueerbare pakket Microsoft Report Viewer 2012 Runtime door de hierboven opgegeven instructies te volgen.
2. Maak <language> map in het project en kopieer de bijbehorende resource-assembly bestanden bevat. De resource assembly-bestanden moeten worden gekopieerd: **Microsoft.ReportViewer.Webforms.Resources.dll** en **Microsoft.ReportViewer.Common.Resources.dll**. Selecteer de resource-assembly-bestanden en stel in het deelvenster met eigenschappen **naar uitvoermap kopiëren** naar "**altijd kopiëren**'.
3. De cultuur & UICulture instellen voor de webproject. Zie voor meer informatie over het instellen van de cultuur en gebruikersinterfacecultuur voor een ASP.NET-webpagina [hoe: de cultuur en gebruikersinterfacecultuur instellen voor ASP.NET-webpagina globalisatie](https://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Configureren van verificatie en autorisatie
ReportViewer moet de juiste referenties gebruikt voor verificatie met de rapportserver en de referenties voor toegang tot de rapporten die u wilt dat door de rapportserver moeten worden geautoriseerd. Zie voor informatie over het verifiëren van het technische document [Reporting Services report viewer control en Microsoft Azure virtuele machine op basis van rapportservers](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>Publiceer de ASP.NET-webtoepassing naar Azure
Zie voor instructies over het publiceren van een ASP.NET-webtoepassing naar Azure [hoe: migreren en publiceren van een Web-App naar Azure vanuit Visual Studio](../../../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) en [aan de slag met Web-Apps en ASP.NET](../../../app-service/app-service-web-get-started-dotnet.md).

> [!IMPORTANT]
> Als de opdracht toevoegen implementatieproject voor Azure of Azure Cloud Service-Project toevoegen niet wordt weergegeven in het snelmenu in Solution Explorer, moet u mogelijk om te wijzigen van de doel-framework voor het project in .NET Framework 4.
> 
> De twee opdrachten bieden in feite dezelfde functionaliteit. Een of andere de opdracht wordt weergegeven in het snelmenu, afhankelijk van welke versie van de Microsoft Azure-SDK is geïnstalleerd.
> 
> 

## <a name="resources"></a>Resources
[Rapporten van Microsoft](https://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Business Intelligence in virtuele machines van Azure](../classic/ps-sql-bi.md)

[PowerShell gebruiken om een Azure VM te maken met een rapportserver in systeemeigen modus](../classic/ps-sql-report.md)


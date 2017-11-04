---
title: ReportViewer gebruiken in een website | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe u een Microsoft Azure-website met de Visual Studio ReportViewer-besturingselement dat wordt een rapport dat is opgeslagen op een Microsoft Azure virtuele Machine.
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
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
ms.author: asaxton
ms.openlocfilehash: c4f7c829e6fe3890342bd973185e679dd3ea2df5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>ReportViewer gebruiken op een in Azure gehoste website
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

U kunt een Microsoft Azure-website met de Visual Studio ReportViewer-besturingselement dat wordt een rapport dat is opgeslagen op een Microsoft Azure virtuele Machine maken. Het ReportViewer-besturingselement is in een webtoepassing dat u met behulp van de ASP.NET-webtoepassing toepassingssjabloon maken.

> [!IMPORTANT]
> De ASP.NET MVC-webtoepassing sjablonen bieden geen ondersteuning voor het ReportViewer-besturingselement.

Als u wilt opnemen ReportViewer in uw Microsoft Azure-website, moet u de volgende taken uitvoeren.

* **Voeg** assembly's aan het implementatiepakket
* **Configureer** verificatie en autorisatie
* **Publiceren** de ASP.NET-webtoepassing naar Azure

## <a name="prerequisites"></a>Vereisten
Raadpleeg de sectie 'algemene aanbevelingen en best practices' in [SQL Server Business Intelligence in Azure Virtual Machines](../classic/ps-sql-bi.md).

> [!NOTE]
> Besturingselementen van rapportviewer zijn verzonden met Visual Studio, Standard Edition of hoger. Als u Web Developer Express Edition gebruikt, moet u de [MICROSOFT REPORT VIEWER 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) ReportViewer runtime functies te kunnen gebruiken.
> 
> ReportViewer geconfigureerd in de van de lokale verwerkingsmodus wordt niet ondersteund in Microsoft Azure.

## <a name="adding-assemblies-to-the-deployment-package"></a>Assembly's toe te voegen aan het implementatiepakket
Wanneer u uw ASP.NET-toepassing on-premises host, wordt de rapportviewer assembly's meestal rechtstreeks in de global assemblycache (GAC) van de IIS-server worden geïnstalleerd tijdens de installatie van Visual Studio en rechtstreeks door de toepassing toegankelijk. Echter, wanneer u uw ASP.NET-toepassing in de cloud host, Microsoft Azure is niet toegestaan niets om te worden geïnstalleerd in de GAC, daarom moet u ervoor zorgen dat de ReportViewer-assembly's zijn lokaal beschikbaar voor uw toepassing. U kunt dit doen door verwijzingen naar deze in uw project toevoegen en configureren zodat ze kunnen lokaal worden gekopieerd.

Maakt gebruik de volgende assembly's in de modus externe verwerking van het ReportViewer-besturingselement:

* **Microsoft.ReportViewer.WebForms.dll**: bevat de code ReportViewer, die u wilt gebruiken, ReportViewer in uw pagina. Een verwijzing voor deze assembly wordt toegevoegd aan uw project wanneer u een ReportViewer-besturingselement naar een ASP.NET-pagina in uw project neerzetten.
* **Microsoft.ReportViewer.Common.dll**: bevat klassen die door het ReportViewer-besturingselement wordt gebruikt tijdens de uitvoering. Het is niet automatisch toegevoegd aan uw project.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Een verwijzing naar Microsoft.ReportViewer.Common toevoegen
* Met de rechtermuisknop op uw project **verwijzingen** uit en selecteer **verwijzing toevoegen**, selecteert u de assembly in het tabblad .NET en klikt u op **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>De assembly's lokaal toegankelijk te maken met uw ASP.NET-toepassing
1. In de **verwijzingen** map, klikt u op de assembly Microsoft.ReportViewer.Common zodat de bijbehorende eigenschappen worden weergegeven in het deelvenster Properties.
2. Stel in het deelvenster Properties **lokale kopie** op True.
3. Herhaal stap 1 en 2 voor Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Taalpakket voor ReportViewer ophalen
1. Installeer de juiste Microsoft Report Viewer 2012 Runtime herdistribueerbaar pakket van [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=317386).
2. Selecteer de taal in de vervolgkeuzelijst en de pagina wordt omgeleid naar de bijbehorende downloadpagina voor center.
3. Klik op **downloaden** om het downloaden van ReportViewerLP.exe te starten.
4. Nadat u ReportViewerLP.exe downloaden, klikt u op **uitvoeren** onmiddellijk installeren of klik op **opslaan** wilt opslaan op uw computer. Als u op **opslaan**, vergeet niet de naam van de map waar u het bestand opslaat.
5. Zoek de map waar u het bestand opgeslagen. Met de rechtermuisknop op ReportViewerLP.exe, klikt u op **als administrator uitvoeren**, en klik vervolgens op **Ja**.
6. Na het uitvoeren van ReportViewerLP.exe, ziet u de c:\windows\assembly heeft de bronbestanden **Microsoft.ReportViewer.Webforms.Resources** en **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Om te configureren voor gelokaliseerde ReportViewer-besturingselement
1. Download en installeer het herdistribueerbare pakket Microsoft Report Viewer 2012 Runtime door de hierboven opgegeven instructies te volgen.
2. Maak <language> map in het project en kopieer de bijbehorende resource assemblybestanden er. Worden de bronbestanden van de assembly moet worden gekopieerd: **Microsoft.ReportViewer.Webforms.Resources.dll** en **Microsoft.ReportViewer.Common.Resources.dll**. De bronbestanden van de assembly selecteren en in het deelvenster Properties instellen **naar uitvoermap kopiëren** naar '**altijd kopiëren**'.
3. De cultuur & UICulture ingesteld voor het webproject. Zie voor meer informatie over het instellen van de cultuur en gebruikersinterfacecultuur voor een ASP.NET-webpagina [hoe: de cultuur en gebruikersinterfacecultuur instellen voor ASP.NET-webpagina globalisatie](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Verificatie en autorisatie configureren
ReportViewer moet de juiste referenties gebruiken om te verifiëren met de rapportserver en de referenties voor toegang tot de rapporten die u wilt dat door de rapportserver moeten worden geautoriseerd. Zie voor informatie over het verifiëren van het witboek [Reporting Services report viewer-besturingselement en Microsoft Azure virtuele machine op basis van rapportservers](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>Publiceer de ASP.NET-webtoepassing naar Azure
Zie voor instructies over het publiceren van een ASP.NET-webtoepassing naar Azure [hoe: migreren en publiceren van een webtoepassing naar Azure vanuit Visual Studio](../../../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) en [aan de slag met Web-Apps en ASP.NET](../../../app-service/app-service-web-get-started-dotnet.md).

> [!IMPORTANT]
> Als de opdracht Azure implementatieproject toevoegen of Azure Cloud Service-Project toevoegen niet wordt weergegeven in het snelmenu in Solution Explorer, moet u wellicht de doel-framework voor het project wijzigen in .NET Framework 4.
> 
> De twee opdrachten bieden in wezen dezelfde functionaliteit. Een of andere de opdracht wordt weergegeven in het snelmenu afhankelijk van welke versie van de Microsoft Azure-SDK die u hebt geïnstalleerd.
> 
> 

## <a name="resources"></a>Resources
[Rapporten van Microsoft](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Business Intelligence in virtuele machines van Azure](../classic/ps-sql-bi.md)

[PowerShell gebruiken om een Azure VM te maken met een rapportserver in systeemeigen modus](../classic/ps-sql-report.md)


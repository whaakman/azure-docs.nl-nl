---
title: Uw app implementeren in Azure App Service met behulp van FTP/S | Microsoft Docs
description: Informatie over het implementeren van uw app in Azure App Service met behulp van de FTP- of FTPS.
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2016
ms.author: cephalin;dariac
ms.openlocfilehash: e3ac2f2156719ad975049b0c2b4cbca81d88e779
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Uw app implementeren in Azure App Service met behulp van FTP/S

Dit artikel laat zien hoe u FTP- of FTPS voor het implementeren van uw web-app, back-end voor mobiele app of API-app [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Het eindpunt van de FTP-/ S voor uw app is al actief. Er is geen configuratie nodig zijn voor de FTP-/ S-implementatie in te schakelen.

> [!IMPORTANT]
> We zijn voortdurend stappen voor verbeterde beveiliging van Microsoft Azure-Platform. Als onderdeel van deze continue inspanningen is een upgrade van webtoepassingen gepland voor Duitsland centraal en Duitsland noordoosten regio's. Tijdens dit wordt het gebruik van tekst zonder opmaak FTP-protocol voor implementaties op Web-Apps worden uitgeschakeld. Onze aanbeveling voor onze klanten, is overschakelen naar FTPS voor implementaties. We niet van plan bent een onderbreking van uw service tijdens deze upgrade die is gepland voor 9/5. Bedankt dat je hebt in deze inspanningen ondersteunen.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Stap 1: Stel de referenties voor implementatie

Voor toegang tot de FTP-server voor uw app, moet u eerst referenties voor implementatie. 

Als u wilt instellen of opnieuw instellen van uw referenties voor implementatie, Zie [referenties voor de Azure App Service-implementatie](app-service-deployment-credentials.md). Deze zelfstudie laat zien dat het gebruik van beveiliging op gebruikersniveau referenties.

## <a name="step-2-get-ftp-connection-information"></a>Stap 2: Gegevens van de FTP-verbinding ophalen

1. In de [Azure-portal](https://portal.azure.com), opent u uw app [resourceblade](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Selecteer **overzicht** Noteer de waarden voor in het menu links **FTP-/ Implementatiegebruiker gebruiker**, **FTP-hostnaam**, en **FTPS hostnaam**. 

    ![FTP-verbindingsgegevens](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > De **FTP-/ Implementatiegebruiker gebruiker** gebruiker waarde zoals weergegeven door de Azure Portal, met inbegrip van de app-naam zodat de juiste context voor de FTP-server.
    > U vindt de dezelfde informatie wanneer u selecteert **eigenschappen** in het menu links. 
    >
    > Het wachtwoord voor de implementatie wordt nooit weergegeven. Als u uw implementatie wachtwoord bent vergeten, gaat u terug naar [stap 1](#step1) en uw implementatie-wachtwoord opnieuw instellen.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>Stap 3: Bestanden implementeren in Azure

1. Van uw FTP-client ([Visual Studio](https://www.visualstudio.com/vs/community/), [FileZilla](https://filezilla-project.org/download.php?type=client), enzovoort), de verbindingsgegevens die u hebt verzameld om verbinding met uw app te gebruiken.
3. Kopieer uw bestanden en hun respectieve mapstructuur op de [ **/site/wwwroot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (of de **/site/wwwroot/App_Data/taken/** map voor WebJobs).
4. Blader naar uw app-URL om te controleren of dat de app correct wordt uitgevoerd. 

> [!NOTE] 
> In tegenstelling tot [implementaties op basis van Git](app-service-deploy-local-git.md), FTP-implementatie biedt geen ondersteuning voor de volgende implementatie automatische: 
>
> - afhankelijkheid herstellen (zoals NuGet, NPM, PIP en Composer automatische)
> - compilatie van binaire bestanden voor .NET
> - generatie van web.config (dit is een [Node.js-voorbeeld](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> U moet herstellen, maken, en deze vereiste bestanden handmatig op uw lokale machine genereren en deze samen met uw app te implementeren.
>
>

## <a name="next-steps"></a>Volgende stappen

Probeer meer geavanceerde implementatiescenario's voor [implementeren naar Azure met Git](app-service-deploy-local-git.md). Implementatie op basis van GIT in Azure kunt versiebeheer, pakket herstellen en MSBuild.

## <a name="more-resources"></a>Meer bronnen

* [Referenties voor Azure App Service-implementatie](app-service-deploy-ftp.md)

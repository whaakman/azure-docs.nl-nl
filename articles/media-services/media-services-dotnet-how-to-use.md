---
title: Computer instellen voor het ontwikkelen van Media Services met .NET
description: Meer informatie over de vereisten voor Media Services met behulp van de Media Services SDK voor .NET. Ook informatie over het maken van een Visual Studio-app.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ec2804c7-c656-4fbf-b3e4-3f0f78599a7f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako
ms.openlocfilehash: 19760b743e7cdcba3e30503090b61243911441ee
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="media-services-development-with-net"></a>Het ontwikkelen van Media Services met .NET
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Dit artikel wordt beschreven hoe u kunt beginnen met het ontwikkelen van Media Services-toepassingen met behulp van .NET.

De **Azure Media Services .NET SDK** bibliotheek kunt u voor het programmeren in Media Services met .NET. Zelfs gemakkelijker te ontwikkelen met .NET, de **Azure Media Services .NET SDK Extensions** bibliotheek is opgegeven. Deze bibliotheek bevat een set uitbreidingsmethoden en Help-functies die uw .NET-code vereenvoudigen. Beide bibliotheken zijn beschikbaar via **NuGet** en **GitHub**.

## <a name="prerequisites"></a>Vereisten
* Een Media Services-account in een nieuw of bestaand Azure-abonnement. Zie het artikel [het maken van een Media Services-Account](media-services-portal-create-account.md).
* Besturingssystemen: Windows 10, Windows 7, Windows 2008 R2 of Windows 8.
* .NET framework 4.5 of hoger.
* Visual Studio.

## <a name="create-and-configure-a-visual-studio-project"></a>Maak en configureer een Visual Studio-project.
Deze sectie wordt beschreven hoe u een project in Visual Studio maken en instellen voor het ontwikkelen van Media Services.  In dit geval wordt het project is een Windows C#-consoletoepassing, maar dezelfde installatiestappen hier weergegeven gelden voor andere soorten projecten die u voor Media Services-toepassingen (bijvoorbeeld een Windows Forms-toepassing of een ASP.NET-webtoepassing maken kunt).

Deze sectie wordt beschreven hoe u **NuGet** Media Services .NET SDK extensions en andere afhankelijke bibliotheken toe te voegen.

U kunt ook de meest recente Media Services .NET SDK bits ophalen vanuit GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) of [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), de oplossing bouwen en de verwijzingen naar de client-project toevoegen. De vereiste afhankelijkheden ophalen gedownload en uitgepakt automatisch.

1. Maak in Visual Studio een nieuwe C#-consoletoepassing. Voer de **naam**, **locatie**, en **oplossingsnaam**, en klik op OK.
2. De oplossing bouwen.
3. Gebruik **NuGet** installeren en toevoegen **Azure Media Services .NET SDK Extensions** (**windowsazure.mediaservices.extensions**). Als u dit pakket installeert, wordt ook de **Media Services .NET SDK** geïnstalleerd en worden alle andere vereiste afhankelijkheden toegevoegd.
   
    Zorg ervoor dat u de nieuwste versie van NuGet geïnstalleerd. Zie voor meer informatie en installatie-instructies, [NuGet](http://nuget.codeplex.com/).

    1. Klik in Solution Explorer met de rechtermuisknop op de naam van het project en kies **NuGet-pakketten beheren**.

    2. Het dialoogvenster NuGet-pakketten beheren wordt weergegeven.

    3. Kies in de Online-galerie, zoeken naar Azure MediaServices Extensions **Azure Media Services .NET SDK Extensions** (**windowsazure.mediaservices.extensions**), en klik vervolgens op de  **Installeer** knop.
   
    4. Het project is gewijzigd en verwijzingen naar de Media Services .NET SDK Extensions, Media Services .NET SDK en andere afhankelijke assembly's worden toegevoegd.
4. Als u een schonere ontwikkelomgeving verhogen, kunt u inschakelen NuGet-pakket herstellen. Zie voor meer informatie [NuGet-pakket herstellen '](http://docs.nuget.org/consume/package-restore).
5. Voeg een verwijzing naar **System.Configuration** assembly. Deze assembly bevat de System.Configuration. **ConfigurationManager** klasse die wordt gebruikt voor toegang tot de configuratiebestanden (bijvoorbeeld App.config).
   
    1. Om verwijzingen te voegen met het dialoogvenster referenties beheren, met de rechtermuisknop op de projectnaam in Solution Explorer. Klik vervolgens op **toevoegen**, klikt u vervolgens op **verwijzing...** .
   
    2. Het dialoogvenster referenties beheren wordt weergegeven.
    3. Onder de .NET framework-assembly's vinden en selecteer de System.Configuration-assembly en druk op **OK**.
6. Open het bestand App.config en voeg een **appSettings** sectie naar het bestand. Stel de waarden die nodig zijn voor het verbinding maken met het Media Services-API. Zie voor meer informatie [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

Stel de waarden die nodig zijn om te verbinden via de **Service-principal** verificatiemethode.  

        <configuration>
        ...
            <appSettings>
                <add key="AMSAADTenantDomain" value="tenant"/>
                <add key="AMSRESTAPIEndpoint" value="endpoint"/>
                <add key="AMSClientId" value="id"/>
                <add key="AMSClientSecret" value="secret"/>
            </appSettings>
        </configuration>
7. Voeg de **System.Configuration** verwijzing in naar uw project.
7. Overschrijf de bestaande **met** instructies aan het begin van het bestand Program.cs met de volgende code:
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

U bent nu klaar om te beginnen met het ontwikkelen van een Media Services-toepassing.    

## <a name="example"></a>Voorbeeld

Hier volgt een voorbeeld van een kleine die verbinding maakt met de AMS-API en een lijst met alle beschikbare Media-Processors.
    
    class Program
    {
        // Read values from the App.config file.

        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];
    
        private static CloudMediaContext _context = null;
        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials = 
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
    
            // List all available Media Processors
            foreach (var mp in _context.MediaProcessors)
            {
                Console.WriteLine(mp.Name);
            }
    
        }

##<a name="next-steps"></a>Volgende stappen

Nu [u kunt verbinding maken met de AMS API](media-services-use-aad-auth-to-access-ams-api.md) en start [ontwikkelen](media-services-dotnet-get-started.md).


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


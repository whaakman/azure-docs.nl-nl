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
ms.date: 08/23/2017
ms.author: juliako
ms.openlocfilehash: f5dd263a2e925989069c3b0257cfafa4c43e6157
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="media-services-development-with-net"></a>Het ontwikkelen van Media Services met .NET
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

In dit onderwerp wordt beschreven hoe beginnen met het ontwikkelen van Media Services-toepassingen met behulp van .NET.

De **Azure Media Services .NET SDK** bibliotheek kunt u voor het programmeren in Media Services met .NET. Zelfs gemakkelijker te ontwikkelen met .NET, de **Azure Media Services .NET SDK Extensions** bibliotheek is opgegeven. Deze bibliotheek bevat een set uitbreidingsmethoden en Help-functies die uw .NET-code vereenvoudigen. Beide bibliotheken zijn beschikbaar via **NuGet** en **GitHub**.

## <a name="prerequisites"></a>Vereisten
* Een Media Services-account in een nieuw of bestaand Azure-abonnement. Zie het onderwerp [Een Media Services-account maken](media-services-portal-create-account.md) voor meer informatie.
* Besturingssystemen: Windows 10, Windows 7, Windows 2008 R2 of Windows 8.
* .NET framework 4.5.
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
6. Open het bestand App.config en voeg een **appSettings** sectie naar het bestand.     
   
    Stel de waarden die nodig zijn voor het verbinding maken met het Media Services-API. Zie voor meer informatie [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

    Als u [gebruikersverificatie](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) uw configuratiebestand hebt waarschijnlijk waarden voor uw Azure AD-tenant-domein en de AMS REST API-eindpunt.
    
    >[!Note]
    >De meeste codevoorbeelden in de documentatie van Azure Media Services instellen, gebruikt u een gebruiker (interactief) type verificatie verbinding maken met de AMS-API. Deze verificatiemethode werkt goed voor beheer- of systeemeigen apps bewaking: mobiele apps, Windows-apps en toepassingen van de Console.
    
    >[!Important]
    > **Interactieve** verificatiemethode is niet geschikt is voor de server, webservices, API's typen toepassingen. Gebruik voor deze typen toepassingen, **Service-principal** verificatiemethode. Zie voor meer informatie [toegang tot de AMS-API met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

7. Overschrijf de bestaande instructies aan het begin van het bestand Program.cs **met** de volgende code.
           
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
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];
    
        private static CloudMediaContext _context = null;
        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
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


---
title: Gebruik Azure AD-verificatie voor toegang tot API voor Azure Media Services met .NET | Microsoft Docs
description: Dit onderwerp leest hoe u verificatie van Azure Active Directory (Azure AD) gebruikt voor toegang tot API voor Azure Media Services (AMS) met .NET.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: a9355200a05a3aa1b494b76977d38ddc42bfe179
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-ad-authentication-to-access-azure-media-services-api-with-net"></a>Azure AD-verificatie gebruiken voor toegang tot API voor Azure Media Services met .NET

Beginnen met windowsazure.mediaservices 4.0.0.4, Azure Media Services biedt ondersteuning voor verificatie op basis van Azure Active Directory (Azure AD). Dit onderwerp leest u hoe u Azure AD-verificatie gebruikt voor toegang tot API voor Azure Media Services met Microsoft .NET.

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
- Een Media Services-account. Zie voor meer informatie [een Azure Media Services-account maken met de Azure-portal](media-services-portal-create-account.md).
- De meest recente [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) pakket.
- Bekend bent met het onderwerp [toegang tot Azure Media Services-API met AAD verificatie-overzicht](media-services-use-aad-auth-to-access-ams-api.md). 

Wanneer u Azure AD-verificatie met Azure Media Services gebruikt, kunt u verifiëren op twee manieren:

- **Gebruikersverificatie** verifieert van een persoon die de app wordt gebruikt om te communiceren met Azure Media Services-resources. De interactieve toepassing moet eerst de gebruiker om referenties gevraagd. Een voorbeeld is een management console-app die wordt gebruikt door gemachtigde gebruikers codering taken bewaken of live streamen. 
- **Verificatie van de service-principal** verifieert u een service. Toepassingen die veel deze verificatiemethode gebruiken zijn apps die worden uitgevoerd daemon-services, middelste laag services of geplande taken, zoals web-apps, apps van de functie, logische apps, API's of microservices.

>[!IMPORTANT]
>Azure Media Service ondersteunt momenteel een model van Azure Access Control Service-verificatie. Access Control-autorisatie gaat echter op 1 juni 2018 afgeschaft. Het is raadzaam om te migreren naar een model van Azure Active Directory-verificatie zo snel mogelijk.

## <a name="get-an-azure-ad-access-token"></a>De toegang van een Azure AD-token ophalen

Voor verbinding met de API van Azure Media Services met Azure AD-verificatie, moet de client-app voor het aanvragen van een Azure AD-toegangstoken. Wanneer u de client-Media Services .NET SDK, worden veel van de informatie over het verkrijgen van een Azure AD-toegangstoken ingepakt en vereenvoudigd voor u in de [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) en [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) klassen. 

Bijvoorbeeld, u hoeft niet te geven over de Azure AD-instantie, Media Services resource-URI of systeemeigen Azure AD-App-details. Dit zijn bekende waarden die al zijn geconfigureerd door de Azure AD access tokenprovider klasse. 

Als u geen van .NET-SDK van Azure Media gebruikmaakt, raden wij aan dat u de [Azure AD-Verificatiebibliotheek](../active-directory/develop/active-directory-authentication-libraries.md). Als u de waarden voor de parameters die u wilt gebruiken met Azure AD-Verificatiebibliotheek, Zie [Azure portal gebruiken voor toegang tot Azure AD-verificatie-instellingen](media-services-portal-get-started-with-aad.md).

U hebt ook de mogelijkheid van het vervangen van de standaardimplementatie van de **AzureAdTokenProvider** met uw eigen implementatie.

## <a name="install-and-configure-azure-media-services-net-sdk"></a>Installeren en configureren van Azure Media Services .NET SDK

>[!NOTE] 
>Azure AD om verificatie te gebruiken met Media Services .NET SDK, moet u de meest recente hebben [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) pakket. Ook, Voeg een verwijzing naar de **Microsoft.IdentityModel.Clients.ActiveDirectory** assembly. Als u een bestaande app gebruikt, zijn de **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** assembly. 

1. Maak een nieuwe C#-consoletoepassing in Visual Studio.
2. Gebruik de [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet-pakket voor het installeren van **Azure Media Services .NET SDK**. 

    Als u wilt toevoegen verwijzingen via NuGet, de volgende stappen uitvoeren: in **Solution Explorer**, met de rechtermuisknop op de projectnaam en selecteer vervolgens **beheren NuGet-pakketten**. Zoek vervolgens naar **windowsazure.mediaservices** en selecteer **installeren**.
    
    -of-

    Voer de volgende opdracht **Package Manager Console** in Visual Studio.

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. Voeg **met** de broncode.

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a>Verificatie van gebruiker gebruiken

Voor verbinding met de Azure Media Service API met de optie voor verificatie, moet de client-app aanvragen van een Azure AD-token met behulp van de volgende parameters:  

- Azure AD-tenant-eindpunt. De tenant-informatie kan worden opgehaald uit de Azure-portal. Beweeg de muisaanwijzer over de aangemelde gebruiker in de rechterbovenhoek.
- Media Services resource-URI.
- Media Services (systeemeigen) toepassingsclient-id. 
- Media Services (systeemeigen)-toepassing omleidings-URI. 

De waarden voor deze parameters vindt u in **AzureEnvironments.AzureCloudEnvironment**. De **AzureEnvironments.AzureCloudEnvironment** constante is een helper in de .NET SDK om de juiste omgeving variabele instellingen voor een openbare Azure-Datacenter. 

Het bevat vooraf gedefinieerde omgevingsinstellingen voor toegang tot Media Services in de openbare datacenters alleen. Voor cloud-regio, soevereine of overheidsinstelling, kunt u **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, of **AzureGermanCloudEnvironment** respectievelijk.

Het volgende codevoorbeeld maakt een token:
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
Als u wilt beginnen met programmeren op basis van Media Services, moet u maken een **CloudMediaContext** -exemplaar dat de servercontext vertegenwoordigt. De **CloudMediaContext** bevat verwijzingen naar belangrijke verzamelingen, met inbegrip van taken, assets, bestanden, toegangsbeleid en locators. 

U moet ook doorgeven de **resource-URI voor Media Services-REST** naar de **CloudMediaContext** constructor. Als u de bron-URI voor REST mediaservices aanmelden bij de Azure-portal, selecteert u uw Azure Media Services-account, selecteert u **API-toegang**, en selecteer vervolgens **verbinding maken met Azure Media Services met gebruikersverificatie**. 

Het volgende codevoorbeeld maakt een **CloudMediaContext** exemplaar:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

Het volgende voorbeeld ziet u hoe de Azure AD-token en de context maken:

    namespace AADAuthSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Specify your Azure AD tenant domain, for example "microsoft.onmicrosoft.com".
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR AAD TENANT DOMAIN HERE}", AzureEnvironments.AzureCloudEnvironment);
    
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
            }
    
        }
    }

>[!NOTE]
>Als u krijgt een uitzondering met de tekst "de externe server heeft een fout geretourneerd: (401) niet geautoriseerd, ' Zie de [toegangsbeheer](media-services-use-aad-auth-to-access-ams-api.md#access-control) sectie van de toegang tot Azure Media Services API met overzicht van Azure AD-authenticatie.

## <a name="use-service-principal-authentication"></a>Service-principal-verificatie gebruiken
    
Aanvragen voor verbinding met de Azure Media Services-API met de service principal optie, uw app voor de middelste laag (web-API of de webtoepassing) moet een Azure AD-token met de volgende parameters:  

- Azure AD-tenant-eindpunt. De tenant-informatie kan worden opgehaald uit de Azure-portal. Beweeg de muisaanwijzer over de aangemelde gebruiker in de rechterbovenhoek.
- Media Services resource-URI.
- Waarden van Azure AD-toepassing: de **Client-ID** en **clientgeheim**.

De waarden voor de **Client-ID** en **clientgeheim** parameters vindt u in de Azure-portal. Zie voor meer informatie [aan de slag met Azure AD-verificatie met Azure portal](media-services-portal-get-started-with-aad.md).

Het volgende codevoorbeeld maakt een token met behulp van de **AzureAdTokenCredentials** constructor die **AzureAdClientSymmetricKey** als een parameter: 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

U kunt ook opgeven de **AzureAdTokenCredentials** constructor die **AzureAdClientCertificate** als parameter. 

Zie voor instructies over het maken en configureren van een certificaat in een formulier dat kan worden gebruikt door Azure AD [zich verifiëren bij Azure AD in daemon-apps met certificaten - handmatige configuratiestappen](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

Als u wilt beginnen met programmeren op basis van Media Services, moet u maken een **CloudMediaContext** -exemplaar dat de servercontext vertegenwoordigt. U moet ook doorgeven de **resource-URI voor Media Services-REST** naar de **CloudMediaContext** constructor. U krijgt de **resource-URI voor Media Services-REST** waarde vanuit Azure portal.

Het volgende codevoorbeeld maakt een **CloudMediaContext** exemplaar:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
Het volgende voorbeeld ziet u hoe de Azure AD-token en de context maken:

    namespace AADAuthSample
    {
    
        class Program
        {
            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                            new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                            AzureEnvironments.AzureCloudEnvironment);
            
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
    
                Console.ReadLine();
            }
    
        }
    }

## <a name="next-steps"></a>Volgende stappen

Aan de slag met [bestanden uploaden naar uw account](media-services-dotnet-upload-files.md).

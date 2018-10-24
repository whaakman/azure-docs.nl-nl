---
title: Gebruik Azure AD-verificatie voor toegang tot Azure Media Services API met .NET | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe verificatie van Azure Active Directory (Azure AD) gebruiken voor toegang tot Azure Media Services (AMS)-API met .NET.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: e36673c7baa03e1bcf36c149851e6455cea798fe
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945566"
---
# <a name="use-azure-ad-authentication-to-access-azure-media-services-api-with-net"></a>Azure AD-verificatie gebruiken voor toegang tot Azure Media Services API met .NET

Beginnen met windowsazure.mediaservices 4.0.0.4, Azure Media Services biedt ondersteuning voor verificatie op basis van Azure Active Directory (Azure AD). Dit onderwerp ziet u hoe u Azure AD-verificatie gebruikt voor toegang tot de API van Azure Media Services met Microsoft .NET.

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie. 
- Een Media Services-account. Zie voor meer informatie, [een Azure Media Services-account maken met de Azure-portal](media-services-portal-create-account.md).
- De meest recente [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) pakket.
- Vertrouwd zijn met het onderwerp [toegang tot Azure Media Services API met Azure AD-verificatieoverzicht](media-services-use-aad-auth-to-access-ams-api.md). 

Wanneer u Azure AD-verificatie met Azure Media Services gebruikt, kunt u verificatie in twee manieren:

- **Verificatie van de gebruiker** verifieert van een persoon die de app wordt gebruikt om te communiceren met Azure Media Services-resources. De interactieve toepassing moet eerst de gebruiker om referenties gevraagd. Een voorbeeld is een management console-app die wordt gebruikt door gemachtigde gebruikers om te controleren coderingstaken of live streamen. 
- **Service-principal verificatie** verifieert, een service. Toepassingen die gebruikmaken van deze verificatiemethode vaak zijn apps die daemon-services, services van de middelste laag of geplande taken, zoals web-apps, functie-apps, logic apps, API's of microservices worden uitgevoerd.

>[!IMPORTANT]
>Azure Media Service ondersteunt momenteel een model van Azure Access Control Service-verificatie. Access Control-autorisatie gaat echter op 22 juni 2018 afgeschaft. Het is raadzaam dat u naar een Azure Active Directory-verificatiemodel zo snel mogelijk migreert.

## <a name="get-an-azure-ad-access-token"></a>Een Azure AD-toegangstoken verkrijgen

Voor verbinding met de API van Azure Media Services met Azure AD-verificatie, moet de client-app om aan te vragen van een Azure AD-toegangstoken. Wanneer u de client-Media Services .NET SDK gebruikt, worden veel van de informatie over het verkrijgen van een Azure AD-toegangstoken verpakt en vereenvoudigd voor u in de [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) en [AzureAdTokenCredentials](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs)klassen. 

Bijvoorbeeld, u hoeft te geven over de Azure AD-instantie, Media Services-resource-URI of systeemeigen Azure AD-toepassingsgegevens. Dit zijn bekende waarden die al zijn geconfigureerd door de Azure AD access tokenprovider klasse. 

Als u .NET-SDK van Azure Media niet gebruikt, raden wij aan dat u de [Azure AD-Verificatiebibliotheek](../../active-directory/develop/active-directory-authentication-libraries.md). Als u waarden voor de parameters die u wilt gebruiken met Azure AD-Verificatiebibliotheek, Zie [Azure portal gebruiken voor toegang tot Azure AD-verificatie-instellingen](media-services-portal-get-started-with-aad.md).

U hebt ook de mogelijkheid van het vervangen van de standaardimplementatie van de **AzureAdTokenProvider** met uw eigen implementatie.

## <a name="install-and-configure-azure-media-services-net-sdk"></a>Installeren en configureren van Azure Media Services .NET SDK

>[!NOTE] 
>Voor het gebruik van Azure AD-verificatie met de Media Services .NET SDK, moet u de meest recente versie [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) pakket. Ook, Voeg een verwijzing naar de **Microsoft.IdentityModel.Clients.ActiveDirectory** assembly. Als u van een bestaande app gebruikmaakt, bevatten de **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** assembly. 

1. Maak een nieuwe C#-consoletoepassing in Visual Studio.
2. Gebruik de [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet-pakket te installeren **Azure Media Services .NET SDK**. 

    Als u wilt verwijzingen toevoegen met behulp van NuGet, de volgende stappen uitvoeren: in **Solution Explorer**, met de rechtermuisknop op de projectnaam en selecteer vervolgens **NuGet-pakketten beheren**. Zoek vervolgens **windowsazure.mediaservices** en selecteer **installeren**.
    
    -of-

    Voer de volgende opdracht **Package Manager Console** in Visual Studio.

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. Voeg **met behulp van** naar uw broncode.

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a>Verificatie van de gebruiker gebruiken

Voor verbinding met de Azure Media Service-API met de optie voor verificatie, moet de client-app om aan te vragen van een Azure AD-token met behulp van de volgende parameters:  

- Azure AD-tenant-eindpunt. De tenant-gegevens kan worden opgehaald uit de Azure-portal. Beweeg de muisaanwijzer over de gebruiker is aangemeld in de rechterbovenhoek.
- Media Services-resource-URI.
- Media Services (systeemeigen) toepassingsclient-id. 
- Media Services (native modus)-toepassing omleidings-URI. 

De waarden voor deze parameters vindt u **AzureEnvironments.AzureCloudEnvironment**. De **AzureEnvironments.AzureCloudEnvironment** constante is een helper in de .NET SDK aan de juiste omgeving variabele instellingen voor een openbare Azure-Datacenter. 

Het bevat vooraf gedefinieerde omgevingsinstellingen voor toegang tot Media Services in de openbare datacenters alleen. Voor onafhankelijke of government cloud-regio's, kunt u **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, of **AzureGermanCloudEnvironment** respectievelijk.

Het volgende codevoorbeeld wordt een token gemaakt:
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
Als u wilt beginnen met programmeren op basis van Media Services, moet u maken een **CloudMediaContext** -exemplaar dat staat voor de servercontext. De **CloudMediaContext** bevat verwijzingen naar belangrijke verzamelingen, met inbegrip van taken, assets, bestanden, toegangsbeleid en locators. 

Moet u ook om door te geven de **resource-URI voor de REST-Services Media** naar de **CloudMediaContext** constructor. Als u de resource-URI voor Media Services REST, aanmelden bij Azure portal, selecteert u uw Azure Media Services-account, selecteert u **API-toegang**, en selecteer vervolgens **verbinding maken met Azure Media Services met verificatie van de gebruiker**. 

Het volgende codevoorbeeld maakt een **CloudMediaContext** exemplaar:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

Het volgende voorbeeld laat zien hoe de Azure AD-token en de context maken:

    namespace AzureADAuthSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Specify your Azure AD tenant domain, for example "microsoft.onmicrosoft.com".
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", AzureEnvironments.AzureCloudEnvironment);
    
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
>Als er een uitzondering met de tekst ' de externe server heeft een fout geretourneerd: niet geautoriseerd (401), "Zie de [toegangsbeheer](media-services-use-aad-auth-to-access-ams-api.md#access-control) sectie van de toegang tot Azure Media Services API met een overzicht van Azure AD-verificatie.

## <a name="use-service-principal-authentication"></a>Service-principal verificatie gebruiken
    
Verzoeken om te verbinden met de Azure Media Services-API met de service principal optie, de middelste laag-app (web-API of webtoepassing) moet een Azure AD-token met de volgende parameters:  

- Azure AD-tenant-eindpunt. De tenant-gegevens kan worden opgehaald uit de Azure-portal. Beweeg de muisaanwijzer over de gebruiker is aangemeld in de rechterbovenhoek.
- Media Services-resource-URI.
- Waarden van Azure AD-toepassing: de **Client-ID** en **clientgeheim**.

De waarden voor de **Client-ID** en **clientgeheim** parameters kunnen u vinden in de Azure-portal. Zie voor meer informatie, [aan de slag met Azure AD-verificatie met behulp van de Azure-portal](media-services-portal-get-started-with-aad.md).

Het volgende codevoorbeeld wordt een token gemaakt met behulp van de **AzureAdTokenCredentials** constructor waarmee **AzureAdClientSymmetricKey** als een parameter: 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

U kunt ook opgeven de **AzureAdTokenCredentials** constructor waarmee **AzureAdClientCertificate** als parameter. 

Zie voor instructies over het maken en configureren van een certificaat in een formulier dat kan worden gebruikt door Azure AD [zich verifiëren bij Azure AD in daemon-apps met certificaten - handmatige configuratiestappen](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

Als u wilt beginnen met programmeren op basis van Media Services, moet u maken een **CloudMediaContext** -exemplaar dat staat voor de servercontext. Moet u ook om door te geven de **resource-URI voor de REST-Services Media** naar de **CloudMediaContext** constructor. Krijgt u de **resource-URI voor de REST-Services Media** waarde vanuit Azure portal.

Het volgende codevoorbeeld maakt een **CloudMediaContext** exemplaar:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
Het volgende voorbeeld laat zien hoe de Azure AD-token en de context maken:

    namespace AzureADAuthSample
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

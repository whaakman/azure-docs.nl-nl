---
title: Communicatie - hulpmiddel voor het modelleren van Microsoft Threat - beveiliging Azure | Microsoft Docs
description: oplossingen voor bedreigingen die worden weergegeven in het hulpprogramma Threat Modeling
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 71bbe53595f2afab50d6220f335d615ada957a85
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="security-frame-communication-security--mitigations"></a>Beveiliging Frame: Beveiligde communicatie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Azure Event Hub** | <ul><li>[Veilige communicatie naar Event Hub met behulp van SSL/TLS](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[Controleer of-serviceaccount bevoegdheden en controleer dat de aangepaste Services of ASP.NET-pagina's van CRM beveiliging naleven](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[Data management gateway gebruiken bij het verbinden van On-premises SQL Server met Azure Data Factory](#sqlserver-factory)</li></ul> |
| **Identity Server** | <ul><li>[Zorg ervoor dat alle verkeer naar Identity Server via HTTPS-verbinding](#identity-https)</li></ul> |
| **Webtoepassing** | <ul><li>[Controleer of X.509 certificaten gebruikt voor het verifiëren van SSL, TLS en DTLS verbindingen](#x509-ssltls)</li><li>[SSL-certificaat voor een aangepast domein configureren in Azure App Service](#ssl-appservice)</li><li>[Afdwingen van alle verkeer naar Azure App Service via HTTPS-verbinding](#appservice-https)</li><li>[HTTP-strikte transportbeveiliging (HSTS) inschakelen](#http-hsts)</li></ul> |
| **Database** | <ul><li>[Zorg ervoor dat SQL server-versleuteling en certificaat verbindingsvalidatie](#sqlserver-validation)</li><li>[Force versleutelde communicatie naar SQL server](#encrypted-sqlserver)</li></ul> |
| **Azure Storage** | <ul><li>[Zorg ervoor dat de communicatie met Azure Storage via HTTPS](#comm-storage)</li><li>[Valideren van MD5-hash na het downloaden van blob als HTTPS kan niet worden ingeschakeld.](#md5-https)</li><li>[Gebruik van SMB 3.0-compatibele client om te controleren in transit gegevensversleuteling voor het Azure-bestandsshares](#smb-shares)</li></ul> |
| **Mobiele clients** | <ul><li>[Certificaat vastmaken implementeren](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[HTTPS inschakelen - transportkanaal beveiligen](#https-transport)</li><li>[WCF: Set-berichtbeveiliging beveiligingsniveau op EncryptAndSign](#message-protection)</li><li>[WCF: Een account minste bevoegdheden gebruiken voor het uitvoeren van uw WCF-service](#least-account-wcf)</li></ul> |
| **Web-API** | <ul><li>[Zorgen dat alle verkeer naar Web-API's via HTTPS-verbinding](#webapi-https)</li></ul> |
| **Azure Redis-cache** | <ul><li>[Zorg ervoor dat de communicatie met Azure Redis-Cache via SSL](#redis-ssl)</li></ul> |
| **IoT Field Gateway** | <ul><li>[Apparaat Veldgateway communicatie tussen beveiligen](#device-field)</li></ul> |
| **IoT Cloud Gateway** | <ul><li>[Apparaat Cloudgateway communicatie tussen het gebruik van SSL/TLS beveiligen](#device-cloud)</li></ul> |

## <a id="comm-ssltls"></a>Veilige communicatie naar Event Hub met behulp van SSL/TLS

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Verificatie en beveiliging model overzicht van Event Hubs](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | AMQP of HTTP-verbindingen met Event Hub met behulp van SSL/TLS beveiligen |

## <a id="priv-aspnet"></a>Controleer of-serviceaccount bevoegdheden en controleer dat de aangepaste Services of ASP.NET-pagina's van CRM beveiliging naleven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Controleer of-serviceaccount bevoegdheden en controleer dat de aangepaste Services of ASP.NET-pagina's van CRM beveiliging naleven |

## <a id="sqlserver-factory"></a>Data management gateway gebruiken bij het verbinden van On-premises SQL Server met Azure Data Factory

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Data Factory | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Gekoppelde servicetypen - Azure en On-premises |
| **Verwijzingen**              |[Verplaatsen van gegevens tussen On premises en Azure Data Factory](https://azure.microsoft.com/documentation/articles/data-factory-move-data-between-onprem-and-cloud/#create-gateway), [Data management gateway](https://azure.microsoft.com/documentation/articles/data-factory-data-management-gateway/) |
| **Stappen** | <p>Het hulpprogramma Data Management Gateway (DMG) is vereist om verbinding met gegevensbronnen die zijn beveiligd achter een firewall of een corpnet.</p><ol><li>Vergrendelen van de machine Hiermee isoleert u het hulpprogramma DMG en voorkomt dat niet-functionerende programma van beschadigen of op de bronmachine die gegevens stelen. (Bijvoorbeeld) meest recente updates worden geïnstalleerd, enable minimale vereiste poorten, beheerde accounts inrichten, controle is ingeschakeld, schijfversleuteling ingeschakeld enz.)</li><li>De gatewaycode gegevens moet worden gedraaid met regelmatige tussenpozen of wanneer het wachtwoord voor het DMG-service wordt verlengd</li><li>Passages van gegevens via de koppeling Service moeten worden versleuteld.</li></ol> |

## <a id="identity-https"></a>Zorg ervoor dat alle verkeer naar Identity Server via HTTPS-verbinding

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [IdentityServer3 - sleutels, handtekeningen en cryptografische](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html), [IdentityServer3 - implementatie](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Stappen** | IdentityServer moet standaard alle binnenkomende verbindingen die afkomstig zijn via HTTPS. Het is absoluut verplicht dat communicatie met IdentityServer via beveiligde transporten alleen wordt uitgevoerd. Er zijn bepaalde implementatiescenario's zoals het SSL-offloading waar deze vereiste kan minder streng worden gemaakt. Zie de pagina van de identiteit van Server-implementatie in de referenties voor meer informatie. |

## <a id="x509-ssltls"></a>Controleer of X.509 certificaten gebruikt voor het verifiëren van SSL, TLS en DTLS verbindingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Toepassingen die gebruikmaken van SSL, TLS en DTLS moeten de X.509-certificaten van de entiteiten die ze verbinding met maken volledig controleren. Dit omvat de verificatie van de certificaten voor:</p><ul><li>Domeinnaam</li><li>Geldigheidsdatums (begin- en verloopdatum datums)</li><li>Kan de intrekkingsstatus</li><li>Gebruik (bijvoorbeeld Serverauthenticatie voor servers, Client-verificatie voor clients)</li><li>-Keten vertrouwen. Certificaten moeten zijn geketend aan een basiscertificeringsinstantie (CA) die wordt vertrouwd door het platform of expliciet is geconfigureerd door de beheerder</li><li>Sleutellengte van de openbare sleutel van het certificaat moet > 2048 bits</li><li>Hash-algoritme moet SHA256 en hoger |

## <a id="ssl-appservice"></a>SSL-certificaat voor een aangepast domein configureren in Azure App Service

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - Azure |
| **Verwijzingen**              | [HTTPS inschakelen voor een app in Azure App Service](../app-service/app-service-web-tutorial-custom-ssl.md) |
| **Stappen** | Standaard Azure al ingeschakeld door HTTPS voor elke app met een jokertekencertificaat voor de *. azurewebsites.net domein. Net als alle Jokertekendomeinen, het is echter niet zo veilig zijn als het gebruik van een aangepast domein met eigen certificaat [Raadpleeg](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/). Het verdient aanbeveling SSL inschakelen voor het aangepaste domein die de geïmplementeerde app wordt geopend via|

## <a id="appservice-https"></a>Afdwingen van alle verkeer naar Azure App Service via HTTPS-verbinding

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - Azure |
| **Verwijzingen**              | [Afdwingen van HTTPS in Azure App Service](../app-service/app-service-web-tutorial-custom-ssl.md#enforce-https) |
| **Stappen** | <p>Hoewel Azure kan al HTTPS voor apps van Azure-services met een jokertekencertificaat voor het domein *. azurewebsites.net, het HTTPS niet afdwingen. Bezoekers mogelijk nog steeds toegang tot de app met behulp van HTTP, Hierbij kan de app een beveiligingsrisico en daarom HTTPS moet expliciet worden afgedwongen. ASP.NET MVC-toepassingen gebruikmaken van de [RequireHttps filter](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) die zorgt ervoor dat een niet-beveiligde HTTP-aanvraag moet opnieuw worden verzonden via HTTPS.</p><p>U kunt ook kan de module herschrijven van URL's die deel uitmaakt van Azure App Service worden gebruikt om af te dwingen van HTTPS. Module herschrijven van URL's kan ontwikkelaars regels die worden toegepast op binnenkomende aanvragen voordat de aanvragen worden toegewezen aan uw toepassing definiëren. Regels voor het herschrijven van URL's zijn gedefinieerd in een web.config-bestand dat is opgeslagen in de hoofdmap van de toepassing</p>|

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld bevat een eenvoudige herschrijven van URL's regel dat ervoor zorgt al het inkomende verkeer dat voor gebruik van HTTPS
```XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```
Deze regel werkt door te retourneren van een HTTP-statuscode 301 (permanente omleiding) wanneer de gebruiker vraagt een pagina via HTTP. De 301 omgeleid naar dezelfde URL als voor de bezoeker aangevraagd, maar het HTTP-gedeelte van de aanvraag met HTTPS vervangt. Bijvoorbeeld zou HTTP://contoso.com worden omgeleid naar HTTPS://contoso.com. 

## <a id="http-hsts"></a>HTTP-strikte transportbeveiliging (HSTS) inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [OWASP HTTP strikte transportbeveiliging cheats blad](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) |
| **Stappen** | <p>HTTP strikte Transport beveiliging (HSTS) is een opt-in voor beveiligingsuitbreiding die is opgegeven door een webtoepassing met behulp van een speciale antwoordheader. Wanneer een ondersteunde browser deze header ontvangt die browser wordt voorkomen dat de communicatie van het opgegeven domein worden verzonden via HTTP en het alle communicatie via HTTPS in plaats daarvan verzendt. Dit voorkomt ook dat HTTPS klik door de aanwijzingen in browsers.</p><p>Voor het implementeren van HSTS heeft de antwoordheader van de volgende worden geconfigureerd voor een website globaal in code of in de configuratie. Strikt-Transport-beveiliging: maximumleeftijd = 300; includeSubDomains HSTS adressen van de volgende bedreigingen:</p><ul><li>Gebruiker bladwijzers of handmatig http://example.com typen en is onderworpen aan een aanvaller man-in-the-middle: HTTP-aanvragen naar HTTPS voor het doeldomein HSTS automatisch omgeleid</li><li>Webtoepassing die is bedoeld als puur HTTPS per ongeluk bevat HTTP-koppelingen of fungeert van inhoud via HTTP: HTTP-aanvragen naar HTTPS voor het doeldomein HSTS automatisch omgeleid</li><li>Een aanvaller man-in-the-middle probeert te onderscheppen verkeer van een gebruiker van het slachtoffer met behulp van een ongeldig certificaat en hoopt het ongeldige certificaat worden geaccepteerd door de gebruiker: HSTS staat niet toe dat een gebruiker voor het onderdrukken van het bericht ongeldig certificaat</li></ul>|

## <a id="sqlserver-validation"></a>Zorg ervoor dat SQL server-versleuteling en certificaat verbindingsvalidatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure  |
| **Kenmerken**              | SQL-versie - V12 |
| **Verwijzingen**              | [Aanbevolen procedures op schrijven verbindingsreeksen voor SQL-Database beveiligen](http://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **Stappen** | <p>Alle communicatie tussen de SQL-Database en een clienttoepassing worden versleuteld met behulp van Secure Sockets Layer (SSL) te allen tijde. SQL-Database biedt geen ondersteuning voor niet-versleutelde verbindingen. Voor het valideren van certificaten met toepassingscode of hulpprogramma's, expliciet een versleutelde verbinding aanvragen en de servercertificaten niet vertrouwt. Als uw toepassingscode of hulpprogramma's voor een versleutelde verbinding geen aanvragen, steeds ze nog versleutelde verbindingen</p><p>Echter de servercertificaten die mogelijk niet worden gevalideerd en worden dus vatbaar voor aanvallen 'man-in het midden'. Voor het valideren van certificaten met de ADO.NET-toepassingscode ingesteld `Encrypt=True` en `TrustServerCertificate=False` in de verbindingsreeks voor de database. Open het verbinding maken met Server-dialoogvenster voor het valideren van certificaten via SQL Server Management Studio. Klik op verbinding op het tabblad verbindingseigenschappen versleutelen</p>|

## <a id="encrypted-sqlserver"></a>Force versleutelde communicatie naar SQL server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | OnPrem |
| **Kenmerken**              | SQL-versie - MsSQL2016, SQL-versie - MsSQL2012, SQL-versie - MsSQL2014 |
| **Verwijzingen**              | [Versleutelde verbindingen naar de Database-Engine inschakelen](https://msdn.microsoft.com/library/ms191192)  |
| **Stappen** | Inschakelen van SSL-versleuteling verhoogt de beveiliging van gegevens die worden overgedragen via netwerken tussen exemplaren van SQL Server en toepassingen. |

## <a id="comm-storage"></a>Zorg ervoor dat de communicatie met Azure Storage via HTTPS

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Azure Storage transportniveau codering – via HTTPS](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_encryption-in-transit) |
| **Stappen** | Om te controleren of de beveiliging van Azure Storage-gegevens in de doorvoer, gebruik altijd het HTTPS-protocol tijdens het aanroepen van de REST API's of toegang tot in de opslag objecten. Handtekeningen voor gedeelde toegang, die kan worden gebruikt voor toegang tot Azure Storage-objecten delegeren, ook een optie om op te geven dat alleen het HTTPS-protocol kan worden gebruikt bij het gebruik van handtekeningen voor gedeelde toegang, ervoor te zorgen dat iedereen verstuurt koppelingen met SAS-tokens gebruiken het juiste protocol.|

## <a id="md5-https"></a>Valideren van MD5-hash na het downloaden van blob als HTTPS kan niet worden ingeschakeld.

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - Blob |
| **Verwijzingen**              | [Overzicht van Windows Azure-blobopslag MD5](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **Stappen** | <p>Windows Azure Blob-service biedt mechanismen om de integriteit van gegevens zowel op de toepassing en ervoor te zorgen. Als voor een bepaalde reden moet u HTTP gebruiken in plaats van HTTPS en u werkt met blok-blobs, kunt u MD5 controleren om te controleren of de integriteit van de BLOB's worden overgedragen</p><p>Dit helpt met bescherming tegen netwerk/transport layer fouten, maar niet noodzakelijkerwijs met tussenliggende aanvallen. Als u HTTPS, waardoor transport level security, dan is het gebruik van MD5 controleren redundante en onnodige gebruiken kunt.</p>|

## <a id="smb-shares"></a>Gebruik van SMB 3.0-compatibele client om te controleren in transit gegevensversleuteling naar Azure-bestandsshares

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele clients | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - bestand |
| **Verwijzingen**              | [Azure File Storage](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931), [Azure File Storage SMB-ondersteuning voor Windows-Clients](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files/#_mount-the-file-share) |
| **Stappen** | Azure File Storage ondersteunt HTTPS wanneer met de REST API, maar meer vaak gebruikt als een SMB-bestandsshare op een virtuele machine is gekoppeld. SMB 2.1 biedt geen ondersteuning voor versleuteling, zodat de verbindingen zijn alleen toegestaan binnen dezelfde regio in Azure. Echter, SMB 3.0 ondersteunt codering en kan worden gebruikt met Windows Server 2012 R2, Windows 8, Windows 8.1 en Windows 10, zodat de regio-overschrijdende toegang en zelfs toegang op het bureaublad. |

## <a id="cert-pinning"></a>Certificaat vastmaken implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, Windows Phone |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Certificaat en openbare sleutel vastmaken](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#.Net) |
| **Stappen** | <p>Certificaat vastmaken beveiligingsinfrastructuur tegen Man-In-The-Middle (MITM)-aanvallen. Vastmaken is het proces van het koppelen van een host met de verwachte X509 certificaat of de openbare sleutel. Als een certificaat of de openbare sleutel bekend of die u voor een host, wordt het certificaat of de openbare sleutel die is gekoppeld of vastgemaakt aan de host. </p><p>Dus wanneer een adversary probeert te doen SSL MITM-aanvallen, tijdens de SSL-handshake de sleutel van de hacker server zullen afwijken van de sleutel van het vastgemaakte certificaat, en de aanvraag worden genegeerd, waardoor wordt voorkomen dat MITM certificaat vastmaken kan worden bereikt implementatie van de ServicePointManager `ServerCertificateValidationCallback` delegeren.</p>|

### <a name="example"></a>Voorbeeld
```csharp
using System;
using System.Net;
using System.Net.Security;
using System.Security.Cryptography;

namespace CertificatePinningExample
{
    class CertificatePinningExample
    {
        /* Note: In this example, we're hardcoding a the certificate's public key and algorithm for 
           demonstration purposes. In a real-world application, this should be stored in a secure
           configuration area that can be updated as needed. */

        private static readonly string PINNED_ALGORITHM = "RSA";

        private static readonly string PINNED_PUBLIC_KEY = "3082010A0282010100B0E75B7CBE56D31658EF79B3A1" +
            "294D506A88DFCDD603F6EF15E7F5BCBDF32291EC50B2B82BA158E905FE6A83EE044A48258B07FAC3D6356AF09B2" +
            "3EDAB15D00507B70DB08DB9A20C7D1201417B3071A346D663A241061C151B6EC5B5B4ECCCDCDBEA24F051962809" +
            "FEC499BF2D093C06E3BDA7D0BB83CDC1C2C6660B8ECB2EA30A685ADE2DC83C88314010FFC7F4F0F895EDDBE5C02" +
            "ABF78E50B708E0A0EB984A9AA536BCE61A0C31DB95425C6FEE5A564B158EE7C4F0693C439AE010EF83CA8155750" +
            "09B17537C29F86071E5DD8CA50EBD8A409494F479B07574D83EDCE6F68A8F7D40447471D05BC3F5EAD7862FA748" +
            "EA3C92A60A128344B1CEF7A0B0D94E50203010001";


        public static void Main(string[] args)
        {
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("https://azure.microsoft.com");
            request.ServerCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) =>
            {
                if (certificate == null || sslPolicyErrors != SslPolicyErrors.None)
                {
                    // Error getting certificate or the certificate failed basic validation
                    return false;
                }

                var targetKeyAlgorithm = new Oid(certificate.GetKeyAlgorithm()).FriendlyName;
                var targetPublicKey = certificate.GetPublicKeyString();
                
                if (targetKeyAlgorithm == PINNED_ALGORITHM &&
                    targetPublicKey == PINNED_PUBLIC_KEY)
                {
                    // Success, the certificate matches the pinned value.
                    return true;
                }
                // Reject, either the key or the algorithm does not match the expected value.
                return false;
            };

            try
            {
                var response = (HttpWebResponse)request.GetResponse();
                Console.WriteLine($"Success, HTTP status code: {response.StatusCode}");
            }
            catch(Exception ex)
            {
                Console.WriteLine($"Failure, {ex.Message}");
            }
            Console.WriteLine("Press any key to end.");
            Console.ReadKey();
        }
    }
}
```

## <a id="https-transport"></a>HTTPS inschakelen - transportkanaal beveiligen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Stappen** | Configuratie van de toepassing moet ervoor zorgen dat HTTPS voor alle toegang tot gevoelige informatie wordt gebruikt.<ul><li>**UITLEG:** als een toepassing omgaat met gevoelige gegevens en geen berichtniveau versleuteling gebruikt, dan moet alleen worden toegestaan om te communiceren via een versleutelde transportkanaal.</li><li>**AANBEVELINGEN:** Zorg ervoor dat de HTTP-transport is uitgeschakeld en in plaats daarvan HTTPS-transport inschakelen. Vervang de `<httpTransport/>` met `<httpsTransport/>` label. Vertrouw niet op een netwerkconfiguratie (firewall) om te garanderen dat de toepassing kan alleen worden benaderd via een beveiligd kanaal. Vanuit een levensbeschouwelijke oogpunt van moet de toepassing niet afhankelijk van het netwerk voor de beveiliging.</li></ul><p>Vanuit een praktische oogpunt van volgen de mensen die verantwoordelijk zijn voor het beveiligen van het netwerk niet altijd de beveiligingsvereisten van de toepassing zoals ze ontwikkelen.</p>|

## <a id="message-protection"></a>WCF: Set-berichtbeveiliging beveiligingsniveau op EncryptAndSign

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff650862.aspx) |
| **Stappen** | <ul><li>**UITLEG:** wanneer beveiliging is ingesteld op 'none' Dit bericht beveiliging wordt uitgeschakeld. Vertrouwelijkheid en integriteit wordt bereikt met een passend niveau van de instelling.</li><li>**AANBEVELINGEN:**<ul><li>Wanneer `Mode=None` -bericht beveiliging uitgeschakeld</li><li>Wanneer `Mode=Sign` -tekenen maar heeft het bericht niet versleutelen; moet worden gebruikt als de integriteit van gegevens is belangrijk</li><li>Wanneer `Mode=EncryptAndSign` -tekens en versleutelt het bericht</li></ul></li></ul><p>Houd rekening met het uitschakelen van versleuteling en uw bericht alleen ondertekening wanneer u hoeft de integriteit van de gegevens zonder problemen van vertrouwelijkheid moet worden gevalideerd. Dit kan handig zijn voor bewerkingen of servicecontracten in die u wilt valideren van de oorspronkelijk afzender, maar geen vertrouwelijke gegevens worden verzonden. Wanneer het niveau van bescherming verminderen, let er dan voor dat het bericht bevat geen persoonsgegevens (PII).</p>|

### <a name="example"></a>Voorbeeld
Configureren van de service en de bewerking voor het ondertekenen van alleen het bericht wordt weergegeven in de volgende voorbeelden. Voorbeeld van de service Contract van `ProtectionLevel.Sign`: de volgt een voorbeeld van het gebruik van ProtectionLevel.Sign op het niveau van servicecontract: 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>Voorbeeld
Voorbeeld van de bewerking Contract van `ProtectionLevel.Sign` (voor gedetailleerde controle): de volgt een voorbeeld van het gebruik van `ProtectionLevel.Sign` op het niveau van de OperationContract:

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a id="least-account-wcf"></a>WCF: Een account minste bevoegdheden gebruiken voor het uitvoeren van uw WCF-service

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648826.aspx ) |
| **Stappen** | <ul><li>**UITLEG:** WCF-services onder beheer of een account met hoge bevoegdheden niet uitvoeren. in geval van inbreuk op services zal het leiden tot hoge impact.</li><li>**AANBEVELINGEN:** een account met minste bevoegdheden gebruiken voor het hosten van WCF-service omdat deze wordt Verminder de kwetsbaarheid van uw toepassing en het potentiële schade voorkomen verminderen als u wordt aangevallen. Als het serviceaccount extra toegangsrechten voor de infrastructuurresources, zoals MSMQ, het gebeurtenislogboek, prestatiemeteritems en het bestandssysteem vereist, moeten gemachtigd krijgen tot deze bronnen zodat de WCF-service met succes kunt uitvoeren.</li></ul><p>Als uw service nodig heeft voor toegang tot specifieke bronnen namens de oorspronkelijke aanvrager, gebruikt u imitatie en overdracht stromen van de aanroeper ID voor een downstream-autorisatie-controle. Gebruik het lokale netwerk-serviceaccount is een speciale ingebouwde account dat is verminderde bevoegdheden in een ontwikkelingsscenario. Maak een aangepast domein minder rechten-serviceaccount in een productiescenario.</p>|

## <a id="webapi-https"></a>Zorgen dat alle verkeer naar Web-API's via HTTPS-verbinding

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Afdwingen van SSL in een Web API-Controller](http://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **Stappen** | Als een toepassing zowel een HTTPS- en een HTTP-binding heeft, kunnen clients nog HTTP gebruiken om toegang tot de site. Om dit te voorkomen, gebruikt u een filter in te grijpen om ervoor te zorgen dat aanvragen voor beveiligde API's altijd via HTTPS zijn.|

### <a name="example"></a>Voorbeeld 
De volgende code toont een verificatie-filter van Web-API waarmee wordt gecontroleerd of SSL: 
```csharp
public class RequireHttpsAttribute : AuthorizationFilterAttribute
{
    public override void OnAuthorization(HttpActionContext actionContext)
    {
        if (actionContext.Request.RequestUri.Scheme != Uri.UriSchemeHttps)
        {
            actionContext.Response = new HttpResponseMessage(System.Net.HttpStatusCode.Forbidden)
            {
                ReasonPhrase = "HTTPS Required"
            };
        }
        else
        {
            base.OnAuthorization(actionContext);
        }
    }
}
```
Dit filter toevoegen aan de Web-API-acties die SSL vereisen: 
```csharp
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a id="redis-ssl"></a>Zorg ervoor dat de communicatie met Azure Redis-Cache via SSL

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Redis-cache | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Azure Redis-SSL-ondersteuning](https://azure.microsoft.com/documentation/articles/cache-faq/#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis) |
| **Stappen** | Redis-server biedt geen ondersteuning voor SSL gebruiksklaar, maar biedt Azure Redis-Cache. Als u verbinding met Azure Redis-Cache maakt en de client SSL, zoals StackExchange.Redis ondersteunt, moet u SSL gebruiken. Niet-SSL-poort is standaard uitgeschakeld voor nieuwe exemplaren van Azure Redis-Cache. Zorg ervoor dat de veilige standaardinstellingen niet zijn gewijzigd, tenzij er een afhankelijkheid op SSL-ondersteuning voor redis-clients. |

Houd er rekening mee dat Redis is ontworpen om te worden geopend door vertrouwde clients binnen vertrouwde omgevingen. Dit betekent dat meestal het is niet een goed idee om het Redis-exemplaar rechtstreeks met het internet of in het algemeen een omgeving waarin niet-vertrouwde clients rechtstreeks toegang de Redis-TCP-poort- of UNIX-socket tot weer te geven. 

## <a id="device-field"></a>Apparaat Veldgateway communicatie tussen beveiligen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Veld IoT Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Voor IP-gebaseerde apparaten, kan het communicatieprotocol doorgaans worden ingekapseld in een SSL/TLS-kanaal om gegevens onderweg te beschermen. Onderzoek of er beveiligde versies van het protocol waarmee beveiliging op transport of bericht-laag zijn voor andere protocollen die bieden geen ondersteuning voor SSL/TLS. |

## <a id="device-cloud"></a>Apparaat Cloudgateway communicatie tussen het gebruik van SSL/TLS beveiligen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT Cloud Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Kies uw communicatieprotocol](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#messaging) |
| **Stappen** | Protocollen die beveiligde HTTP-/ AMQP of MQTT met behulp van SSL/TLS. |

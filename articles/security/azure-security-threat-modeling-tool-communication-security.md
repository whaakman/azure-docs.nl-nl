---
title: Communicatie Security - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
description: oplossingen voor bedreigingen die beschikbaar zijn in de Threat Modeling Tool
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 8534f30c17208e77adfa47ea41506a3a61d3548d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57897296"
---
# <a name="security-frame-communication-security--mitigations"></a>Beveiliging-Frame: Communicatiebeveiliging | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Azure Event Hub** | <ul><li>[Beveiligde communicatie naar Event Hub met behulp van SSL/TLS](#comm-ssltls)</li></ul> |
| **Dynamics CRM** | <ul><li>[Controleer of bevoegdheden van het service en controleer dat de aangepaste Services of ASP.NET-pagina's van CRM-beveiliging naleven](#priv-aspnet)</li></ul> |
| **Azure Data Factory** | <ul><li>[Data management gateway gebruiken bij het verbinden van On-premises SQL Server met Azure Data Factory](#sqlserver-factory)</li></ul> |
| **Identiteitsserver** | <ul><li>[Zorg ervoor dat alle verkeer naar Identiteitsserver via HTTPS-verbinding](#identity-https)</li></ul> |
| **Web-App** | <ul><li>[Controleer of X.509 certificaten die worden gebruikt om SSL, TLS en DTLS verbindingen te verifiëren](#x509-ssltls)</li><li>[SSL-certificaat voor het aangepaste domein in Azure App Service configureren](#ssl-appservice)</li><li>[Al het verkeer naar Azure App Service via HTTPS-verbinding afdwingen](#appservice-https)</li><li>[HTTP-strikte Transport Security (HSTS) inschakelen](#http-hsts)</li></ul> |
| **Database** | <ul><li>[Zorg ervoor dat SQL server-verbinding versleuteling en certificaat-validatie](#sqlserver-validation)</li><li>[Versleutelde communicatie met SQL server afdwingen](#encrypted-sqlserver)</li></ul> |
| **Azure Storage** | <ul><li>[Zorg ervoor dat de communicatie met Azure Storage via HTTPS](#comm-storage)</li><li>[MD5-hash valideren nadat u hebt blob gedownload als HTTPS kan niet worden ingeschakeld](#md5-https)</li><li>[SMB 3.0-compatibele client gebruiken om te controleren of in-transit gegevensversleuteling voor Azure-bestandsshares](#smb-shares)</li></ul> |
| **Mobiele Client** | <ul><li>[Implementeren van certificaten vast te maken](#cert-pinning)</li></ul> |
| **WCF** | <ul><li>[HTTPS inschakelen: Beveilig transportkanaal](#https-transport)</li><li>[WCF: Ingesteld op EncryptAndSign, berichtbeveiliging beveiligingsniveau](#message-protection)</li><li>[WCF: Een account minste bevoegdheden gebruiken voor het uitvoeren van de WCF-service](#least-account-wcf)</li></ul> |
| **Web-API** | <ul><li>[Al het verkeer naar de Web-API's via HTTPS-verbinding afdwingen](#webapi-https)</li></ul> |
| **Azure Cache voor Redis** | <ul><li>[Zorg ervoor dat communicatie met Azure-Cache voor Redis via SSL](#redis-ssl)</li></ul> |
| **IoT Field Gateway** | <ul><li>[Apparaat Veldgateway communicatie beveiligen](#device-field)</li></ul> |
| **IoT Cloud Gateway** | <ul><li>[Apparaat naar Cloud-gatewaycommunicatie met behulp van SSL/TLS beveiligen](#device-cloud)</li></ul> |

## <a id="comm-ssltls"></a>Beveiligde communicatie naar Event Hub met behulp van SSL/TLS

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Event-Hubs verificatie- en beveiligingsmodellen](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | AMQP of HTTP-verbindingen naar Event Hub met behulp van SSL/TLS beveiligen |

## <a id="priv-aspnet"></a>Controleer of bevoegdheden van het service en controleer dat de aangepaste Services of ASP.NET-pagina's van CRM-beveiliging naleven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Controleer of bevoegdheden van het service en controleer dat de aangepaste Services of ASP.NET-pagina's van CRM-beveiliging naleven |

## <a id="sqlserver-factory"></a>Data management gateway gebruiken bij het verbinden van On-premises SQL Server met Azure Data Factory

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Data Factory | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | Gekoppelde servicetypen - Azure en On-premises |
| **Verwijzingen**              |[Gegevens verplaatsen tussen On-premises en Azure Data Factory](https://azure.microsoft.com/documentation/articles/data-factory-move-data-between-onprem-and-cloud/#create-gateway), [Data management gateway](https://azure.microsoft.com/documentation/articles/data-factory-data-management-gateway/) |
| **Stappen** | <p>Het hulpprogramma Data Management Gateway (DMG) is vereist om verbinding maken met gegevensbronnen die worden beveiligd achter een firewall of een bedrijfsnetwerk.</p><ol><li>Vergrendelen van de machine Hiermee isoleert u het hulpprogramma DMG en voorkomt u dat niet-functionerende programma's van beschadigen of op de bronmachine gegevens stelen. (Bijvoorbeeld) meest recente updates moeten worden geïnstalleerd, enable minimale vereiste poorten, beheerde accounts die zijn ingericht, controle ingeschakeld, schijfversleuteling ingeschakeld enz.)</li><li>Data Gateway sleutel moet worden gedraaid met regelmatige intervallen of wanneer het wachtwoord van het DMG-serviceaccount wordt vernieuwd</li><li>Passages van gegevens via de Koppelingsservice moeten worden versleuteld.</li></ol> |

## <a id="identity-https"></a>Zorg ervoor dat alle verkeer naar Identiteitsserver via HTTPS-verbinding

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Identiteitsserver | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [IdentityServer3 - sleutels, handtekeningen en cryptografische](https://identityserver.github.io/Documentation/docsv2/configuration/crypto.html), [IdentityServer3 - implementatie](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Stappen** | IdentityServer moet standaard alle binnenkomende verbindingen afkomstig zijn via HTTPS. Het is absoluut verplicht dat communicatie met IdentityServer via beveiligde transporten alleen wordt uitgevoerd. Er zijn bepaalde implementatiescenario's, zoals SSL-offloading waar deze vereiste kan worden verminderd. Zie de pagina van de implementatie-id-Server in de referenties voor meer informatie. |

## <a id="x509-ssltls"></a>Controleer of X.509 certificaten die worden gebruikt om SSL, TLS en DTLS verbindingen te verifiëren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Toepassingen die gebruikmaken van SSL, TLS en DTLS moeten de X.509-certificaten van de entiteiten die ze verbinding met maken volledig controleren. Dit omvat de verificatie van de certificaten voor:</p><ul><li>Domeinnaam</li><li>Geldigheidsdatums (zowel begin- en eindtijd)</li><li>Status van de id</li><li>Gebruik (bijvoorbeeld: Server-verificatie voor servers, clientverificatie voor clients)</li><li>Keten vertrouwen. Certificaten moeten zijn geketend aan een basiscertificeringsinstantie (CA) die wordt vertrouwd door het platform of expliciet worden geconfigureerd door de beheerder</li><li>Sleutellengte van de openbare sleutel van het certificaat moet > 2048 bits</li><li>Hash-algoritme moet SHA256 en hoger |

## <a id="ssl-appservice"></a>SSL-certificaat voor het aangepaste domein in Azure App Service configureren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - Azure |
| **Verwijzingen**              | [HTTPS inschakelen voor een app in Azure App Service](../app-service/app-service-web-tutorial-custom-ssl.md) |
| **Stappen** | Standaard Azure al schakelt HTTPS voor elke app met een certificaat met jokertekens voor de *. azurewebsites.net domein. Net als alle Jokertekendomeinen, het is echter niet zo veilig met behulp van een aangepast domein met eigen certificaat [Raadpleeg](https://casecurity.org/2014/02/26/pros-and-cons-of-single-domain-multi-domain-and-wildcard-certificates/). Het verdient aanbeveling om in te schakelen SSL voor het aangepaste domein die de door u geïmplementeerde app wordt geopend via|

## <a id="appservice-https"></a>Al het verkeer naar Azure App Service via HTTPS-verbinding afdwingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | EnvironmentType - Azure |
| **Verwijzingen**              | [HTTPS op Azure App Service afdwingen](../app-service/app-service-web-tutorial-custom-ssl.md#enforce-https) |
| **Stappen** | <p>Hoewel Azure al HTTPS voor Azure app services met een certificaat met jokertekens voor het domein kunt *. azurewebsites.net, deze HTTPS niet verplicht. Bezoekers kunnen nog steeds toegang tot de app via HTTP, die mogelijk van de app-beveiliging in gevaar en kan daarom HTTPS heeft moeten expliciet worden afgedwongen. ASP.NET MVC-toepassingen moeten gebruiken de [https vereisen filter](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) die ervoor zorgt dat een niet-beveiligde HTTP-aanvraag moet opnieuw worden verzonden via HTTPS.</p><p>U kunt ook kan de module herschrijven van URL's, dat opgenomen in Azure App Service is worden gebruikt om af te dwingen van HTTPS. Module herschrijven van URL's biedt ontwikkelaars de mogelijkheid om regels die worden toegepast op binnenkomende aanvragen voordat de aanvragen worden doorgegeven aan uw toepassing te definiëren. Regels voor het herschrijven van URL's zijn gedefinieerd in een web.config-bestand opgeslagen in de hoofdmap van de toepassing</p>|

### <a name="example"></a>Voorbeeld
Het volgende voorbeeld bevat een eenvoudige herschrijven van URL's-regel die ervoor zorgt al het binnenkomende verkeer dat voor gebruik van HTTPS
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
Deze regel werkt door een HTTP-statuscode 301 (permanente omleiding) wanneer de gebruiker een pagina in met behulp van HTTP-aanvragen. De 301 leidt de aanvraag naar de dezelfde URL als de bezoeker aangevraagd, maar vervangt het HTTP-gedeelte van de aanvraag met HTTPS. Bijvoorbeeld, HTTP://contoso.com zou worden omgeleid naar HTTPS://contoso.com. 

## <a id="http-hsts"></a>HTTP-strikte Transport Security (HSTS) inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Overzichtskaart OWASP HTTP strikte Transport Security](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) |
| **Stappen** | <p>HTTP strikte Transport Security (HSTS) is een beveiligingsuitbreiding van opt-in voor die is opgegeven door een web-App door het gebruik van een speciale response-header. Wanneer een ondersteunde browser, deze header ontvangt wordt deze browser wordt voorkomen dat alle communicatie via HTTP worden verzonden naar het opgegeven domein en ontvangt alle communicatie via HTTPS in plaats daarvan. Dit voorkomt ook dat HTTPS klik door de aanwijzingen in browsers.</p><p>Voor het implementeren van HSTS, heeft de volgende response-header moet worden geconfigureerd voor een website wereldwijd in code of in de configuratie. Strikt-Transport-Security: max-age = 300; includeSubDomains HSTS adressen bedreigingen voor de volgende:</p><ul><li>Bladwijzers van de gebruiker of handmatig typen https://example.com en is onderworpen aan een aanvaller man-in-the-middle: HTTP-aanvragen omleidt naar HTTPS voor het doeldomein HSTS automatisch</li><li>Web-App die is bedoeld voor alleen HTTPS per ongeluk worden HTTP-koppelingen bevat of waarvoor inhoud via HTTP: HTTP-aanvragen omleidt naar HTTPS voor het doeldomein HSTS automatisch</li><li>Een aanvaller man-in-the-middle probeert te onderscheppen verkeer van een gebruiker van het slachtoffer met behulp van een ongeldig certificaat en hoopt accepteert de gebruiker het ongeldige certificaat: HSTS staat niet toe dat een gebruiker het bericht ongeldig certificaat negeren</li></ul>|

## <a id="sqlserver-validation"></a>Zorg ervoor dat SQL server-verbinding versleuteling en certificaat-validatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | SQL Azure  |
| **Kenmerken**              | Versie van SQL - V12 |
| **Verwijzingen**              | [Best Practices voor schrijven verbindingsreeksen voor SQL-Database beveiligen](https://social.technet.microsoft.com/wiki/contents/articles/2951.windows-azure-sql-database-connection-security.aspx#best) |
| **Stappen** | <p>Alle communicatie tussen de SQL-Database en een clienttoepassing worden versleuteld met behulp van Secure Sockets Layer (SSL) te allen tijde. SQL Database biedt geen ondersteuning voor niet-versleutelde verbindingen. Voor het valideren van certificaten met toepassingscode of hulpprogramma's, expliciet vraag een versleutelde verbinding en de certificaten niet vertrouwt. Als uw toepassingscode of hulpprogramma's geen een versleutelde verbinding aanvragen, ontvangen ze nog steeds versleutelde verbindingen</p><p>Echter mogelijk niet gevalideerd door de servercertificaten en daarom wordt wel vatbaar voor 'man in the middle'-aanvallen. Voor het valideren van certificaten met de code van de ADO.NET-toepassing instellen `Encrypt=True` en `TrustServerCertificate=False` in de verbindingsreeks van de database. Open het dialoogvenster verbinding maken met Server voor het valideren van certificaten via SQL Server Management Studio. Klik op verbinding op het tabblad Eigenschappen van de verbinding versleutelen</p>|

## <a id="encrypted-sqlserver"></a>Versleutelde communicatie met SQL server afdwingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | OnPrem |
| **Kenmerken**              | SQL-versie - MsSQL2016, SQL-versie - MsSQL2012, versie van SQL - MsSQL2014 |
| **Verwijzingen**              | [Versleutelde verbindingen met de Database-Engine inschakelen](https://msdn.microsoft.com/library/ms191192)  |
| **Stappen** | Inschakelen van SSL-versleuteling vergroot de veiligheid van gegevens die worden verzonden via netwerken tussen exemplaren van SQL Server en toepassingen. |

## <a id="comm-storage"></a>Zorg ervoor dat de communicatie met Azure Storage via HTTPS

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Implementatie |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Versleuteling op transportniveau van Azure Storage – via HTTPS](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_encryption-in-transit) |
| **Stappen** | Om te controleren of de beveiliging van Azure Storage gegevens die worden verzonden, gebruik altijd de HTTPS-protocol als de REST API's aanroepen of toegang tot in de opslag objecten. Handtekeningen voor gedeelde toegang, die kunnen worden gebruikt voor toegang tot Azure Storage-objecten delegeren, ook een optie om op te geven dat alleen het HTTPS-protocol kan worden gebruikt bij het gebruik van handtekeningen voor gedeelde toegang, ervoor te zorgen dat iedereen verstuurt koppelingen met SAS-tokens gebruiken het juiste protocol.|

## <a id="md5-https"></a>MD5-hash valideren nadat u hebt blob gedownload als HTTPS kan niet worden ingeschakeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - Blob |
| **Verwijzingen**              | [Overzicht van Windows Azure-Blob MD5](https://blogs.msdn.microsoft.com/windowsazurestorage/2011/02/17/windows-azure-blob-md5-overview/) |
| **Stappen** | <p>Windows Azure Blob-service biedt mechanismen om te controleren of de integriteit van gegevens zowel op de toepassing en de transport-lagen. Als voor een bepaalde reden u moet voor gebruik van HTTP in plaats van HTTPS en u werkt met blok-blobs, kunt u MD5 controleren om te controleren of de integriteit van de blobs die worden overgebracht</p><p>Hiermee kunt u met beveiliging tegen fouten van netwerk/transport layer, maar niet per se met tussenliggende aanvallen. Als u HTTPS, dit transport level security biedt, gebruik van MD5 controleren is overbodig en onnodige gebruiken kunt.</p>|

## <a id="smb-shares"></a>SMB 3.0-compatibele client gebruiken om te controleren of in-transit gegevensversleuteling voor Azure-bestandsshares

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele Client | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | StorageType - bestand |
| **Verwijzingen**              | [Azure File Storage](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/#comment-2529238931), [ondersteuning voor de SMB-Azure File Storage voor Windows-Clients](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-files/#_mount-the-file-share) |
| **Stappen** | Azure File Storage biedt ondersteuning voor HTTPS wanneer u de REST-API, maar meer vaak gebruikt als een SMB-bestandsshare aan een virtuele machine is gekoppeld. SMB 2.1 biedt geen ondersteuning voor versleuteling, zodat de verbindingen zijn alleen toegestaan binnen dezelfde regio in Azure. Echter SMB 3.0 ondersteunt versleuteling, en kan worden gebruikt met Windows Server 2012 R2, Windows 8, Windows 8.1 en Windows 10, zodat de regio-overschrijdende toegang en zelfs toegang op het bureaublad. |

## <a id="cert-pinning"></a>Implementeren van certificaten vast te maken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Generic, Windows Phone |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Certificaat en openbare sleutel vast te maken](https://www.owasp.org/index.php/Certificate_and_Public_Key_Pinning#.Net) |
| **Stappen** | <p>Certificaat vastmaken beschermt tegen aanvallen van Man-In-The-Middle (MITM). Vast te maken, is het proces van het koppelen van een host met hun verwachte X509 certificaat of de openbare sleutel. Nadat u een certificaat of een openbare sleutel is bekend of gezien voor een host, wordt het certificaat of de openbare sleutel die is gekoppeld of vastgemaakt aan de host. </p><p>Dus wanneer een aanvaller probeert te doen SSL MITM-aanvallen, tijdens de SSL-handshake de sleutel van de server van de kwaadwillende persoon wordt afwijken van de sleutel van het vastgemaakte certificaat, en de aanvraag worden genegeerd, waardoor wordt voorkomen dat MITM certificaat vast te maken kan worden bereikt door implementatie van de ServicePointManager `ServerCertificateValidationCallback` delegeren.</p>|

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

## <a id="https-transport"></a>HTTPS inschakelen: Beveilig transportkanaal

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Voeg Koninkrijk](https://vulncat.fortify.com/en/detail?id=desc.semantic.dotnet.wcf_misconfiguration_transport_security_enabled) |
| **Stappen** | Configuratie van de toepassing moet ervoor zorgen dat HTTPS voor alle toegang tot gevoelige informatie wordt gebruikt.<ul><li>**EXPLANATION:** Als een toepassing omgaat met gevoelige gegevens en maakt geen gebruik van versleuteling op apparaatniveau bericht, moet klikt u vervolgens het alleen worden toegestaan om te communiceren via een gecodeerde transportkanaal.</li><li>**AANBEVELINGEN:** Zorg ervoor dat de HTTP-transport is uitgeschakeld en in plaats daarvan HTTPS-transport inschakelen. Vervang bijvoorbeeld de `<httpTransport/>` met `<httpsTransport/>` tag. Vertrouw niet op een netwerkconfiguratie (firewall) om te waarborgen dat de toepassing alleen kan worden geopend via een beveiligd kanaal. Vanuit een levensbeschouwelijke oogpunt, moet de toepassing niet afhankelijk zijn van het netwerk voor de beveiliging.</li></ul><p>Uit praktisch oogpunt volgen de mensen die verantwoordelijk zijn voor het beveiligen van het netwerk niet altijd de beveiligingsvereisten van de toepassing als ze zich ontwikkelen.</p>|

## <a id="message-protection"></a>WCF: Ingesteld op EncryptAndSign, berichtbeveiliging beveiligingsniveau

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff650862.aspx) |
| **Stappen** | <ul><li>**EXPLANATION:** Niveau is ingesteld op 'none' deze bescherming wordt wanneer het uitschakelen van bericht-beveiliging. Vertrouwelijkheid en integriteit wordt bereikt met het juiste niveau van instelling.</li><li>**AANBEVELINGEN:**<ul><li>Wanneer `Mode=None` -schakelt de bericht-beveiliging</li><li>Wanneer `Mode=Sign` -tekens, maar het bericht niet-versleuteld; moet worden gebruikt wanneer de integriteit van gegevens belangrijk is</li><li>Wanneer `Mode=EncryptAndSign` -tekens en versleutelt het bericht</li></ul></li></ul><p>Houd rekening met het uitschakelen van versleuteling en alleen uw bericht ondertekening wanneer u alleen wilt valideren van de integriteit van de gegevens zonder problemen van vertrouwelijkheid. Dit kan nuttig zijn voor bewerkingen zijn of servicecontracten in die u nodig hebt voor het valideren van de oorspronkelijke afzender, maar geen gevoelige gegevens worden verzonden. Bij het reduceren van het niveau van bescherming, zorg er dan voor dat het bericht bevat geen een persoonlijk identificeerbare informatie (PII).</p>|

### <a name="example"></a>Voorbeeld
Configureren van de service en de bewerking voor het ondertekenen van alleen het bericht wordt weergegeven in de volgende voorbeelden. Voorbeeld van de overeenkomst van de service `ProtectionLevel.Sign`: Hier volgt een voorbeeld van het gebruik van ProtectionLevel.Sign op het niveau van het servicecontract: 
```
[ServiceContract(Protection Level=ProtectionLevel.Sign] 
public interface IService 
  { 
  string GetData(int value); 
  } 
```

### <a name="example"></a>Voorbeeld
Voorbeeld van de bewerking Contract van `ProtectionLevel.Sign` (voor een nauwkeurige controle): Hier volgt een voorbeeld van het gebruik van `ProtectionLevel.Sign` op het niveau van de Webget:

```
[OperationContract(ProtectionLevel=ProtectionLevel.Sign] 
string GetData(int value);
``` 

## <a id="least-account-wcf"></a>WCF: Een account minste bevoegdheden gebruiken voor het uitvoeren van de WCF-service

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | .NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff648826.aspx ) |
| **Stappen** | <ul><li>**EXPLANATION:** WCF-services onder beheer of hoge bevoegdheid account niet uitgevoerd. in het geval van inbreuk op de services, het hoge impact leidt.</li><li>**AANBEVELINGEN:** Een account van de minste bevoegdheden gebruiken voor het hosten van de WCF-service omdat het verminderen van kwetsbaarheid voor aanvallen van uw toepassing en verlagen van de potentiële schade voorkomen als u wordt aangevallen. Als de serviceaccount extra toegangsrechten op beschikken over infrastructurele resources, zoals MSMQ, het gebeurtenislogboek, prestatiemeteritems en het bestandssysteem vereist, moeten juiste machtigingen krijgen tot deze resources zodat de WCF-service met succes kunt uitvoeren.</li></ul><p>Als uw service nodig heeft voor toegang tot bepaalde resources namens de oorspronkelijke aanvrager, gebruikt u imitatie en de delegatie op die moeten stromen van de oproepende functie identiteit voor een downstream autorisatie-controle. In een scenario voor het ontwikkelen, gebruikt het lokale netwerk service-account, dat is een speciaal ingebouwd account dat machtigingen heeft beperkt. Maak een serviceaccount van de minste bevoegdheden aangepast domein in een productiescenario voor.</p>|

## <a id="webapi-https"></a>Al het verkeer naar de Web-API's via HTTPS-verbinding afdwingen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Afdwingen van SSL in een Web-API-Controller](https://www.asp.net/web-api/overview/security/working-with-ssl-in-web-api) |
| **Stappen** | Als een toepassing zowel een HTTPS- en een HTTP-binding heeft, kunnen clients nog steeds HTTP gebruiken voor toegang tot de site. U kunt dit voorkomen door een actiefilter te gebruiken om ervoor te zorgen dat aanvragen voor beveiligde API's altijd via HTTPS zijn.|

### <a name="example"></a>Voorbeeld 
De volgende code toont een Web-API-verificatie-filter waarmee wordt gecontroleerd voor SSL: 
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
Dit filter toevoegen aan een Web-API-acties die SSL vereisen: 
```csharp
public class ValuesController : ApiController
{
    [RequireHttps]
    public HttpResponseMessage Get() { ... }
}
```
 
## <a id="redis-ssl"></a>Zorg ervoor dat communicatie met Azure-Cache voor Redis via SSL

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Cache voor Redis | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Azure Redis-SSL-ondersteuning](https://azure.microsoft.com/documentation/articles/cache-faq/#when-should-i-enable-the-non-ssl-port-for-connecting-to-redis) |
| **Stappen** | Redis-server biedt geen ondersteuning voor SSL gebruiksklaar, maar biedt Azure Cache voor Redis. Als u verbinding met Azure-Cache voor Redis maakt en de client SSL, zoals StackExchange.Redis ondersteunt, moet u SSL gebruiken. Niet-SSL-poort is standaard uitgeschakeld voor nieuwe Azure-Cache voor instanties van Redis. Zorg ervoor dat het instellen van veilige standaardwaarden niet worden gewijzigd, tenzij er een afhankelijkheid op SSL-ondersteuning voor redis-clients. |

Houd er rekening mee dat Redis is ontworpen om te worden geopend door vertrouwde clients in vertrouwde omgevingen. Dit betekent dat meestal het niet een goed idee om de Redis-exemplaar rechtstreeks met internet of in het algemeen tot een omgeving waar niet-vertrouwde clients rechtstreeks toegang de Redis-TCP-poort of UNIX-socket tot hebben zichtbaar te maken. 

## <a id="device-field"></a>Apparaat Veldgateway communicatie beveiligen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT Field Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Voor IP op basis van apparaten, kan het communicatieprotocol doorgaans worden ingekapseld in een SSL/TLS-kanaal voor het beveiligen van gegevens die onderweg zijn. Onderzoek of er zijn veilig versies van het protocol dat beveiliging op het niveau van transport of het bericht voor andere protocollen die geen ondersteuning voor SSL/TLS. |

## <a id="device-cloud"></a>Apparaat naar Cloud-gatewaycommunicatie met behulp van SSL/TLS beveiligen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT Cloud Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Kies uw communicatieprotocol](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#messaging) |
| **Stappen** | Beveiligde HTTP-/ AMQP- of MQTT-protocol met behulp van SSL/TLS. |

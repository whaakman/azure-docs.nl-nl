---
title: Axinom gebruiken om te Widevine-licenties leveren aan Azure Media Services | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure Media Services (AMS) kunt gebruiken voor het leveren van een stroom die dynamisch wordt versleuteld met AMS met zowel PlayReady als Widevine DRM's. De PlayReady-licentie is afkomstig van licentieserver Media Services PlayReady en Widevine-licentie wordt geboden door Axinom licentieserver.
services: media-services
documentationcenter: ''
author: willzhan
manager: femila
editor: ''
ms.assetid: 9c93fa4e-b4da-4774-ab6d-8b12b371631d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: willzhan;Mingfeiy;rajputam;Juliako
ms.openlocfilehash: ae9a8873be3fbd3cead23e27e80931f78ea57eb4
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992534"
---
# <a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Axinom gebruiken om Widevine-licenties te leveren aan Azure Media Services 
> [!div class="op_single_selector"]
> * [castLabs](media-services-castlabs-integration.md)
> * [Axinom](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a>Overzicht
Azure Media Services (AMS) heeft extra Google Widevine dynamische beveiliging (Zie [van Mingfei blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) voor meer informatie). Bovendien Azure Media Player (AMP) ook ondersteuning voor Widevine is toegevoegd (Zie [AMP document](http://amp.azure.net/libs/amp/latest/docs/) voor meer informatie). Dit is een belangrijke vervullen in de streaming-DASH-inhoud beveiligd met CENC met meerdere-native-DRM (PlayReady en Widevine) op moderne browsers die zijn uitgerust met MSE en EME.

Media Services kunt te beginnen met de Media Services SDK voor .NET versie 3.5.2 gebruikt, u om te configureren van Widevine-licentiesjabloon en Widevine-licenties. U kunt ook de volgende AMS-partners gebruiken om Widevine-licenties te leveren: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

In dit artikel wordt beschreven hoe om te integreren en Widevine-licentieserver die wordt beheerd door Axinom testen. Met name het komen aan bod:  

* Dynamic Common Encryption configureren met multi-DRM (PlayReady en Widevine) met de bijbehorende licentie overname URL;
* Een JWT-token genereren om te voldoen aan de vereisten van de licentie-server;
* Azure Media Player-app, waar verwerving van licenties met JWT-tokenverificatie; ontwikkelen

Het volledige systeem en de stroom van inhoudssleutel, sleutel-ID, sleutel seed JTW token en de claims kunnen beste worden beschreven op basis van het volgende diagram:

![DASH en CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a>Content Protection
Raadpleeg de Mingfei blog voor het configureren van dynamische beveiliging en belangrijke leveringsbeleid voor: [Widevine-verpakking configureren met Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

U kunt dynamische CENC beveiliging configureren met multi-DRM voor DASH streaming beide van de volgende:

1. PlayReady-bescherming voor Microsoft Edge en IE11, die een beperking van de token autorisatie kunnen hebben. Beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een Secure Token Service (STS), zoals Azure Active Directory.
2. Widevine-beveiliging voor Chrome, kan er tokenverificatie vereist met een token dat is uitgegeven door een andere STS. 

Zie [genereren van JWT-tokens](media-services-axinom-integration.md#jwt-token-generation) sectie voor waarom Azure Active Directory kan niet worden gebruikt als een STS voor Axinom van Widevine-licentie-server.

### <a name="considerations"></a>Overwegingen
1. Moet u de opgegeven Axinom sleutel seed (8888000000000000000000000000000000000000) en uw gegenereerd of de geselecteerde sleutel-ID voor het genereren van de inhoudssleutel voor het configureren van sleutelleveringsservice. Axinom licentieserver alle licenties die inhoud sleutels op basis van de dezelfde sleutel seed die geldig is voor het testen en productie.
2. De Widevine-licentie URL voor het verkrijgen voor het testen: [ https://drm-widevine-licensing.axtest.net/AcquireLicense ](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP- en HTTS zijn toegestaan.

## <a name="azure-media-player-preparation"></a>Azure Media Player voorbereiden
AMP v1.4.0 biedt ondersteuning voor het afspelen van AMS-inhoud die dynamisch wordt geleverd met zowel PlayReady als Widevine DRM.
Als tokenverificatie Widevine-licentieserver niet vereist, is er niets meer hoeft te doen als u wilt een DASH-inhoud beveiligd met Widevine testen. Voor bijvoorbeeld het team en biedt een eenvoudige [voorbeeld](https://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevineFairPlay_notoken.html), waar u het werken in Microsoft Edge en IE11 met PlayReady en Chrome met Widevine kunt zien.
De server Widevine-licentie van Axinom vereist JWT-token verificatie. De JWT-token moet worden verzonden met licentieaanvraag via een HTTP-header 'X-AxDRM-bericht'. Voor dit doel moet u de volgende javascript toevoegen in de webpagina die als host fungeert en voordat u de bron:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

De rest van de code en is standaard AMP API zoals in het document AMP [hier](http://amp.azure.net/libs/amp/latest/docs/).

De bovenstaande javascript voor instelling aangepaste autorisatie-header is nog steeds een benadering op korte termijn voordat de officiële op lange termijn benadering in AMP wordt uitgebracht.

## <a name="jwt-token-generation"></a>Genereren van JWT-tokens
Axinom Widevine-licentie-server voor het testen van vereist JWT-token verificatie. Bovendien is een van de claims in de JWT-token van een complex object in plaats van primitieve gegevenstype.

Azure AD kan helaas alleen JWT-tokens uitgeven met primitieve typen. Op dezelfde manier, .NET Framework-API (System.IdentityModel.Tokens.SecurityTokenHandler en JwtPayload) kunt u alleen voor het invoeren van complexe objecttype als claims. De claims worden echter nog steeds geserialiseerd als tekenreeks. Daarom niet we gebruiken een van de twee voor het genereren van de JWT-token voor aanvraag van Widevine-licentie.

John Sheehan van [JWT NuGet-pakket](https://www.nuget.org/packages/JWT) voldoet aan de behoeften, zodat we gaan dit NuGet-pakket gebruiken.

Hieronder volgt de code voor het genereren van JWT-token met de benodigde claims zoals vereist door de licentieserver Axinom Widevine voor het testen van:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;

    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.

                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };

                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);

                return token;
            }

            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }

                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }

                return HexAsBytes;
            }

        }  

    }  

Axinom Widevine-licentieserver

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

### <a name="considerations"></a>Overwegingen
1. Hoewel leveringsservice voor AMS PlayReady-licentie is vereist ' Bearer = "voorafgaand aan een verificatietoken, Axinom Widevine-licentieserver maakt geen gebruik.
2. De sleutel van de communicatie Axinom wordt gebruikt als het ondertekenen van de sleutel. De sleutel is een hexadecimale tekenreeks, maar deze moet worden behandeld als een reeks bytes niet een tekenreeks wanneer codering. Dit wordt bereikt door de methode ConvertHexStringToByteArray.

## <a name="retrieving-key-id"></a>Bij het ophalen van sleutel-ID
U wellicht hebt opgemerkt dat in de code voor het genereren van een JWT-token, sleutel-ID is vereist. Omdat de JWT-token behoeften gereed is voordat het laden en-speler, sleutel moet-ID worden opgehaald om lange JWT-token.

Natuurlijk, er zijn meerdere manieren voor het ophalen van sleutel-id. Bijvoorbeeld, een kan worden opgeslagen ID sleutel samen met de metagegevens van inhoud in een database. Haalt u sleutel-ID van DASH MPD (presentatie Mediabeschrijving)-bestand. De onderstaande code is voor de laatste.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }

        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();

        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);

        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }

        return key_id;
    }

## <a name="summary"></a>Samenvatting
Met de meest recente toevoeging van Widevine-ondersteuning in zowel Azure Media Services Content Protection en Azure Media Player kunnen kunnen we implementeren met het streamen van DASH + meerdere-native-DRM (PlayReady en Widevine) met zowel PlayReady-licentieservice AMS- en Widevine-licentie de server van Axinom voor de volgende moderne browsers:

* Chrome
* Microsoft Edge on Windows 10
* IE 11 voor Windows 8.1 en Windows 10
* Zowel Firefox (Desktop) en Safari op Mac (niet iOS) worden ook ondersteund via Silverlight en dezelfde URL met Azure Media Player

De volgende parameters zijn vereist in de mini oplossing gebruik te maken van Axinom Widevine-licentie-server. Met uitzondering van sleutel-ID, de rest van de parameters worden geleverd door Axinom op basis van hun Widevine-server-instellingen.

| Parameter | Hoe deze wordt gebruikt |
| --- | --- |
| Communicatie sleutel-ID |Moet worden opgenomen als waarde van de claim 'com_key_id' in de JWT-token (Zie [dit](media-services-axinom-integration.md#jwt-token-generation) sectie). |
| Communicatie-sleutel |Als de ondertekeningssleutel van JWT-token moet worden gebruikt (Zie [dit](media-services-axinom-integration.md#jwt-token-generation) sectie). |
| Belangrijkste seed |Moet worden gebruikt voor het genereren van inhoudssleutel met bepaalde inhoud ID-sleutel (Zie [dit](media-services-axinom-integration.md#content-protection) sectie). |
| URL voor het verkrijgen van Widevine-licentie |Moet worden gebruikt bij het configureren van het leveringsbeleid voor Assets voor het streamen van streepje (Zie [dit](media-services-axinom-integration.md#content-protection) sectie). |
| Inhoud sleutel-ID |Moet worden opgenomen als onderdeel van de waarde van de claim rechten van een bericht van JWT-token (Zie [dit](media-services-axinom-integration.md#jwt-token-generation) sectie). |

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Bevestigingen
Wij willen graag om te bevestigen van de volgende personen die heeft bijgedragen tot het maken van dit document: Kristjan Jõgi van Axinom, Mingfei Yan en Amit Rajput.


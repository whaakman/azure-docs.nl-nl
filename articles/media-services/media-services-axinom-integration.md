---
title: Met behulp van Axinom Widevine-licenties te leveren met Azure Media Services | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure Media Services (AMS) kunt gebruiken voor het leveren van een stroom die dynamisch wordt versleuteld met AMS met PlayReady en Widevine DRM's. De licentie PlayReady afkomstig is van de licentieserver Media Services PlayReady en Widevine-licentie wordt geleverd door de licentieserver Axinom.
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 9c93fa4e-b4da-4774-ab6d-8b12b371631d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;Mingfeiy;rajputam;Juliako
ms.openlocfilehash: 9a3aa1680ada03e4472db3a198a3b806511671ed
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Axinom gebruiken om Widevine-licenties te leveren aan Azure Media Services
> [!div class="op_single_selector"]
> * [castLabs](media-services-castlabs-integration.md)
> * [Axinom](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a>Overzicht
Azure Media Services (AMS) is toegevoegd Google Widevine dynamic beveiliging (Zie [blog van Mingfei](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) voor meer informatie). Bovendien Azure Media Player (AMP) is ook Widevine ondersteuning toegevoegd (Zie [AMP document](http://amp.azure.net/libs/amp/latest/docs/) voor meer informatie). Dit is een belangrijke vervullen in de streaming DASH-inhoud beveiligd met CENC met meerdere-native-DRM (PlayReady en Widevine) moderne browsers zijn uitgerust met muis en EME.

Media Services kunt vanaf de Media Services SDK voor .NET versie 3.5.2, u de sjabloon voor Widevine-licentie configureren en ophalen van Widevine-licenties. U kunt ook de volgende AMS-partners gebruiken om Widevine-licenties te leveren: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) en [castLabs](http://castlabs.com/company/partners/azure/).

In dit artikel wordt beschreven hoe integreren en Widevine-licentieserver beheerd door Axinom testen. In het bijzonder omvatten ze:  

* Dynamic Common Encryption configureren met multi-DRM (PlayReady en Widevine) met de bijbehorende licentie overname-URL's;
* Een JWT-token genereren om te voldoen aan de vereisten voor productlicenties server;
* Azure Media Player-app die aanschaf van licentie met JWT-token verificatie zorgt; ontwikkelt

Het volledige systeem als de stroom van inhoudssleutel, sleutel-ID, sleutel seed JTW token en de claims kunnen beste door het volgende diagram worden beschreven:

![DASH en CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a>Content Protection
Zie voor het configureren van dynamische beveiliging en belangrijke leveringsbeleid Mingfei van blog: [Widevine-verpakking met Azure Media Services configureren](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

U kunt dynamische CENC beveiliging configureren met multi-DRM voor beide van de volgende streaming DASH:

1. PlayReady-beveiliging voor MS Edge en IE11 die een token autorisatie-beperking kan hebben. Beleid met de tokenbeperking moet vergezeld gaan van een token dat is uitgegeven door een Secure Token Service (STS), zoals Azure Active Directory.
2. Widevine-beveiliging voor Chrome, kan deze tokenverificatie vereisen met token dat is uitgegeven door een andere STS. 

Zie [genereren van JWT-tokens](media-services-axinom-integration.md#jwt-token-generation) sectie voor waarom Azure Active Directory kan niet worden gebruikt als een STS voor Axinom van Widevine-licentieserver.

### <a name="considerations"></a>Overwegingen
1. Moet u de opgegeven Axinom sleutel seed (8888000000000000000000000000000000000000) en de gegenereerde of geselecteerde ID voor het genereren van de inhoudssleutel voor het configureren van de service voor het leveren van sleutel van de sleutel. Axinom licentieserver alle licenties die inhoud sleutels op basis van de dezelfde sleutel seed die geldig voor testen en productie is.
2. De URL Widevine-licentie verkrijgen voor het testen: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP- en HTTS zijn toegestaan.

## <a name="azure-media-player-preparation"></a>Azure Media Player voorbereiding
AMP v1.4.0 ondersteunt het afspelen van inhoud van het AMS die dynamisch wordt verpakt met PlayReady en Widevine DRM.
Als de licentieserver Widevine tokenverificatie niet vereist, zijn er geen extra dat u moet doen om de inhoud van een streepje beveiligd met Widevine testen. Voor een voorbeeld het team AMP biedt een eenvoudige [voorbeeld](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), waar u deze in de werkt rand en IE11 met PlayReady en Chrome met Widevine kunt zien.
De Widevine-licentie geleverd door Axinom JWT-token is verificatie vereist. De JWT-token moet worden verzonden met licentieaanvraag via een HTTP-header 'X AxDRM bericht'. Hiertoe moet u de volgende javascript toevoegen op de webpagina die als host fungeert AMP voordat u de bron:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

De rest van AMP code is standaard AMP API zoals in het document AMP [hier](http://amp.azure.net/libs/amp/latest/docs/).

De bovenstaande javascript voor instelling aangepaste autorisatie-header is nog steeds een korte termijn benadering voordat de officiële op lange termijn benadering in AMP wordt uitgebracht.

## <a name="jwt-token-generation"></a>Genereren van JWT-tokens
Axinom Widevine-licentie voor het testen van JWT-token is verificatie vereist. Bovendien is een van de claims in de JWT-token van een complex type in plaats van een primitief gegevenstype.

Azure AD kunt helaas alleen JWT-tokens uitgeven met primitieve typen. Op deze manier kunt .NET Framework-API (System.IdentityModel.Tokens.SecurityTokenHandler en JwtPayload) u alleen voor het invoeren van complexe objecttype als claims. De claims zijn echter nog steeds geserialiseerd als tekenreeks. Er kan daarom geen gebruikmaken van de twee voor het genereren van de JWT-token voor Widevine-licentie-aanvraag.

John Sheehan van [JWT NuGet-pakket](https://www.nuget.org/packages/JWT) voldoet aan de behoeften, dus gaan we gebruiken deze NuGet-pakket.

Hieronder volgt de code voor het genereren van JWT-token met de benodigde claims door de server voor Axinom Widevine-licentie vereist voor het testen:

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
1. Hoewel AMS PlayReady-service voor het leveren van licentie is vereist ' Bearer = "geen verificatietoken verdergaat Axinom Widevine-licentieserver gebruikt deze niet.
2. De communicatie Axinom sleutel wordt gebruikt als de handtekeningsleutel. De sleutel is een hexadecimale tekenreeks, maar deze moet worden behandeld als een reeks bytes niet naar een tekenreeks als codering. Dit wordt bereikt door de methode ConvertHexStringToByteArray.

## <a name="retrieving-key-id"></a>Bij het ophalen van sleutel-ID
U mogelijk opgevallen dat in de code voor het genereren van een JWT-token, sleutel-ID is vereist. Aangezien de JWT-token behoeften gereed is voordat het laden van de AMP player sleutel moet-ID worden opgehaald om te genereren van JWT-token.

Natuurlijk, er zijn meerdere manieren om op te halen van de blokkering van sleutel-id. Bijvoorbeeld, een kan opslaan ID sleutel samen met de metagegevens van inhoud in een database. Of u kunt ophalen sleutel-ID van het streepje MPD (beschrijving Media presentatie)-bestand. De volgende code is voor de laatste.

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
Met de meest recente toevoeging van Widevine-ondersteuning in Azure Media Services Content Protection zowel Azure Media Player kunnen we voor het implementeren van streaming van DASH + meerdere-native-DRM (PlayReady en Widevine) met zowel PlayReady-licentie-service in AMS en Widevine-licentie de server van Axinom voor de volgende moderne browsers:

* Chrome
* Microsoft Edge op Windows 10
* IE 11 op Windows 8.1 en Windows 10
* Zowel Firefox (Desktop) en Safari op Mac (geen iOS), worden ook ondersteund via Silverlight en dezelfde URL met Azure Media Player

De volgende parameters zijn vereist in de licentieserver Mini-oplossing van Axinom Widevine. Met uitzondering van sleutel-ID, de rest van de parameters worden geleverd door Axinom op basis van hun installatie Widevine-server.

| Parameter | Hoe deze wordt gebruikt |
| --- | --- |
| Communicatie ID sleutel |Moet worden opgenomen als waarde van de claim 'com_key_id' in de JWT-token (Zie [dit](media-services-axinom-integration.md#jwt-token-generation) sectie). |
| Communicatie-sleutel |Moeten worden gebruikt als de ondertekeningssleutel van JWT-token (Zie [dit](media-services-axinom-integration.md#jwt-token-generation) sectie). |
| Sleutel seed |Moet worden gebruikt voor het genereren van inhoudssleutel met alle inhoud van de opgegeven ID sleutel (Zie [dit](media-services-axinom-integration.md#content-protection) sectie). |
| URL voor Widevine-licentie |Moet worden gebruikt bij het configureren van het leveringsbeleid voor Assets voor DASH streaming (Zie [dit](media-services-axinom-integration.md#content-protection) sectie). |
| Inhoud sleutel-ID |Moet worden opgenomen als onderdeel van de waarde van de claim recht bericht van JWT-token (Zie [dit](media-services-axinom-integration.md#jwt-token-generation) sectie). |

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Bevestigingen
Wij willen graag erkent de volgende personen die hebben bijgedragen tot de verwezenlijking van dit document: Kristjan Jõgi van Axinom Mingfei Yan en Amit Rajput.


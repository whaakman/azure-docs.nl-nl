---
title: Voer de validatie - hulpmiddel voor het modelleren van Microsoft Threat - Azure | Microsoft Docs
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
ms.openlocfilehash: b7ce6f353cf8cf48d5fb038ee77b0d3fdae16fb7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-input-validation--mitigations"></a>Beveiliging Frame: Invoervalidatie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Webtoepassing** | <ul><li>[Schakel XSLT uitvoeren van scripts voor alle transformaties met behulp van niet-vertrouwde opmaakmodellen](#disable-xslt)</li><li>[Zorg ervoor dat elke pagina waarvan de gebruiker instelbare inhoud buiten automatische MIME-controle kiest](#out-sniffing)</li><li>[Beperken of XML-entiteit omzetting uitschakelen](#xml-resolution)</li><li>[Toepassingen die gebruikmaken van http.sys uitvoeren URL standaardisatie verificatie](#app-verification)</li><li>[Zorg passende controles zijn geïmplementeerd tijdens het accepteren van bestanden van gebruikers](#controls-users)</li><li>[Zorg ervoor dat type veilig parameters in webtoepassing worden gebruikt voor toegang tot gegevens](#typesafe)</li><li>[Afzonderlijke model binding klassen gebruiken of bindingsfilter weer om te voorkomen dat MVC massaopslag toewijzing beveiligingslek](#binding-mvc)</li><li>[Niet-vertrouwde webuitvoer vóór rendering coderen](#rendering)</li><li>[Validatie voor invoer- en filteren op alle tekenreekstype modeleigenschappen uitvoeren](#typemodel)</li><li>[Opschoning moet worden toegepast op formuliervelden die tekens, bijvoorbeeld, RTF-editor accepteren](#richtext)</li><li>[Geen DOM elementen toewijzen aan put waarvoor geen ingebouwde codering](#inbuilt-encode)</li><li>[Valideer alle omleidingen in de toepassing worden gesloten of veilig gedaan](#redirect-safe)</li><li>[Validatie voor invoer op alle tekenreeks typeparameters geaccepteerd door de methoden van de domeincontroller implementeren](#string-method)</li><li>[Bovengrens-out voor reguliere expressie om te voorkomen van DoS vanwege ongeldige reguliere expressies verwerken instellen](#dos-expression)</li><li>[Vermijd het gebruik van Html.Raw in Razor weergaven](#html-razor)</li></ul> | 
| **Database** | <ul><li>[Gebruik geen dynamische query's in de opgeslagen procedures](#stored-proc)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat de validatie is uitgevoerd voor Web-API-methoden](#validation-api)</li><li>[Validatie voor invoer op alle tekenreeks typeparameters geaccepteerd door de methoden van de Web-API implementeren](#string-api)</li><li>[Zorg ervoor dat type veilig parameters in Web-API worden gebruikt voor toegang tot gegevens](#typesafe-api)</li></ul> | 
| **Azure Documentdb** | <ul><li>[SQL-query's constructorreeks mag gebruiken voor DocumentDB](#sql-docdb)</li></ul> | 
| **WCF** | <ul><li>[Validatie van de WCF-invoer via schemabinding](#schema-binding)</li><li>[Validatie van de WCF - invoer via Parameter Inspectors](#parameters)</li></ul> |

## <a id="disable-xslt"></a>Schakel XSLT uitvoeren van scripts voor alle transformaties met behulp van niet-vertrouwde opmaakmodellen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Beveiliging van de XSLT-](https://msdn.microsoft.com/library/ms763800(v=vs.85).aspx), [eigenschap XsltSettings.EnableScript](http://msdn.microsoft.com/library/system.xml.xsl.xsltsettings.enablescript.aspx) |
| **Stappen** | XSLT ondersteunt scripting binnen StyleSheets met behulp van de `<msxml:script>` element. Hiermee kunt aangepaste functies worden gebruikt in een XSLT-transformatie. Het script wordt uitgevoerd onder de context van het proces voor het uitvoeren van de transformatie. XSLT-script moet worden uitgeschakeld wanneer u zich in een niet-vertrouwde omgeving om te voorkomen dat de uitvoering van niet-vertrouwde code. *Als met .NET:* XSLT-scripting is standaard uitgeschakeld; echter, moet u ervoor zorgen dat deze is niet expliciet ingeschakeld via de `XsltSettings.EnableScript` eigenschap.|

### <a name="example"></a>Voorbeeld 

```C#
XsltSettings settings = new XsltSettings();
settings.EnableScript = true; // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Voorbeeld
Als u met behulp van MSXML 6.0, is XSLT-scripting standaard uitgeschakeld. echter, moet u ervoor zorgen dat deze is niet expliciet ingeschakeld via de eigenschap XML DOM-object AllowXsltScript. 

```C#
doc.setProperty("AllowXsltScript", true); // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Voorbeeld
Als u van MSXML 5 gebruikmaakt of hieronder XSLT-scripting is standaard ingeschakeld en u expliciet moet uitschakelen. De eigenschap XML DOM-object AllowXsltScript ingesteld op false. 

```C#
doc.setProperty("AllowXsltScript", false); // CORRECT. Setting to false disables XSLT scripting.
```

## <a id="out-sniffing"></a>Zorg ervoor dat elke pagina waarvan de gebruiker instelbare inhoud buiten automatische MIME-controle kiest

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [IE8 Beveiliging deel V - Uitgebreide beveiliging](http://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)  |
| **Stappen** | <p>Voor elke pagina die gebruiker instelbare inhoud kan bevatten, moet u de HTTP-Header `X-Content-Type-Options:nosniff`. U kunt de vereiste koptekst per pagina voor deze pagina's die mogelijk door de gebruiker instelbare inhoud bevat ofwel instellen om te voldoen aan deze eis wordt voldaan, of kunt u deze globaal instellen voor alle pagina's in de toepassing.</p><p>Elk type bestand geleverd door een webserver heeft een bijbehorende [MIME-type](http://en.wikipedia.org/wiki/Mime_type) (ook wel een *inhoudstype*) die worden beschreven de aard van de inhoud (dat wil zeggen, image, tekst, toepassing, enz.)</p><p>De header X-inhoud-Type-opties is een HTTP-header waarmee ontwikkelaars om op te geven dat de inhoud ervan niet MIME-sniff worden moet. Deze header is ontworpen om te bekijken van MIME-aanvallen te verminderen. Ondersteuning voor deze header is toegevoegd in Internet Explorer 8 (IE8)</p><p>Alleen gebruikers van Internet Explorer 8 (IE8) profiteren van de X-inhoud-Type-opties. Vorige versies van Internet Explorer bieden momenteel geen ondersteuning voor de header X-inhoud-Type-opties</p><p>Internet Explorer 8 (of hoger) zijn alleen primaire browsers voor het implementeren van een opt-out bekijken van MIME-functie. Als andere belangrijke browsers (Firefox, Safari, Chrome) soortgelijke functies implementeren, zal deze aanbeveling worden bijgewerkt met de syntaxis voor deze browsers ook</p>|

### <a name="example"></a>Voorbeeld
Om de vereiste koptekst globaal voor alle pagina's in de toepassing, kunt u het volgende doen: 

* De koptekst toevoegen in het web.config-bestand als de toepassing wordt gehost door Internet Information Services (IIS) 7 

```
<system.webServer> 
  <httpProtocol> 
    <customHeaders> 
      <add name=""X-Content-Type-Options"" value=""nosniff""/>
    </customHeaders>
  </httpProtocol>
</system.webServer> 
```

* De koptekst tot en met de globale toepassing toevoegen\_BeginRequest 

``` 
void Application_BeginRequest(object sender, EventArgs e)
{
  this.Response.Headers[""X-Content-Type-Options""] = ""nosniff"";
} 
```

* Aangepaste HTTP-module implementeren 

``` 
public class XContentTypeOptionsModule : IHttpModule 
  {
    #region IHttpModule Members 
    public void Dispose() 
    { 

    } 
    public void Init(HttpApplication context)
    { 
      context.PreSendRequestHeaders += newEventHandler(context_PreSendRequestHeaders); 
    } 
    #endregion 
    void context_PreSendRequestHeaders(object sender, EventArgs e) 
      { 
        HttpApplication application = sender as HttpApplication; 
        if (application == null) 
          return; 
        if (application.Response.Headers[""X-Content-Type-Options ""] != null) 
          return; 
        application.Response.Headers.Add(""X-Content-Type-Options "", ""nosniff""); 
      } 
  } 

``` 

* U kunt de vereiste koptekst alleen voor specifieke's toevoegen aan afzonderlijke antwoorden inschakelen: 

```
this.Response.Headers[""X-Content-Type-Options""] = ""nosniff""; 
``` 

## <a id="xml-resolution"></a>Beperken of XML-entiteit omzetting uitschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [XML-entiteit uitbreiding](http://capec.mitre.org/data/definitions/197.html), [XML Denial of Service-aanvallen en beveiliging](http://msdn.microsoft.com/magazine/ee335713.aspx), [MSXML beveiligingsoverzicht](http://msdn.microsoft.com/library/ms754611(v=VS.85).aspx), [aanbevolen procedures voor het beveiligen van MSXML Code](http://msdn.microsoft.com/library/ms759188(VS.85).aspx), [NSXMLParserDelegate Protocolnaslaginformatie](http://developer.apple.com/library/ios/#documentation/cocoa/reference/NSXMLParserDelegate_Protocol/Reference/Reference.html), [het omzetten van externe verwijzingen](https://msdn.microsoft.com/library/5fcwybb2.aspx) |
| **Stappen**| <p>Hoewel deze optie niet veel gebruikt is, is er een functie van XML waarmee de XML-parser macro entiteiten uitbreiden met waarden die zijn gedefinieerd in het document zichzelf of van externe bronnen. Kan het document een entiteit 'NaamBedrijf"met de waarde 'Microsoft', bijvoorbeeld zo definiëren dat telkens wanneer de tekst '&companyname;' wordt weergegeven in het document, wordt deze automatisch vervangen door de tekst van Microsoft. Of het document een entiteit 'MSFTStock' dat verwijst naar een externe webservice voor het ophalen van de huidige waarde van Microsoft stock kunt definiëren.</p><p>Klik op elk gewenst moment '&MSFTStock;' wordt weergegeven in het document, wordt deze automatisch vervangen door de huidige aandelenkoersen. Deze functionaliteit kan echter worden misbruikt voor het maken van denial-of-service (DoS) voorwaarden. Een aanvaller kan meerdere entiteiten voor het maken van een uitbreiding van de exponentiële XML bomb die al het beschikbare geheugen op het systeem verbruikt nesten. </p><p>U kunt ook hij een oneindige hoeveelheid gegevens kunt maken voor een externe verwijzing die back-streams of die de thread gewoon loopt vast. Als gevolg hiervan moeten alle teams interne en/of externe XML-entiteit resolutie uitschakelen volledig als de toepassing niet worden gebruikt of de hoeveelheid geheugen en het tijdstip waarop de toepassing voor het omzetten van de entiteit gebruiken kan als deze functionaliteit absoluut noodzakelijk is handmatig te beperken. Als de omzetting van de entiteit niet door uw toepassing vereist is, klikt u vervolgens het uitschakelen. </p>|

### <a name="example"></a>Voorbeeld
Voor .NET Framework-code, kunt u de volgende methoden gebruiken:

```C#
XmlTextReader reader = new XmlTextReader(stream);
reader.ProhibitDtd = true;

XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);

// for .NET 4
XmlReaderSettings settings = new XmlReaderSettings();
settings.DtdProcessing = DtdProcessing.Prohibit;
XmlReader reader = XmlReader.Create(stream, settings);
```
Houd er rekening mee dat de standaardwaarde van `ProhibitDtd` in `XmlReaderSettings` is ingesteld op true, maar in `XmlTextReader` ONWAAR is. Als u XmlReaderSettings gebruikt, u hoeft niet in te stellen ProhibitDtd op ' True ' expliciet, maar het wordt aanbevolen voor veiligheid verjaardagen doen. Let ook op dat de klasse XmlDocument entiteit resolutie standaard toestaat. 

### <a name="example"></a>Voorbeeld
U kunt de omzetting van de entiteit voor XmlDocuments uitschakelen met de `XmlDocument.Load(XmlReader)` van de overbelasting van de Load-methode en de juiste eigenschappen instellen in het argument XmlReader voor het uitschakelen van de resolutie, zoals geïllustreerd in de volgende code: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);
XmlDocument doc = new XmlDocument();
doc.Load(reader);
```

### <a name="example"></a>Voorbeeld
Als het uitschakelen van de omzetting van de entiteit is niet mogelijk voor uw toepassing, moet u de eigenschap XmlReaderSettings.MaxCharactersFromEntities ingesteld op een redelijke waarde volgens de behoeften van uw toepassing. Dit beperkt de gevolgen van mogelijke uitbreiding van de exponentiële DoS-aanvallen. De volgende code geeft een voorbeeld van deze benadering: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
XmlReader reader = XmlReader.Create(stream, settings);
```

### <a name="example"></a>Voorbeeld
Als u wilt oplossen inline entiteiten, maar dit niet nodig om op te lossen externe entiteiten, stel de eigenschap XmlReaderSettings.XmlResolver op null. Bijvoorbeeld: 

```C#
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
settings.XmlResolver = null;
XmlReader reader = XmlReader.Create(stream, settings);
```
Houd er rekening mee dat in MSXML6, ProhibitDTD is standaard ingesteld op true (uitschakelen DTD-verwerking). Apple iOS-OS x-code, er zijn twee XML-parsers kunt u: NSXMLParser en libXML2. 

## <a id="app-verification"></a>Toepassingen die gebruikmaken van http.sys uitvoeren URL standaardisatie verificatie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Een toepassing die gebruikmaakt van http.sys moet als volgt:</p><ul><li>Beperkt de lengte van de URL niet meer dan 16.384 tekens (ASCII of Unicode). Dit is het absoluut maximale URL-lengte op basis van de standaardinstelling voor Internet Information Services (IIS) 6. Websites moeten streven voor korter is dan deze indien mogelijk</li><li>De standaard .NET Framework bestand i/o-klassen (zoals FileStream) gebruiken, zoals deze van de-regels in de .NET-FX profiteren</li><li>Expliciet samen een toestaan-lijst met bekende bestandsnamen</li><li>Expliciet afwijzen bekende bestandstypen die u niet voor de UrlScan geweigerd: exe, bat-, cmd, com, htw, ida, idq, htr, idc, shtm-[l], stm, printer, ini, pol, dat bestanden</li><li>Catch de volgende uitzonderingen:<ul><li>System.ArgumentException (voor apparaatnamen)</li><li>System.NotSupportedException (voor gegevensstromen)</li><li>System.IO.FileNotFoundException (voor ongeldige escape-teken bestandsnamen)</li><li>System.IO.DirectoryNotFoundException (voor ongeldige escape-teken dir-opdrachten)</li></ul></li><li>*Geen* naar Win32-bestand I/O APIs roepen. Op een ongeldige URL probleemloos retourneert een 400-fout voor de gebruiker en de echte fout vastgelegd.</li></ul>|

## <a id="controls-users"></a>Zorg passende controles zijn geïmplementeerd tijdens het accepteren van bestanden van gebruikers

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Bestand uploaden onbeperkte](https://www.owasp.org/index.php/Unrestricted_File_Upload), [bestand handtekening tabel](http://www.garykessler.net/library/file_sigs.html) |
| **Stappen** | <p>Geüploade bestanden vertegenwoordigen een aanzienlijke risico voor toepassingen.</p><p>De eerste stap bij veel aanvallen is om op te halen van bepaalde code wordt het systeem om te worden aangevallen. De aanval alleen moet zoeken naar een manier om de code uitgevoerd. Met uploaden van een bestand, kunt u de aanvaller de eerste stap bereiken. De gevolgen van het uploaden van een bestand kunnen variëren, met inbegrip van volledige systeem overname, een overbelaste bestandssysteem of de database, aanvallen back-end-systemen en eenvoudige defacement doorsturen.</p><p>Dit is afhankelijk van de toepassing heeft met het geüploade bestand en met name wordt bewaard. Server side validatie van bestandsuploads ontbreekt. Volgende beveiligingsmechanismen moet worden geïmplementeerd voor functionaliteit bestand laden:</p><ul><li>Extensie bestandscontrole (een geldige reeks toegestane bestandstype moet worden geaccepteerd)</li><li>Limiet voor de maximale grootte</li><li>Bestand mag niet worden geüpload naar webroot; de locatie moet een map op niet-systeemstation</li><li>Naamgevingsconventie moet worden gevolgd, zodanig dat de naam van het geüploade bestand sommige aanvraaggrootte, om te voorkomen dat bestand overschreven</li><li>Bestanden moeten worden gecontroleerd voor anti-virus voordat het schrijven naar de schijf</li><li>Zorg ervoor dat de bestandsnaam en andere metagegevens (zoals een bestandspad) worden gevalideerd voor schadelijke tekens</li><li>Indeling van bestandshandtekening moet worden gecontroleerd om te voorkomen dat een gebruiker een masqueraded bestand uploaden (bijvoorbeeld een exe-bestand uploaden door extensie te wijzigen naar txt)</li></ul>| 

### <a name="example"></a>Voorbeeld
Raadpleeg de klasse hieronder voor meer informatie voor het laatste punt met betrekking tot de validatie van handtekening bestand: 

```C#
        private static Dictionary<string, List<byte[]>> fileSignature = new Dictionary<string, List<byte[]>>
                    {
                    { ".DOC", new List<byte[]> { new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 } } },
                    { ".DOCX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".PDF", new List<byte[]> { new byte[] { 0x25, 0x50, 0x44, 0x46 } } },
                    { ".ZIP", new List<byte[]> 
                                            {
                                              new byte[] { 0x50, 0x4B, 0x03, 0x04 },
                                              new byte[] { 0x50, 0x4B, 0x4C, 0x49, 0x54, 0x55 },
                                              new byte[] { 0x50, 0x4B, 0x53, 0x70, 0x58 },
                                              new byte[] { 0x50, 0x4B, 0x05, 0x06 },
                                              new byte[] { 0x50, 0x4B, 0x07, 0x08 },
                                              new byte[] { 0x57, 0x69, 0x6E, 0x5A, 0x69, 0x70 }
                                                }
                                            },
                    { ".PNG", new List<byte[]> { new byte[] { 0x89, 0x50, 0x4E, 0x47, 0x0D, 0x0A, 0x1A, 0x0A } } },
                    { ".JPG", new List<byte[]>
                                    {
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE1 },
                                              new byte[] { 0xFF, 0xD8, 0xFF, 0xE8 }
                                    }
                                    },
                    { ".JPEG", new List<byte[]>
                                        { 
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
                                            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 }
                                        }
                                        },
                    { ".XLS", new List<byte[]>
                                            {
                                              new byte[] { 0xD0, 0xCF, 0x11, 0xE0, 0xA1, 0xB1, 0x1A, 0xE1 },
                                              new byte[] { 0x09, 0x08, 0x10, 0x00, 0x00, 0x06, 0x05, 0x00 },
                                              new byte[] { 0xFD, 0xFF, 0xFF, 0xFF }
                                            }
                                            },
                    { ".XLSX", new List<byte[]> { new byte[] { 0x50, 0x4B, 0x03, 0x04 } } },
                    { ".GIF", new List<byte[]> { new byte[] { 0x47, 0x49, 0x46, 0x38 } } }
                };

        public static bool IsValidFileExtension(string fileName, byte[] fileData, byte[] allowedChars)
        {
            if (string.IsNullOrEmpty(fileName) || fileData == null || fileData.Length == 0)
            {
                return false;
            }

            bool flag = false;
            string ext = Path.GetExtension(fileName);
            if (string.IsNullOrEmpty(ext))
            {
                return false;
            }

            ext = ext.ToUpperInvariant();

            if (ext.Equals(".TXT") || ext.Equals(".CSV") || ext.Equals(".PRN"))
            {
                foreach (byte b in fileData)
                {
                    if (b > 0x7F)
                    {
                        if (allowedChars != null)
                        {
                            if (!allowedChars.Contains(b))
                            {
                                return false;
                            }
                        }
                        else
                        {
                            return false;
                        }
                    }
                }

                return true;
            }

            if (!fileSignature.ContainsKey(ext))
            {
                return true;
            }

            List<byte[]> sig = fileSignature[ext];
            foreach (byte[] b in sig)
            {
                var curFileSig = new byte[b.Length];
                Array.Copy(fileData, curFileSig, b.Length);
                if (curFileSig.SequenceEqual(b))
                {
                    flag = true;
                    break;
                }
            }

            return flag;
        }
```

## <a id="typesafe"></a>Zorg ervoor dat type veilig parameters in webtoepassing worden gebruikt voor toegang tot gegevens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Als u de parameterverzameling, SQL beschouwen is de invoer als een letterlijke waarde in plaats als uitvoerbare code. De verzameling Parameters kan worden gebruikt voor het type en de lengte beperkingen toepassen op de invoergegevens. Waarden die buiten het bereik activeren een uitzondering. Als de type-safe SQL-parameters worden niet gebruikt, is het mogelijk dat aanvallers kunnen uitvoeren die zijn ingesloten in de ongefilterde invoer-injectieaanvallen.</p><p>Veilige typeparameters tijdens het construeren van de SQL-query's gebruiken om te voorkomen dat mogelijk SQL-injectieaanvallen die zich met ongefilterde invoer voordoen kunnen. U kunt veilig typeparameters met opgeslagen procedures en dynamische SQL-instructies. Parameters worden behandeld als letterlijke waarden door de database en niet als uitvoerbare code. Parameters zijn ook gecontroleerd op type en de lengte.</p>|

### <a name="example"></a>Voorbeeld 
De volgende code toont hoe u veilige typeparameters met de SqlParameterCollection bij het aanroepen van een opgeslagen procedure. 

```C#
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
De invoerwaarde mag niet langer zijn dan 11 tekens in het vorige codevoorbeeld. Als de gegevens niet met het type of de lengte die zijn gedefinieerd door de parameter overeen, genereert de klasse SqlParameter een uitzondering. 

## <a id="binding-mvc"></a>Afzonderlijke model binding klassen gebruiken of bindingsfilter weer om te voorkomen dat MVC massaopslag toewijzing beveiligingslek

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Metagegevenskenmerken](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.metadatatypeattribute), [openbare sleutel beveiligingslek en Beveiligingsrisicobeperking](https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation), [volledige handleiding voor massaopslag toewijzing in ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2012/03/11/complete-guide-to-mass-assignment-in-asp-net-mvc.aspx), [aan de slag met EF met MVC](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) |
| **Stappen** | <ul><li>**Wanneer moet ik letten te veel boeken beveiligingslekken? -** Te veel boeken beveiligingsproblemen kan optreden als een willekeurige plaats u modelklassen binding uit de invoer van de gebruiker. Frameworks zoals MVC kunnen gebruikersgegevens in aangepaste .NET-klassen, met inbegrip van gewone oude CLR-objecten (POCOs) vertegenwoordigen. MVC wordt automatisch gevuld deze modelklassen met gegevens van de aanvraag bieden een handige weergave voor het omgaan met invoer van gebruiker. Wanneer deze klassen eigenschappen die niet door de gebruiker moeten worden ingesteld bevatten, kan de toepassing kwetsbaar voor over-postingaanvallen, waardoor Gebruikersbeheer van gegevens die de toepassing nooit bedoeld zijn. Net als modelbinding MVC, database toegang tot technologieën zoals object relationele mappers zoals Entity Framework vaak ook ondersteuning POCO-objecten gebruiken om aan te duiden databasegegevens. Deze gegevens modelklassen bieden de dezelfde gebruiksgemak omgaan met gegevens uit een database, zoals MVC in omgaan met invoer van gebruiker. Omdat zowel MVC als de database ondersteuning soortgelijke modellen, zoals POCO-objecten, lijkt het eenvoudig om opnieuw de dezelfde klassen voor beide doeleinden te gebruiken. Deze procedure niet behouden scheiding van problemen en is een algemene gebied waar onbedoelde eigenschappen worden blootgesteld aan modelbinding, het inschakelen van te veel boeken aanvallen.</li><li>**Waarom mag niet ik mijn modelklassen ongefilterde database als parameters op mijn acties MVC gebruiken? -** Omdat MVC modelbinding in deze klasse wordt gebonden. Zelfs als de gegevens niet wordt weergegeven in de weergave, een kwaadwillende gebruiker kan een HTTP-aanvraag verzenden met deze gegevens worden opgenomen en MVC wordt graag binden omdat de actie wordt gemeld dat database klasse de vorm van gegevens die deze voor invoer van gebruiker accepteren moet.</li><li>**Waarom moet ik de vorm gebruikt voor het modelbinding interesseren? -** Met behulp van ASP.NET MVC modelbinding met makkelijk modellen beschrijft een toepassing op over-postingaanvallen. Te veel boeken aanvallers wijzigen toepassingsgegevens afgezien van wat de ontwikkelaar is bedoeld, zoals het overschrijven van de prijs van een item of de beveiligingsbevoegdheden voor een account kan worden ingeschakeld. Toepassingen moeten specifieke acties binding modellen (of specifieke toegestane filter eigenschappenlijsten) om een expliciete contract voor welke niet-vertrouwde invoer om toe te staan via de modelbinding gebruiken.</li><li>**Afzonderlijke binding modellen NET dupliceren code heeft? -** Nee, is een kwestie van scheiding van problemen. Als u de databasemodellen in actiemethoden hergebruikt, wordt u een eigenschap (of een onderliggende eigenschap) gezegd in deze klasse kan worden ingesteld door de gebruiker in een HTTP-aanvraag. Als dat niet naar wens MVC te doen, moet u een filterlijst of een afzonderlijke klasse vorm MVC weergeven welke gegevens kunnen afkomstig zijn van gebruikersinvoer in plaats daarvan.</li><li>**Als ik afzonderlijke binding modellen voor invoer van de gebruiker hebt, heb ik mijn kenmerken voor de aantekening gegevens dubbele? -** Niet noodzakelijkerwijs. MetadataTypeAttribute kunt u op de modelklasse database koppelen aan de metagegevens voor een model binding-klasse. NET Houd er rekening mee dat het type waarnaar wordt verwezen door de MetadataTypeAttribute moet een subset van het verwijzende type (mogelijk minder eigenschappen, maar niet meer).</li><li>**Verplaatsen van gegevens heen en weer tussen de gebruiker invoer modellen en databasemodellen is omslachtig. Kan ik alleen kopiëren via alle eigenschappen met reflectie? -** Ja. De enige eigenschappen die worden weergegeven in de binding-modellen zijn die u hebt vastgesteld veilig voor invoer van gebruiker. Er is geen reden beveiliging waarmee wordt voorkomen dat alle eigenschappen die gemeenschappelijk bestaan tussen deze twee modellen worden overschreven met behulp van reflectie.</li><li>**Hoe zit [binden (uitsluiten = "¦ â €")]. Kan ik die in plaats van afzonderlijke binding modellen gebruiken? -** Deze methode wordt niet aanbevolen. Met behulp van [binden (uitsluiten = "¦ â €")] betekent dat een nieuwe eigenschap bindbare standaard. Wanneer een nieuwe eigenschap wordt toegevoegd, er is een extra stap te onthouden dingen om veilig te houden, plaats van het ontwerp zijn standaard veilig. Afhankelijk van de ontwikkelaar is het riskant deze lijst controleren telkens wanneer een eigenschap wordt toegevoegd.</li><li>**Is [binden (inclusief = "¦ â €")] handig voor het bewerken? -** Nee. [Binden (inclusief = "¦ â €")] is alleen geschikt voor INSERT-stijl-bewerkingen (toevoegen van nieuwe gegevens). Gebruik een andere methode, zoals afzonderlijke binding modellen hebben of een expliciete lijst van toegestane eigenschappen doorgegeven aan UpdateModel of TryUpdateModel voor bewerkingen van UPDATE-stijl (herziening van bestaande gegevens). Toevoegen van een [binden (opnemen = "â €¦")] kenmerk voor een bewerking betekent dat MVC wordt een objectexemplaar maken en alleen de vermelde eigenschappen instellen als u alle andere gebruikers op hun standaardwaarden. Wanneer de gegevens worden bewaard, wordt de bestaande entiteit, de standaardwaarden opnieuw instellen van de waarden voor de weggelaten eigenschappen volledig vervangen. Bijvoorbeeld, als IsAdmin is weggelaten uit een [binden (inclusief = 'â €¦')] kenmerk voor een bewerking, een gebruiker waarvan de naam is bewerkt via deze actie zouden worden opnieuw ingesteld op IsAdmin = false (elke gebruiker bewerkte verliezen status van de administrator). Als u dat bepaalde eigenschappen van updates wilt, gebruikt u een van de andere methoden hierboven. Houd er rekening mee dat sommige versies van MVC-tooling Controllerklassen met genereren [binden (inclusief = 'â €¦')] acties bewerken en impliceren dat een eigenschap te verwijderen uit de lijst u te veel boeken aanvallen voorkomt. Echter zoals hierboven wordt beschreven, die benadering niet naar behoren werkt en in plaats daarvan alle gegevens in de eigenschappen van weggelaten wordt ingesteld op hun standaardwaarden.</li><li>**Voor de bewerkingen maken, zijn er aanvullende opmerkingen met [binden (opnemen = "¦ â €")] in plaats van afzonderlijke binding modellen? -** Ja. Deze methode werkt eerst niet voor scenario's bewerken, vereisen van twee afzonderlijke methoden voor alle te veel boeken zwakke plekken beperkende onderhouden. Tweede, afzonderlijk binding modellen Dwing scheiding van zorgen tussen de vorm gebruikt voor invoer van de gebruiker en de vorm die is gebruikt voor persistentie, af iets [binden (inclusief = "â €¦")] wordt niet. Derde, houd er rekening mee dat [binden (inclusief = 'â €¦')] zijn alleen geschikt voor eigenschappen op het hoogste niveau. u kunt alleen gedeelten van de onderliggende eigenschappen (zoals 'Details.Name') kan niet toestaan in het kenmerk. Tot slot en mogelijk belangrijker is, met behulp van [binden (opnemen = "¦ â €")] voegt een extra stap die moet worden opgeslagen telkens wanneer u de klasse wordt gebruikt voor het modelbinding. Als een nieuwe actiemethode wordt gekoppeld aan de gegevensklasse rechtstreeks en vergeet om op te nemen een [binden (opnemen = 'â €¦')]-kenmerk, het kan zijn kwetsbaar voor over-postingaanvallen, zodat de [binden (opnemen = "â €¦")] benadering standaard wat minder veilig is. Als u [binden (inclusief = 'â €¦')], altijd om te onthouden te geven telkens wanneer de gegevensklassen van uw worden weergegeven als actie-methodeparameters behandelen.</li><li>**Voor de bewerkingen maken, hoe staat te stellen de [binden (opnemen = "¦ â €")]-kenmerk op de modelklasse zelf? Voorkomt dat niet op deze manier hoeft te onthouden plaatsen van het kenmerk op elke actiemethode? -** Deze benadering in sommige gevallen werkt. Met behulp van [binden (opnemen = 'â €¦')] van het modeltype zelf (in plaats van op Actieparameters in met behulp van deze klasse), hoeft te onthouden om op te nemen voorkomt dat de [binden (opnemen = 'â €¦')] kenmerk voor elke methode in te grijpen. Met het kenmerk effectief rechtstreeks op de klasse, maakt een afzonderlijke surface area van deze klasse voor model binding doeleinden. Deze methode kan echter slechts voor één model binding vorm per modelklasse. Deze methode werkt niet als één actiemethode moet toestaan modelbinding van een veld (bijvoorbeeld een alleen-lezen beheerder actie die gebruikersrollen updates) en andere acties nodig om te voorkomen dat de modelbinding van dit veld. Elke klasse kan slechts één vorm van binding; hebben. Als u verschillende acties moeten een ander model binding vormen, moeten ze deze afzonderlijke vormen met behulp van een afzonderlijke binding modelklassen of afzonderlijke [binden (opnemen = "¦ â €")] kenmerken voor het actiemethoden.</li><li>**Wat zijn het binden van modellen? Zijn ze hetzelfde als modellen weergeven -** Dit twee verwante concepten zijn. De term binding model naar een model verwijst klasse die wordt gebruikt in een actie is parameterlijst (de vorm van MVC-modelbinding doorgegeven aan de actiemethode). Het model van de weergave term verwijst naar een modelklasse van een actiemethode doorgegeven aan een weergave. Met behulp van een weergave-specifieke model is een algemene benadering voor het doorgeven van gegevens van een actiemethode aan een weergave. Vaak worden deze vorm is ook geschikt voor modelbinding en het model van de weergave term kan worden gebruikt om te verwijzen van hetzelfde model op beide plaatsen worden gebruikt. Een nauwkeurige, is deze procedure wordt gesproken over de binding van modellen, gericht op de vorm die is doorgegeven aan de actie is wat belangrijk is voor massaopslag toewijzing doeleinden.</li></ul>| 

## <a id="rendering"></a>Niet-vertrouwde webuitvoer vóór rendering coderen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, Web Forms, MVC5, MVC6 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Om te voorkomen dat in ASP.NET Cross-site scripting](http://msdn.microsoft.com/library/ms998274.aspx), [Cross-site Scripting](http://cwe.mitre.org/data/definitions/79.html), [XSS (Cross-Site Scripting) preventie cheats blad](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet) |
| **Stappen** | Cross-site scripting (vaak afgekort als XSS) is een aanvalsvector voor online services of een toepassing/onderdeel is dat invoer via het web verbruikt. XSS beveiligingsproblemen kunnen een aanvaller script uitvoeren op de computer een andere gebruiker via een kwetsbaar webtoepassing. Schadelijke scripts kunnen worden gebruikt voor het stelen van cookies en anders kunnen knoeien met de machine van een slachtoffer via JavaScript. XSS wordt voorkomen door het valideren van invoer van gebruiker, zodat is opgemaakt en codering voordat deze wordt weergegeven in een webpagina. Validatie voor invoer en uitvoercodering kunnen worden gedaan met behulp van Web-bibliotheek voor beveiliging. Voor beheerde code (C\#, VB.net, enz.), gebruikt u een of meer coderingsmethoden uit de bibliotheek Web beveiliging (Anti-XSS), afhankelijk van de context waarin de gebruikersinvoer opgehaald gemanifesteerd nodig:| 

### <a name="example"></a>Voorbeeld

```C#
* Encoder.HtmlEncode 
* Encoder.HtmlAttributeEncode 
* Encoder.JavaScriptEncode 
* Encoder.UrlEncode
* Encoder.VisualBasicScriptEncode 
* Encoder.XmlEncode 
* Encoder.XmlAttributeEncode 
* Encoder.CssEncode 
* Encoder.LdapEncode 
```

## <a id="typemodel"></a>Validatie voor invoer- en filteren op alle tekenreekstype modeleigenschappen uitvoeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, MVC5, MVC6 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Toe te voegen validatie](http://www.asp.net/mvc/overview/getting-started/introduction/adding-validation), [modelgegevens valideren in een MVC-toepassing](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [richtsnoer voor uw ASP.NET MVC-toepassingen](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Stappen** | <p>De invoerparameters moeten worden gevalideerd voordat ze worden gebruikt in de toepassing om ervoor te zorgen dat de toepassing zijn beveiligd tegen schadelijke gebruikersinvoer. Valideren van de invoerwaarden met reguliere expressie validaties aan serverzijde met een strategie voor een lijst met geaccepteerde validatie. Gebruikersinvoer unsanitized / parameters doorgegeven aan de methoden kunnen leiden tot code injectie beveiligingsproblemen.</p><p>Voor webtoepassingen bevatten toegangspunten ook formuliervelden, QueryStrings, cookies, HTTP-headers en web serviceparameters.</p><p>De volgende validatie voor invoer controles moeten worden uitgevoerd bij modelbinding:</p><ul><li>De eigenschappen van het model moeten worden gemarkeerd met RegularExpression aantekening, voor het accepteren van toegestane aantal tekens en maximaal toegestane lengte</li><li>De controllermethoden ModelState geldigheid moeten worden uitgevoerd</li></ul>|

## <a id="richtext"></a>Opschoning moet worden toegepast op formuliervelden die tekens, bijvoorbeeld, RTF-editor accepteren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Onveilige invoer coderen](https://msdn.microsoft.com/library/ff647397.aspx#paght000003_step3), [HTML Sanitizer](https://github.com/mganss/HtmlSanitizer) |
| **Stappen** | <p>Identificeer alle statische markup-labels die u wilt gebruiken. Vaak is het beperken van de opmaak op veilige HTML-elementen, zoals `<b>` (vet weergegeven) en `<i>` (cursief).</p><p>Voordat u de gegevens, HTML en coderen schrijft deze. Dit maakt schadelijke scripts veilig door waardoor moet worden verwerkt als tekst niet als uitvoerbare code.</p><ol><li>Validatie van de ASP.NET-aanvraag uitschakelen door het toevoegen van de ValidateRequest = 'false' kenmerk met de @-pagina-instructie</li><li>De tekenreeksinvoer met de methode HtmlEncode coderen</li><li>Gebruik een StringBuilder en roep de methode vervangen om selectief verwijderen de codering voor de HTML-elementen die u wilt toestaan</li></ol><p>De pagina in de verwijzingen schakelt ASP.NET aanvraag gevalideerd door in te stellen `ValidateRequest="false"`. Het HTML-coderen van de invoer, waardoor selectief de `<b>` en `<i>` ook een .NET-bibliotheek voor HTML-opschoning kan ook worden gebruikt.</p><p>HtmlSanitizer is een .NET-bibliotheek voor het reinigen HTML-fragmenten en documenten van constructies die tot XSS-aanvallen leiden kunnen. AngleSharp wordt gebruikt om te parseren, bewerken en HTML en CSS weergeven. HtmlSanitizer kan worden geïnstalleerd als een NuGet-pakket en de gebruikersinvoer kan worden doorgegeven via de relevante HTML of CSS-opschoning methoden, indien van toepassing op de server. Houd er rekening mee dat opschoning als een beveiligingscontrole alleen als een laatste optie moet worden beschouwd.</p><p>Validatie voor invoer en uitvoercodering worden beschouwd als de besturingselementen voor betere beveiliging.</p> |

## <a id="inbuilt-encode"></a>Geen DOM elementen toewijzen aan put waarvoor geen ingebouwde codering

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | Veel javascript-functies doen niet codering standaard. Als u niet-vertrouwde invoer voor de DOM-elementen via dergelijke functies toewijst, kan leiden tot cross-site-script (XSS) uitvoeringen.| 

### <a name="example"></a>Voorbeeld
Hieronder vindt u onveilige voorbeelden: 

```
document.getElementByID("div1").innerHtml = value;
$("#userName").html(res.Name);
return $('<div/>').html(value)
$('body').append(resHTML);   
```
Gebruik geen `innerHtml`; gebruik in plaats daarvan `innerText`. Op deze manier in plaats van `$("#elm").html()`, gebruiken`$("#elm").text()` 

## <a id="redirect-safe"></a>Valideer alle omleidingen in de toepassing worden gesloten of veilig gedaan

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Het Framework van de autorisatie OAuth 2.0 - Open Redirectors](http://tools.ietf.org/html/rfc6749#section-10.15) |
| **Stappen** | <p>Ontwerp van de toepassing waarvoor omleiding naar een door de gebruiker opgegeven locatie moet de doelen mogelijke omleiding naar een vooraf gedefinieerde "veilig" lijst met sites of domeinen beperken. Alle omleidingen in de toepassing moeten worden gesloten/veilig.</p><p>Om dit te doen:</p><ul><li>Alle omleidingen identificeren</li><li>Een juiste Risicobeperking voor het omleiden van elke implementeren. Juiste oplossingen omvatten omleiding goedgekeurde IP-adressen of gebruiker bevestigen. Als een website of de service met een beveiligingslek open omleiding identiteitsproviders OAuth-Facebook/OpenID gebruikt, kan een aanvaller stelen van een gebruiker aanmeldingstokens en die gebruiker imiteren. Dit risico is wanneer met behulp van OAuth, die wordt beschreven in RFC 6749 'De OAuth 2.0 autorisatie Framework', sectie 10.15 'Open leidt' op dezelfde manier, referenties van gebruikers kunnen worden aangetast door spear phishing-aanvallen met open omleidingen</li></ul>|

## <a id="string-method"></a>Validatie voor invoer op alle tekenreeks typeparameters geaccepteerd door de methoden van de domeincontroller implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, MVC5, MVC6 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Valideren van modelgegevens in een MVC-toepassing](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [richtsnoer voor uw ASP.NET MVC-toepassingen](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Stappen** | Methoden die primitief gegevenstype en geen modellen als argument accepteert, moet met een reguliere expressie voor validatie voor invoer worden gedaan. Hier moet Regex.IsMatch worden gebruikt met een geldige regex-patroon. Als de invoer komt niet overeen met de opgegeven reguliere expressie, besturingselement moet niet verder en een passende waarschuwing met betrekking tot mislukte validatie moet worden weergegeven.| 

## <a id="dos-expression"></a>Bovengrens-out voor reguliere expressie om te voorkomen van DoS vanwege ongeldige reguliere expressies verwerken instellen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, Web Forms, MVC5, MVC6  |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [De eigenschap DefaultRegexMatchTimeout](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.defaultregexmatchtimeout.aspx) |
| **Stappen** | Om ervoor te zorgen denial of service-aanvallen op onjuist ingesteld gemaakte reguliere expressies, waardoor een groot aantal backtracing als de time-out voor de algemene standaardwaarde. Als de verwerkingstijd langer dan de bovengrens van de gedefinieerde duurt, zou dit een time-out-uitzondering genereert. Als er niets is geconfigureerd, zou de time-out van oneindig zijn.| 

### <a name="example"></a>Voorbeeld
De volgende configuratie genereert bijvoorbeeld een RegexMatchTimeoutException als de verwerking langer dan 5 seconden duurt: 

```C#
<httpRuntime targetFramework="4.5" defaultRegexMatchTimeout="00:00:05" />
```

## <a id="html-razor"></a>Vermijd het gebruik van Html.Raw in Razor weergaven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| Stap | ASP.Net-webpagina's (Razor) uitvoeren van automatische HTML-codering. Alle tekenreeksen die worden afgedrukt door ingesloten code nuggets (@ blokken) zijn automatisch HTML-codering. Echter, wanneer `HtmlHelper.Raw` methode wordt aangeroepen, resulteert dit in de opmaak die is geen HTML-codering. Als `Html.Raw()` Help-methode wordt gebruikt, wordt de automatische codering bescherming die Razor biedt omzeild.|

### <a name="example"></a>Voorbeeld
Hieronder volgt een onbeveiligde voorbeeld: 

```C#
<div class="form-group">
            @Html.Raw(Model.AccountConfirmText)
        </div>
        <div class="form-group">
            @Html.Raw(Model.PaymentConfirmText)
        </div>
</div>
```
Gebruik geen `Html.Raw()` tenzij u nodig hebt om opmaak weer te geven. Deze methode voert geen uitvoer impliciet codering. Gebruik andere helpers ASP.NET bijv.`@Html.DisplayFor()` 

## <a id="stored-proc"></a>Gebruik geen dynamische query's in de opgeslagen procedures

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Een SQL-injectieaanvallen gebruikmaakt van kwetsbaarheden in validatie voor invoer willekeurige opdrachten kunt uitvoeren in de database. Dit kan optreden wanneer de toepassing gebruikmaakt van invoer om dynamische SQL-instructies voor toegang tot de database samen te stellen. Het kan ook optreden als opgeslagen procedures die zijn doorgegeven tekenreeksen met onbewerkte gebruikersinvoer wordt gebruikt in uw code. Met behulp van de SQL-injectieaanvallen, uitvoeren de aanvaller willekeurige opdrachten in de database. De parameters van alle SQL-instructies (met inbegrip van de SQL-instructies in de opgeslagen procedures) moeten worden gebruikt. SQL-instructies met parameters accepteert tekens met een speciale betekenis naar SQL (zoals enkel aanhalingsteken) zonder problemen omdat ze sterk getypeerd zijn. |

### <a name="example"></a>Voorbeeld
Hieronder volgt een voorbeeld van onbeveiligde dynamische opgeslagen Procedure: 

```C#
CREATE PROCEDURE [dbo].[uspGetProductsByCriteria]
(
  @productName nvarchar(200) = NULL,
  @startPrice float = NULL,
  @endPrice float = NULL
)
AS
 BEGIN
  DECLARE @sql nvarchar(max)
  SELECT @sql = ' SELECT ProductID, ProductName, Description, UnitPrice, ImagePath' +
       ' FROM dbo.Products WHERE 1 = 1 '
       PRINT @sql
  IF @productName IS NOT NULL
     SELECT @sql = @sql + ' AND ProductName LIKE ''%' + @productName + '%'''
  IF @startPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice > ''' + CONVERT(VARCHAR(10),@startPrice) + ''''
  IF @endPrice IS NOT NULL
     SELECT @sql = @sql + ' AND UnitPrice < ''' + CONVERT(VARCHAR(10),@endPrice) + ''''

  PRINT @sql
  EXEC(@sql)
 END
```

### <a name="example"></a>Voorbeeld
Hieronder vindt u de dezelfde opgeslagen procedure veilig geïmplementeerd: 
```C#
CREATE PROCEDURE [dbo].[uspGetProductsByCriteriaSecure]
(
             @productName nvarchar(200) = NULL,
             @startPrice float = NULL,
             @endPrice float = NULL
)
AS
       BEGIN
             SELECT ProductID, ProductName, Description, UnitPrice, ImagePath
             FROM dbo.Products where
             (@productName IS NULL or ProductName like '%'+ @productName +'%')
             AND
             (@startPrice IS NULL or UnitPrice > @startPrice)
             AND
             (@endPrice IS NULL or UnitPrice < @endPrice)         
       END
```

## <a id="validation-api"></a>Zorg ervoor dat de validatie is uitgevoerd voor Web-API-methoden

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Modelvalidatie van het in ASP.NET-Web-API](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Stappen** | Wanneer een client verzendt de gegevens naar een web-API, is het verplichte de om gegevens te valideren voordat de verwerking. Gebruik gegevens aantekeningen op modellen validatieregels instellen voor de eigenschappen van het model voor ASP.NET Web-API's die modellen als invoer accepteren.|

### <a name="example"></a>Voorbeeld
De volgende code toont dezelfde: 

```C#
using System.ComponentModel.DataAnnotations;

namespace MyApi.Models
{
    public class Product
    {
        public int Id { get; set; }
        [Required]
        [RegularExpression(@"^[a-zA-Z0-9]*$", ErrorMessage="Only alphanumeric characters are allowed.")]
        public string Name { get; set; }
        public decimal Price { get; set; }
        [Range(0, 999)]
        public double Weight { get; set; }
    }
}
```

### <a name="example"></a>Voorbeeld
In de actiemethode van de API-controllers heeft de geldigheid van het model moet expliciet worden gecontroleerd zoals hieronder wordt weergegeven: 

```C#
namespace MyApi.Controllers
{
    public class ProductsController : ApiController
    {
        public HttpResponseMessage Post(Product product)
        {
            if (ModelState.IsValid)
            {
                // Do something with the product (not shown).

                return new HttpResponseMessage(HttpStatusCode.OK);
            }
            else
            {
                return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
            }
        }
    }
}
```

## <a id="string-api"></a>Validatie voor invoer op alle tekenreeks typeparameters geaccepteerd door de methoden van de Web-API implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene MVC 5, 6 MVC |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [Valideren van modelgegevens in een MVC-toepassing](http://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [richtsnoer voor uw ASP.NET MVC-toepassingen](http://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Stappen** | Methoden die primitief gegevenstype en geen modellen als argument accepteert, moet met een reguliere expressie voor validatie voor invoer worden gedaan. Hier moet Regex.IsMatch worden gebruikt met een geldige regex-patroon. Als de invoer komt niet overeen met de opgegeven reguliere expressie, besturingselement moet niet verder en een passende waarschuwing met betrekking tot mislukte validatie moet worden weergegeven.|

## <a id="typesafe-api"></a>Zorg ervoor dat type veilig parameters in Web-API worden gebruikt voor toegang tot gegevens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | N.v.t.  |
| **Stappen** | <p>Als u de parameterverzameling, SQL beschouwen is de invoer als een letterlijke waarde in plaats als uitvoerbare code. De verzameling Parameters kan worden gebruikt voor het type en de lengte beperkingen toepassen op de invoergegevens. Waarden die buiten het bereik activeren een uitzondering. Als de type-safe SQL-parameters worden niet gebruikt, is het mogelijk dat aanvallers kunnen uitvoeren die zijn ingesloten in de ongefilterde invoer-injectieaanvallen.</p><p>Veilige typeparameters tijdens het construeren van de SQL-query's gebruiken om te voorkomen dat mogelijk SQL-injectieaanvallen die zich met ongefilterde invoer voordoen kunnen. U kunt veilig typeparameters met opgeslagen procedures en dynamische SQL-instructies. Parameters worden behandeld als letterlijke waarden door de database en niet als uitvoerbare code. Parameters zijn ook gecontroleerd op type en de lengte.</p>|

### <a name="example"></a>Voorbeeld
De volgende code toont hoe u veilige typeparameters met de SqlParameterCollection bij het aanroepen van een opgeslagen procedure. 

```C#
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter(LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
De invoerwaarde mag niet langer zijn dan 11 tekens in het vorige codevoorbeeld. Als de gegevens niet met het type of de lengte die zijn gedefinieerd door de parameter overeen, genereert de klasse SqlParameter een uitzondering. 

## <a id="sql-docdb"></a>SQL-query's constructorreeks mag gebruiken voor Cosmos-DB

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Documentdb | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [SQL-Parameterisering in DocumentDB aangekondigd](https://azure.microsoft.com/blog/announcing-sql-parameterization-in-documentdb/) |
| **Stappen** | Hoewel DocumentDB biedt alleen ondersteuning voor alleen-lezen-query's, is SQL-injectie nog steeds mogelijk als de query's worden gemaakt met cookievalidatie met invoer van gebruiker. Het is mogelijk dat een gebruiker toegang te krijgen tot gegevens die ze mag niet in dezelfde verzameling openen door schadelijke SQL-query's. Gebruik SQL-query's met parameters als de query's zijn samengesteld op basis van gebruikersinvoer. |

## <a id="schema-binding"></a>Validatie van de WCF-invoer via schemabinding

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff647820.aspx) |
| **Stappen** | <p>Gebrek aan validatie leidt tot een ander type injectieaanvallen.</p><p>Berichtverificatie vertegenwoordigt één regel verdediging in de bescherming van uw WCF-toepassing. Met deze methode kunt u berichten schema's met WCF servicebewerkingen beschermen tegen een aanval door een schadelijke client valideren. Alle berichten ontvangen door de client naar de client te beschermen tegen een aanval door een kwaadwillende service valideren. Berichtverificatie maakt het mogelijk om berichten te valideren wanneer bewerkingen verbruiken bericht contracten of gegevenscontracten, kunnen niet worden uitgevoerd met behulp van de validatie van parameters. Berichtverificatie kunt u validatielogica binnen schema's, waardoor meer flexibiliteit en het ontwikkeling maken. Schema's kunnen opnieuw worden gebruikt door verschillende toepassingen binnen de organisatie, standaarden voor weergave van gegevens maken. Bovendien kunt berichtverificatie u bewerkingen beveiligen wanneer ze complexe gegevenstypen met betrekking tot contracten die zakelijke logica verbruiken.</p><p>Bericht als validatie wilt uitvoeren, moet u eerst een schema dat de bewerkingen van uw service en de gegevenstypen die wordt gebruikt door deze bewerkingen vertegenwoordigt bouwen. Vervolgens maakt u een .NET-klasse die een aangepaste clientinstellingen bericht inspector en aangepaste dispatcher bericht inspector voor het valideren van de berichten verzonden/ontvangen van de service implementeert. Vervolgens maakt implementeren u een aangepaste eindpuntgedrag zodat berichtverificatie op zowel de client als de service. Ten slotte implementeren u een aangepaste configuratie-element van de klasse die kunt u zichtbaar maken van de uitgebreide aangepaste eindpuntgedrag in het configuratiebestand van de service of de client'</p>|

## <a id="parameters"></a>Validatie van de WCF - invoer via Parameter Inspectors

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N.v.t.  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff647875.aspx) |
| **Stappen** | <p>Invoer en gegevensvalidatie vertegenwoordigt een belangrijke line-of verdediging in de bescherming van uw WCF-toepassing. U moet alle parameters die worden weergegeven in de bewerkingen van de WCF-service naar de service te beveiligen tegen een aanval door een schadelijke client valideren. Als u daarentegen, moet u ook alle retourwaarden ontvangen door de client naar de client te beschermen tegen een aanval door een kwaadwillende service valideren</p><p>WCF biedt andere uitbreidbaarheidspunten waarmee u kunt het gedrag van de runtime WCF aanpassen door het maken van aangepaste extensies. Bericht inspecteurs en Parameter Inspectors zijn twee uitbreidbaarheid mechanismen meer controle krijgen over de gegevens die tussen een client en een service gebruikt. U moet de parameter inspectors gebruiken voor validatie voor invoer en inspectors bericht alleen gebruiken wanneer u moet het volledige bericht dat naar en vanuit een service te controleren.</p><p>Om uit te voeren validatie voor invoer, u een .NET-klasse bouwen en implementeren van een aangepaste parameter inspector om parameters voor bewerkingen in uw service te valideren. Vervolgens implementeert u een aangepaste eindpuntgedrag zodat validatie op zowel de client als de service. Ten slotte wilt u een aangepaste configuratie-element op de klasse die kunt u zichtbaar maken van de uitgebreide aangepaste eindpuntgedrag in het configuratiebestand van de service of de client implementeren</p>|

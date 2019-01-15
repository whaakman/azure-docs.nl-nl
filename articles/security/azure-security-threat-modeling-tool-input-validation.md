---
title: Voer de validatie - Microsoft Threat Modeling Tool - Azure | Microsoft Docs
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
ms.openlocfilehash: dd2914c675d3bca32ca8951ffca1b04e23786400
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266901"
---
# <a name="security-frame-input-validation--mitigations"></a>Beveiliging-Frame: Invoervalidatie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Web-App** | <ul><li>[XSLT-scripts voor alle transformaties met behulp van niet-vertrouwde opmaakmodellen uitschakelen](#disable-xslt)</li><li>[Zorg ervoor dat elke pagina waarop gebruikers te beheren zijn inhoud bevatten, kan worden gebruikt buiten het automatische MIME-controle](#out-sniffing)</li><li>[Beveiliging van in- of uitschakelen van XML-entiteit resolutie](#xml-resolution)</li><li>[URL-standaardisatie verificatie uitvoeren met toepassingen die gebruikmaken van http.sys](#app-verification)</li><li>[Zorg ervoor dat de juiste besturingselementen wordt toegepast bij het accepteren van bestanden van gebruikers](#controls-users)</li><li>[Zorg ervoor dat type veilig parameters in Web-App worden gebruikt voor toegang tot gegevens](#typesafe)</li><li>[Afzonderlijke model-binding klassen gebruiken of bindingsfilter weer te geven om te voorkomen dat MVC massale toewijzing beveiligingsproblemen](#binding-mvc)</li><li>[Niet-vertrouwde webuitvoer vóór rendering coderen](#rendering)</li><li>[Invoervalidatie en filteren op alle tekenreekstype modeleigenschappen uitvoeren](#typemodel)</li><li>[Opschonen moet worden toegepast op formuliervelden die alle tekens, bijvoorbeeld, RTF-editor accepteren](#richtext)</li><li>[Wijs geen DOM-elementen naar sinks waarvoor geen ingebouwde codering](#inbuilt-encode)</li><li>[Controleer of alle omleidingen in de toepassing worden gesloten of veilig gedaan](#redirect-safe)</li><li>[Invoervalidatie implementeren op alle tekenreeksparameters van het type geaccepteerd door controllermethoden](#string-method)</li><li>[Bovenlimiet van time-out voor reguliere expressie om te voorkomen van DoS vanwege ongeldige reguliere expressies verwerking instellen](#dos-expression)</li><li>[Vermijd het gebruik van Html.Raw in Razor-weergaven](#html-razor)</li></ul> | 
| **Database** | <ul><li>[Gebruik geen dynamische query's in opgeslagen procedures](#stored-proc)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat de validatie is uitgevoerd op een Web-API-methoden](#validation-api)</li><li>[Invoervalidatie implementeren op alle tekenreeksparameters van het type geaccepteerd door de Web-API-methoden](#string-api)</li><li>[Zorg ervoor dat type veilig parameters in Web-API worden gebruikt voor toegang tot gegevens](#typesafe-api)</li></ul> | 
| **Azure Document DB** | <ul><li>[Geparameteriseerde SQL-query's gebruiken voor Azure Cosmos DB](#sql-docdb)</li></ul> | 
| **WCF** | <ul><li>[Validatie van WCF-invoer via schemabinding](#schema-binding)</li><li>[Validatie van WCF - invoer via Parameter Inspectors](#parameters)</li></ul> |

## <a id="disable-xslt"></a>XSLT-scripts voor alle transformaties met behulp van niet-vertrouwde opmaakmodellen uitschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [XSLT Security](https://msdn.microsoft.com/library/ms763800(v=vs.85).aspx), [XsltSettings.EnableScript Property](https://msdn.microsoft.com/library/system.xml.xsl.xsltsettings.enablescript.aspx) |
| **Stappen** | XSLT biedt ondersteuning voor scripts in opmaakmodellen met behulp van de `<msxml:script>` element. Dit kan aangepaste functies worden gebruikt in een XSLT-transformatie. Het script wordt uitgevoerd in de context van het proces voor het uitvoeren van de transformatie. XSLT-script moet worden uitgeschakeld wanneer u zich in een niet-vertrouwde omgeving om te voorkomen dat de uitvoering van niet-vertrouwde code. *Als u met behulp van .NET:* XSLT-scripting is standaard uitgeschakeld. echter, moet u ervoor zorgen dat deze is niet expliciet ingeschakeld via de `XsltSettings.EnableScript` eigenschap.|

### <a name="example"></a>Voorbeeld 

```csharp
XsltSettings settings = new XsltSettings();
settings.EnableScript = true; // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Voorbeeld
Als u van MSXML 6.0 gebruikmaakt, is XSLT-scripting standaard uitgeschakeld. echter, moet u ervoor zorgen dat deze is niet expliciet ingeschakeld via de object-eigenschap van XML-DOM AllowXsltScript. 

```csharp
doc.setProperty("AllowXsltScript", true); // WRONG: THIS SHOULD BE SET TO false
```

### <a name="example"></a>Voorbeeld
Als u van MSXML 5 gebruikmaakt of hieronder, XSLT-scripting is standaard ingeschakeld en u expliciet moet uitschakelen. De XML-DOM-eigenschap van het object AllowXsltScript ingesteld op false. 

```csharp
doc.setProperty("AllowXsltScript", false); // CORRECT. Setting to false disables XSLT scripting.
```

## <a id="out-sniffing"></a>Zorg ervoor dat elke pagina waarop gebruikers te beheren zijn inhoud bevatten, kan worden gebruikt buiten het automatische MIME-controle

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [IE8 Security deel V - Uitgebreide bescherming](https://blogs.msdn.com/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx)  |
| **Stappen** | <p>Voor elke pagina die gebruikers te beheren zijn inhoud kan bevatten, moet u de HTTP-Header `X-Content-Type-Options:nosniff`. U kunt de vereiste header pagina voor deze pagina's die mogelijk door de gebruiker instelbare inhoud bevatten die ofwel instellen om te voldoen aan deze eis wordt voldaan, of kunt u dit wereldwijd instellen voor alle pagina's in de toepassing.</p><p>Elk type bestand geleverd vanaf een webserver heeft een bijbehorende [MIME-type](http://en.wikipedia.org/wiki/Mime_type) (ook wel een *inhoudstype*) met de beschrijving van de aard van de inhoud (dat wil zeggen, afbeeldingen, tekst, toepassing, enzovoort.)</p><p>De header X-inhoud-Type-opties is een HTTP-header die ontwikkelaars de mogelijkheid om op te geven biedt dat de inhoud ervan niet MIME-sniff worden moet. Deze header is ontworpen voor het beperken van aanvallen MIME bekijken. Ondersteuning voor deze header is in Internet Explorer 8 (IE8) toegevoegd</p><p>Alleen gebruikers van Internet Explorer 8 (IE8) profiteren van de X-inhoud-Type-opties. Vorige versies van Internet Explorer op dit moment niet aansluiten bij de header X-inhoud-Type-opties</p><p>Internet Explorer 8 (en hoger) zijn de enige grote browsers voor het implementeren van een functie opt-out MIME bekijken. Als andere belangrijke browsers (Firefox, Safari, Chrome) soortgelijke functies implementeren, wordt deze aanbeveling worden bijgewerkt met de syntaxis voor deze browsers ook</p>|

### <a name="example"></a>Voorbeeld
Om in te schakelen de vereiste koptekst wereldwijd voor alle pagina's in de toepassing, kunt u het volgende doen: 

* De header toevoegen in het web.config-bestand als de toepassing wordt gehost door Internet Information Services (IIS) 7 

```
<system.webServer> 
  <httpProtocol> 
    <customHeaders> 
      <add name=""X-Content-Type-Options"" value=""nosniff""/>
    </customHeaders>
  </httpProtocol>
</system.webServer> 
```

* De header via de wereldwijde toepassing toevoegen\_BeginRequest 

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

* U kunt de vereiste koptekst alleen voor bepaalde pagina's inschakelen door toe te voegen aan afzonderlijke antwoorden: 

```
this.Response.Headers[""X-Content-Type-Options""] = ""nosniff""; 
``` 

## <a id="xml-resolution"></a>Beveiliging van in- of uitschakelen van XML-entiteit resolutie

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [XML-entiteit uitbreiding](http://capec.mitre.org/data/definitions/197.html), [XML Denial of Service-aanvallen en verdediging](https://msdn.microsoft.com/magazine/ee335713.aspx), [MSXML beveiligingsoverzicht](https://msdn.microsoft.com/library/ms754611(v=VS.85).aspx), [aanbevolen procedures voor het beveiligen van MSXML Code](https://msdn.microsoft.com/library/ms759188(VS.85).aspx), [ Naslaginformatie over NSXMLParserDelegate Protocol](http://developer.apple.com/library/ios/#documentation/cocoa/reference/NSXMLParserDelegate_Protocol/Reference/Reference.html), [het omzetten van externe verwijzingen](https://msdn.microsoft.com/library/5fcwybb2.aspx) |
| **Stappen**| <p>Hoewel het niet veel wordt gebruikt, is er een functie van XML waarmee de XML-parser macro entiteiten uitbreiden met waarden die zijn gedefinieerd in het document zelf of uit externe bronnen. Kan het document een entiteit 'Bedrijfsnaam' met de waarde 'Microsoft', bijvoorbeeld zo definiëren dat telkens wanneer de tekst '&companyname;' wordt weergegeven in het document, wordt deze automatisch vervangen door de tekst van Microsoft. Of het document een entiteit 'MSFTStock' die verwijst naar een externe webservice om op te halen van de huidige waarde van Microsoft voorraad kunt definiëren.</p><p>Klik op elk gewenst moment "&MSFTStock;' wordt weergegeven in het document, wordt deze automatisch vervangen door de huidige aandelenkoers. Deze functionaliteit kan echter worden misbruikt voor het maken van denial-of-service (DoS) voorwaarden. Een aanvaller kan meerdere entiteiten voor het maken van een exponentiële groei XML bomb dat alle beschikbare geheugen op het systeem verbruikt nesten. </p><p>U kunt ook hij een onbeperkte hoeveelheid gegevens kunt maken voor een externe verwijzing die back gegevensstromen of die gewoon de thread is vastgelopen. Als gevolg hiervan moeten alle teams interne en/of externe XML-entiteit resolutie uitschakelen volledig als de toepassing niet gebruiken of de hoeveelheid geheugen en het tijdstip waarop de toepassing voor het omzetten van de entiteit gebruiken kunt als deze functionaliteit is handmatig te beperken dit absoluut noodzakelijk is. Als de resolutie van de entiteit niet voor de toepassing vereist is, klikt u vervolgens uitschakelen. </p>|

### <a name="example"></a>Voorbeeld
Voor de code van .NET Framework, kunt u de volgende methoden gebruiken:

```csharp
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
Houd er rekening mee dat de standaardwaarde van `ProhibitDtd` in `XmlReaderSettings` is ingesteld op true, maar `XmlTextReader` ONWAAR is. Als u XmlReaderSettings gebruikt, hoeft u niet ProhibitDtd op waar het expliciet instellen, maar het wordt aanbevolen voor veiligheid geeft een beeld te doen. Let ook op dat de klasse XmlDocument entiteit resolutie standaard toestaat. 

### <a name="example"></a>Voorbeeld
Als u wilt uitschakelen entiteit resolutie voor XmlDocuments, gebruiken de `XmlDocument.Load(XmlReader)` overbelasting van de Load-methode en de juiste eigenschappen instellen in het argument XmlReader voor het uitschakelen van resolutie, zoals wordt geïllustreerd in de volgende code: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = true;
XmlReader reader = XmlReader.Create(stream, settings);
XmlDocument doc = new XmlDocument();
doc.Load(reader);
```

### <a name="example"></a>Voorbeeld
Als het uitschakelen van de resolutie van de entiteit is niet mogelijk is voor uw toepassing, moet u de eigenschap XmlReaderSettings.MaxCharactersFromEntities instellen op een redelijke waarde op basis van de behoeften van uw toepassing. Dit beperkt de gevolgen van mogelijke exponentiële groei DoS-aanvallen. De volgende code geeft een voorbeeld van deze benadering: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
XmlReader reader = XmlReader.Create(stream, settings);
```

### <a name="example"></a>Voorbeeld
Als u wilt oplossen inline-entiteiten, maar wel niet nodig om op te lossen externe entiteiten de eigenschap XmlReaderSettings.XmlResolver instellen op null. Bijvoorbeeld: 

```csharp
XmlReaderSettings settings = new XmlReaderSettings();
settings.ProhibitDtd = false;
settings.MaxCharactersFromEntities = 1000;
settings.XmlResolver = null;
XmlReader reader = XmlReader.Create(stream, settings);
```
Houd er rekening mee dat in MSXML6, ProhibitDTD is standaard ingesteld op true (uitschakelen DTD processing). Er zijn twee XML-parsers die kunt u voor Apple iOS-OS x-code: NSXMLParser en libXML2. 

## <a id="app-verification"></a>URL-standaardisatie verificatie uitvoeren met toepassingen die gebruikmaken van http.sys

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Elke toepassing die gebruikmaakt van http.sys moet als volgt:</p><ul><li>De URL-lengte niet meer dan 16.384 tekens (ASCII of Unicode) beperken. Dit is de absolute maximale URL-lengte op basis van de standaardinstelling voor Internet Information Services (IIS) 6. Websites moeten streven naar een korter is dan deze indien mogelijk</li><li>De standaard .NET Framework-bestand i/o-klassen (zoals FileStream) gebruiken, zoals deze wordt benut de-regels in de .NET-FX</li><li>Expliciet bouw een acceptatielijst van bekende bestandsnamen</li><li>Expliciet weigeren bekende bestandstypen die u moet geen URL-scans weigert fungeren: exe, bat-, cmd, com, htw, ida, idq, htr, idc, shtm-[l], stm, printer, ini, pol, dat bestanden</li><li>Bekijk de volgende uitzonderingen:<ul><li>System.ArgumentException (voor apparaatnamen)</li><li>System.NotSupportedException (voor gegevensstromen)</li><li>System.IO.FileNotFoundException (gebruikt voor bestandsnamen voor ongeldige met escape-teken)</li><li>System.IO.DirectoryNotFoundException (voor ongeldige met escape-teken dir-opdrachten)</li></ul></li><li>*Geen* naar Win32-bestand I/O APIs belichten. Op een ongeldige URL probleemloos retourneert een 400-fout voor de gebruiker en de echte fout vastgelegd.</li></ul>|

## <a id="controls-users"></a>Zorg ervoor dat de juiste besturingselementen wordt toegepast bij het accepteren van bestanden van gebruikers

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Bestand uploaden onbeperkte](https://www.owasp.org/index.php/Unrestricted_File_Upload), [bestand handtekening-tabel](http://www.garykessler.net/library/file_sigs.html) |
| **Stappen** | <p>Geüploade bestanden vertegenwoordigen een aanzienlijke risico's voor toepassingen.</p><p>De eerste stap in veel aanvallen is om op te halen van bepaalde code wordt het systeem om te worden aangevallen. Vervolgens worden de aanval alleen moet worden gezocht een manier om de code die wordt uitgevoerd. Met behulp van een bestand is geüpload, kunt de aanvaller de eerste stap uitvoeren. De gevolgen van het uploaden van het onbeperkte bestand kunnen variëren, met inbegrip van volledige systeem overnemen, een overbelaste bestandssysteem of het doorsturen van aanvallen op back-endsystemen en beschadiging van eenvoudige wilden staat-database.</p><p>Dit is afhankelijk van wat de toepassing doet met het geüploade bestand en vooral waar deze zijn opgeslagen. Server side validatie van het uploaden van bestanden ontbreekt. Besturingselementen voor de beveiliging te volgen moet worden geïmplementeerd voor de functionaliteit van bestand uploaden:</p><ul><li>Extensie bestandscontrole (alleen een geldige set toegestane bestandstype moet worden geaccepteerd)</li><li>Maximale grootte</li><li>Bestand mag niet worden geüpload naar webroot; de locatie moet een map op niet-systeemstation</li><li>Naamgevingsconventie moet worden gevolgd, zodat de naam van het geüploade bestand hebben enkele aanvraaggrootte, om te voorkomen dat bestand overschreven</li><li>Bestanden moeten worden gecontroleerd op antivirusprogramma's voor het schrijven naar de schijf</li><li>Zorg ervoor dat de bestandsnaam en andere metagegevens (bijv, bestandspad) zijn gevalideerd voor schadelijke tekens</li><li>Handtekening van de indeling bestand moet worden gecontroleerd om te voorkomen dat een gebruiker een masqueraded bestand uploaden (bijvoorbeeld een exe-bestand uploaden door te wijzigen van de extensie in txt)</li></ul>| 

### <a name="example"></a>Voorbeeld
Voor het laatste punt met betrekking tot de validatie van bestandsindeling handtekening, raadpleegt u de klasse hieronder voor meer informatie: 

```csharp
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

## <a id="typesafe"></a>Zorg ervoor dat type veilig parameters in Web-App worden gebruikt voor toegang tot gegevens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Als u de Parameters-collectie, SQL behandeld wordt de invoer is als een letterlijke waarde in plaats daarvan vervolgens als uitvoerbare code. De verzameling Parameters kan worden gebruikt om af te dwingen type en de lengte van de beperkingen met betrekking tot invoergegevens. Waarden buiten het bereik activeren een uitzondering. Als het type kluis SQL-parameters worden niet gebruikt, kunnen kwaadwillende personen om uit te voeren die zijn opgenomen in de ongefilterde invoer-injectieaanvallen mogelijk.</p><p>Veilige parameters van het type bij het maken van SQL-query's gebruiken om te voorkomen dat mogelijk SQL-injectieaanvallen die met ongefilterde invoer optreden kunnen. U kunt parameters van het type veilig met opgeslagen procedures en dynamische SQL-instructies. Parameters worden behandeld als letterlijke waarden door de database en niet als uitvoerbare code. Parameters zijn ook gecontroleerd op type en de lengte.</p>|

### <a name="example"></a>Voorbeeld 
De volgende code toont hoe u veilige parameters van het type met de SqlParameterCollection gebruikt bij het aanroepen van een opgeslagen procedure. 

```csharp
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter("LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
De invoerwaarde kan niet langer zijn dan 11 tekens in het vorige codevoorbeeld. Als de gegevens niet met het type of de lengte die zijn gedefinieerd door de parameter overeenkomt, de klasse SqlParameter een uitzondering genereert. 

## <a id="binding-mvc"></a>Afzonderlijke model-binding klassen gebruiken of bindingsfilter weer te geven om te voorkomen dat MVC massale toewijzing beveiligingsproblemen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [De metagegevens van kenmerken](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.metadatatypeattribute), [openbare sleutel door een beveiligingslek en Beveiligingsrisicobeperking](https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation), [volledige handleiding voor de toewijzing voor massaopslag in ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2012/03/11/complete-guide-to-mass-assignment-in-asp-net-mvc.aspx), [aan de slag met EF met MVC](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost) |
| **Stappen** | <ul><li>**Wanneer moet ik er voor het plaatsen van te veel beveiligingsproblemen? -** Beveiligingslekken te veel plaatsen waar u modelklassen van de invoer van de gebruiker binden kan optreden. Frameworks als MVC kunnen gebruikersgegevens in aangepaste .NET-klassen, met inbegrip van gewone oude CLR-objecten (POCOs) vertegenwoordigen. MVC vult automatisch deze modelklassen met gegevens van de aanvraag bieden een handige weergave voor het omgaan met invoer van de gebruiker. Wanneer deze klassen eigenschappen die niet door de gebruiker moeten worden ingesteld bevatten, kan de toepassing kwetsbaar voor over-postingaanvallen, waarmee de gebruiker controle over gegevens die de toepassing nooit bedoeld zijn. Als de binding van MVC-model, database toegang tot technologieën zoals van objecten/relationele mappers zoals Entity Framework vaak ook ondersteuning POCO-objecten gebruiken om weer te geven gegevens uit een database. Deze gegevens modelklassen bieden dezelfde gemakkelijk in het omgaan met gegevens uit een database als MVC in omgaan met invoer van de gebruiker. Omdat zowel de database als MVC soortgelijke modellen, zoals POCO-objecten ondersteunt, lijkt het eenvoudig om opnieuw de dezelfde klassen voor beide doeleinden te gebruiken. Met deze procedure niet behouden scheiding van taken en is een algemene gebied waar onbedoelde eigenschappen worden blootgesteld aan model binding sprake van redundante boeken aanvallen inschakelen.</li><li>**Waarom mag niet ik mijn modelklassen ongefilterde database als parameters op mijn MVC-acties gebruiken? -** Omdat MVC model binding binden in deze branche. Zelfs als de gegevens niet in de weergave weergegeven worden, een kwaadwillende gebruiker een HTTP-aanvraag met deze gegevens die zijn opgenomen verzenden kan en MVC graag moet u het binden omdat uw actie zegt dat databaseklasse de vorm van de gegevens die er toestemming moet geven voor de invoer van de gebruiker is.</li><li>**Waarom moet ik de vorm gebruikt voor het model binding belangrijkst? -** Met behulp van ASP.NET MVC model binding met ruim modellen wordt aangegeven dat een toepassing over-postingaanvallen. Te veel boeken kan inschakelen aanvallers om toepassingsgegevens dan wat de ontwikkelaar bedoeld, zoals het overschrijven van de prijs voor een item of de bevoegdheden voor een account te wijzigen. Toepassingen moeten specifieke acties binding modellen (of specifieke toegestane filter eigenschappenlijsten) voor een expliciete contract voor welke niet-vertrouwde invoer om toe te staan via de model-binding gebruikt.</li><li>**Is, hebt u alleen het dupliceren van code afzonderlijke binding-modellen? -** Nee, er is een kwestie van scheiding van taken. Als u databasemodellen in de actiemethoden gebruiken, zijn u een eigenschap (of een onderliggende eigenschap) dat in deze klasse kan worden ingesteld door de gebruiker in een HTTP-aanvraag. Als dat niet wat u wilt dat MVC te doen, moet u een lijst met filters of de vorm van een afzonderlijke klasse MVC weergeven welke gegevens kunnen afkomstig zijn van gebruikersinvoer in plaats daarvan.</li><li>**Als ik afzonderlijke binding modellen voor invoer van de gebruiker, heb ik alle kenmerken van mijn gegevens aantekening dupliceren? -** Niet per se. MetadataTypeAttribute kunt u op de modelklasse van de database te koppelen aan de metagegevens op een modelklasse van de binding. Houd er alleen rekening mee dat het type waarnaar wordt verwezen door de MetadataTypeAttribute een subset van het verwijzende type moet (dit kan hebben minder eigenschappen, maar niet meer).</li><li>**Verplaatsen van gegevens heen en weer tussen de gebruiker invoer modellen en databasemodellen is nogal omslachtig. Kan ik gewoon kopiëren via alle eigenschappen met behulp van reflectie? -** Ja. De enige eigenschappen die worden weergegeven in de binding-modellen zijn die u hebt vastgesteld dat moet worden veilig voor invoer van de gebruiker. Er is geen reden beveiliging waarmee wordt voorkomen met behulp van reflectie dat moeten worden gekopieerd van alle eigenschappen die in algemene tussen deze twee modellen bestaat.</li><li>**Hoe zit [binden (uitsluiten = "(c) €¦")]. Kan ik die in plaats van afzonderlijke binding modellen gebruiken? -** Deze benadering wordt niet aanbevolen. Met behulp van [binden (uitsluiten = "(c) €¦")] betekent dat een nieuwe eigenschap binden standaard. Wanneer een nieuwe eigenschap wordt toegevoegd, er is een extra stap te onthouden dingen om veilig te houden, in plaats dat u hoeft het ontwerp worden standaard veilig. Afhankelijk van de ontwikkelaar is het riskante deze lijst te controleren telkens wanneer een eigenschap is toegevoegd.</li><li>**Is [binden (inclusief = "(c) €¦")] handig voor het bewerken bewerkingen? -** Nee. [Binden (inclusief = "(c) €¦")] is alleen geschikt voor INSERT-stijl-bewerkingen (toevoegen van nieuwe gegevens). Gebruik een andere benadering, zoals met afzonderlijke binding modellen of een expliciete lijst van toegestane eigenschappen doorgegeven aan UpdateModel of TryUpdateModel voor UPDATE-stijl-bewerkingen (herziening van bestaande gegevens). Toevoegen van een [binden (opnemen = "onderdelen ¦ €")]-kenmerk voor een bewerking betekent dat MVC wordt een exemplaar van het object te maken en instellen van alleen de vermelde eigenschappen, waardoor alle andere gebruikers op hun standaardwaarden. Wanneer de gegevens worden opgeslagen, wordt de bestaande entiteit, de standaardwaarden opnieuw instellen van de waarden voor elke weggelaten eigenschappen volledig vervangen. Bijvoorbeeld, als IsAdmin is weggelaten uit een [binden (inclusief = "onderdelen ¦ €")]-kenmerk voor een bewerking, elke gebruiker waarvan de naam is bewerkt via deze actie wordt ingesteld op IsAdmin = false (elke bewerkte gebruiker verliest status van de administrator). Als u wilt om te voorkomen dat updates op bepaalde eigenschappen, gebruikt u een van de andere methoden hierboven. Houd er rekening mee dat sommige versies van MVC-tooling Controllerklassen met genereren [binden (inclusief = "onderdelen ¦ €")] op acties te bewerken en impliceren dat een eigenschap uit de lijst wordt verwijderd, kunnen sprake van redundante boeken aanvallen. Echter, zoals hierboven beschreven, deze aanpak werkt niet zoals bedoeld en in plaats daarvan de gegevens in de eigenschappen van weggelaten wordt ingesteld op hun standaardwaarden.</li><li>**Voor bewerkingen voor maken, zijn er aanvullende opmerkingen met behulp van [binden (inclusief = "(c) €¦")] in plaats van afzonderlijke binding-modellen? -** Ja. Deze benadering werkt eerst niet voor bewerken-scenario's, waarvoor twee afzonderlijke methoden voor het oplossen van alle te veel boeken beveiligingsproblemen onderhouden. Tweede, afzonderlijk binding modellen afdwingen scheiding van taken tussen de vorm gebruikt voor invoer van de gebruiker en de vorm die is gebruikt voor persistentie, iets [binden (inclusief = "(c) €¦")] hoeven niet te worden. Derde, houd er rekening mee dat [binden (inclusief = "(c) €¦")] eigenschappen voor de op het hoogste niveau kunnen alleen worden verwerkt. u kan niet alleen de gedeelten van onderliggende eigenschappen (zoals 'Details.Name') in het kenmerk toestaan. Ten slotte, en mogelijk belangrijker nog, met behulp van [binden (inclusief = "(c) €¦")] voegt een extra stap die moet worden opgeslagen telkens wanneer u de klasse wordt gebruikt voor het model binding. Als een nieuwe actiemethode wordt gebonden aan de gegevensklasse rechtstreeks en vergeet om op te nemen een [binden (opnemen = "onderdelen ¦ €")] kenmerk, kan het zijn kwetsbaar voor over-postingaanvallen, zodat de [binden (inclusief = "onderdelen ¦ €")] benadering is iets minder veilig is standaard. Als u [binden (inclusief = "(c) €¦")], Let erop dat altijd om te onthouden te geven telkens wanneer de gegevensklassen van uw worden weergegeven als actie-methodeparameters.</li><li>**Voor bewerkingen voor maken, wat ongeveer plaatsen de [binden (inclusief = "(c) €¦")] kenmerk voor de modelklasse zelf? Voorkomt dat niet op deze manier hoeft te plaatsen van het kenmerk op elke actiemethode onthouden? -** Deze benadering werkt in sommige gevallen. Met behulp van [binden (opnemen = "(c) €¦")] het modeltype zelf (in plaats van met behulp van deze klasse actieparameters), voorkomt de noodzaak om te onthouden om op te nemen dat de [binden (opnemen = "(c) €¦")]-kenmerk voor elke actiemethode. Effectief gebruik van het kenmerk rechtstreeks op de klasse, maakt u een afzonderlijke surface area van deze klasse voor model binding doeleinden. Deze aanpak kan echter slechts voor één model binding vorm per modelklasse. Deze benadering werkt niet als één actiemethode nodig heeft om toe te staan model binding van een veld (bijvoorbeeld een alleen-lezen beheerder actie die updates gebruikersrollen) en andere acties nodig hebt om te voorkomen dat model binding van dit veld. Elke klasse kan slechts één model binding vorm; hebben. Als u verschillende acties moeten een ander model binding vormen, moeten ze deze afzonderlijke vormen met behulp van een afzonderlijke binding modelklassen of afzonderlijke [binden (inclusief = "(c) €¦")] kenmerken op de actiemethoden.</li><li>**Wat zijn modellen binding? Zijn ze hetzelfde als modellen weergeven? -** Dit twee verwante concepten zijn. Het model van de binding term verwijst naar een modelklasse die wordt gebruikt in de lijst met parameters van de actie (de vorm van MVC-model binding worden doorgegeven aan de actiemethode). Het model van de weergave term verwijst naar een modelklasse doorgegeven vanuit een actiemethode naar een weergave. Met behulp van een weergave-specifieke model is een veelgebruikte manier voor het doorgeven van gegevens uit een actiemethode naar een weergave. Vaak deze vorm is ook geschikt voor het model-binding en het model van de weergave term kan worden gebruikt om te verwijzen van hetzelfde model gebruikt op beide plaatsen. Om precies te zijn, deze procedure vertelt over de binding van modellen, ligt de focus op de vorm die is doorgegeven aan de actie is wat belangrijk is voor grootschalige toewijzing doeleinden.</li></ul>| 

## <a id="rendering"></a>Niet-vertrouwde webuitvoer vóór rendering coderen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Generic, Web Forms, MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Om te voorkomen dat Cross-site scripting in ASP.NET](https://msdn.microsoft.com/library/ms998274.aspx), [Cross-site Scripting](http://cwe.mitre.org/data/definitions/79.html), [XSS (Cross-Site Scripting) preventie Cheat Sheet](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet) |
| **Stappen** | Cross-site scripting (vaak afgekort als XSS) is een aanvalsvector voor online services of een toepassing/onderdeel is dat invoer van het web worden verbruikt. XSS beveiligingsproblemen kunnen een aanvaller om uit te voeren script op de computer een andere gebruiker via een kwetsbaar webtoepassing. Schadelijke scripts kunnen worden gebruikt om het stelen van cookies en anders kunnen knoeien met de computer van het slachtoffer via JavaScript. XSS is voorkomen door het valideren van invoer van de gebruiker en ervoor te zorgen dat deze goed is gevormd codering voordat deze wordt weergegeven in een webpagina. Validatie voor invoer en uitvoer encoding kunnen worden gedaan met behulp van Web-beveiliging-bibliotheek. Voor beheerde code (C\#, VB.net, enzovoort), gebruikt u een of meer coderingsmethoden van de beveiliging van Internet (Anti-XSS)-bibliotheek, afhankelijk van de context waarin de invoer van de gebruiker met deze eigenschap wordt overgenomen van toepassing:| 

### <a name="example"></a>Voorbeeld

```csharp
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

## <a id="typemodel"></a>Invoervalidatie en filteren op alle tekenreekstype modeleigenschappen uitvoeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Generic, MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Toe te voegen validatie](http://www.asp.net/mvc/overview/getting-started/introduction/adding-validation), [valideren van modelgegevens in een MVC-toepassing](https://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [richtsnoer voor uw ASP.NET MVC-toepassingen](https://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Stappen** | <p>Alle invoerparameters die moeten worden gevalideerd voordat ze worden gebruikt in de toepassing om ervoor te zorgen dat de toepassing waarin wordt beschermd tegen kwaadwillende gebruikersinvoer. De ingevoerde waarden met behulp van de reguliere expressie validaties aan serverzijde met een strategie voor een lijst met toegestane adressen validatie valideren. Gebruikersinvoer unsanitized / parameters doorgegeven aan de methoden kunnen leiden tot code beveiligingsproblemen met injectie.</p><p>Voor webtoepassingen bevatten toegangspunten ook velden in het formulier, queryreeksen waarmee, cookies, HTTP-headers en webserviceparameters.</p><p>De volgende Invoervalidatie controles moeten worden uitgevoerd na de binding model:</p><ul><li>De eigenschappen van het model moeten worden voorzien van aantekeningen met RegularExpression aantekening, voor het accepteren van toegestane tekens en maximaal toegestane lengte</li><li>De controllermethoden moeten ModelState geldigheid uitvoeren</li></ul>|

## <a id="richtext"></a>Opschonen moet worden toegepast op formuliervelden die alle tekens, bijvoorbeeld, RTF-editor accepteren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Codeer onveilige invoer](https://msdn.microsoft.com/library/ff647397.aspx#paght000003_step3), [HTML Sanitizer](https://github.com/mganss/HtmlSanitizer) |
| **Stappen** | <p>Identificeer alle statische opmaakcodes die u wilt gebruiken. Een algemene praktijk is het beperken van opmaak die voldoet aan de HTML-elementen, veilige, zoals `<b>` (vet) en `<i>` (cursief).</p><p>Voor het schrijven van gegevens, HTML-coderen deze. Dit maakt een schadelijke scripts veilig door waardoor moet worden verwerkt als tekst, niet als uitvoerbare code.</p><ol><li>ASP.NET-Aanvraagvalidatie uitschakelen door het toevoegen van de ValidateRequest = "false" kenmerk aan de \@ pagina-instructie</li><li>De tekenreeksinvoer met de methode HtmlEncode coderen</li><li>Gebruik een StringBuilder en aanroepen van de vervangen-methode voor het selectief verwijderen de codering van de HTML-elementen die u wilt toestaan</li></ol><p>De pagina in de validatie van referenties wordt uitgeschakeld ASP.NET aanvraag door in te stellen `ValidateRequest="false"`. Het HTML-coderen van de invoer en selectief kunnen de `<b>` en `<i>` ook een .NET-bibliotheek voor HTML-opschonen kan ook worden gebruikt.</p><p>HtmlSanitizer is een .NET-bibliotheek voor het opschonen van HTML-fragmenten en documenten van constructies die tot XSS-aanvallen leiden kunnen. AngleSharp wordt gebruikt om te parseren, bewerken en weergeven van HTML en CSS. HtmlSanitizer kan worden geïnstalleerd als een NuGet-pakket en de invoer van de gebruiker kan worden doorgegeven via relevante HTML of CSS opschonen methoden, zoals van toepassing op de server. Houd er rekening mee dat opschonen als een beveiligingscontrole alleen als een laatste optie moet worden beschouwd.</p><p>Validatie voor invoer en uitvoercodering worden beschouwd als de besturingselementen voor betere beveiliging.</p> |

## <a id="inbuilt-encode"></a>Wijs geen DOM-elementen naar sinks waarvoor geen ingebouwde codering

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | Veel javascript-functies doen niet codering standaard. Bij het toewijzen van niet-vertrouwde invoer voor DOM-elementen via dergelijke functies, kan resulteren in cross-site-script (XSS) uitvoeringen.| 

### <a name="example"></a>Voorbeeld
Hieronder vindt u onveilige voorbeelden: 

```
document.getElementByID("div1").innerHtml = value;
$("#userName").html(res.Name);
return $('<div/>').html(value)
$('body').append(resHTML);   
```
Gebruik geen `innerHtml`; gebruik in plaats daarvan `innerText`. Op dezelfde manier, in plaats van `$("#elm").html()`, gebruiken `$("#elm").text()` 

## <a id="redirect-safe"></a>Controleer of alle omleidingen in de toepassing worden gesloten of veilig gedaan

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Het Framework van de autorisatie OAuth 2.0 - Open Redirectors](http://tools.ietf.org/html/rfc6749#section-10.15) |
| **Stappen** | <p>Ontwerp van de toepassing waarvoor omleiding naar een door de gebruiker opgegeven locatie moet de doelen mogelijk omleiden naar een vooraf gedefinieerde "veilig" lijst van sites of domeinen beperken. Alle omleidingen in de toepassing moeten worden gesloten/safe.</p><p>Om dit te doen:</p><ul><li>Alle omleidingen identificeren</li><li>Implementeer een juiste Risicobeperking voor elke omleiden. Juiste oplossingen omvatten omleidings-whitelist of gebruiker bevestigen. Als een website of de service met een open omleiding beveiligingslek gebruikmaakt van OAuth-Facebook/OpenID id-providers, kan een aanvaller een aanmeldingstoken stelen en die gebruiker imiteren. Dit risico is wanneer met behulp van OAuth, die wordt beschreven in RFC 6749 'De OAuth 2.0 machtiging Framework', sectie 10.15 "openen wordt omgeleid' op dezelfde manier, referenties van gebruikers kunnen worden aangetast door spear phishing-aanvallen met behulp van open omleidingen</li></ul>|

## <a id="string-method"></a>Invoervalidatie implementeren op alle tekenreeksparameters van het type geaccepteerd door controllermethoden

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Generic, MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Valideren van modelgegevens in een MVC-toepassing](https://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [richtsnoer voor uw ASP.NET MVC-toepassingen](https://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Stappen** | Methoden die alleen primitieve gegevenstype en geen modellen als argument accepteren, moet Invoervalidatie met behulp van de reguliere expressie worden uitgevoerd. Hier moet Regex.IsMatch worden gebruikt met een geldige regex-patroon. Als de invoer komt niet overeen met de opgegeven reguliere expressie, besturingselement moet niet verder en een voldoende waarschuwing met betrekking tot mislukte validatie moet worden weergegeven.| 

## <a id="dos-expression"></a>Bovenlimiet van time-out voor reguliere expressie om te voorkomen van DoS vanwege ongeldige reguliere expressies verwerking instellen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Generic, Web Forms, MVC5, MVC6  |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [DefaultRegexMatchTimeout Property ](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.defaultregexmatchtimeout.aspx) |
| **Stappen** | Gemaakte reguliere expressies, die ertoe leiden veel backtracking dat, ingesteld om ervoor te zorgen denial of service-aanvallen op een ongeldige time-out voor de algemene standaardwaarde. Als de verwerkingstijd langer duurt dan de bovengrens van de gedefinieerde, zou dit een time-outuitzondering genereren. Als er niets is geconfigureerd, is de time-out is oneindig.| 

### <a name="example"></a>Voorbeeld
Bijvoorbeeld, genereert de volgende configuratie een RegexMatchTimeoutException als de verwerking langer dan 5 seconden duurt: 

```csharp
<httpRuntime targetFramework="4.5" defaultRegexMatchTimeout="00:00:05" />
```

## <a id="html-razor"></a>Vermijd het gebruik van Html.Raw in Razor-weergaven

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| Stap | ASP.Net-webpagina's (Razor) uitvoeren van automatische HTML-codering. Alle tekenreeksen die worden afgedrukt door ingesloten code nuggets (@ blokken) zijn automatisch HTML-codering. Echter, wanneer `HtmlHelper.Raw` methode wordt aangeroepen, resulteert dit in markup die geen HTML-codering. Als `Html.Raw()` Help-methode wordt gebruikt, wordt de automatische versleuteling beveiliging waarmee Razor omzeild.|

### <a name="example"></a>Voorbeeld
Hieronder volgt een onbeveiligde voorbeeld: 

```csharp
<div class="form-group">
            @Html.Raw(Model.AccountConfirmText)
        </div>
        <div class="form-group">
            @Html.Raw(Model.PaymentConfirmText)
        </div>
</div>
```
Gebruik geen `Html.Raw()` tenzij u nodig hebt om aantekeningen weer te geven. Deze methode voert geen uitvoer impliciet codering. Bijvoorbeeld, andere ASP.NET-hulpprogramma's gebruiken `@Html.DisplayFor()` 

## <a id="stored-proc"></a>Gebruik geen dynamische query's in opgeslagen procedures

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Een SQL-injectie-aanval wordt misbruik gemaakt zwakke plekken in de Invoervalidatie willekeurige opdrachten kunt uitvoeren in de database. Dit kan gebeuren wanneer de toepassing gebruikmaakt van invoer om te maken van dynamische SQL-instructies gebruikt voor toegang tot de database. Het kan ook optreden als de code wordt gebruikgemaakt van opgeslagen procedures die tekenreeksen met onbewerkte invoer worden doorgegeven. Met behulp van de SQL-injectieaanvallen verminderen, uitvoeren de aanvaller willekeurige opdrachten in de database. De parameters van alle SQL-instructies (met inbegrip van de SQL-instructies in opgeslagen procedures) moeten worden gebruikt. SQL-instructies met parameters accepteert tekens met een speciale betekenis naar SQL (zoals enkel aanhalingsteken) zonder problemen omdat ze sterk getypeerd zijn. |

### <a name="example"></a>Voorbeeld
Hieronder volgt een voorbeeld van onbeveiligde dynamische opgeslagen Procedure: 

```csharp
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
```csharp
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

## <a id="validation-api"></a>Zorg ervoor dat de validatie is uitgevoerd op een Web-API-methoden

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | MVC5, MVC6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Modelvalidatie van het in ASP.NET Web-API ](http://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Stappen** | Wanneer een client gegevens naar een web-API verzendt, is het is verplicht voor het valideren van de gegevens voordat u een verwerking. Gebruik gegevens aantekeningen op modellen validatieregels instellen voor de eigenschappen van het model voor ASP.NET Web-API's die modellen als invoer accepteert.|

### <a name="example"></a>Voorbeeld
De volgende code toont dezelfde: 

```csharp
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
In de actiemethode van de API-controllers heeft de geldigheid van het model moet expliciet worden gecontroleerd, zoals hieronder weergegeven: 

```csharp
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

## <a id="string-api"></a>Invoervalidatie implementeren op alle tekenreeksparameters van het type geaccepteerd door de Web-API-methoden

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Generic, MVC 5, MVC 6 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Valideren van modelgegevens in een MVC-toepassing](https://msdn.microsoft.com/library/dd410404(v=vs.90).aspx), [richtsnoer voor uw ASP.NET MVC-toepassingen](https://msdn.microsoft.com/magazine/dd942822.aspx) |
| **Stappen** | Methoden die alleen primitieve gegevenstype en geen modellen als argument accepteren, moet Invoervalidatie met behulp van de reguliere expressie worden uitgevoerd. Hier moet Regex.IsMatch worden gebruikt met een geldige regex-patroon. Als de invoer komt niet overeen met de opgegeven reguliere expressie, besturingselement moet niet verder en een voldoende waarschuwing met betrekking tot mislukte validatie moet worden weergegeven.|

## <a id="typesafe-api"></a>Zorg ervoor dat type veilig parameters in Web-API worden gebruikt voor toegang tot gegevens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | N/A  |
| **Stappen** | <p>Als u de Parameters-collectie, SQL behandeld wordt de invoer is als een letterlijke waarde in plaats daarvan vervolgens als uitvoerbare code. De verzameling Parameters kan worden gebruikt om af te dwingen type en de lengte van de beperkingen met betrekking tot invoergegevens. Waarden buiten het bereik activeren een uitzondering. Als het type kluis SQL-parameters worden niet gebruikt, kunnen kwaadwillende personen om uit te voeren die zijn opgenomen in de ongefilterde invoer-injectieaanvallen mogelijk.</p><p>Veilige parameters van het type bij het maken van SQL-query's gebruiken om te voorkomen dat mogelijk SQL-injectieaanvallen die met ongefilterde invoer optreden kunnen. U kunt parameters van het type veilig met opgeslagen procedures en dynamische SQL-instructies. Parameters worden behandeld als letterlijke waarden door de database en niet als uitvoerbare code. Parameters zijn ook gecontroleerd op type en de lengte.</p>|

### <a name="example"></a>Voorbeeld
De volgende code toont hoe u veilige parameters van het type met de SqlParameterCollection gebruikt bij het aanroepen van een opgeslagen procedure. 

```csharp
using System.Data;
using System.Data.SqlClient;

using (SqlConnection connection = new SqlConnection(connectionString))
{ 
DataSet userDataset = new DataSet(); 
SqlDataAdapter myCommand = new SqlDataAdapter("LoginStoredProcedure", connection); 
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure; 
myCommand.SelectCommand.Parameters.Add("@au_id", SqlDbType.VarChar, 11); 
myCommand.SelectCommand.Parameters["@au_id"].Value = SSN.Text; 
myCommand.Fill(userDataset);
}  
```
De invoerwaarde kan niet langer zijn dan 11 tekens in het vorige codevoorbeeld. Als de gegevens niet met het type of de lengte die zijn gedefinieerd door de parameter overeenkomt, de klasse SqlParameter een uitzondering genereert. 

## <a id="sql-docdb"></a>Geparameteriseerde SQL-query's gebruiken voor Cosmos DB

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Document DB | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemene |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [Aankondiging van de SQL-Parameterisering in Azure Cosmos DB](https://azure.microsoft.com/blog/announcing-sql-parameterization-in-documentdb/) |
| **Stappen** | Hoewel Azure Cosmos DB biedt alleen ondersteuning voor alleen-lezen query's, is SQL-injectie nog steeds mogelijk als query's zijn gebouwd door het samenvoegen met invoer van de gebruiker. Het is mogelijk dat een gebruiker toegang te krijgen tot gegevens die ze al dan niet mogen toegang binnen dezelfde verzameling tot worden door schadelijke SQL-query's. Gebruik geparameteriseerde SQL-query's als query's zijn gebouwd op basis van de invoer van de gebruiker. |

## <a id="schema-binding"></a>Validatie van WCF-invoer via schemabinding

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff647820.aspx) |
| **Stappen** | <p>Gebrek aan validatie leidt tot een ander type-injectieaanvallen.</p><p>Validatie van het bericht vertegenwoordigt een line-of defense in de beveiliging van uw WCF-toepassing. Met deze methode voert valideren u berichten schema's met WCF-service operations beschermen tegen aanvallen door een schadelijke client. Alle berichten ontvangen door de client naar de client beveiligen tegen aanvallen door een kwaadwillende service valideren. Validatie van het bericht maakt het mogelijk om berichten te valideren wanneer bewerkingen verbruiken bericht contracten en gegevenscontracten, die kunnen niet worden uitgevoerd met behulp van validatie van de parameter. Bericht validatie kunt u validatielogica binnen schema's, waardoor meer flexibiliteit en de ontwikkelingstijd te maken. Schema's kunnen worden hergebruikt binnen verschillende toepassingen binnen de organisatie, het maken van standaarden voor weergave van gegevens. Bovendien kunt bericht validatie u bewerkingen beveiligen wanneer ze complexe gegevenstypen met betrekking tot contracten voor bedrijfslogica verbruiken.</p><p>Bericht als validatie wilt uitvoeren, moet u eerst een schema dat staat voor de bewerkingen van uw service en de gegevenstypen die worden gebruikt door de bewerkingen maken. Vervolgens maakt u een .NET-klasse die de implementatie van een aangepaste clientinstellingen bericht inspector en aangepaste functie voor berichtverzending bericht inspector voor het valideren van de berichten verzonden/ontvangen van de service. Vervolgens maakt implementeren u een aangepast eindpuntgedrag om in te schakelen bericht validatie op zowel de client en de service. Ten slotte implementeren u een aangepaste configuratie-element op de klasse waarmee u het gedrag van de uitgebreide aangepast eindpunt in het configuratiebestand van de service of de client beschikbaar'</p>|

## <a id="parameters"></a>Validatie van WCF - invoer via Parameter Inspectors

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Van toepassing technologieën** | Algemeen, NET Framework 3 |
| **Kenmerken**              | N/A  |
| **Verwijzingen**              | [MSDN](https://msdn.microsoft.com/library/ff647875.aspx) |
| **Stappen** | <p>Invoer en gegevensvalidatie vertegenwoordigt één belangrijke line-of defense in de beveiliging van uw WCF-toepassing. U moet alle parameters die worden weergegeven in de bewerkingen van WCF-service naar de service te beveiligen tegen aanvallen door een schadelijke client valideren. Daarentegen, moet u ook alle retourwaarden ontvangen door de client naar de client beveiligen tegen aanvallen door een kwaadwillende service valideren</p><p>WCF biedt verschillende uitbreidbaarheidspunten waarmee u kunt de WCF-runtime-gedrag aanpassen door het maken van aangepaste extensies. Bericht Inspectors en Parameter Inspectors zijn twee uitbreidbaarheid mechanismen gebruikt om toegang te krijgen meer controle over de gegevens tussen een client en een service wordt doorgegeven. U moet de parameter inspectors gebruiken voor Invoervalidatie en bericht inspectors gebruikt alleen wanneer u nodig hebt om te controleren van het hele bericht binnenkomt en uitgaat van een service.</p><p>Om uit te voeren Invoervalidatie, u een .NET-klasse bouwen en implementeren van een aangepaste parameter inspector om te valideren van de parameters in operations in uw service. Vervolgens implementeert u de werking van een aangepast eindpunt voor validatie op zowel de client en de service inschakelen. Ten slotte implementeert u een aangepaste configuratie-element op de klasse waarmee u het gedrag van de uitgebreide aangepast eindpunt in het configuratiebestand van de service of de client beschikbaar</p>|

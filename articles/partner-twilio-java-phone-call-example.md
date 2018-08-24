---
title: Hoe u bellen vanuit Twilio (Java) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep van een webpagina met behulp van Twilio in een Java-toepassing in Azure.
services: ''
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 0381789e-e775-41a0-a784-294275192b1d
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 9ef754e9952bcbd47d668331e906b19ad582b90c
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818595"
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Hoe u een telefonische oproep met behulp van Twilio in een Java-toepassing in Azure
Het volgende voorbeeld ziet u hoe u Twilio kunt gebruiken om een aanroep van een webpagina die wordt gehost in Azure te maken. De resulterende toepassing wordt de gebruiker gevraagd om telefonische oproep waarden, zoals wordt weergegeven in de volgende schermopname.

![De aanroep van de Azure-formulier met behulp van Twilio en Java][twilio_java]

U moet het volgende voor het gebruik van de code in dit onderwerp doen:

1. Een Twilio-account en de verificatie verkrijgen token. Evalueren om te beginnen met Twilio, gelden prijzen [ http://www.twilio.com/pricing ] [ twilio_pricing]. U kunt zich aanmelden bij [ https://www.twilio.com/try-twilio ] [ try_twilio]. Zie voor meer informatie over de API die door Twilio [ http://www.twilio.com/api ] [ twilio_api].
2. De Twilio-JAR verkrijgen. Op [ https://github.com/twilio/twilio-java ] [ twilio_java_github], kunt u downloaden van de GitHub-bronnen en uw eigen JAR maken of een vooraf gemaakte JAR (met of zonder afhankelijkheden) te downloaden.
   De code in dit onderwerp is geschreven met behulp van de JAR met vooraf gebouwde TwilioJava 3.3.8 met afhankelijkheden.
3. De JAR toevoegen aan uw Java build path.
4. Als u Eclipse gebruikt om deze Java-toepassing te maken, bevatten de Twilio-JAR in uw toepassing implementatie-bestand (WAR) met behulp van Eclipse de implementatiefunctie van assembly. Als u Eclipse niet gebruikt om deze Java-toepassing te maken, controleert u of de Twilio-JAR is inbegrepen bij de dezelfde Azure-rol als uw Java-toepassing en toegevoegd aan het klassepad van uw toepassing.
5. Zorg ervoor dat uw keystore cacerts de Equifax beveiligde CA-certificaat bevat met MD5 vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (het serienummer is 35:DE:F4:CF en de SHA1-vingerafdruk is D2:32:09:AD:23:D 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dit is het certificaat van de certificeringsinstantie (CA) voor de [ https://api.twilio.com ] [ twilio_api_service] -service, die wordt aangeroepen wanneer u Twilio APIs gebruiken. Zie voor meer informatie over het toevoegen van deze CA-certificaat aan van de JDK cacert store [een certificaat toevoegen aan de Java CA-certificaat Store][add_ca_cert].

Bovendien vertrouwd zijn met de informatie op [maken van een Hello World-toepassing met behulp van de Azure Toolkit voor Eclipse][azure_java_eclipse_hello_world], of met andere technieken voor het hosten van Java-toepassingen in Azure als u bent niet met behulp van Eclipse, wordt sterk aanbevolen.

## <a name="create-a-web-form-for-making-a-call"></a>Maken van een webformulier voor het maken van een aanroep
De volgende code toont het maken van een webformulier om op te halen gebruikersgegevens voor het maken van een aanroep. Ten behoeve van dit voorbeeld wordt een nieuw dynamisch webproject, met de naam **TwilioCloud**, is gemaakt, en **callform.jsp** is toegevoegd als een JSP-bestand.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>De code voor het maken van de aanroep maken
De volgende code, die wordt aangeroepen wanneer de gebruiker het formulier weergegeven door callform.jsp is voltooid, maakt de aanroep-bericht en de oproep wordt gegenereerd. Voor dit voorbeeld de JSP-bestand met de naam **makecall.jsp** en is toegevoegd aan de **TwilioCloud** project. (Gebruik in plaats van de tijdelijke aanduiding voor waarden die zijn toegewezen aan uw Twilio-account en de verificatie-token **accountSID** en **authToken** in de onderstaande code.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";

         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");

         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");

         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");

         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");

         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;

         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");

         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);

         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Makecall.jsp weergegeven naast het maken van de aanroep van het eindpunt van de Twilio API-versie en de status van de aanroep. Een voorbeeld is de volgende schermopname:

![De aanroep van de Azure-antwoord met Twilio en Java][twilio_java_response]

## <a name="run-the-application"></a>De toepassing uitvoeren
Hier volgen de hoofdstappen voor het starten van uw toepassing. Details voor deze stappen kunnen u vinden op [maken van een Hello World-toepassing met behulp van de Azure Toolkit voor Eclipse][azure_java_eclipse_hello_world].

1. Uw TwilioCloud WAR exporteren naar de Azure **approot** map. 
2. Wijzigen **startup.cmd** uw WAR TwilioCloud uitpakken.
3. Compileer uw toepassing voor de rekenemulator.
4. Begint met de implementatie in de rekenemulator.
5. Open een browser en voer **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Geef waarden op in het formulier, klikt u op **deze aanroep**, en klikt u vervolgens bekijkt de resultaten in makecall.jsp.

Wanneer u klaar bent om te implementeren in Azure, recompile voor implementatie naar de cloud implementeren in Azure en http:// uitgevoerd*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp in de browser (Vervang de waarde voor  *your_hosted_name*).

## <a name="next-steps"></a>Volgende stappen
Deze code is opgegeven om weer te geven u basisfunctionaliteit met behulp van Twilio in Java op Azure. Voordat u implementeert naar Azure in de productieomgeving, kunt u meer foutafhandeling of andere functies toe te voegen. Bijvoorbeeld:

* U kunt in plaats van een webformulier, Azure storage-blobs of SQL-Database gebruiken voor het opslaan van telefoonnummers en aanroepen van tekst. Zie voor meer informatie over het gebruik van Azure storage-blobs in Java [over het gebruik van de Blob Storage-Service met Java][howto_blob_storage_java]. 
* U kunt **RoleEnvironment.getConfigurationSettings** om op te halen van de Twilio-account-ID en -verificatie-token van uw implementatie van configuratie-instellingen, in plaats van hard-coding de waarden in makecall.jsp. Voor informatie over de **RoleEnvironment** klasse, Zie [met behulp van de Azure Service Runtime Library in JSP] [ azure_runtime_jsp] en de documentatie van het pakket Azure Service Runtime op [ http://dl.windowsazure.com/javadoc][azure_javadoc].
* De code makecall.jsp wijst een Twilio-voorwaarde-URL, [ http://twimlets.com/message ] [ twimlet_message_url], naar de **Url** variabele. Deze URL bevat een Twilio Markup Language (TwiML)-antwoord dat informeert Twilio hoe om door te gaan met de aanroep. Bijvoorbeeld, de TwiML dat wordt geretourneerd kan bevatten een **&lt;zeg&gt;** term die resulteert in de tekst die wordt gesproken naar de ontvanger. In plaats van de geleverde Twilio-URL, kunt u uw eigen service om te reageren op aanvraag van Twilio; bouwen Zie voor meer informatie, [over het gebruik Twilio voor spraak- en SMS-mogelijkheden in Java][howto_twilio_voice_sms_java]. Meer informatie over TwiML kan worden gevonden op [ http://www.twilio.com/docs/api/twiml ] [ twiml], en meer informatie over **&lt;zeg&gt;** en andere Twilio-termen vinden op [ http://www.twilio.com/docs/api/twiml/say ] [ twilio_say].
* Lees de richtlijnen voor de beveiliging van Twilio op [ https://www.twilio.com/docs/security ] [ twilio_docs_security].

Zie voor meer informatie over Twilio [ https://www.twilio.com/docs ] [ twilio_docs].

## <a name="see-also"></a>Zie ook
* [Over het gebruik van Twilio voor spraak en SMS-mogelijkheden in Java][howto_twilio_voice_sms_java]
* [Een certificaat toe te voegen aan de Store met Java CA-certificaat][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app 
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg

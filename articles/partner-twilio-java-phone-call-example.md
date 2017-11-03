---
title: Het maken van een telefonische oproep van Twilio (Java) | Microsoft Docs
description: Informatie over het maken van een telefonische oproep van een webpagina met Twilio in een Java-toepassing in Azure.
services: 
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
ms.openlocfilehash: 04ecb80a2a9e15b549b47138caf71c7e64bda500
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Het maken van een telefoongesprek met Twilio in een Java-toepassing in Azure
Het volgende voorbeeld ziet u hoe u Twilio kunt gebruiken voor het maken van een aanroep van een webpagina die wordt gehost in Azure. De resulterende toepassing wordt de gebruiker gevraagd om telefoongesprek waarden, zoals wordt weergegeven in de volgende schermopname.

![De aanroep van de Azure-formulier met Twilio en Java][twilio_java]

U moet voor het gebruik van de code in dit onderwerp als volgt:

1. Verkrijgen van een Twilio-account en verificatie token. Om te beginnen met Twilio evalueren prijzen op [http://www.twilio.com/pricing][twilio_pricing]. U kunt zich aanmelden op [https://www.twilio.com/try-twilio][try_twilio]. Zie voor meer informatie over de API die wordt geleverd door Twilio [http://www.twilio.com/api][twilio_api].
2. De Twilio JAR verkrijgen. Op [https://github.com/twilio/twilio-java][twilio_java_github], kunt u de GitHub-bronnen te downloaden en uw eigen JAR maken of een vooraf samengestelde JAR (met of zonder afhankelijkheden) downloaden.
   De code in dit onderwerp is geschreven met behulp van de vooraf gemaakte TwilioJava 3.3.8 met afhankelijkheden JAR.
3. De JAR toevoegen aan uw Java build path.
4. Als u Eclipse gebruikt voor het maken van deze Java-toepassing, bevatten de Twilio JAR in uw implementatie toepassingsbestand (WAR) met behulp van de functie van Eclipse implementatie-assembly. Als u Eclipse niet gebruikt voor deze Java-toepassing maakt, controleert u de Twilio JAR is opgenomen binnen dezelfde Azure rol heeft als uw Java-toepassing en toegevoegd aan het klassepad van uw toepassing.
5. Zorg ervoor dat uw keystore cacerts de Equifax beveiligde CA-certificaat bevat met MD5 vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (het serienummer is 35:DE:F4:CF en de SHA1-vingerafdruk is D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dit is het certificaat (certificeringsinstantie)-certificaat voor de [https://api.twilio.com] [ twilio_api_service] -service, die wordt aangeroepen wanneer u Twilio APIs gebruiken. Zie voor meer informatie over het toevoegen van deze CA-certificaat aan uw JDK cacert store [een certificaat toevoegen aan de Java CA-certificaatarchief][add_ca_cert].

Bovendien bekend bent met de informatie op [maken een Hello World toepassing met behulp van de Azure-werkset voor Eclipse][azure_java_eclipse_hello_world], of met andere technieken voor het hosten van Java-toepassingen in Azure als u geen van Eclipse gebruikmaakt wordt sterk aanbevolen.

## <a name="create-a-web-form-for-making-a-call"></a>Maken van een webformulier voor het maken van een aanroep
De volgende code laat zien hoe een webformulier voor het ophalen van gegevens van de gebruiker voor een verbinding maken. Voor dit voorbeeld, een nieuw dynamic webproject met de naam **TwilioCloud**, is gemaakt, en **callform.jsp** is toegevoegd als een JSP-bestand.

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

## <a name="create-the-code-to-make-the-call"></a>De aanroep van de code maken
De volgende code, die wordt aangeroepen wanneer de gebruiker het formulier dat wordt weergegeven door callform.jsp is voltooid, maakt de aanroep-bericht en de oproep wordt gegenereerd. Voor dit voorbeeld het JSP-bestand de naam **makecall.jsp** en is toegevoegd aan de **TwilioCloud** project. (Gebruik in plaats van de tijdelijke aanduiding voor waarden die zijn toegewezen aan uw Twilio-account en de verificatie-token **accountSID** en **authToken** in de onderstaande code.)

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

Naast het maken van de aanroep geeft makecall.jsp het Twilio-eindpunt, API-versie en de status van de aanroep. Een voorbeeld is de volgende schermopname:

![De aanroep van de Azure-antwoord met Twilio en Java][twilio_java_response]

## <a name="run-the-application"></a>De toepassing uitvoeren
Hieronder volgen de hoofdstappen voor het starten van uw toepassing. Details voor deze stappen vindt u op [maken een Hello World toepassing met behulp van de Azure-werkset voor Eclipse][azure_java_eclipse_hello_world].

1. Uw TwilioCloud WAR exporteren naar de Azure **approot** map. 
2. Wijzig **startup.cmd** uw WAR TwilioCloud uitpakken.
3. Uw toepassing voor de rekenemulator compileren.
4. De implementatie start in de rekenemulator.
5. Open een browser en voer **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Voer waarden in het formulier, klikt u op **bellen**, en vervolgens de resultaten bekijken in makecall.jsp.

Wanneer u klaar bent om te implementeren in Azure, recompile voor implementatie naar de cloud implementeren in Azure en http:// uitgevoerd*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp in de browser (vervangen door de waarde voor *your_hosted_name*).

## <a name="next-steps"></a>Volgende stappen
Deze code is opgegeven om weer te geven u basisfunctionaliteit met Twilio in Java in Azure. Voordat u Azure implementeert in productie, wilt u mogelijk meer foutafhandeling of andere functies toevoegen. Bijvoorbeeld:

* U kunt in plaats van een webformulier, Azure storage-blobs of SQL-Database gebruiken op te slaan telefoonnummers en aanroepen van tekst. Zie voor meer informatie over het gebruik van Azure storage-blobs in Java [het gebruik van de Blob Storage-Service vanuit Java][howto_blob_storage_java]. Zie voor meer informatie over het gebruik van SQL-Database in Java [met behulp van SQL-Database in Java][howto_sql_azure_java].
* U kunt **RoleEnvironment.getConfigurationSettings** voor het ophalen van de Twilio-account-ID en verificatie token van uw implementatie-configuratie-instellingen, in plaats van hard-coding van de waarden in makecall.jsp. Voor informatie over de **RoleEnvironment** klasse, Zie [met behulp van de Azure Service Runtime-bibliotheek in JSP] [ azure_runtime_jsp] en de documentatie van het pakket Azure Service Runtime op [http://dl.windowsazure.com/javadoc][azure_javadoc].
* De code makecall.jsp wijst een URL Twilio-voorwaarde [http://twimlets.com/message][twimlet_message_url]toe aan de **Url** variabele. Deze URL biedt een Twilio Markup Language (TwiML) antwoord dat informeert Twilio hoe om door te gaan met de aanroep. Bijvoorbeeld, de TwiML die wordt geretourneerd kunt bevatten een  **&lt;zeg&gt;**  term die resulteert in de tekst wordt gesproken naar de ontvanger aanroep. In plaats van de geleverde Twilio-URL, kan u uw eigen service om te reageren op aanvragen van Twilio; samenstellen Zie voor meer informatie [hoe gebruik Twilio voor spraak- en SMS-mogelijkheden in Java][howto_twilio_voice_sms_java]. Meer informatie over TwiML kan worden gevonden op [http://www.twilio.com/docs/api/twiml][twiml], en meer informatie over  **&lt;zeg&gt;**  en andere Twilio-termen kunnen worden gevonden op [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lees de richtlijnen voor de beveiliging van Twilio op [https://www.twilio.com/docs/security][twilio_docs_security].

Zie voor meer informatie over Twilio [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Zie ook
* [Het gebruik van Twilio voor spraak- en SMS-mogelijkheden in Java][howto_twilio_voice_sms_java]
* [Een certificaat toe te voegen aan het certificaatarchief van de Java-CA][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
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

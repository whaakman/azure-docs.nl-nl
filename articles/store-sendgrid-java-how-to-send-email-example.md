---
title: Store-sendgrid-Java-How-to-Send-email-example
description: Hoe u e-mail verzenden via SendGrid van Java in een Azure-implementatie
services: ''
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: af096a73-6985-4350-92e4-ce1722c8d72f
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com
ms.openlocfilehash: 400c8ac229e00e818e336f1c47b126d3e8a2d155
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51253836"
---
# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Hoe u E-mail verzenden via SendGrid van Java in een Azure-implementatie
Het volgende voorbeeld ziet u hoe u SendGrid kunt gebruiken voor het verzenden van e-mailberichten van een webpagina die wordt gehost in Azure. De resulterende toepassing krijgt de gebruiker voor e-waarden, zoals wordt weergegeven in de volgende schermopname.

![E-formulier][emailform]

Het e-mailbericht ziet eruit als de volgende schermopname.

![E-mailbericht][emailsent]

U moet het volgende voor het gebruik van de code in dit onderwerp doen:

1. De javax.mail JAR-bestanden, bijvoorbeeld verkrijgen van <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Voeg de JAR-bestanden toe aan uw Java build path.
3. Als u Eclipse gebruikt om deze Java-toepassing te maken, kunt u de SendGrid-bibliotheken opnemen in uw toepassing implementatie-bestand (WAR) met behulp van Eclipse de implementatiefunctie van assembly. Als u Eclipse niet gebruikt om deze Java-toepassing te maken, controleert u of de clientbibliotheken zijn opgenomen in de dezelfde Azure-rol als uw Java-toepassing en toegevoegd aan het klassepad van uw toepassing.

Ook moet u uw eigen SendGrid-gebruikersnaam en wachtwoord, kunnen het e-mailbericht verzenden. Als u wilt beginnen met SendGrid, Zie [e-mail via SendGrid van Java verzenden](store-sendgrid-java-how-to-send-email.md).

Bovendien vertrouwd zijn met de informatie op [een Hello World-toepassing voor Azure maken in Eclipse](https://msdn.microsoft.com/library/windowsazure/hh690944), of met andere technieken voor het hosten van Java-toepassingen in Azure als u niet met behulp van Eclipse, wordt ten zeerste aangeraden.

## <a name="create-a-web-form-for-sending-email"></a>Maken van een webformulier voor het verzenden van e-mailadres
De volgende code toont het maken van een webformulier om op te halen gebruikersgegevens voor het verzenden van e-mailbericht. Voor de doeleinden van deze inhoud, de JSP-bestand met de naam **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>De code voor het verzenden van het e-mailadres maken
De volgende code, die wordt aangeroepen wanneer u het formulier in emailform.jsp hebt voltooid, wordt het e-mailbericht gemaakt en verzonden. Voor de doeleinden van deze inhoud, de JSP-bestand met de naam **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%

     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");

     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;

            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {

         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";

         Properties properties;

         properties = new Properties();

         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");

         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");

         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();

         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);

         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);

         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 

         message = new MimeMessage(mailSession);

         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            

         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);

         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");

         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");

         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();

        out.println("<p>Email processing completed.</p>");

     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>

    </body>
    </html>

Naast het e-mailbericht verzendt, biedt emailform.jsp een resultaat voor de gebruiker. een voorbeeld is de volgende schermopname:

![Resultaat van de e-mail verzenden][emailresult]

## <a name="next-steps"></a>Volgende stappen
Uw toepassing in de rekenemulator implementeert en uitvoert in een browser emailform.jsp, voer waarden in de vorm, klik op **dit e-mailbericht verzenden**, en vervolgens de resultaten in sendemail.jsp bekijken.

Deze code is opgegeven leert u hoe u SendGrid in Java op Azure gebruiken. Voordat u implementeert naar Azure in de productieomgeving, kunt u meer foutafhandeling of andere functies toe te voegen. Bijvoorbeeld: 

* U kunt Azure storage-blobs of SQL-Database gebruiken voor het opslaan van e-mailadressen en e-mailberichten, in plaats van een webformulier. Zie voor meer informatie over het gebruik van Azure storage-blobs in Java [over het gebruik van de Blob Storage-Service met Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Zie voor meer informatie over het gebruik van SQL-Database in Java [SQL-Database in Java met behulp van](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-java).
* U kunt `RoleEnvironment.getConfigurationSettings` om op te halen van de SendGrid-gebruikersnaam en het wachtwoord van uw implementatie van configuratie-instellingen, in plaats van het webformulier om op te halen die waarden. Voor informatie over de `RoleEnvironment` klasse, Zie [met behulp van de Azure Service Runtime Library in JSP](https://msdn.microsoft.com/library/windowsazure/hh690948) en de documentatie van het pakket Azure Service Runtime op <http://dl.windowsazure.com/javadoc>.
* Zie voor meer informatie over het gebruik van SendGrid in Java [e-mail via SendGrid van Java verzenden](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg

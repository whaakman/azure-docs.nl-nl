---
title: Het gebruik van de SendGrid-e-mailservice (Java) | Microsoft Docs
description: Meer informatie over hoe verzenden van e-mailbericht met de SendGrid-e-mailservice op Azure. Codevoorbeelden geschreven in Java.
services: 
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: cc75c43e-ede9-492b-98c2-9147fcb92c21
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork
ms.openlocfilehash: 85a0e302626ca14ac039ee6f662f372ddbeb62c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-send-email-using-sendgrid-from-java"></a>Het verzenden van E-mail via SendGrid met Java
Deze handleiding wordt uitgelegd hoe u algemene programmeertaken met de SendGrid-e-mailservice uitvoert op Azure. De voorbeelden zijn geschreven in Java. De scenario's worden behandeld: **construeren e**, **verzenden van e-mail**, **bijlagen toevoegen**, **met filters**, en **eigenschappen bijwerken**. Zie voor meer informatie over SendGrid en verzenden van e-mail, de [Vervolgstappen](#next-steps) sectie.

## <a name="what-is-the-sendgrid-email-service"></a>Wat is de SendGrid-e-mailservice?
SendGrid is een [cloud-gebaseerde e-mailservice] die biedt betrouwbare [levering van de transactionele e], schaalbaarheid en realtime-analyses samen met de flexibele API's die aangepaste integratie te vereenvoudigen. Veelvoorkomende gebruiksscenario's van SendGrid zijn onder andere:

* Automatisch verzenden van meldingen op klanten
* Beheer van distributiepunt bevat voor het verzenden van klanten maandelijkse e-Folders en speciale aanbiedingen
* Realtime metrische gegevens voor items zoals geblokkeerd e- en reactietijd van de klant verzamelen
* Genereren van rapporten om trends te identificeren
* Doorsturen van vragen van klanten
* E-mailmeldingen van uw toepassing

Zie voor meer informatie <http://sendgrid.com>.

## <a name="create-a-sendgrid-account"></a>Een SendGrid-account maken
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-the-javaxmail-libraries"></a>Procedure: de bibliotheken javax.mail gebruiken
De bibliotheken javax.mail bijvoorbeeld verkrijgen van <http://www.oracle.com/technetwork/java/javamail> en importeer ze in uw code. Op een hoog niveau is het proces voor het gebruik van de bibliotheek javax.mail om via SMTP e-mail te verzenden naar het volgende doen:

1. Geef de SMTP-waarden, met inbegrip van de SMTP-server die voor SendGrid smtp.sendgrid.net.

```
        import java.util.Properties;
        import javax.activation.*;
        import javax.mail.*;
        import javax.mail.internet.*;

        public class MyEmailer {
           private static final String SMTP_HOST_NAME = "smtp.sendgrid.net";
           private static final String SMTP_AUTH_USER = "your_sendgrid_username";
           private static final String SMTP_AUTH_PWD = "your_sendgrid_password";

           public static void main(String[] args) throws Exception{
               new MyEmailer().SendMail();
           }

           public void SendMail() throws Exception
           {
              Properties properties = new Properties();
                 properties.put("mail.transport.protocol", "smtp");
                 properties.put("mail.smtp.host", SMTP_HOST_NAME);
                 properties.put("mail.smtp.port", 587);
                 properties.put("mail.smtp.auth", "true");
                 // …
```

1. Breid de *javax.mail.Authenticator* klasse, en in uw implementatie van de *getPasswordAuthentication* methode, uw SendGrid-gebruikersnaam en wachtwoord worden geretourneerd.  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. Maken van een sessie geverifieerde e-mail via een *javax.mail.Session* object.  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. Maken van uw bericht en toewijzen **naar**, **van**, **onderwerp** en waarden van inhoud. Dit wordt weergegeven in de [How To: maken van een e-mailbericht](#how-to-create-an-email) sectie.
4. Het bericht verzenden via een *javax.mail.Transport* object. Dit wordt weergegeven in de [How To: e-mailbericht verzenden] [hoe: e-mailbericht verzenden] sectie.

## <a name="how-to-create-an-email"></a>Procedure: een e-mailbericht maken
Hieronder ziet u hoe u waarden opgeven voor een e-mailbericht.

    MimeMessage message = new MimeMessage(mailSession);
    Multipart multipart = new MimeMultipart("alternative");
    BodyPart part1 = new MimeBodyPart();
    part1.setText("Hello, Your Contoso order has shipped. Thank you, John");
    BodyPart part2 = new MimeBodyPart();
    part2.setContent(
        "<p>Hello,</p>
        <p>Your Contoso order has <b>shipped</b>.</p>
        <p>Thank you,<br>John</br></p>",
        "text/html");
    multipart.addBodyPart(part1);
    multipart.addBodyPart(part2);
    message.setFrom(new InternetAddress("john@contoso.com"));
    message.addRecipient(Message.RecipientType.TO,
       new InternetAddress("someone@example.com"));
    message.setSubject("Your recent order");
    message.setContent(multipart);

## <a name="how-to-send-an-email"></a>Procedure: een e-mailbericht verzenden
Hieronder ziet u hoe u een e-mailbericht verzendt.

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a>Procedure: een bijlage toevoegen
De volgende code laat zien hoe een bijlage toevoegen.

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\";
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify the local file to attach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses the local file name as the attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Procedure: filters gebruiken om in te schakelen voetteksten, tracerings- en analyses
SendGrid biedt extra e-mailfunctionaliteit met behulp van *filters*. Dit zijn de instellingen die kunnen worden toegevoegd aan een e-mailbericht om in te schakelen van specifieke functionaliteit, zoals het inschakelen van Klik bijhouden, Google analytics en abonnement bijhouden. Zie voor een volledige lijst met filters [filterinstellingen][Filter Settings].

* Hieronder ziet u hoe u een filter voettekst die resulteert in HTML-tekst die wordt weergegeven aan de onderkant van het e-mailbericht wordt verzonden invoegen.

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* Een ander voorbeeld van een filter is klikt u op bijhouden. Stel dat uw e-mailtekst een hyperlink, zoals de volgende bevat, en u wilt bijhouden van de snelheid klikt u op:

      messagePart.setContent(
          "Hello,
          <p>This is the body of the message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* Zodat het klikt u op het bijhouden van de volgende code gebruiken:

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a>Procedure: e-eigenschappen bijwerken
Sommige eigenschappen van de e-mail kunnen worden overschreven met  **ingesteld*eigenschap*** of toegevoegd met behulp van  **toevoegen*eigenschap***.

Bijvoorbeeld, om op te geven **ReplyTo** adressen, gebruikt u de volgende:

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

Om toe te voegen een **Cc** ontvanger, gebruikt u de volgende:

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a>Procedure: aanvullende SendGrid-services gebruiken
SendGrid biedt web-API's waarmee u kunt gebruikmaken van aanvullende SendGrid-functionaliteit van uw Azure-toepassing. Zie voor meer informatie de [SendGrid-API-documentatie][SendGrid API documentation].

## <a name="next-steps"></a>Volgende stappen
Nu u de basisprincipes van de SendGrid-e-mailservice hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Voorbeeldbestand dat laat zien met SendGrid in een Azure-implementatie: [het verzenden van e-mail via SendGrid van Java in een Azure-implementatie](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* SendGrid-API-documentatie: <https://sendgrid.com/docs/API_Reference/index.html>
* SendGrid speciale aanbieding voor Azure-klanten: <https://sendgrid.com/windowsazure.html>

[http://sendgrid.com]: https://sendgrid.com
[http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[http://sendgrid.com/features]: https://sendgrid.com/features
[http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
[cloud-gebaseerde e-mailservice]: https://sendgrid.com/email-solutions
[levering van de transactionele e]: https://sendgrid.com/transactional-email

---
title: Het gebruik van de e-mailservice van SendGrid (Java) | Microsoft Docs
description: Informatie over hoe e-mailbericht met de e-mailservice van SendGrid verzenden op Azure. Voorbeelden van code geschreven in Java.
services: ''
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
ms.openlocfilehash: cdc6e082207b9128be3e3005305510f3ee493850
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427838"
---
# <a name="how-to-send-email-using-sendgrid-from-java"></a>E-mail via SendGrid van Java verzenden
Deze handleiding laat zien hoe u veelvoorkomende programming taken met de e-mailservice van SendGrid uitvoeren op Azure. De voorbeelden zijn geschreven in Java. De behandelde scenario's zijn **maken e**, **verzenden van e-mail**, **bijlagen toevoegen**, **met behulp van filters**, en **bijwerken van eigenschappen**. Zie voor meer informatie over SendGrid en het verzenden van e-mail, het [Vervolgstappen](#next-steps) sectie.

## <a name="what-is-the-sendgrid-email-service"></a>Wat is het e-mailservice van SendGrid?
SendGrid is een [cloud-gebaseerde e-mailservice] die zorgt voor betrouwbare [transactionele e-mail delivery], schaalbaarheid en realtime analyses en flexibele API's waarmee u aangepaste integratie eenvoudig. Veelvoorkomende SendGrid gebruiksscenario's zijn onder andere:

* Automatisch verzenden van meldingen voor klanten
* Beheer van distributie bevat voor het verzenden van klanten maandelijks e-flyers en speciale aanbiedingen
* Realtime metrische gegevens voor zaken zoals geblokkeerde e-mail- en reactietijd van klanten verzamelen
* Genereren van rapporten voor het identificeren van trends
* Doorsturen van vragen van klanten
* E-mailmeldingen van uw toepassing

Zie voor meer informatie, <http://sendgrid.com>.

## <a name="create-a-sendgrid-account"></a>Een SendGrid-account maken
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-the-javaxmail-libraries"></a>Hoe: Gebruik de bibliotheken javax.mail
De javax.mail-bibliotheken, bijvoorbeeld verkrijgen van <http://www.oracle.com/technetwork/java/javamail> en importeer ze in uw code. Bij een op hoog niveau, wordt het proces voor het gebruik van de bibliotheek javax.mail e-mail verzenden via SMTP is het volgende doen:

1. Geef de SMTP-waarden, met inbegrip van de SMTP-server, waarbij voor SendGrid smtp.sendgrid.net is.

```
        import java.util.Properties;
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
2. Maken van een e-mailadres geverifieerde sessie via een *javax.mail.Session* object.  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. Uw bericht maken en toewijzen **naar**, **van**, **onderwerp** en waarden van inhoud. Dit wordt weergegeven in de [How To: maken van een e-mailbericht](#how-to-create-an-email) sectie.
4. Het bericht verzenden via een *javax.mail.Transport* object. Dit wordt weergegeven in de [How To: een e-mailbericht verzenden] [# How-to-naar-een-e-mail verzenden] sectie.

## <a name="how-to-create-an-email"></a>Hoe: een e-mailbericht maken
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

## <a name="how-to-send-an-email"></a>Hoe: een e-mailbericht verzenden
Hieronder ziet u hoe u een e-mailbericht verzendt.

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a>Hoe: een bijlage toevoegen
De volgende code toont u hoe u een bijlage toevoegen.

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

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Hoe: filters gebruiken om in te schakelen voetteksten, tracering en analytics
SendGrid zorgt voor extra e-mailfunctionaliteit via het gebruik van *filters*. Dit zijn de instellingen die kunnen worden toegevoegd aan een e-mailbericht om in te schakelen specifieke functionaliteit, zoals het inschakelen van klikken traceren in, Google analytics en abonnement bijhouden. Zie voor een volledige lijst met filters, [filterinstellingen][Filter Settings].

* Hieronder ziet u hoe u een voettekst filter die resulteert in HTML-tekst die wordt weergegeven aan de onderkant van het e-mailbericht wordt verzonden invoegt.

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* Een ander voorbeeld van een filter is bijhouden van kliks. Stel dat uw e-mailtekst een hyperlink, zoals de volgende bevat, en u wilt bijhouden van het tarief klikt u op:

      messagePart.setContent(
          "Hello,
          <p>This is the body of the message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* Gebruik de volgende code waarmee de klikken traceren in:

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a>Hoe: eigenschappen van de e-mailadres bijwerken
Sommige eigenschappen van het e-mailadres kunnen worden overschreven met behulp van **eigenschap** of toegevoegd met behulp van **voegt u de eigenschap**.

Bijvoorbeeld, om op te geven **ReplyTo** adressen, gebruikt u de volgende:

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

Om toe te voegen een **Cc** ontvanger, gebruikt u de volgende:

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a>Hoe: extra SendGrid-services gebruiken
SendGrid biedt web-API's die u kunt gebruikmaken van aanvullende SendGrid-functionaliteit van uw Azure-toepassing. Zie voor meer informatie, de [SendGrid-API-documentatie][SendGrid API documentation].

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van de SendGrid-e-mailservice hebt geleerd, volgt u deze koppelingen voor meer informatie.

* Voorbeeld waarin wordt gedemonstreerd hoe SendGrid in een Azure-implementatie: [hoe e-mail verzenden via SendGrid van Java in een Azure-implementatie](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* SendGrid-API-documentatie: <https://sendgrid.com/docs/API_Reference/index.html>
* SendGrid speciaal aanbod voor Azure-klanten: <https://sendgrid.com/windowsazure.html>

[http://sendgrid.com]: https://sendgrid.com
[http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[http://sendgrid.com/features]: https://sendgrid.com/features
[http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
[cloud-gebaseerde e-mailservice]: https://sendgrid.com/email-solutions
[transactionele e-mail delivery]: https://sendgrid.com/transactional-email

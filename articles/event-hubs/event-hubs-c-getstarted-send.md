---
title: Gebeurtenissen verzenden naar Azure Event Hubs met C | Microsoft Docs
description: Gebeurtenissen verzenden naar Azure Event Hubs met C
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 12/4/2017
ms.author: sethm
ms.openlocfilehash: 2b714c5de96a8fb7ed66a30c62daaa38b84fdc5b
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Gebeurtenissen verzenden naar Azure Event Hubs met C

## <a name="introduction"></a>Inleiding
Event Hubs is een zeer schaalbaar systeem die kan worden miljoenen gebeurtenissen per seconde opnemen voor het inschakelen van een toepassing te verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en toepassingen. Zodra verzameld in een event hub, kunt u transformeren en opslaan van gegevens met behulp van een realtime analytics-provider of opslagcluster.

Zie voor meer informatie [Event Hubs-overview] [Event Hubs-overview].

Deze zelfstudie wordt beschreven hoe u kunt gebeurtenissen verzenden naar een event hub met een consoletoepassing in C. Meer informatie over gebeurtenissen ontvangen, klikt u op de juiste ontvangende taal in de tabel links van inhoud.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een C-ontwikkelomgeving. Deze zelfstudie wordt ervan uitgegaan dat de stack gcc op een Azure Linux-VM met Ubuntu 14.04.
* [Microsoft Visual Studio](https://www.visualstudio.com/).
* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

## <a name="send-messages-to-event-hubs"></a>Berichten verzenden naar Event Hubs
In deze sectie wordt beschreven hoe een C-App voor het verzenden van gebeurtenissen naar uw event hub. De code maakt gebruik van de bibliotheek Proton AMQP van de [Apache Qpid project](http://qpid.apache.org/). Dit is vergelijkbaar met het gebruik van Service Bus-wachtrijen en onderwerpen met AMQP van C zoals [in dit voorbeeld](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Zie voor meer informatie de [Qpid Proton documentatie](http://qpid.apache.org/proton/index.html).

1. Van de [Qpid AMQP Messenger pagina](https://qpid.apache.org/proton/messenger.html), volg de instructies voor het installeren van Qpid Proton, afhankelijk van uw omgeving.
2. Samengesteld op basis van de bibliotheek Proton, installeert u de volgende pakketten:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Download de [Qpid Proton bibliotheek](http://qpid.apache.org/proton/index.html), en pak het bestand, bijvoorbeeld:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Maak een build-directory, compileren en installeren:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Maak een nieuw bestand met de naam in uw directory work **sender.c** met de volgende code. Vergeet niet ter vervanging van de waarden voor uw SAS-sleutel/naam, de naam event hub en de naamruimte. U moet ook vervangen door een URL-codering versie van de sleutel voor de **SendRule** eerder hebt gemaakt. U kunt URL coderen deze [hier](http://www.w3schools.com/tags/ref_urlencode.asp).
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://{SAS Key Name}:{SAS key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. Compileren van het bestand, ervan uitgaande dat **gcc**:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > Deze code wordt een uitgaande 1-venster om af te dwingen de uitgaande berichten zo snel mogelijk. Het is raadzaam dat uw toepassing probeert te batch berichten doorvoer te verhogen. Zie de [Qpid AMQP Messenger pagina](https://qpid.apache.org/proton/messenger.html) voor informatie over het gebruik van de bibliotheek Qpid Proton in deze en andere omgevingen en platforms waarvoor bindingen worden geleverd (momenteel Perl, PHP, Python en Ruby).


## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

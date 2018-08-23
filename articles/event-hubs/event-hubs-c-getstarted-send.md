---
title: Gebeurtenissen verzenden naar Azure Event Hubs met c# | Microsoft Docs
description: Gebeurtenissen verzenden naar Azure Event Hubs met c#
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 262f274541f486f5457ef8eae6fd4f60fb34824e
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42060583"
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Gebeurtenissen verzenden naar Azure Event Hubs met c#

## <a name="introduction"></a>Inleiding
Eventhubs is een zeer schaalbare opname-systeem dat kan miljoenen gebeurtenissen per seconde opnemen, zodat een toepassing te verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door uw verbonden apparaten en toepassingen. Zodra de verzameld in een event hub, kunt u deze kunt transformeren en opslaan van gegevens met behulp van een realtime analytics-provider of opslagcluster.

Zie voor meer informatie de [Event Hubs-overzicht](https://docs.microsoft.com/azure/event-hubs/event-hubs-overview).

In deze zelfstudie wordt beschreven hoe u voor het verzenden van gebeurtenissen naar een event hub met een consoletoepassing in c Voor meer informatie over het ontvangen van gebeurtenissen, klikt u op de juiste taal voor ontvangst in de linker inhoudsopgave.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een C-ontwikkelomgeving. In deze zelfstudie wordt ervan uitgegaan dat de gcc-stack op een virtuele Azure Linux-machine met Ubuntu 14.04.
* [Microsoft Visual Studio](https://www.visualstudio.com/).
* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.

## <a name="send-messages-to-event-hubs"></a>Berichten verzenden naar Event Hubs
In deze sectie wordt beschreven hoe een App C voor het verzenden van gebeurtenissen naar uw event hub. De code wordt gebruikgemaakt van de bibliotheek Proton AMQP uit de [Apache Qpid project](http://qpid.apache.org/). Dit is vergelijkbaar met het gebruik van Service Bus-wachtrijen en onderwerpen met AMQP from C zoals [in dit voorbeeld](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Zie voor meer informatie de [Qpid Proton documentatie](http://qpid.apache.org/proton/index.html).

1. Uit de [Qpid AMQP Messenger pagina](https://qpid.apache.org/proton/messenger.html), volg de instructies voor het installeren van Qpid Proton, afhankelijk van uw omgeving.
2. Installeer de bibliotheek Proton compileren, de volgende pakketten:
   
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
5. Maak een nieuw bestand met de naam in uw directory work **sender.c** door de volgende code. Vergeet niet om u te vervangen door de waarden voor uw SAS-sleutel/name, naam event hub en naamruimte. U moet ook vervangen door een URL-gecodeerde versie van de sleutel voor de **SendRule** eerder hebt gemaakt. U kunt URL coderen deze [hier](http://www.w3schools.com/tags/ref_urlencode.asp).
   
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
    > Deze code gebruikt een uitgaande 1-venster om af te dwingen de berichten uit zo snel mogelijk. Het verdient aanbeveling dat uw toepassing probeert te batchberichten om doorvoer te vergroten. Zie de [Qpid AMQP Messenger pagina](https://qpid.apache.org/proton/messenger.html) voor informatie over het gebruik van de bibliotheek Qpid Proton in deze en andere omgevingen, en vanaf het platform waarvoor bindingen zijn opgegeven (momenteel Perl, PHP, Python en Ruby).


## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

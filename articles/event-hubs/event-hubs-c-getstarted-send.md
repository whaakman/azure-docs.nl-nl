---
title: Gebeurtenissen verzenden met behulp van C - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een overzicht voor het maken van een toepassing C die gebeurtenissen naar Azure Event Hubs verzendt.
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
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 8c134ae9944517d6ae66fcd22e06bbfc599912b4
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53076389"
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Gebeurtenissen verzenden naar Azure Event Hubs met c#

## <a name="introduction"></a>Inleiding
Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

In deze zelfstudie wordt beschreven hoe u voor het verzenden van gebeurtenissen naar een event hub met een consoletoepassing in c 

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een C-ontwikkelomgeving. In deze zelfstudie wordt ervan uitgegaan dat de gcc-stack op een virtuele Azure Linux-machine met Ubuntu 14.04.
* [Microsoft Visual Studio](https://www.visualstudio.com/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken
In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Als u wilt een naamruimte en een event hub maken, volgt u de procedure in [in dit artikel](event-hubs-create.md).

Haal de waarde van de toegangssleutel voor de event hub met de instructies in het artikel: [-verbindingsreeks ophalen](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). U gebruikt de toegangssleutel in de code die u verderop in deze zelfstudie schrijft. De naam van de standaardwaarde is: **RootManageSharedAccessKey**.

Nu gaat u verder met de volgende stappen in deze zelfstudie.

## <a name="write-code-to-send-messages-to-event-hubs"></a>Schrijf code om berichten te verzenden naar Event Hubs
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

Voer de toepassing berichten te verzenden naar de event hub. 

Gefeliciteerd! U hebt nu berichten verzonden naar een Event Hub.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over het ontvangen van gebeurtenissen uit de event hub, klikt u op de juiste taal voor ontvangst onder **gebeurtenissen ontvangen van een event hub** knooppunt in de inhoudsopgave.


<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png

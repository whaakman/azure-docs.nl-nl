---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van .NET Framework | Microsoft Docs
description: Ga aan de slag met het verzenden van gebeurtenissen naar Event Hubs met .NET Framework
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/10/2017
ms.author: sethm
ms.openlocfilehash: 16da4e1732445b2480daf18130ea74935c6e6c49
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="send-events-to-azure-event-hubs-using-the-net-framework"></a>Gebeurtenissen verzenden naar Azure Event Hubs met behulp van .NET Framework

## <a name="introduction"></a>Inleiding

Event Hubs is een service die grote hoeveelheden gebeurtenisgegevens (telemetrie) van verbonden apparaten en toepassingen verwerkt. Nadat u gegevens in Event Hubs hebt verzameld, kunt u de gegevens opslaan met behulp van een opslagcluster of transformeren met een provider van realtime-analyses. Deze functie voor grootschalige gebeurtenisverzameling en -verwerking is een belangrijk onderdeel van de architectuur van moderne toepassingen, met inbegrip van het Internet der dingen (IoT).

In deze zelfstudie kunt u zien hoe u [Azure Portal](https://portal.azure.com) gebruikt om een Event Hub te maken. U leert ook hoe u gebeurtenissen met .NET Framework naar een Event Hub verzendt via een in C# geschreven consoletoepassing. Zie het artikel [Gebeurtenissen ontvangen met .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) of klik op de juiste taal voor ontvangst in de tabel links om gebeurtenissen te ontvangen via .NET Framework.

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

* [Microsoft Visual Studio 2015 of hoger](http://visualstudio.com). In de schermafbeeldingen in deze zelfstudie wordt Visual Studio 2017 gebruikt.
* Een actief Azure-account. Als u geen Azure-account hebt, kunt u binnen een paar minuten een gratis account maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/free/) voor meer informatie.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken

In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Volg de procedure in [dit artikel](event-hubs-create.md) om een naamruimte en Event Hub te maken en ga daarna verder met de volgende stappen in deze zelfstudie.

## <a name="create-a-sender-console-application"></a>Een consoletoepassing voor afzenders maken

In deze sectie schrijft u een Windows-consoletoepassing die gebeurtenissen naar uw Event Hub verzendt.

1. Maak in Visual Studio een nieuw Visual C# bureaublad-app-project met behulp van de projectsjabloon**Consoletoepassing**. Noem het project **Afzender**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. Klik in Solution Explorer met de rechtermuisknop op het project **Afzender** en klik op **NuGet-pakketten beheren voor oplossing**. 
3. Klik op het tabblad **Bladeren** en zoek vervolgens naar `WindowsAzure.ServiceBus`. Klik op **Installeren** en accepteer de gebruiksvoorwaarden. 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    Er wordt door Visual Studio een verwijzing naar het [ NuGet-pakket Azure Service Bus-bibliotheek](https://www.nuget.org/packages/WindowsAzure.ServiceBus) gedownload, geïnstalleerd en toegevoegd.
4. Voeg aan het begin van het bestand **Program.cs** de volgende `using` instructies toe:
   
  ```csharp
  using System.Threading;
  using Microsoft.ServiceBus.Messaging;
  ```
5. Voeg de volgende velden toe aan de klasse **Program**, waarbij u de waarden van de tijdelijke aanduiding vervangt door de naam van de Event Hub die u in de vorige sectie hebt gemaakt, en de verbindingsreeks op naamruimteniveau die u eerder hebt opgeslagen.
   
  ```csharp
  static string eventHubName = "{Event Hub name}";
  static string connectionString = "{send connection string}";
  ```
6. Voeg de volgende methode toe aan de klasse **Program**:
   
  ```csharp
  static void SendingRandomMessages()
  {
      var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
      while (true)
      {
          try
          {
              var message = Guid.NewGuid().ToString();
              Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
              eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
          }
          catch (Exception exception)
          {
              Console.ForegroundColor = ConsoleColor.Red;
              Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
              Console.ResetColor();
          }
   
          Thread.Sleep(200);
      }
  }
  ```
   
  Met deze methode worden er continu gebeurtenissen naar uw Event Hub verzonden, met een vertraging van 200 ms.
7. Voeg tot slot de volgende regels toe aan de methode **Main**:
   
  ```csharp
  Console.WriteLine("Press Ctrl-C to stop the sender process");
  Console.WriteLine("Press Enter to start now");
  Console.ReadLine();
  SendingRandomMessages();
  ```
8. Voer het programma uit en controleer of er geen fouten zijn.
  
Gefeliciteerd. U hebt nu berichten verzonden naar een Event Hub.

## <a name="next-steps"></a>Volgende stappen
Nu u een werkende toepassing hebt gebouwd die een Event Hub maakt en gegevens verzendt, kunt u naar de volgende scenario's gaan:

* [Gebeurtenissen ontvangen met de gebeurtenisprocessorhost](event-hubs-dotnet-framework-getstarted-receive-eph.md)
* [Documentatie over de gebeurtenisprocessorhost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png


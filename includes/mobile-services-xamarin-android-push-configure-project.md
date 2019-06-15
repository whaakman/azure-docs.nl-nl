---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 69dc0e1c14bc88cdbf0aa48700f95058ba759cc0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66140294"
---
1. In de oplossingsweergave (of **Solution Explorer** in Visual Studio), met de rechtermuisknop op de **onderdelen** map, klikt u op **meer onderdelen ophalen...** , zoek de **Google Cloud Messaging-Client** onderdeel en voeg deze toe aan het project.
2. Open het bestand ToDoActivity.cs-project en voeg de volgende gebruiksinstructie aan de klasse:

    ```csharp
    using Gcm.Client;
    ```

3. In de **ToDoActivity** klasse, voeg de volgende nieuwe code: 

    ```csharp
    // Create a new instance field for this activity.
    static ToDoActivity instance = new ToDoActivity();

    // Return the current activity instance.
    public static ToDoActivity CurrentActivity
    {
        get
        {
            return instance;
        }
    }
    // Return the Mobile Services client.
    public MobileServiceClient CurrentClient
    {
        get
        {
            return client;
        }
    }
    ```

    Hiermee kunt u toegang tot de mobiele client-exemplaar van het serviceproces van de push-handler.
4. Voeg de volgende code aan de **OnCreate** methode na de **MobileServiceClient** wordt gemaakt:

    ```csharp
    // Set the current instance of TodoActivity.
    instance = this;

    // Make sure the GCM client is set up correctly.
    GcmClient.CheckDevice(this);
    GcmClient.CheckManifest(this);

    // Register the app for push notifications.
    GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);
    ```

Uw **ToDoActivity** is nu klaar voor het toevoegen van pushmeldingen.

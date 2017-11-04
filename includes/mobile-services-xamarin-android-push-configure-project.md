
1. In de weergave oplossing (of **Solution Explorer** in Visual Studio), met de rechtermuisknop op de **onderdelen** map, klikt u op **meer onderdelen ophalen...** , zoekt de **Google Cloud Messaging-Client** onderdeel en voeg deze toe aan het project.
2. Open het bestand ToDoActivity.cs-project en voeg de volgende gebruiksinstructie aan de klasse:
   
        using Gcm.Client;
3. In de **ToDoActivity** klasse, voeg de volgende nieuwe code: 
   
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
   
    Hiermee kunt u toegang hebben tot het mobiele client-exemplaar van het serviceproces voor push-handler.
4. Voeg de volgende code naar de **OnCreate** methode na de **MobileServiceClient** wordt gemaakt:
   
       // Set the current instance of TodoActivity.
       instance = this;
   
       // Make sure the GCM client is set up correctly.
       GcmClient.CheckDevice(this);
       GcmClient.CheckManifest(this);
   
       // Register the app for push notifications.
       GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Uw **ToDoActivity** is nu klaar voor het toevoegen van pushmeldingen.


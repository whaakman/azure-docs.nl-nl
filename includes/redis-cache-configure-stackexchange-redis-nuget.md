.NET-toepassingen kunnen de cache-client **StackExchange.Redis** gebruiken. Deze kan in Visual Studio worden geconfigureerd met een NuGet-pakket dat de configuratie van de cacheclienttoepassingen eenvoudiger maakt. 

> [!NOTE]
> Zie de GitHub-pagina [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) en de [documentatie over de cacheclient StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis#documentation) voor meer informatie.
> 
> 

Als u in Visual Studio een clienttoepassing wilt configureren met het NuGet-pakket StackExchange.Redis, klikt u in **Solution Explorer** met de rechtermuisknop op het project en kiest u **Manage NuGet Packages**. 

![Manage NuGet Packages](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Typ in het zoekvak **StackExchange.Redis** of **StackExchange.Redis.StrongName**, selecteer de gewenste versie in de resultaten en klik op **Install**.

> [!NOTE]
> Als u liever een versie van de clientbibliotheek **StackExchange.Redis** gebruikt met een sterke naam, kiest u **StackExchange.Redis.StrongName**. Kies anders **StackExchange.Redis**.
> 
> 

![NuGet-pakket StackExchange.Redis](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

Het NuGet-pakket downloadt de vereiste assembly-verwijzingen voor de clienttoepassing en voegt deze toe om met de cacheclient StackExchange.Redis toegang te krijgen tot de Azure Redis-cache.

> [!NOTE]
> Als u uw project eerder hebt geconfigureerd voor gebruik van StackExchange.Redis, kunt u bij **NuGet Package Manager** controleren op updates voor het pakket. Als u wilt controleren op bijgewerkte versies van het StackExchange.Redis-pakket NuGet en deze updates wilt installeren, klikt u op **Updates** in het venster **NuGet Package Manager**. Als er een update voor het StackExchange.Redis-pakket NuGet beschikbaar is, kunt u uw project bijwerken zodat de bijgewerkte versie wordt gebruikt.
> 
> 



<!--HONumber=Nov16_HO2-->



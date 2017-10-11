De stappen om de registratie van een processerver ongedaan te maken, verschillen afhankelijk van de status van de verbinding met de configuratieserver.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>De registratie ongedaan maken van een processerver die is verbonden

1. Meld u extern bij de processerver aan als beheerder.
2. Start het **Configuratiescherm** en open **Programma's > Een programma verwijderen**
3. Verwijder het programma met de naam **Microsoft Azure Site Recovery-configuratie-/processerver**
4. Nadat stap 3 is voltooid, verwijdert u **Microsoft Azure Site Recovery-configuratie-/processerverafhankelijkheden**

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>De registratie ongedaan maken van een processerver die niet is verbonden

> [!WARNING]
> Volg onderstaande stappen als het niet lukt om de virtuele machine te reactiveren waarop de processerver is geïnstalleerd.

1. Meld u bij uw configuratieserver aan als beheerder.
2. Open een beheeropdrachtprompt en blader naar de map `%ProgramData%\ASR\home\svsystems\bin`.
3. Voer nu de opdracht uit.

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Hiermee worden de gegevens van de processerver van het systeem verwijderd.

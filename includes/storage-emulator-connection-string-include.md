De opslagemulator ondersteunt één vaste account en een bekende verificatiesleutel voor verificatie met gedeelde sleutel. Dit account en de sleutel zijn de enige gedeelde sleutel referenties zijn toegestaan voor gebruik met de opslagemulator. Ze zijn:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> De verificatiesleutel wordt ondersteund door de opslagemulator is alleen bedoeld voor het testen van de functionaliteit van de clientcode voor verificatie. Deze dienen geen elk doeleinde beveiliging. U kunt uw storage-account voor productie en de sleutel niet gebruiken met de opslagemulator. U moet het account ontwikkeling niet gebruiken met productiegegevens.
> 
> De opslagemulator ondersteunt alleen verbinding via HTTP. HTTPS is echter de aanbevolen protocol voor toegang tot resources in een productie-Azure storage-account.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Verbinding maken met de emulator account via een snelkoppeling
De eenvoudigste manier verbinding maken met de opslagemulator van uw toepassing is voor het configureren van een verbindingsreeks in het configuratiebestand van de toepassing die verwijst naar de snelkoppeling `UseDevelopmentStorage=true`. Hier volgt een voorbeeld van een verbindingsreeks voor de opslagemulator in een *app.config* bestand: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Verbinding maken met de emulator-account met behulp van de bekende accountnaam en de sleutel
U moet de eindpunten voor elk van de services die u wilt gebruiken van de emulator in de verbindingsreeks opgeven voor het maken van een verbindingsreeks die verwijst naar de emulator accountnaam en de sleutel. Dit is noodzakelijk om de verbindingsreeks verwijst naar de emulator-eindpunten die anders dan de voor een productie-opslagaccount zijn. Bijvoorbeeld: de waarde van de verbindingsreeks ziet er als volgt:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Deze waarde is gelijk aan de bovenstaande snelkoppeling `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Geef een HTTP-proxy
U kunt ook een HTTP-proxy moet worden gebruikt wanneer u bij het testen van uw service op basis van de opslagemulator opgeven. Dit kan handig zijn voor de naleving van HTTP-aanvragen en antwoorden terwijl u fouten bewerkingen op de storage-services opspoort zijn. Als u een proxy, voeg de `DevelopmentStorageProxyUri` optie de verbindingsreeks en stel de waarde op de proxy-URI. Hier is bijvoorbeeld een verbindingsreeks die verwijst naar de opslagemulator en een HTTP-proxy configureert:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```


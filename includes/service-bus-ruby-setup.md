## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken
Zie voor instructies [een Ruby-toepassing maken op Azure](../articles/virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik Service Bus
Voor het gebruik van Service Bus, downloaden en gebruiken van het pakket Azure Ruby, waaronder een set van gemak bibliotheken die met de storage REST-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems gebruiken om het pakket te verkrijgen
1. Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).
2. Typ 'azure gem installeren' in het opdrachtvenster voor het installeren van de gem en afhankelijkheden.

### <a name="import-the-package"></a>Het pakket importeren
Met behulp van uw favoriete teksteditor, voeg de volgende boven aan het Ruby bestand waarin u van plan bent opslag gebruiken:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Een Service Bus-verbinding instellen
Gebruik de volgende code in te stellen van de waarden van de naamruimte, de naam van de sleutel, key, ondertekenaar en host:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Stel de naamruimtewaarde op de waarde die u hebt gemaakt in plaats van de volledige URL. Bijvoorbeeld: **'yourexamplenamespace'**, niet 'yourexamplenamespace.servicebus.windows.net'.

---
author: spelluru
ms.service: service-bus
ms.topic: include
ms.date: 11/09/2018
ms.author: spelluru
ms.openlocfilehash: 16ce537a54fc77fc0f72b859d6d193501d86c1fc
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52269674"
---
## <a name="create-a-ruby-application"></a>Een Ruby-toepassing maken
Zie voor instructies [een Ruby-toepassing maken op Azure](../articles/virtual-machines/linux/classic/ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing configureren voor gebruik Service Bus
Voor het gebruik van Service Bus, downloaden en gebruiken van het Azure Ruby-pakket bevat een set met gemak bibliotheken die met de storage REST-services communiceren.

### <a name="use-rubygems-to-obtain-the-package"></a>RubyGems gebruiken om het pakket te verkrijgen
1. Gebruik een opdrachtregelinterface zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix).
2. Typ 'azure gem installeren' in het opdrachtvenster om de gem en de afhankelijkheden te installeren.

### <a name="import-the-package"></a>Het pakket importeren
Met behulp van uw favoriete teksteditor, voeg de volgende naar de bovenkant van het Ruby-bestand waarin u van plan bent opslag gebruiken:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Instellen van een Service Bus-verbinding
Gebruik de volgende code om in te stellen de waarden van de naamruimte en naam van de sleutel, sleutel, ondertekend door en host:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Stel de naamruimtewaarde op de waarde die u hebt gemaakt in plaats van de volledige URL. Gebruik bijvoorbeeld **"yourexamplenamespace"**, niet 'yourexamplenamespace.servicebus.windows.net'.

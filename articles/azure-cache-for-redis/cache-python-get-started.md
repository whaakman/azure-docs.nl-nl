---
title: Snelstart voor het maken van een Python-app die gebruikmaakt van Azure Cache voor Redis | Microsoft Docs
description: In deze snelstart leert u een Python-app te maken die gebruikmaakt van Azure Cache voor Redis
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: quickstart
ms.tgt_pltfrm: azure-cache-for-redis
ms.workload: tbd
ms.date: 05/11/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: e9faf30d591cab513695ece1d68704e5bef4ce9c
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53018729"
---
# <a name="quickstart-use-azure-cache-for-redis-with-python"></a>Snelstart: Azure Cache voor Redis met Python gebruiken


## <a name="introduction"></a>Inleiding

Deze snelstart toont hoe u verbinding maakt met een Azure Cache voor Redis-instantie met Python om een cache te lezen en ernaar te schrijven. 

![Python-test voltooid](./media/cache-python-get-started/cache-python-completed.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Python 2- of Python 3-omgeving](https://www.python.org/downloads/) geïnstalleerd met [pip](https://pypi.org/project/pip/). 

## <a name="create-an-azure-cache-for-redis-on-azure"></a>Azure Cache voor Redis maken op Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="install-redis-py"></a>Redis-py installeren

[Redis-py](https://github.com/andymccurdy/redis-py) is een Python-interface voor Azure Cache voor Redis. Gebruik het hulpprogramma voor Python-pakketten, *pip*, om het redis-py-pakket te installeren. 

Het volgende voorbeeld gebruikt *pip3* voor Python3 om het redis-py-pakket te installeren op Windows 10 met behulp van een Visual Studio 2017 Developer-opdrachtprompt die wordt uitgevoerd met verhoogde Administrator-bevoegdheden.

    pip3 install redis

![Redis-py installeren](./media/cache-python-get-started/cache-python-install-redis-py.png)


## <a name="read-and-write-to-the-cache"></a>Lezen en schrijven naar de cache

Voer Python uit en test met behulp van de cache vanaf de opdrachtregel. Vervang `<Your Host Name>` en `<Your Access Key>` door de waarden voor uw Azure Cache voor Redis-instantie. 

```python
>>> import redis
>>> r = redis.StrictRedis(host='<Your Host Name>.redis.cache.windows.net',
        port=6380, db=0, password='<Your Access Key>', ssl=True)
>>> r.set('foo', 'bar')
True
>>> r.get('foo')
b'bar'
```

## <a name="create-a-python-script"></a>Een Python-script maken

Maak een nieuw script-tekstbestand met de naam *PythonApplication1.py*.

Voer het volgende script voor *PythonApplication1.py* uit en sla het bestand op. Dit script zal de toegang tot de cache testen. Vervang `<Your Host Name>` en `<Your Access Key>` door de waarden voor uw Azure Cache voor Redis-instantie. 

```python
import redis

myHostname = "<Your Host Name>.redis.cache.windows.net"
myPassword = "<Your Access Key>"

r = redis.StrictRedis(host=myHostname, port=6380,password=myPassword,ssl=True)

result = r.ping()
print("Ping returned : " + str(result))

result = r.set("Message", "Hello!, The cache is working with Python!")
print("SET Message returned : " + str(result))

result = r.get("Message")
print("GET Message returned : " + result.decode("utf-8"))

result = r.client_list()
print("CLIENT LIST returned : ") 
for c in result:
    print("id : " + c['id'] + ", addr : " + c['addr'])
```

Voer het script uit met behulp van Python.

![Python-test voltooid](./media/cache-python-get-started/cache-python-completed.png)


## <a name="clean-up-resources"></a>Resources opschonen

Als u verder wilt gaan met de volgende zelfstudie, kunt u de resources die in deze snelstart zijn gemaakt behouden en opnieuw gebruiken.

Als u niet verder wilt met de voorbeeldtoepassing uit de snelstart, kunt u de Azure-resources verwijderen die in deze snelstart zijn gemaakt om kosten te voorkomen. 

> [!IMPORTANT]
> Het verwijderen van een resourcegroep kan niet ongedaan worden gemaakt. De resourcegroep en alle bijbehorende resources worden permanent verwijderd. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de resources voor het hosten van dit voorbeeld in een bestaande resourcegroep hebt gemaakt en deze groep ook resources bevat die u wilt behouden, kunt u elke resource afzonderlijk verwijderen via hun respectievelijke blade.
>

Meld u aan bij [Azure Portal](https://portal.azure.com) en klik op **Resourcegroepen**.

Typ de naam van de resourcegroep in het tekstvak **Filteren op naam...**. In de instructies voor dit artikel is een resourcegroep met de naam *TestResources* gebruikt. Klik in de resourcegroep in de lijst met resultaten op **...** en vervolgens op **Resourcegroep verwijderen**.

![Verwijderen](./media/cache-web-app-howto/cache-delete-resource-group.png)

U wordt gevraagd om het verwijderen van de resourcegroep te bevestigen. Typ de naam van de resourcegroep om te bevestigen en klik op **Verwijderen**.

Na enkele ogenblikken worden de resourcegroep en alle resources in de groep verwijderd.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Maak een eenvoudige ASP.NET-web-app die gebruikmaakt van Azure Cache voor Redis.](./cache-web-app-howto.md)



<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png

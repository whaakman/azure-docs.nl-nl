---
title: Ingebouwde Python-installatiekopie configureren in Azure App Service
description: Deze zelfstudie beschrijft opties voor het schrijven en configureren van een Python-toepassing op Azure App Service, met de ingebouwde Python-installatiekopie.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/25/2018
ms.author: astay;cephalin
ms.custom: mvc
ms.openlocfilehash: 9316805993b81e4d2511e833e0cc8f240807a1f9
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228545"
---
# <a name="configure-built-in-python-image-in-azure-app-service-preview"></a>Ingebouwde Python-installatiekopie configureren in Azure App Service (preview)

Dit artikel laat zien hoe u de ingebouwde installatiekopie van Python in [App Service op Linux](app-service-linux-intro.md) kunt configureren om uw Python-toepassingen uit te voeren.

## <a name="python-version"></a>Python-versie

De Python-runtime in de App-service op Linux gebruikt versie `python-3.7.0`.

## <a name="supported-frameworks"></a>Ondersteunde frameworks

Alle webframeworks die voldoen aan de Web Server Gateway Interface (WSGI) en die compatibel zijn met de `python-3.7`-runtime, worden ondersteund.

## <a name="package-management"></a>Pakketbeheer

Tijdens Git-publicatie zoekt de Kudu-engine naar [requirements.txt](https://pip.pypa.io/en/stable/user_guide/#requirements-files) in de hoofdmap van de opslagplaats en installeert de pakketten automatisch in Azure met behulp van `pip`.

Voor het genereren van dit bestand voordat u publiceert, gaat u naar de hoofdmap van de opslagplaats en voer de volgende opdracht uit in uw Python-omgeving:

```bash
pip freeze > requirements.txt
```

## <a name="configure-your-python-app"></a>Uw Python-app configureren

De ingebouwde Python-installatiekopie in de App-service gebruikt de [Gunicorn](http://gunicorn.org/)-server om uw Python toepassing uit te voeren. Gunicorn is een Python WSGI HTTP-server voor UNIX. App Service configureert Gunicorn automatisch voor Django en Flask-projecten.

### <a name="django-app"></a>Django-app

Als u een Django-project publiceert dat een `wsgi.py`-module bevat, roept Azure automatisch Gunicorn aan met behulp van de volgende opdracht:

```bash
gunicorn <path_to_wsgi>
```

### <a name="flask-app"></a>Flask-app

Als u een Flask-app publiceert en het invoerpunt zich in een `application.py`- of `app.py`-module bevindt, roept Azure automatisch Gunicorn aan met een van de volgende opdrachten, respectievelijk:

```bash
gunicorn application:app
```

of 

```bash
gunicorn app:app
```

### <a name="customize-start-up"></a>Opstarten aanpassen

Als u een aangepast invoerpunt voor uw app wilt definiëren, maakt u eerst een _.txt_-bestand met een aangepaste Gunicorn opdracht en plaatst u dit in de hoofdmap van uw project. Als u bijvoorbeeld de server wilt starten met de module _helloworld.py_ en de variabele `app`, maakt u een _startup.txt_ met de volgende inhoud:

```bash
gunicorn helloworld:app
```

Op de pagina [Toepassingsinstellingen](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) kiest u **Python|3.7** als **runtimestack** en geeft u de naam van uw **opstartbestand** uit de vorige stap op. Bijvoorbeeld _startup.txt_.

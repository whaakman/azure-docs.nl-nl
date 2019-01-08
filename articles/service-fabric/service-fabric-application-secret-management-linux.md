---
title: Instellen van een versleutelingscertificaat en geheimen in Azure Service Fabric Linux-clusters versleutelen | Microsoft Docs
description: Informatie over het instellen van een versleutelingscertificaat en geheimen op Linux-clusters te coderen.
services: service-fabric
documentationcenter: .net
author: shsha
manager: ''
editor: ''
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2019
ms.author: shsha
ms.openlocfilehash: 9589d6ea69a2293d592a9e63f2b726f1a620bb9e
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54068975"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-linux-clusters"></a>Instellen van een versleutelingscertificaat en geheimen op Linux-clusters te coderen
In dit artikel laat zien hoe een versleutelingscertificaat instellen en deze gebruiken voor het versleutelen van geheimen op Linux-clusters. Zie voor Windows-clusters, [instellen van een versleutelingscertificaat en geheimen op Windows-clusters versleutelen][secret-management-windows-specific-link].

## <a name="obtain-a-data-encipherment-certificate"></a>Een versleutelingscertificaat gegevens ophalen
Een versleutelingscertificaat gegevens wordt alleen gebruikt voor versleuteling en ontsleuteling van [parameters] [ parameters-link] in van een service Settings.xml en [omgevingsvariabelen] [ environment-variables-link] in ServiceManifest.xml van een service. Het is niet wordt gebruikt voor verificatie of het ondertekenen van gecodeerde tekst. Het certificaat moet voldoen aan de volgende vereisten:

* Het certificaat moet een persoonlijke sleutel bevatten.
* De certificaat-sleutelgebruik gegevenscodering (10) moet bevatten en mag geen Server-verificatie of clientverificatie.

  De volgende opdrachten kunnen bijvoorbeeld worden gebruikt voor het genereren van het vereiste certificaat met behulp van OpenSSL:
  
  ```console
  user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
  user@linux:~$ cat TestCert.prv >> TestCert.pem
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Het certificaat installeren in uw cluster
Het certificaat moet worden geïnstalleerd op elk knooppunt in het cluster onder `/var/lib/sfcerts`. Het gebruikersaccount waarmee de service (sfuser standaard) wordt uitgevoerd **moet leestoegang hebben** naar het geïnstalleerde certificaat (dat wil zeggen, `/var/lib/sfcerts/TestCert.pem` voor het huidige voorbeeld).

## <a name="encrypt-secrets"></a>Geheimen versleutelen
Het volgende codefragment kan worden gebruikt voor het versleutelen van een geheim. Dit fragment versleutelt alleen de waarde; Dit gebeurt **niet** Meld u aan de gecodeerde tekst. **U moet gebruiken** de dezelfde versleutelingscertificaat dat is geïnstalleerd in uw cluster voor het produceren van gecodeerde tekst voor de geheime waarden.

```console
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```
De resulterende tekenreeks met base 64-codering uitvoer naar encrypted.txt bevat zowel de geheime gecodeerde tekst, evenals informatie over het certificaat dat is gebruikt om ze te versleutelen. U kunt controleren de geldigheid ervan ontsleutelt met OpenSSL.
```console
user@linux:$ cat encrypted.txt | base64 -d | openssl smime -decrypt -inform der -inkey TestCert.prv
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [versleutelde geheimen in een toepassing opgeven.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application

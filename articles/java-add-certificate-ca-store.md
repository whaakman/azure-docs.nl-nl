---
title: Een certificaat toevoegen aan het archief voor de CA Java | Microsoft Docs
description: Informatie over het toevoegen van een certificeringsinstantie (CA) van certificaat naar de store Java-CA-certificaat (cacerts) voor Twilio-service of Azure Service Bus.
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: d3699b0a-835c-43fb-844d-9c25344e5cda
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: b6e1a305e19415ab1c4b4c208dac98ad1e2689c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Een certificaat toevoegen aan het archief met Java CA-certificaten
De volgende stappen laten zien hoe een certificeringsinstantie (CA) van certificaat toevoegen aan het certificaatarchief van Java-CA (cacerts). Het voorbeeld gebruikt is voor de CA-certificaat vereist voor de Twilio-service. Informatie later in het onderwerp wordt beschreven hoe het CA-certificaat installeren voor de Azure Service Bus. 

U kunt keytool gebruiken voor het toevoegen van het CA-certificaat voordat uw JDK comprimeren en deze toe te voegen aan uw Azure project **approot** map, of u een taak die voor Azure opstarten die keytool gebruikt voor het toevoegen van het certificaat kan worden uitgevoerd. In dit voorbeeld wordt ervan uitgegaan dat u een CA-certificaat voordat de JDK wordt ingepakt toevoegt. Ook een specifieke CA-certificaat wordt gebruikt in het voorbeeld, maar de stappen voor het verkrijgen van een andere CA-certificaat en u deze importeert in de store cacerts vergelijkbaar zijn.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Een certificaat aan het archief cacerts toevoegen
1. Bij de opdrachtprompt van een beheerder die is ingesteld op uw JDK **jdk\jre\lib\security** map, voert u het volgende om te controleren welke certificaten zijn geïnstalleerd:
   
    `keytool -list -keystore cacerts`
   
    Wordt u gevraagd om het wachtwoord van de store. Is het standaardwachtwoord **changeit**. (Als u wilt het wachtwoord te wijzigen, Zie de documentatie keytool op <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) In dit voorbeeld wordt ervan uitgegaan dat het certificaat met MD5 vingerafdruk 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 niet wordt weergegeven, en die u wilt importeren (dit bepaalde certificaat nodig met de Twilio-API-service).
2. Het certificaat te verkrijgen uit de lijst met certificaten die wordt vermeld op [GeoTrust basiscertificaten](http://www.geotrust.com/resources/root-certificates/). Met de rechtermuisknop op de koppeling voor het certificaat met serienummer 35:DE:F4:CF en opslaan voor de **jdk\jre\lib\security** map. Voor dit voorbeeld is opgeslagen in een bestand met de naam **Equifax\_Secure\_certificaat\_Authority.cer**.
3. Importeer het certificaat via de volgende opdracht:
   
    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`
   
    Als u dit certificaat vertrouwen wordt gevraagd of het certificaat MD5 vingerafdruk 67:CB:9 D heeft: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4, reageren door te typen **y**.
4. Voer de volgende opdracht om te controleren of dat het CA-certificaat zijn geïmporteerd:
   
    `keytool -list -keystore cacerts`
5. De JDK ZIP en toe te voegen aan uw Azure project **approot** map.

Zie voor meer informatie over keytool <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure basiscertificaten
De toepassingen die gebruikmaken van Azure-services (zoals Azure Service Bus) moeten het Baltimore CyberTrust Root-certificaat vertrouwen. (15 April 2013 vanaf Azure begon met het migreren van de globale GTE CyberTrust-hoofdmap naar het Baltimore CyberTrust Root. Deze migratie duurt enkele maanden om te voltooien.)

Het certificaat mogelijk al zijn geïnstalleerd in uw archief cacerts Baltimore dus onthouden om uit te voeren de **keytool-lijst** opdracht eerst om te zien als deze al bestaat.

Als u het Baltimore CyberTrust Root toevoegen moet, heeft het serienummer 02:00:00:b9 en SHA1 vingerafdruk d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Kan worden gedownload vanaf <https://cacert.omniroot.com/bc2025.crt>, opgeslagen in een lokaal bestand met extensie **.cer**, en vervolgens importeert met **keytool** zoals hierboven.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de basiscertificaten gebruikt door Azure [Azure Root Certificate migratie](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Zie voor meer informatie over Java [Azure voor Java-ontwikkelaars](/java/azure).


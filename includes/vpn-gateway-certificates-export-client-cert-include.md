---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b657d54c3ebbe5afc20fc98c1348bb783410df60
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53444088"
---
Wanneer u een clientcertificaat genereert, wordt deze automatisch geïnstalleerd op de computer die u hebt gebruikt voor het genereren. Als u het clientcertificaat installeren op een andere clientcomputer wilt, moet u voor het exporteren van het clientcertificaat dat u hebt gegenereerd.

1. Als u een clientcertificaat wilt exporteren, opent u **Gebruikerscertificaten beheren**. De clientcertificaten die u hebt gegenereerd zijn, zich standaard in 'Certificaten - Huidige gebruiker\Persoonlijk\Certificaten'. Met de rechtermuisknop op het clientcertificaat dat u wilt exporteren, klikt u op **alle taken**, en klik vervolgens op **exporteren** openen de **Wizard Certificaat exporteren**.

   ![Exporteren](./media/vpn-gateway-certificates-export-client-cert-include/export.png)
2. Klik in de Wizard Certificaat exporteren op **volgende** om door te gaan.

   ![Volgende](./media/vpn-gateway-certificates-export-client-cert-include/next.png)
3. Selecteer **Ja, de persoonlijke sleutel exporteren**, en klik vervolgens op **volgende**.

   ![persoonlijke sleutel exporteren](./media/vpn-gateway-certificates-export-client-cert-include/privatekeyexport.png)
4. Laat op de pagina **Bestandsindeling voor export** de standaardinstellingen geselecteerd. Zorg ervoor dat **en mogelijk alle certificaten in het certificeringspad opnemen** is geselecteerd. Deze instelling wordt daarnaast exporteert gegevens van het basiscertificaat dat is vereist voor geslaagde clientverificatie. Zonder deze clientverificatie is mislukt omdat de client beschikt niet over het vertrouwde basiscertificaat. Klik op **Volgende**.

   ![bestandsindeling exporteren](./media/vpn-gateway-certificates-export-client-cert-include/includeallcerts.png)
5. Op de pagina **Beveiliging** moet u de persoonlijke sleutel beveiligen. Als u ervoor kiest om een wachtwoord te gebruiken, is het belangrijk dat u het wachtwoord voor dit certificaat ergens noteert of onthoudt. Klik op **Volgende**.

   ![security](./media/vpn-gateway-certificates-export-client-cert-include/security.png)
6. Op de pagina **Te exporteren bestand** **bladert** u naar de locatie waar u het certificaat wilt exporteren. Geef bij **Bestandsnaam** de naam van het certificaatbestand op. Klik op **Volgende**.

   ![te exporteren bestand](./media/vpn-gateway-certificates-export-client-cert-include/filetoexport.png)
7. Klik op **Voltooien** om het certificaat te exporteren.

   ![voltooien](./media/vpn-gateway-certificates-export-client-cert-include/finish.png)
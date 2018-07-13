---
title: Hoe u bewijs van eigendom voor x.509-CA-certificaten met Azure IoT Hub Device Provisioning Service doet | Microsoft Docs
description: X.509-CA-certificaten met de DPS-service controleren
author: bryanla
ms.author: bryanla
ms.date: 02/26/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 7dd4f8cc53d86933d10294dbbf55157d2b1cf290
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001776"
---
# <a name="how-to-do-proof-of-possession-for-x509-ca-certificates-with-your-device-provisioning-service"></a>Hoe u doet bewijs van eigendom voor x.509-CA-certificaten met de Device Provisioning Service

Een geverifieerde x.509-certificaat (Certificeringsinstantie)-certificaat is een CA-certificaat dat is geüpload en geregistreerd bij uw provisioning-service en bewijs van eigendom met de service is voltooid. 

Bewijs van eigendom omvat de volgende stappen uit:
1. Een unieke verificatiecode die worden gegenereerd door de provisioning-service voor uw X.509-CA-certificaat ophalen. U kunt dit doen vanuit Azure portal.
2. Maken van een X.509-certificaat voor verificatie met de verificatiecode in als het onderwerp en het certificaat met de persoonlijke sleutel die is gekoppeld aan uw X.509-CA-certificaat ondertekent.
3. Upload het ondertekende verificatiecertificaat naar de service. De service beoordeelt het verificatiecertificaat met behulp van het openbare gedeelte van het CA-certificaat om te worden geverifieerd, dus aan te tonen dat u in het bezit is van de persoonlijke sleutel van de CA-certificaat bent.

Geverifieerde certificaten speelt een belangrijke rol bij het gebruik van Registratiegroepen. Certificaat eigendom verifiëren biedt een extra beveiligingslaag door ervoor te zorgen dat de uploader van het certificaat in het bezit is van de persoonlijke sleutel van het certificaat is. Verificatie wordt voorkomen dat een kwaadwillende actor bekijken van uw verkeer van een tussenliggende certificaat ophalen en maken van een registratiegroep zit in hun eigen provisioning-service, met behulp van dat certificaat effectief overname van uw apparaten. Door aan te tonen het eigendom van de hoofdmap of een tussenliggende certificaten in een certificaatketen, bent u aan te tonen dat u gemachtigd bent leaf-certificaten voor de apparaten die zullen worden geregistreerd als een onderdeel van die inschrijvingsgroep te genereren. Voor deze reden, de basis- of tussencertificaat geconfigureerd in een registratiegroep zit lid van een geverifieerd certificaat zijn moet of moet geeft de implementatie van maximaal een geverifieerd certificaat in de certificaatketen een apparaat wanneer deze wordt geverifieerd met de service. Zie voor meer informatie over het registreren van groepen, [X.509-certificaten](concepts-security.md#x509-certificates) en [Apparaattoegang tot de inrichtingsservice met X.509-certificaten](concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

## <a name="register-the-public-part-of-an-x509-certificate-and-get-a-verification-code"></a>Registreren van het openbare deel van een X.509-certificaat en een verificatiecode ophalen

Volg deze stappen voor het registreren van een CA-certificaat met de provisioning-service en een verificatiecode in die u tijdens het bewijs van eigendom gebruiken kunt te halen. 

1. In de Azure-portal, gaat u naar uw provisioning-service en open **certificaten** in het menu links. 
2. Klik op **toevoegen** om toe te voegen een nieuw certificaat.
3. Voer een beschrijvende weergavenaam voor uw certificaat. Blader naar het cer- of .pem-bestand dat het openbare deel van uw X.509-certificaat aangeeft. Klik op **Uploaden**.
4. Wanneer u een melding dat uw certificaat is geüpload, klikt u op **opslaan**.

    ![Certificaat uploaden](./media/how-to-verify-certificates/add-new-cert.png)  

   Uw certificaat wordt weergegeven de **Certificatenverkenner** lijst. Houd er rekening mee dat de **STATUS** van dit certificaat is *niet geverifieerd*.

5. Klik op het certificaat dat u in de vorige stap hebt toegevoegd.

6. In **certificaatgegevens**, klikt u op **verificatiecode genereren**.

7. De inrichtingsservice maakt een **verificatiecode** die u gebruiken kunt voor het valideren van het eigendom van het certificaat. Kopieer de code naar het Klembord. 

   ![Certificaat controleren](./media/how-to-verify-certificates/verify-cert.png)  

## <a name="digitally-sign-the-verification-code-to-create-a-verification-certificate"></a>De verificatiecode te maken van een verificatiecertificaat digitaal te ondertekenen

Nu moet u zich aanmelden de *verificatiecode* met de persoonlijke sleutel die is gekoppeld aan uw X.509-CA-certificaat, dat een handtekening wordt gegenereerd. Dit staat bekend als [bewijs van eigendom](https://tools.ietf.org/html/rfc5280#section-3.1) en de resultaten in een ondertekende verificatiecertificaat.

Microsoft biedt hulpprogramma's en voorbeelden waarmee u kunnen een ondertekende verificatiecertificaat maken: 

- De **C-SDK van Azure IoT Hub** biedt PowerShell (Windows) en (Linux) Bash-scripts voor het maken van de CA en leaf-certificaten voor het ontwikkelen en uit te voeren van bewijs van eigendom met behulp van een verificatiecode. U kunt downloaden de [bestanden](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) relevant zijn voor uw systeem naar een werkmap en volg de instructies in de [beheren CA certificaten Leesmij](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) bewijs van bezit uitvoeren op een CA-certificaat. 
- De **Azure IoT Hub C# SDK** bevat de [voorbeeld van verificatie](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples/GroupCertificateVerificationSample), die u kunt gebruiken om te doen bewijs van eigendom.
 
> [!IMPORTANT]
> Naast het uitvoeren van bewijs van eigendom, kunnen de eerder genoemde ook PowerShell en Bash-scripts u basiscertificaten en tussenliggende certificaten leaf-certificaten die kunnen worden gebruikt om te verifiëren en inrichten van apparaten te maken. Deze certificaten moeten worden gebruikt voor het ontwikkelen van alleen. Ze moeten nooit worden gebruikt in een productieomgeving. 

Afhankelijk van de PowerShell en Bash-scripts die zijn opgegeven in de documentatie en SDK's [OpenSSL](https://www.openssl.org/). U kunt ook OpenSSL of andere hulpprogramma's van derden kunt u doen bewijs van eigendom. Zie voor meer informatie over hulpprogramma's die zijn opgegeven met de SDK's, [over het gebruik van hulpprogramma's in de SDK's](how-to-use-sdk-tools.md). 


## <a name="upload-the-signed-verification-certificate"></a>Upload het ondertekende verificatiecertificaat

1. De resulterende handtekening uploaden als een verificatiecertificaat voor aan uw provisioning-service in de portal. In **certificaatgegevens** op de Azure-portal, gebruikt u de _Verkenner_ pictogram naast de **verificatiecertificaat PEM- of cer-bestand** veld voor het uploaden van de ondertekende het verificatiecertificaat van uw systeem.

2. Nadat het certificaat is geüpload, klikt u op **controleren**. De **STATUS** van uw certificaat wijzigingen **_Verified_** in de **Certificatenverkenner** lijst. Klik op **vernieuwen** als deze wordt niet automatisch bijgewerkt.

   ![Certificaatcontrole uploaden](./media/how-to-verify-certificates/upload-cert-verification.png)  

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het gebruik van de portal voor het maken van een registratiegroep zit, [apparaatinschrijvingen met Azure portal beheren](how-to-manage-enrollments.md).
- Zie voor meer informatie over het gebruik van de service-SDK's te maken van een registratiegroep zit, [apparaatinschrijvingen met service-SDK's beheren](how-to-manage-enrollments-sdks.md).











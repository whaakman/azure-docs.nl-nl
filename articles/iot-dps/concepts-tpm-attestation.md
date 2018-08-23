---
title: Azure IoT Hub Device Provisioning Service - TPM-Attestation
description: In dit artikel biedt een conceptueel overzicht van de TPM-attestation-stroom met behulp van IoT Device Provisioning Service.
author: nberdy
ms.author: nberdy
ms.date: 04/23/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: cb763327eb292feb9d58fb21b1ca808a3f2909aa
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42055116"
---
# <a name="tpm-attestation"></a>TPM-attestation

IoT Hub Device Provisioning Service is een helper-service voor IoT Hub die u met zero-touch-apparaatinrichting voor een opgegeven IoT hub configureren. Met de Device Provisioning Service, kunt u miljoenen apparaten inrichten op een veilige manier.

In dit artikel beschrijft de attestation-proces identiteit bij het gebruik van een [TPM](./concepts-device.md). TPM staat voor Trusted Platform Module en is een hardware security module (HSM). In dit artikel wordt ervan uitgegaan dat u gebruikmaakt van een aparte, firmware of TPM geïntegreerd. Software geëmuleerde TPM's zijn zeer geschikt voor het ontwikkelen van prototypen of testen, maar ze bieden hetzelfde niveau van beveiliging als discrete, firmware of geïntegreerde TPM's doen. We raden niet met behulp van software TPM's in de productieomgeving. Zie voor meer informatie over de typen van TPM's [een korte inleiding op TPM](http://trustedcomputinggroup.org/wp-content/uploads/TPM-2.0-A-Brief-Introduction.pdf).

In dit artikel is alleen relevant voor apparaten met behulp van TPM 2.0 en belangrijke ondersteuning voor HMAC en hun endorsement sleutels. Het is niet voor apparaten die gebruikmaken van X.509-certificaten voor verificatie. TPM is een sectorbreed, ISO-norm voor de Trusted Computing Group en vindt u meer over de TPM op de [volledige TPM 2.0-specificatie](https://trustedcomputinggroup.org/tpm-library-specification/) of de [ISO/IEC 11889 spec](https://www.iso.org/standard/66510.html). In dit artikel ook wordt ervan uitgegaan dat u bekend bent met openbare en persoonlijke sleutelparen en hoe ze worden gebruikt voor versleuteling.

Het apparaat van de Device Provisioning Service met de SDK's verwerken alles wat u in dit artikel wordt beschreven. Er is niet nodig voor u extra implementeren als u de SDK's op uw apparaten. In dit artikel helpt u conceptueel gezien begrijpen wat er gebeurt met uw TPM-beveiligingschip als de bepalingen van uw apparaat en waarom dit zo beveiligt.

## <a name="overview"></a>Overzicht

TPM's gebruik zoiets als de goedkeuringssleutel (EK) als de beveiligde vertrouwensbasis. De EK is uniek is voor de TPM en het apparaat in feite wijzigen verandert in een nieuw wachtwoord.

Er is een ander type sleutel dat TPM's hebt, met de naam de opslaghoofdsleutel (SRK). Een SRK kan worden gegenereerd door de eigenaar van de TPM nadat het eigendom van de TPM duurt. Eigenaar worden van de TPM is de TPM-specifieke manier van de uitspraak 'iemand Hiermee stelt u een wachtwoord op de HSM." Als een TPM-apparaat aan een nieuwe eigenaar wordt verkocht, kan de eigenaar van de nieuwe eigenaar van de TPM voor het genereren van een nieuwe SRK kunt uitvoeren. De nieuwe generatie voor SRK zorgt ervoor dat de vorige eigenaar de TPM niet gebruiken. Omdat de SRK uniek is voor de eigenaar van de TPM is, kan de SRK het verzegelen van gegevens in de TPM zelf voor de eigenaar van die worden gebruikt. De SRK biedt een sandbox voor de eigenaar voor het opslaan van de sleutels en biedt toegang tot revocability als het apparaat of de TPM wordt verkocht. Het is alsof u verplaatst naar een nieuwe huis: eigenaar is wijzigen van de vergrendelingen op de deuren en vernietigen van alle meubels links van de vorige eigenaars (SRK), maar u kunt het adres van het huis (EK) niet wijzigen.

Wanneer een apparaat is ingesteld en klaar voor gebruik, is er een EK zowel een SRK beschikbaar voor gebruik.

![Eigenaar worden van een TPM](./media/concepts-tpm-attestation/tpm-ownership.png)

Een opmerking op de eigenaar van de TPM: eigendom van een TPM is afhankelijk van veel factoren, met inbegrip van TPM-fabrikant, de set met TPM-hulpprogramma's wordt gebruikt en het besturingssysteem van het apparaat. Volg de instructies die relevant zijn voor uw systeem om eigenaar te worden.

De Device Provisioning Service maakt gebruik van het openbare deel van de EK (EK_pub) om te bepalen en -apparaten inschrijven. Leverancier van het apparaat kan de EK_pub lezen tijdens de productie- of hierop de laatste testen en de EK_pub uploaden naar de inrichtingsservice, zodat het apparaat wordt herkend wanneer verbinding wordt gemaakt om in te richten. De Device Provisioning Service controleert niet of de SRK of de eigenaar, dus 'wissen' van de TPM wist u de gegevens van de klant, maar de EK (en andere leveranciersgegevens) behouden blijven en het apparaat wordt nog steeds worden herkend door de Device Provisioning Service wanneer deze verbinding om in te richten maakt.

## <a name="detailed-attestation-process"></a>Gedetailleerde attestation-proces

Wanneer een apparaat met een TPM is eerst verbinding met de Device Provisioning Service maakt, wordt eerst de opgegeven EK_pub tegen de EK_pub die zijn opgeslagen in de lijst inschrijving in de service controleert. Als de EK_pubs niet overeenkomen, wordt het apparaat is niet toegestaan om in te richten. Als de EK_pubs overeen komen, moet de service vervolgens het apparaat om te bewijzen dat eigendom van het persoonlijke gedeelte van de EK via een nonce uitdaging, dit is een veilige uitdaging gebruikt om identiteit te bewijzen. De Device Provisioning Service genereert een nonce en versleutelt deze met de SRK en vervolgens de EK_pub, die beide worden geleverd door het apparaat tijdens de registratie-aanroep. De TPM beveiligt altijd het persoonlijke gedeelte van de EK. Dit voorkomt dat vervalsing en zorgt ervoor dat de SAS-tokens veilig zijn ingericht met geautoriseerde apparaten.

Laten we eens het attestation-proces in detail.

### <a name="device-requests-an-iot-hub-assignment"></a>De toewijzing van een IoT Hub-apparaat aanvragen

Eerst wordt het apparaat verbinding maakt met de Device Provisioning Service en aanvragen om in te richten. In dat geval, biedt het apparaat de service met de registratie-ID, een ID-bereik en de EK_pub en SRK_pub van de TPM. De service wordt de gecodeerde nonce terug naar het apparaat doorgegeven en wordt u gevraagd het apparaat de nonce ontsleutelen en gebruiken om te ondertekenen van een SAS-token naar opnieuw verbinding te maken en het inrichten voltooien.

![Apparaataanvragen inrichten](./media/concepts-tpm-attestation/step-one-request-provisioning.png)

### <a name="nonce-challenge"></a>Nonce challenge

Het apparaat de nonce neemt en de persoonlijke gedeelten van de EK en SRK gebruikt voor het ontsleutelen van de nonce in de TPM; de volgorde van de nonce versleuteling delegeert vertrouwensrelatie van de EK, onveranderbaar is, aan de SRK, welke wijzigen kunt als een nieuwe eigenaar eigenaar van de TPM wordt.

![De nonce decoderen](./media/concepts-tpm-attestation/step-two-nonce.png)

### <a name="validate-the-nonce-and-receive-credentials"></a>De nonce valideren en te ontvangen van referenties

Het apparaat vervolgens Meld u aan een SAS-token met behulp van de ontsleutelde nonce en herstellen van een verbinding met de Device Provisioning Service met behulp van de ondertekende SAS-token. Met de uitdaging Nonce is voltooid, kan de service het apparaat om in te richten.

![Apparaat herstelt u verbinding met de Device Provisioning Service voor het valideren van de eigenaar van de EK](./media/concepts-tpm-attestation/step-three-validation.png)

## <a name="next-steps"></a>Volgende stappen

Nu het apparaat verbinding maakt met IoT Hub en u de rest-beveiligd in de wetenschap dat uw apparaten met sleutels veilig worden opgeslagen. Nu dat u hoe de Device Provisioning Service veilig controleert of TPM met behulp van een apparaat-id weet, bekijkt u de volgende artikelen voor meer informatie:

* [Meer informatie over de concepten in automatische inrichting](./concepts-auto-provisioning.md)
* [Aan de slag met behulp van automatische inrichting](./quick-setup-auto-provision.md) met de SDK's om te zorgen voor van de stroom.

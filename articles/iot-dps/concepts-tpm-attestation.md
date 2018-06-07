---
title: Azure IoT Hub apparaat-Service - TPM Attestation inrichten
description: Dit artikel bevat een conceptueel overzicht van de TPM attestation-stroom met IoT Device inrichtingsservice.
author: nberdy
ms.author: nberdy
ms.date: 04/23/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 90f41e56f8e95584959576d3e5ad837f4774048a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629079"
---
# <a name="tpm-attestation"></a>TPM-attestation

IoT Hub apparaat-inrichtingsservice is een helper-service voor IoT-Hub die u gebruikt om zonder tussenkomst apparaat inrichten met een opgegeven IoT-hub te configureren. U kunt met de Service apparaat inrichten miljoenen apparaten inrichten op een veilige manier.

In dit artikel beschrijft het proces van de attestation identiteit bij gebruik van een [TPM](./concepts-device.md). TPM staat voor de Trusted Platform Module en is een soort hardware security module (HSM). In dit artikel wordt ervan uitgegaan dat u gebruikt een discrete, firmware of TPM geïntegreerd. Software geëmuleerde TPM's zijn geschikt voor het maken van een prototype of testen, maar ze doen geen op hetzelfde niveau van beveiliging als discrete, firmware of geïntegreerde TPM's doen. We raden niet met behulp van software TPM's in de productieomgeving. [Meer informatie](http://trustedcomputinggroup.org/wp-content/uploads/TPM-2.0-A-Brief-Introduction.pdf) over de typen TPM's.

In dit artikel is alleen relevant voor apparaten met TPM 2.0 HMAC sleutel ondersteuning en de sleutels goedkeurt. Het is niet voor apparaten met behulp van X.509-certificaten voor verificatie. TPM is een algemeen geaccepteerde, ISO-standaard van de Trusted Computing Group en vindt u meer over TPM op de [voltooid TPM 2.0-specificatie](https://trustedcomputinggroup.org/tpm-library-specification/) of de [ISO/IEC 11889 spec](https://www.iso.org/standard/66510.html). In dit artikel ook wordt ervan uitgegaan dat u bekend bent met paren van openbare en persoonlijke sleutels en hoe ze worden gebruikt voor versleuteling.

Het apparaat inrichtingsservice apparaat-SDK's ingang alles wat u in dit artikel wordt beschreven. Er is niet nodig voor u extra implementeren als u de SDK's op uw apparaten. In dit artikel helpt u conceptueel begrijpen wat er gebeurt met de TPM-beveiligingschip bij het beveiligen van de bepalingen van uw apparaat en waarom het geval is.

## <a name="overview"></a>Overzicht

TPM's gebruiken zoiets als de goedkeuringssleutel (EK) als de beveiligde hoofdmap van de vertrouwensrelatie. De EK is uniek voor de TPM en het apparaat in wezen wijzigen verandert in een nieuwe.

Er is een ander type sleutel dat TPM's de opslaghoofdsleutel (SRK) hebt, aangeroepen. Een SRK kan worden gegenereerd door de TPM-eigenaar nadat het eigendom van de TPM duurt. Eigenaar worden van de TPM is de TPM-specifieke manier van de melding 'iemand stelt een wachtwoord in voor de HSM." Als een TPM-apparaat aan een nieuwe eigenaar wordt verkocht, kan de nieuwe eigenaar eigendom van de TPM voor het genereren van een nieuwe SRK duren. De nieuwe generatie van SRK zorgt ervoor dat de TPM kan niet worden gebruikt in de vorige eigenaar. Omdat de SRK uniek is voor de eigenaar van de TPM is, kan de SRK verzegelen van gegevens in de TPM zelf voor die eigenaar worden gebruikt. De SRK biedt een sandbox voor de eigenaar voor het opslaan van de sleutels en biedt toegang tot revocability als het apparaat of de TPM wordt verkocht. Het is net verplaatsen naar een nieuw huis: eigenaar is het wijzigen van de vergrendelingen op de deuren en alle meubelen links van de vorige eigenaars (SRK) vernietigen, maar u het adres van het huis (EK) niet wijzigen.

Wanneer een apparaat is ingesteld en klaar voor gebruik, is er een EK zowel een SRK beschikbaar voor gebruik.

![Eigenaar worden van een TPM](./media/concepts-tpm-attestation/tpm-ownership.png)

Een opmerking over de eigenaar van de TPM: eigendom van een TPM is afhankelijk van veel factoren, met inbegrip van TPM-fabrikant, de set TPM's wordt gebruikt en het besturingssysteem van het apparaat. Volg de instructies die relevant zijn voor uw systeem eigenaar.

De Service voor het inrichten van apparaten maakt gebruik van het openbare deel van de EK (EK_pub) om te bepalen en apparaten inschrijven. Leverancier van het apparaat kan de EK_pub lezen tijdens de productie of de laatste test en de EK_pub uploaden naar de inrichting service, zodat het apparaat wordt herkend wanneer deze verbinding om in te richten maakt. De Service voor het inrichten van apparaten wordt niet gecontroleerd de SRK of de eigenaar, zodat het 'wissen' van de TPM worden gewist klantgegevens, maar de EK (en andere leveranciersgegevens) behouden blijven en het apparaat wordt nog steeds worden herkend door de Service voor het inrichten van apparaten wanneer deze verbinding om in te richten maakt.

## <a name="detailed-attestation-process"></a>Gedetailleerde attestation-proces

Wanneer een apparaat met een TPM is eerst verbinding met de Service voor het inrichten van apparaten maakt, controleert de service eerst de opgegeven EK_pub tegen de EK_pub opgeslagen in de lijst van de inschrijving. Als de EK_pubs niet overeenkomen, wordt het apparaat is niet toegestaan om in te richten. Als de EK_pubs overeenkomen's, moet de service vervolgens het apparaat om te bewijzen eigendom van het persoonlijke gedeelte van de EK via een nonce uitdaging is een beveiligde uitdaging gebruikt om identiteit te bewijzen. De Service voor het inrichten van apparaten genereert een nonce en versleutelt dit met de SRK en vervolgens de EK_pub, die beide zijn geleverd door het apparaat tijdens de registratie van de eerste aanroep. De TPM houdt altijd het persoonlijke gedeelte van de EK beveiligd. Dit voorkomt dat vervalsing en garandeert een SAS-tokens veilig zijn ingericht voor geautoriseerde apparaten.

Laten we helpt u stapsgewijs door het proces attestation in detail.

### <a name="device-requests-an-iot-hub-assignment"></a>Apparaat aanvraagt een IoT Hub-toewijzing

Eerst het apparaat verbinding maakt met de Service voor het inrichten van apparaten en aanvragen om in te richten. Zo biedt het apparaat de service met de registratie-ID, een ID-bereik en de EK_pub en SRK_pub van de TPM. De service de versleutelde nonce terug naar het apparaat is geslaagd en wordt u gevraagd het apparaat gebruiken voor het ondertekenen van een SAS-token opnieuw verbinding te maken en klaar bent met het inrichten en de nonce ontsleuteld.

![Apparaataanvragen inrichten](./media/concepts-tpm-attestation/step-one-request-provisioning.png)

### <a name="nonce-challenge"></a>Nonce uitdaging

Het apparaat neemt de nonce en de persoonlijke gedeelten van de EK en SRK gebruikt voor het ontsleutelen van de nonce in de TPM; de volgorde van de nonce versleuteling delegeert vertrouwensrelatie van de EK, onveranderbaar is, aan de SRK, wijzigen kunt als een nieuwe eigenaar eigenaar van de TPM wordt.

![De nonce ontsleutelen](./media/concepts-tpm-attestation/step-two-nonce.png)

### <a name="validate-the-nonce-and-receive-credentials"></a>Valideren van de nonce en referenties ontvangen

Het apparaat kan vervolgens een SAS-token met behulp van de ontsleutelde nonce melden en opnieuw verbinding te maken met de apparaat-inrichting-Service met behulp van de ondertekende SAS-token. De service kunnen met de uitdaging Nonce is voltooid, het apparaat in te richten.

![Apparaat herstelt u verbinding met DP's voor het valideren EK eigenaar](./media/concepts-tpm-attestation/step-three-validation.png)

## <a name="next-steps"></a>Volgende stappen

Nu het apparaat verbinding maakt met IoT Hub en u plaatst u beveiligd in de wetenschap dat uw apparaten sleutels veilig worden opgeslagen. Nu dat u hoe de Service voor het inrichten van apparaten veilig controleert of een apparaat-id met TPM weet, Raadpleeg de volgende artikelen voor meer informatie:

* [Meer informatie over de concepten automatisch inrichten](./concepts-auto-provisioning.md)
* [Aan de slag met automatische inrichting](./quick-setup-auto-provision.md) zorgt voor de stroom met de SDK's.

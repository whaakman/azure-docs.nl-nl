---
title: Verbinding maken met 3270 apps op IBM-mainframes met Azure - Azure Logic Apps
description: Integreren en automatiseren 3270 scherm gebaseerde apps met Azure met behulp van Azure Logic Apps en IBM 3270 connector
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: estfan, valthom
ms.topic: article
ms.date: 03/06/2019
tags: connectors
ms.openlocfilehash: 7388dc0c61dad9c31da0c178febcee4c8481bc50
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58313189"
---
# <a name="integrate-3270-screen-driven-apps-on-ibm-mainframes-with-azure-by-using-azure-logic-apps-and-ibm-3270-connector"></a>3270 scherm gebaseerde apps voor IBM-mainframes integreren met Azure met behulp van Azure Logic Apps en IBM 3270 connector

> [!NOTE]
> Deze connector bevindt zich in [ *openbare preview*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

U kunt met Azure Logic Apps en de connector voor IBM 3270, toegang en uitvoeren van IBM-mainframes apps waarmee u meestal uitbreiden door te navigeren door 3270 emulator schermen. Op die manier kunt u uw IBM mainframe-toepassingen met Azure, Microsoft, en andere apps, services en systemen integreren door geautomatiseerde werkstromen maken met Azure Logic Apps. De connector communiceert met IBM-mainframes via het protocol TN3270 en is beschikbaar in alle Azure Logic Apps-regio's behalve Azure Government en Azure China 21Vianet. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

In dit artikel beschrijft deze aspecten voor het gebruik van de connector 3270: 

* Waarom gebruikt de IBM 3270-connector in Azure Logic Apps en hoe de connector wordt uitgevoerd 3270 scherm gebaseerde apps

* De vereisten en instellingen voor het gebruik van de connector 3270

* De stappen voor het 3270 connectoracties toevoegen aan uw logische app

## <a name="why-use-this-connector"></a>Waarom deze connector gebruiken?

Voor toegang tot apps van IBM-mainframes, gebruikt u meestal een 3270 terminal emulator, vaak aangeduid als een "groen"scherm. Deze methode is een betrouwbare manier, maar heeft beperkingen. Hoewel HIS Host Integration Server () helpt die u bij het gebruik met deze apps rechtstreeks soms scheiden van het scherm en bedrijfslogica is niet mogelijk. Of misschien niet meer hebt u informatie over de werking van de hosting van toepassingen.

Om uit te breiden deze scenario's, de connector voor IBM 3270 in Azure Logic Apps werkt met het hulpprogramma voor het ontwerpen van 3270, waarmee u met de record of 'capture', de schermen van de host gebruikt voor een specifieke taak, de navigatie-stroom voor die taak door uw mainframe-app te definiëren en definieert de methoden met parameters voor invoer en uitvoer voor die taak. Het hulpprogramma ontwerp zet om die informatie in metagegevens die de connector 3270 wordt gebruikt bij het aanroepen van een actie die staat voor die taak van uw logische app.

Nadat u het bestand met metagegevens van het ontwerpprogramma gegenereerd, kunt u dat bestand toevoegen aan een integratieaccount in Azure. Op die manier kunnen uw logische app hebben toegang tot uw app metagegevens wanneer u een actie van de connector 3270 toevoegen. De connector leest het bestand met metagegevens van uw integratie-account, navigatie door de 3270 schermen worden verwerkt en dynamisch geeft de parameters voor de connector 3270 actie. U kunt vervolgens gegevens naar de hosttoepassing opgeven en de connector retourneert de resultaten aan uw logische app. Op die manier kunt u uw verouderde apps integreren met Azure, Microsoft, en andere apps, services en systemen die ondersteuning biedt voor Azure Logic Apps.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Aanbevolen: Een [integratieserviceomgeving (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) 

  U kunt deze omgeving selecteren als de locatie voor het maken en uitvoeren van uw logische app. Een ISE kunt openen vanuit uw logische app aan resources die worden beveiligd in virtuele netwerken van Azure.

* De logische app te gebruiken voor het automatiseren en uitvoeren van uw app 3270 scherm gebaseerde

  De connector voor IBM 3270 geen triggers, Gebruik daarom een andere trigger om te beginnen uw logische app, zoals de **terugkeerpatroon** trigger. U kunt vervolgens 3270 connectoracties toevoegen. Aan de slag [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 
  Als u een ISE gebruikt, selecteert u die ISE als locatie voor uw logische app.

* [Download en installeer het hulpprogramma voor het ontwerp van 3270](https://aka.ms/3270-design-tool-download).
De enige vereiste is [Microsoft .NET Framework 4.6.1](https://aka.ms/net-framework-download).

  Dit hulpprogramma kunt u de schermen, navigatiepaden, methoden en parameters voor de taken in uw app, die u toevoegt en run as-3270 connectoracties opnemen. Het hulpprogramma genereert een Host Integration Designer XML (HIDX)-bestand dat de vereiste metagegevens voor de connector te gebruiken voor het aansturen van uw mainframe-app biedt.
  
  Na het downloaden en installeren van dit hulpprogramma, volg deze stappen voor het verbinden met uw host:

  1. Open het hulpprogramma voor het ontwerpen van 3270. Uit de **sessie** in het menu **hostsessies**.
  
  1. Geef uw host TN3270 servergegevens.

* Een [integratieaccount](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), dit is de plaats waar u uw HIDX-bestand opslaan als een kaart, zodat uw logische app toegang heeft tot de definities van metagegevens en methode in dat bestand. 

  Zorg ervoor dat uw integratieaccount is gekoppeld aan de logische app die u gebruikt. Als u een ISE gebruikt, zorg er ook dat de locatie van uw integratie-account is de dezelfde ISE die gebruikmaakt van uw logische app.

* Toegang tot de TN3270-server die als host fungeert voor uw mainframe-app

<a name="define-app-metadata"></a>

## <a name="create-metadata-overview"></a>Overzicht van metagegevens maken

In een app 3270 scherm gebaseerde zijn de schermen en gegevensvelden uniek voor uw scenario's, zodat de connector 3270 moet u deze informatie over uw app, die u als metagegevens opgeven kunt. Deze metagegevens worden gegevens beschreven die helpt uw logische app te identificeren en te herkennen schermen, wordt beschreven hoe u tussen schermen navigeren waar de voor het invoeren van gegevens en waar u kunt verwachten van resultaten. Als u wilt opgeven en deze metagegevens genereren, die u gebruikt het hulpprogramma voor het ontwerpen van 3270, die u bij deze specifieke helpt *modi*, of fasen, zoals verderop in gedetailleerder beschreven:

* **Vastleggen**: In deze modus te registreren, de schermen die vereist zijn voor het uitvoeren van een specifieke taak met uw mainframe-app, bijvoorbeeld aan een Banksaldo.

* **Navigatie**: In deze modus geeft u het plan of het pad voor het navigeren door uw mainframe-app-schermen voor de specifieke taak.

* **methoden**: In deze modus, definieert u de methode, bijvoorbeeld `GetBalance`, met de beschrijving van het navigatiepad scherm. U wordt ook de velden op elk scherm die geworden van de methode invoer en uitvoer van parameters.

### <a name="unsupported-elements"></a>Niet-ondersteunde elementen

Het hulpprogramma ontwerp biedt geen ondersteuning voor deze elementen:

* Gedeeltelijke IBM eenvoudige toewijzing ondersteuning (BMS) kaarten: Als u een kaart BMS importeert, negeert de ontwerpomgeving gedeeltelijke scherm definities.
* In/uit-parameters: U kunt geen In/uit-parameters definiëren.
* Menu verwerken: Tijdens de Preview-versie ondersteund niet
* Verwerken van de matrix: Tijdens de Preview-versie ondersteund niet

<a name="capture-screens"></a>

## <a name="capture-screens"></a>Schermen vastleggen

In deze modus kunt markeren u een item op elk scherm 3270 die een unieke identificatie van dit scherm. U kunt bijvoorbeeld een regel tekst of een complexere set voorwaarden, zoals specifieke tekst en een niet-lege veld opgeven. U kunt deze schermen over een live-verbinding met de host-server registreren of u deze gegevens importeren uit een IBM eenvoudige toewijzing ondersteuning (BMS)-kaart. De live-verbinding gebruikt een emulator TN3270 om verbinding te maken met de host. De actie voor elke connector moet worden toegewezen aan een enkele taak die verbinding kunt maken met uw sessie begint en eindigt met de sessie verbreken.

1. Als u niet hebt gedaan, opent u de ontwerpomgeving 3270. Kies op de werkbalk **vastleggen** zodat u de modus voor vastleggen opgeven.

1. Druk op F5, om te starten op te nemen, of vanuit de **op te nemen** in het menu **starten op te nemen**. 

1. Uit de **sessie** in het menu **Connect**.

1. In de **vastleggen** deelvenster vanaf het eerste scherm in uw app, stap voor stap door uw app voor de specifieke taak die u opneemt.

1. Nadat u de taak, meld u af van uw app in zoals u dat gewoonlijk doet.

1. Uit de **sessie** in het menu **verbinding verbreken**.

1. Opname stoppen, drukt u op Shift + F5 drukken, of vanuit de **op te nemen** in het menu **opname stoppen**.

   Nadat u de schermen voor een taak vastlegt, geeft de ontwerpfunctie hulpprogramma miniatuurweergaven die staan voor deze schermen. Sommige opmerkingen over deze miniaturen:

   * Opgenomen met de vastgelegde schermen, hebt u een scherm met de naam "Leeg".

     Wanneer u eerst verbinding maken met [CICS](https://www.ibm.com/it-infrastructure/z/cics), moet u de 'Lege' sleutel verzenden voordat u de naam voor de transactie die u wilt uitvoeren kunt invoeren. Het scherm waar u de 'Lege' sleutel verzendt geen *erkenning kenmerken*, zoals een schermtitel, die u toevoegen kunt met behulp van het scherm opname-editor. Voor dit scherm bevat de miniatuur van een scherm met de naam 'Lege'. U kunt dit scherm later gebruiken voor de weergave van het scherm waarin u de transactienaam van de invoeren.

   * De naam van een vastgelegde scherm gebruikt standaard het eerste woord in het scherm. Als deze naam al bestaat, voegt het hulpprogramma ontwerp de naam met een onderstrepingsteken en een nummer, bijvoorbeeld 'WBGB' en 'WBGB_1'.

1. Als u wilt een zinvollere naam geven aan een vastgelegde scherm, de volgende stappen uit:

   1. In de **Host schermen** deelvenster, selecteert u het scherm dat u wilt wijzigen.

   1. Zoek in hetzelfde deelvenster onderaan in het deelvenster met dezelfde de **schermnaam** eigenschap.

   1. Wijzig de schermnaam van het huidige in een meer beschrijvende naam.

1. Geef nu de velden voor het identificeren van elk scherm.

   Met de 3270 gegevensstroom hebt schermen geen standaard-id's, dus u moet unieke tekst op elk scherm selecteren. Voor complexe scenario's, kunt u meerdere voorwaarden, bijvoorbeeld, unieke tekst en een veld met een bepaalde voorwaarde opgeven.

Nadat u de opname-velden te selecteren, verplaatsen naar de volgende modus.

### <a name="conditions-for-identifying-repeated-screens"></a>Voorwaarden voor het identificeren van herhaalde schermen

Voor de connector om te navigeren en onderscheid maken tussen schermen, u gewoonlijk unieke tekst zoeken op een scherm dat u als een id van de vastgelegde schermen gebruiken kunt. Voor herhaalde schermen moet u mogelijk meer identificatiemethoden. Stel bijvoorbeeld dat u hebt twee schermen die zien er hetzelfde, behalve één scherm resulteert in een geldige waarde, terwijl het andere scherm een foutbericht weergegeven wordt.

In de ontwerp-hulpprogramma, kunt u toevoegen *erkenning kenmerken*, bijvoorbeeld een schermtitel zoals 'Opvragen saldo', met behulp van het scherm opname-editor. Als u een Gevorkte pad hebt en beide vertakkingen hetzelfde scherm retourneren, maar met verschillende resultaten, moet u andere kenmerken voor de opname. De connector maakt tijdens de uitvoering gebruik van deze kenmerken voor het bepalen van de current branch- en fork. Dit zijn de voorwaarden die u kunt gebruiken:

* Specifieke waarde: Deze waarde komt overeen met de opgegeven tekenreeks die op de opgegeven locatie.
* Specifieke waarde: Deze waarde komt niet overeen met de opgegeven tekenreeks die op de opgegeven locatie.
* Leeg: Dit veld is leeg.
* Mag niet leeg zijn: Dit veld is niet leeg zijn.

Zie voor meer informatie, de [voorbeeld navigatie plan](#example-plan) verderop in dit onderwerp.

<a name="define-navigation"></a>

## <a name="define-navigation-plans"></a>Navigatie abonnementen definiëren

In deze modus definieert u de stroom of de stappen voor het navigeren door uw mainframe-app-schermen voor uw specifieke taak. Bijvoorbeeld, soms mogelijk hebt u meer dan één pad die uw app uitvoeren kunt, waarbij één pad produceert het juiste resultaat, terwijl de andere pad, een fout treedt. Geef voor elk scherm de toetsaanslagen die nodig zijn voor het verplaatsen naar het volgende scherm, zoals `CICSPROD <enter>`.

> [!TIP]
> Als u verschillende taken die gebruikmaken van de dezelfde verbinding maken en verbinding verbreken schermen automatiseren bent, biedt de ontwerpomgeving speciale verbinden en verbinding verbreken plantypen. Wanneer u deze abonnementen definieert, kunt u ze toevoegen aan het begin en einde van uw navigatie-abonnement.

### <a name="guidelines-for-plan-definitions"></a>Richtlijnen voor het plan definities

* Zijn alle schermen, beginnend met de verbinding te maken en eindigend met de verbinding wordt verbroken.

* U kunt een zelfstandig abonnement maken of de abonnementen verbinden en verbinding verbreken, zodat u een reeks schermen gebruikelijk voor alle transacties opnieuw kunt gebruiken.

  * Het laatste scherm in uw Connect-abonnement moet hetzelfde scherm als het eerste scherm in uw plan navigatie.

  * Het eerste scherm in uw plan verbinding verbreken moet hetzelfde scherm als het laatste scherm in uw plan navigatie.

* De vastgelegde schermen mogelijk bevatten veel herhaalde schermen, dus u moet selecteren en slechts één exemplaar van de herhaalde schermen in uw plan gebruiken. Hier volgen enkele voorbeelden van herhaalde schermen:

  * Het teken in het scherm, bijvoorbeeld de **MSG 10** scherm
  * Het welkomstscherm wordt weergegeven voor CICS
  * De "wissen" of **leeg** scherm

<a name="create-plans"></a>

### <a name="create-plans"></a>Maak plannen

1. Kies op de werkbalk van het 3270 ontwerp hulpprogramma **navigatie** zodat u de navigatie-modus opgeven.

1. Voor het eerst in uw abonnement, de **navigatie** deelvenster Kies **nieuw Plan**.

1. Onder **naam nieuwe Plan kiezen**, voer een naam voor uw abonnement. Uit de **Type** , selecteert u het type:

   | Type abonnement wijzigen | Description |
   |-----------|-------------|
   | **Proces** | Voor zelfstandige of gecombineerde plannen |
   | **Verbinding maken** | Verbinding maken met plannen |
   | **Verbinding verbreken** | Voor verbinding verbreken plannen |
   |||

1. Uit de **Host schermen** deelvenster, Sleep de vastgelegde miniaturen op het plan navigatie surface in de **navigatie** deelvenster.

   Om weer te geven de leeg scherm waar u de transactienaam van de invoert, gebruikt u het scherm 'Lege'.

1. Rangschik de schermen in de volgorde waarin de taak die u definieert.

1. Kies voor het definiëren van het pad van de stroom tussen schermen, met inbegrip van splitsingen en joins, op de werkbalk van het ontwerpprogramma, **stroom**.

1. Kies het eerste scherm in de stroom. Sleep en tekenen van een verbinding naar het volgende scherm in de stroom.

1. Voor elk scherm, geef de waarden voor de **steun sleutel** eigenschap (aandacht Identifier) en voor de **vaste tekst** eigenschap, die de stroom naar het volgende scherm wordt verplaatst.

   Mogelijk hebt u alleen de steun-sleutel, of zowel de steun sleutel en vaste tekst.

Nadat u uw navigatie-abonnement hebt, kunt u [methoden definiëren in de volgende modus](#define-method).

<a name="example-plan"></a>

### <a name="example"></a>Voorbeeld

Stel dat u een CICS transactie met de naam 'WBGB"met deze stappen uitvoeren in dit voorbeeld: 

* Op het eerste scherm voert u een naam en een account.
* In het tweede scherm krijgt u het saldo.
* U sluit af naar het scherm 'Lege'.
* U zich afmelden bij CICS naar het scherm "MSG-10".

Ook Stel dat u deze stappen herhalen, maar u onjuiste gegevens invoeren, zodat u het scherm waarin de fout kunt vastleggen. Hier volgen de schermen die u vastleggen:

* MSG-10
* CICS Welkom
* Leeg
* WBGB_1 (input)
* WBGB_2 (error)
* Empty_1
* MSG-10_1

Hoewel veel schermen hier u unieke namen, zijn sommige schermen hetzelfde scherm, bijvoorbeeld "MSG-10" en "Leeg". Voor een herhaalde scherm, slechts één exemplaar voor dit scherm te gebruiken in uw abonnement. Hier volgen enkele voorbeelden die laten zien hoe een zelfstandig abonnement, verbinding maken met abonnement, verbinding verbreken plan en een gecombineerde plan kunnen er volgt uitzien:

* Standalone abonnement

  ![Zelfstandig navigatie-abonnement](./media/connectors-create-api-3270/standalone-plan.png)

* Verbinding maken met abonnement

  ![Verbinding maken met abonnement](./media/connectors-create-api-3270/connect-plan.png)

* Plan verbreken

  ![Plan verbreken](./media/connectors-create-api-3270/disconnect-plan.png)

* Gecombineerde plan

  ![Gecombineerde plan](./media/connectors-create-api-3270/combined-plan.png)

#### <a name="example-identify-repeated-screens"></a>Voorbeeld: Herhaalde schermen identificeren

Voor de connector om te navigeren en onderscheid maken tussen schermen, vindt u unieke tekst meestal op een scherm dat u als een id voor de vastgelegde schermen gebruiken kunt. Voor herhaalde schermen moet u mogelijk meer identificatiemethoden. De voorbeeld-plan heeft een fork waar u schermen die er ongeveer als volgt kunt krijgen. Een scherm retourneert een saldo, terwijl het andere scherm een foutbericht weergegeven wordt.

Het hulpprogramma ontwerp kunt u erkenning kenmerken toevoegen, bijvoorbeeld een schermtitel met de naam 'Ophalen saldo', met behulp van het scherm erkenning editor. In het geval is met vergelijkbare schermen moet u andere kenmerken. De connector maakt tijdens de uitvoering gebruik van deze kenmerken voor het bepalen van de vertakking en fork.

* U kunt een veld met een 'niet-lege' voorwaarde toevoegen in de vertakking die geldige invoer, die het scherm met het saldo is, retourneert.

* In de vertakking die met een fout retourneert, kunt u een veld met een 'lege' voorwaarde toevoegen.

<a name="define-method"></a>

## <a name="define-methods"></a>Methoden definiëren

In deze modus definieert u een methode die is gekoppeld aan uw abonnement navigatie. Voor elke methodeparameter die u kunt het gegevenstype, zoals een tekenreeks, geheel getal, datum of tijd opgeven en enzovoort. Wanneer u klaar bent, kunt u de methode op de live-host testen en controleren of de methode werkt zoals verwacht. U vervolgens het bestand met metagegevens of Host Integration Designer XML (HIDX)-bestand, dat nu is de methodedefinities te gebruiken voor het maken en uitvoeren van een actie voor de connector voor IBM 3270 genereren.

1. Kies op de werkbalk van het 3270 ontwerp hulpprogramma **methoden** zodat u de methoden modus. 

1. In de **navigatie** deelvenster, selecteert u het scherm met de gewenste velden.

1. Als u wilt toevoegen de eerste invoerparameter voor de methode, de volgende stappen uit:

   1. In de **vastleggen** in het deelvenster op het scherm van de emulator 3270 Kies het hele veld, niet alleen tekst in het veld dat u wilt dat als eerste invoer.

      > [!TIP]
      > Voor het weergeven van alle velden en zorg ervoor dat u selecteert het veld voltooid op de **weergave** in het menu **alle velden**.

   1. Kies op de werkbalk van het ontwerpprogramma, **invoer veld**. 

   Als u wilt meer invoerparameters toevoegen, herhaalt u de vorige stappen voor elke parameter.

1. Als u wilt toevoegen de eerste uitvoerparameter voor de methode, de volgende stappen uit:

   1. In de **vastleggen** in het deelvenster op het scherm van de emulator 3270 Kies het hele veld, niet alleen tekst in het veld dat u wilt dat als de eerste uitvoer.

      > [!TIP]
      > Voor het weergeven van alle velden en zorg ervoor dat u selecteert het veld voltooid op de **weergave** in het menu **alle velden**.

   1. Kies op de werkbalk van het ontwerpprogramma, **uitvoerveld**.

   Herhaal de vorige stappen voor elke parameter om toe te voegen meer uitvoerparameters.

1. Nadat u de parameters van de methode hebt toegevoegd, moet u deze eigenschappen voor elke parameter definiëren:

   | Naam van eigenschap | Mogelijke waarden | 
   |---------------|-----------------|
   | **Gegevenstype** | Datum en tijd, Decimal, Int, byte lang, kort gezegd, tekenreeks |
   | **Veld opvulling techniek** | Parameters ondersteuning van deze typen opvulling, vullen met lege waarden, indien nodig: <p><p>- **Type**: Sequentieel tekens invoeren in het veld. <p>- **Fill**: Inhoud van het veld vervangen door tekens, vullen met lege waarden indien nodig. <p>- **EraseEofType**: Wis het veld en geef de tekens sequentieel worden verwerkt in het veld. |
   | **Notatietekenreeks** | Sommige gegevenstypes parameter gebruikt een opmaaktekenreeks, die u informeert de 3270 connector tekst converteren van het scherm in een .NET-gegevenstype: <p><p>- **Datum-/**: De tekenreeks voor de datum/tijd-indeling volgt de [.NET aangepaste datum en tijd opmaaktekenreeksen](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings). Bijvoorbeeld, de datum `06/30/2019` maakt gebruik van de notatietekenreeks `MM/dd/yyyy`. <p>- **Decimaal**: Maakt gebruik van de tekenreeks voor notatie decimaal de [COBOL afbeelding component](https://www.ibm.com/support/knowledgecenter/SS6SG3_5.2.0/com.ibm.cobol52.ent.doc/PGandLR/ref/rlddepic.html). Bijvoorbeeld, het aantal `100.35` maakt gebruik van de notatietekenreeks `999V99`. |
   |||

## <a name="save-and-view-metadata"></a>Opslaan en weergeven van metagegevens

Nadat u uw methode hebt gedefinieerd, maar voordat u uw methode testen, moet u alle gegevens die u tot nu toe hebt gedefinieerd in een bestand RAP (.rap) opslaan.
U bespaart tot dit bestand RAP op elk gewenst moment tijdens een modus. Het hulpprogramma ontwerp bevat ook een voorbeeldbestand RAP's die u kunt openen en bekijken door te bladeren naar de installatiemap van de ontwerp-hulpprogramma op deze locatie en het bestand 'WoodgroveBank.rap' te openen:

`..\Program Files\Microsoft Host Integration Server - 3270 Design Tool\SDK\WoodgroveBank.rap`

Als u opslaan van wijzigingen in het voorbeeldbestand RAP of het genereren van een bestand HIDX uit het voorbeeldbestand RAP probeert terwijl het bestand in de ontwerpomgeving installatiemap zich bevindt blijft, kunt u een fout 'toegang geweigerd' krijgen. Standaard wordt het ontwerp-hulpprogramma geïnstalleerd in de map Program Files zonder verhoogde bevoegdheden. Als er een fout optreedt, probeert u een van deze oplossingen:

* Het voorbeeldbestand kopiëren naar een andere locatie.
* Het hulpprogramma ontwerp uitvoeren als beheerder.
* De eigenaar van de SDK-map zelf maken.

## <a name="test-your-method"></a>De methode testen

1. Voor het uitvoeren van de methode op basis van de live-host, terwijl u nog in de modus van de methoden, druk op F5 of via de werkbalk van het ontwerpprogramma, kies **uitvoeren**.

   > [!TIP]
   > U kunt de modi op elk gewenst moment wijzigen. Op de **bestand** in het menu **modus**, en selecteer vervolgens de gewenste modus.

1. Voer uw parameterwaarden in en kies **OK**.

1. Kies het volgende om door te gaan naar het volgende scherm **volgende**.

1. Wanneer u klaar bent, kiest u **gedaan**, uw uitvoerparameterwaarden laat zien.

<a name="add-metadata-integration-account"></a>

## <a name="generate-and-upload-hidx-file"></a>Genereren en HIDX-bestand uploaden

Wanneer u klaar bent, genereert het bestand HIDX, zodat u kunt uploaden naar uw integratie-account. Het ontwerp 3270 hulpprogramma maakt de HIDX-bestand in een nieuwe submap waar u uw RAP-bestand hebt opgeslagen.

1. Kies op de werkbalk van het 3270 ontwerp hulpprogramma **Code genereren**.

1. Ga naar de map waarin uw bestand RAP en open de submap die het hulpprogramma na het genereren van uw HIDX-bestand gemaakt. Controleer of het hulpprogramma voor het bestand HIDX gemaakt.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en zoek uw integratie-account.

1. Uw HIDX-bestand als een kaart toevoegen aan uw integratie-account door [uitvoeren van deze gelijksoortige stappen voor het toevoegen van maps](../logic-apps/logic-apps-enterprise-integration-liquid-transform.md), maar wanneer u het kaarttype selecteren, selecteert u **HIDX**.

Verderop in dit onderwerp als u een actie IBM 3270 aan uw logische app voor de eerste keer toevoegen u wordt gevraagd om u te maken van een verbinding tussen uw logische app en de host-server door te geven van de verbindingsinformatie in, zoals de naam voor uw integratie-account en host-server . Nadat u de verbinding hebt gemaakt, kunt u uw eerder toegevoegde HIDX-bestand, de methode om uit te voeren en de parameters die u wilt gebruiken.

Wanneer u klaar bent met al deze stappen, kunt u de actie die u in uw logische app maakt voor het verbinden met uw IBM-mainframes, station schermen voor uw app wordt gebruikt, -gegevens invoeren, resultaten worden geretourneerd, enzovoort. U kunt ook andere acties toe te voegen aan uw logische app voor het integreren met andere apps, services en systemen.

<a name="run-action"></a>

## <a name="run-ibm-3270-actions"></a>IBM 3270 acties uitvoeren

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies onder de laatste stap waar u een actie toevoegen, **nieuwe stap**, en selecteer **een actie toevoegen**. 

1. Kies onder het zoekvak **Enterprise**. Typ '3270' als filter in het zoekvak. Selecteer deze actie uit de lijst met acties: **Een mainframe-programma wordt uitgevoerd via een verbinding TN3270**

   ![3270 actie selecteren](./media/connectors-create-api-3270/select-3270-action.png)

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Als er geen verbinding nog niet bestaat, geef de benodigde gegevens voor uw verbinding en kies **maken**.

   | Eigenschap | Vereist | Value | Description |
   |----------|----------|-------|-------------|
   | **Verbindingsnaam** | Ja | <*naam van de verbinding*> | De naam voor de verbinding |
   | **Integratie-Account-ID** | Ja | <*integration-account-name*> | De naam van uw integratie-account |
   | **Integratie-Account-SAS-URL** | Ja | <*integration-account-SAS-URL*> | Uw integratie-account Shared Access Signature (SAS)-URL, die u op basis van de instellingen van uw integratieaccount in Azure portal genereren kunt. <p>1. Op uw integratie-account in het menu onder **instellingen**, selecteer **URL voor terugbellen**. <br>2. Kopieer in het rechterdeelvenster de **URL voor terugbellen voor gegenereerd** waarde. |
   | **Server** | Ja | <*TN3270-server-name*> | De naam van de server voor uw service TN3270 |
   | **Poort** | Nee | <*TN3270-server-port*> | De poort die wordt gebruikt door uw TN3270-server. Als dit veld leeg blijft, wordt de connector gebruikt `23` als de standaardwaarde. |
   | **Apparaattype** | Nee | <*IBM-terminal-model*> | De modelnaam of het getal voor de IBM-terminal te emuleren. Als dit veld leeg blijft, wordt door de connector standaardwaarden gebruikt. |
   | **Codetabel** | Nee | <*code-page-number*> | Het codepaginanummer voor de host. Als dit veld leeg blijft, wordt de connector gebruikt `37` als de standaardwaarde. |
   | **Naam van logische eenheid** | Nee | <*logical-unit-name*> | De naam van de specifieke logische eenheid aan te vragen van de host |
   | **SSL inschakelen?** | Nee | In- of uitschakelen | Op of SSL-versleuteling uitschakelen. |
   | **Ssl-certificaat voor host valideren?** | Nee | In- of uitschakelen | Inschakelen of uitschakelen van validatie van het certificaat van de server. |
   ||||

   Bijvoorbeeld:

   ![Verbindingseigenschappen](./media/connectors-create-api-3270/connection-properties.png)

1. Geef de benodigde gegevens voor de actie:

   | Eigenschap | Vereist | Value | Description |
   |----------|----------|-------|-------------|
   | **De naam van de Hidx** | Ja | <*HIDX-bestandsnaam*> | Selecteer het 3270 HIDX-bestand dat u wilt gebruiken. |
   | **Methodenaam** | Ja | <*Methodenaam*> | Selecteer de methode in het HIDX-bestand dat u wilt gebruiken. Nadat u een methode selecteren de **toevoegen van nieuwe parameter** er wordt een lijst weergegeven zodat u de parameters voor gebruik met deze methode kunt selecteren. |
   ||||

   Bijvoorbeeld:

   **Selecteer het bestand HIDX**

   ![Selecteer HIDX bestand](./media/connectors-create-api-3270/select-hidx-file.png)

   **Selecteer de methode**

   ![Een methode selecteren](./media/connectors-create-api-3270/select-method.png)

   **Selecteer de volgende parameters**

   ![Selecteer parameters](./media/connectors-create-api-3270/add-parameters.png)

1. Wanneer u klaar bent, opslaan en uw logische app uitvoeren.

   Nadat uw logische app uitgevoerd is, de stappen van de uitvoering weer te geven. 
   Geslaagde stappen leert vinkjes, terwijl de mislukte stappen laten zien de letter 'X'.

1. Als u wilt controleren van de invoer en uitvoer voor elke stap, vouwt u deze stap.

1. Als u wilt de uitvoer bekijken, kiest u **onbewerkte uitvoer Zie**.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties beperken die worden beschreven van de connector openapi (voorheen Swagger) beschrijving, Controleer de [van de connector-verwijzingspagina](/connectors/si3270/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.

* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)

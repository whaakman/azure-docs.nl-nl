---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: d8e249a788d80108d46a61f316b795ab04e541a2
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203475"
---
## <a name="setting-up-your-ios-application"></a>Instellen van uw iOS-toepassing

In deze sectie bevat stapsgewijze instructies voor het maken van een nieuw project te laten zien hoe een toepassing voor iOS (Swift) integreren met *aanmelden met Microsoft* , zodat de Web-API's waarvoor een token op te vragen.

> Voorkeur voor het downloaden van dit voorbeeld XCode-project in plaats daarvan? [Een project hebt gedownload](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) en gaat u naar de [configuratiestap](#register-your-application) het codevoorbeeld configureren voordat u uitvoert.

## <a name="install-carthage-to-download-and-build-msal"></a>Carthage om te downloaden en bouwen van MSAL installeren

Carthage package manager wordt gebruikt tijdens de preview-periode van MSAL – het kan worden geïntegreerd met XCode behoud van de mogelijkheid voor Microsoft wijzigingen aanbrengen in de bibliotheek.

- Download en installeer de nieuwste versie van Carthage [hier](https://github.com/Carthage/Carthage/releases "Carthage download-URL").

## <a name="creating-your-application"></a>Het maken van uw toepassing

1. Xcode openen en selecteer **maken van een nieuwe Xcode-project**.
2. Selecteer **iOS > één weergave toepassing** en selecteer **volgende**.
3. Geef een productnaam en selecteer **volgende**.
4. Selecteer een map te maken van uw app en op *maken*

## <a name="build-the-msal-framework"></a>De MSAL-Framework bouwen

Volg de instructies hieronder voor het ophalen en bouw vervolgens de meest recente versie van MSAL bibliotheken met behulp van Carthage:

1. Open de bash-terminal en Ga naar de hoofdmap van de app.
2. Kopieer de onderstaande en plak deze in de bash-terminal te maken van een bestand 'Cartfile':

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Kopieer en plak de onderstaande. Met deze opdracht haalt afhankelijkheden in een map Carthage/Checkouts en bouwt vervolgens de MSAL-bibliotheek:
</li>
</ol>

```bash
carthage update
```

> De bovenstaande procedure wordt gebruikt om te downloaden en bouwen van de Microsoft Authentication Library (MSAL). MSAL verwerkt ophalen, opslaan in cache en gebruikt voor toegang tot API's die zijn beveiligd door de Azure Active Directory v2.0 gebruikerstokens te vernieuwen.

## <a name="add-the-msal-framework-to-your-application"></a>De MSAL-framework toevoegen aan uw toepassing

1. Open in Xcode, de **algemene** tabblad.
2. Ga naar de **Linked Frameworks and Libraries** sectie en selecteer **+**.
3. Selecteer **toevoegen...** .
4. Selecteer **Carthage > bouwen > iOS > MSAL.framework** en selecteer vervolgens **Open**. U ziet `MSAL.framework` toegevoegd aan de lijst.
5. Ga naar de **Build Phases** tabblad de **+** pictogram en selecteer vervolgens **nieuwe uitgevoerd Script fase**.
6. Voeg de volgende inhoud in de *gebied script*:

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Het volgende toevoegen aan <code>Input Files</code> door te klikken op <code>+</code>:
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a>Het maken van de gebruikersinterface van uw toepassing

Een bestand Main.storyboard moet automatisch worden gemaakt als onderdeel van de projectsjabloon, maken. Volg de onderstaande instructies om de app UI te maken:

1.  Beheren en klikt u op `Main.storyboard` het contextmenu, en klik vervolgens op: `Open As` > `Source Code`
2.  Vervang de `<scenes>` knooppunt met de volgende code:

```xml
 <scenes>
    <scene sceneID="tne-QT-ifu">
        <objects>
            <viewController id="BYZ-38-t0r" customClass="ViewController" customModule="MSALiOS" customModuleProvider="target" sceneMemberID="viewController">
                <layoutGuides>
                    <viewControllerLayoutGuide type="top" id="y3c-jy-aDJ"/>
                    <viewControllerLayoutGuide type="bottom" id="wfy-db-euE"/>
                </layoutGuides>
                <view key="view" contentMode="scaleToFill" id="8bC-Xf-vdC">
                    <rect key="frame" x="0.0" y="0.0" width="375" height="667"/>
                    <autoresizingMask key="autoresizingMask" widthSizable="YES" heightSizable="YES"/>
                    <subviews>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Microsoft Authentication Library" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="ifd-fu-zjm">
                            <rect key="frame" x="64" y="28" width="246" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Logging" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="98g-dc-BPL">
                            <rect key="frame" x="16" y="277" width="62" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="2rX-Vv-T1i">
                            <rect key="frame" x="87" y="100" width="200" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Call Microsoft Graph API"/>
                            <connections>
                                <action selector="callGraphButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="Kx0-JL-Bv9"/>
                            </connections>
                        </button>
                        <textView clipsSubviews="YES" multipleTouchEnabled="YES" contentMode="scaleToFill" fixedFrame="YES" editable="NO" textAlignment="natural" selectable="NO" translatesAutoresizingMaskIntoConstraints="NO" id="qXW-2z-J7K">
                            <rect key="frame" x="16" y="324" width="343" height="291"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <color key="backgroundColor" white="1" alpha="1" colorSpace="calibratedWhite"/>
                            <accessibility key="accessibilityConfiguration">
                                <accessibilityTraits key="traits" updatesFrequently="YES"/>
                            </accessibility>
                            <fontDescription key="fontDescription" type="system" pointSize="14"/>
                            <textInputTraits key="textInputTraits" autocapitalizationType="sentences"/>
                        </textView>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="9u4-b8-vmX">
                            <rect key="frame" x="137" y="138" width="100" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Sign Out"/>
                            <connections>
                                <action selector="signoutButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="kZT-P8-0Zy"/>
                            </connections>
                        </button>
                    </subviews>
                    <color key="backgroundColor" red="1" green="1" blue="1" alpha="1" colorSpace="custom" customColorSpace="sRGB"/>
                </view>
                <connections>
                    <outlet property="loggingText" destination="qXW-2z-J7K" id="uqO-Yw-AsK"/>
                    <outlet property="signoutButton" destination="9u4-b8-vmX" id="OCh-qk-ldv"/>
                </connections>
            </viewController>
            <placeholder placeholderIdentifier="IBFirstResponder" id="dkx-z0-nzr" sceneMemberID="firstResponder"/>
        </objects>
        <point key="canvasLocation" x="140" y="137.18140929535232"/>
    </scene>
</scenes>
```

---
title: TensorFlow model uitvoeren in cognitieve Python - Service aangepaste visie - Azure-Services | Microsoft Docs
description: TensorFlow model uitvoeren in Python
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/17/2018
ms.author: areddish
ms.openlocfilehash: d31036404604104ca28328b6c8bc5d3ca74d83ea
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345759"
---
# <a name="run-tensorflow-model-in-python"></a>TensorFlow model uitvoeren in Python

Nadat u hebt [geëxporteerd van uw model TensorFlow](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model) van de Service van de visie aangepaste deze snelstartgids wordt beschreven hoe u dit model lokaal gebruiken voor het classificeren van afbeeldingen.

## <a name="install-required-components"></a>Vereiste onderdelen installeren

### <a name="prerequisites"></a>Vereisten

Als u wilt gebruiken de zelfstudie, moet u het volgende doen:

- Python 2.7 + of Python 3.5 + installeren.
- Pip installeren.

U moet ook de volgende pakketten installeren:

```
pip install tensorflow
pip install pillow
pip install numpy
pip install opencv-python
```

## <a name="load-your-model-and-tags"></a>Laden van uw model en labels

Het gedownloade zip-bestand bevat een model.pb en een labels.txt. Deze bestanden vertegenwoordigen het getrainde model en de classificatielabels. De eerste stap is het laden van het model in uw project.

```Python
import tensorflow as tf
import os

graph_def = tf.GraphDef()
labels = []

# Import the TF graph
with tf.gfile.FastGFile(filename, 'rb') as f:
    graph_def.ParseFromString(f.read())
    tf.import_graph_def(graph_def, name='')

# Create a list of labels.
with open(labels_filename, 'rt') as lf:
    for l in lf:
        labels.append(l.strip())
```

## <a name="prepare-an-image-for-prediction"></a>Installatiekopie van een voor de prognose voorbereiden

Er zijn een paar stappen voor het voorbereiden van de installatiekopie, zodat deze de juiste vorm voor de prognose. Deze stappen volgens de afbeeldingsbewerking uitgevoerd tijdens de training:

### <a name="open-the-file-and-create-an-image-in-the-bgr-color-space"></a>Open het bestand en een installatiekopie maken in de ruimte van de kleur BGR

```Python
from PIL import Image
import numpy as np
import cv2

# Load from a file
imageFile = "<path to your image file>"
image = Image.open(imageFile)

# Update orientation based on EXIF tags, if the file has orientation info.
image = update_orientation(image)

# Convert to OpenCV format
image = convert_to_opencv(image)
```

### <a name="deal-with-images-with-a-dimension-1600"></a>Omgaan met afbeeldingen met een dimensie > 1600

```Python
# If the image has either w or h greater than 1600 we resize it down respecting
# aspect ratio such that the largest dimension is 1600
image = resize_down_to_1600_max_dim(image)
```

### <a name="crop-the-largest-center-square"></a>Het grootste center kwadraat bijsnijden

```Python
# We next get the largest center square
h, w = image.shape[:2]
min_dim = min(w,h)
max_square_image = crop_center(image, min_dim, min_dim)
```

### <a name="resize-down-to-256x256"></a>Vergroten of verkleinen naar beneden 256 x 256

```Python
# Resize that square down to 256x256
augmented_image = resize_to_256_square(max_square_image)
```


### <a name="crop-the-center-for-the-specific-input-size-for-the-model"></a>Het center voor de grootte van de specifieke invoer voor het model bijsnijden

```Python
# The compact models have a network size of 227x227, the model requires this size.
network_input_size = 227

# Crop the center for the specified network_input_Size
augmented_image = crop_center(augmented_image, network_input_size, network_input_size)

```

De bovenstaande stappen de volgende helperfuncties gebruiken:

```Python
def convert_to_opencv(image):
    # RGB -> BGR conversion is performed as well.
    r,g,b = np.array(image).T
    opencv_image = np.array([b,g,r]).transpose()
    return opencv_image

def crop_center(img,cropx,cropy):
    h, w = img.shape[:2]
    startx = w//2-(cropx//2)
    starty = h//2-(cropy//2)
    return img[starty:starty+cropy, startx:startx+cropx]

def resize_down_to_1600_max_dim(image):
    h, w = image.shape[:2]
    if (h < 1600 and w < 1600):
        return image

    new_size = (1600 * w // h, 1600) if (h > w) else (1600, 1600 * h // w)
    return cv2.resize(image, new_size, interpolation = cv2.INTER_LINEAR)

def resize_to_256_square(image):
    h, w = image.shape[:2]
    return cv2.resize(image, (256, 256), interpolation = cv2.INTER_LINEAR)

def update_orientation(image):
    exif_orientation_tag = 0x0112
    if hasattr(image, '_getexif'):
        exif = image._getexif()
        if (exif != None and exif_orientation_tag in exif):
            orientation = exif.get(exif_orientation_tag, 1)
            # orientation is 1 based, shift to zero based and flip/transpose based on 0-based values
            orientation -= 1
            if orientation >= 4:
                image = image.transpose(Image.TRANSPOSE)
            if orientation == 2 or orientation == 3 or orientation == 6 or orientation == 7:
                image = image.transpose(Image.FLIP_TOP_BOTTOM)
            if orientation == 1 or orientation == 2 or orientation == 5 or orientation == 6:
                image = image.transpose(Image.FLIP_LEFT_RIGHT)
    return image
```

## <a name="predict-an-image"></a>Een installatiekopie van het voorspellen

Zodra de installatiekopie is bedoeld als een tensor kunnen we het verzenden via het model voor een voorspelling:

```Python

# These names are part of the model and cannot be changed.
output_layer = 'loss:0'
input_node = 'Placeholder:0'

with tf.Session() as sess:
    prob_tensor = sess.graph.get_tensor_by_name(output_layer)
    predictions, = sess.run(prob_tensor, {input_node: [augmented_image] })
```

## <a name="view-the-results"></a>De resultaten bekijken

De resultaten van de afbeelding tensor uitgevoerd door het model moet vervolgens terug naar de labels worden toegewezen.

```Python
    # Print the highest probability label
    highest_probability_index = np.argmax(predictions)
    print('Classified as: ' + labels[highest_probability_index])
    print()

    # Or you can print out all of the results mapping labels to probabilities.
    label_index = 0
    for p in predictions:
        truncated_probablity = np.float64(round(p,8))
        print (labels[label_index], truncated_probablity)
        label_index += 1
```
## <a name="next-steps"></a>Volgende stappen

U kunt ook het model verpakt in een mobiele toepassing:
* [Gebruik uw geëxporteerde Tensorflow-model in een Android-toepassing](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample)
* [Gebruik uw geëxporteerde CoreML-model in een Swift iOS-toepassing](https://go.microsoft.com/fwlink/?linkid=857726)
* [Gebruik uw geëxporteerde CoreML-model in een toepassing met Xamarin voor iOS](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel)


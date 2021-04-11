---
title: Entidades con nombre para la asistencia sanitaria
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/11/2021
ms.author: aahi
ms.openlocfilehash: 805c726d33f2050f6f2797c0689069aa5ec4ee71
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104599312"
---
[Text Analytics for Health](../../how-tos/text-analytics-for-health.md) procesa y extrae información de datos médicos no estructurados. El servicio detecta y expone conceptos médicos, asigna aserciones a los conceptos, deduce las relaciones semánticas entre los conceptos y las vincula a ontologías médicas comunes.

Text Analytics for Health detecta conceptos médicos en las siguientes categorías. En esta versión preliminar solo se admite texto en inglés y solo hay disponible una única versión del modelo.

| Category  | Descripción  |
|---------|---------|
| [ANATOMÍA](#anatomy) | conceptos que capturan información sobre el cuerpo y los sistemas anatómicos, los sitios, las ubicaciones o las regiones. |
 | [DATOS DEMOGRÁFICOS](#demographics) | conceptos que capturan información sobre el sexo y la edad. |
 | [EXAMEN MÉDICO](#examinations) | conceptos que capturan información acerca de los procedimientos de diagnóstico y las pruebas. |
 | [ATRIBUTOS GENERALES](#general-attributes) | conceptos que proporcionan más información sobre otros conceptos de las categorías anteriores. |
 | [GENÓMICA](#genomics) | conceptos que capturan información sobre los genes y las variantes. |
 | [ATENCIÓN SANITARIA](#healthcare) | conceptos que capturan información sobre eventos administrativos, entornos sanitarios y profesiones sanitarias. |
 | [ENFERMEDADES](#medical-condition) | conceptos que capturan información sobre diagnósticos, síntomas o signos. |
 | [MEDICACIÓN](#medication) | conceptos que capturan información sobre la medicación, incluidos los nombres de la misma, sus clases, la dosis y la forma de administración. |
 | [SOCIAL](#social) | conceptos que capturan información sobre aspectos sociales de interés médico, como la relación de la familia. |
 | [TRATAMIENTO](#treatment) | conceptos que capturan información acerca de los procedimientos terapéuticos. |

Encontrará más información y ejemplos a continuación.

## <a name="anatomy"></a>Anatomía

### <a name="entities"></a>Entidades

**BODY_STRUCTURE**: sistemas del cuerpo, ubicaciones o regiones anatómicas y sitios de cuerpo. Por ejemplo, brazo, rodilla, abdomen, nariz, hígado, cabeza, sistema respiratorio, linfocitos.

:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure.png" alt-text="Ejemplo de la entidad de la estructura del cuerpo.":::


:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure-2.png" alt-text="Ejemplo expandido de la entidad de la estructura del cuerpo.":::

## <a name="demographics"></a>Demografía

### <a name="entities"></a>Entidades

**AGE**: todos los términos y las frases de edad, incluidos los de pacientes, familiares y otros usuarios. Por ejemplo, 40 años, 51 años, 3 meses, adulto, lactante, anciano, joven, menor de edad, mediana edad.

:::image type="content" source="../../media/ta-for-health/age-entity.png" alt-text="Ejemplo de una entidad de edad.":::

:::image type="content" source="../../media/ta-for-health/age-entity-2.png" alt-text="Otro ejemplo de una entidad de edad.":::


**GENDER**: términos que divulgan el sexo del sujeto. Por ejemplo, masculino, femenino, mujer, señor, señora.

:::image type="content" source="../../media/ta-for-health/gender-entity.png" alt-text="Ejemplo de una entidad de sexo.":::

## <a name="examinations"></a>Exámenes médicos

### <a name="entities"></a>Entidades

**EXAMINATION_NAME**: procedimientos y pruebas de diagnóstico, incluidos los signos vitales y las medidas del cuerpo. Por ejemplo, resonancia magnética, electrocardiograma, test de VIH, hemoglobina, recuento de plaquetas, sistemas de escala como la *Escala de heces de Bristol*.

:::image type="content" source="../../media/ta-for-health/exam-name-entities.png" alt-text="Ejemplo de una entidad de examen.":::

:::image type="content" source="../../media/ta-for-health/exam-name-entities-2.png" alt-text="Otro ejemplo de una entidad de nombre de examen.":::

## <a name="general-attributes"></a>Atributos generales

### <a name="entities"></a>Entidades

**DATE**: fecha completa relativa a una enfermedad, un examen, un tratamiento, una medicación o un evento administrativo.

**DIRECTION**: términos direccionales que pueden referirse a una estructura corporal, una enfermedad, un examen o un tratamiento, por ejemplo: izquierda, lateral, superior, posterior.

**FREQUENCY**: describe la frecuencia con la que se produce una enfermedad, un examen, un tratamiento o una medicación.

**MEASUREMENT_VALUE**: el valor relacionado con la medición de un examen o una enfermedad.

**MEASUREMENT_UNIT**: la unidad de medida relacionada con la medición de un examen o una enfermedad.

**RELATIONAL_OPERATOR**: frases que expresan la relación cuantitativa entre una entidad e información adicional.

**TIME**: términos temporales relacionados con el inicio o la longitud (duración) de una enfermedad, un examen, un tratamiento, una medicación o un evento administrativo. 

## <a name="genomics"></a>Genomics

### <a name="entities"></a>Entidades

**GENE_OR_PROTEIN**: todas las menciones de los nombres y símbolos de los genes humanos, así como los cromosomas y las partes de los cromosomas y las proteínas. Por ejemplo, MTRR, F2.

:::image type="content" source="../../media/ta-for-health/genomics-entities.png" alt-text="Ejemplo de una entidad génica.":::

**VARIANT**: todas las menciones de las variaciones y mutaciones genéticas. Por ejemplo, `c.524C>T`, `(MTRR):r.1462_1557del96`
  
## <a name="healthcare"></a>Salud

### <a name="entities"></a>Entidades
  
**ADMINISTRATIVE_EVENT**: eventos relacionados con el sistema sanitario pero con una naturaleza administrativa o semiadministrativa. Por ejemplo, registro, admisión, evaluación, entrada de estudio, transferencia, alta, hospital, estancia en el hospital. 

:::image type="content" source="../../media/ta-for-health/healthcare-event-entity.png" alt-text="Ejemplo de una entidad de eventos de atención sanitaria.":::

**CARE_ENVIRONMENT**: un entorno o una ubicación donde se proporciona atención a los pacientes. Por ejemplo, sala de emergencias, oficina del doctor, unidad de cardio, hospital de cuidados paliativos, hospital.

:::image type="content" source="../../media/ta-for-health/healthcare-environment-entity.png" alt-text="Esta captura de pantalla muestra un ejemplo de una entidad del entorno de atención sanitaria.":::

**HEALTHCARE_PROFESSION**: un médico con licencia o sin licencia. Por ejemplo, dentista, patólogo, neurólogo, radiólogo, farmacéutico, nutricionista, fisioterapeuta, quiropráctico.

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity.png" alt-text="Esta captura de pantalla muestra otro ejemplo de una entidad del entorno de atención sanitaria.":::

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity-2.png" alt-text="Otro ejemplo de una entidad de entorno de atención sanitaria.":::

## <a name="medical-condition"></a>Enfermedades

### <a name="entities"></a>Entidades

**DIAGNOSIS**: enfermedad, síndrome, intoxicación. Por ejemplo, cáncer de mama, Alzheimer, hipertensión, insuficiencia cardíaca, lesiones en la espina dorsal.

:::image type="content" source="../../media/ta-for-health/medical-condition-entity.png" alt-text="Ejemplo de una entidad de enfermedad.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-entity-2.png" alt-text="Otro ejemplo de una entidad de enfermedad.":::

**SYMPTOM_OR_SIGN**: evidencia subjetiva u objetiva de la enfermedad u otros diagnósticos. Por ejemplo, dolor de pecho, dolor de cabeza, mareos, sarpullido, disnea, el abdomen está relajado, buen sonido abdominal, bien nutrido.

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity.png" alt-text="Ejemplo de una entidad de síntoma o de signo de enfermedad.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity-2.png" alt-text="Otro ejemplo de una entidad de síntoma o de signo de enfermedad.":::

**CONDITION_QUALIFIER**: términos cualitativos que se usan para describir una enfermedad. Todas las subcategorías siguientes se consideran calificadores:

1.  Expresiones relacionadas con el tiempo: son los términos que describen la dimensión de tiempo cualitativamente, como repentino, agudo, crónico y duradero. 
2.  Expresiones de calidad:  Son términos que describen la "naturaleza" de la enfermedad, como quemaduras o intenso.
3.  Expresiones de gravedad: grave, leve, moderado, incontrolado.
4.  Expresiones de extensión: local, focal, difuso.
5.  Expresiones de radiación: radianes, radiación.
6.  Escala de enfermedad: En algunos casos, una enfermedad se caracteriza por una escala, que es una lista ordenada y finita de valores. Por ejemplo, pacientes en fase III con cáncer de páncreas.
7.  Curso de la enfermedad: término relacionado con el curso o la progresión de una enfermedad, como la mejora, el empeoramiento, la curación y la remisión. 

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis.png" alt-text="Ejemplo de un atributo calificador de enfermedad y una entidad de diagnóstico.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-2.png" alt-text="Otro ejemplo de un atributo calificador de enfermedad y una entidad de diagnóstico.":::

:::image type="content" source="../../media/ta-for-health/conditional-qualifier-symptom-medication.png" alt-text="Ejemplo de un atributo calificador de enfermedad con entidades de síntomas y medicamentos.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-3.png" alt-text="Esta captura de pantalla muestra otro ejemplo de un atributo calificador de enfermedad con una entidad de diagnóstico.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-symptom.png" alt-text="Esta captura de pantalla muestra un ejemplo adicional de un atributo calificador de enfermedad con una entidad de diagnóstico.":::

## <a name="medication"></a>Medicación

### <a name="entities"></a>Entidades

**MEDICATION_CLASS**: conjunto de medicamentos que tienen un mecanismo de acción similar, un modo de acción relacionado, una estructura química similar o que se usan para tratar la misma enfermedad. Por ejemplo, el inhibidor de ACE, opioides, antibióticos y analgésicos.

:::image type="content" source="../../media/ta-for-health/medication-entities-class.png" alt-text="Ejemplo de una entidad de clase de medicación.":::

**MEDICATION_NAME**: menciones a la medicación, incluidos los nombres de marca con copyright y los nombres sin marca. Por ejemplo, Ibuprofeno.

:::image type="content" source="../../media/ta-for-health/medication-entities-name.png" alt-text="Ejemplo de una entidad de nombre de medicación.":::

**DOSAGE**: cantidad de medicación solicitada. Por ejemplo, inocular *1000 mL* de una solución de cloruro sódico.

:::image type="content" source="../../media/ta-for-health/medication-dosage.png" alt-text="Ejemplo de un atributo de dosis de medicación.":::

**MEDICATION_FORM**: la forma del medicamento. Por ejemplo, solución, pastilla, cápsula, tableta, parche, gel, pasta, espuma, aerosol, gotas, crema y jarabe.

:::image type="content" source="../../media/ta-for-health/medication-form.png" alt-text="Ejemplo de un atributo de forma de medicación.":::

**MEDICATION_ROUTE**: el método de administración de la medicación. Por ejemplo, oral, vaginal, intravenosa, epidural, tópica, inhalada.

:::image type="content" source="../../media/ta-for-health/medication-route.png" alt-text="Ejemplo de un atributo de ruta de medicación.":::

## <a name="social"></a>Redes sociales

### <a name="entities"></a>Entidades

**FAMILY_RELATION**: menciones de familiares relativas al sujeto. Por ejemplo, padre, hija, hermanos, progenitores.

:::image type="content" source="../../media/ta-for-health/family-relation.png" alt-text="Captura de pantalla que muestra otro ejemplo de un atributo de duración de tratamiento.":::

## <a name="treatment"></a>Tratamiento

### <a name="entities"></a>Entidades

**TREATMENT_NAME**: procedimientos terapéuticos. Por ejemplo, cirugía de sustitución de rodilla, trasplante de médula ósea, implante valvular aórtico transcatéter, dieta.

:::image type="content" source="../../media/ta-for-health/treatment-entities-name.png" alt-text="Ejemplo de una entidad de nombre de tratamiento.":::

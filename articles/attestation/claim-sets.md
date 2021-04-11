---
title: Conjuntos de notificaciones de Azure Attestation
description: Los conjuntos de notificaciones de Azure Attestation.
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 0d6d5a08ea85ebb666acc0336f1e1d7ec5e097da
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2021
ms.locfileid: "105044675"
---
# <a name="claim-sets"></a>Conjuntos de notificaciones

Las notificaciones generadas en el proceso de atestación de enclaves mediante Microsoft Azure Attestation se pueden dividir en las siguientes categorías:

- **Notificaciones entrantes**: las notificaciones que genera Microsoft Azure Attestation después de analizar la evidencia de atestación; lo pueden usar los creadores de directivas para definir las reglas de autorización en una directiva personalizada.

- **Notificaciones salientes**: las notificaciones que genera Azure Attestation y que se incluyen en el token de atestación.

- **Notificaciones de propiedad**: notificaciones que Azure Attestation crea como salida. Contiene todas las notificaciones que representan las propiedades del token de atestación, como la codificación del informe, la duración de la validez del informe, etc.

## <a name="incoming-claims"></a>Notificaciones entrantes 

### <a name="sgx-attestation"></a>Atestación de SGX

Notificaciones que deben usar los creadores de directivas para definir las reglas de autorización en una directiva de atestación de SGX:

- **x-ms-sgx-is-debuggable**: valor booleano que indica si el enclave tiene habilitada la depuración o no
- **x-ms-sgx-product-id**: valor de identificador del producto del enclave de SGX 
- **sgx-mrsigner**: valor hexadecimal codificado del campo "mrsigner" de la oferta.
- **x-ms-sgx-mrenclave**: valor hexadecimal codificado del campo "mrenclave" de la oferta.
- **x-ms-sgx-svn**: número de versión de seguridad codificado en la oferta. 

Las siguientes notificaciones se consideran en desuso, pero se admiten por completo y seguirán estando incluidas en el futuro. Se recomienda usar los nombres de notificaciones que no estén en desuso.

Notificación en desuso | Notificación recomendada
--- | --- 
$is-debuggable | x-ms-sgx-is-debuggable
$product-id | x-ms-sgx-product-id
$sgx-mrsigner | x-ms-sgx-mrsigner
$sgx-mrenclave | x-ms-sgx-mrenclave
$svn | x-ms-sgx-svn

### <a name="tpm-attestation"></a>Atestación de TPM

Notificaciones que deben usar los creadores de directivas para definir las reglas de autorización en una directiva de atestación de TPM:

- **aikValidated**: valor booleano que indica si se ha validado o no el certificado de la clave de identidad de la atestación (AIK).
- **aikPubHash**:  cadena que contiene el hash SHA256 en base64 (clave pública de identidad de atestación en formato DER).
- **tpmVersion**:   valor entero que contiene la versión principal del Módulo de plataforma segura (TPM).
- **secureBootEnabled**: valor booleano que indica si está habilitado el arranque seguro.
- **iommuEnabled**:  valor booleano que indica si está habilitada la unidad de administración de memoria de entrada y salida (IOMMU).
- **bootDebuggingDisabled**: valor booleano que indica si está deshabilitada la depuración de arranque.
- **notSafeMode**:  valor booleano que indica si Windows no se ejecuta en modo seguro.
- **notWinPE**:  valor booleano que indica si Windows no se ejecuta en modo WinPE.
- **vbsEnabled**:  valor booleano que indica si VBS está habilitado.
- **vbsReportPresent**:  valor booleano que indica si está disponible el informe de enclaves de VBS.

### <a name="vbs-attestation"></a>Atestación de VBS

Además de las notificaciones de directivas de atestación de TPM, los creadores de directivas pueden usar las notificaciones siguientes para definir reglas de autorización en una directiva de atestación de VBS.

- **enclaveAuthorId**:  valor de cadena que contiene el valor codificado en Base64Url del identificador de autor del enclave; es decir, el identificador de autor del módulo principal del enclave.
- **enclaveImageId**: valor de cadena que contiene el valor codificado en Base64Url del identificador de imagen del enclave, es decir, el identificador de imagen del módulo principal del enclave.
- **enclaveOwnerId**: valor de cadena que contiene el valor codificado en Base64Url del identificador de propietario del enclave.
- **enclaveFamilyId**:  valor de cadena que contiene el valor codificado en Base64Url del identificador de familia del enclave; es decir, el identificador de familia del módulo principal del enclave.
- **enclaveSvn**:  valor entero que contiene el número de versión de seguridad del módulo principal del enclave.
- **enclavePlatformSvn**:  valor entero que contiene el número de versión de seguridad de la plataforma que hospeda el enclave.
- **enclaveFlags**:  la notificación enclaveFlags es un valor entero que contiene marcas que describen la directiva de tiempo de ejecución del enclave.

## <a name="outgoing-claims"></a>Notificaciones salientes 

### <a name="common-for-all-attestation-types"></a>Común para todos los tipos de atestación

Azure Attestation incluye las siguientes notificaciones en el token de atestación de todos los tipos de atestación. 

- **x-ms-ver**: versión de esquema de JWT (se espera que sea "1.0").
- **x-ms-attestation-type**: valor de cadena que representa el tipo de atestación. 
- **x-ms-policy-hash**: hash de la directiva de evaluación de Azure Attestation calculada como BASE64URL (SHA256 (UTF8 (BASE64URL (UTF8 (texto de directiva))))
- **x-ms-policy-signer**: objeto JSON con un miembro "jwk" que representa la clave que un cliente usó para firmar su directiva. Se puede aplicar cuando el cliente carga una directiva firmada

Los nombres de notificaciones siguientes se usan en la [especificación JWT de IETF](https://tools.ietf.org/html/rfc7519)

- **Notificación "jti" (ID. de JWT)** : identificador único para el JSON Web Token.
- **Notificación "iss" (emisor)** : entidad de seguridad que ha emitido el JSON Web Token. 
- **Notificación "iat" (emitida a las)** : hora a la que se ha emitido el JSON Web Token 
- **Notificación "exp" (hora de expiración)** : hora de expiración tras la que el JSON Web Token no debe aceptarse para su procesamiento.
- **Notificación "nbf" (no antes de)** : hora antes de la cual no debe aceptarse el JSON Web Token para su procesamiento. 

Los nombres de notificaciones siguientes se usan en la [especificación del borrador EAT de IETF](https://tools.ietf.org/html/draft-ietf-rats-eat-03#page-9)

- **"Notificación nonce" (nonce)** : una copia directa no transformada de un valor de nonce opcional proporcionado por un cliente 

Las siguientes notificaciones se consideran en desuso, pero se admiten por completo y seguirán estando incluidas en el futuro. Se recomienda usar los nombres de notificaciones que no estén en desuso.

Notificación en desuso | Notificación recomendada
--- | --- 
ver | x-ms-ver
tee | x-ms-attestation-type
policy_hash | x-ms-policy-hash
maa-policyHash | x-ms-policy-hash
policy_signer  | x-ms-policy-signer

### <a name="sgx-attestation"></a>Atestación de SGX 

El servicio genera las siguientes notificaciones y las incluyen en el token de atestación para la atestación de SGX.

- **x-ms-sgx-is-debuggable**: valor booleano que indica si el enclave tiene habilitada la depuración o no
- **x-ms-sgx-product-id**: valor de identificador del producto del enclave de SGX 
- **sgx-mrsigner**: valor hexadecimal codificado del campo "mrsigner" de la oferta.
- **x-ms-sgx-mrenclave**: valor hexadecimal codificado del campo "mrenclave" de la oferta.
- **x-ms-sgx-svn**: número de versión de seguridad codificado en la oferta. 
- **x-MS-SGX-EHD**: datos contenidos en el enclave con el formato BASE64URL (datos contenidos en el encave).
- **x-ms-sgx-collateral**: objeto JSON que describe el colateral que se usa para realizar la atestación. El valor de la notificación colateral x-ms-sgx es un objeto JSON anidado con los siguientes pares clave-valor:
    - **qeidcertshash**: valor SHA256 de los certificados de emisión de identidades de Quoting Enclave (QE)
    - **qeidcrlhash**: valor SHA256 de la lista CRL de certificados de emisión de identidades de QE.
    - **qeidhash**: valor SHA256 del colateral de identidad de QE.
    - **quotehash**: valor SHA256 del presupuesto evaluado.
    - **tcbinfocertshash**: valor SHA256 de los certificados de emisión de información de TCB.
    - **tcbinfocrlhash**: valor SHA256 de la lista CRL de certificados de emisión de información de TCB.
    - **tcbinfohash**: valor SHA256 de la documentación y material adjunto de la información de TCB

Las siguientes notificaciones se consideran en desuso, pero se admiten por completo y seguirán estando incluidas en el futuro. Se recomienda usar los nombres de notificaciones que no estén en desuso.

Notificación en desuso | Notificación recomendada
--- | --- 
$is-debuggable | x-ms-sgx-is-debuggable
$product-id | x-ms-sgx-product-id
$sgx-mrsigner | x-ms-sgx-mrsigner
$sgx-mrenclave | x-ms-sgx-mrenclave
$svn | x-ms-sgx-svn
$maa-ehd | x-ms-sgx-ehd
$aas-ehd | x-ms-sgx-ehd
$maa-attestationcollateral | x-ms-sgx-collateral

### <a name="tpm-and-vbs-attestation"></a>Atestación de TPM y VBS

- **cnf (confirmación)** : esta notificación se usa para identificar la clave de la prueba de posesión. La notificación de confirmación, tal como se define en RFC 7800, contiene la parte pública de la clave del enclave atestado que se representa como un objeto de clave web JSON (JWK) (RFC 7517).
- **rp_data (datos de usuario de confianza)**: datos de usuario de confianza, si los hay, especificados en la solicitud, que utiliza este usuario como nonce para garantizar que el informe está actualizado. rp_data solo se agrega si hay rp_data

## <a name="property-claims"></a>Notificaciones de propiedad

### <a name="tpm-and-vbs-attestation"></a>Atestación de TPM y VBS

- **report_validity_in_minutes**: notificación de entero que indica el periodo de validez del token.
  - **Valor predeterminado (tiempo)**: un día, expresado en minutos.
  - **Valor máximo (tiempo)**: un año, expresado en minutos.
- **omit_x5c**: notificación booleana que indica si Azure Attestation debe omitir el certificado usado para proporcionar la autenticidad de la prueba de servicio. Si es True, se agrega x5t al token de atestación. Si es False (valor predeterminado), se agregar x5c al token de atestación.

## <a name="next-steps"></a>Pasos siguientes
- [Creación y firma de una directiva de atestación](author-sign-policy.md)
- [Configuración de Azure Attestation con PowerShell](quickstart-powershell.md)

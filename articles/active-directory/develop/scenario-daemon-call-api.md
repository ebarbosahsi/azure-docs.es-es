---
title: Llamada a una API web desde una aplicación de demonio | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo crear una aplicación de demonio que llama a una API web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: f4b223c9195168b445b7eb81c2709a61807fddac
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104578403"
---
# <a name="daemon-app-that-calls-web-apis---call-a-web-api-from-the-app"></a>Aplicación de demonio que llama a las API web: llamada a una API web desde la aplicación

Las aplicaciones de demonio de .NET pueden llamar a una API web. Las aplicaciones de demonio de .NET también pueden llamar a varias API web aprobadas previamente.

## <a name="calling-a-web-api-from-a-daemon-application"></a>Llamada a una API web desde una aplicación de demonio

Aquí se muestra cómo usar el token para llamar a una API:

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="java"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Con un cliente HTTP como [Axios](https://www.npmjs.com/package/axios), llame al URI del punto de conexión de la API con un token de acceso como *portador de autorización*.

```JavaScript
const axios = require('axios');

async function callApi(endpoint, accessToken) {

    const options = {
        headers: {
            Authorization: `Bearer ${accessToken}`
        }
    };

    console.log('request made to web API at: ' + new Date().toString());

    try {
        const response = await axios.default.get(endpoint, options);
        return response.data;
    } catch (error) {
        console.log(error)
        return error;
    }
};
```

# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

---

## <a name="calling-several-apis"></a>Llamada a varias API

Para las aplicaciones de demonio, las API web a las que llame deben estar aprobadas previamente. No hay ningún consentimiento incremental con las aplicaciones de demonio. (No hay interacción del usuario). El administrador de inquilinos debe dar el consentimiento previo a la aplicación y a todos los permisos de la API. Si quiere llamar a varias API, adquiera un token para cada recurso, cada vez llamando a `AcquireTokenForClient`. MSAL usará la caché de tokens de aplicación para evitar las llamadas de servicio innecesarias.

## <a name="next-steps"></a>Pasos siguientes

# <a name="net"></a>[.NET](#tab/dotnet)

Avance al siguiente artículo de este escenario, [Paso a producción](./scenario-daemon-production.md?tabs=dotnet).

# <a name="java"></a>[Java](#tab/java)

Avance al siguiente artículo de este escenario, [Paso a producción](./scenario-daemon-production.md?tabs=java).

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Avance al siguiente artículo de este escenario, [Paso a producción](./scenario-daemon-production.md?tabs=nodejs).

# <a name="python"></a>[Python](#tab/python)

Avance al siguiente artículo de este escenario, [Paso a producción](./scenario-daemon-production.md?tabs=python).

---

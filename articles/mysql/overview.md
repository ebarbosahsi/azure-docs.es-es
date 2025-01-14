---
title: 'Introducción: Azure Database for MySQL'
description: Obtenga más información acerca de Azure Database for MySQL, que es un servicio de base de datos relacional de Microsoft Cloud basado en MySQL Community Edition.
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc
ms.topic: overview
ms.date: 3/18/2020
ms.openlocfilehash: 20b54afdfbb5641def921ffc17b3c38f6650acab
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101736226"
---
# <a name="what-is-azure-database-for-mysql"></a>¿Qué es Azure Database for MySQL?

Azure Database for MySQL es un servicio de base de datos relacional de Microsoft Cloud basado en el motor de base de datos de [MySQL Community Edition](https://www.mysql.com/products/community/) (disponible bajo la licencia GPLv2), versiones 5.6, 5.7 y 8.0. Azure Database for MySQL ofrece lo siguiente:

- Alta disponibilidad integrada
- Protección de datos mediante copias de seguridad automáticas y restauración a un momento dado durante un máximo de 35 días.
- Mantenimiento automatizado para el hardware subyacente, el sistema operativo y el motor de base de datos para mantener el servicio seguro y actualizado.
- Rendimiento predecible, con precios de pago por uso inclusivos.
- Escalado elástico en cuestión de segundos.
- Controles de optimización de costos con capacidad para detener o iniciar un servidor. 
- Seguridad de nivel empresarial y cumplimiento normativo líder en el mercado para proteger la información confidencial en reposo y en movimiento.
- Supervisión y automatización para simplificar la administración y la supervisión de las implementaciones a gran escala.
- Experiencia de soporte técnico líder del sector.

Estas funcionalidades no requieren casi ninguna tarea de administración y todas se proporcionan sin ningún costo adicional. Le permiten centrarse en el desarrollo rápido de aplicaciones y en reducir el plazo de acceso al mercado, en lugar de tener que dedicar tiempo y recursos a la administración tanto de máquinas virtuales como de infraestructura. Además, puede seguir desarrollando su aplicación con las herramientas y la plataforma de código abierto de su elección, y entregar con la velocidad y eficacia que exige su negocio sin tener que aprender nuevas habilidades.

:::image type="content" source="media/overview/1-azure-db-for-mysql-conceptual-diagram.png" alt-text="Diagrama conceptual de Azure Database for MySQL":::

## <a name="deployment-models"></a>Modelos de implementación

Azure Database for MySQL, con tecnología de MySQL Community Edition, está disponible en dos modos de implementación:
- Servidor único 
- Servidor flexible (versión preliminar)
  
### <a name="azure-database-for-mysql---single-server"></a>Servidor único de Azure Database for MySQL

Servidor único de Azure Database for MySQL es un servicio de base de datos totalmente administrado con requisitos mínimos para las personalizaciones de base de datos. La plataforma de servidor único está diseñada para administrar la mayoría de las funciones de administración de bases de datos, como las de aplicación de revisión, copias de seguridad, alta disponibilidad y seguridad con una configuración y un control mínimos para el usuario. La arquitectura está optimizada para una alta disponibilidad integrada, ya que ofrece una disponibilidad del 99,99 % en una zona de disponibilidad única. Es compatible con la versión Community de MySQL 5.6, 5.7 y 8.0. El servicio está disponible con carácter general hoy en muchas [regiones de Azure](https://azure.microsoft.com/global-infrastructure/services/).

La opción de implementación Un solo servidor ofrece tres planes de tarifa: Básico, De uso general y Optimizado para memoria. Cada plan ofrece capacidades de recursos diferente para admitir sus cargas de trabajo de la base de datos. Puede compilar su primera aplicación en una base de datos pequeña por poco dinero al mes y, después, ajustar la escala para satisfacer las necesidades de la solución. La escalabilidad dinámica permite a la base de datos responder de manera transparente a los cambiantes requisitos de recursos. Solo paga por los recursos que necesite y cuando los necesite. Vea los [Planes de tarifa](concepts-pricing-tiers.md) para obtener más información.

Los servidores únicos son los más adecuados para las aplicaciones nativas en la nube diseñadas para controlar la aplicación de revisiones automatizada sin necesidad de un control granular sobre la programación de aplicación de revisiones ni de una configuración personalizada de MySQL. 

Para obtener información general detallada sobre el modo de implementación de servidor único, consulte [Información general de servidor único](single-server-overview.md).

### <a name="azure-database-for-mysql---flexible-server-preview"></a>Servidor flexible (versión preliminar) de Azure Database for MySQL

Servidor flexible de Azure Database for MySQL es un servicio de base de datos totalmente administrado diseñado para proporcionar un control más granular y una mayor flexibilidad sobre las funciones de administración de bases de datos y las opciones de configuración. En general, el servicio proporciona mayor flexibilidad y personalizaciones en función de los requisitos del usuario. La arquitectura de servidor flexible permite a los usuarios optar por la alta disponibilidad dentro de una única zona de disponibilidad o entre varias. Los servidores flexibles proporcionan mejores controles de optimización de costos con la capacidad de iniciar o detener el servidor y niveles de proceso flexibles, que son idóneos para cargas de trabajo que no necesitan una capacidad de proceso completa de forma continua. El servicio es compatible con la versión Community de MySQL 5.7 y 8.0. El servicio está disponible actualmente en versión preliminar pública en muchas [regiones de Azure](https://azure.microsoft.com/global-infrastructure/services/).

Los servidores flexibles son idóneos para 
- Desarrollo de aplicaciones que requieren un control mejorado y personalizaciones.
- Alta disponibilidad con redundancia de zona
- Ventanas de mantenimiento administradas

Para obtener información general detallada sobre el modo de implementación de servidor flexible, consulte [Información general de servidor flexible](flexible-server/overview.md).

## <a name="contacts"></a>Contactos
Para cualquier pregunta o sugerencia que pueda tener con respecto al uso de Azure Database for MySQL, envíe un correo electrónico al equipo de Azure Database for MySQL ([@Ask Azure DB for MySQL](mailto:AskAzureDBforMySQL@service.microsoft.com)). Esta dirección de correo electrónico no es un alias de soporte técnico.

Además, tenga en cuenta los siguientes puntos de contacto según corresponda:

- Para ponerse en contacto con el servicio de soporte técnico de Azure, [presente una incidencia de soporte técnico en Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Para corregir un problema con la cuenta, envíe una [solicitud de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) en Azure Portal.
- Para proporcionar comentarios o solicitar nuevas características, cree una entrada mediante [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los dos modos de implementación de Azure Database for MySQL y elija las opciones adecuadas en función de sus necesidades.

- [Servidor único](single-server/index.yml)
- [Servidor flexible](flexible-server/index.yml)
- [Elección de la opción de implementación de MySQL adecuada para su carga de trabajo](select-right-deployment-type.md)

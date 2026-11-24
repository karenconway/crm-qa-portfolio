# 01 · Análisis de la historia de usuario

| | |
|---|---|
| **Historia** | HU-001 · Campo personalizado "Cantidad de empleados" |
| **Producto de referencia** | Declan CRM · módulo de campos personalizados (EAV) |
| **QA** | Karen Conway |

## Contexto

Una empresa B2B usa el CRM para administrar a sus clientes y necesita registrar cuántos empleados tiene cada uno. Ese dato no existe en el formulario de Cuentas. El CRM permite agregar **campos personalizados** sin modificar el sistema.

## Historia de usuario

> **Como** administrador comercial,
> **quiero** agregar un campo personalizado llamado "Cantidad de empleados" al formulario de Cuentas,
> **para** registrar el tamaño de cada cliente sin pedir cambios al sistema.

## Criterios de aceptación

| ID | Criterio |
|---|---|
| CA-01 | El administrador puede crear un campo personalizado de tipo numérico. |
| CA-02 | El nuevo campo aparece en el formulario de alta y edición de Cuentas. |
| CA-03 | Los usuarios pueden ingresar y guardar un valor. |
| CA-04 | Al volver a abrir la cuenta, el valor permanece guardado. |
| CA-05 | El sistema no permite ingresar letras en un campo numérico. |
| CA-06 | El campo solamente aparece en el tenant que lo creó. |

> **Tenant:** cada empresa que usa el CRM tiene su propio espacio con sus datos. Un campo creado por una empresa no debe verse en otra.

## Preguntas al Product Owner

Antes de escribir los casos, revisé la historia y anoté lo que no estaba claro:

| # | Pregunta | Respuesta / supuesto usado |
|---|---|---|
| 1 | ¿Acepta decimales o negativos? | Solo números enteros desde 0. |
| 2 | ¿Hay un valor máximo? | 999.999.999. |
| 3 | ¿El campo es obligatorio? ¿El 0 es válido? | Es opcional. El 0 es un valor válido (no es lo mismo que vacío). |
| 4 | "No permite ingresar letras": ¿bloquea la tecla o avisa al guardar? | Avisa al guardar con un mensaje y no guarda el dato. |
| 5 | ¿Quién puede crear el campo? | Solo el Administrador. |
| 6 | ¿Puede haber dos campos con el mismo nombre? | No en la misma empresa (tenant). |

## Riesgos principales

| Riesgo | Por qué importa | Caso que lo cubre |
|---|---|---|
| Una empresa ve datos de otra | Es el error más grave en un CRM multiempresa. | CP-06 |
| El valor se pierde al reabrir | El usuario pierde información cargada. | CP-04 |
| Se guardan letras o valores inválidos | Datos incorrectos en reportes. | CP-05 |

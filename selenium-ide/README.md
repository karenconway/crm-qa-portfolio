# Automatización con Selenium IDE

Los 6 casos de prueba de HU-001 están grabados en Selenium IDE, en el archivo [HU-001-cantidad-de-empleados.side](HU-001-cantidad-de-empleados.side). Se usan como **prueba de regresión**: se ejecutan en cada versión nueva para confirmar que lo que funcionaba sigue funcionando.

## Cómo ejecutarlo

1. Instalar la extensión **Selenium IDE** en Chrome o Firefox.
2. Tener el CRM abierto en `http://127.0.0.1:5173`.
3. Abrir Selenium IDE → **Open an existing project** → elegir el archivo `.side`.
4. Elegir la suite **"Regresión HU-001"** y hacer clic en **Run all tests**.

## Qué hace cada test

| Test | Qué verifica |
|---|---|
| CP-01 Crear el campo numérico | Error con nombre vacío, mensaje de creación y contador "1 campo". |
| CP-02 El campo aparece en alta y edición | El campo está en "Nueva cuenta" y en "Editar" de Litoral Logística. |
| CP-03 Guardar un valor (límite 0) | Guarda 0 y el directorio muestra "1 de 1 completos". |
| CP-04 El valor se mantiene al recargar | Guarda 80, recarga la página y el valor sigue siendo 80. |
| CP-05 No se pueden guardar letras | Con "abc" aparece el error y el valor no se guarda. |
| CP-06 El campo solo aparece en su tenant | En Sur Distribuciones no hay campos; en Atlas Comercial sí. |

Cada test empieza restableciendo los datos desde **Laboratorio QA**, así no depende de lo que hizo el anterior.

## Comandos usados

| Comando | Para qué |
|---|---|
| `open` | Abrir la página. |
| `click` | Hacer clic en un botón o enlace. |
| `type` | Escribir en un campo. |
| `select` | Elegir una opción de una lista (tenant). |
| `assert text` | Verificar que un mensaje dice exactamente lo esperado. |
| `assert value` | Verificar el valor de un campo. |
| `assert element present` / `not present` | Verificar que algo aparece o no aparece. |

Los elementos se buscan por su atributo `data-testid` (por ejemplo, `css=[data-testid="save-account"]`), que no cambia aunque cambie el diseño de la pantalla.

## Resultado de la última ejecución (v1.1)

| Test | Resultado |
|---|---|
| CP-01 | ✅ PASS |
| CP-02 | ✅ PASS |
| CP-03 | ✅ PASS |
| CP-04 | ✅ PASS |
| CP-05 | ✅ PASS |
| CP-06 | ✅ PASS |

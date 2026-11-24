# 02 · Casos de prueba

**Historia:** HU-001 · **Diseñados por:** Karen Conway · **Total:** 6 casos (uno por criterio de aceptación)

También en Excel: [casos-de-prueba-HU001.xlsx](casos-de-prueba-HU001.xlsx)

| ID | Caso | Criterio | Prioridad | Técnica |
|---|---|---|---|---|
| [CP-01](#cp-01) | Crear el campo numérico "Cantidad de empleados" | CA-01 | Alta | Partición de equivalencia · Valores límite |
| [CP-02](#cp-02) | El campo aparece en el alta y en la edición de Cuentas | CA-02 | Alta | Flujo principal |
| [CP-03](#cp-03) | Guardar un valor, incluidos los límites 0 y 999999999 | CA-03 | Alta | Valores límite |
| [CP-04](#cp-04) | El valor se mantiene al reabrir la cuenta y al recargar la página | CA-04 | Alta | Transición de estado |
| [CP-05](#cp-05) | No se pueden guardar letras ni valores inválidos | CA-05 | Alta | Partición de equivalencia · Valores límite |
| [CP-06](#cp-06) | El campo y su valor solo aparecen en el tenant que lo creó | CA-06 | Alta | Aislamiento entre tenants |

**Precondiciones**

| Código | Estado inicial |
|---|---|
| D0 | Tenant Atlas Comercial, rol Administrador, datos restablecidos desde Laboratorio QA (3 cuentas, sin campos personalizados). |
| D1 | D0 + campo "Cantidad de empleados" creado. |
| D2 | D1 + Andes Tecnología con Cantidad de empleados = 80. |

**Técnicas usadas**

- **Partición de equivalencia:** separar valores en grupos que el sistema trata igual (válidos, letras, negativos, decimales) y probar uno de cada grupo.
- **Valores límite:** probar justo en el borde de lo permitido: 0 y 999999999 (válidos), 1000000000 (inválido), nombre de 61 caracteres (inválido).

---

### CP-01

**Crear el campo numérico "Cantidad de empleados"**

| | |
|---|---|
| Criterio | CA-01 |
| Prioridad | Alta |
| Tipo | Positivo / Negativo |
| Precondición | D0 |
| Datos | Válido: "Cantidad de empleados" · Inválidos: nombre vacío, nombre duplicado ("cantidad DE empleados"), nombre de 61 caracteres |

**Pasos**

1. Ir a Campos personalizados y hacer clic en "Crear campo".
2. Hacer clic en "Crear campo" con el nombre vacío.
3. Ingresar un nombre de 61 caracteres y guardar.
4. Ingresar "Cantidad de empleados" y guardar.
5. Abrir "Crear campo" otra vez, ingresar "cantidad DE empleados" y guardar.

**Resultado esperado:** Vacío: "Ingresá el nombre del campo." · 61 caracteres: "El nombre admite hasta 60 caracteres." · Válido: mensaje "Campo creado…" y el campo aparece en el listado como Numérico entero, Opcional, Activo, con el contador "1 campo" · Duplicado: "Ya existe un campo con ese nombre en este tenant." En total queda 1 solo campo.

---

### CP-02

**El campo aparece en el alta y en la edición de Cuentas**

| | |
|---|---|
| Criterio | CA-02 |
| Prioridad | Alta |
| Tipo | Positivo |
| Precondición | D1 |
| Datos | Alta: formulario "Nueva cuenta" · Edición: Litoral Logística (creada antes que el campo) |

**Pasos**

1. Ir a Cuentas y hacer clic en "Nueva cuenta".
2. Verificar la sección Campos personalizados y cancelar.
3. Hacer clic en "Editar" en Litoral Logística.
4. Verificar la sección Campos personalizados.

**Resultado esperado:** En ambos formularios aparece "Cantidad de empleados", marcado como Opcional y vacío. En la edición se mantienen los datos de la cuenta.

---

### CP-03

**Guardar un valor, incluidos los límites 0 y 999999999**

| | |
|---|---|
| Criterio | CA-03 |
| Prioridad | Alta |
| Tipo | Positivo |
| Precondición | D1 |
| Datos | Delta Servicios (nueva) = 250 · Litoral Logística = 0 · Andes Tecnología = 999999999 |

**Pasos**

1. Crear la cuenta "Delta Servicios" con 250 y guardar.
2. Editar Litoral Logística, ingresar 0 y guardar.
3. Editar Andes Tecnología, ingresar 999999999 y guardar.
4. Revisar la columna "Campos adicionales" del directorio.

**Resultado esperado:** Las tres cuentas se guardan con el mensaje "Cuenta guardada correctamente." y muestran "1 de 1 completos". El 0 se guarda como valor (no como vacío).

---

### CP-04

**El valor se mantiene al reabrir la cuenta y al recargar la página**

| | |
|---|---|
| Criterio | CA-04 |
| Prioridad | Alta |
| Tipo | Positivo |
| Precondición | D2 |
| Datos | Andes Tecnología = 80 |

**Pasos**

1. Editar Andes Tecnología y verificar el valor. Cancelar.
2. Recargar la página (F5).
3. Volver a editar Andes Tecnología.

**Resultado esperado:** Antes y después de recargar, Cantidad de empleados muestra 80.

---

### CP-05

**No se pueden guardar letras ni valores inválidos**

| | |
|---|---|
| Criterio | CA-05 |
| Prioridad | Alta |
| Tipo | Negativo |
| Precondición | D2 |
| Datos | abc · 12a · -1 · 1.5 · 1000000000 (sobre el valor guardado 80) |

**Pasos**

1. Editar Andes Tecnología.
2. Ingresar cada valor de la lista y hacer clic en "Guardar cuenta".
3. Cancelar y volver a abrir la cuenta.

**Resultado esperado:** Cada valor se rechaza con un mensaje de validación, el campo queda marcado en rojo y no se guarda. Al reabrir, el valor sigue siendo 80.

---

### CP-06

**El campo y su valor solo aparecen en el tenant que lo creó**

| | |
|---|---|
| Criterio | CA-06 |
| Prioridad | Alta |
| Tipo | Negativo |
| Precondición | D2 |
| Datos | Campo creado en Atlas Comercial · tenant Sur Distribuciones |

**Pasos**

1. Cambiar el tenant a Sur Distribuciones.
2. Revisar Campos personalizados y editar una cuenta.
3. Volver a Atlas Comercial y editar Andes Tecnología.

**Resultado esperado:** En Sur Distribuciones no hay campos personalizados y el formulario no muestra "Cantidad de empleados". Al volver a Atlas, el campo sigue y Andes Tecnología muestra 80.

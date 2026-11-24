# 05 · Validación de datos con SQL

Además de verificar en pantalla, estas consultas sirven para confirmar en la base de datos que la información quedó bien guardada. Usan un modelo simplificado de campos personalizados (EAV) con tres tablas.

## Tablas

**cuentas**

| id | tenant | nombre |
|---|---|---|
| 1 | Atlas Comercial | Andes Tecnología |
| 2 | Atlas Comercial | Litoral Logística |
| 3 | Atlas Comercial | Pampa Alimentos |
| 4 | Sur Distribuciones | Sur Distribuciones |
| 5 | Sur Distribuciones | Costa Servicios |

**campos** (los campos personalizados que se crean)

| id | tenant | nombre | tipo |
|---|---|---|---|
| 10 | Atlas Comercial | Cantidad de empleados | Numérico |

**valores** (el dato que se carga en cada cuenta)

| cuenta_id | campo_id | valor |
|---|---|---|
| 1 | 10 | 80 |
| 2 | 10 | 0 |

## Consultas

### 1. ¿Quedó guardado el valor de Andes Tecnología? (CP-03, CP-04)

```sql
SELECT cuentas.nombre, campos.nombre, valores.valor
FROM valores
JOIN cuentas ON cuentas.id = valores.cuenta_id
JOIN campos  ON campos.id  = valores.campo_id
WHERE cuentas.nombre = 'Andes Tecnología';
```

| cuentas.nombre | campos.nombre | valor |
|---|---|---|
| Andes Tecnología | Cantidad de empleados | 80 |

### 2. Todas las cuentas de Atlas, con o sin valor cargado (CP-03)

```sql
SELECT cuentas.nombre, valores.valor
FROM cuentas
LEFT JOIN valores ON valores.cuenta_id = cuentas.id AND valores.campo_id = 10
WHERE cuentas.tenant = 'Atlas Comercial'
ORDER BY cuentas.nombre;
```

| nombre | valor |
|---|---|
| Andes Tecnología | 80 |
| Litoral Logística | 0 |
| Pampa Alimentos | NULL |

Se usa `LEFT JOIN` para que aparezcan también las cuentas **sin** valor. Así se ve la diferencia entre **0** (dato cargado) y **NULL** (sin dato).

### 3. ¿Sur Distribuciones tiene campos personalizados? (CP-06)

```sql
SELECT *
FROM campos
WHERE tenant = 'Sur Distribuciones';
```

Resultado esperado: **ninguna fila**. El campo se creó solo en Atlas Comercial.

### 4. ¿Hay valores mezclados entre tenants? (CP-06)

```sql
SELECT cuentas.nombre, cuentas.tenant, campos.tenant
FROM valores
JOIN cuentas ON cuentas.id = valores.cuenta_id
JOIN campos  ON campos.id  = valores.campo_id
WHERE cuentas.tenant <> campos.tenant;
```

Resultado esperado: **ninguna fila**. Si apareciera alguna, sería una cuenta de un tenant con un valor de un campo de otro tenant: un bug grave.

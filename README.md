## 🙋‍♂️ ¡Hola!


<p align="center">
  <img src="https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi_ML0kykqSnXdoXfnBcP3Djv5CYR4zCRawmZVJiZjicALstQFQcxdVdRzdf3kt4JdTSh3EkAQVc0EeDDc0hLOU4v7FcVSSKBFF5rGAbXiArSTNWQKKVgGuz85mSbhyNOjcCX5WnnlEA2U/s1600/gif+bienvenidos.gif" alt="Bienvenidos" width="600"/>
</p>


Soy **Wily Duvan Villamil Rey**, estudiante de **Ingeniería en Sistemas** y apasionado por la administración de bases de datos. Actualmente me desempeño como **Administrador de Bases de Datos Junior**, y me complace presentarte uno de mis proyectos académicos más recientes.

Este trabajo consiste en el diseño e implementación de una **base de datos relacional en PostgreSQL** orientada a la gestión de información clínica. Para complementar el análisis y facilitar la toma de decisiones estratégicas, utilicé una **conexión con Microsoft Excel** como herramienta de visualización, lo que permitió observar patrones clave y respaldar decisiones acordes al modelo de negocio.

Este repositorio es una muestra de cómo la combinación entre modelado relacional, automatización de consultas y visualización clara puede aportar valor en escenarios reales del sector salud.

---
## 🛠️ Tecnologías Utilizadas

- **PostgreSQL**: para modelado relacional, sentencias SQL y gestión eficiente de datos.
- **Microsoft Excel**: para visualización de datos y generación de reportes estratégicos.
- **SQL**: consultas, creación de tablas, relaciones y automatización de extracción de datos.
---

## 🎯 Objetivo del Proyecto

El principal objetivo de esta base de datos es **gestionar de forma eficiente y estructurada la información clínica** de una institución de salud. El sistema está diseñado para:

- **Centralizar los datos** de pacientes, citas médicas, tratamientos y personal clínico en un entorno relacional confiable.
- **Optimizar la consulta, actualización y análisis** de la información mediante sentencias SQL bien estructuradas.
- **Garantizar la integridad referencial** y la consistencia de los datos mediante el uso de llaves foráneas y restricciones.
- **Proveer una base sólida** para la generación de reportes estratégicos mediante la conexión con herramientas de visualización como Microsoft Excel.
- **Apoyar la toma de decisiones clínicas y administrativas**, facilitando el acceso a información clave del negocio.

Este enfoque permite aplicar los conocimientos adquiridos en bases de datos, modelado relacional y análisis de datos, en un contexto realista y con impacto directo en la gestión del sector salud.

---
## 🧩 Consultas y Scripts Destacados

A continuación, se presentan fragmentos clave del desarrollo de la base de datos MediCare_db, los cuales demuestran un enfoque profesional y buenas prácticas en el diseño clínico relacional:

```sql
-- Creación de la base de datos y esquema
CREATE DATABASE medicare_db
WITH
OWNER = postgres
ENCODING = 'UTF8'
LC_COLLATE = 'es_CO.UTF-8'
LC_CTYPE = 'es_CO.UTF-8'
TEMPLATE = template0;

CREATE SCHEMA IF NOT EXISTS mediCare;

```

## 🧬 Estructura clínica relacional 
```sql
--Tabla de pacientes:
CREATE TABLE mediCare.pacientes (
  id_paciente SERIAL PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  fecha_nacimiento DATE NOT NULL,
  genero VARCHAR(10),
  direccion TEXT,
  telefono VARCHAR(20),
  email VARCHAR(50),
  fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

```
```sql
--Tabla de consultas médicas con relaciones
CREATE TABLE mediCare.consultas (
  id_consulta SERIAL PRIMARY KEY,
  id_paciente INT REFERENCES mediCare.pacientes(id_paciente) ON DELETE CASCADE,
  id_medico INT REFERENCES mediCare.medicos(id_medico) ON DELETE SET NULL,
  fecha_consulta TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  motivo TEXT,
  diagnostico TEXT,
  tratamiento TEXT
);

```

```sql
-- Consulta analítica de resumen de cantidad de registros por tabla
WITH conteo_tablas AS (
  SELECT 'pacientes' AS tabla, COUNT(*) AS cantidad FROM mediCare.pacientes
  UNION ALL
  SELECT 'medicos', COUNT(*) FROM mediCare.medicos
  UNION ALL
  SELECT 'consultas', COUNT(*) FROM mediCare.consultas
  UNION ALL
  SELECT 'historial_medico', COUNT(*) FROM mediCare.historial_medico
  UNION ALL
  SELECT 'tratamientos', COUNT(*) FROM mediCare.tratamientos
  UNION ALL
  SELECT 'facturacion', COUNT(*) FROM mediCare.facturacion
  UNION ALL
  SELECT 'citas_medicas', COUNT(*) FROM mediCare.citas_medicas
  UNION ALL
  SELECT 'recetas_medicas', COUNT(*) FROM mediCare.recetas_medicas
  UNION ALL
  SELECT 'hospitalizaciones', COUNT(*) FROM mediCare.hospitalizaciones
  UNION ALL
  SELECT 'laboratorios', COUNT(*) FROM mediCare.laboratorios
  UNION ALL
  SELECT 'especialidades', COUNT(*) FROM mediCare.especialidades
  UNION ALL
  SELECT 'usuarios', COUNT(*) FROM mediCare.usuarios
  UNION ALL
  SELECT 'seguros_medicos', COUNT(*) FROM mediCare.seguros_medicos
  UNION ALL
  SELECT 'proveedores', COUNT(*) FROM mediCare.proveedores
  UNION ALL
  SELECT 'notas_evolucion', COUNT(*) FROM mediCare.notas_evolucion
  UNION ALL
  SELECT 'procedimientos_medicos', COUNT(*) FROM mediCare.procedimientos_medicos
  UNION ALL
  SELECT 'equipos_medicos', COUNT(*) FROM mediCare.equipos_medicos
  UNION ALL
  SELECT 'suministros_medicos', COUNT(*) FROM mediCare.suministros_medicos
  UNION ALL
  SELECT 'turnos_guardia', COUNT(*) FROM mediCare.turnos_guardia
)
SELECT tabla, cantidad
FROM conteo_tablas
ORDER BY cantidad DESC;

```

```sql
-- Relaciones entre entidades
SELECT
  rel.relname AS tabla_relacionada,
  ref.relname AS tabla_referenciada,
  att_rel.attname AS columna_relacionada,
  att_ref.attname AS columna_referenciada,
  con.conname AS restriccion
FROM pg_constraint con
JOIN pg_class rel ON con.conrelid = rel.oid
JOIN pg_class ref ON con.confrelid = ref.oid
JOIN pg_attribute att_rel ON att_rel.attrelid = con.conrelid AND att_rel.attnum = ANY(con.conkey)
JOIN pg_attribute att_ref ON att_ref.attrelid = con.confrelid AND att_ref.attnum = ANY(con.confkey)
WHERE con.contype = 'f'
ORDER BY tabla_relacionada;

```


## 🔗 Integración PostgreSQL + Excel

## 🔗 Conexión de PostgreSQL hacia Excel

Una parte clave del proyecto fue la integración entre **PostgreSQL** y **Microsoft Excel** mediante ODBC. Este enlace muestra cómo se realizó la conexión de manera práctica:

[📎 Ver archivo de conexión (Google Drive)](https://drive.google.com/file/d/1oz25Z0FYRzL6zc5_vCEy-cDDmirsADHV/view?usp=sharing)

> Esta conexión permitió cargar dinámicamente las tablas clínicas desde el motor de base de datos `medicare_db` hacia Excel, facilitando la creación de reportes visuales y análisis estratégicos.



# Convert to SQL

**Convert to SQL** is a QGIS plugin that converts vector layers into SQL scripts containing `INSERT` statements for PostgreSQL/PostGIS databases.

The plugin is designed to simplify the transfer of spatial data from QGIS to PostgreSQL by generating SQL statements directly from vector layer features.

## Features

- Generate one SQL `INSERT` statement for each feature in the input vector layer.
- Configure output fields using QGIS field mapping.
- Rename, reorder, remove or create fields using QGIS expressions.
- Transform geometries to a selected target coordinate reference system (CRS).
- Handle attribute values such as `NULL`, text, numbers, booleans and date/time values.
- Escape apostrophes in text values to ensure valid SQL string literals.
- Handle null and empty geometries.
- Export the generated SQL script to a file.
- Wrap the generated statements in a database transaction (`BEGIN` / `COMMIT`).

## Requirements

- QGIS 3.28 or later.
- A PostgreSQL database with PostGIS.

## Installation

### From the QGIS Plugin Repository

Once the plugin is published, it will be available through the QGIS Plugin Manager.

### Manual installation

1. Download or clone this repository.
2. Copy the plugin folder into your QGIS Python plugins directory.
3. Restart QGIS or reload the plugins.
4. Enable **Convert to SQL** in the Plugin Manager.

## Usage

1. Open the **Processing Toolbox** in QGIS.
2. Find the **Convert to SQL** provider.
3. Run **Generate INSERT statement**.
4. Select the input vector layer.
5. Configure the field mapping if necessary.
6. Enter the target PostgreSQL table name using the `schema.table` format.
7. Select the target coordinate reference system.
8. Choose the destination SQL file.
9. Run the algorithm.

The generated SQL file contains one `INSERT` statement per input feature.

Before executing the script, make sure that the target table and its columns exist and are compatible with the generated data.

## Project structure

The plugin is organized into several modules, each with a specific responsibility.

```text
ConvertToSql/
├── core/
│   ├── __init__.py
│   ├── converter.py
│   └── sql_generator.py
├── __init__.py
├── convert_to_sql_insert_algorithm.py
├── convert_to_sql_provider.py
├── convert_to_sql.py
├── icon.png
└── metadata.txt
```

### Main files

| File | Description |
|---|---|
| `__init__.py` | Initializes the plugin and exposes the required entry point to QGIS. |
| `metadata.txt` | Contains plugin metadata such as its name, version, description and license. |
| `convert_to_sql.py` | Handles plugin initialization, registration and removal of the Processing provider. |
| `convert_to_sql_provider.py` | Defines the Convert to SQL Processing provider and registers its algorithms. |
| `icon.png` | Plugin and algorithm icon. |

### Algorithms

- `convert_to_sql_insert_algorithm.py`: Defines the algorithm parameters, runs the conversion and handles the generated SQL output.

### Core

The `core/` directory contains the main conversion and SQL generation logic, separated from the QGIS Processing interface.

- `converter.py`: Handles feature conversion, attribute values and geometry processing, including coordinate transformations.
- `sql_generator.py`: Builds the PostgreSQL SQL statements and formats values as SQL literals.

## Generated SQL example

A simplified example of the generated SQL:

```sql
BEGIN;

INSERT INTO admin.city (name, population, geom)
VALUES (
    'Example',
    1250,
    ST_GeomFromText('POINT(0 0)', 4326)
);

COMMIT;
```

The actual SQL depends on the input layer, selected fields and target CRS.

## Limitations

- The current version generates `INSERT` statements only.
- The target PostgreSQL table must already exist.
- The target table structure must be compatible with the selected field mapping.
- The geometry column and its SRID must be compatible with the generated geometries.

Always review the generated SQL script before executing it against your database.

## Issues and support

If you encounter a problem, discover a bug or have a suggestion for improvement, please check the existing [GitHub Issues](../../issues).

If the problem has not already been reported, feel free to [open a new issue](../../issues/new).

When reporting a bug, please include:

- Your QGIS version.
- Your operating system.
- The processing parameters used.
- A description of the problem.
- The error message or relevant log, if available.

## License

This project is licensed under the **GNU General Public License v3.0 or later (GPL-3.0-or-later)**.

You can find the full license text in the [LICENSE](LICENSE) file.

You are free to use, study, modify and redistribute this software under the terms of the license.

## Contributions

Suggestions, bug reports and contributions are welcome. Please use the GitHub Issues section to discuss bugs or proposed improvements.
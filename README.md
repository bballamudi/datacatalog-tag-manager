# datacatalog-tag-manager

A Python package to manage Google Cloud Data Catalog tags, loading metadata from external
sources. Currently supports the CSV file format.

[![CircleCI][1]][2]

## 1. Environment setup

### 1.1. Python + virtualenv

Using [virtualenv][3] is optional, but strongly recommended unless you use [Docker](#12-docker).

#### 1.1.1. Install Python 3.6+

#### 1.1.2. Create a folder

This is recommended so all related stuff will reside at same place, making it easier to follow
below instructions.

````bash
mkdir ./datacatalog-tag-manager
cd ./datacatalog-tag-manager
````

_All paths starting with `./` in the next steps are relative to the `datacatalog-tag-manager`
folder._

#### 1.1.3. Create and activate an isolated Python environment

```bash
pip install --upgrade virtualenv
python3 -m virtualenv --python python3 env
source ./env/bin/activate
```

#### 1.1.4. Install the package

```bash
pip install --upgrade datacatalog-tag-manager
```

### 1.2. Docker

Docker may be used as an alternative to run `datacatalog-tag-manager`. In this case, please
disregard the [above](#11-python--virtualenv) _virtualenv_ setup instructions.

#### 1.2.1. Get the source code
```bash
git clone https://github.com/ricardolsmendes/datacatalog-tag-manager
cd ./datacatalog-tag-manager
```

### 1.3. Auth credentials

#### 1.3.1. Create a service account and grant it below roles

- BigQuery Metadata Viewer
- Data Catalog TagTemplate User
- A custom role with `bigquery.datasets.updateTag` and `bigquery.tables.updateTag` permissions 

#### 1.3.2. Download a JSON key and save it as
- `./credentials/datacatalog-tag-manager.json`

#### 1.3.3. Set the environment variables

_This step may be skipped if you're using [Docker](#12-docker)._

```bash
export GOOGLE_APPLICATION_CREDENTIALS=./credentials/datacatalog-tag-manager.json
```

## 2. Manage Tags

### 2.1. Create

The metadata schema to create Tags is described below. Use as many lines as needed to create all
the Tags and Fields you need.

| Column              | Description                                           | Mandatory |
| --------------------| ----------------------------------------------------- | :-------: |
| **linked_resource** | Full name of the asset the Entry refers to            |    yes    |
| **template_name**   | Resource name of the Tag Template for the Tag         |    yes    |
| **column**          | Attach Tags to a column belonging to the Entry schema |     no    |
| **field_id**        | Id of the Tag field                                   |    yes    |
| **field_value**     | Value of the Tag field                                |    yes    |

*TIPS* 
- [sample-input/create-tags][4] for reference;
- [Data Catalog Sample Tags][6] (Google Sheets) might help to create/export a CSV file.

#### 2.1.1. From a CSV file 

- Python + virtualenv

```bash
datacatalog-tag-manager create-tags --csv-file CSV_FILE_PATH
```

- Docker

```bash
docker build --rm --tag datacatalog-tag-manager .
docker run --rm --tty \
  --volume CREDENTIALS_FILE_FOLDER:/credentials --volume CSV_FILE_FOLDER:/data \
  datacatalog-tag-manager create-tags --csv-file /data/CSV_FILE_NAME
```

### 2.2. Delete

The metadata schema to delete Tags is described below. Use as many lines as needed to delete all
the Tags you want.

| Column              | Description                                             | Mandatory |
| --------------------| ------------------------------------------------------- | :-------: |
| **linked_resource** | Full name of the asset the Entry refers to              |    yes    |
| **template_name**   | Resource name of the Tag Template of the Tag            |    yes    |
| **column**          | Delete Tags from a column belonging to the Entry schema |     no    |

*TIPS* 
- [sample-input/delete-tags][5] for reference;
- [Data Catalog Sample Tags][6] (Google Sheets) might help to create/export a CSV file.

#### 2.2.1. From a CSV file 

- Python + virtualenv

```bash
datacatalog-tag-manager delete-tags --csv-file CSV_FILE_PATH
```

- Docker

```bash
docker build --rm --tag datacatalog-tag-manager .
docker run --rm --tty \
  --volume CREDENTIALS_FILE_FOLDER:/credentials --volume CSV_FILE_FOLDER:/data \
  datacatalog-tag-manager delete-tags --csv-file /data/CSV_FILE_NAME
```

[1]: https://circleci.com/gh/ricardolsmendes/datacatalog-tag-manager.svg?style=svg
[2]: https://circleci.com/gh/ricardolsmendes/datacatalog-tag-manager
[3]: https://virtualenv.pypa.io/en/latest/
[4]: https://github.com/ricardolsmendes/datacatalog-tag-manager/tree/master/sample-input/create-tags
[5]: https://github.com/ricardolsmendes/datacatalog-tag-manager/tree/master/sample-input/delete-tags
[6]: https://docs.google.com/spreadsheets/d/1bqeAXjLHUq0bydRZj9YBhdlDtuu863nwirx8t4EP_CQ

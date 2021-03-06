# Introduction to Airflow
1. [What is a Workflow](#what-is-a-workflow)
2. [What is Airflow](#what-is-airflow)
3. [DAGs](#dags)
4. [Airflow Dags](#airflow-dags)
5. [Airflow UI](#airflow-ui)

## What is a Workflow?
_A set of steps to accomplish a given data engineerning task_
- Dowloading files, copying data, filtering, writing to a database, etc.
- Scheduled at a time/ triggered by an event

## What is Airflow?
_A platform to program workflows_
- Creation
- Scheduling
- Monitoring
- Workflows are written in Python
- Implements workflows as DAGs: Directed Acyclic Graphs

### Installing and Configuring Airflow
```shell
export AIRFLOW_HOME=~/airflow
pip install apache-airflow
airflow initdb
```
- folders
  -  `dags`: store the dags
  - `test`: unit test the possible deployment, ensure consistency across DAGs
  - `plugins`: store custom operators, and hooks
  - `connections, pools, variables`: provide a location for various configuration files you can import into Airflow

## DAGs
- The set of tasks that make up the workflow
- Consists of the tasks and the dependencies between tasks
- Created with details about the DAG: name, start date, owner, etc.

<img src="https://airflow.apache.org/docs/stable/_images/subdag_before.png">

- DAG Definition Example
```python
etl_dag = DAG(
  dag_id='etl_pipeline',
  default_args={'start_date': '2020-01-08'}
)
```

- Run a workflow
```shell
airflow run <dag_id> <task_id> <start_date>
```

## Airflow DAGs
- Directed: representing depenndencies between components
- Acyclic: does not loop/cycle/repeat
- Graph: actual set of components

### DAG in Python
- Create a DAG
- Edit the individual properties of a DAG

- Define a DAG in Python
```python
from airflow.models import DAG
from datetime import datetime

default_arguments  = {
  'owner': 'vee',
  'email': 'abc@def.gh'
  'start_date': datetime(2020, 1,  1)  // earliest datetime the DAG can be run
}

etl_dag = DAG( 'etl_workflow', default_args = default_arguments )
```

### DAGs on the command Line
- Start Airflow processes
- Manually run DAGs/Tasks
- Get logging information from Airflow
- `airflow -h`: descriptions/help
- `airflow list_dags`: show all recognized DAGs

## Airflow UI
- `Tree View`: lists the tasks and any ordering between them in a tree structure
- `Graph View`: shows any tasks and their dependencies in a graph structure
- `Code`: provides full access to the Python code that makes up the DAG

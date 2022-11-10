## Airflow  
  
Usamos como orquestador a Airflow.  
Con él, se elabora una secuencia de tareas que pueden ser monitoreadas y controladas desde la interfaz gráfica: [Airflow](http://localhost:8080).  
El archivo _users_spotify_ contiene la creación/configuración de las tareas del DAGs (Directed acyclic graph) de Airflow.  

### DAG:

La secuencia de tareas es la siguiente:

create_tables >> api_extract_users >> insert_staging_users_file >> insert_users >> insert_playlists_users >> api_extract_playlist >> insert_staging_playlist_file >> insert_playlist_artist >> insert_traspuesta_artista_userid

* Creación de tablas:  A través de un _PostgresOperator_ accedemos a la carpeta dags/sql/ y creamos las tablas.  
En _create_tables.sql_ se encuentra la creación de las dos tablas iniciales: _public.users_, _public.playlists_.  
  
* Extracción de usuarios: 

* Staging en _users_file.csv_: Seleccionamos algunos campos de usuarios.  
  
* Inserción de info de usuarios: Traemos información de staging.users_file y la casteamos a public.users.  
  
* _Insert playlist users_ 

* La tarea _api extract playlist_ llama a una función, _run_playlist_ que se conecta a la bd de postgres con la función _get_playlists_db_

(ACÁ VOY. DE ACÁ PARA ABAJO ES TEXTO DE GUÍA)

El jupyter notebook contiene el tratamiento, preprocesamiento y análisis de los datos: [Clustering](red_spotify.ipynb).  

Las librerías estandar usadas: [Numpy](https://numpy.org/), [pandas](https://pandas.pydata.org/), [seaborn](https://seaborn.pydata.org/) y [matplotlib](https://matplotlib.org/).  
Para preprocesamiento: [Sklearn](https://scikit-learn.org/stable/).  
Mapeo de base de datos: [SQLAlchemy](https://www.sqlalchemy.org/).  
Grafos: [Networkx](https://networkx.org/).  




######################################################################################
#                       CONFIGURACIÓN DEL DAG - AIRFLOW                              #
######################################################################################


default_args = {
    'owner':'seminario-itba',
    'depends_on_past':False,
    'email':['nicolasarosteguy@gmail.com'],
    'email_on_failure':False,
    'email_on_retry':False,
    'retries':1,
    'retry_delay': timedelta(minutes=5),
}


with DAG(
    'users_spotify',
    default_args=default_args,
    description='segunda prueba',
    schedule_interval=timedelta(days=1),
    start_date=days_ago(2),
    tags=['le pega a la api de spotify y trae playlist de usuarios'],
) as dag:

    create_tables = PostgresOperator(
        task_id="create_tables",
        postgres_conn_id="postgresl_local",
        sql="sql/create_tables.sql",
    )
    api_extract_users = PythonOperator(
        task_id='api_extract_users',
        python_callable=run
    )
    insert_staging_users_file = PostgresOperator(
        task_id="insert_staging_users_file",
        postgres_conn_id="postgresl_local",
        sql="sql/insert_table_stage_users_file.sql",
    )
    insert_users = PostgresOperator(
        task_id="insert_users",
        postgres_conn_id="postgresl_local",
        sql="sql/insert_table_user_final.sql",
    )
    insert_playlists_users = PostgresOperator(
        task_id="insert_playlists_users",
        postgres_conn_id="postgresl_local",
        sql="sql/insert_playlist_users.sql",
    )
    api_extract_playlist = PythonOperator(
        task_id='api_extract_playlist',
        python_callable=run_playlist
    )
    insert_staging_playlist_file = PostgresOperator(
        task_id="insert_staging_playlist_file",
        postgres_conn_id="postgresl_local",
        sql="sql/insert_table_stage_playlist_file.sql",
    )
    insert_playlist_artist = PostgresOperator(
        task_id="insert_playlist_artist",
        postgres_conn_id="postgresl_local",
        sql="sql/insert_playlist_artist.sql",
    )
    insert_traspuesta_artista_userid = PostgresOperator(
        task_id="insert_traspuesta_artista_userid",
        postgres_conn_id="postgresl_local",
        sql="sql/insert_traspuesta_artista_userid.sql",
    )

    create_tables >> api_extract_users >> insert_staging_users_file >> insert_users >> insert_playlists_users >> api_extract_playlist >> insert_staging_playlist_file >> insert_playlist_artist >> insert_traspuesta_artista_userid
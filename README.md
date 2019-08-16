# flask-cheatsheet


### Virtual Envs

1. This command installs the module to allow you to create virtual environemts
(needs to be done once on your computer)
`pip3 install virtualenv`


### Starting a project

##### Did You clone a flask app?

1.  setup your virtual env
```bash
virtualenv .env -p python3
source .env/bin/activate
```
a. `virtualenv .env -p python3` - this sents up the environment with python3
b. `source .env/bin/activate` - this activates the environement *So this is all that needs to be done if you already set up an environment*

c.  Install module - `pip3 install -r requirments.txt` (requirments.txt is like package.json)


##### Starting from scratch?

1. ```mkdir flask-app```
2. ```cd flask-app```
3. 
```
virtualenv .env -p python3
source .env/bin/activate
```
4. Install your depenedencies 
```bash
pip3 install flask-restful peewee flask psycopg2 flask_login flask_cors
```
5.  Add them to `requirements.txt` - `pip3 freeze > requirements.txt`

### Leaving a Virtual Env

- type `deactivate` in the terminal window

### Entering a file that has a virtual environment

- ```source .env/bin/activate```

### Setting up database

*[peewee](http://docs.peewee-orm.com/en/latest/)* - is our orm (comparable to mongoose) gives us the power to talk to a sql database - LOOK AT THE DOCS!!!

1. Setup the db and create a connection to whatever your db is (sqlite, postgresql, mysql) and save it a variable,
`DATABASE = SqliteDatabase('dogs.sqlite')`

2. Setup your model
```python
class Dog(Model):
    name = CharField()
    owner = CharField()
    breed = CharField()
    created_at = DateTimeField(default=datetime.datetime.now)

    class Meta:
        # instructions on what database to connect too, in our current case sqlite
        database = DATABASE
 ```
 
 - Notice the model inheriting the Model class, this gives it the query abilities defined in peewee
 - *Meta* - is instructions for the class, here was can assign the database property to the variable that we defined in *step 1* And will allow our model to talk to the db.
 
 
 3. Initialize the tables
 
 ```python
 def initialize():
    DATABASE.connect() #opening a connection to the db
    DATABASE.create_tables([Dog], safe=True)#the array takes our models and will create tables that match them
    DATABASE.close()
    
```

*safe=True* - says when this method is invoked check to see if the table exists if it doens't the create the table

4.  Use the function in `app.py`

```python
# above 
import models

if __name__ == '__main__':
    models.initialize()
    app.run(debug=DEBUG, port=PORT)
```






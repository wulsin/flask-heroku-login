## Simple app setup instructions
```
# Setup 
$ git clone git@github.com:wulsin/flask-heroku-login.git
$ cd flask-heroku-login
$ python3 -m venv venv
$ source venv/bin/activate
$ pip install -r requirements.txt

# Create local postgres database
$ psql
wwulsin=# \l
wwulsin=# create database flask_heroku_login2;
wwulsin=# CREATE TABLE account(user_id SERIAL, username VARCHAR (50) UNIQUE NOT NULL, password VARCHAR (120) NOT NULL, email VARCHAR (355) UNIQUE NOT NULL, PRIMARY KEY(user_id));
wwulsin=# CREATE TABLE books(isbn VARCHAR (50) UNIQUE NOT NULL, book_title VARCHAR (120) NOT NULL, book_author VARCHAR (120) NOT NULL, publication_year INTEGER, image_url VARCHAR (120), price NUMERIC, PRIMARY KEY(isbn));
wwulsin=# \l
wwulsin=# \q


# Run app locally
$ flask run
# Test that it opens in http://127.0.0.1:5000/

# Deploy to Heroku
$ heroku create flask-heroku-login-wulsin
$ heroku addons:create heroku-postgresql:hobby-dev --app flask-heroku-login-wulsin



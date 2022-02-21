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
# Test that Postman can execute post request to http://127.0.0.1:5000/register?username=test3&email=test3@test.com&password=pass3

# Deploy to Heroku
$ heroku create flask-heroku-login-wulsin
$ heroku addons:create heroku-postgresql:hobby-dev --app flask-heroku-login-wulsin
$ heroku logs  --tail --app flask-heroku-login-wulsin

# Import database to Heroku
# See https://devcenter.heroku.com/articles/heroku-postgres-import-export
# and https://devcenter.heroku.com/articles/bucketeer
$ heroku addons:create bucketeer:hobbyist --app flask-heroku-login-wulsin
# Successfully provisioned S3 bucket: "s3://bucketeer-16fc9b52-25bb-4605-aad8-8b28783f56be"
$ heroku config -s | grep BUCKETEER > .env
# Install AWS CLI if needed from https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html 
$ which aws
	/usr/local/bin/aws
$ aws --version
	aws-cli/2.4.19 Python/3.8.8 Darwin/20.6.0 exe/x86_64 prompt/off
$ aws configure  # get values from `heroku config`
$ echo "<h1>hello, world</h1>" > hello.html
$ aws s3 cp hello.html s3://<BUCKETEER_BUCKET_NAME>/public/hello.html
$ curl https://bucketeer-16fc9b52-25bb-4605-aad8-8b28783f56be.s3.amazonaws.com/public/hello.html
$ aws s3 ls s3://bucketeer-16fc9b52-25bb-4605-aad8-8b28783f56be/public/
$ aws s3 rm s3://bucketeer-16fc9b52-25bb-4605-aad8-8b28783f56be/public/hello.html
# Dump the local database
$ pg_dump -Fc --no-acl --no-owner -h localhost -U wwulsin flask_heroku_login > flask_heroku_login.dump
$ aws s3 cp flask_heroku_login.dump s3://bucketeer-16fc9b52-25bb-4605-aad8-8b28783f56be/public/flask_heroku_login.dump
$ aws s3 presign s3://bucketeer-16fc9b52-25bb-4605-aad8-8b28783f56be/public/flask_heroku_login.dump
$ heroku pg:backups:restore 'https://bucketeer-16fc9b52-25bb-4605-aad8-8b28783f56be.s3.us-east-1.amazonaws.com/public/flask_heroku_login.dump?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVVKH7VVUGPQ4PAFB%2F20220221%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20220221T052225Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Signature=9e8cb43acbfc1740ade1ac2df47cd01f7fb2ffe149e2a046e7ae534d391de1dc' DATABASE_URL --app flask-heroku-login-wulsin

# Confirm that Postman can execute post request to https://flask-heroku-login-wulsin.herokuapp.com/register?username=test4&email=test4@test.com&password=pass4
```


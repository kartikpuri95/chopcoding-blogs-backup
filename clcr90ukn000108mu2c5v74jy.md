# SQL-S3 combination for handling images like a pro

## Introduction

In this tutorial, we will explore how to use the combination of SQL and S3 to efficiently handle images. We will be using Python, Flask, and SQLAlchemy to accomplish this task. Specifically, we will be using the Flask-SQLAlchemy library, which is a convenient wrapper for SQLAlchemy that makes it easy to use in a Flask application.

### Prerequisites

* A basic understanding of Python
    
* Familiarity with Flask and SQLAlchemy
    
* An S3 bucket set up with appropriate access credentials
    
* MySQL installed and set up on your system
    

### Step 1: Setting up the Database

First, we need to set up the database. We will use MySQL for this tutorial. Create a new database and table in MySQL. You could use any SQL database like postgres, sqlite etc.

```python
Copyfrom flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://username:password@host:port/dbname'
db = SQLAlchemy(app)

class Image(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    link = db.Column(db.String(255), nullable=False)

db.create_all()
```

In this step, we are just creating the database connection and creating a table to store the images.

### Step 2: Storing Images on S3

Next, we will create a function to store images on S3. For this, we will use the boto3 library, which is a Python SDK for AWS.

```python
import boto3

def upload_to_s3(image_file):
    s3 = boto3.client('s3', aws_access_key_id=ACCESS_KEY, aws_secret_access_key=SECRET_KEY)
    try:
        s3.upload_fileobj(image_file, BUCKET_NAME, image_file.filename)
        return f"https://{BUCKET_NAME}.s3.amazonaws.com/{image_file.filename}"
    except:
        return "Upload failed"
```

Here, we are using the `upload_fileobj` method to upload the image file to our S3 bucket. The function returns the URL of the image on S3, which we will use to store in the database.

### Step 3: Storing Image URLs in the Database

Once we have the image stored on S3, we can store its URL in the database.

```python
@app.route("/upload", methods=["POST"])
def upload():
    image_file = request.files["image"]
    s3_url = upload_to_s3(image_file)
    if s3_url != "Upload failed":
        image = Image(link=s3_url)
        db.session.add(image)
        db.session.commit()
        return "Image uploaded and URL saved to database"
    else:
        return "Upload failed"
```

We are using a Flask route to handle the image upload and database insertion. This route will handle the POST request from the client side and it will store the image to s3 and stores its url in the database using the SQLAlchemy session object.

### Step 4: Retrieving Images from the Database

Now that we have our images stored in S3 and their URLs stored in the database, we can retrieve them whenever we need to. Here's an example of how to retrieve an image by its ID:

```python
@app.route("/image/<int:id>")
def image(id):
    image = Image.query.get(id)
    if image:
        return redirect(image.link)
    else:
        return "Image not found", 404
```

This route takes an ID as a parameter, and it uses the SQLAlchemy query object to retrieve the image from the database by its ID. If the image is found, it returns the image by redirecting to the S3 url, otherwise returns 404

### Conclusion

In this tutorial, we have learned how to use SQL and S3 together to handle images efficiently. We have learned how to store images on S3 and store their URLs in the database using SQLAlchemy, and how to retrieve images from the database and serve them to the client. This method is highly scalable, and it will allow you to handle large amounts of image data easily.

You could also delete the image from s3 once it's deleted from the database. This would complete the lifecycle of the image. Please keep in mind that the provided code is only a skeleton and you might need to adjust it to your specific use case.

This kind of architecture is useful if you need to access images frequently or if you don't want to use expensive server storage for storing images. And using s3 storage you could take advantage of the many features it provides like, data replication, data transfer acceleration, Content delivery network(CDN), etc.
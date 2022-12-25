# How to upload an image from VueJS to MongoDB using Python

## Introduction

As a tech lead in a company, I have noticed that many people starting out in their careers struggle with uploading images to a database. There is so much information available online that it can be overwhelming and difficult to determine the best way to do it. Personally, I believe in simplicity and keeping code simple and concise. With that in mind, I want to share the simplest way to upload an image in Vue.js.

In this tutorial, we will learn how to upload an image from a Vue.js frontend to a Python backend and store it in a MongoDB database. We will use the Flask web framework and the PyMongo library to create a simple server and connect to the database.

## Prerequisites

Before starting this tutorial, you should have the following installed on your development machine:

* Node.js and npm (the Node.js package manager)
    
* Python 3 and pip (the Python package manager)
    
* MongoDB
    

You should also have some basic knowledge of Vue.js and Python.

### Step 1: Set up the Vue.js frontend

First, create a new Vue.js project using the Vue CLI. Open a terminal and run the following command:

```bash
vue create my-project
```

Follow the prompts to create a new Vue.js project.

Next, we will create a form to allow the user to select an image to upload. Open the `src/components/HelloWorld.vue` file and update it with the following code:

```javascript
<template>
  <form>
    <label for="file">Select an image:</label>
    <input type="file" ref="fileInput" @change="uploadImage"/>
  </form>
</template>

<script>
export default {
  methods: {
    uploadImage() {
      const file = this.$refs.fileInput.files[0]
      const formData = new FormData()
      formData.append('file', file)

      fetch('/upload', {
        method: 'POST',
        body: formData
      }).then(response => {
        console.log(response)
      })
    }
  }
}
</script>
```

This code creates a form with a file input. When the user selects a file and clicks the "Open" button, the `uploadImage` method is called. This method creates a `FormData` object and appends the selected file to it. It then sends a `POST` request to the `/upload` endpoint with the `FormData` object as the request body.

### Step 2: Set up the Python backend

Next, we will create the Python backend to handle the image upload. Create a new directory for the backend, and then create and activate a virtual environment:

```bash
mkdir backend
cd backend
python3 -m venv venv
source venv/bin/activate
```

Install the Flask and PyMongo libraries:

```bash
pip install flask pymongo
```

Create a new file [`app.py`](http://app.py) and add the following code to it:

```python
from flask import Flask, request
from pymongo import MongoClient

app = Flask(__name__)
client = MongoClient()
db = client.mydatabase

@app.route('/upload', methods=['POST'])
def upload():
    file = request.files['file']
    if file:
        db.images.insert_one({'image': file.read()})
        return 'Success'
    return 'No file uploaded'

if __name__ == '__main__':
    app.run()
```

This code creates a Flask app and a MongoClient object. It defines a route `/upload` that accepts `POST` requests. When a request is received, it reads the image data from the `request.files` object and stores it

I hope you found this tutorial helpful. If you have any questions or would like me to cover a specific topic, please leave a comment below.
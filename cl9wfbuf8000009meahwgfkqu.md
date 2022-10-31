# Create an intent classification chatbot using python

Python is one of my favourite programming languages, it's easier to code and comes with a fantastic community. Recently I had a requirement to build a chatbot, primarily an intent classification chatbot.

## What is an intent classification chatbot 

Chatbot which can identify what the user is trying to say and based on that return output is nothing but an intent classification chatbot. In this type of chatbot, all the functions are predefined in the backend and based on the identified intent we execute the function. 

For example, A food delivery app can have a chatbot and one of the questions can be 
> What is my total expenditure on food in the month of January?

Here the intent is to find the total amount spend in the month of January by User X, so once the intent is identified we will fire the function getTotalExpenditure(month)
Let's start building our chatbot for the food delivery app

I have created a flask project and will be using the [snipsnlu](https://snips-nlu.readthedocs.io/en/latest/tutorial.html) library to build our chatbot. Snips NLU is a Natural Language Understanding python library that allows parsing sentences written in natural language, and extracting structured information.

## Training Dataset

Before starting with our code, we first need to train a basic dataset so that our system is tuned for most of the random questions. To train the dataset we need to create a 
*food_delivery_intent.yml* file

Which will look like following 


```yaml
type: intent
name: getTotalExpenditure
slots:
  - name: month
    entity: snips/datePeriod
utterances:
  - Give me my total spend for [month]
  - Get my current [month] expenditure
  - How much i have spent on food in [month]
  - What is my total expenditure on food for [month]
``` 
The above files seem pretty clear but let's go through them step by step
1. Here the type of the file is intent
2. getTotalExpenditure is the name of the intent, this name can be anything but for the sake of simplicity I have made this look like a function name so that once our intent is identified we execute the function
3. Slots are the section in the utterances which can be dynamic for example they can be a place, month, date, etc.
4. The utterances are the sample utterances which have slots in square brackets. Slots can have any of the 12 months so we write them inside the square bracket.

You can read more about the training dataset here [https://snips-nlu.readthedocs.io/en/latest/dataset.html](https://snips-nlu.readthedocs.io/en/latest/dataset.html).

Now it's time to parse the YAML file into JSON using the CLI provided by snipsnlu 

```ssh
snips-nlu generate en /path-to-food-delivery-yaml

snips-nlu generate-dataset en sampledataset/food_delivery_intent.yml > food_delivery_intent.json
```

## Time to run the code.

I have created a flask app to demonstrate the chatbot properly below is the code for my app


```python
import io
import json
import flask
from flask import Flask, jsonify

from snips_nlu import SnipsNLUEngine

app = Flask(__name__)
with io.open("food_delivery_intent.json") as f:
    sample_dataset = json.load(f) #loading the food delivery intent json that we created using cli

nlu_engine = SnipsNLUEngine() ##Sninluengine object

nlu_engine.fit(sample_dataset) ## Loading the dataset in the snipnlu object

@app.route("/parse/", methods=["GET"])
def parse():
    sample_text=flask.request.json['sample_text'] ## sample text from our postman request
    parsing = nlu_engine.parse(sample_text) # Parsing the utterence sent from postman request
    return flask.jsonify(data=parsing)


if __name__ == "__main__":
  app.run(debug=True, host='0.0.0.0', port=5000)
``` 

I have commented on most parts of the code, still, you can access the whole code here and let me know if you have any doubts in the comment section. 

## Let's send the request using the postman

Using postman we will send sample utterances and have a look a the response generated from our flask app using nlu engine.

![Screenshot 2022-10-31 at 12.03.55 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667198061190/Zy8V4gkyW.png align="left")

In the above screenshot, you can see that, we have passed a sample utterance *"How much did I spend in march" *. 
On the right side, you can see the response, where the intent was successfully identified as getTotalExpenditure with a probability of 69% along with that, we can also see a slot as march which is useful for running our code in the backend with the month as the March.

There is an endless possibility using a chatbot and one can fine-tune the chatbot based on the inputs, Above code, just demonstrate how easy it is to make an intent classification chatbot just using python. 

I am attaching a link to the above repository [here](https://github.com/kartikpuri95/chatbot-intent) which is a docker project here, you can simply compose up the docker image and run the project.

I hope you found this article to be useful do share and comment if you like it

https://github.com/kartikpuri95/chatbot-intent




---
title: "Learning RQ Scheduler in Flask by Building a Virtual Pet Game"
datePublished: Mon Jul 31 2023 08:26:00 GMT+0000 (Coordinated Universal Time)
cuid: clkqlvmbi000a09l752jkd0u6
slug: learning-rq-scheduler-in-flask-by-building-a-virtual-pet-game
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690791895749/14c4a5cc-ad10-452f-8e48-fedbab9b3ff5.png
tags: python, flask, python-projects, rq

---

### Introduction

In this article, we are about to embark on a fantastic journey to understand the Redis Queue (RQ) Scheduler within the Flask framework, powered by Python. We are going to do this by creating a simple, yet highly educational game - a "Virtual Pet". Why a game, you might ask? Games are not only fun but they can also provide a highly interactive learning experience. This will help us better understand the role of an RQ Scheduler in managing asynchronous tasks in our Flask applications. Let's dive in!

### Understanding the Virtual Pet Game

The Virtual Pet Game is designed to help understand the capabilities of the RQ Scheduler, by making certain tasks in the game occur in real-time and others at scheduled intervals.

The game revolves around taking care of a pet with three main actions: feeding it, playing with it, and letting it sleep. Each action affects the pet's state, namely, its hunger and energy levels.

#### Game Mechanics:

1. **Feeding:** This action decreases the pet's hunger level immediately. However, the pet's hunger gradually increases over time. We schedule a task to increase the pet's hunger every hour using RQ Scheduler.
    
2. **Playing:** Playing with the pet increases its hunger and decreases its energy. This action occurs instantly when the user chooses to play with the pet.
    
3. **Sleeping:** Putting the pet to sleep increases its energy level, but the pet cannot do anything else while asleep. We schedule a task to wake up the pet after a certain period (20 minutes in our case). While the pet is asleep, the user can't play with it or feed it.
    

#### User Interaction:

The user interacts with the game through a web interface, with buttons for each of the three main actions. Clicking on these buttons triggers the corresponding action on the server.

For example, if the user clicks on the 'Feed' button, a request is sent to the '/feed\_pet/&lt;name&gt;' route, where the 'name' is the pet's name. The server then immediately executes the 'feed' function for that pet.

The 'Play' and 'Sleep' actions work similarly. The 'Sleep' action additionally schedules a task to wake the pet up after 20 minutes.

With this setup, the game mimics a real-life pet scenario where a pet gets hungry over time, needs to rest after play, and needs to wake up after enough sleep.

### Getting Started

Before we start, we'll need a few tools in our arsenal. Make sure you have the following installed:

* Python 3
    
* Flask
    
* Redis
    
* RQ Scheduler
    

For this tutorial, we are assuming that you have a basic understanding of Python, Flask and Redis.

### Setting up RQ Scheduler with Flask

We'll start by setting up Flask with RQ Scheduler. You can create a Flask application and configure it with Redis as follows:

```python
from flask import Flask
from redis import Redis
from rq_scheduler import Scheduler

app = Flask(__name__)
app.redis = Redis()
app.scheduler = Scheduler(connection=app.redis)
```

### Creating the Virtual Pet Model

Next, let's create our Virtual Pet model. The model will include methods for each of the user's possible interactions, which modify the pet's state:

```python
class VirtualPet:
    def __init__(self, name):
        self.name = name
        self.hunger = 0
        self.energy = 10

    def feed(self):
        self.hunger -= 5
        if self.hunger < 0:
            self.hunger = 0

    def play(self):
        self.hunger += 1
        self.energy -= 2

    def sleep(self):
        self.energy += 5
```

### Scheduling Tasks

Now that we have our pet, let's create some tasks that need to be scheduled:

* Increase pet's hunger over time.
    
* Wake up the pet after some time has passed since it went to sleep.
    

This can be done using RQ Scheduler as follows:

```python
from datetime import timedelta

def increase_hunger(pet):
    pet.hunger += 1

def wake_up(pet):
    pet.energy = 10

@app.route('/feed_pet/<name>')
def feed_pet(name):
    pet = get_pet(name)
    pet.feed()
    return 'Fed the pet'

@app.route('/play_with_pet/<name>')
def play_with_pet(name):
    pet = get_pet(name)
    pet.play()
    return 'Played with the pet'

@app.route('/put_pet_to_sleep/<name>')
def put_pet_to_sleep(name):
    pet = get_pet(name)
    pet.sleep()
    app.scheduler.enqueue_in(timedelta(minutes=20), wake_up, pet)
    return 'Put the pet to sleep'

# Scheduled task to increase hunger every hour
app.scheduler.schedule(
    scheduled_time=datetime.utcnow(), # Time for first execution
    func=increase_hunger, # Function to be queued
    args=(pet,), # Arguments passed into function when executed
    interval=3600, # Time before the function is called again, in seconds
    repeat=None, # Repeat this number of times (None means repeat indefinitely)
)
```

### Conclusion

Through the Virtual Pet game, we have explored the workings of the RQ Scheduler within a Flask application. By using a fun, interactive setting, we've demonstrated how RQ Scheduler can handle tasks that need to be executed at later time points, ensuring our application remains efficient and user-friendly. As we've seen, it is a powerful tool that can greatly enhance your web applications, making it easier to manage and execute asynchronous tasks.

### Further Reading

RQ Scheduler offers many more features, such as the ability to retry failed jobs, schedule jobs to be run at certain times of day, and more. Be sure to check out the [RQ Scheduler documentation](https://github.com/rq/rq-scheduler) to explore these features.

### Future Steps

In our exploration so far, we've built the backend of our Virtual Pet Game. However, the game is not just about scheduled tasks and server responses; it's also about the player's experience and interaction with the pet.

If you're interested, I can take this tutorial a step further by wiring up an actual front end to this backend. We could design a simple and intuitive UI with a cute virtual pet that users can interact with. This way, we'll bring our virtual pet to life, not just in code but in a more visual and interactive sense!

So, let me know! If there's enough interest, I would be more than happy to guide you through the process of connecting a front end to our Flask application and making our game even more engaging.

Remember, learning is a community journey. Your feedback and engagement can shape the direction we take in our subsequent tutorials. So, don't hesitate to leave your comments, questions, and suggestions.

Until next time, happy coding!

---
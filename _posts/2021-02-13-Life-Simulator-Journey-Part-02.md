# Journey Codding a Life Simulator Part 02

### Entities Class

Last time I went over the *Person* object. Quick recap: Every person in a simulation has randomly generated traits and attributes that will influence their choices and the way they interact with their environment and other people around them. 

To be able to manage every person generated in this world, I created a class called *Entities* that would contain all the generated person objects. 

An array called *entities_members* is assigned to this class and this array will contain every generated person object. We will not be creating these objects through our *main.py* file and storing them in variables anymore. We will generate them like this now:

```python
#file: main.py
world = Entities.entities()
world.addEntity()
```

It works like this:

- We first create a *world* object that will contain all the other person objects.

- Then, we call on the *addEntity* method of *world*, which is defined like this and will generate people for us and storing them in the *entities_members* array: 
  
  ```python
  #file: Entities.py
  def addEntity(self):
        self.entities_members.append(Person.person(len(self.entities_members)))
  ```
  
- As you can see, when generating a new person object, we're passing through a number that will serve as an ID for every person. Every person will have a unique ID and this new attribute will be helpful later on when we want to access information contained in our world. It actually corresponds to a person's index in the *entities_members* array.

Okay, so now if we want to, as an example  create six people and display the fifth person's last name, we can do it like this:

```python
#file: main.py
for i in range(6):
        world.addEntity()
print(world.entities_members[4].last_name)
```
As you can see, it's very simple now to get information on everyone. To list information on everyone on the same time, we can use the following method that'll show all generated people first and last names:

```python
#file Entities.py
 def listEntities(self):
        print("Current member list:")
        for i in range(len(self.entities_members)):
            print(str(i + 1) + ". " + self.entities_members[i].first_name + " " + self.entities_members[i].last_name)
```

### Family Class

The *Family* class will work like *Entities* but instead of accessing the whole world, will only be accessing members of a given family. This class has only two attributes, *members* and *last_name*. Every time a new person is generated, a family object will also be created:
```python
self.family = Family.family(self.last_name)
self.family.addMember(self.id)
```
- As you can see we are only passing through the *ID* of a person and no other information because we can access everything else using the world object.
  
The *Family* class will be very useful for the reproduction system, (coming soon in Part 3).

### It's Time to Say Goodbye

This is it for today! In the next part, I'll show the basic version of the reproduction process and how it works. 

If you want to take a look at the code, go over to [https://github.com/therokdaba/Simulator](https://github.com/therokdaba/Simulator)  (what is described in this post is from Commit 4) and if you have any questions or remarks don't hesitate to reach out on discord to *therokdaba#9872*. 

Goodbye and see you next time :)!
## Journey Coding a Life Simulator Part 03

Hello again! [Last time](https://therokdaba.github.io/2021/02/13/Life-Simulator-Journey-Part-02.html) I went over the Entity and the Family classes and I mentioned that we would be going over the reproduction process in part 3. So here we are...

### Basic Reproduction Process
For now the reproduction process is very simple, it will be improved later on and more conditions will be added. The process is simple, a person will decide to look for a suitable partner and if they find one they will move on to reproducing and creating a child. I created a function called *reproductionProcess* that takes care of the search and the creation of the child:

```python
#file:Person.py
def reproductionProcess(self, world):
        partner = self.findPartner(world)
        if partner != False:
            self.childCreation(partner, world)
```
As you can see, it first calls the function *findPartner* that will look for a suitable partner within an environment and this function will either return a partner or return a False value if no one is available. Let's take a closer look at the way this function looks for a partner:

```python
#file:Person.py, function:findPartner(self, world)
    while ((self.gender == partner.gender or not self.partnerCompatibilityTest(partner)) and len(partner_list) > 0: 
        partner = UtilityFunctions.chooseRandomItemInArray(partner_list)
        partner_list.remove(partner)
    if len(partner_list) <= 0:
        print("No partner available!")
        return False
    print("Partner found... " + partner.first_name + " " + partner.last_name + " for " + self.first_name + " " + self.last_name + ".")
```
This while loop will go through a list of possible partners in a given world, *partner_list* and will choose randomly a partner and then check that the partner does not have the same gender as the person looking to procreate (this simulation imitates the biological reproduction process) and that the partner passes the compatibility test. If it's all good, the partner will be chosen and we can move on to adding them to the other person's family, otherwise if they are no suitable the reproduction process will stop. *(Note: there is no need to check that partner is not self because we are making sure that the gender between two partners is different and so even if self is in the pool of possible partners, self will never be chosen)*

We will go back to the *partnerCompatibilityTest* function later but in the meantime, let's look at the process of adding the female partner to the family: 

```python
#file:Person.py, function:findPartner(self, world)
        couple_last_name = ""
        if self.gender == "female":
            del(world.entities_members[self.id].family)
            print("Family of " + self.first_name + " " + self.last_name + " deleted...")
            world.entities_members[self.id].last_name = partner.last_name
            couple_last_name = partner.last_name
            partner.family.addMember(self.id)
        else:
            del(world.entities_members[partner.id].family)
            print("Family of " + partner.first_name + " " + partner.last_name + " deleted...")
            world.entities_members[partner.id].last_name = self.last_name
            couple_last_name = self.last_name
            world.entities_members[self.id].family.addMember(partner.id)
        print("They are together and now called " + self.first_name + " and " + partner.first_name + " " + couple_last_name + ".")
        return partner
```
*(Note: the simulation is following the most common way of passing down last names which is that the wife gets her husband last name and their kids do the same as we live in a patriarchal society. This will change but for now since it's a basic version of the reproduction process, I decided to choose the most common practice and later I'll add suitable probabilities to decide how it goes)*

It's pretty straightforward, it just finds the female partner, deletes her previous family, gives her the male partner's last name and enters her into the new family that the child will join later.

Okay so that's it for the *findPartner* function, let's go over the *partnerCompatibilityTest* function:
```python
#file:Person.py
 def partnerCompatibilityTest(self, partner):
        compatibility = self.beauty - partner.beauty
        print("Compatibility for " + self.first_name + " " + self.last_name + " and " + partner.first_name + " " + partner.last_name + ": " + str(compatibility))
        if compatibility >= -30 and compatibility <= 30:
            return True 
        else:
            return False
```
This function is also pretty simple, it just calculates the difference in beauty levels between the two possible partners and just checks that this difference is contained between -30 and 30. If it is, then the partner is suitable.

Last but not least, the *childCreation* function. After a partner is found, this function is called and will simply create a child with the same last name as the *main parent*:
```python
#file:Person.py
    def childCreation(self, partner, world):
        if self.gender == "male":
            main_parent = self
            other_parent = partner
        elif partner.gender == "male":
            main_parent = partner
            other_parent = self 
        world.addEntity(main_parent.last_name)
        kid = world.entities_members[len(world.entities_members)-1]
        main_parent.family.addMember(kid.id)
        print(kid.first_name + " is in the " + kid.last_name + " family and " + kid.pronoun[1] + " dad is " + main_parent.first_name + " and " + kid.pronoun[1] + " mom is " + other_parent.first_name + ".")
        main_parent.family.listMembers(world)
```
*(Note: I decided to go with main parent and other parent in this function just to prepare for the probabilities that will be added later and this way I won't have to change the variables names)*

It calls the *addEntity* function of the world object and passes through the last name of the *main parent* and then adds the kid to the *main parent*'s family.

### It's Time to Say Goodbye
This is it for today! This was a very basic version of the reproduction system that will be improved upon later but for now it's effective and helps us slowly add more people to our world.

If you want to take a look at the code, go over to [https://github.com/therokdaba/Simulator](https://github.com/therokdaba/Simulator)  (what is described in this post is from Commit 7) and if you have any questions or remarks don't hesitate to reach out on discord to *therokdaba#9872*. 

Goodbye and see you next time :)!

Check out [Part 01](https://therokdaba.github.io/2021/02/12/Life-Simulator-Journey-Part-01.html) and [Part 02](https://therokdaba.github.io/2021/02/13/Life-Simulator-Journey-Part-02.html)!

Go back to the [homepage](https://therokdaba.github.io/) of this website.
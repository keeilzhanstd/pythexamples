# @classmethod

Декоратор @classmethod позволяет обращатся напрямую к полям и методам класса а не его экземпляра.  
Поэтому, если вы хотите получить доступ к свойству класса в целом, а не к свойству конкретного экземпляра этого класса, используйте classmethod.

###### Code
```python
class Club:

    CLUBS_COUNT = 0

    def __init__(self, name) -> None:
        Club.CLUBS_COUNT = Club.CLUBS_COUNT + 1
        self.name = name

    @classmethod
    def getTotalNumberOfClubs(cls):
        print("Number of clubs in this city: ", cls.CLUBS_COUNT)


malibu = Club("Malibu")
manhattan = Club("Manhattan")
havana = Club("Havana")
cyberpunk = Club("Cyberpunk")

Club.getTotalNumberOfClubs()
malibu.getTotalNumberOfClubs()
print(havana.CLUBS_COUNT)
```
###### Output

> Number of clubs in this city:  4  
Number of clubs in this city:  4  
4  


``` Club.getTotalNumberOfClubs()``` позволяет нам получить поле CLUBS_COUNT относящееся к классу ```Club```.
Однако, используя ```malibu.getTotalNumberOfClubs()``` и ```print(havana.CLUBS_COUNT)``` выдают нам аналогичный результат, посколько все экземпляры класса делят между собой поле ```CLUBS_COUNT``` из класса ```Club```

***

Представьте что вы владелец франшизы OLOLO.  
Вы ведете бизнес связанный с клубами и отмечаете, что данные клубы принадлежат вашей франшизе.

###### Code
```python
class Club:
    FRANCHISE = "OLOLO"
    def __init__(self, name) -> None:
        self.name = name

malibu = Club("Malibu")
manhattan = Club("Manhattan")
havana = Club("Havana")
cyberpunk = Club("Cyberpunk")

print(manhattan.FRANCHISE)
print(havana.FRANCHISE)
print(cyberpunk.FRANCHISE)
 ```
 ###### Output

> OLOLO  
OLOLO  
OLOLO  
  
***
  
Но в какой-то момент приходит клубный-магнат из IT-academy и предлагает вам продать все свои клубы под его франшизу за боснословные деньги.  
И вы недолго думая соглашаетесь.

<img src="https://github.com/keeilzhanstd/pythexamples/blob/main/3BB51116-3552-4650-A061-DAC356D2C7E8.jpeg" width="350px">

###### Code
```python
class Club:
    FRANCHISE = "OLOLO"

    def __init__(self, name) -> None:
        Club.CLUBS_COUNT = Club.CLUBS_COUNT + 1
        self.clubId = Club.CLUBS_COUNT
        self.name = name
        
    @classmethod
    def setClubsFranchise(cls, franchise):
        cls.FRANCHISE = franchise

malibu = Club("Malibu")
manhattan = Club("Manhattan")
havana = Club("Havana")
cyberpunk = Club("Cyberpunk")

# Перед продажой
print(manhattan.FRANCHISE)
print(havana.FRANCHISE)
print(cyberpunk.FRANCHISE)

# Сделка совершена. Все Клубы переходят под франшизу IT Academy
Club.setClubsFranchise("IT Academy")
print(havana.FRANCHISE)
print(cyberpunk.FRANCHISE)
```

 ###### Output

> OLOLO  
OLOLO  
OLOLO  
IT Academy  
IT Academy  
  

# @ regular method
  
Однако вам так нравился ваш любимый клуб *Малибу*, что вы решаетесь вновь выкупить его.  

<img src="https://github.com/keeilzhanstd/pythexamples/blob/main/A866A557-5D2C-46D5-ADC0-60D87EE31FF1.jpeg" width="350px">

Однако при использовании ```malibu.setClubsFranchise("OLOLO")```. Окажется что вы вновь владелец всех своих клубов.  
 ###### Output
> Клубы перед продажей  
        Malibu: OLOLO  
        Manhattan: OLOLO  
        Havana: OLOLO  
        Cyberpunk: OLOLO  
Клубы куплены франшизей IT Academy  
        Malibu: IT Academy  
        Manhattan: IT Academy  
        Havana: IT Academy  
        Cyberpunk: IT Academy  
Выкуп Malibu состоялся.  
        Malibu: OLOLO  
        Manhattan: OLOLO  
        Havana: OLOLO  
        Cyberpunk: OLOLO  

Чтобы выкупить лишь один клуб, вы больше не можете использовать метод класса. Поскольку в таком случае этом метод перезапишет поле ```FRANCHISE``` для класса ```Club```, а следовательно изменится франшиза всех экземпляров класса использующие данное поле.   
Поэтому владелец *IT Academy* позволит вам купить *Малибу* с помощью обычного метода ```malibu.changeFranchiseOfThisClub("OLOLO")```  
При выполнении данного метода поле ```FRANCHISE``` в экземпляре *malibu* перезапишется новым значением.  
При этом если владелец *IT Academy* захочет продать все свои клубы командой ```setClubsFranchise()``` ваш клуб не будет продан иным лицам.  
Так как теперь он имеет свое поле ```FRANCHISE``` независящее от других.

###### Code
```python
class Club:
    FRANCHISE = "OLOLO"

    def __init__(self, name) -> None:
        self.name = name

    def changeFranchiseOfThisClub(self, franchise):
        self.FRANCHISE = franchise

    @classmethod
    def setClubsFranchise(cls, franchise):
        cls.FRANCHISE = franchise


malibu = Club("Malibu")
manhattan = Club("Manhattan")
havana = Club("Havana")
cyberpunk = Club("Cyberpunk")

clubs = [malibu, manhattan, havana, cyberpunk]

def printOutClubs(clubs):
    for club in clubs:
        print(f'\t{club.name}: {club.FRANCHISE}')

# Перед продажой
print("Клубы перед продажей")
printOutClubs(clubs)

# Сделка совершена. Все Клубы переходят под франшизу IT Academy
Club.setClubsFranchise("IT Academy")
print("Клубы куплены франшизей IT Academy")
printOutClubs(clubs)

# Вы выкупаете свой любимый клуб
malibu.changeFranchiseOfThisClub("OLOLO")
print("Выкуп Malibu состоялся.")
printOutClubs(clubs)

# IT Academy Продает свои клубы MAYRAM
Club.setClubsFranchise("MAYRAM")
print("Клубы IT Academy Проданы MAYRAM")
printOutClubs(clubs)
```

###### output
>Клубы перед продажей  
        Malibu: OLOLO  
        Manhattan: OLOLO  
        Havana: OLOLO  
        Cyberpunk: OLOLO  
Клубы куплены франшизей IT Academy  
        Malibu: IT Academy  
        Manhattan: IT Academy  
        Havana: IT Academy  
        Cyberpunk: IT Academy  
Выкуп Malibu состоялся.  
        Malibu: OLOLO  
        Manhattan: IT Academy  
        Havana: IT Academy  
        Cyberpunk: IT Academy  
Клубы IT Academy Проданы MAYRAM  
        Malibu: OLOLO  
        Manhattan: MAYRAM  
        Havana: MAYRAM  
        Cyberpunk: MAYRAM  
        
 > Обратите внимание что ```changeFranchiseOfThisClub(self, franchise)``` требует передавать параметр ```self```, что означает что функция будет работать конкретно с экземляром класса.  
 > В то время как ```setClubsFranchise(cls, franchise)``` принимает параметр cls, под которым подразумевается непосредственно сам класс Club.

# @staticmethod

@staticmethod — используется для создания метода, который ничего не знает о классе или экземпляре, через который он был вызван. Он просто получает переданные аргументы, без неявного первого аргумента, и его определение неизменяемо через наследование.  

Проще говоря, @staticmethod — это вроде обычной функции, определенной внутри класса, которая не имеет доступа к экземпляру, поэтому ее можно вызывать без создания экземпляра класса.

Продолжим тему с клубами.  
Допустим мы имеем два клуба с разной политикой для входа.
###### Допустим это охранник перед входом
```python
class Validator:
    def __init__(self, age, face) -> None:
        self.age = age
        self.face = face

    def validateAge(self, age):
        if age < self.age:
            return False
        else:
            return True

    def validateFace(self, face):
        if(face == self.face):
            return False
        else:
            return True
```
###### Посетитель
```python
class Visitor:
    def __init__(self, name, age, appearance, money):
        self.name = name
        self.age = age
        self.appearance = appearance
        self.money = money
```
###### Клуб
> Обратите внимание как работает инициализация Age и Face контроля в данном классе. Мы инициализируем объект класса ```Validator``` отправляя туда политику клуба. В дальнейшем экземляр класса ```Club``` будет обращатся к данному объекту ```AgeFaceControl``` для проверки допуска посетителя в клуб.

```python
class Club:

    def __init__(self, name, politics, age, appearance) -> None:
        self.name = name
        self.politics = politics
        self.visitors = []
        self.AgeFaceControl = Validator(
            age=age, face=appearance)

    def validateVisitor(self, visitor):
        return self.AgeFaceControl.validateAge(visitor.age) and self.AgeFaceControl.validateFace(visitor.appearance)

    def askPermissionToEnter(self, visitor):
        if self.politics == "Strict":
            if self.validateVisitor(visitor):
                self.visitors.append(visitor)
                print(f'{visitor.name} can come in.')
            else:
                print(f'Sorry {visitor.name} we can\'t do anything with that.')
        else:
            if self.validateVisitor(visitor):
                print(visitor.name + " can come in.")
                self.visitors.append(visitor)
            else:
                if visitor.money > 1000:
                    self.visitors.append(visitor)
                    print(f'{visitor.name} can come in.')
                else:
                    print(
                        f'Sorry {visitor.name} we can\'t do anything with that.')

```

###### Скрипт для проверки пары посетителей в клубы с разной политикой входа.
```python
malibu = Club("Malibu", "Strict", 25, "ugly")
cyberpunk = Club("Cyberpunk", "Ease", 21, "ugly")


visitors = [
    Visitor("Peter", 27, "Not ugly", 500), # Age: Ok, Appearance: Ok, Money: Bad. Allowed in malibu and cyberpunk, because he looks okay, and old enough.
    Visitor("Marie", 20, "Not ugly", 1500), # Age: Bad, Appearance: Ok, Money: Ok. Allowed in cyberpunk, cause have money!
    Visitor("Ben", 44, "ugly", 50000), # Age: Ok, Appearance: Bad, Money: Good! Allowed in cyberpunk, cause have money!
    Visitor("Rose", 23, "ugly", 700) # Age: Ok, Appearance: Bad, Money: Bad. Not allowed anywhere.
]

def askForPermissions(club, visitors):
    print(f'{club.politics} politics at {club.name}.')
    for person in visitors:
        club.askPermissionToEnter(person)


print("\n")
askForPermissions(malibu, visitors)

print("\n")
askForPermissions(cyberpunk, visitors)

print("\n")
print(f'Visitor count at Malibu: {len(malibu.visitors)}')
print(f'Visitor count at Cyberpunk: {len(cyberpunk.visitors)}')
```

###### Output
>Strict politics at Malibu.  
Peter can come in.  
Sorry Marie we can't do anything with that.  
Sorry Ben we can't do anything with that.  
Sorry Rose we can't do anything with that.  


>Ease politics at Cyberpunk.  
Peter can come in.  
Marie can come in.  
Ben can come in.  
Sorry Rose we can't do anything with that.  


>Visitor count at Malibu: 1  
Visitor count at Cyberpunk: 3  


<img src="https://github.com/keeilzhanstd/pythexamples/blob/main/275BC64D-1AAC-4A20-BB1A-97B317D31128.jpeg" width="450px">
***

Но что если я хочу проверить могу ли я ходить в клубы в целом.  
Представим что стандартная политика для входа в клуб в моем городе
  * Быть старше 21
  * Иметь более-менее приятную внешность

В таком случае можно внести изменения в наш код для *охраников клуба* и *клубов*:
###### Охранники клуба
```python
class Validator:
    def __init__(self, age, face) -> None:
        self.age = age
        self.face = face

    def validateAge(self, age):
        if age < self.age:
            return False
        else:
            return True

    def validateFace(self, face):
        if(face == self.face):
            return False
        else:
            return True

    @staticmethod
    def validateAge(age):
        if age < 21:
            return False
        else:
            return True

    @staticmethod
    def validateFace(face):
        if(face == "ugly"):
            return False
        else:
            return True
```

###### Клубы в целом
```python
class Club:

    def __init__(self, name, politics, age, appearance) -> None:
        self.name = name
        self.politics = politics
        self.visitors = []
        self.AgeFaceControl = Validator(
            age=age, face=appearance)

    def validateVisitor(self, visitor):
        return self.AgeFaceControl.validateAge(visitor.age) and self.AgeFaceControl.validateFace(visitor.appearance)

    @staticmethod
    def validateVisitor(visitor):
        return Validator.validateAge(visitor.age) and Validator.validateFace(visitor.appearance)

    def askPermissionToEnter(self, visitor):
        if self.politics == "Strict":
            if self.validateVisitor(visitor):
                self.visitors.append(visitor)
                print(f'{visitor.name} can come in.')
            else:
                print(f'Sorry {visitor.name} we can\'t do anything with that.')
        else:
            if self.validateVisitor(visitor):
                print(visitor.name + " can come in.")
                self.visitors.append(visitor)
            else:
                if visitor.money > 1000:
                    self.visitors.append(visitor)
                    print(f'{visitor.name} can come in.')
                else:
                    print(
                        f'Sorry {visitor.name} we can\'t do anything with that.')
```

Круто теперь я могу проверить могу ли я ходить в клубы!
```python
me = Visitor("Me", 21, "Not ugly", 2000)
print("Just me:")
print("Can go party!") if Club.validateVisitor(me) else print("Can not...") 
# Данный метод не относится к конкретному экземпляру класса (в нашем случае клуб в который мы бы хотели сходить). И мы можем проверить, можно ли нам в целом ходить в клубы.
```
###### Output
>Just me:  
Can go party!  

Также я могу подойти к охранику клуба и спросить, могу ли я посещать клубы по тем или иным критериям?
> Учтите, здесь мы не обращаемся к охранику определенного клуба. Можно сказать мы говорим с охраником свободным от предпосылок и лимитов которые ему ставит его начальство.
```python
print("You are allowed to enter. You look okay, for most of the clubs in this city.") if Validator.validateFace(
    me.appearance) else print("You look like a monster!")
print("You are allowed to enter. You age is okay, for most of the clubs in this city.") if Validator.validateAge(
    me.age) else print("Go drink some milk!")
```

###### Output
>You are allowed to enter. You look okay, for most of the clubs in this city.  
You are allowed to enter. You age is okay, for most of the clubs in this city.  

Прекрасно, напоследок хотел проверить могут ли мой старший и младший брат посещять клубы вместе со мной!

```python
myBrother = Visitor("Brother", 24, "ugly", 2000)
print("Ugly Big Bro:")
print("Can go party!") if Club.validateVisitor(
    myBrother) else print("Can not...")
print("You are allowed to enter. You look okay, for most of the clubs in this city.") if Validator.validateFace(
    myBrother.appearance) else print("You look like a monster!")

myLittleBrother = Visitor("Little Brother", 13, "Not ugly", 0)
print("Handsome Little Bro:")
print("Can go party!") if Club.validateVisitor(
    myLittleBrother) else print("Can not...")
print("You are allowed to enter. You age is okay, for most of the clubs in this city.") if Validator.validateAge(
    myLittleBrother.age) else print("Go drink some milk!")
```

###### Output
>Ugly Big Bro:
Can not...
You look like a monster!  
Handsome Little Bro:
Can not...
Go drink some milk!  

Все просто замечательно!  
Теперь пожалуй я пойду в клуб, не опасаясь, что увижу там кого-то из своих братьев.

<img src="https://github.com/keeilzhanstd/pythexamples/blob/main/8A14CFFE-4DAE-45EE-A569-06305490AE18.jpeg">

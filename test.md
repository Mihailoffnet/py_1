#!/usr/bin/env python
# coding: utf-8

# # ООП-1
# 
# Булыгин Олег:  
# * [LinkedIn](linkedin.com/in/obulygin)  
# * [Мой канал в ТГ по Python](https://t.me/pythontalk_ru)

# In[1]:


print(type('Hello world'))


# In[2]:


# при помощи функции dir мы можем посмотреть все методы класса
print(dir(str))


# Создаем свой первый класс. Он пустой, но работает!

# In[3]:


class Character:
    pass


# Объект — отдельный представитель класса, имеющий конкретное состояние и поведение, полностью определяемое классом.

# In[4]:


peter = Character()

print(type(peter))


# Атрибуты — переменные внутри класса, которая хранит какую-то информацию, о нем.
# 

# In[10]:


# добавим нашему классу несколько атрибутов
class Character:
    name = ''
    power = 0 
    energy = 100
    hands = 2


# In[15]:


# а где хранятся все атрибуты после объявления класса?
print(Character.__dict__)


# In[8]:


# при вызове класса мы всегда создаем новый объект
# у конкретного экземпляра будут все те же атрибуты, что и у его класса
peter = Character()
print(peter.name)
print(peter.power)
print(peter.energy)
print(peter.hands)

# они берутся именно из Character.__dict__ (т.к. не менялись)
print(peter.__dict__)


# In[11]:


# мы можем экземпляру присвоить свои атрибуты
peter.name = 'Peter Parker'
peter.power = 70


# In[12]:


# и даже те, которых изначально в классе нет
peter.alias = 'Spider-Man'
print(peter.alias)


# In[14]:


print(peter.name)
print(peter.power)
print(peter.energy)
print(peter.hands)

# измененные атрибуты уже будут храниться в словаре самого экземпляра
print(peter.__dict__)
print(Character.__dict__)


# In[19]:


# создадим еще один экземпляр класса
bruce = Character()
bruce.name = 'Bruce Wayne'
bruce.power = 85
bruce.alias = 'Batman'

print(bruce.name)
print(bruce.power)
print(bruce.energy)
print(bruce.hands)
print(bruce.alias)
print(bruce.__dict__)

# Придумаем несколько методов для нашего класса
# 

# In[20]:


class Character:
    name = ''
    power = 0
    energy = 100
    hands = 2
# мы видим, что аргумент self ссылается на конкретный экземпляр класса (который еще не создан).
# Его обязательно нужно прописывать, чтобы показывать то, 
# что все действия будут происходить именно с тем объектом, к которому мы применяем метод    
    def eat(self, food):
        if self.energy < 100:
            self.energy += food
        else:
            print('Not hungry')
    
    def do_exercise(self, hours):
        if self.energy > 0:
            self.energy -= hours * 2
            self.power += hours * 2
        else:
            print('Too tired')
    
    def change_alias(self, new_alias):
        print(self) # просто посмотрим, для чего тут self?
        self.alias = new_alias
        

# In[21]:


# еще раз проиницаилизируем создание экземпляра
bruce = Character()
bruce.name = 'Bruce Wayne'
bruce.power = 85


# In[22]:


# пока нет псевдонима
# print(bruce.alias)


# In[23]:


# добавим псевдоним
bruce.change_alias('Batman')
print(bruce.alias)


# In[24]:


# изменим псевдоним
bruce.change_alias('Dark Knight')
print(bruce.alias)


# In[27]:


# проверим методы
print(bruce.power)
print(bruce.energy)
print('--------------------')
bruce.do_exercise(1)
print(bruce.power)
print(bruce.energy)
print('--------------------')
bruce.do_exercise(2)
print(bruce.power)
print(bruce.energy)
print('--------------------')
bruce.eat(5)
print(bruce.power)
print(bruce.energy)


# Проблема с инициализацией параметров изменяемыми типами

# In[28]:


class Character:
    name = ''
    power = 0
    energy = 0
    hands = 2
    backpack = [] # добавляем атрибут с изменяемым типом – списком
    
    def eat(self, food):
        if self.energy < 100:
            self.energy += food
        else:
            print('Not hungry')
        
    
    def do_exercise(self, hours):
        if self.energy > 0:
            self.energy -= hours * 2
            self.power += hours * 2
        else:
            print('Too tired')
    
    def change_alias(self, new_alias):
        self.alias = new_alias


# In[29]:


peter = Character()
bruce = Character()

peter.backpack.append('web-shooters') # дадим Питеру веб-шутеры


# In[30]:


# проверяем

print(peter.backpack)


# In[31]:


# а что с Бэтманом?
print(bruce.backpack)


# In[32]:


# значение инициализируется при создании класса, а изменяемые типы ссылаются на один и тот же объект в памяти
# т.е. они будут общими у экземпляров класса. 
# Поэтому никогда не нужно делать изменяемые типы значениями по-умолчанию

print(Character.__dict__)


# Магический метод __init__ позволяет задать атрибуты при инициализации экземпляра класса, а также решить проблему, указанную выше

# In[35]:


class Character:
    def __init__(self, name, power, energy=100, hands=2):
        # параметром по-умолчанию backpack делать не будем, чтобы он не был общим
        self.name = name
        self.power = power
        self.energy = energy
        self.backpack = [] # будем присваивать пустой список именно для КОНКРЕТНОГО экземпляра при создании (self)
        self.hands = hands

    def eat(self, food):
        if self.energy < 100:
            self.energy += food
        else:
            print('Not hungry')        
    
    def do_exercise(self, hours):
        if self.energy > 0:
            self.energy -= hours * 2
            self.power += hours * 2
        else:
            print('Too tired')
    
    def change_alias(self, new_alias):
        self.alias = new_alias
        


# In[37]:


# теперь при создании экземпляра класса нам надо обязательно передать аргументы
# peter = Character()
peter = Character('Peter Parker', 80)
bruce = Character('Bruce Wayne', 85)
print(peter.name)
print(peter.power)
#если они не заданы по умолчанию
print(peter.hands)


# In[39]:


print(Character.__dict__) 


# In[38]:


# при таком раскладе (init) все атрибуты сразу же попадают в словарь экземпляра (а не только измененные)
print(peter.__dict__) 


# In[40]:


# плюс мы решим проблемы общих изменяемых атрибутов
peter.backpack.append('web-shooters')


# In[41]:


print(peter.backpack)
print(bruce.backpack)


# Итого: в init будем прописывать то, что хотим задавать при инициализации экзмепляров класса. Все атрибуты с изменямыми значениями по-умолчанию, которые по плану будут общие для всех экзмепляров можно прописывать без него

# Взаимодействия классов: посмотрим на основе сложения

# In[42]:


num1 = 5
num2 = 10


# In[43]:


# числа являются экземплярами класса int
print(type(num1))


# In[44]:


print(num1 + num2)


# In[45]:


# на самом деле происходит следующее
print(num1.__add__(num2))


# In[46]:


class Character:
    def __init__(self, name, power, energy=100, hands=2):
        self.name = name
        self.power = power
        self.energy = energy
        self.backpack = []
        self.hands = hands
    
    def eat(self, food):
        if self.energy < 100:
            self.energy += food
        else:
            print('Not hungry')
        
    def do_exercise(self, hours):
        if self.energy > 0:
            self.energy -= hours * 2
            self.power += hours * 2
        else:
            print('Too tired')
    
    def change_alias(self, new_alias):
        self.alias = new_alias

    # в методы мы без проблем можем передавать другие объекты и с ними взаимодействовать  
    def beat_up(self, foe):
        if not isinstance(foe, Character): # проверка является ли объект экземпляром указанного класса
            return
        if foe.power < self.power:
            foe.status = 'defeated'
            self.status = 'winner'
        else:
            print('Retreat!')


# In[47]:


peter = Character('Peter Parker', 80)
bruce = Character('Bruce Wayne', 85)


# In[48]:


bruce.beat_up(peter)


# In[49]:


print(peter.status)
print(bruce.status)


# In[50]:


peter.beat_up(bruce)


# In[ ]:





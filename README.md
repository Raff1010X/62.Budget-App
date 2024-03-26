# [freeCodeCamp Scientific Computing with Python](https://www.freecodecamp.org/learn/scientific-computing-with-python)

## [Budget-App](https://www.freecodecamp.org/learn/scientific-computing-with-python/scientific-computing-with-python-projects/budget-app)

My git repo: https://github.com/Raff1010X/01.Roadmap

```python
class Category:
    def __init__(self, category):
        self.category = category
        self.ledger = []

    def __str__(self):
        title =  self.category.center(30, "*")
        items = ''
        for item in self.ledger:
            description = f'\n{item["description"][:23]}'
            description += " " * (23 - min(len(item["description"]), 23))
            amount = ("%.2f" % item["amount"]).rjust(7)
            items += f'{description}{amount}'
        total = f'Total: {"%.2f" % self.get_balance()}'
        return f'{title}{items}\n{total}'

    def deposit(self, amount, description = ''):
        self.ledger.append({"amount": amount, "description": description})

    def withdraw(self, amount, description = ''):
        if self.check_funds(amount):
            self.ledger.append({"amount": - amount, "description": description})
            return True
        else:
            return False

    def get_balance(self):
        return sum(item["amount"] for item in self.ledger)

    def get_withdraws(self):
        return sum(- item["amount"] for item in self.ledger if item["amount"] < 0)

    def transfer(self, amount, other_object):
        if self.check_funds(amount):
            self.ledger.append({"amount": - amount, "description": f'Transfer to {other_object.category}'})
            other_object.ledger.append({"amount": amount, "description": f'Transfer from {self.category}'})
            return True
        else: 
            return False
    
    def check_funds(self, amount): 
        return self.get_balance() >= amount

def create_spend_chart(categories):
    header = 'Percentage spent by category'
    suma = sum(category.get_withdraws() for category in categories)
    
    percentage = ''
    for line in range(100, -10, -10):
        percentage += f'\n{(str(line) + "| ").rjust(5)}'
        for category in categories:
            if (category.get_withdraws() / suma) * 100 >= line:
                percentage += f'o  '
            else:
                percentage += '   '

    dots = f'\n{" " * 4}{"-" * 3 * len(categories)}-'
    max_name = max(len(category.category) for category in categories)
    categories_names = ''
    for line in range(0, max_name):
        categories_names += f'\n{" " * 5}'
        for category in categories:
            categories_names += f'{category.category.lower().capitalize().ljust(max_name)[line]}{" " * 2}'

    chart = f'{header}{percentage}{dots}{categories_names}'
    return chart

# test
food = Category("Food")
food.deposit(900, "deposit")
entertainment = Category("Entertainment")
entertainment.deposit(900, "deposit")
business = Category("business")
business.deposit(900, "deposit")
food.withdraw(105.55)
entertainment.withdraw(33.40)
business.withdraw(10.99)

create_spend_chart([business, food, entertainment])
```

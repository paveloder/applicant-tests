# Задания

## Python

#### Выполните рефакторинг кода ниже. Реализуйте новый *payment_type* (скажем, *bank*). Убедитесь, что добавление новых *payment_type* осуществляется легко.<br>
*Подсказка 1: руководствуйтесь некоторыми из принципов SOLID.*
<br><br>*Подсказка 2: В коде можно улучшить много вещей. Чем больше вы заметите и отрефакторите, тем лучше.*

```python
class Order:

   def __init__(self):
      self.items = []
      self.quantities = []
      self.prices = []
      self.status = 'open'

   def add_item(self, name, quantity, price):
      self.items.append(name)
      self.quantities.append(quantity)
      self.prices.append(price)

   def total_price(self):
      total = 0
      for i in range(len(self.prices)):
         total += self.quantities[i] * self.prices[i]
      return total

   def pay(self, payment_type, security_code):
      if payment_type == 'debit':
         print('Какая-то логика реализации debit...')
         print(f'Верифицируем код: {security_code}')
         self.status = 'paid'
      elif payment_type == 'credit':
         print('Какая-то логика реализации credit...')
         print(f'Верифицируем код: {security_code}')
         self.status = 'paid'
      else:
         raise Exception(f'Неизвестный тип платежа: {payment_type}')


def main() -> None:
   order = Order()
   order.add_item('Keyboard', 1, 50)
   order.add_item('SSD', 1, 150)
   order.add_item('USB cable', 2, 5)
   print(order.total_price())
   order.pay('debit', '0372846')


if __name__ == "__main__":
   main()
```

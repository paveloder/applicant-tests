# Задания

## Python

Ниже даны 2 задания. Первое сложнее, второе - проще. Решение 1ого (или 2ух) будет плюсом.

### 1 (качество кода, SOLID, принцип единой ответственности, Dependency Injection)

Выполните рефакторинг кода ниже. Реализуйте новый *payment_type* (скажем, *bank*). Убедитесь, что добавление новых *payment_type* осуществляется легко.<br>
*Подсказка: руководствуйтесь принципом единой ответственности. Также можно применить Dependency Injection.*
<br>*Подсказка 2: Помимо этого в коде можно улучшить ещё много вещей. Чем больше вы заметите и отрефакторите, тем лучше.*

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

### 2 (качество кода, Dependency Injection, Dependency Inversion)

Код ниже работает без ошибок, но требует определенного рефакторинга.
Внесите изменения в код так, чтобы добавлять новые **LampSwitcher'ы** было проще.

```python
from typing import Union


class GlowLampSwitcher:

    def __init__(self) -> None:
        self.on_state = False

    def on(self) -> None:
        print('Лампа накаливания включена...')
        print('Логика реализации включения лампы накаливания...')
        self.on_state = True

    def turn_off(self) -> None:
        print('Лампа накаливания выключена...')
        print('Логика реализации выключения лампы накаливания...')
        self.on_state = False


class HalogenLampSwitcher:

    def __init__(self) -> None:
        self.on_state = False

    def turn_on(self) -> None:
        print('Галогенная лампа включена...')
        print('Логика реализации включения галогена...')
        self.on_state = True

    def off(self) -> None:
        print('Галогенная лампа выключена...')
        print('Логика реализации выключения галогена...')
        self.on_state = False


class AnotherLampSwitcher:

    def __init__(self) -> None:
        self.on_state = False

    def lamp_on(self) -> None:
        print('Ещё лампа включена...')
        print('Специфическая логика реализации включения лампы...')
        self.on_state = True

    def lamp_off(self) -> None:
        print('Ещё лампа выключена...')
        print('Специфическая логика реализации выключения лампы...')
        self.on_state = False


class ElectricLightSwitchManager:

    def __init__(
        self, switcher: Union[GlowLampSwitcher, HalogenLampSwitcher, AnotherLampSwitcher],
    ) -> None:
        self.switcher = switcher

    def press(self) -> None:
        if isinstance(self.switcher, GlowLampSwitcher):
            if self.switcher.on_state:
                self.switcher.turn_off()
            else:
                self.switcher.on()
        elif isinstance(self.switcher, HalogenLampSwitcher):
            if self.switcher.on_state:
                self.switcher.off()
            else:
                self.switcher.turn_on()
        elif isinstance(self.switcher, AnotherLampSwitcher):
            if self.switcher.on_state:
                self.switcher.lamp_off()
            else:
                self.switcher.lamp_on()


def main() -> None:
    switch = ElectricLightSwitchManager(GlowLampSwitcher())
    switch.press()
    switch.press()
    switch = ElectricLightSwitchManager(HalogenLampSwitcher())
    switch.press()
    switch.press()
    switch = ElectricLightSwitchManager(AnotherLampSwitcher())
    switch.press()
    switch.press()


if __name__ == '__main__':
    main()

```

# Sistema de Tienda e Inventario con `Screens` para `Ren'py`
## Explicación de las funciones incorporadas en este sistema
Para poder usar este multi-sistema que incluye o incorpora a través de funciones vinculadas entre sí, los siguientes sistemas:

- Moneda 
- Tienda
- Carrito o lista de compras
- Inventario de objetos comúnes
- Inventario de objetos consumibles
- Efectos de Items consumibles en sistema de puntos

Debemos conocer como funcionan estos mismo e indagar sobre algunos pequeños detalles para poder ejecutarlos correctamente a la hora de usarlos en nuestro proyecto de `Ren'py`

### NOTA
Antes de definir o crear este conjuntos de sistemas, debe estar ubicado dentro de un bloque `Init Python` en cualquiera de los archivos `RPY` de la carpeta `game/` para su correcto funcionamiento, tal como se muestra en el siguiente `Markdown`:
```py
Init Python:
   class Item:
      def __init__(self, name, cost, tag=None, effect=None):
         self.name = name 
         self.cost = cost 
         self.tag = tag 
         self.effect = effect 
         self.num = 0 
         self.amount = 0 
         self.box = 0

   class System:
      def __init__(self):
         self.money = 0
         self.total_cost = 0
         self.total_cost_num = 0
         self.items = []
         self.items_car = []
         self.items_num = []
         self.items_num_car = []
         
      def earn(self, amount):
         self.money += amount

      def buy(self):
         if self.money >=  self.total_cost: 
            self.money -=  self.total_cost
            self.total_cost = 0
            self.items.extend(self.items_car)
            self.items_car.clear()
            renpy.notify("Compra exitosa")
         else:
            renpy.notify("Saldo Insuficiente")

      def add_item(self, item):
         if self.money >= self.total_cost + item.cost
            self.total_cost += item.cost 
            self.items_car.append(item) 
         else:
            renpy.notify("Saldo Insuficiente") 

      def remove_item(self, item):
         if item in self.items_car:
            self.total_cost -= item.cost
            self.items_car.remove(item)

      def has_item_car(self, item):
         if item in self.items_car: 
            return True 
         else:
            return False 

      def has_item(self, item):
         if item in self.items: 
            return True 
         else:
            return False

      def buy_items(self):
         if self.money >=  self.total_cost_num:
            self.money -=  self.total_cost_num 
            self.total_cost_num = 0 
            self.items_num.extend(self.items_num_car) 
            self.items_num_car.clear()
            self.reset_items()
            renpy.notify("Compra exitosa")
         else:
            renpy.notify("Saldo Insuficiente")

      def reset_items(self):
         if energize in self.items_num: 
            energize.num += energize.box 
            energize.box = 0 
         else: 
            pass

      def add_items(self, item):
         if self.money >= self.total_cost_num + item.cost: 
            if item in self.items_num_car: 
               if item.box < 99:
                  self.total_cost += item.cost
                  item.box += 1
               else: 
                  renpy.notify("Máximo Alcanzado")
            elif not item in self.items_num_car:
               self.total_cost_num += item.cost 
               item.box += 1
               self.items_num_car.append(item)
         else:
            renpy.notify("Saldo Insuficiente")

      def remove_items(self, item):
         if item in self.items_num_car: 
            if item.box > 1:
               self.total_cost_num -= item.cost 
               item.box -= 1
            else:
               self.total_cost -= item.cost
               item.box = 0
               self.items_num_car.remove(item)

      def delete_items(self, item):
         if item in self.items_num_car: 
            self.total_cost_num -= item.cost * item.box 
            item.box = 0 
            self.items_num_car.remove(item)

      def not_use(self, item):
         if item in self.items_num:
            if item.num - item.amount + 1 <= item.num: 
               item.amount -= 1 
            elif item.num - item.amount == item.num: 
               renpy.notify("Maximo Alcanzado") 

      def use(self, item):
         if item in self.items_num: 
            if item.tag == "energy": 
               if store.energy + item.effect*item.amount < 100 and item.num - item.amount -1 > 0:
                  item.amount += 1 
               else:
                  renpy.notify(f"No se puede usar {item.name}") 

      def consume(self, item):
         if item in self.items_num: 
            if item.num - item.amount >= 0:
               self.effect_use(item) 
               item.num -= item.amount 
               item.amount = 0 
               if item.num == 0: 
                  self.items_num.remove(item) 
               renpy.notify(f"Se usó {item.name}")

      def effect_use(self, item):
         if item.tag == "energy": 
            store.energy += item.effect*item.amount
            if store.energy > 100: 
               store.energy = 100

      def has_items_car(self, item):
         if item in self.items_num_car: 
            return True 
         else:
            return False 

      def has_items(self, item):
         if item in self.items_num: 
            return True
         else:
            return False
```

### ¿Como puedo usar o llamar las funciones de clase?
#### En `Label` con `menu`:
Las funciones en bloques `Label` y `menu` se llaman ubicando `$` delante de la clase definida como objeto dividida por un `.` seguido del nombre de la funcion que se desea usar y un `()` (en caso dicha funcion requiera de un parametro por definir, esto parametros deben ser ubicados dentro de `()` y en caso sean mas de uno, separados por `,` entre si).

Ejemplos:
```py
Label ejemplo:
   $ system = System()

   "Actualmente tengo %[system.money]d como dinero"
   "¿Qué es eso que brilla en el suelo?"

   $ system.earn(50) #<-- Llama a la funcion `earn` de la clase `System()` para añadir 50 monedas al total de dinero del jugador
   
   "Oh, me encontré 50 monedas de oro"
   "¡QUÉ BIEN!"
   "Ahora tengo %[system.money]d como dinero"

```

#### En `Screen` con `button`, `textbutton` e `imagebutton`:
Las funciones en `Screen` que se deseen llamar a trravés de un `button`, `textbutton` e `imagebutton`, se llaman luego de colocar action seguido de la funcion nativa de Re'py Function(), y dentro de `()` ubicar la clase definida como objeto dividida por un `.` seguido del nombre de la funcion que se desea usar, en caso dicha funcion requiera de un parametro por definir, esto parametros deben ser ubicados luego de la funcion de clase por llamar separados por `,` entre si.

Ejemplo:

```py
Screen earn_money:
   vbox:
      frame:
         text "$: [system.money]"
      align (0.5, 0.5)
      button text "incrementar dinero", action Function (system.earn, 50)
      textbutton "Incrementar dinero" action Function (system.earn, 50)
      imagebutton idle "idle.jpg" hover "hover.jpg" action Function (system.earn, 50)
      
Label ejemplo:
   $ system = System()
   call screen earn_money      
```

### Creación de  la clase `Item()`
Para poder crear cada uno de los items requeridos en este tipo, necesitamos establecer una clase que determine ciertos parametros los cuales nos van a servir para distintos usos dentro de la creacion de los items que son y no son consumibles
```py
class Item:
    def __init__(self, name, cost, tag=None, effect=None):
        self.name = name 
        self.cost = cost 
        self.tag = tag 
        self.effect = effect 
        self.num = 0 
        self.amount = 0 
        self.box = 0
```
Esta clase define las variables de clase y algunos parametros para validar los items

Parametros: `name`, `cost`, `tag=None`, `effect=None`

Variables:

`self.name` (`str`): Define el nombre del Item

`self.cost` (`int`): Define el coste o valor de compra

`self.tag` (`str`): Define el grupo o etiqueta del Item para facilitar el uso de sus efectos tanto de variables globales (store.variable)
como en variables propias de la clase (self.variable)
(Este parametro es opcional, usalo solo si el item por crear es del tipo consumible, de lo contrario es recomendable no definirlo)

`self.effect` (`int`): (Este parametro es opcional, usalo solo si el item por crear es del tipo consumible, de lo contrario es recomendable no definirlo)

`self.num` (`int`): Almacena la cantidad de total Items existentes adquiridos del mismo nombre (self.name)

`self.amount` (int): Almacena la cantidad de Items

`self.box` (`int`): Almacena la cantidad de total Items existentes adquiridos del mismo nombre (self.name)

### Definicion o Creacion de los `Items`
(Se recomienda definir todos los objetos en el bloque `label start` dentro del juego para evitar errores posteriores al arranque o inicio del juego)


Para poder definir y establecer independientemente los items (Ya sea consumibles o no) con `Item()` dentro del codigo nativo de `Ren'py` debemos declarar esta clase como un objeto dentro de un bloque `Label`:
```
label ejemplo:
    $ Item = Item()
```
#### Items comunes, no acumulables o inconsumibles
Los `Items` comunes, no acumulables o inconsumibles se definen haciendo uso solo de dos parametros de la clase `Item()`, por ejemplo, quiero definir un conjunto de ropa para un personaje 

```
label start:
    $ dress = Item("Dress", 15)
```
`$ dress` (`Objeto`): Establece el `Item` como un objeto el cual se podrá usar más adelante

`Item()` (`Clase`): Establece la clase definida anteriormente que se usará para la creación y uso del `Item` consumible

`"Dress"` (`Parametro`: `str`): Define el Nombre del `Item`

`15` (`Parametro`: `int`): Establece el valor o costo del `Item` por el cual se podrá adquirir a través de la Tienda, restandolo entre el valor actual de monedas adquiridas



#### Items consumibles
Los `Items` consumibles se definen o crean casi de la misma forma en la que se establece la clase de estos mismos, dentro de un bloque `Label` (De preferencia se recomienda definirlos en el bloque `label start` del juego para evitar errores posteriores al arranque o inicio del juego)

```py
label start:
    $ energize = Item("Energizante", 15, "energy", 25)
```
`$ energize` (`Objeto`): Establece el `Item` como un objeto el cual se podrá usar más adelante

`Item()` (`Clase`): Establece la clase definida anteriormente que se usará para la creación y uso del `Item` consumible

`"Energizante"` (`Parametro`: `str`): Define el Nombre del `Item`

`15` (`Parametro`: `int`): Establece el valor o costo del `Item` por el cual se podrá adquirir a través de la Tienda, restandolo entre el valor actual de monedas adquiridas

`"energy"` (`Parametro`: `str`): Define la etiqueta o grupo el cual pertenece dicho `Item` (Este parametro es opcional si el `Item` creado es independiente y no pertenece a un conjunto de `Items` con el mismo proposito de consumo, en este caso, el item al ser consumido por el jugador le da ciertos puntos de energía al jugador, por ende, pertenece a un conjunto de `Items` con un mismo propósito, brindar puntos de energía al ser consumidos)

`25` (`Parametro`: `int`): Establece la cantidad de puntos de effecto que tendrá el `Item` al ser consumido (Por ejemplo, al ser un item que le brinda energía al jugador, al ser consumido se otorgarán 25 puntos de energía al total establecido)


### Definicion o Creación de los demás sistemas y sus funciones

#### Clase 
```py
class System:
   def __init__(self):
      self.money = 0
      self.total_cost = 0
      self.total_cost_num = 0
      self.items = []
      self.items_car = []
      self.items_num = []
      self.items_num_car = []
```
Esta clase acumula los datos de inventario, dinero y el carrito de compras

`self.money` (`int`): Cantidad total de dinero actual (si ya hay una variable global existente que se use para establecer la cantidad de dinero adquirida por el jugador, se recomienda eliminarla y reemplazarla por esta variable de clase o reemplazar directamente `self.money`en todas las lineas de codigo que se encuentre dentro de la clase `System()` por la definición `store.` seguido del nombre de la variable ya existente que ocupe este rol, en caso se quiera mostrar en tiempo real la cantidad de dinero consumido y adquirido, después de haber definido la clase `System()` como `$ system = System()` dentro del `Label start`, se recomienda mostrar en un bloque o linea de texto `[system.money]` de la siguiente forma `text "$: [system.money]"`)

`self.total_cost` (`int`): Cantidad total de coste de items no consumibles a comprar

`self.total_cost_num` (`int`): Cantidad total de coste de items consumibles a comprar

`self.items` (`List`): Lista de los items no consumiles ya adquirirdos

`self.items_car` (`List`): Lista de compras de items no consumibles por comprar

`self.items_num` (`List`): Lista de los items consumiles ya adquirirdos

`self.items_num_car` (`List`): Lista de compras de items consumibles por comprar


La clase como tal se define de la misma forma en la que se definió la clase `Item()`, dentro del bloque `Label start`, solo que esta se diferencia por no llevar ningun otro parametro adicional:

```py
label start:
    $ system = System()
```

#### Funciones de clase

##### Ganar o Aumentar Dinero
Agrega la cantidad de dinero definida (`self.money`) por el parametro de la función (`amounth`) al total del dinero actual
```py
   def earn(self, amount):
      self.money += amount
```

##### Tienda de Inconsumibles

###### Comprar
Realiza las compras de los items que no son consumibles
```py
   def buy(self):
      if self.money >=  self.total_cost
         self.money -=  self.total_cost 
         self.total_cost = 0
         self.items.extend(self.items_car)
         self.items_car.clear()
         renpy.notify("Compra exitosa") 
      else:
         renpy.notify("Saldo Insuficiente") 
```
`if self.money >=  self.total_cost`: Verifica si el dinero (`self.money`) es mayor o igual (a través de los signos `>` y `=` juntos, `>=`) al coste total por pagar (`self.total_cost`) usando una condicional

`self.money -=  self.total_cost`: Resta (a través de los signos `-` y `=` juntos, `-=`) el valor total de la compra (`self.total_cost`) con la cantidad de dinero actual (`self.money`)

`self.total_cost = 0`: Restaura el valor del coste total (`self.total_cost`) a 0 luego de haber completado la compra

`self.items.extend(self.items_car)`: Agrega los items del carrito de compras (`self.items`) al inventario de items no consumibles (`self.items_car`) usando la función de `Python` `.extend` para extender los items del carrito de compras (`self.items`) e incrustar dentro de si el inventario de items no consumibles (`self.items_car`)

`self.items_car.clear()`: Elimina todos los items existentes dentro del carrito de compras (`self.items_car`) usando la función de `Python` `.clear()`, la cual se encarga de eliminar todos los datos que se encuentren dentro de la lista selecionada (`self.items_car`)

`renpy.notify("Compra exitosa")`: Notifica que el/los Items se compraron con exito y fueron añadidos al inventario (self.items) la compra a través de la función `renpy.notify`

`renpy.notify("Saldo Insuficiente")`: Notifica que no hay dinero suficiente para realizar la compra a través de la función `renpy.notify` en caso la primera condicional puesta haya dado como retorno `False`

###### Agregar Item a la Lista de Compras
Añade a la lista de compras los items que no son consumibles
```py
   def add_item(self, item):
      if self.money >= self.total_cost + item.cost:
         self.total_cost += item.cost
         self.items_car.append(item) 
      else:
         renpy.notify("Saldo Insuficiente") 
```
`if self.money >= self.total_cost + item.cost`: Verifica con una condicional si la cantidad total de dinero adquirido (`self.money`) por el jugador es mayor o igual (a través de los signos `>` y `=` juntos, `>=`) que el costo total por pagar (`self.total_cost`) mas (`+`) el costo del item (`item.cost`)

`self.total_cost += item.cost`: Suma independientemente el costo de cada item (`item.cost`) a al total de items a pagar (`self.total_cost`)

`self.items_car.append(item)`: Agrega el Item (`item`) a su lista de compras correspondiente (`self.items_car`) usando la función de `Python` `.append()`, la cual se encarga de añadir los valores definidos a la lista especificada (`self.items_car`)

`renpy.notify("Saldo Insuficiente")`: Notifica que no hay dinero suficiente para realizar la compra a través de la función `renpy.notify` en caso la primera condicional puesta haya dado como retorno `False`

###### Remover Item de la Lista de Compras
Elimina los items no comsumibles de su respectiva lista de compras y reduce el costo del item al total por pagar 
```py
   def remove_item(self, item):
      if item in self.items_car:
         self.total_cost -= item.cos
         self.items_car.remove(item) 
```
`if item in self.items_car`: Verifica si el item está dentro de su respectiva lista

`self.total_cost -= item.cost`: Resta (a través de los signos `-` y `=` juntos, `-=`) individualmente el valor del item (`item.cost`) con el costo total a pagar (`self.total_cost`)

`self.items_car.remove(item)`: Elimina el valor del item (`item`) de la lista de compras (`self.items_car`) de forma permanente usando la función de `Python` `.remove()`, la cual se encarga de eliminar los valores definidos de la lista especificada (`self.items_car`)

###### Verificar Item en Lista de Compras
Verifica si el item definido por el parametro (`item`) se encuentra dentro de la lista de compras de items no consumibles (`self.items_car`) a través de una condicional, retorna `True` si el item se encuentra dentro de la lista (`self.items_car`), de lo contrario retornará `False` al no hallarse dicho item definido por el parámetro (`item`)
```py
   def has_item_car(self, item):
      if item in self.items_car: 
         return True 
      else:
         return False
```

###### Verificar Item en Inventario
Verifica si el item definido por el parametro (`item`) se encuentra dentro de la lista de items adquiridos (`self.items`), retorna `True` si el item se encuentra dentro de la lista (`self.items`) a través de una condicional, de lo contrario retornará `False` al no hallarse dicho item definido por el parámetro (`item`)
```py
   def has_item(self, item):
      if item in self.items:
         return True 
      else:
         return False
```

##### Tienda de consumibles

###### Comprar
Realiza las compras de los items que son consumibles haciendo uso de funciones propias de `Python` o funciones propias de esta clase (`Class System`)
```py
   def buy_items(self):
      if self.money >=  self.total_cost_num:
         self.money -=  self.total_cost_num 
         self.total_cost_num = 0
         self.items_num.extend(self.items_num_car)
         self.items_num_car.clear() 
         self.reset_items()
         renpy.notify("Compra exitosa") 
      else:
         renpy.notify("Saldo Insuficiente")
```

`if self.money >=  self.total_cost_num`: Verifica si el dinero (`self.money`) es mayor o igual (a través de los signos `>` y `=` juntos, `>=`) al coste total por pagar (`self.total_cost_num`) usando una condicional

`self.money -=  self.total_cost_num`: Resta (a través de los signos `-` y `=` juntos, `-=`) el valor total de la compra (`self.total_cost_num`) con la cantidad de dinero actual (`self.money`)

`self.total_cost_num = 0`: Restaura el valor del coste total (`self.total_cost_num`) a 0

`self.items_num.extend(self.items_num_car)`: Agrega los items del carrito de compras al inventario de items consumibles (`self.items_num`) al inventario de items consumibles (`self.items_num_car`) usando la función de `Python` `.extend` para extender los items del carrito de compras (`self.items_num`) e incrustar dentro de si el inventario de items no consumibles (`self.items_num_car`)

`self.items_num_car.clear() `: Elimina todos los items existentes dentro del carrito de compras (`self.items_num_car`) usando la función de `Python` `.clear()`

`self.reset_items()`: Usa la función `reset_items()`

`renpy.notify("Compra exitosa")`: Notifica a través de la función `renpy.notify` que el/los Items se compraron con exito y fueron añadidos al inventario de consumibles (`self.items_num`)

`renpy.notify("Saldo Insuficiente")`: Notifica a través de la función `renpy.notify` que no hay dinero suficiente para realizar la compra (sucede si la comparacion anterior dió False)

###### Restaurar Lista de Compras y Añadir al Inventario
NOTA:
Es OBLIGATORIO copiar toda la siguiente linea de código de acuerdo a la cantidad de items que se definen en su proyecto, cambiando `item_name` por el nombre de cada item definido o establecido (`$ Item_example = Item("name_example", ...)`, aquí el item definido sería `Item_example`) por separado y pegar en `def reset_items()`:
```py
      if item_name in self.items_num: 
         item_name.num += item_name.box
         item_name.box = 0
```
Explicación:

Agrega el total de items comprados al total de los items que son consumibles adquirirdos y restaura el valor de los items a comprar a 0 según el grupo o etiqueta especificado (en este caso el item por usar será `energize`, el cual se definió casi al comienzo de esta documentación)
```py
   def reset_items(self):
      if energize in self.items_num: 
         energize.num += energize.box
         energize.box = 0
```

`if energize in self.items_num`: Verifica si el Item (en este caso el item especificado es `energize`) se encuentra dentro de los items consumibles adquiridos (`self.items_num`) usando una condicional

`energize.num += energize.box`: Agrega (a través de los signos `+` y `=` juntos, `+=`) el total de items comprados (`energize.box`) al total de los items que son consumibles adquirirdos (`energize.num`)

`energize.box = 0`: Restaura el valor de los items a comprar (`energize.box`) a 0

###### Agregar Item a la Lista de Compras
Agrega a la lista de compras los items que son consumibles haciendo uso también de la función de `Python` `.append`
```py
   def add_items(self, item):
      if self.money >= self.total_cost_num + item.cost:
         if item in self.items_num_car:
            if item.box < 99: 
               self.total_cost += item.cost
               item.box += 1  
            else: 
               renpy.notify("Máximo Alcanzado")
         elif not item in self.items_num_car:
            self.total_cost_num += item.cost 
            item.box += 1 
            self.items_num_car.append(item) 
      else:
         renpy.notify("Saldo Insuficiente")
```

`if self.money >= self.total_cost_num + item.cost`: Verifica con una condicional si la cantidad total de dinero adquirido (`self.money`) por el jugador es mayor o igual (a través de los signos `>` y `=` juntos, `>=`) que el costo total por pagar (`self.total_cost_num`) mas (`+`) el costo del item (`item.cost`)

`if item in self.items_num_car`: Verifica si el Item se encuentra dentro da lista de compras (`self.items_num_car`) mediante una condicional 

`if item.box < 99`: Establece un limite de de items (`99`) por cada item comprado al maximo

`self.total_cost += item.cost`: Suma (a través de los signos `+` y `=` juntos, `+=`) independientemente el costo de cada item (`item.cost`) a al total de items a pagar (`self.total_cost`)

`item.box += 1`:  Añade un item consumible al total de items del mismo tipo por comprar

`renpy.notify("Máximo Alcanzado")`: Notifica que se alcanzó el límite máximo a través de la función `renpy.notify`

`elif not item in self.items_num_car`: Verifica mediante una condicional si el item no se encuentra en la lista de compras (`self.items_num_car`)

`self.total_cost_num += item.cost`: Suma (a través de los signos `+` y `=` juntos, `+=`) independientemente el costo de cada item a al total de items a pagar

`item.box += 1`: Añade (a través de los signos `+` y `=` juntos, `+=`) un item (`1`) consumible al total de items del mismo tipo por comprar (`item.box`)

`self.items_num_car.append(item)`: Agrega el Item (`item`) a su lista de compras correspondiente (`self.items_num_car`) usando la función de `Python` `.append()`, la cual se encarga de añadir los valores definidos a la lista especificada (`self.items_num_car`)

`renpy.notify("Saldo Insuficiente")`: Notifica que no hay dinero suficiente para realizar la compra a través de la función `renpy.notify` en caso la primera condicional puesta haya dado como retorno `False`

###### Reducir total de Items acumulados en la Lista de Compras
Resta en 1 el valor total los items comsumibles de su respectiva lista de compras según el tipo o nombre del consumible
```py
   def remove_items(self, item):
      if item in self.items_num_car:
         if item.box > 1: 
            self.total_cost_num -= item.cost
            item.box -= 1 
         else:
            self.total_cost -= item.cost
            item.box = 0 
            self.items_num_car.remove(item) 
```

`if item in self.items_num_car`: Verifica si el item está dentro de su lista de compras (`self.items_num_car`) mediante una condicional

`if item.box > 1`: Establece un limite minimo de items por comprar (`item.box`) dentro de la lista de compras (se recomienda no cambiar este valor, de lo contrario, el sistema podría funcionar de forma erronea)

`self.total_cost_num -= item.cost`: Resta (a través de los signos `-` y `=` juntos, `-=`) individualmente el valor del item (`item.cost`) del costo total a pagar (`self.total_cost_num`)

`item.box -= 1`: Resta (a través de los signos `-` y `=` juntos, `-=`) la cantidad de items por comprar (`item.box`) indiviualmente

`self.total_cost -= item.cost`: Resta (a través de los signos `-` y `=` juntos, `-=`) individualmente el valor(`item.cost`) del item del costo total a pagar (`self.total_cost`)

`item.box = 0`: Restaura la cantidad de items por comprar (`item.box`) a 0

`self.items_num_car.remove(item)`: Elimina el valor del item (`item`) de la lista de compras (`self.items_car`) de forma permanente usando la función de `Python` `.remove()`, la cual se encarga de eliminar los valores definidos de la lista especificada (`self.items_num_car`)

###### Remover Item de la Lista de Compras
Elimina y restaura los valores del item en la lista de compras de los items que son consumibles
```py
   def delete_items(self, item):
      if item in self.items_num_car: 
         self.total_cost_num -= item.cost * item.box
         item.box = 0
         self.items_num_car.remove(item)
```

`if item in self.items_num_car`: Verifica si el Item se encuentra dentro de la lista de compras (`self.items_num_car`) mediante una condicional

`self.total_cost_num -= item.cost * item.box`: Elimina el costo total (`self.total_cost_num`) de la cantidad de item en el costo total a pagar restando (a través de los signos `-` y `=` juntos, `-=`) el costo totael costo del item (`item.cost`) multiplicado (`*`) por la cantidad total de items por consumir (`item.box`)

`item.box = 0`:  Restaura la cantidad de items por comprar (`item.box`) a 0

`self.items_num_car.remove(item)`:  Elimina el valor del item (`item`) de la lista de compras (`self.items_num_car`) de forma permanente usando la función de `Python` `.remove()`, la cual se encarga de eliminar los valores definidos de la lista especificada (`self.items_num_car`)

###### Restar Items
Resta los items que son consumibles del total a consumir
```py
   def not_use(self, item):
      if item in self.items_num: 
         if item.num - item.amount + 1 <= item.num:
            item.amount -= 1
         elif item.num - item.amount == item.num: 
            renpy.notify("Maximo Alcanzado") 
```

`if item in self.items_num`: Verifica si el Item se encuentra dentro de los items consumibles adquiridos (`self.items_num`) mediante una condicional

`if item.num - item.amount + 1 <= item.num`: Verifica mediante una condicional si la cantidad de items (`item.num`) menos (`-`) el total a consumir (`item.amount`)  mas (`+`) 1 es menor o igual (a través de los signos `<` y `=` juntos, `<=`) al total de items actuales (`item.num`)

`item.amount -= 1`: Resta (a través de los signos `-` y `=` juntos, `-=`) la cantidad de items por usar (`item.amount`) indiviualmente

`elif item.num - item.amount == item.num`: Verifica si la cantidad de items (`item.num`) menos (`-`) el total a consumir (`item.amount`) es igual al total de items actuales (item.num)

`renpy.notify("Maximo Alcanzado")`: Notifica a través de la función `renpy.notify` que se alcanzó el límite máximo 


###### Aumentar Items por Consumir
NOTA:
Es OBLIGATORIO copiar toda la siguiente linea de código, cambiando `tag_name`, `variable_name` y `max_limit_effect` por el nombre de cada tag o etiqueta, variable definida o establecida para el efecto del item y la cantidad maxima de puntos establecidos para el efecto de dicho item por separado y pegar en `def effect_use()`
```py
   def use(self, item):
      if item in self.items_num: 
         if item.tag == "tag_name":
            if store.variable_name + item.effect*item.amount < max_limit_effect and item.num - item.amount -1 > 0: 
               item.amount += 1
            else:
               renpy.notify(f"No se puede usar {item.name}") 
```
Explicación:

Agrega los items que son consumibles al total a consumir
```py
   def use(self, item):
      if item in self.items_num: 
         if item.tag == "energy":
            if store.energy + item.effect*item.amount < 100 and item.num - item.amount -1 > 0: 
               item.amount += 1
            else:
               renpy.notify(f"No se puede usar {item.name}") 
```

`if item in self.items_num`: Verifica si el Item se encuentra dentro de los items consumibles adquiridos (`self.items_num`)

`if item.tag == "energy"`: Verifica si el item pretenece a un grupo o etiqueta en especifico (en este caso, `"energy"`) usando su variable de clase (`item.tag`) mediante una condicional

`if store.energy + item.effect*item.amount < 100 and item.num - item.amount -1 > 0`: Mediante una condicional, establece un limite del efecto (en este caso es `100`) y verifica si el valor de la variable (`store.energy`) mas (`+`) el valor del efecto del item (`item.effect`) multiplicado (`*`) por la cantidad de items por consumir (`item.amount`) es menor a `100` (o sea el limite, este digito se puede cambiar de acuerdo a la necesidad o limite maximo de un efecto) y si la cantidad total del item adquirido (`item.num`) menos (`-`) la cantidad total por consumir (`item.amount`) menos (`-`) es mayor que (`>`) `0`

`item.amount += 1`: Agrega (a través de los signos `+` y `=` juntos, `+=`) la cantidad de items por usar (`item.amount`) indiviualmente

`renpy.notify(f"No se puede usar {item.name}")`: Notifica que el item no se puede usar mas o ya no se puede usar a través de la función `renpy.notify` usando también `{item.name}` dentro de un `str` (sucede si la condicional anterior retornó False)

###### Consumir Item Seleccionado
Consume y ejecuta los efectos de los items que son consumibles
```py
   def consume(self, item):
      if item in self.items_num: 
         if item.num - item.amount >= 0:
            self.effect_use(item) 
            item.num -= item.amount

            item.amount = 0  
            if item.num == 0:   
               self.items_num.remove(item)
            renpy.notify(f"Se usó {item.name}")
```
`if item in self.items_num`: Verifica si el Item se encuentra dentro de los items consumibles adquiridos a través de una condicional

`if item.num - item.amount >= 0`: Verifica si la cantidad total de items adquiridos (`item.num`) menos los items a consumir (`item.amount`) es igual o mayor a 0

`self.effect_use(item)`: Usa la funcion efect_use() junto al parametro `item` definido en la función actual

`item.num -= item.amount`: Resta la cantidad de items a consumir (`item.amount`) de la cantidad total de items adquirirdos (`item.num`)

`item.amount = 0`: Restaura la cantidad de items a consumir (`item.amount) en 0 luego de haber consumido dicho item y restado el total adquirido

`if item.num == 0`: Verifica si la cantidad total de items (`item.num`) es igual a 0 a través de una condicional

`self.items_num.remove(item)` : Usa la funcion nativa de `Python` `.remove` para eliminar el `item` del total de items adquiridos si la condicional anterior retornó `True`

`renpy.notify(f"Se usó {item.name}")`: Notifica que se consumió el item a través de la función `renpy.notify` usando también `{item.name}` dentro de un `str` para poder adquirir el nombre del `item` usado satisfactoriamente

###### Effectos de Item en Sistema de Puntos
NOTA:
Es OBLIGATORIO copiar toda la siguiente linea de código de acuerdo a la cantidad de tags o etiquetas que se definen en sus conjuntos de items, cambiando `tag_name`, `variable_name` y `limit_max_effect` por el nombre de cada tag o etiqueta, la variable definida o establecida y el limite máximo del effecto en puntaje por separado y pegar en `def effect_use()`:
```py
      if item.tag == "tag_name":
         store.energy += item.effect*item.amount 
         if store.variable_name > limit_max_effect:
            store.variable_name = limit_max_effect 
```
Explicación:

Consume y ejecuta los efectos de los items que son consumibles, definiendo al mismo tiempo un limite de puntos sin excederlo (se recomienda cambiar)
```py
   def effect_use(self, item):
      if item.tag == "energy":
         store.energy += item.effect*item.amount 
         if store.energy > 100:
            store.energy = 100 
```
`if item.tag == "energy"`: Verifica a través de una condicional si el item pertenece a un grupo o etiqueta (`item.tag`) (esto facilita el uso de los efectos en generale) al mismo tiempo en el que determina si el grupo o etiqueta del item está definido especificamente, en este caso, se quiere veririficar si el `item.tag` es `"energy"`

`store.energy += item.effect*item.amount`: Multiplica (a través del signo o simbolo `*`) el efecto (llamado a través de `item.effect`) por la cantidad de items consumidos (llamada por `item.amount`) y los agrega (En este caso) a la energía total del jugador dentro de un sistema de puntos definido o establecido por una variable global la cual se llama con `store.` seguido por el nombre de dicha variable (en este caso `store.energy`)

`if store.energy > 100`: Establece un limite para la variable de energía definida al principio, este limite será de tipo `int`, en este caso el numero limite especificado es `100` (Tanto el limite como la variable despues de `store.` pueden ser reemplazados), Si el limite establecido es superado, se restaura la variable en el limite definido`store.energy = 100` (aqui se debe colocar el mismo numero o cantidad que se colocó anteriormente en la condicional)

###### Verificar Item en Lista de Compras
Verifica si el item definido por el parámetro se encuentra dentro de la lista de compras de items consumibles, retorna `True` si el item se encuentra dentro de la lista, de lo contrario retornará `False` al no hallarse dicho item definido por el parámetro
```py
   def has_items_car(self, item):
      if item in self.items_num_car: 
         return True 
      else:
         return False
```
   
###### Verificar Item en Inventario
Verifica si el item definido por el parametro se encuentra dentro de la lista de items consumibles adquiridos, retorna `True` si el item se encuentra dentro de la lista, de lo contrario retornará `False` al no hallarse dicho item definido por el parámetro
```py
   def has_items(self, item):
      if item in self.items_num:
         return True
      else:
         return False
```
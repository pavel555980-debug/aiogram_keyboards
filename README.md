Aiogram keyboards
=================

Package help you in construct and use inline or text keyboards.

Simple usage
------------

Let's start with example

```python

from aiogram_keyboards import Keyboard, Button

class MainMenu(Keyboard):
    settings = Button('Settings')
    help = Button('Help')

```

So, you can construct your markups by class,
inhered from `Keyboard`. But how you can use
it? You can get full markup and set handler
on any button in your menu. Example:

```python

bot = Bot(TOKEN)
dp = Dispatcher(bot)


@dp.message_handler(commands=['start'])
async def start_handler(message):
    await message.answer(
        "Hi, im message with keyboard powered on aiogram_keyboards",
        reply_markup=MainMenu.get_markup()
    )


@dp.message_handler(MainMenu.settings.filter())
async def settings_handler(message):
    await message.answer("I know that you press on settings button!")


```

> Note:
> You can create button aliases. it means
> that button will be have same functional,
> only names were different. To create an
> alias, use func `button.alias(...)`

If you need in inline keyboard markup, not 
text, all that you need: instead method
`get_markup`, use method `get_inline_markup`, 
no changes in filter usage.

But you can ask, what is callback data of the 
button callback query? Callback data creating 
from prefix and MD5 text hash. Default prefix
is `::keyboard::` (in Button.CALLBACK_ROOT), 
and I not recommend to change it. But if you 
change, one condition: it must end on colon. 

How you can setup module muddleware to void
all functional, let's look in following code:

```python

from aiogram_keyboards import setup_aiogram_keyboards


aiogram_keyboards(dp)

```

What I did? I'm setup specify middleware, that 
skip states if sent text or callback found in 
any button pattern.

> Note:
> You can configure if button call raise state
> skip. Just give to button argument 
> `ignore_state` to initialization method. Also 
> you can set default call answer on button in
> argument `on_call`.

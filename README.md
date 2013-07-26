# RussianMetaphone

This gem provides an implementation of 'Metaphone' phonetic algorithm adapted for Russian language. Check [this Wikipedia article](http://rubydoc.info/gems/carrierwave/frames) for Metaphone intro.

## Installation

Add this line to your application's Gemfile:

    gem 'russian_metaphone'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install russian_metaphone

## Usage

Going to switch to Russian at this point... Drop me a line if you'd like me to translate the Usage section to English...

Алгоритм русского "Метафон" был предложен Петром Каньковски более 10 лет назад. Именно он и лег в основу этой реализации. Оригинал статьи, за давностью лет, не сохранился, но его можно посмотреть в [архиве](http://web.archive.org/web/20071107145942/http://kankowski.narod.ru/dev/metaphoneru.htm).

Реализация RussianMetaphone не претендует на высокую производительность. Основной упор сделан на модульность реализации - позволяет легко менять, настраивать, тестировать и подстраивать его под нужды конкретного проекта. Думаю оптимизация хорошо настроенного алгоритма не будет сложной задачей, гораздо сложнее "оттюнить" сам алгоритм.

### Как пользоваться

```ruby
puts RussianMetaphone::process("Ахматова") # => ахмат%5
puts RussianMetaphone::process("Бродский") # => працк%9
puts RussianMetaphone::process("Мальденштам") # => малдинштам
```

### Как это работает

Входные данные проходят через набор фильтров и каждый фильтр по-своему модифицирует строку. Та строка, которую вернет последний в цепочке фильтр и будет конечным результатом.

### Фильтры

Фильтр - это руби модуль, который реализует метод *filter*

```ruby
def filter(string, options = {})
```

результатом выполнения фильтра должна быть строка. 

RussianMetaphone имеет готовый набор фильтров для работы с именами и фамилиями, эти фильтры перечислены ниже. Вы можете добавить свои фильтры в цепочку если алгоритм не совсем четко справляется с Вашей задачей. Про добавление "кастомных" фильтров в цепочку смотрите ниже.

#### RussianMetaphone::Filter::Normalization

Нормализует строку - убирает из нее все не кириллическое, а так-же символы твердого и мягкого знаков ('Ъ' и 'Ь')

#### RussianMetaphone::Filter::DuplicatesRemoval

Исключает повторяющиеся символы - (Метревели многие напишут как Метревелли)

#### RussianMetaphone::Filter::LastnameEnding

При работе с фамилиями бывает полезным заменить часто употребимые окончания фамилий на что-то более короткое. Этот фильтр заменяет окончание *овский* на *%1*, *евский* на *%2* и т.д. Остальные замены см. в исходниках.

#### RussianMetaphone::Filter::Replacement

Заменяет символы следующим образом:

* ТС, ДС - заменяются на Ц
* ЙО, ИО, ЙЕ, ИЕ - заменяются на И
* О, Ы, А, Я - заменяются на А
* Ю, У - заменяются на У
* Е, Ё, Э - заменяются на И

#### RussianMetaphone::Filter::BreathConsonants

Производит оглушение согласных в слабой позиции. См. исходник - там описаны детали.


## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

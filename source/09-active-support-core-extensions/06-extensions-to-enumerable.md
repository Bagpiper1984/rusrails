# Расширения для Enumerable

### `sum`

Метод `sum` складывает элементы перечисления:

```ruby
[1, 2, 3].sum # => 6
(1..100).sum  # => 5050
```

Сложение применяется только к элементам, откликающимся на `+`:

```ruby
[[1, 2], [2, 3], [3, 4]].sum    # => [1, 2, 2, 3, 3, 4]
%w(foo bar baz).sum             # => "foobarbaz"
{:a => 1, :b => 2, :c => 3}.sum # => [:b, 2, :c, 3, :a, 1]
```

Сумма пустой коллекции равна нулю по умолчанию, но это может быть настроено:

```ruby
[].sum    # => 0
[].sum(1) # => 1
```

Если задан блок, `sum` становится итератором, вкладывающим элементы коллекции и суммирующим возвращаемые значения:

```ruby
(1..5).sum {|n| n * 2 } # => 30
[2, 4, 6, 8, 10].sum    # => 30
```

Сумма пустого получателя также может быть настроена в такой форме:

```ruby
[].sum(1) {|n| n**3} # => 1
```

Метод `ActiveRecord::Observer#observed_subclasses`, к примеру, применяет это так:

```ruby
def observed_subclasses
  observed_classes.sum([]) { |klass| klass.send(:subclasses) }
end
```

NOTE: Определено в `active_support/core_ext/enumerable.rb`.

### `index_by`

Метод `index_by` создает хэш с элементами перечисления, индексированными по некоторому ключу.

Он перебирает коллекцию и передает каждый элемент в блок. Значение, возвращенное блоком, будет ключом для элемента:

```ruby
invoices.index_by(&:number)
# => {'2009-032' => <Invoice ...>, '2009-008' => <Invoice ...>, ...}
```

WARNING. Ключи, как правило, должны быть уникальными. Если блок возвратит то же значение для нескольких элементов, для этого ключа не будет построена коллекция. Победит последний элемент.

NOTE: Определено в `active_support/core_ext/enumerable.rb`.

### `many?`

Метод `many?` это сокращение для `collection.size > 1`:

```erb
<% if pages.many? %>
  <%= pagination_links %>
<% end %>
```

Если задан необязательный блок `many?` принимает во внимание только те элементы, которые возвращают true:

```ruby
@see_more = videos.many? {|video| video.category == params[:category]}
```

NOTE: Определено в `active_support/core_ext/enumerable.rb`.

### `exclude?`

Условие `exclude?` тестирует, является ли заданный объект **не** принадлежащим коллекции. Это противоположность встроенного `include?`:

```ruby
to_visit << node if visited.exclude?(node)
```

NOTE: Определено в `active_support/core_ext/enumerable.rb`.
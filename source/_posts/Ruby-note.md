title: '[Ruby Note]'
author: Naetw, Brainsp8210
tags:
  - Ruby
categories:
  - note
date: 2016-12-05 09:47
-------------------------

> 為了用 RoR 架站，跟 @Brainsp8210 一起學點 Ruby 的基礎

## String
Ruby 是 strong typing，不會隱性轉換型別，需特別注意 type error

跟 Python 一樣 單引或雙引號都是 string type，也可以用 '+' 來連結字串

一些字串 function:

```ruby
var1 = 'stop'
var2 = 'foobar'
var3 = "aAbBcC"

puts var1.reverse # pots
puts var2.length # 6
puts var3.upcase # AABBCC
puts var3.downcase # aabbcc
```

有類似 Python 裡 format string 的方法，需要用**雙引號**:

```ruby
verb = 'work'
where = 'office'
puts "I #{verb} at the #{where}" # 輸出 I work at the office
puts 'I #{verb} at the #{where}' # output I #{verb} at the #{where}
```

## Variable and Type
Local Variable
* 通常用小寫開頭，連接用底線 '_'

type conversion
* `to_s` - 轉換 string
* `to_i` - 轉換 int
* `to_f` - 轉換 float

大寫開頭的是常數:

```ruby
Foo = 1
Foo = 2 # (irb):3: warning: already initialized constant Foo
```

nil
* 表示未設值，未定義的狀態

## Comment
* 偏好單行註解，用 '#'
* 多行用 =begin =end 包起來

```ruby
# This is comment

=begin
  This is comment line.
  This is comment line.
=end
```

## String Symbols
Symbol 是唯一且不會變動得識別名稱，用冒號開頭，相同名稱的 symbol 不會重建，相對 string 較有效率

```ruby
puts "foobar".object_id      # 輸出 2151854740
puts "foobar".object_id      # 輸出 2151830100
puts :foobar.object_id       # 輸出 577768
puts :foobar.object_id       # 輸出 577768
```

## Array
* 每個 element 的 type 並沒有限制
* 可以用 `array.size` 取得長度

一些陣列的使用方法:

```ruby
colors = ["red", "blue"]

colors.push("black")
colors << "white"
puts colors.join(", ") # red, blue, black, white

colors.pop
puts colors.last #black
```

用 `each` 走訪 array:

```ruby
languages = ['Ruby', 'Javascript', 'Perl']

languages.each do |lang|
  puts 'I love ' + lang + '!'
end

# I Love Ruby!
# I Love Javascript!
# I Love Perl!
```

## Hash

是一種 key-value 的資料結構，雖然任何東西都可以當 key，但是通常都會用 Symbol 來當 key

```ruby
config = { foo: 123, bar: 456 } # 等同於 { :foo => 123, :bar => 456 }
```

使用 `each` 走訪 hash:

```ruby
config = { foo: 123, bar: 456 }
config.each do |key, value|
  puts "#{key} is #{value}"
end

# foo is 123
# bar is 456
 
 ```

## Flow Control
* else if 寫成 `elsif`
* 只有一個 if 可以放最後
    * `puts "greater than ten" if total > 10`
* 有支援 Ternary 用法 `?:`

`Case`

```ruby
case name
  when "John"
    puts "Howdy John!"
  when "Ryan"
    puts "Whatz up Ryan!"
  else
    puts "Hi #{name}!"
end
```

`while`

```ruby
i=0
while ( i < 10 )
  i += 1
  next if i % 2 == 0 #跳過雙數
  puts i
end

# Output
# 1
# 3
# 5
# 7
# 9
```

`until`

```ruby
i = 0
i += 1 until i > 10
puts i
# 11
```

`loop`

```ruby
i = 0
loop do
  i += 1
  break if i > 10 # 中斷迴圈
end
puts i
# 11
```

## True of False

只有 false & nil 是 false

```ruby
puts "execute" if “” # 輸出 execute (和JavaScript不同)
puts "execute" if 0 # 輸出 execute (和C不同)
```

## Regular expression

Use `=~`

```ruby
# 抓出手機號碼
phone = "123-456-7890"
if phone =~ /(\d{3})-(\d{3})-(\d{4})/
  ext  = $1
  city = $2
  num  = $3
end
```

## Methods

```ruby
def say_hello(name)
  result = "Hi, " + name
  return result
end

puts say_hello('naetw')
# Output Hi, naetw
```

其實 return 可以省略，他會回傳最後一行的運算值

```ruby
def say_hello(name)
  "Hi, " + name
end

puts say_hello('naetw')
# 輸出 Hi, naetw
```

## `?` & `!` convention

* 用 ? 會回傳 boolean
* 用 ! 會有特別效果:

```ruby
arr = [2,3,1]

arr.sort # [1,2,3]

arr.inspect #[2,1,3]
arr.sort! # [1,2,3]
arr.inspect # [1,2,3]

# 加了 ! 相當於 arr = arr.sort
```

## Object-oriented
* 跟 Python 一樣每樣東西都是一個 object
* Ruby 的類別也是一種常數，所以是大寫開頭，可用 `new` 產生新物件

```ruby
color_string = String.new
color_string = "" # 等同

color_array = Array.new
color_array = [] # 等同

color_hash = Hash.new
color_hash = {} # 等同
```

```ruby
# 輸出 -5 的絕對值
puts -5.abs

# 輸出 Fixnum 類別
puts 99.class

# 輸出五次「Ruby Rocks!」
5.times do
  puts "Ruby Rocks!"
end
```

**Difference between class variable and instance variable on a class**

* instance variable on a class

```ruby
class Parent
  @things = []
    
  def self.things
    @things
  end
              
  def things
    self.class.things
  end  
end

class Child < Parent
  @things = []
end

Parent.things << :car
Child.things << :doll

mom = Parent.new
kid = Child.new

p Parent.things # [:car]
p Child.things # [:doll]
p mom.things # [:car]
p child.things # [:car]
```

* Class variable

```ruby
class Monther
  @@things = []

  def self.things
    @@things
  end

  def things
    @@things
  end
end

class Kid < Monther
end

Monther.things << :car
Kid.things << :doll

m = Monther.new
d = Monther.new
k = Kid.new
son1 = Kid.new
son2 = Kid.new

puts "\nThis is class variable."

[m, d, k, son1, son2].each do |person|
  p person.things
end

# Output 
# [:car, :doll]
# [:car, :doll]
# [:car, :doll]
# [:car, :doll]
# [:car, :doll]
```

使用 instance variable on a class 可以讓同個 class 共同擁有東西，但是 sub-class 就不會共同擁有，反之亦然

如果使用 class variable，除了省去寫 `self.class`，之外，可以自動 share thoughout the class hierarchy 

## Data Encapsulation

`@` & `@@` 開頭的變數，類別外都無法存取

* 為了可以存取到變數，必須定義 method

```ruby
class Person
  def initialize(name)
    @name = name
  end
          
  def name
    @name
  end
                    
  def name=(name)
    @name = name
  end
end

p = Person.new('nae')
p p.name # nae
p.name = "naetw"
p p.name # naetw
```

上面存取 class 裡的變數很常需要，因此有以下方法

* `attr_accessor`
* `attr_writer`
* `attr_reader`

所以上面的程式可以改寫成

```ruby
class Person
  attr_accessor :name
            
  def initialize(name)
    @name = name
  end
                      
end

p = Person.new('nae')
p p.name # nae
p.name = "naetw"
p p.name # naetw
```
## Method encapsulation

* class 中 method 預設是 public
* 一個例外 `initialize` 是 `private`，只能被 `new` 呼叫

以下是常見的存取限制:

* 較常見的寫法是

```ruby
class Myclass
  def public_method
  end
            
  private
                
  def private_method
  end
                      
  protected
                          
  def protected_method
  end
end
```

* 第二種寫法

```ruby
class Father

  def method_a
    puts "I am Method A in #{self.class}"
  end

  def method_b
    puts "I am Method B in #{self.class}"
  end

  def method_c
    puts "I am Method C in #{self.class}"
  end

  def method_secret
    puts "I am Iron Man in #{self.class}"
  end

  protected :method_c
  private :method_secret

end
```

Ruby 的方法存取限制跟一般的程式語言不大一樣

* Ex:

```ruby
father = Father.new
father.method_a
father.method_b
father.method_c          # NoMethodError
father.method_secret     # NoMethodError
```

* 但是如果是由 sub-class 呼叫，會發現神奇的事，他不會 `NoMethodError`

```ruby
class Son < Father

  def son_method_c # call Father's method_c
    method_c
  end

  def son_method_secret # call Father's method_secret
    method_secret
  end
end

son = Son.new
son.method_a             # I am Method A in Son
son.method_b             # I am Method B in Son
son.method_c             # NoMethodError
son.method_secret        # NoMethodError
son.son_method_c         # I am Method C in Son
son.son_method_secret    # I am Iron Man in Son
                                                                                                               ```

來看這行：

```ruby
son.method_a
```

一般來說會翻譯成:

* 有個 object 叫 son，call method_a of son

如果是 Objective-C 會翻譯成:

* 有一個 receiver 叫做 son，對這個 receiver 送一個叫 method_a 的 msg


在 Ruby 裡的 private method，只要沒有明確指出 receiver 的話都可以呼叫，所以上面的 son 呼叫 Father 的 private method 才不會有 Error，因為沒有 receiver

因此 Ruby 的 private 方法其實不只有自己內部可以 access，sub-class 也可以:

```ruby
class A

  def a
    self.b
  end

  private
  def b
    puts "Hello, This is private B"
  end

end

the_a = A.new
the_a.a # NoMethodError
```

上面看起來很合理，但是會出現 Error

原因是在 call method b 的時候加上了 `self`，這裡明確的指出了 receiver 是 `self`，所以才會有 Error

* Little Knowledge
    * 平常用到的 `puts`，其實他是 Object 這個 class 的 private method

```ruby
puts "hello"      # hello
self.puts "hello" # NoMethodError
```

### Protected Method

* 內部指定或不指定 receiver 都可以 
* 但如果從外部直接呼叫則會 Error

```ruby
class A

  def a
    self.b
  end

  def c
    b
  end

  protected
  def b
    puts "Hello, This is private B"
  end

end

the_a = A.new
the_a.a # Hello, This is private B
the_a.c # Hello, This is private B
the_a.b # NoMethodError
```

private 好像也不是很 private

```ruby
father = Father.new
father.method_secret        # NoMethodError
father.send(:method_secret) # I am Iron Man in Father
```

前面說的 puts

```ruby
Object.send(:puts "Hello") # Hello
```

## Class 繼承

```ruby
class Pet
  attr_accessor :name, :age

  def say(word)
    puts "Say: #{word}"
  end
end

class Cat < Pet
  def say(word)
    puts "Meow"
    super
  end
end

class Dog < Pet
  def say(word, person)
    puts "Bark at #{person}"
    super(word)
  end
end

Cat.new.say("Hi") # Meow
                  # Say: Hi
Dog.new.say("Hi", "naetw") # Bark at naetw
                           # Say: Hi
```

                                                                                                          `super` 可以用來呼叫被覆寫的 Pet say method，沒有括號的 `super` 和有括號的 `super()` 不大一樣:

                                                                                                          * `super` 會把所有參數都傳進去
                                                                                                          * `super()` 則可以自己定義傳入的參數

## Module

Module 跟 Class 相似，可以定義 method，可以當作 `Namespace` 來放一些 method

另一個功能是 Mixins，可以把 Module 混入 class 裡，這個 class 就可以有這 Module 的 method，來拆成兩個檔案，debug.rb, foobar.rb，在 foobar.rb 中用 `require` 來引用 debug.rb

* debug.rb

```ruby
module Debug
  def who_am_i?
    puts "#{self.class.name}: #{self.inspect}"
  end
end
```

* foobar.rb

```ruby
require "./debug.rb"
class Foo
  include Debug # Mixin
end

class Bar
  include Debug
end

f = Foo.new
b = Bar.new

f.who_am_i? # Foo: #<Foo:0x0055cc32215058>
b.who_am_i? # Bar: #<Bar:0x0055cc32215008> 
```

Ruby 利用 Module 來解決多重繼承的問題

## 迴圈走訪 & Iterator

`each` 是一個陣列的 method，其中 `do....end` 是 `each` 的參數，稱作 Code Block，是一個 anonymous function:

```ruby
languages = ['Ruby', 'Python', 'C']

languages.each do |lang|
  puts "I love #{lang}"
end
```

其中兩個 `|` 之間的 lang 被稱為 Block variable，其他例子:

```ruby
# 反覆三次
3.times do
  puts 'Good Job!'
end
# Good Job!
# Good Job!
# Good Job!

# 從一數到九
1.upto(9) do |x|
  puts x
end

# 多一個索引區塊變數
languages = ['Ruby', 'Javascript', 'Perl']
languages.each_with_index do |lang, i|
    puts "#{i}, I love #{lang}!"
end
# 0, I Love Ruby!
# 1, I Love Javascript!
# 2, I Love Perl!
```

除了 `do....end` 的形式外其實也可以用大括號，通常單行是用大括號

其他 iterator 範例:

```ruby
a = ["a", "b", "c", "d"]
b = a.map {|x| x + "!"}

puts b.inspect

# output: ["a!", "b!", "c!", "d!"]


b = [1, 2, 3].find_all{ |x| x % 2 == 0 }
puts b.inspect

# output: [2]


a = [51, 101, 256]
a.delete_if { |x| x >= 100 }
puts a.inspect

# output: [51]


puts [2, 1, 3].sort! { |a,b| b <=> a}.inspect

# output: [3, 2, 1]


puts (5..10).inject(0) { |sum, n| sum + n }
puts (5..10).inject { |sum, n| sum + n }

# output: 45 45


longest = ["cat", "sheep", "bear"].inject do |memo, word|
  ( memo.length > word.length ) ? memo : word
end
puts longest

# output: "sheep"
```

* `<=>` 是比較運算子，左邊較大時 return 1，反之亦然

## Inject

```ruby
[1, 2, 3, 4].inject(0) { |result, element| result + element } # 10
```

`inject` 有一個 argument 跟一個 block，在第一次 iteration 的時候，傳給 `inject` 的參數會被當作 first argument for the block，second argument 則來自呼叫他的 object，上面例子就是 [1, 2, 3, 4]

在這邊 first argument 我們稱為 result argument，second argument 則稱為 element argument

在 block 裡想要做什麼都可以，但是 return value of the block 會在下次走訪被當作 result argument

所以上面這例子的 result argument 的變化就是:

* 0 -> 1 -> 3 -> 6 -> 10

## 僅執行一次呼叫

Code block 只會執行一次的特性很好用，例如檔案處理，有了它我們不必特地寫 close:

```ruby
file = File.new("testfile", "r")
# ...
file.close

# another way

File.open("testfile", "r") do |file|
  # ...
  end
# file will be closed automatically  
  ```

## Yield

在 method 裡使用 `yield` 可以執行 code block 的參數:

```ruby
def call_block
  puts "Start"
  yield
  yield
  puts "End"
end

call_block { puts "Blocks are cool" }

# Start
# Block are cool
# Block are cool
# End
```

`yield` 也可以帶參數

```ruby
def call_block
  yield(1)
  yield(2)
  yield(3)
end

call_block { |i|
  puts "#{i}: Blocks are cool!"
}
# 輸出
# "1: Blocks are cool!"
# "2: Blocks are cool!"
# "3: Blocks are cool!"
```

## Proc Object

可以將 code block 轉成一個變數:

```ruby
def call_block(&block)
  block.call(1)
  block.call(2)
  block.call(3)
end

call_block { |i| puts "#{i}: Blocks are cool!" }

# 輸出
# "1: Blocks are cool!"
# "2: Blocks are cool!"
# "3: Blocks are cool!"

# 或是先宣告出 proc object
proc_1 = Proc.new { |i| puts "#{i}: Blocks are cool!" }
proc_2 = lambda { |i| puts "#{i}: Blocks are cool!" }

call_block(&proc_1)
call_block(&proc_2)

# 分別輸出
# "1: Blocks are cool!"
# "2: Blocks are cool!"
# "3: Blocks are cool!"
```

## 傳遞不定參數

```ruby
def my_sum(*val)
  val.inject { |sum, v| sum + v }
end

puts my_sum(1, 2, 3, 4) # val 變數就是 [1, 2, 3, 4]
# 輸出 10
```

## Exception Handling

用 rescue 可以把 exception 救回來

```ruby
begin
  puts 10 / 0 # 這會丟出 ZeroDivisionError 的例外錯誤
rescue => e
  puts e.class # 如果發生例外會執行 rescue 這一段
ensure
  # 無論有沒有發生例外，ensure 這一段都一定會執行
end
# 輸出 ZeroDivisionError
```

用 raise 可以手動觸發 exception

```ruby
raise "Not works!!"
# 丟出一個 RuntimeError

# 自行自定例外物件
class MyException < RuntimeError
end

raise MyException
```

## p v.s puts

* `p foo` 相當於 print foo.inspect + '\n'
* 相對來說比用 puts 來要好 debug
* 如果是一個 array，用 puts 他會把每個 element 分別 puts，且會用 `to_s` 轉換再 output

```ruby
list = ["car", "bar"]

p list # ["car", "bar"]
puts list # car
          # bar
```

## method calls

呼叫帶有參數的 method，method_name 和參數列的左括號之間不要有空格。

```ruby
method_name(parameter1, parameter2,…)
```

呼叫沒有參數的 method，或是沒有立即使用 method 回傳值的需求時，可以不加括號。

```ruby
# 沒有立即使用回傳值，括號可省略
results = method_name parameter1, parameter2

# 有立即使用回傳值做操作的需求時，括號不可省
results = method_name(parameter1, parameter2).reverse
```

## Delegate Method

如果兩個 Model 沒有繼承關係，而又想讓其中一個使用另一個的 method，這時就可以用 delegate

Delegation 的設計模式是他讓 objecet 曝露一些 behavior，而這些 behavior 其實是從跟原本 object 有關係的另一個 object 而來的

舉例，a `Post` belongs_to a `User`

```ruby
class Post < ActiveRecord::Base
  belongs_to :user
  end

class User < ActiveRecord::Base
  has_many :posts
end
```

如果想要呼叫 `post.name` 讓他回傳 `user.name`，通常的做法是:

```ruby
class Post < ActiveRecord::Base
  belongs_to :user
        
  def name
    user.try(:name)
  end  
end
```

如果換成是 `delegate` 方法表示(需要是 public):

```ruby
class Post
  belongs_to :user
  delegate :name, :to => :user, :allow_nil => true
end
```

### Options

`:prefix` 可以設為 `true` 使得 delegated 的 object name 可以被拿來當 prefix。除此之外也可以自己設定 prefix

```ruby
class Post
  belongs_to :user

  delegate :name, :to => :user, :prefix => true
  # post.user_name

  delegate :name, :to => :user, :prefix => "author"
  # post.author_name
end
```

`:allow_nil` 允許某個 Model delegate 的 method 可以是 nil，在上面例子就是允許 `post.user_name` 可以是 `nil`

## Reference

[ihower](https://ihower.tw/rails/ruby.html)

[Public, Protected and Private Method in Ruby](http://kaochenlong.com/2011/07/26/public-protected-and-private-method-in-ruby/)

[Inject](http://blog.jayfields.com/2008/03/ruby-inject.html)

[another inject explanation](http://stackoverflow.com/questions/710501/need-a-simple-explanation-of-the-inject-method)

[delegation](https://sharefunyeh.gitbooks.io/webdev/content/articles/understanding-rails-delegate.html?q=)

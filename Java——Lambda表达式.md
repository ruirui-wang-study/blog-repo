lambda 表达式是一个简短的代码块，它接受参数并返回一个值。Lambda 表达式与方法类似，但它们不需要名称，并且可以直接在方法主体中实现。
## 语法
> parameter -> expression
多个参数
> (parameter1, parameter2) -> expression
lambda表达方式有限。它们必须立即返回一个值，并且不能包含变量、赋值或语句，例如if或for。为了进行更复杂的操作，可以使用带有花括号的代码块。如果 lambda 表达式需要返回一个值，那么代码块应该有一个return语句。
> (parameter1, parameter2) -> { code block }
## 用法
- 结合foreach对列表进行遍历
  ```
  import java.util.ArrayList;

public class Main {
  public static void main(String[] args) {
    ArrayList<Integer> numbers = new ArrayList<Integer>();
    numbers.add(5);
    numbers.add(9);
    numbers.add(8);
    numbers.add(1);
    numbers.forEach( (n) -> { System.out.println(n); } );
  }
}
```
- 如果变量的类型是只有一个方法的接口，则 Lambda 表达式可以存储在变量中。lambda 表达式应具有与该方法相同数量的参数和相同的返回类型。Java 内置了许多此类接口，例如列表使用的Consumer接口（在java.util包中找到）。
```
import java.util.ArrayList;
import java.util.function.Consumer;

public class Main {
  public static void main(String[] args) {
    ArrayList<Integer> numbers = new ArrayList<Integer>();
    numbers.add(5);
    numbers.add(9);
    numbers.add(8);
    numbers.add(1);
    Consumer<Integer> method = (n) -> { System.out.println(n); };
    numbers.forEach( method );
  }
}
```
- 在方法中使用 lambda 表达式，该方法应具有一个以单方法接口作为其类型的参数
```
interface StringFunction {
  String run(String str);
}

public class Main {
  public static void main(String[] args) {
    StringFunction exclaim = (s) -> s + "!";
    StringFunction ask = (s) -> s + "?";
    printFormatted("Hello", exclaim);
    printFormatted("Hello", ask);
  
  public static void printFormatted(String str, StringFunction format) {
    String result = format.run(str);
    System.out.println(result);
  }
}
```

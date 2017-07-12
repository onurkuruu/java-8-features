# Java 8 Özellikleri

Java 8 bir çok güzel özellik ile birlikte 2014 yılında yayınlandı.

### Functional Interface

Sadece bir soyut metodu ve sınırsız sayıda **default** ve **static** metot içeren interfacelerdir. **Object** sınıfının metotlarını kullanmak şartıyla soyut metot sayısı arttırabilir. Fakat yazdığımız **lambda expression** object sınıfından override edilmeyen fonksiyon içindir. 

Fonksiyonel programlama yaklaşımı katmaktadır. Fonksiyonel interfaceler birbirinden kalıtım alabilir fakat hepsinin toplamda 1 tane soyut fonksiyon içermesi gerekir. Fonksiyonel ve fonksiyonel olmayan interfaceler birbirinden kalıtım alabilir fakat göz önünde bulundurulması gereken konu fonksiyonal interface olduğunda toplamda 1 tane soyut metot içermeli.

**Default Method**: Default parametresi verdiğimiz metotun gövdesini doldurabiliyoruz. Birden fazla default parametreli fonksiyon olabilir. Default parametreli fonksiyon kullanan bir interface implement edildiğinde fonksiyonu override etmeye gerek yoktur ama istenirse edilebilir. **<İnterfaceİsmi>.super.defaultFonksiyon** şeklinde implement edilen interfacenin fonksiyonu çağrılabilir.

**Static Method**: Bir interface içinde birden fazla static metot tanımlanabilir. Bunların gövdeleri olmak zorundadır. Bu interfaceyi implement eden sınıf bu static metodları override edemez.**<İnterfaceİsmi>.staticMetot** şeklinde ulaşılabilir.

```java
@FunctionalInterface
interface A {

    //Abstract Method
    void run();

    //Abstract Methods from Object Class
    int hashCode();

    String toString();

    boolean equals(Object obj);

    //others
    default void defaultRunMethod() {
        System.out.println("Default Run Method");
    }

    static void staticRunMethod() {
        System.out.println("Static Run Method");
    }
}
```

### Lambda Expression

Lambda Expressionlar, uzun **Inner Class** kodlarından kurtulmamızı sağlayan, kısa ve basit bir şekilde kullanılabilen bir yapı ile gelmiştir. Önceden belirli interfaceleri implemente eden sınıflar yazarak ya da direk olarak kodun içerisinde anonim iç sınıf oluşturarak implemente sağlardık.

Kullanım Örnekleri:

* Tek satır ve parametresiz fonksiyon örneği:

```java
@FunctionalInterface
interface A {
    void run();
}

class Test {
    public static void main(String args[]) {
        A a = () -> System.out.println("Lambda Expression is running.");
        a.run();
    }
}
```
* Parametreli ve çok satırlı fonksiyon örneği:

```java
@FunctionalInterface
interface A {
    void run(String message);
}

class Test {
    public static void main(String args[]) {
        
        A a = (String message) -> {
            System.out.println("Multiple Line");
            System.out.println("Lambda Expression is running with " + message + " parameter.");
        };

        a.run("Hello");
    }
}
```

### Method Reference

Referans alınan fonksiyonu, fonksiyonal interfacedeki soyut metot ile eşleştirir. **Lambda Expression** gibi soyut metodun implementesi sağlanır.

3 farklı metot tipi edilebilir:

* **Static Metot**: **sınıf ismi::statik_metot** ismi şeklinde kullanılır. Şu şekilde çalışır; functional interfacede ki soyut metodun gövdesi, referans ettiğimiz static metot olur.

```java
@FunctionalInterface
interface A {
    void run();
}

class Test {

    static void runMetotImplemente() {
        System.out.println("Method Reference");
    }

    public static void main(String args[]) {
        A a = Test::runMetotImplemente;
        a.run();
    }
}
```

* **Instance Method**: Sınıf içerisinde ki metotlardan biri referans edilebilir.

```java
@FunctionalInterface
interface A {
    void run();
}

class Test {

    void runMetotImplemente() {
        System.out.println("Method Reference");
    }

    public static void main(String args[]) {
        Test test = new Test();
        A a = test::runMetotImplemente;
        a.run();
    }
}
```

* **Constructor**: Kurucu fonksiyon referans olarak gösterilebilir.

```java
@FunctionalInterface
interface A {
    void run();
}

class Test {

    Test() {
        System.out.println("Method Reference");
    }

    public static void main(String args[]) {
        A a = Test::new;
        a.run();
    }
}
```

### Java Stream API

Verileri işlemede kullanılan yöntemleri içeren yeni **API** denebilir. Collection, Array, I/O  ile uyumlu çalışabiliyor. forEach, map, filter, sorted, collect, statistic vs fonksiyonları vardır. Veri saklamaz pipeline kullanarak onları işler.

```java
public static void main(String args[]) {
        List<Integer> numberList = new ArrayList<>();

        numberList.add(15);
        numberList.add(5);
        numberList.add(50);
        numberList.add(30);
        numberList.add(12);

        List<Integer> tempNumberList = numberList.stream()
                .filter(number -> number > 15)// filtering data
                .collect(Collectors.toList()); // collecting as list

        System.out.println(tempNumberList);
}
```

### Collectors

Object sınıfını genişleten bir yapı olarak düşünebiliriz. Elemanları çeşitli özelliklere göre özetleme, indirgeme işlemleri yapılabilir. Listelerden Setlere dönüşüm, dizilerden listelere dönüşüm, liste elemanlarını toplama, çıkarma işlemleri gibi bir çok güzel işlem sağlıyor.

```java
public static void main(String args[]) {
        List<Integer> numberList = new ArrayList<>();

        numberList.add(15);
        numberList.add(5);
        numberList.add(50);
        numberList.add(30);
        numberList.add(12);

        Integer sum = numberList.stream().mapToInt(number -> number).sum();

        System.out.println("Sum: " + sum);
}
```

### String Joiner 

Verdiğimiz prefix ve sofixlere göre bize bir string döndürür.

```java
public static void main(String args[]) {
        StringJoiner joinNames = new StringJoiner(" - ", "[", "]");

        joinNames.add("Onur");
        joinNames.add("Kuru");

        System.out.println(joinNames);
}
```


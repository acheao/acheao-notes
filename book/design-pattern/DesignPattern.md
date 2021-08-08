1. 迭代器模式(Iterator)
2. 适配器模式(Adapter)
 - The object adapter way implements the target interface by delegating to an adaptee object at run-time (adaptee.specificOperation()).
 - The class adapter way implements the target interface by inheriting from an adaptee class at compile-time (specificOperation()).
 - 对象适配器方式，使用委托
 - 类适配器方式，使用继承方式
    ~~~java
    public interface LightningPhone {
        void recharge();
        void useLightning();
    }

    public interface MicroUsbPhone {
        void recharge();
        void useMicroUsb();
    }

    public class Iphone implements LightningPhone {
        private boolean connector;


        @Override
        public void recharge() {
            if (connector) {
                System.out.println("Recharge started");
                System.out.println("Recharge finished");
            } else {
                System.out.println("Connect Lightning first");
            }
        }

        @Override
        public void useLightning() {
            connector = true;
            System.out.println("Lightning connected");
        }
    }

    public class Android implements MicroUsbPhone{
        private boolean connector;

        @Override
        public void recharge() {
            connector = true;
            System.out.println("MicroUsb connected");
        }

        @Override
        public void useMicroUsb() {
            if (connector) {
                System.out.println("Recharge started");
                System.out.println("Recharge finished");
            } else {
                System.out.println("Connect MicroUsb first");
            }
        }
    }

    public class LightningToMicroUsbAdapter implements MicroUsbPhone{
        private final LightningPhone lightningPhone;


        public LightningToMicroUsbAdapter(LightningPhone lightningPhone) {
            this.lightningPhone = lightningPhone;
        }

        @Override
        public void recharge() {
            lightningPhone.recharge();
        }

        @Override
        public void useMicroUsb() {
            System.out.println("MicroUsb connected");
            lightningPhone.useLightning();
        }
    }

    public class AdapterDemo {
        static void rechargeMicroUsbPhone(MicroUsbPhone phone) {
            phone.useMicroUsb();
            phone.recharge();
        }

        static void rechargeLightningPhone(LightningPhone phone) {
            phone.useLightning();
            phone.recharge();
        }

        public static void main(String[] args) {
            Android android = new Android();
            Iphone iPhone = new Iphone();

            System.out.println("Recharging android with MicroUsb");
            rechargeMicroUsbPhone(android);

            System.out.println("Recharging iPhone with Lightning");
            rechargeLightningPhone(iPhone);

            System.out.println("Recharging iPhone with MicroUsb");
            rechargeMicroUsbPhone(new LightningToMicroUsbAdapter (iPhone));
        }
    }
    ~~~

3. 模板模式(Template Method)
 - 在父类中定义处理流程的框架，在子类中实现具体处理的模式，称为模板模式。
    ~~~java
    public abstract class AbstractDisplay {
        public abstract void open();
        public abstract void print();
        public abstract void close();
        public final void display(){
            open();
            for(int i = 0; i < 5; i++){
                print();
            }
            close();
        }

    }

    public class CharDisplay extends AbstractDisplay{
        private char ch;

        public CharDisplay(char ch){
            this.ch = ch;
        }

        @Override
        public void open() {
            System.out.print("<<");
        }

        @Override
        public void print() {
            System.out.print(ch);
        }

        @Override
        public void close() {
            System.out.println(">>");
        }
    }

    public class StringDisplay extends AbstractDisplay{
        private String string;
        private int wide;

        public StringDisplay(String string){
            this.string = string;
            this.wide = string.getBytes().length;
        }

        @Override
        public void open() {
            printLine();

        }

        @Override
        public void print() {
            System.out.println("|" + string + "|");
        }

        @Override
        public void close() {
            printLine();
        }

        private void printLine(){
            System.out.print("+");
            for(int i = 0; i < wide; i++){
                System.out.print("-");
            }
            System.out.println("+");
        }
    }

    public class Main {
        public static void main(String[] args) {
            AbstractDisplay d1 = new CharDisplay('H');
            AbstractDisplay d2 = new StringDisplay("Hello, World.");
            AbstractDisplay d3 = new StringDisplay("你好，世界。");
            d1.display();
            d2.display();
            d3.display();
        }
    }
    ~~~
4. 工厂模式(Factory Method)
  - 这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。在工厂模式中，我们在创建对象时不会对客户端暴露创建逻辑，并且是通过使用一个共同的接口来指向新创建的对象。
    ~~~java
    public abstract class Product {
        public abstract void use();
    }

    public abstract class Factory {
        public final Product create(String owner){
            Product product = createProduct(owner);
            registerProduct(product);
            return product;
        }
        protected abstract Product createProduct(String owner);
        protected abstract void registerProduct(Product product);

    }

    public class IDCard extends Product{
        private String owner;
        IDCard(String owner){
            System.out.println("制作" + owner + "的ID卡。");
            this.owner = owner;
        }

        @Override
        public void use() {
            System.out.println("使用" + owner + "的ID卡");
        }

        public String getOwner(){
            return owner;
        }

    }

    public class IDCardFactory extends Factory{
        private List owners = new ArrayList();

        @Override
        protected Product createProduct(String owner) {
            return new IDCard(owner);
        }

        @Override
        protected void registerProduct(Product product) {
            owners.add(((IDCard)product).getOwner());
        }

        public List getOwners(){
            return owners;
        }
    }

    public class Main {
        public static void main(String[] args) {
            Factory factory = new IDCardFactory();
            Product card1 = factory.create("小明");
            Product card2 = factory.create("小红");
            Product card3 = factory.create("小刚");
            card1.use();
            card2.use();
            card3.use();
        }
    }
    ~~~
5. 单例模式(Singleton)
 - 懒汉式，线程不安全
    ~~~java
    public class Singleton{
        private static Singleton instance;
        private Singleton(){}
        public static Singleton getInstance(){
            if(instance == null){
                instance = new Singleton();
            }
            return instance;
        }
    }
    ~~~
  - 懒汉式，线程安全
    ~~~java
    public class Singleton{
        private static Singleton instance;
        private Singleton(){}
        //这里加锁了
        public static synchronized getInstance(){
            if(instance == null){
                instance = new Singleton();
            }
            return instance;
        }

    }
    ~~~
 - 饿汉式，线程安全
    ~~~java
    public class Singleton{
        private static Singleton instance = new Singleton();
        private Singleton(){}
        private static Singleton getInstance(){
            return instance;
        }
    }   
    ~~~
 - 懒汉式，双重校验锁
    ~~~java
    public class Singleton{
        private volatile static Singleton instance;
        private Singleton(){}
        public static Singleton getInstance(){
            if(instance == null){
                synchronized(Singleton.class){
                    if(instance == null){
                        instance = new Singleton();
                    }
                }
            }
            return instance;
        }
    }
    ~~~
 - 登记式/静态内部类
    ~~~java
    public class Singleton{
        private static class SingletonHolder{
            private static final Singleton instance = new Singleton();
        }
        private Singleton(){}
        public static Singleton getInstance(){
            return SingletonHolder.instance;
        }
    }
    ~~~
 - 枚举实现单例模式
    ~~~java
    public enum Singleton{
        INSTANCE;
        public void someMethod(){
            
        }
    }
    ~~~
6. 原型模式(Prototype)
7. 建造者模式(Builder)
8. 抽象工厂模式(Abstarct Factory)
9.  桥接模式(Bridge)
10. 策略模式(Strategy)
11. 组合模式(Composite)
12. 装饰者模式(Decorator)
13. 访问者模式(Visitor)
14. 责任链模式(Chain of Responsibility)
15. 外观模式(Facade)
16. 中介者模式(Mediator)
17. 观察者模式(Observer)
18. 备忘录模式(Memento)
19. 状态模式(State)
20. 享元模式(Flyweight)
21. 代理模式(Proxy)
22. 命令模式(Command)
23. 解释器模式(Interpreter)
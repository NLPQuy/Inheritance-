# 5 Loại Kế Thừa trong Lập Trình Hướng Đối Tượng

## Giới thiệu

Kế thừa (Inheritance) là một trong những tính chất quan trọng nhất của lập trình hướng đối tượng, cho phép lớp con (derived class) kế thừa các thuộc tính và phương thức từ lớp cha (base class). Trong lập trình hướng đối tượng, có 5 loại kế thừa chính mà lập trình viên cần nắm vững để thiết kế hệ thống hiệu quả.

---

## 1. Kế Thừa Đơn (Single Inheritance)

### Định nghĩa
Kế thừa đơn là loại kế thừa cơ bản nhất, trong đó một lớp con chỉ kế thừa từ một lớp cha duy nhất. Đây là mối quan hệ một-một (1:1) giữa lớp cha và lớp con.

### Cấu trúc
```
Lớp Cha → Lớp Con
```

### Ví dụ minh họa
```cpp
// Lớp cha
class Animal {
protected:
    string name;
    int age;
    
public:
    Animal(string n, int a) : name(n), age(a) {}
    
    void eat() {
        cout << name << " đang ăn" << endl;
    }
    
    void sleep() {
        cout << name << " đang ngủ" << endl;
    }
};

// Lớp con kế thừa từ Animal
class Dog : public Animal {
public:
    Dog(string n, int a) : Animal(n, a) {}
    
    void bark() {
        cout << name << " đang sủa: Gâu gâu!" << endl;
    }
    
    void wagTail() {
        cout << name << " đang vẫy đuôi" << endl;
    }
};

// Sử dụng
Dog myDog("Bobby", 3);
myDog.eat();    // Kế thừa từ Animal
myDog.bark();   // Phương thức riêng của Dog
```

### Ứng dụng thực tế
- Hệ thống quản lý nhân viên: Person → Employee
- Hệ thống học tập: Course → OnlineCourse
- Quản lý tài khoản: Account → SavingsAccount

### Ưu điểm
- Đơn giản, dễ hiểu và dễ triển khai
- Không có xung đột tên hoặc nhập nhằng
- Hiệu suất tốt, ít overhead
- Dễ bảo trì và debug
- Tuân theo nguyên tắc đơn giản hóa

### Nhược điểm
- Hạn chế trong việc tái sử dụng code từ nhiều nguồn
- Có thể dẫn đến duplicate code
- Ít linh hoạt trong thiết kế phức tạp

---

## 2. Kế Thừa Đa (Multiple Inheritance)

### Định nghĩa
Kế thừa đa cho phép một lớp con kế thừa từ hai hoặc nhiều lớp cha khác nhau cùng lúc. Lớp con sẽ có tất cả thuộc tính và phương thức từ tất cả các lớp cha.

### Cấu trúc
```
Lớp Cha A   Lớp Cha B
     \         /
      \       /
       Lớp Con
```

### Ví dụ minh họa
```cpp
// Lớp cha thứ nhất
class Flyable {
public:
    void fly() {
        cout << "Đang bay trong không trung" << endl;
    }
    
    void land() {
        cout << "Đang hạ cánh" << endl;
    }
};

// Lớp cha thứ hai
class Swimmable {
public:
    void swim() {
        cout << "Đang bơi trong nước" << endl;
    }
    
    void dive() {
        cout << "Đang lặn xuống nước" << endl;
    }
};

// Lớp con kế thừa từ cả hai lớp cha
class Duck : public Flyable, public Swimmable {
private:
    string name;
    
public:
    Duck(string n) : name(n) {}
    
    void quack() {
        cout << name << " kêu: Quạc quạc!" << endl;
    }
    
    void introduce() {
        cout << "Tôi là " << name << ", tôi có thể:" << endl;
        fly();    // Từ Flyable
        swim();   // Từ Swimmable
        quack();  // Phương thức riêng
    }
};

// Sử dụng
Duck mallard("Vịt Xanh");
mallard.introduce();
```

### Ứng dụng thực tế
- Thiết bị điện tử: Camera + Phone → SmartPhone
- Phương tiện: Car + Boat → AmphibiousCar
- Game character: Warrior + Mage → Paladin

### Ưu điểm
- Tái sử dụng code từ nhiều nguồn khác nhau
- Linh hoạt trong thiết kế
- Mô hình hóa tốt các quan hệ phức tạp trong thực tế
- Giảm thiểu code duplication

### Nhược điểm
- **Diamond Problem**: Khi hai lớp cha cùng kế thừa từ một lớp tổ tiên, rồi lớp con lại kế thừa cả hai lớp cha → có thể gây rối khi lớp tổ tiên bị gọi nhiều lần.
- Việc sửa lỗi hoặc nâng cấp chương trình sẽ khó hơn vì cấu trúc kế thừa phức tạp.
- **Ambiguity**: Nếu hai lớp cha có phương thức cùng tên, chương trình không biết dùng cái nào → dễ xảy ra lỗi.
- Khi các lớp liên kết chặt chẽ với nhau, việc thay đổi một lớp có thể ảnh hưởng đến nhiều lớp khác.
- Không được hỗ trợ trong một số ngôn ngữ (như Java, C#)

---

## 3. Kế Thừa Đa Cấp (Multilevel Inheritance)

### Định nghĩa
Kế thừa đa cấp là chuỗi kế thừa nối tiếp nhau, trong đó lớp con của cấp này trở thành lớp cha của cấp tiếp theo. Tạo thành một chuỗi kế thừa nhiều tầng như một cây gia phả.

### Cấu trúc
```
Lớp Tổ tiên
     ↓
  Lớp Cha
     ↓
  Lớp Con
     ↓
 Lớp Cháu
```

### Ví dụ minh họa
```cpp
// Cấp 1: Lớp tổ tiên
class Vehicle {
protected:
    string brand;
    int year;
    
public:
    Vehicle(string b, int y) : brand(b), year(y) {}
    
    void start() {
        cout << "Phương tiện " << brand << " đã khởi động" << endl;
    }
    
    void stop() {
        cout << "Phương tiện " << brand << " đã dừng" << endl;
    }
};

// Cấp 2: Lớp cha (kế thừa từ Vehicle)
class Car : public Vehicle {
protected:
    int doors;
    string fuelType;
    
public:
    Car(string b, int y, int d, string fuel) 
        : Vehicle(b, y), doors(d), fuelType(fuel) {}
    
    void honk() {
        cout << "Xe ô tô " << brand << " đang bấm còi" << endl;
    }
    
    void refuel() {
        cout << "Đổ " << fuelType << " cho xe " << brand << endl;
    }
};

// Cấp 3: Lớp con (kế thừa từ Car)
class SportsCar : public Car {
private:
    int maxSpeed;
    bool turboMode;
    
public:
    SportsCar(string b, int y, int d, string fuel, int speed) 
        : Car(b, y, d, fuel), maxSpeed(speed), turboMode(false) {}
    
    void turboBoost() {
        turboMode = true;
        cout << "Xe thể thao " << brand << " đang bật chế độ tăng tốc!" << endl;
        cout << "Tốc độ tối đa: " << maxSpeed << " km/h" << endl;
    }
    
    void race() {
        start();      // Từ Vehicle
        refuel();     // Từ Car
        turboBoost(); // Phương thức riêng
        cout << "Sẵn sàng đua!" << endl;
    }
};

// Sử dụng
SportsCar ferrari("Ferrari", 2023, 2, "Xăng RON 95", 350);
ferrari.race();
```

### Ứng dụng thực tế
- Phân loại sinh vật: LivingThing → Animal → Mammal → Dog
- Hệ thống file: FileSystemObject → File → TextFile → ConfigFile
- Hierarchy trong công ty: Person → Employee → Manager → CEO

### Ưu điểm
- Mô hình hóa tốt các cấu trúc phân cấp tự nhiên
- Tái sử dụng code hiệu quả qua nhiều cấp
- Dễ mở rộng với các cấp mới
- Phản ánh đúng mối quan hệ IS-A trong thực tế
- Tổ chức code có logic và dễ hiểu

### Nhược điểm
- Tạo ra coupling chặt chẽ giữa các lớp
- Thay đổi lớp cha ảnh hưởng đến tất cả lớp con
- Có thể tạo ra hierarchy quá sâu, khó quản lý
- Khó maintain khi chuỗi quá dài
- Violate nguyên tắc "Favor composition over inheritance"

---

## 4. Kế Thừa Phân Cấp (Hierarchical Inheritance)

### Định nghĩa
Kế thừa phân cấp là khi nhiều lớp con khác nhau cùng kế thừa từ một lớp cha chung. Tạo thành cấu trúc giống như cây với một gốc và nhiều nhánh.

### Cấu trúc
```
      Lớp Cha
    /    |    \
Con A  Con B  Con C
```

### Ví dụ minh họa
```cpp
// Lớp cha chung
class Shape {
protected:
    string color;
    double x, y; // tọa độ
    
public:
    Shape(string c, double posX, double posY) 
        : color(c), x(posX), y(posY) {}
    
    virtual void draw() {
        cout << "Vẽ hình " << color << " tại (" << x << ", " << y << ")" << endl;
    }
    
    virtual double getArea() = 0; // pure virtual function
    
    void setColor(string newColor) {
        color = newColor;
    }
    
    string getColor() const {
        return color;
    }
};

// Lớp con thứ nhất
class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(string c, double x, double y, double r) 
        : Shape(c, x, y), radius(r) {}
    
    double getArea() override {
        return 3.14159 * radius * radius;
    }
    
    void draw() override {
        cout << "Vẽ hình tròn " << color << " bán kính " << radius 
             << " tại (" << x << ", " << y << ")" << endl;
    }
    
    double getRadius() const { return radius; }
};

// Lớp con thứ hai
class Rectangle : public Shape {
private:
    double width, height;
    
public:
    Rectangle(string c, double x, double y, double w, double h) 
        : Shape(c, x, y), width(w), height(h) {}
    
    double getArea() override {
        return width * height;
    }
    
    void draw() override {
        cout << "Vẽ hình chữ nhật " << color << " " << width << "x" << height 
             << " tại (" << x << ", " << y << ")" << endl;
    }
    
    double getPerimeter() const {
        return 2 * (width + height);
    }
};

// Lớp con thứ ba
class Triangle : public Shape {
private:
    double base, height;
    
public:
    Triangle(string c, double x, double y, double b, double h) 
        : Shape(c, x, y), base(b), height(h) {}
    
    double getArea() override {
        return 0.5 * base * height;
    }
    
    void draw() override {
        cout << "Vẽ tam giác " << color << " đáy " << base << " cao " << height 
             << " tại (" << x << ", " << y << ")" << endl;
    }
};

// Sử dụng polymorphism
void drawShapes() {
    vector<Shape*> shapes;
    
    shapes.push_back(new Circle("đỏ", 0, 0, 5));
    shapes.push_back(new Rectangle("xanh", 10, 10, 4, 6));
    shapes.push_back(new Triangle("vàng", 20, 20, 8, 5));
    
    for (Shape* shape : shapes) {
        shape->draw();
        cout << "Diện tích: " << shape->getArea() << endl;
        cout << "---" << endl;
    }
    
    // Cleanup
    for (Shape* shape : shapes) {
        delete shape;
    }
}
```

### Ứng dụng thực tế
- Hệ thống quản lý nhân viên: Employee → Manager, Developer, Designer, Tester
- UI Components: UIComponent → Button, TextBox, Label, CheckBox
- Các loại tài khoản ngân hàng: Account → SavingsAccount, CheckingAccount, CreditAccount

### Ưu điểm
- Code reuse hiệu quả từ lớp cha chung
- Dễ thêm các lớp con mới mà không ảnh hưởng đến code hiện tại
- Polymorphism được áp dụng tốt
- Tách biệt rõ ràng các functionality khác nhau
- Dễ maintain và extend
- Tuân theo Open/Closed Principle

### Nhược điểm
- Lớp cha có thể trở nên quá phức tạp khi phải phục vụ nhiều lớp con
- Thay đổi lớp cha ảnh hưởng đến tất cả lớp con
- Có thể dẫn đến "God Class" anti-pattern
- Interface của lớp cha có thể không phù hợp với tất cả lớp con

---

## 5. Kế Thừa Hỗn Hợp (Hybrid Inheritance)

### Định nghĩa
Kế thừa hỗn hợp là sự kết hợp của hai hoặc nhiều loại kế thừa khác nhau trong cùng một chương trình. Đây là loại kế thừa phức tạp nhất và có thể gây ra nhiều vấn đề nghiêm trọng.

### Cấu trúc
```
    Lớp A
   /     \
Lớp B   Lớp C
   \     /
    Lớp D
(Kết hợp Multiple + Hierarchical)
```

### Ví dụ minh họa
```cpp
// Lớp gốc
class LivingBeing {
protected:
    string name;
    int age;
    
public:
    LivingBeing(string n, int a) : name(n), age(a) {}
    
    virtual void breathe() {
        cout << name << " đang thở" << endl;
    }
    
    virtual void grow() {
        cout << name << " đang phát triển" << endl;
    }
    
    string getName() const { return name; }
};

// Multilevel inheritance: LivingBeing -> Animal
class Animal : public LivingBeing {
protected:
    string species;
    
public:
    Animal(string n, int a, string s) : LivingBeing(n, a), species(s) {}
    
    virtual void move() {
        cout << name << " đang di chuyển" << endl;
    }
    
    virtual void makeSound() = 0;
    
    string getSpecies() const { return species; }
};

// Hierarchical inheritance: Animal -> Bird, Fish
class Bird : public Animal {
protected:
    double wingSpan;
    
public:
    Bird(string n, int a, double ws) : Animal(n, a, "Bird"), wingSpan(ws) {}
    
    virtual void fly() {
        cout << name << " đang bay với sải cánh " << wingSpan << "m" << endl;
    }
    
    void makeSound() override {
        cout << name << " kêu: Chíp chíp!" << endl;
    }
    
    void buildNest() {
        cout << name << " đang xây tổ" << endl;
    }
};

class Fish : public Animal {
protected:
    string waterType; // nước ngọt/mặn
    
public:
    Fish(string n, int a, string wType) : Animal(n, a, "Fish"), waterType(wType) {}
    
    virtual void swim() {
        cout << name << " đang bơi trong " << waterType << endl;
    }
    
    void makeSound() override {
        cout << name << " tạo bong bóng nước" << endl;
    }
    
    void schooling() {
        cout << name << " đang bơi theo đàn" << endl;
    }
};

// Multiple inheritance: FlyingFish kế thừa từ cả Bird và Fish
class FlyingFish : public Bird, public Fish {
private:
    double glideDistance;
    
public:
    FlyingFish(string n, int a, double ws, string wType, double dist) 
        : Bird(n, a, ws), Fish(n, a, wType), glideDistance(dist) {}
    
    void glide() {
        cout << "Cá bay " << Bird::name << " lướt " << glideDistance 
             << "m trên mặt nước" << endl;
    }
    
    // Giải quyết ambiguity - phải chọn implementation
    void makeSound() override {
        cout << "Cá bay tạo tiếng khi nhảy lên khỏi nước" << endl;
    }
    
    void move() override {
        swim();  // Sử dụng move của Fish
        fly();   // Và cả của Bird
    }
    
    // Phải explicitly call các hàm từ base classes
    void breathe() override {
        Bird::breathe(); // Hoặc Fish::breathe() - chọn một
    }
    
    void showAbilities() {
        cout << "=== Khả năng của " << Bird::name << " ===" << endl;
        Bird::breathe();  // Từ LivingBeing qua Bird
        swim();           // Từ Fish
        fly();            // Từ Bird
        glide();          // Riêng của FlyingFish
    }
};

// Sử dụng
void demonstrateHybridInheritance() {
    FlyingFish exocet("Cá Bay Đại Tây Dương", 2, 1.2, "nước mặn", 400);
    
    exocet.showAbilities();
    
    // Có thể cast về các base class
    Bird* bird = &exocet;
    Fish* fish = &exocet;
    
    cout << "\nXem như Bird:" << endl;
    bird->fly();
    
    cout << "\nXem như Fish:" << endl;
    fish->swim();
}
```

### Ứng dụng thực tế
- Hệ thống quản lý trường học: Person → Student, Teacher; Employee → Teacher → TeachingAssistant (vừa là Student vừa là Teacher)
- Game RPG: Character → Warrior, Mage; Equipment → Weapon, Armor → MagicSword (vừa là weapon vừa có magic)
- Hệ thống thiết bị: Device → AudioDevice, VideoDevice → Smartphone (vừa audio vừa video)

### Ưu điểm
- Mô hình hóa được các mối quan hệ cực kỳ phức tạp trong thực tế
- Kết hợp được ưu điểm của nhiều loại kế thừa
- Linh hoạt cao trong thiết kế
- Tái sử dụng code tối đa từ nhiều nguồn

### Nhược điểm
- **Diamond Problem nghiêm trọng**: Khi có multiple paths đến cùng một base class
- **Cực kỳ phức tạp**: Khó hiểu, khó thiết kế đúng
- **Ambiguity nghiêm trọng**: Compiler không biết chọn method nào
- **Performance overhead cao**: Do phải resolve multiple inheritance
- **Debugging nightmare**: Rất khó trace lỗi
- **High coupling**: Các class phụ thuộc lẫn nhau phức tạp
- **Maintenance hell**: Thay đổi một chỗ có thể ảnh hưởng không lường trước

---

## So Sánh và Khuyến Nghị

### Bảng So Sánh

| Loại Kế Thừa | Độ Phức Tạp | Mức Sử Dụng | Ứng Dụng Chính | Nguy Cơ |
|---------------|-------------|-------------|----------------|---------|
| **Single** | ⭐ | Rất cao | Hệ thống đơn giản, prototype | Thấp |
| **Multiple** | ⭐⭐⭐⭐ | Thấp | Mixins, interfaces đa năng | Cao |
| **Multilevel** | ⭐⭐ | Cao | Taxonomy, classification | Trung bình |
| **Hierarchical** | ⭐⭐ | Rất cao | UI components, employee systems | Thấp |
| **Hybrid** | ⭐⭐⭐⭐⭐ | Rất thấp | Enterprise systems phức tạp | Rất cao |

### Khuyến Nghị Sử Dụng

#### 🟢 Nên Sử Dụng:
1. **Single Inheritance**: Luôn ưu tiên đầu tiên, đặc biệt cho người mới học
2. **Hierarchical Inheritance**: Tuyệt vời cho hầu hết các ứng dụng thực tế
3. **Multilevel Inheritance**: Phù hợp cho các hệ thống có cấu trúc phân cấp tự nhiên

#### 🟡 Cân Nhắc Kỹ:
1. **Multiple Inheritance**: Chỉ khi thực sự cần thiết và có kinh nghiệm
2. **Hybrid Inheritance**: Chỉ cho các hệ thống enterprise cực kỳ phức tạp

#### 🔴 Nguyên Tắc Chung:
- **"Favor Composition over Inheritance"**: Trong nhiều trường hợp, composition an toàn hơn
- **KISS Principle**: Keep It Simple, Stupid
- **Start Simple**: Bắt đầu với Single, mở rộng dần khi cần
- **Think Twice**: Trước khi dùng Multiple hoặc Hybrid, hỏi "Có cách nào khác không?"

### Lời Khuyên Cuối

Kế thừa là công cụ mạnh mẽ nhưng cần được sử dụng một cách khôn ngoan. Hãy nhớ rằng mục tiêu cuối cùng là tạo ra code dễ hiểu, dễ maintain và ít bug. Đôi khi, một thiết kế đơn giản hơn sẽ tốt hơn một thiết kế "thông minh" nhưng phức tạp.

Trong thực tế, 80% các trường hợp chỉ cần Single và Hierarchical Inheritance là đủ. Hãy thành thạo hai loại này trước khi nghĩ đến các loại phức tạp hơn.

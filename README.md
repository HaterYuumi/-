# Класс Rectangle для работы с прямоугольниками в C#

## Постановка задачи
Мой класс реализует работу с прямоугольниками на плоскости, включая их создание, ввод/вывод, вычисление периметра, проверку свойств и взаимодействие между прямоугольниками.

> [!NOTE]
> Прямоугольник задается левой нижней точкой (A), шириной (a) и высотой (b). Все операции выполняются с учетом этих параметров.

## Реализация класса

### 1. Структуры данных

- **Point** - вспомогательный класс для работы с точками на плоскости:
```csharp
public class Point
{
    public double X { get; }
    public double Y { get; }

    public Point(double x, double y)
    {
        X = x;
        Y = y;
    }
}
```

- **Rectangle** - основной класс для работы с прямоугольниками:
```csharp
public class Rectangle
{
    private Point A;       // Левый нижний угол
    private double a;      // Ширина
    private double b;      // Высота
}
```

### 2. Конструкторы

- Конструктор по умолчанию:
```csharp
public Rectangle()
{
    A = new Point(0, 0);
    a = 1;
    b = 1;
}
```

- Конструктор с заданием точки и размеров:
```csharp
public Rectangle(Point point, double width, double height)
{
    A = point;
    a = width > 0 ? width : 1;  // Проверка на положительность ширины
    b = height > 0 ? height : 1; // Проверка на положительность высоты
}
```

- Конструктор с заданием координат и размеров:
```csharp
public Rectangle(double x, double y, double width, double height)
{
    A = new Point(x, y);
    a = width > 0 ? width : 1;
    b = height > 0 ? height : 1;
}
```

### 3. Основные методы

- **Input()** - ввод параметров прямоугольника с консоли:
```csharp
public void Input()
{
    Console.WriteLine("Введите координаты левого нижнего угла:");
    Console.Write("X: ");
    double x = double.Parse(Console.ReadLine());
    Console.Write("Y: ");
    double y = double.Parse(Console.ReadLine());
    A = new Point(x, y);

    // Ввод с проверкой положительности ширины
    Console.Write("Введите ширину прямоугольника (a > 0): ");
    a = double.Parse(Console.ReadLine());
    while (a <= 0)
    {
        Console.Write("Некорректный ввод. Введите ширину (a > 0): ");
        a = double.Parse(Console.ReadLine());
    }

    // Ввод с проверкой положительности высоты
    Console.Write("Введите высоту прямоугольника (b > 0): ");
    b = double.Parse(Console.ReadLine());
    while (b <= 0)
    {
        Console.Write("Некорректный ввод. Введите высоту (b > 0): ");
        b = double.Parse(Console.ReadLine());
    }
}
```

- **Output()** - вывод информации о прямоугольнике:
```csharp
public void Output()
{
    Console.WriteLine($"Прямоугольник с левым нижним углом в точке ({A.X}, {A.Y})");
    Console.WriteLine($"Ширина: {a}, Высота: {b}");
}
```

### 4. Вычислительные методы

- **GetPerimeter()** - вычисление периметра:
```csharp
public double GetPerimeter()
{
    return 2 * (a + b);
}
```

- **GetCircumscribedCircleCenter()** - центр описанной окружности:
```csharp
public Point GetCircumscribedCircleCenter()
{
    return new Point(A.X + a / 2, A.Y + b / 2);
}
```

### 5. Методы проверки свойств

- **IsSquare()** - проверка является ли квадратом:
```csharp
public bool IsSquare()
{
    return Math.Abs(a - b) < double.Epsilon;
}
```

- **AreSimilar()** - проверка на подобие с другим прямоугольником:
```csharp
public bool AreSimilar(Rectangle other)
{
    double ratio1 = this.a / this.b;
    double ratio2 = other.a / other.b;
    return Math.Abs(ratio1 - ratio2) < double.Epsilon;
}
```

### 6. Методы взаимодействия

- **GetSymmetricRectangle()** - получение симметричного прямоугольника:
```csharp
public Rectangle GetSymmetricRectangle()
{
    return new Rectangle(-A.X - a, -A.Y - b, a, b);
}
```

- **IntersectsAllQuadrants()** - проверка пересечения всех четвертей:
```csharp
public bool IntersectsAllQuadrants()
{
    Point topRight = new Point(A.X + a, A.Y + b);
    return A.X < 0 && topRight.X > 0 && A.Y < 0 && topRight.Y > 0;
}
```

- **Intersect()** - проверка пересечения с другим прямоугольником:
```csharp
public bool Intersect(Rectangle other)
{
    return this.A.X < other.A.X + other.a &&
           this.A.X + this.a > other.A.X &&
           this.A.Y < other.A.Y + other.b &&
           this.A.Y + this.b > other.A.Y;
}
```

## Примеры использования

```csharp
static void Main(string[] args)
{
    // Создание и ввод прямоугольника
    Rectangle rect1 = new Rectangle();
    rect1.Input();
    rect1.Output();

    // Создание прямоугольника с заданными параметрами
    Rectangle rect2 = new Rectangle(new Point(1, 1), 3, 4);
    rect2.Output();
    
    // Вычисление периметра
    Console.WriteLine($"Периметр: {rect2.GetPerimeter()}");
    
    // Получение центра описанной окружности
    Console.WriteLine($"Центр описанной окружности: ({rect2.GetCircumscribedCircleCenter().X}, {rect2.GetCircumscribedCircleCenter().Y})");
    
    // Проверка на квадрат
    Console.WriteLine($"Это квадрат? {rect2.IsSquare()}");

    // Получение симметричного прямоугольника
    Rectangle rect3 = rect2.GetSymmetricRectangle();
    Console.WriteLine("Симметричный прямоугольник:");
    rect3.Output();

    // Проверка на подобие
    Console.WriteLine($"rect1 и rect2 подобны? {rect1.AreSimilar(rect2)}");
    
    // Проверка пересечения всех четвертей
    Console.WriteLine($"rect1 пересекает все четверти? {rect1.IntersectsAllQuadrants()}");
    
    // Проверка пересечения прямоугольников
    Console.WriteLine($"rect1 и rect2 пересекаются? {rect1.Intersect(rect2)}");
}
```

## Тестовые примеры

1. Прямоугольник по умолчанию:
```csharp
Rectangle rect = new Rectangle();
// Левая нижняя точка: (0, 0)
// Ширина: 1, Высота: 1
// Это квадрат
```

2. Прямоугольник с параметрами:
```csharp
Rectangle rect = new Rectangle(2, 3, 5, 4);
// Левая нижняя точка: (2, 3)
// Ширина: 5, Высота: 4
// Периметр: 18
```

3. Проверка пересечения:
```csharp
Rectangle rect1 = new Rectangle(0, 0, 2, 2);
Rectangle rect2 = new Rectangle(1, 1, 2, 2);
// Прямоугольники пересекаются
```

4. Проверка на квадрат:
```csharp
Rectangle rect = new Rectangle(0, 0, 3, 3);
// Это квадрат (true)
```

5. Симметричный прямоугольник:
```csharp
Rectangle rect = new Rectangle(2, 3, 4, 5);
Rectangle symRect = rect.GetSymmetricRectangle();
// Симметричный будет иметь координаты (-6, -8)
```# -

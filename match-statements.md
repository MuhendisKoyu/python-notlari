Python 3.10 ile birlikte bir çok dilde bulunan `switch-case` ifadeleri `structural pattern matching` ismiyle daha gelişmiş ve esnek kullanım senaryoları ile dile eklendi. Yeni `match-statement` ifadesi ile temel veri tiplerinin yanı sıra listeler, sözlükler gibi sıralı veri tipleri, sınıf örnekleri gibi tipler de eşleştirme ifadelerinde kullanılabilecektir.

match-statement syntax yapısı:

```py
match <ifade>:
    case <desen_1>:
        <islem_1>
    case <desen_2>:
        <islem_2>
    case <desen_3>:
        <islem_3>
    case _:
        <beklenmedik_durum>
```

- Eşleştirilecek ifade match-statement içerisinde işleme sokulur.
- İlk case deyiminden başlayarak eşleşme sağlanana kadar kontroller yapılır.
- Eşleşme sağlanması durumunda ilgili case bloğu çalıştırılır.
- Eşleşme sağlanmaması durumunda ise şu iki durumdan biri gerçekleşir:
  - `_ (wildcard)` karakteri ile oluşturulan case bloğu çalışır
  - Her hangi bir blok çalıştırılmadan program akışı devam eder.

- Temel eşleştirme ifadeleri:

Aşağıda HTTP isteği durum kodları ile ilişkilendirilmiş eşleştirme bloğu bulunmaktadır.

```py
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 404:
            return "Not found"
        case 418:
            return: "I'm a teapot"
        case _:
            return "Something's wrong with the Internet"
```

Yukarıdaki fonksiyonda durum kodu `404` olması durumunda fonksiyon `"Not Found"` dönderecektir, durum kodu `500` olması durumunda ise `_` bloğu çalışıp `"Something's wrong with the Internet"` döndürülür.

Benzer şekilde durum kodlarına ortak bir yanıt vermek istenirse kontrol ifadeler `(veya, "|")` operatörü ile zincirlenebilir.

```py
def http_error(status):
    match status:
        case 401 | 403 | 404:
            return "Not allowed"
        case _:
            return "Something's wrong with the Internet"
```

Her iki match-statement ifadesinde de `_` durumunu kaldırırsak ifade her hangi bir kontrol ifadesine girmez, dolayisiyla fonksiyonun geri donus degeri olmaz.

- Sabitler ve değişkenlerle oluşturulan eşleştirme ifadeleri:

Eşleştirilecek ifadeler karşılaştırma durumlarında değişkenlere atanarak kontrol sağlanabilir. Aşağıdaki örnekte point verisi `x` ve `y` koordinatlarına açılarak kontrol edilmiştir.

```py
# point parametresi (x, y) seklinde bir tuple nesnesi olmak uzere:
def coordinate(point):
    match point:
        case (0, 0):
            print("Origin")
        case (0, y):
            print(f"Y = {y}")
        case (x, 0):
            print(f"X = {x}")
        case (x, y)
            print(f"X = {x}, Y = {y}")
        case _:
            raise ValueError("Not a point")
```

- Siniflar ve eslestirme ifadeleri:

Verilerinizi yapılandırmak için sınıf yapıları kullanıyorsanız kurucu işleve (constructor) benzer bir şekilde sınıf özelliklerini değişkenlere atayabilirsiniz.

```py
from dataclasses import dataclass

@dataclass
class Point:
    x: int
    y: int

def location(point):
    match point:
        case Point(x=0, y=0):
            print("Origin is the point's location.")
        case Point(x=0, y=y):
            print(f"Y={y} and the point is on the y-axis.")
        case Point(x=x, y=0):
            print(f"X={x} and the point is on the x-axis.")
        case Point():
            print("The point is located somewhere else on the plane.")
        case _:
            print("Not a point")

var = 5
location(Point(0, 0))
location(Point(0, y=var))
location(Point(x=1, y=0))
location(Point(y=var, x=1))
```

- İç içe eşleştirme ifadeleri:
  
Kontrol edilecek ifadeler liste, tuple, sözlük gibi iç içe (nested) yapılarla oluşturulabilir.
```py
class Point:
    def __init__(self,x,y) -> None:
        self.x = x
        self.y = y

def location(points):
    match points:
        case []:
            print("No points in the list.")
        case [Point(x=0, y=0)]:
            print("The origin is the only point in the list.")
        case [Point(x=x, y=y)]:
            print(f"A single point {x}, {y} is in the list.")
        case [Point(x=0, y=y1), Point(x=0, y=y2)]:
            print(f"Two points on the Y axis at {y1}, {y2} are in the list.")
        case _:
            print("Something else is found in the list.")   

var = 5
location([Point(0,0)])
location([Point(0,3), Point(x=0, y= 10)])
location([Point(0,0), Point(1,1), Point(2,2)])
```

- Karmaşık eşleştirme ifadeleri ve özel durum (wildcard) karakteri:
  
Önceki örneklerde `_` karakteri diğer case'lerin sağlanmaması durumunda kullanılmıştı. Bunun dışında `_` karakteri case ifadelerinde de aynı amaçla kullanılabilmektedir.

```py
match test_variable:
    case ("warning", code, 40):
        print("A warning has been received.")
    case ("error", code, _):
        print(f"An error {code} occured .")
```


Kaynaklar ve diğer yenilikler:
- [Python Docs - What's New In Python 3.10](hhttps://docs.python.org/3.10/whatsnew/3.10.html)
- [PEP 636 - Structural Pattern Matching Tutorial](https://www.python.org/dev/peps/pep-0636/)

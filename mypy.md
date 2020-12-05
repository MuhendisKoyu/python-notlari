# Python Statik Tip Kontrolü (mypy)

## Dinamik Programlama Dili Nedir?

Değişken tiplerinin programın çalışma anında belirlendiği dillere Dinamik Programlama dilleri denir. Dinamik yapılı dillerde kullanacağınız değişken tipinin `string`, `integer` veya `double` olduğunu veya kullanacağınız dizinin elemanları için herhangi bir veri tipi belirtmenize gerek yoktur. Dolayısıyla bu durum programın yazım hızını bir miktar arttırıyor gibi gözükse de veri tipleri kaynaklı oluşabilecek hatalar programın çalışma anına kadar anlaşılamayabilir.

```python
def printChars(text):
	for char in text:
		print(char)
```

Örneğin yukarıdaki gibi bir fonksiyon `string` bir parametre alıp her karakterini teker teker ekrana basmaktadır. Bu fonksiyon her ne kadar `string` bir parametre alması için yazılmış olsa da herhangi bir iterable nesne ile çalışabilir durumdadır fakat yanlışlıkla `integer` tipli bir değişken ile çağırılırsa `TypeError: 'int' object is not iterable` hatası vererek program sonlanır.

```python
x = 1
y = '1'
print(x+y)
```

İkinci bir örnek olarak bu kadar bariz bir hata yapılmayacak olsa da yukarıdaki koda benzer öngörülmeyen bir durumda farklı bir tip hatası `(TypeError)` ile karşılaşılır. `TypeError: unsupported operand type(s) for +: 'int' and 'str'`

Python'da veri tipleri sebebiyle oluşabilecek tip hatası `(TypeError)` sorunlarının asgari seviyeye indirilmesi ve kodun başka bir programcı tarafından incelendiğinde daha anlaşılabilir olması için `mypy` ile *statik tip kontrolü* yapılabilir.

## `mypy` Nedir?

`mypy`, Python kodlarınızda statik olarak tip kontrolü yapabilmenizi sağlayan bir araçtır.

## `mypy` Stili Statik Yazımın Faydaları Nelerdir?

- Statik yazım programların okunması ve bakımını kolaylaştırır. Kodlar yazılmaktan çok okunduğu için büyük ve karışık programlarda bu önemli bir husustur.
- Statik yazım daha az test ve debug ile hataları bulmanızı sağlar. Özellikle büyük ve karmaşık projelerde önemli bir zaman tasarrufu sağlanır.
- Statik yazım kodunuzdaki bulunması zor hataları bulmanıza yardımcı olur. Böylece kodunuzun güvenilirliği artırabilir ve güvenlik sorunları azaltabilir.
- Statik yazım, hassas ve güvenilir kod tamamlama özellikli IDE'ler, statik analiz araçları vb. dahil programlama üretkenliğini veya yazılım kalitesini artırabilen çok kullanışlı geliştirme araçları oluşturmayı pratik hale getirir.
- Statik yazım, IDE'lerin daha doğru ve güvenilir kod tamamlaması, statik analiz araçları vb. programlama üretkenliğini ve yazılım kalitesini arttırabilen kullanışlı geliştirme araçları oluşturmayı pratik hale getirir
- Tek bir dil üzerinde dinamik ve statik yazımın faydalarına erişebilmeyi sağlar.

## `mypy`'ın Kullanışlı Olabileceği Durumlar

- Projeniz büyük ve karmaşıksa,
- Birden fazla kişi aynı kod üzerinde çalışıyorsa,
- Test süreçleri oldukça zaman alması (tip kontrolü geliştirme süreçlerinde hataların hızlıca bulunmasında yardımcı olur ve yapılan test sayısını azaltır),
- Projenizdeki bazı arkadaşlarınız dinamik yazım şeklini sevmezken bazıları dinamik yazımı ve Python syntax'ını seviyor olabilir. `mypy` herkesin kolayca kabul edebileceği bir çözüm olabilir,
- Yukarıdakilerden hiçbiri olmasa bile projenizi geleceğe hazır bir hale getirmek istiyor olabilirsiniz. Statik yazıma ne kadar erken başlarsanız o kadar kolay alışırsınız.


## `mypy` Nasıl Kurulur?

Terminal üzerinde `pip install mypy` komutu ile `mypy` kurulumu tamamlanır.

## Statik Tip Nitelemesi Nasıl Yapılır?

- Python'da tip niteleme syntax'ı `Değişkenİsmi: VeriTipi` şeklinde gerçekleştirilir.

```python
condition: bool = True
number: int = 2020
decimal: float = 13.13
name: str = "Mühendis Köyü"
numbers: list = [1, 2, 3, 4, 5, 6]
```

- Fonksiyon tanımlamaları şu şekilde yapılır:
`def FonksiyonAdı(Parametre1:VeriTipi, Parametre2:VeriTipi) -> GeriDonüşDeğeri:` 

```python
def printChars(text: str) -> None:
	for char in text:
		print(char)
```

```python
def reverseText(text: str) -> str:
	return text[::-1]
```

```python
from typing import Any
def check(names: list, item:Any) -> bool:
	return item in names
```

```python
from typing import List
def sumList(numbers: List[int]) -> int:
	return sum(numbers)
```

## `mypy` Nasıl Kullanılır?

Projenizi yazarken veya yazdıktan sonra veri tipleri ile ilgili olası bir problemin testini gerçekleştirmek için terminal üzerinde `mypy proje.py` komutu çalıştırılır.

```python
# guessGame.py
number: int = input("Guess Number: ")
# ...
```

```
$ mypy guessGame.py
guessGame.py:1: error: Incompatible types in assignment (expression has type "str", variable has type "int")
```

- Eğer tanımlanan değişken iterable ise ve nesnenin elemanlarının tipleri de belli ise tip nitelemesi aşağıdaki şekilde yapılır.

```python
# sample.py
from typing import Tuple
sample: Tuple[int, str, float] = (2020, "MühendisKöyü", "https://t.me/koyumuhendis")
```

```
$ mypy sample.py
sample.py:2: error: Incompatible types in assignment (expression has type "Tuple[int, str, str]", variable has type "Tuple[int, str, float]")
```

- Aşağıdaki `sumList` fonksiyonu, değişkenleri `integer` türde bir parametre beklerken fonksiyona gönderilen parametrenin 4. elemanının `string` tipinde olmasından ötürü program çalışma esnasında `TypeError` verecektir. Öncesinde `mypy` ile kontrol edilerek hata oluşumunun önüne geçilebilir.

```python
# sumNum.py
from typing import List
def sumList(numbers: List[int]) -> int:
	return sum(numbers)

print(sumList([1,2,3,"4",5]))
```

```
$ mypy sumNum.py
sumNum.py:5: error: List item 3 has incompatible type "str"; expected "int"
```

## Linter olarak MyPy

Lint veya linter, programlama hatalarını, hataları, biçimsel hataları veya şüpheli yapıları işaretlemek için kullanılan statik bir kod analiz aracıdır. Editör veya IDE'lerin büyük kısmı code linting özelliğine sahiptir.

- **VS Code - Python Linter**

Python projenizde mypy linter'ini aktif etmek için yapılması gerekenler:

- `F1` veya `Ctrl + Shift + P`
- Üstte açılan kutucuğa `Select Linter` yazıp
- `Python: Select Linter`
- Açılan listede `mypy` seçilir
- Eğer `mypy` yüklü değil ise sağ altta yüklemek için bildirim çıkacaktır

VS Code üzerinde `mypy` linting aktif edilir. Böylece tip kontrolü proje yazımı sırasında aktif olarak gerçekleşir. `mypy linter` kod yazımındaki hataların çoğunu yakalamaktadır. Tip kontrolünü daha kapsamlı olarak gerçekleştirmek için terminal üzerinden yapmak faydalı olabilir.

## Ek Not

Kod üzerindeki tip nitelemeleri programın çalışmasından tamamen bağımsızdır. Yani yapılan tip nitelemeleri programın çalışmasını etkilememektedir. Eğer yazım sırasında aktif bir linter kullanmıyor veya projenizi mypy ile manuel olarak kontrol etmiyorsanız eklediğiniz tip nitelemeleri kodunuzda pasif olarak yer alır.

Örneğin aşağıdaki Python kodundaki tip nitelemeleri tamamen hatalı olarak yazılmıştır.

```python
# sample.py
def combineNames(names:str) -> int:
	return " ".join(names)
```

Eğer yazılan kodda `mypy` ile tip kontrolü yaparsak alacağımız sonuç aşağıdaki gibi olacaktır:

```
$ mypy sample.py
sample.py:2: error: Incompatible return value type (got "str", expected "int")
sample.py:4: error: Argument 1 to "combineNames" has incompatible type "List[str]"; expected "str"
```

Fakat tip kontrolü yapmak yerine kodumuzu direkt olarak çalıştırırsak program herhangi bir hata vermeden olması gerektiği gibi çalışacaktır.

```
$ python sample.py
Mühendis Köyü Python
```


### Kaynaklar

- [MyPy Dokümantasyon](https://mypy.readthedocs.io/)
- [PEP 484](https://www.python.org/dev/peps/pep-0484/)
- [Linux Journal *Introducing Mypy*](https://www.linuxjournal.com/content/introducing-mypy-experimental-optional-static-type-checker-python)

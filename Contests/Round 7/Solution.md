# Round 7 - راه حل

## A

هر عدد زوجی همانند n را می‌توان به‌صورت جمع 2 + (n - 2) نوشت. از آن‌جایی که n زوج است، پس n - 2 نیز زوج است. برای هیچ عدد فردی نمی‌توانید چنین ترکیبی پیدا کنید چون اگر از یک فرد عددی زوج را کم کنید، عددی فرد برایتان باقی می‌ماند.

تنها استثناء قانون بالا خود عدد ۲ است که تنها به‌صورت جمع ۱ + ۱ نوشته می‌شود و غیرقابل‌قبول است.

```cpp
#include <iostream>

using namespace std;

int main() {
    int w;
    cin >> w;

    if (w % 2 == 0 && w != 2)
        cout << "YES" << endl;
    else
        cout << "NO" << endl;

    return 0;
}
```

```py
w = int(input())

if w % 2 == 0 and w != 2:
    print("YES")
else:
    print("NO")
```

## B

تنها نکته‌ی این مسئله در C++‎ این است که باید متغیر را `long long` بگیرید زیرا `int` کافی نیست.

با اینکه ورودی مسئله با اطمینان در `int` جا می‌شود، اما دقت کنید که ممکن است در هر مرحله عدد را ۳ برابر کنیم و نمی‌دانیم تا کی! این یعنی با بعضی از ورودی‌های خاص (که در این مسئله استفاده نشده) حتی `long long` هم کافی نیست.

خوش‌بختانه در پایتون چنین مشکلی وجود ندارد، چون به‌طور خودکار مقدار حافظه‌ی عدد زیاد می‌شود (تا جایی که کل حافظه پر شود).

```cpp
#include <iostream>

using namespace std;

int main() {
    long long n;
    cin >> n;

    cout << n << ' ';
    while (n != 1) {
        if (n % 2 == 0)
            n /= 2;
        else
            n = n * 3 + 1;

        cout << n << ' ';
    }
    cout << endl;

    return 0;
}
```

```py
n = int(input())

print(n, end=" ")

while n != 1:
    if n % 2 == 0:
        n //= 2
    else:
        n = n * 3 + 1
    
    print(n, end=" ")

print()
```

## C

اگر طول رشته کم‌تر یا مساوی ۱۰ باشد که همان را چاپ می‌کنید. چاپ کاراکتر اول و آخر رشته هم که آسان است، تنها مشکل در پیدا کردن تعداد حروف بین کاراکترهای اول و آخر است: کافی است از طول رشته عدد ۲ را کم کنید!

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; i++) {
        string s;
        cin >> s;

        if (s.length() <= 10)
            cout << s << endl;
        else
            cout << s[0] << s.length() - 2 << s.back() << endl;
    }

    return 0;
}
```

```py
n = int(input())

for _ in range(n):
    s = input()

    if len(s) <= 10:
        print(s)
    else:
        print(s[0], len(s) - 2, s[-1], sep='')
```

## D

آرایه‌ای تشکیل می‌دهیم که هر ایندکس i، به حرف i + 1 -ام انگلیسی اشاره کند. برای مثال `exist[0]` نشان می‌دهد که آیا حرف `a` وجود داشته یا خیر.

پس از تحلیل کل رشته و پر کردن آرایه با مقادیر مناسب، کافی است یک حلقه بزنید و اولین حرفی که وجود نداشته را چاپ کنید.

از آن‌جایی که حداکثر طول رشته برابر ۲۵ است، این تضمین را داریم که حداقل یک حرف در رشته وجود ندارد (تعداد حروف انگلیسی ۲۶تاست).

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

int main() {
    string s;
    cin >> s;

    vector<bool> exist(26, false);
    
    for (char c : s)
        exist[c - 'a'] = true;

    for (int i = 0; i < 26; i++) {
        if (!exist[i]) {
            cout << (char)('a' + i) << endl;
            break;
        }
    }

    return 0;
}
```

```py
s = input()

exist = [False] * 26

for c in s:
    exist[ord(c) - ord('a')] = True

for i in range(26):
    if not exist[i]:
        print(chr(ord('a') + i))
        break
```

## E

برای مثال طول را در نظر بگیرید. برای اینکه کاشی‌ها بتوانند طول `n` را پوشش دهند، دو حالت پیش می‌آید:

- طول مستطیل بر طول کاشی بخش‌پذیر است.
- و بخش‌پذیر نیست.

اگر بخش‌پذیر باشد که به‌طور دقیق توانستیم پوشش دهیم، در غیر این صورت باید تقسیم صحیح انجام دهیم و مقدار حاصل را با ۱ کاشی اضافه‌تر (برای پوشش مقدار باقی‌مانده) جمع کنیم.

این مفهوم اسم دیگری دارد که تابع `ceil()‎` در زبان‌های مختلف این کار را انجام می‌دهد.

```cpp
#include <iostream>
#include <cmath>

using namespace std;

int main() {
    int n, m, a;
    cin >> n >> m >> a;

    long long w = ceil((float)n / a);
    long long h = ceil((float)m / a);

    cout << w * h << endl;

    return 0;
}
```

```py
import math

n, m, a = map(int, input().split())

w = math.ceil(n / a)
h = math.ceil(m / a)

print(w * h)
```

## لینک‌ها

| |
| - |
| [مسابقه](https://vjudge.net/contest/715866) |
| [اطلاعیه مسابقه](./Announcement.md) |
|  |
| [راهنمای شرکت در مسابقات](../../Introduction/Get%20Started.md#شرکت-در-مسابقه) |

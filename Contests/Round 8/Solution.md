# Round 8 - راه حل

## A

از آن‌جایی که هر عبارت تنها یا شامل دو عدد «+» یا دو عدد «-» است، کافی است فقط در هر عبارت وجود کاراکتر «+» یا «-» را بررسی کنیم.

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    int n;
    cin >> n;

    int x = 0;
    for (int i = 0; i < n; ++i) {
        string s;
        cin >> s;

        for (char c : s) {
            if (c == '+') {
                ++x;
                break;
            }
            if (c == '-') {
                --x;
                break;
            }
        }
    }

    cout << x << endl;

    return 0;
}
```

```py
n = int(input())

x = 0
for _ in range(n):
    s = input()

    if '+' in s:
        x += 1
    else:
        x -= 1

print(x)
```

## B

آرایه‌ی `exist` می‌سازیم و به هر عددی که رسیدیم، مقدار آن را `true` می‌کنیم. در نهایت نگاه می‌کنیم که کدام عدد `true` نشده است.

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    int n;
    cin >> n;

    vector<char> exist(n + 1, false);
    for (int i = 0; i < n - 1; ++i) {
        int a;
        cin >> a;
        exist[a] = true;
    }

    for (int x = 1; x <= n; ++x) {
        if (!exist[x]) {
            cout << x << endl;
            break;
        }
    }
    
    return 0;
}
```

```py
n = int(input())

exist = [False] * (n + 1)

input = list(map(int, input().split()))

for x in input:
    exist[x] = True

for i in range(1, n + 1):
    if not exist[i]:
        print(i)
        break
```

## C

کافی است کل رشته‌ی ورودی را پیمایش کنیم. ابتدا کاراکتر را به حرف کوچک تبدیل می‌کنیم و اگر حرف مصوت نبود، آن را به‌همراه یک نقطه چاپ می‌کنیم.

ممکن است با خود بگویید که از کجا باید بدانیم که تابع `tolower()` در C++‎ در کتاب‌خانه‌ی `cctype` است؟ یک راه ساده این است که این کتاب‌خانه را اینکلود کنید: `bits/stdc++.h`

تمام کتاب‌خانه‌ها به‌طور خودکار اضافه می‌شوند.

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <cctype>

using namespace std;

int main() {
    vector<char> vowels {'a', 'o', 'y', 'e', 'u', 'i'};

    string s;
    cin >> s;

    for (char c : s) {
        c = tolower(c);

        bool is_vowel = false;
        for (char vowel : vowels) {
            if (c == vowel) {
                is_vowel = true;
                break;
            }
        }

        if (!is_vowel) {
            cout << '.' << c;
        }
    }

    cout << endl;

    
    return 0;
}
```

```py
vowels = ['a', 'o', 'y', 'e', 'u', 'i']
s = input().lower()

for c in s:
    if not c in vowels:
        print('.' + c, end='')

print()
```

## D

از آن‌جایی که در هر خط هر کاراکتر یک بار تکرار می‌شود، خطی را پیدا می‌کنیم که شامل علامت سؤال باشد. سپس حرفی که در آن خط وجود ندارد را چاپ می‌کنیم.

```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

int main() {
    int t;
    cin >> t;

    for (int test = 0; test < t; ++test) {
        vector<string> grid(3);
        for (int i = 0; i < 3; ++i)
            cin >> grid[i];

        for (int i = 0; i < 3; ++i) {
            bool target = false;

            for (char c : grid[i])
                if (c == '?')
                    target = true;
            
            if (!target)
                continue;

            vector<char> exist(3, false);
            for (char c : grid[i])
                exist[c - 'A'] = true;
            
            for (int j = 0; j < 3; ++j) {
                if (!exist[j]) {
                    cout << (char)('A' + j) << endl;
                    break;
                }
            }
            
            break;
        }
    }

    return 0;
}
```

```py
chars = ['A', 'B', 'C']
t = int(input())

for _ in range(t):
    grid = [str] * 3
    for i in range(3):
        grid[i] = input()
    
    for line in grid:
        if not '?' in line:
            continue
            
        for c in chars:
            if not c in line:
                print(c)
                break
        break
```

## E

فرض کنید یک ستون با ارتفاع ۸ در سمت چپ یک ستون دیگر با ارتفاع ۲ قرار دارد. اگر جاذبه را تغییر دهیم، می‌بینیم که ستون سمت راست (که قبلاً مقدار ۲ داشته) اکنون مقدار ۸ را دارد و ستون سمت چپ (که قبلاً مقدار ۸) را داشته اکنون مقدار ۲ را دارد.

متوجه می‌شوید که تغییر مقدار ارتفاع‌ها تا زمانی ادامه پیدا می‌کند که برای هیچ ستونی که سمت راست ستون دیگر باشد، ارتفاع آن کم‌تر نباشد. (ترتیب اعداد صعودی باشد)

این به‌طور خلاصه مفهوم مرتب‌سازی اعداد در یک آرایه را نشان می‌دهد.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n;
    cin >> n;

    vector<int> arr(n);
    for (int i = 0; i < n; ++i)
        cin >> arr[i];
    
    sort(arr.begin(), arr.end());

    for (int i = 0; i < n; ++i)
        cout << arr[i] << ' ';
    cout << endl;

    return 0;
}
```

```py
n = int(input())
arr = list(map(int, input().split()))

arr.sort()

print(*arr)
```

## F

می‌توان به هر تعداد دلخواهی `S` را چرخاند. اما ۴ بار چرخاندن `S` ما را به حالت اولیه می‌رساند، پس فقط کافی است بررسی کنیم با فقط  ۰، ۱، ۲، یا ۳ چرخش کدام حالت بهتر است.

از آن‌جایی که چرخاندن کل `S` کاری پرهزینه است، ما در ظاهر آن را می‌چرخانیم و به دنبال مکانی از `T` می‌گردیم که معادل آن چرخش باشد.

کاراکتری که در مکان `S[x][y]` قرار داشته باشد، بعد از یک بار چرخش به مکان `S[y][n - x - 1]` می‌رود. پس لازم است که بررسی کنیم `S[x][y]` برابر است با ‌`T[y][n - x - 1]` یا نه. اگر برابر نبود، پس مجبوریم از عملیات اول (تغییر رنگ) استفاده کنیم.

پس از محاسبه‌ی تعداد تغییر رنگ لازم برای هر چرخش، باید تعداد کل عملیات‌ها را محاسبه کنیم. این عدد برابر تعداد چرخش + تعداد تغییر رنگ است. کوچک‌ترین عدد به‌دست‌آمده، جواب مسئله است.

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <utility>

using namespace std;

pair<int, int> rotate(pair<int, int> pos, int n) {
    return {pos.second, n - pos.first - 1};
}

pair<int, int> rotate(pair<int, int> pos, int n, int count) {
    for (int i = 0; i < count; ++i)
        pos = rotate(pos, n);
    return pos;
}

int main() {
    int n;
    cin >> n;

    vector<string> s(n), t(n);
    for (string& line : s)
        cin >> line;
    for (string& line : t)
        cin >> line;
    
    vector<int> color_invert_count(4, 0); // {0 degree, 90 degree, 180 degree, 270 degree}

    for (int x = 0; x < n; ++x) {
        for (int y = 0; y < n; ++y) {
            for (int count = 0; count < 4; ++count) {
                pair<int, int> result = rotate({x, y}, n, count);

                if (s[x][y] != t[result.first][result.second])
                    ++color_invert_count[count];
            }
        }
    }

    int answer = color_invert_count[0] + 0; // 0 rotations
    for (int i = 1; i < 4; ++i)
        answer = min(answer, color_invert_count[i] + i); // i is the count of rotations
    
    cout << answer << endl;

    return 0;
}
```

```py
def rotate_once(x, y, n):
    return y, n - x - 1

def rotate(x, y, n, count):
    for _ in range(count):
        x, y = rotate_once(x, y, n)
    
    return x, y

n = int(input())
s = [str] * n
t = [str] * n

for i in range(n):
    s[i] = input()
for i in range(n):
    t[i] = input()

color_invert_count = [0] * 4
for x in range(n):
    for y in range(n):
        for count in range(4):
            t_x, t_y = rotate(x, y, n, count)

            if s[x][y] != t[t_x][t_y]:
                color_invert_count[count] += 1
            
ans = color_invert_count[0] + 0
for i in range(1, 4):
    ans = min(ans, color_invert_count[i] + i)

print(ans)
```

## لینک‌ها

| |
| - |
| [مسابقه](https://vjudge.net/contest/717895) |
| [اطلاعیه مسابقه](./Announcement.md) |
|  |
| [راهنمای شرکت در مسابقات](../../Introduction/Get%20Started.md#شرکت-در-مسابقه) |

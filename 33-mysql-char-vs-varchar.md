# MySQL CHAR v.s. VARCHAR

首先來看一下這兩種資料型別的差別\(可容忍的長度\):

| CHAR | VARCHAR |
| :--- | :--- |
| 0~255 | 0~65535 |

從CHAR\(5\)的角度來看儲存:

| Value | Actual Storage |
| :--- | :--- |
| 'ap' | 'ap   ' \(5 bytes, 這邊多存了3個空白\) |
| 'april' | 'april' \(5 bytes\) |
| 'april1' | 'april' \(5 bytes\) |

從VARCHAR\(5\)的角度來看儲存:

| Value | Actual Storage |
| :--- | :--- |
| 'ap' | 'ap' \(3 bytes, 1 byte存長度\) |
| 'apri' | 'apri' \(5 bytes\) |
| 'april' | 'april' \(6 bytes\) |
| 'april1' | 'april' \(6 bytes, 多的一個byte是存長度, 不是存字元'1'\) |

-&gt; 相較於CHAR, VARCHAR會多花1~2個byte去儲存長度\(length prefix\), 至於是多花1個還是2個byte, 則必須根據當前儲存資料之大小來判斷, 若資料少於255個char, 則只會使用1個byte去儲存長度, 反之, 若VARCHAR超過255個char的話, 則會使用2個bytes去儲存資料長度.

根據以上的比較, 試想以下情境: 若你有個欄位可能是要當flag用的\(y/n\), 則你可以選擇CHAR\(1\)而不是VARCHAR\(1\), 因為VARCHAR\(1\)會多用一個byte去儲存長度.


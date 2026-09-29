# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_249480.jpg
## adasind_261480.jpg
- L9+R7 mid ATTRIBUTE
- L1+R2 mid ATTRIBUTE
- L3 mid SPURIOUS
- L4 mid SPURIOUS
## adasind_265065.jpg
- L4 mid IGNORE_SCOPE
- L3+R3 mid WRONG_CLASS
- R1 mid MISSING
- R5 mid MISSING
- R6 mid MISSING
- R7 mid MISSING
- R8 mid MISSING

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 4 | 4 | 0 | 1 |
| mid | 13 | 7 | 6 | 2 |
| edge | 0 | 0 | 0 | 0 |

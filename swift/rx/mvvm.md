# MVVM

## 制約

### 入力と出力の違いが識別できるか

プロトコルにより入力と出力が分かれており、採用するそれぞれの型で `transform` メソッドの引数と戻り値として表現することを条件をされている

### 入力は外部から読み出しできないようになっているか

入力は `transform` メソッドの引数によって渡されているため、外部から読み出しされることはない

### 入力は必須か

入力は `transform` メソッドの必須な引数とすることで、利用時に設定し忘れるとコンパイルエラーとなる

### 入力はストリームを渡せるか

入力はDriverを渡しており、ストリームを渡せる
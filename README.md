##  Pergunta 1: Como estão representadas as funções `soma`, `multiplica` e `calcula` em IR?

No LLVM IR, as funções do código C são representadas pela diretiva `define`, com os tipos de entrada, retorno e as instruções em linguagem intermediária. Cada parâmetro é identificado por `%` seguido de um número (ex: `%0`, `%1`), e as instruções são expressas em estilo SSA.

### 🔹 Função `soma`:

```llvm
define dso_local i32 @soma(i32 noundef %0, i32 noundef %1) #0 {
  %3 = alloca i32, align 4
  %4 = alloca i32, align 4
  store i32 %0, ptr %3, align 4
  store i32 %1, ptr %4, align 4
  %5 = load i32, ptr %3, align 4
  %6 = load i32, ptr %4, align 4
  %7 = add nsw i32 %5, %6
  ret i32 %7
}
```

### 🔹 Função `multiplica`:

```llvm
define dso_local i32 @multiplica(i32 noundef %0, i32 noundef %1) #0 {
  %3 = alloca i32, align 4
  %4 = alloca i32, align 4
  store i32 %0, ptr %3, align 4
  store i32 %1, ptr %4, align 4
  %5 = load i32, ptr %3, align 4
  %6 = load i32, ptr %4, align 4
  %7 = mul nsw i32 %5, %6
  ret i32 %7
}
```

### 🔹 Função `calcula`:

```llvm
define dso_local i32 @calcula(i32 noundef %0) #0 {
  %2 = alloca i32, align 4
  %3 = alloca i32, align 4
  store i32 %0, ptr %3, align 4
  %4 = load i32, ptr %3, align 4
  %5 = icmp sgt i32 %4, 10
  br i1 %5, label %6, label %9

6:                                                ; preds = %1
  %7 = load i32, ptr %3, align 4
  %8 = call i32 @multiplica(i32 noundef %7, i32 noundef 2)
  store i32 %8, ptr %2, align 4
  br label %12

9:                                                ; preds = %1
  %10 = load i32, ptr %3, align 4
  %11 = call i32 @soma(i32 noundef %10, i32 noundef 5)
  store i32 %11, ptr %2, align 4
  br label %12

12:                                               ; preds = %9, %6
  %13 = load i32, ptr %2, align 4
  ret i32 %13
}
```
---

## Pergunta 2: O que aparece no IR que representa a condição `if (valor > 10)`?

A condição `valor > 10` é representada em LLVM IR pelas instruções de comparação (`icmp`) e salto condicional (`br`).

```llvm
%5 = icmp sgt i32 %4, 10
br i1 %5, label %6, label %9
```

## Pergunta 3: Como são representadas as chamadas às funções auxiliares em IR?

As chamadas às funções auxiliares como `soma` e `multiplica` são representadas com a instrução `call`, especificando a função, os tipos de parâmetros e os valores passados.

### 🔹 Chamada à função `multiplica`:

```llvm
%7 = load i32, ptr %3, align 4
%8 = call i32 @multiplica(i32 noundef %7, i32 noundef 2)
store i32 %8, ptr %2, align 4
```

### 🔹 Chamada à função `soma`:

```llvm
%10 = load i32, ptr %3, align 4
%11 = call i32 @soma(i32 noundef %10, i32 noundef 5)
store i32 %11, ptr %2, align 4
```

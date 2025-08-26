# 📝 Relatório do Laboratório 2 - Chamadas de Sistema

---

## 1️⃣ Exercício 1a - Observação printf() vs 1b - write()

### 💻 Comandos executados:
```bash
strace -e write ./ex1a_printf
strace -e write ./ex1b_write
```

### 🔍 Análise

**1. Quantas syscalls write() cada programa gerou?**
- ex1a_printf: 9 syscalls
- ex1b_write: 7 syscalls

**2. Por que há diferença entre os dois métodos? Consulte o docs/printf_vs_write.md**

```
[Sua análise aqui]
```

**3. Qual método é mais previsível? Por quê você acha isso?**

```
[Sua análise aqui]
```

---

## 2️⃣ Exercício 2 - Leitura de Arquivo

### 📊 Resultados da execução:
- File descriptor: 3
- Bytes lidos: 127

### 🔧 Comando strace:
```bash
strace -e openat,read,close ./ex2_leitura
```

### 🔍 Análise

**1. Qual file descriptor foi usado? Por que não começou em 0, 1 ou 2?**

```
oi usado o file 3, não começou com o 0, porque ele é usado para entrada, o 1 é usado para a saída e o 2 é usado para a saída com erro.
```

**2. Como você sabe que o arquivo foi lido completamente?**

```
Porque ele me mostrou o número de Bytes lidos nesse conteúdo.
```

**3. Por que verificar retorno de cada syscall?**

```
Para vermos se existem alguns erros e ver se tudo foi feito corretamente 
```

---

## 3️⃣ Exercício 3 - Contador com Loop

### 📋 Resultados (BUFFER_SIZE = 64):
- Linhas: 25 (esperado: 25)
- Caracteres: 1300
- Chamadas read(): 21
- Tempo: 0.000515 segundos

### 🧪 Experimentos com buffer:

| Buffer Size | Chamadas read() | Tempo (s) |
|-------------|-----------------|-----------|
| 16          |      21         | 0.000112  |
| 64          |      21         | 0.000515  |
| 256         |      21         | 0.000094  |
| 1024        |      21         | 0.000087  |

### 🔍 Análise

**1. Como o tamanho do buffer afeta o número de syscalls?**

```
Ele irá afetar reduzindo o número de syscalls
```

**2. Todas as chamadas read() retornaram BUFFER_SIZE bytes? Discorra brevemente sobre**

```
Não, isso ocorreu, porque a última retornou menos bytes.
```

**3. Qual é a relação entre syscalls e performance?**

```
A relação, é que quanto menos syscalls terá menos sobrecarga do kernel e com isso ele irá ficar mais rápido.
```

---

## 4️⃣ Exercício 4 - Cópia de Arquivo

### 📈 Resultados:
- Bytes copiados: 1364
- Operações: 6
- Tempo: 0.000144 segundos
- Throughput: 9250.22 KB/s

### ✅ Verificação:
```bash
diff dados/origem.txt dados/destino.txt
```
Resultado: [ ] Idênticos [x] Diferentes

### 🔍 Análise

**1. Por que devemos verificar que bytes_escritos == bytes_lidos?**

```
Para realmente ver se todos os dados foram lidos corretamentes e realmente escritos, para não perdemos informação. 
```

**2. Que flags são essenciais no open() do destino?**

```
O_CREAT, O_TRUNC, O_WRONLY.
```

**3. O número de reads e writes é igual? Por quê?**

```
Sim, porque cada read tem um write igual.
```

**4. Como você saberia se o disco ficou cheio?**

```
Porque irá retornar -1.
```

**5. O que acontece se esquecer de fechar os arquivos?**

```
Os dados não serão gravados, e daria oara identificar um arquivo aberto.
```

---

## 🎯 Análise Geral

### 📖 Conceitos Fundamentais

**1. Como as syscalls demonstram a transição usuário → kernel?**

```
As syscalls mostram a mudança porque quando a gente precisa de algo do sistema,o programa sai do modo usuário e o kernel faz pra gente, depois volta.

```

**2. Qual é o seu entendimento sobre a importância dos file descriptors?**

```
File descriptors são números que representam arquivos, ajudam a gente a ler e escrever sem mexer direto no hardware, deixando mais facil.

```

**3. Discorra sobre a relação entre o tamanho do buffer e performance:**

```
Se o buffer for maior, tem como ler ou escrever mais dados de uma vez, deixando mais rápido. Se for pequeno, fica lento por causa das várias chamadas ao kernel
```

### ⚡ Comparação de Performance

```bash
# Teste seu programa vs cp do sistema
time ./ex4_copia
time cp dados/origem.txt dados/destino_cp.txt
```

**Qual foi mais rápido?** teste do meu prgrama 

**Por que você acha que foi mais rápido?**

```
Porque o meu ficou com 
real    0m0.005s
user    0m0.001s
sys     0m0.003s 
e o do cp ficou com 
real    0m0.010s
user    0m0.002s
sys     0m0.005s
```

---

## 📤 Entrega
Certifique-se de ter:
- [ x] Todos os códigos com TODOs completados
- [ x] Traces salvos em `traces/`
- [ x] Este relatório preenchido como `RELATORIO.md`

```bash
strace -e write -o traces/ex1a_trace.txt ./ex1a_printf
strace -e write -o traces/ex1b_trace.txt ./ex1b_write
strace -o traces/ex2_trace.txt ./ex2_leitura
strace -c -o traces/ex3_stats.txt ./ex3_contador
strace -o traces/ex4_trace.txt ./ex4_copia
```
# Bom trabalho!

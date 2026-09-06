+++
title = 'Como Tratamos Erros em Rust'
date = '2026-09-06T12:01:08-03:00'
draft = false
image = '/images/pikmin-reference.webp'
+++

![Referência Pikmin](/images/pikmin-reference.webp)

> "I've made a new discovery! The Pikmin can choose their own routes! But...does this indicate rational thought or just basic instinct? Unfortunately, I cannot determine that at this point. I will be vigilant in my studies, though..."

Por volta de 2017/2019 (quase 10 anos atrás 😭), eu dei o meu primeiro pulo no Rust
Na época, o ecossistema era bem menor, e para um desenvolvedor iniciante que só sabia Lua e Javascript,
coisas como ter que esperar para rodar o programa (compilação) e ter que se preocupar com o compilador reclamando, eram só problemas criados sem motivo algum

O tempo passou, e nesse meio tempo eu aprendi muita coisa, me apaixonei por Go, mexi bastante com C, e trabalhei em um bocadinho de projetos

Esse é o primeiro post do **Diário de Bordo Rust**: uma série de postagens onde vou tentar explicar coisas do Rust para desenvolvedores Go, à medida que eu mesmo vou aprendendo (ou reaprendendo) a linguagem

---

> _"Go oferece simplicidade, Rust oferece expressividade"_ 

Acho que esse é um dos grandes motivos de o pessoal
falar que ele tem uma grande curva de aprendizado

Uma coisa que você vai perceber escrevendo Rust é que
a linguagem te oferece diferentes formas de fazer
a mesma coisa. Existe muita oportunidade para melhorar
o código e deixar ele mais claro e direto

Você vai ver seu código envelhecer bem rápidinho
conforme você aprende novos truques e vira um programador
Rust melhor

---

> _"Sem `nil`, sem `null`"_

Em Go você provavelmente já viu ou escreveu código parecido
com isso:

```Go
if value != nil {
	// usando o valor de forma "segura"
}
// No Go isso é seguro, o pior que pode acontecer é um panic
// (em C, um ponteiro nulo pode virar undefined behavior/segfault)
```
E talvez você já até tenha esquecido de checar por `nil`
alguma vez

Você não deve ver nulos no nosso amigo Rust, por conta do enum `Option<T>` e
do compilador, que obriga que a gente trate isso

Basicamente, se você chama uma função onde o valor pode ser vazio, você provavelmente vai receber um `Option<T>`. Ele vem com 2 valorezinhos, `Some(T)` e `None`, e Rust obriga que você defina um `match` para cada valor, para dizer o que deve acontecer se o valor
vier como `None` (vazio), ou se veio como `Some` (algo dentro)

Você também pode ser explícito e só dizer `.unwrap()`, que é basicamente dizer que você garante que vai ter algo, e se não tiver pode dar panic (tipo esquecer de fazer um nil check no Go)

---

> _"if err != nil"_ 

Seguindo a mesma regra do nulo, se você recebe um valor que
pode ter um erro, você provavelmente vai receber um valor do tipo enum `Result<T, E>`

Esse carinha é bem parecido com o `Option`, ele vem com `Ok(T)` (retorno sem erro)
ou `Err(E)` (retorno com erro)

Você pode ignorar o erro usando o `.unwrap()` (se houver Err, dá panic)

Em Go, quando você não quer tratar o erro ali mesmo, você propaga ele pra cima:

```Go
func fazerTalCoisa() error {
	resultado, err := coisar()
	if err != nil {
		return err // retornando o erro
	}
	
	// usando o resultado...
	 
	return nil
}
```

Em Rust existe um jeito bem mais curto de fazer a mesma coisa: o operador `?`

Se o valor for `Err`, a função já retorna esse erro na hora, sem precisar
escrever o `if`:

```Rust
fn fazer_tal_coisa() -> Result<(), std::io::Error> { // tipo do erro explícito
	let resultado = coisar()?; // se coisar() der Err, a função já retorna o erro
	
	// usando o resultado...
	 
	Ok(()) // deu tudo certo, retorna Ok sem valor nenhum dentro
}
```

Repara no tipo de retorno: `Result<(), std::io::Error>`. Isso quer dizer "ou
dá certo e não retorna nada (`()`, o tipo unit), ou dá errado e retorna um
`std::io::Error`". Esse `io::Error` já vem prontinho da biblioteca padrão do
Rust, é o mesmo tipo de erro que você recebe ao mexer com arquivo, rede,
etc

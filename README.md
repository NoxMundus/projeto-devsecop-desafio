# Desafio DevSecOps — Gerenciador de Tarefas

## Sobre o Projeto
Este repositório faz parte do desafio prático do módulo de DevSecOps da ADA Tech.
Você receberá este projeto com vulnerabilidades propositais e uma pipeline incompleta.
Seu objetivo é **implementar a pipeline de segurança** e **corrigir as vulnerabilidades**.

## Estado atual
A pipeline está **incompleta**. Os steps de segurança precisam ser implementados por você.

## Sua missão
1. Implementar os steps de segurança no `pipeline.yml`
2. Fazer a pipeline **quebrar** ao detectar os problemas
3. Corrigir as vulnerabilidades encontradas
4. Fazer a pipeline **passar** com tudo verde ✅
5. Documentar o funcionamento da pipeline neste README

## O que implementar
- [ ] Secrets Scanning com **Gitleaks**
- [ ] SAST com **Semgrep**
- [ ] SCA com **Grype**
- [ ] Deploy com **GitHub Pages**

## Como a pipeline funciona
> **Substitua este bloco pela sua explicação após implementar a pipeline.**
> Descreva cada step, o que ele faz e por que ele é importante para a segurança.

### Primeiro passo - Gitleaks

Esse step é importante para procurar referencias diretas no codigos de segredos. 
Esses segredos deveriam estar sendo passados como variaveis e estar contidos em algum vault que a aplicação então faz referencia para e busca em. 
Dessa forma reduzindo a exposição dos mesmos.
Durante primeiros testes com o gitleaks, foi encontrado problema com só com o API_KEY e não estava pegando o DB_PASSWORD

Foi preciso então colocar um novo arquivo de configuração do gitleaks para forçar mais configs alem do padrão. 
Uma vez feito isso pegou o DB_PASSWORD

Eu basicamente aqui fiz uso do GitSecrets para "ocultar" as variaveis. Então agora esta como referenciado no script e no pipeline as variaveis. 

### Segundo Step - Semgrep

Esse step é importante para procurar vulnerabilidades no codigo do proprio time, como o exemplo abaixo encontrado.
Pode ocorrer de que o time escreva um codigo que seja funcional, mas que exponha vulnerabilidades desnecessarias aumentando a superficie de ataque da aplicação como um todo, esses problemas precisam ser corrigidos.

Ao executar o Semgrep apontou um problema com o:
    src/script.js
    ❯❱ javascript.browser.security.eval-detected.eval-detected
          ❰❰ Blocking ❱❱
          Detected the use of eval(). eval() can be dangerous if used to evaluate dynamic content. If this 
          content can be input from outside the program, this may be a code injection vulnerability. Ensure
          evaluated content is not definable by external sources.                                          
          Details: https://sg.run/7ope                                                                     
                                                                                                           
           29┆ eval('console.log("Tarefa adicionada: ' + input.value + '")');

Conforme listado então, seria uma vulnerabilidade de Injection, foi feito a correção do codigo.

### Terceiro Step - Grype

Esse step é importante para procurar vulnerabilidades no supplychain, ou seja, caso alguma dependencia que está sendo utilizada tem uma vulnerabilidade já mapeada.
Como hoje em dia é muito comum que se utilize bibliotecas de terceiros e assim por diante, é vital se verificar se essas mesmas tem vulnerabilidades conhecidas. 

Para esse step o aparentemente não treve erros? Imagino que por ser um codigo mais simples ele não chegou a importar algo com problema. 

Run curl -sSfL https://raw.githubusercontent.com/anchore/grype/main/install.sh | sh -s -- -b /usr/local/bin
[info] checking github for the current release tag 
[info] fetching release script for tag='v0.115.0' 
[info] checking github for the current release tag 
[info] using release tag='v0.115.0' version='0.115.0' os='linux' arch='amd64' 
[info] installed /usr/local/bin/grype 
[0000]  WARN no explicit name and version provided for directory source, deriving artifact ID from the given path (which is not ideal) from=syft
No vulnerabilities found

## URL de Produção
> [Link para a pagina desse projeto](https://noxmundus.github.io/projeto-devsecop-desafio/)

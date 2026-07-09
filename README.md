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
Dessa forma reduzindo a exposição do mesmo.
Durante primeiros testes com o gitleaks, foi encontrado problema com só com o
const API_KEY = ghp_xK92mNpL34rTvQ87wZaB56cDeFgHiJkL;

Não estava pegando o 
const DB_PASSWORD = admin@prod#2024;

Foi preciso então colocar um novo arquivo de configuração do gitleaks para forçar mais configs alem do padrão. 
Uma vez feito isso pegou o DB_PASSWORD

Ambas variaveis foram então corrigidas para valores temporarios que devem ser corrigidos no futuro. 
Com isso dito, acho que o arquivo de conf do GitLeaks teria que ser editado de forma correta para uso real, o atual esta talvez abrangente demais e não sei se isso é uma boa prática. 

## URL de Produção
> Adicione aqui o link do GitHub Pages após o deploy.

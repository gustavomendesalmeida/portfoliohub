# portfoliohub
Plataforma centralizada para exibir e gerenciar meus projetos e portfólios
<!doctype html>
<html lang="pt-br">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>PortfolioHUB</title>
<style>
  :root{--rosa:#9b1f7a;--rosa2:#d62b8a;--bg:#faf7fb;--txt:#222;--card:#fff}
  *{box-sizing:border-box}
  body{margin:0;font-family:Arial,Helvetica,sans-serif;background:var(--bg);color:var(--txt)}
  header{background:linear-gradient(135deg,var(--rosa),var(--rosa2));color:#fff;padding:48px 20px;text-align:center}
  header h1{margin:0 0 8px;font-size:2.2rem}
  header p{margin:0;opacity:.9}
  main{max-width:1000px;margin:0 auto;padding:24px 16px}
  .busca{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:20px}
  .busca input,.busca select{padding:10px;border:1px solid #ccc;border-radius:8px;font-size:1rem}
  .busca input{flex:1;min-width:200px}
  .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:16px}
  .card{background:var(--card);border-radius:12px;padding:18px;box-shadow:0 2px 8px rgba(0,0,0,.08);display:flex;flex-direction:column;gap:10px}
  .card h3{margin:0;color:var(--rosa)}
  .card p{margin:0;font-size:.95rem;line-height:1.4}
  .tags{display:flex;gap:6px;flex-wrap:wrap}
  .tag{background:#f2d7e8;color:var(--rosa);padding:3px 9px;border-radius:20px;font-size:.78rem}
  .status{font-size:.8rem;font-weight:bold}
  .concluido{color:#1b7f3b}.andamento{color:#b36b00}
  .links{margin-top:auto;display:flex;gap:10px}
  .links a{text-decoration:none;background:var(--rosa);color:#fff;padding:7px 12px;border-radius:8px;font-size:.9rem}
  .links a.sec{background:#fff;color:var(--rosa);border:1px solid var(--rosa)}
  .vazio{text-align:center;color:#777;padding:30px}
  footer{text-align:center;padding:24px;color:#777;font-size:.85rem}
</style>
</head>
<body>
<header>
  <h1>PortfolioHUB</h1>
  <p id="sub">Meus projetos e portfólios digitais em um só lugar</p>
</header>

<main>
  <div class="busca">
    <input id="q" type="search" placeholder="Buscar projeto ou tecnologia...">
    <select id="filtro">
      <option value="">Todos os status</option>
      <option>Concluído</option>
      <option>Em andamento</option>
    </select>
  </div>
  <div id="lista" class="grid"></div>
  <div id="vazio" class="vazio" hidden>Nenhum projeto encontrado.</div>
</main>

<footer>
  <span id="autor"></span> · Versionado com Git e GitHub
</footer>

<script>
 
  const AUTOR = "Gustavo Mendes Almeida";
  const USUARIO_GITHUB = "gustavomendesalmeida"; 

  const projetos = [
    {
      nome: "Study Hub",
      descricao: "Organização do portfólio profissional em TI.",
      tecnologias: ["Git", "GitHub", "Documentação"],
      status: "Em andamento",
      repo: "https://github.com/" + USUARIO_GITHUB + "/study-hub",
      demo: ""
    },
    {
      nome: "TaskTracker",
      descricao: "Aplicação de tarefas conteinerizada com Docker.",
      tecnologias: ["Docker", "Docker Compose"],
      status: "Concluído",
      repo: "https://github.com/" + USUARIO_GITHUB + "/tasktracker",
      demo: ""
    },
    {
      nome: "Projeto Python",
      descricao: "Descreva aqui o que o projeto faz.",
      tecnologias: ["Python"],
      status: "Concluído",
      repo: "https://github.com/" + USUARIO_GITHUB + "/projeto-python",
      demo: ""
    }
  ];
  // =======================

  document.getElementById("autor").textContent = "© " + new Date().getFullYear() + " " + AUTOR;

  const lista = document.getElementById("lista");
  const vazio = document.getElementById("vazio");
  const q = document.getElementById("q");
  const filtro = document.getElementById("filtro");

  function el(tag, cls, texto){
    const e = document.createElement(tag);
    if (cls) e.className = cls;
    if (texto) e.textContent = texto;
    return e;
  }

  function render(){
    const termo = q.value.trim().toLowerCase();
    const st = filtro.value;
    lista.replaceChildren();
    const filtrados = projetos.filter(p => {
      const alvo = (p.nome + " " + p.descricao + " " + p.tecnologias.join(" ")).toLowerCase();
      return alvo.includes(termo) && (!st || p.status === st);
    });
    vazio.hidden = filtrados.length > 0;

    filtrados.forEach(p => {
      const card = el("div", "card");
      card.append(el("h3", "", p.nome));
      card.append(el("span", "status " + (p.status === "Concluído" ? "concluido" : "andamento"), p.status));
      card.append(el("p", "", p.descricao));
      const tags = el("div", "tags");
      p.tecnologias.forEach(t => tags.append(el("span", "tag", t)));
      card.append(tags);

      const links = el("div", "links");
      const a = el("a", "", "Código no GitHub");
      a.href = p.repo; a.target = "_blank"; a.rel = "noopener";
      links.append(a);
      if (p.demo){
        const d = el("a", "sec", "Ver demo");
        d.href = p.demo; d.target = "_blank"; d.rel = "noopener";
        links.append(d);
      }
      card.append(links);
      lista.append(card);
    });
  }

  q.addEventListener("input", render);
  filtro.addEventListener("change", render);
  render();
</script>
</body>
</html>

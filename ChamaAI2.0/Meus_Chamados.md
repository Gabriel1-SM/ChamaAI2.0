<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Meus Chamados - Gerenciador de Chamados</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" />
  <link href="https://cdn.jsdelivr.net/npm/simple-datatables@7.1.2/dist/style.min.css" rel="stylesheet" />
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.0/font/bootstrap-icons.css">
  <style>
    :root {
      --primary-color: #4e73df;
      --secondary-color: #858796;
      --success-color: #1cc88a;
      --info-color: #36b9cc;
      --warning-color: #f6c23e;
      --danger-color: #e74a3b;
      --light-color: #f8f9fc;
      --dark-color: #5a5c69;
    }
    
    body {
      background-color: #f8f9fa;
      font-family: 'Nunito', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
    }
    
    .user-container {
      max-width: 1200px;
      margin: 30px auto;
      padding: 30px;
      background-color: #ffffff;
      border-radius: 15px;
      box-shadow: 0 0.15rem 1.75rem 0 rgba(58, 59, 69, 0.15);
    }
    
    .page-header {
      border-bottom: 1px solid #e3e6f0;
      padding-bottom: 1rem;
      margin-bottom: 2rem;
    }
    
    .section-title {
      font-weight: 600;
      font-size: 1.1rem;
      color: var(--dark-color);
      margin-bottom: 1.5rem;
      display: flex;
      align-items: center;
    }
    
    .section-title i {
      margin-right: 10px;
      color: var(--primary-color);
    }
    
    .table th {
      background-color: #f8f9fc;
      color: var(--dark-color);
      font-weight: 700;
      text-transform: uppercase;
      font-size: 0.75rem;
      letter-spacing: 0.1em;
    }
    
    .badge {
      padding: 0.5em 0.75em;
      font-size: 0.75rem;
      font-weight: 700;
      border-radius: 0.35rem;
      text-transform: uppercase;
      letter-spacing: 0.1em;
    }
    
    .badge-primary {
      background-color: var(--primary-color);
    }
    
    .badge-warning {
      background-color: var(--warning-color);
      color: #212529;
    }
    
    .badge-success {
      background-color: var(--success-color);
    }
    
    .badge-danger {
      background-color: var(--danger-color);
    }
    
    .action-buttons .btn {
      margin-left: 5px;
      font-size: 0.8rem;
      padding: 0.375rem 0.75rem;
    }
    
    .card {
      border: none;
      border-radius: 0.35rem;
      box-shadow: 0 0.15rem 1.75rem 0 rgba(58, 59, 69, 0.1);
      margin-bottom: 2rem;
    }
    
    .card-header {
      background-color: #f8f9fc;
      border-bottom: 1px solid #e3e6f0;
      padding: 1rem 1.35rem;
      font-weight: 700;
      color: var(--dark-color);
    }
    
    .card-body {
      padding: 1.5rem;
    }
    
    .status-indicator {
      display: inline-block;
      width: 12px;
      height: 12px;
      border-radius: 50%;
      margin-right: 5px;
    }
    
    .status-open {
      background-color: var(--primary-color);
    }
    
    .status-progress {
      background-color: var(--warning-color);
    }
    
    .status-closed {
      background-color: var(--success-color);
    }
    
    .status-urgent {
      background-color: var(--danger-color);
    }
    
    .nav-tabs .nav-link {
      color: var(--secondary-color);
      font-weight: 600;
    }
    
    .nav-tabs .nav-link.active {
      color: var(--primary-color);
      border-bottom: 2px solid var(--primary-color);
    }
    
    .tab-content {
      padding: 20px 0;
    }
    
    @media (max-width: 768px) {
      .user-container {
        padding: 15px;
      }
    }
    
    .priority-high {
      color: var(--danger-color);
      font-weight: bold;
    }
    
    .priority-medium {
      color: var(--warning-color);
      font-weight: bold;
    }
    
    .priority-low {
      color: var(--success-color);
      font-weight: bold;
    }
  </style>
</head>
<body>
  <div class="user-container">
    <div class="page-header d-flex justify-content-between align-items-center">
      <div>
        <h2 class="text-primary"><i class="bi bi-person-circle"></i> Meus Chamados</h2>
        <p class="text-muted">Gerencie seus chamados e solicitações</p>
      </div>
      <button id="btnLogout" class="btn btn-outline-danger">
        <i class="bi bi-box-arrow-right"></i> Sair
      </button>
    </div>

    <ul class="nav nav-tabs" id="myTab" role="tablist">
      <li class="nav-item" role="presentation">
        <button class="nav-link active" id="abertos-tab" data-bs-toggle="tab" data-bs-target="#abertos" type="button" role="tab">
          <i class="bi bi-folder2-open"></i> Abertos
        </button>
      </li>
      <li class="nav-item" role="presentation">
        <button class="nav-link" id="historico-tab" data-bs-toggle="tab" data-bs-target="#historico" type="button" role="tab">
          <i class="bi bi-clock-history"></i> Histórico
        </button>
      </li>
      <li class="nav-item" role="presentation">
        <button class="nav-link" id="novo-tab" data-bs-toggle="tab" data-bs-target="#novo" type="button" role="tab">
          <i class="bi bi-plus-circle"></i> Novo Chamado
        </button>
      </li>
    </ul>

    <div class="tab-content" id="myTabContent">
      <!-- Aba de Chamados Abertos -->
      <div class="tab-pane fade show active" id="abertos" role="tabpanel">
        <div class="d-flex justify-content-between mb-3">
          <h5 class="section-title"><i class="bi bi-folder2-open"></i> Meus Chamados Abertos</h5>
          <button id="btnExportarAbertos" class="btn btn-sm btn-outline-secondary">
            <i class="bi bi-download"></i> Exportar CSV
          </button>
        </div>
        
        <div class="table-responsive">
          <table class="table table-hover">
            <thead>
              <tr>
                <th>ID</th>
                <th>Título</th>
                <th>Setor</th>
                <th>Data Abertura</th>
                <th>Prioridade</th>
                <th>Status</th>
                <th>Ações</th>
              </tr>
            </thead>
            <tbody id="tabelaAbertos">
              <!-- Preenchido por JavaScript -->
            </tbody>
          </table>
        </div>
      </div>

      <!-- Aba de Histórico -->
      <div class="tab-pane fade" id="historico" role="tabpanel">
        <div class="d-flex justify-content-between mb-3">
          <h5 class="section-title"><i class="bi bi-clock-history"></i> Histórico de Chamados</h5>
          <button id="btnExportarHistorico" class="btn btn-sm btn-outline-secondary">
            <i class="bi bi-download"></i> Exportar CSV
          </button>
        </div>
        
        <div class="table-responsive">
          <table class="table table-hover">
            <thead>
              <tr>
                <th>ID</th>
                <th>Título</th>
                <th>Setor</th>
                <th>Data Abertura</th>
                <th>Data Fechamento</th>
                <th>Status</th>
                <th>Ações</th>
              </tr>
            </thead>
            <tbody id="tabelaHistorico">
              <!-- Preenchido por JavaScript -->
            </tbody>
          </table>
        </div>
      </div>

      <!-- Aba de Novo Chamado -->
      <div class="tab-pane fade" id="novo" role="tabpanel">
        <h5 class="section-title"><i class="bi bi-plus-circle"></i> Abrir Novo Chamado</h5>
        
        <form id="formNovoChamado">
          <div class="row mb-3">
            <div class="col-md-6">
              <label for="titulo" class="form-label">Título do Chamado</label>
              <input type="text" class="form-control" id="titulo" required>
            </div>
            <div class="col-md-6">
              <label for="setor" class="form-label">Setor Responsável</label>
              <select class="form-select" id="setor" required>
                <option value="">Selecione um setor</option>
                <!-- Preenchido por JavaScript -->
              </select>
            </div>
          </div>
          
          <div class="row mb-3">
            <div class="col-md-6">
              <label for="prioridade" class="form-label">Prioridade</label>
              <select class="form-select" id="prioridade" required>
                <option value="">Selecione a prioridade</option>
                <option value="Baixa">Baixa</option>
                <option value="Média">Média</option>
                <option value="Alta">Alta</option>
                <option value="Urgente">Urgente</option>
              </select>
            </div>
            <div class="col-md-6">
              <label for="anexo" class="form-label">Anexo (Opcional)</label>
              <input type="file" class="form-control" id="anexo">
            </div>
          </div>
          
          <div class="mb-3">
            <label for="descricao" class="form-label">Descrição Detalhada</label>
            <textarea class="form-control" id="descricao" rows="5" required></textarea>
          </div>
          
          <div class="d-grid gap-2 d-md-flex justify-content-md-end">
            <button type="reset" class="btn btn-outline-secondary me-md-2">
              <i class="bi bi-x-circle"></i> Limpar
            </button>
            <button type="submit" class="btn btn-primary">
              <i class="bi bi-send"></i> Enviar Chamado
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>

  <!-- Modal para visualizar chamado -->
  <div class="modal fade" id="modalChamado" tabindex="-1" aria-hidden="true">
    <div class="modal-dialog modal-lg">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="modalChamadoTitulo">Detalhes do Chamado</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body" id="modalChamadoBody">
          <!-- Preenchido por JavaScript -->
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Fechar</button>
        </div>
      </div>
    </div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/simple-datatables@7.1.2/dist/umd/simple-datatables.min.js"></script>
  <script>
    // Verifica se o usuário está logado
    const usuarioLogado = JSON.parse(localStorage.getItem('usuarioLogado'));
    if (!usuarioLogado) {
      window.location.href = 'login.html';
    }

    // Dados iniciais
    const chamados = JSON.parse(localStorage.getItem('chamados')) || [];
    const setores = JSON.parse(localStorage.getItem('setores')) || ['Desenvolvimento', 'Manutenção', 'Redes', 'Suporte Técnico'];

    // Filtra chamados do usuário logado
    const meusChamados = chamados.filter(c => c.usuario === usuarioLogado.login);
    const chamadosAbertos = meusChamados.filter(c => c.status !== 'Finalizado');
    const chamadosFinalizados = meusChamados.filter(c => c.status === 'Finalizado');

    // Preenche a tabela de chamados abertos
    function preencherTabelaAbertos() {
      const tbody = document.getElementById('tabelaAbertos');
      tbody.innerHTML = '';

      chamadosAbertos.forEach(chamado => {
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${chamado.id}</td>
          <td>${chamado.titulo}</td>
          <td>${chamado.setor}</td>
          <td>${formatarData(chamado.data)}</td>
          <td class="priority-${chamado.prioridade.toLowerCase()}">${chamado.prioridade}</td>
          <td><span class="badge badge-${getStatusClass(chamado.status)}">${chamado.status}</span></td>
          <td>
            <button class="btn btn-sm btn-outline-primary btn-visualizar" data-id="${chamado.id}">
              <i class="bi bi-eye"></i> Visualizar
            </button>
          </td>
        `;
        tbody.appendChild(tr);
      });

      // Adiciona eventos aos botões de visualizar
      document.querySelectorAll('.btn-visualizar').forEach(btn => {
        btn.addEventListener('click', () => visualizarChamado(btn.dataset.id));
      });
    }

    // Preenche a tabela de histórico
    function preencherTabelaHistorico() {
      const tbody = document.getElementById('tabelaHistorico');
      tbody.innerHTML = '';

      chamadosFinalizados.forEach(chamado => {
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${chamado.id}</td>
          <td>${chamado.titulo}</td>
          <td>${chamado.setor}</td>
          <td>${formatarData(chamado.data)}</td>
          <td>${chamado.dataFim ? formatarData(chamado.dataFim) : '-'}</td>
          <td><span class="badge badge-${getStatusClass(chamado.status)}">${chamado.status}</span></td>
          <td>
            <button class="btn btn-sm btn-outline-primary btn-visualizar" data-id="${chamado.id}">
              <i class="bi bi-eye"></i> Visualizar
            </button>
          </td>
        `;
        tbody.appendChild(tr);
      });

      // Adiciona eventos aos botões de visualizar
      document.querySelectorAll('.btn-visualizar').forEach(btn => {
        btn.addEventListener('click', () => visualizarChamado(btn.dataset.id));
      });
    }

    // Preenche o select de setores
    function preencherSelectSetores() {
      const select = document.getElementById('setor');
      select.innerHTML = '<option value="">Selecione um setor</option>';
      
      setores.forEach(setor => {
        const option = document.createElement('option');
        option.value = setor;
        option.textContent = setor;
        select.appendChild(option);
      });
    }

    // Visualiza um chamado no modal
    function visualizarChamado(id) {
      const chamado = meusChamados.find(c => c.id == id);
      if (!chamado) return;

      const modalTitulo = document.getElementById('modalChamadoTitulo');
      const modalBody = document.getElementById('modalChamadoBody');

      modalTitulo.textContent = `Chamado #${chamado.id} - ${chamado.titulo}`;

      modalBody.innerHTML = `
        <div class="row mb-3">
          <div class="col-md-6">
            <h6>Setor Responsável</h6>
            <p>${chamado.setor}</p>
          </div>
          <div class="col-md-6">
            <h6>Prioridade</h6>
            <p class="priority-${chamado.prioridade.toLowerCase()}">${chamado.prioridade}</p>
          </div>
        </div>
        
        <div class="row mb-3">
          <div class="col-md-6">
            <h6>Data de Abertura</h6>
            <p>${formatarData(chamado.data)}</p>
          </div>
          <div class="col-md-6">
            <h6>Status</h6>
            <p><span class="badge badge-${getStatusClass(chamado.status)}">${chamado.status}</span></p>
          </div>
        </div>
        
        <div class="mb-3">
          <h6>Descrição</h6>
          <p>${chamado.descricao || 'Nenhuma descrição fornecida.'}</p>
        </div>
        
        ${chamado.dataFim ? `
        <div class="mb-3">
          <h6>Data de Fechamento</h6>
          <p>${formatarData(chamado.dataFim)}</p>
        </div>
        ` : ''}
        
        ${chamado.resposta ? `
        <div class="mb-3">
          <h6>Resposta do Suporte</h6>
          <div class="alert alert-light border">
            <p>${chamado.resposta}</p>
          </div>
        </div>
        ` : ''}
      `;

      const modal = new bootstrap.Modal(document.getElementById('modalChamado'));
      modal.show();
    }

    // Exporta chamados para CSV
    function exportarParaCSV(dados, nomeArquivo) {
      if (dados.length === 0) {
        alert('Não há dados para exportar.');
        return;
      }

      // Cabeçalho do CSV
      const header = Object.keys(dados[0]);

      // Mapeia cada chamado como uma linha do CSV
      const rows = dados.map(chamado => 
        header.map(field => {
          // Tratamento especial para campos de data
          if (field === 'data' || field === 'dataFim') {
            return chamado[field] ? formatarData(chamado[field]) : '';
          }
          return chamado[field] || '';
        })
      );

      // Monta o conteúdo CSV
      const csvContent = [header, ...rows]
        .map(e => e.map(v => `"${String(v).replace(/"/g, '""')}"`).join(','))
        .join('\n');

      // Cria arquivo e inicia download
      const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
      const url = URL.createObjectURL(blob);

      const link = document.createElement('a');
      link.href = url;
      link.setAttribute('download', `${nomeArquivo}_${new Date().toISOString().slice(0, 10)}.csv`);
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
      URL.revokeObjectURL(url);
    }

    // Formata data para o padrão brasileiro
    function formatarData(dataString) {
      if (!dataString) return '';
      const data = new Date(dataString);
      return data.toLocaleDateString('pt-BR');
    }

    // Retorna a classe CSS para o status
    function getStatusClass(status) {
      switch (status) {
        case 'Aberto': return 'primary';
        case 'Em Andamento': return 'warning';
        case 'Finalizado': return 'success';
        default: return 'secondary';
      }
    }

    // Evento para logout
    document.getElementById('btnLogout').addEventListener('click', () => {
      localStorage.removeItem('usuarioLogado');
      window.location.href = 'login.html';
    });

    // Evento para exportar chamados abertos
    document.getElementById('btnExportarAbertos').addEventListener('click', () => {
      exportarParaCSV(chamadosAbertos, 'meus_chamados_abertos');
    });

    // Evento para exportar histórico
    document.getElementById('btnExportarHistorico').addEventListener('click', () => {
      exportarParaCSV(chamadosFinalizados, 'meu_historico_chamados');
    });

    // Evento para enviar novo chamado
    document.getElementById('formNovoChamado').addEventListener('submit', e => {
      e.preventDefault();
      
      const titulo = document.getElementById('titulo').value.trim();
      const setor = document.getElementById('setor').value;
      const prioridade = document.getElementById('prioridade').value;
      const descricao = document.getElementById('descricao').value.trim();
      
      if (!titulo || !setor || !prioridade || !descricao) {
        alert('Por favor, preencha todos os campos obrigatórios.');
        return;
      }
      
      // Cria novo chamado
      const novoChamado = {
        id: Math.max(0, ...chamados.map(c => c.id)) + 1,
        titulo,
        setor,
        prioridade,
        descricao,
        status: 'Aberto',
        data: new Date().toISOString(),
        usuario: usuarioLogado.login
      };
      
      // Adiciona à lista de chamados
      chamados.push(novoChamado);
      localStorage.setItem('chamados', JSON.stringify(chamados));
      
      // Atualiza a interface
      alert('Chamado aberto com sucesso!');
      document.getElementById('formNovoChamado').reset();
      
      // Atualiza as tabelas
      meusChamados.push(novoChamado);
      chamadosAbertos.push(novoChamado);
      preencherTabelaAbertos();
      
      // Volta para a aba de abertos
      const tabAbertos = new bootstrap.Tab(document.getElementById('abertos-tab'));
      tabAbertos.show();
    });

    // Inicializa a página
    document.addEventListener('DOMContentLoaded', () => {
      preencherTabelaAbertos();
      preencherTabelaHistorico();
      preencherSelectSetores();
    });
  </script>
</body>
</html>
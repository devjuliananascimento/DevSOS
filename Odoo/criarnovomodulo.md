# Como Criar um Novo Model com Menu no Odoo

Este guia explica o processo completo para criar um novo model no módulo Instituto Fernanda Schneider.

---

## 📋 Checklist Rápido

- [ ] Criar arquivo Python do model
- [ ] Criar `__init__.py` na pasta do model
- [ ] Importar no `__init__.py` principal
- [ ] Criar arquivo XML com views
- [ ] Adicionar permissões de segurança
- [ ] Registrar XML no manifesto
- [ ] Criar entrada no menu
- [ ] Fazer upgrade do módulo

---

## 1️⃣ Criar o Model Python

### Localização
```
addons/saas_extra_addons/instituto_fernanda_schneider/models/nome_pasta/nome_arquivo.py
```

### Estrutura Básica
```python
from odoo import models, fields, api, _

class NomeModel(models.Model):
    _name = 'ifs.nome_model'
    _description = 'Descrição do Model'
    _inherit = ['mail.thread', 'mail.activity.mixin']
    
    # Campo obrigatório para arquivamento
    active = fields.Boolean(
        string='Ativo',
        default=True,
        help='Marque para manter o registro ativo'
    )
    
    name = fields.Char(
        string='Nome',
        required=True,
        tracking=True
    )
    
    code = fields.Char(
        string='Código',
        required=True,
        copy=False,
        readonly=True,
        default=lambda self: _('New')
    )
    
    # Adicione seus campos aqui
    description = fields.Text(string='Descrição')
    
    # Se usar sequência, adicionar método create
    @api.model
    def create(self, vals):
        if vals.get('code', _('New')) == _('New'):
            vals['code'] = self.env['ir.sequence'].next_by_code('ifs.nome.sequence') or _('New')
        return super(NomeModel, self).create(vals)
```

---

## 2️⃣ Criar `__init__.py` na Pasta do Model

### Localização
```
addons/saas_extra_addons/instituto_fernanda_schneider/models/nome_pasta/__init__.py
```

### Conteúdo
```python
from . import nome_arquivo
```

---

## 3️⃣ Importar no `__init__.py` Principal

### Localização
```
addons/saas_extra_addons/instituto_fernanda_schneider/models/__init__.py
```

### Adicionar
```python
from . import nome_pasta
```

**Exemplo de ordem:**
```python
from . import school
from . import curriculum
from . import people
from . import empresas
from . import nome_pasta  # <- Adicione aqui
from . import avaliation
```

---

## 4️⃣ Criar as Views XML

### Localização
```
addons/saas_extra_addons/instituto_fernanda_schneider/views/nome_pasta/nome_arquivo.xml
```

### Estrutura Completa
```xml
<?xml version="1.0" encoding="UTF-8"?>
<odoo>

    <!-- ========================= -->
    <!-- VIEW: FORMULÁRIO -->
    <!-- ========================= -->
    <record id="view_ifs_nome_form" model="ir.ui.view">
        <field name="name">ifs.nome.form</field>
        <field name="model">ifs.nome_model</field>
        <field name="arch" type="xml">
            <form string="Nome do Model">
                <sheet>
                    <group>
                        <field name="name"/>
                        <field name="code" readonly="1"/>
                        <field name="description"/>
                    </group>
                </sheet>
                
                <!-- Chatter (histórico) -->
                <div class="oe_chatter">
                    <field name="message_follower_ids"/>
                    <field name="activity_ids"/>
                    <field name="message_ids"/>
                </div>
            </form>
        </field>
    </record>

    <!-- ========================= -->
    <!-- VIEW: LISTA (TREE) -->
    <!-- ========================= -->
    <record id="view_ifs_nome_tree" model="ir.ui.view">
        <field name="name">ifs.nome.tree</field>
        <field name="model">ifs.nome_model</field>
        <field name="arch" type="xml">
            <tree string="Lista">
                <field name="name"/>
                <field name="code"/>
                <field name="active"/>
            </tree>
        </field>
    </record>

    <!-- ========================= -->
    <!-- VIEW: BUSCA -->
    <!-- ========================= -->
    <record id="view_ifs_nome_search" model="ir.ui.view">
        <field name="name">ifs.nome.search</field>
        <field name="model">ifs.nome_model</field>
        <field name="arch" type="xml">
            <search string="Buscar">
                <field name="name"/>
                <field name="code"/>
                <filter string="Ativos" name="active" domain="[('active', '=', True)]"/>
                <filter string="Arquivados" name="inactive" domain="[('active', '=', False)]"/>
            </search>
        </field>
    </record>

    <!-- ========================= -->
    <!-- ACTION WINDOW -->
    <!-- ========================= -->
    <record id="action_ifs_nome" model="ir.actions.act_window">
        <field name="name">Nome Plural</field>
        <field name="res_model">ifs.nome_model</field>
        <field name="view_mode">tree,form</field>
        <field name="search_view_id" ref="view_ifs_nome_search"/>
        <field name="help" type="html">
            <p class="o_view_nocontent_smiling_face">
                Criar novo registro
            </p>
            <p>
                Descrição do que este módulo gerencia.
            </p>
        </field>
    </record>

</odoo>
```

---

## 5️⃣ Adicionar Permissões de Segurança

### Localização
```
addons/saas_extra_addons/instituto_fernanda_schneider/security/ir.model.access.csv
```

### Adicionar no Final do Arquivo
```csv
access_ifs_nome_user,ifs.nome.user,model_ifs_nome_model,base.group_user,1,1,1,0
access_ifs_nome_manager,ifs.nome.manager,model_ifs_nome_model,group_ifs_manager,1,1,1,1
access_ifs_nome_admin,ifs.nome.admin,model_ifs_nome_model,group_ifs_admin,1,1,1,1
```

**Explicação das colunas:**
- `id`: Identificador único
- `name`: Nome descritivo
- `model_id:id`: Model no formato `model_ifs_nome_model`
- `group_id:id`: Grupo de usuários
- `perm_read`: Permissão de leitura (1=sim, 0=não)
- `perm_write`: Permissão de escrita
- `perm_create`: Permissão de criação
- `perm_unlink`: Permissão de exclusão

---

## 6️⃣ Registrar XML no Manifesto

### Localização
```
addons/saas_extra_addons/instituto_fernanda_schneider/__manifest__.py
```

### Adicionar na Lista `'data'`
```python
'data': [
    'security/ifs_security.xml',
    # ... outras entradas ...
    'views/people/job_views.xml',
    'views/nome_pasta/nome_arquivo.xml',  # <- Adicione aqui
    'views/curriculum/course_views.xml',
    # ... restante ...
    'views/reporting/menu_views.xml',
    'security/ir.model.access.csv',
],
```

**⚠️ Importante:** O XML deve vir **antes** de `menu_views.xml` mas **depois** de `security/ifs_security.xml`

---

## 7️⃣ Criar Menu

### Localização
```
addons/saas_extra_addons/instituto_fernanda_schneider/views/reporting/menu_views.xml
```

### Adicionar no Local Apropriado
```xml
<record id="menu_register_nome" model="ir.ui.menu">
    <field name="name">Nome Plural</field>
    <field name="parent_id" ref="menu_register_root"/>
    <field name="sequence">80</field>
    <field name="action" ref="action_ifs_nome"/>
</record>
```

**Dica:** Ajuste o `sequence` para definir a ordem no menu (números menores aparecem primeiro)

---

## 8️⃣ (Opcional) Criar Sequência

Se o model usa código sequencial, criar arquivo de dados:

### Localização
```
addons/saas_extra_addons/instituto_fernanda_schneider/data/nome_sequence.xml
```

### Conteúdo
```xml
<?xml version="1.0" encoding="utf-8"?>
<odoo>
    <data noupdate="1">
        <record id="seq_ifs_nome" model="ir.sequence">
            <field name="name">Sequência Nome</field>
            <field name="code">ifs.nome.sequence</field>
            <field name="prefix">NOM</field>
            <field name="padding">4</field>
            <field name="number_next">1</field>
            <field name="number_increment">1</field>
        </record>
    </data>
</odoo>
```

Adicionar no manifesto antes das views:
```python
'data/nome_sequence.xml',
```

---

## 9️⃣ Fazer Upgrade do Módulo

### No Odoo Web
1. Ir em **Apps** (Aplicativos)
2. Remover filtro "Apps"
3. Buscar por "Instituto Fernanda Schneider"
4. Clicar no botão **Upgrade**
5. Aguardar conclusão

### Via Linha de Comando (alternativa)
```bash
sudo docker-compose restart
```

---

## 📝 Convenções de Nomenclatura

| Elemento | Padrão | Exemplo |
|----------|--------|---------|
| Model | `ifs.nome_minusculo` | `ifs.company` |
| Classe Python | `PascalCase` | `Company` |
| View Form | `view_ifs_nome_form` | `view_ifs_company_form` |
| View Tree | `view_ifs_nome_tree` | `view_ifs_company_tree` |
| View Search | `view_ifs_nome_search` | `view_ifs_company_search` |
| Action | `action_ifs_nome` | `action_ifs_company` |
| Menu | `menu_register_nome` | `menu_register_company` |
| Access CSV | `access_ifs_nome_grupo` | `access_ifs_company_user` |

---

## 🔍 Checklist de Verificação

Antes de fazer upgrade, verifique:

- [ ] Arquivo Python criado com todos os campos
- [ ] `__init__.py` criado na pasta do model
- [ ] Import adicionado no `__init__.py` principal
- [ ] XML criado com form, tree, search e action
- [ ] Permissões adicionadas no CSV (3 linhas mínimo)
- [ ] XML registrado no manifesto
- [ ] Menu criado no `menu_views.xml`
- [ ] Sequência criada (se necessário)
- [ ] Nenhum erro de sintaxe Python ou XML

---

## 🚨 Erros Comuns

### "External ID not found"
- **Causa:** Action não definida ou não carregada antes do menu
- **Solução:** Verificar se o XML da view está no manifesto antes de `menu_views.xml`

### "Model not found"
- **Causa:** Model não importado no `__init__.py`
- **Solução:** Adicionar import no `models/__init__.py`

### "Access Denied"
- **Causa:** Falta permissão no `ir.model.access.csv`
- **Solução:** Adicionar linhas de permissão para os grupos

### "Field does not exist"
- **Causa:** Campo no XML não existe no model Python
- **Solução:** Adicionar campo no arquivo `.py` ou remover do XML

---

## 📚 Exemplos Completos no Projeto

- **Model Simples:** `/models/people/job.py` + `/views/people/job_views.xml`
- **Model Completo:** `/models/empresas/empresas.py` + `/views/empresas/empresas.xml`
- **Com Sequência:** `/models/people/professor.py` + `/data/professor_sequence.xml`

---

## 🆘 Suporte

Em caso de dúvidas, consulte:
- Documentação oficial: https://www.odoo.com/documentation/17.0/
- Models existentes no projeto como referência
- Logs do Odoo em caso de erro

# Introdução
🚀 Este projeto em SQL tem como objetivo explorar o mercado de trabalho para analistas de dados, identificando insights valiosos sobre as melhores oportunidades na área. Através de consultas otimizadas, é possível descobrir quais empregos oferecem os maiores salários 💰, quais habilidades são mais requisitadas 🛠️, quais competências estão associadas aos melhores salários 📈, e quais são as habilidades mais demandadas entre os cargos de alta remuneração ⭐. 

📊SQL queries? Aqui estão: [project_sql folder](/project_sql/)
# Background
📚 Este projeto surgiu a partir da minha decisão de mergulhar no universo da análise de dados, uma área em constante crescimento e cheia de oportunidades. Sabendo que o SQL é uma das ferramentas mais essenciais para quem deseja atuar nesse campo, concentrei meus estudos iniciais nessa linguagem poderosa. Para isso, tive como principal referência o canal do Luke Barousse no YouTube, que recomendo fortemente — com suas explicações claras e projetos práticos, fui capaz de aprender praticamente tudo que sei sobre SQL até aqui. Este projeto é, portanto, um reflexo do que venho aprendendo e aplicando nessa jornada. 🚀
# Ferramentas Utilizadas
🛠️ Para o desenvolvimento deste projeto, utilizei um conjunto de ferramentas essenciais no ecossistema de dados e desenvolvimento.

- **SQL**: Linguagem principal usada para realizar todas as análises e extrações de dados.

- **PostgreSQL**: Serviu como sistema de gerenciamento de banco de dados, oferecendo robustez e compatibilidade com diversas queries analíticas.

- **Visual Studio Code (VSCode)**: Nele escrevi e organizei meu código com agilidade e praticidade, aproveitando extensões úteis para SQL.

- **Git & GitHub**: Ferramentas para controle de versão e compartilhamento do projeto, garantindo que todo o progresso estivesse documentado e acessível de forma colaborativa.
# Análise
Cada query deste projeto focou em investigar aspectos específicos do mercado de trabalho para analista de dados.

### 1.Empregos de analista de dados mais bem pagos
Com o objetivo de encontrar os cargos mais bem pagos, selecionei vagas de analista de dados com base na média salarial anual e na localização, dando prioridade às oportunidades remotas. Essa análise evidencia as melhores remunerações disponíveis no setor.

```sql
SELECT job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE job_title_short = 'Data Analyst'
    AND job_location = 'Anywhere'
    AND salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```
- **Grande Potencial Salarial**: As 10 vagas de analista de dados com os maiores salários variam de $184.000 a $650.000 por ano, revelando as oportunidades financeiramente atrativas da área.

- **Diversidade de Empresas**: Cargos com altos salários são oferecidos por empresas como SmartAsset, Meta e AT&T, evidenciando uma forte demanda em diversos setores.

- **Variedade de Cargos**: A ampla gama de títulos — de Analista de Dados a Diretor de Análises — mostra as diversas especializações e caminhos de carreira dentro da análise de dados.

### 2. Habilidades mais requisitadas pelos empregos mais bem remunerados
Para descobrir quais habilidades são mais valorizadas nos cargos com maiores salários, relacionei os dados de vagas com as habilidades exigidas, revelando as qualificações essenciais que os empregadores buscam para oferecer altos níveis de remuneração.

```sql
WITH top_paying_jobs AS (
    SELECT job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM job_postings_fact
        LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE job_title_short = 'Data Analyst'
        AND job_location = 'Anywhere'
        AND salary_year_avg IS NOT NULL
    ORDER BY salary_year_avg DESC
    LIMIT 10
)
SELECT top_paying_jobs.*,
    skills
FROM top_paying_jobs
    INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC
```

SQL se destaca como a habilidade mais requisitada, aparecendo 8 vezes. Python vem logo em seguida com 7 menções, enquanto Tableau também demonstra alta valorização, com 6 aparições. Outras habilidades como R, Snowflake, Pandas e Excel também apresentam níveis variados de demanda.

### 3. Habilidades mais frequentemente requisitadas para analistas de dados

Essa query foi útil para identificar as habilidades mais frequentemente solicitadas nas vagas de emprego, destacando as áreas com maior demanda.

```sql
SELECT skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
    AND job_work_from_home = True
GROUP BY skills
ORDER BY demand_count DESC
LIMIT 5
```

**SQL** e **Excel** continuam sendo essenciais, ressaltando a importância de habilidades fundamentais sólidas em processamento de dados e gerenciamento de planilhas.

Ferramentas de programação e visualização como **Python**, **Tableau** e **Power BI** são cruciais, refletindo a crescente importância da expertise técnica na narrativa de dados e no suporte à tomada de decisões.

### 4. Habilidades baseadas no salário

A análise dos salários médios associados a diferentes habilidades revelou quais são as que oferecem os maiores ganhos.

```sql
SELECT skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True
GROUP BY skills
ORDER BY avg_salary DESC
LIMIT 25
```

- **Alta demanda por habilidades em Big Data e Machine Learning**: Os maiores salários são conquistados por analistas que dominam tecnologias de big data (PySpark, Couchbase), plataformas de machine learning (DataRobot, Jupyter) e bibliotecas Python (Pandas, NumPy), evidenciando a forte valorização do setor em capacidades de processamento de dados e modelagem preditiva.

- **Proficiência em desenvolvimento e implantação de software**: O conhecimento em ferramentas de desenvolvimento e implantação (GitLab, Kubernetes, Airflow) revela uma valiosa interseção entre análise de dados e engenharia, com valorização das habilidades que possibilitam automação e gerenciamento eficiente de pipelines de dados.

- **Especialização em computação em nuvem**: A familiaridade com plataformas de nuvem e engenharia de dados (Elasticsearch, Databricks, GCP) destaca a crescente importância dos ambientes analíticos baseados na nuvem, indicando que a proficiência em nuvem aumenta significativamente o potencial de ganhos em análise de dados.

### 5. Habilidades mais ideais para aprender

Combinando informações sobre demanda e salários, esta query teve como objetivo identificar habilidades que são tanto muito requisitadas quanto bem remuneradas, oferecendo um foco estratégico para o desenvolvimento de competências.

```sql
WITH skills_demand AS (
    SELECT skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_count
    FROM job_postings_fact
        INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
        INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL
        AND job_work_from_home = True
    GROUP BY skills_dim.skill_id
),
average_salary AS (
    SELECT skills_job_dim.skill_id,
        ROUND(AVG(salary_year_avg), 0) AS avg_salary
    FROM job_postings_fact
        INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
        INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE job_title_short = 'Data Analyst'
        AND salary_year_avg IS NOT NULL
        AND job_work_from_home = True
    GROUP BY skills_job_dim.skill_id
)
SELECT skills_demand.skill_id,
    skills_demand.skills,
    demand_count,
    avg_salary
FROM skills_demand
    INNER JOIN average_salary ON skills_demand.skill_id = average_salary.skill_id
ORDER BY demand_count DESC,
    avg_salary DESC
LIMIT 25
```

- **Linguagens de programação com alta demanda**: Python e R se destacam pela forte procura, com contagens de demanda de 236 e 148, respectivamente. Apesar de serem muito requisitadas, seus salários médios são cerca de $101.397 para Python e $100.499 para R, indicando que, embora a proficiência seja altamente valorizada, essas habilidades também são amplamente disponíveis.

- **Ferramentas e tecnologias em nuvem**: O domínio de plataformas especializadas como Snowflake, Azure, AWS e BigQuery apresenta demanda significativa acompanhada de salários médios relativamente altos, evidenciando a crescente importância das plataformas em nuvem e das tecnologias de big data na análise de dados.

- **Ferramentas de Business Intelligence e visualização**: Tableau e Looker, com demandas de 230 e 49, respectivamente, e salários médios em torno de $99.288 e $103.795, destacam o papel fundamental da visualização de dados e da inteligência de negócios na extração de insights acionáveis a partir dos dados.

- **Tecnologias de banco de dados**: A demanda por habilidades em bancos de dados tradicionais e NoSQL (Oracle, SQL Server, NoSQL), com salários médios variando entre $97.786 e $104.534, reflete a necessidade contínua de expertise em armazenamento, recuperação e gestão de dados.
# O que aprendi

Após esse projeto posso afirmar que dei não só os passos inicias mas também um grande upgrade naquilo que fui aprendendo no caminho.

- 🧩 **Criação Avançada de Queries**: Aperfeiçoei o domínio de SQL complexo, juntando tabelas com habilidade e utilizando cláusulas WITH para criar tabelas temporárias poderosas.

- 📊 **Agregação de Dados**: Tornei-me proficiente no uso do GROUP BY e em funções agregadas como COUNT() e AVG() para resumir dados de forma eficiente.

- 💡 **Habilidades Analíticas**: Elevei minha capacidade de resolver problemas práticos, transformando perguntas em queries SQL claras e perspicazes.
# Conclusões

### Alguns insights finais:
**Posições de Analista de Dados com Altos Salários**: As vagas de analista de dados remotas com maior remuneração apresentam uma ampla faixa salarial, com o topo chegando a $650.000!

**Habilidades Chave para Cargos Bem Remunerados**: A proficiência avançada em SQL é fundamental para conquistar posições de analista de dados bem pagas, ressaltando sua importância.

**Habilidades Mais Requisitadas**: SQL é a habilidade mais demandada no mercado de trabalho para analistas de dados, tornando-se essencial para os candidatos.

**Habilidades Associadas a Salários Maiores**: Competências especializadas como SVN e Solidity estão ligadas aos maiores salários médios, destacando o valor da expertise em nichos específicos.

**Melhores Habilidades para o Sucesso no Mercado**: SQL se destaca tanto na demanda quanto nas ofertas salariais, sendo uma das melhores habilidades que analistas de dados podem desenvolver para maximizar seu valor no mercado.

### Considerações Finais

Este projeto aprimorou minhas habilidades em SQL e forneceu insights valiosos sobre o mercado de trabalho para analistas de dados. Os resultados da análise servem como um guia para priorizar o desenvolvimento de competências e a busca por emprego. Analistas de dados em formação podem se posicionar melhor em um mercado competitivo ao focar em habilidades de alta demanda e alta remuneração. Esta investigação destaca a importância do aprendizado contínuo e da adaptação às tendências emergentes na área de análise de dados.
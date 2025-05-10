# SemRepo

[**SemRepo**](https://semrepo.org/) is a fine-grained RDF knowledge graph with over 81 million triples on nearly 200,000 GitHub repositories linked to scientific research. It captures metadata like contributors, issues, dependencies, and languages, and connects authors to [SemOpenAlex](https://semopenalex.org/) profiles. Released under CC0, the data are available as RDF dumps and via a public SPARQL endpoint. SemRepo supports use cases such as research-to-code tracing, repository discovery, collaboration analysis, impact tracking, and expertise identification—offering a unified, queryable resource for the Semantic Web.


**SemRepo** is available at [https://semrepo.org](https://semrepo.org/)


![Knowledge Graph Schema](https://raw.githubusercontent.com/abdulrafay97/SemRepo/main/Suplementry-Material/kg-schema.png)


Schema of SemRepo

## Knowledge Graph Construction 

### Crawl MetaData
To Crawl the metadata from the GitHub repositories, we used [python scripts](./Crawling_GitHub_Metadata). An overview of the crawled data is available as [JSON](./Suplementry-Material/aashqar_dsclrcn-pytorch_repo.json).

### Extract Pacakges
For Extracting the packages being used in the code files, we used the following [scripts](./Extract_Libraries_From_Code). It basically downloads the repository, then read the code files to get the package name. An overview of the data is available as [JSON](./Suplementry-Material/package_example.json)

### Constructing the Metadata KG (ie .nt file)
To construct the knowledge graph of the metadata and linking the knowledge graph with [LPWC](https://linkedpaperswithcode.com) and [SemOpenAlex](https://semopenalex.org/), we employed these [scripts.](./Making_Repo_Metadata_KG)

### Example SPARQL Queries
<pre> ```sparql SELECT ?topic ?progLang (COUNT(?progLang) AS ?langCount) WHERE { GRAPH <https://semrepo.org> { ?repository <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <https://semrepo.org/class/repository>. ?repository <http://xmlns.com/foaf/0.1/topic> ?topic. ?repository <https://semrepo.org/property/hasLanguageReference> ?langref. ?langref <https://semrepo.org/property/hasLanguageName> ?progLang. } } GROUP BY ?topic ?progLang ORDER BY DESC(?langCount) LIMIT 100 ``` </pre>

# SemRepo

[**SemRepo**](https://semrepo.org/) is a fine-grained RDF knowledge graph with over 81 million triples on nearly 200,000 GitHub repositories linked to scientific research. It captures metadata like contributors, issues, dependencies, and languages, and connects authors to [SemOpenAlex](https://semopenalex.org/) profiles. Released under CC0, the data are available as RDF dumps and via a public SPARQL endpoint. SemRepo supports use cases such as research-to-code tracing, repository discovery, collaboration analysis, impact tracking, and expertise identification—offering a unified, queryable resource for the Semantic Web.


**SemRepo** is available at [https://semrepo.org](https://semrepo.org/)


![Knowledge Graph Schema](https://raw.githubusercontent.com/abdulrafay97/SemRepo/main/Suplementry-Material/kg-schema.png)


Schema of SemRepo

## Knowledge Graph Construction 

### Crawl MetaData
To Crawl the metadata from the GitHub repositories, we used [python scripts](./Crawling_GitHub_Metadata)

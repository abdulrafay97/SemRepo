# SemRepo.org

[**SemRepo**](https://semrepo.org/) is a fine-grained RDF knowledge graph with over 81 million triples on nearly 200,000 GitHub repositories linked to scientific research. It captures metadata like contributors, issues, dependencies, and languages, and connects authors to [SemOpenAlex](https://semopenalex.org/) profiles and with corresponding repositories present in [LinkedPapersWithCode(LPWC)](https://linkedpaperswithcode.com/). Released under CC0, the data are available as RDF dumps and via a public SPARQL endpoint. SemRepo supports use cases such as research-to-code tracing, repository discovery, collaboration analysis, impact tracking, and expertise identification—offering a unified, queryable resource for the Semantic Web.


**SemRepo** is available at [https://semrepo.org](https://semrepo.org/)


![Knowledge Graph Schema](https://raw.githubusercontent.com/abdulrafay97/SemRepo/main/assets/kg-schema.png)


Schema of SemRepo

## Knowledge Graph Construction 

### Crawl MetaData
To Crawl the metadata from the GitHub repositories, we used [python scripts](./crawling-gitHub-metadata). An overview of the crawled data is available as [JSON](./assets/khuangaf_awesome-chart-understanding.json).

### Extract Pacakges
For Extracting the packages being used in the code files, we used the following [scripts](./extract-libraries-from-code). It basically downloads the repository, then read the code files to get the package name.

### Constructing the Metadata KG (ie .nt file)
To construct the knowledge graph of the metadata and linking the knowledge graph with [LPWC](https://linkedpaperswithcode.com) and [SemOpenAlex](https://semopenalex.org/), we employed these [scripts.](./making-repo-metadata-kg)

### Example SPARQL Queries

Programming Languages Used for each topic
```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
PREFIX sr: <https://semrepo.org/property/>
PREFIX srclass: <https://semrepo.org/class/>

SELECT ?topic ?progLang (COUNT(?progLang) AS ?langCount)
WHERE {
  GRAPH <https://semrepo.org> {
    ?repository rdf:type srclass:repository .
    ?repository foaf:topic ?topic .
    ?repository sr:hasLanguageReference ?langref .
    ?langref sr:hasLanguageName ?progLang .
  }
}
GROUP BY ?topic ?progLang
ORDER BY DESC(?langCount)
LIMIT 100
```

Top 5 contributors with most commits with SemOpenAlex profile
```sparql
PREFIX sr: <https://semrepo.org/property/>

SELECT ?contributor ?soa_url (SUM(?no_of_commits) AS ?total_commits)
WHERE {
  GRAPH <https://semrepo.org> {
    ?contRef sr:hasContributor ?contributor .
    ?contRef sr:hasCommits ?no_of_commits .
    ?contributor sr:hasSoaUrl ?soa_url .
  }
}
GROUP BY ?contributor ?soa_url
ORDER BY DESC(?total_commits)
LIMIT 5
```

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX dct: <http://purl.org/dc/terms/>
PREFIX fabio: <http://purl.org/spar/fabio/>
PREFIX sr: <https://semrepo.org/property/>
PREFIX srclass: <https://semrepo.org/class/>
PREFIX org: <http://www.w3.org/ns/org#>
PREFIX lpwc: <https://linkedpaperswithcode.com/property/>

SELECT *
WHERE {
  GRAPH <https://semrepo.org> {
    ?SemRepo_repository rdf:type srclass:repository .
    ?SemRepo_repository fabio:hasURL ?Github_Url .
    ?SemRepo_repository sr:hasTotalStargazers ?Stars .
    ?SemRepo_repository sr:hasTotalWatchers ?Watchers .
    ?SemRepo_repository sr:hasTotalForks ?Forks .
    ?SemRepo_repository sr:hasTotalContributor ?Contributor .
    ?SemRepo_repository sr:hasTotalIssues ?TotalIssues .
    ?SemRepo_repository sr:hasLpwcUrl ?Lpwc_repository .
    ?SemRepo_repository dct:creator ?SemRepo_person .
    ?SemRepo_person sr:hasSoaUrl ?Soa_Author .
  }

  SERVICE <https://semopenalex.org/sparql> {
    ?Soa_Author org:memberOf ?Institution .
  } 

  SERVICE <https://linkedpaperswithcode.com/sparql> {
    ?Lpwc_paper lpwc:hasOfficialRepository ?Lpwc_repository .
    ?Lpwc_paper lpwc:hasTask ?Lpwc_Task .
    ?Lpwc_paper lpwc:hasMethod ?Lpwc_Method .
    ?Lpwc_paper lpwc:hasEvaluation ?Lpwc_Evaluation .
    ?Lpwc_paper lpwc:hasConference ?Lpwc_Conference .
  } 
}
LIMIT 100


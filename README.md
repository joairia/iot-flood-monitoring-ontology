# Reproducibility Material for the Semantic IoT Ontology Paper

This repository provides the reproducibility material associated with the paper:

**“Toward a Lightweight Ontology for Heterogeneous Data in Multi-IoT Platforms”**

The purpose of this repository is to make the semantic-web resources used in the paper publicly available. It includes the OWL ontology file and the SPARQL queries used for validation, so that other researchers can inspect, reuse, and reproduce the semantic modeling and querying process.

---

## Repository Contents

```text
.
├── ontology/
│   └── data_format_heterogeneity_proposed_onto.owl
│
├── queries/
│   ├── Q1_all_temperature_observations.rq
│   ├── Q2_temperature_above_threshold.rq
│   ├── Q3_observations_by_sensor_type.rq
│   └── Q4_observations_by_phenomenon_and_time.rq
│
│
└── README.md
```

---

## Ontology File

The ontology used in this work is provided in OWL format:

```text
ontology/data_format_heterogeneity_proposed_onto.owl
```

The ontology models heterogeneous IoT data using semantic-web technologies. It represents the main IoT entities involved in multi-domain environments, including:

* `Data`
* `Phenomena`
* `Device`
* `Sensor`
* `Actuator`
* `Thing`
* `User`
* `Location`
* `ApplicationDomain`
* `SubDomain`
* `CommunicationProtocol`

The ontology also defines object properties and data properties to describe the relationships between IoT devices, sensors, observed phenomena, produced data, locations, domains, and communication protocols.

---

## Ontology Namespace

The ontology namespace used in the SPARQL queries is:

```sparql
PREFIX onto: <http://www.semanticweb.org/hp/ontologies/2025/10/data_format_heterogeneity_proposed_onto#>
```

Depending on the RDF/OWL tool used, the ontology may also require the following standard prefixes:

```sparql
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl:  <http://www.w3.org/2002/07/owl#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>
```

---

## SPARQL Queries

The four SPARQL queries used in the paper are provided in the `queries/` folder.

### Query 1: Retrieve all temperature observations

File:

```text
queries/Q1_all_temperature_observations.rq
```

Purpose:

This query retrieves all observations related to temperature phenomena, including the observed value, unit, label, and timestamp.

```sparql
PREFIX onto: <http://www.semanticweb.org/hp/ontologies/2025/10/data_format_heterogeneity_proposed_onto#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT 
    ?data 
    ?phenomenon 
    ?value 
    ?unit 
    ?label 
    ?timestamp
WHERE {
    ?data rdf:type onto:Data .
    ?data onto:describes ?phenomenon .

    ?phenomenon onto:Label ?label .
    ?phenomenon onto:Value ?value .
    ?phenomenon onto:Unity ?unit .
    ?phenomenon onto:Timestamp ?timestamp .

    FILTER(CONTAINS(LCASE(STR(?label)), "temperature"))
}
ORDER BY ?timestamp
```

---

### Query 2: Retrieve temperature observations above a threshold

File:

```text
queries/Q2_temperature_above_threshold.rq
```

Purpose:

This query retrieves temperature observations whose value is higher than a defined threshold.

```sparql
PREFIX onto: <http://www.semanticweb.org/hp/ontologies/2025/10/data_format_heterogeneity_proposed_onto#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

SELECT 
    ?data 
    ?phenomenon 
    ?value 
    ?unit 
    ?label 
    ?timestamp
WHERE {
    ?data rdf:type onto:Data .
    ?data onto:describes ?phenomenon .

    ?phenomenon onto:Label ?label .
    ?phenomenon onto:Value ?value .
    ?phenomenon onto:Unity ?unit .
    ?phenomenon onto:Timestamp ?timestamp .

    FILTER(CONTAINS(LCASE(STR(?label)), "temperature"))
    FILTER(xsd:decimal(?value) > 23)
}
ORDER BY DESC(?value)
```

---

### Query 3: Retrieve observations by sensor type

File:

```text
queries/Q3_observations_by_sensor_type.rq
```

Purpose:

This query retrieves observations according to the type of sensor that produced the data.

```sparql
PREFIX onto: <http://www.semanticweb.org/hp/ontologies/2025/10/data_format_heterogeneity_proposed_onto#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT 
    ?sensor 
    ?sensorType
    ?data 
    ?phenomenon 
    ?value 
    ?unit 
    ?timestamp
WHERE {
    ?sensor rdf:type onto:Sensor .
    ?sensor onto:Type ?sensorType .

    ?data rdf:type onto:Data .
    ?data onto:producedBy ?sensor .
    ?data onto:describes ?phenomenon .

    ?phenomenon onto:Value ?value .
    ?phenomenon onto:Unity ?unit .
    ?phenomenon onto:Timestamp ?timestamp .

    FILTER(CONTAINS(LCASE(STR(?sensorType)), "temperature"))
}
ORDER BY ?timestamp
```

---

### Query 4: Retrieve observations by phenomenon and timestamp

File:

```text
queries/Q4_observations_by_phenomenon_and_time.rq
```

Purpose:

This query retrieves observations related to a specific phenomenon and orders them according to their timestamp.

```sparql
PREFIX onto: <http://www.semanticweb.org/hp/ontologies/2025/10/data_format_heterogeneity_proposed_onto#>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>

SELECT 
    ?data 
    ?phenomenon 
    ?label 
    ?value 
    ?unit 
    ?timestamp
WHERE {
    ?data rdf:type onto:Data .
    ?data onto:describes ?phenomenon .

    ?phenomenon onto:Label ?label .
    ?phenomenon onto:Value ?value .
    ?phenomenon onto:Unity ?unit .
    ?phenomenon onto:Timestamp ?timestamp .

    FILTER(CONTAINS(LCASE(STR(?label)), "humidity"))
}
ORDER BY ?timestamp
```

---

## How to Reproduce the SPARQL Validation

To reproduce the semantic validation:

1. Download or clone this repository.

```bash
git clone https://github.com/USERNAME/REPOSITORY-NAME.git
```

2. Open the ontology file using an ontology editor such as **Protégé**.

```text
ontology/data_format_heterogeneity_proposed_onto.owl
```

3. Alternatively, load the OWL file into an RDF triple store such as **Apache Jena Fuseki**.

4. Open the SPARQL endpoint of the triple store.

5. Execute the queries provided in the `queries/` folder.

6. Compare the obtained results with the validation results reported in the paper.

---

## Tools Used

The ontology and queries were developed and tested using the following tools:

* Protégé
* Apache Jena Fuseki
* SPARQL
* OWL
* RDF
* Python
* Owlready2
* Node-RED
* MQTT

---

## Reproducibility Notes

The provided ontology and queries allow researchers to:

* Inspect the semantic structure of the proposed ontology.
* Verify the modeled IoT entities and relationships.
* Execute the SPARQL queries used in the validation stage.
* Reproduce the semantic retrieval process described in the paper.
* Reuse or extend the ontology for other IoT application domains.

---

## Citation

If you use this ontology or the provided SPARQL queries, please cite the associated paper:

```text
Lafhal, J., Elkhamlichi, Y. Toward a Lightweight Ontology for Heterogeneous Data in Multi-IoT Platforms.
```

---

## Contact

For questions regarding this repository or the associated research work, please contact:

Joairia Lafhal
Email: joairia.lafhal@etu.uae.ac.ma


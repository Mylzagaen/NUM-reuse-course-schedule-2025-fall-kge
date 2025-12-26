# 5. Entity Definition

This section describes the **Entity Definition** phase of the *iTelos methodology*. Building on the outcomes of the *Data Preparation* and *Language Definition* phases, this phase focuses on translating the abstract conceptual model into concrete, instantiable entities. The main objective is to ensure that all real-world instances from the prepared datasets are consistently identified, correctly matched, and accurately mapped to the knowledge teleontology before constructing the Knowledge Graph.

---

## 5.1 Entity Identification

Entity identification was performed by analyzing the aligned datasets and determining which records correspond to concrete instances of the conceptual entities defined in the *Language Definition* phase. Core entities such as **Course**, **Course_schedule**, **Time_slot**, **Curriculum**, **Program**, and **Student** were directly instantiated from their respective datasets, as they represent fundamental academic objects within the domain.

In addition, contextual and activity-based entities, including **Curriculum_course**, **Planned_course**, **Selected_course**, and **Prerequisite**, were identified and modeled as separate entities. These entities capture curriculum structure and student behavior rather than standalone academic objects, enabling reasoning tasks such as schedule conflict detection and curriculum validation.

---

## 5.2 Entity Matching

Entity matching ensured that the same real-world object was represented by a single entity across all datasets. Stable identifiers such as `course_id`, `curriculum_id`, and `student_id` were used to generate consistent IRIs for core entities.

For composite or context-dependent entities, such as time slots, a **hash-based identifier strategy** was adopted to guarantee uniqueness. In particular, each **Time_slot** entity was generated using a hash of its schedule identifier, day of week, start time, and end time. This approach prevents duplicate time slot instances across schedules.

This matching strategy ensures consistency while remaining robust against heterogeneous identifier schemes in the source data. Its main strength lies in reproducibility and collision avoidance, while its limitation is that identifier semantics are derived from data values rather than explicit global identifiers.

---

## 5.3 Data Mapping and Model Refinements

During data mapping, dataset fields were systematically linked to the corresponding classes and properties defined in the ontology. The entity definitions were applied by distinguishing between:

- **Core entities**: Course, Course_schedule, Time_slot, Curriculum, Program, Student  
- **Contextual entities**: Curriculum_course, Planned_course, Selected_course, Prerequisite  

This distinction ensures a clear separation between standalone academic objects and relationship- or state-dependent entities.

### Data Property Mapping

- **Course** entities were associated with identifiers and descriptive attributes such as `course_id`, `course_index`, and `course_name`.
- **Course_schedule** captured scheduling-related attributes including `schedule_start`, `schedule_end`, `course_type`, `course_frequency`, and `cycle`.
- **Time_slot** entities represented concrete temporal information (`day_of_week`, `start_time`, `end_time`).
- Location-related properties (`building_name`, `room_name`, `room_number`) were associated with scheduled course instances.
- **Program** and **Curriculum** entities were mapped using attributes such as `program_id`, `program_name`, `academic_level`, `first_year`, and `curriculum_id`.
- **Student** entities were assigned student-specific attributes including `student_id` and `enrollment_year`.
- Curriculum groupings were represented through **Curriculum_course** using `group_name` and `required_course_id`.

### Model Refinements

Several refinements were applied to improve semantic clarity and support the project’s competency questions:

- The `degree_program` attribute was removed from **Course**, as it was redundant with program and curriculum relationships already modeled through object properties.
- The `room_number` attribute in **Course_schedule** was converted from a numeric type to a string to accommodate alphanumeric identifiers (e.g., “1000A”).
- An `academic_level` attribute was added to **Program** to explicitly distinguish between Bachelor and Master programs.

### Object Property Instantiation

Object properties were instantiated to represent structural, temporal, prerequisite, and behavioral relationships:

- `has_schedule` links courses to their schedules.
- `includes` and `has_course` capture curriculum–course relationships.
- `has_curriculum` and `part_of` represent institutional and structural hierarchies.
- Course dependencies were modeled using `has_prerequisite` and its inverse `is_prerequisite_of`.
- Student behavior was captured through `plans` and `selects`, linking students to planned and selected courses.

Together, these mappings ensure a consistent and semantically grounded Knowledge Graph aligned with the ontology and the prepared datasets.

---

## 5.4 Entity Instantiation Using Python

After completing entity identification, matching, and mapping, the Knowledge Graph was instantiated using **Python**. The implementation relied on the `rdflib` library to load the existing OWL teleontology, define namespaces, and generate RDF triples programmatically.

Each dataset was processed independently, and entities were created using stable IRIs derived from dataset identifiers. Object and data properties were assigned according to the mappings defined in this phase, with explicit XML Schema datatypes applied to all literal values.

This Python-based construction approach ensures repeatability, scalability, and consistency between the datasets and the ontology. The final Knowledge Graph was serialized in both **OWL/XML** and **Turtle** formats and prepared for querying and validation using **SPARQL**.

---

## 5.5 Summary

The Entity Definition phase transformed the abstract conceptual model into concrete, interoperable entities ready for Knowledge Graph construction. Through careful entity identification, robust matching strategies, targeted model refinements, and programmatic instantiation using Python, this phase ensured semantic consistency across datasets and provided a reliable foundation for subsequent querying and reasoning activities.

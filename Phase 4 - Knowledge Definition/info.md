# Knowledge Definition

In this section, we describe how the prepared datasets and the purpose-specific language are transformed into a formal Knowledge Graph representation. Using the standardized datasets from Phase 2 and the conceptual vocabulary defined in Phase 3, a knowledge teleontology is defined and all datasets are aligned to it.

The goal of this phase is to bring all information into a unified and consistent structure and to prepare both the data and the schema for Knowledge Graph construction and querying.

## 4.1 Knowledge Teleontology and kTelos

In this phase, the concepts defined in the Language Definition phase were used as the primary input for constructing an OWL knowledge teleontology. The objective of this activity was to transform the previously defined conceptual vocabulary into a formal and machine-readable schema suitable for Knowledge Graph instantiation.

Following the iTelos methodology, the kTelos process was applied by reviewing the finalized language resource and encoding the identified entity types, object properties, and data properties as OWL classes and properties. This ensured direct traceability between the language definition and the resulting ontology.

Core domain concepts such as **Student, Program, Curriculum, Course**, and **Time_slot** were modeled as general classes, reflecting their central role in the academic scheduling domain.

Project-specific concepts, including **Course_schedule, Curriculum_course, Planned_course,** and **Selected_course**, were introduced to explicitly represent relationships and course selection states that are not directly captured by generic educational ontologies.

- The ontology distinguishes between Action and Event to separate student-driven planning and selection operations from time-bound academic occurrences:

- Action represents conceptual operations such as planning or selecting courses, describing a student’s academic intent or state rather than a temporal occurrence.

- Event represents scheduled academic activities that occur at specific times, such as course schedules and time slots.

This separation avoids conflating decision-related concepts with temporal events and accurately reflects the structure of the source datasets.

In the current project, Prerequisite represents a simple dependency indicating that one course must be completed before another course can be taken within a specific curriculum. The prerequisite information is defined by three identifiers:

- **curriculum_id**

- **pre_course_id**

- **course_id**

Although this dependency could be modeled as a direct object property between Course entities, it was modeled as a separate class to ensure a direct and lossless mapping from the source dataset, where prerequisite information is represented as an explicit relational table. In the current implementation, the Prerequisite class does not introduce additional semantics beyond representing this relationship.

The Program concept was modeled as a subclass of a general educational organization concept to reflect that academic programs are defined and managed within an institutional context. This design choice ensures conceptual coherence with the dataset structure without introducing unnecessary organizational semantics.

The ontology was intentionally kept lightweight, fully aligned with the language resource, and created without reusing external ontologies. It serves as the formal schema for the Knowledge Graph.

## 4.2 Dataset Alignment

In this activity, the datasets prepared in Phase 2 were aligned with the conceptual vocabulary defined in the Language Definition phase and formalized in the ontology. The primary objective was to ensure structural and semantic consistency between the datasets and the ontology schema.

Column names in all datasets were adjusted to match the corresponding classes and properties defined in the language resource, and data types were verified for compatibility with the OWL model. No semantic reinterpretation or restructuring of the data was performed beyond this alignment step.

All aligned datasets were consolidated into a single Excel file named aligned_datasets.xlsx, where each dataset is represented as a separate sheet:

- **Student** → Student entities

- **Curriculum** → Curriculum entities

- **Program** → Program entities

- **Course** → Course entities

- **Course_schedule** → Course schedule entities

- **Time_slot** → Course schedule time slots

- **Prerequisite** → Prerequisite relations

- **Curriculum_course** → Curriculum–Course relations

- **Planned_course** → Planned course selections

- **Selected_course** → Selected course records

This step ensures that all datasets are structurally consistent with the ontology and ready for instantiation in the Knowledge Graph.

## 4.3 Phase Outcomes

The Knowledge Definition phase resulted in the following outcomes:

- An OWL knowledge teleontology formally encoding the conceptual vocabulary defined in the Language Definition phase.

- A Knowledge Graph schema describing students, programs, curricula, courses, schedules, and their relationships.

- A set of aligned datasets whose structure and data types are consistent with the ontology and ready for Knowledge Graph instantiation in the subsequent phase.

## 4.4 Summary

In this phase, the conceptual language defined earlier was transformed into a formal OWL-based knowledge representation. The ontology was created by faithfully encoding the language resource, and the datasets were aligned accordingly. This phase provides a consistent and well-grounded foundation for Knowledge Graph instantiation and querying in the next phase.

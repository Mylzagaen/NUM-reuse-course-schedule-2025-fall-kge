# Language Definition

This section describes the Language Definition phase of the project. As in the previous phase, it outlines the sub-activities carried out by the team and presents the main outcomes produced. The goal of this phase is to define a purpose-specific semantic language capable of supporting academic scheduling and curriculum-related reasoning tasks within the Knowledge Graph.

## 3.1 Concept Identification

This activity aimed to identify all concepts necessary to represent the academic scheduling and curriculum domain. The identification process relied on three concrete sources produced in the previous phases:
(1) the conceptual ER model,
(2) the aligned datasets, and
(3) the defined Competency Questions (CQs).

Based on these sources, the following concept groups were identified:

Core EType concepts:
Student, Curriculum, Program, Course, Course_schedule, Time_slot, Academic_year, Academic_term, Moment.
These represent the main academic objects required to describe course offerings, institutional structure, and temporal organization.

Linking and activity entities:
Curriculum_course, Prerequisite, Planned_course, Selected_course.
These entities model many-to-many relationships, prerequisite constraints, and student activity records, which are essential for reasoning tasks such as curriculum validation and schedule conflict detection.

Data and auxiliary properties:
course_id, schedule_start, schedule_end, start_time, end_time, day_of_week, building_name, room_number, group_name, and similar attributes directly extracted from the source tables.

Together, these concepts capture the structural, relational, and operational elements required for schedule-based reasoning in the Knowledge Graph. During this activity, concepts were classified into Core, Contextual, and Custom categories based on their contribution to Competency Question coverage. Concepts that did not support reasoning tasks or did not appear in the datasets were removed from the language, as documented in Section 7.3.

### 3.1.1 Difficulties Encountered

The most challenging aspect of concept identification was aligning the ER model with heterogeneous datasets, particularly where the source data did not explicitly distinguish between Course, Course_schedule, Time_slot, and Moment. Several attributes (e.g., day_of_week, course_type, frequency) required careful interpretation to determine whether they should be modeled as independent ETypes or as simple data properties. Additionally, deciding whether Academic_year and Academic_term should be represented as standalone concepts or contextual attributes required multiple iterations and validation against the CQs.

### 3.1.2 Aspects That Were Straightforward

Core domain concepts such as Student, Course, Curriculum, and Program were easy to identify, as they appeared consistently and explicitly across all datasets and were directly referenced in the Competency Questions. Similarly, linking entities such as Curriculum_course and Prerequisite were straightforward to model, since they already existed as relational tables in the source data.

## 3.2 UKC Alignment

The second activity involved aligning the selected concepts with the Universal Knowledge Core (UKC) to ensure semantic consistency and reuse of established ontology definitions. Each concept was reviewed to determine whether a corresponding UKC entry existed and whether its semantics matched the requirements of the academic scheduling domain.

Concepts aligned with UKC:
Student, Program, Curriculum, Course, Time_slot, Academic_year, Academic_term.
These concepts had clear UKC equivalents, enabling reuse of well-established semantic structures and reducing ambiguity.

Concepts not available in UKC:
Course_schedule, Curriculum_course, Planned_course, Selected_course, Prerequisite, Moment.
These represent domain-specific structures or operational activities not covered by UKC. They were therefore defined as new concepts and assigned project-specific identifiers (GID 500XX). All glosses were written following UKC conventions to maintain terminological consistency.

Through this alignment, standard concepts benefited from existing ontology definitions, while domain-specific concepts were precisely formalized to support the project’s reasoning requirements. The result is a hybrid conceptual model that balances reuse and customization, ensuring coherence, reusability, and clarity in the purpose-specific language.

### 3.2.1 Difficulties Encountered

The main challenge was determining whether temporal concepts such as Moment, Time_slot, and Course_schedule could be meaningfully aligned with existing UKC entries. Although partial similarities existed, the semantic mismatch required the creation of new definitions rather than forced mappings. Deciding how deeply to reuse the UKC event hierarchy also required careful consideration.

### 3.2.2 Aspects That Were Straightforward

Aligning Student, Course, Program, and Curriculum was straightforward, as all have well-established equivalents in UKC and are also consistently mapped in related vocabularies such as schema.org and VIVO. This ensured clear semantic grounding and minimized the need for additional clarification.

## 3.3 Dataset Filtering

In this stage, all collected resources were examined to determine whether they were necessary, connected, and semantically useful for the Knowledge Graph. Elements marked in red in the original spreadsheet were identified as unused or redundant and were therefore removed. The primary goal of filtering was to keep the graph minimal, coherent, and focused on entities and relations that directly support reasoning tasks such as schedule validation and duplicate detection.

### 3.3.1 Removed Elements and Justification

The following classes and properties were excluded:

Academic_year and Academic_term:
Academic periods (e.g., Fall 2025) are already implicitly handled through schedule duplication and conflict checking. Explicit modeling would add complexity without improving reasoning capabilities.

course_name as a separate Name entity:
Course names are sufficiently represented as literal attributes of the Course entity. No multilingual or identity-linking requirements justified a separate entity.

credits:
Credit hours are not used in any Competency Questions or reasoning tasks related to scheduling, and were therefore removed.

planned_id, selected_id, timestamp:
These system-level metadata fields were not semantically connected to other entities and would have resulted in isolated nodes.

### 3.3.2 Filtering Rationale

Filtering decisions were guided by the following principles:

Eliminate unused information

Avoid redundant representations

Prevent isolated nodes

Reduce overall model complexity

This ensured that the Knowledge Graph remained efficient, interpretable, and focused on its intended purpose.

## 3.4 Phase Outcomes

The Language Definition phase produced the following key outputs:

Purpose-specific conceptual vocabulary
A complete set of ETypes, linking entities, object properties, and data properties required to model the academic scheduling domain.

UKC-aligned concept set
Reusable concepts aligned with UKC definitions, alongside clearly defined domain-specific concepts with project identifiers.

Filtered and aligned dataset schema
A simplified yet semantically robust schema supporting effective reasoning tasks.

Final Language Resource Sheet
A consolidated spreadsheet containing identifiers, labels, glosses, superclasses, and mappings, ready for Phase 4 (Resource-to-KG Mapping).

## 3.5 Summary

The Language Definition phase established a well-structured, purpose-specific semantic language for constructing the Knowledge Graph. Through systematic Concept Identification, UKC Alignment, and Dataset Filtering, only essential concepts were retained, terminology was standardized, and unnecessary complexity was eliminated. This phase provides a strong and reliable foundation for the subsequent stages of the iTelos methodology.

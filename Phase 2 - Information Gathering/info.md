# Information Gathering

This section reports the collection, analysis, and processing of all resources that form the foundation of our Knowledge Graph (KG), which integrates existing reusable knowledge graphs and newly produced student-specific contextual data. The goal of this stage was to gather and classify all relevant language, knowledge, and data resources needed to model curriculum structures, course offerings, student plans, and schedule-based constraints.

Our domain of interest is the academic environment of NUM (National University of Mongolia), including curriculum structures, course definitions, and real class schedules. To this end, we relied on three pre-existing iTelos-compliant datasets and generated new informal resources from the university’s internal systems.

## 6.1 Data and Knowledge Sources
### 6.1.1 Informal Data and Knowledge Sources (Producer)

These sources are not fully structured and not iTelos-compliant, requiring additional cleaning, formatting, and standardization.

We identified and extracted the following informal datasets:

- **Student Dataset**
Contains raw student identifiers, names, enrolment years, and program information extracted from NUM’s student information system.
This dataset is essential for creating the Student entity and linking students to their curriculum.

- **Planned Course Dataset**
Collected from a prototype advising interface where students record the courses they plan to take for the Fall 2025 semester.
Contains student IDs, selected course IDs, timestamps.
These serve as the foundation for the Planned_course contextual entity used for schedule conflict detection.

- **Selected Course Dataset**
Represents historically chosen courses by students during enrollment periods.
Includes student ID and course schedule ID.
These entries are crucial for modeling student behaviors and validating progression toward curriculum requirements.

These datasets were semi-structured (CSV/Excel), contained missing values (e.g., names, or curriculum IDs), and required extensive transformation to align with the ER model.

### 6.1.2 Formal Data and Knowledge Sources (Consumer)

These datasets are high-quality, diversity-aware resources that we reuse as the foundation of the knowledge layer:

Course Knowledge Graph (course-kg.ttl)
Contains canonical course definitions at NUM, including course codes, labels, names (in multiple languages), credits, academic terms, and department affiliations.

Curriculum–Course Knowledge Graph (course-curriculum-kg.ttl)
Describes curriculum structures, including which courses belong to which curriculum, their categories (core/elective), and recommended semesters.

Course Schedule Knowledge Graph (course-schedule-kg.ttl)
Provides real course scheduling instances, including day of week, timeslots, instructors, classroom locations, and academic term/year.

These are iTelos-compliant reusable KGs that require minimal processing.

## 6.2 Resource Collection, Processing, and Scraping

We employed both automated and manual extraction approaches depending on data type and format.

### 6.2.1 Course KG Resources (Reuse)

The TTL files were retrieved directly from the public repository:

course-kg.ttl

course-curriculum-kg.ttl

course-schedule-kg.ttl

### 6.2.2 Student and Planning Data (New Informal Resources)

The following files were manually exported from NUM:

Dataset	Format	Description
student_raw.csv	CSV	Student demographic and academic data
planned_raw.csv	CSV	Student-entered planned courses
selected_raw.csv CSV Enrollment selections from the Fall 2025 semester.

Processing included:

removing duplicate student IDs

normalizing name formats

aligning course identifiers to those in course-kg.ttl

converting timestamps to ISO 8601 format

assigning UUIDs to planned_course entries

## 6.3 Data Scraping and Conversion
### 6.3.1 Student Data Processing

Python scripts extracted attributes such as:

student_id

first_name, last_name

email

enrollment_year

curriculum_code

and stored them as normalized RDF triples in .ttl format.

### 6.3.2 Planned Course Processing

For each planned course entry:

validated course_id against course-kg.ttl

matched schedule instance via day/time → schedule_id

produced Planned_course triples

### 6.3.3 Schedule Data Integration

The course-schedule-kg.ttl file contained properties such as:

num:Schedule_start (xsd:time)

num:Schedule_end (xsd:time)

num:Day_of_week (string)

num:Course_type (string)

num:Course_frequency (integer)

num:has_professor → Person

num:has_location → Room

We used these to generate:

Time_slot instances

schedule conflict detection rules

## 6.4 Standardization and Cleaning

The following steps were applied to all resources:

### 6.4.1 Attribute Standardization

all times converted to xsd:time

academic years standardized to format "YYYY–YYYY"

day-of-week normalized to English + Mongolian tags

course intervals converted to xsd:duration

### 6.4.2 Entity Reconciliation

matched student-entered course codes → course-kg.ttl course_index

resolved ambiguous course names using num:Entity_name resources

linked schedules to curricula using series_of relationships

## 6.5 Ontology Integration (Consumer-side Formalization)

Using Protege, we reconstructed the ontology representing:

classes: Student, Planned_course, Course_schedule, Curriculum, Course

object properties: planned, selects, includes, part_of, has_interval, has_professor

datatype properties: schedule_start, schedule_end, credits, course_index, etc.

The final ontology integrates all reused and new resources into a unified schema.


## 6.6 Summary

This phase ensured the creation of a clean, unified resource set composed of:

three iTelos-compliant reusable KGs

three newly generated contextual datasets

a fully standardized, ontology-ready integrated graph

These resources are now prepared for the Language Definition and Knowledge Definition phases.